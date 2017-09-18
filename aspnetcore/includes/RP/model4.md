<span data-ttu-id="04133-101">Die folgende Tabelle zeigt die Details der Parameter des Codegenerators von ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="04133-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="04133-102">Parameter</span><span class="sxs-lookup"><span data-stu-id="04133-102">Parameter</span></span>               | <span data-ttu-id="04133-103">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="04133-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="04133-104">-m</span><span class="sxs-lookup"><span data-stu-id="04133-104">-m</span></span>  | <span data-ttu-id="04133-105">Der Name des Modells.</span><span class="sxs-lookup"><span data-stu-id="04133-105">The name of the model.</span></span> |
| <span data-ttu-id="04133-106">-dc</span><span class="sxs-lookup"><span data-stu-id="04133-106">-dc</span></span>  | <span data-ttu-id="04133-107">Der Datenkontext.</span><span class="sxs-lookup"><span data-stu-id="04133-107">The data context.</span></span> |
| <span data-ttu-id="04133-108">-udl</span><span class="sxs-lookup"><span data-stu-id="04133-108">-udl</span></span> | <span data-ttu-id="04133-109">Verwendung des Standardlayouts.</span><span class="sxs-lookup"><span data-stu-id="04133-109">Use the default layout.</span></span> |
| <span data-ttu-id="04133-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="04133-110">-outDir</span></span> | <span data-ttu-id="04133-111">Der relative Ausgabeordnerpfad zur Erstellung der Ansichten.</span><span class="sxs-lookup"><span data-stu-id="04133-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="04133-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="04133-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="04133-113">Fügt `_ValidationScriptsPartial` zu den Seiten „Create“ und „Edit“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="04133-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="04133-114">Verwenden Sie den `h`-Switch um Hilfe für den `aspnet-codegenerator razorpage`-Befehl zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="04133-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="04133-115">Testen der App</span><span class="sxs-lookup"><span data-stu-id="04133-115">Test the app</span></span>

* <span data-ttu-id="04133-116">Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="04133-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="04133-117">Testen Sie den Link **Create** (Erstellen).</span><span class="sxs-lookup"><span data-stu-id="04133-117">Test the **Create** link.</span></span>

 ![Seite „Create“](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="04133-119">Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).</span><span class="sxs-lookup"><span data-stu-id="04133-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="04133-120">Wenn die folgende Fehlermeldung angezeigt wird, stellen Sie sicher, dass Migrationen ausgeführt wurden und die Datenbank aktualisiert ist:</span><span class="sxs-lookup"><span data-stu-id="04133-120">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```