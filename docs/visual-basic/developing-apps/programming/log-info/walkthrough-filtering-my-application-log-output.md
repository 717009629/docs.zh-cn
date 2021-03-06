---
title: "筛选 My.Application.Log 输出 (Visual Basic) | Microsoft Docs"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
dev_langs:
- VB
helpviewer_keywords:
- My.Log object, filtering output
- My.Application.Log object, filtering output
- application event logs, output filtering
ms.assetid: 2c0a457a-38a4-49e1-934d-a51320b7b4ca
caps.latest.revision: 22
author: stevehoag
ms.author: shoag
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: caa4b8be16e5000d02d82a83199a25d13ad07bba
ms.lasthandoff: 03/13/2017

---
# <a name="walkthrough-filtering-myapplicationlog-output-visual-basic"></a>演练：筛选 My.Application.Log 输出 (Visual Basic)
本演练演示如何更改对 `My.Application.Log` 对象的默认日志筛选，以控制哪些信息可从 `Log` 对象传递到侦听器以及哪些信息可由侦听器编写。 生成应用程序后仍可以更改日志记录行为，因为配置信息存储在应用程序的配置文件中。  
  
## <a name="getting-started"></a>入门  
 `My.Application.Log` 编写的每条消息均具有相关严重级别，筛选机制使用此级别控制日志输出。 此示例应用程序使用 `My.Application.Log` 方法编写具有不同严重级别的若干日志消息。  
  
#### <a name="to-build-the-sample-application"></a>生成示例应用程序  
  
1.  打开一个新的 [!INCLUDE[vbprvb](../../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)] Windows 应用程序项目。  
  
2.  向 Form1 添加一个名为 Button1 的按钮。  
  
3.  在 Button1 的 <xref:System.Windows.Forms.Control.Click> 事件处理程序中，添加以下代码：  
  
     [!code-vb[VbVbcnMyApplicationLogFiltering#1](../../../../visual-basic/developing-apps/programming/log-info/codesnippet/VisualBasic/walkthrough-filtering-my-application-log-output_1.vb)]  
  
4.  在调试器中运行该应用程序。  
  
5.  按“Button1”****。  
  
     应用程序将向应用程序的调试输出和日志文件中写入以下信息。  
  
     `DefaultSource Information: 0 : In Button1_Click`  
  
     `DefaultSource Error: 2 : Error in the application.`  
  
6.  关闭该应用程序。  
  
     有关如何查看应用程序的调试输出窗口的信息，请参阅[输出窗口](https://docs.microsoft.com/visualstudio/ide/reference/output-window)。 有关应用程序日志文件的位置的信息，请参阅[演练：确定 My.Application.Log 写入信息的位置](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-determining-where-my-application-log-writes-information.md)。  
  
    > [!NOTE]
    >  默认情况下，应用程序关闭时将刷新日志文件输出。  
  
     在上例中，对 <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A> 方法的第二次调用以及对 <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A> 方法的调用将生成日志输出，而对 `WriteEntry` 方法的第一次和最后一次调用则不会。 这是因为 `WriteEntry` 和 `WriteException` 的严重级别为“信息”和“错误”，而 `My.Application.Log` 对象的默认日志筛选支持这两种级别。 但是，不允许严重级别为“开始”和“停止”的事件生成日志输出。  
  
## <a name="filtering-for-all-myapplicationlog-listeners"></a>对所有 My.Application.Log 侦听器的筛选  
 `My.Application.Log` 对象使用名为 `DefaultSwitch` 的 <xref:System.Diagnostics.SourceSwitch>，以控制将哪些消息从 `WriteEntry` 和 `WriteException` 方法传递到侦听器。 通过将 `DefaultSwitch` 的值设置为 <xref:System.Diagnostics.SourceLevels> 枚举的一个值，可在应用程序的配置文件中对其进行配置。 默认情况下，其值为“信息”。  
  
 此表显示了在给定的特定 `DefaultSwitch` 设置下，日志将消息写入侦听器所需的严重级别。  
  
|DefaultSwitch 值|输出所需的消息严重级别|  
|---|---| 
|`Critical`|`Critical`|  
|`Error`|`Critical` 或 `Error`|  
|`Warning`|`Critical`、`Error` 或 `Warning`|  
|`Information`|`Critical`、`Error`、`Warning` 或 `Information`|  
|`Verbose`|`Critical`、`Error`、`Warning`、`Information` 或 `Verbose`|  
|`ActivityTracing`|`Start`、`Stop`、`Suspend`、`Resume` 或 `Transfer`|  
|`All`|允许所有消息。|  
|`Off`|阻止所有消息。|  
  
> [!NOTE]
>  `WriteEntry` 和 `WriteException` 方法分别具有一个未指定严重级别的重载。 `WriteEntry` 重载的隐式严重级别是“信息”，`WriteException` 重载的隐式严重级别是“错误”。  
  
 此表说明上一示例中显示的日志输出：在 `DefaultSwitch` 默认设置为“信息”的情况下，只有对 `WriteEntry` 方法的第二次调用和对 `WriteException` 方法的调用能生成日志输出。  
  
#### <a name="to-log-only-activity-tracing-events"></a>仅记录活动跟踪事件  
  
1.  在“解决方案资源管理器”****中右键单击 app.config，然后选择“打开”****。  
  
     - 或 -  
  
     如果其中没有 app.config 文件：  
  
    1.  在 **“项目”** 菜单上选择 **“添加新项”**。  
  
    2.  在“添加新项” **** 对话框中，选择“应用程序配置文件” ****。  
  
    3.  单击 **“添加”**。  
  
2.  找到 `<switches>` 部分，该部分位于 `<system.diagnostics>` 部分中，后者位于顶级 `<configuration>` 部分中。  
  
3.  找到将 `DefaultSwitch` 添加到开关集合的元素。 此元素应与下面的元素类似：  
  
     `<add name="DefaultSwitch" value="Information" />`  
  
4.  将 `value` 属性的值更改为“ActivityTracing”。  
  
5.  app.config 文件的内容应类似于下面的 XML：  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.diagnostics>  
        <sources>  
          <!-- This section configures My.Application.Log -->  
          <source name="DefaultSource" switchName="DefaultSwitch">  
            <listeners>  
              <add name="FileLog"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="DefaultSwitch" value="ActivityTracing" />  
        </switches>  
        <sharedListeners>  
          <add name="FileLog"  
               type="Microsoft.VisualBasic.Logging.FileLogTraceListener,   
                     Microsoft.VisualBasic, Version=8.0.0.0,   
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a,   
                     processorArchitecture=MSIL"   
               initializeData="FileLogWriter"/>  
        </sharedListeners>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
6.  在调试器中运行该应用程序。  
  
7.  按“Button1”****。  
  
     应用程序将向应用程序的调试输出和日志文件中写入以下信息：  
  
     `DefaultSource Start: 4 : Entering Button1_Click`  
  
     `DefaultSource Stop: 5 : Leaving Button1_Click`  
  
8.  关闭该应用程序。  
  
9. 将 `value` 属性的值改回“信息”。  
  
    > [!NOTE]
    >  `DefaultSwitch` 开关设置仅控制 `My.Application.Log`。 它不会更改 [!INCLUDE[dnprdnshort](../../../../csharp/getting-started/includes/dnprdnshort_md.md)] <xref:System.Diagnostics.Trace?displayProperty=fullName> 和 <xref:System.Diagnostics.Debug?displayProperty=fullName> 类的行为方式。  
  
## <a name="individual-filtering-for-myapplicationlog-listeners"></a>对 My.Application.Log 侦听器的单独筛选  
 上面的示例演示如何更改对所有 `My.Application.Log` 输出的筛选。 此示例演示如何筛选单个日志侦听器。 默认情况下，应用程序有两个可写入应用程序的调试输出和日志文件的侦听器。  
  
 配置文件控制日志侦听器的行为，方法是允许每个侦听器拥有一个筛选器，这类似于 `My.Application.Log` 的开关。 仅当日志的 `DefaultSwitch` 和日志侦听器的筛选器均支持消息的严重级别时，日志侦听器才会输出消息。  
  
 此示例演示如何为新的调试侦听器配置筛选并将其添加到 `Log` 对象。 应删除 `Log` 对象中的默认调试侦听器，以便明确调试消息来自新的调试侦听器。  
  
#### <a name="to-log-only-activity-tracing-events"></a>仅记录活动跟踪事件  
  
1.  在“解决方案资源管理器”****中右键单击 app.config，然后选择“打开”****。  
  
     - 或 -  
  
     如果其中没有 app.config 文件：  
  
    1.  在 **“项目”** 菜单上选择 **“添加新项”**。  
  
    2.  在“添加新项” **** 对话框中，选择“应用程序配置文件” ****。  
  
    3.  单击 **“添加”**。  
  
2.  在“解决方案资源管理器”****中右键单击 app.config。 选择“打开”****。  
  
3.  找到 `<listeners>` 部分，该部分位于 `name` 属性为“DefaultSource”的 `<source>` 部分中，后者位于 `<sources>` 部分中。 `<sources>` 部分位于 `<system.diagnostics>` 部分中，后者位于顶级 `<configuration>` 部分中。  
  
4.  将此元素添加到 `<listeners>` 部分：  
  
    ```xml  
    <!-- Remove the default debug listener. -->  
    <remove name="Default"/>  
    <!-- Add a filterable debug listener. -->  
    <add name="NewDefault"/>  
    ```  
  
5.  找到 `<sharedListeners>` 部分，该部分位于 `<system.diagnostics>` 部分当中，后者又位于顶级 `<configuration>` 部分之下。  
  
6.  将此元素添加到该 `<sharedListeners>` 部分：  
  
    ```xml  
    <add name="NewDefault"   
         type="System.Diagnostics.DefaultTraceListener,   
               System, Version=2.0.0.0, Culture=neutral,   
               PublicKeyToken=b77a5c561934e089,   
               processorArchitecture=MSIL">  
        <filter type="System.Diagnostics.EventTypeFilter"   
                initializeData="Error" />  
    </add>  
    ```  
  
     <xref:System.Diagnostics.EventTypeFilter> 筛选器将其中的一个 <xref:System.Diagnostics.SourceLevels> 枚举值作为其 `initializeData` 属性。  
  
7.  app.config 文件的内容应类似于下面的 XML：  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <configuration>  
      <system.diagnostics>  
        <sources>  
          <!-- This section configures My.Application.Log -->  
          <source name="DefaultSource" switchName="DefaultSwitch">  
            <listeners>  
              <add name="FileLog"/>  
              <!-- Remove the default debug listener. -->  
              <remove name="Default"/>  
              <!-- Add a filterable debug listener. -->  
              <add name="NewDefault"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="DefaultSwitch" value="Information" />  
        </switches>  
        <sharedListeners>  
          <add name="FileLog"  
               type="Microsoft.VisualBasic.Logging.FileLogTraceListener,   
                     Microsoft.VisualBasic, Version=8.0.0.0,   
                     Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a,   
                     processorArchitecture=MSIL"   
               initializeData="FileLogWriter"/>  
          <add name="NewDefault"   
               type="System.Diagnostics.DefaultTraceListener,   
                     System, Version=2.0.0.0, Culture=neutral,   
                     PublicKeyToken=b77a5c561934e089,   
                     processorArchitecture=MSIL">  
            <filter type="System.Diagnostics.EventTypeFilter"   
                    initializeData="Error" />  
          </add>  
        </sharedListeners>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
8.  在调试器中运行该应用程序。  
  
9. 按“Button1”****。  
  
     应用程序将以下信息写入应用程序的日志文件：  
  
     `Default Information: 0 : In Button1_Click`  
  
     `Default Error: 2 : Error in the application.`  
  
     由于筛选限制性更强，应用程序将较少的信息写入应用程序的调试输出。  
  
     `Default Error   2   Error`  
  
10. 关闭该应用程序。  
  
 有关在部署后更改日志设置的详细信息，请参阅[使用应用程序日志](../../../../visual-basic/developing-apps/programming/log-info/working-with-application-logs.md)。  
  
## <a name="see-also"></a>请参阅  
 [演练：确定 My.Application.Log 写入信息的位置](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-determining-where-my-application-log-writes-information.md)   
 [演练：更改 My.Application.Log 写入信息的位置](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-changing-where-my-application-log-writes-information.md)   
 [演练：创建自定义日志侦听器](../../../../visual-basic/developing-apps/programming/log-info/walkthrough-creating-custom-log-listeners.md)   
 [如何：编写日志消息](../../../../visual-basic/developing-apps/programming/log-info/how-to-write-log-messages.md)   
 [跟踪开关](http://msdn.microsoft.com/library/8ab913aa-f400-4406-9436-f45bc6e54fbe)   
 [记录来自应用程序的信息](../../../../visual-basic/developing-apps/programming/log-info/logging-information-from-the-application.md)
