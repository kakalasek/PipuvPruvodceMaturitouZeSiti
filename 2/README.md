OS Linux - Souborový sytém, formátování, dělení a připojování souborových systémů, účel základních adresářů
===

Přehled
---
Tato otázka má docela pěkně vyhrazené limity, jen těžko můžete odejít od tématu. Z mého pohledu není vyloženě složitá, nicméně téma, kterým se zabývá, dokáže být velmi komplexní. Nicméně k maturitě nebude potřebovat zacházet do detailů, nebude na to ani dost času. My si zde probereme základní souborový systém Linuxu trochu více dopodrobna pro lepší pochopení souborových systémů obecně, k maturitě se by ale mělo stačit zaměřit se na základní charakteristiky a příkazy.

Povídání
---
Pod pojmem souborový systém si můžeme představit dvě věci. Za prvé jistý program v kernelu, který se stará soubory a adresáře a většinou poskytuje pěkné API ostatním programům, tedy jistý level abstrakce. Existují různé způsoby, jak takový souborový systém implementovat, těmi se zde ale nebudeme zabývat. Druhé možné chápání souborového systému, pro většinu asi přirozenější, je souborový systém přímo na disku. Každý takový souborový systém potřebuje svůj specifický řadič. Nicméně jejich idea zůstává plus minus stejná. Snaží se z cylindrů, stop a hlav dělat soubory a adresáře. Toho dociluje mimo jiné pomocí různých metadat na disku, definují ještě např. řešení oprávnění přístupu k souborům, atp. V Linuxu to funguje tak, že si souborový systém (proces v kernelu) pamatuje, jaký soubor je pod jakým souborovým systémem a podle toho využije příslušní řadič.      
	 Příklady takových systémů jsou např. FAT32, exFAT, NTFS nebo ext4. FAT a exFAT jsou staré, přesto však široce podporované FS, hodí se například do přenosných disků, protože fungují v podstatě na každém OS. NTFS je moderní souborový systém, který je používaný především na Vidlých. Linux ho ale se správným řadičem také umí číst a zapisovat do něj. Jablíčka ho umějí jenom číst.  Ext4 je souborový systém primárně používaný na Linuxu, takže právě jím se budeme dále zabývat. Věc, kterou je třeba mít na paměti, pokud pracujeme s Linuxem, je, že všechno je soubor. Adresář je také soubor, jen trochu speciálního typu, I/O zařízení je v systému také reprezentováno jako soubor.     
	Jedna z důležitých vlastností souborového systému ext4 je, že je to tzv. souborový systém žurnálovací. To znamená, že předtím, než začne provádět všechny požadavky, zapíše si je do něčeho, čemu se říká žurnál, a až potom je začne postupně spouštět. Pro každý splněný požadavek si udělá v žurnálu „fajfku“. Proč se to hodí? Např. v případě náhlého výpadku proudu. Klasický souborový systém by mohl být vyhozen z rovnováhy, protože si nepamatuje, kde přestal, ačkoliv existují způsoby, jak jej vrátit zpět do stabilního stavu. Žurnálový souborový systém si na druhou stranu pamatuje, co už stihl a co ne, takže se směle vrátí k práci. Jediná chvíle, kdy se může žurnálovací systém porouchat, je, když se nestihne správně zapsat právě do žurnálu, ale to se stává jen velmi zřídka.       
	Nyní si povíme něco o tom, jak souborový systém ext4 vypadá interně. Budeme se řídit obrázkem níže. Základní rozdělovací jednotkou jsou skupiny. Ty mají nějakou velikost, tu si systém určuje sám. Na začátku každé skupiny existuje několik jednoduchých struktur s různými úděly. První z nich je tzv. Super Block. Obsahuje základní informace jako hranice souborového systému, datum vytvoření, jestli je namontován, … Za ním následuje Block Group Descriptor, tím se zde nebudeme zabývat. Dále Block Bit Map, ta zaznamenává, jestli jednotlivé datové bloky jsou zabrané, nebo volné. 0 znamená volný, 1 pak zabraný. Úplně stejně se chová Inode Bit Map. O tom, co je to inode si povíme za chvíli. Inode table je pole určitého počtu inodů. Za ním následují datové bloky.       
	Disk je blokové zařízení, to znamená, že k datům přistupuje pomocí bloků. Typická velikost bloku může být 4KiB, nebo 8KiB, čili 4096B, resp. 8192B. Pokud uložíme do souboru např. jen a pouze text „Ahoj!“, samotný text zabere jen několik málo bytů. Můžeme si ale povšimnout, že obsah na disku nám klesl o značně větší číslo, třeba právě o 4KiB. To je právě proto, že disk se k datům chová jako k blokům a nejmenší jednotka, se kterou dokáže pracovat, je právě jeden blok dat. Jeden soubor může samozřejmě využívat více datových bloků.       
	Nyní je načase si povědět, co je to vlastně ten inode. Inode je základní popisná jednotka, která slouží k popisu informací o objektu uloženém na disku. Takovými informace může být třeba typ souboru, vlastník, skupina, oprávnění, datum vytvoření, datum posledního zápisu, … Není tam však jméno souboru, to je uložené pouze v adresáři, ve kterém se soubor nachází. V souborovém systému ext4 tedy musí existovat nějaký kořenový adresář, ze kterého vychází všechny ostatní soubory. Pro kořenový adresář vašeho systému je to adresář značený lomítkem. Pokud namontujeme nějaký souborový systém např. do na místo /mnt/usb, stane se tento adresář jakýmsi kořenovým adresářem daného souborového systému. Touto problematikou se ale nebude dál zabývat, důležité pro nás je, že adresáře a soubory v Linuxu jsou řazeny do jakési stromové struktury, všechny soubory jsou spolu spojeny společným kořenem. Od toho se také odvíjí dvě možnosti zápisu cesty k souboru. Relativní, od aktuální pozice v souborovém systému např. directory/file, a absolutní, od kořenového adresáře např. /home/honza/directory/file.        
	Každý soubor musí mít nejméně jeden inode. Kdy a proč může mít soubor více než jeden inode zde nebudeme probírat. Mimo obecné informace o souboru obsahuje inode seznam datových bloků, které jsou souboru přiřazeny. Počet inodů je samozřejmě omezený, takže když se vám zaplní disk, stala se jedna ze dvou věcí. Buď vám došly inody, nebo vám došli datové bloky.      
	Jak vlastně vypadá adresář, když je to také jenom soubor? Jednoduše, je v něm seznam, kde je vždy uloženo jméno souboru a číslo inodu, který mu náleží.     
	Při vytváření souboru nejdříve proběhne tzv. linkování. Souborový systém přiřadí inode k datovému bloku a v bitmapě označí obojí jako obsazené. Tím vznikne klasický link, neboli hard link. Pokud vytvoříme nový hard link k souboru, znamená to, že v adresáři bude ukazovat na stejný inode jako originální soubor. Hard link lze vytvořit jen a pouze ve stejném souborovém systému. V inodu je uložen tzv. počet referencí, resp. položek v adresářové struktuře, které na inode ukazují. Při smazání jednoho z nich se počet pouze sníží. Soubor se tedy smaže jen tehdy, pokud se tento počet sníží na hodnotu 0.        
	Druhým typem linku je tzv. soft linku, v češtině symbolický link. Symbolický link má svůj vlastní inode, který ukazuje na datový blok, ve kterém je cesta k souboru, na který ukazuje. Lze ho použít skrze souborové systémy. Pokud je soubor, na který soft link ukazuje, smazán, soft link zůstane, ale přestane fungovat.        
	Teď jen taková zajímavá vsuvka. V souborovém systému ext4 je možné povolit něco, co se nazývá data inlining. Inode má v sobě trochu volného místa. Data inlining toto místo využije, takže pro velmi malé soubory není potřeba alokovat data blok a data uloží přímo do inodu. To se hodí např. v případě symbolických linků.       
	V Linuxu je několik typů souborů. Typ souboru lze zjistit např. pomocí příkazu ls -l a je to první písmenko u uživatelských práv. Pomlčka – znamená klasický soubor. Malé d je adresář. Malé b je blokové zařízení, např. disk, malé c je znakové zařízení, např. klávesnice nebo myš. Malé l je symbolický link. Malé p je pojmenovaná pipe a malé s je pojmenovaný socket, ani jedno zde nebudeme probírat.       
	Každý disk lze libovolně přeformátovat pomocí příkazu mkfs. Lze formátovat pouze nenamontované disky. Disk lze namontovat to adresářové struktury pomocí příkazu mount. Odstranitelná media jako flešky se typicky montují do adresáře /media, dočasné souborové systémy jako síťové disky se montují do adresáře /mnt. K souborovému systému pak lze přistupovat přes adresář, na který byl namontován. Následně ho lze odmontovat příkazem umount.        
	V Linuxu lze také vytvořit skryté soubory. Ty se vytváří tak, že se před jejich jméno vloží tečka. Tyto soubory pak nejsou vidět v klasickém výpisu adresáře a je nutné použít přepínač -a.
	Poslední téma, kterým se zde budeme zabývat, je účel základních systémových adresářů. Všechny tyto adresáře se nachází v kořenovém adresáři systému.
	Začneme adresářem /bin. Zde se nacházejí základní binární, spustitelné, programy. Třeba ls, passwd, bash, …         
	V adresáři /boot najdeme soubory určené k bootování systémy nebo s ním nějak spojené.       
	Adresář /dev obsahuje raw přístupy k zařízením k systému. Třeba když připojíte nový disk, najdete pro něj soubor právě v tomto adresáři. Tento soubor pak musí být ještě namontován.
	Adresář s trochu zvláštním jménem, /etc, se využívá k ukládání různých konfiguračních souborů. Bydlí v něm třeba konfigurační soubory apache, mysql, soubory passwd, shadow, group atp.
	Dále adresář /home. V něm se ukrývají domovské adresáře všech uživatel, kromě roota.        
	V adresářích /lib a /lib64 jsou uloženy systémové knihovny.         
	Další je speciální servisní adresář souborového systému, /lost+found. V tomto adresáři lze najít třeba osiřelé inody, které vznikly při chybách v nějakém souborovém systému.       
	Do adresáře /media se typicky montují všechny souborové systémy odstranitelných medií, třeba flešky.        
	Adresář /mnt je pro montování dočasných souborových systému, např. nějakých síťových disků.     
	/opt je adresář, který slouží k ukládání různých volitelných balíčků        
	/proc ukládá údaje o právě běžících procesech a obsahuje i nějaké informace o systému, např. o procesoru.       
	Uživatel root má svůj domovský adresář právě v adresáři /root.      
	/run obsahuje tranzientní, na běhu závislé, pracovní soubory, např. sokety pro komunikaci s procesy, …      
	Adresář /sbin je podobný /bin, ale obsahuje spustitelné soubory určené pro uživatele root.      
	/sys je relativně nově implementovaný adresář. Obsahuje různé detailní možnosti práce s jádrem a podobně jako /proc obsahuje informace o systému.       
	/share je adresář se sdílenými informacemi, které může využít celý systém např. překlady …      
	Adresář /tmp, jak jméno napovídá, obsahuje dočasné soubory a je průběžně systémem čištěn. Nepřežije ani reboot systému.     
	/usr je adresář, ve kterém se nacházejí méně kritické utility a programy. Někdy bývají /bin a /sbin symbolické linky na /usr/bin a /usr/sbin, nebo naopak.      
	Posledním adresářem je /var, tam se nacházejí data, která se často mění, třeba logy.        
	
	
Příkazy
---

mkfs        
Vytvoří souborový systém na blokovém zařízení

fsck        
Zkontroluje příslušný souborový systémem

ln      
Používá se zpravidla pro vytvoření hard linku, nebo symbolického linku

ls      
Ukáže obsah adresáře. S přepínačem -l ukáže i mnohem více informací. S přepínačem -a jsou vidět i utajené soubory

mount       
Namontuje nový souborový systémech

umount      
Odmontuje souborový systém

debugfs     
Pro debugování souborového systému

cd      
Změní aktuální adresář

lsblk       
Vypíše všechna bloková zařízení

df      
Ukáže zaplnění souborových systémů, typ, atp.



Materiály
---

Ing. P. Troller – Síťové operační systémy, díl 5 - https://www.youtube.com/watch?v=ZyF0vetfwLo&t=3030s      
Techquickie – File Systems as Fast as Possible - https://www.youtube.com/watch?v=BV0-EPUYuQc        
PowerCert – Windows File Systems - https://www.youtube.com/watch?v=bYjQakUxeVY      
Learn Linux TV – Formatting and Mounting Storage Volumes - https://www.youtube.com/watch?v=2Z6ouBYfZr8&t=192s       
Kernel Docs ext4 - https://docs.kernel.org/filesystems/ext4/overview.html       
Shawn Powers – Filesystem Tools - https://www.youtube.com/watch?v=sweL9An23Vc       
Shawn Powers – Mounting Filesystems in Linux - https://www.youtube.com/watch?v=fspjhtUhlp4      
LYM – File systém mounting - https://lym.readthedocs.io/en/latest/mounting.html     
Shawn Powers – Linux Root Filesystem - https://www.youtube.com/watch?v=tZZinDVu0J8      
Baeldung – What is Rootfs - https://www.baeldung.com/linux/rootfs       
Root Filesystem specification - https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03.html      
Linux Fundamentals, Paul Cobbaut - https://linux-training.be/linuxfun.pdf       
	Kapitoly – 8, 9, 11, 35