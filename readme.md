#Pubsubclient API Documentation #
本文档由内含子编制，用于Pubsubclient库所有API的介绍，方便在Arduino进行MQTT开发时查阅使用，如有遗漏错误请联系我。
## 构造函数 ##
    PubSubClient ()
    PubSubClient (client)
    PubSubClient (server, port, [callback], client, [stream])
## 功能函数 ##
1.用来连接的函数

    boolean connect (clientID)
    boolean connect (clientID, willTopic, willQoS, willRetain, willMessage)
    boolean connect (clientID, username, password)
    boolean connect (clientID, username, password, willTopic, willQoS, willRetain, willMessage)
    boolean connect (clientID, username, password, willTopic, willQoS, willRetain, willMessage, cleanSession)

2.断开连接的函数

    void disconnect ()

3.发布消息的函数

    int publish (topic, payload)
    int publish (topic, payload, retained)
    int publish (topic, payload, length)
    int publish (topic, payload, length, retained)
    int publish_P (topic, payload, length, retained)
    boolean beginPublish (topic, payloadLength, retained)
    size_t write (uint8_t)
    size_t write (payload, length)
    boolean endPublish ()

4.订阅主题的函数

    boolean subscribe (topic, [qos])
    boolean unsubscribe (topic)

5.客户端状态函数

    boolean loop ()
    int connected ()
    int state ()

6.客户端配置函数

    PubSubClient setServer (server, port)
    PubSubClient setCallback (callback)
    PubSubClient setClient (client)
    PubSubClient setStream (stream)

## 函数详解 ##


**PubSubClient ()**

创建未初始化的客户端实例。<br>
在使用之前，必须使用属性设置器进行配置：

    EthernetClient ethClient;
    PubSubClient client;
    
    void setup() {
    client.setClient(ethClient);
    client.setServer("broker.example.com",1883);
    // 客户端现已配置使用
    }
***

**PubSubClient (client)**

创建部分初始化的客户端实例。<br>
在使用之前，必须配置服务器详细信息：

    EthernetClient ethClient;
    PubSubClient client(ethClient);
    
    void setup() {
    client.setServer("broker.example.com",1883);
    // 客户端现在可以使用了
    }
    
参数：<br>
client : Client的一个实例，通常是EthernetClient类型。
***

**PubSubClient (server, port, [callback], client, [stream])**

创建完全配置的客户端实例。

参数：<br>
server : 服务器的地址（IPAddress，uint8_t []或const char []）。<br>
port : 要连接的端口（int）。<br>
callback : 可选指向消息到达此客户端创建的订阅时调用的消息回调函数的指针。<br>
client : Client的一个实例，通常是EthernetClient类型。<br>
stream : 可选的实例Stream，用于存储收到的消息。<br>
***

**boolean connect (clientID)**

连接客户端。

参数：<br>
clientID : 连接到服务器时使用的客户端ID。<br>
返回：<br>
false - 连接失败。<br>
true - 连接成功。<br>
***

**boolean connect (clientID, willTopic, willQoS, willRetain, willMessage)**

使用指定的Will消息连接客户端。

参数：<br>
clientID : 连接到服务器时使用的客户端ID。<br>
willTopic : will消息使用的主题（const char []）。<br>
willQoS : will消息使用的服务质量（int：0,1或2）。<br>
willRetain : 是否应该使用retain标志发布will（boolean）。<br>
willMessage : will消息的有效负载（const char []）。<br>
返回：<br>
false - 连接失败。<br>
true - 连接成功。<br>
***

**boolean connect (clientID, username, password)**

使用指定的用户名和密码连接客户端。

参数：<br>
clientID : 连接到服务器时使用的客户端ID。<br>
username : 要使用的用户名。如果为NULL，则不使用用户名或密码（const char []）。<br>
password : 要使用的密码。如果为NULL，则不使用密码（const char []）。<br>
返回：<br>
false - 连接失败。<br>
true - 连接成功。<br>
***

**boolean connect (clientID, username, password, willTopic, willQoS, willRetain, willMessage)**

使用指定的Will消息，用户名和密码连接客户端。

参数：<br>
clientID : 连接到服务器时使用的客户端ID。<br>
username : 要使用的用户名。如果为NULL，则不使用用户名或密码（const char []）。<br>
password : 要使用的密码。如果为NULL，则不使用密码（const char []）。<br>
willTopic : will消息使用的主题（const char []）。<br>
willQoS : will消息使用的服务质量（int：0,1或2）。<br>
willRetain : 是否应该使用retain标志发布遗嘱消息（boolean）。<br>
willMessage : will消息的有效负载（const char []）。<br>
返回：<br>
false - 连接失败。<br>
true - 连接成功。<br>
***

**boolean connect (clientID, username, password, willTopic, willQoS, willRetain, willMessage, cleanSession)**

使用指定的Will消息，用户名，密码和clean-session标志连接客户端。

Note : 即使cleanSession设置为false/ 0客户端也不会重试qos 1发布失败。此标志仅用于维护代理上的订阅。

参数：<br>
clientID : 连接到服务器时使用的客户端ID。<br>
username : 要使用的用户名。如果为NULL，则不使用用户名或密码（const char []）。<br>
password : 要使用的密码。如果为NULL，则不使用密码（const char []）。<br>
willTopic : will消息使用的主题（const char []）。<br>
willQoS : will消息使用的服务质量（int：0,1或2）。<br>
willRetain : 是否应该使用retain标志发布will（boolean）。<br>
willMessage : will消息的有效负载（const char []）。<br>
cleanSession : 是否连接clean-session（boolean）。<br>
返回：<br>
false - 连接失败。<br>
true - 连接成功。<br>
***

**void disconnect ()**

断开客户端连接。
***

**int publish (topic, payload)**

将字符串消息发布到指定的主题。

参数：<br>
topic - 要发布到的主题（const char []）。<br>
payload -  要发布的消息（const char []）。<br>
返回：<br>
false - 发布失败，连接丢失或消息太大。<br>
true - 发布成功。<br>
***

**int publish (topic, payload, retained)**

将字符串消息发布到指定的主题。

参数：<br>
topic - 要发布到的主题（const char []）。<br>
payload -  要发布的消息（const char []）。<br>
retained - 是否应保留消息 (boolean)，false - 不保留。<br>
返回：<br>
false - 发布失败，连接丢失或消息太大。<br>
true - 发布成功。<br>
***

**int publish (topic, payload, length)**

将消息发布到指定的主题。

参数：<br>
topic - 要发布到的主题（const char []）。<br>
payload -  要发布的消息（byte []）。<br>
length - 消息的长度 (byte)。<br>
返回：<br>
false - 发布失败，连接丢失或消息太大。<br>
true - 发布成功。<br>
***

**int publish (topic, payload, length, retained)**

将消息发布到指定的主题，并使用指定的保留标志

参数：<br>
topic - 要发布到的主题（const char []）。<br>
payload -  要发布的消息（byte []）。<br>
length - 消息的长度 (byte)。<br>
retained - 是否应保留消息 (boolean)，false - 不保留。<br>
返回：<br>
false - 发布失败，连接丢失或消息太大。<br>
true - 发布成功。<br>
***

**int publish_P (topic, payload, length, retained)**

发布存储在程序中指定主题中的消息，并指定保留标志。

参数：<br>
topic - 要发布到的主题（const char []）。<br>
payload -  要发布的消息（PROGMEM byte []）。<br>
length - 消息的长度 (byte)。<br>
retained - 是否应保留消息 (boolean)，false - 不保留。<br>
返回：<br>
false - 发布失败。<br>
true - 发布成功。<br>
***

**boolean beginPublish (topic, payloadLength, retained)**

开始发送发布消息。消息的有效负载由一个或多个写入调用提供，然后调用EndPublish。

参数：<br>
topic - 要发布到的主题（const char []）。<br>
payloadLength - 要发布的消息的长度。
retained - 是否应保留消息 (boolean)，false - 不保留。<br>
返回：<br>
false - 发布失败。<br>
true - 发布成功。<br>
***

**size_t write (uint8_t)**

写入一个字节数组作为 beginPublish的内容.

参数：<br>
uint8_t - 要写入的字节<br>
返回：<br>
false - 发布失败。<br>
true - 发布成功。<br>
***

**size_t write (payload, length)**

写入一个字节数组作为beginPublish的内容.

参数：<br>
payload -  要写入的字节数 (byte[])。<br>
length - 字节数组的长度 (byte)。<br>
返回：<br>
false - 发布失败。<br>
true - 发布成功。<br>
***

**boolean endPublish ()**

调用完beginPublish后调用此函数。

返回：<br>
false - 发布失败。<br>
true - 发布成功。<br>
***

**boolean subscribe (topic, [qos])**

订阅发布到指定主题的消息。

参数：<br>
topic - 要订阅的主题 (const char[])。<br>
qos - 可选择要订阅的qos（int：0或1）。<br>
返回：<br>
false - 发送订阅失败，连接丢失或消息太大。<br>
true - 发送订阅成功。请求以异步方式完成。<br>
***

**boolean unsubscribe （topic）**

取消订阅指定的主题。

参数：<br>
topic - 取消订阅的主题 (const char[])。<br>
返回：<br>
false - 发送订阅失败，连接丢失或消息太大。<br>
true - 发送订阅成功。请求以异步方式完成。<br>
***

**boolean loop ()** 

应定期调用此方法以允许客户端处理传入消息并维护其与服务器的连接。

返回：<br>
false - 客户端不再连接。<br>
true - 客户端仍然连接。<br>
***

**int connected ()**

检查客户端是否已连接到服务器。

返回：<br>
false - 客户端不再连接。<br>
true - 客户端仍然连接。<br>
***

**int state ()**

返回客户端的当前状态。如果连接尝试失败，则可以使用此方法获取有关失败的更多信息。

返回：<br>
int - 客户端状态，可以采用以下值 (常量定义在 PubSubClient.h):<br>
-4 : MQTT_ CONNECTION_ TIMEOUT - 服务器在保持活动时间内没有响应。<br>
-3 : MQTT_ CONNECTION_ LOST - 网络连接中断。<br>
-2 : MQTT_ CONNECT_ FAILED - 网络连接失败。<br>
-1 : MQTT_ DISCONNECTED - 客户端干净地断开连接。<br>
0 : MQTT_ CONNECTED - 客户端已连接。<br>
1 : MQTT_ CONNECT_ BAD_ PROTOCOL - 服务器不支持请求的MQTT版本。<br>
2 : MQTT_ CONNECT_ BAD_ CLIENT_ ID - 服务器拒绝了客户端标识符。<br>
3 : MQTT_ CONNECT_ UNAVAILABLE - 服务器无法接受连接。<br>
4 : MQTT_ CONNECT_ BAD_ CREDENTIALS - 用户名/密码被拒绝。<br>
5 : MQTT_ CONNECT_ UNAUTHORIZED - 客户端无权连接。<br>

**PubSubClient setServer (server, port)**

设置服务器详细信息。

参数：<br>
server : 服务器的地址（IPAddress，uint8_t []或const char []）。<br>
port : 要连接的端口（int）。<br>
返回：<br>
PubSubClient - 客户端实例，允许链接函数。<br>
***

**PubSubClient setCallback (callback)**

设置消息回调函数。

参数：<br>
callback :指向消息到达此客户端创建的订阅时调用的消息回调函数的指针。<br>
返回：<br>
PubSubClient - 客户端实例，允许链接函数。<br>
***

**PubSubClient setClient (client)**

设置客户端。

参数：<br>
client : Client的一个实例，通常是EthernetClient类型。<br>
返回：<br>
PubSubClient - 客户端实例，允许链接函数。<br>
***

**PubSubClient setStream (stream)**

设置流。

参数：
stream : Stream用于存储接收消息的实例。
返回：<br>
PubSubClient - 客户端实例，允许链接函数。<br>


有关mqtt_stream的例程：
    
    #include <SPI.h>
    #include <Ethernet.h>
    #include <PubSubClient.h>
    #include <SRAM.h>
    
    // Update these with values suitable for your network.
    byte mac[]= {  0xDE, 0xED, 0xBA, 0xFE, 0xFE, 0xED };
    IPAddress ip(172, 16, 0, 100);
    IPAddress server(172, 16, 0, 2);
    
    SRAM sram(4, SRAM_1024);
    
    void callback(char* topic, byte* payload, unsigned int length) {
      sram.seek(1);
    
      // do something with the message
      for(uint8_t i=0; i<length; i++) {
    Serial.write(sram.read());
      }
      Serial.println();
    
      // Reset position for the next message to be stored
      sram.seek(1);
    }
    
    EthernetClient ethClient;
    PubSubClient client(server, 1883, callback, ethClient, sram);
    
    void setup()
    {
      Ethernet.begin(mac, ip);
      if (client.connect("arduinoClient")) {
    client.publish("outTopic","hello world");
    client.subscribe("inTopic");
      }
    
      sram.begin();
      sram.seek(1);
    
      Serial.begin(9600);
    }
    
    void loop()
    {
      client.loop();
    }
    


## 更多配置选项 ##

以下配置选项可用于配置库。它们包含在PubSubClient.h。

**MQTT_ MAX_ PACKET_ SIZE<br>**
设置客户端将处理的最大数据包大小（以字节为单位）。收到的超过此大小的任何数据包都将被忽略。<br>
默认值：128个字节<br>

**MQTT_ KEEPALIVE<br>**
设置客户端将使用的keepalive间隔（以秒为单位）。这用于在没有发送或接收其他数据包时维持连接。<br>
默认值：15秒<br>

**MQTT_ VERSION<br>**
设置要使用的MQTT协议的版本。<br>
默认值：MQTT 3.1.1<br>

**MQTT_ MAX_ TRANSFER_ SIZE<br>**
设置每次写入调用中传递给网络客户端的最大字节数。某些硬件限制了可以一次传递给他们的数据，例如Arduino Wifi Shield。<br>
默认值：undefined（在每次写入调用中传递完整数据包）<br>

**MQTT_ SOCKET_ TIMEOUT<br>**
设置从网络读取时的超时。这也适用于调用的超时connect。<br>
默认值：15秒<br>

## 订阅回调 ##

如果客户端用于订阅主题，则必须在构造函数中提供回调函数。当新消息到达客户端时，将调用此函数。

回调函数具有以下签名：

    void callback(const char[] topic, byte* payload, unsigned int length)

参数：<br>
topic -  消息到达的主题 (const char[])。<br>
payload - 消息有效负载 (byte array)。<br>
length - 消息有效负载的长度 (unsigned int)。<br>

在内部，客户端对入站和出站消息使用相同的缓冲区。在回调函数返回之后，或者如果从回调函数中调用其中任何一个publish 或之后subscribe，将覆盖传递给函数的topic 和payload值。如果超出此要求，应用程序应创建自己的值副本。

## PubSubClient.h文件代码 ##

    /*
     PubSubClient.h - A simple client for MQTT.
      Nick O'Leary
      http://knolleary.net
    */
    
    #ifndef PubSubClient_h
    #define PubSubClient_h
    
    #include <Arduino.h>
    #include "IPAddress.h"
    #include "Client.h"
    #include "Stream.h"
    
    #define MQTT_VERSION_3_1  3
    #define MQTT_VERSION_3_1_14
    
    // MQTT_VERSION : Pick the version
    //#define MQTT_VERSION MQTT_VERSION_3_1
    #ifndef MQTT_VERSION
    #define MQTT_VERSION MQTT_VERSION_3_1_1
    #endif
    
    // MQTT_MAX_PACKET_SIZE : Maximum packet size
    #ifndef MQTT_MAX_PACKET_SIZE
    #define MQTT_MAX_PACKET_SIZE 128
    #endif
    
    // MQTT_KEEPALIVE : keepAlive interval in Seconds
    #ifndef MQTT_KEEPALIVE
    #define MQTT_KEEPALIVE 15
    #endif
    
    // MQTT_SOCKET_TIMEOUT: socket timeout interval in Seconds
    #ifndef MQTT_SOCKET_TIMEOUT
    #define MQTT_SOCKET_TIMEOUT 15
    #endif
    
    // MQTT_MAX_TRANSFER_SIZE : limit how much data is passed to the network client
    //  in each write call. Needed for the Arduino Wifi Shield. Leave undefined to
    //  pass the entire MQTT packet in each write call.
    //#define MQTT_MAX_TRANSFER_SIZE 80
    
    // Possible values for client.state()
    #define MQTT_CONNECTION_TIMEOUT -4
    #define MQTT_CONNECTION_LOST-3
    #define MQTT_CONNECT_FAILED -2
    #define MQTT_DISCONNECTED   -1
    #define MQTT_CONNECTED   0
    #define MQTT_CONNECT_BAD_PROTOCOL1
    #define MQTT_CONNECT_BAD_CLIENT_ID   2
    #define MQTT_CONNECT_UNAVAILABLE 3
    #define MQTT_CONNECT_BAD_CREDENTIALS 4
    #define MQTT_CONNECT_UNAUTHORIZED5
    
    #define MQTTCONNECT 1 << 4  // Client request to connect to Server
    #define MQTTCONNACK 2 << 4  // Connect Acknowledgment
    #define MQTTPUBLISH 3 << 4  // Publish message
    #define MQTTPUBACK  4 << 4  // Publish Acknowledgment
    #define MQTTPUBREC  5 << 4  // Publish Received (assured delivery part 1)
    #define MQTTPUBREL  6 << 4  // Publish Release (assured delivery part 2)
    #define MQTTPUBCOMP 7 << 4  // Publish Complete (assured delivery part 3)
    #define MQTTSUBSCRIBE   8 << 4  // Client Subscribe request
    #define MQTTSUBACK  9 << 4  // Subscribe Acknowledgment
    #define MQTTUNSUBSCRIBE 10 << 4 // Client Unsubscribe request
    #define MQTTUNSUBACK11 << 4 // Unsubscribe Acknowledgment
    #define MQTTPINGREQ 12 << 4 // PING Request
    #define MQTTPINGRESP13 << 4 // PING Response
    #define MQTTDISCONNECT  14 << 4 // Client is Disconnecting
    #define MQTTReserved15 << 4 // Reserved
    
    #define MQTTQOS0(0 << 1)
    #define MQTTQOS1(1 << 1)
    #define MQTTQOS2(2 << 1)
    
    // Maximum size of fixed header and variable length size header
    #define MQTT_MAX_HEADER_SIZE 5
    
    #if defined(ESP8266) || defined(ESP32)
    #include <functional>
    #define MQTT_CALLBACK_SIGNATURE std::function<void(char*, uint8_t*, unsigned int)> callback
    #else
    #define MQTT_CALLBACK_SIGNATURE void (*callback)(char*, uint8_t*, unsigned int)
    #endif
    
    #define CHECK_STRING_LENGTH(l,s) if (l+2+strlen(s) > MQTT_MAX_PACKET_SIZE) {_client->stop();return false;}
    
    class PubSubClient : public Print {
    private:
       Client* _client;
       uint8_t buffer[MQTT_MAX_PACKET_SIZE];
       uint16_t nextMsgId;
       unsigned long lastOutActivity;
       unsigned long lastInActivity;
       bool pingOutstanding;
       MQTT_CALLBACK_SIGNATURE;
       uint16_t readPacket(uint8_t*);
       boolean readByte(uint8_t * result);
       boolean readByte(uint8_t * result, uint16_t * index);
       boolean write(uint8_t header, uint8_t* buf, uint16_t length);
       uint16_t writeString(const char* string, uint8_t* buf, uint16_t pos);
       // Build up the header ready to send
       // Returns the size of the header
       // Note: the header is built at the end of the first MQTT_MAX_HEADER_SIZE bytes, so will start
       //   (MQTT_MAX_HEADER_SIZE - <returned size>) bytes into the buffer
       size_t buildHeader(uint8_t header, uint8_t* buf, uint16_t length);
       IPAddress ip;
       const char* domain;
       uint16_t port;
       Stream* stream;
       int _state;
    public:
       PubSubClient();
       PubSubClient(Client& client);
       PubSubClient(IPAddress, uint16_t, Client& client);
       PubSubClient(IPAddress, uint16_t, Client& client, Stream&);
       PubSubClient(IPAddress, uint16_t, MQTT_CALLBACK_SIGNATURE,Client& client);
       PubSubClient(IPAddress, uint16_t, MQTT_CALLBACK_SIGNATURE,Client& client, Stream&);
       PubSubClient(uint8_t *, uint16_t, Client& client);
       PubSubClient(uint8_t *, uint16_t, Client& client, Stream&);
       PubSubClient(uint8_t *, uint16_t, MQTT_CALLBACK_SIGNATURE,Client& client);
       PubSubClient(uint8_t *, uint16_t, MQTT_CALLBACK_SIGNATURE,Client& client, Stream&);
       PubSubClient(const char*, uint16_t, Client& client);
       PubSubClient(const char*, uint16_t, Client& client, Stream&);
       PubSubClient(const char*, uint16_t, MQTT_CALLBACK_SIGNATURE,Client& client);
       PubSubClient(const char*, uint16_t, MQTT_CALLBACK_SIGNATURE,Client& client, Stream&);
    
       PubSubClient& setServer(IPAddress ip, uint16_t port);
       PubSubClient& setServer(uint8_t * ip, uint16_t port);
       PubSubClient& setServer(const char * domain, uint16_t port);
       PubSubClient& setCallback(MQTT_CALLBACK_SIGNATURE);
       PubSubClient& setClient(Client& client);
       PubSubClient& setStream(Stream& stream);
    
       boolean connect(const char* id);
       boolean connect(const char* id, const char* user, const char* pass);
       boolean connect(const char* id, const char* willTopic, uint8_t willQos, boolean willRetain, const char* willMessage);
       boolean connect(const char* id, const char* user, const char* pass, const char* willTopic, uint8_t willQos, boolean willRetain, const char* willMessage);
       boolean connect(const char* id, const char* user, const char* pass, const char* willTopic, uint8_t willQos, boolean willRetain, const char* willMessage, boolean cleanSession);
       void disconnect();
       boolean publish(const char* topic, const char* payload);
       boolean publish(const char* topic, const char* payload, boolean retained);
       boolean publish(const char* topic, const uint8_t * payload, unsigned int plength);
       boolean publish(const char* topic, const uint8_t * payload, unsigned int plength, boolean retained);
       boolean publish_P(const char* topic, const char* payload, boolean retained);
       boolean publish_P(const char* topic, const uint8_t * payload, unsigned int plength, boolean retained);
       // Start to publish a message.
       // This API:
       //   beginPublish(...)
       //   one or more calls to write(...)
       //   endPublish()
       // Allows for arbitrarily large payloads to be sent without them having to be copied into
       // a new buffer and held in memory at one time
       // Returns 1 if the message was started successfully, 0 if there was an error
       boolean beginPublish(const char* topic, unsigned int plength, boolean retained);
       // Finish off this publish message (started with beginPublish)
       // Returns 1 if the packet was sent successfully, 0 if there was an error
       int endPublish();
       // Write a single byte of payload (only to be used with beginPublish/endPublish)
       virtual size_t write(uint8_t);
       // Write size bytes from buffer into the payload (only to be used with beginPublish/endPublish)
       // Returns the number of bytes written
       virtual size_t write(const uint8_t *buffer, size_t size);
       boolean subscribe(const char* topic);
       boolean subscribe(const char* topic, uint8_t qos);
       boolean unsubscribe(const char* topic);
       boolean loop();
       boolean connected();
       int state();
    };
    
    
    #endif
    