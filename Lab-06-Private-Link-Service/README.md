# Labb 6: Azure Private Link Service – Säker kommunikation mellan nätverk 

I den här labben har jag byggt en avancerad nätverkslösning för att simulera hur man säkert delar tjänster mellan olika nätverk eller organisationer utan att någonsin exponera trafiken mot det publika internet.

---

###  Centrala Koncept

För att förstå arkitekturen i denna labb använde jag mig av följande komponenter:

* **Azure Load Balancer:** Fungerar på Layer 4 (Transport) i OSI-modellen. Den distribuerar TCP- och UDP-trafik mellan virtuella maskiner för att säkerställa hög tillgänglighet.
* **Azure Virtual Network (VNet):** Grunden för det privata nätverket i molnet. Det ger samma kontroll som ett traditionellt on-premise nätverk men med molnets skalbarhet och segmentering.
* **Private Link Service:** Fungerar som en "adapter" som gör att min interna tjänst kan delas med andra privat.
* **Private Endpoint:** Kan liknas vid en "osynlig kabel" som dras direkt in i ett annat nätverk för privat kontakt.

---

###  Vad jag gjorde

#### 1. Uppsättning av källnätverket (Nätverk 1)
Jag började med att konfigurera **Nätverk 1**, där min tjänst lever. Här satte jag upp en **Azure Load Balancer** som fungerar som en dörrvakt för all inkommande trafik. Ovanpå denna skapade jag sedan min **Private Link Service**.

#### 2. Konfiguration av mottagarnätverket (Nätverk 2)
I ett helt separat nätverk (**Nätverk 2**) skapade jag en **Private Endpoint**. Syftet var att skapa en direkt länk mellan de två isolerade näten.

#### 3. Verifiering av anslutningen
Genom att koppla ihop tjänsten i Nätverk 1 med endpointen i Nätverk 2 fick jag en intern IP-adress: **10.2.0.10**. 
* **Resultat:** Tjänsten i Nätverk 1 gick nu att nå från Nätverk 2 via denna statiska, privata IP-adress.



---

###  Resultat & Reflektion
Genom att bygga denna arkitektur har jag skapat en lösning där ett företag kan dela sina applikationer med en annan avdelning eller partner på ett extremt säkert sätt. 

**De största fördelarna är:**
1.  **Ingen internetexponering:** Trafiken lämnar aldrig Azures interna stamnät.
2.  **Enkelhet:** Vi slipper krångliga VPN-anslutningar eller komplex routing.
3.  **Säkerhet:** Attackytan minimeras till en enda privat IP-adress.


