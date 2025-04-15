# sql-server-docker (azure-sql-edge)
📁 Project Structure
```
sql-server-docker/
├── sql-import/
│   └── your-database.bacpac
└── sql-package-image/
│   └── Dockerfile
└── sql-server-yml
```
***
✅ 1. Ensure SQL Server is Running in Docker Start it:
```bash
docker compose -f sql-server.yml up -d
```
✅ 2. Prepare .bacpac File

Place your .bacpac file into the `docker/sql-import/` directory:

✅ 3. Build the Docker image with `sqlpackage`
```bash
cd sql-package-image
```
```bash
docker buildx create --use
docker buildx build --platform linux/amd64 -t sqlpackage-image . --load
```
✅ 4. Run `sqlpackage` Container and Import `.bacpac`

Go to `sql-import` directory:
```bash
cd ../sql-import
```
and then:
```bash
docker run --rm -it \
  -v "$(pwd)":/tmp \
  --platform linux/amd64 \
  sqlpackage-image bash
```
Inside the container, run:
```bash
sqlpackage /a:Import \
/sf:/tmp/your-database.bacpac \
/tsn:localhost \
/tdn:ImportedDB \
/tu:sa \
/tp:'YourStrong!Passw0rd' \
/ttsc:true
```
🧼 5. Exit and Verify

> In case `sqlpackage` container doesn't work for you - I have been encountered the following issue:
```
*** An unexpected failure occurred: The lazily-initialized type does not have a public, parameterless constructor..
```
✅ 1. Download and install `Azure Data Studio` - (GUI) -> [Azure website][azure] -> Download now 

✅ 2.  Install the SQL Server Dacpac Extension
Open Azure Data Studio.

* Click on the `Extensions` icon in the `Activity Bar` on the side (or press Ctrl+Shift+X).
* In the search bar, type `SQL Server Dacpac`.
* Find the extension in the list and click Install.
* After installation, you may need to reload `Azure Data Studio` to activate the extension.

✅ 3. Importing a .bacpac File

* In `Azure Data Studio`, connect to your SQL Server instance.
* In the Connections pane, expand your server to view the `databases`.
* Right-click on the `Databases` folder and select `Data-tier Application Wizard`.
* In the wizard, choose `Create a database from a .bacpac file [Import Bacpac]`.
* Follow the prompts to specify the `database name` and `file location`.
* Review the summary and click `Finish` to start the import process.
***

[azure]: https://azure.microsoft.com/en-us/products/data-studio# sql-server-docker-azure
