# Labb 10: Säkra Azure SQL – Dataklassificering & Skydd 

I den här labben har jag fokuserat på att säkra "hjärtat" i många applikationer: databasen. Istället för att bara låsa dörren till servern, har jag här gått på djupet för att skydda själva datan och se till att vi följer regelverk som GDPR.

> **Snabbfakta:**
> * **Mål:** Implementera ett flerskiktat skydd för Azure SQL Database.
> * **Huvudteknik:** Microsoft Defender for Cloud, Data Discovery & Classification, SQL Auditing.
> * **Fokus:** Compliance (GDPR) och Threat Protection.

---

###  Tre lager av säkerhet

I labben implementerade jag tre viktiga säkerhetsfunktioner:

#### 1. Microsoft Defender for SQL
Jag aktiverade Defender för att få proaktiv övervakning. Detta fungerar som en dygnet-runt-vakt som letar efter ovanliga inloggningsförsök eller SQL-injektioner. Den ger också rekommendationer för att täppa till säkerhetshål i konfigurationen.

#### 2. Data Discovery & Classification (GDPR-säkring) 
Detta var en av de viktigaste delarna. Jag klassificerade känslig data (som kundnamn) med etiketten **Confidential - GDPR**. 
* **Varför?** Det gör att vi får kontroll på var vår mest känsliga data finns och kan prioritera skyddet för just de tabellerna. Det är grundläggande för att följa moderna dataskyddslagar.



#### 3. SQL Auditing (Spårbarhet) 
Jag satte upp automatisk loggning av alla händelser i databasen till ett dedikerat Storage Account. Jag konfigurerade även en **retention period** på 5 dagar. Detta säkerställer att vi i efterhand kan se exakt vem som har gjort vad i databasen om en incident skulle inträffa.

---

###  Genomförande
1. **Deployment:** Distribuerade en SQL-instans via en ARM-mall (JSON).
2. **Threat Protection:** Aktiverade Microsoft Defender på servernivå.
3. **Klassificering:** Gick igenom tabeller (`SalesLT.Customer`) och markerade personuppgifter som konfidentiella.
4. **Loggning:** Kopplade servern till ett lagringskonto för att spara audit-loggar.
<img width="1255" height="749" alt="image31_34_26" src="https://github.com/user-attachments/assets/4a200042-a924-4bee-85ca-af85cc4ad4bb" />

---

###  Resultat & Reflektion
Genom att kombinera dessa tjänster skapade jag en databasmiljö som inte bara är tekniskt säkrad, utan även "audit-ready". 

**Viktig lärdom:** Säkerhet handlar inte bara om att stoppa hackare (Defender), utan lika mycket om att veta vad man äger (Klassificering) och att kunna bevisa vad som hänt (Auditing).
