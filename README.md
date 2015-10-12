# Falak
import java.io.IOException;
import java.net.*;
 
/**
 * @author lycog
 */
public class SimpleUDPServer {
  public static void main(String[] args){
    DatagramSocket socket = null;
    DatagramPacket inPacket = null; //recieving packet
    DatagramPacket outPacket = null; //sending packet
    byte[] inBuf, outBuf;
    String msg;
    Integer vowelCount;
    final int PORT = 8888;
 
    try{
      socket = new DatagramSocket(PORT);
      while(true){
        System.out.println("Waiting for client...");
 
        //Receiving datagram from client
        inBuf = new byte[256];
        inPacket = new DatagramPacket(inBuf, inBuf.length);
        socket.receive(inPacket);
 
        //Extract data, ip and port
        int source_port = inPacket.getPort();
        InetAddress source_address = inPacket.getAddress();
        msg = new String(inPacket.getData(), 0, inPacket.getLength());
        //System.out.println("Client " + source_address + ":" + msg);
 
        //Send back to client as an echo
        vowelCount=countVowels(msg.trim());
        String actualcount=vowelCount.toString();
        outBuf = actualcount.getBytes();
     outPacket = new DatagramPacket(outBuf, 0, outBuf.length,
                        source_address, source_port);
   socket.send(outPacket);
      }
    }catch(IOException ioe){
      ioe.printStackTrace();
    }
    finally{
    	socket.close();
    }
  }
 
  private static int countVowels(String trim) {
	  int count=0;
	// TODO Auto-generated method stub
	for (int i = 0; i < trim.length(); i++) {
		if(trim.charAt(i)=='a' || trim.charAt(i)=='e' || trim.charAt(i)=='i' || trim.charAt(i)=='o' || trim.charAt(i)=='u')
		{
			
			count++;
		}
	}
	return count;
}

}




import java.io.IOException;
import java.net.*;
import java.util.Scanner;
 
/**
 * @author lycog
 */
public class SimpleUDPClient {
  public static void main(String[] args){
    DatagramSocket socket = null;
    DatagramPacket inPacket = null;
    DatagramPacket outPacket = null;
    byte[] inBuf, outBuf;
    final int PORT = 8888;
    String msg = null;
 
    try {
      InetAddress address = InetAddress.getByName("127.0.0.1");
      socket = new DatagramSocket();
 
      //Convert string to byte and send to server
      //msg = "Hello";
      Scanner in = new Scanner(System.in);
      
      System.out.println("Enter a Sentence");
      msg = in.nextLine();

      outBuf = msg.getBytes();
      outPacket = new DatagramPacket(outBuf, 0, outBuf.length,
              address, PORT);
      socket.send(outPacket);
 
      //Receive reversed message from server
      inBuf = new byte[256];
      inPacket = new DatagramPacket(inBuf, inBuf.length);
      socket.receive(inPacket);
 
      String data = new String(inPacket.getData(), 0, inPacket.getLength());
 
      System.out.println("Server : " + data);
 
    } catch (IOException ioe) {
      System.out.println(ioe);
    }
    finally{
    	socket.close();
    }
  }
}
