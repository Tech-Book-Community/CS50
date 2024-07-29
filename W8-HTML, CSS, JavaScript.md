# Week 8. HTML, CSS, JavaScript   

# 課程與連結
- [Week 8 HTML, CSS, JavaScript](https://cs50.harvard.edu/x/2024/weeks/8/)

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/ciz2UaifaNM/0.jpg)](https://www.youtube.com/watch?v=ciz2UaifaNM)

# 目錄：
- [The Internet & Routers](#The-Internet-and-Routers)
- [DNS and DHCP](#DNS-and-DHCP)
- [HTTP](#HTTP)
- [HTML](#HTML)
- [Regular Expressions](#Regular-Expressions)
- [CSS](#CSS)
- [JavaScript](#JavaScript)


## The Internet and Routers

The world nowadays is filled with routers.
在網際網路的世界，路由無所不在。

其中路由在網路世界裡扮演的角色，好比兩台電腦(或伺服器)之間溝通的橋樑，經由路由可以實現將資料自 A 電腦成功傳送到 B 電腦；

路由常見的功能主要有(參自維基百科):

    · 確定路徑：路由器可以決定從來源到目的地所採用的路徑，這個作業稱為路由。
    · 資料轉傳：路由器會將資料轉傳至所選路徑的下一個裝置，重複這個過程，最終資料可以抵達目的地。運算裝置和路由器可能位於相同的網路或不同的網路。
    · 負載平衡：路由器有時會用多個不同路徑，傳送相同資料封包副本。其目的是為了減少因資料遺失而造成錯誤、並建立備援及管理流量。

為了確保路由能夠順利完成任務，還需倚靠 TCP/IP 這兩組通協定(protocols)的規範，分別定義:

TCP: 確保資料傳輸的封包(Packet)能被完整傳遞，當封包丟失會再要求重新發送；依據埠號(Ports)區分服務內容。

IP: 主要記錄目的地地址及發送地址，以四組十進位數字組成，長度為 32 位元。

## DNS and DHCP

域名系統(DNS)也廣泛遍佈在網路世界中，其目的是完成域名解析，以幫助路由能清楚目的地及發送地位址 IP 位置。

動態主機設定協定(DHCP)為幫助連線裝置(e.g. 電腦、手機、平板)動態分配一組供連線使用的 IP 而無須再透過人工的方式完成設定。


## HTTP

超文本傳輸協定(HTTP)為網頁開發者最廣泛使用的通訊協定，此協定規範了客戶端（使用者）和伺服器端（網站）之間 請求(Request) 和 應答 (Response)的標準。

HTTP 常見的請求包含 GET 與 POST 而這兩者的差別分別為前者負責向伺服器索求資源，後者則是向伺服器發送資料(e.g. 新增一筆或多筆資料)。

我們在使用 HTTP 發送請求的 連結(URL) 與瀏覽電腦檔案使用的檔案路徑結構類似，只是在 URL 的最後都常會隱藏(或省略)明確對應的檔案名稱。 

## HTML

超文本標記語言(HTML)是現代網頁透過瀏覽器渲染畫面的主要內容，由標籤(tag)及屬性(attributes)所構成。

此外標籤的層次結構可以由 DOM tree 的結構來呈現(下圖右半邊):


<img src ="https://cs50.harvard.edu/x/2024/notes/8/cs50Week8Slide065.png" width = 80% style="display: block; margin: auto"> 

## Regular Expressions

正規表示式(Regular Expressions)是一種可以用來驗證使用者輸入的資料(或從中擷取部分資訊)的一種表達模式，通常為一種特殊的字串，且能被廣泛與現代的程式語言結合使用(e.g. Python、JavaScript...)。

## CSS

階層樣式表(Cascading Stylesheets；CSS)是讓 HTML 的網頁外觀有更具體的描述，用於塑造網站的特殊風格。

例如這段文字要用一般的黑色，或是改用紅色標明重點？某段重要內容應該置於畫面的何處？想用什麼背景圖片及顏色裝飾你的網站？

## JavaScript

若想為由 HTML 及 CSS 組成的網頁增添互動性，學好 JavaScript 必不可少。

JavaScript 原本存在於瀏覽器操作 HTML 網頁的 DOM 元素，而 Node.js 的問世讓 JavaScript 也可以在自己的電腦上運行，及搭配 IDE 的使用來進行各種專案開發，包括與資料庫進行溝通等操作。
