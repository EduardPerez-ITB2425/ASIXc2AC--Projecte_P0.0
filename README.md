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

### Esquema d'IPs - Xarxa 192.168.6.X

#### DMZ (192.168.6.10/24)
- **W-NCC (Web Server):** 192.168.6.10
- **F-NCC (FTP Server):** 192.168.6.11

#### Intranet (192.168.60.20/24)
- **B-NCC (Database Server):** 192.168.60.20
- **DHCP Server:** 192.168.60.21
- **DNS Server:** 192.168.60.22
- **Pool DHCP:** 192.168.60.30-100

#### Router
- **Hostname:** R-N01
- **Interfícies:** DMZ, Intranet, NAT

## Hardware Desplegat

### Servidors
- **W-NCC:** Web Server + SSH
- **B-NCC:** Base de Dades MySQL
- **F-NCC:** Servidor FTP
- **Servidors de Xarxa:** DHCP + DNS

### Clients
- PC Windows
- PC Linux

![Descripció de la imatge](./Photos/estructura.png)

---
