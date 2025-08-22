# Enterprice Java : --

- First Download Payara Server Community Edition 6.2025.7(Full) -> [https://www.payara.fish/downloads/payara-platform-community-edition/]

# How to Create project When using Servlet :--
- Apache Netbeans 25 -> New Project -> Java with Maven -> Web Application -> Payara Server -> Jakarata EE 10 Web ->Source Packages ->Servlet Package -> Servlet.java
  (If Network not Available then Java with Ant -> Java Web -> Web Application)

# How to add dependencies :---
- in mavenproject -> Project Files ->
- **pom.xml** :-- Add below Dependencies  
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.47</version>
      <scope>provided</scope>            
  </dependency>

=> mysql-connector-java is the official MySQL JDBC driver.It allows your Servlet (Java code) to connect to a MySQL database.Adding this dependency makes Maven download the MySQL driver jar and put it in project’s classpath.add this dependency so Servlet can connect and interact with a MySQL database.

# How to perform CRUD using Servlet
- in mavenproject -> Source Package -> new package create -> model ->
  
- **OptimizedLogic.java :--**

package model;

import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.Collection;
import java.util.logging.Level;
import java.util.logging.Logger;

public class OptimizedLogic {

    Connection con;
    ResultSet rs;

    public OptimizedLogic() {
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            con = DriverManager.getConnection("jdbc:mysql://localhost:3306/dbEmp?useSSL=false", "root", "");
        } catch (ClassNotFoundException ex) {
            System.out.println("Driver Not found");
        } catch (SQLException e) {
            System.out.println("Not connnect to the Database");
        }
    }

    public Collection<Employee> getAllEmployees() {
        Collection<Employee> emp = new ArrayList<>();
        try {
            Statement st = con.createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_UPDATABLE);

            ResultSet rs = st.executeQuery("select * from employee");

            while (rs.next()) {
                Employee e = new Employee();
                e.setEmpno(rs.getInt("empno"));
                e.setEname(rs.getString("ename"));
                e.setSalary(rs.getDouble("salary"));
                emp.add(e);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return emp;
    }

    public void insertEmployee(int empno, String name, double salary) {
        try {
            String insert = "insert into employee values(?,?,?)";
            PreparedStatement ps = con.prepareStatement(insert);
            ps.setInt(1, empno);
            ps.setString(2, name);
            ps.setDouble(3, salary);
            ps.executeUpdate();
        } catch (SQLException e) {
            Logger.getLogger(DataLogic.class.getName()).log(Level.SEVERE, null, e);
        }
    }

    public void removeEmployee(int empno) {
        try {
            String delete = "delete from employee where empno=?";
            PreparedStatement stmt = con.prepareStatement(delete);
            stmt.setInt(1, empno);
            stmt.executeUpdate();
        } catch (SQLException e) {
            Logger.getLogger(DataLogic.class.getName()).log(Level.SEVERE, null, e);
        }
    }
}

=> Above class is a Data Access Layer (DAL) or DAO (Data Access Object).It acts as a bridge between your Servlet (Java code) and the MySQL database.
=> Instead of writing SQL directly inside your servlet, you put all database logic here → making code cleaner, reusable, and easier to maintain.

- **Employee.java** :--



        


 



