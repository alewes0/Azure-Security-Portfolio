# Labb 1: Webbserver och första steget med Inbound Rules 

I den här labben ville jag testa grunderna i hur man får en maskin att prata med internet. Huvudmålet var att starta upp en VM och installera en webbserver som man faktiskt kan nå utifrån via den publika IP-adressen.

### Hur jag gjorde:

* **Satte upp maskinen:** Jag började med att starta en Windows VM i Azure. 
* **Fick tillgång via RDP:** För att ens kunna komma in i maskinen och börja jobba var jag tvungen att öppna port **3389 (RDP)**. Det var mitt första steg för att få fjärrtillgång.
* **Installerade Webbservern:** Väl inne i maskinen drog jag igång en webbserver (IIS). Men även om den var igång så gick det inte att nå den utifrån än, eftersom Azure blockerar allt som standard.
* **Öppnade port 80:** För att folk ska kunna surfa in på servern skapade jag en ny **Inbound Rule** i Azure-portalen som tillät trafik på port 80. Det gjorde att maskinen äntligen kunde "prata" med internet.
* **Testet:** Jag skrev in maskinens publika IP-adress i min webbläsare, och då funkade det! Jag kom direkt till webbserverns startsida.

### Cleanup
När jag såg att allt fungerade och jag hade kommit åt maskinen via IP-adressen så raderade jag allt vi hade byggt. Det är skönt att rensa efter sig så man inte lämnar öppna portar eller drar onödiga kostnader.

---

###  Min tanke efteråt
Det som fastnade mest var hur viktigt det är med de här reglerna (NSG). Utan dem är maskinen helt låst, men så fort man öppnar en port så är man "ute" på nätet. Det gäller att ha koll på vad man öppnar och varför!
