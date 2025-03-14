# Challenge Statement 
![image](https://github.com/user-attachments/assets/9d2648bb-ec91-41d3-9c13-0de5dd5a8900)

# Solution
We are provided with a netcat server which shows a system which resembles the hacking terminal from **Fallout: New Vegas**. In the game, the terminals use a “likeness” system, where each incorrect guess indicates the number of letters that are in the correct positions. <br />
Using this information, we have to find the correct password.

![image](https://github.com/user-attachments/assets/55c62430-1fdb-4640-875b-ed64416c75dc)

I start with guessing `MOUNTAIN` and get a likeness of 0/8. This means that none of the letters are in the correct spot. <br />
Now, the only possible correct passwords are `PRODUCER` and `AUTONOMY`. I tried `PRODUCER` and got the flag. 

![image](https://github.com/user-attachments/assets/4893856a-4735-4298-bc92-b0eabff45b7f)

