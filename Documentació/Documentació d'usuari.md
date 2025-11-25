# Manual d'Usuari - Sistema de Consulta d'Equipaments de Barcelona

## Informació General

**Versió del Sistema:** 1.0  
**Data d'Actualització:** Novembre 2025  
**Equip de Desenvolupament:** Grup 6 - ASIX C2  
**Membres:** Hamza Tayibi, Eduard Pérez, Guim Ballvé, Francesc Martínez

---

## Introducció

Aquest manual explica com utilitzar el sistema de consulta d'equipaments públics de Barcelona. El sistema permet accedir a informació detallada sobre centres educatius, culturals, esportius i altres equipaments públics de la ciutat mitjançant una aplicació web.

---

## Requisits Mínims

### Equipament Necessari
- Ordinador amb connexió a Internet
- Navegador web actualitzat (Chrome, Firefox, Safari o Edge)
- Connexió de xarxa estable
- Resolució mínima de pantalla: 1280x720

---

## Accés al Sistema

### Des de la Xarxa Interna

Si es troba connectat a la xarxa interna de l'organització:

1. Obri el seu navegador web
2. A la barra d'adreces, escrigui: **http://192.168.6.10**
3. Premi Enter
4. La pàgina principal de l'aplicació es carregarà automàticament

### Des de Casa o Ubicació Externa

Per accedir des de fora de l'oficina, contacti amb el departament de sistemes per obtenir:
- Credencials d'accés VPN
- Configuració de xarxa necessària
- Permisos d'accés remot

---

## Interfície de l'Aplicació Web

### Descripció de la Pàgina Principal

Quan accedeixi al sistema, veurà una interfície amb tema fosc que inclou:

#### 1. Capçalera
- Títol: "Sprint3 - MySQL"
- Informació de l'equip de desenvolupament
- Disseny amb gradient blau

#### 2. Indicador d'Estat de Connexió
- Punt verd parpellejant: Sistema connectat correctament
- Punt vermell: Error de connexió
- Missatge descriptiu: "Connected to 192.168.60.15" o estat d'error

#### 3. Panell de Control

**a) Selector de Base de Dades**
- Desplegable amb totes les bases de dades disponibles
- Opció per defecte: "-- Select a database --"
- Seleccioni "EquipamentsBCN" per accedir als equipaments de Barcelona

**b) Selector de Taula**
- Disponible després de seleccionar una base de dades
- Mostra totes les taules disponibles:
  - `equipaments` - Informació bàsica dels equipaments
  - `direccions` - Adreces i ubicacions
  - `valors` - Valors i atributs dels equipaments
  - `filtres_secundaris` - Categories i filtres

**c) Consulta SQL Personalitzada**
- Camp de text per escriure consultes SQL
- Botó "Execute Query" per executar la consulta
- Només es permeten consultes SELECT per seguretat

#### 4. Panell d'Estadístiques

Mostra quatre targetes amb informació en temps real:
- **Database:** Nom de la base de dades activa
- **Table:** Nom de la taula seleccionada
- **Total Records:** Nombre total de registres carregats
- **Status:** Estat de l'última operació

#### 5. Àrea de Resultats

- Taula amb tots els registres de la consulta
- Scroll horitzontal per columnes extenses
- Disseny responsiu amb efectes hover
- Màxim 1000 registres per consulta

---

## Utilitzar l'Aplicació Web

### Pas 1: Connexió Inicial

1. Accedeixi a **http://192.168.6.10**
2. L'aplicació es connectarà automàticament al servidor MySQL
3. Espereu fins que l'indicador d'estat mostri: **"Connected to 192.168.60.15"**
4. El punt indicador canviarà de vermell a verd

### Pas 2: Seleccionar Base de Dades

1. Localitzeu el selector **"Select Database"** al Panell de Control
2. Feu clic sobre el desplegable
3. Seleccioneu **"EquipamentsBCN"** de la llista
4. El sistema carregarà automàticament les taules disponibles

### Pas 3: Seleccionar Taula

1. Després de seleccionar la base de dades, el selector **"Select Table"** s'activarà
2. Feu clic sobre el desplegable de taules
3. Seleccioneu una de les taules disponibles
4. Els resultats apareixeran automàticament a la secció "Table Data"

### Pas 4: Visualitzar Resultats

**Format de la Taula:**
- Capçaleres en blau amb el nom de cada columna
- Files amb fons fosc i efecte hover
- Text en blanc/gris clar per a fàcil lectura
- Scroll horitzontal si hi ha moltes columnes

**Estadístiques Visibles:**
- Veureu 4 targetes a la part superior amb informació resumida

---

## Consultes Bàsiques per Equipaments

### Consulta 1: Veure Tots els Equipaments

**Passos:**
1. Seleccioneu la base de dades: **EquipamentsBCN**
2. Seleccioneu la taula: **equipaments**
3. La taula mostrarà tots els registres disponibles

**Dades Visibles:**
- `register_id` - Identificador únic
- `nom` - Nom de l'equipament
- `institution_name` - Institució responsable
- `latitude` / `longitude` - Coordenades GPS
- `start_date` / `end_date` - Dates de funcionament
- `timetable` - Horaris

### Consulta 2: Veure Adreces d'Equipaments

**Passos:**
1. Base de dades: **EquipamentsBCN**
2. Taula: **direccions**

**Dades Visibles:**
- `equipament_id` - Referència a l'equipament
- `road_name` - Nom del carrer
- `start_street_number` - Número inicial
- `neighborhood_name` - Nom del barri
- `district_name` - Nom del districte
- `zip_code` - Codi postal

### Consulta 3: Veure Atributs dels Equipaments

**Passos:**
1. Base de dades: **EquipamentsBCN**
2. Taula: **valors**

**Dades Visibles:**
- `equipament_id` - Referència a l'equipament
- `category` - Categoria de l'atribut
- `attribute_name` - Nom de l'atribut
- `value` - Valor de l'atribut
- `description` - Descripció detallada

---

## Consultes SQL Personalitzades

### Com Executar una Consulta SQL

1. Localitzeu el camp **"Custom SQL Query (optional)"** al Panell de Control
2. Escriviu la vostra consulta SQL al camp de text
3. Feu clic al botó **"Execute Query"**
4. Els resultats apareixeran a la secció "Table Data"

### Exemples de Consultes SQL

#### Exemple 1: Equipaments per Nom
```sql
SELECT nom, institution_name, latitude, longitude 
FROM equipaments 
WHERE nom LIKE '%biblioteca%'
```

Mostra tots els equipaments que continguin "biblioteca" al nom.

#### Exemple 2: Equipaments per Districte
```sql
SELECT e.nom, d.district_name, d.neighborhood_name, d.road_name
FROM equipaments e
JOIN direccions d ON e.register_id = d.equipament_id
WHERE d.district_name = 'Eixample'
```

Mostra equipaments del districte de l'Eixample amb la seva adreça.

#### Exemple 3: Equipaments amb Coordenades
```sql
SELECT nom, latitude, longitude, timetable
FROM equipaments
WHERE latitude IS NOT NULL AND longitude IS NOT NULL
LIMIT 50
```

Mostra els primers 50 equipaments amb coordenades GPS i horaris.

#### Exemple 4: Recompte per Barri
```sql
SELECT d.neighborhood_name, COUNT(*) as total_equipaments
FROM direccions d
GROUP BY d.neighborhood_name
ORDER BY total_equipaments DESC
LIMIT 10
```

Mostra els 10 barris amb més equipaments.

#### Exemple 5: Equipaments amb Filtres Específics
```sql
SELECT e.nom, f.filter_name, f.filter_tree
FROM equipaments e
JOIN filtres_secundaris f ON e.register_id = f.equipament_id
WHERE f.filter_name LIKE '%educació%'
```

Mostra equipaments relacionats amb educació.

### Restriccions de Seguretat

**Important:**
- Només es permeten consultes **SELECT**
- Les consultes INSERT, UPDATE, DELETE, DROP estan bloquejades
- Missatge d'error: "Solo se permiten consultas SELECT por seguridad"
- Límit màxim de resultats: **1000 registres**

### Consells per a Consultes Eficients

**Bones Pràctiques:**
- Utilitzeu `LIMIT` per limitar el nombre de resultats
- Especifiqueu només les columnes necessàries
- Utilitzeu `WHERE` per filtrar resultats
- Utilitzeu `JOIN` per combinar taules relacionades

**Eviteu:**
- Consultes sense `LIMIT` que retornin milers de registres
- Utilitzar `SELECT *` si no necessiteu totes les columnes
- Consultes amb múltiples `JOIN` complexos

---

## Característiques de la Interfície

### Disseny Visual

**Colors i Tema:**
- Fons amb gradient fosc (blau nit)
- Accent principal: Blau
- Text: Blanc/gris clar per contrast òptim
- Targetes amb efectes de vidre esmerilat

**Animacions:**
- Efecte "fade in" a l'entrada de la pàgina
- Efecte "slide up" als panells de control i dades
- Pulsació suau de l'indicador d'estat
- Efecte hover a les files de la taula
- Transicions suaus a botons i selectors

### Compatibilitat

**Navegadors Compatible:**
- Google Chrome (versió 90+)
- Mozilla Firefox (versió 88+)
- Microsoft Edge (versió 90+)
- Safari (versió 14+)

**Dispositius:**
- Ordinadors de sobretaula
- Portàtils
- Tauletes (experiència limitada)
- Telèfons mòbils (no optimitzat)

---

## Consells d'Ús

### Optimitzar el Rendiment

1. **Utilització de Filtres:**
   - Utilitzeu `WHERE` a les consultes SQL per reduir resultats
   - Seleccioneu només les columnes necessàries
   - Apliqueu `LIMIT` per consultes exploratòries

2. **Navegació Eficient:**
   - Mantingueu obertes només les pestanyes necessàries
   - Refresqueu la pàgina (F5) si detecteu lentitud
   - Utilitzeu les dreceres de teclat per agilitzar tasques

3. **Interpretació de Resultats:**
   - Reviseu les estadístiques abans de la taula de dades
   - Utilitzeu el scroll horitzontal per columnes ocultes
   - Observeu els canvis de color hover per identificar files

### Bones Pràctiques

**Recomanacions:**
- Començeu amb consultes simples i augmenteu la complexitat gradualment
- Guardeu les consultes SQL útils en un document extern
- Utilitzeu noms de columna específics en lloc de `SELECT *`
- Comproveu l'indicador d'estat abans de fer consultes

**Eviteu:**
- Executar consultes molt complexes sense testejar-les primer
- Deixar la sessió oberta sense supervisió
- Intentar executar consultes que modifiquin dades
- Obrir múltiples pestanyes amb la mateixa aplicació

---

## Resolució de Problemes

### Problema 1: No puc accedir a la pàgina

**Símptomes:**
- El navegador no carrega la pàgina
- Missatge "No es pot arribar al lloc"
- Temps d'espera esgotat

**Solucions:**
1. Verifiqueu que està connectat a la xarxa interna
2. Comprovi que ha escrit correctament l'adreça: `http://192.168.6.10`
3. Provi amb un navegador diferent
4. Netegi la memòria cau del navegador (Ctrl + Shift + Delete)
5. Contacti amb el servei d'assistència tècnica

### Problema 2: Indicador d'estat en vermell

**Símptomes:**
- El punt indicador està en vermell
- Missatge: "Error: Connection error"
- No es carreguen les bases de dades

**Solucions:**
1. Refresqueu la pàgina (F5)
2. Espereu 30 segons i torneu a intentar-ho
3. Comproveu que el servidor MySQL està actiu
4. Verifiqueu la connectivitat de xarxa
5. Si persisteix, reporteu l'incidència

### Problema 3: La taula no mostra resultats

**Símptomes:**
- Missatge: "No data in this table"
- La taula està buida després de seleccionar-la
- Les estadístiques mostren 0 registres

**Solucions:**
1. Verifiqueu que ha seleccionat la base de dades correcta
2. Proveu amb una taula diferent
3. Executeu una consulta SQL simple: `SELECT * FROM equipaments LIMIT 10`
4. Comproveu que la taula conté dades al servidor

### Problema 4: Error en consulta SQL personalitzada

**Símptomes:**
- Missatge d'error vermell: "Query error: ..."
- La consulta no s'executa
- Resultats inesperats

**Causes i Solucions:**

**Error de Sintaxi:**
```
Missatge: "You have an error in your SQL syntax"
Solució: Reviseu la sintaxi SQL, comproveu parèntesis, comes i cometes
```

**Consulta No Permesa:**
```
Missatge: "Solo se permiten consultas SELECT por seguridad"
Solució: Utilitzeu només consultes SELECT, no INSERT/UPDATE/DELETE
```

**Taula o Columna No Existent:**
```
Missatge: "Unknown column 'xxx' in 'field list'"
Solució: Verifiqueu els noms de columnes i taules correctes
```

**Temps d'Espera Esgotat:**
```
Missatge: "Query timeout"
Solució: Simplifiqueu la consulta, afegiu LIMIT
```

### Problema 5: La pàgina carrega lentament

**Símptomes:**
- Temps de càrrega superior a 10 segons
- La taula triga a mostrar-se
- Navegador lent o bloquejat

**Solucions:**
1. Reduïu el nombre de resultats amb `LIMIT`
2. Tanqueu pestanyes i aplicacions que no utilitzi
3. Comproveu la velocitat de connexió a Internet
4. Eviteu consultes amb múltiples `JOIN` complexos
5. Reinicieu el navegador
6. Netegeu la memòria cau

### Problema 6: Missatges d'error genèrics

**Símptomes:**
- Missatge: "Error loading data"
- Missatge: "Error loading tables"
- La interfície no respon

**Solucions:**
1. Refresqueu la pàgina completament (Ctrl + F5)
2. Obriu les eines de desenvolupador (F12) i reviseu la consola
3. Proveu amb un navegador diferent
4. Desactiveu extensions del navegador que puguin interferir
5. Contacteu amb suport tècnic amb captures de pantalla

---

## Seguretat i Privadesa

### Mesures de Seguretat Implementades

1. **Validació de Consultes:**
   - Només es permeten consultes SELECT
   - Protecció contra SQL Injection
   - Escapament de caràcters especials

2. **Límits de Dades:**
   - Màxim 1000 registres per consulta
   - Timeout de connexió configurat
   - Control d'errors robust

3. **Accés Restringit:**
   - Només accessible des de la xarxa interna
   - Connexió directa sense autenticació externa
   - Logs de consultes al servidor

### Recomanacions de Seguretat

**Feu:**
- Tancau la pestanya quan acabeu de treballar
- No compartiu pantalles amb informació sensible
- Informeu d'errors o comportaments estranys
- Utilitzeu connexions segures de xarxa

**No Feu:**
- Intentar modificar o eliminar dades
- Compartir captures amb dades sensibles
- Executar consultes no verificades de tercers
- Deixar la sessió oberta sense supervisió

---

## Contacte i Assistència

### Servei d'Assistència Tècnica

**Horari d'Atenció:**
- Dilluns a Divendres: 9:00 - 18:00
- Cap de setmana i festius: Tancat

**Canals de Contacte:**
- Telèfon intern: Extensió 1234
- Correu electrònic: suport@grup6.itb.cat
- Portal d'incidències: http://suport.intern

### Temps de Resposta

| Tipus d'Incidència | Temps de Resposta |
|-------------------|-------------------|
| Crítica (sistema caigut) | 2 hores |
| Normal (errors de consulta) | 24 hores |
| Consulta general | 48 hores |

### Informació a Proporcionar

Quan contacteu amb assistència tècnica, incloeu:

1. **Informació Bàsica:**
   - Nom complet i departament
   - Data i hora de l'incident
   - Descripció detallada del problema

2. **Informació Tècnica:**
   - Navegador i versió
   - Sistema operatiu
   - IP de la màquina (si és possible)

3. **Evidències:**
   - Captura de pantalla de l'error
   - Consulta SQL que va fallar (si escau)
   - Missatges d'error complets
   - Passos per reproduir el problema

---

## Glossari de Termes

| Terme | Definició |
|-------|-----------|
| **API** | Interfície que permet la comunicació entre el frontend i el backend |
| **Backend** | Part del sistema que gestiona la lògica i les dades (api.php) |
| **Base de Dades** | Conjunt organitzat d'informació estructurada (MySQL) |
| **Consulta SQL** | Instrucció per obtenir dades d'una base de dades |
| **Dashboard** | Panell de control visual amb informació resumida |
| **Endpoint** | Punt d'accés específic de l'API |
| **Frontend** | Part visual de l'aplicació que veu l'usuari (index.html) |
| **Interfície** | Conjunt d'elements visuals per interactuar amb el sistema |
| **JOIN** | Operació SQL per combinar dades de múltiples taules |
| **LIMIT** | Restricció del nombre de resultats d'una consulta |
| **MySQL** | Sistema de gestió de bases de dades utilitzat |
| **Query** | Consulta o petició de dades |
| **SELECT** | Instrucció SQL per consultar dades |
| **Taula** | Estructura que emmagatzema dades en files i columnes |
| **WHERE** | Clàusula SQL per filtrar resultats |

---

## Casos d'Ús Pràctics

### Cas d'Ús 1: Cercar Biblioteques a Gràcia

**Objectiu:** Trobar totes les biblioteques del barri de Gràcia amb les seves adreces.

**Passos:**
1. Accediu a l'aplicació web
2. Seleccioneu base de dades: **EquipamentsBCN**
3. Al camp "Custom SQL Query", escriviu:
```sql
SELECT e.nom, d.road_name, d.start_street_number, d.neighborhood_name
FROM equipaments e
JOIN direccions d ON e.register_id = d.equipament_id
WHERE e.nom LIKE '%biblioteca%' 
  AND d.district_name = 'Gràcia'
```
4. Feu clic a "Execute Query"
5. Visualitzeu els resultats a la taula

### Cas d'Ús 2: Equipaments Esportius amb Horaris

**Objectiu:** Consultar tots els centres esportius amb els seus horaris d'obertura.

**Passos:**
1. Base de dades: **EquipamentsBCN**
2. Consulta SQL:
```sql
SELECT nom, institution_name, timetable, latitude, longitude
FROM equipaments
WHERE nom LIKE '%esportiu%' OR nom LIKE '%poliesportiu%'
  AND timetable IS NOT NULL
LIMIT 50
```
3. Execute Query
4. Analitzeu els horaris a la columna `timetable`

### Cas d'Ús 3: Estadístiques per Districte

**Objectiu:** Obtenir un resum del nombre d'equipaments per districte.

**Passos:**
1. Base de dades: **EquipamentsBCN**
2. Consulta SQL:
```sql
SELECT d.district_name, COUNT(*) as total_equipaments
FROM direccions d
GROUP BY d.district_name
ORDER BY total_equipaments DESC
```
3. Execute Query
4. Reviseu les estadístiques al panell superior

---

## Tutorial Pas a Pas per a Nous Usuaris

### Primera Vegada: Guia Completa

#### Pas 1: Accés Inicial (2 minuts)

1. Obriu el vostre navegador
2. Escriviu a la barra d'adreces: `http://192.168.6.10`
3. Premeu Enter i espereu la càrrega
4. Heu de veure la capçalera "Sprint3 - MySQL"

#### Pas 2: Comprensió de la Interfície (3 minuts)

1. **Observeu la part superior:**
   - Títol de l'aplicació amb gradient blau
   - Noms de l'equip de desenvolupament

2. **Indicador d'estat:**
   - Espereu que el punt canviï de vermell a verd
   - Llegiu el missatge: "Connected to 192.168.60.15"

3. **Panell de Control:**
   - Identifiqueu els 3 elements principals

#### Pas 3: Primera Consulta Simple (5 minuts)

1. **Seleccionar Base de Dades:**
   - Feu clic al primer desplegable
   - Seleccioneu: **EquipamentsBCN**
   - Espereu 1-2 segons

2. **Seleccionar Taula:**
   - El segon desplegable ara estarà actiu
   - Seleccioneu: **equipaments**
   - Els resultats es carregaran automàticament

3. **Analitzar Resultats:**
   - Mireu les 4 targetes superiors amb estadístiques
   - Desplaceu-vos cap avall per veure la taula de dades

#### Pas 4: Consulta SQL Personalitzada (10 minuts)

1. **Exemple Bàsic:**
   - Feu clic al camp "Custom SQL Query"
   - Escriviu:
```sql
SELECT nom, latitude, longitude FROM equipaments LIMIT 10
```
   - Premeu "Execute Query"

2. **Exemple amb Filtre:**
   - Netegeu el camp anterior
   - Escriviu:
```sql
SELECT nom, institution_name 
FROM equipaments 
WHERE nom LIKE '%cultura%'
```
   - Execute Query

3. **Exemple amb JOIN:**
   - Nova consulta:
```sql
SELECT e.nom, d.road_name, d.neighborhood_name
FROM equipaments e
JOIN direccions d ON e.register_id = d.equipament_id
LIMIT 20
```
   - Execute Query

#### Pas 5: Exportar/Guardar Informació (5 minuts)

1. **Copiar Resultats:**
   - Seleccioneu les files que us interessin
   - Ctrl + C per copiar
   - Enganxeu a Excel o un document

2. **Captura de Pantalla:**
   - Windows: Win + Shift + S
   - Mac: Cmd + Shift + 4
   - Guardeu la imatge amb els resultats

---

## Dreceres de Teclat

### Navegació General

| Drecera | Acció |
|---------|-------|
| `F5` | Refrescar pàgina |
| `Ctrl + F5` | Refrescar ignorant cau |
| `Ctrl + F` | Cercar text a la pàgina |
| `Ctrl + +` | Augmentar zoom |
| `Ctrl + -` | Reduir zoom |
| `Ctrl + 0` | Zoom 100% |
| `F12` | Obrir eines de desenvolupador |
| `Ctrl + Shift + Delete` | Netejar cau del navegador |

### Edició de Consultes SQL

| Drecera | Acció |
|---------|-------|
| `Ctrl + A` | Seleccionar tot el text |
| `Ctrl + C` | Copiar text seleccionat |
| `Ctrl + V` | Enganxar text |
| `Ctrl + X` | Tallar text |
| `Ctrl + Z` | Desfer acció |
| `Tab` | Indentar text SQL |

---

## Exemples Avançats de Consultes

### Consulta Complexa 1: Equipaments amb Múltiples Filtres
```sql
SELECT 
    e.nom,
    e.institution_name,
    d.district_name,
    d.neighborhood_name,
    d.road_name,
    d.zip_code,
    e.latitude,
    e.longitude
FROM equipaments e
INNER JOIN direccions d ON e.register_id = d.equipament_id
WHERE 
    d.district_name IN ('Eixample', 'Gràcia', 'Sarrià-Sant Gervasi')
    AND e.latitude IS NOT NULL
    AND e.longitude IS NOT NULL
ORDER BY d.district_name, e.nom
LIMIT 100
```

### Consulta Complexa 2: Anàlisi per Categories
```sql
SELECT 
    f.filter_name as categoria,
    COUNT(DISTINCT e.register_id) as total_equipaments,
    COUNT(DISTINCT d.district_name) as districtes_amb_equipaments
FROM equipaments e
JOIN filtres_secundaris f ON e.register_id = f.equipament_id
JOIN direccions d ON e.register_id = d.equipament_id
GROUP BY f.filter_name
HAVING total_equipaments > 5
ORDER BY total_equipaments DESC
```

### Consulta Complexa 3: Equipaments amb Atributs Específics
```sql
SELECT 
    e.nom,
    e.institution_name,
    v.attribute_name,
    v.value,
    v.description
FROM equipaments e
JOIN valors v ON e.register_id = v.equipament_id
WHERE 
    v.attribute_name LIKE '%accés%'
    OR v.attribute_name LIKE '%discapacitat%'
ORDER BY e.nom
LIMIT 50
```

---

## Preguntes Freqüents

### Generals

**P: Puc utilitzar l'aplicació des de casa?**  
R: No, l'aplicació només és accessible des de la xarxa interna. Contacteu amb IT per obtenir accés VPN si necessiteu treballar remotament.

**P: És necessari registrar-se o iniciar sessió?**  
R: No, l'aplicació no requereix autenticació ja que està protegida a nivell de xarxa.

**P: Quantes consultes puc fer simultàniament?**  
R: Podeu executar una consulta a la vegada. Espereu que es completi abans d'executar la següent.

**P: On puc trobar exemples de consultes SQL?**  
R: Al mateix manual, secció "Consultes SQL Personalitzades" i "Exemples Avançats de Consultes".

### Tècniques

**P: Per què no puc executar consultes INSERT o UPDATE?**  
R: Per motius de seguretat, només es permeten consultes SELECT. La base de dades és de només lectura per als usuaris web.

**P: Quin és el límit màxim de resultats?**  
R: El sistema retorna un màxim de 1000 registres per consulta. Utilitzeu `LIMIT` per controlar el nombre de resultats.

**P: Com puc exportar els resultats a Excel?**  
R: Seleccioneu les files desitjades a la taula, copieu-les (Ctrl+C) i enganxeu-les directament a Excel.

**P: Puc guardar les meves consultes SQL?**  
R: L'aplicació no té funcionalitat de desar consultes. Es recomana mantenir un document extern amb les consultes més utilitzades.

### Dades

**P: Amb quina freqüència s'actualitzen les dades?**  
R: Les dades s'actualitzen periòdicament segons les fonts oficials de l'Ajuntament de Barcelona. Consulteu amb IT per conèixer l'última actualització.

**P: Per què alguns equipaments no tenen coordenades GPS?**  
R: No tots els equipaments de la base de dades original tenen informació de geolocalització completa.

**P: Com interpreto els horaris (timetable)?**  
R: El camp `timetable` conté text lliure amb els horaris. Pot incloure dies, hores i excepcions.

**P: Què significa NULL a les dades?**  
R: NULL indica que no hi ha informació disponible per a aquell camp específic.

---

## Annexos

### Annex A: Estructura de la Base de Dades

#### Taula: equipaments
| Camp | Tipus | Descripció |
|------|-------|------------|
| register_id | VARCHAR(50) | Identificador únic PK |
| nom | VARCHAR(255) | Nom de l'equipament |
| institution_id | VARCHAR(50) | ID de la institució |
| institution_name | VARCHAR(255) | Nom de la institució |
| created | TIMESTAMP | Data de creació |
| modified | TIMESTAMP | Darrera modificació |
| geo_x | FLOAT | Coordenada X |
| geo_y | FLOAT | Coordenada Y |
| latitude | FLOAT | Latitud GPS |
| longitude | FLOAT | Longitud GPS |
| estimated_dates | VARCHAR(100) | Dates estimades |
| start_date | DATE | Data inici |
| end_date | DATE | Data final |
| timetable | TEXT | Horaris |

#### Taula: direccions
| Camp | Tipus | Descripció |
|------|-------|------------|
| equipament_id | VARCHAR(50) | FK a equipaments |
| roadtype_name | VARCHAR(100) | Tipus de via |
| road_name | VARCHAR(255) | Nom del carrer |
| start_street_number | VARCHAR(10) | Número inicial |
| neighborhood_name | VARCHAR(100) | Nom del barri |
| district_name | VARCHAR(100) | Nom del districte |
| zip_code | VARCHAR(10) | Codi postal |
| town | VARCHAR(100) | Població |

#### Taula: valors
| Camp | Tipus | Descripció |
|------|-------|------------|
| equipament_id | VARCHAR(50) | FK a equipaments |
| attribute_id | VARCHAR(50) | ID de l'atribut |
| category | VARCHAR(100) | Categoria |
| attribute_name | VARCHAR(100) | Nom de l'atribut |
| value | VARCHAR(255) | Valor |
| description | TEXT | Descripció |

#### Taula: filtres_secundaris
| Camp | Tipus | Descripció |
|------|-------|------------|
| equipament_id | VARCHAR(50) | FK a equipaments |
| filter_id | VARCHAR(50) | ID del filtre |
| filter_name | VARCHAR(255) | Nom del filtre |
| filter_fullpath | TEXT | Ruta completa |
| filter_tree | VARCHAR(255) | Arbre de categories |

### Annex B: Codis d'Error Comuns

| Codi | Missatge | Causa | Solució |
|------|----------|-------|---------|
| 404 | Page not found | URL incorrecta | Verificar http://192.168.6.10 |
| 500 | Internal server error | Error al servidor | Contactar amb IT |
| 1064 | SQL syntax error | Sintaxi SQL incorrecta | Revisar consulta SQL |
| 1146 | Table doesn't exist | Taula no existent | Verificar nom de taula |
| 1054 | Unknown column | Columna no existent | Verificar nom de columna |
| Connection timeout | Temps d'espera | Servidor no respon | Refrescar i reintentar |

### Annex C: Compatibilitat de Navegadors

| Navegador | Versió Mínima | Versió Recomanada | Suport |
|-----------|---------------|-------------------|---------|
| Chrome | 90 | 120+ | Complet |
| Firefox | 88 | 115+ | Complet |
| Edge | 90 | 120+ | Complet |
| Safari | 14 | 17+ | Complet |
| Opera | 75 | 100+ | Limitat |
| Internet Explorer | - | - | No suportat |

---

## Notes Finals

### Actualitzacions del Manual

Aquest manual està subjecte a actualitzacions periòdiques. Consulteu sempre la versió més recent disponible al portal intern de documentació.

- **URL Interna:** http://docs.intern/manuals/aplicacio-web
- **Versió Actual:** 1.0
- **Data Última Revisió:** Novembre 2025
- **Propera Revisió Prevista:** Març 2026

### Feedback i Millores

Per enviar suggeriments, correccions o preguntes:

- Email: suport@grup6.itb.cat
- Formulari de feedback: http://feedback.intern

---

Grup 6 - ASIX | Institut Tecnològic de Barcelona  
Hamza Tayibi, Eduard Pérez, Guim Ballvé, Francesc Martínez