# Mock-Hospital-System
Implementation of Java methods that allow the insertion of records into a database, removal of duplicates from tables in a database using a user friendly GUI. Also user is able to retrieve and display a particular patient's information from various hospital systems. Created several graphical user interfaces that allow application user to modify the records contained in the database. Another useful functionality of this application is that user is able to view all the records within all 3 hospital systems all at once.    

package packDemo;

import java.awt.Dimension;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.Scanner;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JList;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextField;
import javax.swing.JTextPane;







public class DatabaseHandler implements DatabaseManager {
// The variable of type Connection is declared as a global variable because other operations in this program need to reference this variable. 
 private Connection connect = null; 
 private PreparedStatement preparedStmt = null;

// The following method allows access to the database. To perform any modifications to the database
// or to retrieve the information in the database, the code in method connect is executed. 
// The following method is void so it does not return any data, but executes the code within the method.
public void connect()
{
	try 
	{
		Class.forName("com.mysql.jdbc.Driver");
	} 
// The line of code in the try block may throw an exception called ClassNotFoundException.
// The catch block allows the thrown exception to be handled.
// The catch block specifies the code to be executed if an exception occurs. 
// Inclusion of the catch block prevents the program from crashing. 
	catch (ClassNotFoundException e) {
		System.out.println("Was not able to intialize Java class Driver. Initialization of this class is necessary to connect to database");
	// Statement below invokes a method of Java class ClassNotFoundException. 	
		e.printStackTrace();
	}
	
	try {
		// Statement below returns an object of type Connection. 
		connect = DriverManager.getConnection("jdbc:mysql://localhost:3306/database1", "User1", "User1Access");
	} 
	catch (SQLException e) 
	{
		System.out.println("Method getConnection of Java class DriverManager was not able to complete execution");
		e.printStackTrace();
	}
}
	

// A hospital official can insert records into the database by providing patient information in the console.
// Method below allows user to add records into the Mass General Hospital system. 
public void add_MGH()
	{
// Array list aL1 is used to store all the social security numbers currently in the hospital system. 	
	ArrayList<String> aL1 = new ArrayList<>(); 
	ArrayList<String> aL2 = new ArrayList<>();
	PreparedStatement preparedStmt2 = null;
	PreparedStatement preparedStmt3 = null;
	
	Scanner scanner1 = new Scanner(System.in);
	Scanner scanner2 = new Scanner(System.in);
	Scanner scanner3 = new Scanner(System.in);
	Scanner scanner4 = new Scanner(System.in);

	
	int userSelection = 0;
	int patientID = 0;
	int visitNumber = 1;
	double coPayment = 0;
	
	String lastName = "";
	String firstName = "";
	String gender = "";
	String ssn = "";
	String rs2_ssNumber;
	String dob = "";
	String visitDate = "";
	String reason = "";
	String insCo = "";
	
	
	
try{
		

				System.out.println("Would you like to add records into MGH system? Press 1 for Yes and 2 for No");	
				userSelection = scanner4.nextInt();
// The 2 statements above allow for user input. User input determines if code below will get executed. 				
				while(userSelection == 1)
				{
					
					System.out.println("Please enter patient last name: ");
					lastName = scanner2.nextLine();
			
					scanner2.nextLine();
					
					System.out.println("Please enter patient first name: ");
					firstName = scanner2.nextLine();
					
					scanner2.nextLine();
			
					System.out.println("Please enter patient gender: ");
					gender = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter the patient SSN");
					ssn = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient DOB in the format YEAR-MM-DD");
					dob = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's insurance company");
					insCo = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's visit date in the format YEAR-MM-DD");
					visitDate = scanner2.nextLine();
					
					scanner2.nextLine();
					
// Statements below collect information to insert into another table. 	
					System.out.println("Please enter reason for patient visit.");
					reason = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's copayment amount.");
					coPayment = scanner2.nextDouble();
					
					scanner2.nextLine();
					
		
			
// Query below accesses all the social security numbers from the table patients_mgh. 
// There are 2 tables associated with each hospital. One of the tables only accepts unique records.
// The social security number is used as the patient identifier. There is one record per patient in patients_mgh table.
// Each patient in this table gets assigned a unique patientID. 
String sql4 = "SELECT SSN FROM patients_mgh";
PreparedStatement preparedStmt4 = connect.prepareStatement(sql4);
ResultSet rs2 = preparedStmt4.executeQuery();
// Statement above executes the SELECT statement. 
// Statements below access the info in the ResultSet object. 
	while(rs2.next())
		{
			rs2_ssNumber = rs2.getString("SSN");
			aL1.add(rs2_ssNumber);
		}

// New record is created only if user inputted social security number does not already exist in hospital system. 
if(!aL1.contains(ssn))
{

String sql = "INSERT INTO patients_mgh (last_Name, first_Name, gender, SSN, dob, insurance_co) VALUES (?,?,?,?,?,?)";
preparedStmt = connect.prepareStatement(sql);
preparedStmt.setString(1, lastName);
preparedStmt.setString(2, firstName);
preparedStmt.setString(3, gender);
preparedStmt.setString(4, ssn);
preparedStmt.setString(5, dob);
preparedStmt.setString(6, insCo);
preparedStmt.execute();


}
//int rowsInserted = preparedStmt.executeUpdate();
//System.out.println(rowsInserted + " row(s) were inserted in the MySQL database");

String sql2 = "SELECT patient_id FROM patients_mgh WHERE SSN = ?";
preparedStmt2 = connect.prepareStatement(sql2);
preparedStmt2.setString(1, ssn);
//System.out.println("Social Security number is: " + ssn);
ResultSet rs1 = preparedStmt2.executeQuery();
if(rs1.next())
{
	patientID = rs1.getInt("patient_id");
}
//System.out.println("Value of variable patientID: " + patientID);


 
// Statements below get executed only if user inputted social security number already exists in the system.  
 
if(aL1.contains(ssn))
	{
		String sql5 = "SELECT visit_number FROM patient_history_mgh WHERE patient_id = ?";
		PreparedStatement preparedStmt5 = connect.prepareStatement(sql5);
		preparedStmt5.setInt(1, patientID);
		ResultSet rs3 = preparedStmt5.executeQuery();
// statement below retrieves the latest visit number in the system of a particular patient. 		
		while(rs3.next())
		{
			if(rs3.last())
			{
				visitNumber = rs3.getInt("visit_number");
		
			
			}
		}
	
		visitNumber = visitNumber + 1;
		

			
	}
		
	


 
 
// Code below creates a new record in the second table associated with this hospital. 
// The new record displays the newly calculated visit number. 
String sql3 = "INSERT INTO patient_history_mgh(visit_number, patient_id, visit_date, reason, copay_amount_paid) VALUES(?,?,?,?,?)";
preparedStmt3 = connect.prepareStatement(sql3);
preparedStmt3.setInt(1, visitNumber);
preparedStmt3.setInt(2, patientID);
preparedStmt3.setString(3, visitDate);
preparedStmt3.setString(4, reason);
preparedStmt3.setDouble(5, coPayment);
preparedStmt3.execute();




System.out.println("Would like to enter another patient? Type 1 for 'Yes' or 2 for 'No': ");
userSelection = scanner3.nextInt();


				}				
		

	 // end of if statement
}
catch(SQLException e) 
{
	System.out.println("Could not execute method add_MGH to completion");
	e.printStackTrace();
}
	
}	

public void add_Lahey()
{
	
	ArrayList<String> aL1 = new ArrayList<>(); 
	ArrayList<String> aL2 = new ArrayList<>();
	PreparedStatement preparedStmt2 = null;
	PreparedStatement preparedStmt3 = null;
	
	Scanner scanner1 = new Scanner(System.in);
	Scanner scanner2 = new Scanner(System.in);
	Scanner scanner3 = new Scanner(System.in);

	
	int selection = 0;
	String lastName = "";
	String firstName = "";
	String gender = "";
	String ssn = "";
	String rs2_ssNumber;
	String dob = "";
	int patientID = 0;
	String visitDate = "";
	String reason = "";
	String insCo = "";
	double coPayment = 0;
	int latest_visitNumber = 1;

	
	
try{
		
			System.out.println("Would you like to add records into Lahey system? Type 1 for Yes and 2 for No and press enter: ");
			selection = scanner1.nextInt();
			while(selection == 1)
			{
					
				
				

					
					System.out.println("Please enter patient last name: ");
					lastName = scanner2.nextLine();
			
					scanner2.nextLine();
					
					System.out.println("Please enter patient first name: ");
					firstName = scanner2.nextLine();
					
					scanner2.nextLine();
			
					System.out.println("Please enter patient gender: ");
					gender = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter the patient SSN");
					ssn = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient DOB in the format YEAR-MM-DD");
					dob = scanner2.nextLine();
					scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's insurance company");
					insCo = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's visit date in the format YEAR-MM-DD");
					visitDate = scanner2.nextLine();
					
					scanner2.nextLine();
					
// data for the other table. 	
					System.out.println("Please enter reason for patient visit.");
					reason = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's copayment amount.");
					coPayment = scanner2.nextDouble();
					
					scanner2.nextLine();
					
		
			

String sql4 = "SELECT SSN FROM patients_lahey";
PreparedStatement preparedStmt4 = connect.prepareStatement(sql4);
ResultSet rs2 = preparedStmt4.executeQuery();
					 
	while(rs2.next())
		{
			rs2_ssNumber = rs2.getString("SSN");
			aL1.add(rs2_ssNumber);
		}
if(!aL1.contains(ssn))
{

String sql = "INSERT INTO patients_lahey (last_Name, first_Name, gender, SSN, dob, insurance_co) VALUES (?,?,?,?,?,?)";
preparedStmt = connect.prepareStatement(sql);
preparedStmt.setString(1, lastName);
preparedStmt.setString(2, firstName);
preparedStmt.setString(3, gender);
preparedStmt.setString(4, ssn);
preparedStmt.setString(5, dob);
preparedStmt.setString(6, insCo);
preparedStmt.execute();


}
//int rowsInserted = preparedStmt.executeUpdate();
//System.out.println(rowsInserted + " row(s) were inserted in the MySQL database");

String sql2 = "SELECT patient_id FROM patients_lahey WHERE SSN = ?";
preparedStmt2 = connect.prepareStatement(sql2);
preparedStmt2.setString(1, ssn);
ResultSet rs1 = preparedStmt2.executeQuery();
if(rs1.next())
{
	patientID = rs1.getInt("patient_id");
}
//System.out.println("Value of variable patientID: " + patientID);


 
 
 if(aL1.contains(ssn))
	{
		String sql5 = "SELECT visit_number FROM patient_history_lahey WHERE patient_id = ?";
		PreparedStatement preparedStmt5 = connect.prepareStatement(sql5);
		preparedStmt5.setInt(1, patientID);
		ResultSet rs3 = preparedStmt5.executeQuery();
		while(rs3.next())
		{
			if(rs3.last())
			{
				latest_visitNumber = rs3.getInt("visit_number");
		
			
			}
		}
	
		latest_visitNumber = latest_visitNumber + 1;
		

			
	}
		
	

String sql3 = "INSERT INTO patient_history_lahey(visit_number, patient_id, visit_date, reason, copay_amount_paid) VALUES(?,?,?,?,?)";
preparedStmt3 = connect.prepareStatement(sql3);
preparedStmt3.setInt(1, latest_visitNumber);
preparedStmt3.setInt(2, patientID);
preparedStmt3.setString(3, visitDate);
preparedStmt3.setString(4, reason);
preparedStmt3.setDouble(5, coPayment);
preparedStmt3.execute();


System.out.println("Would like to enter another patient? Type 1 for 'Yes' or 2 for 'No': ");
selection = scanner3.nextInt();


				
	
	}  
				 	

	 // end of if statement
}
catch(SQLException e) 
{
	System.out.println("Could not execute method add_Lahey to completion");
	e.printStackTrace();
}




}

public void add_Winchester()
{
	
	ArrayList<String> aL1 = new ArrayList<>(); 
	ArrayList<String> aL2 = new ArrayList<>();
	PreparedStatement preparedStmt2 = null;
	PreparedStatement preparedStmt3 = null;
	
	Scanner scanner1 = new Scanner(System.in);
	Scanner scanner2 = new Scanner(System.in);
	Scanner scanner3 = new Scanner(System.in);

	
	int selection = 0;
	String lastName = "";
	String firstName = "";
	String gender = "";
	String ssn = "";
	String rs2_ssNumber;
	String dob = "";
	int patientID = 0;
	String visitDate = "";
	String reason = "";
	String insCo = "";
	double coPayment = 0;
	int latest_visitNumber = 1;

	
	
try{
		
			System.out.println("Would you like to add records into the Winchester system? Type 1 for Yes and 2 for No and press enter: ");
			selection = scanner1.nextInt();
			while(selection == 1)
			{
					
				
				

					
					System.out.println("Please enter patient last name: ");
					lastName = scanner2.nextLine();
			
					scanner2.nextLine();
					
					System.out.println("Please enter patient first name: ");
					firstName = scanner2.nextLine();
					
					scanner2.nextLine();
			
					System.out.println("Please enter patient gender: ");
					gender = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter the patient SSN");
					ssn = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient DOB in the format YEAR-MM-DD");
					dob = scanner2.nextLine();
					scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's insurance company");
					insCo = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's visit date in the format YEAR-MM-DD");
					visitDate = scanner2.nextLine();
					
					scanner2.nextLine();
					
// data for the other table. 	
					System.out.println("Please enter reason for patient visit.");
					reason = scanner2.nextLine();
					
					scanner2.nextLine();
					
					System.out.println("Please enter patient's copayment amount.");
					coPayment = scanner2.nextDouble();
					
					scanner2.nextLine();
					
		
			

String sql4 = "SELECT SSN FROM patients_winchester";
PreparedStatement preparedStmt4 = connect.prepareStatement(sql4);
ResultSet rs2 = preparedStmt4.executeQuery();
					 
	while(rs2.next())
		{
			rs2_ssNumber = rs2.getString("SSN");
			aL1.add(rs2_ssNumber);
		}

//Situation 1
if(!aL1.contains(ssn))
{

String sql = "INSERT INTO patients_winchester (last_Name, first_Name, gender, SSN, dob, insurance_co) VALUES (?,?,?,?,?,?)";
preparedStmt = connect.prepareStatement(sql);
preparedStmt.setString(1, lastName);
preparedStmt.setString(2, firstName);
preparedStmt.setString(3, gender);
preparedStmt.setString(4, ssn);
preparedStmt.setString(5, dob);
preparedStmt.setString(6, insCo);
preparedStmt.execute();

}
//int rowsInserted = preparedStmt.executeUpdate();
//System.out.println(rowsInserted + " row(s) were inserted in the MySQL database");


//System.out.println("Value of variable patientID: " + patientID);

String sql2 = "SELECT patient_id FROM patients_winchester WHERE SSN = ?";
preparedStmt2 = connect.prepareStatement(sql2);
preparedStmt2.setString(1, ssn);
ResultSet rs1 = preparedStmt2.executeQuery();
	if(rs1.next())
	{
		patientID = rs1.getInt("patient_id");
	}
// Situation 2 
 if(aL1.contains(ssn))
	{
		
	 	
	 
	 
	 	String sql5 = "SELECT visit_number FROM patient_history_winchester WHERE patient_id = ?";
		PreparedStatement preparedStmt5 = connect.prepareStatement(sql5);
		preparedStmt5.setInt(1, patientID);
		ResultSet rs3 = preparedStmt5.executeQuery();
		while(rs3.next())
		{
			if(rs3.last())
			{
				latest_visitNumber = rs3.getInt("visit_number");
		
			
			}
		}
	
		latest_visitNumber = latest_visitNumber + 1;
		

			
	}
		
	


 
 
//System.out.println("Value of variable converted_patientID: " + converted_patientID);
String sql3 = "INSERT INTO patient_history_winchester(visit_number, patient_id, visit_date, reason, copay_amount_paid) VALUES(?,?,?,?,?)";
preparedStmt3 = connect.prepareStatement(sql3);
preparedStmt3.setInt(1, latest_visitNumber);
preparedStmt3.setInt(2, patientID);
preparedStmt3.setString(3, visitDate);
preparedStmt3.setString(4, reason);
preparedStmt3.setDouble(5, coPayment);

preparedStmt3.execute();

System.out.println("Would like to enter another patient? Type 1 for 'Yes' or 2 for 'No': ");
selection = scanner3.nextInt();


				
	
	}   	

	 // end of if statement
}
catch(SQLException e) 
	{
		System.out.println("Could not execute method add_winchester to completion");
		e.printStackTrace();
	}

}



// Method below will allow hospital official to view all of the records in all 3 hospital systems. 
// Method below joins the 2 tables within each hospital system. 
// Results of the query indicate from which hospital a particular record in from.
// If a particular patient has visited more than one hospital, all of the records of this patient are grouped together in the query result. 
public void viewAllRecords()
{

int rowNumber = 0;
String col0 = "";
String col1 = "";
String col2 = "";
String col3 = "";
String col4 = "";
String col5 = "";
String col6 = "";
String col7 = "";
String col8 = "";
String col9 = "";
String col10 = "";
String col11 = "";
boolean lastRow = false;
int num_lastRow = 0;


System.out.println("Would you like to view all of the records in the system now? Type 1 for Yes and 2 for No");
Scanner scanner4 = new Scanner(System.in);
int preference = scanner4.nextInt();
if(preference == 1)
{
	
	try 
	{
		String sql1 = "SELECT 'MGH', mgh1.last_Name, mgh1.first_name, mgh1.gender, mgh1.SSN, mgh1.dob, mgh1.insurance_co, mgh2.reason, mgh2.copay_amount_paid, mgh2.visit_date, mgh2.visit_number, mgh2.time_stamp FROM patients_mgh mgh1, patient_history_mgh mgh2 WHERE mgh2.patient_id = mgh1.patient_id UNION SELECT 'Lahey', L1.last_Name, L1.first_name, L1.gender, L1.SSN, L1.dob, L1.insurance_co, L2.reason, L2.copay_amount_paid, L2.visit_date, L2.visit_number, L2.time_stamp FROM patients_lahey L1, patient_history_lahey L2 WHERE L1.patient_id = L2.patient_id UNION SELECT 'Winchester', W1.last_Name, W1.first_name, W1.gender, W1.SSN, W1.dob, W1.insurance_co, W2.reason, W2.copay_amount_paid, W2.visit_date, W2.visit_number, W2.time_stamp FROM patients_winchester W1, patient_history_winchester W2 WHERE W1.patient_id = W2.patient_id ORDER BY last_Name";
		PreparedStatement prepareStmt1 = connect.prepareStatement(sql1);
		ResultSet rs1 = prepareStmt1.executeQuery();
		lastRow = rs1.last();
		num_lastRow = rs1.getRow();
		String[][] tableValues = new String[1][12];
		int counter = 0;
		
		rs1.beforeFirst();
		
		while(rs1.next())
		{
			counter++;
			for(int i = 0; i < 12; i++ )
			{
				String value = rs1.getString(i + 1); // here, argument represents the column index. 
				tableValues[0][i] = value;
					

			}
			
			System.out.println("Record " + counter + Arrays.deepToString(tableValues));
		}
			
		
// Used multidimensional arrays to display the results of the query to the application user.   				

		
		 	
	
			
	} 
	catch (SQLException e) 
	{
		
		e.printStackTrace();
	} 


}
}






// The table that contains patient SSN cannot accept duplicate records because of unique key.
// However, any duplicate records can be removed from the other table within each hospital system. 
public void removeDuplicates_MGH()
{
	int count = 0;
	int visitNumber = 0;
	int patientID = 0;
	int counter = 0;
	ArrayList<Date> aL1 = new ArrayList<>();
	Date maxDate = null;
	Timestamp timeStamp = null;
	try
	{
// If there is more than one record with the same patient ID and visit number, it is determined that the table contains duplicate records. 
// Sql query below determines how many records have the same patient id and same visit number?
		String sql1 = "SELECT *, COUNT(*) FROM patient_history_mgh GROUP BY patient_id, visit_number;";
		PreparedStatement preparedStmt1 = connect.prepareStatement(sql1);	 
		ResultSet rs1 = preparedStmt1.executeQuery();
		while(rs1.next())
		{
			count = rs1.getInt("count(*)");
			
			
			if(count > 1)
			{
				visitNumber = rs1.getInt("visit_number");
				patientID = rs1.getInt("patient_id");
				counter++; 
				String sql2 = "SELECT visit_date FROM patient_history_mgh WHERE visit_Number = ? AND patient_ID = ?";
				PreparedStatement preparedStmt2 = connect.prepareStatement(sql2);
				preparedStmt2.setInt(1, visitNumber);
				preparedStmt2.setInt(2, patientID);
				ResultSet rs2 = preparedStmt2.executeQuery();
				while(rs2.next())
				{
					// check the data type of the below variable. 
					Date date = rs2.getDate("visit_date");		// check which Date object to import. 
			// adding all the visit dates to the arrayList. 
					aL1.add(date);
					
				}

	// information stored in the arrayList is of the same data type as what's retrieved from the ResultSet.  	
				maxDate = Collections.max(aL1, null);
			
				String sql3 = "DELETE FROM patient_history_mgh WHERE visit_date != ? AND visit_number = ? AND patient_id = ?";
				PreparedStatement preparedStmt3 = connect.prepareStatement(sql3);
				preparedStmt3.setDate(1, maxDate);
				preparedStmt3.setInt(2, visitNumber);
				preparedStmt3.setInt(3, patientID);
				int rowsUpdated = preparedStmt3.executeUpdate();
// In this case, the record associated with the older date is deleted from the table.  			
			}
		
			
		
		}
		
		
		
// end of situation 1.  
// Code below handles a situation where patient ID, visit number and visit date are the same. 
// In this case, the record with the older time stamp is deleted from the table. 

String sql5 = "SELECT COUNT(*), patient_id, visit_number, visit_date, MAX(time_stamp) FROM patient_history_mgh GROUP BY patient_id, visit_number, visit_date";
PreparedStatement preparedStmt4 = connect.prepareStatement(sql5);
ResultSet rs3 = preparedStmt4.executeQuery();
while(rs3.next())
	{
		
	int totalRecords = rs3.getInt("COUNT(*)");
	if(totalRecords > 1)
	{
		patientID = rs3.getInt("patient_ID");
		visitNumber = rs3.getInt("visit_number");
		Date date = rs3.getDate("visit_date");
		timeStamp = rs3.getTimestamp("MAX(time_stamp)");
		System.out.println("Maximum value of time stamp is: " + timeStamp);
		System.out.println("The visit date is: " + date);
		
				
			String sql6 = "DELETE FROM patient_history_mgh WHERE patient_id = ? AND visit_number = ? AND visit_date = ? AND time_stamp != ?";
			PreparedStatement preparedStmt5 = connect.prepareStatement(sql6);
			preparedStmt5.setInt(1, patientID);
			preparedStmt5.setInt(2, visitNumber);
			preparedStmt5.setDate(3, date);
			preparedStmt5.setTimestamp(4, timeStamp);
			preparedStmt5.execute();
			
		}
	
	
	
	}
	
		
		
	}// end of try block

	catch(SQLException e)
	{
		e.printStackTrace();
		
	}
}


public void removeDuplicates_Lahey()
{
	
	ArrayList<Date> aL1 = new ArrayList<>();
	Timestamp timeStamp = null;
	Date maxDate = null;
	
	int count = 0;
	int visitNumber = 0;
	int patientID = 0;
	
	
	
	
	try
	{
		String sql1 = "SELECT *, COUNT(*) FROM patient_history_lahey GROUP BY patient_id, visit_number";
		PreparedStatement preparedStmt1 = connect.prepareStatement(sql1);	// how many records have the same patientID and same visit number?
		ResultSet rs1 = preparedStmt1.executeQuery();
		while(rs1.next())
		{
			count = rs1.getInt("count(*)");
			
			
			if(count > 1)
			{
				visitNumber = rs1.getInt("visit_number");
				patientID = rs1.getInt("patient_id");
				 
				
				String sql2 = "SELECT visit_date FROM patient_history_lahey WHERE visit_Number = ? AND patient_ID = ?";
				PreparedStatement preparedStmt2 = connect.prepareStatement(sql2);
				preparedStmt2.setInt(1, visitNumber);
				preparedStmt2.setInt(2, patientID);
				ResultSet rs2 = preparedStmt2.executeQuery();
				while(rs2.next())
				{
					 
					Date date = rs2.getDate("visit_date");		
			// adding all the visit dates to the arrayList. 
					aL1.add(date);
					
				}

	
				maxDate = Collections.max(aL1, null);
			
				String sql3 = "DELETE FROM patient_history_lahey WHERE visit_date != ? AND visit_number = ? AND patient_id = ?";
				PreparedStatement preparedStmt3 = connect.prepareStatement(sql3);
				preparedStmt3.setDate(1, maxDate);
				preparedStmt3.setInt(2, visitNumber);
				preparedStmt3.setInt(3, patientID);
				int rowsUpdated = preparedStmt3.executeUpdate();
			
			}
		
			
		
		}
		
		
		
// end of situation 1.  
String sql5 = "SELECT COUNT(*), patient_id, visit_number, visit_date, MAX(time_stamp) FROM patient_history_lahey GROUP BY patient_id, visit_number, visit_date";
PreparedStatement preparedStmt4 = connect.prepareStatement(sql5);
ResultSet rs3 = preparedStmt4.executeQuery();
while(rs3.next())
	{
		
	int totalRecords = rs3.getInt("COUNT(*)");
	if(totalRecords > 1)
	{
		patientID = rs3.getInt("patient_ID");
		visitNumber = rs3.getInt("visit_number");
		Date date = rs3.getDate("visit_date");
		timeStamp = rs3.getTimestamp("MAX(time_stamp)");
		System.out.println("Maximum value of time stamp is: " + timeStamp);
		System.out.println("The visit date is: " + date);
		
		
		
			String sql6 = "DELETE FROM patient_history_lahey WHERE patient_id = ? AND visit_number = ? AND visit_date = ? AND time_stamp != ?";
			PreparedStatement preparedStmt5 = connect.prepareStatement(sql6);
			preparedStmt5.setInt(1, patientID);
			preparedStmt5.setInt(2, visitNumber);
			preparedStmt5.setDate(3, date);
			preparedStmt5.setTimestamp(4, timeStamp);
			preparedStmt5.execute();
			
		}
	
	
	
	}
	
	
		
	}// end of try block

	catch(SQLException e)
	{
		e.printStackTrace();
		
	}
}

public void removeDuplicates_Winchester()
{
	ArrayList<Date> aL1 = new ArrayList<>();
	Timestamp timeStamp = null;
	Date maxDate = null;
	
	int count = 0;
	int visitNumber = 0;
	int patientID = 0;
	
	
	
	
	try
	{
		String sql1 = "SELECT *, COUNT(*) FROM patient_history_winchester GROUP BY patient_id, visit_number";
		PreparedStatement preparedStmt1 = connect.prepareStatement(sql1);	// how many records have the same patientID and same visit number?
		ResultSet rs1 = preparedStmt1.executeQuery();
		while(rs1.next())
		{
			count = rs1.getInt("count(*)");
			
			
			if(count > 1)
			{
				visitNumber = rs1.getInt("visit_number");
				patientID = rs1.getInt("patient_id");
			 

				String sql2 = "SELECT visit_date FROM patient_history_winchester WHERE visit_number = ? AND patient_id = ?";
				PreparedStatement preparedStmt2 = connect.prepareStatement(sql2);
				preparedStmt2.setInt(1, visitNumber);
				preparedStmt2.setInt(2, patientID);
				ResultSet rs2 = preparedStmt2.executeQuery();
				while(rs2.next())
				{
					 
					Date date = rs2.getDate("visit_date");		
			// adding all the visit dates to the arrayList. 
					aL1.add(date);
					
				}
  	
				maxDate = Collections.max(aL1, null);
			
				String sql3 = "DELETE FROM patient_history_winchester WHERE visit_date != ? AND visit_number = ? AND patient_id = ?";
				PreparedStatement preparedStmt3 = connect.prepareStatement(sql3);
				preparedStmt3.setDate(1, maxDate);
				preparedStmt3.setInt(2, visitNumber);
				preparedStmt3.setInt(3, patientID);
				int rowsUpdated = preparedStmt3.executeUpdate();
			
			}   
		
			
		
		}
		
		
		
// end of situation 1.  
String sql5 = "SELECT COUNT(*), patient_id, visit_number, visit_date, MAX(time_stamp) FROM patient_history_winchester GROUP BY patient_id, visit_number, visit_date";

PreparedStatement preparedStmt4 = connect.prepareStatement(sql5);
ResultSet rs3 = preparedStmt4.executeQuery();
while(rs3.next())
	{
		
	int totalRecords = rs3.getInt("COUNT(*)");
	if(totalRecords > 1)
	{
		patientID = rs3.getInt("patient_ID");
		visitNumber = rs3.getInt("visit_number");
		Date date = rs3.getDate("visit_date");
		timeStamp = rs3.getTimestamp("MAX(time_stamp)");
		System.out.println("Maximum value of time stamp is: " + timeStamp);
		System.out.println("The visit date is: " + date);
		
		
		
			String sql6 = "DELETE FROM patient_history_winchester WHERE patient_id = ? AND visit_number = ? AND visit_date = ? AND time_stamp != ?";
			PreparedStatement preparedStmt5 = connect.prepareStatement(sql6);
			preparedStmt5.setInt(1, patientID);
			preparedStmt5.setInt(2, visitNumber);
			preparedStmt5.setDate(3, date);
			preparedStmt5.setTimestamp(4, timeStamp);
			preparedStmt5.execute();
			
		}
	
	
	
	}
	
		
		
	}// end of try block

	catch(SQLException e)
	{
		e.printStackTrace();
		
	}
}

// The method below retrieves a particular patient's information from all three hospitals.
// Application user is asked to input the last name of the patient whose information needs to be retrieved.
// If 2 patients have the same last name, then a pop up window is displayed asking user to specify the first name from a drop down list. 
public void retrievePatientInformation()
{
	System.out.println("Would you like to view a patient's information from all 3 hospitals?");
	System.out.println("Enter 1 for Yes and 2 for No");
	Scanner scanner = new Scanner(System.in);
	int selection = scanner.nextInt();
	if(selection == 1)
	{
	String lastName_input;
	ArrayList<String> arraylist1 = new ArrayList();
	ArrayList<String> arraylist2 = new ArrayList();
	int count_records = 0;
	String selectedName = null;
	
	System.out.println("Please type the last name of the patient whose information you'd like to view");
	Scanner scanner1 = new Scanner(System.in);
	lastName_input = scanner1.nextLine();
	
	
	
try
{
	String sql1 = "DELETE FROM allRecords";
	PreparedStatement preparedStmt1 = connect.prepareStatement(sql1);
	preparedStmt1.execute();
	
	String sql2 = "INSERT INTO allRecords SELECT * FROM (SELECT mgh1.last_Name, mgh1.first_name, mgh1.gender, mgh1.SSN, mgh1.dob, mgh1.insurance_co, mgh2.reason, mgh2.copay_amount_paid, mgh2.visit_date, mgh2.visit_number, mgh2.time_stamp, 'mgh' FROM patients_mgh mgh1, patient_history_mgh mgh2 WHERE mgh2.patient_id = mgh1.patient_id UNION SELECT L1.last_Name, L1.first_name, L1.gender, L1.SSN, L1.dob, L1.insurance_co, L2.reason, L2.copay_amount_paid, L2.visit_date, L2.visit_number, L2.time_stamp, 'Lahey' FROM patients_lahey L1, patient_history_lahey L2 WHERE L1.patient_id = L2.patient_id UNION SELECT W1.last_Name, W1.first_name, W1.gender, W1.SSN, W1.dob, W1.insurance_co, W2.reason, W2.copay_amount_paid, W2.visit_date, W2.visit_number, W2.time_stamp, 'Winchester' FROM patients_winchester W1, patient_history_winchester W2 WHERE W1.patient_id = W2.patient_id ORDER BY last_Name)a ORDER BY last_name";
	PreparedStatement preparedStmt2 = connect.prepareStatement(sql2);
	preparedStmt2.execute();
	
 
	

	

		
		arraylist2.add(0, "Patient Selection");
		// below we find all the first names for the inputted last name and store them in an array. 
		String sql5 = "SELECT first_name FROM (SELECT last_name, first_name FROM allrecords GROUP BY last_name, first_name)aR WHERE last_name = ? ";
		PreparedStatement preparedStmt5 = connect.prepareStatement(sql5);
		preparedStmt5.setString(1, lastName_input);
		ResultSet rs2 = preparedStmt5.executeQuery();
		
			while(rs2.next())
			{
				count_records++;
				String firstName= rs2.getString("first_name");
				arraylist2.add(firstName);

			}
			System.out.println("Records counted" + count_records);
			System.out.println(arraylist2.toString());
		
		if(count_records == 1)
		{
			selectedName = arraylist2.get(1);
			String sql3 = "SELECT * FROM allRecords WHERE last_name = ? AND first_name = ?";
			PreparedStatement preparedStmt3 = connect.prepareStatement(sql3);
			preparedStmt3.setString(1, lastName_input);
			preparedStmt3.setString(2, selectedName);
			ResultSet rs3 = preparedStmt3.executeQuery();
			String[][] tableValues = new String[1][12];
			while(rs3.next())
			{
			
				for(int i = 0; i < 12; i++ )
				{
					String value = rs3.getString(i + 1);
					tableValues[0][i] = value;
						

				}
				System.out.println(Arrays.deepToString(tableValues));	
							
			
			}
		
			
		
		
		
		}
		if(count_records > 1)
		{
			Object[] options = arraylist2.toArray();
			selectedName = (String)JOptionPane.showInputDialog(null, "Select a patient from the dropdown list", "Choose first name of patient", JOptionPane.QUESTION_MESSAGE, null, options, options[0]);
			String sql6 = "SELECT * FROM allRecords WHERE last_name = ? and first_name = ?";
			PreparedStatement preparedStmt4 = connect.prepareStatement(sql6);
			preparedStmt4.setString(1, lastName_input);
			preparedStmt4.setString(2, selectedName);
			ResultSet rs4 = preparedStmt4.executeQuery();
			
			String[][] tableValues = new String[1][12];
			
			while(rs4.next())
			{
				for(int i = 0; i <= 10; i++ )
				{
					String value = rs4.getString(i + 1);
					
					tableValues[0][i] = value;
						
				}
				String recSource = rs4.getString("rec_source");
				System.out.println("Record Source: " + recSource);
				tableValues[0][11] = recSource; 
				System.out.println(Arrays.deepToString(tableValues));
			}
				
						
			
		}// end of if statement
	



}

	
catch(SQLException e)
{
	e.printStackTrace();
	System.out.println("Could not execute method");

}
	
	}




	
}

public void modify_MGHRecords()
{

	System.out.println("Would you like to modify a MGH record? Press 1 for Yes and 2 for No");
	Scanner scanner3 = new Scanner(System.in);
	int b = scanner3.nextInt();
		while(b != 2)
		{		
		
		int patientID = 0;
	
// Program should display a particular record in a pop up window. 

	String[] fields = new String[8];
	fields[0] = "Last Name";
	fields[1] = "First Name";
	fields[2] = "Gender";
	fields[3] = "Date of Birth";
	fields[4] = "Insurance Company";
	fields[5] = "Visit Date";
	fields[6] = "Reason";
	fields[7] = "Amount paid";
	
	ArrayList<Integer> list_patientID = new ArrayList<>();
	final String[] tableValues = new String[8]; 
	final int[] arr_patientID = new int[1];
	
	String lastName = "";
	String firstName = "";
	String gender = "";
	Date date1 = null;
	Date date2 = null;
	String reason = "";
	Double amt = 0.00;
	String insCo = null;
	int optionSelected = 1;
	
	// JOptionPane should ask which fields the user would like to modify and allow multiple selections. 
	// Based on user input create a JFrame. 
	

	JFrame jFrame = new JFrame();
	JPanel jPanel = new JPanel();
	JButton jButton2 = new JButton();
	jButton2.setText("Save Changes");
	jButton2.setFont(new Font("SansSerif",Font.BOLD,25));
	jFrame.add(jPanel);
	jPanel.add(jButton2);
	
	JTextField textfield1 = new JTextField();
	JTextField textfield2 = new JTextField();
	JTextField textfield3 = new JTextField();
	JTextField textfield4 = new JTextField();
	JTextField textfield5 = new JTextField();
	JTextField textfield6 = new JTextField();
	JTextField textfield7 = new JTextField();
	JTextField textfield8 = new JTextField();

	JLabel jLabel1 = new JLabel();
	JLabel jLabel2 = new JLabel();
	JLabel jLabel3 = new JLabel();
	JLabel jLabel4 = new JLabel();
	JLabel jLabel5 = new JLabel();
	JLabel jLabel6 = new JLabel();
	JLabel jLabel7 = new JLabel();
	JLabel jLabel8 = new JLabel();
	jLabel1.setText("Last Name");
	jLabel1.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel2.setText("First Name");
	jLabel2.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel3.setText("Gender");
	jLabel3.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel4.setText("Date of Birth");
	jLabel4.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel5.setText("Insurance Company");
	jLabel5.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel6.setText("Visit Date");
	jLabel6.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel7.setText("Reason");
	jLabel7.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel8.setText("Amount Paid");
	jLabel8.setFont(new Font("SansSerif",Font.BOLD,18));
	
	textfield1.setPreferredSize(new Dimension(150,28));
	textfield2.setPreferredSize(new Dimension(150,28));
	textfield3.setPreferredSize(new Dimension(150,28));
	textfield4.setPreferredSize(new Dimension(150,28));
	textfield5.setPreferredSize(new Dimension(150,28));
	textfield6.setPreferredSize(new Dimension(150,28));
	textfield7.setPreferredSize(new Dimension(150,28));
	textfield8.setPreferredSize(new Dimension(150,28));
	
	textfield1.setEditable(false);
	textfield2.setEditable(false);
	textfield3.setEditable(false);
	textfield4.setEditable(false);
	textfield5.setEditable(false);
	textfield6.setEditable(false);
	textfield7.setEditable(false);
	textfield8.setEditable(false);
	
	jPanel.add(jLabel1);
	jPanel.add(textfield1);
	jPanel.add(jLabel2);
	jPanel.add(textfield2);
	jPanel.add(jLabel3);
	jPanel.add(textfield3);
	jPanel.add(jLabel4);
	jPanel.add(textfield4);
	jPanel.add(jLabel5);
	jPanel.add(textfield5);
	jPanel.add(jLabel6);
	jPanel.add(textfield6);
	jPanel.add(jLabel7);
	jPanel.add(textfield7);
	jPanel.add(jLabel8);
	jPanel.add(textfield8);

	
try
	{
		
	
	do
			{	
				// User inputs patientID to determine which record to modify. 
				
				System.out.println("To modify a MGH record, please enter patientID in numeric form.");
				Scanner scanner_inputID = new Scanner(System.in); 
				patientID = scanner_inputID.nextInt();
				
				String sql = "SELECT patient_id FROM patients_mgh";
				PreparedStatement preparedStmt = connect.prepareStatement(sql);
				ResultSet rs = preparedStmt.executeQuery();
				while(rs.next())
				{
					int retrievedPatientID = rs.getInt("patient_id");
					list_patientID.add(retrievedPatientID);
					
				}
				
				if(!list_patientID.contains(patientID))
				{
					System.out.println("This patient ID does not exist in our records");
					System.out.println("Would you like to check for another record to modify? Enter 1 for 'Yes' and 2 for 'No'");
					int c = scanner3.nextInt();
					if (c == 2)
						return;
				}
				arr_patientID[0] = patientID;
				
				String sql1 = "SELECT * FROM (SELECT mgh1.visit_number, mgh1.patient_id, mgh1.visit_date, mgh1.reason, mgh1.copay_amount_paid, mgh1.time_stamp, mgh2.last_name, mgh2.first_name, mgh2.gender, mgh2.SSN, mgh2.dob, mgh2.insurance_co FROM patient_history_mgh mgh1, patients_mgh mgh2 WHERE mgh1.patient_ID = mgh2.patient_ID)a WHERE patient_id = ?; ";
				PreparedStatement preparedStmt1 = connect.prepareStatement(sql1);
// below patientID is the user inputted Id. 		
				preparedStmt1.setInt(1, patientID);
				ResultSet rs1 = preparedStmt1.executeQuery();
				if(rs1.next())
				{
					lastName = rs1.getString("last_Name");
					firstName = rs1.getString("first_Name");
					gender = rs1.getString("gender");
					date2 = rs1.getDate("dob");
					insCo = rs1.getString("insurance_co");
					date1 = rs1.getDate("visit_date");
					reason = rs1.getString("reason");
					amt = rs1.getDouble("copay_amount_paid");
//					ssn = rs1.getString("ssn");
					tableValues[0] = lastName;
					tableValues[1] = firstName;
					tableValues[2] = gender;
					tableValues[3] = date2.toString();
					tableValues[4] = insCo;
					tableValues[5] = date1.toString();
					tableValues[6] = reason;
					tableValues[7] = amt.toString();		
					// String below encompasses all the values retrieved from the ResultSet object.
					String displayMessage1 = "Last Name: " + lastName + ", First Name: " + firstName + ", Gender: " + gender;
					JTextPane jTextPane = new JTextPane();
					jTextPane.setText(displayMessage1);
					Font font = new Font("SansSerif", Font.BOLD, 18);
					jTextPane.setFont(font);
					optionSelected = JOptionPane.showConfirmDialog(null, jTextPane, "Is this the record you'd like to modify?", JOptionPane.YES_NO_OPTION);

				}
 			
		
			
			}while(optionSelected == 1);
	
			
			if(optionSelected == 0)
				{
				
				JFrame jFrame1 = new JFrame("Below please select the fields you'd like to modify");
				JPanel jPanel1 = new JPanel();
				jPanel1.setSize(200, 200);
				JButton jButton1 = new JButton();
				jButton1.setText("Submit Selection");
				jButton1.setFont(new Font("SansSerif",Font.BOLD,25));
				
				
				jFrame1.add(jPanel1);
				JList jlist = new JList(fields);
				jlist.setFont(new Font("SansSerif", Font.BOLD, 30));
				jPanel1.add(jlist);
				jPanel1.add(jButton1);
				jButton1.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e)
					{
						// jButton1ExecuteAction(e);
						int[] selectedIndexes = jlist.getSelectedIndices();
						List<Integer> selectedIndexesList = new ArrayList<Integer>();
						for(int i = 0; i < selectedIndexes.length; i++)
						{
							selectedIndexesList.add(selectedIndexes[i]);
						}
	
						System.out.println(Arrays.toString(selectedIndexes));
						jFrame1.setVisible(false);
						jFrame1.dispose();
						
						
						if(selectedIndexesList.contains(0))
						{
							textfield1.setEditable(true);
							textfield1.setText(tableValues[0]);
						}
					
						if(selectedIndexesList.contains(1))
						{
							textfield2.setEditable(true);
							textfield2.setText(tableValues[1]);
						}
						if(selectedIndexesList.contains(2))
						{
							textfield3.setEditable(true);
							textfield3.setText(tableValues[2]);
						}
						if(selectedIndexesList.contains(3))
						{
							textfield4.setEditable(true);
							textfield4.setText(tableValues[3]);
						}
						if(selectedIndexesList.contains(4))
						{
							textfield5.setEditable(true);
							textfield5.setText(tableValues[4]);
						}
						if(selectedIndexesList.contains(5))
						{
							textfield6.setEditable(true);
							textfield6.setText(tableValues[5]);
						}
						if(selectedIndexesList.contains(6))
						{
							textfield7.setEditable(true);
							textfield7.setText(tableValues[6]);
						}
						if(selectedIndexesList.contains(7))
						{
							textfield8.setEditable(true);
							textfield8.setText(tableValues[7]);
						}
					
					}
				}

				);
				
				jButton2.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e2)
					{
						try
							{
							if(textfield1.isEditable())
							{
								String input1 = textfield1.getText();
								String sql2 = "UPDATE patients_mgh SET last_name = ? WHERE patient_id = ?  ";
								PreparedStatement preparedStmt2 = connect.prepareStatement(sql2);
								preparedStmt2.setString(1, input1);
								preparedStmt2.setInt(2, arr_patientID[0]);
								preparedStmt2.execute();
								
							}
					
							if(textfield2.isEditable())
							{
								String input2 = textfield2.getText();
								String sql3 = "UPDATE patients_mgh SET first_name = ? WHERE patient_id = ?  ";
								PreparedStatement preparedStmt3 = connect.prepareStatement(sql3);
								preparedStmt3.setString(1, input2);
								preparedStmt3.setInt(2, arr_patientID[0]);
								preparedStmt3.execute();
								
							}
							if(textfield3.isEditable())
							{
								String input3 = textfield3.getText();
								String sql4 = "UPDATE patients_mgh SET gender = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt4 = connect.prepareStatement(sql4);
								preparedStmt4.setString(1, input3);
								preparedStmt4.setInt(2, arr_patientID[0]);
								preparedStmt4.execute();
								
							}
							if(textfield4.isEditable())
							{
								String input4 = textfield4.getText();
								String sql5 = "UPDATE patients_mgh SET dob = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt5 = connect.prepareStatement(sql5);
								preparedStmt5.setString(1, input4);
								preparedStmt5.setInt(2, arr_patientID[0]);
								preparedStmt5.execute();
								
							}
							
							if(textfield5.isEditable())
							{
								String input5 = textfield5.getText();
								String sql6 = "UPDATE patients_mgh SET insurance_co = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt6 = connect.prepareStatement(sql6);
								preparedStmt6.setString(1, input5);
								preparedStmt6.setInt(2, arr_patientID[0]);
								preparedStmt6.execute();
								
							}
							
							if(textfield6.isEditable())
							{
								String input6 = textfield6.getText();
								String sql7 = "UPDATE patient_history_mgh SET visit_date = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt7 = connect.prepareStatement(sql7);
								preparedStmt7.setString(1, input6);
								preparedStmt7.setInt(2, arr_patientID[0]);
								preparedStmt7.execute();
								
							}
							
							if(textfield7.isEditable())
							{
								String input7 = textfield7.getText();
								String sql8 = "UPDATE patient_history_mgh SET reason = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt8 = connect.prepareStatement(sql8);
								preparedStmt8.setString(1, input7);
								preparedStmt8.setInt(2, arr_patientID[0]);
								preparedStmt8.execute();
								
							}
							if(textfield8.isEditable())
							{
								String input8 = textfield8.getText();
								String sql9 = "UPDATE patient_history_mgh SET copay_amount_paid = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt8 = connect.prepareStatement(sql9);
								preparedStmt8.setString(1, input8);
								preparedStmt8.setInt(2, arr_patientID[0]);
								preparedStmt8.execute();
								
							}
							
							}// end of try block
						catch(SQLException e3)
						{
							System.out.println("ZZZZZZ");
							e3.printStackTrace();
						}
					
						jFrame.setVisible(false);
						jFrame.dispose();
					
					
					}
				}
				
				);
				
				jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
				jFrame.setSize(300, 300);
				jFrame.setVisible(true);
				
				jFrame1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
				jFrame1.setSize(300, 300);
				jFrame1.setVisible(true);

				

					
				
				
}
			
			System.out.println("Would you like to modify another record? Enter 1 for Yes and 2 for No");
			b = scanner3.nextInt();
			
				
	
			//	JOptionPane.showMessageDialog(null, jlist, "Please select the fields that you'd like to modify", JOptionPane.PLAIN_MESSAGE);
			
			 // end of while statement b
		
	
	// add code here to repeat loop	
		
		
		}// end of the try block

	
		catch(SQLException e)
		{
			e.printStackTrace();
		}

	} // end of while loop
}
	 // end of method declaration. 		 

public void modify_WinchesterRecords()
{

	System.out.println("Would you like to modify a Winchester Hospital record? Press 1 for Yes and 2 for No");
	Scanner scanner3 = new Scanner(System.in);
	int b = scanner3.nextInt();
		while(b != 2)
		{		
		
		int patientID = 0;
	
	// Program should display a particular record in a pop up window. 
	String[] fields = new String[8];
	fields[0] = "Last Name";
	fields[1] = "First Name";
	fields[2] = "Gender";
	fields[3] = "Date of Birth";
	fields[4] = "Insurance Company";
	fields[5] = "Visit Date";
	fields[6] = "Reason";
	fields[7] = "Amount paid";
	
	ArrayList<Integer> list_patientID = new ArrayList<>();
	final String[] tableValues = new String[8]; 
	final int[] arr_patientID = new int[1];
	
	String lastName = "";
	String firstName = "";
	String gender = "";
	Date date1 = null;
	Date date2 = null;
	String reason = "";
	Double amt = 0.00;
	String insCo = null;
	int optionSelected = 1; 
	
	// JOptionPane should ask which fields the user would like to modify and allow multiple selections. 
	// Based on user input create a JFrame. 
	
	JFrame jFrame = new JFrame();
	JPanel jPanel = new JPanel();
	JButton jButton2 = new JButton();
	jButton2.setText("Save Changes");
	jButton2.setFont(new Font("SansSerif",Font.BOLD,25));
	jFrame.add(jPanel);
	jPanel.add(jButton2);
	
	JTextField textfield1 = new JTextField();
	JTextField textfield2 = new JTextField();
	JTextField textfield3 = new JTextField();
	JTextField textfield4 = new JTextField();
	JTextField textfield5 = new JTextField();
	JTextField textfield6 = new JTextField();
	JTextField textfield7 = new JTextField();
	JTextField textfield8 = new JTextField();

	JLabel jLabel1 = new JLabel();
	JLabel jLabel2 = new JLabel();
	JLabel jLabel3 = new JLabel();
	JLabel jLabel4 = new JLabel();
	JLabel jLabel5 = new JLabel();
	JLabel jLabel6 = new JLabel();
	JLabel jLabel7 = new JLabel();
	JLabel jLabel8 = new JLabel();
	jLabel1.setText("Last Name");
	jLabel1.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel2.setText("First Name");
	jLabel2.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel3.setText("Gender");
	jLabel3.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel4.setText("Date of Birth");
	jLabel4.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel5.setText("Insurance Company");
	jLabel5.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel6.setText("Visit Date");
	jLabel6.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel7.setText("Reason");
	jLabel7.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel8.setText("Amount Paid");
	jLabel8.setFont(new Font("SansSerif",Font.BOLD,18));
	
	textfield1.setPreferredSize(new Dimension(150,28));
	textfield2.setPreferredSize(new Dimension(150,28));
	textfield3.setPreferredSize(new Dimension(150,28));
	textfield4.setPreferredSize(new Dimension(150,28));
	textfield5.setPreferredSize(new Dimension(150,28));
	textfield6.setPreferredSize(new Dimension(150,28));
	textfield7.setPreferredSize(new Dimension(150,28));
	textfield8.setPreferredSize(new Dimension(150,28));
	
	textfield1.setEditable(false);
	textfield2.setEditable(false);
	textfield3.setEditable(false);
	textfield4.setEditable(false);
	textfield5.setEditable(false);
	textfield6.setEditable(false);
	textfield7.setEditable(false);
	textfield8.setEditable(false);
	
	jPanel.add(jLabel1);
	jPanel.add(textfield1);
	jPanel.add(jLabel2);
	jPanel.add(textfield2);
	jPanel.add(jLabel3);
	jPanel.add(textfield3);
	jPanel.add(jLabel4);
	jPanel.add(textfield4);
	jPanel.add(jLabel5);
	jPanel.add(textfield5);
	jPanel.add(jLabel6);
	jPanel.add(textfield6);
	jPanel.add(jLabel7);
	jPanel.add(textfield7);
	jPanel.add(jLabel8);
	jPanel.add(textfield8);

	
try
	{
		
	
	do
			{	
				// User inputs patientID which determines which record gets modified.  
				
				System.out.println("To modify a Winchester Hospital record, please enter patientID in numeric form.");
				Scanner scanner_inputID = new Scanner(System.in); 
				patientID = scanner_inputID.nextInt();
				
				String sql = "SELECT patient_id FROM patients_winchester";
				PreparedStatement preparedStmt = connect.prepareStatement(sql);
				ResultSet rs = preparedStmt.executeQuery();
				while(rs.next())
				{
					int retrievedPatientID = rs.getInt("patient_id");
					list_patientID.add(retrievedPatientID);
					
				}
				
				if(!list_patientID.contains(patientID))
				{
					System.out.println("This patient ID does not exist in Winchester records");
					System.out.println("Would you like to check for another record to modify? Enter 1 for 'Yes' and 2 for 'No'");
					int c = scanner3.nextInt();
					if (c == 2)
						return;
				}
				arr_patientID[0] = patientID;
				String sql1 = "SELECT * FROM (SELECT win1.visit_number, win1.patient_id, win1.visit_date, win1.reason, win1.copay_amount_paid, win1.time_stamp, win2.last_name, win2.first_name, win2.gender, win2.SSN, win2.dob, win2.insurance_co FROM patient_history_winchester win1, patients_winchester win2 WHERE win1.patient_ID = win2.patient_ID)a WHERE patient_id = ?; ";
				PreparedStatement preparedStmt1 = connect.prepareStatement(sql1);
				// below patientID is the user inputted value. 		
				preparedStmt1.setInt(1, patientID);
				ResultSet rs1 = preparedStmt1.executeQuery();
				if(rs1.next())
				{
					lastName = rs1.getString("last_Name");
					firstName = rs1.getString("first_Name");
					gender = rs1.getString("gender");
					date2 = rs1.getDate("dob");
					insCo = rs1.getString("insurance_co");
					date1 = rs1.getDate("visit_date");
					reason = rs1.getString("reason");
					amt = rs1.getDouble("copay_amount_paid");
//					ssn = rs1.getString("ssn");
					tableValues[0] = lastName;
					tableValues[1] = firstName;
					tableValues[2] = gender;
					tableValues[3] = date2.toString();
					tableValues[4] = insCo;
					tableValues[5] = date1.toString();
					tableValues[6] = reason;
					tableValues[7] = amt.toString();		
					// String below encompasses all the values retrieved from the ResultSet object.
					String displayMessage1 = "Last Name: " + lastName + ", First Name: " + firstName + ", Gender: " + gender;
					JTextPane jTextPane = new JTextPane();
					jTextPane.setText(displayMessage1);
					Font font = new Font("SansSerif", Font.BOLD, 18);
					jTextPane.setFont(font);
					optionSelected = JOptionPane.showConfirmDialog(null, jTextPane, "Is this the record you'd like to modify?", JOptionPane.YES_NO_OPTION);

				}
 			
		
			
			}while(optionSelected == 1);
	
			
			if(optionSelected == 0)
				{
				
				JFrame jFrame1 = new JFrame("Below please select the fields you'd like to modify");
				JPanel jPanel1 = new JPanel();
				jPanel1.setSize(200, 200);
				JButton jButton1 = new JButton();
				jButton1.setText("Submit Selection");
				jButton1.setFont(new Font("SansSerif",Font.BOLD,25));
				
				
				jFrame1.add(jPanel1);
				JList jlist = new JList(fields);
				jlist.setFont(new Font("SansSerif", Font.BOLD, 30));
				jPanel1.add(jlist);
				jPanel1.add(jButton1);
				jButton1.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e)
					{
						
						int[] selectedIndexes = jlist.getSelectedIndices();
						List<Integer> selectedIndexesList = new ArrayList<Integer>();
						for(int i = 0; i < selectedIndexes.length; i++)
						{
							selectedIndexesList.add(selectedIndexes[i]);
						}
	
//						System.out.println(Arrays.toString(selectedIndexes));
//						Disposal of the current JFrame						
						jFrame1.setVisible(false);
						jFrame1.dispose();
						
						
						if(selectedIndexesList.contains(0))
						{
							textfield1.setEditable(true);
							textfield1.setText(tableValues[0]);
						}
					
						if(selectedIndexesList.contains(1))
						{
							textfield2.setEditable(true);
							textfield2.setText(tableValues[1]);
						}
						if(selectedIndexesList.contains(2))
						{
							textfield3.setEditable(true);
							textfield3.setText(tableValues[2]);
						}
						if(selectedIndexesList.contains(3))
						{
							textfield4.setEditable(true);
							textfield4.setText(tableValues[3]);
						}
						if(selectedIndexesList.contains(4))
						{
							textfield5.setEditable(true);
							textfield5.setText(tableValues[4]);
						}
						if(selectedIndexesList.contains(5))
						{
							textfield6.setEditable(true);
							textfield6.setText(tableValues[5]);
						}
						if(selectedIndexesList.contains(6))
						{
							textfield7.setEditable(true);
							textfield7.setText(tableValues[6]);
						}
						if(selectedIndexesList.contains(7))
						{
							textfield8.setEditable(true);
							textfield8.setText(tableValues[7]);
						}
					
					}
				}

				);
				
				jButton2.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e2)
					{
						try
							{
							if(textfield1.isEditable())
							{
								String input1 = textfield1.getText();
								String sql2 = "UPDATE patients_winchester SET last_name = ? WHERE patient_id = ?  ";
								PreparedStatement preparedStmt2 = connect.prepareStatement(sql2);
								preparedStmt2.setString(1, input1);
								preparedStmt2.setInt(2, arr_patientID[0]);
								preparedStmt2.execute();
								
							}
					
							if(textfield2.isEditable())
							{
								String input2 = textfield2.getText();
								String sql3 = "UPDATE patients_winchester SET first_name = ? WHERE patient_id = ?  ";
								PreparedStatement preparedStmt3 = connect.prepareStatement(sql3);
								preparedStmt3.setString(1, input2);
								preparedStmt3.setInt(2, arr_patientID[0]);
								preparedStmt3.execute();
								
							}
							if(textfield3.isEditable())
							{
								String input3 = textfield3.getText();
								String sql4 = "UPDATE patients_winchester SET gender = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt4 = connect.prepareStatement(sql4);
								preparedStmt4.setString(1, input3);
								preparedStmt4.setInt(2, arr_patientID[0]);
								preparedStmt4.execute();
								
							}
							if(textfield4.isEditable())
							{
								String input4 = textfield4.getText();
								String sql5 = "UPDATE patients_winchester SET dob = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt5 = connect.prepareStatement(sql5);
								preparedStmt5.setString(1, input4);
								preparedStmt5.setInt(2, arr_patientID[0]);
								preparedStmt5.execute();
								
							}
							
							if(textfield5.isEditable())
							{
								String input5 = textfield5.getText();
								String sql6 = "UPDATE patients_winchester SET insurance_co = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt6 = connect.prepareStatement(sql6);
								preparedStmt6.setString(1, input5);
								preparedStmt6.setInt(2, arr_patientID[0]);
								preparedStmt6.execute();
								
							}
							
							if(textfield6.isEditable())
							{
								String input6 = textfield6.getText();
								String sql7 = "UPDATE patient_history_winchester SET visit_date = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt7 = connect.prepareStatement(sql7);
								preparedStmt7.setString(1, input6);
								preparedStmt7.setInt(2, arr_patientID[0]);
								preparedStmt7.execute();
								
							}
							
							if(textfield7.isEditable())
							{
								String input7 = textfield7.getText();
								String sql8 = "UPDATE patient_history_winchester SET reason = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt8 = connect.prepareStatement(sql8);
								preparedStmt8.setString(1, input7);
								preparedStmt8.setInt(2, arr_patientID[0]);
								preparedStmt8.execute();
								
							}
							if(textfield8.isEditable())
							{
								String input8 = textfield8.getText();
								String sql9 = "UPDATE patient_history_winchester SET copay_amount_paid = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt8 = connect.prepareStatement(sql9);
								preparedStmt8.setString(1, input8);
								preparedStmt8.setInt(2, arr_patientID[0]);
								preparedStmt8.execute();
								
							}
							
							}// end of try block
						catch(SQLException e3)
						{
							System.out.println("Encountered problem while trying to update database");
							e3.printStackTrace();
						}
					//Disposal of the current JFrame object
						jFrame.setVisible(false);
						jFrame.dispose();
					
					
					}
				}
				
				);
				
				jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
				jFrame.setSize(300, 300);
				jFrame.setVisible(true);
				
				jFrame1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
				jFrame1.setSize(300, 300);
				jFrame1.setVisible(true);

				

					
				
				
}
			
			System.out.println("Would you like to modify another record? Enter 1 for Yes and 2 for No");
			b = scanner3.nextInt();
			
					
		
		} // end of the try block

	
		catch(SQLException e)
		{
			e.printStackTrace();
		}

	} // end of while loop
} // end of method declaration
	

public void modify_LaheyRecords()
{

	System.out.println("Would you like to modify a Lahey Hospital record? Press 1 for Yes and 2 for No");
	Scanner scanner3 = new Scanner(System.in);
	int b = scanner3.nextInt();
		while(b != 2)
		{		
		
		int patientID = 0;
	
	// Program should display a particular record in a pop up window. 
	String[] fields = new String[8];
	fields[0] = "Last Name";
	fields[1] = "First Name";
	fields[2] = "Gender";
	fields[3] = "Date of Birth";
	fields[4] = "Insurance Company";
	fields[5] = "Visit Date";
	fields[6] = "Reason";
	fields[7] = "Amount paid";
	
	ArrayList<Integer> list_patientID = new ArrayList<>();
	final String[] tableValues = new String[8]; 
	final int[] arr_patientID = new int[1];
	
	String lastName = "";
	String firstName = "";
	String gender = "";
	Date date1 = null;
	Date date2 = null;
	String reason = "";
	Double amt = 0.00;
	String insCo = null;
	int optionSelected = 1; 
	
	// JOptionPane should ask which fields the user would like to modify and allow multiple selections. 
	// Based on user input create a JFrame. 
	
	JFrame jFrame = new JFrame();
	JPanel jPanel = new JPanel();
	JButton jButton2 = new JButton();
	jButton2.setText("Save Changes");
	jButton2.setFont(new Font("SansSerif",Font.BOLD,25));
	jFrame.add(jPanel);
	jPanel.add(jButton2);
	
	JTextField textfield1 = new JTextField();
	JTextField textfield2 = new JTextField();
	JTextField textfield3 = new JTextField();
	JTextField textfield4 = new JTextField();
	JTextField textfield5 = new JTextField();
	JTextField textfield6 = new JTextField();
	JTextField textfield7 = new JTextField();
	JTextField textfield8 = new JTextField();

	JLabel jLabel1 = new JLabel();
	JLabel jLabel2 = new JLabel();
	JLabel jLabel3 = new JLabel();
	JLabel jLabel4 = new JLabel();
	JLabel jLabel5 = new JLabel();
	JLabel jLabel6 = new JLabel();
	JLabel jLabel7 = new JLabel();
	JLabel jLabel8 = new JLabel();
	jLabel1.setText("Last Name");
	jLabel1.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel2.setText("First Name");
	jLabel2.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel3.setText("Gender");
	jLabel3.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel4.setText("Date of Birth");
	jLabel4.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel5.setText("Insurance Company");
	jLabel5.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel6.setText("Visit Date");
	jLabel6.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel7.setText("Reason");
	jLabel7.setFont(new Font("SansSerif",Font.BOLD,18));
	jLabel8.setText("Amount Paid");
	jLabel8.setFont(new Font("SansSerif",Font.BOLD,18));
	
	textfield1.setPreferredSize(new Dimension(150,28));
	textfield2.setPreferredSize(new Dimension(150,28));
	textfield3.setPreferredSize(new Dimension(150,28));
	textfield4.setPreferredSize(new Dimension(150,28));
	textfield5.setPreferredSize(new Dimension(150,28));
	textfield6.setPreferredSize(new Dimension(150,28));
	textfield7.setPreferredSize(new Dimension(150,28));
	textfield8.setPreferredSize(new Dimension(150,28));
	
	textfield1.setEditable(false);
	textfield2.setEditable(false);
	textfield3.setEditable(false);
	textfield4.setEditable(false);
	textfield5.setEditable(false);
	textfield6.setEditable(false);
	textfield7.setEditable(false);
	textfield8.setEditable(false);
	
	jPanel.add(jLabel1);
	jPanel.add(textfield1);
	jPanel.add(jLabel2);
	jPanel.add(textfield2);
	jPanel.add(jLabel3);
	jPanel.add(textfield3);
	jPanel.add(jLabel4);
	jPanel.add(textfield4);
	jPanel.add(jLabel5);
	jPanel.add(textfield5);
	jPanel.add(jLabel6);
	jPanel.add(textfield6);
	jPanel.add(jLabel7);
	jPanel.add(textfield7);
	jPanel.add(jLabel8);
	jPanel.add(textfield8);

	
try
	{
		
	
	do
			{	
				// User inputs patientID which determines which record gets modified.  
				
				System.out.println("To modify a Lahey Hospital record, please enter patientID in numeric form.");
				Scanner scanner_inputID = new Scanner(System.in); 
				patientID = scanner_inputID.nextInt();
				
				String sql = "SELECT patient_id FROM patients_lahey";
				PreparedStatement preparedStmt = connect.prepareStatement(sql);
				ResultSet rs = preparedStmt.executeQuery();
				while(rs.next())
				{
					int retrievedPatientID = rs.getInt("patient_id");
					list_patientID.add(retrievedPatientID);
					
				}
				
				if(!list_patientID.contains(patientID))
				{
					System.out.println("This patient ID does not exist in Lahey records");
					System.out.println("Would you like to check for another record to modify? Enter 1 for 'Yes' and 2 for 'No'");
					int c = scanner3.nextInt();
					if (c == 2)
						return;
				}
				arr_patientID[0] = patientID;
				String sql1 = "SELECT * FROM (SELECT lah1.visit_number, lah1.patient_id, lah1.visit_date, lah1.reason, lah1.copay_amount_paid, lah1.time_stamp, lah2.last_name, lah2.first_name, lah2.gender, lah2.SSN, lah2.dob, lah2.insurance_co FROM patient_history_lahey lah1, patients_lahey lah2 WHERE lah1.patient_ID = lah2.patient_ID)a WHERE patient_id = ?; ";
				PreparedStatement preparedStmt1 = connect.prepareStatement(sql1);
				// below patientID is the user inputted value. 		
				preparedStmt1.setInt(1, patientID);
				ResultSet rs1 = preparedStmt1.executeQuery();
				if(rs1.next())
				{
					lastName = rs1.getString("last_Name");
					firstName = rs1.getString("first_Name");
					gender = rs1.getString("gender");
					date2 = rs1.getDate("dob");
					insCo = rs1.getString("insurance_co");
					date1 = rs1.getDate("visit_date");
					reason = rs1.getString("reason");
					amt = rs1.getDouble("copay_amount_paid");
//					ssn = rs1.getString("ssn");
					tableValues[0] = lastName;
					tableValues[1] = firstName;
					tableValues[2] = gender;
					tableValues[3] = date2.toString();
					tableValues[4] = insCo;
					tableValues[5] = date1.toString();
					tableValues[6] = reason;
					tableValues[7] = amt.toString();		
					// String below encompasses all the values retrieved from the ResultSet object.
					String displayMessage1 = "Last Name: " + lastName + ", First Name: " + firstName + ", Gender: " + gender;
					JTextPane jTextPane = new JTextPane();
					jTextPane.setText(displayMessage1);
					Font font = new Font("SansSerif", Font.BOLD, 18);
					jTextPane.setFont(font);
					optionSelected = JOptionPane.showConfirmDialog(null, jTextPane, "Is this the record you'd like to modify?", JOptionPane.YES_NO_OPTION);

				}
 			
		
			
			}while(optionSelected == 1);
	
			
			if(optionSelected == 0)
				{
				
				JFrame jFrame1 = new JFrame("Below please select the fields you'd like to modify");
				JPanel jPanel1 = new JPanel();
				jPanel1.setSize(200, 200);
				JButton jButton1 = new JButton();
				jButton1.setText("Submit Selection");
				jButton1.setFont(new Font("SansSerif",Font.BOLD,25));
				
				
				jFrame1.add(jPanel1);
				JList jlist = new JList(fields);
				jlist.setFont(new Font("SansSerif", Font.BOLD, 30));
				jPanel1.add(jlist);
				jPanel1.add(jButton1);
				jButton1.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e)
					{
						
						int[] selectedIndexes = jlist.getSelectedIndices();
						List<Integer> selectedIndexesList = new ArrayList<Integer>();
						for(int i = 0; i < selectedIndexes.length; i++)
						{
							selectedIndexesList.add(selectedIndexes[i]);
						}
	
//						System.out.println(Arrays.toString(selectedIndexes));
//						Disposal of the current JFrame						
						jFrame1.setVisible(false);
						jFrame1.dispose();
						
						
						if(selectedIndexesList.contains(0))
						{
							textfield1.setEditable(true);
							textfield1.setText(tableValues[0]);
						}
					
						if(selectedIndexesList.contains(1))
						{
							textfield2.setEditable(true);
							textfield2.setText(tableValues[1]);
						}
						if(selectedIndexesList.contains(2))
						{
							textfield3.setEditable(true);
							textfield3.setText(tableValues[2]);
						}
						if(selectedIndexesList.contains(3))
						{
							textfield4.setEditable(true);
							textfield4.setText(tableValues[3]);
						}
						if(selectedIndexesList.contains(4))
						{
							textfield5.setEditable(true);
							textfield5.setText(tableValues[4]);
						}
						if(selectedIndexesList.contains(5))
						{
							textfield6.setEditable(true);
							textfield6.setText(tableValues[5]);
						}
						if(selectedIndexesList.contains(6))
						{
							textfield7.setEditable(true);
							textfield7.setText(tableValues[6]);
						}
						if(selectedIndexesList.contains(7))
						{
							textfield8.setEditable(true);
							textfield8.setText(tableValues[7]);
						}
					
					}
				}

				);
				
				jButton2.addActionListener(new ActionListener()
				{
					public void actionPerformed(ActionEvent e2)
					{
						try
							{
							if(textfield1.isEditable())
							{
								String input1 = textfield1.getText();
								String sql2 = "UPDATE patients_lahey SET last_name = ? WHERE patient_id = ?  ";
								PreparedStatement preparedStmt2 = connect.prepareStatement(sql2);
								preparedStmt2.setString(1, input1);
								preparedStmt2.setInt(2, arr_patientID[0]);
								preparedStmt2.execute();
								
							}
					
							if(textfield2.isEditable())
							{
								String input2 = textfield2.getText();
								String sql3 = "UPDATE patients_lahey SET first_name = ? WHERE patient_id = ?  ";
								PreparedStatement preparedStmt3 = connect.prepareStatement(sql3);
								preparedStmt3.setString(1, input2);
								preparedStmt3.setInt(2, arr_patientID[0]);
								preparedStmt3.execute();
								
							}
							if(textfield3.isEditable())
							{
								String input3 = textfield3.getText();
								String sql4 = "UPDATE patients_lahey SET gender = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt4 = connect.prepareStatement(sql4);
								preparedStmt4.setString(1, input3);
								preparedStmt4.setInt(2, arr_patientID[0]);
								preparedStmt4.execute();
								
							}
							if(textfield4.isEditable())
							{
								String input4 = textfield4.getText();
								String sql5 = "UPDATE patients_lahey SET dob = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt5 = connect.prepareStatement(sql5);
								preparedStmt5.setString(1, input4);
								preparedStmt5.setInt(2, arr_patientID[0]);
								preparedStmt5.execute();
								
							}
							
							if(textfield5.isEditable())
							{
								String input5 = textfield5.getText();
								String sql6 = "UPDATE patients_lahey SET insurance_co = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt6 = connect.prepareStatement(sql6);
								preparedStmt6.setString(1, input5);
								preparedStmt6.setInt(2, arr_patientID[0]);
								preparedStmt6.execute();
								
							}
							
							if(textfield6.isEditable())
							{
								String input6 = textfield6.getText();
								String sql7 = "UPDATE patient_history_lahey SET visit_date = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt7 = connect.prepareStatement(sql7);
								preparedStmt7.setString(1, input6);
								preparedStmt7.setInt(2, arr_patientID[0]);
								preparedStmt7.execute();
								
							}
							
							if(textfield7.isEditable())
							{
								String input7 = textfield7.getText();
								String sql8 = "UPDATE patient_history_lahey SET reason = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt8 = connect.prepareStatement(sql8);
								preparedStmt8.setString(1, input7);
								preparedStmt8.setInt(2, arr_patientID[0]);
								preparedStmt8.execute();
								
							}
							if(textfield8.isEditable())
							{
								String input8 = textfield8.getText();
								String sql9 = "UPDATE patient_history_lahey SET copay_amount_paid = ? WHERE patient_id = ? ";
								PreparedStatement preparedStmt8 = connect.prepareStatement(sql9);
								preparedStmt8.setString(1, input8);
								preparedStmt8.setInt(2, arr_patientID[0]);
								preparedStmt8.execute();
								
							}
							
							}// end of try block
						catch(SQLException e3)
						{
							System.out.println("Encountered problem while trying to update database");
							e3.printStackTrace();
						}
					//Disposal of the current JFrame object
						jFrame.setVisible(false);
						jFrame.dispose();
					
					
					}
				}
				
				);
				
				jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
				jFrame.setSize(300, 300);
				jFrame.setVisible(true);
				
				jFrame1.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
				jFrame1.setSize(300, 300);
				jFrame1.setVisible(true);

				

					
				
				
}
			
			System.out.println("Would you like to modify another record? Enter 1 for Yes and 2 for No");
			b = scanner3.nextInt();
			
					
		
		} // end of the try block

	
		catch(SQLException e)
		{
			e.printStackTrace();
		}

	} // end of while loop
} // end of method declaration


} // end of class declaration. 		 

		
