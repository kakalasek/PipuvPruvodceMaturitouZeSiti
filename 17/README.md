DHCP4 - funkce, možnosti konfigurace (rozsahy IP adres, dle MAC adresy), použití
===

Přehled
---

Velmi příjemná otázka. DHCP je jednoduchý protokol a lze o něm mluvit dlouho, navíc určitě můžete zapojit do svého výkladu i nějaké vysvětlení IPv4. Určitě doporučuji si nastudovat konfiguraci DHCP na Cisco routeru.

Povídání
---

Takže, povíme si něco o DHCP. K čemu vlastně slouží a co to vlastně je? Je to protokol aplikační vrstvy, který slouží k dynamickému nastavení IP adresy zařízení. Ovšem může zařízením poskytovat i jiné informace, jako třeba IP adresu brány, Domain Name Server, Time Server, Hostname, ...          
Dobře, kde můžeme takový server najít? Naprosto typicky ho může tyto služby poskytovat router. Třeba právě u vás doma. Sami určitě dobře víte, že když připojíte nové zařízení k síti, nemusítě mu konfigurovat IP adresu, aby mohlo síť využívat, ale je mu přidělena automaticky. To zařizuje právě DHCP. Lze ale samozřejmě tuto povinnost delegovat nějakému serveru.           
Ukážeme si, jak probíhá proces získání IP adresy. Můžete ho vidět na obrázku níže.          
Nejprve počítač vyšle **DHCP Discover**. Tato zpráva je broadcast, protože počítač nezná IP adresu ani MAC adresu DHCP serveru. Tuto zprávu tedy vyšle všem serverům na síti.           
Všechny servery na ni odpoví **DHCP Offer**, ta je typicky zaslána jako unicast přímo zařízení, které chtělo IP adresu. Obsahuje možnou IP adresu, popř. jiné věci (options).       
Zařízení pak odpoví zprávou **DHCP Request**, která je vždy zaslána jako broadcast. Je to tak, protože touto zprávou, zároveň odmítne případné nabídky ostatních DHCP serverů. Vzhledem k tomu, že DHCP Discover je také broadcast, pokud

![DHCP Process](dhcp_process.png)

Materiály
---
Jeremys's IT Lab - DHCP - https://www.youtube.com/watch?v=hzkleGAC2_Y           
NetworkLessons.com - DCHP Messages - https://notes.networklessons.com/dhcp-message-types            
Incognito.com - DHCP Options - https://www.incognito.com/tutorials/dhcp-options-in-plain-english/           
RFC 2131 (DHCP) - https://datatracker.ietf.org/doc/html/rfc2131         
