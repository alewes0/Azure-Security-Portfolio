# Azure Security Engineer Portfolio

Välkommen till mitt labbarkiv! Under den senaste månaden har jag genomfört en resa för att djupdyka i **Azure-plattformens säkerhetsarkitektur**. Här dokumenterar jag mina praktiska labbar med fokus på nätverksisolering, identitetshantering och dataskydd.

> **Mål:** Att bygga en djup teknisk förståelse för hur man säkrar molnbaserad infrastruktur enligt "Zero Trust"-principen. Jag beskriver varje labb övergripande med fokus på arkitektur och säkerhetstänk snarare än steg-för-steg-instruktioner.

---

##  Dokumenterade Labbar

| Labb | Ämne | Fokusområde | Status | Dokumentation |
| :--- | :--- | :--- | :--- | :--- |
| **01** | NSG Basic | Network Security | ✅ Klar | [Visa Labb](https://github.com/alewes0/AZ-500-Azure-Security-Labs/tree/a630c3925a6b1d3c655015cd4ab0c8350d7d8eb4/Lab-01-Basic-NSG%20) |
| **02** | Linux & Nginx | Compute Security | ✅ Klar | [Visa Labb](./Lab-02-Linux-Nginx/) |
| **03** | Service Endpoints | Data Protection | ✅ Klar | [Visa Labb](./Lab-03-Service-Endpoints/) |
| **04** | Traffic Manager | High Availability | ✅ Klar | [Visa Labb](./Lab-04-Traffic-Manager/) |
| **05** | ARM & Private Link | Infrastructure as Code | ✅ Klar | [Visa Labb](./Lab-05-ARM-Private-Endpoint/) |
| **06** | Private Link Service | Network Security | ✅ Klar | [Visa Labb](./Lab-06-Private-Link-Service/) |
| **07** | Functions & Bastion | Serverless Security | ✅ Klar | [Visa Labb](https://github.com/alewes0/AZ-500-Azure-Security-Labs/tree/f957ef718cd360a3b2d7494effa22f40f2677b96/Lab-07-Azure%20Functions-Private-Site) |
| **08** | Azure Firewall | Network Security & Routing | ✅ Klar | [Visa Labb](./Lab-08-Azure-Firewall/) |
| **09** | Service Endpoints | Storage Security | ✅ Klar | [Visa Labb](./Lab-09-Storage-PrivateEndpoint/) |
| **10** | Azure SQL Security | Database & Compliance | ✅ Klar | [Visa Labb](./Lab-10-Azure-SQL-Database/) |
| **11** | Azure Key Vault & AI | Application & Secrets | ✅ Klar | [Visa Labb](./Lab-11-Key-Vault-AI/) |
| **12** | Cosmos DB & Private Link | Advanced Isolation | ✅ Klar | [Visa Labb](./Lab-12-CosmosDB-Private-Link/) |

---

##  Nyckelinsikter från projektet
* **Defense in Depth:** Säkerhet handlar inte om en enskild inställning, utan om lager-på-lager-skydd (NSG + Firewall + Private Link).
* **Identity is the Perimeter:** Genom att använda Key Vault och Managed Identities kan vi helt eliminera risken med hårdkodade lösenord och läckta hemligheter.
* **Nätverksisolering:** Att flytta resurser från det publika internet till privata endpoints via Private Link är det enskilt mest effektiva sättet att minska attackytan.

---

##  Kontakt & Info
---

## 📬 Kontakt & Info
* **Certifiering:** AZ-500 (Under utbildning 2026)
* **LinkedIn:** [Alexander Westberg](https://www.linkedin.com/in/alexander-westberg00/)
* **Källor:** Praktiska scenarier inspirerade av och baserade på Whizlabs AZ-500 utbildningsmaterial.
