# Rust fixme 1

```rust
 fn main() {
     // Key for decryption
-    let key = String::from("CSUCKS") // How do we end statements in Rust?
+    let key = String::from("CSUCKS"); // How do we end statements in Rust?
 
     // Encrypted flag values
     let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "7f", "5a", "60", "50", "11", "38", "1f", "3a", "60", "e9", "62", "20", "0c", "e6", "50", "d3", "35"];
@@ -15,14 +15,14 @@ fn main() {
     // Create decrpytion object
     let res = XORCryptor::new(&key);
     if res.is_err() {
-        ret; // How do we return in rust?
+        return; // How do we return in rust?
     }
     let xrc = res.unwrap();
 
     // Decrypt flag and print it out
     let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
     println!(
-        ":?", // How do we print out a variable in the println function? 
+        "{}", // How do we print out a variable in the println function? 
         String::from_utf8_lossy(&decrypted_buffer)
     );
 }
```

```
$ cargo run
picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}
```
