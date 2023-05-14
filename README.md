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

Once in this space, double-check the version by typing:

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

Next we want to open up PostgreSQL so our host machine can access it through
pgAdmin.

Navigate to the `/etc/postgresql/15/main` directory and, as the `sudo`
user, open up the `postgresql.conf` file:

```bash
sudo vi postgresql.conf
```

Once in the file, search for "list_addresses". It may be commented out.
Un-comment it out and change the line to the following:

```text
listen_addresses = '*'
```

Within this file, note the port that PostgreSQL will be running on. By default,
this should be 5432. If it is a different port number, take note of
it, as we will need this in a later section. Before saving the file, check the
changes made with the screenshot below:

![postgresql.conf-changes](https://curriculum-content.s3.amazonaws.com/pe-mod-3/setup-postgres/postgresql-conf-file-changes.png)

Save and exit out of the `postgresql.conf` file and run the following command:

```bash
sudo ufw allow 5432
```

This will enable the Postgres port.

We'll need to make one more change so that our host machine can access the
database on our virtual machines. Open up the `pg_hba.conf` file:

```bash
sudo vi pg_hba.conf
```

Search for "IPv4 local connections" inside the file. We may see a line under
that comment that looks like this:

```text
host    all             all             127.0.0.1/32            scram-sha-256
```

Copy and paste the following line below the existing line:

```text
host    all             all             0.0.0.0/0                md5
```

> **Note**: Adding "host all all 0.0.0.0/0 md5" to the PostgreSQL configuration
> file will allow any client to connect to the database server using any user
> account, as long as the client can provide the correct password. While this
> can be convenient for development or testing purposes, it is generally not
> recommended for production environments due to several potential security
> issues. In a real production environment, we should either whitelist an IP or
> use a different method of connection like
> [SSH Tunnels](https://www.postgresql.org/docs/8.3/ssh-tunnels.html).

Before saving the file, check the changes align with the text below:

```text
host    all             all             127.0.0.1/32            scram-sha-256
host    all             all             0.0.0.0/0                md5
```

Save and close the `pg_hba.conf` file and reboot the virtual machine by typing
the `reboot` command or exit out of VirtualBox.

Once the virtual machine has restarted, we'll need to install one more
dependency to get everything working and connected. Run the following command
below and ensure there are no errors:

```bash
sudo apt install libpq-dev
```

This dependency is needed for something called `psycopg2`, which will help
connect our Python code to our database. We'll learn more about that later in
another section.

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

Once you open up pgAdmin 4, navigate to the "Dashboard" tab in the main window
and choose "Add New Server" under "Quick Links":

![add-new-server](https://curriculum-content.s3.amazonaws.com/pe-mod-3/setup-postgres/pgAdmin-add-new-server.png)

A "Register - Server" dialogue will then pop up. In the "General" tab of this
dialogue, specify a name for the server. For example, enter in "my-vm" or
"vm-db":

![name-server](https://curriculum-content.s3.amazonaws.com/pe-mod-3/setup-postgres/pgAdmin-register-server-general.png)

After filling out the name field in the "General" tab, navigate over to the
"Connection" tab. Fill out the following fields:

![set-up-connection](https://curriculum-content.s3.amazonaws.com/pe-mod-3/setup-postgres/pgAdmin-register-server-connection.png)

- For the "Host name/address" field, enter the virtual machine's IP address. For
  example: 10.0.0.XX.
- The default port number for a PostgreSQL database is 5432. If your database is
  running on a different port, enter that port number in the "Port" field.
- The "Password" field should be set to the password you used to alter the
  `postgres` user in the previous section.

Click "Save". pgAdmin will then attempt to connect to the server. If all goes
well, the "Dashboard" tab should then look something like this:

![connected-server](https://curriculum-content.s3.amazonaws.com/pe-mod-3/setup-postgres/pgAdmin-connected-vm.png)

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
- [How to Connect Remotely to PostgreSQL Database Using pgAdmin](https://chemicloud.com/kb/article/postgresql-database-pgadmin/)
- [StackOverflow: Can't Connect to PostgreSQL on Port 5432](https://stackoverflow.com/questions/38466190/cant-connect-to-postgresql-on-port-5432)
