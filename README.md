# Java Socket Programming

Used to create a network between two computers.

## Constructors

- `Socket(String host, int port)`

## Methods

- `getInputStream()`: Returns the InputStream for reading data (used to read data from the server)
- `getOutputStream()`: Returns the OutputStream for writing data (used to send data to the server) 
- `close()`: Closes the socket (used to close the connection)
- `accept()`: Waits for an incoming connection (used by server to accept a connection)

## Steps to Develop a Client Application

1. Create a server socket object with port number
2. Create output stream to send data to the server
3. Create input stream to read data from the server
4. Perform the read/write operation using input/output stream
5. Close the socket connection

## Steps to Develop a Server Application

1. Create a server socket object with port number
2. Call accept() method to wait for client connection
3. Create output stream to send data to the client
4. Create input stream to read data from the client
5. Perform the read/write operation using input/output stream
6. Close the socket connection




## Example Code

### Server.java

```java
import java.io.*;
import java.net.*;

public class Server {
    public static void main(String[] args) throws Exception {
        ServerSocket serverSocket = new ServerSocket(1234);
        System.out.println("Server is running on port 1234");

        serverSocket.accept();
        System.out.println("Client connected");
    }
}
```

### Client.java

```java
import java.io.*;
import java.net.*;

public class Client {
    public static void main(String[] args) throws Exception {
        Socket clientSocket = new Socket("localhost", 1234);
        System.out.println("Connected to server at localhost:1234");

    }
}
```


## Client-Server Communication Example

Here's a complete example of a simple chat application using Java sockets where the client and server can exchange messages:

### Client.java

```java
import java.io.*;
import java.net.*;
import java.util.Scanner;

public class Client {
    public static void main(String[] args) throws Exception {
        Socket clientSocket = new Socket("localhost", 1234);
        System.out.println("Connected to server at localhost:1234");

        BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
        PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
        Scanner scanner = new Scanner(System.in);

        while (true) {
            
            System.out.print("You: ");
            String messageToSend = scanner.nextLine();
            if ("quit".equalsIgnoreCase(messageToSend)) {
                break;
            }
            out.println(messageToSend);

            
            String receivedMessage = in.readLine();
            System.out.println("Server: " + receivedMessage);
        }

        scanner.close();
        in.close();
        clientSocket.close();
    }
}
```

### Explanation of Client.java

Let's break down the Client.java code:

1. **Imports**
   ```java
   import java.io.*;    // For input/output streams
   import java.net.*;   // For Socket class
   import java.util.Scanner;  // For reading user input
   ```

2. **Socket Creation**
   ```java
   Socket clientSocket = new Socket("localhost", 1234);
   ```
   - Creates a socket connection to the server running on localhost at port 1234
   - The constructor takes two parameters:
     - host: Server address ("localhost" means the server is running on same machine)
     - port: Port number where server is listening (1234 in this case)

3. **Stream Setup**
   ```java
   BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
   PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
   Scanner scanner = new Scanner(System.in);
   ```
   - `BufferedReader`: For reading messages from server
   - `PrintWriter`: For sending messages to server
   - `Scanner`: For reading user input from console
   - The `true` parameter in PrintWriter enables auto-flushing (automatically sends data)

4. **Message Exchange Loop**
   ```java
   while (true) {
       System.out.print("You: ");
       String messageToSend = scanner.nextLine();
       if ("quit".equalsIgnoreCase(messageToSend)) {
           break;
       }
       out.println(messageToSend);
       
       String receivedMessage = in.readLine();
       System.out.println("Server: " + receivedMessage);
   }
   ```
   - Continuously loops until user types "quit"
   - First gets user input and sends it to server
   - Then waits for and displays server's response
   - The `println()` method adds a newline character after the message

5. **Cleanup**
   ```java
   scanner.close();
   in.close();
   clientSocket.close();
   ```
   - Properly closes all resources to prevent memory leaks
   - Closes the scanner, input stream, and socket connection

### Error Handling Note
The code uses `throws Exception` for simplicity, but in production code, you should:
- Handle specific exceptions like `IOException`, `UnknownHostException`
- Implement proper error handling with try-catch blocks
- Add connection timeout handling
- Implement reconnection logic if connection is lost

### Usage
1. Make sure the server is running first
2. Run the client program
3. Type messages and press Enter to send
4. Type "quit" to exit the program





### Server.java

```java
import java.io.*;
import java.net.*;
import java.util.Scanner;
public class Server {
    public static void main(String[] args) throws Exception {
        ServerSocket serverSocket = new ServerSocket(6000);
        System.out.println("Server is running on port 6000");

        Socket clientSocket = serverSocket.accept();
        System.out.println("Client connected");

        BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
        PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
        Scanner scanner = new Scanner(System.in);

        while (true) {
           
            String clientMessage = in.readLine();
            System.out.println("Client: " + clientMessage);

            
            System.out.print("You: ");
            String messageToSend = scanner.nextLine();
            if ("quit".equalsIgnoreCase(messageToSend)) {
                break;
            }
            out.println(messageToSend);
        }

        scanner.close();
        in.close();
        clientSocket.close();
        serverSocket.close();
    }
}
```
### Server.java Explanation

The Server.java file implements a simple chat server using Java Socket programming. Let's break down the code:

1. **Imports**
   ```java
   import java.io.*;
   import java.net.*;
   import java.util.Scanner;
   ```
   These imports provide classes for input/output operations, networking, and user input handling.

2. **Server Setup**
   ```java
   ServerSocket serverSocket = new ServerSocket(6000);
   System.out.println("Server is running on port 6000");
   ```
   - Creates a ServerSocket that listens on port 6000
   - Prints a message confirming the server is running

3. **Client Connection**
   ```java
   Socket clientSocket = serverSocket.accept();
   System.out.println("Client connected");
   ```
   - `accept()` waits for a client to connect
   - Once connected, creates a Socket for communication with that client

4. **Stream Setup**
   ```java
   BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
   PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
   Scanner scanner = new Scanner(System.in);
   ```
   - `BufferedReader`: Reads messages from the client
   - `PrintWriter`: Sends messages to the client
   - `Scanner`: Reads server user input from console

5. **Message Loop**
   ```java
   while (true) {
       String clientMessage = in.readLine();
       System.out.println("Client: " + clientMessage);
       
       System.out.print("You: ");
       String messageToSend = scanner.nextLine();
       if ("quit".equalsIgnoreCase(messageToSend)) {
           break;
       }
       out.println(messageToSend);
   }
   ```
   - Continuously reads messages from client
   - Displays client messages on server console
   - Allows server to type and send responses
   - Exits loop if server types "quit"

6. **Cleanup**
   ```java
   scanner.close();
   in.close();
   clientSocket.close();
   serverSocket.close();
   ```
   - Properly closes all resources when done
   - Prevents resource leaks

### Key Features:
- Two-way communication between server and client
- Simple console-based interface
- Graceful shutdown with "quit" command
- Basic error handling with throws Exception

### Limitations:
- Handles only one client at a time
- No encryption or security measures
- Basic text-based communication only
- No reconnection mechanism if connection is lost


