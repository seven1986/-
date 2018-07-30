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
    :linenos:

     $(function () {
            // Declare a proxy to reference the hub.
            var chat = $.connection.chatHub;
            // Create a function that the hub can call to broadcast messages.
            chat.client.broadcastMessage = function (name, message) {
                // Html encode display name and message.
                var encodedName = $('<div />').text(name).html();
                var encodedMsg = $('<div />').text(message).html();
                // Add the message to the page.
                $('#discussion').append('<li><strong>' + encodedName
                    + '</strong>:&nbsp;&nbsp;' + encodedMsg + '</li>');
            };
            // Get the user name and store it to prepend to messages.
            $('#displayname').val(prompt('Enter your name:', ''));
            // Set initial focus to message input box.
            $('#message').focus();
            // Start the connection.
            $.connection.hub.start().done(function () {
                $('#sendmessage').click(function () {
                    // Call the Send method on the hub.
                    chat.server.send($('#displayname').val(), $('#message').val());
                    // Clear text box and reset focus for next comment.
                    $('#message').val('').focus();
                });
            });
        });