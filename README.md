# Installation Guide for PHP and Apache HTTP Server on Windows

This guide explains how to install PHP and Apache HTTP Server (`httpd`) on Windows.

---

## Prerequisites
## 1. Install Apache HTTP Server

1. **Visual C++ Redistributable**
   - Download and install the repository 
   - Extract the ZIP file to `c:\services` or another location.
2. **Configure Apache**:
   - Open the `httpd.conf` file located in `c:\services\httpd\conf` and update the following lines:
     ```apache
        Define SRVROOT "c:/services/httpd"
        Define DIRPHP "c:/services/php"
        Define DIRROOT "c:/www"
     ```

4. **Install & Start Apache**:
   - Open Command Prompt as Administrator.
   - Navigate to the Apache `bin` directory:
     ```cmd
     cd C:/services/httpd/bin
     ```
   - Install the Apache server:
     ```cmd
     httpd.exe -k install
     ```
   - Start the Apache server:
     ```cmd
     httpd -k start
     ```

---

## 2. Install PHP

1. **Download PHP**:
   - Choose your desired PHP version:

    | PHP Version | Download Link |
    |-------------|---------------|
    | PHP 5.6.40  | [Download](https://windows.php.net/downloads/releases/archives/php-5.6.40-Win32-VC11-x64.zip) |
    | PHP 7.0.33  | [Download](https://windows.php.net/downloads/releases/archives/php-7.0.33-Win32-VC14-x64.zip) |
    | PHP 7.1.33  | [Download](https://windows.php.net/downloads/releases/archives/php-7.1.33-Win32-VC14-x64.zip) |
    | PHP 7.2.34  | [Download](https://windows.php.net/downloads/releases/archives/php-7.2.34-Win32-VC15-x64.zip) |
    | PHP 7.3.29  | [Download](https://windows.php.net/downloads/releases/archives/php-7.3.29-Win32-VC15-x64.zip) |
    | PHP 7.4.33  | [Download](https://windows.php.net/downloads/releases/archives/php-7.4.33-Win32-vc15-x64.zip) |
    | PHP 8.0.30  | [Download](https://windows.php.net/downloads/releases/archives/php-8.0.30-Win32-vs16-x64.zip) |
    | PHP 8.1.29  | [Download](https://windows.php.net/downloads/releases/archives/php-8.1.29-Win32-vs16-x64.zip) |
    | PHP 8.2.22  | [Download](https://windows.php.net/downloads/releases/archives/php-8.2.22-Win32-vs16-x64.zip) |
    | PHP 8.3.10  | [Download](https://windows.php.net/downloads/releases/archives/php-8.3.10-Win32-vs16-x64.zip) |

    ---

2. **Extract PHP**:
   - Extract the ZIP file to `C:/services/php/5.6 .....` or another location.
3. **Configure PHP**:
   - Rename `php.ini-development` or `php.ini-production` to `php.ini`.
   - Open `php.ini` in a text editor and:
     - Enable extensions:
       ```ini
       extension=mbstring
       extension=curl
       extension=gd2
       ```
     - Set the timezone:
       ```ini
       date.timezone = "America/New_York"
       ```

4. **Add PHP to System Path**:
   - Add `c:/services/php/5.6` to the system `Path` environment variable.

5. **Verify PHP**:
   - Open Command Prompt and type:
     ```cmd
     php -v
     ```

---

## 3. Integrate PHP with Apache HTTP Server

1. **Configure Apache to Use PHP**:
   - Open the `httpd-vhosts.conf` file in `c:/services/httpd/conf/extra/httpd-vhosts.conf`.
    Using the PHP version you downloaded fcgi enabled & vhost-host configured
    add the following configuration:
    ```cmd
    Listen 9081
    <VirtualHost *:9081>
        ServerName localhost:9081
        FcgidInitialEnv PATH "${DIRPHP}/8.1;C:/WINDOWS/system32;C:/WINDOWS;C:/WINDOWS/System32/Wbem;"
        FcgidInitialEnv SystemRoot "C:/Windows"
        FcgidInitialEnv SystemDrive "C:"
        FcgidInitialEnv TEMP "C:/WINDOWS/Temp"
        FcgidInitialEnv TMP "C:/WINDOWS/Temp"
        FcgidInitialEnv windir "C:/WINDOWS"
        FcgidIOTimeout 64
        FcgidConnectTimeout 16
        FcgidMaxRequestsPerProcess 1000
        FcgidMaxRequestLen 8131072
        # Location php.ini:
        FcgidInitialEnv PHPRC "${DIRPHP}/8.1"
        FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 1000
        DirectoryIndex index.php
        FcgidInitialEnv SystemRoot "C:/Windows"
        FcgidInitialEnv SystemDrive "C:"

        <Files ~ "\.php$>"
            AddHandler fcgid-script .php
            FcgidWrapper "${DIRPHP}/8.1/php-cgi.exe" .php
        </Files>

        DocumentRoot "${DIRROOT}"
        <Directory "${DIRROOT}">
            Require all granted
            Options Indexes FollowSymLinks Includes ExecCGI
            AllowOverride All
        </Directory>
    </VirtualHost>
    ```

2. **Restart Apache**:
   - Restart Apache to apply the changes:
     ```cmd
     httpd -k restart
     ```

3. **Test PHP with Apache**:
   - Create a `phpinfo.php` file in the `htdocs` directory (`c:/www`) with the following content:
     ```php
     <?php
     phpinfo();
     ?>
     ```
   - Open your browser and navigate to `http://localhost:9056/phpinfo.php`. You should see the PHP information page. PHP 5.6

---

## 4. Common Issues and Solutions

- **Apache won’t start**:
  - Check the `httpd.conf` file for syntax errors:
    ```cmd
    httpd -t
    ```
  - Ensure no other application (e.g., Skype) is using port 80.

- **PHP extensions not working**:
  - Verify the `extension_dir` setting in `php.ini` matches your PHP installation:
    ```ini
    extension_dir = "c:/services/php/5.6/ext"
    ```

---

You now have PHP and Apache installed and configured on your Windows system. Happy coding! 🎉