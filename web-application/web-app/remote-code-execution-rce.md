# Remote Code Execution (RCE)

If you need to quickly make RCE code from bash disguised as an image for an LFI/malicious upload.

```
echo -n -e '\xFF\xD8\xFF\xE0.' > shell.jpg
echo -n -e '\x89\x50\x4E\x47.' > shell.png
```
