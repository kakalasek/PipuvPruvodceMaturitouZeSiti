SSL/TLS, RSA - Symetrické a asymetrické šifrování, SSL/TLS handshake, certifikační autority
===

Přehled
---

Relativně pěkná otázka, nicméně šifrování není žádná triviální věc, takže hodně štěstí.


Povídání
---

Začneme rozdílem mezi symetrickým a asymetrickým šifrování.             

![Symmetric And Asymmetric Encryption](sym_and_asym.webp)

Symetrické šifrování pravděpodobně to, co si přestavíte jako první. Máme nějaký klíč, pomocí kterého zašifrujeme nějakou zprávu. Tuto zprávu lze rozšifrovat opět pouze tímto klíčem. Nevýhodou je, že pokud se k tomuto klíči dostane někdo, kdo ho zneužije, máme po ptákách. Velkou výhodou ale je, a je to také důvod, proč se symetrické šifrování stále aktivně používá, jeho rychlost.           
Asymetrické šifrování funguje trochu jinak. Využívá principu veřejného a privátního klíče. Veřejný klíč se používá k šifrování zprávy, privátní klíč zprávu rozšifruje. Privátní klíč byste nikdy neměli nikomu ukazovat, posílat. Je váš, jenom váš. Veřejný klíč můžete zaslat, komu chcete, nemůže nikomu ublížit. Ten se využívá pouze a jenom k zašifrování zprávy. Velkou nevýhodou asymetrického šifrování je především šifrování a rozširování, která je díky často velkým klíčů dosti dlouhá.          
Díky atributům obou metod šifrování se často využívá společně. Právě to si ukážeme ve spojitosti s SSL/TLS.         

![TLS Handshake](tls_handshake.png)

Takže, jako znalý studenti sítí a webových aplikací jistě víte, že protokol HTTP (Hyper Text Transfer Protocol) odesílá data jako prostý text. Kdyby lidi nebyli banda mega čůráků, tak je to možná v pohodě. No, ale lidi jsou banda totálních zmrdů a chtějí vědět, co zajímavého děláte na internetu. K tomu potřebujeme šifrování. Byl tedy vyvinut protokol HTTPS (Hyper Text Trasfer Protocol Secure).            
Tento protokol není nic jiného než HTTP na steroidech. Tedy, přesněji, HTTP na SSL/TLS. SSL a TLS bohužel nejsou žádné exotické drogy, ale alrogrimy pro zašifrování naší komunikace. Je dobré vědět, že se neomezují pouze na HTTP. Najdeme je třeba u SFTP (Secure File Transfer Protocol) nebo emailových protoklů.     
Teď se dozvíte jednu věc, která vám pravděpodobně zničí život. SSL už se nepoužívá .. a pokud vám ho někdo nabídne, utíkejte na policii, že vás chce buď okrást, nebo je to totální šílenec. Poslední verze SSL bylá vydáná v druhé polovině 90. let a jmenuje se SSL 3.0. SSL znamená Secure Socket Layer. Všechny verze SSL jsou povážovány za nebezpečné a neměly by se za žádných okolností používat.           
Čím to, že je tento pojem tedy stále tak často využíván. Ze dvou důvodů. Lidé si špatně zvykají na změny a jeho nástupce, TLS (Transport Layer Security) se svou první verzi vlastně tolik od SSL neliší. Nicméně se změnil developer tohoto protokolu, tak raději rovnou změnili jméno. Dnes se tedy používá zásadně a jenom TLS, konkrétně TLS 1.3. My si budeme vysvětlovat, jak vypadá handshake protokolu TLS 1.2 s využitím RSA, což je symetrický šifrovací protokol. Pro ilustraci nám to totiž bude stačit.            
Ještě předtím si ale řekeme o něčem, co nám tento protokol vůbec dovoluje využívat, SSL certifikát. Prakticky je to TLS certifikát, ale na to sere pes. Co to je? Je to digitální certifikát, který ověřuje identity serveru a ujišťuje nás, že komunikace je opravdu šifrovaná. Také se aktivně využívá v procesu šifrování, alespoň v procesu s RSA. Co obsahuje? Především doménu, pro kterou byl vydaný, organizaci, které byl vydaný, CA (Certification Authority), která ho vydala, digitální podpis CA, subdomény (pokud jsou), datum vydání a expirace a předvším veřejný klíč serveru.             
Kde takový certifikát získáme? Koupíme si ho od některé z CA. Třeba CloudFlare by ho měl dokonce nabízet zdarma. Druhou možností je si ho podepsat sami. Nicméně vám bohužel nikdo nevěří a prohlížeč to moc dobře ví, takže u stránek s takovými certifikáty zobrazí patřičné varování. Ani vy byste neměli chodit na stránky s expirovanými, nebo tzv. self-signed certifikáty, pokud opravdu nevíte, co děláte.          
Když teď víme, jak funguje SSL certifikát, můžeme si konečně ukázat proces šifrování komunikace pomocí SSL/TLS.             
V první řadě musí obě strany úspěšně **navázat spojení pomocí TCP spojení**. TLS samo o sobě nefunguje s UDP.           
Potom **klient pošle hello message**. Opět připomínám, že tohle platí pro TLS 1.2 handshake s využitím RSA. RSA je dnes považovaný za nebezpečný, TLS 1.3 ho již nepodporuje. Našim účelům ale bohatě postačí. Tato hello message obsahuje 3 věci. Verzi TLS, kterou klient podporuje, šifrovací algoritmy, které podporuje, a náhodný řetězec bytů, tzv. client random.            
**Server také vyšle svou hello message**. Ta obsahuje opět 3 věci. SSL certifikát serveru, jaký šifrovací algoritmus si server vybral a náhodný řetězec bytů, tentokrát tzv. server random.         
**Klient si následně od CA ověří, že server je věrohodný**.                 
**Klient pak zašifruje další náhodný řetězec bytů pomocí veřejného klíče, který našel v certifikátu**. To pak zašle zpět na server. Nazýváme ho premaster secret 
**Server ho pomocí svého veřejného klíče odšifruje**.      
**Server i klient si pomocí client random, server random a premaster secretu vytvoří stejné klíče**, které se využíjí ve zbytku komunikace. Ta je již šifrována symetricky.
**Obě strany zašlou finished message** a šifrovaná komunikace může začít.


Materiály
---

Professor Messer - Symmetric vs Assymetric Encryption - https://invidious.reallyaweso.me/watch?v=z2aueocJE8Q        
ByteByteGo - SSL, TLS, HTTPS Explained - https://invidious.jing.rocks/watch?v=j9QmMEWmcfo       
CloudFlare - What Happens in a TLS Handshake - https://www.cloudflare.com/learning/ssl/what-happens-in-a-tls-handshake/             
Geeksforgeeks - RSA Algorhitm in Cryptography - https://www.geeksforgeeks.org/rsa-algorithm-cryptography/           
CloudFlare - How Does SSL Works - https://www.cloudflare.com/learning/ssl/how-does-ssl-work/            
CloudFlare - What is and SSL Certificate - https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/          

