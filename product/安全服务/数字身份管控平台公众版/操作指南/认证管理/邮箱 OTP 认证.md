## 操作场景
数字身份管控平台（公众版）支持使用邮箱 OTP 认证源，即采用邮箱地址和一次性验证码的方式对用户进行身份验证。

## 操作步骤
### 新建认证源
1. 登录 [数字身份管控平台（公众版）控制台](https://console.cloud.tencent.com/ciam)，在左侧导航栏，选择**认证管理** > **通用认证源**,进入通用认证源页面。
2. 在通用认证源页面，单击**新建认证源**，进入新建认证源页面。
![](https://main.qcloudimg.com/raw/cac93cbfd835e939665b4d3b450d5bfc.png)
3. 在新建认证源页面，选择邮箱 OTP 认证，单击**下一步**。
4. 在新建认证源页面，设置认证源图标、名称、属性和描述，单击**下一步**。
>?
>- 认证源图标：用于在列表和门户中展示，用户可单击**重新上传**代替默认图标。
>- 认证源名称：用户标识认证源。
>- 认证源属性：邮箱 OTP 认证源默认使用邮箱地址属性，不可修改。
>- 认证源描述：认证源的简单描述。
>
![](https://main.qcloudimg.com/raw/b3a9071b547a1d5614faeabe0b169a10.png)
5. 在新建认证源页面，配置相关参数，单击**确定**，即可创建认证源。
>?
>- 邮箱验证码长度：用户配置为用户发送邮件时生成的验证码长度，范围1～128位。
>- 邮箱验证码有效期：用户配置该验证码的有效期，范围1～300秒。
>
![](https://main.qcloudimg.com/raw/80265c3a4f46f551263d0fd6b72ae016.png)

### 编辑认证源
1. 登录 [数字身份管控平台（公众版）控制台](https://console.cloud.tencent.com/ciam)，在左侧导航栏，选择**认证管理** > **通用认证源**,进入通用认证源页面。
2. 在通用认证源页面，选择邮箱 OTP 认证，单击**编辑**，进入编辑邮箱 OTP 认证的基本信息页面。
3. 在基本信息页面，根据需求修改基本信息，单击**确定**，即可修改基本信息。
![](https://main.qcloudimg.com/raw/785c71dc84cf225498605644383ca47e.png)
4. 单击**邮箱策略**，切换至编辑邮箱 OTP 认证的邮箱策略页面。
5. 在邮箱策略页面，对邮箱验证码长度和邮箱验证码有效期进行修改，单击**确定**，即可修改认证源邮箱策略。
>?
>- 短信验证码长度：用户配置为用户发送短信时生成的验证码长度，范围1～6位。
>- 短信验证码有效期：用户配置该验证码的有效期，范围1～300秒。


### 测试邮箱
1. 登录 [数字身份管控平台（公众版）控制台](https://console.cloud.tencent.com/ciam)，在左侧导航栏，选择**认证管理** > **通用认证源**,进入通用认证源页面。
2. 在通用认证源页面，选择邮箱 OTP 认证源，单击**测试邮箱**，弹出测试邮箱弹窗。
>?测试邮箱功能仅在邮箱 OTP 认证源开启状态下可用。
>
![](https://main.qcloudimg.com/raw/7453c31cdcdbfc3cf0976123895e0cc3.png)
3. 在测试邮箱弹窗中，输入邮箱地址，单击**发送测试邮箱**，将以该邮箱 OTP 认证源配置内容，为该用户发送一条测试邮件。
