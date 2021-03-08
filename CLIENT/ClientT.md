```

package client;

import java.io.DataInputStream;
import java.io.IOException;
import java.net.Socket;
import java.net.SocketException;

public class ClientT extends Thread {
	String talk;
	Socket socket = null;
	DataInputStream din;
	public ClientT(Socket socket) {
		this.socket=socket;
	}   
	public void run(){
		try {
			din =new DataInputStream(socket.getInputStream());
    	  while(true) {
            talk = din.readUTF(); //인코딩
            Client.chat.setText(Client.chat.getText()+talk);
    	  }
		} 
		catch(SocketException e) { //서버 닫혔을때
			Client.chat.setText(Client.chat.getText()+"서버가 닫혔습니다.\n클라이언트를 종료시켜주세요");
		}
		catch (IOException e) {  
			e.printStackTrace();
		}
   }
}

```
