OS Linux - Tvorba skriptů (BASH). Práce s textovými soubory, filtrace, regulární výrazy, kompilace, zálohování
===
Přehled
---

Setsakra rozsáhlá otázka, nevím, jak to tohle může nějak chytře vejít do 15 minut, ale dobrá, alespoň máte spoustu prostoru manévrovat. Určitě doporučuji si připravit nějaké příklady příkazů a skriptování.

Povídání
---

Myslím, že v případě BASH scriptingu bude lepší, když se podíváte do materiálů a zjistíte si vše tam. Pochopíte to určitě lépe, než když tu budu vypisovat jednotlivé možnosti, jak napsat loop, if atd.
	Nicméně práci s textovými soubory už si trochu objasníme. Budu si plus minus držet knihy od Paula Cobbauta, která je i v materiálech, ale budu si to říkat podle svého. Nejdříve pár základních charakteristik práce se soubory v Linuxu. Všechny soubory jsou case-sensitive. Takže Ahoj.txt není ahoj.txt. Zvykejte si, Vidláci. Ačkoliv v tato charakteristika souborového systému ext4 se dá, pokud to jádro podporuje, vypnout, nevím, proč byste to proboha dělali. Další důležité pravidlo zní: „Všechno je soubor. Všechno!“. Adresář? Soubor. Váš disk? Soubor. Unix socket? Soubor. Moje klávesnice přece nemůže být soubor .. nebo ano? To si pište, soubor.     
	Dalším rozdílem oproti Vidlím je, že Linux vám, alespoň ve většině případů, solidně sere na přípony souborů. Už žádné .txt, .svg, .whatever. Soubor je soubor a jeho kontent se dá prohlížet tak či onak, i když je třeba binární a nemůžeme ho přečíst. Nicméně je takovou nepsanou konvencí v Linuxu i přesto přípony používat, ať už kvůli přehlednosti, tak kvůli některých programům, třeba kompilátorům, které tuto vlastnost aktivně využívají.      
	V Linuxu existuje několik typů souborů, základní rozdělení jsme si už řekli v otázce o souborových systémech. Kdybyste chtěli vědět typ souboru, aniž byste se do něj chtěli koukat, příkaz file vám to mile rád poví.      
	Pokud chcete soubory vytvářet, v Linuxu existuje nespočet způsobů. Nicméně jedním z nejpoužívanějších je příkaz touch, originálně vytvořený ke změně časové známky na souboru. Velmi vtipný je také zněný příkazu pro ukázání manuálové stránku tohoto příkazu .. velice mužné man touch. Jediný příkaz, který je ještě mužnější, je man man. Příkaz touch zkrátka vytvoří prázdný soubor, lze mu nastavit timestamp, kdyby jste chtěli … zatím jsem neviděl nikoho, kdo by chtěl, ale můžete.      
	Další užitečný příkaz je rm. Jednoduše odstraní soubor. Existuje i jeho ekvivalent pro adresáře .. resp. prázdné adresáře, rmdir. Nicméně pamatujete na to, že v Linuxu je vše soubor, že? No, tak příkaz rmdir vůbec nepotřebujeme, stačí použít příkaz rm -r. Ten dovede odstranit dokonce i adresář, ve kterém něco je. A pokud by vám to stejně nešlo, rm -rf to kryje. To f tam znamená forcefully. Pokud jako uživatel root provede rm -rf /, nebo rm -rf /* na některých odvážnějších Linuxech, můžete si spolehlivě odjebat celý souborový systém. Hezké, že?
	Pozn.: Pro vytváření adresářů je příkaz mkdir.      
	Příkazem cp se dají soubory kopírovat, příkazem mv přesouvat nebo přejmenovávat. Bum, to je všechno, zbytek si najděte na manuálových stránkách.
	Tak, teď to ještě nebyla pořádná práce s textovými soubory, tu začínáme teprve teď. Začneme dvěma za sebe mluvícími příkazy, head a tail. Head dokáže vypisovat řádky souboru odshora, tail odzdola … to je fakt všechno … nic víc k tomu není, zkuste si to klidně.        
	Dalším velmi zajímavým příkazem je cat. Bohužel nemá nic společného s kočkou, je to pouze zkratka pro concatenate. Hojně se využívá k vypsání obsahu souboru na obrazovku .. dokáže vypsat dokonce i více souborů najednou … k čemu že to je dobrý? Kromě toho vypsání na obrazovku samozřejmě. Dobrá otázka. Abych na ni mohl odpovědět, musíme si objasnit pár základních konceptů v Bashi, STDIN, STDOUT a STDERR.       
	STDIN je stream … jak to říct česky … proud? Zůstaneme u toho streamu. Nicméně to je basically vše, co je chápáno jako input. Třeba všechno z naše klávesnice je STDIN.
	STDOUT na druhé straně je všechno, co Bash ukazuje na obrazovce. Takže všechno z naší klávesnice jde na STDOUT.
	Poslední je STDERR, který, jak název vypovídá, je určen pro chybová hlášení. Běžně je také přesměrován na výstup v terminálu, takže běžný uživatel nepozná rozdíl mezi STDOUT a STDERR. Nicméně výstup z STDERR, stejně jako z ostatních streamů, jak si za chvíli ukážeme, se dá přesměrovat, třeba do souboru. Čehož se běžně využívá.
	Když jsme si to teď tak pěkně vysvětlili, co teda ten cat vlastně dělá? Kopíruje STDIN na STDOUT.       
	Jak toho můžeme využít? To si teď ukážeme. Bash nám totiž poskytuje hned několik možností. První z nich je přesměrovat STDOUT pomocí operátoru >. To nalevo půjde do souboru napravo … to je všechno. Pokud použijeme tento operátor >>. Bash soubor před přesměrováním nepřemaže, ale přidá to nakonec souboru. Můžeme také použít 1>. Protože STDOUT je stream číslo 1.
	STDERR má číslo 2, také 2> je schopné ho přesměrovat. Pokud chceme přesměrovat STDOUT a STDERR do stejného souboru, lze využít 2>&1.
	Pozn.: Určitě jste někde již viděli STDERR přesměrovaný do /dev/null. Co to je? To je řekněme takový odpadkový koš systému. Co tam pošleme, už se nikdy nevrátí. Podobný koncept je /dev/zero, avšak ta nám poskytuje neomezený počet nul.      
	Dalším velmi silným konceptem v Linuxu jsou trubky, pipes. Trubka přesměruje výstup jednoho programu do vstupu druhého, toť vše. Velmi užitečné.
	Lze také přesměrovat STDIN. Můžeme tak učinit pomocí <. Nejlépe zjistíte, co přesně to dělá, pokud si to opravdu vyzkoušíte. STDIN je stream 0, takže můžeme použít i 0<. Co tedy pak dělá příkaz <<? Nu, pokud za tímto znakem specifikujeme třeba nějaký string, řekneme programu, do kterého načítáme, aby ukončil načítání, jakmile narazí na tuto sekvenci. Existuje i <<<, které funguje v podstatě jako trubka, ale trubky jsou víc cool a pravděpodobně i flexibilnější, takže to nemusíte řešit.       
	Trochu to zrychlíme, tohle všechno si stejně musíte vyzkoušet sami. Další užitečné příkazy jsou more a less … less je novější a lepší. Dají nám možnost procházet delšími texty a souboru. Mají i několik užitečných zkratek, třeba / pro vyhledání sekvence znaků.     
	Když jsme si teď tak pěkně ukázali, jak dostat výstup jednoho příkazu do druhého, hodilo by se taky vědět, jak řetězit příkazy, které na sebe nutně nenavazují vstupem a výstupem. Naprosto naivní možností je mezi dva příkazy vložit středník. Ten zkrátka oba příkazy provede, jako byste je zadali za sebou. Když ale ten druhý závisí na úspěchu toho prvního, je nám z druhého vrácena pouze chybová hláška.      
	Pokud chcete, aby se druhý příkaz provedl pouze a jen tehdy, pokud ten první svůj úkol dokončí úspěšně, lze využít místo středníku dva ampersandy takto &&. Opačného efektu, tedy pokud první skončí chybně, má se provést druhý, lze dosáhnout použitím ||.        
	Pozn.: Nevim, kam to dát, tak třeba sem. V Bashi můžeme pomocí zkratky CTRL+D zaslat terminálu EOF, čili End Of File. Hodí se např. při načítání z STDIN.       
	Pokud před nějaký kontrolní znak, třeba středník, vložíme backslash \, bude tzv. escapován, tudíž nebude interpretován jako kontrolní znak. Pokud s backlashem ukončíme řádek, můžeme příkaz rozdělit do více řádku.        
	Další věc, kterou by bylo dobré zmínit, je rozdíl mezi ‘,“ a `. Single qoutes znamenají, že výraz mezi nimi bude interpretován přesně tak, jak je napsán. Nepovoluje žádné proměnné nebo příkazy.       
	Double quoutes naopak povolují proměnné, takže třeba $PATH, při použití příkazu echo nevypíše $PATH, ale skutečnou hodnotu proměnné $PATH.
	Nakonec backticks. Ty provedou příkaz, který v nich je a vrátí jeho output.     
	Ukážeme si ještě poslední věc, než se posuneme na regulární výrazy a následně konečně k něčemu zajímavému, tedy ke kompilaci a zálohování. Tou věcí je file globbing … sorry, nenašel jsem překlad.     
	V Bashi lze využít některých znaků jako takových žolíků např. při specifikaci jmen souborů. Začneme u hvězdičky. Je využíván, pokud hledáme soubor v nějakém tvaru, třeba File_*. Vyhledá všechny soubory, které začínají File_. Nezáleží na tom, co je za tím.     
	Další znak je otazník. Lze specifikovat název souboru takto File??-something. To vyhledá všechny soubory, které jsou ve stejném tvaru a mají libovolný znak míst těch, které jsme nahradili otazníkem.      
	Poslední jsou hranaté závorky … víte co, to si najděte sami, kapitola 17 to je, ta vám to řekne lépe.       
	Vrhneme se na filtraci a regulární výrazy. První příkaz, který přijde na mysl, je samozřejmě grep. Velmi mocný příkaz, který nám vrátí všechny řádky, na kterých se v souboru, nebo v outputu objevil námi specifikovaný výraz.         
	Program cut dokáže vybrat sloupce ze souboru na základě specifikovaného delimeteru.     
	Příkaz tr dokáže vyměnit znaky ve výstupu za jiné … hrejte si.      
	Příkaz wc vás bohužel neodkáže na nejbližší záchod, nicméně vám rád spočítá řádky, slova, písmena, vlastně co chcete v souboru.     
	Sort nečekaně seřadí výstup podle kritéria.     
	Program sed dokáže editovat data, které mu pošlete na základe regulárních výrazů.
	Awk je další super mocný příkaz pro string processing, dokonce tak mocný, že ho tu ani nebudeme popisovat.
	Pokud chcete vysvětlit regulární výrazy, jděte se podívat na můj Github do souboru s vysvětlením otázek s PV, zde se tím nebudu unavovat.
	Dále nás čeká zálohování, kompilaci si necháme na konec.  Nevím, co přesně si zde pod slovem zálohování představují. Nicméně my si zde povíme o komprimaci, protože ta se při zálohování hojně využívá.     
	Když chceme něco zálohovat, pravděpodobně to chceme někde odložit na delší dobu. Určitě nám to pak ale bude zabírat nějaké místo. To se dá vyřešit pomocí něčeho, čemu se říká komprimace. Existuje ztrátová a bezztrátová komprimace. Nám stačí vědět, že pro soubory se používá ta bezztrátová (logicky). Určitě všichni znáte z Vidlí třeba zip. Ten je na Linuxu také, ale existují tam mnohem efektivnější způsoby komprimace a archivace.     
	K archivaci se využívá program tar. Archivace zjednodušeně znamená uložení množství souborů do jednoho souboru společně se všemi jejich metadaty. Typicky je archivy ještě komprimují.
	V Linuxu jsou k tomu tři různé utility, dnes se používá xz, existuje ale také bzip2 a gzip. Jediný rozdíl, který pro nás bude zajímavý, je, že xz funguje nejlépe a gzip nejhůře. Také má každý z nich svůj program a jiné písmenko při rozbalování souboru z archivu pomocí utililty tar.      
	Tímto způsobem můžeme efektivně zálohovat data s maximální úsporou místa. Samozřejmě je chceme zálohovat jinde, než na našem systému, ideálně fleška, externí harddisk, domácí server, … Dobrý nápad také je, zálohovat věci na více místech, kdyby nám náhodou třeba kleknul i počítač i fleška.       
	Nyní se konečně dostáváme ke kompilaci. Co to ta kompilace vlastně je? No, když máme nějaký program, vezmeme si třeba céčko, my mu rozumíme, nebo bychom alespoň měli, nicméně procesoru nějaký ASCII text nic moc neřekne. To je jeden z důvodů, proč je potřeba program nejdříve zkompilovat. Zanedbáme existenci interpretovaných programovacích jazyků. Pokud tedy napíšeme program, je to nějaký ASCII text. V případě jazyka C, se ještě nejdříve volá koprocesor, který vykoná různé užitečné věci, nicméně proces kompilace může být samozřejmě pro různé jazyky různý. Obecně je to nějaký překlad z jednoho jazyka do druhého, typicky to strojového kódu, kterému počítače už rozumí. V případě jazyka C, je kód obvykle nejdříve převeden do assebmly kódu, následně formátu ELF (Extendable and Linkable Format), ten si nebudeme blíže přibližovat. A pak jsou pomocí nástroje, kterému se říká linker, do souboru nalikovány funkce, které se v samotném programu nenacházejí a jsou třeba v nějaké knihovně.        
	Existují různé kompilátory, pro jazyk C se velmi používá kompilátor GCC. Při jeho volání obvykle nemusíme dělat všechny kroky po jednom, ale všechny udělá za nás a vyplivne pouze binární spustitelný soubor.
	




Příkazy
---

Je jich fakt hodně, nebudu je sem vypisovat, nemusíte si pamatovat všechny, ale asi by bylo supr si připravit nějaký příklad.



Materiály
---

Ing. P. Troller – Síťové operační systémy, díl 9 - https://www.youtube.com/watch?v=gBJFSS1AHJI      
Linux Fundamentals, Paul Cobbaut - https://linux-training.be/linuxfun.pdf       
	- Kapitoly 9, 10, 12, 13, 14, 17, 18, 19, 21, 23, 24, 25, 26
Shawn Powers – Bash Scripting 101 - https://www.youtube.com/playlist?list=PL78ppT-_wOmvUO9OuhpZiAOB0f_eHxjnO        
Shawn Powers – Linux Pipes - https://youtu.be/Kdr0OrCPtyk?si=tAVkD7rJM7AMG4Ad       
