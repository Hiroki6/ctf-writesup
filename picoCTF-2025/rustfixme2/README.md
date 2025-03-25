# Rust fixme 2

```rust
-fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &String){ // How do we pass values to a function that we want to change?
+fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &mut String){ // How do we pass values to a function that we want to change?
 
     // Key for decryption
     let key = String::from("CSUCKS");
@@ -31,6 +31,6 @@ fn main() {
         .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
         .collect();
 
-    let party_foul = String::from("Using memory unsafe languages is a: "); // Is this variable changeable?
-    decrypt(encrypted_buffer, &party_foul); // Is this the correct way to pass a value to a function so that it can be changed?
+    let mut party_foul = String::from("Using memory unsafe languages is a: "); // Is this variable changeable?
+    decrypt(encrypted_buffer, &mut party_foul); // Is this the correct way to pass a value to a function so that it can be changed?
 }
```

```
$ cargo run
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{4r3_y0u_h4v1n5_fun_y31?}
```
