# Middleware Engineering "*Message Oriented Middleware*"

Verfasser: **Julian Neuwirt, Felix Aust, Leonhard Stransky, 4AHIT**

Datum: **28.11.2024**

## Aufgabenstellung

Die detaillierte [Aufgabenstellung](TASK.md) beschreibt die notwendigen Schritte zur Realisierung.

## Implementierung

### MOMSender

```java
    // Parameter für die Verbindung
    private static String user = ActiveMQConnection.DEFAULT_USER;
    private static String password = ActiveMQConnection.DEFAULT_PASSWORD;
    private static String url = ActiveMQConnection.DEFAULT_BROKER_URL;
    private static String subject = "windengine_001";
    
    // Creating Session
    session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE); 
    destination = session.createTopic( subject ); 
    // Create the producer. 
    producer = session.createProducer(destination); 
    producer.setDeliveryMode( DeliveryMode.NON_PERSISTENT );     
    // Create the message 
    TextMessage m = session.createTextMessage(i); p
    roducer.send(m);
```

Dieser Java-Code stellt eine Verbindung zu einem ActiveMQ-Nachrichtenbroker her und sendet eine Textnachricht zu einem bestimmten Thema. Er verwendet Standardverbindungseinstellungen und erstellt dann eine Sitzung sowie einen Produzenten für das gewählte Thema. Die Nachricht wird im nicht persistierenden Modus gesendet, was bedeutet, dass sie nicht dauerhaft auf der Festplatte gespeichert wird. Insgesamt ermöglicht der Code das effiziente Versenden von Nachrichten in Java-Anwendungen über ActiveMQ.

### Reciever

```java
consumer = session.createConsumer( destination );     
// Start receiving 
TextMessage message = (TextMessage) consumer.receive(); 
while ( message != null ) {     
    System.out.println("Message received: " + message.getText());     
    message.acknowledge();     
    message = (TextMessage) consumer.receive(); 
} 
connection.stop();
```

Dieser Java-Code erstellt einen Consumer für ein Ziel in einer ActiveMQ-Sitzung und empfängt Textnachrichten in einer Endlosschleife. Jede empfangene Nachricht wird in der Konsole ausgegeben und anschließend bestätigt. Der Empfangsprozess läuft, bis keine weiteren Nachrichten vorhanden sind oder die Verbindung gestoppt wird.

### Conversion

```java
WarehouseData data = service.getWarehouseData( inID ); 
ObjectMapper objectMapper = new ObjectMapper(); 
String json_string = objectMapper.writeValueAsString(data); 
System.out.println( ""  + data); 
new MOMSender( json_string);
```

In folgendem Text wird das WarehouseData-Objekt mit hilfe eines ObjektMapper in einen String umgewandelt. Dazu wird die Mehtode writeValueAsString verwendet. Dann am schluss wieder der Sender aufgerufen

## Quellen

[1] www.Github.com [online] 
Available at: https://github.com/ThomasMicheler/DEZSYS_GK771_WAREHOUSE_REST.git [Accessed 28 November 2024].
