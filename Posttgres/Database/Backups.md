Backups
=======

Overview
--------

On [Pro and Business plans](https://www.prisma.io/pricing), Prisma Postgres automatically creates snapshots of your database to support recovery and backup workflows. Navigate to the **Backups** tab of your Prisma Postgres instance in the [Prisma Console](https://console.prisma.io/?) to view and re-instantiate your available backups.

Snapshots are created _daily_, but only on days when the database has activity. Depending on your plan, you will see a different number of available snapshots:

**PlanSnapshot retention**ProLast 7 daysBusinessLast 30 days

Please note that any database changes or events that occurred after the most recent snapshot may not be restored.

For more details about backup availability and plan-specific features, visit our [pricing page](https://www.prisma.io/pricing).

**note**

In the future, Prisma Postgres will provide more fine-grained backup mechanisms based on user specific configurations and with point-in-time restore functionality.

Manually creating a backup file via pg\_dump
--------------------------------------------

If you would like to create a backup file of your database, you can use pg\_dump and use a [direct connection](https://www.prisma.io/docs/postgres/database/direct-connections). This is useful for migrating data between databases or creating a local copy of your database.

Prerequisites
-------------

Before you begin, make sure you have:

*   **Node.js** installed (version 16 or higher).
    
*   **PostgreSQL CLI Tools** (pg\_dump) for creating backups. Use Postgres version 17 as Prisma Postgres is based on this version.
    
*   A **direct connection string** for your Prisma Postgres database.
    

1\. Install PostgreSQL command-line tools
-----------------------------------------

To create backups, ensure you have the PostgreSQL command-line tools installed. Run the following commands based on your operating system:

*   macOS
    
*   Windows
    
*   Linux
    

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   brew install postgresql@17which pg_dumpwhich pg_restore   `

**tip**

If you installed PostgreSQL but still see a “command not found” error for pg\_dump or pg\_restore, ensure your installation directory is in your system’s PATH environment variable.

### 2\. Creating the Backup with pg\_dump

Get your direct connection string for Prisma Postgres by following the instructions [here](https://www.prisma.io/docs/postgres/database/direct-connections#how-to-connect-to-prisma-postgres-via-direct-tcp).

You can now dump the database by running the following command and using your own connection string:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   pg_dump --dbname="postgres://USER:PASSWORD@db.prisma.io:5432/?sslmode=require" > ./mydatabase.bak   `

This will create your backup file named mydatabase.bak in the current directory.
