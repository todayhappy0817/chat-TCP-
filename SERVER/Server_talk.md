```
package server;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.Socket;
import java.net.SocketException;

public class Server_talk extends Thread {
   Socket socket=null;
   String talk;
   String port;
   String mail1,mail2;
   
   public Server_talk(Socket socket) {
      this.socket = socket;
   }
   public void run(){
      Jdbc DB  = new Jdbc();
      try { 
         DataInputStream din =new DataInputStream(socket.getInputStream()); 
         while(true){
           talk=din.readUTF(); //인코딩
           port= Integer.toString(socket.getPort()); //port-> 랩퍼클래스를 이용해 변환
           int idx = talk.indexOf(":");
           mail1=talk.substring(0,idx); //닉네임
           mail2=talk.substring(idx+1); //토크
           DB.insert(mail1,mail2,port);
           for(int i=0; i<Server.list.size();i++) {
              DataOutputStream dos = new DataOutputStream( Server.list.get(i).getOutputStream());   
              dos.writeUTF(talk);   //디코딩
           }
         }
      } 
      catch(SocketException e) {
         try { //클라이언트가 꺼질경우 소켓을 꺼버리기
        	 DB.insert(mail1,"종료되었습니다.",port);
        	 socket.close(); 
             Server.list.remove(socket); //리스트에 없에기
            
         } catch (IOException e1) {
            e1.printStackTrace();
         }
      }
      catch (IOException e) {
         e.printStackTrace();
      }

   }
}

```
