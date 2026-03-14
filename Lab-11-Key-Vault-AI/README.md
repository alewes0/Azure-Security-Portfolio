# Labb 11: Säkra AI-tjänster med Azure Key Vault 

I den här labben har jag implementerat en säker metod för att hantera känsliga API-nycklar. Istället för att "hårdkoda" hemligheter direkt i applikationens källkod (vilket är en stor säkerhetsrisk), har jag använt **Azure Key Vault** som en säker mellanhand.

> **Snabbfakta:**
> * **Mål:** Skydda API-nycklar för AI-tjänster och implementera säker åtkomst via kod.
> * **Huvudteknik:** Azure AI Foundry, Azure Key Vault, .NET SDK, Azure Identity (DefaultAzureCredential).
> * **Säkerhetsprincip:** "Secret Management" och "Least Privilege Access".

---

###  Centrala Koncept

#### 1. Azure Key Vault (Värdeskåpet)
Key Vault fungerar som ett centraliserat värdeskåp för hemligheter, krypteringsnycklar och certifikat. Genom att lagra nycklar här kan vi kontrollera exakt vem (eller vilken app) som får hämta dem, och vi får fullständig loggning på varje åtkomstförsök.

#### 2. DefaultAzureCredential (Det smarta passerkortet)
I koden använde jag klassen `DefaultAzureCredential`. Det är en "magisk" funktion som automatiskt letar efter ett sätt att autentisera sig. 
* När jag kör koden lokalt använder den mina inloggningsuppgifter från Azure CLI.
* Om koden kördes i molnet skulle den automatiskt använda en **Managed Identity**.
* **Resultat:** Ingenstans i min kod finns det ett användarnamn eller lösenord!

#### 3. Named Entity Recognition (NER)
Jag använde en AI-tjänst för att analysera text och identifiera "entiteter" (platser, tider, händelser).

---

###  Genomförande

1.  **Azure AI Foundry:** Skapade ett multi-service konto för att få tillgång till AI-verktyg.
2.  **Key Vault & Secrets:** Skapade ett Key Vault och lade in API-nyckeln (`CognitiveServicesKey`) och slutpunkten (`CognitiveServicesEndpoint`) som hemligheter (Secrets).
3.  **Access Policy:** Konfigurerade en åtkomstpolicy som gav min användare rättigheter att hämta (`get`) och lista (`list`) hemligheter.
4.  **C# Utveckling:** Byggde en konsolapplikation i .NET som:
    * Ansluter till Key Vault med `Azure.Identity`.
    * Hämtar hemligheterna säkert.
    * Skickar en mening ("I had a wonderful trip to Seattle last week") till AI-tjänsten för analys.

---

###  Resultat & Verifiering

Applikationen lyckades hämta hemligheterna och AI-tjänsten analyserade texten med hög precision:
* **Seattle** identifierades som **Location** (Confidence: 1.00).
* **Last week** identifierades som **DateTime** (Confidence: 1.00).
* **Trip** identifierades som **Event** (Confidence: 0.76).

![Analysresultat i terminalen](Result)




###  Reflektion
Den största lärdomen här är hur man eliminerar "Credential Leakage". Om en hacker skulle få tag i min källkod idag, skulle de inte hitta några nycklar – bara en referens till ett låst valv som de inte har tillgång till. Detta är ett fundamentalt krav för alla moderna molnapplikationer.
