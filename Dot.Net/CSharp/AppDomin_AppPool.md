## 基本概念

### 应用程序域
.Net提供的隔离方式，应用程序域，也是CLR资源的边界。
每个AppDomain代表单个运行的.NET应用程序。


### 应用程序池
将一个或多个应用程序连接到一个或多个工作进程集合的配置。因为应用程序池中的应用程序与其他应用程序被工作进程边界分隔，所以某个应用程序池中的应用程序不会受到其他应用程序池中的应用程序所产生的问题的影响。

应用程序池可能有几个AppDomains。每个AppDomain代表单个运行的ASP.NET应用程序。许多ASP.NET应用程序可能属于单个应用程序池。

应用程序池代表有限数量的可能承载更大数量的应用程序的工作进程。

默认情况下，应用程序池会获得一个工作进程(w3wp.exe)。尽管如此，应用程序池可以配置为使用任意数量的进程。

应用程序池中工作进程数的设置为1(默认值)意味着池中的所有应用程序/ AppDomains共享相同的工作进程。

### Web Gradle
Web 园时，您只需在“应用程序池属性”的“性能”选项卡的“最大工作进程数”框中，设置一个大于 1 的工作进程数。如果这个值大于 1，每个请求都将启动一个新的工作进程实例，可启动的最多进程数为您所指定的最大工作进程数。后续的请求将以循环的方式发送至工作进程。




