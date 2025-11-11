# ASIXc2AC--Projecte_P0.0

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
---

##  Plan de Prevención de Riesgos Laborales

(https://docs.google.com/document/d/1FECaZZFULFxXc-NEI9rzEbMYEfymHPD1h15zuCWg2hA/edit?usp=sharing)


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