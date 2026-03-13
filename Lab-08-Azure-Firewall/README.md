# Labb 08: Azure Firewall – Den Intelligenta Ordningsvakten 

I den här labben har jag implementerat **Azure Firewall** för att centralisera säkerheten och styra exakt vilken trafik som får lämna mitt nätverk. Detta ger en betydligt högre säkerhetsnivå än vanliga NSG-regler.

> **Snabbfakta:**
> * **Mål:** Begränsa internetåtkomst till specifika domäner (FQDN).
> * **Huvudteknik:** Azure Firewall (Basic SKU), Firewall Policy, Route Tables (UDR).
> * **Resultat:** En "Default Deny"-miljö där endast godkända sidor går att nå.

---

###  Centrala Koncept

#### Vad är Azure Firewall?
Det är en intelligent, molnbaserad brandväggstjänst som skyddar dina resurser i Azure. Den största fördelen är att den är **Fully Stateful**. 

**Pizzabuds-analogin (Stateful vs Stateless):** 
Tänk dig att du bor i ett lägenhetshus med en receptionist:
* **Stateful (Azure Firewall):** Du säger till receptionisten: *"Jag har beställt pizza, släpp in budet"*. Receptionisten loggar detta. När budet kommer tittar hen i boken, ser din beställning och släpper in budet direkt. Hen **minns** sammanhanget.
* **Stateless (NSG):** Receptionisten har inget minne. Varje gång någon knackar på dörren måste hen ha en specifik regel för att släppa in dem, oavsett om du precis pratat med dem eller inte.

#### Vad är Azure Policy?
Det är molnets ordningsvakt. Den ser till att alla resurser följer företagets regler (governance). Om en resurs bryter mot reglerna (t.ex. skapas i fel region) kan den stoppas eller markeras som **"out of compliance"**.

---

###  Genomförande

#### 1. Infrastruktur & Nätverk
Jag satte upp ett virtuellt nätverk med specifika subnät för att separera brandväggen från arbetslasterna:
* **AzureFirewallSubnet:** Här bor brandväggen.
* **Firewall Management:** Konfigurerat för *Forced Tunneling*.
* **Workload-SN:** Här placerades min test-VM (`Srv-Workload`).

#### 2. Konfiguration av regler (Firewall Policy)
För att styra trafiken skapade jag en policy med tre regeltyper:
* **Application Rules:** Jag använde **FQDN** (Full Qualified Domain Name) för att tillåta trafik till `www.google.com`.
* **Network Rules:** Tillät DNS-trafik (UDP 53) så att servern kan hitta hemsidor.
* **DNAT Rules:** Gjorde det möjligt att ansluta till servern via brandväggens publika IP, som sedan skickar trafiken vidare internt.

#### 3. Route Table (UDR) – Den digitala vägvisaren 
För att tvinga trafiken genom brandväggen skapade jag en **Route Table**. Jag ställde in en route för `0.0.0.0/0` (all trafik) med brandväggens privata IP som **Next hop**. Utan denna skulle servern ha gått direkt ut på internet och struntat i brandväggen.

---

###  Resultat & Verifiering

Efter att ha anslutit till min VM testade jag brandväggens filter:
1. **Google.com:** Fungerade klockrent eftersom domänen fanns i min "Allow-list".
2. **Microsoft.com:** Blockerades omedelbart. 


Detta bevisar att brandväggen fungerar som en **Default Deny**-lösning; allt som inte är uttryckligen tillåtet stoppas automatiskt.

---

###  Reflektion
Den här labben visade styrkan i att flytta säkerheten från enskilda nätverkskort (NSG) till en centraliserad brandvägg. Att kunna styra trafik baserat på domännamn istället för bara IP-adresser gör hanteringen både säkrare och enklare att skala upp.
