**PHP8.3-apache-mssql**

Start the dockerized web-apllication(php) with <<./launcher.sh>> command


**Purpose of each file in this task:**

**apache-config.conf:**

configuration snippet sets up a virtual host to serve content from a specific document root directory, defines access control and directory options for that directory, and specifies the locations for error and access logs.

**Dockerfile for PHP:**

An environment for executing a PHP web application with the Apache web server is built up by this Dockerfile. This is a succinct explanation:

Base Graphic: It begins with the php:8.3-apache base image, which comes with PHP version 8.3 and the Apache web server.

System Requirements: It uses apt-get to install the required system dependencies, such as Git, zip utilities, and image processing libraries.
Apache Setup: It changes the document root to /var/www/html/ and activates the Apache rewrite module. It also substitutes a custom Apache site configuration for the default one.

PHP Extensions: It sets up and installs the PDO extension for database access and the PHP GD extension for image processing.
Microsoft ODBC Driver: This allows PHP programs to connect to SQL Server databases by installing the Microsoft ODBC driver 18 for SQL Server.
PHP extensions for SQL Server are installed so that PDO and SQLSRV can be used to connect to SQL Server databases.

Application Setup: QuickDbTest.php is copied to /var/www/html/index.php, and the working directory is configured to /var/www/html.

Package Updates: It installs the Bash shell and updates and upgrades the image's packages.

File Permissions: The Apache user and group (www-data) now owns the /var/www/html directory.

Port Exposure: To enable external connections to the Apache web server, port 80 is made available.

Last but not least, it identifies the command to be executed upon container startup. The Apache web server (apache2-foreground) is started, and PHP is used to run the index.php file.


**QuickDbTest.php**


This PHP script establishes a connection to a SQL Server database, loads some data into it, builds a table if one doesn't already exist, and then extracts and exports the data in JSON format. This is a succinct explanation:

Class Definition: A QuickDbTest class is defined as final, which prevents it from being expanded. This class includes methods for connecting to databases, logging in, and executing database tests.

Constants: It establishes constants for the host, database name, username, and password database connection settings.

connectToDatabase() Method: This private method uses PHP Data Objects (PDO) to connect to the SQL Server database. It records the error message if the connection is lost.

quickLog() Private Method: This method records messages and, if supplied, ends the script with an obituary letter.

The public function runDbTest() carries out a number of database activities. It establishes a connection to the database first, and if the testing_table table doesn't already exist, it executes a SQL query to create it. After that, two rows of data are inserted into this table. Lastly, it returns the result set as an associative array after selecting every entry in the table.

JSON Output: A QuickDbTest class instance is created, the runDbTest() function is called, the returned result set is encoded as JSON using json_encode(), and the JSON is output using echo.

All in all, this script shows how to use PDO to connect with a database in PHP, managing errors and producing JSON output.


**init.sql**


This PHP script establishes a connection to a SQL Server database, loads some data into it, builds a table if one doesn't already exist, and then extracts and exports the data in JSON format. This is a succinct explanation:

Class Definition: A QuickDbTest class is defined as final, which prevents it from being expanded. This class includes methods for connecting to databases, logging in, and executing database tests.

Constants: It establishes constants for the host, database name, username, and password database connection settings.

connectToDatabase() Method: This private method uses PHP Data Objects (PDO) to connect to the SQL Server database. It records the error message if the connection is lost.

quickLog() Private Method: This method records messages and, if supplied, ends the script with an obituary letter.

The runDbTest() Method

GO: An additional GO batch separator.
This script, in its entirety, creates a new database called db_vero_digital and then changes the database context to it. In SQL scripts, it's standard practice to build a database and start running statements in it right away.


**docker-compose.yml**


The services for a PHP web application (app), a SQL Server database (sqlserver), and a SQL Server configurator (sqlserver.configurator) are defined in this Docker Compose file. This is a succinct explanation:
Version: Indicates the Docker Compose file syntax version that is in use. It is version 3.8 in this instance.


Volumes: Specifies the volume that will be used to store SQL Server data, called sqlserver_data.
Services: app: Using the given Dockerfile, creates a Docker container for the PHP web application. It sets the environment variable APACHE_DOCUMENT_ROOT to /var/www/html and exposes port 80. Sqlserver service is the determining factor.

sqlserver: Makes use of the official Docker image for SQL Server 2022. establishes the environment variables MSSQL_PID, SA_PASSWORD, and ACCEPT_EULA. opens port 1433 to connections to SQL Server. To save data, mount the sqlserver_data drive. To make sure the SQL Server service is in good condition, it defines a health check.
The SQL Server image is used by sqlserver.configurator. mounts a local directory called "./init" that contains SQL scripts for database initialization. It is contingent upon the health of the SQL Server service. When it starts up, sqlcmd is used to run the SQL script and initialize the database.
All in all, this Docker Compose file configures a PHP web application and SQL Server database in a development environment and makes sure the SQL Server database is initialized with the necessary information when it starts up.


**launcher.sh**


This script is a straightforward Bash script that launches containers specified in a docker-compose.yml file using Docker Compose.
