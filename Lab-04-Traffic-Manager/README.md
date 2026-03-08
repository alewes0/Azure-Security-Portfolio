# Labb 4: Azure Traffic Manager – Att inte lägga alla ägg i samma korg 

I den här labben utforskade jag **Azure Traffic Manager**, som fungerar som en DNS-baserad load balancer. Det är en tjänst som låter mig dirigera trafik till mina applikationer så att de alltid är tillgängliga, även om en server skulle gå ner.



### Vad jag gjorde:

* **Satte upp två servrar:** Jag skapade två virtuella maskiner med **Windows Server 2025**. För att göra det ordentligt lade jag dem i två helt olika Resource Groups.
* **Tänkte på säkerhet och åtkomst:** Jag öppnade port **3389 (RDP)** för att kunna styra maskinerna, men också port **80 (HTTP)** så att webbtrafiken kunde komma fram.
* **Zon-indelning (Availability Zones):** Här tänkte jag på redundans. 
  * **VM1:** Ingen specifik zon.
  * **VM2:** Placerades i **Zon 3**. 
  Detta gjorde jag för att ha en fysisk separation mellan maskinerna. Om ett datacenter får problem ska den andra servern fortfarande rulla!

### Automatisering med PowerShell
Istället för att klicka runt i menyer inne i Windows, använde jag **PowerShell** för att snabbt installera webbservern (IIS) och fixa en enkel hemsida:

```powershell
# Installera IIS
Install-WindowsFeature -name Web-Server -IncludeManagementTools

# Ta bort standard-sidan
remove-item C:\inetpub\wwwroot\iisstart.htm

# Skapa en ny sida som visar vilken server vi är på
Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World från min " + $env:computername)
