# bobabo
<?php

  function request($url, $headers, $put = null) {
 	$ch = curl_init();
 	curl_setopt($ch, CURLOPT_URL, $url);
 	if($put):
 	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
 	endif;
 	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
 	if($headers):
     curl_setopt($ch, CURLOPT_HEADER, false);
 	curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
 	endif;
 	curl_setopt($ch, CURLOPT_ENCODING, "GZIP");
 	return curl_exec($ch);
 }

  function regis($email, $nomor, $id) {
 $url = "https://m.borongbareng.com/api-gateway/service-marketing/app/bargain/help/$id";
 //$data = '[{"operationName":"OTPRequest","variables":{"email":"'.$email.'","otpType":"126","mode":"email","otpDigit":4},"query":"query OTPRequest($otpType: String!, $mode: String, $msisdn: String, $email: String, $otpDigit: Int) {\n  OTPRequest(otpType: $otpType, mode: $mode, msisdn: $msisdn, email: $email, otpDigit: $otpDigit) {\n    success\n    message\n    errorMessage\n    __typename\n  }\n}\n"}]';
 $headers = array();
 $headers [] = "Host: m.borongbareng.com";
 $headers [] = "Connection: close";
 $headers [] = "Accept: application/json, text/plain, */*";
 $headers [] = "language: Indonesian";
 $headers [] = "Authorization: $email";
 $headers [] = "User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Mobile Safari/537.36";
 $headers [] = "Sec-Fetch-Site: same-origin";
 $headers [] = "Sec-Fetch-Mode: cors";
 $headers [] = "Sec-Fetch-Dest: empty";
 $headers [] = "Referer: https://m.borongbareng.com/";
 $headers [] = "Accept-Encoding: gzip, deflate";
 $headers [] = "Accept-Language: en-US,en;q=0.9";
 $headers [] = "Cookie: _ga=GA1.2.513239636.1592924479; _gid=GA1.2.728015094.1593092400; _gat_gtag_UA_164611555_2=1";

  $getotp = request($url, $headers);
 $json = json_decode($getotp, true);
 $a = $json['message'];
 if (strpos($a, 'memiliki') !== false) {
     echo "$nomor --> Tidak berhasil, pesanan ini sudah terslash\n";
 } else {
 	echo "$nomor --> Berhasil slash\n";
 }
 }

  function getno($id) {
 $url = "https://m.borongbareng.com/api-gateway/service-marketing/app/bargain/self/detail/$id";
 //$data = '[{"operationName":"OTPRequest","variables":{"email":"'.$email.'","otpType":"126","mode":"email","otpDigit":4},"query":"query OTPRequest($otpType: String!, $mode: String, $msisdn: String, $email: String, $otpDigit: Int) {\n  OTPRequest(otpType: $otpType, mode: $mode, msisdn: $msisdn, email: $email, otpDigit: $otpDigit) {\n    success\n    message\n    errorMessage\n    __typename\n  }\n}\n"}]';
 $headers = array();
 $headers [] = "Host: m.borongbareng.com";
 $headers [] = "Connection: close";
 $headers [] = "Accept: application/json, text/plain, */*";
 $headers [] = "language: Indonesian";
 $headers [] = "Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMyMTMwNjQsImlhdCI6MTU5MzEyNjY2NCwidXNlciI6IntcImlkXCI6NTA3OTMsXCJuaWNrbmFtZVwiOlwiODkqKio2OTk1NjQ1XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTg5NTM0Njk5NTY0NVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.BZdhJM-k37AKs8XeiZ_mhN0xMPqZo9CL3omeaGuRfkM";
 $headers [] = "User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Mobile Safari/537.36";
 $headers [] = "Sec-Fetch-Site: same-origin";
 $headers [] = "Sec-Fetch-Mode: cors";
 $headers [] = "Sec-Fetch-Dest: empty";
 $headers [] = "Referer: https://m.borongbareng.com/";
 $headers [] = "Accept-Encoding: gzip, deflate";
 $headers [] = "Accept-Language: en-US,en;q=0.9";
 $headers [] = "Cookie: _ga=GA1.2.513239636.1592924479; _gid=GA1.2.728015094.1593092400; _gat_gtag_UA_164611555_2=1";

  $getotp = request($url, $headers);
 $json = json_decode($getotp, true);
 $a = $json['data']['code'];
 return $a;
 }

  echo "=========================\n";
 echo "Borong Barang Slash\n";
 echo "Created by @hardiansyah88\n";
 echo "=========================\n";
 echo "Masukan ID: ";
 $id = trim(fgets(STDIN));
 $getno = getno($id);
 //echo "$getno";

  $data=file_get_contents("token1.txt");
 $ex = explode("\n", str_replace("\r", "", $data));
 $count = count($ex);
 for($i=0;$i<$count;$i++) {
 $nomor = 1+ $i;
 regis($ex[$i], $nomor, $getno);
 } 

<?php

function request($url, $headers, $put = null) {
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $url);
	if($put):
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
	endif;
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	if($headers):
    curl_setopt($ch, CURLOPT_HEADER, false);
	curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
	endif;
	curl_setopt($ch, CURLOPT_ENCODING, "GZIP");
	return curl_exec($ch);
}

function regis($email, $nomor, $id) {
$url = "https://m.borongbareng.com/api-gateway/service-marketing/app/bargain/help/$id";
//$data = '[{"operationName":"OTPRequest","variables":{"email":"'.$email.'","otpType":"126","mode":"email","otpDigit":4},"query":"query OTPRequest($otpType: String!, $mode: String, $msisdn: String, $email: String, $otpDigit: Int) {\n  OTPRequest(otpType: $otpType, mode: $mode, msisdn: $msisdn, email: $email, otpDigit: $otpDigit) {\n    success\n    message\n    errorMessage\n    __typename\n  }\n}\n"}]';
$headers = array();
$headers [] = "Host: m.borongbareng.com";
$headers [] = "Connection: close";
$headers [] = "Accept: application/json, text/plain, */*";
$headers [] = "language: Indonesian";
$headers [] = "Authorization: $email";
$headers [] = "User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Mobile Safari/537.36";
$headers [] = "Sec-Fetch-Site: same-origin";
$headers [] = "Sec-Fetch-Mode: cors";
$headers [] = "Sec-Fetch-Dest: empty";
$headers [] = "Referer: https://m.borongbareng.com/";
$headers [] = "Accept-Encoding: gzip, deflate";
$headers [] = "Accept-Language: en-US,en;q=0.9";
$headers [] = "Cookie: _ga=GA1.2.513239636.1592924479; _gid=GA1.2.728015094.1593092400; _gat_gtag_UA_164611555_2=1";

$getotp = request($url, $headers);
$json = json_decode($getotp, true);
$a = $json['message'];

if ($json['code'] == 0) {
	echo "$nomor --> berhasil slash\n";
} else {
	echo "$nomor --> $a";
	echo "\n";
}
}

function getno($id) {
$url = "https://m.borongbareng.com/api-gateway/service-marketing/app/bargain/self/detail/$id";
//$data = '[{"operationName":"OTPRequest","variables":{"email":"'.$email.'","otpType":"126","mode":"email","otpDigit":4},"query":"query OTPRequest($otpType: String!, $mode: String, $msisdn: String, $email: String, $otpDigit: Int) {\n  OTPRequest(otpType: $otpType, mode: $mode, msisdn: $msisdn, email: $email, otpDigit: $otpDigit) {\n    success\n    message\n    errorMessage\n    __typename\n  }\n}\n"}]';
$headers = array();
$headers [] = "Host: m.borongbareng.com";
$headers [] = "Connection: close";
$headers [] = "Accept: application/json, text/plain, */*";
$headers [] = "language: Indonesian";
$headers [] = "Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMyMTMwNjQsImlhdCI6MTU5MzEyNjY2NCwidXNlciI6IntcImlkXCI6NTA3OTMsXCJuaWNrbmFtZVwiOlwiODkqKio2OTk1NjQ1XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTg5NTM0Njk5NTY0NVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.BZdhJM-k37AKs8XeiZ_mhN0xMPqZo9CL3omeaGuRfkM";
$headers [] = "User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Mobile Safari/537.36";
$headers [] = "Sec-Fetch-Site: same-origin";
$headers [] = "Sec-Fetch-Mode: cors";
$headers [] = "Sec-Fetch-Dest: empty";
$headers [] = "Referer: https://m.borongbareng.com/";
$headers [] = "Accept-Encoding: gzip, deflate";
$headers [] = "Accept-Language: en-US,en;q=0.9";
$headers [] = "Cookie: _ga=GA1.2.513239636.1592924479; _gid=GA1.2.728015094.1593092400; _gat_gtag_UA_164611555_2=1";

$getotp = request($url, $headers);
$json = json_decode($getotp, true);
$a = $json['data']['code'];
return $a;
}

echo "=========================\n";
echo "Borong Barang Slash\n";
echo "Created by @ikballnh\n";
echo "=========================\n";
echo "Masukan ID: ";
$id = trim(fgets(STDIN));
$getno = getno($id);
//echo "$getno";

$data=file_get_contents("token1.txt");
$ex = explode("\n", str_replace("\r", "", $data));
$count = count($ex);
for($i=0;$i<$count;$i++) {
$nomor = 1+ $i;
regis($ex[$i], $nomor, $getno);
}





	<?php

function request($url, $headers, $put = null) {
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $url);
	if($put):
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
	endif;
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	if($headers):
    curl_setopt($ch, CURLOPT_HEADER, false);
	curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
	endif;
	curl_setopt($ch, CURLOPT_ENCODING, "GZIP");
	return curl_exec($ch);
}

function regis($email, $nomor, $id) {
$url = "https://m.borongbareng.com/api-gateway/service-marketing/app/bargain/help/$id";
//$data = '[{"operationName":"OTPRequest","variables":{"email":"'.$email.'","otpType":"126","mode":"email","otpDigit":4},"query":"query OTPRequest($otpType: String!, $mode: String, $msisdn: String, $email: String, $otpDigit: Int) {\n  OTPRequest(otpType: $otpType, mode: $mode, msisdn: $msisdn, email: $email, otpDigit: $otpDigit) {\n    success\n    message\n    errorMessage\n    __typename\n  }\n}\n"}]';
$headers = array();
$headers [] = "Host: m.borongbareng.com";
$headers [] = "Connection: close";
$headers [] = "Accept: application/json, text/plain, */*";
$headers [] = "language: Indonesian";
$headers [] = "Authorization: $email";
$headers [] = "User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Mobile Safari/537.36";
$headers [] = "Sec-Fetch-Site: same-origin";
$headers [] = "Sec-Fetch-Mode: cors";
$headers [] = "Sec-Fetch-Dest: empty";
$headers [] = "Referer: https://m.borongbareng.com/";
$headers [] = "Accept-Encoding: gzip, deflate";
$headers [] = "Accept-Language: en-US,en;q=0.9";
$headers [] = "Cookie: _ga=GA1.2.513239636.1592924479; _gid=GA1.2.728015094.1593092400; _gat_gtag_UA_164611555_2=1";

$getotp = request($url, $headers);
$json = json_decode($getotp, true);
$a = $json['message'];
echo "$nomor --> $a";
echo "\n";
if ($json['code'] == 0) {
	echo "$nomor --> berhasil slash ";
} 
}

function getno($id) {
$url = "https://m.borongbareng.com/api-gateway/service-marketing/app/bargain/self/detail/$id";
//$data = '[{"operationName":"OTPRequest","variables":{"email":"'.$email.'","otpType":"126","mode":"email","otpDigit":4},"query":"query OTPRequest($otpType: String!, $mode: String, $msisdn: String, $email: String, $otpDigit: Int) {\n  OTPRequest(otpType: $otpType, mode: $mode, msisdn: $msisdn, email: $email, otpDigit: $otpDigit) {\n    success\n    message\n    errorMessage\n    __typename\n  }\n}\n"}]';
$headers = array();
$headers [] = "Host: m.borongbareng.com";
$headers [] = "Connection: close";
$headers [] = "Accept: application/json, text/plain, */*";
$headers [] = "language: Indonesian";
$headers [] = "Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMyMTMwNjQsImlhdCI6MTU5MzEyNjY2NCwidXNlciI6IntcImlkXCI6NTA3OTMsXCJuaWNrbmFtZVwiOlwiODkqKio2OTk1NjQ1XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTg5NTM0Njk5NTY0NVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.BZdhJM-k37AKs8XeiZ_mhN0xMPqZo9CL3omeaGuRfkM";
$headers [] = "User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Mobile Safari/537.36";
$headers [] = "Sec-Fetch-Site: same-origin";
$headers [] = "Sec-Fetch-Mode: cors";
$headers [] = "Sec-Fetch-Dest: empty";
$headers [] = "Referer: https://m.borongbareng.com/";
$headers [] = "Accept-Encoding: gzip, deflate";
$headers [] = "Accept-Language: en-US,en;q=0.9";
$headers [] = "Cookie: _ga=GA1.2.513239636.1592924479; _gid=GA1.2.728015094.1593092400; _gat_gtag_UA_164611555_2=1";

$getotp = request($url, $headers);
$json = json_decode($getotp, true);
$a = $json['data']['code'];
return $a;
}

echo "=========================\n";
echo "Borong Barang Slash\n";
echo "Created by @hardiansyah88\n";
echo "=========================\n";
echo "Masukan ID: ";
$id = trim(fgets(STDIN));
$getno = getno($id);
//echo "$getno";

$data=file_get_contents("token1.txt");
$ex = explode("\n", str_replace("\r", "", $data));
$count = count($ex);
for($i=0;$i<$count;$i++) {
$nomor = 1+ $i;
regis($ex[$i], $nomor, $getno);
}







eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxNzk4MjMsImlhdCI6MTU5MzA5MzQyMywidXNlciI6IntcImlkXCI6NzM2NzUsXCJuaWNrbmFtZVwiOlwiMDgqKio4MDkwMTA0XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTcxODA5MDEwNFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.4sLsNTxxo7b95hxh2Z4xoG6pdF0gbHHoN6xGvutgSCQ
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODAxNDksImlhdCI6MTU5MzA5Mzc0OSwidXNlciI6IntcImlkXCI6NzM3NTYsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMTExXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTExMVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.K3hGrfai4fAn43iARE0zi0hVduwpd5ArmP2NYs07oYY
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODAzNDksImlhdCI6MTU5MzA5Mzk0OSwidXNlciI6IntcImlkXCI6NzM4MDMsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMTEyXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTExMlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.Sx6f-ziLxKGpVBnBD1CntIAw_qzBEg0ovEfMzDYGkcE
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODA0NzgsImlhdCI6MTU5MzA5NDA3OCwidXNlciI6IntcImlkXCI6NzM4MzQsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMTEzXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTExM1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.aSIRbeTnVujiFYEoq5O85MhpXZ9JWfIwDznasUYU4P0
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODExNzUsImlhdCI6MTU5MzA5NDc3NSwidXNlciI6IntcImlkXCI6NzM5NzgsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMTE2XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTExNlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.rquTIXjNAqgzbeaEMcZHCshefaSOh7JOSuVw8tSP_IY
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODEzNjUsImlhdCI6MTU5MzA5NDk2NSwidXNlciI6IntcImlkXCI6NzQwMTgsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMTE4XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTExOFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.3NLK0VeCegRiLnIserQp1mqi_6OxHyd2zxbNfWGDBkg
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODE4MzgsImlhdCI6MTU5MzA5NTQzOCwidXNlciI6IntcImlkXCI6NzQxMTAsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMTE5XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTExOVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.xf1kohuZoGWtvX-fIEVKwnYdUWR-lJYGtC2RDWcJNYg
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODE5NTMsImlhdCI6MTU5MzA5NTU1MywidXNlciI6IntcImlkXCI6NzQxMzIsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMTIwXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTEyMFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.SUIFi2rjyVRLerr-xdG8HYqDw5UITyzCIig-3GkDtIk
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODIwODIsImlhdCI6MTU5MzA5NTY4MiwidXNlciI6IntcImlkXCI6NzQxNTksXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDAxXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAwMVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.TBuaNwnpeQtXlTDDtE7dWUEqCaisIKfYSMFsAFE9px4
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODIzMDIsImlhdCI6MTU5MzA5NTkwMiwidXNlciI6IntcImlkXCI6NzQxODgsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDAzXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAwM1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.HAZCs5EBInZaDRzQMRuHzP47scPBc8rVq4-tQazH8-E
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODIyMTksImlhdCI6MTU5MzA5NTgxOSwidXNlciI6IntcImlkXCI6NzQxNzksXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDAyXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAwMlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.NQQZGk8jFWMHrsrdaA7WMMYedNDPurAUX7l3e-9rpwg
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI2MTgsImlhdCI6MTU5MzA5NjIxOCwidXNlciI6IntcImlkXCI6NzQyNDEsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDA0XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAwNFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.hqq73vh4fUH_lkp4qJVVhmyS2inoInBPGdfGn0Fn4HY
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMxMDQsImlhdCI6MTU5MzA5NjcwNCwidXNlciI6IntcImlkXCI6NzQzNDksXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDA4XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAwOFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.Df1eDcRNbJEblSPu1OZxeXo4TpS9HkjCAIIz_rlC0nw
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMyMzIsImlhdCI6MTU5MzA5NjgzMiwidXNlciI6IntcImlkXCI6NzQzNzYsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDEwXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAxMFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ._Lrgh1ENvwfxUb52RBNLeBrtjldcGb5q7GVDYqbVv_Q
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMzNzQsImlhdCI6MTU5MzA5Njk3NCwidXNlciI6IntcImlkXCI6NzQ0MDUsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDEyXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAxMlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.gdHMpRQ0o6lsMKieXyIGdwPujbwFK5Jdw2dCzGb2Od4
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM2MjAsImlhdCI6MTU5MzA5NzIyMCwidXNlciI6IntcImlkXCI6NzQ0NjQsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDEzXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAxM1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.8upJB0M1QtHKk81N8-MuIQ0azVZzrRSQmJ7uhdDojaM
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM1ODUsImlhdCI6MTU5MzA5NzE4NSwidXNlciI6IntcImlkXCI6NzQ0NTMsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDE1XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAxNVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.dlhwU-MAmDquvLXwE-xHB_WjdJbn1F2xt8jKceXYJY0
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM4NTksImlhdCI6MTU5MzA5NzQ1OSwidXNlciI6IntcImlkXCI6NzQ1MjAsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDE2XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAxNlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.BsIkIOdMCKnVD7B1DkkfNlfmMYvxqgHj_PNlJBCnmOU
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM5NTgsImlhdCI6MTU5MzA5NzU1OCwidXNlciI6IntcImlkXCI6NzQ1NDQsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDE3XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAxN1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.4-FwgL4QDmW4S6clibaggrVPZI4wij9iTZnWbDyYDOI
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODQwODksImlhdCI6MTU5MzA5NzY4OSwidXNlciI6IntcImlkXCI6NzQ1NzQsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDE5XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAxOVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.MIDKSguOjg1Q6V0wPqKZNxfnr5VbXPsdYXgoxr5R-yE
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODQxMjAsImlhdCI6MTU5MzA5NzcyMCwidXNlciI6IntcImlkXCI6NzQ1NzcsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDE4XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAxOFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.EAUb3VlQOnnU5IRANdb4acm-ZWDbaPx0hnwEnKcmtM8
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODQ1OTQsImlhdCI6MTU5MzA5ODE5NCwidXNlciI6IntcImlkXCI6NzQ2NzcsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDIwXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAyMFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.YLoXwLuMIFsj3c9zPQsE76PEDvHYGIOgZ-o1D5_kL4M
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODQ3NzksImlhdCI6MTU5MzA5ODM3OSwidXNlciI6IntcImlkXCI6NzQ3MjIsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDIzXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAyM1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.w19fAgrkzedzND6oyXq6QEEhm0MgPF_Of-YbNUlxRUE
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODQ4ODUsImlhdCI6MTU5MzA5ODQ4NSwidXNlciI6IntcImlkXCI6NzQ3NTMsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDIyXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAyMlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.bTqM-uETqeOJbtkwE7gY11bexJm8EF8mowd7L9cQAMU
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTM5MjMsImlhdCI6MTU5MzEwNzUyMywidXNlciI6IntcImlkXCI6NzYyNDUsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDI1XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAyNVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.uwI4NQT_UwKIxCgKkumBxm8V7aNIG0oHjD6UIENwokg
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTQxMzAsImlhdCI6MTU5MzEwNzczMCwidXNlciI6IntcImlkXCI6NzYyNzAsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDI0XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAyNFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.q6U5xYDF9LCuWn31dUw5D4KmZyXyQigsvsEmnGTDO_I
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTQ0NTUsImlhdCI6MTU5MzEwODA1NSwidXNlciI6IntcImlkXCI6NzYzMTIsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDI2XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAyNlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.XtvskgBlB2mYnnO2eN2-hZsfCSFj8bd6638ZW_c8XYk
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTQ2MzcsImlhdCI6MTU5MzEwODIzNywidXNlciI6IntcImlkXCI6NzYzMzIsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDI3XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAyN1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.sd-_iyijItEieTFOMvy-WmRXCOAyIHvJu4i4HN9gKp8
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTQ5MzQsImlhdCI6MTU5MzEwODUzNCwidXNlciI6IntcImlkXCI6NzYzNzEsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDI5XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAyOVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.uKi1JLRwMlb7q5Cv9wf9MG0dxDpyfsfvqWN58_KQxos
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTU2MDYsImlhdCI6MTU5MzEwOTIwNiwidXNlciI6IntcImlkXCI6NzY0NTksXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDMwXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzMFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.jYKU1XWpLhw5v-4NSInp2n01s_iGC9B_LVRgqWkqRow
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTU4NjgsImlhdCI6MTU5MzEwOTQ2OCwidXNlciI6IntcImlkXCI6NzY0OTcsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDMxXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzMVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.5US8d89OLv8ag84e88Zsy7Fv6avEvLDMjuwnoijuds8
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTYyNzQsImlhdCI6MTU5MzEwOTg3NCwidXNlciI6IntcImlkXCI6NzY1NDYsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDMzXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzM1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.CIvwcHLQbfMOQP3YYrgXvKBbNuuuwmPxCqiBNbjRso4
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTY1MjEsImlhdCI6MTU5MzExMDEyMSwidXNlciI6IntcImlkXCI6NzY1NzAsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDM0XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzNFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.Ektx1zKLtHoAh0E9zeXYE2wgKgIaVLidKihvIyFFbi4
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTY0MzEsImlhdCI6MTU5MzExMDAzMSwidXNlciI6IntcImlkXCI6NzY1NjQsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDM1XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzNVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.Vf3ra3xy0A51f_FvtsvLU59crdZJwHln6hKixK6d1Z4
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTY2NzcsImlhdCI6MTU5MzExMDI3NywidXNlciI6IntcImlkXCI6NzY1NzksXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDM3XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzN1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.1KD1ACRlX4kBXta0-tcfZc5D1BpnHzROQnOFugRkfNU
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTY3MDgsImlhdCI6MTU5MzExMDMwOCwidXNlciI6IntcImlkXCI6NzY1ODMsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDM2XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzNlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.JVztI3GzPK5vS7D28tNQlP7U_sr1UCi5tQ6U-NDs-5s
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTY4OTUsImlhdCI6MTU5MzExMDQ5NSwidXNlciI6IntcImlkXCI6NzY2MDMsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDM5XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzOVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.OFsQvI7Nel9nsJX58cdMwfw9bOOMISH88v9vZDICRKE
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTY4NTUsImlhdCI6MTU5MzExMDQ1NSwidXNlciI6IntcImlkXCI6NzY1OTgsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDM4XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTAzOFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.369C4f6Zk5RASDuypAyb-P9qBfAgPzAnKYubL9BJhJc
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTczNTUsImlhdCI6MTU5MzExMDk1NSwidXNlciI6IntcImlkXCI6NzY2NTYsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQyXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0MlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.nR7aF049A31HoTNTtZr0Xre6JYlq4S_mS5BluJnxjOY
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTczNzksImlhdCI6MTU5MzExMDk3OSwidXNlciI6IntcImlkXCI6NzY2NjAsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQxXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0MVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.tin7o3BbXtayIJ3_Ay8kHXtINErybmZoayo0ntOj54o
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTc2NzAsImlhdCI6MTU5MzExMTI3MCwidXNlciI6IntcImlkXCI6NzY2OTAsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQzXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0M1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.S1pl6HMBulPfn1PMRbqwtb62uIPgwYGcn3hiZMpOlio
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTc2OTcsImlhdCI6MTU5MzExMTI5NywidXNlciI6IntcImlkXCI6NzY2OTQsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQ0XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0NFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.Gyqy_8gedBX2kXGoqTixXKkX_IpYhqOMXpLI9Fm0GH4
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTc4NTAsImlhdCI6MTU5MzExMTQ1MCwidXNlciI6IntcImlkXCI6NzY3MTEsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQ2XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0NlwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.noYPJHJ3EEIwM4BONskJV6xA1ljE7Xr0RlV1JwZfxT0
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTc5MDAsImlhdCI6MTU5MzExMTUwMCwidXNlciI6IntcImlkXCI6NzY3MTgsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQ1XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0NVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.La51HGkghCsFQp2Z75Ns1XMI2VswV-L_hlabQ-4_OFc
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTgwNTUsImlhdCI6MTU5MzExMTY1NSwidXNlciI6IntcImlkXCI6NzY3NDQsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQ4XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0OFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.8PSDVt3KybezOO0OyM1PiXsh2La_UX-VR61TS44IjS8
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTgwNTgsImlhdCI6MTU5MzExMTY1OCwidXNlciI6IntcImlkXCI6NzY3NDUsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQ3XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0N1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.Q8nL6OgICyyxnx50nMSH4K75kdIz6-9QcttWNkxdViA
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTgzMzIsImlhdCI6MTU5MzExMTkzMiwidXNlciI6IntcImlkXCI6NzY3NzYsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDQ5XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA0OVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.wYpx4hG75JAe_DTtgVgGL6h8Q4U94S7E48Pc6W5UwCU
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTg0MDEsImlhdCI6MTU5MzExMjAwMSwidXNlciI6IntcImlkXCI6NzY3ODQsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDUwXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA1MFwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.UI5AYP-pZM3lzVClpO76tSdIWyBhPxFH7dNxXhsV7rg
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxOTg2OTEsImlhdCI6MTU5MzExMjI5MSwidXNlciI6IntcImlkXCI6NzY4MTAsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgxMDUxXCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MTA1MVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.snlAEL3Ie4jXBwKk4cssVCcA_98pTdFc3jsU8QGnJPU
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMyMDEwMTMsImlhdCI6MTU5MzExNDYxMywidXNlciI6IntcImlkXCI6NzY5ODIsXCJuaWNrbmFtZVwiOlwiMDgqKio3MDgwMDU3XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTA4MTMxNzA4MDA1N1wiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.oZgsb70bYX1OZnMulTHFJEyHbZrFERaADnaZg60leds
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODIzMTMsImlhdCI6MTU5MzA5NTkxMywidXNlciI6IntcImlkXCI6MjE5NjUsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5MTVcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5MTVcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.V28eqjI_IXfcxMhu7Ez26QkfV_AQtbRuhs0r6RcfYzk
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODIzOTksImlhdCI6MTU5MzA5NTk5OSwidXNlciI6IntcImlkXCI6MjE5NzcsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5MTRcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5MTRcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.PQOeqX3_l6mu7Xul2iq9IaXu64yi4MXDdUnNYD-6UAs
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI1MzYsImlhdCI6MTU5MzA5NjEzNiwidXNlciI6IntcImlkXCI6MjE5ODgsXCJuaWNrbmFtZVwiOlwiODEqKioyNjY0NzRcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDUyNjY0NzRcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.fwgwcnowTSXcHyRM6WF5_sFdJA-EMdTAphKrBvAit4k
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI1ODQsImlhdCI6MTU5MzA5NjE4NCwidXNlciI6IntcImlkXCI6MjIwMTEsXCJuaWNrbmFtZVwiOlwiODEqKiowNzQxNTNcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDcwNzQxNTNcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.fQpFe9FoML4w6llNqkoev-wM_dI_MiMhs0sdz9es0fc
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI2NTAsImlhdCI6MTU5MzA5NjI1MCwidXNlciI6IntcImlkXCI6MjIwNDAsXCJuaWNrbmFtZVwiOlwiODEqKioyNjYyNzlcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDUyNjYyNzlcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.xrhbYlA45nQkwbNbUyBb_7uHs7XboOFIBrF80Wyg-kA
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI2OTMsImlhdCI6MTU5MzA5NjI5MywidXNlciI6IntcImlkXCI6MjIwMzAsXCJuaWNrbmFtZVwiOlwiODcqKiozMzQ5MDlcIixcInVzZXJuYW1lXCI6XCIoNjIpODc3NzYzMzQ5MDlcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0._8z6z3NQM8RQAQ9XmoBkvBdwp4wWt537kvDzbxQAHQM
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI3MjUsImlhdCI6MTU5MzA5NjMyNSwidXNlciI6IntcImlkXCI6MjIwNTAsXCJuaWNrbmFtZVwiOlwiODEqKiowNzQxMjJcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDcwNzQxMjJcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.RzuKeJjOR3FJSePdUz0TMCNpJ5qOD0xYkcu37NLVIuY
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI3NTQsImlhdCI6MTU5MzA5NjM1NCwidXNlciI6IntcImlkXCI6MjIwNTYsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5NzBcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5NzBcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.BzsG4-zRAlTaZYemPRPvb8P_ZQFvw_KDmB-VQBP3phg
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI3ODcsImlhdCI6MTU5MzA5NjM4NywidXNlciI6IntcImlkXCI6MjIwNjMsXCJuaWNrbmFtZVwiOlwiODEqKioyNjYyNjlcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDUyNjYyNjlcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.vMe0ezkbhvEtC3VmkJlRxq8g08K4-iMQAgObomZlqko
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI4MTQsImlhdCI6MTU5MzA5NjQxNCwidXNlciI6IntcImlkXCI6MjIwNjcsXCJuaWNrbmFtZVwiOlwiODcqKiozMzQ4OThcIixcInVzZXJuYW1lXCI6XCIoNjIpODc3NzYzMzQ4OThcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.18H5RBAu0oERaMSskO3YLEg3WhKG1R7lCyQLSmnm20A
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI4NDIsImlhdCI6MTU5MzA5NjQ0MiwidXNlciI6IntcImlkXCI6MjIwNzMsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5NTNcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5NTNcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.-0hfnaDPGCFqcJ5OidM3kVaU4cqVNsGQAGU9pV2d8qk
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODI5NjIsImlhdCI6MTU5MzA5NjU2MiwidXNlciI6IntcImlkXCI6MjIwODEsXCJuaWNrbmFtZVwiOlwiODEqKioyNjYyOTBcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDUyNjYyOTBcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.QIPvjodnvPQ68PK9tPVpI5oYWQGscK7CcBjr3yvHwf0
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMxMDAsImlhdCI6MTU5MzA5NjcwMCwidXNlciI6IntcImlkXCI6MjIwODQsXCJuaWNrbmFtZVwiOlwiODcqKiozMzQ5MjVcIixcInVzZXJuYW1lXCI6XCIoNjIpODc3NzYzMzQ5MjVcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.QT1Uukmp-SVJTDEVkzkNuWj-R8s_3QPYOtn530X-WZU
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMxNDEsImlhdCI6MTU5MzA5Njc0MSwidXNlciI6IntcImlkXCI6MjIwOTEsXCJuaWNrbmFtZVwiOlwiODEqKiowNzQyOTNcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDcwNzQyOTNcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.3OpLy-CFxkbBK8w3R2j8aUsZqrUJ5eR8FkMMChlWclk
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMxODksImlhdCI6MTU5MzA5Njc4OSwidXNlciI6IntcImlkXCI6MjIxNTYsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5MDJcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5MDJcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.ByKooDtqifGxBklw90PRsu776gNVy14UcC8lOTUlTtA
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMyNzQsImlhdCI6MTU5MzA5Njg3NCwidXNlciI6IntcImlkXCI6MjIxNjEsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5MjdcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5MjdcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.0N9vDlk45r36wS9cT8gNZEgRbb-zbL4yFWss-Syp_7w
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMzMjMsImlhdCI6MTU5MzA5NjkyMywidXNlciI6IntcImlkXCI6MjIxNzMsXCJuaWNrbmFtZVwiOlwiODEqKioyNjYyNjJcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDUyNjYyNjJcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.ORO4HgnSAZzouiN2615W66TIKAWvPQ8TL10GdaGp508
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODMzODcsImlhdCI6MTU5MzA5Njk4NywidXNlciI6IntcImlkXCI6MjIxODMsXCJuaWNrbmFtZVwiOlwiODEqKiowNzQxMzlcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDcwNzQxMzlcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.cRAzZ5MTdnjv1iztU1u3TgMRULYx9a1tNkxAimbwKeI
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM0MjcsImlhdCI6MTU5MzA5NzAyNywidXNlciI6IntcImlkXCI6MjIxODYsXCJuaWNrbmFtZVwiOlwiODcqKiozMzQ5MzNcIixcInVzZXJuYW1lXCI6XCIoNjIpODc3NzYzMzQ5MzNcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.SkePXvTqLVzwqd0rCmZhZqP5pbh4i-EQ4AdraXoHk1Q
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM0NTYsImlhdCI6MTU5MzA5NzA1NiwidXNlciI6IntcImlkXCI6MjIxOTEsXCJuaWNrbmFtZVwiOlwiODcqKiowOTU0NzlcIixcInVzZXJuYW1lXCI6XCIoNjIpODc3ODUwOTU0NzlcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.hx5NMR8vHnwUvDMr2cMZ0Q_iqPe_FkBKNTKt7S0leiQ
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM0OTYsImlhdCI6MTU5MzA5NzA5NiwidXNlciI6IntcImlkXCI6MjIyMTEsXCJuaWNrbmFtZVwiOlwiODcqKiozMzQ4NDFcIixcInVzZXJuYW1lXCI6XCIoNjIpODc3NzYzMzQ4NDFcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.VN0RKuyR48aFF9qDlLGrVOqLytPapFnkOu_4_EIABBA
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM1MjksImlhdCI6MTU5MzA5NzEyOSwidXNlciI6IntcImlkXCI6MjIyMDgsXCJuaWNrbmFtZVwiOlwiODEqKioyNjYzMTRcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDUyNjYzMTRcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.Kxbjf6LJiwNpuO5Pk9M7CKB6XYErYGS7dZG_qzL2Fsw
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM1NTQsImlhdCI6MTU5MzA5NzE1NCwidXNlciI6IntcImlkXCI6MjIyMTQsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5MjFcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5MjFcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.4wurFD3i4ciAOeuXf_dDyhWAr1iIF97d5LoQ4WGdj4I
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM1ODIsImlhdCI6MTU5MzA5NzE4MiwidXNlciI6IntcImlkXCI6MjIyMjEsXCJuaWNrbmFtZVwiOlwiODEqKiowNzQwOTRcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDcwNzQwOTRcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.YRFNeK0i42e1a3o6tvKGWSL2PLdILXASHK3wiOhIGlU
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM2MDcsImlhdCI6MTU5MzA5NzIwNywidXNlciI6IntcImlkXCI6MjIyMjYsXCJuaWNrbmFtZVwiOlwiODcqKiozMzQ4NTFcIixcInVzZXJuYW1lXCI6XCIoNjIpODc3NzYzMzQ4NTFcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.SOUoc7mcSFv3aAtY-YyJnZ5EBdWhsvdFZwHJSBMu_FE
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM2NzAsImlhdCI6MTU5MzA5NzI3MCwidXNlciI6IntcImlkXCI6MjIyNTYsXCJuaWNrbmFtZVwiOlwiODcqKiozMzQ4ODNcIixcInVzZXJuYW1lXCI6XCIoNjIpODc3NzYzMzQ4ODNcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.xH83jK5x3jGMSYIl1BsWTK6l4iyGsFMgADdIWOtDYSE
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM2OTUsImlhdCI6MTU5MzA5NzI5NSwidXNlciI6IntcImlkXCI6MjIyNjAsXCJuaWNrbmFtZVwiOlwiODEqKiowNzQxNjBcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDcwNzQxNjBcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0._ZnHv0heOyu9dVb_UbdyN5evp-6ArIEwzcLdS_-Sog4
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM2OTUsImlhdCI6MTU5MzA5NzI5NSwidXNlciI6IntcImlkXCI6MjIyNjAsXCJuaWNrbmFtZVwiOlwiODEqKiowNzQxNjBcIixcInVzZXJuYW1lXCI6XCIoNjIpODE5MDcwNzQxNjBcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0._ZnHv0heOyu9dVb_UbdyN5evp-6ArIEwzcLdS_-Sog4
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM3OTIsImlhdCI6MTU5MzA5NzM5MiwidXNlciI6IntcImlkXCI6MjIyNjIsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5MDlcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5MDlcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.5JMsIXC6vu4hJVP83R8C4ER0zRU3pks7-fo7uCFxFFg
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMxODM4MjAsImlhdCI6MTU5MzA5NzQyMCwidXNlciI6IntcImlkXCI6MjIyNzEsXCJuaWNrbmFtZVwiOlwiODcqKio1NzQ5NDlcIixcInVzZXJuYW1lXCI6XCIoNjIpODc4Nzg1NzQ5NDlcIixcImF2YXRhclwiOlwiaHR0cHM6Ly9ib3JvbmdiYXJlbmctaDUub3NzLWFwLXNvdXRoZWFzdC01LmFsaXl1bmNzLmNvbS9kZXYvMjAyMDA0MTMvYzBiZjk2OTQzY2ExNGFlNDk2YzkwODA0YTdmM2YzYWQuanBnXCIsXCJsYW5ndWFnZVwiOm51bGx9In0.tQ-Myz5QrK_o6JGREh6zLKvA6O1v9H9iPnKCvSdG7bo



<?php

function request($url, $headers, $put = null) {
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $url);
	if($put):
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
	endif;
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	if($headers):
    curl_setopt($ch, CURLOPT_HEADER, false);
	curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
	endif;
	curl_setopt($ch, CURLOPT_ENCODING, "GZIP");
	return curl_exec($ch);
}

function regis($email, $nomor, $id) {
$url = "https://m.borongbareng.com/api-gateway/service-marketing/app/bargain/help/$id";
//$data = '[{"operationName":"OTPRequest","variables":{"email":"'.$email.'","otpType":"126","mode":"email","otpDigit":4},"query":"query OTPRequest($otpType: String!, $mode: String, $msisdn: String, $email: String, $otpDigit: Int) {\n  OTPRequest(otpType: $otpType, mode: $mode, msisdn: $msisdn, email: $email, otpDigit: $otpDigit) {\n    success\n    message\n    errorMessage\n    __typename\n  }\n}\n"}]';
$headers = array();
$headers [] = "Host: m.borongbareng.com";
$headers [] = "Connection: close";
$headers [] = "Accept: application/json, text/plain, */*";
$headers [] = "language: Indonesian";
$headers [] = "Authorization: $email";
$headers [] = "User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Mobile Safari/537.36";
$headers [] = "Sec-Fetch-Site: same-origin";
$headers [] = "Sec-Fetch-Mode: cors";
$headers [] = "Sec-Fetch-Dest: empty";
$headers [] = "Referer: https://m.borongbareng.com/";
$headers [] = "Accept-Encoding: gzip, deflate";
$headers [] = "Accept-Language: en-US,en;q=0.9";
$headers [] = "Cookie: _ga=GA1.2.513239636.1592924479; _gid=GA1.2.728015094.1593092400; _gat_gtag_UA_164611555_2=1";

$getotp = request($url, $headers);
$json = json_decode($getotp, true);
$a = $json['message'];

if ($json['code'] == 0) {
	echo "$nomor --> berhasil slash\n";
} else {
	echo "$nomor --> $a";
	echo "\n";
}
}

function getno($id) {
$url = "https://m.borongbareng.com/api-gateway/service-marketing/app/bargain/self/detail/$id";
//$data = '[{"operationName":"OTPRequest","variables":{"email":"'.$email.'","otpType":"126","mode":"email","otpDigit":4},"query":"query OTPRequest($otpType: String!, $mode: String, $msisdn: String, $email: String, $otpDigit: Int) {\n  OTPRequest(otpType: $otpType, mode: $mode, msisdn: $msisdn, email: $email, otpDigit: $otpDigit) {\n    success\n    message\n    errorMessage\n    __typename\n  }\n}\n"}]';
$headers = array();
$headers [] = "Host: m.borongbareng.com";
$headers [] = "Connection: close";
$headers [] = "Accept: application/json, text/plain, */*";
$headers [] = "language: Indonesian";
$headers [] = "Authorization: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJBcHAiLCJleHAiOjE1OTMyMTMwNjQsImlhdCI6MTU5MzEyNjY2NCwidXNlciI6IntcImlkXCI6NTA3OTMsXCJuaWNrbmFtZVwiOlwiODkqKio2OTk1NjQ1XCIsXCJ1c2VybmFtZVwiOlwiKDYyKTg5NTM0Njk5NTY0NVwiLFwiYXZhdGFyXCI6XCJodHRwczovL2Jvcm9uZ2JhcmVuZy1oNS5vc3MtYXAtc291dGhlYXN0LTUuYWxpeXVuY3MuY29tL2Rldi8yMDIwMDQxMy9jMGJmOTY5NDNjYTE0YWU0OTZjOTA4MDRhN2YzZjNhZC5qcGdcIixcImxhbmd1YWdlXCI6bnVsbH0ifQ.BZdhJM-k37AKs8XeiZ_mhN0xMPqZo9CL3omeaGuRfkM";
$headers [] = "User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Mobile Safari/537.36";
$headers [] = "Sec-Fetch-Site: same-origin";
$headers [] = "Sec-Fetch-Mode: cors";
$headers [] = "Sec-Fetch-Dest: empty";
$headers [] = "Referer: https://m.borongbareng.com/";
$headers [] = "Accept-Encoding: gzip, deflate";
$headers [] = "Accept-Language: en-US,en;q=0.9";
$headers [] = "Cookie: _ga=GA1.2.513239636.1592924479; _gid=GA1.2.728015094.1593092400; _gat_gtag_UA_164611555_2=1";

$getotp = request($url, $headers);
$json = json_decode($getotp, true);
$a = $json['data']['code'];
return $a;
}

echo "=========================\n";
echo "Borong Barang Slash\n";
echo "Created by @hardiansyah88\n";
echo "=========================\n";
echo "Masukan ID: ";
$id = trim(fgets(STDIN));
$getno = getno($id);
//echo "$getno";

$data=file_get_contents("token1.txt");
$ex = explode("\n", str_replace("\r", "", $data));
$count = count($ex);
for($i=0;$i<$count;$i++) {
$nomor = 1+ $i;
regis($ex[$i], $nomor, $getno);
}

