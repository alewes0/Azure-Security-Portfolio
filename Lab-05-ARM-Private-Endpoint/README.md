# Labb 5: Private Link & ARM Templates – Säkra anslutningar med IaC 

I den här labben har jag arbetat med att säkra trafikflödet till en databas genom en **Private Endpoint**. För att göra processen effektiv och repeterbar använde jag mig av ett **ARM Template** (Infrastructure as Code).

###  Vad är ett ARM Template?
Ett ARM-template är en JSON-fil som innehåller instruktioner för Azure om exakt vilka resurser som ska skapas och hur de ska konfigureras. Det gör det möjligt att automatisera hela uppsättningen av en miljö, vilket minskar risken för mänskliga fel och sparar tid.

###  Varför använda Private Endpoints?
En Private Endpoint ger en tjänst (som en SQL-databas) en **privat IP-adress** inuti ditt virtuella nätverk (VNet). 
* **Ingen internetexponering:** Trafiken går aldrig ut på det publika internet.
* **Säkerhet:** Det minskar attackytan drastiskt då databasen inte har en publik slutpunkt.

---

###  Vad jag gjorde

#### 1. Deployment via IaC
Jag använde Azure CLI/Bash för att deploya ett ARM-template som satte upp följande infrastruktur:
* **Virtuellt nätverk (VNet):** Den isolerade miljön.
* **SQL Database:** Målet för vår säkra anslutning.
* **Private Endpoint:** Den privata "tunneln" till databasen.
* **Windows Server 2025 (VM):** En hanteringsmaskin för att testa anslutningen.



#### 2. Konfiguration av servern
Efter att resurserna skapats anslöt jag till den virtuella maskinen via **RDP (Port 3389)**. För att kunna installera nödvändiga verktyg gjorde jag följande:
* **Inaktiverade IE ESC:** Jag stängde av *IE Enhanced Security Configuration* för administratörer. Detta är nödvändigt på en ren server för att kunna ladda ner programvara utan att varje enskild domän blockeras.
* **Installation av SSMS:** Jag laddade ner och installerade **SQL Server Management Studio (SSMS)** för att kunna kommunicera med databasen.

#### 3. Verifiering av anslutningen
Genom att använda inloggningsuppgifterna som definierades i ARM-templatet loggade jag in på SQL-databasen via SSMS. 
* **Resultat:** Jag kunde verifiera att jag nådde databasen via dess **Private Link connection**. All trafik skedde internt i nätverket, helt skyddat från omvärlden.
<img width="582" height="210" alt="20_32_33" src="https://github.com/user-attachments/assets/38a267b7-8dfc-42c4-98c4-0b7f554cfdce" />

---

###  Reflektion
Att använda ARM-templates för att sätta upp komplexa nätverkslösningar som Private Endpoints är en "best practice" inom Azure Security. Det säkerställer att säkerhetsinställningarna (som att stänga av publik åtkomst) alltid blir rätt utförda varje gång man bygger miljön.

> **Säkerhetsinsikt:** Private Link är en av de viktigaste tjänsterna i AZ-500 för att skydda känslig data i SQL-databaser och Storage Accounts.
