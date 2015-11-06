# TransAjax is an easy-to-use one line usage library to add expert AJAX features to your site. #

## Features: ##
  * 2 functions (ajax + ajax\_submit), where You can use callback or Dom-element as receiver
  * More stable crossbrowser work
  * Full featured TransAjax.exec( objArgs ) method
  * Result type auto-detect (Text, XML, JSON, PHP-serialized object)
  * Submit forms via AJAX
  * Submit files via AJAX (iframe method)
  * Crossbrowser functionality
  * Build-in JS urlencode()
  * TransAjax.setInnerHtml( objID, str ) to fix scripts in response body
  * Global events TransAjax.onStart, TransAjax.onStop, TransAjax.onError
  * PHP JSON helper function as part of library: ajax( $data `[`, $data\_charset`]`);
## License ##
You must use it AS IS.
If You want more:
  * please contact me **sshuchkin@ sibvision.ru**

Thank U for direct link to project page `<a href="http://code.google.com/p/transajax/">TransAjax features</a>` on your website

I want populate this tool and have karma bonus...
## Usage: ##
```
<!-- news.php -->
<?php
  if (isset($_GET['ajax'])) {
    echo '<h1>News</h1>';
    for ($i = 1; $i < 10; $i++) echo '<h2>News '.$i.'</h2>';
    exit();
  }
?>
<html>
<head>
  <script type="text/javascript" src="transajax.js"></script>
</head>
<body>

  <div id="divNews">
    <a href="#" onclick="ajax('news.php?ajax=1', 'divNews'); return false">Get News</a>
  </div>
</body>
</html>
```

## Snippets: ##
### Basic ###
```

<!-- user.php -->
<?php
  if (isset($_GET['ajax'])) {
    sleep(2); // ajax emulator
    if ($_GET['id'] == 5)
      die('<h1>Great Sergey profile</h1>');
    else
      die('<span style="color: red">Unknown user</span>');
  }
?>
<html>
<head>
  <script type="text/javascript" src="transajax.js"></script>
</head>
<body>
<a href="#" onclick="view_profile_ajax(5); return false">View Sergey Profile</a><br/>
<a href="#" onclick="view_profile_ajax(6); return false">View Ivan Profile</a>
<div id="progress" style="display: none">Loading...</div>
<div id="profile"></div>

<script type="text/javascript">
<!--
function view_profile_ajax( id ) {
  ajax('user.php?ajax=1&id='+id, _view_profile_ajax);
  document.getElementById('progress').style.display = '';
}
function _view_profile_ajax( obj ) {
//  TransAjax.setInnerHTML('profile', obj.responseText); // if we've scripts in response body
   document.getElementById('profile').innerHTML = obj.responseText; // simple
   document.getElementById('progress').style.display = 'none';
}
-->
</script>
</body>
</html>

```
### More callbacks ###

```

<!-- index.php-->
<html>
<head>
<script src="transajax.js"></script>
</head>
<body>
<b>MyMovie</b> rating = <span id="rating4"><input type="button" onclick="rating_ajax(4)" value=" + " /></span>

<script>
<!--
function rating_ajax( id ) {
  ajax('rating.php?id='+id, _rating_ajax);
}
function _rating_ajax( obj ) {
  document.getElementById('rating'+obj.id).innerHtml = obj.rating; 
}
</script>
</body>
</html>

```

```

<!-- rating.php -->
<?php

$id = (int) $_GET['id'];
if ($id) {
   // some SQL, extract & calc rating to $data

   die('{success: true, id: '.$id.', rating: "'.$data['rating'].'"}');
}
?>

```

### Form submission via AJAX ###
```

<!-- contact.php -->
<?php
  if (isset($_GET['ajax'])) {
    sleep(2);
    if (empty($_POST['name']) || empty($_POST['email']) || empty($_POST['message']))
      die('{success: false, error: "Name, Email and Message is required"'});
    else
      die('{success: true}');
  }
?>
<html>
<head>
<script type="text/javascript" src="transajax.js"></script>
</head>
<body>
<form id="frmContact" action="contact.php?ajax=1" method="post">
Name:<br/>
 <input type="text" name="name" /><br/>
Email:<br/>
 <input type="text" name="email" />
Message: <br/>
<textarea name="message"></textarea><br/>
<input type="button" value="send" onclick="send_message_ajax();" />
</form>
<script>
<!--
function send_message_ajax() {
  // "action" attribute used
  ajax_submit('frmContact', _send_message_ajax);
}
function _send_message_ajax( obj ) {
  if (obj.success) {
    alert('Message sent.');
  } else {
    alert(obj.error);
  }
}
-->
</script>

```
### Global events ###
```
...
<img id="ajaxBusyImage" src="loading.gif" style="display: none" />
...
<script>
<!--
// TransAjax Busy state
TransAjax.onStart = function() { document.getElementById('ajaxBusyImage').style.display = "" };
TransAjax.onStop = function() { document.getElementById('ajaxBusyImage').style.display = "none" };
-->
</script>
```