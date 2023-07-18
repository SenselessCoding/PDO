# PDO
Allrounder Tips for PDO

### Connect

``w3schools`` got great documentation on this topic.<br>
In general, this should do for the most basic sites:
```php
<?php
$servername = "localhost";
$username = "username";
$password = "password";

// Create connection
$conn = new mysqli($servername, $username, $password);

// Check connection
if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}
echo "Connected successfully";
?>
```

