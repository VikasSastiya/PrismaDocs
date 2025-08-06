Postgres extensions
===================

Overview
--------

Prisma Postgres supports the following [PostgreSQL extensions](https://www.postgresql.org/docs/current/sql-createextension.html):

*   [pgvector](https://github.com/pgvector/pgvector)
    
*   [pg\_stat\_statements](https://www.postgresql.org/docs/current/pgstatstatements.html)
    
*   [citext](https://www.postgresql.org/docs/current/citext.html)
    
*   [pg\_trgm](https://www.postgresql.org/docs/current/pgtrgm.html)
    
*   [fuzzystrmatch](https://www.postgresql.org/docs/current/fuzzystrmatch.html)
    
*   [unaccent](https://www.postgresql.org/docs/current/unaccent.html)
    
*   [pg\_search](https://pgxn.org/dist/pg_search)
    

Support for [more extensions](https://www.prisma.io/docs/postgres/database/postgres-extensions#other-extensions-are-coming-soon) will follow soon. If there are specific extensions you'd like to see in Prisma Postgres, [fill out this form](https://pris.ly/i-want-extensions).

**warning**

Postgres extensions support in Prisma Postgres is currently in [Early Access](https://www.prisma.io/docs/platform/maturity-levels#early-access) and not yet recommended for production scenarios.

Using extensions with Prisma ORM
--------------------------------

Some extensions may already be supported by Prisma Postgres but not yet by Prisma ORM. Native support for some Postgres extensions in Prisma ORM is coming soon. In the meantime, you can still use these extensions with Prisma ORM by using [customized migrations](https://www.prisma.io/docs/orm/prisma-migrate/workflows/customizing-migrations) and [TypedSQL](https://www.prisma.io/docs/orm/prisma-client/using-raw-sql/typedsql) (or another mechanism to send [raw SQL](https://www.prisma.io/docs/orm/prisma-client/using-raw-sql) via in Prisma ORM).

Let's walk through an example with pgvector.

### 1\. Create an empty migration file

To customize a migration, first create an empty migration file:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx prisma migrate dev --name add-pgvector --create-only   `

Notice the --create-only flag which will create an empty migration file in your migrations directory.

### 2\. Create and use the extension in your migration file

In the empty migration file, you can write any custom SQL to be executed in the database:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   -- prisma/migrations/-add-pgvector/migration.sqlCREATE EXTENSION IF NOT EXISTS vector;CREATE TABLE "Document" (  id SERIAL PRIMARY KEY,  title TEXT NOT NULL,  embedding VECTOR(4) -- use 4 for demo purposes; real-world values are much bigger);   `

In this case, you:

*   install the pgvector extension in your database using the CREATE EXTENSION statement
    
*   create a Document table that uses the VECTOR type from that extension
    

### 3\. Execute the migration against the database

Run the following command to execute the migration and apply its changes in your database:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx prisma migrate deploy   `

This command will apply the pending prisma/migrations/\-add-pgvector/migration.sql migration and create the Document table in your database.

### 4\. Pull the document table into your Prisma schema

Introspect the database schema with the new Document table and update your Prisma schema with it:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx prisma db pull   `

**Show CLI results**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   Environment variables loaded from .envPrisma schema loaded from prisma/schema.prismaDatasource "db": PostgreSQL database "postgres", schema "public" at "accelerate.prisma-data.net"✔ Introspected 3 models and wrote them into prisma/schema.prisma in 3.23s      *** WARNING ***These fields are not supported by Prisma Client, because Prisma currently does not support their types:  - Model: "Document", field: "embedding", original data type: "vector"Run prisma generate to generate Prisma Client.   `

The [warning in the CLI output of the command is expected](https://www.prisma.io/docs/orm/prisma-schema/introspection#introspection-warnings-for-unsupported-features) because Prisma ORM doesn't natively support the VECTOR type yet.

You Prisma schema will now contain the Document model:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   model Document {  id        Int                    @id @default(autoincrement())  title     String  embedding Unsupported("vector")?}   `

Because the VECTOR type is not yet natively supported by Prisma ORM, it's represented as an [Unsupported](https://www.prisma.io/docs/orm/prisma-schema/data-model/models#unsupported-types) type.

### 4\. Query with raw SQL

Here's an example query for inserting a new row into the Document table:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML``   await prisma.$executeRaw`  INSERT INTO "Document" (title, embedding)  VALUES ('My Title', '[1,22,1,42]'::vector)`;   ``

You can also use [TypedSQL](https://www.prisma.io/docs/orm/prisma-client/using-raw-sql/typedsql) for type-safe SQL queries against your database.

Temporary limitations
---------------------

### pgvector is only available on newly created Prisma Postgres instances

For now, pgvector is only available on _newly created_ Prisma Postgres instances. It will be rolled out for _existing_ instances soon.

### No Prisma Studio support for pgvector

Prisma Studio currently doesn't support tables where types of the pgvector extensions are used. It will show the following error:

**Show Prisma Studio error message**

Other extensions are coming soon
--------------------------------

Support for the following extensions is going to come soon:

*   [pgcrypto](https://www.postgresql.org/docs/current/pgcrypto.html)
    
*   [postgis](https://postgis.net/)
    
*   [contrib](https://www.postgresql.org/docs/current/contrib.html) extensions
    

If there are specific extensions you'd like to see, [fill out this form](https://pris.ly/i-want-extensions).
