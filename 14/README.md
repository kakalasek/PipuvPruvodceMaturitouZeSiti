OSI 3 - Síťová vrstva - Dynamické směrování (RIP, OSPF), konfigurace, metriky, administrativní vzdálenost
===

Přehled
---
Docela pěkná otázka, když si dáte trochu práce a naučíte se řádně fungování OSPF, tak vám rozhodně nedojdou témata. Administrativní vzdálenost jsme si vysvětlili v předchozí otázce, nebudu ji zde opakovat.


Povídání
---
Dynamické směrování. Bylo navrženo pro velké sítě, kde by mohlo být složité udržovat statické routy. Na směrovačích v této síti se spustí jeden z protokolů (RIP, OSPF, EIGRP, případně BGP), nastaví se, na jakých sítích nebo portech mají fungovat, a směrovací tabulky se pak vytvoří dynamicky. Tento přístup má především jednu zásadní nevýhodu. Síť se tak zpomalí, protože směrovače musí odbavovat ještě tento protokol. Typicky mezi sebou směrovače s takovým protokolem zapnutým komunikují a tím vytvaří na sítí další traffic.       
K vybrání těch nejlepších cest využívají různé protokoly různé metriky. Nejjednodušší metriku využívá RIP, který při každém skoku přičte hodnotu 1, takže bere v potaz pouze počet skoků. OSPF už bere v potaz i rychlost jednotlivých propojení. Metrika je tedy jakési kritérium, podle kterého se prokol rozhoduje, jaký route vybrat, má-li jich na výběr více. Jako metriku označujeme číslo, které tímto procesem vzniklo.       
Ještě předtím, než se podíváme, jak funguje OSPF, si povíme, kde se který protokol nejčastěji používá, ať máte nějakou představu. RIP je nejstarší z nich, je neefektivní, zato jde velmi jednoduše nastavit, někdy se pro svou jednoduchost využívá v laboratorních podmínkách. OSPF a EIGRP se využívají běžně. Hlavním rozdílem je, že EIGRP je Cisco priprietary a OSPF je volně dostupný protokol. Proto si budeme říkat, jak funguje právě OSPF. Zajímavým protokolem z této rodiny je BGP. S tímto protokolem se spíš nesetkáte, přesto hraje klíčovou roli ve fungování internetu. Routuje velké bloku IP adres v nejvyšších vrstvách internetu mezi předními firmami na tomto poli. Problematika BGP je mnohem rozsáhlejší, ale my se jím tu nemusíme zabývat, otázka ho přímo nezmiňuje. Existují i různé typy těchto protokolů podle toho, jak mezi sebou komunikují a vytváří svou routovací tabulku. Pokud vás to zajímá, je to zmíněné ve videích v materiálech, já se o tom bavit nebudu.          
Tak, teď se již můžeme podívat na OSPF (Open Shortest Path First). Představení fungování tohoto protokolu nám dá jakýsi vhled do fungování ostatních protokolů tohoto typu. Existují tři verze OSPF. Nás bude zajímat verze 2. Verze 1 je zastaralá a verze 3 byla vytvořena především pro IPv6. OSPF je postavené na tom, že každý směrovač má STEJNOU a ÚPLNOU mapu celé sítě. Routery si mezi sebou posílají LSA (Link State Adverisement) a pomocí nich si vytváří LSDB (Link State Database). Směrovače rozesílají LSA, dokud si všechny nevytvoří stejnou LSDB.

![OSPF](ospf_overview.png)

Materiály
---
Jeremy's IT Lab - Dynamic Routing - https://inv.nadeko.net/watch?v=xSTgb8JLkvs             
Jeremy's IT Lab - RIP & EIGRP - https://inv.nadeko.net/watch?v=N8PiZDld6Zc          
Jeremy's IT Lab - OSPF Part 1 - https://inv.nadeko.net/watch?v=pvuaoJ9YzoI          
Jeremy's IT Lab - OSPF Part 2 - https://inv.nadeko.net/watch?v=VtzfTA21ht0              
Jeremy's IT Lab - OSPF Part 3 - https://inv.nadeko.net/watch?v=3ew26ujkiDI