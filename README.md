# Desktop-Chatting-Application

Chat Application


What is it?
This is a basic program where two computers can talk to each other. One computer is called the Server and the other is called the Client. Think of it as a two-way conversation where both can send and receive messages.

How does it work?
Server:

Listens for messages from the client.

Can send messages back to the client.

Client:

Sends messages to the server.

Can receive messages from the server.

What do you need?
Java Development Kit (JDK) installed on your computer.

A code editor like Visual Studio Code, NetBeans, or even a simple text editor.

Basic understanding of Java.

How to run the code?
1. Start the Server
Step-by-Step:

Open your code editor.

Copy the Server code into a file named Server.java.

Save the file.

Open your terminal or command prompt.

Navigate to the directory where you saved Server.java.

Compile the server code by typing: javac Server.java

Run the server by typing: java Server

The server will now be ready to accept connections.

Server Code Explanation:

java
import java.net.*;
import java.io.*;

class Server {
    ServerSocket Server;
    Socket socket;
    BufferedReader br;
    PrintWriter out;

    // Constructor to initialize the server
    public Server() {
        try {
            Server = new ServerSocket(7777);
            System.out.println("server is ready to accept the connection");
            System.out.println("waiting for the connection...");
            socket = Server.accept();

            // Setup input and output streams
            br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            out = new PrintWriter(socket.getOutputStream());

            // Start reading and writing threads
            startReading();
            startWriting();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // Function to read messages from the client
    public void startReading() {
        Runnable r1 = () -> {
            System.out.println("reader started...");
            try {
                while (true) {
                    String msg = br.readLine();
                    if (msg.equals("exit")) {
                        System.out.println("clinent is no more available in the chat");
                        socket.close();
                        break;
                    }
                    System.out.println("client : " + msg);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        };
        new Thread(r1).start();
    }

    // Function to write messages to the client
    public void startWriting() {
        Runnable r2 = () -> {
            System.out.println("writer has started");
            try {
                while (true) {
                    BufferedReader br1 = new BufferedReader(new InputStreamReader(System.in));
                    String content = br1.readLine();
                    out.println(content);
                    out.flush();
                    
                    if(content.equals("exit")) {
                        socket.close();
                        break;
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        };
        new Thread(r2).start();
    }

    public static void main(String[] args) {
        System.out.println("this is server going to start server");
        new Server();
    }
}
2. Start the Client
Step-by-Step:

Open your code editor.

Copy the Client code into a file named Client.java.

Save the file.

Open your terminal or command prompt.

Navigate to the directory where you saved Client.java.

Compile the client code by typing: javac Client.java

Run the client by typing: java Client

The client will now connect to the server.

Client Code Explanation:

java
import java.io.*;
import java.net.*;

public class Client {
    Socket socket;
    BufferedReader br;
    PrintWriter out;

    // Constructor to initialize the client
    public Client() {
        try {
            System.out.println("sending request to server:");
            socket = new Socket("127.0.0.1", 7777);
            System.out.println("connection done.");
            
            // Setup input and output streams
            br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            out = new PrintWriter(socket.getOutputStream());

            // Start reading and writing threads
            startReading();
            startWriting();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // Function to read messages from the server
    public void startReading() {
        Runnable r1 = () -> {
            System.out.println("reader started...");
            try {
                while (true) {
                    String msg = br.readLine();
                    if (msg.equals("exit")) {
                        System.out.println("Server is no more available in the chat");
                        socket.close();
                        break;
                    }
                    System.out.println("Server : " + msg);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        };
        new Thread(r1).start();
    }

    // Function to write messages to the server
    public void startWriting() {
        Runnable r2 = () -> {
            System.out.println("writer has started");
            try {
                while (true) {
                    BufferedReader br1 = new BufferedReader(new InputStreamReader(System.in));
                    String content = br1.readLine();
                    out.println(content);
                    out.flush();

                    if(content.equals("exit")) {
                        socket.close();
                        break;
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        };
        new Thread(r2).start();
    }

    public static void main(String[] args) {
        System.out.println("this is client...");
        new Client();
    }
}
How to use the chat application?
Server Side:

Run the Server.java file first.

It will print messages indicating it is ready and waiting for the client.

Client Side:

Run the Client.java file after the server is running.

It will print messages indicating it is connected to the server.

Chat:

Type messages into the client’s console.

The server will read and print the messages, and vice versa.

Type exit to leave the chat.

Important Notes:
Ensure both server and client are running on the same network.

Make sure you use the correct IP address and port number.

Typing exit will close the connection and end the chat.
