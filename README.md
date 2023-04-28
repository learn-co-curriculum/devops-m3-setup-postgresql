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
install psql on our VM we created and pgAdmin on our host computers. We can then
connect pgAdmin to our database on our VM in a similar way to how we connected
VSCode to our VM in the last module.

## Installing PostgreSQL on the Virtual Machine

We'll be installing PostgreSQL with version 15. Since this package isn't in the
default repository, we can enable its official package using the commands below.

```bash
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null
```

Remember, when running these commands in the terminal to check for any errors.

Next we'll update and upgrade the packages:

```bash
sudo apt update && sudo apt upgrade
```

Now we can install PostgreSQL on our VM!

```bash
sudo apt install postgresql-15
```

Running the above command should install PostgreSQL with version 15. If we left
off the "-15", it would install the latest PostgreSQL version. It should be
noted these commands could take a little bit to run.

Once everything looks like it has been successfully installed, switch over to
the `postgres` user. This user is added once we install PostgreSQL. To switch to
the `postgres` user, run the following command:

```bash
sudo -i -u postgres
```

Once there, we can run the `psql` command. Try entering `psql` into the
terminal.

<details>
    <summary>Did you run into an error that looks like this? <br>
      <code>psql: could not connect to server: No such file or directory<br>
            Is the server running locally and accepting
            connections on Unix domain socket "/var/run/postgresql/.s.PGSQL.5432"
      </code>
    </summary>

  <p>If so, don't worry! It just means PostgreSQL is probably not running!</p>
  <p>Type <code>exit</code> to get out of the `postgres` user space and then run the command:</p>
  <code>sudo service postgresql start</code>
  <p>This can take a couple of seconds to start the PostgreSQL service. Once it has started, check the status to make sure it is running:</p>
  <code>sudo service postgresql status</code>
  <p>This might produce the following output:</p>
  <img src="https://curriculum-content.s3.amazonaws.com/pe-mod-3/setup-postgres/postgres-status.png"/>
  <p>Now go back and switch to the <code>postgres</code> user and try the <code>psql</code> command again.</p>

</details>

The `psql` command will take us into the PostgreSQL space:

```bash
postgres=#
```

Once in this space, double check the version by typing:

```bash
SELECT version();
```

Ensure this prints out the correct version we installed! To quit, enter `\q`.
This will lead us back to our `postgres` user space. To get out of the
`postgres` user space, type `exit`.

We're all done installing PostgreSQL on our virtual machine! Now let's install
pgAdmin 4 on our host machines to give us a nice user interface to interact with
the databases we will create.
123456789101112131415161718192021222324252627ac282930313233343536373839404041434

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
