# Challenge Statement 
![image](https://github.com/user-attachments/assets/d5141344-31ed-4cbf-9db8-019e8101b5f8)

Attachment: [TimeOff.zip](https://github.com/harishkannan05/SnykCTF-2024-Writeup/blob/main/Attachments/TimeOff.zip)

# Solution
Going through the source code, we find that it is vulnerable to path traversal, as the @document.name is used to construct the file path without proper validation.
```
def download
  @document = Document.find(params[:id])
  base_directory = Rails.root.join("public", "uploads")
  path_to_file = File.join(base_directory, @document.name)

  if File.exist?(path_to_file)
    send_file path_to_file,
            filename: @document.file_path.presence || "document",
            type: "application/octet-stream"
  else
    flash[:alert] = "File not found: #{path_to_file}"
    redirect_back(fallback_location: root_path)
  end
end
```

By trial and error I found the correct path, which then lets us download flag.txt.
``` ../../../flag.txt ```
