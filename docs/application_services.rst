.. _Application Services:

Application Services
====================

Login Service
Application Login

To login as an application first log in to Tamigo as an administrator. Go to Configuration -> Manage Application Keys to create a new application key.
The application key is used for authentication of your application and it is therefore paramount that you secure it properly.
If your key has been compromised, it is possible to regenerate the key to ensure the security of your Tamigo instance.
The response returned from the service contains the SessionToken, use this token in subsequent calls to the service to authenticate yourself.


Request (JSON)
--------------

POST /login/application
Content-Type: application/json
{"Name":"micros", "Key":"longstringasyourpasscodegeneratedfromtheapplication"}
Response (JSON)
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
{"DefaultDepartmentId":"",
" DefaultDepartmentName ":"",
"DefaultCompanyId":"",
"DefaultCompanyName":"",
"Email":"",
"Password":"",
"Role":"Application",
"MenuId":"5",
"SessionToken":"e2c152f3-49ea-4b50-948b-bc0520261737"}