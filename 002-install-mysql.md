# MySQL

Now that you know what a database, table and a column are, the next thing that you would need to do is install a database service where you would be running your SQL queries on.

We would be using MySQL as it is free, opensource and very widely used.

## Installing MySQL

As we are going to use **Ubuntu**, in order to install to install MySQL run the following commands:

* First update your `apt` repository:

```
sudo apt update -y
```

* Then install MySQL:

```
sudp apt install mysql-server mysql-client
```

We are installing 2 packages, one is the actual MySQL server and the other is the MySQL client which would allow us to connect to the MySQL server and run our queries.

In order to check if MySQL is running run the following command:

```
sudo systemctl status mysql.service
```
In order to secure your MySQL server you could run the following command:

```
sudo mysql_secure_installation
```

Then follow the promt and finally choose a secure password and save it in a secure place like a password manager.

With that you would have MySQL installed on your Ubuntu server. The above should also work just fine on Debina.

### Install MySQL on Mac

I would recommend installing MySQL using [Homebrew]():

```
brew install mysql
```

After that start MySQL:

```
brew services start mysql
```

And finally secure it:

```
mysql_secure_installation
```

In case that you ever need to stop the MySQL service you could do so with the following command:

```
brew services stop mysql
```

### Install MySQL on Windows

In order to install MySQL on Windows, I would recommend following the steps from the official documentation here:

[https://dev.mysql.com/doc/refman/8.0/en/windows-installation.html](https://dev.mysql.com/doc/refman/8.0/en/windows-installation.html)

## Accessing MySQL via CLI

To access MySQL run the `mysql` command followed by your user:

```
mysql -u root -p
```

## Creating a database

After that switch to the `demo` database that we created in the previous chapter:

```
use demo;
```

## GUI clients

If you prefer using a GUI clients, you could take a look a the following ones and install them locally on your laptop:

* [MySQL Workbench](https://www.mysql.com/products/workbench/)
* [Sequel Pro](https://www.sequelpro.com/)
* [TablePlus](https://tableplus.com/)

This will allow you to connect to your database via a graphical interface rather than the `mysql` command line tool.