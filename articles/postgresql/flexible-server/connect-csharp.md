---
title: 'Inicio rápido: Conexión a Azure Database for PostgreSQL: servidor flexible con C#'
description: 'En este inicio rápido se proporciona un ejemplo de código de C# (.NET) que puede usar para conectarse a Azure Database for PostgreSQL: servidor flexible y realizar consultas ahí.'
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.custom: mvc, devcenter, devx-track-csharp
ms.devlang: csharp
ms.topic: quickstart
ms.date: 01/16/2021
ms.openlocfilehash: 7eb4d405982b3a069c77d3d6bf994d3d9f6a2bdd
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2021
ms.locfileid: "131429059"
---
# <a name="quickstart-use-net-c-to-connect-and-query-data-in-azure-database-for-postgresql---flexible-server"></a>Inicio rápido: Uso de .NET (C#) para conectarse a Azure Database for PostgreSQL: servidor flexible y realizar consultas ahí

En este tutorial rápido se muestra cómo conectarse a una instancia de Azure Database for PostgreSQL mediante una aplicación de C#. Se indica cómo usar instrucciones SQL para consultar, insertar, actualizar y eliminar datos en la base de datos. En los pasos de este artículo se da por hecho que está familiarizado con el desarrollo mediante C#, pero que nunca ha trabajado con Azure Database for PostgreSQL.

## <a name="prerequisites"></a>Prerrequisitos
Para esta guía de inicio rápido, necesitará lo siguiente:

- Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free).
- Creación de una instancia de Azure Database for PostgreSQL: servidor flexible mediante [Azure Portal](./quickstart-create-server-portal.md) <br/> o la [CLI de Azure](./quickstart-create-server-cli.md) si no tiene uno.
- Use la base de datos *postgres* vacía disponible en el servidor o cree [otra base de datos](./quickstart-create-server-portal.md#connect-to-the-postgresql-database-using-psql).
- Instale [.NET SDK para su plataforma](https://dotnet.microsoft.com/download) (Windows, Ubuntu Linux o macOS).
- Instale [Visual Studio](https://www.visualstudio.com/downloads/) para compilar el proyecto.
- Instale el paquete NuGet [Npgsql](https://www.nuget.org/packages/Npgsql/) en Visual Studio.

## <a name="get-connection-information"></a>Obtención de información sobre la conexión
Obtenga la información de conexión necesaria para conectarse a Azure Database for PostgreSQL. Necesitará el nombre completo del servidor y las credenciales de inicio de sesión.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En el menú izquierdo de Azure Portal, haga clic en **Todos los recursos** y, luego, busque el servidor que ha creado, por ejemplo, **mydemoserver**.
3. Haga clic en el nombre del servidor.
4. En el panel **Información general** del servidor, anote el **nombre del servidor** y el **nombre de inicio de sesión del administrador del servidor**. Si olvida la contraseña, puede restablecerla en este panel.
 :::image type="content" source="./media/connect-csharp/1-connection-string.png" alt-text="Nombre de servidor de Azure Database for PostgreSQL":::

## <a name="step-1-connect-and-insert-data"></a>Paso 1: Conexión e inserción de datos
Use el código siguiente para conectarse y cargar los datos mediante las instrucciones SQL **CREATE TABLE** e **INSERT INTO**. El código usa la clase NpgsqlCommand con el método:
- [()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) para establecer una conexión a la base de datos de PostgreSQL.
- [CreateCommand()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) establece la propiedad CommandText.
- [ExecuteNonQuery()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) para ejecutar los comandos de base de datos.

> [!IMPORTANT]
> Reemplace los parámetros host, dbname, user y password por los valores especificados al crear el servidor y la base de datos.

```csharp
using System;
using Npgsql;

namespace Driver
{
    public class AzurePostgresCreate
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mydemoserver.postgres.database.azure.com";
        private static string User = "mylogin";
        private static string  DBname = "postgres";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0};Username={1};Database={2};Port={3};Password={4};SSLMode=Prefer",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);


            using (var conn = new NpgsqlConnection(connString))

            {
                Console.Out.WriteLine("Opening connection");
                conn.Open();

                using (var command = new NpgsqlCommand("DROP TABLE IF EXISTS inventory", conn))
                {
                    command.ExecuteNonQuery();
                    Console.Out.WriteLine("Finished dropping table (if existed)");

                }

                using (var command = new NpgsqlCommand("CREATE TABLE inventory(id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER)", conn))
                {
                    command.ExecuteNonQuery();
                    Console.Out.WriteLine("Finished creating table");
                }

                using (var command = new NpgsqlCommand("INSERT INTO inventory (name, quantity) VALUES (@n1, @q1), (@n2, @q2), (@n3, @q3)", conn))
                {
                    command.Parameters.AddWithValue("n1", "banana");
                    command.Parameters.AddWithValue("q1", 150);
                    command.Parameters.AddWithValue("n2", "orange");
                    command.Parameters.AddWithValue("q2", 154);
                    command.Parameters.AddWithValue("n3", "apple");
                    command.Parameters.AddWithValue("q3", 100);

                    int nRows = command.ExecuteNonQuery();
                    Console.Out.WriteLine(String.Format("Number of rows inserted={0}", nRows));
                }
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```

[¿Tiene algún problema? Háganoslo saber.](https://github.com/MicrosoftDocs/azure-docs/issues)

## <a name="step-2-read-data"></a>Paso 2: Lectura de datos
Use el código siguiente para conectarse y leer los datos mediante la instrucción SQL **SELECT**. El código usa la clase NpgsqlCommand con el método:
- [()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) para establecer una conexión con PostgreSQL.
- [CreateCommand()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) y [ExecuteReader()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteReader) para ejecutar los comandos de base de datos.
- [Read()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_Read) para avanzar al registro de los resultados.
- [GetInt32()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetInt32_System_Int32_) y [GetString()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlDataReader.html#Npgsql_NpgsqlDataReader_GetString_System_Int32_) para analizar los valores del registro.

> [!IMPORTANT]
> Reemplace los parámetros host, dbname, user y password por los valores especificados al crear el servidor y la base de datos.

```csharp
using System;
using Npgsql;

namespace Driver
{
    public class AzurePostgresRead
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mydemoserver.postgres.database.azure.com";
        private static string User = "mylogin";
        private static string  DBname = "postgres";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};SSLMode=Prefer",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            using (var conn = new NpgsqlConnection(connString))
            {

                Console.Out.WriteLine("Opening connection");
                conn.Open();


                using (var command = new NpgsqlCommand("SELECT * FROM inventory", conn))
                {

                    var reader = command.ExecuteReader();
                    while (reader.Read())
                    {
                        Console.WriteLine(
                            string.Format(
                                "Reading from table=({0}, {1}, {2})",
                                reader.GetInt32(0).ToString(),
                                reader.GetString(1),
                                reader.GetInt32(2).ToString()
                                )
                            );
                    }
                    reader.Close();
                }
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}
```
[¿Tiene algún problema? Háganoslo saber.](https://github.com/MicrosoftDocs/azure-docs/issues)


## <a name="step-3-update-data"></a>Paso 3: Actualización de datos
Use el código siguiente para conectarse y actualizar los datos mediante la instrucción SQL **UPDATE**. El código usa la clase NpgsqlCommand con el método:
- [()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) para establecer una conexión con PostgreSQL.
- [CreateCommand()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand) para establecer la propiedad CommandText.
- [ExecuteNonQuery()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) para ejecutar los comandos de base de datos.

> [!IMPORTANT]
> Reemplace los parámetros host, dbname, user y password por los valores especificados al crear el servidor y la base de datos.

```csharp
using System;
using Npgsql;

namespace Driver
{
    public class AzurePostgresUpdate
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mydemoserver.postgres.database.azure.com";
        private static string User = "mylogin";
        private static string  DBname = "postgres";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};SSLMode=Prefer",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            using (var conn = new NpgsqlConnection(connString))
            {

                Console.Out.WriteLine("Opening connection");
                conn.Open();

                using (var command = new NpgsqlCommand("UPDATE inventory SET quantity = @q WHERE name = @n", conn))
                {
                    command.Parameters.AddWithValue("n", "banana");
                    command.Parameters.AddWithValue("q", 200);
                    int nRows = command.ExecuteNonQuery();
                    Console.Out.WriteLine(String.Format("Number of rows updated={0}", nRows));
                }
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}


```

[¿Tiene algún problema? Háganoslo saber.](https://github.com/MicrosoftDocs/azure-docs/issues)

## <a name="step-4-delete-data"></a>Paso 4: Eliminación de datos
Utilice el código siguiente para conectarse y eliminar los datos mediante una instrucción SQL **DELETE**.

El código usa la clase NpgsqlCommand con el método [Open()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_Open) para establecer una conexión a la base de datos PostgreSQL. A continuación, el código usa el método [CreateCommand()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlConnection.html#Npgsql_NpgsqlConnection_CreateCommand), establece la propiedad CommandText y llama al método [ExecuteNonQuery()](https://www.npgsql.org/doc/api/Npgsql.NpgsqlCommand.html#Npgsql_NpgsqlCommand_ExecuteNonQuery) para ejecutar los comandos de base de datos.

> [!IMPORTANT]
> Reemplace los parámetros host, dbname, user y password por los valores especificados al crear el servidor y la base de datos.

```csharp
using System;
using Npgsql;

namespace Driver
{
    public class AzurePostgresDelete
    {
        // Obtain connection string information from the portal
        //
        private static string Host = "mydemoserver.postgres.database.azure.com";
        private static string User = "mylogin@mydemoserver";
        private static string  DBname = "postgres";
        private static string Password = "<server_admin_password>";
        private static string Port = "5432";

        static void Main(string[] args)
        {
            // Build connection string using parameters from portal
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4};SSLMode=Prefer",
                    Host,
                    User,
                    DBname,
                    Port,
                    Password);

            using (var conn = new NpgsqlConnection(connString))
            {
                Console.Out.WriteLine("Opening connection");
                conn.Open();

                using (var command = new NpgsqlCommand("DELETE FROM inventory WHERE name = @n", conn))
                {
                    command.Parameters.AddWithValue("n", "orange");

                    int nRows = command.ExecuteNonQuery();
                    Console.Out.WriteLine(String.Format("Number of rows deleted={0}", nRows));
                }
            }

            Console.WriteLine("Press RETURN to exit");
            Console.ReadLine();
        }
    }
}

```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para limpiar todos los recursos utilizados durante esta guía de inicio rápido, elimine el grupo de recursos con el siguiente comando:

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Administración de un servidor de Azure Database for MySQL mediante Azure Portal](./how-to-manage-server-portal.md)<br/>

> [!div class="nextstepaction"]
> [Administración con la CLI de Azure de una instancia de Azure Database for MySQL: Servidor único](./how-to-manage-server-cli.md)
