# Challenge Statement 
![image](https://github.com/user-attachments/assets/c7254cd6-7b17-458a-873c-465a0cf760a6)

Attachment: [Weblog.zip](https://github.com/harishkannan05/SnykCTF-2024-Writeup/blob/main/Attachments/Weblog.zip)

# Solution
After analyzing the source code, we find that there are multiple vulnerabilities. 
1) SQL Injection in the search functionality.
2) MD5 hashing
3) Command Injection in Admin Panel

**SQL Injection**
```
raw_query = text(
                f"SELECT * FROM blog_posts WHERE title LIKE '%{query}%'")
            current_app.logger.info(f"Executing Raw Query: {raw_query}")
            posts = db.session.execute(raw_query).fetchall()
            current_app.logger.info(f"Query Results: {posts}")
```

Because user input `query` is directly appended to the SQL query without proper sanitization, this vulnerability can be exploited to extract data from the database.
A simple ` ' OR 1-1 # ` does the trick. 

We can extract data from the `users` table using: 
``` ' UNION SELECT id, username, password, 'content', 'author' FROM users WHERE role='admin' # ```

This gives us the md5 hash of the admin password, which we can crack using hashcat.

Now, we can log into the admin panel and find the flag, courtesy of the command injection vulnerability. 
![image](https://github.com/user-attachments/assets/5938603c-94c9-4ea2-8b1f-c5f460c4bf39)
