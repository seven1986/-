实时游戏
===========

.. Note::

    接口调用前需要引入:

    - http://signalrchat20180727022234.chinacloudsites.cn/Scripts/jquery-1.6.4.min.js

    - http://signalrchat20180727022234.chinacloudsites.cn/Scripts/jquery.signalR-2.0.0.js
    
    - http://signalrchat20180727022234.chinacloudsites.cn/signalr/hubs


Javascript
----------

.. code-block:: javascript

     const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
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
    
    // 加入群组,统一的名字"AddToGroup"
    document.getElementById("joinButton").addEventListener("click", event => {
        const groupName = document.getElementById("groupName").value;
        const userName = document.getElementById("userInput").value;
        connection.invoke("AddToGroup", groupName, userName).catch(err => console.error(err.toString()));
        event.preventDefault();
    });
    
    // 离开群组,统一的名字"RemoveFromGroup"
    document.getElementById("leaveButton").addEventListener("click", event => {
        const groupName = document.getElementById("groupName").value;
        const userName = document.getElementById("userInput").value;
        connection.invoke("RemoveFromGroup", groupName, userName).catch(err => console.error(err.toString()));
        event.preventDefault();
    });

