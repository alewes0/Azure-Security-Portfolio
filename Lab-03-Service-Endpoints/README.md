# Labb 3: Nätverksisolering med Bash och Service Endpoints 

I den här labben tog jag steget från att klicka i portalen till att köra **Bash**. Målet var att bygga en säker miljö där jag helt låser in ett lagringskonto (Storage Account) så att bara en specifik maskin kommer åt det.



### Vad jag gjorde:

* **Automatisering med Bash:** Jag använde Bash-kommandon för att rulla ut hela miljön: 1 Resource Group, ett virtuellt nätverk (VNet) och ett **privat subnät**.
* **Fixade Service Endpoint:** För att lagringskontot ska gå att nå direkt från det låsta nätverket aktiverade jag en **Service Endpoint** för Azure Storage.
* **Härdade nätverket (NSG):** Jag skapade en **NSG** där jag satte upp hårda regler:
  * Neka all trafik till internet (stängde av omvärlden).
  * Tillät trafik till Azure Storage.
  * Tillät **RDP (3389)** så jag kunde komma in i maskinerna.
* **Skapade lagringen:** Jag fixade ett Storage Account och en File Share som hette **Marketing**. Här ställde jag in brandväggen så att *bara* mitt privata subnät fick komma in. Inget annat.
* **Skapade maskinerna:** Med hjälp av JSON-filer laddade jag upp mallar till Bash och skapade två maskiner: **ContosoPrivat** och **ContosoPublic**.

---

### Testning – Vem kommer åt vad?

Det här var den mest intressanta delen, där jag testade om mina säkerhetsregler faktiskt fungerade:

1. **ContosoPrivat (I det låsta nätverket):**
   * Jag lyckades kartlägga min File Share till en **Z-drive** via ett längre kommando. Allt funkade!
   * Jag försökte pinga Google, men det gick inte. Min regel att neka internet fungerade alltså perfekt.

2. **ContosoPublic (I det publika nätverket):**
   * Här fick jag **"Access is denied"**. Trots att det är samma moln så saknade det här subnätet en Service Endpoint, och lagringskontot vägrade släppa in maskinen.
   * Däremot kunde den här maskinen surfa på internet eftersom den inte låg under samma hårda NSG-regler.

3. **Azure Portalen:**
   * Jag försökte gå in och titta på filerna i Marketing-mappen via portalen på min egen dator, men jag blev blockerad. Det bevisade att brandväggen var så tät att inte ens jag kom in utanför det privata nätverket.

---

### Min tanke efteråt
Det var riktigt häftigt att se skillnaden mellan de två maskinerna. Att få "Access is denied" på den publika maskinen var faktiskt målet – det visade att jag lyckats isolera lagringen. Det är en sak att läsa om Service Endpoints, men en helt annan sak att faktiskt se hur det stoppar trafik i verkligheten!
