OS Linux - SSH, CRON, procesy a služby, systémové logy, virtualizace
===


Přehled
---
Dalši pěkně definovaná otázka. Témata jsou jasná, jde o nich mluvit dlouho, především pak o virtualizaci. Povídání o procesech a službách najdete v první otázce, zde to již nebudu opakovat.


Povídání
---

Začneme s protokolem SSH (Secure Shell). Nebudeme zkoumat, jak funguje SSH do hloubky a nebudeme se zde ani koukat na způsoby šifrování, které používá, to si objasníme v jiné otázce. Uděláme si tu ale takový základní obrázek.       
	Nejdříve bychom si měli objasnit, proč SSH vlastně vzniklo. Na konci šedesátých let byl vyvinut protokol Telnet. Využíval textové uživatelské rozhraní pro správu vzdálených zařízení. Tím mohl být počítač, ale třeba i switch, router, … Tento protokol tehdy fungoval docela dobře. Internet ještě nebyl ani zdaleka tak rozšířený. Nicméně měl jednu menší chybičku, která vyvstala jako chybička, až když se internet začal patřičně rozšiřovat. Nebyl šifrovaný .. stále není samozřejmě, nicméně dnes už se používá jen výjimečně v lokálním prostředí. Třeba váš domácí router má dost možná otevřený port 23, tedy Telnet. Nicméně právě pro svůj nedostatek v oblasti bezpečnosti se dnes pro správu vzdálených zařízení nepoužívá.
	Jeho funkci spolehlivě nahradil právě protokol SSH. To je, podobně jako Telnet, protokol aplikační vrstvy, který byl vyvinut v polovině devadesátých let. Tradičně běží na portu 22, ale velmi často se pro něj využívají i jiné porty, z bezpečnostních i praktických důvodů. Na rozdíl od Telnetu již komunikaci šifruje, a využívá se dokonce i jako jádro některých jiných protokolů, třeba SFTP (Secure File Transfer Protocol). Využívá se hojně i k různému tunelování, tím se tu ale nebudeme zabývat. SSH používá jako protokol transportní vrstvy TCP, ale může využívat i jiné protokoly, které udržují stálé spojení mezi klientem a serverem.      
	Jak probíhá spojení se serverem pomocí SSH? Malinko si to tu nastíníme. Nejdříve se musí vytvořit TCP spojení mezi oběma zařízeními. Potom se obě strany domluví na šifrovacích algoritmech. Zde se hodí zmínit, proč se doporučuje použití SSHv2. Je to jednoduché, zkrátka podporuje modernější šifrovací algoritmy. Pak probíhá autentikace, výměna společného klíče pro symetrické šifrování, … detaily se nebudeme zabývat. Důležité pro nás je, že SSH vytvoří spojení, domluví se na šifrování, autentikuje uživatele a dále pro svou komunikaci používá různé kanály, takže podporuje např. více spojení najednou.      
	Jen rychlý pohled do paketu SSH, tedy toho, co se nachází v aplikační vrstvě celého paketu. Je zde délka paketu, délka paddingu, samotný payload, padding a Message Authentication Code. Jsou přesně v tomto pořadí. Délka paketu nečekaně určuje délku paketu .. nezapočítává do toho Message Authentication Code a velikost samotného pole délky paketu. Délka paddingu, opět velmi nečekaně, obsahuje délku paddingu. Paddingu jsou náhodné byty přidané nakonec payloadu pro bezpečnost. Následuje samotný payload, tedy přenášená data, a hned po něm právě padding. Poslední je MAC, což je pole pro kontrolu správnosti paketu.      
	SSH podporuje dva způsoby autentikace. Jedním, naprosto intuitivním a klasickým, je prosté heslo. To má ale nespočet problémů a nevýhod .. heslo se nějakým způsobem musí přenést k cíli, někdo si ho musí pamatovat, nikdo ho nesmí prozradit .. zkrátka spousta míst, kde se může něco podělat.       
	Druhý způsob je pomocí klíčů. Uděláme si takový rychlý vhled do asymetrického šifrování, ke kterému se vrátíme později v jiné otázce. Existuje něco, čemu se říká private key a public key. Veřejný klíč šifruje zprávy, soukromý ji umí zase odšifrovat. Soukromý klíč je soukromý, protože bychom ho nikdy neměli nikomu dávat a dobře si ho střežit na svém zařízení. Veřejný klíč na druhou stranu, můžeme dát, kam chceme, třeba právě na server, ke kterému se chceme připojit. Pokud pak zahájíme připojení k serveru, stane se následující. Server pošle zašifrovanou zprávu, kterou zašifroval právě našim veřejným klíčem. My ji odšifrujeme, proběhne jakási kouzelná kalkulace, která dokazuje, že jsme zprávu úspěšně odšifrovali, a ta je poslána zpět na server. Pokud vše projde hladce, server nám přístup povolí.     
	To by zatím bylo k SSH. Jak se využívá v Linuxu příkaz ssh, se můžete podívat v manuálových stránkách. Teď se jen rychle podíváme na systémovou službu CRON. Služby a procesy jsme již podrobně probraly v jedné z předchozích otázek, takže se k nim již nebudeme vracet.
	CRON je služba, která se stará o to, aby se nějaký program volal periodicky v nějaký čas. Může to být např. periodický git fetch a commit, nějaké logování, diagnostika, no prostě co si zamanete.      
	V systému existuje soubor crontab pro každého uživatele a systémový crontab. Každý uživatel může svůj crontab editovat přes příkaz crontab -e. CRON má svůj velmi specifický způsob definování, kdy se má funkce spouštět. Lze nastavit minuty, hodiny, dny v měsíci, měsíce a dny v týdnu. Systémový crontab má navíc i usera, pod kterým chceme daný program spustit. Podívejte se na nějaké video, třeba v materiálech, to vám vysvětlí lépe než já, jak plánování v CRONu funguje.      
	Čistě ze zajímavosti, existuje ještě program at. Ten dovede naplánovat něco na určitý čas. Spustí se to však pouze jednou.
	Poslední věcí, kterou se budeme zabývat, než se dostaneme k virtualizaci, jsou systémové logy. Protože ani já sám nevím přesně, co si pod tím u maturitu představují, jen tak rychle projedu základy logování v Linuxu.     
	Většina logů se nachází v adresáři /var/log. Logují sem různé aplikace, je tu třeba boot.log, který obsahuje informace o startu systému. Bývají tu logy balíčkovacího systému. Některé logy mohou být binární. To většinou znamená, že se dají přečíst s pomocí nějaké aplikace. Běžně mají logy příponu .log, ale nemusí to tak vždy být. Navíc se logy opravdu zásadně liší distribuce od distribuce.     
	Ke čtení logů se hodí utilita v SysmtemD, journalctl. Ukazuje logy initu, takže spoustu zajímavých věcí, především pak logy jednotlivých služeb … to by k logování bylo tak asi všechno … možná ještě jedině, co to logování vlastně je. Jednoduše, program si uschovává informace o jeho běhu. Změnách, úspěších, errorech … je to standartní praxe.
	Posledním tématem, které zde budeme řešit, je virtualizace. Společně s ní si zmíníme i kontejnerizaci, ať to máme všechno hezky pokryté. Nebudeme probírat, jak přesně funguje virtualizace a kontejnerizace po povrchem, nicméně si vysvětlíme především její praktické využití a povíme si základní charakteristiku.      
	Začneme jednou důležitou věcí, rozdílem mezi simulací a emulací. Simulace je metoda, kterou využívá třeba váš jistě známý Cisco Packet Tracer. Ten simuluje switche, routery a mnoho dalšího, včetně jejich síťového připojení. Je to fajn věcička. Nicméně jste si jistě všimli, že velmi často působí tak nějak .. uměle. Možná právě protože to vše umělé opravdu je. Některé funkce dokonce chybí, nebo funguje trošku jinak než na opravdové verzi zařízení. Nenarazíte tam ani třeba na zajímavé bugy, které se nachází v originálním zařízení, a možná tam dokonce narazíte na nějaké úplně jiné. Simulovaná zařízení se ze všech sil snaží tvářit jako že opravdu provádí nějaký kód. Realita je ale taková, že simulované systémy jsou napsané v nějakém vyšším programovacím jazyce, třeba v Javě, a snaží se jen velmi dobře imitovat chování reálného stroje. Konkrétně v případě Packet Traceru se za zadanými příkazy bohužel neschovávají nějaké instrukce pro virtuální procesor, ale spíš funkce, která něco jen něco vypíše na obrazovku.     
	Emulace ale funguje trochu jinak. Právě na konceptu emulace je postavená virtualizace. Emulace využívá velmi přesně simulovaného hardwaru, na kterém rozbíhá nativní programy daného prostředí. Simulovaný procesor dané architektury provádí instrukce tak, jako by byl reálný, takže systém, který chceme emulovat, často ani nepozná, že emulovaný je. To je dnes už u moderních systémů ale změnilo a ty dokáží aktivně využít znalosti toho, že jsou emulované, např. použitím speciálních ovladačů pro emulované disky.       
	Jaký je tedy rozdíl mezi emulací a virtualizací? Alespoň z mého pohledu jen velmi malý .. v virtualizace v podstatě využívá emulace jednotlivých komponent, aby mohla rozběhnout celý operační systém. Moderní virtualizace také dokáže využívat přímo procesoru vašeho systému, takže nemusí být emulovaný, to vše efektivně zrychluje.        
	Dobrá, teď už přímo k samotné virtualizaci. Zkusím se vyhnout nějakým klasickým buzz words, protože už vám je určitě všem moc dobře jasné, k čemu se takové virtualizování operačního systému hodí. Velmi hojně se využívá v cloudovém prostředí, nicméně i vy jste si už jistě rozjeli nějakou virtuálku třeba ve VirtualBoxu. Virtualizace s sebou také přináší izolaci .. jo, to je jeden z těch buzz wordů, ale hear me out on this one. Protože virtualizovaný počítač běží jako naprosto samostatný systém s vlastními knihovnami, síťovým stackem, disky, … no vším prostě, právě touhle izolací vám dává jistou dávku bezpečnosti .. pokud si do ní samozřejmě netunelujete některé periferie nebo třeba síť, to pak tuhle izolaci efektivně malinko rozbijí. Nicméně takové virtuálka je ještě k tomu všemu většinou za NATem, a to ji dělá ještě izolovanější. K čemu to je? Třeba pokud si nechtěně na VM nainstalujeme nějaký malware, poškodí tak maximálně naši VM a možná ani nebude vědět, že ve VM je. Tak pro zajímavost, moderní malwary už většinou vědí, že jsou ve virtuálce, není tak těžké to zjistit. Ano, je možné se z virtuálky dostat .. alespoň teoreticky. Existují takové exploity, ale většina z nich už je zapatchovaná a v praxi se moc nepoužívají. Jak to víme? No .. AWS furt běží ne? Nemá kompletně zavirované servery, takže z toho by se dalo usuzovat, že tzv. VM Escape je buď velmi složitý, nebo si ho profesionální hackeři nechávají jako eso v rukávu. Budeme společně doufat, že to první …       
	Virtuální stroje využívají ke svému běhu něčeho, co se nazývá hypervisor. Tahle hračka virtuální systémy spravuje, stará se právě o emulaci periferii a spoustu jiných věcí, které nebudeme řešit. Existují dva typy hypervisorů. Type 1 a type 2.      
	Začneme u type 1 hypervisoru. Hojně se používá třeba na serverech, kde je potřeba rozjet více systémů, kde na každém poběží něco jiného. Můžeme si ho představit jako takový operační systém, který je schopný rozjíždět ostatní operační systémy. Sedí přímo na hardwaru a vytvořen specificky pro rozjíždění virtuální strojů, má i speciální grafický interface pro správu virtuálních strojů na serveru. Příklady takových hypervisorů jsou třeba Vmware ESXi (o tom dnes již můžete jenom snít, protože Broadcom to svými cenami a zrušením verze zdarma kompletně zkurvil), HyperV, KVM, …        
	Type 2 hypervisor je program, který běží nad vaším operačním systémem. Funguje velmi podobně jako type 1 hypervisor, nicméně při používání systémových zdrojů a podobně se musí vždy ptát operačního systému, který má pod sebou. Používá se spíš v domácích podmínkách, když si chcete hrát. Příklady takových hypervisorů jsou VirtualBox, Wmware workstation, qemu, ….       
	Dnes se rozšiřuje tzv. kontejnerizace. Jaký je rozdíl? Nu, existuje více typů kontejnerů, ale tak nějak obecně. Rozdíl mezi kontejnerem a VM je především v tom, že VM stojí samo o sobě. Má vlastní knihovny, vlastní disk, vlastní síťovou kartu .. samozřejmě vše virtualizované. Kontejner na druhou stranu využívá zdroje operačního systému, pod kterým běží. Využívá jeho knihovny. To znamená, že pod konkrétním operačním systémem může běžet pouze kontejner s tím operačním systémem. Pod Linuxem tedy nespustíte kontejner s Windows. Vice versa to ale jde, protože Windows má WSL … ale to nebudeme řešit. Kontejnery jsou tudíž mnohem více lightweight a rychlejší. Existují systémy pro správu kontejnerů jako Docker nebo Podman. Linux má už ve svém jádře zabudované tzv. namespaces, které jsou v podstatě kontejnery. V kontejneru se typicky vždy spustí jedna aplikace, třeba webová stránka, nebo databáze, která běží naprosto izolovaně od zbytku systému. Kontejnery se pak dají jednoduše přenášet do jiných systémů, nebo jednoduše vytvářet.








Příkazy
---

ssh     
připojí se k serveru pomocí ssh

ssh -i      
připojí se k serveru pomocí ssh s klíčem

crontab -e      
editace crontab souboru .. plánování periodicky spouštěných služeb

at      
naplánování nějaké služby .. kdy se má spustit

head a tail     
vypíšou určitý počet řádku ze shora, resp. zdola, souboru

journalctl      
zobrazí logy systemd

journalctl -u       
zobrazí logy nějaké služby


Materiály
SSH Keys - https://www.youtube.com/watch?v=dPAw4opzN9g&t=3s     
How SSH Works - https://www.youtube.com/watch?v=5JvLV2-ngCI     
SSH vs Telnet - https://www.youtube.com/watch?v=tZop-zjYkrU     
Jádro SSH je definováno v RFC 4251-4254, kdyby si někdo chtěl počíst - https://www.rfcreader.com/       
Shawn Powers – Cron, Crontab, & Tab - https://www.youtube.com/watch?v=f-xRrEtZA9A       
Logy podle Canonical - https://ubuntu.com/tutorials/viewing-and-monitoring-log-files#1-overview     
Learn Linux TV – Understanding Logging - https://www.youtube.com/watch?v=6uP_f_z3CbM        
PowerCert – Virtualization Explained - https://www.youtube.com/watch?v=UBVVq-xz5i0      
PowerCert – Virtual Machines vs Containers - https://www.youtube.com/watch?v=eyNBf1sqdBQ        
Different Types of Virtualization - https://www.youtube.com/watch?v=pUb8vVL5wo0     
Understanding Virtualization podle Red Hatu - https://www.redhat.com/en/topics/virtualization       
Pro zvědavé – What is Paraviltualization - https://www.redhat.com/en/topics/virtualization      
Ing. P. Troller – Síťové operační systémy, díl 12 - https://www.youtube.com/watch?v=58Sokc4ru9M     