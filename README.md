# ASIXc2AC--Projecte_P0.0

## 📑 Índex

- [Informació del Projecte](#informació-del-projecte)
- [Descripció](#descripció)
- [Objectius](#objectius)
- [Arquitectura de Xarxa](#arquitectura-de-xarxa)
  - [Esquema d'IPs - Xarxa 192.168.6.X](#esquema-dips---xarxa-1921686x)
- [Hardware Desplegat](#hardware-desplegat)
- [Justificació de l'Estructura i Tecnologia de Xarxa](#justificació-de-lestructura-i-tecnologia-de-xarxa)
  - [Arquitectura General](#arquitectura-general)
  - [Zona DMZ (192.168.6.0/24)](#zona-dmz-19216860024)
  - [Zona Intranet (192.168.60.0/24)](#zona-intranet-192168600024)
  - [Avantatges de l'Arquitectura Proposada](#avantatges-de-larquitectura-proposada)
  - [Flux de Comunicació](#flux-de-comunicació)
  - [Conclusió](#conclusió)
- [Sprint 1 - Configuració Serveis de Xarxa](#sprint-1---configuració-serveis-de-xarxa)
  - [Configuració DNS Server (D-N03)](#configuració-dns-server-d-n03)
  - [Configuració DHCP Server](#configuració-dhcp-server)
  - [Configuració Router R-N01](#configuració-router-r-n01)
- [Sprint 2 - Configuració del Servidor Web](#sprint-2---configuració-del-servidor-web)
  - [Configuració Web Server (W-N02)](#configuració-web-server-w-n02)
  - [Configuració Database Server (B-N03)](#configuració-database-server-b-n03)
  - [Configuració FTP Server (F-N02)](#configuració-ftp-server-f-n02)

---

## Informació del Projecte

**Nom del Projecte:** P0.0-ASIXc2gC-Gnn  
**Durada:** 6 setmanes (fins el 18/11)  
**Sprints:** 3 sprints quinzenals (10h cadascun)  
**Grup:** Eduard, Hamza, Guim, Francesc

## Descripció

Projecte de desplegament d'infraestructura multicapa que inclou:
- Web Server
- Monitor de xarxes
- SSH
- Base de Dades (MySQL)
- DHCP
- DNS
- FTP

## Objectius

- Preparar infraestructura completa multicapa
- Implementar arquitectura de xarxa amb DMZ, Intranet i NAT
- Desplegar serveis de xarxa essencials
- Crear aplicació web de consulta de dades
- Gestionar projecte mitjançant sprints en ProofHub

## Arquitectura de Xarxa

![Diagrama d'infraestructura](./Photos/Sprint%201/estructura.png)

### Esquema d'IPs - Xarxa 192.168.6.X

#### DMZ (192.168.6.0/24)
- **Gateway Router (DMZ):** 192.168.6.1
- **W-N02 (Web Server):** 192.168.6.10
  - Apache/Nginx
  - SSH
  - Aplicació Web de Consulta
- **F-N02 (FTP Server):** 192.168.6.11
  - vsftpd/ProFTPD
  - Transferència arxius

#### Intranet (192.168.60.0/24)
- **Gateway Router (Intranet):** 192.168.60.1
- **Intranet Host:** 192.168.60.20
- **B-N03 (Database Server):** 192.168.60.15
  - MySQL
  - CSV Educación BCN
  - User: bchecker
- **DHCP Server:** 192.168.60.20
  - Pool DHCP: 192.168.60.30-100
- **DNS Server:** 192.168.60.20
  - Resuelve R-N01, R...
- **PC Windows (Cliente 1):** IP DHCP (192.168.60.x)
- **PC Linux (Cliente 2):** IP DHCP (192.168.60.x)

#### Router R-N01
- **Hostname:** R-N01
- **Interfície NAT:** Internet
- **Interfície DMZ:** 192.168.6.1/24
- **Interfície Intranet:** 192.168.60.1/24

## Hardware Desplegat

### Servidors
- **W-NCC:** Web Server + SSH
- **B-NCC:** Base de Dades MySQL
- **F-NCC:** Servidor FTP
- **Servidors de Xarxa:** DHCP + DNS

### Clients
- PC Windows
- PC Linux

## Justificació de l'Estructura i Tecnologia de Xarxa

### Arquitectura General

#### Router Central amb Tres Interfícies (R-N01)
L'arquitectura proposada utilitza un **router amb tres interfícies** que segmenta la xarxa en tres zones diferents:
- **Interfície NAT** (cap a Internet)
- **Interfície DMZ** (192.168.6.0/24)
- **Interfície Intranet** (192.168.60.0/24)

Aquesta segmentació proporciona **aïllament lògic** entre les diferents zones de seguretat, permetent aplicar polítiques de firewall específiques per a cada segment.

---

### Zona DMZ (192.168.6.0/24)

#### Justificació de Serveis Públics

##### Web Server (W-N02) - 192.168.6.10
- **Tecnologia**: Apache/Nginx amb SSH
- **Justificació**: 
  - Servidor web accessible des d'Internet per allotjar l'aplicació web pública
  - SSH permet administració remota segura
  - Ubicat a la DMZ per protegir la xarxa interna d'amenaces externes
  - En cas de compromís, no exposa directament la xarxa interna

##### FTP Server (F-N02) - 192.168.6.11
- **Tecnologia**: vsftpd/ProFTPD
- **Justificació**:
  - Transferència d'arxius des de/cap a l'exterior
  - Separat del servidor web per limitar l'impacte de vulnerabilitats
  - Permet gestió d'arxius sense accés directe al servidor web

#### Avantatges de la DMZ
- **Capa de seguretat addicional**: Els servidors públics estan aïllats de la xarxa interna
- **Control de trànsit**: El router pot inspeccionar i filtrar tot el trànsit entre DMZ i Intranet
- **Contenció d'amenaces**: Si un servidor a la DMZ és compromès, l'atacant no té accés directe a la Intranet

---

### Zona Intranet (192.168.60.0/24)

#### Database Server (B-N03) - 192.168.60.15
- **Tecnologia**: MySQL
- **Justificació**:
  - **Base de dades protegida**: No està directament accessible des d'Internet
  - **Dades sensibles segures**: Emmagatzema informació de "CSV Educación BCN" amb usuari "bchecker"
  - **Accés controlat**: Només accessible des de la xarxa interna i mitjançant regles específiques des de la DMZ
  - Redueix significativament el risc d'atacs d'injecció SQL des de l'exterior

#### DHCP Server - 192.168.60.20
- **Rang**: 30-100
- **Justificació**:
  - **Gestió automàtica d'IPs**: Facilita l'administració de clients
  - **Escalabilitat**: Permet agregar nous dispositius sense configuració manual
  - **Rang controlat**: El rang 30-100 permet 71 adreces dinàmiques, deixant les primeres IPs per a serveis estàtics

#### DNS Server - 192.168.60.20
- **Funció**: Resol R-N01, R
- **Justificació**:
  - **Resolució de noms interna**: Facilita l'accés als recursos per nom en lloc d'IP
  - **Simplificació de gestió**: Els canvis d'IP no afecten les aplicacions que usen noms
  - **Centralització**: Mateix servidor que DHCP per eficiència

#### Clients (PC Windows i PC Linux)
- **Configuració**: DHCP
- **IPs**: 192.168.60.x
- **Justificació**:
  - **Flexibilitat**: Els usuaris no necessiten coneixements de xarxa
  - **Mobilitat**: Els dispositius poden canviar d'ubicació sense reconfiguració
  - **Gestió centralitzada**: Canvis de configuració es realitzen al servidor DHCP

---

### Avantatges de l'Arquitectura Proposada

#### Seguretat
1. **Segmentació en capes**: Internet → DMZ → Intranet
2. **Principi de mínim privilegi**: Cada zona té accés limitat a les altres
3. **Protecció de dades**: Base de dades inaccessible des d'Internet
4. **Punt únic de control**: El router R-N01 gestiona tot el trànsit entre zones

#### Escalabilitat
- Fàcil afegir nous servidors a la DMZ o Intranet
- El DHCP permet creixement de clients sense reconfiguració
- Estructura modular que facilita expansions futures

#### Mantenibilitat
- Separació clara de responsabilitats per zona
- DNS intern simplifica canvis d'infraestructura
- Serveis especialitzats en servidors dedicats

#### Rendiment
- El trànsit intern (Intranet) no passa per la DMZ
- La base de dades està a la mateixa xarxa que els clients, reduint latència
- Serveis distribuïts eviten colls d'ampolla

---

### Flux de Comunicació

#### Accés Extern → Aplicació Web
1. Usuari d'Internet → R-N01 (NAT)
2. R-N01 → W-N02 (DMZ)
3. W-N02 → R-N01 → B-N03 (consulta BD)
4. Resposta inversa

#### Accés Intern
- Clients (192.168.60.x) → B-N03: Comunicació directa a la mateixa xarxa
- Clients → Internet: NAT a R-N01

---

### Conclusió

Aquesta arquitectura implementa les **millors pràctiques de seguretat en xarxes** mitjançant:
- Defensa en profunditat (múltiples capes de seguretat)
- Segmentació de xarxa basada en funcionalitat i nivell d'exposició
- Serveis crítics protegits a la xarxa interna
- Serveis públics aïllats a la DMZ
- Gestió centralitzada i automatitzada de la xarxa interna

---


## Sprint 1 - Configuració Serveis de Xarxa

### Configuració DNS Server (D-N03)

#### Pas 1: Configuració de la interfície de xarxa

Visualització del fitxer `/etc/netplan/01-network-manager-all.yaml` amb la configuració de la interfície enp3s0 del servidor DNS amb IP estàtica 192.168.6.20/24, gateway 192.168.6.1 i servidors DNS externs (8.8.8.8 i 8.8.4.4).

![Configuració Netplan DNS](./Photos/Sprint%201/DNS1.png)

---

#### Pas 2: Creació de l'usuari bchecker

Creació de l'usuari `bchecker` amb el grup bchecker (1001) mitjançant la comanda `sudo adduser bchecker`. Es configura el directori personal i la contrasenya per complir amb els requisits del projecte.

![Creació usuari bchecker](./Photos/Sprint%201/DNS2.png)

---

#### Pas 3: Configuració del fitxer named.conf.options

Visualització del fitxer `/etc/bind/named.conf.options` amb la configuració del servidor BIND9:
- Directori de caché: `/var/cache/bind`
- Permet consultes de qualsevol origen
- Recursió habilitada
- Escolta en el port 53
- Forwarders configurats (8.8.8.8 i 8.8.4.4)
- DNSSEC validation en mode auto

![Configuració named.conf.options](./Photos/Sprint%201/DNS3.png)

---

#### Pas 4: Configuració del fitxer named.conf.local

Visualització del fitxer `/etc/bind/named.conf.local` amb la definició de les zones DNS:
- **Zona directa "grup6.itb.cat":** Tipus master amb fitxer `/etc/bind/db.grup6.itb.cat`
- **Zona inversa "60.168.192.in-addr.arpa":** Tipus master amb fitxer `/etc/bind/db.192.168.60`

![Configuració named.conf.local](./Photos/Sprint%201/DNS4.png)

---

#### Pas 5: Fitxer de zona directa db.grup6.itb.cat

Contingut del fitxer `/etc/bind/db.grup6.itb.cat` amb els registres DNS:
- **SOA:** DN-03.grup6.itb.cat amb admin.grup6.itb.cat
- **NS:** Servidor de noms DN-03
- **A:** Registre que apunta DN-03 a la IP 192.168.60.20

![Zona directa grup6.itb.cat](./Photos/Sprint%201/DNS5.png)

---

#### Pas 6: Fitxer de zona inversa db.192.168.60

Contingut del fitxer `/etc/bind/db.192.168.60` amb els registres de resolució inversa:
- **SOA:** Configuració idèntica a la zona directa
- **NS:** Servidor de noms DN-03
- **PTR:** Registre que apunta 20 (192.168.60.20) a DN-03.grup6.itb.cat

![Zona inversa 192.168.60](./Photos/Sprint%201/DNS6.png)

---

#### Pas 7: Verificació del fitxer de zona inversa

Comprovació addicional del contingut del fitxer `/etc/bind/db.192.168.60` confirmant la correcta configuració dels registres PTR per a la resolució inversa.

![Verificació zona inversa](./Photos/Sprint%201/DNS7.png)

---

#### Pas 8: Configuració del fitxer named.conf principal

Visualització del fitxer `/etc/bind/named.conf` que inclou els fitxers de configuració principals:
- `/etc/bind/named.conf.options`
- `/etc/bind/named.conf.local`
- `/etc/bind/named.conf.default-zones`

Aquest és el fitxer principal que carrega tota la configuració del servidor BIND9.

![Configuració named.conf](./Photos/Sprint%201/DNS8.png)

---

#### Pas 9: Verificació de l'estat del servei BIND9

Comprovació amb `sudo systemctl status bind9` que el servei named.service està actiu i funcionant correctament (active/running). Es pot veure que el servei va arrencar correctament i està escoltant en les interfícies IPv6 enp4s0 i enp5s0.

![Estat servei BIND9](./Photos/Sprint%201/DNS9.png)

---


### Configuració DHCP Server

#### Pas 1: Instal·lació del servei DHCP

![Instal·lació DHCP](./Photos/Sprint%201/DHCP1.png)

Instal·lació del paquet `isc-dhcp-server` al servidor Ubuntu.

---

#### Pas 2: Configuració del fitxer dhcpd.conf

Configuració del fitxer `/etc/dhcp/dhcpd.conf` amb el rang d'IPs (192.168.60.30-100), gateway (192.168.60.1), DNS (8.8.8.8, 4.4.4.4) i reserva estàtica per adminPC (192.168.60.20).

![Configuració dhcpd.conf](./Photos/Sprint%201/DHCP2.png)

---

#### Pas 3: Verificació de l'estat del servei

Verificació que el servei DHCP està actiu i funcionant correctament (status active/running).

![Estat servei DHCP](./Photos/Sprint%201/DHCP3.png)


---

#### Pas 4: Configuració client Ubuntu

Configuració del client Ubuntu per obtenir IP automàticament via DHCP i DNS manual (192.168.60.20).

![Configuració client Ubuntu](./Photos/Sprint%201/DHCP4.png)

---

#### Pas 5: Verificació IP assignada - Client Ubuntu

Verificació que el client Ubuntu ha rebut la IP 192.168.60.30 del pool DHCP.

![IP client Ubuntu](./Photos/Sprint%201/DHCP5.png)

---

#### Pas 6: Verificació IP assignada - Client Windows

Verificació que el client Windows ha rebut la IP 192.168.60.31 del servidor DHCP amb gateway 192.168.60.1.

![IP client Windows](./Photos/Sprint%201/DHCP6.png)

---

#### Pas 7: Comprovació del fitxer de leases

Comprovació del fitxer de leases que mostra l'assignació d'IP al client Windows (DESKTOP-JNU2BQU amb IP dinàmica).

![Fitxer leases DHCP](./Photos/Sprint%201/DHCP7.png)

---


### Configuració Router R-N01

#### Pas 1: Pantalla d'inici de sessió

Pantalla d'inici de sessió del sistema Ubuntu Server amb els usuaris isardVDI, Grup6 i bchecker disponibles per accedir al router.

![Pantalla inici Router](./Photos/Sprint%201/R1.png)

---

#### Pas 2: Configuració del fitxer /etc/hosts

Edició del fitxer `/etc/hosts` assignant el nom "R-N01" al localhost (127.0.1.1) per identificar correctament el router a la xarxa.

![Configuració hosts](./Photos/Sprint%201/R2.png)

---

#### Pas 3: Configuració de les interfícies de xarxa

Visualització del fitxer `/etc/netplan/01-network-manager-all.yaml` amb la configuració de les 3 interfícies del router:
- **enp1s0:** NAT amb DHCP (52:54:00:34:64:69)
- **enp2s0:** DMZ amb IP 192.168.6.1/24 (52:54:00:38:57:0d)
- **enp3s0:** Intranet amb IP 192.168.60.1/24 (52:54:00:1d:14:5e)

![Configuració Netplan](./Photos/Sprint%201/R3.png)

---

#### Pas 4: Verificació de les interfícies actives

Comprovació amb `ip a` de l'estat de totes les interfícies de xarxa del router. Es poden veure les tres interfícies configurades i actives amb les seves respectives adreces IP i MAC.

![Estat interfícies](./Photos/Sprint%201/R4.png)

---

#### Pas 5: Configuració de les regles d'iptables

Configuració completa de les regles d'iptables per gestionar el tràfic entre les xarxes:
- **NAT:** Configuració de MASQUERADE per sortida a Internet
- **FORWARD:** Regles per permetre tràfic entre DMZ ↔ Internet i Intranet ↔ DMZ
- **Port Forwarding:** Redirecció del port 3306 de DMZ a Intranet (192.168.6.10 → 192.168.60.20)

Les regles es guarden amb `iptables-save` al fitxer `/etc/iptables/rules.v4`.

![Regles iptables](./Photos/Sprint%201/R5.png)

---

#### Pas 6: Verificació de les taules de rutes i iptables

Comprovació amb `ip route show` de les rutes configurades i verificació amb `iptables -L -n -v` de totes les cadenes (INPUT, FORWARD, OUTPUT, PREROUTING, POSTROUTING) amb les regles actives i estadístiques de paquets processats.

![Taules de rutes i verificació](./Photos/Sprint%201/R6.png)

---

#### Pas 7: Proves de connectivitat

Proves de ping des del router cap als servidors de la xarxa Intranet:
- **192.168.60.20:** Servidor de Base de Dades (B-N03) - Connectivitat correcta
- **192.168.60.30:** Client Ubuntu amb IP DHCP - Connectivitat correcta  
- **192.168.60.31:** Client Windows amb IP DHCP - Connectivitat correcta

Totes les proves mostren 0% packet loss confirmant la correcta configuració del router.

![Proves connectivitat](./Photos/Sprint%201/R7.png)

---

## Sprint 2 - Configuració del Servidor Web

### Configuració Web Server (W-N02)

#### Pas 1: Configuració de la interfície de xarxa

Visualització del fitxer `/etc/netplan/01-network-manager-all.yaml` amb la configuració de la interfície enp2s0 del servidor web amb IP estàtica 192.168.6.10/24, gateway 192.168.6.1 per a la xarxa DMZ.

![Configuració Netplan Web Server](./Photos/sprint%202/web/1.png)

---

#### Pas 2: Proves de connectivitat des del servidor web

Proves de connectivitat mitjançant ping des del servidor web cap al servidor FTP (192.168.6.11) i cap al router (192.168.6.1). Ambdues proves mostren 0% packet loss confirmant la correcta configuració de xarxa a la DMZ.

![Proves connectivitat Web Server](./Photos/sprint%202/web/2.png)

---

#### Pas 3: Configuració de regles iptables al router per al servidor web

Configuració de les regles d'iptables al router R-N01 per permetre l'accés al servidor web. S'afegeixen regles INPUT per acceptar tràfic des de les xarxes DMZ (192.168.6.0/24) i Intranet (192.168.60.0/24), així com per a IPs específiques del web server (192.168.6.10 i 192.168.6.11).

![Regles iptables per Web Server](./Photos/sprint%202/web/3.png)

---

#### Pas 4: Instal·lació i verificació del servei Apache2

Instal·lació del servidor Apache2 i verificació que el servei està actiu (active/running) des del 10 de novembre. Es mostra l'estat del servei amb PID 2266 i el hostname del servidor 192.168.121.26 192.168.6.10.

![Estat servei Apache2](./Photos/sprint%202/web/4.png)

---

#### Pas 5: Configuració del firewall UFW i SSL

Verificació de l'estat del servei Apache2, configuració del hostname, habilitació de la regla UFW per "Apache Full" (ports 80 i 443), i habilitació dels mòduls SSL necessaris (ssl, socache_shmcb). El servei està actiu i escoltant en múltiples ports incloent 80, 443 i [::]:22.

![Configuració UFW i SSL](./Photos/sprint%202/web/5.png)

---

#### Pas 6: Habilitació de mòduls SSL i configuració del lloc per defecte

Execució de la comanda `sudo a2enmod ssl` per habilitar els mòduls SSL (setenvif, mime, socache_shmcb, ssl). Després s'habilita el lloc SSL per defecte amb `sudo a2ensite default-ssl.conf` i es recarrega Apache2. Es verifica la configuració amb `apache2ctl configtest` mostrant un warning sobre el ServerName.

![Habilitació SSL i configuració](./Photos/sprint%202/web/6.png)

---

#### Pas 7: Accés HTTP al servidor web des del navegador

Accés al servidor web mitjançant el navegador Firefox a l'adreça http://192.168.6.10 mostrant la pàgina per defecte d'Apache2 Ubuntu. Es visualitza la pàgina de benvinguda confirmant que el servidor web està operatiu i accessible des de la xarxa.

![Accés HTTP al Web Server](./Photos/sprint%202/web/7.png)

---

#### Pas 8: Advertència de seguretat al accedir per HTTPS

Intent d'accés al servidor web mitjançant HTTPS (https://192.168.6.10). Firefox detecta un risc de seguretat potencial perquè el certificat SSL és autosignat (self-signed). Es mostra l'error "MOZILLA_PKIX_ERROR_SELF_SIGNED_CERT" amb opcions per retrocedir o acceptar el risc.

![Advertència certificat SSL](./Photos/sprint%202/web/8.png)

---

#### Pas 9: Accés HTTPS exitós després d'acceptar el certificat

Després d'acceptar el risc de seguretat, s'accedeix correctament al servidor web per HTTPS (https://192.168.6.10) mostrant la mateixa pàgina per defecte d'Apache2 Ubuntu. Això confirma que el servidor està funcionant tant en HTTP com en HTTPS.

![Accés HTTPS al Web Server](./Photos/sprint%202/web/9.png)

---

#### Pas 10: Configuració del servei SSH

Verificació de l'estat del servei SSH amb `sudo systemctl status ssh` mostrant que està actiu des de les 17:02. Es configura el fitxer `/etc/ssh/sshd_config`, es reinicia el servei, i s'afegeix la regla UFW per permetre el port 2222/tcp. L'estat del firewall mostra les regles actives per Apache Full i SSH (port 2222).

![Configuració servei SSH](./Photos/sprint%202/web/10.png)

---

#### Pas 11: Configuració detallada del fitxer sshd_config

Visualització del fitxer de configuració `/etc/ssh/sshd_config` amb nano mostrant els paràmetres principals: Port 2222, autenticació per clau pública habilitada (PubkeyAuthentication yes), login de root deshabilitat (PermitRootLogin no), i configuració de logging i autenticació.

![Configuració sshd_config](./Photos/sprint%202/web/11.png)

---

#### Pas 12: Configuració d'iptables al router per SSH

Configuració de les regles d'iptables al router R-N01 per permetre l'accés SSH al servidor web. S'afegeixen regles INPUT per permetre: loopback, connexions establertes, ping (ICMP), SSH al router (port 22), accés des de les xarxes DMZ i Intranet, i accés a IPs específiques del web server. També s'afegeix una regla FORWARD bidireccional entre les xarxes DMZ i Intranet.

![Regles iptables per SSH](./Photos/sprint%202/web/12.png)

---

#### Pas 13: Configuració del forwarding IPv4 al router

Edició del fitxer `/etc/sysctl.conf` al router amb nano per habilitar el forwarding de paquets IPv4. Es descomenta la línia `net.ipv4.ip_forward=1` per permetre que el router encamini paquets entre diferents interfícies de xarxa.

![Configuració IP forwarding](./Photos/sprint%202/web/13.png)

---

#### Pas 14: Creació i habilitació del servei de persistència d'iptables

Creació de l'script `/usr/local/bin/iptables-rules.sh` i del servei systemd `/etc/systemd/system/iptables-rules.service` per fer persistents les regles d'iptables. S'habilita i s'inicia el servei amb `systemctl enable/start iptables-rules.service`. La verificació mostra que el servei està actiu (active/exited) i s'ha carregat correctament.

![Servei persistència iptables](./Photos/sprint%202/web/14.png)

---

#### Pas 15: Creació del servei de ruta estàtica al router (Servidor Web)

Visualització del fitxer `/etc/systemd/system/add-static-route.service` al servidor web (W-NCC) que configura una ruta estàtica cap a la xarxa Intranet (192.168.60.0/24) via el router (192.168.6.1). Aquest servei s'executa després de la xarxa estar disponible.

![Servei ruta estàtica Web Server](./Photos/sprint%202/web/15.png)

---

#### Pas 16: Verificació del servei de ruta estàtica

Comprovació del contingut del fitxer `/etc/systemd/system/add-static-route.service` al servidor de base de dades (B-N06) amb una configuració similar, establint la ruta estàtica cap a la xarxa DMZ (192.168.6.0/24) via el router de la Intranet (192.168.60.1).

![Verificació ruta estàtica Database](./Photos/sprint%202/web/16.png)

---

#### Pas 17: Connexió SSH des del servidor FTP al Web Server

Connexió SSH exitosa des del servidor FTP (F-NCC) al servidor web utilitzant el port 2222 amb la comanda `ssh -p 2222 bchecker@192.168.6.10`. S'accedeix correctament al sistema Ubuntu 22.04.4 LTS mostrant informació de documentació, management i suport. El darrer login va ser des de 192.168.6.11.

![Connexió SSH FTP a Web](./Photos/sprint%202/web/17.png)

---

#### Pas 18: Configuració del DirectoryIndex per PHP

Edició del fitxer `/etc/apache2/mods-enabled/dir.conf` amb nano per configurar l'ordre del DirectoryIndex. S'estableix que index.php tingui prioritat sobre els altres fitxers d'índex (index.html, index.cgi, etc.).

![Configuració DirectoryIndex](./Photos/sprint%202/web/19.png)

---

#### Pas 19: Verificació de la instal·lació de PHP

Comprovació de la versió de PHP instal·lada mostrant PHP 8.1.2-1ubuntu2.22 amb Zend Engine v4.1.2 i Zend OPcache v8.1.2. S'executa la comanda `php -m | grep -E 'mysqli|pdo'` per verificar que els mòduls mysqli i pdo_mysql estan instal·lats correctament.

![Verificació PHP i mòduls](./Photos/sprint%202/web/20.png)

---

#### Pas 20: Creació de l'arxiu test.php i configuració de permisos

Creació de l'arxiu `/var/www/html/test.php` amb la funció `phpinfo();` per mostrar la informació de configuració de PHP. Es configuren els permisos adequats amb `chown www-data:www-data` i `chmod 644`. Es verifica el contingut del fitxer mostrant el codi PHP bàsic.

![Creació test.php](./Photos/sprint%202/web/21.png)

---

#### Pas 21: Accés web a test.php i verificació de PHP

Accés mitjançant el navegador a http://192.168.6.10/test.php mostrant la pàgina d'informació de PHP (phpinfo). Es visualitza la versió PHP 8.1.2-1ubuntu2.22 amb informació detallada del sistema, build date, server API, directives de configuració, PHP Extension, Zend Extension i altres paràmetres de configuració de PHP.

![Visualització phpinfo()](./Photos/sprint%202/web/22.png)

---

#### Pas 22: Detall complet de la configuració PHP

Vista ampliada de la pàgina phpinfo() mostrant informació completa sobre la configuració de PHP incloent: System, Build Date, Build System, Server API, Virtual Directory Support, Configuration File, Loaded Configuration File, extensions carregades, PHP API, PHP Extension, Zend Extension, Debug Build, Thread Safety, i altres paràmetres tècnics del servidor PHP.

![Detall configuració PHP](./Photos/sprint%202/web/22.png)

---

### Configuració Database Server (B-N03)

#### Pas 1: Instal·lació del servidor MySQL

Instal·lació del paquet `mysql-server` amb la comanda `sudo apt install mysql-server`. El sistema descarrega i instal·la automàticament tots els paquets necessaris incloent llibreries perl, llibcgi, libevent, mecab i les dependències de MySQL 8.0.

![Instal·lació MySQL Server](./Photos/sprint%202/BBDD/BBDD1.png)

---

#### Pas 2: Creació de la taula equipaments

Script SQL per crear la taula `equipaments` amb els camps: register_id (VARCHAR 50), nom (VARCHAR 255 NOT NULL), institution_id (VARCHAR 50), institution_name (VARCHAR 255), created i modified (TIMESTAMP), geo_x i geo_y (FLOAT), latitude i longitude (FLOAT), estimated_dates (VARCHAR 100), start_date i end_date (DATE), i timetable (TEXT).

![Taula equipaments](./Photos/sprint%202/BBDD/BBDD2.png)

---

#### Pas 3: Creació de la taula direccions

Script SQL per crear la taula `direccions` amb clau forana (FOREIGN KEY) referenciada a equipaments. Inclou camps: equipment_id (VARCHAR 50), roadtype_id, roadtype_name, road_id, road_name (VARCHAR 255), start_street_number i end_street_number (VARCHAR 10), neighborhood_id, neighborhood_name, district_id, district_name (VARCHAR 100), zip_code (VARCHAR 10), town (VARCHAR 100), main_address i address_type (VARCHAR 50).

![Taula direccions](./Photos/sprint%202/BBDD/BBDD3.png)

---

#### Pas 4: Creació de la taula valors

Script SQL per crear la taula `valors` amb clau forana referenciada a equipaments. Inclou camps: equipment_id (VARCHAR 50), attribute_id (VARCHAR 50), values_id (VARCHAR 50), category (VARCHAR 100), attribute_name (VARCHAR 100), value (VARCHAR 255), outstanding (VARCHAR 50), i description (TEXT).

![Taula valors](./Photos/sprint%202/BBDD/BBDD4.png)

---

#### Pas 5: Creació de la taula filtres_secundaris

Script SQL per crear la taula `filtres_secundaris` amb clau forana referenciada a equipaments. Inclou camps: equipment_id (VARCHAR 50), filter_id (VARCHAR 50), filter_name (VARCHAR 255), filter_fullpath (TEXT), filter_tree (VARCHAR 255), i filter_asia_id (VARCHAR 50).

![Taula filtres_secundaris](./Photos/sprint%202/BBDD/BBDD5.png)

---

#### Pas 6: Verificació de les taules creades

Execució de la comanda `SHOW TABLES;` a MySQL mostrant les 4 taules creades correctament a la base de dades EquipamentsBCN: direccions, equipaments, filtres_secundaris i valors. El resultat mostra "4 rows in set (0,01 sec)".

![Verificació taules MySQL](./Photos/sprint%202/BBDD/BBDD6.png)

---

#### Pas 7: Visualització de l'estructura de la taula equipaments

Execució de la comanda `SHOW COLUMNS FROM equipaments;` mostrant l'estructura completa de la taula amb 14 camps: register_id (PRI, varchar 50), nom (varchar 255), institution_id, institution_name, created i modified (timestamp), geo_x, geo_y, latitude i longitude (float), estimated_dates (varchar 100), start_date i end_date (date), i timetable (text).

![Columnes taula equipaments](./Photos/sprint%202/BBDD/BBDD7.png)

---

#### Pas 8: Visualització de l'estructura de la taula direccions

Execució de la comanda `SHOW COLUMNS FROM direccions;` mostrant l'estructura completa amb 15 camps. Equipment_id està configurat com a MUL (clau múltiple/forana) referenciant la taula equipaments. Tots els camps són VARCHAR excepte address_type, amb mides que varien entre VARCHAR(10) i VARCHAR(255).

![Columnes taula direccions](./Photos/sprint%202/BBDD/BBDD8.png)

---

#### Pas 9: Visualització de l'estructura de la taula valors

Execució de la comanda `SHOW COLUMNS FROM valors;` mostrant l'estructura amb 8 camps. Equipment_id està configurat com a MUL (clau forana). Els camps inclouen: equipment_id, attribute_id, values_id (VARCHAR 50), category i attribute_name (VARCHAR 100), value (VARCHAR 255), outstanding (VARCHAR 50) i description (TEXT).

![Columnes taula valors](./Photos/sprint%202/BBDD/BBDD9.png)

---

#### Pas 10: Visualització de l'estructura de la taula filtres_secundaris

Execució de la comanda `SHOW COLUMNS FROM filtres_secundaris;` mostrant l'estructura amb 6 camps. Equipment_id està configurat com a MUL (clau forana). Els camps inclouen: equipment_id, filter_id (VARCHAR 50), filter_name (VARCHAR 255), filter_fullpath (TEXT), filter_tree (VARCHAR 255) i filter_asia_id (VARCHAR 50).

![Columnes taula filtres_secundaris](./Photos/sprint%202/BBDD/BBDD10.png)

---

### Configuració FTP Server (F-N02)

#### Pas 1: Instal·lació del servidor vsftpd

Instal·lació del paquet `vsftpd` (Very Secure FTP Daemon) amb la comanda `sudo apt install vsftpd`. El sistema descarrega i instal·la el paquet vsftpd versió 3.0.5-0ubuntu1.1 amb una mida de 123 kB. Es crea automàticament el servei systemd i s'instal·len les dependències necessàries.

![Instal·lació vsftpd](./Photos/sprint%202/ftp/ftp1.png)

---

#### Pas 2: Verificació de la versió de vsftpd

Comprovació de la versió instal·lada de vsftpd amb la comanda `vsftpd -v`. El sistema confirma que s'ha instal·lat la versió 3.0.5 del servidor FTP.

![Versió vsftpd](./Photos/sprint%202/ftp/ftp2.png)

---

#### Pas 3: Habilitació del servei vsftpd

Habilitació del servei vsftpd per iniciar-se automàticament amb l'arrencada del sistema mitjançant la comanda `sudo systemctl enable vsftpd`. Això crea els enllaços simbòlics necessaris per al servei.

![Habilitació servei vsftpd](./Photos/sprint%202/ftp/ftp3.png)

---

#### Pas 4: Inici i verificació de l'estat del servei vsftpd

Execució de les comandes `sudo systemctl start vsftpd` i `sudo systemctl status vsftpd` per iniciar i verificar l'estat del servei. El servei està actiu (active/running) des del 10 de novembre a les 16:07, amb PID 2329, consumint 868.0K de memòria i llegint la configuració del fitxer `/etc/vsftpd.conf`.

![Estat servei vsftpd](./Photos/sprint%202/ftp/ftp4.png)

---

#### Pas 5: Accés al fitxer de configuració vsftpd.conf

Edició del fitxer de configuració principal `/etc/vsftpd.conf` amb nano per configurar els paràmetres del servidor FTP.

![Edició vsftpd.conf](./Photos/sprint%202/ftp/ftp5.png)

---

#### Pas 6: Habilitació d'usuaris locals

Configuració del paràmetre `local_enable=YES` al fitxer vsftpd.conf per permetre que els usuaris locals del sistema puguin iniciar sessió al servidor FTP.

![Habilitació usuaris locals](./Photos/sprint%202/ftp/ftp6.png)

---

#### Pas 7: Deshabilitació de l'accés anònim

Configuració del paràmetre `anonymous_enable=NO` al fitxer vsftpd.conf per deshabilitar l'accés anònim al servidor FTP, millorant així la seguretat del sistema.

![Deshabilitació accés anònim](./Photos/sprint%202/ftp/ftp7.png)

---

#### Pas 8: Habilitació d'escriptura FTP

Configuració del paràmetre `write_enable=YES` al fitxer vsftpd.conf per permetre qualsevol forma de comandes d'escriptura FTP, incloent pujada, eliminació i modificació d'arxius.

![Habilitació escriptura FTP](./Photos/sprint%202/ftp/ftp8.png)

---

#### Pas 9: Restricció d'usuaris al directori personal

Configuració del paràmetre `chroot_local_user=YES` al fitxer vsftpd.conf per restringir els usuaris locals als seus directoris personals, impedint que naveguin per altres parts del sistema de fitxers per motius de seguretat.

![Restricció chroot](./Photos/sprint%202/ftp/ftp9.png)

---

#### Pas 10: Reinici i verificació del servei vsftpd

Reinici del servei vsftpd amb `sudo systemctl restart vsftpd` per aplicar els canvis de configuració. La verificació amb `sudo systemctl status vsftpd` confirma que el servei està actiu (active/running) des de les 16:13:16 amb PID 2787.

![Reinici servei vsftpd](./Photos/sprint%202/ftp/ftp10.png)

---

#### Pas 11: Creació de l'usuari FTP usuarigroup6

Creació de l'usuari `usuarigroup6` amb la comanda `sudo adduser usuarigroup6`. El sistema crea l'usuari amb UID 1002, el grup usuarigroup6 (GID 1002), el directori personal `/home/usuarigroup6`, i es configura la contrasenya i la informació del usuari.

![Creació usuari FTP](./Photos/sprint%202/ftp/ftp11.png)

---

#### Pas 12: Configuració del directori FTP i permisos

Creació del directori `/home/usuarigroup6/ftp` amb `sudo mkdir -p`, assignació de propietat a nobody:nogroup amb `sudo chown nobody:nogroup`, i configuració dels permisos a+w amb `sudo chmod a+w` per permetre l'escriptura a tots els usuaris.

![Configuració directori FTP](./Photos/sprint%202/ftp/ftp12.png)

---

#### Pas 13: Configuració del firewall per FTP

Habilitació del port 22/tcp al firewall UFW amb la comanda `sudo ufw allow 22/tcp` per permetre les connexions FTP. El sistema confirma que les regles han estat actualitzades tant per IPv4 com per IPv6.

![Configuració firewall FTP](./Photos/sprint%202/ftp/ftp13.png)

---

#### Pas 14: Prova de connexió SSH des del servidor FTP

Connexió SSH exitosa des del servidor FTP (F-NCC) al servidor web utilitzant el port 2222 amb la comanda `ssh -p 2222 bchecker@192.168.6.10`. S'accedeix correctament al sistema Ubuntu 22.04.4 LTS mostrant la informació de benvinguda. El darrer login va ser el 10 de novembre a les 17:32:01 des de 192.168.6.11.

![Connexió SSH des de FTP](./Photos/sprint%202/ftp/ftp14.png)

---