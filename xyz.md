# Anuroop Documentation

## What Technology is Used Here?

The project is an ASP.NET Web Forms application built on the .NET Framework 4.5 using C# as the primary programming language.

In addition to the core framework, the project integrates a wide array of modern third-party technologies:
- **Database: Microsoft SQL Server.** The web.config handles connection strings.
- **Search Engine: Algolia** is heavily used for fast profile caching and searching.
- **Payment Gateway: Razorpay** is used for handling transactions (e.g., event packages, vendor subscriptions).
- **Cloud Storage & CDN: Cloudinary** is used for image/document management, and a CDN handles static assets.
- **Real-time / NoSQL Databases: Firebase** and **Google Firestore** are actively used for specific data synchronization and counting (e.g., tracking profile views, interactions).
- **Messaging & Communication: Twilio** (for SMS), WhatsApp API Integrations, **SendGrid** (for emails), and **Intercom** (for customer support/chats).
- **Background Jobs: Hangfire** is initialized in Global.asax to handle scheduled background tasks.

## Server Configuration

### Test Server (Server106) {#test-server}
- **IP Address**: 198.120.130.106
- **Purpose**: Test environment and development, Serves as backup for live environment
- **Database**: Test database (replica of live)
- **Cron Jobs**: Full Member Update
- **Application Used**: Task Schedular
- **Environment**: Development/Testing
- **Last Updated**: [Date will be updated when changed]

### Live Server (Server22) {#live-server}
- **IP Address**: 198.120.130.22
- **Purpose**: Production environment
- **Database**: Live database
- **Cron Jobs**: Used to execute cron jobs, primarily handling lightweight jobs such as interest updates etc.
- **Application Used**: Z-Cron
- **Traffic**: Serves all production users
- **Environment**: Production
- **Last Updated**: [Date will be updated when changed]

### Replica Server (Server 194) {#Replica-server}
- **IP Address**: 
- **Purpose**: Contains only the replica database, which is used by cron jobs to update Firestore data.
- **Database**: Replica database
- **Sync**: Real-time replication from Live Server
- **Last Updated**: [Date will be updated when changed]

## Database Configuration

### Primary Database (SQL Server) {#primary-database-sql-server}
- **Type**: Microsoft SQL Server
- **Purpose**: Primary data storage for all user and system data
- **Location**: All three servers
- **Backup**: Automated daily backups
- **Replication**: Test DB is replica of Live DB
- **Last Updated**: [Date will be updated when changed]

## System Architecture
### Overview

The system is distributed across three servers: **22 (Production), 106 (Development), and 194 (Replication Database Server)**. Each server has a specific role in handling application logic, data management, and background processing.

### 1. Server 22 – Production (Source of Truth)

Server 22 is the **primary production server** and acts as the **single source of truth** for all data.

### Components Hosted
- SQL Server Database
- API Project
- Web Project
- Cron Project

### Behavior
The **API** and **Web** applications interact directly with the local SQL database on this server.
The **Cron Project**, however, does not use the local database. Instead, it connects to the database hosted on Server 194 for processing tasks.

### 2. Server 106 – Development

Server 106 is used for **development and testing** purposes.

### Components Hosted
- SQL Server Database
- API Project
- Web Project
- Cron Project

### Behavior
- The API and Web applications use the local database on this server.
- The Cron Project connects to the Server 194 database, similar to the production setup.

### Current Cron Usage
- At present, only one cron job is active:
    - **FullMemberUpdate** → Responsible for replicating frequently used data to Firestore.
  
### Monthly Replication Process
On the 15th of every month, the entire data and database structure from **Server 22** is replicated to **Server 106**, including:
- Data
- Stored Procedures
- Functions
- Scripts
  
### Drawback of Monthly Replication
- Any new or modified database objects (e.g., stored procedures, functions, scripts) created on Server 106 will be **overwritten** during replication if they do not exist on Server 22.
- This can lead to loss of development work if changes are not synchronized with the production server.

### 3. Server 194 – Replication Database Server

Server **194** is dedicated solely to **database replication**.

### Components Hosted
- SQL Server Database (Replica of Server 22)
  
### Behavior
- This database is a real-time replica of the database on Server 22.
- Any changes made on Server 22 are instantly replicated to Server 194.

### Role in System
- All cron jobs (from both Server 22 and Server 106) use this database instead of directly accessing the production database.
- The cron jobs process data from this replicated database and update Firestore accordingly.

### Reliability
- In case of any issues, full database backups are available to restore data and ensure system stability.

## Data Flow

1. All primary data operations occur on **Server 22 (Production)**
2. Data is replicated to **Server 194 (real-time)**
3. **Cron jobs** (from Server 22 and 106) read data from **Server 194**
4. Processed data is pushed to **Firestore**
5. Monthly replication syncs **Server 22 → Server 106**

## Advantages

- Production database is protected from heavy cron load
- Real-time replication ensures near-live data processing
- Better scalability  
- Reliable backup support  

## Challenges

- Risk of losing development changes during monthly sync  
- Requires discipline to sync changes to production  
- Dependency on synchronization between development and production environments

## Architecture Diagram




## System Actors

### Internal Actors
- **Owner**: System owner and administrator
- **Developer**: All development team members
- **Control Team**: Testing team members (also known as Testing Team)

### External Actors
- **Members**: Registered users with full platform access
- **Visitors**: Non-registered users with limited access
