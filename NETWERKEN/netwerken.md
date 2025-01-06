Socket = end-point van een verbinding

## Een eenvoudige Server met Stream Sockets opzetten

```java
public class Server {
private ServerSocket server;
private Socket connection;
public void start() throws IOException {
try {
// Stap 1: creëer een ServerSocket-object
server = new ServerSocket(portNumber,queueLength);
// Stap 2: de server luistert onafgebroken
// naar een poging van een client om een connectie te maken
while (true) {
    //methode accept levert een Socket af wanneer een connecti emet een client tot stand gekomen is
    connection = server.accept();
    //Door de Socket kande server interageren met de client


              // ObjectOutputStream voor het verzenden van objecten naar client
            OutputStream out = connection.getOutputStream();
    						// Flusht het ObjectOutputStream-object om alle gegevens te verzenden
            out.flush()
    						// ObjectInputStream voor ontvangen van objecten
            InputStream in = connection.getInputStream();

              // Stap 4: tijdens de verwerkingsfase communiceren
              // de server en de client via de OutputStream- en InputStream-objecten
              // ...
              //hier worden dus alle transacties gedaan: verzenden en ontvangen tussen client en server
          }
      } catch (IOException ex) {
          System.out.println("No ServerSocket : " + ex.getMessage());
      } finally {
          // Wordt hier wel nooit uitgevoerd omdat while oneindig lang loopt
          // Stap 5: wanneer de transmissie is afgehandeld,
          // sluit de server de connectie
          connection.close();
      }

}
}

```
