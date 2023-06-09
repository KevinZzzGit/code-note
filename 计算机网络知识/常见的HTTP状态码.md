# 常见的HTTP状态码

## 基础分类

- 1XX：指定客户端应相应的某些动作，代表请求已被接受，需要继续处理。由于 HTTP/1.0 协议中没有定义任何 1xx 状态码，所以除非在某些试验条件下，服务器禁止向此类客户端发送 1xx 响应。
- 2XX：代表请求已成功被服务器接收、理解、并接受。这系列中最常见的有200、201状态码。

- 3XX：代表需要客户端采取进一步的操作才能完成请求，这些状态码用来重定向，后续的请求地址（重定向目标）在本次响应的 Location 域中指明。这系列中最常见的有301、302状态码。
- 4XX：表示请求错误。代表了客户端看起来可能发生了错误，妨碍了服务器的处理。常见有：401、404状态码。
- 5XX：代表了服务器在处理请求的过程中有错误或者异常状态发生，也有可能是服务器意识到以当前的软硬件资源无法完成对请求的处理。常见有500、503状态码。

## 分类详解

### 2XX

- 200 （成功） 服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。
- 201 （已创建） 请求成功并且服务器创建了新的资源。
- 202 （已接受） 服务器已接受请求，但尚未处理。
- 203 （非授权信息） 服务器已成功处理了请求，但返回的信息可能来自另一来源。
- 204 （无内容） 服务器成功处理了请求，但没有返回任何内容。
- 205 （重置内容） 服务器成功处理了请求，但没有返回任何内容。
- 206 （部分内容） 服务器成功处理了部分 GET 请求。

### 3XX

- 300 （多种选择） 针对请求，服务器可执行多种操作。 服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。
- 301 （永久移动） 请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。
- 302 （临时移动） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
- 303 （查看其他位置） 请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。
- 304 （未修改） 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。
- 305 （使用代理） 请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。
- 307 （临时重定向） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。

### 4XX

- 400 （错误请求） 服务器不理解请求的语法。
- 401 （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。
- 403 （禁止） 服务器拒绝请求。
- 404 （未找到） 服务器找不到请求的网页。
- 405 （方法禁用） 禁用请求中指定的方法。
- 406 （不接受） 无法使用请求的内容特性响应请求的网页。
- 407 （需要代理授权） 此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。
- 408 （请求超时） 服务器等候请求时发生超时。
- 409 （冲突） 服务器在完成请求时发生冲突。 服务器必须在响应中包含有关冲突的信息。
- 410 （已删除） 如果请求的资源已永久删除，服务器就会返回此响应。
- 411 （需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。
- 412 （未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。
- 413 （请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。
- 414 （请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。
- 415 （不支持的媒体类型） 请求的格式不受请求页面的支持。
- 416 （请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态代码。
- 417 （未满足期望值） 服务器未满足"期望"请求标头字段的要求。

### 5XX

- 500 （服务器内部错误） 服务器遇到错误，无法完成请求。
- 501 （尚未实施） 服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。
- 502 （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。
- 503 （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。
- 504 （网关超时） 服务器作为网关或代理，但是没有及时从上游服务器收到请求。
- 505 （HTTP 版本不受支持） 服务器不支持请求中所用的 HTTP 协议版本。

## RESTful规范状态码

| 状态码                                     | 含义                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| **200** OK - [GET]                         | 服务器成功返回用户请求的数据                                 |
| 201 CREATED - [POST/PUT/PATCH]             | 用户新建或修改数据成功                                       |
| 202 Accepted                               | 表示一个请求已经进入后台排队（异步任务）                     |
| 204 NO CONTENT - [DELETE]                  | 用户删除数据成功                                             |
|                                            |                                                              |
| **400** INVALID REQUEST - [POST/PUT/PATCH] | 用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的 |
| **401** Unauthorized - [*]                 | 表示用户没有权限（令牌、用户名、密码错误）                   |
| **403** Forbidden - [*]                    | 表示用户得到授权（与401错误相对），但是访问是被禁止的        |
| **404** NOT FOUND - [*]                    | 用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的 |
| **405** Method Not Allowed                 | 方法不允许，服务器没有该方法                                 |
| 406 Not Acceptable - [GET]                 | 用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式） |
| 410 Gone -[GET]                            | 用户请求的资源被永久删除，且不会再得到的                     |
| 422 Unprocesable entity - [POST/PUT/PATCH] | 当创建一个对象时，发生一个验证错误                           |
| **500** INTERNAL SERVER ERROR - [*]        | 服务器发生错误，用户将无法判断发出的请求是否成功             |



