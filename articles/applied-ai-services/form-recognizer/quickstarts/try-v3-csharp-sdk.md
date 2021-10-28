---
title: 'Inicio rápido: SDK de Form Recognizer para C# v3.0 | Versión preliminar'
titleSuffix: Azure Applied AI Services
description: Procesamiento de formularios y documentos, extracción de datos y análisis mediante la versión v3.0 de los SDK de biblioteca cliente de Form Recognizer para C# (versión preliminar)
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 10/07/2021
ms.author: lajanuar
recommendations: false
ms.openlocfilehash: 50c1cfbcfc79212f03fd67f783afaff110ce1e09
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2021
ms.locfileid: "130216739"
---
# <a name="quickstart-form-recognizer-c-client-library-sdks-v30--preview"></a>Inicio rápido: Versión v3.0 de los SDK de biblioteca cliente de Form Recognizer para C# | Versión preliminar

Comience a usar Azure Form Recognizer mediante el lenguaje de programación C#. Azure Form Recognizer es un servicio en la nube de [Azure Applied AI Services](../../../applied-ai-services/index.yml) que permite crear software de procesamiento de datos automatizado mediante la tecnología de aprendizaje automático. Puede usar Form Recognizer mediante la API REST o un SDK. Se recomienda usar el servicio gratuito cuando se está aprendiendo la tecnología. Recuerde que el número de páginas gratuitas se limita a 500 al mes.

>[!NOTE]
> Form Recognizer v3.0 está actualmente en versión preliminar pública. Es posible que algunas características no se admitan o que tengan funcionalidades limitadas. 

[Documentación de referencia](/dotnet/api/overview/azure/ai.formrecognizer-readme?view=azure-dotnet&preserve-view=true ) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/formrecognizer/Azure.AI.FormRecognizer/src) | [Paquete (NuGet)](https://www.nuget.org/packages/Azure.AI.FormRecognizer) | [Ejemplos](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/formrecognizer/Azure.AI.FormRecognizer/samples/README.md)

Form Recognizer de Azure Cognitive Services es un servicio en la nube que usa el aprendizaje automático para extraer y analizar campos de formulario, texto y tablas de los documentos. Puede llamar fácilmente a los modelos de Form Recognizer mediante la integración de los SDK de biblioteca cliente en los flujos de trabajo y aplicaciones.

### <a name="form-recognizer-models"></a>Modelos de Form Recognizer

El SDK para C# admite los siguientes modelos y funcionalidades:

* 🆕Documento general: analizar y extraer texto, tablas, estructura, pares clave-valor y entidades con nombre.|
* Diseño: analizar y extraer tablas, líneas, palabras y marcas de selección, como botones de radio y casillas en documentos de formularios, sin necesidad de entrenar un modelo.
* Personalizado: analizar y extraer campos de formulario y otro contenido de los formularios personalizados mediante modelos entrenados con sus propios tipos de formulario.
* Facturas: analizar y extraer los campos comunes de las facturas mediante un modelo de facturas entrenado previamente.
* Recibos: analizar y extraer los campos comunes de los recibos mediante un modelo de recibos entrenado previamente.
* Documentos de identificación: analizar y extraer campos comunes de documentos de identificación, como pasaportes o permisos de conducir, mediante un modelo de documentos de identificación entrenado previamente.
* Tarjetas de presentación: analizar y extraer campos comunes de tarjetas de presentación mediante un modelo de tarjetas de presentación previamente entrenado.

En este inicio rápido, usará las siguientes características para analizar y extraer datos y valores de formularios y documentos:

* [**Documento general**](#try-it-general-document-model)

* [**Layout**](#try-it-layout-model)

* [**Factura precompilada**](#try-it-prebuilt-invoice-model)

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/)

* Versión actual del [IDE de Visual Studio](https://visualstudio.microsoft.com/vs/) o [.NET Core](https://dotnet.microsoft.com/download).

* Un recurso de Cognitive Services o Form Recognizer. Una vez que tenga la suscripción de Azure, cree un recurso de Form Recognizer de [servicio único](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) o de [varios servicios](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) en Azure Portal para obtener la clave y el punto de conexión. Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.

> [!TIP] 
> Cree un recurso de Cognitive Services si tiene previsto acceder a varios servicios de Cognitive Services en un único punto de conexión o clave. Para acceder únicamente a Form Recognizer, cree un recurso de Form Recognizer. Tenga en cuenta que necesitará un recurso de servicio único si tiene previsto usar la [autenticación de Azure Active Directory](../../../active-directory/authentication/overview-authentication.md).

* Una vez que se implemente el recurso, haga clic en **Ir al recurso**. Necesitará la clave y el punto de conexión del recurso que ha creado para conectar la aplicación a la API de Form Recognizer. En una sección posterior de este mismo inicio rápido, pegará la clave y el punto de conexión en el código siguiente:

  :::image type="content" source="../media/containers/keys-and-endpoint.png" alt-text="Captura de pantalla: claves y ubicación del punto de conexión en Azure Portal.":::

## <a name="set-up"></a>Configurar

### <a name="option-1-net-command-line-interface-cli"></a>[Opción 1: Interfaz de la línea de comandos (CLI) de .NET](#tab/cli)

En una ventana de consola (por ejemplo, cmd, PowerShell o Bash), use el comando `dotnet new` para crear una nueva aplicación de consola con el nombre `formrecognizer-quickstart`. Este comando crea un sencillo proyecto "Hola mundo" de C# con un solo archivo de origen: *Program.cs*.

```console
dotnet new console -n formrecognizer-quickstart
```

Abra una línea de comandos y cambie al directorio que contiene el archivo del proyecto. Compile la aplicación:

```console
dotnet build
```

La salida de la compilación no debe contener advertencias ni errores.

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library-with-nuget"></a>Instalación de la biblioteca cliente con NuGet

Dentro del directorio que contiene el proyecto, instale la biblioteca cliente de Form Recognizer para .NET con el siguiente comando:

```console
dotnet add package Azure.AI.FormRecognizer
```

Esta versión de la biblioteca cliente tiene como valor predeterminado la versión 2021-09-30-preview del servicio.

### <a name="option-2-visual-studio"></a>[Opción 2: Visual Studio](#tab/vs)

1. Inicie Visual Studio 2019.

1. En la página de inicio, elija Crear un proyecto nuevo.

    :::image type="content" source="../media/quickstarts/start-window.png" alt-text="Captura de pantalla: ventana de inicio de Visual Studio.":::

1. En la página **Crear un proyecto**, escriba **consola** en el cuadro de búsqueda. Elija la plantilla **Aplicación de consola** y, a continuación, seleccione **Siguiente**.

    :::image type="content" source="../media/quickstarts/create-new-project.png" alt-text="Captura de pantalla: página Crear un proyecto de Visual Studio.":::

1. En la ventana de diálogo **Configure su nuevo proyecto**, escriba `formRecognizer_quickstart` en el cuadro Nombre de proyecto. Después, haga clic en Siguiente.

    :::image type="content" source="../media/quickstarts/configure-new-project.png" alt-text="Captura de pantalla: ventana de diálogo Configure su nuevo proyecto de Visual Studio.":::

1. En la ventana de diálogo **Información adicional**, seleccione **.NET 5.0 (Actual)** y, a continuación, seleccione **Crear**.

    :::image type="content" source="../media/quickstarts/additional-information.png" alt-text="Captura de pantalla: ventana de diálogo de información adicional de Visual Studio.":::

### <a name="install-the-client-library-with-nuget"></a>Instalación de la biblioteca cliente con NuGet

 1. Haga clic con el botón derecho en el proyecto **formRecognizer_quickstart** y seleccione **Administrar paquetes NuGet...** .

    :::image type="content" source="../media/quickstarts/select-nuget-package.png" alt-text="Captura de pantalla: select-nuget-package.png":::

 1. Seleccione la pestaña Examinar y escriba Azure.AI.FormRecognizer.

     :::image type="content" source="../media/quickstarts/azure-nuget-package.png" alt-text="Captura de pantalla: select-form-recognizer-package.png":::

 1. Seleccione la versión deseada en el menú desplegable y seleccione **Instalar**.

     Esta versión de la biblioteca cliente tiene como valor predeterminado la versión 2021-09-30-preview del servicio.

---

## <a name="build-your-application"></a>Compilación de la aplicación

Para interactuar con el servicio Form Recognizer, tiene que crear una instancia de la clase `DocumentAnalysisClient`. Para ello, creará un elemento `AzureKeyCredential` con el elemento apiKey y una instancia de `DocumentAnalysisClient` con el elemento `AzureKeyCredential` y el elemento `endpoint` de Form Recognizer.

1. Abra el archivo **Program.cs**.

1. Agregue las siguientes directivas using:

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Azure.AI.FormRecognizer.DocumentAnalysis;
    ```

1. Establezca las variables de entorno `endpoint` y `apiKey`, y cree la instancia de `AzureKeyCredential` y `DocumentAnalysisClient`:

    ```csharp
    string endpoint = "<your-endpoint>";
    string apiKey = "<your-apiKey>";
    var credential = new AzureKeyCredential(apiKey);
    var client = new DocumentAnalysisClient(new Uri(endpoint), credential);
    ```

1. Elimine la línea `Console.Writeline("Hello World!");` y agregue el código de **Pruébelo** al método **Main** en el archivo **Program.cs**:

    :::image type="content" source="../media/quickstarts/add-code-here.png" alt-text="Captura de pantalla: adición del código de ejemplo al método Main.":::

## <a name="try-it-general-document-model"></a>**Pruébelo**: modelo de documento general

> [!div class="checklist"]
>
> * Para este ejemplo, necesitará un **archivo de documento de formulario en un identificador URI**. Puede usar nuestro [documento de formulario de ejemplo](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-layout.pdf) para este inicio rápido.
> * Agregue el valor del identificador URI del archivo a la variable `string fileUri` en la parte superior del método Main.
> * Para analizar un archivo determinado en un identificador URI, utilizará el método `StartAnalyzeDocumentFromUri`. El valor devuelto es un objeto `AnalyzeResult` que contiene datos sobre el documento enviado.

### <a name="add-the-following-code-to-your-general-document-application-main-method"></a>Agregue el código siguiente al método **Main** de la aplicación de documento general.

```csharp
// sample form document
string fileUri = "https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-layout.pdf";

AnalyzeDocumentOperation operation = await client.StartAnalyzeDocumentFromUriAsync("prebuilt-document", fileUri);

await operation.WaitForCompletionAsync();

AnalyzeResult result = operation.Value;

Console.WriteLine("Detected entities:");

foreach (DocumentEntity entity in result.Entities)
{
    if (entity.SubCategory == null)
    {
        Console.WriteLine($"  Found entity '{entity.Content}' with category '{entity.Category}'.");
    }
    else
    {
        Console.WriteLine($"  Found entity '{entity.Content}' with category '{entity.Category}' and sub-category '{entity.SubCategory}'.");
    }
}

Console.WriteLine("Detected key-value pairs:");

foreach (DocumentKeyValuePair kvp in result.KeyValuePairs)
{
    if (kvp.Value.Content == null)
    {
        Console.WriteLine($"  Found key with no value: '{kvp.Key.Content}'");
    }
    else
    {
        Console.WriteLine($"  Found key-value pair: '{kvp.Key.Content}' and '{kvp.Value.Content}'");
    }
}

foreach (DocumentPage page in result.Pages)
{
    Console.WriteLine($"Document Page {page.PageNumber} has {page.Lines.Count} line(s), {page.Words.Count} word(s),");
    Console.WriteLine($"and {page.SelectionMarks.Count} selection mark(s).");

    for (int i = 0; i < page.Lines.Count; i++)
    {
        DocumentLine line = page.Lines[i];
        Console.WriteLine($"  Line {i} has content: '{line.Content}'.");

        Console.WriteLine($"    Its bounding box is:");
        Console.WriteLine($"      Upper left => X: {line.BoundingBox[0].X}, Y= {line.BoundingBox[0].Y}");
        Console.WriteLine($"      Upper right => X: {line.BoundingBox[1].X}, Y= {line.BoundingBox[1].Y}");
        Console.WriteLine($"      Lower right => X: {line.BoundingBox[2].X}, Y= {line.BoundingBox[2].Y}");
        Console.WriteLine($"      Lower left => X: {line.BoundingBox[3].X}, Y= {line.BoundingBox[3].Y}");
    }

    for (int i = 0; i < page.SelectionMarks.Count; i++)
    {
        DocumentSelectionMark selectionMark = page.SelectionMarks[i];

        Console.WriteLine($"  Selection Mark {i} is {selectionMark.State}.");
        Console.WriteLine($"    Its bounding box is:");
        Console.WriteLine($"      Upper left => X: {selectionMark.BoundingBox[0].X}, Y= {selectionMark.BoundingBox[0].Y}");
        Console.WriteLine($"      Upper right => X: {selectionMark.BoundingBox[1].X}, Y= {selectionMark.BoundingBox[1].Y}");
        Console.WriteLine($"      Lower right => X: {selectionMark.BoundingBox[2].X}, Y= {selectionMark.BoundingBox[2].Y}");
        Console.WriteLine($"      Lower left => X: {selectionMark.BoundingBox[3].X}, Y= {selectionMark.BoundingBox[3].Y}");
    }
}

foreach (DocumentStyle style in result.Styles)
{
    // Check the style and style confidence to see if text is handwritten.
    // Note that value '0.8' is used as an example.

    bool isHandwritten = style.IsHandwritten.HasValue && style.IsHandwritten == true;

    if (isHandwritten && style.Confidence > 0.8)
    {
        Console.WriteLine($"Handwritten content found:");

        foreach (DocumentSpan span in style.Spans)
        {
            Console.WriteLine($"  Content: {result.Content.Substring(span.Offset, span.Length)}");
        }
    }
}

Console.WriteLine("The following tables were extracted:");

for (int i = 0; i < result.Tables.Count; i++)
{
    DocumentTable table = result.Tables[i];
    Console.WriteLine($"  Table {i} has {table.RowCount} rows and {table.ColumnCount} columns.");

    foreach (DocumentTableCell cell in table.Cells)
    {
        Console.WriteLine($"    Cell ({cell.RowIndex}, {cell.ColumnIndex}) has kind '{cell.Kind}' and content: '{cell.Content}'.");
    }
}

```

## <a name="try-it-layout-model"></a>**Pruébelo**: modelo de diseño

Extraiga de los documentos texto, marcas de selección, estilos de texto y estructuras de tablas, junto con las coordenadas de su región delimitadora.

> [!div class="checklist"]
>
> * Para este ejemplo, necesitará un **archivo de documento de formulario en un identificador URI**. Puede usar nuestro [documento de formulario de ejemplo](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-layout.pdf) para este inicio rápido.
> * Agregue el valor del identificador URI del archivo a la variable `string fileUri` en la parte superior del método Main.
> * Para extraer el diseño de un archivo determinado en un identificador URI, use el método `StartAnalyzeDocumentFromUri` y pase `prebuilt-layout` como identificador de modelo. El valor devuelto es un objeto `AnalyzeResult` que contiene datos del documento enviado.

### <a name="add-the-following-code-to-your-layout-application-main-method"></a>Agregue el código siguiente al método **Main** de la aplicación de diseño.

```csharp
// sample form document
string fileUri = "https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-layout.pdf";

AnalyzeDocumentOperation operation = await client.StartAnalyzeDocumentFromUriAsync("prebuilt-layout", fileUri);

await operation.WaitForCompletionAsync();

AnalyzeResult result = operation.Value;

foreach (DocumentPage page in result.Pages)
{
    Console.WriteLine($"Document Page {page.PageNumber} has {page.Lines.Count} line(s), {page.Words.Count} word(s),");
    Console.WriteLine($"and {page.SelectionMarks.Count} selection mark(s).");

    for (int i = 0; i < page.Lines.Count; i++)
    {
        DocumentLine line = page.Lines[i];
        Console.WriteLine($"  Line {i} has content: '{line.Content}'.");

        Console.WriteLine($"    Its bounding box is:");
        Console.WriteLine($"      Upper left => X: {line.BoundingBox[0].X}, Y= {line.BoundingBox[0].Y}");
        Console.WriteLine($"      Upper right => X: {line.BoundingBox[1].X}, Y= {line.BoundingBox[1].Y}");
        Console.WriteLine($"      Lower right => X: {line.BoundingBox[2].X}, Y= {line.BoundingBox[2].Y}");
        Console.WriteLine($"      Lower left => X: {line.BoundingBox[3].X}, Y= {line.BoundingBox[3].Y}");
    }

    for (int i = 0; i < page.SelectionMarks.Count; i++)
    {
        DocumentSelectionMark selectionMark = page.SelectionMarks[i];

        Console.WriteLine($"  Selection Mark {i} is {selectionMark.State}.");
        Console.WriteLine($"    Its bounding box is:");
        Console.WriteLine($"      Upper left => X: {selectionMark.BoundingBox[0].X}, Y= {selectionMark.BoundingBox[0].Y}");
        Console.WriteLine($"      Upper right => X: {selectionMark.BoundingBox[1].X}, Y= {selectionMark.BoundingBox[1].Y}");
        Console.WriteLine($"      Lower right => X: {selectionMark.BoundingBox[2].X}, Y= {selectionMark.BoundingBox[2].Y}");
        Console.WriteLine($"      Lower left => X: {selectionMark.BoundingBox[3].X}, Y= {selectionMark.BoundingBox[3].Y}");
    }
}

foreach (DocumentStyle style in result.Styles)
{
    // Check the style and style confidence to see if text is handwritten.
    // Note that value '0.8' is used as an example.

    bool isHandwritten = style.IsHandwritten.HasValue && style.IsHandwritten == true;

    if (isHandwritten && style.Confidence > 0.8)
    {
        Console.WriteLine($"Handwritten content found:");

        foreach (DocumentSpan span in style.Spans)
        {
            Console.WriteLine($"  Content: {result.Content.Substring(span.Offset, span.Length)}");
        }
    }
}

Console.WriteLine("The following tables were extracted:");

for (int i = 0; i < result.Tables.Count; i++)
{
    DocumentTable table = result.Tables[i];
    Console.WriteLine($"  Table {i} has {table.RowCount} rows and {table.ColumnCount} columns.");

    foreach (DocumentTableCell cell in table.Cells)
    {
        Console.WriteLine($"    Cell ({cell.RowIndex}, {cell.ColumnIndex}) has kind '{cell.Kind}' and content: '{cell.Content}'.");
    }
}

```

## <a name="try-it-prebuilt-invoice-model"></a>**Pruébelo**: modelo precompilado de factura

En este ejemplo se muestra cómo analizar datos de determinados tipos de documentos comunes con modelos entrenados previamente, utilizando una factura como ejemplo.

> [!div class="checklist"]
>
> * Para este ejemplo, necesitará un **archivo de documento de factura en un identificador URI**. Puede usar nuestro [documento de factura de ejemplo](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-invoice.pdf) para este inicio rápido.
> * Agregue el valor del identificador URI del archivo a la variable `string fileUri` en la parte superior del método Main.
> * Para analizar un archivo determinado en un identificador URI, use el método `StartAnalyzeDocumentFromUri` y pase `prebuilt-invoice` como identificador de modelo. El valor devuelto es un objeto `AnalyzeResult` que contiene datos del documento enviado.

### <a name="choose-the-invoice-prebuilt-model-id"></a>Elección del identificador de modelo precompilado de factura

No está limitado a las facturas: hay varios modelos precompilados entre los que elegir, cada uno de los cuales tiene su propio conjunto de campos admitidos. El modelo que se va a usar para la operación de análisis depende del tipo de documento que se va a analizar. Estos son los identificadores de modelo de los modelos precompilados admitidos actualmente por el servicio Form Recognizer:

* **prebuilt-invoice**: extrae texto, marcas de selección, tablas, pares clave-valor y la información principal de las facturas.
* **prebuilt-businessCard**: extrae el texto y la información principal de las tarjetas de presentación.
* **prebuilt-idDocument**: extrae el texto y la información principal de permisos de conducir y pasaportes internacionales.
* **prebuilt-receipt**: extrae el texto y la información principal de los recibos.

### <a name="add-the-following-code-to-your-prebuilt-invoice-application-main-method"></a>Agregue el código siguiente al método **Main** de la aplicación de factura pregenerada.

```csharp
// sample invoice document
string filePath = "(https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-invoice.pdf";

using var stream = new FileStream(filePath, FileMode.Open);

AnalyzeDocumentOperation operation = await client.StartAnalyzeDocumentAsync("prebuilt-invoice", stream);

await operation.WaitForCompletionAsync();

AnalyzeResult result = operation.Value;

for (int i = 0; i < result.Documents.Count; i++)
{
    Console.WriteLine($"Document {i}:");

    AnalyzedDocument document = result.Documents[i];

    if (document.Fields.TryGetValue("VendorName", out DocumentField vendorNameField))
    {
        if (vendorNameField.ValueType == DocumentFieldType.String)
        {
            string vendorName = vendorNameField.AsString();
            Console.WriteLine($"Vendor Name: '{vendorName}', with confidence {vendorNameField.Confidence}");
        }
    }

    if (document.Fields.TryGetValue("CustomerName", out DocumentField customerNameField))
    {
        if (customerNameField.ValueType == DocumentFieldType.String)
        {
            string customerName = customerNameField.AsString();
            Console.WriteLine($"Customer Name: '{customerName}', with confidence {customerNameField.Confidence}");
        }
    }

    if (document.Fields.TryGetValue("Items", out DocumentField itemsField))
    {
        if (itemsField.ValueType == DocumentFieldType.List)
        {
            foreach (DocumentField itemField in itemsField.AsList())
            {
                Console.WriteLine("Item:");

                if (itemField.ValueType == DocumentFieldType.Dictionary)
                {
                    IReadOnlyDictionary<string, DocumentField> itemFields = itemField.AsDictionary();

                    if (itemFields.TryGetValue("Description", out DocumentField itemDescriptionField))
                    {
                        if (itemDescriptionField.ValueType == DocumentFieldType.String)
                        {
                            string itemDescription = itemDescriptionField.AsString();

                            Console.WriteLine($"  Description: '{itemDescription}', with confidence {itemDescriptionField.Confidence}");
                        }
                    }

                    if (itemFields.TryGetValue("Amount", out DocumentField itemAmountField))
                    {
                        if (itemAmountField.ValueType == DocumentFieldType.Double)
                        {
                            double itemAmount = itemAmountField.AsDouble();

                            Console.WriteLine($"  Amount: '{itemAmount}', with confidence {itemAmountField.Confidence}");
                        }
                    }
                }
            }
        }
    }

    if (document.Fields.TryGetValue("SubTotal", out DocumentField subTotalField))
    {
        if (subTotalField.ValueType == DocumentFieldType.Double)
        {
            double subTotal = subTotalField.AsDouble();
            Console.WriteLine($"Sub Total: '{subTotal}', with confidence {subTotalField.Confidence}");
        }
    }

    if (document.Fields.TryGetValue("TotalTax", out DocumentField totalTaxField))
    {
        if (totalTaxField.ValueType == DocumentFieldType.Double)
        {
            double totalTax = totalTaxField.AsDouble();
            Console.WriteLine($"Total Tax: '{totalTax}', with confidence {totalTaxField.Confidence}");
        }
    }

    if (document.Fields.TryGetValue("InvoiceTotal", out DocumentField invoiceTotalField))
    {
        if (invoiceTotalField.ValueType == DocumentFieldType.Double)
        {
            double invoiceTotal = invoiceTotalField.AsDouble();
            Console.WriteLine($"Invoice Total: '{invoiceTotal}', with confidence {invoiceTotalField.Confidence}");
        }
    }
}

```

## <a name="run-your-application"></a>Ejecución de la aplicación

### <a name="net-command-line-interface-cli"></a>[Interfaz de la línea de comandos (CLI) de .NET](#tab/cli)

Abra el símbolo del sistema, vaya al directorio que contiene el proyecto y escriba lo siguiente:

```console
dotnet run formrecognizer-quickstart.dll
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

Elija el botón verde **Inicio** situado junto a formRecognizer_quickstart para compilar y ejecutar el programa, o pulse **F5**.

  :::image type="content" source="../media/quickstarts/run-visual-studio.png" alt-text="Captura de pantalla: ejecución del programa en Visual Studio.":::

---

¡Enhorabuena! En este inicio rápido, ha usado el SDK de Form Recognizer para C# para analizar varios formularios y documentos de maneras diferentes. A continuación, explore la documentación de referencia para encontrar más información sobre la API de Form Recognizer.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Documentación de referencia de la versión v3.0 de la API REST](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)