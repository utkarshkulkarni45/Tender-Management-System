# Tender Management System

A web-based application for managing online tenders, bids, and vendor management. This system allows vendors to register, create tenders, place bids, and manage tender status in a centralized platform.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Database Setup](#database-setup)
- [Running the Project](#running-the-project)
- [Project Structure](#project-structure)
- [Key Features](#key-features)
- [Technology Stack](#technology-stack)
- [Troubleshooting](#troubleshooting)

---

## 📖 Project Overview

The **Tender Management System** is designed to streamline the process of publishing, bidding, and awarding tenders in an online environment. It provides:

- **Vendor Management**: User registration, profile updates, and password management
- **Tender Management**: Create, view, and manage tenders with detailed descriptions
- **Bidding System**: Vendors can place bids on available tenders
- **Bid Management**: Accept or reject bids from vendors
- **Notice Board**: Publish announcements and notices
- **User Authentication**: Secure login and session management

---

## ✅ Prerequisites

Before setting up the project, ensure you have the following installed:

1. **Java Development Kit (JDK)**: Version 8 or higher
   - [Download JDK](https://www.oracle.com/java/technologies/downloads/)
2. **Maven**: Version 3.6 or higher
   - [Download Maven](https://maven.apache.org/download.cgi)
   - **Setup**: Add Maven to your system PATH
3. **MySQL Database**: Version 5.7 or higher
   - [Download MySQL](https://www.mysql.com/downloads/)
   - **Setup**: Ensure MySQL server is running
4. **Apache Tomcat**: Version 8.5 or higher (for WAR deployment)
   - [Download Tomcat](https://tomcat.apache.org/download-80.cgi)
5. **Git**: For version control
   - [Download Git](https://git-scm.com/downloads)

---

## 🔧 Installation & Setup

### Step 1: Clone the Repository

```bash
git clone https://github.com/utkarshkulkarni45/Tender-Management-System.git
cd Tender-Management-System
```

### Step 2: Verify Java and Maven Installation

```bash
java -version
mvn -version
```

Both commands should display version information without errors.

### Step 3: Build the Project

Navigate to the project directory and build using Maven:

```bash
cd tendermanagement
mvn clean compile
```

This will:

- Clean any previous builds
- Download all required dependencies from Maven Central Repository
- Compile all Java source files

---

## 🗄️ Database Setup

### Option 1: Using SQL Dump File (Recommended - Includes Sample Data)

1. **Navigate to DataBase folder**:

   ```bash
   cd DataBase
   ```

2. **Open MySQL Command Prompt**:

   ```bash
   mysql -u root -p
   ```

   (Replace `root` with your MySQL username if different)

3. **Import the SQL dump**:

   ```bash
   mysql -u root -p tender < tender.sql
   ```

   (Enter your MySQL password when prompted)

4. **Verify the import**:
   ```bash
   mysql -u root -p
   use tender;
   show tables;
   ```

### Option 2: Manual Table Creation (Empty Database)

If you prefer to create an empty database structure:

1. **Open MySQL Command Prompt**:

   ```bash
   mysql -u root -p
   ```

2. **Execute the following SQL commands**:

   ```sql
   CREATE DATABASE tender;
   COMMIT;
   USE tender;

   CREATE TABLE notice(
       id INT(3) NOT NULL AUTO_INCREMENT,
       title VARCHAR(35),
       info VARCHAR(300),
       PRIMARY KEY(id)
   );

   CREATE TABLE vendor(
       vid VARCHAR(15) PRIMARY KEY,
       password VARCHAR(20),
       vname VARCHAR(30),
       vmob VARCHAR(12),
       vemail VARCHAR(40),
       company VARCHAR(15),
       address VARCHAR(100)
   );

   CREATE TABLE tender(
       tid VARCHAR(15) PRIMARY KEY,
       tname VARCHAR(40),
       ttype VARCHAR(20),
       tprice INT,
       tdesc VARCHAR(300),
       tdeadline DATE,
       tloc VARCHAR(70)
   );

   CREATE TABLE bidder(
       bid VARCHAR(15) PRIMARY KEY,
       vid VARCHAR(15) REFERENCES vendor(vid),
       tid VARCHAR(15) REFERENCES tender(tid),
       bidamount INT,
       deadline DATE,
       status VARCHAR(10)
   );

   CREATE TABLE tenderstatus(
       tid VARCHAR(15) PRIMARY KEY REFERENCES tender(tid),
       bid VARCHAR(15) REFERENCES bidder(bid),
       status VARCHAR(15) NOT NULL,
       vid VARCHAR(15) REFERENCES vendor(vid)
   );

   COMMIT;
   ```

### Step 3: Update Database Configuration

Update the database connection settings in `DBUtil.java`:

- **Location**: `tendermanagement/src/com/hit/utility/DBUtil.java`
- **Modify**:
  - `DB_URL`: MySQL connection URL (default: `jdbc:mysql://localhost:3306/tender`)
  - `DB_USER`: MySQL username (default: `root`)
  - `DB_PASSWORD`: MySQL password

---

## ▶️ Running the Project

### Step 1: Build WAR File

From the `tendermanagement` directory:

```bash
mvn clean package
```

This generates `tendermanagement.war` in the `target/` folder.

### Step 2: Deploy to Tomcat

1. **Copy the WAR file**:

   ```bash
   copy target/tendermanagement.war <TOMCAT_HOME>/webapps/
   ```

   (Replace `<TOMCAT_HOME>` with your Tomcat installation directory)

2. **Start Tomcat**:
   - **Windows**: Run `<TOMCAT_HOME>/bin/startup.bat`
   - **Linux/Mac**: Run `<TOMCAT_HOME>/bin/startup.sh`

3. **Access the Application**:
   - Open browser and go to: `http://localhost:8080/tendermanagement`

### Step 3: Verify Setup

- You should see the login page
- Use sample credentials (if imported with SQL dump):
  - **Vendor ID**: `V20190725020951`
  - **Password**: `12345`

---

## 📂 Project Structure

```
Tender-Management-System/
├── DataBase/
│   ├── tender.sql                 # Complete database dump with sample data
│   ├── mysql_create_tables.md     # Manual table creation guide
│   └── how-to-import-sql-dump.md # Import instructions
├── tendermanagement/              # Main project folder
│   ├── src/
│   │   └── com/hit/
│   │       ├── beans/             # Data beans (Tender, Vendor, Bidder, etc.)
│   │       ├── dao/               # Data Access Objects (Database operations)
│   │       ├── srv/               # Servlets (Request handlers)
│   │       └── utility/           # Utility classes (DBUtil, IDUtil)
│   ├── WebContent/                # Web resources (JSP, CSS, JS, Images)
│   ├── pom.xml                    # Maven configuration
│   ├── .classpath                 # Classpath configuration
│   └── .project                   # Eclipse project file
└── README.md                      # This file
```

### Key Directories:

- **beans/**: Contains Java objects representing database tables
- **dao/**: Implements database operations (CRUD operations)
- **srv/**: Servlet classes that handle HTTP requests and business logic
- **utility/**: Helper classes for database connection and ID generation
- **WebContent/**: Frontend files (JSP pages, stylesheets, scripts)

---

## ✨ Key Features

| Feature                 | Description                                          |
| ----------------------- | ---------------------------------------------------- |
| **User Authentication** | Secure login with vendor credentials                 |
| **Vendor Registration** | Register new vendors with company details            |
| **Tender Creation**     | Create new tenders with specifications and deadlines |
| **Bid Management**      | Place, review, and manage bids on tenders            |
| **Profile Management**  | Update vendor information and change passwords       |
| **Notice Board**        | Post and view important announcements                |
| **Bid Status Tracking** | Monitor bid status (Pending, Accepted, Rejected)     |

---

## 🛠️ Technology Stack

| Component           | Technology               |
| ------------------- | ------------------------ |
| **Backend**         | Java 8                   |
| **Frontend**        | JSP (Java Server Pages)  |
| **Database**        | MySQL 5.7+               |
| **Build Tool**      | Apache Maven 3.6+        |
| **Web Server**      | Apache Tomcat 8.5+       |
| **Database Driver** | MySQL Connector/J 8.0.28 |
| **Protocol**        | HTTP/HTTPS               |

---

## 🐛 Troubleshooting

### Issue: Maven Build Fails - Dependency Not Found

**Solution**:

```bash
mvn clean install -U
```

The `-U` flag forces Maven to download the latest versions.

### Issue: Database Connection Error

**Check**:

1. MySQL server is running
2. Database `tender` exists
3. Username and password are correct in `DBUtil.java`
4. MySQL port (default: 3306) is accessible

**Verify**:

```bash
mysql -u root -p -e "USE tender; SHOW TABLES;"
```

### Issue: Application Not Accessible at localhost:8080

**Check**:

1. Tomcat is running (check logs in `<TOMCAT_HOME>/logs/`)
2. Port 8080 is not blocked by firewall
3. WAR file is in `webapps/` folder
4. Tomcat has started successfully

**Check Logs**:

```bash
tail -f <TOMCAT_HOME>/logs/catalina.out
```

### Issue: Login Fails with Correct Credentials

**Check**:

1. Database has vendor records
2. Database connection is working
3. Password stored in database matches

**Verify**:

```sql
SELECT * FROM vendor;
```

---

## 📝 Important Notes

- **Java Version**: Project uses Java 8 syntax. Ensure JDK 8+ is installed.
- **Database Port**: Default MySQL port is 3306. Update if using a different port.
- **Tomcat Port**: Default is 8080. Change in `<TOMCAT_HOME>/conf/server.xml` if needed.
- **Sample Data**: SQL dump includes sample tenders and vendors for testing.

---

## 📞 Support

For issues or questions, please refer to the project documentation in the `DataBase/` folder or review the servlet implementations in `tendermanagement/src/com/hit/srv/`.

---

## 📄 License

This project is provided as-is for educational and development purposes.

---

**Last Updated**: 2026-05-27
