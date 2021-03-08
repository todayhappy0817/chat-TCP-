```
package server;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ServerSocket;
import java.net.Socket;


public class Server_accept extends Thread {
   ServerSocket serverSocket;
   Socket socket;
   String talk;
   public void run()  {
         try {
            serverSocket = new ServerSocket();
            serverSocket.bind(new InetSocketAddress("localhost", 5001));
            while(true) {
               socket = serverSocket.accept(); //클라이언트의 요청을 수락 후 소켓과 통신이 가능한 소켓 생성
               Server.list.add(socket); //리스트에 저장
               new Server_talk(socket).start();
            }
         } 
         catch(IOException e) {
            e.printStackTrace(); 
         }
   }

}
```
