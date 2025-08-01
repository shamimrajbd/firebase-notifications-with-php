<p align="center">
<p align="center">
  <a href="https://github.com/i-rin-eam">
    <img src="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/firebase-notification-image.png?raw=true"Logo" width="350" height="200">
  </a> 
<h2 align='center'>Send Firebase Push Notifications from Server using the new FCM HTTP v1 API</h12>
</p>

## Step 1: Here is `firebase.php` code.
```php
<?php

// Include the get-access-token.php
require 'get-access-token.php';

// Path to your service account key file
$serviceAccountKeyFile = 'service-account-file.json';

// Obtain the OAuth 2.0 Bearer Token
$accessToken = getAccessToken($serviceAccountKeyFile);

// FCM message details
$token = 'your_target_device_token';
$title = "hey whatsapp?";
$body = "I am fine, be well........";

$url = "https://fcm.googleapis.com/v1/projects/{project-id}/messages:send";

// Prepare FCM message data
$datamsg = array(
    'title' => $title,
    'body' => $body
);
$arrayToSend = array('token' => $token, 'data' => $datamsg);

$json = json_encode(['message' => $arrayToSend]);

// Prepare headers
$headers = array();
$headers[] = 'Content-Type: application/json';
$headers[] = 'Authorization: Bearer ' . $accessToken;

// Initialize curl session
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $json);
curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

// Send the request
$response = curl_exec($ch);

// Check for curl errors
if ($response === FALSE) {
    die('FCM Send Error: ' . curl_error($ch));
}

// Close curl session
curl_close($ch);

?>
```
## Step 2: Here is `get-access-token.php` code.
```php
<?php

// Function to get OAuth 2.0 Bearer Token
function getAccessToken($serviceAccountKeyFile)
{
    $serviceAccount = json_decode(file_get_contents($serviceAccountKeyFile), true);
    $jwt = generateJWT($serviceAccount);
    $url = 'https://oauth2.googleapis.com/token';

    $post = [
        'grant_type' => 'urn:ietf:params:oauth:grant-type:jwt-bearer',
        'assertion' => $jwt,
    ];

    $ch = curl_init($url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $post);
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);

    $response = curl_exec($ch);
    curl_close($ch);

    if (!$response) {
        die('Error obtaining access token');
    }

    $data = json_decode($response, true);

    if (isset($data['access_token'])) {
        return $data['access_token'];
    } else {
        die('Error obtaining access token');
    }
}

// Function to generate JWT
function generateJWT($serviceAccount)
{
    $header = [
        'alg' => 'RS256',
        'typ' => 'JWT',
    ];

    $now = time();
    $exp = $now + 3600; // 1 hour expiration

    $payload = [
        'iss' => $serviceAccount['client_email'],
        'scope' => 'https://www.googleapis.com/auth/firebase.messaging',
        'aud' => 'https://oauth2.googleapis.com/token',
        'iat' => $now,
        'exp' => $exp,
    ];

    $base64UrlHeader = base64UrlEncode(json_encode($header));
    $base64UrlPayload = base64UrlEncode(json_encode($payload));

    $signatureInput = $base64UrlHeader . '.' . $base64UrlPayload;
    $signature = '';
    openssl_sign($signatureInput, $signature, $serviceAccount['private_key'], 'sha256');

    $base64UrlSignature = base64UrlEncode($signature);

    return $base64UrlHeader . '.' . $base64UrlPayload . '.' . $base64UrlSignature;
}

function base64UrlEncode($data)
{
    return str_replace(['+', '/', '='], ['-', '_', ''], base64_encode($data));
}

?>
```

## Step 3: Here go to firebase project overview .


<a href="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/project%20overview.png?raw=true" target="_blank">
    <img src="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/project%20overview.png?raw=true" alt="Logo" width="700" height="500">
  </a> 


  ## Step 4: Here go Service account and generate new private key download .


<a href="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/generate%20private%20key.png?raw=true" target="_blank">
    <img src="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/generate%20private%20key.png?raw=true" alt="Logo" width="700" height="500">
  </a> 


   ## Step 5: Here go to PC File Manager Rename json file .


<a href="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/rename.png?raw=true" target="_blank">
    <img src="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/rename.png?raw=true" alt="Logo" width="700" height="500">
  </a> 

   ## Step 6: Here go to Cpanel and upload json file and other 'firebase.php' , 'get-access-token.php' file .


<a href="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/service-account-file.png?raw=true" target="_blank">
    <img src="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/service-account-file.png?raw=true" alt="Logo" width="700" height="500">
  </a> 


   ## Here Finall Step Notification Sent Successfully
   
<a href="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/Notificaiton%20sent.jpg?raw=true" target="_blank">
    <img src="https://github.com/shamimrajbd/firebase-notifications/blob/main/images/Notificaiton%20sent.jpg?raw=true" alt="Logo" width="700" height="700">
  </a> 


<h1 align="center">Thank You ❤️</h1>
<br><br>
