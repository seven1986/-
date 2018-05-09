调试接口
========

.. Note::

    调试前需准备:

    - subscriptionKey           如何获取
    - access_token      如何获取


网页调试
---------

   * 点击进入调试页面后，设置subscriptionKey和access_token
   * 根据接口要求，设置Request Parameters
   * 点击Send按钮，发送请求，查看返回结果

桌面调试
----------

.. Note::

    桌面调试目前大多数人使用的工具为Postman，所有请先确认安装了Postman for Chrome 或Postman for PC。

* 进入调试页面，点击 "API definition" >> "Open API"，复制弹出页面的网址
* 打开Postman，选择 "导入" >> "From Link"，粘贴网址
* 查看Postman的Collection，选择接口调试即可

错误排查
-----------

   常见错误：

   * 401 未授权，因为没有传入subscriptionKey或access_token
   * 404 不存在的服务，请求地址错误
   * 500 接口内部错误

