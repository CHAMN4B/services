# Installation Guide for PHP and Apache HTTP Server on Windows

This guide explains how to install PHP and Apache HTTP Server (`httpd`) on Windows.

---

## Prerequisites

1. **Visual C++ Redistributable**
   - Download and install the appropriate version for your PHP version:
     - PHP 5.6: [Visual C++ Redistributable for Visual Studio 2012](https://www.microsoft.com/en-us/download/details.aspx?id=30679)
     - PHP 7.0-7.1: [Visual C++ Redistributable for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48145)
     - PHP 7.2-7.4: [Visual C++ Redistributable for Visual Studio 2017](https://aka.ms/vs/16/release/vc_redist.x64.exe)
     - PHP 8.x: [Visual C++ Redistributable for Visual Studio 2019 or 2022](https://aka.ms/vs/17/release/vc_redist.x64.exe)

2. **Ensure your system meets the requirements**:
   - Windows 7 or newer.

---

## 1. Install Apache HTTP Server (`httpd`)

1. **Download Apache**:
   - Visit [Apache Lounge Downloads](https://www.apachelounge.com/download/) or download directly:
     - [Apache 2.4 VC15 (Win64)](https://www.apachelounge.com/download/VS16/binaries/httpd-2.4.57-win64-VS16.zip).

2. **Extract Apache**:
   - Extract the ZIP file to `c:/services` or another location.
3. **Configure Apache**:
   - Open the `httpd.conf` file located in `c:/services/httpd/conf` and update the following lines:
     ```apache
        Define SRVROOT "c:/services/httpd"
        Define DIRPHP "c:/services/php"
        Define DIRROOT "c:/www"
     ```

4. **Start Apache**:
   - Open Command Prompt as Administrator.
   - Navigate to the Apache `bin` directory:
     ```cmd
     cd C:\Apache24\bin
     ```
   - Start the Apache server:
     ```cmd
     httpd
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
   - Extract the ZIP file to `C:\php` or another location.

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
   - Add `C:\php` to the system `Path` environment variable.

5. **Verify PHP**:
   - Open Command Prompt and type:
     ```cmd
     php -v
     ```

---

## 3. Integrate PHP with Apache HTTP Server

1. **Configure Apache to Use PHP**:
   - Open the `httpd.conf` file in `C:\Apache24\conf`.
   - Add the following lines at the end of the file:
     ```apache
     LoadModule php_module "C:/php/php8apache2_4.dll"
     AddHandler application/x-httpd-php .php
     PHPIniDir "C:/php"
     ```

2. **Restart Apache**:
   - Restart Apache to apply the changes:
     ```cmd
     httpd -k restart
     ```

3. **Test PHP with Apache**:
   - Create a `phpinfo.php` file in the `htdocs` directory (`C:\Apache24\htdocs`) with the following content:
     ```php
     <?php
     phpinfo();
     ?>
     ```
   - Open your browser and navigate to `http://localhost/phpinfo.php`. You should see the PHP information page.

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
    extension_dir = "C:/php/ext"
    ```

---

You now have PHP and Apache installed and configured on your Windows system. Happy coding! 🎉