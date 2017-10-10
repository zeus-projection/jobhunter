# session&cookie

**主要是辨认使用者**。

cookie机制采集的是客户端保持状态的方案，而session机制采用的是服务器端保持状态的方案。

cookie的内容主要包括：名字，值，过期时间，路径和域。路径和域一起构成cookie的作用范围，若不设置过期了时间，则表示，cookie的生命周期为浏览器会话期。

session机制是一种服务器端机制，服务器使用一种散列表结构来保存信息。

当程序需要为某个客户端的请求创建一个seesion时，服务器首先检查这个客户端的请求里面是否已包含了一个seesion标示（session id），如果已包含则说明以前已经为此客户端创建过seesion，服务器就按照session id把这个session检索出，检索不到就创建一个，如果客户端的请求不包含session id，则为此客户端创建一个session并且生成一个与此session相关联的seesionid。