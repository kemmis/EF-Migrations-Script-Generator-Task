# Entity Framework Core Migrations Script Generator
**Entity Framework Core Migrations Script Generator** is a very simple extensions to make it easy
to generate migration script for projects using Entity Framework Core with Code-First. This tool internally calls **dotnet ef migrations script**.

## How to generate migration scripts
With this task it's very easy to generate migration scripts:

* Add Entity Framework Core Migrations Script Generator task to your build pipeline.
* Select a project that is using the database contexts. **Note:** If you have your database contexts in a library, select a project that is using that library.
* Enter the names of the database contexts.
* You could also change the directory where the migrations scripts should be stored. By default they are stored in a folder named **migrations**.

When the build is completed you should have migrations scripts stored in the package. They named {{NameOfTheDatabaseContext}}.sql. The migrations scripts are idempotent, meaning that you could run the several times and the end result should be the same even if you have run the script before. So it's safe to run the migration on every release even if you haven't done any changes.

## How to apply migrations to your databases
When you have your migration scripts ready you just need to apply them in a release pipeline. If you have your databases in Azure you could to like this:

* Add the task **Azure SQL Database Deployment** to your release pipeline.
* Enter the details of your database.
* In the option **SQL Script** select the migration script from the package.

If you have several databases, add a new task for each database.



# Notes about the source
In the folder **efcore-migration-task** the complete source is for this project.

The folder **NetCoreTestApplication** contains a test project with two database contexts
that could be used for generating migration scripts.

# How to make changes
Changes are hopefully never needed :-) But if the logic need to be changes index.ts should be modified.
If the UI need to be changed task.json should be updated.

## How to test changes
There are some commands that are good to know to test the extension locally. These should be executed
in the folder **efcore-migration-task/efcore-migration-script-generator**

    tsc

This compiles the typescript file index.ts typescript file into index.js.

    $env:INPUT_PROJECTPATH="C:/Users/msn/Source/Repos/EF-Migrations-Script-Generator-Task/NetCoreTestApplication/NetCoreTestApplication/NetCoreTestApplication.csproj"
    $env:INPUT_TARGETFOLDER="c:/temp"
    $env:INPUT_DATABASECONTEXTS="FirstDatabaseContext`nSecondDatabaseContext"

These commands sets enviroment variables. These are used for setting input values for the script.

    node index.js

This executes the script.

## How to create a new release
To create a new release one single command is needed to create a vsix-file. This should be executed
in the **efcore-migration-task** directory:

    tfx extension create


# References
Mainfest description:
https://docs.microsoft.com/sv-se/azure/devops/extend/develop/manifest

Task.json reference:
https://raw.githubusercontent.com/Microsoft/vsts-task-lib/master/tasks.schema.json
