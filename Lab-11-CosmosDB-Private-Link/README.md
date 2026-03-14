# Labb 12: Nätverksisolering med Cosmos DB & Private Link 

I den här labben har jag fokuserat på att säkra en **Azure Cosmos DB** genom att kombinera brandväggsregler med **Azure Private Link**. Målet var att göra databasen helt oåtkomlig från det publika internet och endast tillåta trafik inifrån ett specifikt virtuellt nätverk (VNet).

###  Nyckelbegrepp i labben

#### Vad är Azure Cosmos DB?
Det är en globalt distribuerad databastjänst (NoSQL) designad för hög tillgänglighet och extremt låg latens. Eftersom den som standard nås via en publik slutpunkt är det kritiskt att säkra den korrekt.

#### Brandväggsregler (Firewall Rules)
Jag använde brandväggsregler på nätverksnivå för att begränsa åtkomsten. Genom att ställa in "Selected Networks" kunde jag blockera alla IP-adresser förutom de som tillhör mitt eget betrodda subnät.

#### Azure Private Link & Private Endpoints
Detta är labben viktigaste del. Istället för att ansluta till Cosmos DB via internet, skapade jag en **Private Endpoint**.
* **Hur det fungerar:** Cosmos DB får en **privat IP-adress** inuti mitt VNet.
* **Säkerhetsfördel:** Trafiken lämnar aldrig Microsofts backbone-nätverk och tjänsten blir helt osynlig för omvärlden.

---

###  Genomförande

1.  **Infrastruktur:** Skapade ett VNet med ett privat subnät och en Windows Server-VM för testning.
2.  **Cosmos DB Setup:** Distribuerade ett Cosmos DB-konto (NoSQL) i samma region (Central US).
3.  **Nätverkskonfiguration:** * Begränsade publik åtkomst till "Selected Networks".
    * Skapade en **Private Endpoint** via Azure CLI för att binda databasen till mitt subnät.
4.  **Verifiering:** Använde `curl` för att testa anslutningen från olika källor.

---

###  Resultat & Verifiering

För att bekräfta att säkerheten fungerade utfördes två tester:

#### Test 1: Inuti det säkrade nätverket (VM)
Från min VM i `private-subnet` körde jag:
`curl -X GET "https://<namn>.documents.azure.com:443/"`
* **Resultat:** `401 Unauthorized`. 
* **Analys:** Detta är ett **lyckat** resultat! Det betyder att nätverket tillåter kontakt med databasen (vi nådde fram till dörren), men vi saknade bara själva autentiseringsnyckeln för att se datan.

#### Test 2: Utanför Azure (Lokal maskin)
Körde samma kommando från min vanliga dator hemma.
* **Resultat:** `Connection Timed Out`.
* **Analys:** Perfekt! Detta bevisar att brandväggen och Private Link gör sitt jobb – databasen existerar inte för det publika internet.



---

###  Reflektion
Genom att implementera Private Link har jag eliminerat en hel attackyta. Även om någon skulle stjäla mina databasnycklar, skulle de inte kunna använda dem från internet, eftersom de måste befinna sig inuti mitt privata nätverk för att ens kunna nå tjänsten.
