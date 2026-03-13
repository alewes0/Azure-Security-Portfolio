# Labb 09: Säkra Azure File Shares med Service Endpoints 

I denna labb var målet att säkra upp **Azure File Shares** med hjälp av **Service Endpoints**. Detta är en central del i AZ-500 för att förstå hur man begränsar åtkomst till lagring på nätverksnivå.

###  Vad är en Service Endpoint?
En Service Endpoint gör det möjligt att knyta specifika Azure-tjänster (som Storage Accounts) direkt till ett virtuellt nätverk (VNet). Detta görs genom att utöka nätverkets identitet hela vägen till tjänsten.

Man kan se det som en **privat tunnel** inuti Microsofts eget nätverk (**Azure Backbone**). Istället för att trafiken mellan vårt "kontor" (VNet) och lagringskontot skickas ut på det osäkra, publika internet, stannar den helt skyddad inom Azure. Detta gör att vi kan stänga lagringskontots brandvägg för alla utom just vårt specifika nätverk.

---

###  Genomförande & Arkitektur

Jag byggde upp följande infrastruktur för att testa säkerheten:

1.  **Virtuellt Nätverk (VNet):** Skapade `vnet1` med två olika subnät:
    * **Private Subnet:** Här aktiverade jag en **Service Endpoint** riktad mot `Microsoft.Storage`.
    * **Public Subnet:** Ett vanligt subnät utan Service Endpoint.
2.  **Network Security Groups (NSG):**
    * Skapade en NSG för det privata subnätet med strikta utgående regler (**Outbound Rules**).
    * Tillät trafik till Storage (`Allow-Storage-All`) men blockerade all annan trafik till internet (`Deny-Internet-All`).
3.  **Storage Account & File Share:**
    * Skapade ett lagringskonto med en File Share (`my-file-share`).
    * **Viktigt steg:** Jag konfigurerade lagringskontots brandvägg till att **endast** tillåta anslutningar från det **Privata subnätet**.
4.  **Virtuella Maskiner:** Distribuerade en VM i det privata subnätet (`myVmPrivate`) och en i det publika (`myVmPublic`).

---

###  Resultat & Verifiering

För att verifiera att tunneln fungerade som tänkt utförde jag följande tester:

#### Test 1: Anslutning från Privat Subnät
Jag anslöt till `myVmPrivate` via RDP och körde PowerShell-skriptet för att mappa upp File Sharen som en Z:-enhet.
* **Resultat:** Lyckat! Drive Z: skapades utan problem eftersom subnätet har en godkänd Service Endpoint och är tillåten i lagringskontots brandvägg.

<img width="1912" height="734" alt="Screenshot 2026-03-13 124007" src="https://github.com/user-attachments/assets/4e2eebf4-b41a-4a09-ae40-cac44fd57ef7" />

<img width="1168" height="873" alt="Screenshot 2026-03-13 124040" src="https://github.com/user-attachments/assets/a4c80587-75b0-4d81-ba47-bfef74754ddc" />

#### Test 2: Anslutning från Publikt Subnät
Jag anslöt till `myVmPublic` och försökte köra samma skript.
* **Resultat:** **Access Denied**. Trots att maskinen fanns i samma VNet, blockerade lagringskontot anslutningen eftersom det publika subnätet inte hade Service Endpoint aktiverad och inte fanns med på "allow-listan".
<img width="1250" height="341" alt="VM2" src="https://github.com/user-attachments/assets/652f3e78-9bee-4897-869f-f957dcab96f0" />

---

###  Reflektion
Genom att använda Service Endpoints kunde jag helt ta bort exponeringen mot internet för min File Share. Detta är en kostnadseffektiv och kraftfull metod för att säkerställa att känslig data endast kan nås från specifika, betrodda delar av nätverket.
