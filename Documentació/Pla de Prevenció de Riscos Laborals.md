# Identificació de Riscos del Projecte

## Riscos Tècnics

### 1. Fallada en la configuració de xarxa
**Descripció:** Error en la configuració d'IPs, màscares o routing que impedeixi la comunicació entre xarxes  
**Prevenció:**  
- Documentar totes les configuracions abans d'aplicar-les  
- Realitzar proves de connectivitat després de cada canvi  
- Mantenir backup de les configuracions funcionals  

### 2. Vulnerabilitats de seguretat
**Descripció:** Exposició de serveis crítics (DB, SSH, FTP) sense mesures de seguretat adequades  
**Prevenció:**  
- Configurar firewalls (iptables) correctament  
- Utilitzar contrasenyes fortes  
- Mantenir serveis actualitzats  
- Implementar separació de xarxes (DMZ/Intranet)  

### 3. Pèrdua de dades
**Descripció:** Pèrdua d'informació de configuració o base de dades  
**Prevenció:**  
- Realitzar backups periòdics  
- Utilitzar control de versions (Git) per configuracions  
- Documentar tots els canvis realitzats  

---

## Riscos d'Organització

### 4. Retards en el calendari
**Descripció:** No complir amb els terminis establerts per cada sprint  
**Prevenció:**  
- Planificació realista de tasques en ProofHub  
- Reunions de seguiment setmanals  
- Identificació primerenca de blocatges  

### 5. Manca de coordinació entre membres
**Descripció:** Duplicació de tasques o configuracions incompatibles  
**Prevenció:**  
- Assignació clara de responsabilitats  
- Comunicació constant via ProofHub  
- Documentació compartida i actualitzada  

---

## Riscos de Salut i Seguretat (PRL)

### 6. Fatiga visual i postural
**Descripció:** Problemes derivats de llargues sessions davant l'ordinador  
**Prevenció:**  
- Pauses cada 50 minuts de treball  
- Ergonomia adequada del lloc de treball  
- Il·luminació correcta  

### 7. Estrés per càrrega de treball
**Descripció:** Sobrecàrrega durant els sprints  
**Prevenció:**  
- Distribució equilibrada de tasques  
- Flexibilitat en terminis si cal  
- Suport mutu entre membres de l'equip  

### 8. Riscos elèctrics
**Descripció:** Manipulació incorrecta d'equips i instal·lacions  
**Prevenció:**  
- Seguir normes de seguretat elèctrica  
- Equips amb certificació CE  
- No manipular equips amb corrient  

---

## Mitjans i Mesures de Prevenció

### Equips de Protecció Individual (EPI)
No aplica en aquest projecte (entorn de laboratori informàtic)  

### Mitjans Tècnics
- Sistemes d'alimentació ininterrompuda (SAI) per als servidors  
- Extintors al laboratori  
- Sistema de ventilació adequat  

### Formació i Procediments
- Formació en seguretat informàtica: Bones pràctiques en configuració de servidors  
- Procediments documentats: Guies pas a pas per cada servei  
- Check-lists de verificació: Abans de posar serveis en producció  

### Mesures Organitzatives
- Planificació en sprints: Evita sobrecàrrega puntual  
- Revisió per parells: Totes les configuracions crítiques revisades per un company  
- Documentació contínua: Cada canvi queda registrat  

---

## Pla d'Acció en Cas d'Incidència

### Incidència Tècnica
- Documentar l'error i els símptomes  
- Restaurar última configuració funcional (backup)  
- Analitzar la causa arrel  
- Aplicar solució i verificar  
- Actualitzar documentació  

### Incidència de Seguretat
- Aïllar el sistema compromès  
- Avaluar l'abast de la incidència  
- Aplicar pegats/actualitzacions  
- Revisar logs i configuracions  
- Implementar mesures correctives addicionals  
