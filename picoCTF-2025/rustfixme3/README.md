# Rust fixme 3

```rust
-    // unsafe {
+    unsafe {
         // Decrypt the flag operations 
         let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
 
@@ -30,8 +30,8 @@ fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
         // Unsafe operation: calling an unsafe function that dereferences a raw pointer
         let decrypted_slice = std::slice::from_raw_parts(decrypted_ptr, decrypted_len);
 
-        borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
-    // }
+       borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
+    }
     println!("{}", borrowed_string);
 }
```

```
$ cargo run
Using memory unsafe languages is a: PARTY FOUL! Here is your flag: picoCTF{n0w_y0uv3_f1x3d_1h3m_411}
```
