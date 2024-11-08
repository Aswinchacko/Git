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
3. Create output stream to send data to the server
4. Create input stream to read data from the server
5. Perform the read/write operation using input/output stream
6. Close the socket connection




## Example Code

### Server.java
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

### Client.java
import java.io.*;
import java.net.*;

public class Client {
    public static void main(String[] args) throws Exception {
        Socket clientSocket = new Socket("localhost", 1234);
        System.out.println("Connected to server at localhost:1234");

    }
}

