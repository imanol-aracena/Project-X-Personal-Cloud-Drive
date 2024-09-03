# Project X: Personal Cloud Drive

**Project X** is a live website designed to function like a personal cloud drive. It allows users to log in from both their phone and PC to connect to a server PC at home. The website provides features such as image uploads, viewing images, and scrolling through them, with a user-friendly navigation system.

## Features

- **Login Page**: Secure access for authenticated users.
- **Image Upload**: Users can upload images to their personal cloud space.
- **Image Gallery**: View and scroll through uploaded images.
- **Responsive Design**: Compatible with both mobile and desktop devices.
- **Navigation Controls**: Easy-to-use back button on all pages (except the login page).

## Requirements

- A web server (Apache, Nginx, etc.) with PHP or another server-side scripting language.
- A database (MariaDB, MySQL, etc.) to store user data and image metadata.
- Arch Linux or a similar Linux distribution for the server environment.
- A secure server configuration, avoiding default ports.

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/userlaws/Project-X-Personal-Cloud-Drive
cd Project-X
```

### 2. Set Up the Web Server

Configure your web server to serve the files from the `Project-X` directory. For example, with Apache, create a new virtual host:

```apache
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /path/to/Project-X
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

### 3. Configure the Database

- Install and start MariaDB or MySQL:

    ```bash
    sudo pacman -S mariadb
    sudo systemctl start mariadb
    sudo mysql_secure_installation
    ```

- Create a new database and user:

    ```sql
    CREATE DATABASE project_x;
    CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON project_x.* TO 'username'@'localhost';
    FLUSH PRIVILEGES;
    ```

- Import the initial database schema from the `schema.sql` file in the repository:

    ```bash
    mysql -u username -p project_x < schema.sql
    ```

### 4. Configure the Application

- Open the configuration file (`config.php` or similar) and set your database credentials:

    ```php
    <?php
    define('DB_SERVER', 'localhost');
    define('DB_USERNAME', 'username');
    define('DB_PASSWORD', 'password');
    define('DB_DATABASE', 'project_x');
    ?>
    ```

### 5. Secure Your Server

- **Enable HTTPS**: Obtain and install an SSL certificate (e.g., via Let's Encrypt).
- **Firewall Configuration**: Only open necessary ports.
- **Regular Updates**: Keep your server updated to protect against vulnerabilities.

### 6. Run the Website

- Start your web server and navigate to your server's IP address or domain name in a web browser.

## Usage

1. **Login**: Enter your credentials on the login page.
2. **Upload Images**: Navigate to the upload page to add images to your personal cloud.
3. **View Images**: Access the gallery to view and scroll through uploaded images.
4. **Navigate**: Use the back button to return to previous pages.

## Example Code

Here is a simplified example of the login script (`login.php`):

```php
<?php
session_start();
include('config.php');

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST['username'];
    $password = $_POST['password'];

    $conn = new mysqli(DB_SERVER, DB_USERNAME, DB_PASSWORD, DB_DATABASE);
    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }

    $sql = "SELECT id FROM users WHERE username = '$username' and password = '$password'";
    $result = $conn->query($sql);
    if ($result->num_rows == 1) {
        $_SESSION['login_user'] = $username;
        header("location: welcome.php");
    } else {
        echo "Your Login Name or Password is invalid";
    }
    $conn->close();
}
?>
```

## Future Features

- **File Deletion**: Enable users to delete uploaded files.
- **Support for Larger Files**: Expand the upload feature to support larger files, such as videos.
- **Remote Access**: Ensure the website is accessible from any location.

## Contributing

Contributions are welcome! Feel free to submit a pull request or open an issue to suggest improvements or report bugs.
