# PostGUI

**A React web application to query and share any PostgreSQL database.**

## Current Status ✅ ✅
This app is currently being updated in the `v2` branch. This new version is written in Typescript, and follows a set of modern React development standards.


## Table of Contents
1. [ Introduction ](#introduction)
1. [ Features ](#features)
1. [ Use Case Scenarios ](#usecases)
1. [ Installation ](#installation)
1. [ Security and Deployment Checklist ](#checklist)
1. [ Contact ](#contact)



<a name="introduction"></a>
## Introduction

PostGUI is a React web application that serves as a front-end to any PostgreSQL database using the PostgREST automatic API tool. PostGUI can automatically adapt any PostgreSQL database to provide an overview of the schema, a visual query builder to execute a SQL query, and a login system to control access to the data. The entire framework can run on a personal computer, or on a web server (for data sharing over the internet).

This is a Masters project intended to be a web-based framework for data sharing and making data more accessible to non-computer programmers. Originally intended for biology community, the generic approach towards data has made this framework into a simple alternative to creating a new custom web application each time data set(s) need to be shared. These web applications are usually non-adaptive and require significant changes if the data source changes.

A PostgreSQL database is a pre-requisite to setting up this framework - specific tips on database schema design are provided in the Installation section. Configuration of PostgREST and PostGUI can be done in a matter of minutes. The PostGUI configuration file (at `/data/config.json`) in this repository demonstrates how to customize the front-end for a [sample PostgreSQL database](http://www.postgresqltutorial.com/postgresql-sample-database/). PostgREST configuration is provided, along with some suggestions on PostgreSQL database schema design for optimal experience.


<p align="center">
  <img src="/docs/images/full-ui.png">
</p>


To get help with confirming use cases/feasibility or anything else, please make a new issue on this GitHub repository. This data sharing framework relies on popular open-source works such as: [Material UI](https://material.io/), [PostgREST](http://postgrest.org), [JS Query Builder](https://querybuilder.js.org/), etc.


<a name="features"></a>
## Features

#### Data Compatibility

PostGUI is designed to be database agnostic by automatically adapting to the client's database schema. This framework can be setup to query and share a biology, finance, or a sports statistics PostgreSQL database. In short, any tabular data set can be accessed and shared using this framework. If the data set(s) in question can be represented as tables, then the data set(s) is compatible with this framework. Complex data types such as images, videos, or sound clips are currently untested and unsupported.

The client must organize the data to be shared in a PostgreSQL database running on a local computer (only data access, no data sharing over the internet) or a web  server (data access, and data sharing over the internet). Certain features of PostGUI require specific database schema design. For example, to allow users to edit the database tables, the database tables must have a defined primary key (PK) attribute. However, if a PK is not defined for a table, PostGUI will gracefully disable the edit feature for the table.


#### Database Picker

<p align="center">
  <img src="/docs/images/db-picker.png">
</p>

The database picker allows multiple PostgreSQL databases to be shared from a single instance of PostGUI. The database name is customizable using the `/data/config.json` file.

#### Database Schema

<p align="center">
  <img src="/docs/images/schema.png">
</p>

The database schema (tables, columns, and foreign keys) is shown in the left panel of the PostGUI user interface (UI). The tables and columns can be renamed for a more user-friendly table and column name. Foreign Keys also are shown with “Referenced by” and “FK to” labels.

#### Search Feature

<p align="center">
  <img src="/docs/images/search.png">
</p>

Search feature can be used to find a table or column quickly. To search for a specific table vs. a column, each search term can be tagged with ‘[table]’ or ‘[column]’.

#### Query Builder and Query Options

<p align="center">
  <img src="/docs/images/query.png">
</p>

Integration of the [JS Query Builder](https://querybuilder.js.org/) in this web application makes it easily usable by users unfamiliar with SQL programming language. Query options can be used to fine tune the data being extracted from the database, and to ensure the full result is being shown (exact row count feature).

#### Data Table

<p align="center">
  <img src="/docs/images/table.png">
</p>

The query result component features a high-performance data table ([React Table](https://github.com/tannerlinsley/react-table)) that can sort columns by their values and edit individual cell values (if edit feature is enabled).

#### Download Data

<p align="center">
  <img src="/docs/images/downloads.png">
</p>

The Downloads card features options to download the currently loaded data in CSV, JSON, and XML formats. In addition, it allows for column values to be copied as comma-separated values – this may be used in lieu of a join-table VIEW.

#### Authentication System

<p align="center">
  <img src="/docs/images/auth.png">
</p>

Basic authentication system gets you started with the “database-first” approach to secure authentication system. Three basic users are available by simply executing the Authentication SQL script – read, edit, and admin. The Installation section provides further instructions on how to set up the authentication system. Further comments in the Security Checklist section.

#### Edit Contents

<p align="center">
  <img src="/docs/images/edit.png">
</p>

Edit Content feature allows authenticated users to change the table contents if a primary key is defined for the table. A record of database changes is also kept in a separate table.



<a name="usecases"></a>
## Use Case Scenarios

This project was built to solve three main scenarios:

#### Accessing a database (no data sharing)

Set up the PostgreSQL database, PostgREST API tool, and the PostGUI web application on a local computer for database access without any data sharing over the internet.

#### Sharing a database (read-only data sharing mode)

Set up the PostgreSQL database, PostgREST API tool, and the PostGUI web application on a web server for data sharing over the internet.

#### Sharing a database with edit access (read and write access for authenticated users)

Set up the PostgreSQL database, PostgREST API tool, and the PostGUI web application on a web server. In addition, enable the edit feature by ensuring each table has a primary key defined, presence of the primary_keys function in the PostgreSQL database, and enabling the feature in the configuration file of PostGUI.



<a name="installation"></a>
## Installation

### Pre-requisite

This installation guide will walk you through setting up a production-like version of the framework with all features enabled on your personal computer. After testing on your personal computer, deploying the three components of the framework on a web server will allow you to share the data over the internet.


### Get Software
1. Install [Node.js](https://nodejs.org/en/).
1. Install [PostgreSQL](https://www.postgresql.org/download/).
1. Download [PostgREST](https://github.com/begriffs/postgrest/releases/tag/v0.4.4.0).

### PostgreSQL Setup

#### Get Started with a Sample Database

1. Download the DVD Rentals database file from [PostgreSQL Tutorial website](http://www.postgresqltutorial.com/postgresql-sample-database/).
1. Unzip the file to retrieve the .tar file.
1. Follow these instructions using a command line tool: http://www.postgresqltutorial.com/load-postgresql-sample-database/.


#### Get Started with Your Data

This framework is compatible with any data set(s) that can be represented as a data table. The data set(s) is required to be inserted into the PostgreSQL database system to ensure data integrity and consistent data access. Having a well designed PostgreSQL database is important because PostGUI does not enforce any hard requirements on the database schema design. However, to take full advantage of all PostGUI features, do consider the following when designing your PostgreSQL database schema:

1. Ensure necessary column and table attributes are present:
   1. Foreign Key: required only to show two-way relationships in the left panel in addition to the foreign_key function from the `/scripts/` directory.
   1. Primary Key: required only to enable the Edit Feature in addition to the primary_key function from the `/scripts` directory.
   1. NOT NULL, CHECK, and data types should be carefully considered because they serve as back-end sanitation of input coming from the Edit Feature.
1. Create necessary VIEWs: To prevent Denial or Service attacks, the PostgREST framework does not allow for arbitrary table joins. Therefore, the client must predefine join tables when queries spanning multiple tables will be executed frequently. 
1. Create necessary indexes: important feature of large tables in order to speed up the query response time. When the CONTAINS operator will be used frequently with a string column, a `pg_trgm` idnex would be important to improve performance.


### PostgREST Setup:
1. Download and extract the PostgREST binary file into a folder from: https://github.com/begriffs/postgrest/releases/tag/v0.4.4.0.
1. Create a configuration file named `db.conf` in the same folder as the PostgREST binary.
1. `db.conf` file should contain the following:
      ```
           db-uri = "postgres://DB_USER:DB_USER_PASS@localhost:5432/DB_NAME"
           db-schema = "public"
           db-anon-role = "DB_USER"
           db-pool = 10
           server-host = "*4"
           server-port = 3001
      ```
   1. This configuration file can be further customized as described in the original docs: https://postgrest.com/en/v4.4/install.html#configuration.
1. Be sure to replace DB_NAME, DB_USER, DB_USER_PASS in the above configuration file.
1. Using a command line tool, navigate to the folder containing the PostgREST binary and config file.
1. Run the PostgREST tool: `./postgrest.exe ./db.conf` (remove the .exe extension if not on Windows operating system).
1. You should see a "Connection successful" message.

### PostGUI Setup:
1. Open a new command line terminal.
1. Clone this repository: `git clone git@github.com:priyank-purohit/PostGUI.git`
1. Navigate into the cloned repository: `cd PostGUI`.
1. Install node dependencies by executing: `npm install`.
1. Create a basic configuration file for the front-end web application:
   1. Configuration file location: `/src/data/config.json`.
   1. Change the `url` if any changes were made to the PostgREST port, or if the PostgREST instance is running on a web server.
   1. Give an appropriate title to the database.
1. Using the command line tool, execute `npm start` to run the web application.
1. Improve configuration file for better user experience according to the Config File Format section of this guide.



<a name="checklist"></a>
## Security and Deployment Checklist

Before deploying the framework on a web server for, consider the following items to improve security.

1. Harden PostgREST for improved performance and security.
   1. Go through the PostgREST docs.
   1. Go through https://postgrest.org/en/v5.0/admin.html section.
1. Enable HTTPS for PostGUI
   1. Install SSL/TLS certificate on the web server. This should automatically enable HTTPS for the web application.
   1. Force the web server to handle the PostgREST traffic as HTTPS by setting up a reverse proxy on the port used by PostgREST.
      1. A sample NGINX configuration file is provided in this repository at `/scripts/Sample NGINX Config File.txt`.
1. Authentication System
   1. Be sure to enable HTTPS first.
   1. Execute the scripts in the `/scripts/` directory to create an authentication system schema in the database.
   1. Insert new emails and passwords for more users.
   1. Fine tune authentication control according to the Authentication System section of this guide.


<!-- ## Authentication System
<coming soon>

## Config file format
<coming soon> -->



<a name="contact"></a>
## Contact

_Priyank K. Purohit, Dr. David Guttman, Dr. Nicholas Provart_

_University of Toronto_
