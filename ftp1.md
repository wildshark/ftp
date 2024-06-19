Here's a PHP script that uploads a file using FTP based on the configuration provided in JSON format:

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

// File to upload
$file_to_upload = '/path/to/local/file.txt'; // Replace with the path to your local file
$remote_path = '/path/on/ftp/server/file.txt'; // Path where the file will be uploaded on the FTP server

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

// Upload file
if (ftp_put($ftp_conn, $remote_path, $file_to_upload, FTP_BINARY)) {
    echo "File uploaded successfully: $file_to_upload\n";
} else {
    echo "Failed to upload file: $file_to_upload\n";
}

// Close FTP connection
ftp_close($ftp_conn);
?>
```

### Instructions:

1. **Configuration JSON:**
   - Replace the `$config_json` variable with your actual JSON configuration.

2. **File Paths:**
   - Set `$file_to_upload` to the path of the file you want to upload from your local machine.
   - Adjust `$remote_path` to specify the destination path on the FTP server where you want to upload the file.

3. **FTP Functions:**
   - The script uses PHP's FTP functions (`ftp_connect`, `ftp_login`, `ftp_put`) to establish a connection to the FTP server, authenticate using the provided credentials, and upload the file.

4. **Error Handling:**
   - Basic error handling is included (`die()` statements) to handle cases where the FTP connection or login fails.

5. **FTP Mode:**
   - Passive mode (`ftp_pasv($ftp_conn, true);`) is enabled by default. Modify according to your FTP server requirements.

Make sure to replace placeholders (`/path/to/local/file.txt`, `/path/on/ftp/server/file.txt`) with actual paths relevant to your setup. This script assumes you have PHP's FTP extension enabled (`extension=ftp` in `php.ini`).
