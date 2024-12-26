Bezdrátové sítě - šíření elektromagnetických vln, antény, druhy modulací, zabezpečení, autentifikace
===

Přehled
---

Neskutečně rozsáhlá otázka, bezdrátové sitě jsou velké téma. Nicméně z mého pohledu rozhodně ne úplně triviální, nepřeju vám ji. Ale pokud ji náhodou dostanete, určitě to nějak zvládnete.

Povídání
---

Ještě předtím, než se podíváme, jak vypadájí elektromagnetické vlny a jejich modulace, podíváme se na nějaké specifikace bezdrtátových sítí. V první řadě, narozdíl od drátových sítí, AP (Access Point) funguje spíše jako hub než switch. Všechna zařízení v okolí mohou odchytit jakýkoliv vyslaný signál. To si můžete ostatně sami vyzkoušet, pokud si přepnete síťovou kartu do monitor módu. Šifrování zde je tedy velmi důležité.           
V jednu chvíli může také mluvit pouze jedno zařízení, aby nedošlo k interferenci a ztrátě dat. Toho se dociluje pomocí CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance). Jednoduše řečeno, zařízení připojené k síti počká, než ostatní dokončí vysílání a pak začne vysílat samo. Kontroluje tedy kanál, na kterém vysílá, a jakmile zjistí, že je volný, začné vysílat.               
Dalšími faktory, které v bezdrátových sítích existují, může být rozdíl mezi kanály v různých zemích. Musíme si také dávat pozor na následující problémy, které by mohli negativně postihnout kvalitu naší bezdrátové sítě.          
Absorbce. Např. stěna může část signálu absorbovat, bude pak na druhé straně slabší.            
Reflekce. Elektromagnetické vlny se mohou odrážet např. od kovových povrchů. Třeba ve výtahu tak můžeme mít horší signál.           
Refrakce. Nastává, pokud signál narazí na materiál, kde putuje jinou rychlostí, třeba na vodu. Může pak změnit svůj směr.           
Difrakce. Tu můžeme pozorovat, pokud jsme dali náš počítač do bodu, který vlny pouze obíhají, nicméně za něj se nedostanou.         
Tříštění. Signál se "roztříští" do různých směrů.       
Pokud tedy umisťujeme AP, musíme vzít tato omezení v potaz.         
Posledním problémem může být rušení od jiných zařízení, které využívají stejný kanál.           
Dobrá, když jsme si teď pověděli, jaké probémy musíme i Wi-Fi řešit, vysvětlíme si elektromagnetické vlny obecně. Vznikají s pomocí střídavého proudu, který je přiveden na anténu. Mohou mít různé vlastnosti, my budeme pozorovat třeba amplitudu a frekvenci. Tyto pojmy bychom již měli znát z matematiky, nebudu je znovu vysvětlovat.             
Radiové frekvence se pohybují od 30Hz do 300GHz. Wi-Fi využívá především dvě pásma z tohoto rozsahu. První se nazývá pásmo 2.4GHz a je od 2.4GHz do 2.4835GHz. Druhé se nazývá 5G band, ten využívá frekvence od 5.15GHz do 5.825GHz.           
2.4GHz pásmo může poskytovat větší dosah a vyšší penetraci zkrz různé povrchy. Nicméně jeho kanály bývají dost obsazené a mohou být pomalé.         
5GHz využívá vyšší frekvence, tím pádem má menší dosah a hůře se mu dostává přes překážky. Nicméně frekvence v tomto pásmu jsou klidnější a 5G poskytuje obecně rychlejší připojení.            
Wi-Fi 6, což je dnes oblíbený standard, zvětšila rozsah 5G i do rozsahu 6G.     
Zmínil jsem pojem *kanál*. Kanály jsou jakési rozdělení pásem. Překrývají se, v evropě jich máme 13. Kvůli překrývání se doporučuje zvolit správné kanály. Třeba 1, 6, 11.              
Existuje nespočet wifi standartů, novější jsou známe i pod názvem Wi-Fi "X" (třeba 6). Nemusíte je znát.            
Bezdrátová zařízení můžeme rozdělit to tzv. service setů. Všechna zařízení v service setu sdílí stejné SSID (Service Set Identifier). To je zkrátka a dobře jméno, které identifikuje service set. Nemusí být unikátní, ale hodí se to.             
Dobrá, teď bychom se mohli zabývat fungováním těchto service setů, nicméně otázka to nezmiňuje, takže se tím nebudeme stresovat.            
Trochu poskočíme a podíváme se rovnou na bezpečnost v bezdrátových sítích.          
Začneme autetikací. Už jsme si řekli, že autentikace je ověření identity uživatele. Lze ji provést třemi základními metodami. Pomocí hesla, pomocí uživatelského jména a hesla nebo pomocí certifikátu.         
Podíváme se na pár metod autentikace v bezdrátových sítích. První z nich je otevřená autentikace. Zařízení zašle request a AP ho přijme, badebum badabem. Sama o sobě není zabezpečená, ale lze ji zkombinovat s jinými metodami.           
Další možností je WEP (Wired Equivalent Privacy). Poskytuje také šifrování. Pro nás stačí vědět, že bychom ho neměli nikdy používat .. NIKDY!           
Existuje také celá plejáda EAP protokolů, kterými se, snad, zabývat nemusíme.           
Bezpečný standard, který bychom měli využívat v domácím prostředí, je WPA2 s AES, nebo WPA3.      
Máme také různé typy antén. Mohou být do všech směrů, parabolické, více směrné. Různé antény mají různé vlastnosti a dají se využít různě.              
Posledním tématem je modulace. ...............................

Materiály
---

Jeremy's IT Lab - Wireless Fundamentals - https://invidious.jing.rocks/watch?v=zuYiktLqNYQ          
Jeremy's IT Lab - Wireless Architectures - https://invidious.jing.rocks/watch?v=uX1h0F6wpBY             
Jeremy's IT Lab - Wireless Security - https://invidious.jing.rocks/watch?v=wHXKo9So5y8
Jeremy's IT Lab - Wireless Configuration - https://invidious.jing.rocks/watch?v=r9o6GFI87go