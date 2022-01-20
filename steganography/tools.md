# Tools

Check for any embedded strings of text within an image

```
strings badimage.jpg
```

See if there is any other binaries embedded within an image

```
binwalk badimage.jpg
```

Check to see if there is a program set to run within the image (can use blank passphrase)

```
steghide extract -sf badimage.jpg
```

