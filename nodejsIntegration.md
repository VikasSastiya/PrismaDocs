Quickstart with Prisma ORM and Prisma Postgres
==============================================

[Prisma Postgres](https://www.prisma.io/docs/postgres) is a fully managed PostgreSQL database that scales to zero and integrates smoothly with both Prisma ORM and Prisma Studio. In this guide, you will learn how to set up a new TypeScript project from scratch, connect it to Prisma Postgres using Prisma ORM, and generate a Prisma Client for easy, type-safe access to your database.

Prerequisites
-------------

You need:

*   [Node.js](https://nodejs.org/en/) v20.19+, v22.12+, or v24.0+ installed on your machine
    
*   Basic knowledge of JavaScript or TypeScript
    

1\. Create a new project
------------------------

Create a project directory and navigate into it:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   mkdir hello-prismacd hello-prisma   `

Initialize a TypeScript project:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npm init -ynpm install typescript tsx @types/node --save-devnpx tsc --init   `

2\. Install required dependencies
---------------------------------

Install the packages needed for this quickstart:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npm install prisma @types/node @types/pg --save-dev npm install @prisma/client @prisma/adapter-pg pg dotenv   `

Here's what each package does:

*   **prisma** - The Prisma CLI for running commands like prisma init, prisma migrate, and prisma generate
    
*   **@prisma/client** - The Prisma Client library for querying your database
    
*   **@prisma/adapter-pg** - The [node-postgres driver adapter](https://www.prisma.io/docs/orm/overview/databases/postgresql#using-the-node-postgres-driver) that connects Prisma Client to your database
    
*   **pg** - The node-postgres database driver
    
*   **@types/pg** - TypeScript type definitions for node-postgres
    
*   **dotenv** - Loads environment variables from your .env file
    

3\. Configure ESM support
-------------------------

Update tsconfig.json for ESM compatibility:

**tsconfig.json**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   {  "compilerOptions": {    "module": "ESNext",    "moduleResolution": "node",    "target": "ES2023",    "strict": true,    "esModuleInterop": true,    "ignoreDeprecations": "6.0"  }}   `

Update package.json to enable ESM:

**package.json**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   {  "type": "module",}   `

4\. Initialize Prisma ORM and create a Prisma Postgres database
---------------------------------------------------------------

You can now invoke the Prisma CLI by prefixing it with npx:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx prisma   `

Next, set up your Prisma ORM project by creating your [Prisma Schema](https://www.prisma.io/docs/orm/prisma-schema) file with the following command:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx prisma init --db --output ../generated/prisma   `

**info**

You'll need to answer a few questions while setting up your Prisma Postgres database. Select the region closest to your location and a memorable name for your database.

This command does a few things:

*   Creates a prisma/ directory with a schema.prisma file containing your database connection and schema models
    
*   Creates a new Prisma Postgres database (when using --db flag)
    
*   Creates a .env file in the root directory for environment variables
    
*   Creates a prisma.config.ts file for Prisma configuration
    

The generated prisma.config.ts file looks like this:

**prisma.config.ts**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   import 'dotenv/config'import { defineConfig, env } from 'prisma/config'export default defineConfig({  schema: 'prisma/schema.prisma',  migrations: {    path: 'prisma/migrations',  },  datasource: {    url: env('DATABASE_URL'),  },})   `

The generated schema uses [the ESM-first prisma-client generator](https://www.prisma.io/docs/orm/prisma-schema/overview/generators#prisma-client) with a custom output path:

**prisma/schema.prisma**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   generator client {  provider = "prisma-client"  output   = "../generated/prisma"}datasource db {  provider = "postgresql"}   `

5\. Define your data model
--------------------------

Open prisma/schema.prisma and add the following models:

**prisma/schema.prisma**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   generator client {  provider = "prisma-client"  output   = "../generated/prisma"}datasource db {  provider = "postgresql"}model User {  id    Int     @id @default(autoincrement())  email String  @unique  name  String?  posts Post[]}model Post {  id        Int     @id @default(autoincrement())  title     String  content   String?  published Boolean @default(false)  author    User    @relation(fields: [authorId], references: [id])  authorId  Int}   `

6\. Create and apply your first migration
-----------------------------------------

Create your first migration to set up the database tables:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx prisma migrate dev --name init   `

This command creates the database tables based on your schema.

Now run the following command to generate the Prisma Client:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx prisma generate   `

7\. Instantiate Prisma Client
-----------------------------

Now that you have all the dependencies installed, you can instantiate Prisma Client. You need to pass an instance of Prisma ORM's driver adapter to the PrismaClient constructor:

**lib/prisma.ts**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML``   import "dotenv/config";import { PrismaPg } from '@prisma/adapter-pg'import { PrismaClient } from '../generated/prisma/client'const connectionString = `${process.env.DATABASE_URL}`const adapter = new PrismaPg({ connectionString })const prisma = new PrismaClient({ adapter })export { prisma }   ``

**tip**

If you need to query your database via HTTP from an edge runtime (Cloudflare Workers, Vercel Edge Functions, etc.), use the [Prisma Postgres serverless driver](https://www.prisma.io/docs/postgres/database/serverless-driver#use-with-prisma-orm).

8\. Write your first query
--------------------------

Create a script.ts file to test your setup:

**script.ts**

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   import { prisma } from './lib/prisma'async function main() {  // Create a new user with a post  const user = await prisma.user.create({    data: {      name: 'Alice',      email: 'alice@prisma.io',      posts: {        create: {          title: 'Hello World',          content: 'This is my first post!',          published: true,        },      },    },    include: {      posts: true,    },  })  console.log('Created user:', user)  // Fetch all users with their posts  const allUsers = await prisma.user.findMany({    include: {      posts: true,    },  })  console.log('All users:', JSON.stringify(allUsers, null, 2))}main()  .then(async () => {    await prisma.$disconnect()  })  .catch(async (e) => {    console.error(e)    await prisma.$disconnect()    process.exit(1)  })   `

Run the script:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx tsx script.ts   `

You should see the created user and all users printed to the console!

9\. Explore your data with Prisma Studio
----------------------------------------

Prisma Studio is a visual editor for your database. Launch it with:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   npx prisma studio --config ./prisma.config.ts   `

This opens a web interface where you can view and edit your data.

**Supported databases**

Prisma Studio currently supports PostgreSQL, MySQL, and SQLite. For more details, see [Databases supported by Prisma Studio](https://www.prisma.io/docs/orm/tools/prisma-studio#databases-supported-by-prisma-studio).

Next steps
----------

You've successfully set up Prisma ORM. Here's what you can explore next:

*   **Learn more about Prisma Client**: Explore the [Prisma Client API](https://www.prisma.io/docs/orm/prisma-client) for advanced querying, filtering, and relations
    
*   **Database migrations**: Learn about [Prisma Migrate](https://www.prisma.io/docs/orm/prisma-migrate) for evolving your database schema
    
*   **Performance optimization**: Discover [query optimization techniques](https://www.prisma.io/docs/orm/prisma-client/queries/query-optimization-performance)
    
*   **Build a full application**: Check out our [framework guides](https://www.prisma.io/docs/guides) to integrate Prisma ORM with Next.js, Express, and more
    
*   **Join the community**: Connect with other developers on [Discord](https://pris.ly/discord)
