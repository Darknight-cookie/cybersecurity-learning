Path Traversal可以使攻擊者讀取運行於伺服器上的各種檔案  
甚至可以透過改寫、寫入文件使得獲得控制權


## URL Path Traversal
若一個含有圖片的網站，可能會用HTML載入圖像，例如：  
```HTML
<img src="/loadImage?filename=123.png">
```

若文件123.png存於`/var/www/images/`，則應用程式會從`/var/www/images/123.png`此路徑讀取該文件  
若沒有對於Path Traversal進行防禦，則攻擊者可能會從URL中來檢索秘密文件，例如`/etc/passwd`  
```text
https:// ~~~ / loadImage?filename=../../../etc/passwd
```  
`../`為目錄結構中上升一層，也就是上一頁的意思  
實際上讀取的文件路徑為`/var/www/images/../../../etc/passwd`，我們透過三個`../`回到根目錄，最後讀到的檔案會是`/etc/passwd`  

## 常見情況
可能可以直接使用絕對路徑，不需要做任何繞過  
```text
filename=/etc/passwd
```
有時系統會預設資料夾開頭，可以用`../`來達到預期的路徑  
```text
filename=/var/www/images/../../../etc/passwd
```
若系統有對`../`字做阻檔，可能可以使用`....//`來試著繞過  
`....//`的意義為他阻檔了字串`../`後最後執行出來的還是`../`  
```text
filename=....//....//....//etc/passwd
```
有時伺服器會移除任何編歷路徑的序列，這時可以對URL進行編碼，例如`%2e%2e%2f`，和`%252e%252e%252f`各種非標準編碼  
`%2e`為`.`的編碼，`%2f`為`/`的編碼。`%25`為`%`的編碼，做一次decode會變成`%2e%2e%2f`，做第二次decode就會是`../` 
```text
filname=%2e%2e%2f%2e%2e%2f%2e%2e%2fetc/passwd
```
如果伺服器有需要以預期的檔名為結尾，則可以使用`%00`放在檔名前面，`%00`為字串終止  
```text
filename=../../../etc.passwd%00.png
```
