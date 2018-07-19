﻿﻿获取access_token
==================

.. Note::

    获取access_token之前，请先申请client_Id、client_secret    :doc:`issues`


- 调用各接口时都需使用access_token。开发者需要进行妥善保存。
- access_token的有效期目前为24个小时。

平台的API调用所需的access_token使用及生成方式说明：

1、建议开发者使用中控服务器统一获取和刷新Access_token，其他业务逻辑服务器所使用的access_token均来自于该中控服务器，不应该各自去刷新，否则容易造成冲突，导致access_token覆盖而影响业务；

2、目前Access_token的有效期通过返回的expire_in来传达，目前是3600秒之内的值。中控服务器需要根据这个有效时间提前去刷新新access_token；

3、Access_token的有效时间可能会在未来有调整，所以中控服务器不仅需要内部定时主动刷新，还需要提供被动刷新access_token的接口，这样便于业务服务器在API调用获知access_token已超时的情况下，可以触发access_token的刷新流程。


授权地址
__________

https请求方式: POST

地址：https://ids.ixingban.com/connect/token

参数说明
__________

grant_type	是	固定填写 client_credentials

client_Id	是	第三方用户唯一凭证（通过申请获得）

client_secret	是	第三方用户唯一凭证密钥（通过申请获得）

scope   是   填写权限集合。（如：iot.cardservice.all iot.paymentservice.all）

返回说明
__________

正常情况下，会返回下述JSON数据包：

{"access_token":"ACCESS_TOKEN","expires_in":7200,"token_type":"Bearer"}


示例
__________

Javascript
----------

.. code-block:: javascript
    :linenos:

    var form = new FormData();
    form.append("grant_type", "client_credentials");
    form.append("client_Id", "{client_Id}");
    form.append("client_secret", "{client_secret}");
    form.append("scope", "iot.cardservice.all iot.paymentservice.all");
    
    var settings = {
      "async": true,
      "crossDomain": true,
      "url": "https://ids.ixingban.com/connect/token",
      "method": "POST",
      "headers": {
        "cache-control": "no-cache"
      },
      "processData": false,
      "contentType": false,
      "mimeType": "multipart/form-data",
      "data": form
    }
    
    $.ajax(settings).done(function (response) {
      console.log(response);
    });

Java
----------

.. code-block:: java
    :linenos:

    OkHttpClient client = new OkHttpClient();

    MediaType mediaType = MediaType.parse("multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW");
    RequestBody body = RequestBody.create(mediaType, "------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"grant_type\"\r\n\r\nclient_credentials\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_Id\"\r\n\r\n{client_Id}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_secret\"\r\n\r\n{client_secret}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"scope\"\r\n\r\niot.cardservice.all iot.paymentservice.all\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--");
    Request request = new Request.Builder()
      .url("https://ids.ixingban.com/connect/token")
      .post(body)
      .addHeader("content-type", "multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW")
      .addHeader("cache-control", "no-cache")
      .addHeader("postman-token", "3e8b9126-7452-75ae-89c3-f0989642b29c")
      .build();
    
    Response response = client.newCall(request).execute();

Nodejs
----------

.. code-block:: javascript
    :linenos:

    var http = require("https");

    var options = {
      "method": "POST",
      "hostname": "ids.ixingban.com",
      "port": null,
      "path": "/connect/token",
      "headers": {
        "content-type": "multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW",
        "cache-control": "no-cache"
      }
    };
    
    var req = http.request(options, function (res) {
      var chunks = [];
    
      res.on("data", function (chunk) {
        chunks.push(chunk);
      });
    
      res.on("end", function () {
        var body = Buffer.concat(chunks);
        console.log(body.toString());
      });
    });
    
    req.write("------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"grant_type\"\r\n\r\nclient_credentials\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_Id\"\r\n\r\n{client_Id}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_secret\"\r\n\r\n{client_secret}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"scope\"\r\n\r\niot.cardservice.all iot.paymentservice.all\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--");
    req.end();

C#
----------

.. code-block:: csharp
    :linenos:

    var client = new RestClient("https://ids.ixingban.com/connect/token");
    var request = new RestRequest(Method.POST);
    request.AddHeader("postman-token", "2a34c6bb-6070-e1f9-5516-7e5c901579b7");
    request.AddHeader("cache-control", "no-cache");
    request.AddHeader("content-type", "multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW");
    request.AddParameter("multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW", "------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"grant_type\"\r\n\r\nclient_credentials\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_Id\"\r\n\r\n{client_Id}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_secret\"\r\n\r\n{client_secret}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"scope\"\r\n\r\niot.cardservice.all iot.paymentservice.all\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--", ParameterType.RequestBody);
    IRestResponse response = client.Execute(request);

PHP
----------

.. code-block:: php
    :linenos:

    <?php

    $request = new HttpRequest();
    $request->setUrl('https://ids.ixingban.com/connect/token');
    $request->setMethod(HTTP_METH_POST);
    
    $request->setHeaders(array(
      'cache-control' => 'no-cache',
      'content-type' => 'multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW'
    ));
    
    $request->setBody('------WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="grant_type"
    
    client_credentials
    ------WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="client_Id"
    
    {client_Id}
    ------WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="client_secret"
    
    {client_secret}
    ------WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="scope"
    
    iot.cardservice.all iot.paymentservice.all
    ------WebKitFormBoundary7MA4YWxkTrZu0gW--');
    
    try {
      $response = $request->send();
    
      echo $response->getBody();
    } catch (HttpException $ex) {
      echo $ex;
    }


Python
----------

.. code-block:: python
    :linenos:

    import http.client

    conn = http.client.HTTPSConnection("ids.ixingban.com")
    
    payload = "------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"grant_type\"\r\n\r\nclient_credentials\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_Id\"\r\n\r\n{client_Id}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_secret\"\r\n\r\n{client_secret}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"scope\"\r\n\r\niot.cardservice.all iot.paymentservice.all\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--"
    
    headers = {
        'content-type': "multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW",
        'cache-control': "no-cache"
        }
    
    conn.request("POST", "/connect/token", payload, headers)
    
    res = conn.getresponse()
    data = res.read()
    
    print(data.decode("utf-8"))


Go
----------

.. code-block:: go
    :linenos:

    package main

    import (
    	"fmt"
    	"strings"
    	"net/http"
    	"io/ioutil"
    )
    
    func main() {
    
    	url := "https://ids.ixingban.com/connect/token"
    
    	payload := strings.NewReader("------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"grant_type\"\r\n\r\nclient_credentials\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_Id\"\r\n\r\n{client_Id}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_secret\"\r\n\r\n{client_secret}\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"scope\"\r\n\r\niot.cardservice.all iot.paymentservice.all\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--")
    
    	req, _ := http.NewRequest("POST", url, payload)
    
    	req.Header.Add("content-type", "multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW")
    	req.Header.Add("cache-control", "no-cache")
    
    	res, _ := http.DefaultClient.Do(req)
    
    	defer res.Body.Close()
    	body, _ := ioutil.ReadAll(res.Body)
    
    	fmt.Println(res)
    	fmt.Println(string(body))
    
    }


Object-C
----------

.. code-block:: object-c
    :linenos:

    #import <Foundation/Foundation.h>

    NSDictionary *headers = @{ @"content-type": @"multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW",
                               @"cache-control": @"no-cache",
    NSArray *parameters = @[ @{ @"name": @"grant_type", @"value": @"client_credentials" },
                             @{ @"name": @"client_Id", @"value": @"{client_Id}" },
                             @{ @"name": @"client_secret", @"value": @"{client_secret}" },
                             @{ @"name": @"scope", @"value": @"iot.cardservice.all iot.paymentservice.all" } ];
    NSString *boundary = @"----WebKitFormBoundary7MA4YWxkTrZu0gW";
    
    NSError *error;
    NSMutableString *body = [NSMutableString string];
    for (NSDictionary *param in parameters) {
        [body appendFormat:@"--%@\r\n", boundary];
        if (param[@"fileName"]) {
            [body appendFormat:@"Content-Disposition:form-data; name=\"%@\"; filename=\"%@\"\r\n", param[@"name"], param[@"fileName"]];
            [body appendFormat:@"Content-Type: %@\r\n\r\n", param[@"contentType"]];
            [body appendFormat:@"%@", [NSString stringWithContentsOfFile:param[@"fileName"] encoding:NSUTF8StringEncoding error:&error]];
            if (error) {
                NSLog(@"%@", error);
            }
        } else {
            [body appendFormat:@"Content-Disposition:form-data; name=\"%@\"\r\n\r\n", param[@"name"]];
            [body appendFormat:@"%@", param[@"value"]];
        }
    }
    [body appendFormat:@"\r\n--%@--\r\n", boundary];
    NSData *postData = [body dataUsingEncoding:NSUTF8StringEncoding];
    
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"https://ids.ixingban.com/connect/token"]
                                                           cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                       timeoutInterval:10.0];
    [request setHTTPMethod:@"POST"];
    [request setAllHTTPHeaderFields:headers];
    [request setHTTPBody:postData];
    
    NSURLSession *session = [NSURLSession sharedSession];
    NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request
                                                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                                                    if (error) {
                                                        NSLog(@"%@", error);
                                                    } else {
                                                        NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *) response;
                                                        NSLog(@"%@", httpResponse);
                                                    }
                                                }];
    [dataTask resume];

cURL
----------

.. code-block:: curl
    :linenos:

    curl -X POST \
    https://ids.ixingban.com/connect/token \
    -H 'cache-control: no-cache' \
    -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
    -H 'postman-token: c47f7cfd-f478-d57d-5c13-0381885a4877' \
    -F grant_type=client_credentials \
    -F 'client_Id={client_Id}' \
    -F 'client_secret={client_secret}' \
    -F 'scope=iot.cardservice.all iot.paymentservice.all'