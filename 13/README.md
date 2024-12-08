OSI 3 - Síťová vrstva - Statické směrování, tabulky, statické vs. dynamické směrování, administrativní vzdálenost
===

Přehled
---
Supr otázka, směrování není složitý koncept. Zde dokonce nemusíte vyprávět ani o protokolech pro dynamické směrování.

Povídání
---
V předchozích otázkách jsme si říkali, že routery rozhodují, kam pakety pošlou. Jak to vlastně dělají? Využívají něco, čemu se říká směrovací tabulka. Ve směrovací tabulce je zapsáno, kam (na jakou síť, nebo kterým portem) se má paket poslat na základě jeho cílové IP adresy. Pokud se ve směrovací tabulce nenajde záznam, do kterého by spadala cílová IP adresa, paket je zahozen.     

![Cisco Routing Table](routing_table.jpg)

Dobře, to bychom měli suchou teorii, teď trochu polopatě. Směrovač sám o sobě neví, kam má paket poslat. Resp. zná jenom sítě, které využívají jeho porty. Pro tyto sítě je v routovací tabulce automaticky vytvořen záznam označený písmenem C (connected). Takže jakmile přiřadíme nějakému portu IP adresu, je ve směrovací tabulce vytvořen záznam pro tuto síť. S tohoto důvodu nepotřebujete konfigurovat routing, pokud mezi sítěmi využíváte pouze jeden router. Co znamená to L, ptáte se? Že to vypadá jako úplně stejný záznam? Ne tak úplně, podívejte se pořádně. Správně, má masku */32*. Co to znamená? To je zkrátka a dobře IP adresa portu, ke kterému je připojena daná síť.

![Directly Connected and Local routes](routing_table_directly_connected.jpg)

Ještě předtím, než se posuneme na další typy routů, si vysvětlíme, jak se orientovat ve směrovací tabulce. Na Cisco zařízeních je vždy první nějaké písmeno velké obecedy. To říká, jakým způsoběm se router záznam naučil. *C* a *L* je pro přímo připojené sítě, *S* je pro statický route, *R* je pro protokol RIP, *O* pro OSPF atd.            
D

![Route Breakdown](route_breakdown.jpg)

Materiály
---
Jeremy's IT Lab - Routing fundamentals - https://www.youtube.com/watch?v=aHwAm8GYbn8        
Jeremy's IT Lab - Static Routing - https://www.youtube.com/watch?v=YCv4-_sMvYE      
Cisco Press - Static Routing - https://www.ciscopress.com/articles/article.asp?p=2180209&seqNum=4
