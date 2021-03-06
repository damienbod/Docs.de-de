---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Ausführen mehrerer Animationen gleichzeitig (VB) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Sie können durch Fallenlassen ausführen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 823764abd4444b5cb8d9bc6e8e8ed34d6f670310
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-at-the-same-time-vb"></a>Ausführen mehrerer Animationen gleichzeitig (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Es kann mehrere Animationen in einer Weise parallel ausgeführt.


## <a name="overview"></a>Übersicht

Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Es kann mehrere Animationen in einer Weise parallel ausgeführt.

## <a name="steps"></a>Schritte

Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

Innerhalb der `<Animations>` Knoten verwenden `<OnLoad>` Animationen ausgeführt, nachdem die Seite vollständig geladen wurde. Im allgemeinen `<OnLoad>` akzeptiert nur eine Animation. Die Animation-Framework bietet die Möglichkeit, mehrere Animationen in einer mit join die `<Parallel>` Element. Alle Animationen in `<Parallel>` zur gleichen Zeit ausgeführt werden.

Hier wird der mögliche-Markup für die `AnimationExtender` gesteuert, ausblenden und Ändern der Größe der Systemsteuerung zur gleichen Zeit:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

Und tatsächlich: Wenn Sie dieses Skript ausführen, im Bereich angezeigt wird, und ändert die Größe (mehr als Threefolding die Breiten- und Halfing seine Höhe) und zur gleichen Zeit ausgeblendet.


[![Das Panel wird ausblenden und Ändern der Größe (einschließlich Inhalt Dank des Browsers-Renderingmodul)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

Das Panel wird ausblenden und Ändern der Größe (einschließlich Inhalt Dank des Browsers-Renderingmodul) ([klicken Sie hier, um das Bild in voller Größe angezeigt](executing-several-animations-at-the-same-time-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](adding-animation-to-a-control-vb.md)
> [Weiter](executing-several-animations-after-each-other-vb.md)
