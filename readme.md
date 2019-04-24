# Laravel for Azure Tutorial

## Installation

Have PHP >= 7.1.3 installed

Below are reuqired extension:
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension
- Ctype PHP Extension
- JSON PHP Extension
- BCMath PHP Extension

## Local Laravel scaffolding
 
### Utilize Composer Create-Project to create scaffold **Laravel** project

``` bash
composer create-project --prefer-dist laravel/laravel Laravel-DEMO
```
### MySQL integration

Create '**sampledb**' database in the your newly created MySQL
``` sql
CREATE DATABASE sampledb;
```

Update '**.env**' file under /Laravel-DEMO
``` conf
DB_CONNECTION = mysql
DB_HOST = 127.0.0.1
DB_PORT = 3306
DB_DATABASE = sampledb
DB_USERNAME = <username>
DB_PASSWORD = <password>
```
### Ready up the App
Since the scaffold Laravel project has already installed all composer vendor and provided a key in **.env** file during its initialization so we don't need '```composer install```' & '```php artisan key:generate```'

``` bash
php artisan migrate
php artisan serve
```
You should see something similar to this after running above commands
``` bash
PS D:\MyProjects\Laravel\DEMO> php artisan migrate
Migration table created successfully.
Migrating: 2014_10_12_000000_create_users_table
Migrated:  2014_10_12_000000_create_users_table
Migrating: 2014_10_12_100000_create_password_resets_table
Migrated:  2014_10_12_100000_create_password_resets_table

PS D:\MyProjects\Laravel\DEMO> php artisan serve
Laravel development server started: <http://127.0.0.1:8000>
```
Open [http://127.0.0.1:8000](http://127.0.0.1:8000 "localhost") to check the site locally.

## Azure Integration

### MySQL Creation
In Microsoft Azure Portal, '**Create a resource**' -> '**Azure Database for MySQL**' to create a MySQL database.

### Connect Azure MySQL from local MySQL Workbench
#### Azure MySQL configuration
1. In Microsoft Azure Portal, go into the MySQL resource just created, go to '**Connection Security**', add your local IP address using '**Add client IP**' button besides '**Save**' and '**Discard**' (find your public IP [here](https://www.google.com/search?as_q=my+ip))
2. Make sure '**Allow access to Azure services**' is **ON** and '**Enforce SSL connection**'
 is **DISABLED** for now.

#### Create holder DB from local MySQL Workbench
Use the info from Azure MySQL overview to connect from local MySQL Workbench, then execute query below

``` sql
CREATE DATABASE sampledb;
```
### Deploy to Azure Web App
#### Web App prep

#### Update MySQL info '**.env**' file accordingly
``` conf
DB_CONNECTION = mysql
DB_HOST = <db_name>.mysql.database.azure.com
DB_PORT = 3306
DB_DATABASE = sampledb
DB_USERNAME = <username>
DB_PASSWORD = <password>
```

#### Edit '**.gitignore**' file to include '**.env**' in the repo
``` 
/node_modules
/public/hot
/public/storage
/storage/*.key
/vendor
.env  <---------------- Remove this line so '.env' is not ginored by Git
.phpunit.result.cache
Homestead.json
Homestead.yaml
npm-debug.log
yarn-error.log
```

#### Deploy to Azure Web App vai local Git
1. Create a Web App in Azure Portal (Windows / Linux)
2. '**Extensions > Add**' and add '**Composer**' extension
3. In Web App '**Configuration - General**', update to corresponding **PHP** stack and preffered version.
4. In Web App '**Configuration - Path mappings - Virtual applications and directories**', update '**site\wwwroot**' to '**site\wwwroot\public**', click save.
5. In Web App '**Deployment Center - Git**', get and add remote repo link to local Git
6. Push from local to Azure remote repo

#### Bring site up
1. In Web App '**Advanced Tools - Go**' to get to Kudu site
2. Under '**Debug console**', go '**CMD/Powershell**' (Windows) / '**Bash**' (Linux)
3. Run ```composer install```
4. Run ```php artisan migrate```

#### Site should be now UP

 -- Shawn.
