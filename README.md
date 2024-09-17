To update your system and install the prerequisite tools as a non-root user, follow these steps:

1. **Update your system:**
   ```bash
   sudo apt-get update
   ```

2. **Install prerequisite tools:**
   ```bash
   sudo apt-get install -y autoconf gcc libc6 make wget unzip apache2 php libapache2-mod-php7.4 libgd-dev
   ```

3. **Install OpenSSL and SSL development libraries:**
   ```bash
   sudo apt-get install -y openssl libssl-dev
   ```

These commands will install all necessary packages, including tools for compiling software, web servers, PHP, and SSL libraries.

When running the command `$ sudo vi /etc/apache2/mods-enabled/dir.conf`, it will open the `dir.conf` file in the **vi editor**, where you can modify the order in which Apache looks for index files (like `index.php`, `index.html`, etc.). Here's the detailed process:

### Step-by-Step Process:

1. **Open the Configuration File**:
   Run the following command to open the `dir.conf` file in the vi editor:
   ```bash
   sudo vi /etc/apache2/mods-enabled/dir.conf
   ```

2. **Edit the File**:
   Inside the file, you'll see a directive that looks like this:
   ```apache
   <IfModule mod_dir.c>
       DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
   </IfModule>
   ```

3. **Move `index.php` to the First Position**:
   Modify the `DirectoryIndex` line so that `index.php` is listed first. It should look like this:
   ```apache
   <IfModule mod_dir.c>
       DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
   </IfModule>
   ```

4. **Save and Exit**:
   After moving `index.php` to the first position, follow these steps to save and exit the vi editor:
   - Press `Esc` to ensure you're in **command mode**.
   - Type `:wq` and press **Enter** to write the changes and quit the editor.

5. **Restart Apache**:
   After saving the changes, restart the Apache service to apply the updated configuration:
   ```bash
   sudo service apache2 restart
   ```

6. **Verify the Changes**:
   To ensure the changes have been applied correctly, you can create or move an `index.php` file to your web root (`/var/www/html`) and access the server via a browser. It should prioritize `index.php` over other index files.

By following these steps, Apache will serve `index.php` as the default index page before trying other index files.


To download, extract, compile, and install the Nagios Core source, follow these steps in detail:

### Step-by-Step Process:

#### 1. **Navigate to `/opt` Directory:**
   Change the current working directory to `/opt` where you will download Nagios Core:
   ```bash
   cd /opt
   ```

#### 2. **Download Nagios Core Source:**
   Download the Nagios Core tarball from the official GitHub repository using `wget`:
   ```bash
   sudo wget -O nagioscore.tar.gz https://github.com/NagiosEnterprises/nagioscore/archive/nagios-4.4.14.tar.gz
   ```

#### 3. **Extract the Downloaded File:**
   After downloading the tarball, extract it using the `tar` command:
   ```bash
   sudo tar xzf nagioscore.tar.gz
   ```

#### 4. **Change to the Extracted Directory:**
   Once extracted, the directory `nagioscore-nagios-4.4.14/` will be created. Change to this directory:
   ```bash
   cd nagioscore-nagios-4.4.14/
   ```

#### 5. **Configure Nagios:**
   Now, configure Nagios to use the Apache configuration directory `/etc/apache2/sites-enabled`:
   ```bash
   sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
   ```

#### 6. **Compile the Nagios Source:**
   After configuring Nagios, compile the source code using `make`. This command will compile everything:
   ```bash
   sudo make all
   ```

### Notes on Each Step:
- **Directory Navigation:**
  You initially navigate to `/opt` because it's a common directory used to store third-party or optional software.
  
- **Downloading with `wget`:**
  The `wget` command fetches the Nagios source code archive from the GitHub repository. You are saving it as `nagioscore.tar.gz`.

- **Extraction with `tar`:**
  The `tar` command extracts the contents of the compressed file into a new directory called `nagioscore-nagios-4.4.14`.

- **Configuration:**
  The `./configure` script prepares the source code for compilation. The option `--with-httpd-conf=/etc/apache2/sites-enabled` ensures that Nagios' web configuration integrates with Apache's site-enabled configuration directory.

- **Compilation:**
  The `make all` command compiles the Nagios Core source code into executable binaries, preparing it for installation.

After running these steps, Nagios Core will be compiled and ready for the next steps, such as installation and configuring the Apache web interface.


To create the required user and group for Nagios, follow these steps:

### Step 5: Create User and Group for Nagios

#### 1. **Run the command to create Nagios user and group:**
   This command will automatically create a Nagios group and user:
   ```bash
   sudo make install-groups-users
   ```

   This creates:
   - A user named `nagios`.
   - A group named `nagcmd`.

#### 2. **Add Apache's `www-data` user to the Nagios group:**
   The Apache web server runs under the `www-data` user, so you need to add this user to the `nagios` group to allow Apache to interact with Nagios.
   Run the following command:
   ```bash
   sudo usermod -a -G nagios www-data
   ```

This ensures that the Apache web server has the necessary permissions to run Nagios and interact with its components.

After this step, you will have successfully created the necessary user and group, and the web server will be granted the required permissions.
To correctly change the permissions by adding the `www-data` user to the `nagios` group, you can follow these steps:

1. **Add the `www-data` user to the `nagios` group:**
   This command ensures that the Apache web server user (`www-data`) is added to the `nagios` group:
   ```bash
   sudo usermod -a -G nagios www-data
   ```

2. **Verify the Changes:**
   To confirm that `www-data` has been successfully added to the `nagios` group, you can run the following command:
   ```bash
   groups www-data
   ```

   This will list all the groups that `www-data` belongs to, and you should see `nagios` in the list.

After running this, Apache will have the appropriate permissions to interact with Nagios files and directories.


To install Nagios binaries, service/daemon, command mode, configuration files, and Apache configuration files, you need to run the following commands in sequence:

### Step 6: Install Binaries, Service/Daemon, Command Mode, Configuration Files, and Apache Config

1. **Install Nagios Binaries:**
   This command installs the core Nagios binaries:
   ```bash
   sudo make install
   ```

2. **Install Nagios Service/Daemon:**
   This command sets up Nagios as a system service/daemon, allowing it to start at boot:
   ```bash
   sudo make install-daemoninit
   ```

3. **Install Command Mode:**
   Command mode allows external commands to be submitted to Nagios (for example, through the web interface). Run the following:
   ```bash
   sudo make install-commandmode
   ```

4. **Install Configuration Files:**
   This command installs the sample configuration files for Nagios:
   ```bash
   sudo make install-config
   ```

5. **Install Apache Configuration File:**
   This command installs the Nagios configuration file for Apache, setting up the web interface:
   ```bash
   sudo make install-webconf
   ```

### Summary of Commands:
- **`make install`**: Installs Nagios core binaries.
- **`make install-daemoninit`**: Installs the service for automatic startup.
- **`make install-commandmode`**: Sets up permissions for submitting commands.
- **`make install-config`**: Installs sample configuration files.
- **`make install-webconf`**: Configures Nagios to work with Apache for the web interface.

Run these commands one after another, and you'll have Nagios installed with all required components, including Apache configuration for the web interface.


To configure Nagios and set up the directory for your server monitoring configuration files, follow these detailed steps:

### Step 7: Configure Nagios

1. **Open the Nagios Configuration File:**
   Open the main Nagios configuration file (`nagios.cfg`) using the `vi` editor:
   ```bash
   sudo vi /usr/local/nagios/etc/nagios.cfg
   ```

2. **Locate the `cfg_dir` Directive:**
   Inside the `nagios.cfg` file, search for the following line (or something similar):
   ```bash
   #cfg_dir=/usr/local/nagios/etc/servers
   ```

3. **Uncomment the `cfg_dir` Line:**
   Press **`i`** to enter insert mode in `vi`. Then, remove the `#` at the beginning of the line to uncomment it:
   ```bash
   cfg_dir=/usr/local/nagios/etc/servers
   ```
   This tells Nagios to look in the `/usr/local/nagios/etc/servers` directory for additional configuration files related to the resources and servers you want to monitor.

4. **Save and Exit the Editor:**
   After making the changes, save and exit the `vi` editor by pressing **`Esc`**, then typing `:wq` and pressing **Enter**.

5. **Create the Servers Directory:**
   Now, create the directory where you will store the configuration files for monitoring servers and resources:
   ```bash
   sudo mkdir /usr/local/nagios/etc/servers
   ```

### Summary:
- You edited the `nagios.cfg` file to include the `cfg_dir=/usr/local/nagios/etc/servers` directive.
- Then, you created the `/servers` directory where you'll store your server monitoring configuration files.

This ensures that Nagios will look for additional configuration files in the `/usr/local/nagios/etc/servers` directory for any resources or servers you want to monitor.


To configure Nagios contacts and set your email address, follow these steps:

### Step 8: Configure Nagios Contacts

1. **Open the Contacts Configuration File:**
   Run the following command to open the `contacts.cfg` file in the `vi` editor:
   ```bash
   sudo vi /usr/local/nagios/etc/objects/contacts.cfg
   ```

2. **Edit Contact Information:**
   In the `contacts.cfg` file, look for the section that defines the contact details for `nagiosadmin`. It will look something like this:
   ```bash
   define contact{
       contact_name            nagiosadmin
       use                     generic-contact
       alias                   Nagios Admin
       email                   nagios@localhost
   }
   ```

3. **Update the Email Address:**
   Press **`i`** to enter insert mode in `vi`, and change the `email` field to your email address (`sk0990670@gmail.com`):
   ```bash
   email                   sk0990670@gmail.com
   ```

4. **Save and Exit:**
   After making the changes, press **`Esc`**, then type `:wq` and press **Enter** to save the file and exit.

This will configure Nagios to send notifications to your email (`sk0990670@gmail.com`) whenever there are alerts or issues with the monitored resources.


To configure the `check_nrpe` command in Nagios, follow these steps:

### Step 9: Configure `check_nrpe` Command

1. **Open the `commands.cfg` File:**
   Use the following command to open the `commands.cfg` file in the `vi` editor:
   ```bash
   sudo vi /usr/local/nagios/etc/objects/commands.cfg
   ```

2. **Go to the Last Line (Line 253):**
   Once inside the `vi` editor, press **`Esc`** and type `:253` to jump to line 253.

3. **Add the `check_nrpe` Command Definition:**
   Press **`i`** to enter insert mode and add the following block of code:
   ```bash
   define command{
       command_name    check_nrpe
       command_line    $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
   }
   ```
   This command definition tells Nagios how to execute the `check_nrpe` command. It uses `$HOSTADDRESS$` (the IP address or hostname of the remote server) and `$ARG1$` (the specific check or command being run on the remote host).

4. **Save and Exit the File:**
   After adding the command, press **`Esc`**, type `:wq`, and press **Enter** to save the file and exit the editor.

### Explanation:
- **`command_name check_nrpe`**: This defines the name of the command as `check_nrpe`.
- **`command_line $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$`**: This specifies the command-line execution format where `USER1` refers to the path of Nagios plugins, `HOSTADDRESS` refers to the host being monitored, and `ARG1` is the specific command to run via NRPE (Nagios Remote Plugin Executor).

After this, Nagios will be configured to use the `check_nrpe` command, allowing it to perform remote checks using NRPE.


To configure Apache for Nagios, you need to enable the `rewrite` and `cgi` modules. Here are the steps:

### Step 10: Configure Apache

1. **Enable the `rewrite` module:**
   This command enables URL rewriting, which is often required by web applications:
   ```bash
   sudo a2enmod rewrite
   ```

2. **Enable the `cgi` module:**
   This command enables the Common Gateway Interface (CGI) module, allowing Apache to execute CGI scripts (which Nagios relies on):
   ```bash
   sudo a2enmod cgi
   ```

After running these commands, restart Apache to apply the changes:
```bash
sudo service apache2 restart
```

Now, Apache is ready to work with Nagios and process CGI scripts.


To configure the firewall using **Uncomplicated Firewall (UFW)** for Nagios, follow these steps:

### Step 11: Configure Firewall

1. **Allow Apache through the firewall:**
   This command allows Apache to communicate through the firewall:
   ```bash
   sudo ufw allow Apache
   ```

2. **Enable the firewall:**
   This command activates UFW. You will need to press **`y`** when prompted to confirm:
   ```bash
   sudo ufw enable
   ```

3. **Reload the firewall rules:**
   After making changes, reload the firewall to apply the new settings:
   ```bash
   sudo ufw reload
   ```

By running these commands, you ensure that Apache can serve Nagios through the firewall, and your firewall rules are properly set and reloaded.


To configure the Nagios service, follow these steps:

### Step 12: Configure a Nagios Service

1. **Create and open the `nagios.service` file:**
   Run the following command to open the file for editing:
   ```bash
   sudo vi /etc/systemd/system/nagios.service
   ```

2. **Add the following content to the file:**
   In the `vi` editor, press **`i`** to enter insert mode, and then add the following lines:
   ```bash
   [Unit] 
   Description=Nagios 
   BindTo=network.target 

   [Install] 
   WantedBy=multi-user.target 

   [Service] 
   Type=simple 
   User=nagios 
   Group=nagios 
   ExecStart=/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
   ```

3. **Save and exit:**
   After entering the data, press **`Esc`**, then type `:wq`, and press **Enter** to save and close the file.

4. **Reload systemd to recognize the new Nagios service:**
   ```bash
   sudo systemctl daemon-reload
   ```

5. **Enable the Nagios service to start on boot:**
   ```bash
   sudo systemctl enable nagios
   ```

6. **Start the Nagios service:**
   ```bash
   sudo systemctl start nagios
   ```

7. **Verify the status of the Nagios service:**
   Check if Nagios is running properly with this command:
   ```bash
   sudo systemctl status nagios
   ```

By creating this service, Nagios will now run as a background service and will start automatically when your system boots.


To create the Nagios admin user password and configure the Nagios web interface, follow these steps:

### Step 13: Create `nagiosadmin` User Password

1. **Create the password for the `nagiosadmin` user:**
   Run the following command to create a new password for the `nagiosadmin` user:
   ```bash
   sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
   ```

2. **Enter the password:**
   - You will be prompted to enter a new password.
   - Type the password (it won't be visible), and then re-type it to confirm.

3. **Configure the Nagios Web Interface:**
   To set up the web interface for Nagios, run the following commands:

   - **Link Nagios configuration to Apache:**
     This creates a symbolic link for the Nagios Apache configuration file:
     ```bash
     sudo ln -s /etc/apache2/sites-available/nagios.conf /etc/apache2/sites-enabled/
     ```

   - **Set Nagios to start at boot:**
     This creates a symbolic link to ensure that Nagios starts automatically when the system boots:
     ```bash
     sudo ln -s /etc/init.d/nagios /etc/rcS.d/S99nagios
     ```

4. **Restart Apache to apply the changes:**
   ```bash
   sudo service apache2 restart
   ```

Now, the `nagiosadmin` user will have a password set for accessing the Nagios web interface, and Apache will be configured to serve the Nagios site.


To restart Apache and start the Nagios services, follow these steps:

### Step 14: Restart Apache and Start Nagios Services

1. **Restart Apache:**
   This command will restart the Apache web server to ensure it applies all configuration changes:
   ```bash
   sudo systemctl restart apache2.service
   ```

2. **Start Nagios:**
   This command starts the Nagios service:
   ```bash
   sudo systemctl start nagios.service
   ```

By running these commands one after the other, you will ensure that both Apache and Nagios are up and running with their latest configurations.


To install the necessary plugins for Nagios, you can use the following command:

### Step 15: Install Nagios Plugins

1. **Run the Command to Install Plugins:**
   This command installs various tools and libraries required for Nagios plugins:
   ```bash
   sudo apt-get install -y autoconf gcc libc6 libmcrypt-dev make libssl-dev wget bc gawk dc build-essential snmp libnet-snmp-perl gettext
   ```

   This command ensures that any missing dependencies and libraries needed for Nagios plugins are installed. It includes:
   - **`autoconf`**: Tool for generating configuration scripts.
   - **`gcc`**: GNU Compiler Collection.
   - **`libc6`**: GNU C Library.
   - **`libmcrypt-dev`**: Development files for the mcrypt library.
   - **`make`**: Build automation tool.
   - **`libssl-dev`**: Development files for OpenSSL.
   - **`wget`**: Command-line utility for downloading files.
   - **`bc`**: Arbitrary precision calculator language.
   - **`gawk`**: GNU version of the `awk` text processing language.
   - **`dc`**: Desk calculator.
   - **`build-essential`**: Packages needed for building software.
   - **`snmp`**: Simple Network Management Protocol tools.
   - **`libnet-snmp-perl`**: Perl modules for SNMP.
   - **`gettext`**: Tools for message translation.

By running this command, you ensure that all necessary plugins and dependencies are available for Nagios to function properly.


To download, extract, compile, and install the Nagios plugins package, follow these steps:

### Step 16: Download, Extract, Compile, and Install the Plugins Package

1. **Change to the `/opt` Directory:**
   Navigate to the `/opt` directory where you'll download and install the plugins:
   ```bash
   cd /opt
   ```

2. **Download the Nagios Plugins Package:**
   Use `wget` to download the Nagios plugins package. The `--no-check-certificate` option bypasses SSL certificate checks if needed:
   ```bash
   sudo wget --no-check-certificate -O nagios-plugins.tar.gz https://github.com/nagios-plugins/nagios-plugins/archive/release-2.4.6.tar.gz
   ```

3. **Extract the Downloaded Package:**
   Use `tar` to extract the contents of the downloaded package:
   ```bash
   sudo tar xzf nagios-plugins.tar.gz
   ```

4. **Change to the Extracted Directory:**
   Navigate to the extracted Nagios plugins directory:
   ```bash
   cd nagios-plugins-release-2.4.6/
   ```

5. **Set Up the Tools:**
   Run the setup script to prepare the tools for configuration:
   ```bash
   sudo ./tools/setup
   ```

6. **Configure the Plugins:**
   Configure the Nagios plugins package:
   ```bash
   sudo ./configure
   ```

7. **Build the Plugins:**
   Compile the plugins using `make`:
   ```bash
   sudo make
   ```

8. **Install the Plugins:**
   Install the compiled plugins:
   ```bash
   sudo make install
   ```

### Summary
- **Change Directory**: `cd /opt`
- **Download**: `wget` command
- **Extract**: `tar` command
- **Navigate**: `cd nagios-plugins-release-2.4.6/`
- **Set Up**: `./tools/setup`
- **Configure**: `./configure`
- **Build**: `make`
- **Install**: `make install`

This process will ensure that the Nagios plugins are correctly downloaded, configured, built, and installed on your system.


To access and manage your Nagios Server, follow these steps:

### Step 17: Opening Nagios Server Website

1. **Access AWS EC2 Instances Page:**
   - Go to the AWS Management Console and navigate to the EC2 Dashboard.

2. **Get the Public IP Address:**
   - Select the Nagios server instance from the list.
   - Copy the Public IP address from the instance details.

3. **Open the Nagios Web Interface:**
   - Open a new browser tab.
   - Paste the Public IP address into the address bar and append `/nagios` to it.
     ```
     http://<PUBLIC_IP_ADDRESS>/nagios
     ```

4. **Log in:**
   - Enter the username (`nagiosadmin`) and the password you set earlier using the `htpasswd` command.

5. **Check the Hosts Section:**
   - Once logged in, go to the **Hosts** section.
   - You should see a host named **localhost** with a status of **Up**.

6. **Check the Services Section:**
   - Go to the **Services** section.
   - You may notice a service with the status **Critical** for **Swap usage**. This is because the Nagios server instance does not have a swap partition configured.

### Explanation for the Critical Status:
- **Swap Usage Error**: The critical error regarding swap usage indicates that there is no swap space on the server. Swap space is used as an extension of the system's physical memory. Without it, the server may face performance issues, especially if it runs out of physical RAM.

To address this error, you might need to configure a swap partition or file on the Nagios server to ensure smooth operation.


To fix the swap usage error by creating a .5GB swap file, follow these steps:

### Step 18: Fixing Swap Usage Error

1. **Create a .5GB Swap File:**
   Use the `dd` command to create a swap file of size .5GB. This file will be located in the `/root` directory and named `myswapfile`:
   ```bash
   sudo dd if=/dev/zero of=/root/myswapfile bs=1M count=512
   ```

   - **`if=/dev/zero`**: Input file is `/dev/zero`, which provides a stream of zeroes.
   - **`of=/root/myswapfile`**: Output file where the swap data will be written.
   - **`bs=1M`**: Block size of 1 megabyte.
   - **`count=1024`**: Number of blocks (1024 blocks of 1MB each make up .5GB).

2. **Set Up the Swap File:**
   After creating the swap file, you need to format it as swap space:
   ```bash
   sudo mkswap /root/myswapfile
   ```

3. **Activate the Swap File:**
   Turn on the swap file to start using it immediately:
   ```bash
   sudo swapon /root/myswapfile
   ```

4. **Verify Swap Usage:**
   Check if the swap file is active and being used:
   ```bash
   sudo swapon --show
   ```

5. **Make the Swap File Permanent:**
   To ensure the swap file is used on boot, add it to the `/etc/fstab` file:
   ```bash
   sudo sh -c 'echo "/root/myswapfile none swap sw 0 0" >> /etc/fstab'
   ```

6. **Verify the Changes:**
   Ensure that the swap space is correctly configured and active:
   ```bash
   free -h
   ```

By following these steps, you will create and configure a .5GB swap file, which should resolve the swap usage error in Nagios.


To check and change the permissions for the swap file, follow these steps:

### Step 19: Check and Change Permissions for the Swap File

1. **Check the Current Permissions:**
   Use the `ls` command to view the current permissions of the swap file:
   ```bash
   sudo ls -lh /root/myswapfile
   ```
   This command will display the file's permissions along with other details in a human-readable format.

2. **Change the Permissions:**
   Set the permissions so that only the root user can access the swap file:
   ```bash
   sudo chmod 600 /root/myswapfile
   ```
   - **`600`**: Sets the file permissions to read and write for the owner (root), and no permissions for others.

By following these steps, you'll ensure that the swap file has the appropriate permissions for security, allowing only the root user to read and write to it.


To set up the file as a swap file, you need to use the `mkswap` command. Here’s how you do it:

### Step 20: Make the File a Swap File

1. **Format the File as Swap:**
   Run the following command to format the file you created as a swap file:
   ```bash
   sudo mkswap /root/myswapfile
   ```

   This command initializes the file for use as swap space.

By running this command, you prepare the file to be used as swap space, which can then be activated and utilized by the system.


To enable the newly created swap file and start using it, follow these steps:

### Step 21: Enable the Newly Created Swap File

1. **Activate the Swap File:**
   Use the following command to enable the swap file:
   ```bash
   sudo swapon /root/myswapfile
   ```

   This command makes the swap file available for use by the system.

2. **Verify the Swap File is Active:**
   Check that the swap file is now active and in use:
   ```bash
   sudo swapon --show
   ```

   You should see the swap file listed, indicating that it is being used by the system.

By executing these commands, you'll activate the swap file, and the system will start using it to handle memory overflow.


### Step 22: Re-schedule the Service to Update Changes

1. **Navigate to the Nagios Website:**
   - Open the Nagios web interface in your browser.

2. **Go to the Services Section:**
   - Click on the **Services** tab to view the list of services being monitored.

3. **Select the Swap Usage Service:**
   - Find and click on the service related to **Swap usage**.

4. **Re-schedule the Next Check:**
   - In the service details page, look for the **Commands** menu.
   - Click on **Re-schedule the next check**. This will force Nagios to check the status of the swap usage again and update its monitoring information.

### Monitoring Remote Linux Host Machine Using NRPE Addon

To monitor a remote Linux host using the NRPE (Nagios Remote Plugin Executor) addon, 


### Step 23: Download, Extract, and Install NRPE Script on Remote Linux Host

1. **Launch and Configure EC2 Instance:**
   - Open the AWS EC2 service.
   - Launch a new instance with the name **Nagios-host-linux**.
   - Ensure the security group allows inbound traffic on HTTP (port 80), HTTPS (port 443), and SSH (port 22).

2. **Connect to the Instance:**
   - Use Super PuTTY to connect to your EC2 instance.

3. **Download, Extract, and Install NRPE Script:**

   ```bash
   # Change to the /opt directory
   cd /opt
   
   # Download the NRPE agent package
   sudo wget http://assets.nagios.com/downloads/nagiosxi/agents/linux-nrpe-agent.tar.gz
   
   # Extract the downloaded tarball
   sudo tar xzf linux-nrpe-agent.tar.gz
   
   # Change to the extracted directory
   cd linux-nrpe-agent
   
   # Run the installation script
   sudo ./fullinstall
   ```

   - During installation, you might be prompted for confirmation. Type `y` when asked to proceed.

4. **Obtain the Nagios Server Hostname:**

   - Run the following command to get the IP address of the Nagios server:
     ```bash
     hostname -i
     ```
   - Copy this IP address.

5. **Configure Allowed Hosts:**
   - On the remote Linux host, configure the `nrpe.cfg` file to allow the Nagios server to connect:
     ```bash
     sudo vi /usr/local/nagios/etc/nrpe.cfg
     ```
   - Locate the `allowed_hosts` directive and add the IP address of your Nagios server:
     ```plaintext
     allowed_hosts=<NAGIOS_SERVER_IP>
     ```
   - Save and exit the file.

By following these steps, you’ll successfully set up the NRPE agent on your remote Linux host, allowing Nagios to monitor it.



### Step 24: Add Host to Nagios Configuration on Nagios Server

To add the new host to your Nagios configuration, follow these steps:

1. **Create the Configuration File:**

   - On the Nagios server, create a new configuration file for the host:
     ```bash
     sudo vi /usr/local/nagios/etc/servers/nagihost.cfg
     ```

2. **Add Host Configuration:**

   - In the `nagihost.cfg` file, add the following configuration. Replace `<ADDRESS>` with the IP address of your Nagios host Linux (obtained using `hostname -i` on the remote host):
     ```plaintext
     define host {
         use                             linux-server
         host_name                       nagihost
         alias                           My first Apache server
         address                         <ADDRESS>
         max_check_attempts              5
         check_period                    24x7
         notification_interval           30
         notification_period             24x7
     }
     ```

   - Save and exit the editor.

3. **Restart Nagios Service:**

   - Apply the configuration changes by restarting the Nagios service:
     ```bash
     sudo systemctl restart nagios.service
     ```

By following these steps, you'll add the new host to your Nagios configuration and ensure that Nagios starts monitoring it.


### Step 25: Monitor the Newly Added Host in the Nagios Website

1. **Open the Nagios Web Interface:**

   - Navigate to the Nagios web interface in your browser.

2. **Go to the Hosts Section:**

   - Click on the **Hosts** tab.

3. **Verify the New Host:**

   - Look for the newly added host named **nagihost** in the list.
   - Ensure that the status is shown as **up**, indicating that Nagios is successfully monitoring the host.

You should now see the newly added host in your Nagios interface, with its status being monitored as expected.


### Step 26: Install Apache2 Web Server on Remote Linux Host Machine

To install and start the Apache2 web server on your remote Linux host, follow these commands:

```bash
# Install Apache2 Web Server
sudo apt-get install apache2 -y

# Start the Apache2 Service
sudo service apache2 start
```

Run these commands on the remote Linux host machine to complete the installation and start the Apache2 web server.


### Step 27: Deploy Custom Webpage in Apache Web Server

To deploy your custom webpage on the Apache web server, follow these steps:

1. **Change to the Web Directory:**

   ```bash
   cd /var/www/html
   ```

2. **Remove the Default `index.html`:**

   ```bash
   sudo rm index.html
   ```

3. **Create and Edit a New `index.html` File:**

   ```bash
   sudo vi index.html
   ```

4. **Add Your Custom HTML Content:**

   - In the `vi` editor, enter your HTML content. For example:

     ```html
     <h1>Welcome to Apache2 Web Server</h1>
     ```

5. **Save and Exit `vi`:**

   - Press `Esc` to enter command mode.
   - Type `:wq` and press `Enter` to save and exit.

Your custom webpage should now be deployed and accessible via your Apache web server.


### Step 28: Add HTTP Service to `nagihost.cfg` in Nagios Server

1. **Edit the `nagihost.cfg` File:**

   ```bash
   sudo vi /usr/local/nagios/etc/servers/nagihost.cfg
   ```

2. **Add the HTTP Service Definition:**

   - Add the following service definition at the end of the file:

     ```plaintext
     define service {
        use                      generic-service
        host_name                nagihost
        service_description      HTTP
        check_command            check_http
     }
     ```

   - Save and exit the editor.

3. **Restart the Nagios Service:**

   ```bash
   sudo systemctl restart nagios.service
   ```

By following these steps, you'll configure Nagios to monitor the HTTP service on your `nagihost` and ensure that Nagios applies the new configuration.


### Step 29: Monitor the HTTP Service in the Nagios Website

1. **Open the Nagios Web Interface:**

   - Navigate to your Nagios web interface in your browser.

2. **Go to the Services Section:**

   - Click on the **Services** tab.

3. **Verify the HTTP Service:**

   - Locate the HTTP service for **nagihost** in the list.
   - Ensure that the status is shown as **OK**.

4. **If Status is Critical or Pending:**

   - Click on the service that is showing a **Critical** or **Pending** status.

5. **Reschedule the Check:**

   - In the service detail view, go to the **Service State Information** section.
   - Click on **Reschedule the Next Check** under the Commands menu.

6. **Commit the Command:**

   - In the command options, click on **Commit** to confirm.

7. **Complete the Process:**

   - Click on **Done** to finish.

8. **Verify the Status:**

   - The service status should now show as **OK** if the configuration and checks are correct.

By following these steps, you'll be able to ensure that the HTTP service status for `nagihost` is properly monitored and displayed as expected in the Nagios web interface.


### Step 30: Terminate Nagios Instances

1. **Exit from the Instances:**

   - To log out of the SSH sessions on both the Nagios server and the Nagios host Linux instances, use the following command:

     ```bash
     exit
     ```

2. **Terminate EC2 Instances:**

   - Go to the **EC2 Instances** page on the AWS Management Console.
   - Select the two Nagios instances (Nagios server and Nagios host Linux).
   - Terminate them one after the other.

3. **Complete the Process:**

   - Ensure that both instances are terminated successfully.

And that’s it! You have now completed setting up and monitoring your Linux server with Nagios, and you’ve properly terminated the instances.

