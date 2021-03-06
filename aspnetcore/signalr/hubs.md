---
title: Verwenden von Hubs in ASP.NET Core SignalR
author: rachelappel
description: Erfahren Sie, wie Hubs in ASP.NET Core SignalR verwendet.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 5c477dd64c4cf8b7d6da1f121a290b00f3864f45
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Verwenden von Hubs in SignalR für ASP.NET Core

Durch [Rachel Appel](https://twitter.com/rachelappel) und [Kevin Griffin](https://twitter.com/1kevgriff)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(Gewusst wie: herunterladen)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Was eine SignalR-Hubs ist

Die SignalR-Hubs-API können Sie Methoden für verbundene Clients vom Server aufgerufen. In den Servercode definieren Sie die Methoden, die vom Client aufgerufen werden. Im Clientcode definieren Sie die Methoden, die vom Server aufgerufen werden. SignalR kümmert sich um alles, was im Hintergrund, die in Echtzeit-Client-zu-Server und Server zu Client-Kommunikation möglich macht.

## <a name="configure-signalr-hubs"></a>Konfigurieren von SignalR-hubs

Die SignalR-Middleware erfordert einige Dienste, die durch den Aufruf konfiguriert werden `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

Wenn eine app ASP.NET Core SignalR-Funktionalität hinzufügen, richten Sie SignalR Routen durch Aufrufen `app.UseSignalR` in die `Startup.Configure` Methode.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a>Erstellen und Verwenden von hubs

Erstellen Sie einen Hub durch Deklarieren einer Klasse, die von erben `Hub`, und fügen Sie öffentliche Methoden hinzu. Clients können als definierten Methoden aufrufen `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Sie können angeben, ein Rückgabetyp und Parameter, einschließlich Arrays und komplexe Typen, wie in einer C#-Methode. SignalR verarbeitet die Serialisierung und Deserialisierung von komplexen Objekten und Arrays in Ihre Parameter und Rückgabewerte.

## <a name="the-clients-object"></a>Das Objekt für Clients

Jede Instanz von der `Hub` -Klasse verfügt über eine Eigenschaft namens `Clients` , enthält die folgenden Elemente für die Kommunikation zwischen Server und Client:

| Eigenschaft | Beschreibung |
| ------ | ----------- |
| `All` | Ruft eine Methode auf alle verbundenen clients |
| `Caller` | Ruft eine Methode auf dem Client, der die hubmethode aufgerufen hat |
| `Others` | Ruft eine Methode auf alle verbundenen Clients mit Ausnahme des Clients, die die Methode aufgerufen hat. |


Darüber hinaus `Hub.Clients` enthält die folgenden Methoden:

| Methode | Beschreibung |
| ------ | ----------- |
| `AllExcept` | Ruft eine Methode auf alle verbundenen Clients mit Ausnahme der angegebenen Verbindungen. |
| `Client` | Ruft eine Methode auf einem bestimmten verbundenen client |
| `Clients` | Ruft eine Methode auf bestimmte verbundene clients |
| `Group` | Sendet eine Nachricht an alle Verbindungen in der angegebenen Gruppe  |
| `GroupExcept` | Sendet eine Nachricht an alle Verbindungen in der angegebenen Gruppe ist, mit Ausnahme der angegebenen Verbindungen. |
| `Groups` | Sendet eine Nachricht in mehrere Gruppen von Verbindungen  |
| `OthersInGroup` | Sendet eine Nachricht an eine Gruppe von Verbindungen mit Ausnahme des Clients, der die hubmethode aufgerufen hat  |
| `User` | Sendet eine Nachricht an alle Verbindungen, die einem bestimmten Benutzer zugeordnet |
| `Users` | Sendet eine Nachricht an alle Verbindungen, die die angegebenen Benutzer zugeordneten |

Jede Eigenschaft oder Methode in den vorherigen Tabellen gibt ein Objekt mit einer `SendAsync` Methode. Die `SendAsync` Methode können Sie den Namen und den Parametern der aufzurufenden Clientmethode bereitzustellen.

## <a name="send-messages-to-clients"></a>Senden von Nachrichten an clients

Um für bestimmte Clients aufrufen können, verwenden Sie die Eigenschaften des der `Clients` Objekt. Im folgenden Beispiel die `SendMessageToCaller` -Methode veranschaulicht das Senden einer Nachricht an die Verbindung, die die hubmethode aufgerufen hat. Die `SendMessageToGroups` Methode sendet eine Nachricht an die Gruppen, die in gespeicherten eine `List` mit dem Namen `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Behandeln von Ereignissen für eine Verbindung

Die SignalR-Hubs-API bietet die `OnConnectedAsync` und `OnDisconnectedAsync` virtuelle Methoden zum Verwalten und Nachverfolgen von Verbindungen. Überschreiben Sie die `OnConnectedAsync` virtuelle Methode, um Aktionen durchzuführen, wenn ein Client eine, auf den Hub Verbindung, z. B. eine Gruppe hinzuzufügen.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Behandeln von Fehlern

In den hubmethoden ausgelösten Ausnahmen werden an den Client gesendet, die die Methode aufgerufen hat. Auf der JavaScript-Client die `invoke` Methode gibt ein [JavaScript-Zusicherung](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Wenn der Client einen Fehler mit einem Handler empfängt die Zusage mit angefügt `catch`, sie wird aufgerufen, und als JavaScript-bestanden `Error` Objekt.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Weitere Informationen

* [Einführung in ASP.NET Core SignalR](xref:signalr/introduction)
* [JavaScript-Client](xref:signalr/javascript-client)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
