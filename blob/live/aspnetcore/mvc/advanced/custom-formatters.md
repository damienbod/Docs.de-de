---
title: Benutzerdefinierten Formatierer in ASP.NET Core MVC-Web-APIs
author: tdykstra
description: "Informationen Sie zum Erstellen und Verwenden von benutzerdefinierten Formatierer für Web-APIs in ASP.NET Core."
keywords: ASP.NET Core, web-api, benutzerdefinierten Formatierer
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 5e665abe10fd7444c3fd5f20cfeca3ef0a5f79d3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a><span data-ttu-id="f5fd0-104">Benutzerdefinierten Formatierer in ASP.NET Core MVC-Web-APIs</span><span class="sxs-lookup"><span data-stu-id="f5fd0-104">Custom formatters in ASP.NET Core MVC web APIs</span></span>

<span data-ttu-id="f5fd0-105">Durch [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f5fd0-105">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="f5fd0-106">ASP.NET Core MVC verfügt über integrierte Unterstützung für den Datenaustausch im Web-APIs mit JSON, XML oder nur-Text-Formate.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-106">ASP.NET Core MVC has built-in support for data exchange in web APIs by using JSON, XML, or plain text formats.</span></span> <span data-ttu-id="f5fd0-107">In diesem Artikel wird die Unterstützung für zusätzliche Formate hinzufügen, durch das Erstellen von benutzerdefinierten Formatierer veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-107">This article shows how to add support for additional formats by creating custom formatters.</span></span>

<span data-ttu-id="f5fd0-108">[Anzeigen oder Herunterladen des Beispiels aus GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span><span class="sxs-lookup"><span data-stu-id="f5fd0-108">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample).</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="f5fd0-109">Verwenden von benutzerdefinierten Formatierer</span><span class="sxs-lookup"><span data-stu-id="f5fd0-109">When to use custom formatters</span></span>

<span data-ttu-id="f5fd0-110">Verwenden Sie ein benutzerdefiniertes Formatierungsprogramm aus, wenn Sie möchten die [content-Aushandlung](xref:mvc/models/formatting) Prozess, um einen Inhaltstyp zu unterstützen, die von den integrierten Formatierungsprogrammen (JSON, XML und nur-Text) nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-110">Use a custom formatter when you want the [content negotiation](xref:mvc/models/formatting) process to support a content type that isn't supported by the built-in formatters (JSON, XML, and plain text).</span></span>

<span data-ttu-id="f5fd0-111">Wenn einige Clients für Ihre Web-API verarbeiten kann z. B. die [Protobuf](https://github.com/google/protobuf) Format, sollten Sie die Protobuf mit diesen Clients zu verwenden, da sie effizienter ist.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-111">For example, if some of the clients for your web API can handle the [Protobuf](https://github.com/google/protobuf) format, you might want to use Protobuf with those clients because it's more efficient.</span></span>  <span data-ttu-id="f5fd0-112">Vielleicht möchten Sie Ihre Web-API zum Senden von Kontaktnamen und Adressen in [vCard](https://wikipedia.org/wiki/VCard) Format, eine häufig verwendete Format zum Austauschen von Daten.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-112">Or you might want your web API to send contact names and addresses in [vCard](https://wikipedia.org/wiki/VCard) format, a commonly used format for exchanging contact data.</span></span> <span data-ttu-id="f5fd0-113">Die Beispiel-app, die mit diesem Artikel bereitgestellte implementiert einen einfachen vCard-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-113">The sample app provided with this article implements a simple vCard formatter.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="f5fd0-114">Übersicht darüber, wie ein benutzerdefiniertes Formatierungsprogramm verwenden</span><span class="sxs-lookup"><span data-stu-id="f5fd0-114">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="f5fd0-115">Es folgen die Schritte zum Erstellen und verwenden ein benutzerdefiniertes Formatierungsprogramm:</span><span class="sxs-lookup"><span data-stu-id="f5fd0-115">Here are the steps to create and use a custom formatter:</span></span>

* <span data-ttu-id="f5fd0-116">Erstellen Sie eine Ausgabe Formatierer-Klasse, wenn Serialisieren von Daten, die an den Client gesendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-116">Create an output formatter class if you want to serialize data to send to the client.</span></span>
* <span data-ttu-id="f5fd0-117">Erstellen Sie eine Eingabe Formatierungsprogrammklasse, wenn beim Deserialisieren von Daten, die vom Client empfangen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-117">Create an input formatter class if you want to deserialize data received from the client.</span></span> 
* <span data-ttu-id="f5fd0-118">Fügen Sie Instanzen der Formatierer den `InputFormatters` und `OutputFormatters` Auflistungen in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span><span class="sxs-lookup"><span data-stu-id="f5fd0-118">Add instances of your formatters to the `InputFormatters` and `OutputFormatters` collections in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).</span></span>

<span data-ttu-id="f5fd0-119">Die folgenden Abschnitte enthalten Anleitungen und Codebeispiele für jede der folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-119">The following sections provide guidance and code examples for each of these steps.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="f5fd0-120">Vorgehensweise: erstellen eine benutzerdefinierten Formatierungsklasse</span><span class="sxs-lookup"><span data-stu-id="f5fd0-120">How to create a custom formatter class</span></span>

<span data-ttu-id="f5fd0-121">So erstellen Sie einen Formatierer</span><span class="sxs-lookup"><span data-stu-id="f5fd0-121">To create a formatter:</span></span>

* <span data-ttu-id="f5fd0-122">Leiten Sie Klasse aus der entsprechenden Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-122">Derive the class from the appropriate base class.</span></span>
* <span data-ttu-id="f5fd0-123">Geben Sie gültige Medientypen und Codierungen im Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-123">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="f5fd0-124">Überschreiben Sie `CanReadType` / `CanWriteType` Methoden</span><span class="sxs-lookup"><span data-stu-id="f5fd0-124">Override `CanReadType`/`CanWriteType` methods</span></span>
* <span data-ttu-id="f5fd0-125">Überschreiben Sie `ReadRequestBodyAsync` / `WriteResponseBodyAsync` Methoden</span><span class="sxs-lookup"><span data-stu-id="f5fd0-125">Override `ReadRequestBodyAsync`/`WriteResponseBodyAsync` methods</span></span>
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="f5fd0-126">Von der entsprechenden Basisklasse abgeleitet werden</span><span class="sxs-lookup"><span data-stu-id="f5fd0-126">Derive from the appropriate base class</span></span>

<span data-ttu-id="f5fd0-127">Text-Medientypen (z. B. vCard), leiten Sie von der [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) oder [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-127">For text media types (for example, vCard), derive from the [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) or [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) base class.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

<span data-ttu-id="f5fd0-128">Leiten Sie für binäre Typen aus den [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) oder [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-128">For binary types, derive from the [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) or [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="f5fd0-129">Geben Sie gültige Medientypen und Codierungen</span><span class="sxs-lookup"><span data-stu-id="f5fd0-129">Specify valid media types and encodings</span></span>

<span data-ttu-id="f5fd0-130">Geben Sie im Konstruktor gültige Medientypen und Codierungen durch Hinzufügen der `SupportedMediaTypes` und `SupportedEncodings` Sammlungen.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-130">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> <span data-ttu-id="f5fd0-131">Abhängigkeitsinjektion in einer Formatierungsklasse Konstruktor ist nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-131">You can't do constructor dependency injection in a formatter class.</span></span> <span data-ttu-id="f5fd0-132">Beispielsweise kann keine Protokollierung durch Hinzufügen eines Parameters für die Protokollierung an den Konstruktor nicht abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-132">For example, you can't get a logger by adding a logger parameter to the constructor.</span></span> <span data-ttu-id="f5fd0-133">Um auf Dienste zuzugreifen, müssen Sie das Context-Objekt verwenden, das in Ihrer Methoden übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-133">To access services, you have to use the context object that gets passed in to your methods.</span></span> <span data-ttu-id="f5fd0-134">Ein Codebeispiel hierfür [unten](#read-write) veranschaulicht dies.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-134">A code example [below](#read-write) shows how to do this.</span></span>

### <a name="override-canreadtypecanwritetype"></a><span data-ttu-id="f5fd0-135">Überschreiben Sie CanReadType/CanWriteType</span><span class="sxs-lookup"><span data-stu-id="f5fd0-135">Override CanReadType/CanWriteType</span></span> 

<span data-ttu-id="f5fd0-136">Geben Sie den können in deserialisiert oder Serialisieren von durch Überschreiben der `CanReadType` oder `CanWriteType` Methoden.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-136">Specify the type you can deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="f5fd0-137">Beispielsweise nur möglicherweise können zum Erstellen von vCard Text aus einer `Contact` Typ und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-137">For example, you might only be able to create vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="f5fd0-138">Die CanWriteResult-Methode</span><span class="sxs-lookup"><span data-stu-id="f5fd0-138">The CanWriteResult method</span></span>

<span data-ttu-id="f5fd0-139">In einigen Szenarien müssen Sie außer Kraft setzen `CanWriteResult` anstelle von `CanWriteType`.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-139">In some scenarios you have to override `CanWriteResult` instead of `CanWriteType`.</span></span> <span data-ttu-id="f5fd0-140">Verwendung `CanWriteResult` wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="f5fd0-140">Use `CanWriteResult` if the following conditions are true:</span></span>

  * <span data-ttu-id="f5fd0-141">Die Aktionsmethode gibt eine Modellklasse zurück.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-141">Your action method returns a model class.</span></span>
  * <span data-ttu-id="f5fd0-142">Es sind abgeleitete Klassen, die zur Laufzeit zurückgegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-142">There are derived classes which might be returned at runtime.</span></span>
  * <span data-ttu-id="f5fd0-143">Sie müssen wissen zur Laufzeit die abgeleitete Klasse wird von der Aktion zurückgegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-143">You need to know at runtime which derived class was returned by the action.</span></span>  

<span data-ttu-id="f5fd0-144">Nehmen wir beispielsweise an Ihre Aktion Methodensignatur gibt eine `Person` aufweisen, aber es gelegten eine `Student` oder `Instructor` aus abgeleiteter Typ `Person`.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-144">For example, suppose your action method signature returns a `Person` type, but it may return a `Student` or `Instructor` type that derives from `Person`.</span></span> <span data-ttu-id="f5fd0-145">Wenn der Formatierer, nur behandeln soll `Student` Objekte, überprüfen Sie den Typ des [Objekt](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in das bereitgestellte Kontextobjekt der `CanWriteResult` Methode.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-145">If you want your formatter to handle only `Student` objects, check the type of [Object](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="f5fd0-146">Beachten Sie, dass es nicht notwendig, verwenden Sie `CanWriteResult` bei Rückgabe der Aktionsmethode `IActionResult`; in diesem Fall die `CanWriteType` Methode empfängt den Laufzeittyp.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-146">Note that it's not necessary to use `CanWriteResult` when the action method returns `IActionResult`; in that case, the `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a><span data-ttu-id="f5fd0-147">Überschreiben Sie ReadRequestBodyAsync/WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="f5fd0-147">Override ReadRequestBodyAsync/WriteResponseBodyAsync</span></span> 

<span data-ttu-id="f5fd0-148">Führen Sie den eigentlichen deserialisiert oder Serialisieren `ReadRequestBodyAsync` oder `WriteResponseBodyAsync`.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-148">You do the actual work of deserializing or serializing in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span>  <span data-ttu-id="f5fd0-149">Die hervorgehobenen Zeilen im folgenden Beispiel werden veranschaulicht Services von der abhängigkeitseinschleusungscontainer abrufen (Sie können nicht aus Konstruktorparameter sie erhalten).</span><span class="sxs-lookup"><span data-stu-id="f5fd0-149">The highlighted lines in the following example show how to get services from the dependency injection container (you can't get them from constructor parameters).</span></span>

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="f5fd0-150">Konfigurieren von MVC, um ein benutzerdefiniertes Formatierungsprogramm verwenden</span><span class="sxs-lookup"><span data-stu-id="f5fd0-150">How to configure MVC to use a custom formatter</span></span>
 
<span data-ttu-id="f5fd0-151">Um ein benutzerdefiniertes Formatierungsprogramm verwenden, fügen Sie eine Instanz der Klasse Formatierer in die `InputFormatters` oder `OutputFormatters` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-151">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

<span data-ttu-id="f5fd0-152">Formatierungsprogramme werden ausgewertet, in der Reihenfolge an, dass Sie sie einfügen.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-152">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="f5fd0-153">Die erste Vorlage hat Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-153">The first one takes precedence.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f5fd0-154">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="f5fd0-154">Next steps</span></span>

<span data-ttu-id="f5fd0-155">Finden Sie unter der [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), implementiert einfache vCard Eingabe und Ausgabe-Formatierer.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-155">See the [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample), which implements simple vCard input and output formatters.</span></span>  <span data-ttu-id="f5fd0-156">Die Anwendung liest und schreibt vCards, die wie im folgenden Beispiel aussehen:</span><span class="sxs-lookup"><span data-stu-id="f5fd0-156">The application reads and writes vCards that look like the following example:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

<span data-ttu-id="f5fd0-157">Sehen vCard auszugeben, führen Sie die Anwendung und senden eine Get-Anforderung mit Accept-Header "Text/Vcard" `http://localhost:63313/api/contacts/` (Wenn in Visual Studio ausgeführt wird) oder `http://localhost:5000/api/contacts/` (wenn von der Befehlszeile aus ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="f5fd0-157">To see vCard output, run the application and send a Get request with Accept header "text/vcard" to `http://localhost:63313/api/contacts/` (when running from Visual Studio) or `http://localhost:5000/api/contacts/` (when running from the command line).</span></span>

<span data-ttu-id="f5fd0-158">Um die in-Memory-Sammlung von Kontakten vCard hinzugefügt haben, senden Sie eine Post-Anforderung zur gleichen URL, mit der Content-Type-Header "Text/Vcard" und vCard Text im Hauptteil, wie im Beispiel oben formatiert.</span><span class="sxs-lookup"><span data-stu-id="f5fd0-158">To add a vCard to the in-memory collection of contacts, send a Post request to the same URL, with Content-Type header "text/vcard" and with vCard text in the body, formatted like the example above.</span></span>