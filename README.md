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
- [ 1 - Configuració Serveis de Xarxa](#sprint-1---configuració-serveis-de-xarxa)
  - [Configuració DNS Server (D-N03)](#configuració-dns-server-d-n03)
  - [Configuració DHCP Server](#configuració-dhcp-server)
  - [Configuració Router R-N01](#configuració-router-r-n01)
- [ 2 - Configuració del Servidor Web](#sprint-2---configuració-del-servidor-web)
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
- **Tecnologia**: Apache2 amb PHP 8.1 sobre Ubuntu 22.04 Desktop
- **Justificació**: 
  - Servidor web accessible des d'Internet per allotjar l'aplicació web pública
  - SSH al port 2222 permet administració remota segura
  - Ubicat a la DMZ per protegir la xarxa interna d'amenaces externes
  - En cas de compromís, no exposa directament la xarxa interna
  - Apache2 ofereix alta estabilitat i compatibilitat amb PHP

##### FTP Server (F-N02) - 192.168.6.11
- **Tecnologia**: vsftpd sobre Ubuntu 22.04 Desktop
- **Justificació**:
  - Transferència segura d'arxius des de/cap a l'exterior
  - Separat del servidor web per limitar l'impacte de vulnerabilitats
  - vsftpd és conegut per la seva seguretat (Very Secure FTP Daemon)
  - Configuració chroot evita accés no autoritzat al sistema de fitxers

#### Avantatges de la DMZ
- **Capa de seguretat addicional**: Els servidors públics estan aïllats de la xarxa interna
- **Control de trànsit**: El router pot inspeccionar i filtrar tot el trànsit entre DMZ i Intranet
- **Contenció d'amenaces**: Si un servidor a la DMZ és compromès, l'atacant no té accés directe a la Intranet

---

### Zona Intranet (192.168.60.0/24)

#### Database Server (B-N03) - 192.168.60.20
- **Tecnologia**: MySQL 8.0 sobre Ubuntu 22.04 Desktop
- **Justificació**:
  - **Base de dades protegida**: No està directament accessible des d'Internet
  - **Dades sensibles segures**: Emmagatzema informació de "CSV Educación BCN" amb usuari "bchecker"
  - **Accés controlat**: Només accessible des de la xarxa interna i mitjançant regles específiques des de la DMZ (port 3306)
  - MySQL 8.0 ofereix millores significatives en rendiment i seguretat respecte a versions anteriors
  - Suport natiu per a JSON i millor gestió de transaccions

#### DHCP Server - 192.168.60.20
- **Tecnologia**: isc-dhcp-server sobre Ubuntu 22.04 Desktop
- **Rang**: 192.168.60.30-100
- **Justificació**:
  - **Gestió automàtica d'IPs**: Facilita l'administració de clients
  - **Escalabilitat**: Permet agregar nous dispositius sense configuració manual
  - **Rang controlat**: El rang 30-100 permet 71 adreces dinàmiques, deixant les primeres IPs per a serveis estàtics
  - isc-dhcp-server és l'estàndard de facto en entorns Linux

#### DNS Server - 192.168.60.20
- **Tecnologia**: BIND9 sobre Ubuntu 22.04 Desktop
- **Domini**: grup6.itb.cat
- **Justificació**:
  - **Resolució de noms interna**: Facilita l'accés als recursos per nom en lloc d'IP
  - **Simplificació de gestió**: Els canvis d'IP no afecten les aplicacions que usen noms
  - **Centralització**: Mateix servidor que DHCP per eficiència de recursos
  - BIND9 és el servidor DNS més utilitzat i fiable

#### Clients
- **PC Ubuntu 22.04 Desktop**: Client Linux amb IP DHCP (192.168.60.30)
- **PC Windows 11**: Client Windows amb IP DHCP (192.168.60.31)
- **Justificació**:
  - **Flexibilitat**: Els usuaris no necessiten coneixements tècnics de xarxa
  - **Mobilitat**: Els dispositius poden canviar d'ubicació sense reconfiguració
  - **Gestió centralitzada**: Tots els canvis es realitzen al servidor DHCP
  - Diversitat de sistemes operatius per simular un entorn real d'oficina

---

### Selecció de Sistemes Operatius

#### Per què Ubuntu 22.04 Desktop en lloc de Server?

**Avantatges per al projecte:**

1. **Interfície gràfica disponible**: Facilita la configuració inicial i resolució de problemes durant el desenvolupament
2. **Eines de diagnòstic visuals**: Permet utilitzar navegadors web i eines gràfiques per verificar el funcionament
3. **Mateix conjunt de paquets**: Ubuntu Desktop inclou tots els paquets de Server amb interfície addicional
4. **Flexibilitat de configuració**: Més senzill alternar entre mode gràfic i línia de comandes
5. **Entorn de proves**: Ideal per a entorns educatius on es necessita visualitzar el que passa

**Per què no Debian 13 Server?**

Tot i que Debian és més lleuger i estable:
- Ubuntu té millor documentació i comunitat més gran
- Cicles de suport LTS (22.04) amb 5 anys de manteniment
- Més familiaritat de l'equip amb l'ecosistema Ubuntu
- Repositories més actualitzats per a paquets com PHP i MySQL

**Per què no Ubuntu 24.04?**

- La versió 22.04 LTS ofereix més estabilitat provada
- Millor compatibilitat amb documentació existent
- Menys problemes potencials amb paquets i dependències
- Suport estès fins 2027

#### Per què Windows 11 per al client?

**Justificació:**

1. **Representació del món real**: La majoria d'usuaris corporatius utilitzen Windows
2. **Proves de compatibilitat**: Permet verificar que els serveis funcionen correctament amb clients Windows
3. **Diversitat de l'entorn**: Combinar Linux i Windows simula un entorn heterogeni real
4. **Funcionalitats natives**: Eines com `ipconfig` i gestió de xarxa gràfica familiars per als usuaris

**Per què no Windows 10?**

- Windows 11 és la versió actual i té millor suport de seguretat
- Millores en l'stack de xarxa i gestió de protocols
- Preparació per a entorns futurs

#### Per què Ubuntu Desktop com a Router?

**Consideracions tècniques:**

1. **iptables preinstal·lat**: Gestió de firewall sense instal·lacions addicionals
2. **Netplan**: Configuració de xarxa moderna i declarativa
3. **Flexibilitat**: Permet configurar NAT, forwarding i regles de firewall fàcilment
4. **Monitorització**: Interfície gràfica per veure l'estat de les interfícies de xarxa
5. **Aprenentatge**: Més didàctic per entendre el funcionament intern d'un router

**Alternativa no seleccionada:**
Un router dedicat (pfSense, OpenWrt) seria més eficient en producció, però menys transparent per a propòsits educatius.

---

### Avantatges de l'Arquitectura Proposada

#### Seguretat
1. **Segmentació en capes**: Internet → DMZ → Intranet
2. **Principi de mínim privilegi**: Cada zona té accés limitat a les altres
3. **Protecció de dades**: Base de dades inaccessible des d'Internet
4. **Punt únic de control**: El router R-N01 gestiona tot el trànsit entre zones
5. **Regles iptables específiques**: Port forwarding només per MySQL (3306) des de Web a Database

#### Escalabilitat
- Fàcil afegir nous servidors a la DMZ o Intranet
- El DHCP permet creixement de clients sense reconfiguració
- Estructura modular que facilita expansions futures
- L'ús d'Ubuntu Desktop facilita la migració a Server en producció

#### Mantenibilitat
- Separació clara de responsabilitats per zona
- DNS intern simplifica canvis d'infraestructura
- Serveis especialitzats en servidors dedicats
- Uniformitat de SO (Ubuntu 22.04) facilita l'administració
- Un únic tipus de sistema operatiu per als servidors redueix la corba d'aprenentatge

#### Rendiment
- El trànsit intern (Intranet) no passa per la DMZ
- La base de dades està a la mateixa xarxa que els clients, reduint latència
- Serveis distribuïts eviten colls d'ampolla
- MySQL 8.0 ofereix millores significatives en velocitat de consultes

#### Cost i Recursos
- Ubuntu Desktop és gratuït i open source
- No requereix llicències adicionals
- Requisits de hardware moderats
- Aprofitament de recursos existents sense necessitat d'hardware especialitzat

---

### Flux de Comunicació

#### Accés Extern → Aplicació Web
1. Usuari d'Internet → R-N01 (NAT via enp1s0)
2. R-N01 → W-N02 (DMZ via enp2s0 - 192.168.6.10)
3. W-N02 → R-N01 → B-N03 (consulta MySQL via enp3s0 - 192.168.60.20:3306)
4. Resposta inversa pel mateix camí

**Seguretat aplicada:**
- Només el Web Server pot accedir al port 3306 del Database Server
- Clients externs no tenen accés directe a la Intranet
- Firewall iptables filtra tot el trànsit no autoritzat

#### Accés Intern
- **Clients → Database**: Comunicació directa a la mateixa xarxa Intranet
- **Clients → Web Server**: A través del router R-N01
- **Clients → Internet**: NAT a R-N01 via enp1s0
- **Clients → DNS/DHCP**: Accés directe al servidor 192.168.60.20

#### Transferència d'Arxius
- **FTP Intern**: Empleats poden pujar/descarregar arxius des de la Intranet
- **FTP Extern**: Accés controlat des d'Internet si es configura port forwarding
- **SSH**: Administració remota segura dels servidors via port 2222

---

### Comparativa amb Altres Opcions

#### Opció Descartada 1: Windows Server 2022

**Desavantatges:**
- Cost de llicències
- Major consum de recursos
- Menys flexibilitat en configuració de xarxa
- Corba d'aprenentatge més pronunciada per a serveis com iptables

**Quan seria adequat:**
- Entorns corporatius 100% Windows
- Necessitat d'Active Directory
- Aplicacions que només funcionen en Windows

#### Opció Descartada 2: Debian 13 Server

**Desavantatges:**
- Menys documentació disponible
- Paquets més antics (estabilitat vs. funcionalitats recents)
- Menor comunitat de suport

**Quan seria adequat:**
- Entorns de producció on la màxima estabilitat és crítica
- Servidors sense necessitat de depuració visual
- Administradors amb experiència en Debian

#### Opció Descartada 3: Ubuntu 24.04

**Desavantatges:**
- Menys temps en producció = menys proves reals
- Possible incompatibilitat amb algunes llibreries
- Documentació encara en desenvolupament

**Quan seria adequat:**
- Necessitat de funcionalitats més modernes
- Projectes que comencen d'zero sense dependències antigues

---

### Conclusió

Aquesta arquitectura implementa les **millors pràctiques de seguretat en xarxes** mitjançant:
- Defensa en profunditat amb múltiples capes de seguretat
- Segmentació de xarxa basada en funcionalitat i nivell d'exposició
- Serveis crítics protegits a la xarxa interna
- Serveis públics aïllats a la DMZ
- Gestió centralitzada i automatitzada de la xarxa interna

La selecció d'**Ubuntu 22.04 Desktop** per als servidors ofereix el millor equilibri entre:
- Facilitat d'ús i configuració
- Suport a llarg termini (LTS fins 2027)
- Compatibilitat amb una àmplia gamma de serveis
- Cost zero (open source)
- Flexibilitat per a entorns educatius i de producció

L'ús de **Windows 11** per al client aporta diversitat al sistema i permet provar la interoperabilitat entre plataformes, simulant un entorn corporatiu real on conviuen diferents sistemes operatius.


---


## 1 - Configuració Serveis de Xarxa

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

---

#### Pas 6: Verificació IP assignada - Client Windows

Verificació que el client Windows ha rebut la IP 192.168.60.31 del servidor DHCP amb gateway 192.168.60.1.

![IP client Windows](./Photos/Sprint%201/DHCP6.png)
```cmd
ipconfig
```

---

#### Pas 7: Comprovació del fitxer de leases

Comprovació del fitxer de leases que mostra l'assignació d'IP al client Windows (DESKTOP-JNU2BQU amb IP dinàmica).

![Fitxer leases DHCP](./Photos/Sprint%201/DHCP7.png)
```bash
sudo tail -f /var/lib/dhcp/dhcpd.leases
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


## 2 - Configuració del Servidor Web

### Configuració Web Server (W-N02)

#### Pas 1: Configuració de la interfície de xarxa

Visualització del fitxer `/etc/netplan/01-network-manager-all.yaml` amb la configuració de la interfície enp2s0 del servidor web amb IP estàtica 192.168.6.10/24, gateway 192.168.6.1 per a la xarxa DMZ.

![Configuració Netplan Web Server](./Photos/sprint%202/web/1.png)
```bash
sudo cat /etc/netplan/01-network-manager-all.yaml
```
```yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enp2s0:
      dhcp4: no
      addresses: [192.168.6.10/24]
      routes:
        - to: default
          via: 192.168.6.1
```
```bash
sudo netplan apply
```

---

#### Pas 2: Proves de connectivitat des del servidor web

Proves de connectivitat mitjançant ping des del servidor web cap al servidor FTP (192.168.6.11) i cap al router (192.168.6.1). Ambdues proves mostren 0% packet loss confirmant la correcta configuració de xarxa a la DMZ.

![Proves connectivitat Web Server](./Photos/sprint%202/web/2.png)
```bash
# Prova de connectivitat amb el servidor FTP
echo "=== FTP ===" && ping 192.168.6.11

# Prova de connectivitat amb el Router
echo "=== Router ===" && ping 192.168.6.1
```

---

#### Pas 3: Configuració de regles iptables al router per al servidor web

Configuració de les regles d'iptables al router R-N01 per permetre l'accés al servidor web. S'afegeixen regles INPUT per acceptar tràfic des de les xarxes DMZ (192.168.6.0/24) i Intranet (192.168.60.0/24), així com per a IPs específiques del web server (192.168.6.10 i 192.168.6.11).

![Regles iptables per Web Server](./Photos/sprint%202/web/3.png)
```bash
# Al router R-N01
sudo iptables -A INPUT -s 192.168.6.1 -j ACCEPT
sudo iptables -A INPUT -s 192.168.6.10 -j ACCEPT
sudo iptables -A INPUT -s 192.168.6.11 -j ACCEPT
sudo iptables -A INPUT -j DROP
```

---

#### Pas 4: Instal·lació i verificació del servei Apache2

Instal·lació del servidor Apache2 i verificació que el servei està actiu (active/running) des del 10 de novembre. Es mostra l'estat del servei amb PID 2266 i el hostname del servidor 192.168.121.26 192.168.6.10.

![Estat servei Apache2](./Photos/sprint%202/web/4.png)
```bash
sudo apt update
sudo apt install apache2 -y
```
```bash
sudo systemctl status apache2
hostname -I
```

---

#### Pas 5: Configuració del firewall UFW i SSL

Verificació de l'estat del servei Apache2, configuració del hostname, habilitació de la regla UFW per "Apache Full" (ports 80 i 443), i habilitació dels mòduls SSL necessaris (ssl, socache_shmcb). El servei està actiu i escoltant en múltiples ports incloent 80, 443 i [::]:22.

![Configuració UFW i SSL](./Photos/sprint%202/web/5.png)
```bash
sudo systemctl status apache2
hostname -I

# Habilitar firewall per Apache
sudo ufw allow 'Apache Full'

# Habilitar UFW
sudo ufw enable

# Verificar estat del firewall
sudo ufw status
```
```bash
# Habilitar mòduls SSL
sudo a2enmod ssl

# Reiniciar Apache
sudo systemctl restart apache2

# Verificar ports escoltant
sudo ss -ltn
```

---

#### Pas 6: Habilitació de mòduls SSL i configuració del lloc per defecte

Execució de la comanda `sudo a2enmod ssl` per habilitar els mòduls SSL (setenvif, mime, socache_shmcb, ssl). Després s'habilita el lloc SSL per defecte amb `sudo a2ensite default-ssl.conf` i es recarrega Apache2. Es verifica la configuració amb `apache2ctl configtest` mostrant un warning sobre el ServerName.

![Habilitació SSL i configuració](./Photos/sprint%202/web/6.png)
```bash
# Habilitar mòdul SSL
sudo a2enmod ssl

# Habilitar lloc per defecte SSL
sudo a2ensite default-ssl.conf

# Recarregar Apache2
sudo systemctl reload apache2

# Verificar configuració
sudo apache2ctl configtest

# Llistar fitxers de configuració SSL
ls -la /etc/apache2/sites-available/default-ssl.conf
```

---

#### Pas 7: Accés HTTP al servidor web des del navegador

Accés al servidor web mitjançant el navegador Firefox a l'adreça http://192.168.6.10 mostrant la pàgina per defecte d'Apache2 Ubuntu. Es visualitza la pàgina de benvinguda confirmant que el servidor web està operatiu i accessible des de la xarxa.

![Accés HTTP al Web Server](./Photos/sprint%202/web/7.png)

**Accedir des del navegador:**
```
http://192.168.6.10
```

---

#### Pas 8: Advertència de seguretat al accedir per HTTPS

Intent d'accés al servidor web mitjançant HTTPS (https://192.168.6.10). Firefox detecta un risc de seguretat potencial perquè el certificat SSL és autosignat (self-signed). Es mostra l'error "MOZILLA_PKIX_ERROR_SELF_SIGNED_CERT" amb opcions per retrocedir o acceptar el risc.

![Advertència certificat SSL](./Photos/sprint%202/web/8.png)

**Accedir des del navegador:**
```
https://192.168.6.10
```

---

#### Pas 9: Accés HTTPS exitós després d'acceptar el certificat

Després d'acceptar el risc de seguretat, s'accedeix correctament al servidor web per HTTPS (https://192.168.6.10) mostrant la mateixa pàgina per defecte d'Apache2 Ubuntu. Això confirma que el servidor està funcionant tant en HTTP com en HTTPS.

![Accés HTTPS al Web Server](./Photos/sprint%202/web/9.png)

---

#### Pas 10: Configuració del servei SSH

Verificació de l'estat del servei SSH amb `sudo systemctl status ssh` mostrant que està actiu des de les 17:02. Es configura el fitxer `/etc/ssh/sshd_config`, es reinicia el servei, i s'afegeix la regla UFW per permetre el port 2222/tcp. L'estat del firewall mostra les regles actives per Apache Full i SSH (port 2222).

![Configuració servei SSH](./Photos/sprint%202/web/10.png)
```bash
# Verificar estat del servei SSH
sudo systemctl status ssh

# Editar configuració SSH
sudo nano /etc/ssh/sshd_config

# Reiniciar servei SSH
sudo systemctl restart ssh

# Permetre SSH al port 2222
sudo ufw allow 2222/tcp

# Habilitar UFW
sudo ufw enable

# Verificar estat del firewall
sudo ufw status
```

---

#### Pas 11: Configuració detallada del fitxer sshd_config

Visualització del fitxer de configuració `/etc/ssh/sshd_config` amb nano mostrant els paràmetres principals: Port 2222, autenticació per clau pública habilitada (PubkeyAuthentication yes), login de root deshabilitat (PermitRootLogin no), i configuració de logging i autenticació.

![Configuració sshd_config](./Photos/sprint%202/web/11.png)
```bash
sudo nano /etc/ssh/sshd_config
```
```conf
# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.

Include /etc/ssh/sshd_config.d/*.conf

Port 2222
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

#HostKey /etc/ssh/ssh_host_rsa_key
#HostKey /etc/ssh/ssh_host_ecdsa_key
#HostKey /etc/ssh/ssh_host_ed25519_key

# Ciphers and keying
#RekeyLimit default none

# Logging
#SyslogFacility AUTH
#LogLevel INFO

# Authentication:
#LoginGraceTime 2m
PermitRootLogin no
#StrictModes yes
MaxAuthTries 3
#MaxSessions 10

PubkeyAuthentication yes
```
```bash
# Reiniciar SSH després dels canvis
sudo systemctl restart ssh
```

---

#### Pas 12: Configuració d'iptables al router per SSH

Configuració de les regles d'iptables al router R-N01 per permetre l'accés SSH al servidor web. S'afegeixen regles INPUT per permetre: loopback, connexions establertes, ping (ICMP), SSH al router (port 22), accés des de les xarxes DMZ i Intranet, i accés a IPs específiques del web server. També s'afegeix una regla FORWARD bidireccional entre les xarxes DMZ i Intranet.

![Regles iptables per SSH](./Photos/sprint%202/web/12.png)
```bash
# Al router R-N01

# Permitir loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Permitir conexiones ya establecidas
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Permitir ping (ICMP)
sudo iptables -A INPUT -p icmp -j ACCEPT

# Permitir SSH al router
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Permitir acceso desde tus redes
sudo iptables -A INPUT -s 192.168.6.0/24 -j ACCEPT
sudo iptables -A INPUT -s 192.168.60.0/24 -j ACCEPT

# Permitir acceso desde IPs específicas
sudo iptables -A INPUT -s 192.168.6.10 -j ACCEPT
sudo iptables -A INPUT -s 192.168.6.11 -j ACCEPT
sudo iptables -A INPUT -s 192.168.60.15 -j ACCEPT

# Permitir tráfico entre 192.168.6.0/24 y 192.168.60.0/24 (BIDIRECCIONAL)
sudo iptables -A FORWARD -i enp2s0 -o enp3s0 -s 192.168.6.0/24 -d 192.168.60.0/24 -j ACCEPT
sudo iptables -A FORWARD -i enp3s0 -o enp2s0 -s 192.168.60.0/24 -d 192.168.6.0/24 -j ACCEPT

# Habilitar forwarding IPv4
sudo sysctl -w net.ipv4.ip_forward=1
```

---

#### Pas 13: Configuració del forwarding IPv4 al router

Edició del fitxer `/etc/sysctl.conf` al router amb nano per habilitar el forwarding de paquets IPv4. Es descomenta la línia `net.ipv4.ip_forward=1` per permetre que el router encamini paquets entre diferents interfícies de xarxa.

![Configuració IP forwarding](./Photos/sprint%202/web/13.png)
```bash
# Al router R-N01
sudo nano /etc/sysctl.conf
```

Descomentar la línia:
```conf
net.ipv4.ip_forward=1
```
```bash
# Aplicar canvis
sudo sysctl -p
```

---

#### Pas 14: Creació i habilitació del servei de persistència d'iptables

Creació de l'script `/usr/local/bin/iptables-rules.sh` i del servei systemd `/etc/systemd/system/iptables-rules.service` per fer persistents les regles d'iptables. S'habilita i s'inicia el servei amb `systemctl enable/start iptables-rules.service`. La verificació mostra que el servei està actiu (active/exited) i s'ha carregat correctament.

![Servei persistència iptables](./Photos/sprint%202/web/14.png)
```bash
# Crear script de regles iptables
sudo nano /usr/local/bin/iptables-rules.sh
```
```bash
#!/bin/bash
# Script per carregar regles iptables

# Aquí van les teves regles iptables
iptables-restore < /etc/iptables/rules.v4
```
```bash
# Fer executable l'script
sudo chmod +x /usr/local/bin/iptables-rules.sh

# Crear servei systemd
sudo nano /etc/systemd/system/iptables-rules.service
```
```ini
[Unit]
Description=Configurar reglas de iptables para el router
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/iptables-rules.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```
```bash
# Recarregar systemd
sudo systemctl daemon-reload

# Habilitar servei
sudo systemctl enable iptables-rules.service

# Iniciar servei
sudo systemctl start iptables-rules.service

# Verificar estat
sudo systemctl status iptables-rules.service

# Verificar contingut del servei
sudo cat /etc/systemd/system/iptables-rules.service
```

---

#### Pas 15: Creació del servei de ruta estàtica al router (Servidor Web)

Visualització del fitxer `/etc/systemd/system/add-static-route.service` al servidor web (W-NCC) que configura una ruta estàtica cap a la xarxa Intranet (192.168.60.0/24) via el router (192.168.6.1). Aquest servei s'executa després de la xarxa estar disponible.

![Servei ruta estàtica Web Server](./Photos/sprint%202/web/15.png)
```bash
# Al servidor Web (W-NCC)
sudo cat /etc/systemd/system/add-static-route.service
```
```ini
[Unit]
Description=Agregar ruta estática al iniciar
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/sbin/ip route replace 192.168.60.0/24 via 192.168.6.1
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

---

#### Pas 16: Verificació del servei de ruta estàtica

Comprovació del contingut del fitxer `/etc/systemd/system/add-static-route.service` al servidor de base de dades (B-N06) amb una configuració similar, establint la ruta estàtica cap a la xarxa DMZ (192.168.6.0/24) via el router de la Intranet (192.168.60.1).

![Verificació ruta estàtica Database](./Photos/sprint%202/web/16.png)
```bash
# Al servidor de Base de Dades (B-N06)
cat /etc/systemd/system/add-static-route.service
```
```ini
[Unit]
Description=Agregar ruta estática al iniciar
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/sbin/ip route replace 192.168.6.0/24 via 192.168.60.1
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

---

#### Pas 17: Connexió SSH des del servidor FTP al Web Server

Connexió SSH exitosa des del servidor FTP (F-NCC) al servidor web utilitzant el port 2222 amb la comanda `ssh -p 2222 bchecker@192.168.6.10`. S'accedeix correctament al sistema Ubuntu 22.04.4 LTS mostrant informació de documentació, management i suport. El darrer login va ser des de 192.168.6.11.

![Connexió SSH FTP a Web](./Photos/sprint%202/web/17.png)
```bash
# Des del servidor FTP (F-NCC)
ssh -p 2222 bchecker@192.168.6.10
```

---

#### Pas 18: Instal·lació de PHP i mòduls necessaris

Instal·lació de PHP 8.1 i tots els mòduls necessaris per a l'aplicació web, incloent php-mysql, php-curl, php-json, php-xml, php-mbstring, php-gd, etc.

![Instal·lació PHP](./Photos/sprint%202/web/18.png)
```bash
sudo apt install php libapache2-mod-php php-mysql php-cli php-curl php-gd php-json php-mbstring php-xml php-gd -y
```

---

#### Pas 19: Configuració del DirectoryIndex per PHP

Edició del fitxer `/etc/apache2/mods-enabled/dir.conf` amb nano per configurar l'ordre del DirectoryIndex. S'estableix que index.php tingui prioritat sobre els altres fitxers d'índex (index.html, index.cgi, etc.).

![Configuració DirectoryIndex](./Photos/sprint%202/web/19.png)
```bash
sudo nano /etc/apache2/mods-enabled/dir.conf
```
```xml
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
```bash
# Reiniciar Apache
sudo systemctl restart apache2
```
---

#### Pas 20: Verificació de la instal·lació de PHP i mòduls

Comprovació de la versió de PHP instal·lada mostrant PHP 8.1.2-1ubuntu2.22 amb Zend Engine v4.1.2 i Zend OPcache v8.1.2. S'executa la comanda `php -m | grep -E 'mysqli|pdo'` per verificar que els mòduls mysqli i pdo_mysql estan instal·lats correctament.

![Verificació PHP i mòduls](./Photos/sprint%202/web/20.png)
```bash
# Verificar versió de PHP
php -v

# Verificar mòduls mysqli i pdo
php -m | grep -E 'mysqli|pdo'
```
```bash
# Editar configuració DirectoryIndex
sudo nano /etc/apache2/mods-enabled/dir.conf
```

---

#### Pas 21: Creació de l'arxiu test.php i configuració de permisos

Creació de l'arxiu `/var/www/html/test.php` amb la funció `phpinfo();` per mostrar la informació de configuració de PHP. Es configuren els permisos adequats amb `chown www-data:www-data` i `chmod 644`. Es verifica el contingut del fitxer mostrant el codi PHP bàsic.

![Creació test.php](./Photos/sprint%202/web/21.png)
```bash
# Crear fitxer test.php
sudo nano /var/www/html/test.php
```
```php
<?php
phpinfo();
?>
```
```bash
# Configurar propietari i permisos
sudo chown www-data:www-data /var/www/html/test.php
sudo chmod 644 /var/www/html/test.php

# Verificar contingut
cat /var/www/html/test.php
```

**Accedir des del navegador:**
```
http://192.168.6.10/test.php
```

---

#### Pas 22: Accés web a test.php i verificació completa de PHP

Accés mitjançant el navegador a http://192.168.6.10/test.php mostrant la pàgina d'informació de PHP (phpinfo). Es visualitza la versió PHP 8.1.2-1ubuntu2.22 amb informació detallada del sistema, build date, server API, directives de configuració, PHP Extension, Zend Extension i altres paràmetres tècnics del servidor PHP. Aquesta pàgina confirma que PHP està correctament integrat amb Apache2 i que tots els mòduls necessaris estan carregats.

![Visualització phpinfo()](./Photos/sprint%202/web/22.png)

**Informació mostrada al phpinfo():**
- **PHP Version:** 8.1.2-1ubuntu2.22
- **System:** Linux
- **Build Date:** Apr 4 14:35
- **Server API:** Apache 2.0 Handler
- **PHP API:** 20210902
- **PHP Extension:** 20210902
- **Zend Extension:** 420210902
- **Debug Build:** no
- **Thread Safety:** disabled
- **Zend Signal Handling:** enabled
- **Zend Memory Manager:** enabled
- **IPv6 Support:** enabled

**Mòduls verificats:**
- mysqli 
- pdo_mysql 
- json 
- mbstring 
- xml 
- curl 
- gd 
```bash
# Opcio opcional: Eliminar test.php després de verificar
# sudo rm /var/www/html/test.php
```

---


### Configuració Database Server (B-N03)

#### Pas 1: Instal·lació del servidor MySQL

Instal·lació del paquet `mysql-server` amb la comanda `sudo apt install mysql-server`. El sistema descarrega i instal·la automàticament tots els paquets necessaris incloent llibreries perl, llibcgi, libevent, mecab i les dependències de MySQL 8.0.

![Instal·lació MySQL Server](./Photos/sprint%202/BBDD/BBDD1.png)
```bash
sudo apt install mysql-server
```

---

#### Pas 2: Creació de la taula equipaments

En aquest projecte s'ha optat per definir alguns identificadors (com register_id i institution_id) com a VARCHAR en lloc d'INTEGER, ja que els valors que provenen de les fonts originals són molt grans o no segueixen un format numèric estrictament. Emprar VARCHAR evita problemes de desbordament d'enter (overflow) i garanteix que es puguin emmagatzemar codis mixtos o identifiers que puguin incloure zeros a l'esquerra o formats no numèrics.

![Taula equipaments](./Photos/sprint%202/BBDD/BBDD2.png)
```sql
CREATE TABLE equipaments (
    register_id VARCHAR(50),
    nom VARCHAR(255) NOT NULL,
    institution_id VARCHAR(50),
    institution_name VARCHAR(255),
    created TIMESTAMP,
    modified TIMESTAMP,
    geo_x FLOAT,
    geo_y FLOAT,
    latitude FLOAT,
    longitude FLOAT,
    estimated_dates VARCHAR(100),
    start_date DATE,
    end_date DATE,
    timetable TEXT
);
```

---

#### Pas 3: Creació de la taula direccions

Script SQL per crear la taula `direccions` amb clau forana (FOREIGN KEY) referenciada a equipaments. Inclou camps: equipment_id (VARCHAR 50), roadtype_id, roadtype_name, road_id, road_name (VARCHAR 255), start_street_number i end_street_number (VARCHAR 10), neighborhood_id, neighborhood_name, district_id, district_name (VARCHAR 100), zip_code (VARCHAR 10), town (VARCHAR 100), main_address i address_type (VARCHAR 50).

![Taula direccions](./Photos/sprint%202/BBDD/BBDD3.png)
```sql
CREATE TABLE direccions (
    equipament_id VARCHAR(50),           -- Referencia a equipaments
    roadtype_id VARCHAR(50),
    roadtype_name VARCHAR(100),
    road_id VARCHAR(50),
    road_name VARCHAR(255),
    start_street_number VARCHAR(10),
    end_street_number VARCHAR(10),
    neighborhood_id VARCHAR(50),
    neighborhood_name VARCHAR(100),
    district_id VARCHAR(50),
    district_name VARCHAR(100),
    zip_code VARCHAR(10),
    town VARCHAR(100),
    main_address VARCHAR(50),
    address_type VARCHAR(50),
    FOREIGN KEY (equipament_id) REFERENCES equipaments(register_id)
);
```

---

#### Pas 4: Creació de la taula valors

Script SQL per crear la taula `valors` amb clau forana referenciada a equipaments. Inclou camps: equipment_id (VARCHAR 50), attribute_id (VARCHAR 50), values_id (VARCHAR 50), category (VARCHAR 100), attribute_name (VARCHAR 100), value (VARCHAR 255), outstanding (VARCHAR 50), i description (TEXT).

![Taula valors](./Photos/sprint%202/BBDD/BBDD4.png)
```sql
CREATE TABLE valors (
    equipament_id VARCHAR(50),
    attribute_id VARCHAR(50),
    values_id VARCHAR(50),
    category VARCHAR(100),
    attribute_name VARCHAR(100),
    value VARCHAR(255),
    outstanding VARCHAR(50),
    description TEXT,
    FOREIGN KEY (equipament_id) REFERENCES equipaments(register_id)
);
```

---

#### Pas 5: Creació de la taula filtres_secundaris

Script SQL per crear la taula `filtres_secundaris` amb clau forana referenciada a equipaments. Inclou camps: equipment_id (VARCHAR 50), filter_id (VARCHAR 50), filter_name (VARCHAR 255), filter_fullpath (TEXT), filter_tree (VARCHAR 255), i filter_asia_id (VARCHAR 50).

![Taula filtres_secundaris](./Photos/sprint%202/BBDD/BBDD5.png)
```sql
CREATE TABLE filtres_secundaris (
    equipament_id VARCHAR(50),
    filter_id VARCHAR(50),
    filter_name VARCHAR(255),
    filter_fullpath TEXT,
    filter_tree VARCHAR(255),
    filter_asia_id VARCHAR(50),
    FOREIGN KEY (equipament_id) REFERENCES equipaments(register_id)
);
```

---

#### Pas 6: Verificació de les taules creades

Execució de la comanda `SHOW TABLES;` a MySQL mostrant les 4 taules creades correctament a la base de dades EquipamentsBCN: direccions, equipaments, filtres_secundaris i valors. El resultat mostra "4 rows in set (0,01 sec)".

![Verificació taules MySQL](./Photos/sprint%202/BBDD/BBDD6.png)
```sql
SHOW TABLES;
```

---

#### Pas 7: Visualització de l'estructura de la taula equipaments

Execució de la comanda `SHOW COLUMNS FROM equipaments;` mostrant l'estructura completa de la taula amb 14 camps: register_id (PRI, varchar 50), nom (varchar 255), institution_id, institution_name, created i modified (timestamp), geo_x, geo_y, latitude i longitude (float), estimated_dates (varchar 100), start_date i end_date (date), i timetable (text).

![Columnes taula equipaments](./Photos/sprint%202/BBDD/BBDD7.png)
```sql
SHOW COLUMNS FROM equipaments;
```

---

#### Pas 8: Visualització de l'estructura de la taula direccions

Execució de la comanda `SHOW COLUMNS FROM direccions;` mostrant l'estructura completa amb 15 camps. Equipment_id està configurat com a MUL (clau múltiple/forana) referenciant la taula equipaments. Tots els camps són VARCHAR excepte address_type, amb mides que varien entre VARCHAR(10) i VARCHAR(255).

![Columnes taula direccions](./Photos/sprint%202/BBDD/BBDD8.png)
```sql
SHOW COLUMNS FROM direccions;
```

---

#### Pas 9: Visualització de l'estructura de la taula valors

Execució de la comanda `SHOW COLUMNS FROM valors;` mostrant l'estructura amb 8 camps. Equipment_id està configurat com a MUL (clau forana). Els camps inclouen: equipment_id, attribute_id, values_id (VARCHAR 50), category i attribute_name (VARCHAR 100), value (VARCHAR 255), outstanding (VARCHAR 50) i description (TEXT).

![Columnes taula valors](./Photos/sprint%202/BBDD/BBDD9.png)
```sql
SHOW COLUMNS FROM valors;
```

---

#### Pas 10: Visualització de l'estructura de la taula filtres_secundaris

Execució de la comanda `SHOW COLUMNS FROM filtres_secundaris;` mostrant l'estructura amb 6 camps. Equipment_id està configurat com a MUL (clau forana). Els camps inclouen: equipment_id, filter_id (VARCHAR 50), filter_name (VARCHAR 255), filter_fullpath (TEXT), filter_tree (VARCHAR 255) i filter_asia_id (VARCHAR 50).

![Columnes taula filtres_secundaris](./Photos/sprint%202/BBDD/BBDD10.png)
```sql
SHOW COLUMNS FROM filtres_secundaris;
```

---

### Configuració FTP Server (F-N02)

#### Pas 1: Instal·lació del servidor vsftpd

Instal·lació del paquet `vsftpd` (Very Secure FTP Daemon) amb la comanda `sudo apt install vsftpd`. El sistema descarrega i instal·la el paquet vsftpd versió 3.0.5-0ubuntu1.1 amb una mida de 123 kB. Es crea automàticament el servei systemd i s'instal·len les dependències necessàries.

![Instal·lació vsftpd](./Photos/sprint%202/ftp/ftp1.png)
```bash
sudo apt install vsftpd
```

---

#### Pas 2: Verificació de la versió de vsftpd

Comprovació de la versió instal·lada de vsftpd amb la comanda `vsftpd -v`. El sistema confirma que s'ha instal·lat la versió 3.0.5 del servidor FTP.

![Versió vsftpd](./Photos/sprint%202/ftp/ftp2.png)
```bash
vsftpd -v
```

---

#### Pas 3: Habilitació del servei vsftpd

Habilitació del servei vsftpd per iniciar-se automàticament amb l'arrencada del sistema mitjançant la comanda `sudo systemctl enable vsftpd`. Això crea els enllaços simbòlics necessaris per al servei.

![Habilitació servei vsftpd](./Photos/sprint%202/ftp/ftp3.png)
```bash
sudo systemctl enable vsftpd
```

---

#### Pas 4: Inici i verificació de l'estat del servei vsftpd

Execució de les comandes `sudo systemctl start vsftpd` i `sudo systemctl status vsftpd` per iniciar i verificar l'estat del servei. El servei està actiu (active/running) des del 10 de novembre a les 16:07, amb PID 2329, consumint 868.0K de memòria i llegint la configuració del fitxer `/etc/vsftpd.conf`.

![Estat servei vsftpd](./Photos/sprint%202/ftp/ftp4.png)
```bash
sudo systemctl start vsftpd
sudo systemctl status vsftpd
```

---

#### Pas 5: Accés al fitxer de configuració vsftpd.conf

Edició del fitxer de configuració principal `/etc/vsftpd.conf` amb nano per configurar els paràmetres del servidor FTP.

![Edició vsftpd.conf](./Photos/sprint%202/ftp/ftp5.png)
```bash
sudo nano /etc/vsftpd.conf
```

---

#### Pas 6: Habilitació d'usuaris locals

Configuració del paràmetre `local_enable=YES` al fitxer vsftpd.conf per permetre que els usuaris locals del sistema puguin iniciar sessió al servidor FTP.

![Habilitació usuaris locals](./Photos/sprint%202/ftp/ftp6.png)
```conf
# Uncomment this to allow local users to log in.
local_enable=YES
```

---

#### Pas 7: Deshabilitació de l'accés anònim

Configuració del paràmetre `anonymous_enable=NO` al fitxer vsftpd.conf per deshabilitar l'accés anònim al servidor FTP, millorant així la seguretat del sistema.

![Deshabilitació accés anònim](./Photos/sprint%202/ftp/ftp7.png)
```conf
# Allow anonymous FTP? (Disabled by default)
anonymous_enable=NO
```

---

#### Pas 8: Habilitació d'escriptura FTP

Configuració del paràmetre `write_enable=YES` al fitxer vsftpd.conf per permetre qualsevol forma de comandes d'escriptura FTP, incloent pujada, eliminació i modificació d'arxius.

![Habilitació escriptura FTP](./Photos/sprint%202/ftp/ftp8.png)
```conf
# Uncomment this to enable any form of FTP write command.
write_enable=YES
```

---

#### Pas 9: Restricció d'usuaris al directori personal

Configuració del paràmetre `chroot_local_user=YES` al fitxer vsftpd.conf per restringir els usuaris locals als seus directoris personals (chroot jail), impedint que naveguin per altres parts del sistema de fitxers per motius de seguretat.

![Restricció chroot](./Photos/sprint%202/ftp/ftp9.png)
```conf
# You may restrict local users to their home directories. See the FAQ for
# the possible risks in this before using chroot_local_user or
# chroot_list_enable below.
chroot_local_user=YES
```

---

#### Pas 10: Reinici i verificació del servei vsftpd

Reinici del servei vsftpd amb `sudo systemctl restart vsftpd` per aplicar els canvis de configuració. La verificació amb `sudo systemctl status vsftpd` confirma que el servei està actiu (active/running) des de les 16:13:16 amb PID 2787.

![Reinici servei vsftpd](./Photos/sprint%202/ftp/ftp10.png)
```bash
sudo systemctl restart vsftpd
sudo systemctl status vsftpd
```

---

#### Pas 11: Creació de l'usuari FTP usuarigroup6

Creació de l'usuari `usuarigroup6` amb la comanda `sudo adduser usuarigroup6`. El sistema crea l'usuari amb UID 1002, el grup usuarigroup6 (GID 1002), el directori personal `/home/usuarigroup6`, i es configura la contrasenya i la informació del usuari.

![Creació usuari FTP](./Photos/sprint%202/ftp/ftp11.png)
```bash
sudo adduser usuarigroup6
```

---

#### Pas 12: Configuració del directori FTP i permisos

Creació del directori `/home/usuarigroup6/ftp` amb `sudo mkdir -p`, assignació de propietat a nobody:nogroup amb `sudo chown nobody:nogroup`, i configuració dels permisos a+w amb `sudo chmod a+w` per permetre l'escriptura a tots els usuaris.

![Configuració directori FTP](./Photos/sprint%202/ftp/ftp12.png)
```bash
sudo mkdir -p /home/usuarigroup6/ftp
sudo chown nobody:nogroup /home/usuarigroup6/ftp
sudo chmod a+w /home/usuarigroup6/ftp
```

---

#### Pas 13: Configuració del firewall per FTP

Habilitació del port 22/tcp al firewall UFW amb la comanda `sudo ufw allow 22/tcp` per permetre les connexions SSH (nota: per FTP passiu caldria obrir també el port 21 i el rang de ports passius).

![Configuració firewall FTP](./Photos/sprint%202/ftp/ftp13.png)
```bash
sudo ufw allow 22/tcp
```

---

#### Pas 14: Prova de connexió SSH des del servidor FTP

Connexió SSH exitosa des del servidor FTP (F-NCC) al servidor web utilitzant el port 2222 amb la comanda `ssh -p 2222 bchecker@192.168.6.10`. S'accedeix correctament al sistema Ubuntu 22.04.4 LTS mostrant la informació de benvinguda. El darrer login va ser el 10 de novembre a les 17:32:01 des de 192.168.6.11.

![Connexió SSH des de FTP](./Photos/sprint%202/ftp/ftp14.png)
```bash
ssh -p 2222 bchecker@192.168.6.10
```

---

# 3 - Configuració de l'Aplicació Web


---

## Configuració de l'Aplicació Web

### Pas 1: Verificació dels fitxers de l'aplicació web

Comprovació dels permisos i propietaris dels fitxers principals de l'aplicació web al directori `/var/www/html/`:
- **index.html:** Propietari root:root amb permisos 644 (rw-r--r--), mida 20097 bytes
- **api.php:** Propietari www-data:www-data amb permisos 644 (rw-r--r--), mida 3480 bytes

![Verificació fitxers aplicació](./Photos/sprint%203/web1.png)
```bash
# Navegar al directori web
cd /var/www/html/

# Verificar permisos i propietaris dels fitxers
ls -ld index.html
ls -ld api.php
```

---

### Pas 2: Creació del fitxer index.html

Creació del fitxer `/var/www/html/index.html` que conté la interfície frontend de l'aplicació web. Aquest fitxer inclou HTML5, CSS3 amb gradients i animacions, i JavaScript per gestionar la comunicació amb l'API.

![Creació index.html](./Photos/sprint%203/web2.png)
```bash
# Crear/editar el fitxer index.html
sudo nano /var/www/html/index.html
```

**Contingut complet del fitxer index.html:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MySQL Dashboard - Database</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0f23 0%, #1a1f35 100%);
            min-height: 100vh;
            padding: 20px;
            color: #e0e6ed;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            color: #e0e6ed;
            margin-bottom: 40px;
            animation: fadeIn 0.8s ease-in;
            border: 1px solid rgba(59, 130, 246, 0.3);
            padding: 30px;
            background: linear-gradient(135deg, rgba(15, 23, 42, 0.9) 0%, rgba(30, 41, 59, 0.9) 100%);
            border-radius: 12px;
            box-shadow: 0 4px 24px rgba(59, 130, 246, 0.1);
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            background: linear-gradient(135deg, #60a5fa 0%, #3b82f6 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            letter-spacing: 2px;
            font-weight: 700;
        }

        .header p {
            font-size: 1em;
            opacity: 0.8;
            color: #94a3b8;
            letter-spacing: 1px;
            word-wrap: break-word;
            overflow-wrap: break-word;
        }

        .controls {
            background: linear-gradient(135deg, rgba(15, 23, 42, 0.95) 0%, rgba(30, 41, 59, 0.95) 100%);
            border-radius: 12px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 4px 24px rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(71, 85, 105, 0.3);
            animation: slideUp 0.8s ease-out;
        }

        .controls h2 {
            color: #60a5fa;
            margin-bottom: 25px;
            font-size: 1.5em;
            font-weight: 600;
            letter-spacing: 0.5px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            font-weight: 600;
            color: #cbd5e1;
            margin-bottom: 8px;
            font-size: 0.9em;
        }

        select, input, button {
            width: 100%;
            padding: 12px 16px;
            border: 1px solid rgba(71, 85, 105, 0.5);
            border-radius: 8px;
            font-size: 0.95em;
            transition: all 0.3s ease;
            background: rgba(15, 23, 42, 0.7);
            color: #e0e6ed;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            word-wrap: break-word;
            overflow-wrap: break-word;
        }

        select {
            white-space: normal;
            height: auto;
            min-height: 44px;
        }

        select option {
            white-space: normal;
            word-wrap: break-word;
            overflow-wrap: break-word;
            padding: 8px;
        }

        select:focus, input:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
            background: rgba(15, 23, 42, 0.9);
        }

        button {
            background: linear-gradient(135deg, #dc2626 0%, #b91c1c 100%);
            color: #fff;
            border: 1px solid #dc2626;
            cursor: pointer;
            font-weight: 600;
            margin-top: 10px;
            font-size: 0.95em;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 16px rgba(220, 38, 38, 0.4);
            background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
        }

        button:active {
            transform: translateY(0);
        }

        .status {
            background: linear-gradient(135deg, rgba(15, 23, 42, 0.95) 0%, rgba(30, 41, 59, 0.95) 100%);
            border-radius: 12px;
            padding: 20px 30px;
            margin-bottom: 30px;
            box-shadow: 0 4px 24px rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(71, 85, 105, 0.3);
        }

        .status-indicator {
            display: flex;
            align-items: center;
            gap: 15px;
            font-size: 0.95em;
            color: #cbd5e1;
        }

        .status-indicator span {
            flex: 1;
            word-wrap: break-word;
            overflow-wrap: break-word;
        }

        .status-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            animation: pulse 2s infinite;
            flex-shrink: 0;
        }

        .status-dot.connected {
            background: #10b981;
            box-shadow: 0 0 12px rgba(16, 185, 129, 0.5);
        }

        .status-dot.disconnected {
            background: #ef4444;
            box-shadow: 0 0 12px rgba(239, 68, 68, 0.5);
        }

        .data-container {
            background: linear-gradient(135deg, rgba(15, 23, 42, 0.95) 0%, rgba(30, 41, 59, 0.95) 100%);
            border-radius: 12px;
            padding: 30px;
            box-shadow: 0 4px 24px rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(71, 85, 105, 0.3);
            animation: slideUp 1s ease-out;
        }

        .data-container h2 {
            color: #60a5fa;
            margin-bottom: 25px;
            font-size: 1.5em;
            font-weight: 600;
            letter-spacing: 0.5px;
        }

        .table-wrapper {
            overflow-x: auto;
            border-radius: 8px;
            border: 1px solid rgba(71, 85, 105, 0.3);
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: rgba(15, 23, 42, 0.6);
            table-layout: auto;
        }

        th {
            background: linear-gradient(135deg, #1e40af 0%, #1e3a8a 100%);
            color: #e0e6ed;
            padding: 14px 16px;
            text-align: left;
            font-weight: 600;
            font-size: 0.9em;
            border-bottom: 2px solid rgba(59, 130, 246, 0.3);
            word-wrap: break-word;
            overflow-wrap: break-word;
            white-space: normal;
            max-width: 300px;
        }

        td {
            padding: 12px 16px;
            border-bottom: 1px solid rgba(71, 85, 105, 0.2);
            color: #cbd5e1;
            font-size: 0.9em;
            word-wrap: break-word;
            overflow-wrap: break-word;
            white-space: normal;
            max-width: 300px;
        }

        tr:hover {
            background: rgba(59, 130, 246, 0.05);
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: linear-gradient(135deg, rgba(15, 23, 42, 0.9) 0%, rgba(30, 41, 59, 0.9) 100%);
            padding: 24px;
            border-radius: 12px;
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
            text-align: center;
            transition: transform 0.3s ease;
            border: 1px solid rgba(71, 85, 105, 0.3);
            overflow: hidden;
        }

        .stat-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 24px rgba(59, 130, 246, 0.2);
            border-color: rgba(59, 130, 246, 0.5);
        }

        .stat-number {
            font-size: 1.8em;
            font-weight: 700;
            color: #60a5fa;
            margin: 10px 0;
            word-wrap: break-word;
            overflow-wrap: break-word;
            hyphens: auto;
            line-height: 1.2;
        }

        .stat-label {
            color: #94a3b8;
            font-size: 0.85em;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: 600;
        }

        .loading {
            text-align: center;
            padding: 40px;
            color: #94a3b8;
            font-size: 1em;
        }

        .error {
            background: rgba(220, 38, 38, 0.1);
            color: #fca5a5;
            padding: 16px 20px;
            border-radius: 8px;
            margin-bottom: 20px;
            border-left: 4px solid #dc2626;
            font-size: 0.9em;
            word-wrap: break-word;
            overflow-wrap: break-word;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Sprint3 - MySQL</h1>
            <p>(Hamza.Tayibi-Eduard.Pérez-Guim.Ballvé-Francesc.Martínez)</p>
        </div>

        <div class="status">
            <div class="status-indicator">
                <div class="status-dot disconnected" id="statusDot"></div>
                <span id="statusText">Connecting to server...</span>
            </div>
        </div>

        <div class="controls">
            <h2>Control Panel</h2>
            <div class="form-group">
                <label for="dbSelect">Select Database:</label>
                <select id="dbSelect">
                    <option value="">-- First connect to server --</option>
                </select>
            </div>
            <div class="form-group">
                <label for="tableSelect">Select Table:</label>
                <select id="tableSelect">
                    <option value="">-- First select a database --</option>
                </select>
            </div>
            <div class="form-group">
                <label for="customQuery">Custom SQL Query (optional):</label>
                <input type="text" id="customQuery" placeholder="SELECT * FROM table WHERE...">
                <button onclick="executeCustomQuery()">Execute Query</button>
            </div>
        </div>

        <div class="stats-grid" id="statsGrid" style="display:none;"></div>

        <div class="data-container">
            <h2>Table Data</h2>
            <div id="errorMessage"></div>
            <div id="dataDisplay" class="loading">
                Please connect to server and select a database
            </div>
        </div>
    </div>

    <script>
        const API_URL = 'api.php';
        let currentDB = '';
        let currentTable = '';

        async function connectToServer() {
            document.getElementById('statusText').textContent = 'Connecting...';
           
            try {
                const response = await fetch(`${API_URL}?action=connect`);
                const data = await response.json();
               
                if (data.success) {
                    document.getElementById('statusDot').className = 'status-dot connected';
                    document.getElementById('statusText').textContent = 'Connected to 192.168.60.15';
                    loadDatabases();
                } else {
                    throw new Error(data.error || 'Connection error');
                }
            } catch (error) {
                document.getElementById('statusDot').className = 'status-dot disconnected';
                document.getElementById('statusText').textContent = 'Error: ' + error.message;
                document.getElementById('errorMessage').innerHTML = `<div class="error">Could not connect to server. Verify that api.php is configured correctly.</div>`;
            }
        }

        async function loadDatabases() {
            try {
                const response = await fetch(`${API_URL}?action=getDatabases`);
                const databases = await response.json();
               
                if (databases.error) {
                    throw new Error(databases.error);
                }
               
                const select = document.getElementById('dbSelect');
                select.innerHTML = '<option value="">-- Select a database --</option>';
               
                databases.forEach(db => {
                    const option = document.createElement('option');
                    option.value = db;
                    option.textContent = db;
                    select.appendChild(option);
                });
            } catch (error) {
                document.getElementById('errorMessage').innerHTML = `<div class="error">Error loading databases: ${error.message}</div>`;
            }
        }

        document.getElementById('dbSelect').addEventListener('change', function() {
            currentDB = this.value;
            if (currentDB) {
                loadTables();
            }
        });

        async function loadTables() {
            try {
                const response = await fetch(`${API_URL}?action=getTables&db=${encodeURIComponent(currentDB)}`);
                const tables = await response.json();
               
                if (tables.error) {
                    throw new Error(tables.error);
                }
               
                const select = document.getElementById('tableSelect');
                select.innerHTML = '<option value="">-- Select a table --</option>';
               
                tables.forEach(table => {
                    const option = document.createElement('option');
                    option.value = table;
                    option.textContent = table;
                    select.appendChild(option);
                });
               
                if (tables.length === 0) {
                    document.getElementById('errorMessage').innerHTML = `<div class="error">No tables found in database "${currentDB}"</div>`;
                }
            } catch (error) {
                document.getElementById('errorMessage').innerHTML = `<div class="error">Error loading tables: ${error.message}</div>`;
            }
        }

        document.getElementById('tableSelect').addEventListener('change', function() {
            currentTable = this.value;
            if (currentTable) {
                loadTableData();
            }
        });

        async function loadTableData() {
            document.getElementById('dataDisplay').innerHTML = '<div class="loading">Loading data...</div>';
            document.getElementById('errorMessage').innerHTML = '';
           
            try {
                const response = await fetch(`${API_URL}?action=getData&db=${encodeURIComponent(currentDB)}&table=${encodeURIComponent(currentTable)}`);
                const result = await response.json();
               
                if (result.error) {
                    throw new Error(result.error);
                }
               
                if (result.length === 0) {
                    document.getElementById('dataDisplay').innerHTML = '<p>No data in this table</p>';
                    showStats(0);
                } else {
                    displayData(result);
                    showStats(result.length);
                }
            } catch (error) {
                document.getElementById('errorMessage').innerHTML = `<div class="error">Error loading data: ${error.message}</div>`;
                document.getElementById('dataDisplay').innerHTML = '';
            }
        }

        function displayData(data) {
            if (data.length === 0) {
                document.getElementById('dataDisplay').innerHTML = '<p>No data in this table</p>';
                return;
            }

            const keys = Object.keys(data[0]);
           
            let html = '<div class="table-wrapper"><table>';
            html += '<thead><tr>';
            keys.forEach(key => {
                html += `<th>${key}</th>`;
            });
            html += '</tr></thead><tbody>';
           
            data.forEach(row => {
                html += '<tr>';
                keys.forEach(key => {
                    html += `<td>${row[key]}</td>`;
                });
                html += '</tr>';
            });
           
            html += '</tbody></table></div>';
            document.getElementById('dataDisplay').innerHTML = html;
        }

        function showStats(count) {
            const statsGrid = document.getElementById('statsGrid');
            statsGrid.style.display = 'grid';
            statsGrid.innerHTML = `
                <div class="stat-card">
                    <div class="stat-label">Database</div>
                    <div class="stat-number">${currentDB}</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Table</div>
                    <div class="stat-number">${currentTable}</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Total Records</div>
                    <div class="stat-number">${count}</div>
                </div>
                <div class="stat-card">
                    <div class="stat-label">Status</div>
                    <div class="stat-number">✓</div>
                </div>
            `;
        }

        function executeCustomQuery() {
            const query = document.getElementById('customQuery').value.trim();
            if (!query) {
                alert('Please enter a SQL query');
                return;
            }
           
            if (!currentDB) {
                alert('Please first select a database');
                return;
            }
           
            document.getElementById('dataDisplay').innerHTML = '<div class="loading">Executing query...</div>';
            document.getElementById('errorMessage').innerHTML = '';
           
            fetch(`${API_URL}?action=customQuery&db=${encodeURIComponent(currentDB)}&query=${encodeURIComponent(query)}`)
                .then(response => response.json())
                .then(result => {
                    if (result.error) {
                        throw new Error(result.error);
                    }
                   
                    if (result.length === 0) {
                        document.getElementById('dataDisplay').innerHTML = '<p>Query executed successfully. No results to display.</p>';
                    } else {
                        displayData(result);
                        document.getElementById('statsGrid').innerHTML = `
                            <div class="stat-card">
                                <div class="stat-label">Custom Query</div>
                                <div class="stat-number">✓</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Database</div>
                                <div class="stat-number">${currentDB}</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-label">Results</div>
                                <div class="stat-number">${result.length}</div>
                            </div>
                        `;
                        document.getElementById('statsGrid').style.display = 'grid';
                    }
                })
                .catch(error => {
                    document.getElementById('errorMessage').innerHTML = `<div class="error">Query error: ${error.message}</div>`;
                    document.getElementById('dataDisplay').innerHTML = '';
                });
        }

        // Start connection on page load
        window.onload = connectToServer;
    </script>
</body>
</html>
```
```bash
# Guardar i sortir de nano: Ctrl+O, Enter, Ctrl+X
```

---

### Pas 3: Creació del fitxer api.php

Creació del fitxer `/var/www/html/api.php` que actua com a backend REST API. Gestiona la connexió amb MySQL (192.168.60.15), implementa endpoints per diferents accions, i inclou mesures de seguretat com validació de consultes.

![Creació api.php](./Photos/sprint%203/web3.png)
```bash
# Crear/editar el fitxer api.php
sudo nano /var/www/html/api.php
```

**Contingut complet del fitxer api.php:**
```php
<?php
header('Content-Type: application/json');
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: GET, POST, OPTIONS');
header('Access-Control-Allow-Headers: Content-Type');

// Responder a peticiones OPTIONS (CORS preflight)
if ($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
    http_response_code(200);
    exit();
}

// Configuración de la base de datos
$host = '192.168.60.15';
$user = 'root';
$pass = '1234';
$action = $_GET['action'] ?? '';

try {
    $conn = new mysqli($host, $user, $pass);
    
    if ($conn->connect_error) {
        throw new Exception("Error de conexión: " . $conn->connect_error);
    }
    
    $conn->set_charset("utf8mb4");
    
    switch($action) {
        case 'connect':
            echo json_encode(['success' => true, 'message' => 'Conectado exitosamente']);
            break;
            
        case 'getDatabases':
            $result = $conn->query("SHOW DATABASES");
            $databases = [];
            while($row = $result->fetch_array()) {
                $databases[] = $row[0];
            }
            echo json_encode($databases);
            break;
            
        case 'getTables':
            $db = $_GET['db'] ?? '';
            if (empty($db)) {
                throw new Exception("No se especificó la base de datos");
            }
            $conn->select_db($db);
            $result = $conn->query("SHOW TABLES");
            $tables = [];
            while($row = $result->fetch_array()) {
                $tables[] = $row[0];
            }
            echo json_encode($tables);
            break;
            
        case 'getData':
            $db = $_GET['db'] ?? '';
            $table = $_GET['table'] ?? '';
            if (empty($db) || empty($table)) {
                throw new Exception("Faltan parámetros: base de datos o tabla");
            }
            $conn->select_db($db);
            $table = $conn->real_escape_string($table);
            $result = $conn->query("SELECT * FROM `$table` LIMIT 1000");
            if (!$result) {
                throw new Exception("Error en la consulta: " . $conn->error);
            }
            $data = [];
            while($row = $result->fetch_assoc()) {
                $data[] = $row;
            }
            echo json_encode($data);
            break;
            
        case 'customQuery':
            $db = $_GET['db'] ?? '';
            $query = $_GET['query'] ?? '';
            if (empty($db) || empty($query)) {
                throw new Exception("Faltan parámetros: base de datos o consulta");
            }
            $conn->select_db($db);
            $query_upper = strtoupper(trim($query));
            if (strpos($query_upper, 'SELECT') !== 0) {
                throw new Exception("Solo se permiten consultas SELECT por seguridad");
            }
            $result = $conn->query($query);
            if (!$result) {
                throw new Exception("Error en la consulta: " . $conn->error);
            }
            $data = [];
            if ($result instanceof mysqli_result) {
                while($row = $result->fetch_assoc()) {
                    $data[] = $row;
                }
            }
            echo json_encode($data);
            break;
            
        default:
            throw new Exception("Acción no válida");
    }
    
    $conn->close();
} catch(Exception $e) {
    http_response_code(500);
    echo json_encode(['error' => $e->getMessage()]);
}
?>
```
```bash
# Guardar i sortir de nano: Ctrl+O, Enter, Ctrl+X
```

---

### Pas 4: Configuració de permisos dels fitxers

Configuració dels permisos adequats per als fitxers de l'aplicació web. S'assigna la propietat de `api.php` a l'usuari `www-data` (utilitzat per Apache) i es configuren els permisos correctes.

![Configuració permisos](./Photos/sprint%203/web4.png)
```bash
# Assignar propietari a index.html
sudo chown root:root /var/www/html/index.html
sudo chmod 644 /var/www/html/index.html

# Assignar propietari a api.php
sudo chown www-data:www-data /var/www/html/api.php
sudo chmod 644 /var/www/html/api.php

# Verificar permisos finals
ls -l /var/www/html/index.html
ls -l /var/www/html/api.php
```

---

### Pas 5: Reinici del servei Apache2

Reinici del servei Apache2 per aplicar tots els canvis realitzats i verificació que el servei està actiu correctament.

![Reinici Apache2](./Photos/sprint%203/web5.png)
```bash
# Reiniciar Apache2
sudo systemctl restart apache2

# Verificar estat del servei
sudo systemctl status apache2
```

---

## Característiques de l'Aplicació Web

### Funcionalitats Implementades

#### Frontend (index.html)
- **Interfície moderna** amb gradient dark theme (blau i gris)
- **Animacions CSS** (fadeIn, slideUp, pulse)
- **Disseny responsiu** amb grid layout adaptatiu
- **Indicador d'estat** de connexió amb MySQL en temps real
- **Selecció dinàmica** de bases de dades i taules
- **Visualització de dades** en taules HTML amb scroll horitzontal
- **Consultes SQL personalitzades** amb validació
- **Estadístiques en temps real** (database, table, total records)

#### Backend (api.php)
- **Connexió segura** amb MySQL (192.168.60.15:3306)
- **5 Endpoints REST**: connect, getDatabases, getTables, getData, customQuery
- **Validació de paràmetres** i gestió d'errors
- **Protecció SQL injection** amb `real_escape_string()`
- **Restricció de seguretat**: només permet consultes SELECT
- **Límit de 1000 registres** per query per evitar sobrecàrrega
- **Headers CORS** configurats per peticions cross-origin
- **Resposta JSON** per tots els endpoints

---

## Accés a l'Aplicació Web
```bash
# Des del navegador accedir a:
http://192.168.6.10
```

**Flux de funcionament:**
1. L'aplicació es connecta automàticament a MySQL (192.168.60.15)
2. Es carrega la llista de bases de dades disponibles
3. L'usuari selecciona una base de dades (ex: EquipamentsBCN)
4. Es carreguen les taules de la base de dades seleccionada
5. L'usuari selecciona una taula per visualitzar les dades
6. Opcionalment, l'usuari pot executar consultes SQL personalitzades

---

## Proves de Funcionament

### Prova 1: Connexió al servidor
```
Resultat: ✓ Connected to 192.168.60.15
```

### Prova 2: Llistat de bases de dades
```
Resultat: EquipamentsBCN, information_schema, mysql, performance_schema, sys
```

### Prova 3: Consulta personalitzada
```sql
SELECT nom, latitude, longitude FROM equipaments LIMIT 10
```

### Prova 4: Validació de seguretat
```sql
DELETE FROM equipaments WHERE register_id = '1'
```
```
Resultat: Error "Solo se permiten consultas SELECT por seguridad"
```

---