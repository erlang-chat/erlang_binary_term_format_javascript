# BERT-JS

For Other Languages

* [Swift](https://github.com/erlang-chat/erlang_binary_term_format_swift)
* [Android](https://github.com/erlang-chat/erlang_binary_term_format_android)
* [JavaScript](https://github.com/erlang-chat/erlang_binary_term_format_javascript)

### What is BERT?
BERT (Binary Erlang Term) is a format created by the Erlang development team for serializing Erlang terms, and promoted by Tom Preston-Werner as a way for different languages to communicate in a simple and efficient manner.

[Erlang External Term Format](http://erlang.org/doc/apps/erts/erl_ext_dist.html)

### What is BERT JS?

BERT-JS is a first cut Javascript implementation of the BERT protocol. In other words, using BERT-JS, you can serialize data into a binary format that can then be de-serialized by Erlang directly into an Erlang term.

### Limitations

* Decoding floats is not yet supported.

### Usage With WebSockets

SetUp Bert

```
    function setUpBert() {
        bert = new BertClass();
        bert.encodeObjectKeysAsNumber = true;
        bert.encodeStringAsBinary = true;


        //decodes erlang map binary value as String for 'key'
        bert.markDecodeBinaryAsStringForKey(key);
    }

```

```
    var WS = false;
    if (window.WebSocket) WS = WebSocket;
    if (!WS && window.MozWebSocket) WS = MozWebSocket;

    if (!WS) { return; }

    var websocket = new WS(serverUrl);
    websocket.binaryType = "arraybuffer";
    websocket.onmessage = onMessage;

```

Receive Json

```
    function onMessage(msg) {

        if (typeof msg.data == "string") {
            console.log("received data: ", msg.data);
            return;
        }

        var array = new Uint8Array(msg.data);
        var charData = bert.bytes_to_string(array);
        var json = bert.decode(charData);

        console.log("decoded", json);
    }
```

Send Json
```
    //json = {}; //json is object

    function send(json) {
        var encoded = bert.encode(Obj);
        var byteArray = bert.binary_to_list(encoded);
        websocket.send(new Uint8Array(byteArray));
    }
```
