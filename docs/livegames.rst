实时游戏
===========

.. Note::

    调用前需要先安装signalr包，然后引用signalr.min.js文件:

    - npm install @aspnet/signalr

    - 猜拳hub地址：https://campaigncore-gameresourcev5.chinacloudsites.cn/cqhub

    - 你画我猜hub地址：https://campaigncore-gameresourcev5.chinacloudsites.cn/drawinghub


Javascript
----------

.. code-block:: javascript

     const connection = new signalR.HubConnectionBuilder()
    .withUrl('{hub地址}') //这里配置hub地址
    .build();
    // 猜拳 - /cQHub
    //你画我猜 - /drawingHub
    
    // 接受信息，统一的名字"ReceiveMessage"
    connection.on("ReceiveMessage", (userName, message) => {
        const msg = message.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
        const encodedMsg = userName + " says " + msg;
        const li = document.createElement("li");
        li.textContent = encodedMsg;
        document.getElementById("messagesList").appendChild(li);
    });

    connection.start().catch(err => console.error(err.toString()));

    // 发信息到群组,统一的名字"SendMessageToGroup"
    document.getElementById("sendButton").addEventListener("click", event => {
        const groupName = document.getElementById("groupName").value;
        const userName = document.getElementById("userInput").value;
        const message = document.getElementById("messageInput").value;
        connection.invoke("SendMessageToGroup", groupName, userName, message).catch(err => console.error(err.toString()));
        event.preventDefault();
    });
    
    // 加入群组,统一的名字"AddToGroup (string groupName, string userName, bool creator)"
    // creator - 是否是建群人
    document.getElementById("joinButton").addEventListener("click", event => {
        const groupName = document.getElementById("groupName").value;
        const userName = document.getElementById("userInput").value;
        connection.invoke("AddToGroup", groupName, userName, 1).catch(err => console.error(err.toString()));
        event.preventDefault();
    });
    
    // 离开群组,统一的名字"RemoveFromGroup"
    document.getElementById("leaveButton").addEventListener("click", event => {
        const groupName = document.getElementById("groupName").value;
        const userName = document.getElementById("userInput").value;
        connection.invoke("RemoveFromGroup", groupName, userName).catch(err => console.error(err.toString()));
        event.preventDefault();
    });

