To list all files from an FTP server using PHP, you can modify the previous script to include functionality for retrieving a directory listing. Here's how you can do it:

```php
<?php
// Configuration JSON
$config_json = '{
  "is_enabled": true,
  "host": "281.293.221.435",
  "port": "423843",
  "user": "acc_623629",
  "password": "74gsgakS@su312isasgsaas"
}';

// Decode JSON to array
$config = json_decode($config_json, true);

// FTP server credentials
$ftp_host = $config['host'];
$ftp_port = $config['port'];
$ftp_user = $config['user'];
$ftp_pass = $config['password'];

// Connect to FTP server
$ftp_conn = ftp_connect($ftp_host, $ftp_port);
if (!$ftp_conn) {
    die("Failed to connect to FTP server: $ftp_host");
}

// Login to FTP
$ftp_login = ftp_login($ftp_conn, $ftp_user, $ftp_pass);
if (!$ftp_login) {
    die("FTP login failed for user: $ftp_user");
}

// Passive mode (optional, enable if needed)
ftp_pasv($ftp_conn, true);

// Get file list
$file_list = ftp_nlist($ftp_conn, ".");

if ($file_list) {
    echo "List of files on FTP server:\n";
    foreach ($file_list as $file) {
        echo "$file\n";
    }
} else {
    echo "Failed to retrieve file list from FTP server.\n";
}

// Close FTP connection
ftp_close($ftp_conn);
?>
```

### Explanation:

1. **FTP Connection and Login:**
   - The script connects to the FTP server using `ftp_connect` and logs in using `ftp_login`, utilizing credentials fetched from the JSON configuration.

2. **Passive Mode:**
   - Passive mode (`ftp_pasv($ftp_conn, true);`) is enabled to accommodate servers behind firewalls or network configurations that require passive FTP connections.

3. **Listing Files:**
   - `ftp_nlist($ftp_conn, ".")` retrieves a list of files in the current directory (`.`). You can specify a different directory path if needed.

4. **Displaying File List:**
   - If the file list is retrieved successfully (`$file_list` is not empty), it iterates through each file and prints its name.
   - If the file list retrieval fails, it prints an error message.

5. **Closing the FTP Connection:**
   - `ftp_close($ftp_conn);` closes the FTP connection after the operation is complete.

### Notes:

- Make sure the PHP environment has the FTP extension enabled (`extension=ftp` in `php.ini`).
- Replace placeholders in the configuration JSON with your actual FTP server credentials and adjust paths as necessary.

This script provides a basic implementation to connect to an FTP server, authenticate, retrieve a list of files, and display them. Adjustments can be made to fit specific requirements or to handle more complex scenarios such as handling directories recursively or sorting the file list.
