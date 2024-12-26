IMAP a SMTP - odesílání a přijímání e-mailů, synchronizace poštovních schránek se serverem
===

Přehled
---

Totální trash otázka pro výplň, která s vámi třeba dovede totálně vyjebat. Rozhodně nezávidím, pokud si ji vytáhnete.

Povídání
---

Takže, email. Jak to všechno běhá? Je to doceal ízi, ale na netu chybí nějaký info. Velmi důležitý prokol, který email využívá, je SMTP (Simple Mail Transfer Protocol).            
Uděláme si nejdříve high level overview toho, jak to běhá, pak se podíváme malinko do hloubky. V první řadě potřebujeme nějaký MUA (Mail User Agent). Ten nám zprostředkovává nějaké hezké rozhraní a dovoluje nám posílat a přijímat emaily. Může to být desktopový klient, třeba ThunderBird, nebo webový. Desktopové klienty bývají bezpečnější, k webovým klientům však lze přistupovat z různých zařízení, proto bývají oblíbenější.           
Pokud napíšeme email, nás email client ho zašle na naše MTA (Mail Transfer Client).         
Pozn. technicky správně ho zašle na MSA (Mail Submission Agent), ten se ale nachází na stejném serveru jako MTA, takže mi původně přišlo zbytečné ho zmiňovat .. ale tak pro úplnost.                  
Pokud je email odesilatele a příjemce ve stejné doméně (třeba seznam.cz), MTA nemusí provádět žádné routování. Pokud jsou oba emaily v jiné doméně, je potřeba konzultovat s DNS. MTA se podá dotaz na MX záznam v DNS serveru domény příjemce. DNS odpoví FQDN MTA příjemce (třeba mail.seznam.cz). MTA se pak ještě doptá na A záznam (popř. AAAA záznam), aby mohlo email poslat na správný server, potřebuje samozřejmě IP adresu.          
Zde nastává taková zajímavá věc. MTA nemusí email hned poslat na MTA příjemce. Může ho ještě poslat na speciální filtrovací nebo backupové mailové servery, které email nakonec opravdu doručí na správné místo. Tyhle věci si většinou řeší váš ISP.           
Jakmile přijde email na MTA příjemce, MTA ho předá MDA (Mail Delivery Agent). To je zpravidla nějaký IMAP nebo POP3 server.         
Celá tato komunikace samozřejmě probíhá pomocí protokolu SMTP.          

**obrázek SMTP**

Podíváme se teď na protokol SMTP více do hloubky. SMTP zpráva se skládá ze tří částí. Envelope, hlavička a data.        
Envelope jako uživatelé nevidíme, nicméně servery ho využívají k routování. Je zde uložena adresa odesilatele a příjemce. Může se dokonce lišit od té v hlavičce, toho využívají například spammeři.            
Hlavička opět obsahuje mailovou adresu odesilatele a příjemce. Může obsahovat i další informace, třeba datum, předmět ...           
Data už jsou opravdu naše data společně s připnutými soubory. Mohou být buď čistě textová, nebo ve formátu HTML.            
SMTP využívá tzv. příkazů. Jsou to např.:           
HELO/EHLO, ty se využívají pro inicializaci komunikace mezi serverem a klientem. HELO je pro klasické SMTP, EHLO se využívá v případě šifrovaného SMTP.         
MAIL FROM, říká, od koho email je.              
RCPT TO, je pro příjemce. Klient tento command může vyslat vícekrát, pokud je příjemců více.            
DATA, předchází opravdovému kontentu emailu.            
RSET, resetuje připojení, vymaže všechny prozatím zaslané informace, aniž by připojení zrušil.      
QUIT, zakončí připojení.            
SMTP samo o sobě není bezpečné, stejně jako v případě HTTP ho musíme bezpečným udělat. Může např. využívat protokol TLS, o kterém jsme se bavili v předchozí otázce. Takové SMTP se pak označuje jako ESMTP (Extended SMTP). TLS není jediná metoda, kterou lze při šifrování SMTP využít. My si ale povíme pouze o této, měla by stačit.           
SMTP využívá protokol TCP. Originálně by mělo být dostupné na TCP portu 25. Nicméně tento port je často blokovaný, protože přes něj by měla probíhat komunikace čistého SMTP, tedy nešifrované varianty.        
Místo toho se nejdříve zavedl TCP port 465, kde se využívalo SSL. To, jak už víme, je zastaralé. Port pro SMTP přes TLS je TCP 587. Tento port už se skutečně v praxi běžně využívá. Další port, kterého lze využít, jsou-li všechny ostatní zablokované, je TCP port 2525.             
Jednotlivé servery mezi sebou tedy vždy vytváří spojení pomocí SMTP.            
Další věc, kterou si objasníme, je vzhled emailové adresy a MX záznamy. Mějmě emailovou adresu *cocksucker@killme.please*. Tato emailová adresa má 2 části. První z nich je *cocksucker*. To je lokální část adresy. Říká emailovému serveru, kde má mail nakonec skončit. Tedy v čí schránce. Druhá část *killme.please* je doména, ve které je email registrován. Lze ji přeložit do IP adresy a právě na ni routují MTA své emaily. Obě části jsou pak oddělený symbolem *@*.                
MX záznam, je typ záznamu, který můžeme najít na DNS serveru. Obsahuje FQDN mailového serveru pro nějakou doménu. Typicky se na DNS serveru nachází pro jednu doménu dva mailové servery. Jeden má vyšší prioritu a druhý nižší. Pokud při pokusy o zaslání emailu na server s vyšší prioritou dojde chybová hláška, že email nedošel, zkusí MTA odesilatele ještě mailový server s nižší prioritou. Na obrázku níže můžete vidět, jak takový MX záznam vypadá.     

**Obrázek MX záznam**

Posledním tématem bude IMAP a POP3. SMTP dovede doručit email na cílové MTA, nicméně už nedovede email stáhnout. Od toho máme MDA a dva zmíněné protokoly.          
Je mezi nimi jeden fundamentální rozdíl. POP3 stáhne email se serveru do počítače a smaže ho ze serveru.        
IMAP email nechá na serveru, zobrazuje třeba pouze hlavičky, a email stahuje teprve potom, co se na něj uživatel chce podívat. Nicméně ze serveru ho nemaže.

Materiály
---

CloudFlare - What is SMTP - https://www.cloudflare.com/learning/email-security/what-is-smtp/        
CloudFlare - What is DNS MX Record -  https://www.cloudflare.com/learning/dns/dns-records/dns-mx-record/            
CloudFlare - IMAP - https://www.cloudflare.com/learning/email-security/what-is-imap/        
CloudFlare - What is Email - https://www.cloudflare.com/learning/email-security/what-is-email/      
PowerCert - What is SMTP - https://invidious.nerdvpn.de/watch?v=PJo5yOtu7o8             
Learning Forum - SMTP Overview - https://invidious.nerdvpn.de/watch?v=hrY6pY0rYis           
PowerCert - POP and IMAP - https://invidious.nerdvpn.de/watch?v=SBaARws0hy4