---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Aktivieren der Windows-Authentifizierung in Katana | Microsoft Docs
author: MikeWasson
description: 'In diesem Artikel wird gezeigt, wie die Windows-Authentifizierung in Katana aktiviert. Es werden zwei Szenarien behandelt: Verwenden von IIS zum Host Katana und HttpListener Kat Selbsthosting mithilfe...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: cc23a053fb1ba60ea84eca59e99f0e375fefc4cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="029c4-104">Aktivieren der Windows-Authentifizierung in Katana</span><span class="sxs-lookup"><span data-stu-id="029c4-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="029c4-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="029c4-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="029c4-106">In diesem Artikel wird gezeigt, wie die Windows-Authentifizierung in Katana aktiviert.</span><span class="sxs-lookup"><span data-stu-id="029c4-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="029c4-107">Es werden zwei Szenarien behandelt: Verwenden von IIS zum Host Katana und Verwenden von HttpListener Katana selbst in einem benutzerdefinierten Prozess gehostet.</span><span class="sxs-lookup"><span data-stu-id="029c4-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="029c4-108">Unser Dank gilt Barry Dorrans, David Matson und Chris Ross zum Überprüfen des in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="029c4-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="029c4-109">Katana ist Microsoft Implementierung des [OWIN](http://owin.org/), die offene Webschnittstelle für .NET.</span><span class="sxs-lookup"><span data-stu-id="029c4-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="029c4-110">Informieren Sie sich eine Einführung in die OWIN und Katana [hier](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="029c4-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="029c4-111">Die OWIN-Architektur besteht aus mehreren Ebenen:</span><span class="sxs-lookup"><span data-stu-id="029c4-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="029c4-112">Host: Verwaltet den Prozess, in dem die OWIN-Pipeline ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="029c4-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="029c4-113">Server: Ein Netzwerksocket geöffnet und aktiv abgehört.</span><span class="sxs-lookup"><span data-stu-id="029c4-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="029c4-114">Middleware: Verarbeitet die HTTP-Anforderung und Antwort.</span><span class="sxs-lookup"><span data-stu-id="029c4-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="029c4-115">Katana bietet derzeit zwei Servern, die beide die integrierte Windows-Authentifizierung unterstützen:</span><span class="sxs-lookup"><span data-stu-id="029c4-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="029c4-116">**"Microsoft.owin.Host.systemweb"**.</span><span class="sxs-lookup"><span data-stu-id="029c4-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="029c4-117">Wird IIS mit der ASP.NET-Pipeline verwendet.</span><span class="sxs-lookup"><span data-stu-id="029c4-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="029c4-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="029c4-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="029c4-119">Verwendet [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="029c4-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="029c4-120">Dieser Server ist derzeit die Standardoption für Selbsthosting Katana.</span><span class="sxs-lookup"><span data-stu-id="029c4-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="029c4-121">Katana bietet derzeit OWIN-Middleware für die Windows-Authentifizierung keine, da diese Funktion bereits in der Server verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="029c4-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="029c4-122">Windows-Authentifizierung in IIS</span><span class="sxs-lookup"><span data-stu-id="029c4-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="029c4-123">Verwenden "Microsoft.owin.Host.systemweb", können Sie einfach Windows-Authentifizierung in IIS aktivieren.</span><span class="sxs-lookup"><span data-stu-id="029c4-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="029c4-124">Zunächst erstellen eine ASP.NET-Anwendung mithilfe der Projektvorlage "Leere ASP.NET-Webanwendung".</span><span class="sxs-lookup"><span data-stu-id="029c4-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="029c4-125">Als Nächstes fügen Sie NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="029c4-125">Next, add NuGet packages.</span></span> <span data-ttu-id="029c4-126">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="029c4-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="029c4-127">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="029c4-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="029c4-128">Nun fügen Sie eine Klasse, die mit dem Namen `Startup` durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="029c4-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="029c4-129">Das ist alles, die müssen Sie eine Anwendung "Hello World" für OWIN, die unter IIS zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="029c4-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="029c4-130">Drücken Sie F5, um die Anwendung zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="029c4-130">Press F5 to debug the application.</span></span> <span data-ttu-id="029c4-131">"Hello World!" sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="029c4-131">You should see "Hello World!"</span></span> <span data-ttu-id="029c4-132">im Browserfenster.</span><span class="sxs-lookup"><span data-stu-id="029c4-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="029c4-133">Aktivieren Sie als Nächstes Windows-Authentifizierung in IIS Express.</span><span class="sxs-lookup"><span data-stu-id="029c4-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="029c4-134">Aus der **Ansicht** klicken Sie im Menü **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="029c4-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="029c4-135">Klicken Sie auf den Projektnamen im Projektmappen-Explorer, um die Projekteigenschaften anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="029c4-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="029c4-136">In der **Eigenschaften** legen **anonyme Authentifizierung** auf **deaktiviert** und legen Sie **Windows-Authentifizierung** auf  **Aktiviert**.</span><span class="sxs-lookup"><span data-stu-id="029c4-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="029c4-137">Wenn Sie die Anwendung aus Visual Studio ausführen, müssen die IIS Express, dass Windows-Anmeldeinformationen des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="029c4-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="029c4-138">Sie können dies verschaffen, indem [Fiddler](http://fiddler2.com/home) oder eine andere HTTP-Tool zum Debuggen.</span><span class="sxs-lookup"><span data-stu-id="029c4-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="029c4-139">Hier ist ein Beispiel für HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="029c4-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="029c4-140">Der WWW-Authenticate-Header in dieser Antwort anzugeben, dass der Server unterstützt die [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) -Protokoll, das entweder Kerberos oder NTLM verwendet.</span><span class="sxs-lookup"><span data-stu-id="029c4-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="029c4-141">Später, wenn Sie die Anwendung auf einem Server bereitstellen, führen Sie [Schritte](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) auf Windows-Authentifizierung in IIS auf diesem Server zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="029c4-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="029c4-142">Windows-Authentifizierung in HttpListener</span><span class="sxs-lookup"><span data-stu-id="029c4-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="029c4-143">Wenn Sie zum Katana Selbsthosting Microsoft.Owin.Host.HttpListener verwenden, können Sie die Windows-Authentifizierung aktivieren, direkt auf die **HttpListener** Instanz.</span><span class="sxs-lookup"><span data-stu-id="029c4-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="029c4-144">Erstellen Sie zunächst eine neue Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="029c4-144">First, create a new console application.</span></span> <span data-ttu-id="029c4-145">Als Nächstes fügen Sie NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="029c4-145">Next, add NuGet packages.</span></span> <span data-ttu-id="029c4-146">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="029c4-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="029c4-147">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="029c4-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="029c4-148">Nun fügen Sie eine Klasse, die mit dem Namen `Startup` durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="029c4-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="029c4-149">Diese Klasse implementiert die gleichen "Hello World"-Beispiel vorher, aber Windows-Authentifizierung wird auch als das Authentifizierungsschema festgelegt.</span><span class="sxs-lookup"><span data-stu-id="029c4-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="029c4-150">Innerhalb der `Main` -Funktion, die OWIN-Pipeline starten:</span><span class="sxs-lookup"><span data-stu-id="029c4-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="029c4-151">Sie können eine Anforderung senden, in Fiddler, um sicherzustellen, dass die Anwendung Windows-Authentifizierung verwendet:</span><span class="sxs-lookup"><span data-stu-id="029c4-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="029c4-152">Verwandte Themen</span><span class="sxs-lookup"><span data-stu-id="029c4-152">Related Topics</span></span>

[<span data-ttu-id="029c4-153">Eine Übersicht über Project Katana</span><span class="sxs-lookup"><span data-stu-id="029c4-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="029c4-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="029c4-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[<span data-ttu-id="029c4-155">Grundlegendes zur Formularauthentifizierung in MVC 5 OWIN</span><span class="sxs-lookup"><span data-stu-id="029c4-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)