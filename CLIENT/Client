```
package client;

import java.awt.BorderLayout;
import java.awt.Color;
import java.io.DataOutputStream;
import java.io.IOException;
import java.net.ConnectException;
import java.net.InetSocketAddress;
import java.net.Socket;
import java.net.SocketException;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;

public class Client extends JFrame{
   private Socket socket=new Socket();;
   JPanel panel_DW = new JPanel();
   JPanel panel_UP = new JPanel();
   static JTextArea chat= new JTextArea(70,60);
   JScrollPane scrollPane = new JScrollPane(chat);
   JTextField chatbar= new JTextField(30);
   JTextField Nick= new JTextField(5);
   JButton stop_button = new JButton("접속 종료");
   JButton connection_button = new JButton("접속하기");
   JButton send_button = new JButton("SEND");
   JLabel Nick_Lb= new JLabel("ID");
   JButton Nick_button = new JButton("닉네임 설정");
   String name=null;
   String send;
   private Client(){
      chat.setBackground(Color.black);
      setSize(700,500);
      setTitle("chat");
      chat.setEnabled(false); 
      Nick_button.addActionListener(e->{
         name= Nick.getText();
      });
      connection_button.addActionListener(e->{ //접속하기
         Socket();
      });
      send_button.addActionListener(e->{ //send 버튼
            send = name+":"+chatbar.getText()+"\n";
            Send(send);
            chatbar.setText("");
      });
      chat.setFocusable(true); 
      stop_button.addActionListener(e->{ //stop 버튼
            send = name+":"+"종료했습니다\n";
            Send(send);
            System.exit(0); 
      });
      panel_UP.add(Nick_button);      
      panel_UP.add(Nick_Lb); //라벨
      panel_UP.add(Nick); // 닉네임
      panel_UP.add(connection_button);
      panel_DW.add(stop_button); //panel에 집어넣기
      panel_DW.add(chatbar);
      panel_DW.add(send_button);
      add(panel_UP,BorderLayout.NORTH); //위치 조정
      add(scrollPane,BorderLayout.CENTER);
      add(panel_DW,BorderLayout.SOUTH);
      setVisible(true);
      setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
   }
   private void Socket(){ //접속 메소드
      try {
        if(name==null && name.length()==0 ) {
           chat.setText("먼저 닉네임을 제대로 설정해주세요\n");
           System.out.print(name.length());
        }
        else {
           socket.connect(new InetSocketAddress("localhost", 5001));
           send = name+":입장하셨습니다.\n"; 
           Send(send);
           new ClientT(socket).start(); //읽기 쓰레드 시작!
        }
      }
      catch(ConnectException e) {
            chat.setText(chat.getText()+"서버가 닫혀졌습니다.\n");
      }
      catch(IOException e) {
         e.printStackTrace();
      }
   }
   private void Send(String send) { //send 메소드
       try {
          if(send.charAt(0)==':') {
             chat.setText("닉네임을 빈칸을 사용할 수 없으며 맨 앞에 ':'를 사용할 수 없습니다.\n");
          }
          else {
             DataOutputStream dos =new DataOutputStream(socket.getOutputStream());
             dos.writeUTF(send);
          }
        }
        catch(SocketException e) {
           chat.setText("먼저 접속해주세요\n");
        }
        catch(IOException e){
           e.printStackTrace();
        }
   }
   public static void main(String[] args) {
      Client chat = new Client();
   }
}



```
