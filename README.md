# Mock-Hospital-System
Implementation of Java methods that allow the insertion of records into the database, and removal of duplicate records from table. Also, user is able to view records associated with a particular patient by inputting patient last name.  

Feature 1: For insertion of records into the database, user input is accepted from the console. User input is used to add information to 2 tables associated with each hospital. Once user is finished creating a record, program asks whether user would like to enter another record into the database. 

Feature 2: Presence of duplicates is determined if more than one record in the table has the same patient ID and same patient visit number. In this case, the record with the older visit date is deleted. A record is also considered duplicate if more than one record in the table has the same patient ID, same patient visit number, and patient visit date. Each record in the table has a time stamp of when the record was created. The record with the older time stamp is deleted from the table. 

Feature 3: User can type in the patient's last name and all the information associated with this patient is dispalyed in the console. Patient information from all 3 hospitals will be dispalyed to the application user. A multi dimensional array was used to dispay information to the user. 

Feature 4: There are 2 tables associated with each hospital. Information from all 8 tables is dispalyed to the application user. Information is displayed to the user with the use of a multidimensional array. In other words, information from all 3 hospitals is compiled and displayed to the user. 

Feature 5: User is able to modify the information within all 8 tables. Pop up window allows the user to verify a record for modification. If the user verifies, a GUI is displayed that shows the pre existing record information and allows the user to edit this information. With a click of a button, new information gets saved to the database. The number of textfields available in the GUI varies depending on the number of records in the table.  




Created several graphical user interfaces that allow application user to modify the records contained in the database. Another useful functionality of this application is that user is able to view all the records within all 3 hospital systems all at once.    
