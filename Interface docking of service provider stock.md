## Explain

> ### The meaning of return code in the returned data（be in common use,every interface will return）

| return code |   return msg      |               remarks               |
| ------------- |----------------|-------------|
|  0   |ok       | success
| -1   |system error         | system Exception
| -1 | sp already forbidden | Service provider has been disabled |
| -1 | ip invalid      | The access IP is illegal |
| -1 | no permission  | permission denied |

# 1、get acess token(you need to provide the acess ip to sekorm,it will Authorized by IP)
> 
>access from： https://wwww.sekorm.com/serviceGate/getAccessToken  <br/>

> request method：  GET/POST

>Interface description ：
get acess token

> ### Request parameter description：

| parameter        | data type           | Is required | The limited length | remarks | Example |
| ----------  |-------------|-------------|-------------|-------------|-------------|
| accessToken | String      | yes |  |The account assigned  by sekorm to the service provider developers | |
| appSecret   | String      | yes |  |The password assigned  by sekorm to the service provider developers|  |

> ### Return result description：

| parameter        | data type           | Is required | remarks | Example |
| ----------  |-------------|-------------|-------------|-------------|
| code        | Integer | yes | return code | -1 |
| msg         | String      | yes | decription of the return code | no permission |
| data        | List      | yes | return data | null |

> ### The meaning of return code in the returned data

| return code|   return msg                  |               remarks               |
| ------------- |----------------|-------------|
|  0   |ok       | success
| -1   |system error         | system Exception
| -1 | sp already forbidden | Service provider has been disabled |
| -1 | ip invalid      | The access IP is illegal |
| -1 | no permission  | permission denied |

>Example of failure return：

{"code":-1,msg:"sp already forbidden",data:null}

> Example of successful return:

```JSON
{
  "code": 0,
  "msg": "ok",
  "data": {
    "expiresIn": 3600,
    "accessToken": "sgOQYs6UKqp/6EUP6e7QEgzcbYgow2KMYveuQnjHtKVeUIh0rJ0DrJE9/X5A0lohTQ7o37JbrOTeHsYbdpOPMw=="
  }
}
```
### Description of data returned successfully

* expiresIn :Validity period，Unit: second；
* accessToken：access token，Credentials accessed by other interfaces;
*  All interfaces need to use the accessToken to access 
* Please don't get accessToken frequently，it can be reused within the validity period

### remarks
serviceGate  The project will check the visited IP address，If the visited IP is not authorized, it will not be accessible

# 2  Interface for batch sync stock（Mainly used for initial one-time）
>  Full stock synchronization interface

> access from： https://wwww.sekorm.com/serviceGate/pnarttnumber/pnStock <br/>

> request method：  POST

> Request parameter description：

|     parameter      |   data type   | Is required | The limited length |          remarks           |                 Example              |
|-------------|--------|----|------|-----------------------|--------------------------------------------|
| accessToken | String | yes  | 100  | certificate,Sekorm assigns to the service provider developers,The developer obtains by calling the interface | xSCMJVMkAYwMAZrIDF4l7tLA55YGZ0VDX7sp0FCfX4pY/af3DjQpD9cdYRczz3xzdqsDRjfoJlt2W9fH9guC2gA==                                             |
| data        | String | yes  |      | Basic data (dictionary), json array        | [ {"pnCode": "ABCDE","brandName": "AMS","stockQuantity": 1000}] |

>Description of object fields in data

| field        | data type           | Is required | remarks|
| ----------  |-------------|-------------|-------------|
| brandName        | String           | yes |Brand name，eg：AMS |
| pnCode        | String           | yes| Material name |
| stockQuantity        | integer           | yes| Quantity in stock：100 |

When the code code is 0, it means successful return，

>  Example of successful return：

{"code":0,"msg":"OK","data":null}
eg：
{"code":0,"msg":"OK","data":null}


# 3 Interface for Single sync stock（Real-time notification）
>  Please notify in real time

> access from： https://wwww.sekorm.com/serviceGate/stock/updQuantity <br/>

> request method：  POST

> Request parameter description：

|      parameter       |   data type    | Is required | The limited length |          remarks           |     Example        |
|---------------|---------|----|------|-----------------------|-----------------------------------------------------|
| accessToken   | String  | yes  | 100  | certificate,Sekorm assigns to the service provider developers,The developer obtains by calling the interface | xSCMJVMkAYwMAZrIDF4l7tLA55YGZ0VDX7sp0FCfX4pY/af3DjQpD9cdYRczz3xzdqsDRjfoJlt2W9fH9guC2gA== |
| pnCode        | String  | yes  | 50   | Material name                    | 1211LL的ON2                                                                                |
| brandName     | String  | yes  | 50   | Brand name                    | AMS                                                                                       |
| stockQuantity | Integer | yes  |      | Quantity in stock                  | 100     

>  Example of successful return：

{"code":0,"msg":"OK","data":null}
eg：
{"code":0,"msg":"OK","data":null}

# 4 Interface for Inventory reconciliation
>  Reconciled once a day,Incoming all changed data of the day for comparison,If the data is incorrect, notify the person in charge to intervene

> access from: https://wwww.sekorm.com/serviceGate/stock/compare <br/>

> request method：  POST

> Request parameter description：

|     parameter      |   data type   | Is required | The limited length |          remarks           |                 Example              |
|-------------|--------|----|------|-----------------------|--------------------------------------------|
| accessToken | String | yes  | 100  | certificate,Sekorm assigns to the service provider developers,The developer obtains by calling the interface | xSCMJVMkAYwMAZrIDF4l7tLA55YGZ0VDX7sp0FCfX4pY/af3DjQpD9cdYRczz3xzdqsDRjfoJlt2W9fH9guC2gA==                                             |
| data        | String | yes  |      | Basic data (dictionary), json array        | [{"pnCode": "PNCODE1","brandName": "AMS","stockQuantity": 100}] |

>Description of object fields in data

| field        | data type           | Is required | remarks|
| ----------  |-------------|-------------|-------------|
| brandName        | String           | yes |Brand name，eg：AMS |
| pnCode        | String           | yes| Material name |
| stockQuantity        | integer           | yes| Quantity in stock：100 |


>  Example of successful return：

{"code":0,"msg":"OK","data":null}
eg：
{"code":0,"msg":"OK","data":null}


