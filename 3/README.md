OS Linux - Správa uživatelů, skupin, zabezpečení přístupu k souborům a službám (práva), SUDO
===



Přehled
Z mého pohledu relativně jednoduchá otázka, která se věnuje, jak název vypovídá, uživatelům, skupinám a právům v Linuxu. Otázka má jasně vymezené téma, v podstatě bez možnosti odbočit od tématu.
---

Povídaní
---
Běžní uživatelé počítače s Vidlemi se uživatelskými právy možná nemuseli ani nikdy zabývat, protože jsou sami jediným uživatelem v systému a tudíž i administrátorem. Nemusejí se tudíž o žádná práva a uživatele moc starat. Pokud jsou potřeba práva administrátora, stačí odkliknout tlačítko.       
	Uživatelé a práva v Linuxu hrají zásadní roli, stejně jako ve většině konvenčních operačních systémů, nicméně na rozdíl od Vidlí se s nimi setkáte značně častěji. Linux je multiuživatelský operační systém. To neznamená nic jiného, než že v systému může existovat více než jen jeden uživatel. Bylo tudíž potřeba vymyslet systém práv, který bude jednotlivé uživatele omezovat. Ten si podrobněji vysvětlíme záhy. Samozřejmě si značnou část správy uživatelů vypůjčil z Unixu.     
	Pro fungování systému musí existovat alespoň jeden uživatel. Takový uživatel zpravidla bývá i administrátor. V Linuxu se ale neužívá termín administrátor pro uživatele, který má nejpriviligovanější přístup k systému, nýbrž se mu říká root. Root uživatel je oprávněn v podstatě ke všemu, třeba včetně smazání životně důležitých knihoven a tím efektivnímu rozbití celého systému atp. Root uživatel tedy není v jeho možnostech nijak limitován, stejně tak však nejsou v možnostech limitovány procesy a služby spuštěné pod tímto uživatelem. Pozornější čtenáři už možná vidí problém, pro ostatní objasním. Pokud bychom byli přihlášeni jako root a spustily script, do kterého nám nějaký vtipálek přidal příkaz pro vymazání kořenového adresáře, příkaz by se bez sebemenších problému provedl a efektivně nám rozbil celý systém. Podobná věc se kdysi stala, pokud jste si zkoušeli na Linux nainstalovat Steam, podrobnosti si dohledejte na internetu. Pokud bychom takový script spustili pod uživatelem s omezenými právy, příkaz by pouze vyhodil nějakou výjimku a náš systém by byl dál v pořádku. Důvodů, proč nepracovat pod root uživatelem je tedy spoustu. Existují proto způsoby, jak provádět věci, které vyžadují oprávnění uživatele root, aniž bychom se na něj museli přihlašovat. Moderní distribuce často ani nedovolují nastavit si při jejich instalaci pro uživatele root heslo a nutí vás, abyste používali svého uživatele. O příkazu sudo si budeme podrobněji vyprávět později.
	V Linuxu je uživatel deklarován dvěma základními atributy. USERID a UID. Nejdříve se podíváme na USERID. To je běžný textový řetězec, zkrátka a dobře jméno našeho uživatele. V klasických Unixových systémech bylo často omezeno na osm znaků skládajících se pouze z malých písmen, čísel a velmi omezeného počtu speciálních znaků (např. podtržítka). V Linuxu mohou být uživatelská jména delší a mohou obsahovat i velká písmena. Dávejte si však pozor, jména uživatelů jsou case-sensitive, to znamená, že Pepa není pepa, ani PePa atp. I přesto se však stále doporučuje vytvářet USERID tak, aby splňovalo původní omezení z Unixu, protože např. při dlouhém USERID se mohou některé programy chovat zvláštně.
	UID je naopak 16-bitový integer, který unikátně identifikuje uživatele a řídí se podle něj jeho práva. Mohou tudíž existovat dva uživatelé s jiným USERID a stejným UID. Údaje root uživatele jsou např. USERID = root a UID = 0. Root musí mít vždy UID 0. Protože systém se řídí podle UID, je možné do systému vložit uživatele z root právy, který se schová mezi ostatními uživateli a může si dělat, co chce. Toho lze využít např. při kompromitaci systému.     
	Dalším atributem uživatele je jeho heslo. Já osobně doporučuji zvolit takové, která má alespoň 12 znaků a pamatujete si ho. Např. nějaká vám blízká věta, ve které navíc záměrně uděláte nějakou chybu, ať je těžší ji uhodnout.        
	Atributy a informace o uživatelích jsou uložena ve dvou důležitých souborech. Jedním z nich je /etc/passwd. Ačkoliv to tak může dle názvu vypadat, tento adresář neobsahuje hesla, ačkoliv historicky je obsahoval. Nyní obsahuje USERID, UID, domovský adresář uživatele, výchozí shell uživatele, … Historicky lze uživateli také přiřadit telefonní číslo, kancelář atp. To vše je uložené v /etc/passwd. Je čitelný i pro běžné uživatele.      
	Druhým důležitým souborech, který má co dočinění s uživateli, je /etc/shadow. Ten obsahuje právě hesla uživatelů, samozřejmě v šifrovaném formátu. Mimo hesla v něm najdete třeba informace o tom, jak často si uživatel musí heslo změnit, jak dlouho jeho účet vydrží po nezměnění hesla, …  Na rozdíl od /etc/passwd, /etc/shadow je čitelný jen pro root uživatele.
	V Linuxu máme tak nějak tři základní typy uživatelský účtů. Prvním z nich je již zmíněný root. Druhým z nich jsou tzv. systémové účty. Ty mají několik základních znaků, my se jimi nebude blíže zabývat, nicméně vznikají speciálně pro různé systémové služby atp. Posledním typem jsou klasické uživatelské účty.       
	Nyní se podíváme na uživatelské skupiny. Každý uživatel musí patřit do jedné primární skupiny. To může být například jeho oddělení, katedra, atd. Moderní linuxové distribuce to řeší trochu specificky. Protože distribuce určené pro běžného uživatele nepočítají s tím, že bude stroj využívat více než několik málo uživatelů a nebudou tudíž implementováný žádné zvláštní skupinové vzorce, vytvoří každému uživateli při vytvoření jeho vlastní skupinu, která se jmenuje stejně jako uživatel a patří do ní jen on sám. Uživatel pak může patřit do libovolného počtu sekundárních skupin.      
	Skupina je, podobně jako uživatel, definována dvěma hlavními atributy. Těmi jsou GROUPID a GID. GROUPID je název skupiny, textový řetězec s pravidly po vzoru USERID. GID je opět 16-bitový integer, který unikátně identifikuje skupinu. I skupina může mít přiřazené heslo, to ale není běžné, a nebudeme se tím zde tudíž zabývat.       
	Soubor /etc/passwd obsahuje GID primarní skupiny každého uživatele. Soubor /etc/group pak obsahuje všechny skupiny v systému a uživatele, kteří jsou k nim přiřazeni. Existuje i soubor /etc/gshadow, který obsahuje hesla ke skupinám.     
	Ještě než se dostaneme k právům k souborům a adresářům, zmíníme si příkaz sudo a su. Příkaz su spustí nový shell pod jiným uživatelem. Příkaz su – spustí nový shell pod uživatelem root, vyžaduje heslo uživatele root. Příkaz lze použít i bez pomlčky, avšak to silně nedoporučuji z důvodů, které tu nemáme čas objasňovat. Pro spuštění nového shellu pod uživatelem root lze také použít příkaz sudo -i, ten však vyžaduje heslo aktuálního uživatele. Zde se dostáváme právě k příkazu sudo, který je silně doporučováno používat místo přihlášení se pod uživatelem root. Příkaz sudo se řídí nastavením v souboru /etc/sudoers, který udává, kdo může provádět jaké příkazy pod jakým uživatelem. Pokud vás zajímají podrobnosti jeho nastavení, dám vám něco do materiálů.        
	Nyní se konečně dostáváme k právům k souborům. Nejlépe si to ukážeme na obrázku.        
Např. při použití příkazu ls -l na libovolný soubor, lze ve výpisu najít stejnou část, jakou je vidět na obrázku. První pomlčka určuje typ souboru, pomlčka znamená běžný soubor. První tři písmena jsou práva pro vlastníka, další pro skupinu a poslední pro všechny. Ve výpisu ls -l je ještě vidět jméno vlastníka souboru a jméno vlastnící skupiny. Male r znamená read a naprosto nečekaně udává práva na čtení souboru. Malé w znamená write a opět na prosto nečekaně udává, zda má daný subjekt právo na zápis do souboru. Nakonec malé x udává, zda může daný subjekt soubor spustit jako spustitelný skript.        
	U některých souborů si můžete povšimnout práv u vlastníka, která vypadají takto rws. Malé s znamená SETUID. Toto speciální právo je nutné pro spuštění některých skriptů, programů. Např. pro spuštění programu passwd, který změní uživatelské heslo. Při autorizaci spuštění takového skriptu se řídí normálními právy, avšak po úspěšné autorizaci běží skript pod jeho vlastníkem, kterým může být třeba v případě passwd uživatel root.        
	Lze také nastavit SETGID, pak bude malé s u práv skupiny po vzoru SETUID u vlastníka. Dělá to samé, akorát pro skupiny. To znamená, že po autentizaci spustí skript pod skupinou, která soubor vlastní.
	Pozn.: Pokud není nastaveno právo x pro vlastníka, resp. pro skupinu, místo malého s je zobrazeno velké S, takto rwS        
	Stejná práva mají ale jiný smysl pro adresáře. Malé r pro adresář znamená, že adresář lze prozkoumat, čili číst jeho obsah. Adresář, který by měl nastaveno pouze r, tedy můžeme vypsat pomocí ls, avšak nedozvíme se nic moc jiného, než názvy souborů, které v něm leží. Na otázky proč přesně bohužel nemáme čas odpovědět. Malé w pro adresář znamená, že můžeme v adresáři vytvářet, mazat nebo přejmenovávat soubory. Právo x znamená, že můžeme přistupovat k souborům v adresáři. Můžeme do něj tedy např. udělat cd, číst a upravovat soubory, ke kterým máme práva.
	SETGID bit pro adresáře má specifické chování. Po jeho zadání se všechny soubory v adresáři vytvoří s jeho skupinou.        
	Pro adresáře existuje také něco, čemu se říká sticky bit. Jeho přítomnost je značena malým t u práv pro všechny, takto rwt. Zaručuje, že soubory v adresáři mohou být mazány pouze jejich vlastníky.        
	Pozn.: Podobně jako v případě SETUID a SETGID, pokud není nastaveno právo pro spuštění u práv pro všechny, je zobrazeno velké T, takto rwT.     
	SETUID pro adresáře a sticky bit pro soubory nemájí žádný smysl, proto jsou systémem ignorovány. Nastavit je ale lze i přesto.

Příkazy
---

useradd a adduser       
Rozdíl mezi těmito příkazy je, že useradd je low level příkaz, který pro správné fungování potřebuje určité parametry, adduser je uživatelsky přívětivější skript, který udělá spoustu věcí za nás. Různé distribuce mohou ale mít různé přístupy k používání těchto příkazů.

passwd      
Změní heslo uživateli. Lze použít bez parametrů jako uživatel a změnit tím vlastní heslo, nebo jako root a změnit tím heslo specifikovanému uživateli.

userdel a deluser       
Odstraní uživatele. Userdel je opět low level příkaz, některé distribuce podporují i příkaz deluser, který je uživatelsky přívětivější.

groupadd        
Vytvoří novou skupinu


groupdel        
Odstraní skupinu        

usermod     
Dovede změnit atributy uživatele, přidat uživatele do skupiny, odstranit ho ze skupiny

groupmod        
Dovede změnit atributy skupiny

chown       
Změní vlastníka souboru, nebo adresáře

chgrp       
změní skupinu souboru, nebo adresáře

chmod       
Změní přístupová práva k souboru, nebo adresáři. Ty lze zadávat v několik možných tvarech, podrobný popis bude v materiálech


Materiály
---

Ing. P. Troller – Síťové operační systémy, díl 3 - https://www.youtube.com/watch?v=2T1Q913oYYM      
Shawn Powers – Account Creation and Management - https://www.youtube.com/watch?v=WQ4pdhkiHNM        
Shawn Powers – Creating Linux Users - https://www.youtube.com/watch?v=Y6iE89SbiHg       
FreeCodeCamp – How to manage users in Linux - https://www.freecodecamp.org/news/how-to-manage-users-in-linux/       
Linux Fundamentals, Paul Cobbaut - https://linux-training.be/linuxfun.pdf       
	Kapitoly – 27, 28, 29, 30, 31, 32, 33