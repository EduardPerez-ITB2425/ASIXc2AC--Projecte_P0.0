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
```bash
sudo cat /etc/netplan/01-network-manager-all.yaml
```
```yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp3s0:
      dhcp4: no
      addresses:
        - 192.168.6.20/24
      routes:
        - to: default
          via: 192.168.6.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```
```bash
sudo netplan apply
```

---

#### Pas 2: Creació de l'usuari bchecker

Creació de l'usuari `bchecker` amb el grup bchecker (1001) mitjançant la comanda `sudo adduser bchecker`. Es configura el directori personal i la contrasenya per complir amb els requisits del projecte.

![Creació usuari bchecker](./Photos/Sprint%201/DNS2.png)
```bash
sudo adduser bchecker
```

**Sortida esperada:**
```
Añadiendo el usuario `bchecker' ...
Añadiendo el nuevo grupo `bchecker' (1001) ...
Añadiendo el nuevo usuario `bchecker' (1001) con grupo `bchecker' ...
Creando el directorio personal `/home/bchecker' ...
Copiando los ficheros desde `/etc/skel' ...
Nueva contraseña:
Vuelva a escribir la nueva contraseña:
passwd: contraseña actualizada correctamente
```

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
```bash
sudo nano /etc/bind/named.conf.options
```
```conf
options {
    directory "/var/cache/bind";

    allow-query { any; };
    recursion yes;

    listen-on port 53 { any; };
    listen-on-v6 { any; };

    forwarders {
        8.8.8.8;
        8.8.4.4;
    };

    dnssec-validation auto;
    auth-nxdomain no;
};
```

---

#### Pas 4: Configuració del fitxer named.conf.local

Visualització del fitxer `/etc/bind/named.conf.local` amb la definició de les zones DNS:
- **Zona directa "grup6.itb.cat":** Tipus master amb fitxer `/etc/bind/db.grup6.itb.cat`
- **Zona inversa "60.168.192.in-addr.arpa":** Tipus master amb fitxer `/etc/bind/db.192.168.60`

![Configuració named.conf.local](./Photos/Sprint%201/DNS4.png)
```bash
sudo nano /etc/bind/named.conf.local
```
```conf
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "grup6.itb.cat" {
    type master;
    file "/etc/bind/db.grup6.itb.cat";
};

zone "60.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.192.168.60";
};
```

---

#### Pas 5: Fitxer de zona directa db.grup6.itb.cat

Contingut del fitxer `/etc/bind/db.grup6.itb.cat` amb els registres DNS:
- **SOA:** DN-03.grup6.itb.cat amb admin.grup6.itb.cat
- **NS:** Servidor de noms DN-03
- **A:** Registre que apunta DN-03 a la IP 192.168.60.20

![Zona directa grup6.itb.cat](./Photos/Sprint%201/DNS5.png)
```bash
sudo nano /etc/bind/db.grup6.itb.cat
```
```conf
$TTL    604800
@       IN      SOA     DN-03.grup6.itb.cat. admin.grup6.itb.cat. (
                        2025101401 ; Serial
                        604800     ; Refresh
                        86400      ; Retry
                        2419200    ; Expire
                        604800 )   ; Negative Cache TTL
;
@       IN      NS      DN-03.grup6.itb.cat.
DN-03   IN      A       192.168.60.20
```

---

#### Pas 6: Fitxer de zona inversa db.192.168.60

Contingut del fitxer `/etc/bind/db.192.168.60` amb els registres de resolució inversa:
- **SOA:** Configuració idèntica a la zona directa
- **NS:** Servidor de noms DN-03
- **PTR:** Registre que apunta 20 (192.168.60.20) a DN-03.grup6.itb.cat

![Zona inversa 192.168.60](./Photos/Sprint%201/DNS6.png)
```bash
sudo nano /etc/bind/db.192.168.60
```
```conf
$TTL    604800
@       IN      SOA     DN-03.grup6.itb.cat. admin.grup6.itb.cat. (
                        2025101401 ; Serial
                        604800     ; Refresh
                        86400      ; Retry
                        2419200    ; Expire
                        604800 )   ; Negative Cache TTL
;
@       IN      NS      DN-03.grup6.itb.cat.
20      IN      PTR     DN-03.grup6.itb.cat.
```

---

#### Pas 7: Verificació del fitxer de zona inversa

Comprovació addicional del contingut del fitxer `/etc/bind/db.192.168.60` confirmant la correcta configuració dels registres PTR per a la resolució inversa.

![Verificació zona inversa](./Photos/Sprint%201/DNS7.png)
```bash
cat /etc/bind/db.192.168.60
```

---

#### Pas 8: Configuració del fitxer named.conf principal

Visualització del fitxer `/etc/bind/named.conf` que inclou els fitxers de configuració principals:
- `/etc/bind/named.conf.options`
- `/etc/bind/named.conf.local`
- `/etc/bind/named.conf.default-zones`

Aquest és el fitxer principal que carrega tota la configuració del servidor BIND9.

![Configuració named.conf](./Photos/Sprint%201/DNS8.png)
```bash
cat /etc/bind/named.conf
```
```conf
// This is the primary configuration file for the BIND DNS server named.
//
// Please read /usr/share/doc/bind9/README.Debian.gz for information on the
// structure of BIND configuration files in Debian, *BEFORE* you customize
// this configuration file.
//
// If you are just adding zones, please do that in /etc/bind/named.conf.local

include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
include "/etc/bind/named.conf.default-zones";
```

---

#### Pas 9: Verificació de l'estat del servei BIND9

Comprovació amb `sudo systemctl status bind9` que el servei named.service està actiu i funcionant correctament (active/running). Es pot veure que el servei va arrencar correctament i està escoltant en les interfícies IPv6 enp4s0 i enp5s0.

![Estat servei BIND9](./Photos/Sprint%201/DNS9.png)
```bash
sudo systemctl status bind9
```

**Comandos addicionals:**
```bash
# Reiniciar el servei
sudo systemctl restart bind9

# Habilitar el servei a l'inici
sudo systemctl enable bind9

# Verificar la configuració
sudo named-checkconf

# Verificar zones
sudo named-checkzone grup6.itb.cat /etc/bind/db.grup6.itb.cat
sudo named-checkzone 60.168.192.in-addr.arpa /etc/bind/db.192.168.60
```
d
---

### Configuració DHCP Server

#### Pas 1: Instal·lació del servei DHCP

Instal·lació del paquet `isc-dhcp-server` al servidor Ubuntu.

![Instal·lació DHCP](./Photos/Sprint%201/DHCP1.png)
```bash
sudo apt install isc-dhcp-server
```

---

#### Pas 2: Configuració del fitxer dhcpd.conf

Configuració del fitxer `/etc/dhcp/dhcpd.conf` amb el rang d'IPs (192.168.60.30-100), gateway (192.168.60.1), DNS (8.8.8.8, 4.4.4.4) i reserva estàtica per adminPC (192.168.60.20).

![Configuració dhcpd.conf](./Photos/Sprint%201/DHCP2.png)
```bash
sudo nano /etc/dhcp/dhcpd.conf
```
```conf
subnet 192.168.60.0 netmask 255.255.255.0 {
    range 192.168.60.30 192.168.60.100;
    option routers 192.168.60.1;
    option subnet-mask 255.255.255.0;
    option domain-name-servers 8.8.8.8, 4.4.4.4;
    option domain-name "D-N03";
}

host adminPC {
    hardware ethernet 52:54:00:5a:73:f9;
    fixed-address 192.168.60.20;
}
```

---

#### Pas 3: Verificació de l'estat del servei

Verificació que el servei DHCP està actiu i funcionant correctament (status active/running).

![Estat servei DHCP](./Photos/Sprint%201/DHCP3.png)
```bash
sudo systemctl status isc-dhcp-server
```

---

#### Pas 4: Configuració client Ubuntu

Configuració del client Ubuntu per obtenir IP automàticament via DHCP i DNS manual (192.168.60.20).

![Configuració client Ubuntu](./Photos/Sprint%201/DHCP4.png)

**Configuració a la interfície gràfica:**
- Mètode IPv4: Automàtic (DHCP)
- DNS: 192.168.60.20

---

#### Pas 5: Verificació IP assignada - Client Ubuntu

Verificació que el client Ubuntu ha rebut la IP 192.168.60.30 del pool DHCP.

![IP client Ubuntu](./Photos/Sprint%201/DHCP5.png)
```bash
ip a | grep enp3s0
```

**Sortida esperada:**
```
inet 192.168.60.30/24 brd 192.168.60.255 scope global dynamic noprefixroute enp3s0
```

---

#### Pas 6: Verificació IP assignada - Client Windows

Verificació que el client Windows ha rebut la IP 192.168.60.31 del servidor DHCP amb gateway 192.168.60.1.

![IP client Windows](./Photos/Sprint%201/DHCP6.png)
```cmd
ipconfig
```

**Sortida esperada:**
```
Adaptador de Ethernet Ethernet:
   Sufijo DNS específico para la conexión. . : D-N03
   Dirección IPv4. . . . . . . . . . . . . : 192.168.60.31
   Máscara de subred . . . . . . . . . . . : 255.255.255.0
   Puerta de enlace predeterminada . . . . : 192.168.60.1
```

---

#### Pas 7: Comprovació del fitxer de leases

Comprovació del fitxer de leases que mostra l'assignació d'IP al client Windows (DESKTOP-JNU2BQU amb IP dinàmica).

![Fitxer leases DHCP](./Photos/Sprint%201/DHCP7.png)
```bash
sudo tail -f /var/lib/dhcp/dhcpd.leases
```

**Exemple de sortida:**
```
lease 192.168.60.31 {
  starts 2 2025/10/14 14:38:11;
  ends 2 2025/10/14 14:28:11;
  cltt 2 2025/10/14 14:28:11;
  binding state active;
  next binding state free;
  rewind binding state free;
  hardware ethernet 52:54:00:52:e7:a3;
  uid "\001RT\000R\347\243";
  set vendor-class-identifier = "MSFT 5.0";
  client-hostname "DESKTOP-JNU2BQU";
}
```

---


### Configuració Router R-N01

#### Pas 1: Pantalla d'inici de sessió

Pantalla d'inici de sessió del sistema Ubuntu Server amb els usuaris isardVDI, Grup6 i bchecker disponibles per accedir al router.

![Pantalla inici Router](./Photos/Sprint%201/R1.png)

---

#### Pas 2: Configuració del fitxer /etc/hosts

Edició del fitxer `/etc/hosts` assignant el nom "R-N01" al localhost (127.0.1.1) per identificar correctament el router a la xarxa.

![Configuració hosts](./Photos/Sprint%201/R2.png)
```bash
sudo nano /etc/hosts
```
```conf
127.0.0.1       localhost
127.0.1.1       R-N01

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

---

#### Pas 3: Configuració de les interfícies de xarxa

Visualització del fitxer `/etc/netplan/01-network-manager-all.yaml` amb la configuració de les 3 interfícies del router:
- **enp1s0:** NAT amb DHCP (52:54:00:34:64:69)
- **enp2s0:** DMZ amb IP 192.168.6.1/24 (52:54:00:38:57:0d)
- **enp3s0:** Intranet amb IP 192.168.60.1/24 (52:54:00:1d:14:5e)

![Configuració Netplan](./Photos/Sprint%201/R3.png)
```bash
sudo cat /etc/netplan/01-network-manager-all.yaml
```
```yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp1s0:
      match:
        macaddress: 52:54:00:34:64:69
      dhcp4: true
      optional: true
    enp2s0:
      match:
        macaddress: 52:54:00:38:57:0d
      addresses:
        - 192.168.6.1/24      # DMZ
    enp3s0:
      match:
        macaddress: 52:54:00:1d:14:5e
      addresses:
        - 192.168.60.1/24     # Intranet
```
```bash
sudo netplan apply
```

---

#### Pas 4: Verificació de les interfícies actives

Comprovació amb `ip a` de l'estat de totes les interfícies de xarxa del router. Es poden veure les tres interfícies configurades i actives amb les seves respectives adreces IP i MAC.

![Estat interfícies](./Photos/Sprint%201/R4.png)
```bash
ip a
```

**Sortida esperada:**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536
    link/loopback 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo

2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    link/ether 52:54:00:34:64:69
    inet 192.168.120.72/22 brd 192.168.123.255 scope global dynamic enp1s0

3: enp2s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    link/ether 52:54:00:38:57:0d
    inet 192.168.6.1/24 brd 192.168.6.255 scope global enp2s0

4: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    link/ether 52:54:00:1d:14:5e
    inet 192.168.60.1/24 brd 192.168.60.255 scope global enp3s0
```

---

#### Pas 5: Configuració de les regles d'iptables

Configuració completa de les regles d'iptables per gestionar el tràfic entre les xarxes:
- **NAT:** Configuració de MASQUERADE per sortida a Internet
- **FORWARD:** Regles per permetre tràfic entre DMZ ↔ Internet i Intranet ↔ DMZ
- **Port Forwarding:** Redirecció del port 3306 de DMZ a Intranet (192.168.6.10 → 192.168.60.20)

Les regles es guarden amb `iptables-save` al fitxer `/etc/iptables/rules.v4`.

![Regles iptables](./Photos/Sprint%201/R5.png)
```bash
# === POLÍTIQUES PER DEFECTE ===
sudo iptables -P FORWARD DROP

# === NAT ===
sudo iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE

# === INTERNET ⟷ DMZ ===
sudo iptables -A FORWARD -i enp1s0 -o enp2s0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i enp2s0 -o enp1s0 -j ACCEPT

# === INTRANET → DMZ ===
sudo iptables -A FORWARD -i enp3s0 -o enp2s0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i enp2s0 -o enp3s0 -j ACCEPT

# === DMZ → INTRANET (NOMÉS WEB → BBDD) ===
sudo iptables -A FORWARD -i enp2s0 -o enp3s0 -s 192.168.6.10 -d 192.168.60.20 -p tcp --dport 3306 -j ACCEPT
sudo iptables -A FORWARD -i enp2s0 -o enp3s0 -j DROP

# === INTRANET → DMZ (clients poden accedir) ===
sudo iptables -A FORWARD -i enp3s0 -o enp2s0 -j ACCEPT

# === GUARDAR CONFIGURACIÓ ===
sudo iptables-save | sudo tee /etc/iptables/rules.v4
```

---

#### Pas 6: Verificació de les taules de rutes i iptables

Comprovació amb `ip route show` de les rutes configurades i verificació amb `iptables -L -n -v` de totes les cadenes (INPUT, FORWARD, OUTPUT, PREROUTING, POSTROUTING) amb les regles actives i estadístiques de paquets processats.

![Taules de rutes i verificació](./Photos/Sprint%201/R6.png)
```bash
# Mostrar rutes
ip route show

# Verificar regles iptables
sudo iptables -L -n -v

# Verificar regles NAT
sudo iptables -t nat -L -n -v
```

**Sortida esperada de `ip route show`:**
```
default via 192.168.120.1 dev enp1s0 proto dhcp src 192.168.120.72 metric 100
192.168.6.0/24 dev enp2s0 proto kernel scope link src 192.168.6.1
192.168.60.0/24 dev enp3s0 proto kernel scope link src 192.168.60.1
192.168.120.0/22 dev enp1s0 proto kernel scope link src 192.168.120.72 metric 100
192.168.120.1 dev enp2s0 proto dhcp scope link src 192.168.120.72 metric 100
```

---

#### Pas 7: Proves de connectivitat

Proves de ping des del router cap als servidors de la xarxa Intranet:
- **192.168.60.20:** Servidor de Base de Dades (B-N03) - Connectivitat correcta
- **192.168.60.30:** Client Ubuntu amb IP DHCP - Connectivitat correcta  
- **192.168.60.31:** Client Windows amb IP DHCP - Connectivitat correcta

Totes les proves mostren 0% packet loss confirmant la correcta configuració del router.

![Proves connectivitat](./Photos/Sprint%201/R7.png)
```bash
# DNS - Servidor de Base de Dades
ping -c 4 192.168.60.20

# Client Ubuntu (DHCP)
ping -c 4 192.168.60.30

# Client Windows (DHCP)
ping -c 4 192.168.60.31
```

**Sortida esperada:**
```
# DNS
PING 192.168.60.20 (192.168.60.20) 56(84) bytes of data.
64 bytes from 192.168.60.20: icmp_seq=1 ttl=64 time=1.55 ms
64 bytes from 192.168.60.20: icmp_seq=2 ttl=64 time=0.349 ms
64 bytes from 192.168.60.20: icmp_seq=3 ttl=64 time=0.464 ms
64 bytes from 192.168.60.20: icmp_seq=4 ttl=64 time=0.384 ms
--- 192.168.60.20 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3039ms

# Cliente Ubuntu
PING 192.168.60.30 (192.168.60.30) 56(84) bytes of data.
64 bytes from 192.168.60.30: icmp_seq=1 ttl=64 time=1.64 ms
64 bytes from 192.168.60.30: icmp_seq=2 ttl=64 time=0.386 ms
64 bytes from 192.168.60.30: icmp_seq=3 ttl=64 time=0.308 ms
64 bytes from 192.168.60.30: icmp_seq=4 ttl=64 time=0.351 ms
--- 192.168.60.30 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3046ms

# Cliente Windows
PING 192.168.60.31 (192.168.60.31) 56(84) bytes of data.
64 bytes from 192.168.60.31: icmp_seq=1 ttl=128 time=1.65 ms
64 bytes from 192.168.60.31: icmp_seq=2 ttl=128 time=0.583 ms
64 bytes from 192.168.60.31: icmp_seq=3 ttl=128 time=0.500 ms
64 bytes from 192.168.60.31: icmp_seq=4 ttl=128 time=0.452 ms
--- 192.168.60.31 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3034ms
```

**Habilitar IP Forwarding permanent:**
```bash
sudo nano /etc/sysctl.conf
```

Descomentar la línea:
```conf
net.ipv4.ip_forward=1
```

Aplicar cambios:
```bash
sudo sysctl -p
```

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

Configuració de les regles d'iptables al router R-N01 per permetre l'accés al servidor web. S'afegeixen regles INPUT per acceptar tràfic des de les xarxes DMZ (192.168.6.0/24) i Intranet (192.168.60.0/24), així com per a IPs específiques del web server (192.168.6.10 i 192.168.6.11). També s'afegeix una regla final de DROP per denegar tot el tràfic no autoritzat.

![Regles iptables per Web Server](./Photos/sprint%202/web/3.png)

---

#### Pas 4: Instal·lació i verificació del servei Apache2

Instal·lació del servidor Apache2 i verificació que el servei està actiu (active/running) des del 10 de novembre a les 17:08:47 CET. Es mostra l'estat del servei amb PID 2266 i múltiples processos d'Apache2 (PIDs 2266, 2269, 2270). El sistema té configurat el hostname amb les IPs 192.168.121.26 i 192.168.6.10.

![Estat servei Apache2](./Photos/sprint%202/web/4.png)

---

#### Pas 5: Configuració del firewall UFW i SSL

Verificació de l'estat del servei Apache2 i configuració del hostname. S'habilita la regla UFW per "Apache Full" (ports 80 i 443 HTTP/HTTPS) amb `sudo ufw allow 'Apache Full'`. Es comprova l'estat del firewall UFW mostrant les regles actives per Apache Full tant en IPv4 com IPv6. S'habiliten els mòduls SSL necessaris amb `sudo a2enmod ssl`, activant les dependències setenvif, mime i socache_shmcb. El servei està actiu i escoltant en múltiples ports.

![Configuració UFW i SSL](./Photos/sprint%202/web/5.png)

---

#### Pas 6: Habilitació de mòduls SSL i configuració del lloc per defecte

Execució de la comanda `sudo a2enmod ssl` confirmant que els mòduls SSL ja estan habilitats (setenvif, mime, socache_shmcb, ssl). S'habilita el lloc SSL per defecte amb `sudo a2ensite default-ssl.conf`. Es recarrega Apache2 amb `sudo systemctl reload apache2`. Es verifica la configuració amb `apache2ctl configtest` mostrant un warning sobre el ServerName (AH00558) i retornant "Syntax OK".

![Habilitació SSL i configuració](./Photos/sprint%202/web/6.png)

---

#### Pas 7: Accés HTTP al servidor web des del navegador

Accés al servidor web mitjançant el navegador Firefox a l'adreça http://192.168.6.10 mostrant la pàgina per defecte d'Apache2 Ubuntu. Es visualitza el logo d'Ubuntu i el títol "Ubuntu" amb el text "This is the default welcome page..." confirmant que el servidor web està operatiu i accessible des de la xarxa. La pàgina mostra la secció "Configuration Overview" amb l'estructura de directoris d'Apache2.

![Accés HTTP al Web Server](./Photos/sprint%202/web/7.png)

---

#### Pas 8: Advertència de seguretat al accedir per HTTPS

Intent d'accés al servidor web mitjançant HTTPS (https://192.168.6.10). Firefox detecta un risc de seguretat potencial amb el missatge "Warning: Potential Security Risk Ahead". El navegador indica que 192.168.6.10 utilitza un certificat de seguretat invàlid perquè és autosignat (self-signed). Es mostra l'error "MOZILLA_PKIX_ERROR_SELF_SIGNED_CERT" amb opcions per retrocedir ("Go Back (Recommended)") o acceptar el risc ("Accept the Risk and Continue").

![Advertència certificat SSL](./Photos/sprint%202/web/8.png)

---

#### Pas 9: Accés HTTPS exitós després d'acceptar el certificat

Després d'acceptar el risc de seguretat, s'accedeix correctament al servidor web per HTTPS (https://192.168.6.10). Es mostra la mateixa pàgina per defecte d'Apache2 Ubuntu amb el missatge "Apache2 Ubuntu Default Page: It works". El navegador mostra el logo d'Ubuntu i confirma que el servidor està funcionant tant en HTTP com en HTTPS. Això verifica que el certificat SSL autosignat està funcionant correctament.

![Accés HTTPS al Web Server](./Photos/sprint%202/web/9.png)

---

#### Pas 10: Configuració del servei SSH

Verificació de l'estat del servei SSH amb `sudo systemctl status ssh` mostrant que està actiu (active/running) des del 10 de novembre a les 17:02:10 CET. El servei OpenSSH (sshd) té PID 621 i està escoltant en el port 22 (0.0.0.0 i ::). S'edita el fitxer de configuració `/etc/ssh/sshd_config` amb nano i es reinicia el servei amb `sudo systemctl restart ssh`. S'afegeix la regla UFW per permetre el port 2222/tcp amb `sudo ufw allow 2222/tcp`. S'habilita el firewall amb `sudo ufw enable`. L'estat del firewall amb `sudo ufw status` mostra les regles actives: Apache Full (80,443/tcp) i 2222/tcp, ambdues des de "Anywhere" tant en IPv4 com IPv6. Finalment es verifica novament l'estat del servei SSH confirmant que està actiu amb PID 4118.

![Configuració servei SSH](./Photos/sprint%202/web/10.png)

---

#### Pas 11: Configuració detallada del fitxer sshd_config

Visualització del fitxer de configuració `/etc/ssh/sshd_config` amb nano mostrant els paràmetres principals de seguretat:
- **Port 2222:** Port personalitzat per SSH
- **PermitRootLogin no:** Deshabilita l'accés root directe
- **PubkeyAuthentication yes:** Habilita autenticació per clau pública
- **SyslogFacility AUTH:** Configuració de logging
- **LogLevel INFO:** Nivell de registre d'esdeveniments

El fitxer també inclou la directiva `Include /etc/ssh/sshd_config.d/*.conf` per incloure configuracions addicionals.

![Configuració sshd_config](./Photos/sprint%202/web/11.png)

---

#### Pas 12: Configuració d'iptables al router per SSH

Configuració de les regles d'iptables al router R-N01 per permetre l'accés SSH i gestionar el tràfic de xarxa. Les regles inclouen:

**INPUT:**
- Permetre loopback (tràfic local)
- Permetre connexions ja establertes
- Permetre ping (ICMP)
- Permetre SSH al router (port 22)
- Permetre accés des de les xarxes DMZ (192.168.6.0/24) i Intranet (192.168.60.0/24)
- Permetre accés des d'IPs específiques (192.168.6.10, 192.168.6.11, 192.168.60.15)

**FORWARD:**
- Regla bidireccional entre DMZ i Intranet amb `sudo iptables -A FORWARD -i enp2s0 -o enp3s0 -s 192.168.6.0/24 -d 192.168.60.0/24 -j ACCEPT`
- Regla inversa amb `sudo iptables -A FORWARD -i enp3s0 -o enp2s0 -s 192.168.60.0/24 -d 192.168.6.0/24 -j ACCEPT`

També s'executa `sudo sysctl -w net.ipv4.ip_forward=1` per habilitar el forwarding de paquets IPv4 de forma temporal.

![Regles iptables per SSH](./Photos/sprint%202/web/12.png)

---

#### Pas 13: Configuració del forwarding IPv4 al router

Edició del fitxer `/etc/sysctl.conf` al router amb nano per habilitar permanentment el forwarding de paquets IPv4. Es descomenta la línia `net.ipv4.ip_forward=1` per permetre que el router encamini paquets entre diferents interfícies de xarxa (DMZ, Intranet i NAT). Aquesta configuració és essencial per al funcionament del router com a gateway entre les diferents zones de seguretat.

![Configuració IP forwarding](./Photos/sprint%202/web/13.png)

---

#### Pas 14: Creació i habilitació del servei de persistència d'iptables

Creació de l'script `/usr/local/bin/iptables-rules.sh` amb permisos d'execució (`chmod +x`) i del servei systemd `/etc/systemd/system/iptables-rules.service` per fer persistents les regles d'iptables després de reinicis. El servei es configura amb:
- **Type=oneshot:** Execució única
- **ExecStart:** Carrega les regles des de l'script
- **RemainAfterExit=yes:** Manté el servei actiu
- **WantedBy=multi-user.target:** S'inicia en mode multiusuari

S'executa `sudo systemctl daemon-reload` per recarregar la configuració de systemd. S'habilita amb `sudo systemctl enable iptables-rules.service` creant el symlink corresponent. S'inicia amb `sudo systemctl start iptables-rules.service`. La verificació amb `sudo systemctl status iptables-rules.service` mostra que el servei està **active (exited)** des del 17 de novembre a les 16:05:05 CET amb PID 2176 i status=0/SUCCESS. El contingut del servei es verifica amb `sudo cat /etc/systemd/system/iptables-rules.service`.

![Servei persistència iptables](./Photos/sprint%202/web/14.png)

---

#### Pas 15: Creació del servei de ruta estàtica al router (Servidor Web)

Visualització del fitxer `/etc/systemd/system/add-static-route.service` al servidor web (W-NCC) que configura una ruta estàtica cap a la xarxa Intranet. El servei està configurat amb:
- **Description:** "Agregar ruta estática al iniciar"
- **After=network-online.target:** S'executa després que la xarxa estigui disponible
- **Wants=network-online.target:** Depèn de la xarxa
- **Type=oneshot:** Execució única
- **ExecStart:** `/usr/sbin/ip route replace 192.168.60.0/24 via 192.168.6.1`
- **RemainAfterExit=yes:** Manté el servei actiu
- **WantedBy=multi-user.target:** S'inicia automàticament

Aquesta ruta permet al servidor web de la DMZ comunicar-se amb la xarxa Intranet a través del router.

![Servei ruta estàtica Web Server](./Photos/sprint%202/web/15.png)

---

#### Pas 16: Verificació del servei de ruta estàtica al servidor de base de dades

Comprovació del contingut del fitxer `/etc/systemd/system/add-static-route.service` al servidor de base de dades (B-N06) amb una configuració similar al del servidor web. El servei estableix la ruta estàtica amb:
- **ExecStart:** `/usr/sbin/ip route replace 192.168.6.0/24 via 192.168.60.1`

Aquesta configuració permet al servidor de base de dades de la Intranet comunicar-se amb la DMZ a través del router. La ruta apunta a la xarxa DMZ (192.168.6.0/24) via el gateway de la Intranet (192.168.60.1).

![Verificació ruta estàtica Database](./Photos/sprint%202/web/16.png)

---

#### Pas 17: Connexió SSH des del servidor FTP al Web Server

Connexió SSH exitosa des del servidor FTP (F-NCC) al servidor web utilitzant el port personalitzat 2222 amb la comanda `ssh -p 2222 bchecker@192.168.6.10`. S'introdueix la contrasenya de l'usuari bchecker i s'accedeix correctament al sistema Ubuntu 22.04.4 LTS (GNU/Linux 6.5.0-28-generic x86_64). 

El sistema mostra informació de:
- **Documentation:** https://help.ubuntu.com
- **Management:** https://landscape.canonical.com
- **Support:** https://ubuntu.com/pro

S'inclou la nota sobre "ABSOLUTELY NO WARRANTY" i els termes de distribució del software lliure. El darrer login va ser el dilluns 10 de novembre a les 17:32:01 2025 des de 192.168.6.11 (servidor FTP). Es mostra un missatge indicant que no es pot canviar al directori personal de bchecker i suggereix executar comandes com a administrador amb sudo. Finalment es mostra el prompt `bchecker@W-NCC:/$` confirmant l'accés exitós.

![Connexió SSH FTP a Web](./Photos/sprint%202/web/17.png)

---

#### Pas 18: Instal·lació de PHP i mòduls necessaris

Instal·lació dels paquets PHP essencials per al funcionament de l'aplicació web amb la comanda:
```bash
sudo apt install php libapache2-mod-php php-mysql php-cli php-curl php-json php-mbstring php-xml php-gd -y
```

El procés instal·la els següents paquets:
- **libapache2-mod-php8.1:** Mòdul PHP per Apache
- **php8.1-cli:** Interfície de línia de comandes de PHP
- **php8.1-common:** Arxius comuns de PHP
- **php8.1-curl:** Mòdul cURL per PHP
- **php8.1-gd:** Mòdul GD per manipulació d'imatges
- **php8.1-json:** Mòdul JSON
- **php8.1-mbstring:** Mòdul per strings multibyte
- **php8.1-mysql:** Mòdul MySQL per PHP
- **php8.1-opcache:** Caché d'opcodes
- **php8.1-readline:** Mòdul readline
- **php8.1-xml:** Mòdul XML

El sistema indica que s'instal·laran 22 paquets nous (0 actualitzats, 0 per eliminar, 362 no actualitzats) amb un total de 6.128 kB d'arxius.

![Instal·lació PHP](./Photos/sprint%202/web/18.png)

---

#### Pas 20: Verificació de la instal·lació de PHP i mòduls

Comprovació de la versió de PHP instal·lada amb la comanda `php -v` mostrant:
- **PHP 8.1.2-1ubuntu2.22 (cli)** compilat el 15 de juliol de 2025
- **Zend Engine v4.1.2** amb Copyright de Zend Technologies
- **Zend OPcache v8.1.2-1ubuntu2.22** habilitat

Verificació dels mòduls MySQL amb `php -m | grep -E 'mysqli|pdo'` confirmant que els mòduls **mysqli**, **nd_mysqli** i **pdo_mysql** estan instal·lats i disponibles. Aquests mòduls són essencials per a la connexió de PHP amb la base de dades MySQL.

Finalment, s'edita el fitxer `/etc/apache2/mods-enabled/dir.conf` amb nano per assegurar que index.php tingui prioritat en el DirectoryIndex.

![Verificació PHP i mòduls](./Photos/sprint%202/web/20.png)

---

#### Pas 21: Creació de l'arxiu test.php i configuració de permisos

Creació de l'arxiu `/var/www/html/test.php` amb el contingut bàsic `<?php phpinfo(); ?>` per mostrar la informació completa de configuració de PHP. Es configuren els permisos adequats:
- `sudo chown www-data:www-data /var/www/html/test.php` per assignar la propietat a l'usuari del servidor web
- `sudo chmod 644 /var/www/html/test.php` per establir permisos de lectura/escriptura per al propietari i només lectura per a altres

Es verifica el contingut del fitxer amb `sudo cat /var/www/html/test.php` mostrant el codi PHP que crida a la funció phpinfo().

Accés mitjançant el navegador a http://192.168.6.10/test.php mostrant la pàgina d'informació de PHP. Es visualitza:
- **PHP Version 8.1.2-1ubuntu2.22** amb el logo de PHP
- Informació del sistema amb TMPT_DYNAMIC del dijous 4 d'abril a les 14:38
- Taula amb paràmetres de configuració incloent System, Build Date, Build System, Server API, Virtual Directory Support, Configuration File, Loaded Configuration File
- Valors de PHP API, PHP Extension, Zend Extension
- Debug Build: no
- Thread Safety: disabled
- Zend Signal Handling: enabled
- Zend Memory Manager: enabled
- Zend Multibyte Support: provided by mbstring
- IPv6 Support: enabled
- DTrace Support: available, disabled
- Registered PHP Streams amb múltiples protocols suportats

![Creació test.php i phpinfo](./Photos/sprint%202/web/21.png)

---

#### Pas 22: Verificació completa de la configuració PHP

Vista ampliada de la pàgina phpinfo() accessible a http://192.168.6.10/test.php mostrant informació detallada sobre la configuració de PHP. La captura mostra:

**Informació general:**
- PHP Version 8.1.2-1ubuntu2.22
- System, Build Date, Build System, Server API
- Virtual Directory Support, Configuration File
- Loaded Configuration Files amb la llista completa d'arxius .ini carregats

**Extensions i mòduls carregats:**
S'observa una llarga llista d'arxius de configuració en el directori `/etc/php/8.1/apache2/conf.d/` incloent:
- 10-opcache.ini
- 20-calendar.ini, 20-ctype.ini, 20-curl.ini
- 20-exif.ini, 20-ffi.ini, 20-fileinfo.ini
- 20-ftp.ini, 20-gd.ini, 20-gettext.ini
- 20-iconv.ini, 20-mbstring.ini, 20-mysqli.ini
- 20-pdo.ini, 20-pdo_mysql.ini
- 20-posix.ini, 20-readline.ini
- 20-shmop.ini, 20-simplexml.ini
- 20-sockets.ini, 20-sysvmsg.ini
- 20-tokenizer.ini, 20-xmlreader.ini, 20-xmlwriter.ini, 20-xsl.ini

**Paràmetres tècnics:**
- PHP API: 20210902
- PHP Extension: 20210902
- Zend Extension: 420210902
- Zend Extension Build: API420210902,NTS
- PHP Extension Build: API20210902,NTS
- Debug Build: no
- Thread Safety: disabled
- Zend Signal Handling: enabled
- Zend Memory Manager: enabled
- Zend Multibyte Support: provided by mbstring
- IPv6 Support: enabled
- DTrace Support: available, disabled
- Registered PHP Streams: php, file, glob, data, http, ftp, phar

Aquesta verificació confirma que PHP està correctament instal·lat i configurat amb tots els mòduls necessaris per executar l'aplicació web, especialment els mòduls de connexió a MySQL (mysqli, pdo_mysql) que són essencials per a la consulta de la base de dades.

![Detall complet configuració PHP](./Photos/sprint%202/web/22.png)

---

#### Pas 19: Configuració del DirectoryIndex per PHP

Edició del fitxer `/etc/apache2/mods-enabled/dir.conf` amb nano per configurar l'ordre del DirectoryIndex. S'estableix que `index.php` tingui prioritat sobre els altres fitxers d'índex:
```apache
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
```

Aquesta configuració assegura que Apache buscarà primer els arxius index.php abans de buscar index.html o altres fitxers d'índex, prioritzant així les pàgines dinàmiques PHP sobre les estàtiques HTML.

![Configuració DirectoryIndex](./Photos/sprint%202/web/19.png)

---

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