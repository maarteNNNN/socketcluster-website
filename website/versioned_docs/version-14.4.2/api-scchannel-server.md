---
id: version-14.4.2-api-scchannel-server
title: API - SCChannel (Server)
sidebar_label: SCChannel (server, worker)
original_id: api-scchannel-server
---

Instances of the SCChannel object let you subscribe to and watch for incoming data which is published on that channel by other clients or publishers.
This object is the server-side equivalent to the client-side [SCChannel](api-scchannel-client) object.

### Inherits from:

[Emitter](https://github.com/component/emitter)

### Properties:

<table>
    <tr>
        <td>name</td>
        <td>The channel's name. Must be a string.</td>
    </tr>
    <tr>
        <td>state</td>
        <td>Holds a string which indicates the state of this channel. Can be 'subscribed', 'unsubscribed' or 'pending'.</td>
    </tr>
    <tr>
        <td>waitForAuth</td>
        <td>A boolean which indicates whether or not this channel will wait for socket authentication before subscribing to the server. It will remain in the 'pending' state until the socket becomes authenticated.</td>
    </tr>
    <tr>
        <td>SUBSCRIBED</td>
        <td>A string constant which is used to indicate that this channel is in a subscribed state. See getState() method.</td>
    </tr>
    <tr>
        <td>PENDING</td>
        <td>A string constant which is used to indicate that this channel is in a pending state. See getState() method.</td>
    </tr>
    <tr>
        <td>UNSUBSCRIBED</td>
        <td>A string constant which is used to indicate that this channel is in an unsubscribed state. See getState() method.</td>
    </tr>
</table>

### Events:

<table>
    <tr>
        <td>'dropOut'</td>
        <td>This is emitted whenever the subscription to this channel is lost.</td>
    </tr>
    <tr>
        <td>'subscribe'</td>
        <td>When the subscription succeeds.</td>
    </tr>
    <tr>
        <td>'subscribeFail'</td>
        <td>Happens when the subscription fails.</td>
    </tr>
    <tr>
        <td>'unsubscribe'</td>
        <td>When the channel becomes deactivated (unsubscribed from the server).</td>
    </tr>
    <tr>
        <td>'subscribeStateChange'</td>
        <td>Whenever the channel's subscription state changes. The handler received an object with three properties: channel, oldState and newState.</td>
    </tr>
</table>

### Methods:

<table>
    <tr>
        <td>getState()</td>
        <td>Returns the state of the channel as a string. Can be channel.SUBSCRIBED, channel.PENDING or channel.UNSUBSCRIBED.</td>
    </tr>
    <tr>
        <td>subscribe()</td>
        <td>Activate this channel so that it will receive all data published to it from the backend.</td>
    </tr>
    <tr>
        <td>unsubscribe()</td>
        <td>Deactivate this channel.</td>
    </tr>
    <tr>
        <td>isSubscribed([includePending])</td>
        <td>Check whether or not this channel is active (subscribed to the backend). The includePending argument is optional, if true, the function will return true if the channel subscription is in a pending state.</td>
    </tr>
    <tr>
        <td>publish(data, [callback])</td>
        <td>
            Publish data to this channel.
            The optional callback is in the form callback(err).
        </td>
    </tr>
    <tr>
        <td>watch(handler)</td>
        <td>Capture any data which is published to this channel. The handler is a function in the form handler(data).</td>
    </tr>
    <tr>
        <td>unwatch([handler])</td>
        <td>Unbind the specified handler from this channel. If no handler is specified, it will unbind all handlers.</td>
    </tr>
    <tr>
        <td>watchers()</td>
        <td>Get a list of functions which are currently observing this channel.</td>
    </tr>
    <tr>
        <td>destroy()</td>
        <td>Destroy the current SCChannel object - This makes it unusable and it will allow it to be garbage collected.</td>
    </tr>
</table>
