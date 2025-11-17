# RESEARCH

## Tools

<https://hackmd.io/hCPPeGgQQ9asyrzKUwq3mQ>

## Reference

{%preview https://blog.cloud-ace.tw/networking-website/load-balance/gcp-load-balancer-introduction/ %}

## Experience

### 實驗_nginx負載平衡

* 三台虛擬機模擬負載平衡。兩台裝後端伺服器，一台當Nginx負載平衡管理者，發送模擬請求
* 虛擬機配置<https://hackmd.io/xoRV-3XORUWK-cIezHJM2w>

### 實驗_Jmeter壓力測試CMS登入

* Jmeter模擬CMS Contest Web Server大量使用者同時登入
* Jmeter配置<https://hackmd.io/bQAiUxHcRx-8BgEBr8ZWJQ>
* 問題 -> _xsrf
一直遇到403`'_xsrf' argument missing from POST`
才發現/login的body必須是`_xsrf, username, password`
_xsrf的值去瀏覽器複製就行
`F12 -> Application -> Cookies 複製貼到Value中`
  ![image](https://hackmd.io/_uploads/BkAti8ulZl.png)

