# Jmeter配置

![image](https://hackmd.io/_uploads/BJWfcI_xbg.png)

### CSV Data Set Config
* user.csv存放所有要測試使用者的帳號密碼 格式:`username,password`
 
### HTTP Header Manager

* 新增一個Header `Name=Cookie Value=_xsrf值`
  _xsrf值到瀏覽器 F12 -> Application -> Cookies 複製貼到Value中
  ![image](https://hackmd.io/_uploads/BkAti8ulZl.png)

### Thread Group

* Number of Threads (users):`1000`
* Ramp-up period (seconds):`1`
* Loop Count:`1`

### HTTP Request

* Protocol: `http`
* Server Name or IP : `CMS Contest Web Server的IP`
* Port Number: `8888` (CMS Contest Web Server Default Port Number)
* HTTP Request: `POST` Path: `/login` (測試登入)
* Parameters

    | Name       | Value           |
    |------------|-----------------|
    | _xsrf      | 從瀏覽器複製過來   |
    | username   | ${username}     |
    | password   | ${password}     |

### 其他效能圖 

想看什麼加什麼