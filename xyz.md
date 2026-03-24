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
- **Purpose**: Test environment and development
- **Database**: Test database (replica of live)
- **Cron Jobs**: Full
- **Backup Role**: Serves as backup for live environment
- **Environment**: Development/Testing
- **Last Updated**: [Date will be updated when changed]

### Live Server (Server22) {#live-server}
- **IP Address**: 198.120.130.22
- **Purpose**: Production environment
- **Database**: Live database
- **Traffic**: Serves all production users
- **Environment**: Production
- **Last Updated**: [Date will be updated when changed]

### Replica Server (Server 194) {#Replica-server}
- **IP Address**: 
- **Purpose**: Used to execute cron jobs, primarily handling lightweight jobs such as interest updates etc.
- **Database**: Replica database
- **Type**: SQL Server Replica
- **Sync**: Real-time replication from Live Server
- **Last Updated**: [Date will be updated when changed]

## Database Configuration

### Primary Database (SQL Server) {#primary-database-sql-server}
- **Type**: Microsoft SQL Server
- **Purpose**: Primary data storage for all user and system data
- **Location**: Both Test Server and Live Server
- **Backup**: Automated daily backups
- **Replication**: Test DB is replica of Live DB
- **Last Updated**: [Date will be updated when changed]
