package packDemo;
import java.sql.Connection;
import java.sql.PreparedStatement;
public class DatabaseHandlerTest {

	public static void main(String[] args) 
	{
		
		
		DatabaseHandler dH = new DatabaseHandler();
		dH.connect();
		
		dH.add_MGH();
		dH.add_Lahey();
		dH.add_Winchester();
		

		dH.removeDuplicates_MGH();
		dH.modify_MGHRecords();
		dH.modify_WinchesterRecords();
		dH.modify_LaheyRecords();
		dH.removeDuplicates_Lahey();
		dH.removeDuplicates_Winchester();
		
		
		dH.retrievePatientInformation();
		dH.viewAllRecords();
		
		

		


	}

}
