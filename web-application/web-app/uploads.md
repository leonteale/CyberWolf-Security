---
description: >-
  Upload vulnerabilities occur when an application does not properly validate or
  sanitise user-uploaded files
---

# Uploads

## What is an Upload Vulnerability?

Upload vulnerabilities occur when an application does not properly validate or sanitize user-uploaded files. This can lead to unauthorized code execution, file inclusion, or other security breaches. Attackers may exploit these vulnerabilities to upload malicious files, such as web shells or configuration files, that can compromise the server or application.

### Example Tests for Upload Vulnerabilities

When performing a penetration test, you can use various payloads to test for upload vulnerabilities. Below are some example tests and methodologies to identify such vulnerabilities.

### **Example Payloads and Techniques**

1. **Uploading a Web Shell**
   *   **PHP Web Shell**: Upload a simple PHP file with the following content:

       ```php
       <?php system($_GET['cmd']); ?>
       ```
   *   **ASP Web Shell**: Upload an ASP file with the following content:

       {% code overflow="wrap" %}
       ```aspnet
       <% @Page Language="C#" %>
       <script runat="server">
       void Page_Load(object sender, EventArgs e) {
           if (Request.QueryString["cmd"] != null) {
               System.Diagnostics.Process.Start("cmd.exe", "/c " + Request.QueryString["cmd"]);
           }
       }
       </script>
       ```
       {% endcode %}
2.  **Uploading .htaccess File**

    *   To change the behaviour of the web server, you can upload an `.htaccess` file:

        ```bash
        AddType application/x-httpd-php .jpg
        ```

    This allows you to upload a PHP file disguised with a `.jpg` extension and have it executed as PHP.
3. **Bypassing File Type Restrictions**
   * **Polyglot Files**: Create a file that can be interpreted as both an image and a script. For example, an image with PHP code hidden in comments.
   * **MIME Type Bypass**: Change the MIME type of the file to bypass content-type checks.
4. **Using Alternate Extensions**
   * Upload files with different extensions that might still be interpreted as scripts by the server, such as `.phtml`, `.phar`, or `.shtml`.
5. **Configuration Files**
   * **Apache Configuration (.htaccess)**: Upload an `.htaccess` file to enable execution of other uploaded files.
   * **Tomcat Configuration**: Modify the `web.xml` file to include malicious servlet mappings.

## Testing Methodology

1. **Identify Upload Points**:
   * Locate file upload functionalities in the application, such as profile picture uploads, document uploads, etc.
2. **Attempt File Uploads**:
   * Use various payloads to attempt uploading different file types and observe the server's response.
   * Monitor the server's directory structure to see where files are stored and if they are accessible.
3. **Analyze Responses**:
   * Check if the uploaded files are executable by accessing them directly via a web browser.
   * Use intercepting proxies like Burp Suite or OWASP ZAP to manipulate file uploads and bypass client-side restrictions.
4. **Monitor Server Behavior**:
   * Observe any changes in server behaviour, error messages, or execution of uploaded files.

### References

* [PayloadsAllTheThings - Upload Insecure Files](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/README.md)
* [PayloadsAllTheThings - Jetty RCE](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Jetty%20RCE/JettyShell.xml)
* [PayloadsAllTheThings - Extension ASP](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20ASP/shell.aspx)

### Example bypass upload restrictions.&#x20;

<figure><img src="../../.gitbook/assets/1669794207400.jfif" alt=""><figcaption></figcaption></figure>

#### Conclusion

Upload vulnerabilities can lead to severe security breaches if not properly mitigated. Always ensure user inputs are sanitised and validated before processing. Implement proper file type checks, size limits, and use safe directories for storing uploaded files. Regular security testing and updates to the application code can help prevent such vulnerabilities.
