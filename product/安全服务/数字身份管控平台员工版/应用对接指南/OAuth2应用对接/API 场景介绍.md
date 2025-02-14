本文将介绍数字身份管控平台（员工版）(以下简称 EIAM) 为 OAuth2 应用系统提供的 API 。通过调用这些 API ，您的应用可以完成与用户身份相关的各类操作，例如：用户认证、用户信息的获取，应用的访问控制等。

这里将具体给出应用系统实现如下几种典型业务场景所需的主要工作，不同业务场景可能调用相同的接口，但请求参数和请求响应一般有所不同。

>!在应用接入前，您需要首先在 [应用管理页面](https://console.cloud.tencent.com/eiam/app-manager) 完成 OAuth2 协议类型应用的新建与配置，获取应用的 Client ID 、 Client Secret 等配置信息。
## 前提条件
已完成 [创建应用并授权访问](https://cloud.tencent.com/document/product/1442/55110#.E5.88.9B.E5.BB.BA.E5.BA.94.E7.94.A8)。

## 用户登录
对于应用实现用户认证的功能，EIAM 提供通过认证门户登录并直接单点登录至应用的能力。基于OAuth2 协议，提供授权码模式、PKCE 模式、简化模式等多种认证授权方式。下面以授权码模式为例展示应用实现用户认证的流程。
1. 登录 [数字身份管控平台（员工版）控制台](https://console.cloud.tencent.com/eiam/app-manager)，在左侧导航栏，单击**应用管理**。
2. 在应用管理页面，选择已创建的 OAuth2 应用，单击**应用配置**。
![](https://main.qcloudimg.com/raw/f8961069101f448f9b1b31b0fd4a521e.png)
3. 在应用信息页面，获取 Authorize URL，此 URL 为用户在应用系统登录时，应用将用户重定向的认证地址，详情请参见 [授权码模式](https://cloud.tencent.com/document/product/1442/62414)。
![](https://main.qcloudimg.com/raw/d70e7d7c178de3a965cb294d77b4c967.png)
4. 使用该地址首次访问会跳转到登录页面进行用户身份认证，输入账号密码后单击**登录**，认证通过后会重定向到指定地址(redirect_uri)，并在 URL 中携带 code 参数。
![](https://main.qcloudimg.com/raw/3bb227d96f6a0d2e38e822d155aa182d.png)
6. 使用应用回调地址，接收到回调请求后，可以通过调用 [获取 Token ](https://cloud.tencent.com/document/product/1442/62422)接口，来获取接口中的 access_token 和 refresh_token。
7. 应用建立用户的登录态，将 access_token 和 refresh_token 与登录态关联起来，完成登录。
>?应用系统可自行选择合适的方法建立并保持登录态，例如：Web 应用可以使用 session cookie ，单页应用可以使用 localStorage 或 sessionStorage 。


## 短信验证码二次认证
针对在 EIAM 控制台开启了二次认证功能的情况下，发起的密码登录场景。
>?二次认证功能在 [安全设置页面](https://console.cloud.tencent.com/eiam/security-setting) 中开启。
>
1. 登录 [数字身份管控平台（员工版）控制台](https://console.cloud.tencent.com/eiam/app-manager)，在左侧导航栏，单击**应用管理**。
2. 调用 [密码模式](https://cloud.tencent.com/document/product/1442/62423)，触发二次认证将会返回`mfa_required`的错误码并返回`mfa_token`。
3. 调用 [ MFA 认证因子质询](https://cloud.tencent.com/document/product/1442/62436) 接口，输入 `mfa_token`，该接口会发送短信验证码，并返回 oob 码。
4. 调用 oob 模式的[ 短信验证码模式](https://cloud.tencent.com/document/product/1442/62428)，传入 oob 码和短信验证码，验证通过后返回 access_token和 refresh_token。
5. 应用建立用户的登录态，将 access_token 和 refresh_token 与登录态关联起来，完成登录。
>?应用系统可自行选择合适的方法建立并保持登录态，例如：Web 应用可以使用 session cookie ，单页应用可以使用 localStorage 或 sessionStorage 。

## 访问用户资源
在完成用户认证后，应用可以使用 access_token 访问用户资源，例如获取用户基本信息。
1. 假设用户在使用应用系统时，系统需要将用户的账号信息显示在界面上。此时，需要调用相应的接口来获取该信息。
2. 应用系统取出当前登录用户的 access_token ，携带 Token 调用 [获取用户信息](https://cloud.tencent.com/document/product/1442/62431) 接口。
3. 如果接口返回了 HTTP 401 错误码，代表 access_token 已过期，需要使用 refresh_token 调用 [令牌刷新](https://cloud.tencent.com/document/product/1442/62429)接口，获取新的 access_token ，再次调用获取用户信息接口。
4. 应用解析接口返回的用户账号信息，并显示给用户。提供如下获取当前登录用户相关信息的 API：

| 接口功能                                                     | 描述                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [获取用户基本信息](https://cloud.tencent.com/document/product/1442/62431) | 使用合法的授权令牌，获取用户基本信息。                 |
| [获取用户授权应用列表](https://cloud.tencent.com/document/product/1442/62432) | 使用合法的授权令牌，获取用户有权限访问的应用系统列表。 |
| [获取当前用户指定应用的账号列表](https://cloud.tencent.com/document/product/1442/62434) | 使用合法的授权令牌，获取当前用户指定应用的账号列表。   |

## 其他 API
1. 如果 refresh token 存在泄漏的可能，可调用 [注销刷新令牌](https://cloud.tencent.com/document/product/1442/62435) 接口。
2. 用户在应用系统进行注销操作时，可调用 [注销接口](https://cloud.tencent.com/document/product/1442/62437)。
>!应用系统需要自行清除用户在应用的登录态。

