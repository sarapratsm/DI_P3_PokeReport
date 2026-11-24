# 🔴 PokeReport PDF Generator

![CFGS DAM](https://img.shields.io/badge/CFGS-Desenvolupament%20d%27Aplicacions%20Multiplataforma%20%28DAM%29-green)
![DI](https://img.shields.io/badge/assignatura-Disseny%20d%27Interf%C3%ADcies-yellow)

![Estat](https://img.shields.io/badge/Estat-Completat-success?style=for-the-badge&logo=github)
![.NET](https://img.shields.io/badge/.NET-8.0-purple?style=for-the-badge&logo=dotnet)
![QuestPDF](https://img.shields.io/badge/Llibreria-QuestPDF-blue?style=for-the-badge)
![WPF](https://img.shields.io/badge/UI-WPF-b02010?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-MariaDB-2496ED?style=for-the-badge&logo=docker)

Una aplicació d'escriptori **WPF** dissenyada per generar informes visuals detallats de Pokémon en format PDF utilitzant la potència de **QuestPDF**. 

Aquest projecte replica l'estètica d'una Pokédex moderna, integrant gràfics d'estadístiques, etiquetes de tipus amb colors dinàmics i gestió d'imatges en temps real.

---

## 📋 Descripció del Projecte

Desenvolupat com a part de l'assignatura de **Desenvolupament d'Interfícies**, l'objectiu principal és demostrar la generació programàtica de documents complexos sense utilitzar dissenyadors visuals "drag-and-drop".

L'aplicació es connecta a una base de dades MySQL per obtenir informació detallada (estadístiques base, tipus, noms), descarrega els actius gràfics oficials i compila un informe paginat i professional.


---

## ✨ Característiques Principals

### 🎨 Disseny i Maquetació (QuestPDF)
* **Layout de 3 Columnes:** Estructura clara amb Imatge, Informació i Estadístiques.
* **Estètica "Card":** Contenidors amb vores arrodonides (`BorderRadius`) i espaiat cuidat.
* **Gràfics de Barres Simulats:** Visualització de stats (HP, ATK, DEF...) alineades a la base mitjançant codi C#.
* **Colors Dinàmics:** Les etiquetes de tipus (ex: *GRASS*, *POISON*) obtenen el seu color hexadecimal (`#78C850`, etc.) directament de la base de dades.

### ⚙️ Funcionalitats Tècniques
* **Càrrega Asíncrona:** Pantalla de càrrega (`LoadingScreen`) amb barra de progrés real mentre es descarreguen les imatges.
* **Sistema de Memòria Cau:** Les imatges es guarden a la carpeta local `/Images` per evitar descàrregues innecessàries en futures execucions.
* **Visor Integrat:** Utilització de `WebView2` per previsualitzar el PDF generat dins de la mateixa finestra WPF.
* **Entity Framework Core:** Consultes complexes amb `Include` per unir taules de Pokémon, Tipus i Estadístiques Base.

---

## 🛠️ Tecnologías Utilitzades

| Tecnologia | Ús |
| :--- | :--- |
| **C# (.NET 8.0)** | Llenguatge principal i lògica de negoci. |
| **WPF** | Interfície d'usuari (XAML). |
| **QuestPDF** | Motor de generació de PDF (Codi obert). |
| **Entity Framework Core** | ORM per a l'accés a dades. |
| **MySQL / MariaDB** | Base de dades relacional. |
| **WebView2** | Component per visualitzar el PDF a l'app. |
| **Docker** | Contenidor de MariaDB per la base de dades. |

---

## 🐳 Base de Dades con Docker

El proyecto incluye un `docker-compose.yml` para levantar una base de datos MariaDB lista para usar con los datos de Pokémon ya cargados.

### Requisitos previos
- [Docker](https://www.docker.com/) y [Docker Compose](https://docs.docker.com/compose/) instalados.

### Levantar la base de datos

Desde la raíz del proyecto, ejecuta:

```bash
docker-compose up -d
```

Esto iniciará:
- **Servicio:** `db` (MariaDB 10.4)
- **Puerto:** `3306:3306` (accesible desde `localhost`)
- **Base de datos:** `pokemon`
- **Usuario:** `root` (sin contraseña)
- **Dump inicial:** Se carga automáticamente desde `dump-pokedex-original.sql`

### Verificar que está corriendo

```bash
docker-compose ps
```

O conectarte directamente:

```bash
docker exec -it pokeapp_mariadb mysql -u root pokemon
```

### Detener y eliminar

```bash
docker-compose down
```

> ⚠️ **Nota:** Si usas `docker-compose down -v`, se eliminarán los datos persistidos de la base de datos.

---

## 🔗 Conectar la aplicación a la BD en Docker

La cadena de conexión en `AppDbContext.cs` ya está configurada para apuntar a `localhost:3306`:

```csharp
server=localhost;database=pokemon;port=3306;uid=root
```

Esto significa que, una vez levantado el contenedor Docker, la aplicación WPF podrá conectarse directamente sin cambios de configuración.

---

## 🚀 Instal·lació i Posada en Marxa

### Opción 1: Con Docker (recomendado)

1.  **Clonar el repositori:**
    ```bash
    git clone https://github.com/kuro-spm/DI_P3_PokeReport.git
    ```

2.  **Levantar la base de datos con Docker:**
    ```bash
    docker-compose up -d
    ```

3.  **Restaurar Dependències:**
    Obre la solució a Visual Studio i deixa que es restaurin els paquets NuGet:
    * `QuestPDF`
    * `Microsoft.EntityFrameworkCore`
    * `Pomelo.EntityFrameworkCore.MySql` (o similar)

4.  **Executar:**
    Prem **F5**. La primera vegada veuràs la barra de progrés mentre es descarreguen les imatges dels Pokémon.

### Opción 2: Sin Docker (BD local)

1.  **Clonar el repositori:**
    ```bash
    git clone https://github.com/kuro-spm/DI_P3_PokeReport.git
    ```

2.  **Configurar la Base de Dades:**
    * Importa el fitxer `dump-pokedex-original.sql` (inclòs al projecte) al teu servidor MySQL (XAMPP, Workbench, etc.).
    * Verifica la cadena de connexió (`connection string`) a `AppDbContext.cs` o `InvoiceDocumentDataSource.cs`.

3.  **Restaurar Dependències:**
    Obre la solució a Visual Studio i deixa que es restaurin els paquets NuGet.

4.  **Executar:**
    Prem **F5**.

---

## 🧪 Cómo probar la app

### 1. Verificar que Docker está corriendo
```bash
docker-compose ps
```
Deberías ver el contenedor `pokeapp_mariadb` en estado `Up`.

### 2. Verificar la conexión a la base de datos
```bash
docker exec -it pokeapp_mariadb mysql -u root pokemon -e "SHOW TABLES;"
```
Deberías ver tablas como `pokemon`, `types`, `base_stats`, etc.

### 3. Ejecutar la aplicación
- Abre `PokeReport_Sara.slnx` en Visual Studio 2022
- Asegúrate de que el proyecto de inicio es `PokeReport_Sara`
- Presiona **F5** (o clic en "Iniciar")

### 4. Qué esperar al ejecutar
1. Aparecerá una pantalla de carga con barra de progreso
2. La app descargará las imágenes de los primeros 9 Pokémon (se guardan en `/Images`)
3. Se generará un archivo `invoice.pdf` en la carpeta del ejecutable
4. Se abrirá el visor de PDF integrado (WebView2) mostrando el informe

### 5. Solución de problemas comunes

| Problema | Solución |
| :--- | :--- |
| Error de conexión a BD | Asegúrate de que `docker-compose up -d` esté corriendo |
| Imágenes no descargan | Verifica conexión a internet (descarga de pokemon.com) |
| PDF no se genera | Revisa que QuestPDF tenga licencia Community configurada |
| Error de NuGet | Limpia la solución y restaura paquetes (`dotnet restore`) |

---

## 📂 Estructura del Codi

L'arquitectura separa clarament la lògica de la vista de la generació del document:

* `MainWindow.xaml`: Orquestra la UI, la barra de progrés i el visor web.
* `InvoiceDocument.cs`: **Nucli del disseny**. Conté tota la lògica visual de QuestPDF (Capçalera, Files de Pokémon, Gràfics).
* `InvoiceDocumentDataSource.cs`: Servei encarregat de parlar amb la BD i preparar el model.
* `Models/`: Classes POCO (`Pokemon`, `Type`, `BaseStat`) mapejades des de la base de dades.

---

## ✒️ Autor

**Sara Prats Morales**
* [LinkedIn](https://www.linkedin.com/in/sara-prats-morales)
* [GitHub](https://github.com/kuro-spm)
