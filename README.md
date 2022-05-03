# Study-Buddy
Study Buddy is an online study app, which rewards users for quiz completions. The app caters to learners of all experience levels by enabling them to easily enroll in courses by difficulty level. Study Buddy’s alluring balance of challenge and reward helps facilitate engaging user experience.


//code//

package com.project0.StudyBuddy;
import java.util.*;
import java.sql.*;
 
public class StudyBuddy  {
	 public static Connection con = null;
	public static Scanner sc = new Scanner(System.in);
	
	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
	
		try
		{
		
		
			Class.forName("com.mysql.cj.jdbc.Driver");
			String url="jdbc:mysql://localhost:3306/project0";
			String user="root";
			String password="Rahul@123";
			con=DriverManager.getConnection(url,user,password);	
			
			
		boolean flag = true;	
		do
		{
		System.out.println();
		System.out.println("*****Welcome To STUDY-BUDDY*****");
		System.out.println("1:Registration");
		System.out.println("2:Take a Quiz");
		System.out.println("3:Enroll Courses");
		System.out.println("4:Display");
		System.out.println("5: Exit");
		
		
		 
		
		int ch = sc.nextInt();
		switch(ch)
		{
		case 1: 
			
			insertRecord();
			break;
        case 2: 
			
        	Quiz quiz = new Quiz();
            quiz.begin();
			break;
        case 3:
        	System.out.println("Select the course which you want to register as per the difficulty level");
        	Displayc();
        	System.out.println("Enter the Course Id");
        	int Id = sc.nextInt();
        	 System.out.println("Enter your mobile number");
            long phone_no = sc.nextLong();
        	
            String s1="update student set c_id="+ Id +" where phone_no=" + phone_no;
            Statement stmt = con.createStatement();
                
                stmt.executeUpdate(s1);
                System.out.println("Database updated successfully ");
    		
        	break;
        case 4:
        	Display();
        	
        	break;
        case 5:
        	System.out.println("Exited successfully");
        	flag= false;
        	break;
        	
		}
		}while(flag);
		
		}
		catch(Exception e)
		{
			System.out.println(e);
		}

	}
	public static int insertRecord() throws Exception {
		 System.out.println("Enter Name");
		 String name = sc.nextLine();
		 System.out.println("Enter Phone number");
		 double phone_no= sc.nextDouble();
		 System.out.println("Enter City");
		 String city = sc.next();
		 int c_id = sc.nextInt();
		 
		 
		String s1="insert into student values(?,?,?,?)";
		PreparedStatement pre= con.prepareStatement(s1);
	
		pre.setString(1, name);
		
		pre.setDouble(2, phone_no);
		
		pre.setString(3,city);
		pre.setString(4,null);
		
		int rows =pre.executeUpdate();
		if(rows>0) {
			System.out.println("Record inserted succesfully");
			
		}return rows;
		}
	private static void Display() throws Exception  {
		String name ;
		 long phone_no;
		 String city;
		 int c_id ;
		 
		 
		String sq="select * from student";
		Statement st= con.createStatement();
		ResultSet rt=st.executeQuery(sq);
		
		System.out.println("name   |    phone_no    |   city   |    c_id" );
		
		while(rt.next()) 
		{
			
			 c_id = rt.getInt("c_id");
			 phone_no = rt.getLong("phone_no");
			
			 name = rt.getString("name");
			
			 city = rt.getString("city");
			
			
			System.out.println(name +"    |  "+phone_no +"    |  " + city + "    |  " +c_id );
			
			
		}
	}
		private static void Displayc() throws Exception  {
			int c_id ;
			 String c_name;
			 String difficulty_level;
			
			 
			 
			String sq="select * from courses";
			Statement st= con.createStatement();
			ResultSet rt=st.executeQuery(sq);
			
			System.out.println("c_id   |    c_name    |   Diff_level" );
			  
			while(rt.next()) {
				
				 c_id = rt.getInt("c_id");
				 
				
				 c_name = rt.getString("c_name");
				
				 difficulty_level = rt.getString("difficulty_level");
				
				
				System.out.println(c_id +"    |  "+c_name +"    |  " + difficulty_level  );
				
				
			} 
		
	}


}

