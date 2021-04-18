---
layout: post
title:  "Customized SOAP Web Service Development (Java + Apache Derby + Netbeans 9.0)"
date:   2020-03-23 23:40:12
author: Zizhun Guo
category: Projects
categories: jekyll update
visible: 1
---

**Stay Strong amid against Covid-19!!!**

<center>淡泊明志，宁静致远。</center>

---

<!-- MarkdownTOC -->

- [1. Tools and Installation](#1-tools-and-installation)
- [2. Project details](#2-project-details)
    - [2.1 Overall structure](#21-overall-structure)
    - [2.2 DB model Design](#22-db-model-design)
    - [2.3 Web Services Application](#23-web-services-application)
        - [Product Info Class](#product-info-class)
        - [2.3.1 Operation: getProductInfo](#231-operation-getproductinfo)
        - [2.3.2 Operation: placeOrder](#232-operation-placeorder)
    - [2.4 Client application](#24-client-application)
    - [2.5 Username Protocol - Client SOAP message handler for outbound messsage modification](#25-username-protocol---client-soap-message-handler-for-outbound-messsage-modification)
    - [2.6 Coordination Protocol - Server SOAP message handler for inbound messsage modification](#26-coordination-protocol---server-soap-message-handler-for-inbound-messsage-modification)
- [3. Results](#3-results)
- [4. Summary and Post-mortem](#4-summary-and-post-mortem)
    - [4.1 Problem Solving Plan in Timeline](#41-problem-solving-plan-in-timeline)
    - [4.2 Debugging](#42-debugging)

<!-- /MarkdownTOC -->



### 1. Tools and Installation
**Tools:**
- Netbeans IDE 11.2
- Maven 3.1 (plugin version)
- JAVA 1.8
- Glassfish 5.0x (local server)
- Apache DerbyDB


**Installation:**
1. Load the 2 projects (Server: PurchaseWSApplication; Client: PurchaseClient) using Netbeans IDE.
2. Select PurchaseWSApplication(Server) project, and navigate to the Services tab on the left.
3. Make sure you have GlassFish Server installed. Make sure you have Database 'Purchase' loaded in 'Java DB'.
*To load the DB:
- Navigate to 'Service' tab, and left click the 'Databases' legend, in the following legends, right click 'Java DB' to open the 'Properties'.
- Specify the 'Java DB installation' location.
- Specify the 'Database Location' at where you unzip the 'Purchase' folder (Database)
![DBinstallation](https://i.imgur.com/l5fYMr6.png)
4. Connect the Purchase DB. Connect jdbc:derby://localhost1527/Purchases [root on ROOT]
5. Deploy the project by right-clicking project on 'RUN'.
6. Switch to select PurchaseClient(Client) in the Projects tab.
7. Run the program by right-clicking the project on 'RUN'.


### 2. Project details
#### 2.1 Overall structure
 ![Overall](https://i.imgur.com/NqOKH3H.png)

1. Server side - web service application
2. Backend Database
3. Client side - java application
4. Username protocol - client SOAP message handler
5. Coordinator protocol - server SOAP message handler

The SOAP message sent from the client-side (outbound) when the client application invokes the web service from the server, would be wrapped by Username Protocol. As the SOAP message gets received by the server-side application, the Coordinator Protocol would decide if the service would be continually processed.
#### 2.2 DB model Design
**ER Model**

![ERModel](https://i.imgur.com/qY4YT5Q.png)

**Initial Views from the DB**
TABLE PRODUCTS:

![PRODUCTSviewbefore](https://i.imgur.com/n9pa0JJ.png)

TABLE ORDERS:

![ORDERSviewbefore](https://i.imgur.com/YRFIcD4.png)

TABLE ACCOUNTS:

![ACCOUNTSviewbefore](https://i.imgur.com/8pKXDxj.png)

#### 2.3 Web Services Application

**Brief Introduction**
This is the web service project that holds a web service named Purchase where it has two operations: **getProductInfo** - Query the list of products by providing the keyword of a product name in String, **placeOrder** - Place the order by providing the keywords of a product id, a number of ordering and a account id from the buyer.

**WSDL**: http://localhost:8080/PurchaseWSApplication/PurchaseWS?WSDL
(The link only works when the web services is currently deployed)
##### Product Info Class
This class works for storing the product information queried from the database.

```java
public class productInfo {
    int id;
    String name;
    String brand;
    int year;
    int price;
    int stock;
    
    public void setId(int id) { this.id = id;}
    public void setName(String name) { this.name = name;}
    public void setBrand(String brand) { this.brand = brand;}
    public void setYear(int year) {this.year = year;}
    public void setPrice(int price) {this.price = price;}
    public void setStock(int stock) {this.stock = stock;}
    public int getId() {return id;}
    public String getName() {return name;}
    public String getBrand() {return brand;}
    public int getYear() {return year;}
    public int getPrice() { return price;}
    public int getStock() {return stock;}

```

##### 2.3.1 Operation: getProductInfo
The following Java code shows the implementation of this operation.
The service would intialize a product instance first, then to query info from DerbyDB by using the provided keyword. The service will wind up returning the instance to the server application.
- **Get the product's information by providing the product name**

```SQL
SELECT * FROM ROOT.PRODUCTS WHERE PRODUCT_NAME = 'keyword'
```

```java
    /**
     * Web service operation
     */
    @WebMethod(operationName = "getProductInfo")
    public productInfo getProductInfo(@WebParam(name = "query") String keyword) {
        // instance initialization
        productInfo product = new productInfo();       
        try{   
            Statement s = Main.getConn().createStatement();
            System.out.println("WS_getProductInfo(): Connection succeeded !");
            // execute Query
            ResultSet rs = 
              s.executeQuery("SELECT * FROM ROOT.PRODUCTS WHERE PRODUCT_NAME = \'"+ keyword + "\'");
            System.out.println("WS_getProductInfo(): Doing Query");
            // sets members from results
            while (rs.next()) {
                product.setId(rs.getInt("PRODUCT_ID"));
                product.setName(rs.getString("PRODUCT_NAME"));
                product.setBrand(rs.getString("BRAND_NAME"));
                product.setYear(rs.getInt("MODEL_YEAR"));
                product.setPrice(rs.getInt("LIST_PRICE"));
                product.setStock(rs.getInt("STOCK"));
            }
        }
        catch(Exception e){}
        return product;
    }

```

##### 2.3.2 Operation: placeOrder
The following Java code shows the implementation of this operation.
The operation conducts 6 queries:
- **Get the product's price by providing the product ID**
```SQL
SELECT LIST_PRICE FROM ROOT.PRODUCTS WHERE PRODUCT_ID = product_ID
```
- **Get the stock number of the products by provding the product ID**
```SQL
SELECT STOCK FROM ROOT.PRODUCTS WHERE PRODUCT_ID = product_ID
```
- **Get the account's credit by providing the customer ID**
```SQL
SELECT ACCOUNT_CREDIT FROM ROOT.ACCOUNTS WHERE CUSTOMER_ID = custmer_ID
```
- **Update the stock value by having it deducted the request number**
```SQL
UPDATE ROOT.PRODUCTS SET STOCK = (number_in_stock- number_product) 
WHERE PRODUCT_ID = product_ID
```
- **Update the credit value by having it deducted the total cost of the order**
```SQL
UPDATE ROOT.ACCOUNTS SET ACCOUNT_CREDIT = (user_credit - price * number_product) 
WHERE CUSTOMER_ID = custmer_ID
```
- **Insert a new record indicating a new transaction**
```SQL
INSERT INTO ROOT.ORDERS VALUES (10, custmer_ID, product_ID, number_product)
```

```java
    /**
     * Web service operation
     */
    @WebMethod(operationName = "placeOrder")
    public String getPlaceOrder(@WebParam(name = "productID") int product_ID, 
                                @WebParam(name = "numberOfProduct") int number_product, 
                                @WebParam(name = "customerID") int custmer_ID) {
        productInfo product = new productInfo();
        
        int price = 0;
        int user_credit = 0;
        int number_in_stock = 0;
        
        try{   
            Statement s = Main.getConn().createStatement();
            ResultSet rs = s.executeQuery("SELECT LIST_PRICE FROM ROOT.PRODUCTS WHERE PRODUCT_ID = "+ product_ID);
            while (rs.next()) {
                price = rs.getInt("LIST_PRICE");
            }
            
            rs = s.executeQuery("SELECT STOCK FROM ROOT.PRODUCTS WHERE PRODUCT_ID = "+ product_ID);
            while (rs.next()) {
                number_in_stock = rs.getInt("STOCK");
            }
            
            rs = s.executeQuery("SELECT ACCOUNT_CREDIT FROM ROOT.ACCOUNTS WHERE CUSTOMER_ID = "+ custmer_ID);
            while (rs.next()) {
                user_credit = rs.getInt("ACCOUNT_CREDIT");
            }
                      
        }
        catch(Exception e){}
        
        if(price * number_product > user_credit 
          && number_in_stock < number_product) return "Order placement fails; Insufficient credits!";
        else{
            try {
                System.out.println("WS_getPlaceOrder(): Start to place order! ");
                Statement s2 = Main.getConn().createStatement();
                
                // update the stock number
                s2.executeUpdate("UPDATE ROOT.PRODUCTS SET STOCK = " 
                  + (number_in_stock- number_product) 
                  + " WHERE PRODUCT_ID = "+ product_ID);
                System.out.println("WS_getPlaceOrder(): Update stock finished!");
                
                // deduct the cost                 
                s2.executeUpdate("UPDATE ROOT.ACCOUNTS SET ACCOUNT_CREDIT = "+ (user_credit - price * number_product) 
                  + "WHERE CUSTOMER_ID = "+ custmer_ID);
                System.out.println("WS_getPlaceOrder(): Updated credit finished! ");
                
                // insert record in order 
                s2.executeUpdate("INSERT INTO ROOT.ORDERS VALUES (" + "10" + "," 
                  + custmer_ID + "," 
                  + product_ID + "," 
                  + number_product + ")");
                System.out.println("WS_getPlaceOrder(): Inserted new record to the order table!");
               
            } catch (Exception e) {
            }
        }
        return "WS: Successful placed the order!";
    }
```

#### 2.4 Client application

**Brief Introduction**
This is the user application that performs two rounds of web service invocation. In the first round, the app would attend to place an order directly. In the second round, the app attends to query the product info first and then use the info to place an order.

The following Java codes show the two-round performances. The user whose ID is 10086 tries to purchase 10 Macbook Pro 15 laptops in one single order. (The laptop's product ID is 101)

```java
    public static void main(String args[]){
        // Round 1
        placeOrder(101, 10, 10086);

        // Round 2
        String keyword = "Macbook Pro 15";
        productInfo = getProduct(keyword);
        placeOrder(productInfo.getId(), 10, 10086);
}
```

The client application sets up the Purchase Web Service by searching through WSDL file. The method is from last project Web Service project.

#### 2.5 Username Protocol - Client SOAP message handler for outbound messsage modification

The following codes show the implementation of adding the header element to a SOAP message through customization in the message handler.
```java
@Override
public boolean handleMessage(SOAPMessageContext messageContext) {
    Boolean outboundProperty = (Boolean) messageContext.get(MessageContext.MESSAGE_OUTBOUND_PROPERTY);
    if (outboundProperty) {
    //if the message is outgoing one, i.e., the request to a web service, we add account information into the header
        SOAPMessage message = messageContext.getMessage();
        try {
            SOAPEnvelope envelope = messageContext.getMessage().getSOAPPart().getEnvelope();
            SOAPHeader header=envelope.getHeader();
            if (header==null){
                header = envelope.addHeader();
            }
            // adding the username element as the child element to the header
            // when sent out the message to server
            SOAPElement usernameToken=header.addChildElement("UsernameToken","wsse", "http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd");
            SOAPElement username=usernameToken.addChildElement("Username","wsse");
            username.addTextNode("splashurpants");

        } catch (Exception e) {
        e.printStackTrace();
        }
    } else { //if the message is an incoming one, i.e., the response from the service, then we print out the whole message
        try {
            SOAPMessage message = messageContext.getMessage();
            System.out.println("");
    } catch (Exception ex) {
    ex.printStackTrace();
    }
    }
    return outboundProperty;
}
```

#### 2.6 Coordination Protocol - Server SOAP message handler for inbound messsage modification
The following codes show the implementation of extracting the header element in the type of usernameType and invoking operation name from the SOAP message. The codes also enforce the operation invoking orders to a specific user.

**How it works?**
The web services application maintains a HashSet that stores the usernames that have once queried the product info (the first round), of which the purpose is hence to enforce the coordination protocol. If a user who has never invoke the getProductInfo service, its username will not be added into the HashSet so that the server will not permit the user to process the placeOrder service by returning the false boolean value in message handling method in message handler and therefore a failed String message will indicate the failure of invocation. However, if a user has ever invoked the service to get product info, the server program will allow its SOAP message to forth process.

```java
    @Override
    public boolean handleMessage(SOAPMessageContext messageContext) {
        // get the HashSet
        HashSet<String> table = Main.getCoordinatorSet();
        System.out.println("Server side");
        String outProperty = SOAPMessageContext.MESSAGE_OUTBOUND_PROPERTY;
        boolean outgoing = (Boolean)messageContext.get(outProperty);
        SOAPMessage msg = messageContext.getMessage();
        String username="";
        try {
            if (outgoing){
                System.err.println("");
            } 
            else{  

              // Get username @username
                SOAPHeader header = msg.getSOAPHeader();
                Iterator it = header.examineAllHeaderElements();
                while(it.hasNext()){
                    SOAPHeaderElement e=(SOAPHeaderElement) it.next();
                    NodeList nl=e.getElementsByTagName("wsse:Username");
                    for (int i=0;i<nl.getLength();i++){
                        username+=nl.item(i).getTextContent();
                    }
                }
                System.out.println("Server: Ongoing username: " + username);
                
                // get the operation name @opNameString
                SOAPEnvelope envelope = messageContext.getMessage().getSOAPPart().getEnvelope();   
                SOAPBody body = envelope.getBody();
                if (body == null){
                    System.err.println("Server: No body in message!");
                    return false;
                }
                Iterator it2 = body.getChildElements();
                String opNameString = "";
                while(it2.hasNext()){
                    SOAPBodyElement e=(SOAPBodyElement) it2.next();
                    opNameString = e.getLocalName();
                    System.out.println("Server: Ongoing operation name: " + opNameString);
                }

                //
                // Core Business Logic
                //
                // old user
                if (table.contains(username)){
                    // registered user that had called product info query
                    if (opNameString.equals("getProductInfo")){
                        System.out.println("Server: Succeeded to query the productInfo");
                    }
                    // place the order after getting the product info 
                    if (opNameString.equals("placeOrder")) {
                        System.out.println("Server: Succeeded to place the order");
                    }
                    return true;
                }
                // new user
                else{
                    // not yet invoke the getProductInfo
                    System.out.println("Server: Not yet registered");
                    if (opNameString.equals("getProductInfo")){
                        table.add(username);
                        System.out.println("Server: Registration done");
                        System.out.println("Server: Succeeded to query the productInfo");
                        return true;
                    } 
                    // new user attends to directly place the order
                    if (opNameString.equals("placeOrder")) {
                        System.out.println("Server: Failed. Attend to place order without the registeration.");
                        return false;
                    }
                }
            }
        }
        catch(SOAPException e){
            System.out.println("SOAP IO Error!!!!");
            throw new RuntimeException(e);
        }
        return false;
    }

```

### 3. Results

**Server side Output**

![ServerResult](https://i.imgur.com/lJMlKcL.png)

**Client side Output**

![ClientResult](https://i.imgur.com/fphuvvd.png)

**Views from DB**
TABLE PRODUCTS:

![PRODUCTSviewafter](https://i.imgur.com/dPEuSgC.png)

TABLE ORDERS:

![ORDERSviewafter](https://i.imgur.com/wQkKUEn.png)

TABLE ACCOUNTS:

![ACCOUNTSviewafter](https://i.imgur.com/U54BtZY.png)

### 4. Summary and Post-mortem
#### 4.1 Problem Solving Plan in Timeline
Split the goal into individual tasks in timeline:
1. Database prepration:
    - Created the ER model and implemented into tables
    - Assigned the initial records for query testing
    - (Tricky thing) Use MySQL benchmark for fast table and SQL statements test and migrate to DerbyDB
2. Linked the DB to web service program
    - Performed queries towards the task requirement in Main method
    - Performed queries in operations
3. Created the client programe that consumes the web service
    - Performed queries by invoking the operations in web service
4. Message Handler preparation
    - Printed out the SOAP message from client for java SOAP class testing
    - Added the username to the header element in outbound SOAP message from Client side
    - Extracted the username and operation name from the inbound SOAP message from Server side

#### 4.2 Debugging

- annotation @override: it should be placed above the message handle method's signature so that to make it works.
- After configure the protocol to the web service, the generated XML file should be placed in the same directory of the .class file in target folder instead of the place where it was first generated in src folder.
- When do query, the the Value in Where clause condition should be wrapped by a pair of ' sign.

**useful resources and references**

Oracle message handling: 

https://docs.oracle.com/cd/E13222_01/wls/docs103/webserv_adv/handlers.html

Noctrl.edu Message handling

http://gcmuganda.faculty.noctrl.edu/classes/Spring12/615/SOAPHeaderExample.txt

Message handling example:

https://www.itread01.com/p/847210.html
https://www.codota.com/code/java/classes/javax.xml.soap.SOAPHeaderElement
https://www.codota.com/code/java/methods/javax.xml.soap.SOAPElement/addChildElement
https://www.codota.com/code/java/methods/javax.xml.soap.SOAPHeader/addChildElement
https://www.programcreek.com/java-api-examples/?class=javax.xml.soap.SOAPEnvelope&method=getHeader

UsernameType XML schema:

https://www.w3.org/2003/05/soap-envelope/

Link JavaDB:

https://www.youtube.com/watch?v=NDT9ByL7jA0

Processing SQL Statements with JDBC:

https://docs.oracle.com/javase/tutorial/jdbc/basics/processingsqlstatements.html

JavaDB SQL Official handbook:

https://db.apache.org/derby/docs/10.8/ref/

Microservices Consistence Maintance:

https://www.itzhai.com/trade-transaction-of-data-consistency.html

https://www.cnblogs.com/study-everyday/p/7605619.html

Derby development:

https://www.ibm.com/developerworks/cn/opensource/os-ad-trifecta7/index.html

Glassfish not seeing my handler-chain.xml:

https://stackoverflow.com/questions/14539296/glassfish-not-seeing-my-handler-chain-xml


---
**Zizhun Guo@All rights reserved**
