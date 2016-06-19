.. _api:

Tamigo API
==========

.. danger:: This is not `official <http://www.tamigo.dk/andet/api-dokumentation/>`_ documentation, but community driven work in progress for offering better api documentation.

Tamigo offers a set of REST based services for you to create new ways to interact with Tamigo. The services are divided into two categories.
The first set of services can be called using an application access, and is meant to be used for integrating third party applications with Tamigo
The other set of services uses a Tamigo user as authentication, and will return data based on the users context. These services can be used to create other user experiences for Tamigo, and we use it ourselves to power the mobile applications for Tamigo.
The services are all capable of returning xml or json-formatted data. Which response you get will be based on type of input and the specified content-type in the HTTP headers. The service URL can also be used without SSL.

**Services URL**

`https://services.tamigo.com`

Application Services
--------------------
Login Service
*************
Application Login
*****************
To login as an application first log in to Tamigo as an administrator. Go to Configuration -> Manage Application Keys to create a new application key.
The application key is used for authentication of your application and it is therefore paramount that you secure it properly.
If your key has been compromised, it is possible to regenerate the key to ensure the security of your Tamigo instance.
The response returned from the service contains the SessionToken, use this token in subsequent calls to the service to authenticate yourself.


**Request**

.. code-block:: javascript

	Request (JSON)
	POST /login/application
	Content-Type: application/json

	{"Name":"micros", "Key":"longstringasyourpasscodegeneratedfromtheapplication"}

**Response**

.. code-block:: javascript

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


Partner Login
*************
To login as a Partner, you need to get a login from Tamigo (suppert@tamigo.com). The partner key is used for authentication of you and it is therefore paramount that you secure it properly.
If your key has been compromised, you need to contacts as right away, so we can disable it and give you a new key.
The response returned from the service contains the SessionToken, use this token in subsequent calls to the service to authenticate yourself.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /login/application
	Content-Type: application/json
	{"Name":"micros", "Key":"longstringasyourpasscodegeneratedfromtheapplication"}

**Response**

.. code-block:: javascript

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


Attendance service
------------------
Tamigo offers the attendance service, which allows you to register check-in and check-out for employees directly in Tamigo from a third-party system (e.g. Point-Of-Sale system).
The Attendance Service requires you to set up keys in Tamigo for the departments and employees whom should be able to use the service.
To set up these keys, login to Tamigo as a Planner or Administrator. Go to Configuration -> Departments, to set the Department ID (POS Key ID). Go to Configuration -> POS Keys, to set the corresponding keys for the Employees.
Check-in
Use check-in to register an employee has arrived at work. Tamigo will register the check-in at the time you send, and match it to the employee and department.

.. note:: As time rounding rules can be applied, the service returns the rounded time.


**Paramters**

==============  ======================  =====================================
Parameter       Format         		    Example
==============  ======================  =====================================
departmentKey   Tekst                   A123, 1234567, abcdefg
employeeKey     Tekst                   A123, 1234567, abcdefg
checkInTime     yyyy-MM-ddThh:mmss      2011-09-02T12:37:00
sessionToken    as returned by login    766c9732-e2d1-46d1-ae3e-a74c560bb8e6
==============  ======================  =====================================

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /attendance/checkin/?token=<sessionToken> HTTP/1.1
	Content-Type: application/json
	{"DepartmentKey":"<departmentKey>",
	"EmployeeKey":"<employeeKey>","Time":"<checkInTime>"}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

Export Absence
**************
Use to export absence from 1 or more departments. If DepartmentId is left out, result is returned for all departments

**Paramters**

==============  ======================  =====================================
Parameter       Format         		    Example
==============  ======================  =====================================
StartDate       MM-DD-YYYY              09-22-2015
EndDate         MM-DD-YYYY              09-22-2015     
DepartmentId    yyyy-MM-ddThh:mmss      Optional.
==============  ======================  =====================================

**Request**

.. code-block:: javascript

	GET /Leave/ByDate/?startDate=<startDate>&endDate=<endDate>&departmentId=<departmentId>securityToken=<sessionToken>
	HTTP/1.1
	Content-Type application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"AbsenceType":"Vacation",
	"Date":"\/Date(1324854000000+0100)\/",
	"Name":"John Doe",
	"WageSystemKey":"00044"},
	...
	{"AbsenceType":"Vacation",
	"Date":"\/Date(1324940400000+0100)\/",
	"Name":" John Doe ",
	"WageSystemKey":"00044"}]


Import Revenue
**************

Use to import the daily revenue.

**Paramters**

================   ======================  ==========================================
Parameter          Format         		   Example
================   ======================  ==========================================
ApplicationName    n/a                     Same as when logging in. Optional
Key                n/and                   Same as when logging in. Optional     
Content            .csv file               departmentId, date (YYYY-MM-DD)], revenue
================   ======================  ==========================================

**Request**

.. code-block:: javascript

	Request (JSON) 
	POST /Revenues/?securityToken=<sessionToken>securityToken=<sessionToken>
	HTTP/1.1
	Content-Type application/json

	{
	"ApplicationName": "<ApplicationName>",
	"Key": "<ApplicationKey>”,
	"Content": [
	"100,2011-01-01,1000",
	"100,2011-01-02,1200"
	],
	"Format": "Standard",
	"DateTimeReceived": "\/Date(1327996809388+0100)\/",
	"Type": null
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

Import Revenue Over day/ Footfall
*********************************

Use to import the daily revenue.

**Paramters**

================  ======================   ==================================================================================
Parameter		  Format 				   Example
================  ======================   ==================================================================================
ApplicationName   n/a                      Same as when logging in. Optional
Key               n/and                    Same as when logging in. Optional     
Content           .csv file, see note      amount;amountType;startDateTime;endDateTime;posId;statusType;employeePosKey
Format                                     should use “Standard”
DateTimeRecived                            the time you send the request Type: for now send NULL
Type 									   for now send NULL
================  ======================   ==================================================================================

.. note:: Content csv explained:  Amount: The number of either revenue or footfall. Amountype: 1 = Revenue, 2 = Footfall (customers) StartDateTime: Start of data to import (should be whole quarters start and end 01-01-2014 13:00 to 01-01-2014 13:15) EndDateTime: End of data to import posId: Department Store Id statusType: 1 = Actual, 2 = forecast. employeePosKey: Should only be filled if its revenue and employee specific


**Request**

.. code-block:: javascript

	Request (JSON) 
	POST /Revenues/UploadRevenueOverDay?securityToken=<sessionToken>
	HTTP/1.1
	Content-Type application/json

	{
	"ApplicationName": "<ApplicationName>",
	"Key": "<ApplicationKey>”,
	"Content": [
	"12;2;2014-01-01 13:00;201-01-01 13:15;999;1;NULL",
	"6;2;2014-01-01 13:15;201-01-01 13:30;999;1;NULL",
	],
	"Format": "Standard",
	"DateTimeReceived": "\/Date(1327996809388+0100)\/",
	"Type": null
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

Import Transactions
*******************

Use to import the daily revenue including related transactions.

**Paramters**

* DepartmentKey: Id of the department
* EmployeePosKey: for now send NULL
* Time: Time of the transaction
* Amount: Total amount of included lines
* Lines: Array with the following parameters:

	* Count: Number of items
	* Price: Item price
	* ProductKey: Product key of product
	* ProductName: Name of product

**Request**

.. code-block:: javascript

	Request (JSON) 
	POST /Revenues/UploadTransactions?securityToken=<sessionToken>
	HTTP/1.1
	Content-Type application/json

	[
	{
	"DepartmentKey": "100",
	"EmployeePosKey": null,
	"Time": "\/Date(1412589803535+0200)\/",
	"Amount": 1000.5,
	"Lines": [
	{
	"ProductKey": "Some-product-key",
	"ProductName": "Some-product-name",
	"Count": 2,
	"Price": 500.25
	}
	]
	}
	]

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK


Employee Service
----------------
The employee service allows you to retrieve and update employee information for use in a third party system (e.g. Salary System). Employee service also allows you to create new employees, planners or administrators from third party applications.

Retrieve Employees
******************

If you want a list of employees this is where you go. All employees in the company will be retrieved.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /employees/?token=<token>
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"Email":"user1@tamigo.com","EmployeeId":"9c0f571e-cd0e-4e38-ac51-ecf965cd3de6","IsPlanner":false,"Name":"Søren","PosKey":null, "DepartmentRoles":null },
	...
	{"Email":"tpl125@tamigo.com","EmployeeId":"d1d0b7db-c774-4e81-b094-d1d2acaa1d83","IsPlanner":false,"Name":"Janik","PosKey":null, ","DepartmentRoles ":null }]

Create Employees
****************

You can create a new employee, planner or administrator. If you leave department blank the default department will be used, and if password is blank tamigo will be used as password.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /Employees/?securityToken={tokenId}&departmentId={departmentId}&password={password}
	Content-Type: application/json
	{"Email": "karina@tamigo.com",
	"Name": "Karina Jensen",
	"Phone": "88888888",
	"EmployeNumber": "1234",
	"WageSystemKey": "4321",
	"Role": "name of the role ex. Employee, Planner, Administrator",
	"IsEnabled": "true"
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	14
	{ Success":true}

User Services
-------------
Login Service
-------------
User Login
**********
To use user services you need to login as a user of Tamigo, providing their email and password for authentication. The response returned from the service contains the SessionToken, use this token in subsequent calls to the service to authenticate yourself.The DefaultCompanyId its an optional parameter, if we left blank DefaultCompanyId, the service do login to Default Department.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /Login/
	HTTP/1.1
	Content-Type: application/json
	{"Email": "karina@tamigo.com",
	"Password": "password",
	"DefaultCompanyId": "3e7131b4-d2e7-44ab-a4bf-5ecbcceff011"}


**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	{"DefaultDepartmentId":"1ca845e9-28f4-5f79-9517-6d0eac942564",
	"DefaultDepartmentName":"Ny Bistro",
	"DefaultCompanyId":"1fc5e5e9-28f4-42e6-9917-6faf8c98da64",
	"DefaultCompanyName":"Ny Bistro",
	"Email":"test@example.com",
	"Password":"",
	"Role":"Planner",
	"SessionToken":"1fcbace9-28f4-4e76-9917-6f0ebc98da64"}

Get Token
*********

Returns the token information. Available for All.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Login/?securitytoken={sessionToken}
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	{"DefaultDepartmentId":"1ca845e9-28f4-5f79-9517-6d0eac942564",
	"DefaultDepartmentName":"Ny Bistro",
	"DefaultCompanyId":"1fc5e5e9-28f4-42e6-9917-6faf8c98da64",
	"DefaultCompanyName":"Ny Bistro",
	"Email":"test@example.com",
	"Password":"",
	"Role":"Planner",
	"SessionToken":"1fcbace9-28f4-4e76-9917-6f0ebc98da64"}

Menu service
------------
The menu service contains everything about menu items.

Mobile Menu
***********

This service returns a list of menu items. The service determines by the companyid and role from the token which menu is to be returned.
The menu contains headers (parentId = null) and an item (parentId is equal to header id).
The list is sorted the right way by the service.
Version determines what the menu will include. Currently highest version of menu is 2.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Menu/mobile/?securitytoken={sessionToken}&Version={version}
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	"[{\"DefaultChildId\":null,\"Description\":\"\",\"Enabled\":true,\"Id\":1024,\"OrderIndex\":1,\"ParentId\":null,\"ResourceId\":\"InfoHeader\",\"Role\":\"4ac90ed7-edd4-4e4c-98d9-3aa4b1b85d7d\",\"Selected\":false,\"Url\":null},{\"DefaultChildId\":null,\"Description\":\"\",\"Enabled\":true,\"Id\":1025,\"OrderIndex\":1,\"ParentId\":1024,\"ResourceId\":\"FrontPage\",\"Role\":\"4ac90ed7-edd4-4e4c-98d9-3aa4b1b85d7d\",\"Selected\":false,\"Url\":null }]"

Shift service
-------------
The shift service can be used to retrieve information on rosters and upcoming shifts.

My overview
***********

Returns a list of the logged in employees upcoming shifts. Dates are expressed as milliseconds since EPOCH (1970-1-1 00:00:00). Available for All.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /shifts/future/?securitytoken={sessionToken}
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"BreakCode":"Q",
	"Comment":"",
	"DepartmentName":"Ny Bistro",
	"EmployeeName":null,
	"EndTime":"\/Date(1318860000000+0200)\/",
	"StartTime":"\/Date(1318831200000+0200)\/"},
	...
	{"BreakCode":"Q",
	"Comment":"",
	"DepartmentName":"Ny Bistro",
	"EmployeeName":null,
	"EndTime":"\/Date(1318946400000+0200)\/",
	"StartTime":"\/Date(1318917600000+0200)\/"}]

Authorized roster - today
*************************
This resource returns a list of the authorized roster for the day including employees that are on leave, signified by an associated activity. Available for all Roles. If department is equal to “all”, shift from all rosters in all departments are returned.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /shifts/today/?securitytoken={sessionToken}(&departmentId={departmentId})
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"},
	...
	{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"}]

Authorized roster - arbitrary day
*********************************
This resource returns a list of the authorized roster for the day including employees that are on leave, signified by an associated activity. Available for all Roles. If department is equal to “all”, shift from all rosters in all departments are returned.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /shifts/day/{date}/?securitytoken={sessionToken}(&departmentId={departmentId})
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"},
	...
	{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"}]

Authorized roster - period of dates by employee
***********************************************
This resource returns a list of the authorized roster for the period of dates for the employee selected. Available for all Roles. If department is equal to “all”, shift from all rosters in all departments are returned.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /shifts/period/{startDate}/{endDate}/?securitytoken={sessionToken}(&departmentId={departmentId})&employeeId=employeeId
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"},
	...
	{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"}]

Get relevant planned shift
**************************
This returns the most relevant planned shift for the employee. By relevant means the shift that has not ended yet.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /shifts/planned/relevant/?securitytoken={sessionTokenContent-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"},
	...
	{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"}

Vacant shifts
*************
.. warning:: Deprecated, use Bid service instead. This resource is only available for backwards compatibility. Returns list of shifts that are open to take. Available for all Roles.

Update shift
************
Use this to update your shift resources. 

..note:: It is important to send all information about the shift back, even if it is not changed. All changes between the original shift and the new shifts will be seen as updates. So if an original shift comment is omitted the comment will be deleted.

This service is also used for employees to set their shifts as “Vacant” by setting “IsAvailable” to “true”. If an employee uses this service, only the “IsAvailable” property will be used. If a planner changes employee on a shift, exchanges and bids will be removed if possible. If the “EmployeeId” is changes to “null” “IsAvailable” will be set as “true” on the server.
This service returns a “Response Message”. If “Success” is true everything is nice. If “Success” is “false” a “Message” will be supplied with a description of the error. This message should be shown to the user.
To update a shift with empty endtime, set endtime to a date less than 01-01-1900.

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /Shifts/{ShiftId}/?securitytoken={sessionToken}
	{
	"ShiftId": "fb60311f-e88c-4a6a-9bc4-6ad09fd6a7b5",
	"StartTime": "\/Date(1355986847000+0100)\/",
	"EndTime": "\/Date(1356008447000+0100)\/",
	"Comment": "Early",
	"BreakCode": "P",
	"IsAvailable": false,
	"IsExchange": false,
	"EmployeeId": "4a6b1e63-666d-4955-b42b-20a289da3414",
	"ShiftActivityId": "00000000-0000-0000-0000-000000000000"
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":null,"Success":true}

Add shift
*********
A planner can use everything on a shift besides the IsAvailable and the “IsExchange” property. If the “EmployeeId” is send as “null”, “IsAvailable” will be set as “true” on the server.
This service returns a “Response Message”. If “Success” is true everything is nice. If “Success” is “false” a “Message” will be supplied with a description of the error. This message should be shown to the user.
“type” should be either “planned” or “actual” to define if you are adding a planned or an actual shift.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /Shifts/{type}/?securitytoken={sessionToken}
	{
	"StartTime": "\/Date(1355986847000+0100)\/",
	"EndTime": "\/Date(1356008447000+0100)\/",
	"Comment": "Early",
	"BreakCode": "P",
	"IsAvailable": false,
	"IsExchange": false,
	"EmployeeId": "4a6b1e63-666d-4955-b42b-20a289da3414",
	"ShiftActivityId": "00000000-0000-0000-0000-000000000000"
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":null,"Success":true}

Delete shift
************
A planner or administrator can delete a shift, if they have the role in the specific department.

**Request**

.. code-block:: javascript

	Request (JSON)
	DELETE /Shifts/{shiftId}/?securityToken={tokenId}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":null,"Success":true}

Bid service
***********
The Bid service gives the user a view on shifts that have been offered, and it enables bidding for these shifts.
Get available shifts
This resource returns a list of shifts that are open to bid on. This resource is available for all roles.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /bids/Available/?securitytoken={sessionToken}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	[{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"},
	...
	{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"}]

Lay bid for available shift
***************************
The logged in employee bids on an available shift
This is only available to Employees. Planners cannot bid on shifts

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /Shifts/Available/{shiftId}/?securitytoken={sessionToken}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Accept bid on available shift
	Use this resource to decide a bid for an available shift. Available only to Planners.
	Use the “IsAccepted” property to accept or reject the request
	Request (JSON)
	PUT /Requests/{requestId}/?securityToken={sessionToken}
	{
	"RequestId": "dbdb767f-1415-4cdb-8763-1a7c2281f1b8",
	"IsAccepted": true
	}


Withdraw vacant shift
*********************
Withdraw shift if it has been made vacant. The bid cannot be withdrawn if other people have bid on the shift.

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /bids/withdraw/{ShiftId}/?securitytoken={sessionToken}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":null,"Success":true}

Shift Exchange Service
----------------------
The Shift exchange service can be used to make exchanges between two shifts, as opposed to the Bid service, where there is only one shift involved.

Create new Shift Exchange
*************************

This creates a new Shift Exchange. Available for Employee.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /exchanges/?securitytoken={sessionToken}
	{"ProposerShiftId":"<shiftId1>","AccepterShiftId":"<shiftId2>"}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

List of shits to exchange with
******************************

This resource returns a list of shifts that the current shift can be exchanged with. Available for Employee.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Exchanges/Available/?shiftId={shiftIdToExchange}&securityToken={token}

**Response**

.. code-block:: javascript

	Response (JSON)
	[{"EmployeeId":"AA1A148A-3138-4BE5-9892-4A6F6EE9F95B",
	"Name":"Peter",
	"HaveShifts":"false",
	"BreakCode":"Q",
	"EndDateTime":"\/Date(1316700000000+0200)\/",
	"StartDateTime":"\/Date(1316671200000+0200)\/"},
	"Comment":"Lukke",
	"CanExchangeShift":"false"},
	...
	{"EmployeeId":"AA1A148A-3138-4BE5-9892-4A6F6EE9F95B",
	"Name":"Peter",
	"HaveShifts":"false",
	"BreakCode":"Q",
	"EndDateTime":"\/Date(1316700000000+0200)\/",
	"StartDateTime":"\/Date(1316671200000+0200)\/"},
	"Comment":"Lukke",
	"CanExchangeShift":"true"}]

Get List of pending approvals (Employee and Planner)
****************************************************

An employee will get a list of exchanges that needs to be approved – this means that the employee accepts that he/she would like to exchange the selected shifts.
A Planner will get a list of exchanges that have been approved by the employee and needs to Accept the exchange before it is final.
The Property Violate11HourRule is only available when the planner is accepting the request.
If the property is True. There should be an alert.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Exchanges/Requests/?securityToken={sessionToken}

**Response**

.. code-block:: xml

	<ArrayOfShiftExchangeRequest xmlns="http://schemas.datacontract.org/2004/07/Tamigo.Services.Entities" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
	<ShiftExchangeRequest>
	<Comment i:nil="true"/>
	<EmployeeId1>38f02810-6ac4-40b4-b238-39236c826118</EmployeeId1>
	<EmployeeId2>3203ba4b-5264-4f36-8daa-76e7e69d0f31</EmployeeId2>
	<EmployeeName1>Peter</EmployeeName1>
	<EmployeeName2>Karina</EmployeeName2>
	<EndDateTime1>2011-12-24T16:00:00</EndDateTime1>
	<EndDateTime2>2011-12-23T16:00:00</EndDateTime2>
	<IsAccepted i:nil="true"/>
	<RequestId>9aeb6e1a-d50a-40f0-bd79-0369fcc25ddc</RequestId>
	<ShiftHours1>7.25</ShiftHours1>
	<ShiftHours2>7.25</ShiftHours2>
	<StartDateTime1>2011-12-24T08:00:00</StartDateTime1>
	<StartDateTime2>2011-12-23T08:00:00</StartDateTime2>
	<Violate11HourRule>False</Violate11HourRule>
	<ShiftComment1>Åbner</ ShiftComment1>
	< ShiftComment2>Lukker</ ShiftComment2>
	< ShiftBreakCode1>P</ ShiftBreakCode1>
	< ShiftBreakCode2>Q</ ShiftBreakCode2>
	</ShiftExchangeRequest>
	</ArrayOfShiftExchangeRequest>

Decide shift exchange
*********************

Approve or reject a shift exchange. The approval is a two-step process:

*1 Approval by the employee who are exchanging his shift (the one that did not create the shift exchange)
*2 Approval by the planner.

Available to All Roles.

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /Exchanges/Requests/{requestId}/ {isAccepted}/?securitytoken={sessionToken}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

Withdraw shift exchange
***********************
Withdraw your shift exchange, if the other employee hasn’t accepted it yet.

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /Exchanges/withdraw/{shiftId}/?securitytoken={sessionToken}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":null,"Success":true}

News Service
------------

The News Service offer access to the information that is posted on the front page of Tamigo.
Get company and department news
This resource returns the content from the front page in Tamigo. The content is returned as HTML. This resource is available to all Roles.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /News/?securityToken={sessionToken})

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: text/html
	<html><body>
	<h1>{departmentNews}</h1>
	<div id="departmentNews">
	{content}
	</div>
	<h1>{companyNews}</h1>
	33
	<div id="companyNews">
	{content}
	</div>

Absence Service
---------------

The absence service enables the Employee to ask for absence, and the Planner can register absence (e.g. an employee no-show due to illness), the Planner can also approve or reject the Employee’s absence requests.
Create new absence request / Register absence
When an employee wishes to plan absence he submits a absence request. When a Planner wishes to register absence he uses the same service. If a planner registers absence there is the option to make any shifts in the absence period “vacant”.

**Paramters**

================   ==================================  ==========================================
Parameter          Format         		   		       Example
================   ==================================  ==========================================
employeeId    	   Guid                                766c9732-e2d1-46d1-ae3e-a74c560bb8e6
DateFrom           DateTime 						   2011-09-02T12:37:00              
DateTo             DateTime                	   		   2011-09-02T12:37:00
LeaveTypeId        as specified by leavetype service   1           
Comment            n/a 								   n/a
================   ==================================  ==========================================

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /LeaveRequests/?securityToken={sessionToken}&makeShiftsVacant={makeShiftsVacant}
	{"EmployeeId":"<employeeId>", "DateFrom":"<DateFrom>","DateTo":"<dateTo>",
	"LeaveTypeId":"<leaveTypeId>, "Comment":"<comment>"}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

List of future absence
**********************

This resource returns a list of the registered absence for the logged in employee.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Leave/Future/?securityToken={sessionToken}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	[{"LeaveRequestId":"766c9732-e2d1-46d1-ae3e-a74c560bb8e6", "EmployeeId":"ffff9732-e2d1-46d1-ae3e-a74c560bb702",
	“EmployeeName”:”Karina”,
	"DateFrom":"\/Date(1316700000000+0200)\/",
	"DateTo":"\/Date(1316700000000+0200)\/",
	"LeaveTypeId":"1",
	”LeaveTypeName”:”Vacation”},
	{"LeaveRequestId":"766c9732-e2d1-46d1-ae3e-a74c560bb8e6", "EmployeeId":"ffff9732-e2d1-46d1-ae3e-a74c560bb702",
	“EmployeeName”:”Karina”,
	"DateFrom":"\/Date(1316700000000+0200)\/",
	"DateTo":"\/Date(1316700000000+0200)\/",
	"LeaveTypeId":"1"
	”LeaveTypeName”:”Vacation”}]

Decide leave requests
*********************

When a planner wishes to approve or deny a leave request he can update the corresponding leave request. Available to Planner only.

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /LeaveRequests/{requestId}/?securityToken={token}
	Request (JSON)
	{ "LeaveRequestId": "<requestId>",
	"IsApproved": “<bool>”,
	"MoveShiftsToVacant":” <bool>”,
	"SmsComment": "<comment>"}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

List of leave types
*******************

This resource returns a list of the currently available leave types for use by the leaverequest service. Available to all roles, employees can only choose between a subset of leavetypes, as specified in Tamigo.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Leave/LeaveRequestTypes/?securityToken={tokenId}&employeeId={employeeId}")

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	[{"LeaveTypeId":"766c9732-e2d1-46d1-ae3e-a74c560bb8e6", "Name":"Ferie"},
	...
	{"LeaveTypeId":"aaaaa732-e2d1-46d1-ae3e-a74c560bb8e6", "Name":"Syg"}]

List of employees from department
*********************************

Returns the list of employees you can register absence on.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Leave/Employees/?securityToken={token}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

Requests Service
----------------
The request service offers the Planner an option to decide on bids from his Employees.
Pending bids for vacant shifts
This resource returns a list of pending bids. Available for Planners


**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Requests/?securitytoken={sessionToken}

**Response**

.. code-block:: javascript

	Response (JSON)
	[{
	"Comment":"",
	"EndDateTime":"\/Date(1324220400000+0100)\/",
	"IsAccepted":null,
	"RequestId":"6a957dc0-bdc8-47bf-9a0e-5f4d936f85da",
	"RequesterId":"38f02810-6ac4-40b4-b238-39236c826118",
	"RequesterName":"Peter",
	"ShiftHours":7.25,
	"StartDateTime":"\/Date(1324191600000+0100)\/",
	"ProviderId":"00000000-0000-0000-0000-000000000000",
	"ProviderName":"Ledig",
	“Violate11HourRule”:”false”
	},
	...
	{
	"Comment":"",
	"EndDateTime":"\/Date(1324220400000+0100)\/",
	"IsAccepted":null,
	"RequestId":"f2a77510-c92b-4543-984b-c7323fc3311b",
	"RequesterId":"38f02810-6ac4-40b4-b238-39236c826118",
	"RequesterName":"Peter",
	"ShiftHours":7.25,
	"StartDateTime":"\/Date(1324191600000+0100)\/",
	"ProviderId":"00000000-0000-0000-0000-000000000000",
	"ProviderName":"Ledig",
	“Violate11HourRule”:”false”
	}]

Decide bid for shift
********************

Approve or reject a bid or shift exchange. Available to Planner, Administrator

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /Requests/{requestId}/?securitytoken={sessionToken}
	{“RequestId”:”<requestID>”,”Approved”:<true/false>}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK


Contact Service
---------------
The contact service returns a list of contact information for employees and planners in the current department.
Contact List
************
This resource returns a list of available contacts, including email addresses and phone numbers.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Contacts/?securitytoken={sessionToken}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"Email":"karina@tamigo.com","FirstName":"Andreas T","ImageUrl":"\/images\/small\/Andreas T_Untitled.png","LastName":null,"MobilePhone":"+4511111111"},
	...
	{"Email":"peter@tamigo.com","FirstName":"Ulrik","ImageUrl":"","LastName":null,"MobilePhone":"+4511111111"}]

Departments Service
-------------------
The department service is used for operations to retrieve a list of departments for the company, and updating of the default department for a User.
All Departments
***************
Get a list of the currently logged in user’s available departments.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Departments/?securitytoken={sessionToken}
	Content-Type:application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"DepartmentId":"223a4516-41d5-4327-97ca-65c67d6b20a0",
	"IsDefault":false,"Name":"Afdeling 1"},
	...
	{"DepartmentId":"c4dcc6f2-ff52-4a22-b29d-aae817b5c8e7",
	"IsDefault":false,"Name":"Slik"}]

Set Default department
**********************
In v1 of our iPhone application you change the department that your currently looking at by setting the default department.

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /departments/c4dcc6f2-ff52-4a22-b29d-aae817b5c8e7/?SecurityToken={sessionToken}
	Content-Type: application/json
	{"DepartmentId":"c4dcc6f2-ff52-4a22-b29d-aae817b5c8e7","IsDefault":true}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK

Revenue Over Day Service
------------------------
The revenue over day service is used to upload data for forecasting purposes.

* AmountTypeId 1 is Revenue and 2 is Customers. In the future this can be expanded.
* StatusTypeId 1 is Actual data and 2 is Forecast data. 2 can be used to upload your own forecast data.
Notice that the types has to exist for the upload to succeed.
Notice that the time span for a single unit has to be less than 24 hours, more than 15 minutes and the upload will fail, if the span cannot be divided into 15 minute spans.
Upload single data unit
***********************
Upload actual or forecast data for a single department in a single period.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /Revenues /period/single/?securityToken={tokenId}
	Content-Type:application/json
	{"Amount":"<float>","AmountTypeId":"[1|2]", "End":"<DateTime>", "PosStoreId":"<string>", "Start":"<DateTime>", "StatusTypeId":"[1|2]"}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"

Upload list of data units
*************************
Upload actual or forecast data for multiple departments or/and multiple periods.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /Revenues /period/list/?securityToken={tokenId}
	Content-Type:application/json
	[{"Amount":"<float>","AmountTypeId":"[1|2]", "End":"<DateTime>", "PosStoreId":"<string>", "Start":"<DateTime>", "StatusTypeId":"[1|2]"},
	...
	{"Amount":"<float>","AmountTypeId":"[1|2]", "End":"<DateTime>", "PosStoreId":"<string>", "Start":"<DateTime>", "StatusTypeId":"[1|2]"]

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"

ICal service
------------
The iCal service returns the users “ical link” this is his personal calendar in iCal format
Get ical link
*************

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Calendar/link/?securityToken={tokenId}
	Content-Type:application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"

Upload list of data units
*************************
*Upload actual or forecast data for multiple departments or/and multiple periods.*

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /Revenues /period/list/?securityToken={tokenId}
	Content-Type:application/json
	Result: "https:\/\/services.tamigo.com\/Calendar\/208ef757-4d91-4219-a9d6-5761d5c03bf3\/Calendar.ics"

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"

Calendar comments service
-------------------------
This service returns the “comments” that a day in the roster have attached. It consists of two short texts like “We are closed” or “Holiday” and a long text that can be several hundred chars long.
It is advised to show the short comments at the top of the roster at all time. The long text should be visible with the press of a button.
Get the calendar comment
************************

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /comments/day/{date}/?securityToken={tokenId}
	Content-Type:application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"

Upload list of data units
*************************
Upload actual or forecast data for multiple departments or/and multiple periods.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /Revenues /period/list/?securityToken={tokenId}
	Content-Type:application/json
	Result: "https:\/\/services.tamigo.com\/Calendar\/208ef757-4d91-4219-a9d6-5761d5c03bf3\/Calendar.ics"

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	{
	"CompanyCommentShort":"Kort besked",
	"DepartmentCommentLong":"Rigtig lang besked!",
	"DepartmentCommentShort":"Kort besked"
	}

Roles service
-------------
The Roles service returns the roles that the user have in the specific department.
Get Roles
*********

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Roles/?securityToken={tokenId}
	Content-Type:application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"
	[{"Role 1"}, {"Role 2"}, {"Role 3"}]

The Set Role service, setting in the token the new role selected by the user.
Set Role
********

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /Roles/?securityToken={tokenId}
	Content-Type:application/json
	Body
	“planner”

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"

Company service
---------------

The Get Employee Companies returns all the user’s companies.

Get Employee Companies
**********************

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Companies/EmployeesCompanies/?securityToken={tokenId}
	Content-Type:application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"
	[{"CompanyId":"766c9732-e2d1-46d1-ae3e-a74c560bb8e6", "Name":"Company Name"},
	...
	{" CompanyId ":"aaaaa732-e2d1-46d1-ae3e-a74c560bb8e6", "Name":"Company Name "}]

Breakcode service
-----------------
The Get Breakcodes returns all the company’s breakcodes.

Get Breakcodes
**************

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /Breakcodes/?securityToken={tokenId}
	Content-Type:application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK "true"
	[{"BreakCodeId":"766c9732-e2d1-46d1-ae3e-a74c560bb8e6", "Code":"Q", "Name":"Q", "Description":"BreakCode1", "Hours":"5", "MinimumHours":"5"}]},
	...
	{" BreakCodeId ":"aaaaa732-e2d1-46d1-ae3e-a74c560bb8e6"," Code ":"P", "Name":"P", "Description":"BreakCode2", "Hours":"8", "MinimumHours":"8"}]

Shift Activity service
----------------------

Actual shifts – arbitrary day
*****************************
This resource returns a list of the actuals shifts for the day including employees that are on leave, signified by an associated activity. Available for all. If department is equal to “all”, shift from all rosters in all departments are returned. If the endTime is equal to 1/1/1900 it means that the endtime is empty at the moment.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /actualshifts/ /day/{date}/?securitytoken={sessionToken}(&departmentId={departmentId})
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"},
	...
	{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/",
	"ShiftActivityId":"376a579a-acc9-4020-b313-d2e826b60b14",
	"ShiftId":"376a579a-acc9-4020-b313-d2e826b60b14"}]

Actual plan day status
**********************
This service returns a true or false if the actual plan day is closed or open.
If closed = true. If Open = false.

**Request**

.. code-block:: javascript

	Request (XML)
	GET
	/actualshifts/ status/day/
	{date}/?securitytoken={sessionToken}(&departmentId={departmentId})
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":’true’,"Success":true}

Actual shifts – actual shifts by employee
*****************************************
This resource returns a list of the actual plan for the past 60 days for an employee selected.
If department is equal to “all”, actual shifts from all employees departments is returned. If Departmentid is left empty defaultdepartment will be chosen. Or you could specify a departmentid.
Takese an employeeid. If the endTime is equal to 1/1/1900 it means that the endtime is empty at the moment.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /actualshifts /past/{date}/?securitytoken={sessionToken}(&departmentId={departmentId}&employeeId={employeeId})
	Content-Type: application/json

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	Content-Type: application/json; charset=utf-8
	[{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"},
	...
	{"BreakCode":"Q","Comment":"",
	"DepartmentName":"Ny Bistro","EmployeeName":"",
	"EndTime":"\/Date(1316700000000+0200)\/",
	"StartTime":"\/Date(1316671200000+0200)\/"}]

Close Actual Day
****************
Use this to close a day of your actual shift plan. Supply the day and the department. Is available for planner and administrator.

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /actualshifts/ close/day/{date}/?securitytoken={sessionToken}(&departmentId={departmentId})
	}
	{
	"Departmentid": "fb60311f-e88c-4a6a-9bc4-6ad09fd6a7b5",
	"Date": "\/Date(1355986847000+0100)\/"
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":’true’,"Success":true}

Open Actual Day
***************
Use this to close a day of your actual shift plan. Supply the day and the department. Is available for administrator, and if the settings is enabled in the Tamigo Web application. It will also be possible for planners to open a day in the current wage period, to see if it is possible use the service “Can planner reopen” .

**Request**

.. code-block:: javascript

	Request (JSON)
	PUT /actualshifts/ open/day/{date}/?securitytoken={sessionToken}(&departmentId={departmentId})
	{
	"Departmentid": "fb60311f-e88c-4a6a-9bc4-6ad09fd6a7b5",
	"Date": "\/Date(1355986847000+0100)\/"
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":’true’,"Success":true}

Can Planner Reopen Actual Day
*****************************
This service returns if it is possible for planners to reopen closed actual plans, in the current wage period.

**Request**

.. code-block:: javascript

	Request (JSON)
	GET /actualshifts/reopen/day/{date}/?securitytoken={sessionToken}
	{
	"Date": "\/Date(1355986847000+0100)\/"
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{Success":true}

Copy authorized plan to actual plan – For a Day
***********************************************
This copies an authorized plan to an actual plan. This is only allowed for administrators or planners.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST /actualshifts/ copy/day/{date}/?securitytoken={sessionToken}(&departmentId={departmentId})
	{
	"Departmentid": "fb60311f-e88c-4a6a-9bc4-6ad09fd6a7b5",
	"Date": "\/Date(1355986847000+0100)\/"
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{"Message":’’,"Success":true}

Services for partner login
--------------------------
Tamigo offers special services for partners. They will be listed below. Only a partner login has access to these services.
Create new company
This service creates a new company, with one administrator. Fill out the different parameters to set culture, package and name, email etc. for the administrator.

**Request**

.. code-block:: javascript

	Request (JSON)
	POST / Companies/?securitytoken={sessionToken}
	Body:
	Company
	{
	"Name": "This is the company name",
	"ContactPerson": "This is the administrators name",
	"Address": "This is the Address",
	"City": "This is the city",
	"Email": "This is the email for the admin login",
	"Password": "for the login",
	"Phone": "This is the phone of the administrator",
	"zipCode": "This is the zipcode",
	"Culture": "This is the culture of the company",
	"PackageType": "package type is ranged from 1-5",
	}

**Response**

.. code-block:: javascript

	Response (JSON)
	HTTP/1.1 200 OK
	{Message:”Company Created!” Success":true}

Touch Services for smartphones
------------------------------
Touch services for the smartphones are used in order to Check in/out, similar as in the main Touch App.
TouchCehckIn
************
The TouchCheckIn method is allowing the user to check in from a smartphone device.

**Request**

.. code-block:: javascript

	Request (JSON) 
	POST/Attendance/TouchCheckIn/?securityToken={tokenId}
	Body:
	Null

TouchCheckIn is returning an AttendanceInformation type of object, which contains a CheckInTime updated with the dateTimeCheckIn.

**Response**

.. code-block:: javascript

	Response (JSON)
	{
	"CheckInTime": "12:37",
	"CheckOutTime": "null",
	"CurrentTime": "12:48:32",
	"CurrentStatus": "AllowCheckOut",
	"EnableShiftBreakCodePanel": "true",
	"IsCheckedInElsewhere": "false",
	}

TouchCheckOut
*************
Similar to the TouchCheckIn method, the TouchCheckOut calls the CheckOut service and returns an Attendance Information object, updated with the CheckOutTime. If breaktime is empty insert null.

**Request**

.. code-block:: javascript

	Request (JSON) 
	POST /Attendance/TouchCheckOut/{breakcodeid}/?securityToken={tokenId}
	Body:
	Null

**Response**

.. code-block:: javascript

	Response (JSON)
	{
	"CheckInTime": "12:37",
	"CheckOutTime": "15:45",
	"CurrentTime": "15:45:00",
	"CurrentStatus": "AllowCheckIn",
	"EnableShiftBreakCodePanel": "false",
	"IsCheckedInElsewhere": "false",
	}

GetTouchStatus
**************
It returns an AttendanceInformation type of object, updated with the latest information.

**Request**

.. code-block:: javascript

	Request (JSON) GET /Attendance/GetTouchStatus/?securityToken={tokenId}
	Body:
	Null

**Response**

.. code-block:: javascript

	Response (JSON)
	TouchCheckIn is returning an AttendanceInformation type of object, which contains a CheckInTime updated with the dateTimeCheckIn.
	{
	"CheckInTime": "12:37",
	"CheckOutTime": "null",
	"CurrentTime": "12:48:32",
	"CurrentStatus": "AllowCheckOut",
	"EnableShiftBreakCodePanel": "true",
	"IsCheckedInElsewhere": "false",
	}

GetBreakCodes
*************
It returns a list of AttendanceBreakCode type of objects, with the breakcodes corresponding to a certain employee.
Breakcodeid Is optional, if sent the service will return a list with the breakcodeid as the selected one.

**Request**

.. code-block:: javascript

	Request (JSON) 
	GET /Attendance/GetBreakCodes/?securityToken={tokenId}&breakcodeid={breakcodeid}
	Body:
	Null

**Response**

.. code-block:: javascript

	Response (JSON)
	[{"BreakCodeID":"1",
	"Code":"12",
	"Enabled":"false",
	"Hours":"0.12",
	"Name":"Q",
	"Selected":"true"},
	...
	[{"BreakCodeID":"4",
	"Code":"30",
	"Enabled":"false",
	"Hours":"1.5",
	"Name":"R",
	"Selected":"true"}]

UpdateBreakCode
***************

This method is used in the applications when an user is selecting a certain breakcode. Its purpose is to change the .Selected property of the selected breakcode. The method returns a ResponseMessage, which has the Success=true in case the breakcode was successfully updated, and false otherwise.

**Request**

.. code-block:: javascript

	Request (JSON) POST /AttendanceService/UpdateBreakCode/{breakcodeid}/?securityToken={tokenId}
	Body:
	Null

**Response**

.. code-block:: javascript

	Response (JSON)
	{Message:””, Success:”true”}

GetTouchAccess
**************

The GetTouchAccess method is used in order to decide whether a device may or may not have access to the Touch application.
In case the company does not have the option AlwaysCheckIPOnTouchLogin enabled, the device using the Touch application will always be able to use it. Otherwise, a set of checks on the IP address is being made:
used when components are first initialized, in the smartphone versions.

**Request**

.. code-block:: javascript

	Request (JSON) GET/AttendanceService/GetTouchAccess/ securityToken={tokenId}
	Body:
	Null

**Response**

.. code-block:: javascript

	Response (JSON)
	{Message:”” Success:”false”}

