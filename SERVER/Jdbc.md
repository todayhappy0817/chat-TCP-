```
package server;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Jdbc extends Thread {   
   //만약 만들어 놓은 메소드 중에서 오류가 발생시 gui에 출력하게 만들어준다.
   Connection con;  //데이터 베이스 드라이브를 로드
   PreparedStatement pstmt; //SQL문을 데이터베이스에 보내기 위한 객체
   ResultSet rs; //처리의 결과를 가져옴
   public void insert(String name, String talk, String port) { //삽입
      try {
    	  String Insert_SQL= "insert into chat_list values(?, ?, ?)"; 
    	  connect();
    	  pstmt = con.prepareStatement(Insert_SQL);
    	  pstmt.setString(1, name);
    	  pstmt.setString(2, talk);
    	  pstmt.setString(3, port);
    	  pstmt.executeUpdate(); 
    	  update_select(name,port);
    	  end();
      	}
      	catch(SQLException e) { 
      		e.printStackTrace();
      	}
      
   }
   public void update(String new_name, String old_name,String port) { //업데이트
      try {
    	 String Update_SQL = "update chat_list set name=? where name=?";
         connect();
         pstmt = con.prepareStatement(Update_SQL);
         pstmt.setString(1, new_name);
         pstmt.setString(2, old_name);
         pstmt.executeUpdate();
         insert(new_name,"닉네임을 "+old_name+" 에서 "+new_name+" (으)로 변경되었습니다.",port);
         end();
      }
      catch(SQLException e) {
         e.printStackTrace();
      }
   }
   
   public void update_select(String name, String port) { //먼저 업데이트할 필요가있는지 확인
      try {
    	  String SelectName_SQL="select distinct name from chat_list where port=?"; //update에서 사용함
    	  connect();
         
    	  pstmt = con.prepareStatement(SelectName_SQL);
    	  pstmt.setString(1,port);
    	  rs = pstmt.executeQuery();
    	  rs.next();
    	  String id = rs.getString("name");
    	  if(id.equals(name) != true) {
    		  update(name,id,port);
    		  end();
    	  }
      }
      catch(SQLException e) {
         e.printStackTrace();
      }
      
   }
   
   public void select(String name, String port) { //확인
      try {
    	  String Select_SQL= "select * from chat_list where name=? and port=?"; //원하는 닉네임의 포트, 이름, 대화내용을 출력
    	  String id = null;
    	  connect();
    	  pstmt = con.prepareStatement(Select_SQL);
    	  pstmt.setString(1, name);
    	  pstmt.setString(2, port);
    	  rs = pstmt.executeQuery();
    	  Server.chat.setText("이름 \t 포트\t 채팅내용\n=======================================================\n");
    	  while(rs.next()) {
    		  id = rs.getString("name");
    		  String talk = rs.getString("talk");
    		  String id_port = rs.getString("port");
    		  Server.chat.setText(Server.chat.getText()+id+"\t"+id_port+"\t"+talk+"\n");
    	  }
    	  if(id==null) {
    		  Server.chat.setText("데이터베이스안에 정보가 없습니다.\n이름과 포트를 다시 정확하게 입력해주세요");
    	  }
    	  end();
      	}
      	catch(SQLException e) {  
      		e.printStackTrace();
      	}
      
   }
   public void run() {
      while(true) { //db 정보 출력
      try{
    	  String list_SQL = "select distinct name, port from chat_list"; //중복을 없에고 닉네임과 포트를 출력하기
    	  connect();
    	  Server.connect_list.setText("이름 \t 포트\n=========================================================\n");
    	  pstmt = con.prepareStatement(list_SQL);
    	  rs = pstmt.executeQuery();
         	while(rs.next()) {
         		String id = rs.getString("name");
         		String id_port = rs.getString("port");
         		Server.connect_list.setText(Server.connect_list.getText()+id+"\t"+id_port+"\n");
         	}
         	end();
         	Thread.sleep(1000);
      	}
      	catch(SQLException e) {
         
      } catch (InterruptedException e) {
    	  Server.connect_list.setText("목록이 종료되어졌습니다.\n다시 서버를 실행해주세요");
      
   }
     }
     
   }
  
  public void delete(String  name,String port){ //삭제
     try {
    	 String delete_SQL = "delete from chat_list where name=? and port=?"; // 삭제 
         connect();
         pstmt = con.prepareStatement(delete_SQL);
         pstmt.setString(1, name);
         pstmt.setString(2, port);
         pstmt.executeUpdate();
         end();
      }
      catch(SQLException e) {
         e.printStackTrace();
      }
   }
  
   public void end() { //db종료 -> 자원누수 이유
      try {
          if(con!=null) {
             con.close();
          }
          if(pstmt!=null) {
             pstmt.close();
          }
          if(rs!=null) {
             rs.close();
          }
       } 
       catch (SQLException e) {
          e.printStackTrace();
       }
   }
   public void connect() { //연결
      try {
    	   String driver = "org.mariadb.jdbc.Driver";
    	   String url = "jdbc:mariadb://localhost:5500/chat";
    	   String UID = "jun";
    	   String uPwd ="jun"; 
    	   Class.forName(driver);
    	   con = DriverManager.getConnection(url, UID, uPwd);
      }
      catch(ClassNotFoundException e) {
            Server.chat.setText("드라이버 로드 실패");
      }
      catch(SQLException e) { 
          Server.chat.setText("접속 실패");
      }
   }

}
```
