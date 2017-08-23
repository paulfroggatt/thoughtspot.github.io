---
title: [ Use the JDBC Driver]
tags: [formatting]
keywords: tbd
last_updated: tbd
summary: "How to configure the JDBC drivr "
sidebar: mydoc_sidebar
---
# Use the JDBC Driver

To use the JDBC driver, include the JDBC library in your path, and provide the connection information.

You need this information to configure the JDBC driver:

-   Driver name: com.simba.client.core.jdbc4.SCJDBC4Driver
-   Server IP address: The ThoughtSpot appliance URL or IP address. The IP address can be found by going to http://<server-ip\>:2201/status/service?name=simba\_server
-   Simba port: The simba port, which is 12345 by default.
-   Database name: The ThoughtSpot Database name to connect to.
-   Database username: The name of a ThoughtSpot user with administrator permissions.

    **Attention:** This is not the machine login username.

-   Database password: The ThoughtSpot user password.

    **Attention:** This is not the machine login password.


To obtain and install the JDBC Driver:

1.   Log in to the local machine where you want to install the JDBC driver.
2.   To obtain the JDBC driver:
    -   Click [**Here**](https://help.thoughtspot.com/help_center/3.5/Downloads) to download the JDBC driver.
    -   Click **JDBC Driver** to download the file `ThoughtSpot_jdbc_<version>.zip`.
3.   Move the driver to the desired directory on your local machine.
4.   Add the JDBC driver to your Java class path on the local machine.
5.   Now write your Java application code. Using JDBC with ThoughtSpot is the same as using any other JDBC driver with any other database. You need to provide the connection information, create a connection, execute statements, and close the connection.

    Specify each of the nodes in the cluster in the connection string, as shown. This enables high availability for JDBC connections. To find out the nodes in the cluster, you can run the command `tscli node ls` from the Linux shell on the ThoughtSpot instance.

    The format for the connection is:

    ```
    jdbc:simba://<node1>:12345,<node2>:12345,<node3>:12345[,…];
               LoginTimeout=<seconds>;DATABASE=<db>;SCHEMA=<schema>
    ```

    For example:

    ```
    jdbc:simba://192.168.2.248:12345,192.168.2.249:12345,192.168.2.247:12345;
               LoginTimeout=5;DATABASE=test;SCHEMA=falcon_default_schema
    ```

    **Note:** The `DATABASE` and `SCHEMA` parameters need to be in all caps.

    **Note:** For the simba JDBC driver to work with Spark, the `DATABASE` and `SCHEMA` must be specified in the URL. They cannot be specified as a name/value pair as a map or property. For example:

    ```
    val tssqldf1 = sparkSession.read.format("jdbc").options(Map("url" ->
    "jdbc:simba://10.84.78.181:12345;DATABASE=movieratings;SCHEMA=falcon_default_schema", "driver" ->
    "com.simba.client.core.jdbc4.SCJDBC4Driver", "dbtable" -> "Movies", "user" ->
    "tsadmin", "password" -> "admin")).load()
    ```


This InsertData.java example shows how to use ThoughtSpot with JDBC. This is an example of a reference JDBC application:

```
import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class InsertData {

  // JDBC class to use.
  private static final String DB_DRIVER = "com.simba.client.core.jdbc4.SCJDBC4Driver";
  // jdbc_example should be an existing database.

  private static final String DB_CONNECTION = "jdbc:simba://192.168.2.129:12345;
     192.168.2.249:12345,192.168.2.247:12345;
     LoginTimeout=5;DATABASE=jdbc_example";SCHEMA=falcon_default_schema

  private static final String TABLE_NAME = "jdbc_example";
  private static final String DB_USER = "<username>";
  private static final String DB_PASSWORD = "<password>";

  // Assuming everything in local directory use:
  //   java -cp .:thoughtspot_jdbc4.jar InsertData
  public static void main(String[] argv) {

    try {
      insertRecordsIntoTable();
    }
    catch (SQLException e) {
      System.out.println(e.getMessage());
    }
  }

  /**
   * Insert some records using batch updates.
   *  Assumes a table exists:  CREATE TABLE "jdbc_example" ( "text" varchar(10) );
   */
  private static void insertRecordsIntoTable() throws SQLException {

    System.out.println("Inserting records.");
    Connection dbConnection = getDBConnection();
    PreparedStatement preparedStatement = null;
    String insertTableSQL = "INSERT INTO falcon_default_schema.jdbc_example (text) VALUES (?)";

    try {
      preparedStatement = dbConnection.prepareStatement(insertTableSQL);

      // Create multiple statements and add to a batch update.
      for (int cnt = 1; cnt <= 10; cnt++) {
        preparedStatement.setString(1, "some string " + cnt);
        preparedStatement.addBatch();
        System.out.println("Record " + cnt + " was added to the batch!");
      }
      preparedStatement.executeBatch();  // For large numbers of records, recommend doing sets of executeBatch commands.
      System.out.println("Records committed");

    }
    catch (SQLException sqle) {
      sqle.printStackTrace();
    }
    finally {

      if (preparedStatement != null) {
        preparedStatement.close();
      }
      if (dbConnection != null) {
        dbConnection.close();
      }
    }
  }

  /** Create a connection to the database. */
  private static Connection getDBConnection() {
    Connection dbConnection = null;
    try {
      Class.forName(DB_DRIVER);
    }
    catch (ClassNotFoundException e) {
      System.out.println(e.getMessage());
    }
    try {
      dbConnection = DriverManager.getConnection(DB_CONNECTION, DB_USER,DB_PASSWORD);
      return dbConnection;
    }
    catch (SQLException sqle) {
      System.out.println(sqle.getMessage());
    }

    return dbConnection;
  }

}

```

**Parent topic:** [About the JDBC Driver](../../data_integration/clients/about_jdbc_driver.html)