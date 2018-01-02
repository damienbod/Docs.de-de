---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "Authentifizierung und Autorisierung für den permanenten SignalR-Verbindungen (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: Dieses Thema beschreibt die Autorisierung auf eine permanente Verbindung zu erzwingen. Allgemeine Informationen zur Integration der Sicherheit in einer SignalR-Anwendung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4c036ddf1e20e3a3be7b043d90b594292013f6c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="5a733-104">Authentifizierung und Autorisierung für den permanenten SignalR-Verbindungen (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="5a733-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>
====================
<span data-ttu-id="5a733-105">durch [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="5a733-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="5a733-106">Dieses Thema beschreibt die Autorisierung auf eine permanente Verbindung zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="5a733-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="5a733-107">Allgemeine Informationen zur Integration der Sicherheit in einer SignalR-Anwendung finden Sie unter [Einführung in die Sicherheit](index.md).</span><span class="sxs-lookup"><span data-stu-id="5a733-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="5a733-108">Erzwingen der Autorisierung</span><span class="sxs-lookup"><span data-stu-id="5a733-108">Enforce authorization</span></span>

<span data-ttu-id="5a733-109">Autorisierungsregeln zu erzwingen, bei Verwendung einer ["persistentconnection"](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) müssen Sie überschreiben die `AuthorizeRequest` Methode.</span><span class="sxs-lookup"><span data-stu-id="5a733-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="5a733-110">Sie können keine der `Authorize` Attribut mit permanenten Verbindungen.</span><span class="sxs-lookup"><span data-stu-id="5a733-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="5a733-111">Die `AuthorizeRequest` Methode wird aufgerufen, durch das SignalR Framework vor jeder Anforderung, um sicherzustellen, dass der Benutzer autorisiert ist, um die angeforderte Aktion auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5a733-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="5a733-112">Die `AuthorizeRequest` Methode wird nicht vom Client aufgerufen; stattdessen authentifiziert den Benutzer über Ihre Anwendung-Authentifizierungsmechanismus.</span><span class="sxs-lookup"><span data-stu-id="5a733-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="5a733-113">Im folgenden Beispiel wird gezeigt, wie Anforderungen für authentifizierte Benutzer beschränkt.</span><span class="sxs-lookup"><span data-stu-id="5a733-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="5a733-114">Sie können eine beliebige benutzerdefinierte Autorisierungslogik in der Methode AuthorizeRequest hinzufügen. wird z. B. überprüft, ob ein Benutzer zu einer bestimmten Rolle gehört.</span><span class="sxs-lookup"><span data-stu-id="5a733-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>