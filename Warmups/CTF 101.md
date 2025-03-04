# Challenge Statement 
![image](https://github.com/user-attachments/assets/5baa9328-b6fb-4c21-adbf-63dfdf06c446)

Attachment: [CTF101.zip](https://github.com/harishkannan05/SnykCTF-2024-Writeup/blob/main/Attachments/CTF101.zip)

# Solution
By analyzing the source code, we can see that the application is vulnerable to command injection. <br />
The `name` input is directly passed into a shell command. 

![image](https://github.com/user-attachments/assets/1695c2ba-d0a9-403b-9a01-eb46b196fbad)

To exploit this, we can use this basic payload to read the flag:
``` 
;cat flag.txt
```

![image](https://github.com/user-attachments/assets/2e842314-0507-4a8e-b60f-1414b72a3fd8)
