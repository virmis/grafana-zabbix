# Grafana-Zabbix
## Zabbix API datasource for Grafana dashboard

![alt tag](https://cloud.githubusercontent.com/assets/4932851/7454206/34bf9f8c-f27a-11e4-8e96-a73829f188c4.png)


Query editor allows to add metric by step-by-step selection from host group, host, application dropdown menus.

![alt tag](https://cloud.githubusercontent.com/assets/4932851/7441162/4f6af788-f0e4-11e4-887b-34d987d00c40.png)
![alt tag](https://cloud.githubusercontent.com/assets/4932851/7441163/56f28f16-f0e4-11e4-9d46-54181c2a2e7e.png)
![alt tag](https://cloud.githubusercontent.com/assets/4932851/7441167/5f29cc94-f0e4-11e4-8d39-7580f33201f6.png)


## Installation

### Grafana 1.9.x

Download latest release and unpack into `<your grafana installation>/plugins/datasource/`. Then edit Grafana config.js:
  * Add dependencies
  
    ```
    plugins: {
      panels: [],
      dependencies: ['datasource/zabbix/datasource', 'datasource/zabbix/queryCtrl'],
    }
    ```
  * Add datasource and setup your Zabbix API url, username and password
  
    ```
    datasources: {
      ...
      },
      zabbix: {
        type: 'ZabbixAPIDatasource',
        url: 'http://www.zabbix.org/zabbix/api_jsonrpc.php',
        username: 'guest',
        password: ''
      }
    },
    ```
    
#### Note for Zabbix 2.2 or less
Zabbix API (api_jsonrpc.php) before zabbix 2.4 don't allow cross-domain requests (CORS). And you can get HTTP error 412 (Precondition Failed).
To fix it add this code to api_jsonrpc.php immediately after the copyright
```
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Headers: Content-Type');
header('Access-Control-Allow-Methods: POST');
header('Access-Control-Max-Age: 1000');

if ($_SERVER['REQUEST_METHOD'] === 'OPTIONS') {
	return;
}
```
before 
```
require_once dirname(__FILE__).'/include/func.inc.php';
require_once dirname(__FILE__).'/include/classes/core/CHttpRequest.php';
```
[Full fix listing](https://gist.github.com/alexanderzobnin/f2348f318d7a93466a0c).
For more info see zabbix issues [ZBXNEXT-1377](https://support.zabbix.com/browse/ZBXNEXT-1377) and [ZBX-8459](https://support.zabbix.com/browse/ZBX-8459).
