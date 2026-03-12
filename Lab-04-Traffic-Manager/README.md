# Labb 4: Azure Traffic Manager – Att inte lägga alla ägg i samma korg 

I den här labben utforskade jag **Azure Traffic Manager**, som fungerar som en DNS-baserad load balancer. Det är en tjänst som låter mig dirigera trafik till mina applikationer så att de alltid är tillgängliga, även om en server skulle gå ner.

###  Steg 1: Infrastruktur och Säkerhet

* **Satte upp två servrar:** Jag skapade två virtuella maskiner med **Windows Server 2025** i två olika resursgrupper.
* **Tänkte på säkerhet och åtkomst:** Jag öppnade port **3389 (RDP)** för hantering och port **80 (HTTP)** för webbtrafik i deras Network Security Groups (NSG).
* **Zon-indelning (Availability Zones):** För att säkra driften placerade jag maskinerna i olika fysiska zoner (**VM1** utan specifik zon och **VM2** i **Zon 3**). Detta ger en fysisk separation om ett datacenter skulle få strömavbrott.

###  Steg 2: Automatisering med PowerShell

Istället för att klicka i menyer använde jag **PowerShell** för att installera webbservern (IIS) och skapa en unik startsida för varje maskin:

```powershell
# Installera IIS
Install-WindowsFeature -name Web-Server -IncludeManagementTools

# Ta bort standard-sidan
remove-item C:\inetpub\wwwroot\iisstart.htm

# Skapa en ny sida som visar vilken server vi är på
Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World från min " + $env:computername)
```
### Steg 3: DNS och Traffic Manager-setup
För att Traffic Manager ska fungera behövde maskinerna vara nåbara via namn. Jag gjorde följande:

Konfigurerade unika DNS-namn för båda servrarna under deras publika IP-inställningar.

Skapade en Traffic Manager-profil och lade till två endpoints (VM1 och VM2).

Traffic Manager övervakar nu servrarnas hälsa och dirigerar automatiskt användare till den server som är tillgänglig.

### Resultat & Reflektion
När allt var klart använde jag URL:en från min Traffic Manager-profil. Genom att surfa in på samma länk såg jag att jag dirigerades mellan de olika servrarna. Det bevisade att lastbalanseringen fungerade!
![image051_13_08](https://github.com/user-attachments/assets/01ef784b-67c6-4e0d-baed-e4fb28a665c9)

### Reflektion: 
Att förstå hur man sprider ut resurser i olika zoner och använder en DNS-lastbalanserare är en viktig del av AZ-500. Det handlar om att bygga system som tål fel och alltid är tillgängliga för användaren.
