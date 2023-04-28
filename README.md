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

```sql
SELECT version();
```

Ensure this prints out the correct version we installed! While we are in this
space, let's change password. For demonstration purposes, we have the password
here set as "postgres". If you use a different password, make sure to remember
it!

```sql
ALTER USER postgres PASSWORD 'postgres';
```

To quit, enter `\q`. This will lead us back to our `postgres` user space. To get
out of the`postgres` user space, type `exit`.

We're all done installing PostgreSQL on our virtual machine! Now let's install
pgAdmin 4 on our host machines to give us a nice user interface to interact with
the databases we will create.

## Installing pgAdmin 4 on the Host Machine

### Windows Instructions

1. Download pgAdmin 4 at [this link here](https://www.pgadmin.org/download/pgadmin-4-windows/).
   Choose the latest version available.
2. Once you have downloaded it, run the executable and follow the set-up
   prompts with the default configurations.

### macOS Instructions

1. Download pgAdmin 4 at [this link here](https://www.pgadmin.org/download/pgadmin-4-macos/).
   Choose the latest version available.
2. Once you have downloaded it, run the executable and follow the set-up
   prompts with the default configurations.

## Connect to PostgreSQL database server using **pgAdmin**

In subsequent lessons we will primarily use the *pgAdmin** tool to work with
PostgreSQL.

| Windows                                                                                                   | macOS                                                                                                     |
|-----------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Launch **pgAdmin 4**                                                                                      | Launch **pgAdmin 4** from the Launchpad.                                                                  |
| ![windows pgadmin tool](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/pgadmin.png) | ![mac pgadmin tool](https://curriculum-content.s3.amazonaws.com/6002/setting-up-postgres/pgadmin_mac.png) |
123456789101112131415161718192021222324252627ac282930313233343536373839404041434
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

- [Getting Started with PostgreSQL](https://www.postgresqltutorial.com/postgresql-getting-started/)
- [Install PostgreSQL Linux](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql-linux/)
- [How to Install PostgreSQL 15 on Ubuntu 22.04 Step-by-Step](https://www.linuxtechi.com/how-to-install-postgresql-on-ubuntu/)
- [How to Start PostgreSQL](https://askubuntu.com/questions/1206416/how-to-start-postgresql)
