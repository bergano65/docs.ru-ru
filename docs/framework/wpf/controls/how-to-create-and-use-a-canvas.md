---
title: Практическое руководство. Создание и использование холста
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [WPF], Canvas
- Canvas control [WPF], creating
- Canvas control [WPF], using
ms.assetid: 420b9487-9a15-477c-9489-a22a4dec7779
ms.openlocfilehash: edef660b2da2f09e0a6edbc0a87f0d1f26eb03da
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2019
ms.locfileid: "69964223"
---
# <a name="how-to-create-and-use-a-canvas"></a>Практическое руководство. Создание и использование холста
В этом примере показано, как создать и использовать экземпляр <xref:System.Windows.Controls.Canvas>.  
  
## <a name="example"></a>Пример  
 В следующем примере показано явное <xref:System.Windows.Controls.TextBlock> позиционирование двух элементов с <xref:System.Windows.Controls.Canvas.SetTop%2A> помощью <xref:System.Windows.Controls.Canvas.SetLeft%2A> методов <xref:System.Windows.Controls.Canvas>и. В примере также назначается <xref:System.Windows.Controls.Control.Background%2A> `LightSteelBlue` цвет для <xref:System.Windows.Controls.Canvas>.  
  
> [!NOTE]
> При использовании [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] для размещения <xref:System.Windows.Controls.TextBlock> элементов используйте <xref:System.Windows.Controls.Canvas.Top%2A> свойства и <xref:System.Windows.Controls.Canvas.Left%2A> .  
  
 [!code-csharp[CanvasCode#1](~/samples/snippets/csharp/VS_Snippets_Wpf/CanvasCode/CSharp/Canvas_Code.cs#1)]
 [!code-vb[CanvasCode#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CanvasCode/VisualBasic/canvas_vb.vb#1)]  
  
## <a name="see-also"></a>См. также

- <xref:System.Windows.Controls.Canvas>
- <xref:System.Windows.Controls.TextBlock>
- <xref:System.Windows.Controls.Canvas.SetTop%2A>
- <xref:System.Windows.Controls.Canvas.SetLeft%2A>
- <xref:System.Windows.Controls.Canvas.Top%2A>
- <xref:System.Windows.Controls.Canvas.Left%2A>
- [Общие сведения о панелях](panels-overview.md)
- [Разделы практического руководства](canvas-how-to-topics.md)
