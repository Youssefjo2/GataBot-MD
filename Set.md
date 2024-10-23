To set up both MySQL 8 and MariaDB 10.2 on an Ubuntu VPS via SSH, you’ll need to manage them carefully as they are conflicting database systems. Here’s a step-by-step guide for setting them up and ensuring they don’t interfere with each other:

### Step 1: Update Your System
Ensure your system is up to date before installing any packages.

```bash
sudo apt update
sudo apt upgrade
```

### Step 2: Install MySQL 8

1. **Add the MySQL APT Repository:**

   Download and add the MySQL repository to your system.

   ```bash
   wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
   sudo dpkg -i mysql-apt-config_0.8.22-1_all.deb
   sudo apt update
   ```

   During installation, it will prompt you to choose which version of MySQL you want to install. Make sure to select **MySQL 8.0**.

2. **Install MySQL:**

   ```bash
   sudo apt install mysql-server
   ```

3. **Secure MySQL Installation:**

   After installing, secure your MySQL server by running:

   ```bash
   sudo mysql_secure_installation
   ```

   Follow the prompts to set up the root password, remove test databases, and apply security settings.

4. **Start and Enable MySQL:**

   ```bash
   sudo systemctl start mysql
   sudo systemctl enable mysql
   ```

5. **Verify Installation:**

   ```bash
   mysql --version
   ```

   Log in to the MySQL shell to ensure everything works:

   ```bash
   sudo mysql -u root -p
   ```

### Step 3: Install MariaDB 10.2

1. **Add the MariaDB Repository:**

   Add the MariaDB repository to your system for version 10.2.

   ```bash
   sudo apt install software-properties-common
   sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://mirror.netinch.com/pub/mariadb/repo/10.2/ubuntu bionic main'
   sudo apt update
   ```

2. **Install MariaDB:**

   ```bash
   sudo apt install mariadb-server
   ```

3. **Secure MariaDB Installation:**

   Secure your MariaDB installation similarly to MySQL.

   ```bash
   sudo mysql_secure_installation
   ```

4. **Start and Enable MariaDB:**

   ```bash
   sudo systemctl start mariadb
   sudo systemctl enable mariadb
   ```

5. **Verify Installation:**

   ```bash
   mariadb --version
   ```

   Log in to the MariaDB shell to confirm everything is set up properly:

   ```bash
   sudo mysql -u root -p
   ```

### Step 4: Configure Ports for Coexistence

By default, both MySQL and MariaDB use port `3306`. To avoid conflicts, you’ll need to change the port for one of them. Here’s how to change MariaDB’s port to `3307`:

1. **Edit the MariaDB Configuration:**

   Open the configuration file:

   ```bash
   sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
   ```

2. **Change the Port:**

   Find the `[mysqld]` section and change the port to `3307`.

   ```ini
   [mysqld]
   port = 3307
   ```

3. **Restart MariaDB:**

   After saving the changes, restart MariaDB:

   ```bash
   sudo systemctl restart mariadb
   ```

### Step 5: Verify Both Services Are Running

Ensure both MySQL and MariaDB are running on different ports:

1. **Check MySQL:**

   ```bash
   sudo netstat -tuln | grep 3306
   ```

2. **Check MariaDB:**

   ```bash
   sudo netstat -tuln | grep 3307
   ```

### Step 6: Managing Services

Since you have both MySQL and MariaDB installed, you can control them with these commands:

- **Start MySQL:**

  ```bash
  sudo systemctl start mysql
  ```

- **Start MariaDB:**

  ```bash
  sudo systemctl start mariadb
  ```

- **Stop MySQL:**

  ```bash
  sudo systemctl stop mysql
  ```

- **Stop MariaDB:**

  ```bash
  sudo systemctl stop mariadb
  ```

### Step 7: Access Databases Separately

When connecting to MySQL and MariaDB, ensure you specify the correct port:

- **MySQL:**

  ```bash
  mysql -u root -p -h 127.0.0.1 --port=3306
  ```

- **MariaDB:**

  ```bash
  mysql -u root -p -h 127.0.0.1 --port=3307
  ```

This setup allows you to run both MySQL 8 and MariaDB 10.2 on the same Ubuntu VPS without conflicts.
