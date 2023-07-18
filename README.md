# PDO
Allrounder Tips for PDO, kept quite basic tho

### Before we start
I will be using a simple setup for front-/backend.<br>
If you got your own configuration skip this part, otherwise have a look at [my setup](https://github.com/SenselessCoding/setup_front-end).<br>

---

### Connect

``w3schools`` got great [documentation](https://www.w3schools.com/php/default.asp) on this topic.<br>
``php`` got [own docs](https://www.php.net/manual/en/), but I do recommend beginers to stick with *w3schools*.<br>
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

---

### Cross file connects
``w3schools`` also explains this topic quite well.


```php
<?php
session_start();
require_once 'connect.php';
?>
```

---

### Query
Great, we should be set now.<br>
With plain PHP we can create a somewhat structured table to display query results.<br>
[Here]() I tried my best to make it a bit prettier, less readable for newbies however.<br>

```php
<table>
    <thead>
      <tr>
        <th>Order ID</th>
        <th>Order Number</th>
        <th>Person ID</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <?php
          $stmt = $pdo->query("SELECT * FROM orders");
          $result = $stmt->fetchAll(PDO::FETCH_ASSOC);
          foreach ($result as $row) {
              echo "<tr>";
              echo "<td> <input type='text' id='orderID' name='orderID' placeholder=" . $row['OrderID'] . " /></td>";
              echo "<td> <input type='text' id='orderNumber' name='orderNumber' placeholder=" . $row['OrderNumber'] . " /></td>";
              echo "<td> <input type='text' id='personID' name='personID' placeholder=" . $row['PersonID'] . " /></td>";
              echo "</tr>";
          }
      ?>
    </tbody>
  </table>
```

---

### Insert
Magnificent! We can read, now we want to write.<br>
For this we will be following ``w3schools`` on their tutorial for [forms](https://www.w3schools.com/php/php_forms.asp).<br>

```php
<?php
session_start();
?>

<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
 <h1>HELLO WORLD</h1>

  <!-- Form for inserting data -->
  <form action="insert.php" method="POST">
    <labelfor="orderID">Order ID:</label>
    <input type="text" id="orderID" name="orderID" placeholder="Enter order ID">

    <label for="orderNumber">Order Number:</label>
    <input type="text" id="orderNumber" name="orderNumber" placeholder="Enter order number">

    <label for="personID">Person ID:</label>
    <input type="text" id="personID" name="personID" placeholder="Enter person ID">

    <button type="submit">Insert</button>
  </form>
</body>
</html>
```
We want to use those variables in a new file so our ``insert.php`` can actually do something, this is why we chose to use sessions<br>

```php
<?php
session_start();
require_once 'connect.php';

<!-- get sent data -->
    $orderID = $_POST['orderID'];
    $orderNumber = $_POST['orderNumber'];
    $personID = $_POST['personID'];
//  md5($personID = $_POST['personID']); // this might be useful if you need to hash smt

<!-- insert data -->
    $stmt = $pdo->prepare("INSERT INTO orders (OrderID, OrderNumber, PersonID) VALUES (?, ?, ?)");
    $stmt->execute([$orderID, $orderNumber, $personID]);
 
    header('Location: index.php');
    exit();
?>
```

As mentioned before ``w3schools`` covers all of this in more depth.<br>
 
---

### Delete
Typo in your insert? It's time to add a delete method!<br>
Just before your closing tag of the ``table row`` in your table add following:
```php
              echo "<td>
                      <form action='delete.php' method='POST' style='display:inline-block;'>
                          <input type='hidden' name='orderID' value='" . $row['OrderID'] . "'>
                          <button type='submit'>Delete</button>
                      </form>
                    </td>";
```

Similiar to the query we build our delete statement:

```php
<?php
session_start();
require_once 'connect.php';

    $orderID = $_POST['orderID'];

    $stmt = $pdo->prepare("DELETE FROM orders WHERE OrderID = ?");
    $stmt->execute([$orderID]);
   
    header('Location: index.php');
    exit();
?>
```

---

### Update

Once I'm done with my current project, I will add an update tutorial aswell, stay tuned!<br>
