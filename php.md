### php研究
>PHP-FPM (PHP FastCGI Process Manager) ：用来管理PHP进程池的软件，用于接收和处理来自web（如 nginx）的请求。PHP-FPM软件会创建一个主进程，控制何时以及如何把http请求转发给一个或多个子进程处理，以及什么时候创建(处理web应用更多的流量)和销毁(子进程运行时间太久或不需要了)PHP子进程。
+ PHP-FPM进程池中每个进程的存在时间都比按个HTTP请求长，可以处理10、50、100、500甚至更多请求。
+ 