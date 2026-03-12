# Labb 7: Private Site Access med Azure Functions & Bastion 

I den här labben var målet att skapa en **Private Site Access**-lösning via Azure Functions. Genom att använda detta kan vi säkerställa att kod endast kan triggas inifrån ett virtuellt nätverk. Det innebär att man måste befinna sig "innanför murarna" för att ens kunna se eller anropa tjänsten.

###  Verktyg & Tekniker
* **Azure:** Functions App, VNet, Virtual Machines, Azure Bastion.
* **Säkerhet:** Private Site Access, Access Restrictions.
* **Utveckling:** .NET 10, Azure CLI, Visual Studio Code.

---

###  Centrala Tjänster i Labben

**Vad är Azure Bastion?**
Azure Bastion är en PaaS-lösning (Platform-as-a-service) som tillåter säker anslutning via RDP och SSH till virtuella maskiner utan att exponera dem mot internet. Genom att deploya Bastion i nätverket kan jag ansluta direkt via Azure-portalen över en säker anslutning, helt utan behov av en publik IP-adress på själva VM:en.

**Vad är Azure Functions App?**
Azure Functions är en serverlös tjänst (Serverless computing) som kör event-driven kod. Man behöver inte oroa sig för infrastrukturen bakom; appen fungerar som en container som hostar funktionerna och skalar dem automatiskt efter behov.

---

###  Genomförande

#### 1. Infrastruktur & Nätverk
Jag började med att bygga upp miljön:
* En **Windows VM** som agerar klient.
* Ett **Virtual Network (VNet)** med två subnät: ett för Azure Bastion och ett för mina Azure Functions.
* När nätverket var klart ändrade jag åtkomstbehörigheterna för min Function App så att **endast** trafik från mitt utvalda VNet och specifika IP-adresser tilläts.

#### 2. Utveckling i .NET 10
I **Visual Studio Code** skapade jag ett nytt projekt baserat på **.NET 10** med en **HTTP-trigger**.
* Jag körde en lokal debugging-process för att verifiera koden.
* Därefter kopplade jag mitt Azure-konto till VS Code och deployade projektet direkt till min Function App i molnet.

---

###  Resultat & Verifiering

Här såg jag tydligt att säkerheten fungerade:
1. **Publik åtkomst (Utifrån):** När jag försökte nå URL:en via min vanliga webbläsare fick jag **Error 403 - Forbidden**. Detta bevisade att muren var uppe och att ingen obehörig kan nå koden via internet.
2. **Privat åtkomst (Inifrån):** Jag anslöt till min VM via **Azure Bastion**. När jag där inne klistrade in samma URL tillsammans med min *Master Key*, fick jag direkt upp meddelandet: **"Welcome to Azure Functions"**.

###  Reflektion
Detta var en intensiv labb som knöt ihop säcken mellan nätverkssäkerhet och modern mjukvaruutveckling. Att använda .NET 10 i en serverlös miljö som är helt låst bakom ett VNet är ett klockrent exempel på hur man bygger säkra företagslösningar i molnet idag.
