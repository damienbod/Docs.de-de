---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "Aktivieren der Ablaufverfolgung für SignalR | Microsoft Docs"
author: tfitzmac
description: "Dieses Dokument beschreibt das Aktivieren und Konfigurieren der Ablaufverfolgung für SignalR-Server und Clients. Ablaufverfolgung können Sie Diagnoseinformationen zu Ereignissen anzeigen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 2f01ab5d66e44cd82634f1b3df1ca6c78b7fd9d5
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2017
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="0d980-104">Aktivieren der Ablaufverfolgung für SignalR</span><span class="sxs-lookup"><span data-stu-id="0d980-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="0d980-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0d980-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0d980-106">Dieses Dokument beschreibt das Aktivieren und Konfigurieren der Ablaufverfolgung für SignalR-Server und Clients.</span><span class="sxs-lookup"><span data-stu-id="0d980-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="0d980-107">Ablaufverfolgung können Sie Diagnoseinformationen zu Ereignissen in der SignalR-Anwendung anzeigen.</span><span class="sxs-lookup"><span data-stu-id="0d980-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="0d980-108">Dieses Thema wurde ursprünglich von Patrick Fletcher geschrieben.</span><span class="sxs-lookup"><span data-stu-id="0d980-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0d980-109">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="0d980-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="0d980-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="0d980-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="0d980-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="0d980-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="0d980-112">SignalR Version 2</span><span class="sxs-lookup"><span data-stu-id="0d980-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="0d980-113">Fragen und Kommentare</span><span class="sxs-lookup"><span data-stu-id="0d980-113">Questions and comments</span></span>
> 
> <span data-ttu-id="0d980-114">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können.</span><span class="sxs-lookup"><span data-stu-id="0d980-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="0d980-115">Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="0d980-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="0d980-116">Wenn Ablaufverfolgung aktiviert ist, erstellt eine Anwendung SignalR Protokolleinträge für Ereignisse an.</span><span class="sxs-lookup"><span data-stu-id="0d980-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="0d980-117">Sie können vom Client und dem Server protokolliert werden.</span><span class="sxs-lookup"><span data-stu-id="0d980-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="0d980-118">Auf die Serververbindung für die Protokolle, Scaleout-Anbieter und Bus nachrichtenereignisse verfolgen.</span><span class="sxs-lookup"><span data-stu-id="0d980-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="0d980-119">Auf dem Client Protokolle Verbindungsereignisse Tracing.</span><span class="sxs-lookup"><span data-stu-id="0d980-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="0d980-120">In SignalR 2.1 und höher protokolliert Ablaufverfolgung auf dem Client den vollständigen Inhalt von Nachrichten für Hub-Aufruf.</span><span class="sxs-lookup"><span data-stu-id="0d980-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="0d980-121">Inhalt</span><span class="sxs-lookup"><span data-stu-id="0d980-121">Contents</span></span>

- [<span data-ttu-id="0d980-122">Aktivieren der Ablaufverfolgung auf dem server</span><span class="sxs-lookup"><span data-stu-id="0d980-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="0d980-123">Protokollierung Serverereignisse in Textdateien</span><span class="sxs-lookup"><span data-stu-id="0d980-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="0d980-124">Serverereignisse in das Ereignisprotokoll zu protokollieren.</span><span class="sxs-lookup"><span data-stu-id="0d980-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="0d980-125">Aktivieren der Ablaufverfolgung in der .NET Client (Windows-Desktop-apps)</span><span class="sxs-lookup"><span data-stu-id="0d980-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="0d980-126">Protokollieren von Ereignissen an die Konsole Desktopclient</span><span class="sxs-lookup"><span data-stu-id="0d980-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="0d980-127">Protokollieren von Ereignissen in eine Textdatei Desktopclient</span><span class="sxs-lookup"><span data-stu-id="0d980-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="0d980-128">Aktivieren der Ablaufverfolgung in Windows Phone 8-clients</span><span class="sxs-lookup"><span data-stu-id="0d980-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="0d980-129">Windows Phone-Client-Ereignisse zu protokollieren, auf die Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="0d980-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="0d980-130">Windows Phone-Client-Ereignisse zu protokollieren, auf der Debugkonsole</span><span class="sxs-lookup"><span data-stu-id="0d980-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="0d980-131">Aktivieren der Ablaufverfolgung in der JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="0d980-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="0d980-132">Aktivieren der Ablaufverfolgung auf dem server</span><span class="sxs-lookup"><span data-stu-id="0d980-132">Enabling tracing on the server</span></span>

<span data-ttu-id="0d980-133">Sie aktivieren die Protokollierung auf dem Server innerhalb der Anwendung-Konfigurationsdatei ("App.config" oder "Web.config", die vom Projekttyp abhängig.) Sie geben die Kategorien der Ereignisse protokolliert werden soll.</span><span class="sxs-lookup"><span data-stu-id="0d980-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="0d980-134">In der Konfigurationsdatei Sie auch angeben, ob die Ereignisse zu protokollieren, um eine Textdatei, die Windows-Ereignisprotokoll oder ein benutzerdefiniertes Protokoll, das mit einer Implementierung der [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="0d980-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="0d980-135">Die Ereigniskategorien Server umfassen die folgenden Arten von Nachrichten:</span><span class="sxs-lookup"><span data-stu-id="0d980-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="0d980-136">Quelle</span><span class="sxs-lookup"><span data-stu-id="0d980-136">Source</span></span> | <span data-ttu-id="0d980-137">Mitteilungen</span><span class="sxs-lookup"><span data-stu-id="0d980-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="0d980-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="0d980-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="0d980-139">Anbieter-Setup für SQL-Nachrichtenbus mit horizontaler Skalierung, Datenbankvorgang, Fehler und Timeout-Ereignisse</span><span class="sxs-lookup"><span data-stu-id="0d980-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="0d980-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="0d980-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="0d980-141">Servicebus-Warteschlangen für horizontale Skalierung Anbieter Topic erstellen und Abonnements, Fehler und messagingereignisse</span><span class="sxs-lookup"><span data-stu-id="0d980-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="0d980-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="0d980-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="0d980-143">Redis-Verbindung trennen und Fehler Anbieterereignisse des mit horizontaler Skalierung</span><span class="sxs-lookup"><span data-stu-id="0d980-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="0d980-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="0d980-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="0d980-145">Messagingereignisse mit horizontaler Skalierung</span><span class="sxs-lookup"><span data-stu-id="0d980-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="0d980-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="0d980-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="0d980-147">WebSocket-Transport-Verbindung trennen, messaging und Fehler-Ereignisse</span><span class="sxs-lookup"><span data-stu-id="0d980-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="0d980-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="0d980-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="0d980-149">ServerSentEvents transport Verbindung, trennen, messaging und Fehlerereignisse</span><span class="sxs-lookup"><span data-stu-id="0d980-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="0d980-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="0d980-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="0d980-151">ForeverFrame Transport-Verbindung trennen, messaging und Fehler-Ereignisse</span><span class="sxs-lookup"><span data-stu-id="0d980-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="0d980-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="0d980-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="0d980-153">LongPolling Transport-Verbindung trennen, messaging und Fehler-Ereignisse</span><span class="sxs-lookup"><span data-stu-id="0d980-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="0d980-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="0d980-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="0d980-155">Übertragen Sie die Verbindung trennen und Keepalive-Ereignisse</span><span class="sxs-lookup"><span data-stu-id="0d980-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="0d980-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="0d980-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="0d980-157">Hub-Ermittlungsereignisse</span><span class="sxs-lookup"><span data-stu-id="0d980-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="0d980-158">Protokollierung Serverereignisse in Textdateien</span><span class="sxs-lookup"><span data-stu-id="0d980-158">Logging server events to text files</span></span>

<span data-ttu-id="0d980-159">Der folgende Code zeigt, wie Sie zum Aktivieren der Ablaufverfolgung für jede Kategorie des Ereignisses.</span><span class="sxs-lookup"><span data-stu-id="0d980-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="0d980-160">In diesem Beispiel konfiguriert die Anwendung zum Protokollieren von Ereignissen in Textdateien.</span><span class="sxs-lookup"><span data-stu-id="0d980-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="0d980-161">**XML-Server-Code für das Aktivieren der Ablaufverfolgung**</span><span class="sxs-lookup"><span data-stu-id="0d980-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="0d980-162">Im obigen Code die `SignalRSwitch` Eintrag gibt an, die [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) für Ereignisse, die gesendet, um das angegebene Protokoll verwendet.</span><span class="sxs-lookup"><span data-stu-id="0d980-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="0d980-163">In diesem Fall auf festgelegt `Verbose` also alle Debug- und Ablaufverfolgungsmeldungen werden protokolliert.</span><span class="sxs-lookup"><span data-stu-id="0d980-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="0d980-164">Die folgende Ausgabe zeigt Einträge aus der `transports.log.txt` -Datei für eine Anwendung mithilfe der oben genannten Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="0d980-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="0d980-165">Es zeigt eine neue Verbindung, einer entfernten Verbindungs- und frequenzermittlungsereignisse Transport.</span><span class="sxs-lookup"><span data-stu-id="0d980-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="0d980-166">Serverereignisse in das Ereignisprotokoll zu protokollieren.</span><span class="sxs-lookup"><span data-stu-id="0d980-166">Logging server events to the event log</span></span>

<span data-ttu-id="0d980-167">Um die Ereignisse im Ereignisprotokoll anstelle einer Textdatei protokollieren möchten, ändern Sie die Werte für die Einträge in der `sharedListeners` Knoten.</span><span class="sxs-lookup"><span data-stu-id="0d980-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="0d980-168">Der folgende Code zeigt, wie Sie Serverereignisse in das Ereignisprotokoll zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="0d980-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="0d980-169">**XML-Servercode zum Protokollieren von Ereignissen im Ereignisprotokoll**</span><span class="sxs-lookup"><span data-stu-id="0d980-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="0d980-170">Die Ereignisse werden im Windows-Anwendungsprotokoll protokolliert und sind über die Ereignisanzeige zur Verfügung, wie unten dargestellt:</span><span class="sxs-lookup"><span data-stu-id="0d980-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Ereignisanzeige mit SignalR-Protokolle](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="0d980-172">Wenn Sie das Ereignisprotokoll verwenden, legen Sie die **TraceLevel** auf **Fehler** auf die Anzahl der Nachrichten überschaubar zu halten.</span><span class="sxs-lookup"><span data-stu-id="0d980-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="0d980-173">Aktivieren der Ablaufverfolgung in der .NET Client (Windows-Desktop-apps)</span><span class="sxs-lookup"><span data-stu-id="0d980-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="0d980-174">Der .NET Client kann Ereignisse protokollieren, an die Konsole, eine Textdatei oder in ein benutzerdefiniertes Protokoll, das mit einer Implementierung der ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="0d980-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="0d980-175">Legen Sie zum Aktivieren der Protokollierung in der .NET Client der Verbindungs `TraceLevel` Eigenschaft, um eine [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) Wert, und die `TraceWriter` -Eigenschaft eine gültige ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) Instanz.</span><span class="sxs-lookup"><span data-stu-id="0d980-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="0d980-176">Protokollieren von Ereignissen an die Konsole Desktopclient</span><span class="sxs-lookup"><span data-stu-id="0d980-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="0d980-177">Der folgende C#-Code zeigt, wie Ereignisse in der .NET Client an die Konsole zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="0d980-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="0d980-178">Protokollieren von Ereignissen in eine Textdatei Desktopclient</span><span class="sxs-lookup"><span data-stu-id="0d980-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="0d980-179">Der folgende C#-Code zeigt, wie Ereignisse in der .NET Client in eine Textdatei zu protokollieren:</span><span class="sxs-lookup"><span data-stu-id="0d980-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="0d980-180">Die folgende Ausgabe zeigt Einträge aus der `ClientLog.txt` -Datei für eine Anwendung mithilfe der oben genannten Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="0d980-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="0d980-181">Es zeigt, dass des Clients eine Verbindung mit dem Server und der Hub Aufrufen einer Clientmethode wird aufgerufen, `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="0d980-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="0d980-182">Aktivieren der Ablaufverfolgung in Windows Phone 8-clients</span><span class="sxs-lookup"><span data-stu-id="0d980-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="0d980-183">SignalR-Anwendungen für Windows Phone-apps verwenden den gleichen .NET Client als desktop-apps, aber [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) der und Schreiben in eine Datei mit [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) sind nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0d980-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="0d980-184">Stattdessen müssen Sie erstellen eine benutzerdefinierte Implementierung von ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) für die Ablaufverfolgung.</span><span class="sxs-lookup"><span data-stu-id="0d980-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="0d980-185">Windows Phone-Client-Ereignisse zu protokollieren, auf die Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="0d980-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="0d980-186">Die [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) enthält ein Beispiel für Windows Phone, die Ablaufverfolgungsausgabe an schreibt eine [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) mithilfe einer benutzerdefinierten ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) -Implementierung mit der Bezeichnung `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="0d980-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="0d980-187">Diese Klasse finden Sie in der **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** Projekt.</span><span class="sxs-lookup"><span data-stu-id="0d980-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="0d980-188">Beim Erstellen einer Instanz von `TextBlockWriter`, übergeben Sie in der aktuellen [SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx), und ein [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) , in dem er erstellt eine [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) für Ablaufverfolgung Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="0d980-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="0d980-189">Die Trace-Ausgabe wird dann in ein neues geschrieben werden [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) erstellt, der [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) im übergeben:</span><span class="sxs-lookup"><span data-stu-id="0d980-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="0d980-190">Windows Phone-Client-Ereignisse zu protokollieren, auf der Debugkonsole</span><span class="sxs-lookup"><span data-stu-id="0d980-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="0d980-191">Zum Senden der Ausgabe auf der Debugkonsole statt der Benutzeroberfläche, erstellen Sie eine Implementierung von ["TextWriter"](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) , die schreibt in das Debug-Fenster, und weisen Sie sie für Ihre Verbindungs [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="0d980-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="0d980-192">Ablaufverfolgungsinformationen werden dann in die Debugfenster in Visual Studio geschrieben:</span><span class="sxs-lookup"><span data-stu-id="0d980-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="0d980-193">Aktivieren der Ablaufverfolgung in der JavaScript-client</span><span class="sxs-lookup"><span data-stu-id="0d980-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="0d980-194">Um die clientseitige Protokollierung für eine Verbindung zu aktivieren, legen Sie die `logging` Eigenschaft für das Verbindungsobjekt vor dem Aufrufen der `start` Methode zum Herstellen der Verbindung.</span><span class="sxs-lookup"><span data-stu-id="0d980-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="0d980-195">**Client-JavaScript-Code für das Aktivieren der Ablaufverfolgung in die Browserkonsole "(mit den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="0d980-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="0d980-196">**Client-JavaScript-Code für das Aktivieren der Ablaufverfolgung in die Browserkonsole "(ohne den generierten Proxy)**</span><span class="sxs-lookup"><span data-stu-id="0d980-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="0d980-197">Wenn Ablaufverfolgung aktiviert ist, protokolliert der JavaScript-Client Ereignisse in der Browserkonsole.</span><span class="sxs-lookup"><span data-stu-id="0d980-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="0d980-198">Für den Zugriff auf die Browserkonsole finden Sie unter [Überwachung Transporte](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="0d980-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="0d980-199">Der folgende Screenshot zeigt einen SignalR JavaScript-Client, ohne die Ablaufverfolgung aktiviert.</span><span class="sxs-lookup"><span data-stu-id="0d980-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="0d980-200">Sie zeigt Verbindungs- und Hub-Aufruf-Ereignisse in der Browserkonsole:</span><span class="sxs-lookup"><span data-stu-id="0d980-200">It shows connection and hub invocation events in the browser console:</span></span>

![SignalR-Ablaufverfolgungsereignisse in der Browserkonsole.](enabling-signalr-tracing/_static/image4.png)