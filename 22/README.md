Bezdrátové sítě - šíření elektromagnetických vln, antény, druhy modulací, zabezpečení, autentifikace
===

Povídání
---

Začneme úplně od začátku, od fyziky. Nebo ....                   
Dobře, když jsme si teď pověděli nějaký fyzikální základ, můžeme pokračovat. Začneme úplně od začátku. V dávných časech si nějaký pán uvědomil, že když tento zdroj střídavého proudu přivedený na nějakou anténu vypíná a zapíná, generuje nějaké signály. Tyto signály ale neměly žádný specifický tvar, druhá strana je mohla zkrátka buď zachytit, nebo ne. Bylo přes ně tedy možné komunikovat jen pomocí nějakého speciálního jazyka, typicky pomocí morseovky. Tento systém se docela uchytil, bylo možné s ním komunikovat na dlouhé vzdálenosti, byl jednoduchý. Nicméně měl nespočet nevýhod. Jednou z nich byla rozhodně neschopnost vypořádat se s nějakým rušením, jiným signálem.                     
Než se podíváme dál, vysvětlíme si, spíš polopaticky než vědecky, co se může signálu stát, když tak putuje vzduchem.            
**Absorbce**. Můžeme mít v cestě třeba nějakou stěnu, ta dovede signál absorbovat, ztlumit. To je významné především u signálů vyšších frekvencí. Tyto signály tak hůře penetrují skrze zdi a nehodí se na dálkové vysílání. Absorbovat může i např. kovový povrch. Absorbovaná energie se zpravidla mění v teplo.              
**Reflekce**. Třeba kovový povrch ale dovede i signál reflektovat, tedy odrazit.                
**Refrakce**. Ta nastává, když se vlny dostanou do jiného materiálu než vzduchu. Světlo putuje v různých materiálech různou rychlostí. Signál tak může např. změnit směr. Podobně jako se může i viditelné světlo různě lámat.          
**Difrakce**. Je specifická vlastnost vln. Vlna za úzkým otvorem opět propaguje do všech stran, ne pouze směrem otvoru. Obrázek popíše nejlépe.             

![Difraction](difraction.png)

**Tříštění**. Mluví samo za sebe. V některých případech se nám signál může roztříštit.                  
Dobře, máme možnost, jak komunikovat na dálku. Nicméně morseovka je docela lame, ne? To si řekl i někdo na začátku 20. stol. A opravdu, dovedl přenést analogový signál. Jak toho docílil? Využil tzv. amplitudovou modulaci. Co to je modulace? Modulace, alespoň jak jsem to já pochopil, je zakódování informace do nosné vlny. Co je nosná vlna? To je vlna pevně dané frekvence. Vysíláč do ni zakóduje informaci a přijímač zná frekvenci této vlny a dovede z výsledné vlny informaci dekódovat.                 

![AM](am.jpg)

Provádíme-li amplitudovou modulaci, výsledný signál získáme složením nosné vlny a našeho signálu. Nosná vlna změní amplitudu podle průběhu našeho signálu. Tato modulace se stále v některých aplikacích využívá. Nicméně většině světa vládnou efektivnější modulace.          
FM funguje podobně jako AM, nicméně místo amplitudy mění frekvenci podle našeho signálu.        

![AM and FM](am_fm.gif)

Existuje ještě PM modulace, neboli fázová modulace. Tu si nebudeme více vysvětlovat, nicméně je základem moderních typů modulací jako QPSK, 8-PSK a QAM.                    

Materiály
---

Jeremy's IT Lab - Wireless Fundamentals - https://invidious.jing.rocks/watch?v=zuYiktLqNYQ          
Jeremy's IT Lab - Wireless Architectures - https://invidious.jing.rocks/watch?v=uX1h0F6wpBY             
Jeremy's IT Lab - Wireless Security - https://invidious.jing.rocks/watch?v=wHXKo9So5y8
Jeremy's IT Lab - Wireless Configuration - https://invidious.jing.rocks/watch?v=r9o6GFI87go                 
Computer Science Lessons - Wireless Communication - https://www.youtube.com/watch?v=OLsbONSQFUI&list=PLTd6ceoshprdYxPhsPcR6yWLMeqsU3t9M         
