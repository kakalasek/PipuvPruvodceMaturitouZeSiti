OS Linux - Charakteristika, struktura OS, start systému, služby, procesy, paměť, balíčkovací systém
===

Přehled
---
Velmi rozsáhlá otázka, asi bude potřeba hodně věcí zestručnit, aby se stihlo vše. Tady bude samozřejmě vše probrané trochu více do podrobna.

Povídání
---
Linux je open-source multiprocesový, multiuživatelský, multi všechno operační systém unixového typu napsaný v jazyce C (z valné většiny). Vyvinul ho jistý finský student Linus Torvalds v roce 1991. Open-source znamená, že jeho kód je volně přístupný, někde zveřejněný. Protože je unixového typu, přebírá spoustu konceptů právě z Unixu, což je rodina operačních systémů, které mají kořeny v originálním Unixu vytvořeném v AT&T. Jedním z doposud používaných Unixů je např. FreeBSD, OpenBSD, … Linux je pod licencí GNU GPL 2, tou se nebudeme dále zabývat, můžete se na ni podívat sami.      
	Každý linuxový operační systém má jednu společnou věc, která ho dělá právě linuxovým operačním systémem, tou je linuxový kernel. Linux je šířený pomocí tzv. linuxových distribucí. Distribuce je operační systém, který je postavený na linuxovém jádře. Může obsahovat třeba vlastní balíčkovací systém, init proces, dělat různé věci různými způsoby. V jádře je to ale vždy Linux.     
	Linux najdete nejčastěji v serverech, různých high availability a critical systémech, protože, na rozdíl od Vidlí, Linux vám jen tak necrashne. Často se Linux a jeho jádro také upravuje do různých embedded zařízení, využívá se v síťových zařízení. Linux dovede fungovat třeba jako router, má v sobě dokonce zabudovaný i jakýsi firewall v podobě iptables a umí i NAT, o kterém si povíme v dalších otázkách. V poslední době ale nabývá popularity i v uživatelských zařízení třeba v podobě distribucí jako Ubuntu nebo Fedora.
	Ještě předtím, než se vrhneme na samotnou strukturu operačního systému, procesy … měli bychom si probrat balíčkovací systémy. Ve světě open source je kód šířen především pomocí různých balíčků, které si můžeme zkompilovat. Ty mohou být např. na githubu nebo na samotných stránkách softwaru. Linuxové distribuce nám ale námahu s instalací balíčků značně zjednodušují. Implementují v sobě tzv. balíčkovací systémy, v debianových distribucím je to např. apt, soubory s příponou .deb, v red hatových distribucích je to pak třeba dnf, soubory s příponou .rpm. Balíčkovací systémy se pro vás starají o instalaci balíčku a jeho dependecí, updaty systému a mnoho dalšího.     
	Teď si povíme něco o struktuře operačního systému. Nejdříve trochu obecně. Operační systém se skládá z uživatelského prostoru a kernelového prostoru. V klasickém schématu běží v uživatelském prostoru především uživatelské programy a některé procesy. V tom kernelovém pak všechno ostatní, správce paměti, scheduler, souborový systém, TCP/IP stack, … Právě tyto komponenty souhrně označujeme jako kernel operačního systému. Kernel se stará o interakci s hardwarem např. pomocí různých ovladačů, stará se o procesy, přiděluje jim paměť, spravuje souborový systém, … Existuje více typů kernelu, Linux je silně monolitický kernel, tzn. že všechny komponenty kernelu pracují spolu a pokud některá z nich selže, selže i celý systém. To má své východy a nevýhody, ty zde nebudeme rozebírat. Obecně jsou linuxové operační systémy silně stabilní. Struktura a vývoj operačních systémů je velmi složité téma, takže ho zde nebudeme dále pitvat. V materiálech vám nechám pár pěkných knížek od Tanenbauma, kdybyste si chtěli počíst.       
	S kernelem jsme schopni komunikovat pomocí tzv. systémových volání. Ty zde nemusíme dále probírat, stará se o ně za nás totiž něco, čemu se říká shell. Shellu existuje několik, třeba Bash, Ksh, Csh, … Netřeba se zabývat jejich rozdíly, všechny dělají to samé. Nabízí nám nějakou možnost interagovat s operačním systémem pomocí příkazů.
	Teď si objasníme start systému od začátku do konce. Budeme se dívat konkrétně na systém Linux, nicméně některé, především první, kroky jsou společně pro všechny operační systémy. Postup se může lišit podle architektury procesoru, typu operačního systému, použitého zavaděče … My si tu jen plus minus nastíníme, jak vypadá boot linuxového systému na architektuře x86 ( detaily ohledně architektury není třeba vědět, k maturitě je po vás určitě nebude nikdo požadovat ), to znamená především pro procesory Intel a AMD.
	Při spuštění počítače jako první proběhne POST (Power On Self Test). Ten inicializuje základní komponenty a otestuje paměť. Pokud najde jakýkoliv problém, většinou základní deska zapípá nebo je na ní alespoň nějaké LED světlo, které svým blikáním prozrazuje typ chyby, který nastal. Ta se pak dá dohledat a patřičně opravit. Průběh POSTu a jeho start zajišťuje BIOS, nebo UEFI. To je program, který se nachází tradičně v ROM (dnes to již může být i jiný typ paměti, třeba NVRAM) paměti na základní desce. Tento program je po startu počítače spuštěn (tzn. procesor ho začne vykonávat), provede POST a pokud vše proběhne bez problému, vydá se hledat bootovatelné médium.        
	Pozn.: Možná jste slyšeli o něčem, čemu se říká CMOS čip, nebo jste se divili, k čemu je na vaší základní desce knoflíková baterie. BIOS (nebo UEFI) je read-only, takže ve své paměti neudrží změny v konfiguraci. To má na starost právě CMOS čip na základní desce. Protože je to ale nestálá paměť (podobně jako třeba RAM), potřebuje stále napětí k udržení informace. K tomu máme na základní desce knoflíkovou baterii.     
	Zde se dostáváme k jistému problému. Jsem si jistý, že s pojmem BIOS jste se již někdy setkali, nicméně pojem UEFI nemusí každému něco říkat. Ačkoliv k maturitě rozdíl mezi těmito dvěma programy vědět nutně nemusíte, pro lepší pochopení si je tu probereme hlouběji. UEFI je často také nesprávně označované jako BIOS, dokonce i samotnými výrobci základních desek. Proč? Lidé si na takové označení zkrátka zvykli. Nicméně pravdou je, že klasický BIOS už je dnes v moderních systémech prakticky vymýcen a je nahrazen právě firmwarem UEFI. BIOS měl spoustu limitací a chyb, které UEFI odstraňuje. Jednou z takových limitací je třeba maximální velikost disku nebo oddílu 2TB. Jak poznáte, co máte na počítači právě vy? Jsem si více než jistý, že UEFI, ale kdyby jste si to chtěli zkontrolovat, přítomnost UEFI spolehlivě indikuje graficky přívětivé rozhraní a podpora myši. Podívejte se na stránky svého výrobce základní desky, jak se dostat do rozhraní BIOSu. Pokud vás přivítá zvláštní modrobílá nabídka, pravděpodobně máte ještě stále legacy BIOS a měli byste si rychle pořídit nový počítač a vaši fosilii dát třeba do muzea.
	Když jsme si teď vysvětlili, že BIOS a UEFI není to samé, ukážeme si, jak se liší jejich chování při startu systému. POST provádí oba firmwary stejně. Co už ale není stejné, je způsob nalezení bootovatelného média. Legacy BIOS používá tzv. MBR (Master Boot Record), což je zastaralý způsob označení bootovatelného disku. BIOS začne prohledávat média podle nastavených priorit a hledá první sektor označení kouzelným číslíčkem 0x55AA, které označuje, že disk je bootovatelný a spustí ho. Toto číslo se nachází na konci MBR. MBR je první sektor bootovatelného média, tedy 512 bytů. V případě klasického MBR se v těchto 512 bytech nachází pouze bootstrap kód, informace o oddílech disku a naše kouzelná číslíčka. Dokážete si představit, že do tak malého prostoru nelze nacpat moc velký program. Proto MBR pouze načte další kód do paměti a spustí ho. Tomuto kódu se říká zavadeč (resp. i o tom kousku kódu v MBR můžeme tvrdit, že je součástí zavaděče) a převezme kontrolu nad dalšími kroky bootovaná systému. Povíme si o něm záhy.      
	UEFI používá značně rozdílný systém, který je ale také komplexnější, takže si zde spíše jen nastíníme, jak funguje. UEFI umí pracovat s jednoduchým souborovým systémem FAT. Prohledává GPT tabulky disků, dokud nenalezne nějaký, na kterém se nachází tzv. EFI oddíl, který je naformátovaný právě souborovým systémem FAT. GPT tabulka zkrátka obsahuje oddíly disku, je flexibilnější než oddíly v MBR a podporuje více oddílu s větší velikostí. V něm najde adresář efi a v něm soubor s určitým jménem, který načte do paměti a spustí. To bývá zpravidla zavaděč, ale může to být třeba rovnou i samotný kernel. UEFI podporuje i tzv. secure boot. Dovede kontrolovat, zda image, který se chystá nabootovat, je podepsán autoritou, jejíž klíč má UEFI k dispozici. Pokud byste chtěli znát o bootování s pomocí UEFI více, nechám vám něco v materiálech, teď se již posuneme dál.       
	Pozn.: GPT tabulka není unikátní pro UEFI, může ji využívat i legacy BIOS, nicméně to není moc běžné. Stejně tak UEFI může využívat legacy boot s MBR, ale tím si nebudeme dále vařit hlavy.
	Rychle si povíme něco k zavaděči. Stará se v podstatě především o načtení kernelu do paměti. Má i jiné funkce, ale o ty se nemusíme zajímat. V Linuxu se nejčastěji používá zavaděč se jménem GRUB 2.       
	Zavaděč do paměti nahraje kernel a něco, čemu se říká initramfs. Kernel detekuje procesor a jeho vlastnosti a inicializuje paměť a moutne dočasný souborový systém žijící v RAM paměti, initramfs. Initframfs obsahuje program se jménem init, který připraví systém na namontování opravdového kořenového adresáře. Inicializuje ovladače disků, popř. nastaví síťové spojení. Kdybyste chtěli vědět více o initramfs, zanechám vám něco v materiálech.        
	Následně předá kontrolu samotnému jádru, to initramfs uklidí, namontuje už reálný kořenový adresář a spustí svůj vlastní první program, což dnes tradičně bývá SystemD. Je možné taky narazit na SystemV. Tento proces donastaví počítač spuštěním různých procesů a služeb tak, aby bylo možné jej používat. Má PID 1 a všechny ostatní procesy jsou jeho děti.        
	To bychom za sebou měli rychlou projížďku bootem systému Linux. Nyní se mrkneme na to, jak uvnitř něho fungují služby, paměť a procesy. Začněme třeba procesy. Ještě předtím, než se podíváme na samotné procesy v Linuxu, stojí za to zmínit něco, čemu se říká plánovač, anglicky scheduler. Na maturitu to sice potřebovat spíš nebudete, ale opět vám to pomůže k lepšímu pochopení spojených témat. Starší systémy nemusely být multiprocesové jako Linux a procesor v jednu chvíli mohl zpracovávat opravdu jen jeden proces, program. Dnešní operační systémy se od této doby značně posunuly a umějí zpracovávat tisíce takových procesů, s přibývajícími procesorovými jádry a představení hyperthreadingu dokonce i několik najednou. Toho se dosahuje právě pomocí plánovače. Ten nedělá nic jiného, než že rozhoduje, jaké procesy právě běží, kdy skončí a které je nahradí.       
	V procesoru musí vždy běžet nějaký program. A i kdyby náhodou nebyl žádný proces, který by mohl procesor zpracovávat, plánovač má připravený tzv. proces idle, který procesor v tomto čase zaměstná.        
	Tradičně se úlohy přepínají na základě jakéhosi tikání programovatelného časovače, který je zabudovaný kdesi v našem systému. Interval tikání tohoto časovače se dá nastavit při kompilaci jádra na přednastavené hodnoty, tedy alespoň u Linuxu. Důležitým konceptem, který je potřeba alespoň povrchově pochopit, abychom pochopili plánovač s časovačem, je tzv. interrupt, přerušení. To může být voláno různými způsoby. Naprosto typicky třeba klávesnicí, myší, diskem ale právě taky třeba časovačem. Využívá se tzv. vektorovaného přerušení, nebudu vysvětlovat, nicméně důležité je, že přerušení zastaví aktuálně běžící proces, uloží jeho stav a zavolá obsluhu přerušení. Pokud toto přerušení zavolal časovač, plánovač se podívá a rozhodne, jakou úlohu nechat běžet teď, či jestli nenechat ještě chvíli běžet aktuální úlohu. Při každém takovém výměně úlohy se uloží stav právě přerušené úlohy, tj. stav registrů v procesoru v době vyvolání přerušení, aby se mohla později spustit opět bodu, kde skončila.       
	Pozn.: Přepínání úloh tak, jak si ho zde představujeme, funguje pro uživatelský prostor, nemusí však vypadat stejně pro procesy a úlohy volané kernelem. Podrobnosti si můžete dohledat sami.
	Pozn.: V Linuxu existuje možnost plánování (lze vybrat při kompilaci jádra), která pracuje s časovačem trochu jinak. Místo toho, aby časovač tikal pravidelně, nastaví jej plánovač vždy na určitou dobu, tedy dobu, po kterou má běžet následující proces. Po probuzení a vybrání dalšího procesu se nastavení časovače opakuje a pak zase a zase a zase …         
	Dříve se používal tzv. kooperativní multitasking. Tento přístup se choval zhruba tak, že každou úlohu nechal běžet po pevně daný čas. Pokud úloha nic nedělala, třeba čekala na vstup uživatele, byla efektivně promarněna doba, po kterou proces běžel. V takovém plánovači měl proces pouze dva stavy, běží, nebo neběží.     
	Dnes jsme už trochu pokročily a využíváme něčeho, čemu se říká preemptivní multitasking. Ten má typicky více různých stavů, liší se od OS k OS. My si zde rozebereme pár základních, které v nějaké formě velmi pravděpodobně existují v každém z nich. Právě tato rozmanitost různých stavů nám dovoluje využít procesorový čas co nejefektivněji.     
	Prvním stavem, na který se podíváme, je stav running, tedy běžící. Když proces běží, je aktivně zpracováván nějakou výpočetní jednotkou. Tou může být třeba jádro procesoru. V počítači může běžet tolik procesů, kolik máme výpočetních jednotek. Např. v procesoru s osmi jádry, každé po dvou vláknech, máme 2*8 výpočetních jednotek, tedy 16 procesů naráz. Stojí za zmínku, že pokud v takovém případě přijde přerušení, přeruší pouze jednu výpočetní jednotku.      
	Dalším stavem je stav ready, tedy připravený. Připravené jsou procesy, které by mohly běžet, ale brání jim v tom pouze to, že právě nejsou na řadě. Pokud se neděje nic zvláštního, plánovač přepíná mezi úlohami ready a running.      
	Zajímavým stavem je stav waiting, čili čekající. Čekající je proces, který potřebuje ke svému dalšímu postupu něco, na co musí, teď se držte, čekat. Tím mohou být třeba data z disku, uživatelský vstup, data ze sítě … Procesy ve stavu waiting jsou uloženy kdesi na haldě a jsou probuzeny pouze tehdy, dočkají-li se jejich požadovaných dat, resp. toho, na co čekají. V normálním případě by taková úloha byla opět přivedena do stavu ready a čekala by, než nadejde její čas. V Linuxu dnes existuje tzv. asynchronní plánování. To jest, když systém přivede úlohy ze stavu waiting do stavu ready, podívá se, jestli dost vysokou prioritu na to, aby ji pustila hned, tedy hned dala do stavu running. Pokud ano, udělá tak, v opačném případě pokračuje v přerušené úloze.     
	Teď se dostáváme ke konceptu priority procesu. V Linuxu mají všechny procesy defaultní prioritu 0, čili jsou všechny zpracovávány férově. V Linuxu lze procesu prioritu snížit příkazem nice. Aby to náhodou nebylo matoucí, tak čím vyšší číslo, tím nižší priorita. Root uživatel může prioritu i zvýšit, tedy udat zápornou prioritu. O konceptu uživatele root si povíme v kapitole o uživatelích. Plánovač pak přednostně zpracovává úlohy s vyšší prioritou. Ve starších systémech bylo možné třeba nastavit prioritu nesmyslně vysokou a proces tak efektivně zabil celý systém, protože nemohl být vypnut kernelem, jehož procesy neměly dost vysokou prioritu, tudíž nikdy nemohly běžet. Takových incidentům zabraňuje v Linuxu tzv. dynamická priorita. Linux tedy vnímá nastavenou prioritu jen jako jakousi defaultní. Pokud proces běží moc dlouho, prioritu mu sníží, pokud dlouho neběží, prioritu mu zvýší. To je proto, aby mohli běžet i procesy s nižší prioritou. Priorita daným procesům zase opět stoupne, resp. klesne a vše se opakuje.        
	Posledním stavem procesu, který si představíme, je stav suspended. Do tohoto stavu může se může proces dostat například za pomoci uživatelské zkratky CTRL-Z, která vyšle procesu v shellu jakýsi signál (o těch si řekneme záhy). Suspendovaný proces zůstává v systému, není však aktivně zpracováván plánovačem. Oprávněné úlohy mohou proces opět dostat do stavu ready, nebo ho rovnou ukončit.        
	Pozn.: V shellu existuje něco, čemu se říká job management … nebudeme se jím více zabývat, ale v příkazech mám pár příkazů, které je využívají.
	Když jsme si teď představili funkci plánovače a stavy procesů, je na čase, podívat se na práci s procesy v Linuxu. A možná by se nám hodila nějaká ta jednoduchá definice procesu, ať se v tom zbytečně netopíte. Proces je nějaký program, který právě běží. Resp. takový program může mít těch procesů více. Může mít i několik vláken, což je v Linuxu takový speciální případ procesu. Rozdílům mezi vlákny a procesy se zde nebudeme trápit. Důležité je vědět, že každý takový běžící proces má svou speciální strukturu, která je udržována v paměti. Jsou v ní všechny informace o daném procesu. Třeba jeho vlastník, oprávnění, priorita, stav, … Každý proces má také své PID, které ho unikátně identifikuje.       
	Možná jste někdy narazili na výraz daemon. Předem upozorňuji, že nemá nic společného s démonem, kterého můžete vyvolat pomocí rituálu, nýbrž je to pouze označení pro proces, který běží na pozadí, např. logovací daemon.      
	Zde nastal čas si rychle zmínit systémové služby. Služba je nějaký proces nebo aplikace, která běží na pozadí nebo na něco čeká. Služby spravuje systém init, dnes většinou SystemD. Služby lze zapínat, vypínat, povolovat zapínaní při spuštění, zakazovat ho. Lze proházet logy služby a mnoho dalšího. To vše je přístupné přes příkazy správy služeb. Typickými příklady služeb mohou být httpd, mysqld, sshd, …       
	Existuje proces, který má PID 1. Ten by se měl nacházet v souboru /sbin/init a je to buď klasický init, nebo SystemD. Tento proces je jako jediný vytvořen kernelem a je jakýmsi pomyslným pradědou všech ostatních procesů. Z bezpečnostních důvodů nemůže přijímat běžné signály, které si objasníme za chvíli.       
	Každý proces, až na proces init, může vzniknout jedině tak, že naklonuje z jiného procesu. K tomu se v unixových systémech tradičně využívá systémová funkce fork(). Proces, ze kterého forkujeme, se nazývá tatínek, výsledný proces se nazývá synek. Fork tedy vytvoří kompletní kopie procesu, ten pak funguje sám o sobě. Jediný způsob, jak v kódu oba procesy odlišit, jest, že tatínkovi vrátí fork() PID synka a synkovi vrátí fork() nulu. Pokud vrátí fork() záporné číslo, při vytváření nového procesu nastala chyba.       
	Dalším často používaným systémovým voláním je exec() a jeho varianty. Exec má několik parametrů a nahradí data cílového procesu daty programu, který specifikujeme. Proces, který vytvoříme pomocí forku, tak může zpracovávat i jiný program než jeho tatík.       
	Poslední systémovou funkci, kterou si zmíníme, je clone(). Pro značné zjednodušení si povíme, že při určitých parametrech funguje jako fork(), ale má např. možnost také tvořit vlákna atp. Linuxem je volána s danými parametry místo starší funkce fork(). V kódu ale fork() samozřejmě stále použít můžeme, systém si ho následně přeloží sám.       
	Poslední věci, kterou si ve spojení s procesy zmíníme, je tabulka signálů a samotné signály. Každý proces má ve své popisné struktuře také několik bitmap a tabulek. Jedním z nich je bitmapa signálů, které blokuje, dalším bitmapa signálů, které přijal, nebo tabulka uživatelských handlerů na tyto signály. Procesy ovšem obsahují ještě další informace, ale na ně tu nemáme čas. Bitmapa je struktura, kde máme jedničky a nuly za sebou a podobně jako u flagů, každá jedna z nich něco znamená. Třeba v případě blokovaných systémů určuje, zda je signál blokován, nebo ne. Nutno podotknout, že ne všechny signály lze blokovat.     
	Co to vlastně ten signál procesu je? Nu, zkrátka a dobře můžeme běžícímu procesu poslat jakýsi signál, který mu něco poví, např. že se má ukončit. Většina signálů může mít uživatelsky definovaný handler a toho se hojně využívá. Představíme si pár signálů. Čirou náhodou všechny se nějak vážou k ukončení procesu, to byla ostatně jejich původně jediné zamýšlené využití. Pamatujte ale, že signálů existuje velké množství a že mohou dělat u různých programů různé věci. Signál lze poslat pomocí příkazu kill nebo pkill.       
	Pomocí klávesy CTRL-C můžeme vyslat právě běžícímu procesu v shellu signál SIGINT. Ten zdvořile poprosí proces, aby se ukončil. Podobně funguje i signál SIGTERM.
	Dalším signálem může být SIGQUIT. Ten proces varuje, že jestli je okamžitě neukončí, bude zabit a může přijít o data.       
	Posledním signálem, o kterým si povíme, je SIGKILL. Ten zkrátka a dobře zabije proces. Nedá se nahradit uživatelským handlerem ani blokovat.        
	A konečně jsme u našeho posledního tématu. Správa paměti v Linuxu. Těžko říct, kolik toho bude u maturity třeba, nicméně já se vám zde pokusím poskytnout trochu hlubší vhled do obecné správy paměti v moderních operačních systémech, abyste se při nejhorším měli čeho chytit.       
	Budu předpokládat, že otázka míří právě na operační paměť systému, tedy RAM. To je nestálá paměť, kterou systém využívá k uchovávání věcí, ke kterým potřebuje rychlý přístup. Programy, procesy, kernel, … My si tu společně jen opravdu velmi povrchově nastíníme, jak se procesům přiděluje paměť v moderních operačních systémech a objasníme si pojem swap. V operačním systému se nachází něco, čemu se říká VMM (Virtual Memory Manager). Pojem virtuální paměť si nebudeme objasňovat. Stačí nám vědět, že se stará o správu paměti.        
	V mikroprocesoru se nachází další důležitá jednotka, tou je MMU (Memory Management Unit). To je čip, který má za úkol překládat logické adresy na fyzické. Malá lekce hardwaru, procesor je spojen s pamětí RAM pomocí několika sběrnic. Datová, dnes typicky 64bit. Ta je oboustranná a putují po ní .. well, data. Adresová, ta vede z procesory do paměti a udává paměťový blok, který chceme přečíst. Dále je tam ještě sběrnice řídící, ale tu necháme být. Na adresové sběrnici se nachází právě MMU.     
	MMU pracuje se stránkami paměti. Ty se dají určit počtem překládaných bitů adresy a závisí na velikosti paměti RAM. Typicky mohou být velké třeba 4KiB. Všechny stránky jsou stejně velké. MMU jednoduše mapuje logické stránky generované procesorem na stránky fyzické.       
	Jak to vlastně všechno funguje? Mělo by to být plus minus takhle. Proces požádá systém o množství paměti. Systém mu přislíbí danou paměť, nealokuje ji však hned. Alokuje pouze to, co proces aktuálně využívá. Proces se tedy chová skoro tak, jako by byl v RAM jediný, protože vidí a má přístup pouze ke svým logickým adresám. Ty jsou normálně za sebou. Logické paměť procesu je samozřejmě také stránkována, aby mohla být překládána a mapována na fyzické adresy v paměti. RAM totiž přirozeně logickým adresám nerozumí.     
	Pozn.: Fyzické stránku za sebou být nemusí a často také nejsou.     
	Pozn.: Pokud tak úplně nerozumíte konceptu stránek, nevadí, nemám zde dost prostoru tento koncept vysvětlit podrobněji. Pokud vás to zajímá, na internetu je spousta výborných zdrojů.
	Nyní si vysvětlíme, co se stane, pokud si proces vyžádá další stránku paměti. Procesor vygeneruje logickou adresu, pošle ji do MMU, ta se podívá do svých tabulek, zjistí, kde je stránka namapována a do paměti RAM vyšle příslušnou fyzikou adresu.       
	Může se také stát, že adresa, kterou proces žádá, ještě nebyla namapována. V tomto případě se stránka musí nejdříve namapovat. Procesor tedy jako v prvním případě vygeneruje logickou adresu, MMU se podívá do svých tabulek a zjistí, oh no, kde nic, tu nic. Vyvolá tedy Segmentation Fault (pomocí kontrolní sběrnice), ta doputuje do procesoru, ten spustí VMM, který obslouží Segmentation Fault jedním ze dvou způsobů.     
	Pokud byla adresa, o kterou si proces žádá, opravdu procesu přislíbena, namapuje volnou stránku v paměti a vrátí se do procesu, který zkusí o adresu zažádat znovu, tentokrát úspěšně. Samozřejmě se může stát, že všechny stránky v paměti již jsou alokované .. v tomto případě přichází na řadu swap, o kterém si povíme za chvilku.     
	Druhá možnost nastává, žádá-li si proces adresu, kterou si vytáhl někde z prdele a nemá na ni právo. V tomto případě VMM předpokládá, že procesu buď úplně mrdlo a je tedy vadný, nebo že je pro tento neoprávněné chování přímo naprogramován. A protože VMM je spravedlivý, a ví, že takový proces si zkrátka dovolil moc a takové chování je naprosto nepřípustné, v obou případech udělá to samé. Zavolá plánovač, aby proces zabil. Plánovač proces samozřejmě bez reptání nekompromisně ukončí.       
	Poslední věc, které se dotkneme, je paměť swap. To jest paměť, která je uložena v oddílu na disku, typicky se jménem swap, který se doporučuje udělat dvojnásobný ku operační paměti. Využívá se, pokud dojde místo v paměti RAM. V tuto chvíli je vybrána stránka, která bude přesunuta do swapu, většinou stránka, která dlouho nebyla použita. Tato stránka je odmapována a uložena do swapu, nová stránka je namapována na její místo. Ještě předtím se místo stránky většinou vynuluje, aby se zabránilo úniku dat z předchozího procesu. Pokud je stránka ve swapu opět potřebována, je přesunuta zpět do paměti, popř. I vyměněna za jinou stránkou, pokud je paměť plná.        

Příkazy
---
apt update  
Updatuje repositáře

apt upgrade     
Updatuje systém

apt install     
Nainstaluje balíček

apt remove      
Odinstaluje balíček

man bootup      
Ukáže informace o startu systému

jobs        
Ukáže suspendované joby nebo joby na pozadí spuštěné akutálním shellem

fg      
Dá job do popředí

bg      
Dá job do pozadí

&       
Pokud přidáme za příkaz ampersand, spustí se na pozadí


man bash        
Ukáže spoustu zajímavých informací o jobech

nice        
Sníží, nebo zvýší prioritu procesu

ps      
Ukáže spuštěné procesy

ps aux      
Ukáže všechny spuštěné procesy na systému

top     
Real timově ukazuje běžící procesy a využití komponentou

kill        
Pošle procesu signál na základě PID

pkill       
Pošle procesu signál na základě jeho jména

systemctl       
Lze ho užít pro vykonání různých příkazů se službami

journalctl      
Pro prohledávání logů služeb

cat /proc/meminfo       
Zobrazí informace o paměti

free -h     
Zobrazí informace o volné paměti v čitelných jednotkách

Materiály
---
Linux kernel source tree - https://github.com/torvalds/linux        
Linuxový kernel ke stažení - https://www.kernel.org/        
Docs k linuxovému kernelu - https://docs.kernel.org/#       
Shawn Powers – What is Linux … - https://www.youtube.com/watch?v=meAGfhD3_ww        
Open source initiative (open-source definice, licence) - https://opensource.org/        
Shawn Powers – Open source licensing - https://www.youtube.com/watch?v=x_U9Rkc3TmI      
Tanenbaum – Operating Systems Design and Implementation - https://csc-knu.github.io/sys-prog/books/Andrew S. Tanenbaum - Operating Systems. Design and Implementation.pdf       
Tannenbaum – Modern Operating Systems:
https://csc-knu.github.io/sys-prog/books/Andrew S. Tanenbaum - Modern Operating Systems.pdf     
Ing. P. Troller Síťové operační systémy, díl 1 - https://www.youtube.com/watch?v=LDaZ8bij4tw        
Linux systém boot podle Red Hatu - https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/4/html/reference_guide/ch-boot-init-shutdown#s1-boot-process-basics        
Linux systém boot podle SUSE - https://documentation.suse.com/sles/12-SP5/html/SLES-all/cha-boot.html       
Shawn Powers – The Linux Boot Process - https://www.youtube.com/watch?v=esH6GUjVa8Y&t=1206s     
UEFI Specification - https://uefi.org/specs/UEFI/2.10/01_Introduction.html      
David Hand – Linux initramfs for fun, and, uh… - https://www.youtube.com/watch?v=KQjRnuwb7is        
Ing. P. Troller – Síťové operační systémy, díl 2 - https://www.youtube.com/watch?v=YyAZh2bYxXM      
Ing. P. Troller – Síťové operační systémy, díl 6 - https://www.youtube.com/watch?v=nL-GvxoZh6c      
Shawn Powers – Kill Signals - https://www.youtube.com/watch?v=vNajVby4qrM       
Shawn Powers – Finding Process IDs - https://www.youtube.com/watch?v=rK5jLH1cyLk        
Shawn Powers – Why Zombies cant be killed - https://www.youtube.com/watch?v=dl0e6GxO9DI     
Shawn Powers – Linux Package Managers - https://www.youtube.com/watch?v=wR6j4uXVm8U     
SystemD služby podle SUSE - https://documentation.suse.com/smart/systems-management/html/systemd-management/index.html      
Linux služby podle LYM - https://lym.readthedocs.io/en/latest/services.html     
Linux procesy podle LYM - https://lym.readthedocs.io/en/latest/processes.html       
Linux Fundamentals, Paul Cobbaut - https://linux-training.be/linuxfun.pdf       
	Kapitoly – 1, 2, 3