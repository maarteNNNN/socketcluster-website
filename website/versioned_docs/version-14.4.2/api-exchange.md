---
id: version-14.4.2-api-exchange
title: API - SCExchange (SCBroker)
sidebar_label: SCExchange (server, worker)
original_id: api-exchange
---

Can be accessed using SCWorker.exchange property.

The exchange object is a top-level scBroker client which lets you publish events and manipulate data within your brokers - It represents a cluster of 1 or more brokers.
You can use any method of an scBroker client on instances of this object (see [scBroker Client](https://github.com/SocketCluster/sc-broker#client-methods)).
Note that the hashing function used by the SCExchange object to shard actions/channels accross brokers can be found in the [sc-hasher](https://www.npmjs.com/package/sc-hasher)
module - You can use this function in your code to help with sharding (advanced use case).
See [this comment](https://github.com/SocketCluster/socketcluster/issues/245#issuecomment-261041729) for details.

### Inherits from:

[EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter)

### Events:

<table>
    <tr>
        <td>'subscribe'</td>
        <td>When the subscription succeeds.</td>
    </tr>
    <tr>
        <td>'subscribeFail'</td>
        <td>Happens when the subscription fails. The first argument passed to the handler will be the error which caused the subscription to fail.</td>
    </tr>
    <tr>
        <td>'unsubscribe'</td>
        <td>When the exchange becomes unsubscribed from a channel.</td>
    </tr>
</table>

### Methods:

<table>
    <tr>
        <td colspan="2">
            <p>
                <i style="color: #999;">
                    If you're running the full version of SC (not just the standalone <a href="https://github.com/SocketCluster/socketcluster-server">socketcluster-server</a>),
                    this <code>SCExchange</code> object will inherit all the methods of the scBroker Client documented <a href="https://github.com/SocketCluster/sc-broker#client-methods">here</a>.
                    So for example, if you want to store data to share accross all your worker processes, you could call
                    <code>worker.exchange.set('myCustomVariable', {foo: 'Hello world'}, function (err) { /*...*/ })</code>.
                    Then <code>worker.exchange.get('myCustomVariable', function (err, data) { /* data will be {foo: 'Hello world'} */ })</code>.
                    Make sure that <code>err</code> is handled within the callback if it exists.
                </i>
            </p>
        </td>
    </tr>
    <tr>
        <td>publish(channel, data, [callback])</td>
        <td>
            Publish any JSON-compatible object/array or primitive to channel.
            Optional callback is in form callback(err) - Where err is undefined on success.
        </td>
    </tr>
    <tr>
        <td>subscribe(channelName)</td>
        <td>
            Subscribe this scBroker exchange client to the specified channel.
            This function returns a <a href="/docs/14.4.2/api-scchannel-server">Channel</a> object which lets you watch for incoming data on that channel.
        </td>
    </tr>
    <tr>
        <td>unsubscribe(channelName, [callback])</td>
        <td>
            Unsubscribe this scBroker exchange client from the specified channel.
            You can reactivate the <a href="/docs/14.4.2/api-scchannel-server">Channel</a> object by calling subscribe(channelName) again at a later time.
        </td>
    </tr>
    <tr>
        <td>channel(channelName)</td>
        <td>
            Returns a Channel instance.
            This is different from subscribe() in that it will not try to subscribe to that channel.
            The returned channel will be inactive initially. You can call channel.subscribe() later to activate that channel when required.
        </td>
    </tr>
    <tr>
        <td>watch(channel, handler)</td>
        <td>
            Capture data that is published on a particular channel.
            The handler function must be in the form: handler(data)
        </td>
    </tr>
    <tr>
        <td>unwatch(channel, [handler])</td>
        <td>
            Stop capturing data from channel.
            The handler function is options. If none is specified, it will remove all handlers from that channel.
        </td>
    </tr>
    <tr>
        <td>watchers(channel)</td>
        <td>
            Returns an array of all handler functions which are bound to channel.
        </td>
    </tr>
    <tr>
        <td>destroyChannel(channelName)</td>
        <td>
            Destroys the specified SCChannel object - This makes it unusable and will allow it to be garbage collected.
        </td>
    </tr>
    <tr>
        <td>
            subscriptions(includePending)
        </td>
        <td>
            Returns an array of active channel subscriptions which this exchange client is bound to.
            If includePending is true, pending subscriptions will also be included in the list.
        </td>
    </tr>
    <tr>
        <td>
            isSubscribed(channelName, [includePending])
        </td>
        <td>
            Check if exchange client is subscribed to channelName.
            If includePending is true, pending subscriptions will also be included in the list.
        </td>
    </tr>
    <tr>
        <td>run(queryFn,[data,] callback)</td>
        <td>
            This method is a wrapper around the <a href="https://github.com/SocketCluster/sc-broker#run">scBroker run</a> method.
            The queryFn function will be executed directly on one of your brokers and the result will be passed back to your callback.
            If your SC instance has more than one broker, the queryFn will be executed on the first one by default - In order to spread
            the load evenly between all your brokers, you should add a <b>mapIndex</b> key to your queryFn before you pass it to the run method.
            For example, if you wanted to map to a specific broker based on a string stored in channelName you would just go: queryFn.mapIndex = channelName;
            then you just pass it to exchange.run(queryFn, ...) as normal.
        </td>
    </tr>
    <tr>
        <td>setMapper(mapper)</td>
        <td>
            Allows you to define a mapping function which is used to map specific broker actions to specific broker processes in order to
            distribute the load between them (assuming you have more than 1 broker). This mapping function is particularly important
            for the subscribe and publish actions as it decides which brokers will handle what channel names. This is only for advanced use cases.
        </td>
    </tr>
    <tr>
        <td>getMapper()</td>
        <td>
            Returns the current mapping function which is used to map specific broker actions to specific broker processes.
            For the publish and subscribe actions, the default mapping function will shard channels such that each broker will
            handle approximately (1/n)th of all channels (where n is the number of brokers). This is only for advanced use cases.
        </td>
    </tr>
    <tr>
        <td>send(data, mapIndex, callback)</td>
        <td>
            Send data to one or more brokers. mapIndex can be a number, a string, an array of strings or null.
            If mapIndex is a number, it will cause the message to be sent to the broker with that id.
            If mapIndex is a string, it will be hashed to a number and the message will be sent to the relevant broker.
            If mapIndex is an array (of strings or numbers), the message will be sent to multiple brokers.
            If mapIndex is null, the message will be sent to all brokers.
            You can listen for the data on the broker (in broker.js) using <code>broker.on('message', function (data, respond) {})</code> -
            The respond function is a callback which can be invoked with <code>respond(err, data)</code>.
        </td>
    </tr>
</table>

Because the exchange object potentially handles a cluster of local brokers, you can optionally use the
setMapper(mapperFn) method to set a function which will be used to map individual scBroker actions to one of your
brokers (assuming you have more than one). Changing the default mapper is an advanced use case though and you generally don't need to do it.
The mapperFn takes three arguments:

<table>
    <tr>
        <td>key</td>
        <td>
            The key/keyChain (typically string or Array) which is was passed to this scBroker client method/action.
            If the method is 'query' then the key will be an scBroker query function which you will have to interpret in order to come up with a mapping.
        </td>
    </tr>
    <tr>
        <td>method</td>
        <td>The scBroker method which was invoked and needs to be mapped to one or more broker processes.</td>
    </tr>
    <tr>
        <td>clientIds</td>
        <td>An array of available scBroker client IDs which you can map to.</td>
    </tr>
</table>

The mapper must return either an ID (integer) or an array of IDs and SC will send the action to all corresponding brokers.
So if the mapperFn returns 0, then the action wil be sent to scBroker broker with ID 0.
If you return the clientIds array directly, then all clients will receive the action.