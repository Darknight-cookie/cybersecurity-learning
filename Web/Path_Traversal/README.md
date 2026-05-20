Path Traversal可以使攻擊者讀取運行於伺服器上的各種檔案
甚至可以透過改寫、寫入文件使得獲得控制權


## URL Path Traversal
若一個含有圖片的網站，可能會用HTML載入圖像，例如：
```HTML
<img src="/loadImage?filename=123.png">
```

若文件123.png存於`/var/www/images/`，則應用程式會從`/var/www/images/123.png`此路徑讀取該文件
若沒有對於Path Traversal進行防禦，則攻擊者可能會從URL中來檢索秘密文件，例如`/etc/passwd`
`https:// ~~~ / loadImage?filename=../../../etc/passwd`
`../`為目錄結構中上升一層，也就是上一頁的意思
實際上讀取的文件路徑為`/var/www/images/../../../etc/passwd`，我們透過三個`../`回到根目錄，最後讀到的檔案會是`/etc/passwd`

