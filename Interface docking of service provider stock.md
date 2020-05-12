## Explain

> ### The explanation of return code as below: ( In general, every interface will return)

| return code |   return msg      |               Explanation               |
| ------------- |----------------|-------------|
|  0   |ok       | success
| -1   |system error         | system Exception
| -1 | sp already forbidden | Service provider has been disabled |
| -1 | ip invalid      | The access IP is illegal |
| -1 | no permission  | permission denied |

# 1、Obtain an access token (need to provide the access interface IP to SEKORM,  we will authorize according to IP you provided)
> 
>access from： https://wwww.sekorm.com/serviceGate/getAccessToken  <br/>

> request method：  GET/POST

>Interface description ：
get acess token

> ### Request parameter description：

| parameter        | data type           | Is required | The limited length | Explanation | Example values |
| ----------  |-------------|-------------|-------------|-------------|-------------|
| accessToken | String      | yes |  |SEKORM will assign accounts to the developers | |
| appSecret   | String      | yes |  |SEKORM will assign password to the developers.|  |

> ### Return result description：

| parameter        | data type           | Is required | Explanation | Example values |
| ----------  |-------------|-------------|-------------|-------------|
| code        | Integer | yes | return code | -1 |
| msg         | String      | yes | decription of the return code | no permission |
| data        | List      | yes | return data | null |

> ### The meaning of return code in the returned data

| return code|   return msg                  |               Explanation               |
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
The serviceGate project will detect the IP address of the accessor, if the IP address is not added to the list, it will not be accessible

# 2  Batch stock synchronization interface (mainly used for one-time initialization)
>  Full stock synchronization interface

> access from： https://wwww.sekorm.com/serviceGate/pnarttnumber/pnStock <br/>

> request method：  POST

> Request parameter description：

|     parameter      |   data type   | Is required | The limited length |          Explanation           |                 Example values              |
|-------------|--------|----|------|-----------------------|--------------------------------------------|
| accessToken | String | yes  | 100  | The credential information，the developer can obtain  through the access interface assigned by SEKORM. | xSCMJVMkAYwMAZrIDF4l7tLA55YGZ0VDX7sp0FCfX4pY/af3DjQpD9cdYRczz3xzdqsDRjfoJlt2W9fH9guC2gA==                                             |
| data        | String | yes  |      | Basic data (dictionary), json array        | [ {"pnCode": "ABCDE","brandName": "AMS","stockQuantity": 1000}] |

>Description of object fields in data

| field        | data type           | Is required | Explanation|
| ----------  |-------------|-------------|-------------|
| brandName        | String           | yes |Brand name，eg：AMS |
| pnCode        | String           | yes| Part No# |
| stockQuantity        | integer           | yes| Quantity in stock：100 |

When the code is 0, it means successful return，

>  Example of successful return：

{"code":0,"msg":"OK","data":null}
eg：
{"code":0,"msg":"OK","data":null}


# 3 Interface for Single sync stock（Real-time notification）
>  Please notify in real time

> access from： https://wwww.sekorm.com/serviceGate/stock/updQuantity <br/>

> request method：  POST

> Request parameter description：

|      parameter       |   data type    | Is required | The limited length |          Explanation           |     Example values        |
|---------------|---------|----|------|-----------------------|-----------------------------------------------------|
| accessToken   | String  | yes  | 100  | The credential information，the developer can obtain  through the access interface assigned by SEKORM. | xSCMJVMkAYwMAZrIDF4l7tLA55YGZ0VDX7sp0FCfX4pY/af3DjQpD9cdYRczz3xzdqsDRjfoJlt2W9fH9guC2gA== |
| pnCode        | String  | yes  | 50   | Material name                    | 1211LL的ON2                                                                                |
| brandName     | String  | yes  | 50   | Brand name                    | AMS                                                                                       |
| stockQuantity | Integer | yes  |      | Quantity in stock                  | 100     

>  Example of successful return：

{"code":0,"msg":"OK","data":null}
eg：
{"code":0,"msg":"OK","data":null}

# 4 Interface for stock reconciliation
>  Reconciled once a day,Incoming all changed data of the day for comparison,If the data is incorrect, notify the person in charge to intervene

> access from: https://wwww.sekorm.com/serviceGate/stock/compare <br/>

> request method：  POST

> Request parameter description：

|     parameter      |   data type   | Is required | The limited length |          Explanation           |                 Example values              |
|-------------|--------|----|------|-----------------------|--------------------------------------------|
| accessToken | String | yes  | 100  | The credential information，the developer can obtain  through the access interface assigned by SEKORM. | xSCMJVMkAYwMAZrIDF4l7tLA55YGZ0VDX7sp0FCfX4pY/af3DjQpD9cdYRczz3xzdqsDRjfoJlt2W9fH9guC2gA==                                             |
| data        | String | yes  |      | Basic data (dictionary), json array        | [{"pnCode": "PNCODE1","brandName": "AMS","stockQuantity": 100}] |

>Description of object fields in data

| field        | data type           | Is required | Explanation|
| ----------  |-------------|-------------|-------------|
| brandName        | String           | yes |Brand name，eg：AMS |
| pnCode        | String           | yes|Part No# |
| stockQuantity        | integer           | yes| Quantity in stock：100 |


>  Example of successful return：

{"code":0,"msg":"OK","data":null}
eg：
{"code":0,"msg":"OK","data":null}


