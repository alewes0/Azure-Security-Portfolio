# Labb 2: Linux-server med Nginx och lite SSH-strul 

I den här labben gjorde jag ungefär samma sak som i första labben, men med en twist: jag bytte ut Windows mot Linux (**Ubuntu 24.04 LTS**) och körde **Nginx** som webbserver istället.

### Hur jag gjorde:

* **Satte upp en Linux-VM:** Jag började med att skapa en Ubuntu-maskin i Azure. 
* **Problem med SSH:** Planen var att använda min vanliga CMD för att SSH:a mig in i maskinen, men det ville sig inte. För att lösa det använde jag **PuTTY** istället, vilket funkade direkt så att jag kunde börja jobba i terminalen.
* **Installationen:** Väl inne i Ubuntu-maskinen installerade jag Nginx. För att ändra på hemsidan var jag tvungen att navigera till rätt mapp:  
  `cd /var/www/html`
* **Redigering i Nano:** Jag använde texteditorn **Nano** för att gå in och ändra i filerna så att det blev min egen sida. 
* **Öppnade för trafik:** Precis som förra gången var jag tvungen att gå in i Azure och fixa en **Inbound Port Rule**. Jag öppnade **port 80** så att webbtrafik kunde komma fram till maskinen.
* **Slutkollen:** När regeln var på plats skrev jag in maskinens publika IP-adress i webbläsaren och då dök min Nginx-server upp precis som den skulle!

---

###  Min tanke efteråt
Det var kul att testa Linux den här gången. Det kändes lite mer "under huven" när man fick sitta i terminalen och navigera med kommandon som `cd` och ändra filer direkt i Nano. Att CMD strulade var faktiskt bra, för nu vet jag hur man använder PuTTY som backup för att komma in i sina servrar!
