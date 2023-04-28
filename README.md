# Setting up PostgreSQL

## Learning Goals

- Install the **PostgreSQL** database management system software.
- Use the **psql** tool to connect to a PostgreSQL server.
- Use the **pgAdmin** tool to connect to a PostgreSQL server.

## Introduction

**PostgreSQL** is a popular open source object-relational DBMS that uses the
SQL language to store and manage data. PostgreSQL provides two client
application tools to connect and interact with the database server:

- **psql** - a terminal-based front-end to PostgreSQL database server.
- **pgAdmin** - a desktop or web-based front-end to PostgreSQL database server.

In this lesson, we will download and install the PostgreSQL software. We will
then use the psql and pgAdmin tools to connect to the database server.

## Installing PostgreSQL

1. Download the appropriate PostgreSQL version 15.0 installer from
   [https://www.enterprisedb.com/downloads/postgres-postgresql-downloads](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads).
2. Step through the installation instructions, using the recommended default
   configuration.
   The port should be 5432, and all recommended components should be installed.

   ![select postgresql components](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/selectcomponents.png)
    - [Install PostgreSQL for Windows](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql/)
    - [Install PostgreSQL for macOS](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-macos//)
        - You do not need to follow the instructions to "Load the sample database".
    - [Install PostgreSQL for Linux](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-linux/)
        - You do not need to follow the instructions to "Load the sample database".

## Connect to PostgreSQL database server using **psql**

### Windows Instructions

1. Launch the **psql** tool.

   ![windows psql tool](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/psql.png)

2. Hit enter to accept default values for server, database, port, and username.
3. Enter the password for the **postgres** user.
4. At the prompt **postgres=#**, enter `SELECT version();`
5. Confirm the version is "PostgreSQL 15.0" (or whatever version you have
   installed).
6. At the prompt **postgres=#**, enter `exit` to quit the program and return to
   the command line prompt.

![psql windows session](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/windows_psql_session.jpg)

### macOS Instructions

1. Launch **psql** from the Launchpad (or enter the command `psql -U postgres`
   in a terminal window).

   ![mac psql tool](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/psql_mac.png)

2. Hit enter to accept default values for server, database, port, and username.
3. Enter the password for the **postgres** user.
4. At the prompt **postgres=#**, enter `SELECT version();`
5. Confirm the version is "PostgreSQL 15.0" (or whatever version you have
   installed).
6. At the prompt **postgres=#**, enter `exit` to quit the program (running in
   terminal window) or close the application window (launched from launchpad).

![psql mac session](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/mac_psql_session.png)

### Linux instructions

1. Scroll down to follow the instructions labeled
   [Connect to the PostgreSQL database server via psql](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-linux/).
2. At the prompt **postgres=#**, enter `SELECT version();`
3. Confirm the version is "PostgreSQL 15.0" (or whatever version you have
   installed).
4. At the prompt **postgres=#**, enter `exit` to quit the program and return to
   the command line prompt.

## Connect to PostgreSQL database server using **pgAdmin**

In subsequent lessons we will primarily use the *pgAdmin** tool to work with
PostgreSQL.

| Windows                                                                                                   | macOS                                                                                                     | Linux                            |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|----------------------------------|
| Launch **pgAdmin 4**                                                                                      | Launch **pgAdmin 4** from the Launchpad.                                                                  | /usr/local/pgadmin4/bin/pgadmin4 |
| ![windows pgadmin tool](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/pgadmin.png) | ![mac pgadmin tool](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/pgadmin_mac.png) | Adjust the path as necessary.    |

1. Enter the password for the **postgres** user.
2. You should see a default server **PostgreSQL 15** along with a database named
   **postgres**:

![pgadmin default server](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/pgadmindefaultserver.png)

We will create new databases to work with in subsequent lessons and labs.
However, do not delete or alter the default **postgres** database.

If you do not have the default **PostgreSQL 15** server, scroll down to the
heading labeled
[2) Connect to PostgreSQL database server using pgAdmin](https://www.postgresqltutorial.com/postgresql-getting-started/connect-to-postgresql-database/)
and follow the directions to create the server.

## Conclusion

PostgreSQL provides two client application tools to connect and interact with
the database server:

- **psql** - a terminal-based front-end to PostgreSQL database server.
- **pgAdmin** - a desktop or web-based front-end to PostgreSQL database server.

## Resources

[Getting Started with PostgreSQL](https://www.postgresqltutorial.com/postgresql-getting-started/)
