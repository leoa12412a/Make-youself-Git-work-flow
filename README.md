# Git-Alias-Commands-Hooks
# 1.git alias
  alias的目的在簡化程式碼，例如你可以把status設定成st<br>
  方法很簡單只需要在git內輸入 $ git config --global alias.st status<br>
  這個輸入相當於直接輸入在.gitconfig裡面(.gitconfig的位置可以使用git config --global --edit來查找)<br>
  ![image]()</br></br>
# 2.git custom commands
   custom command顧名思義就是自定義屬於自己的指令，關於如何設定自己的指令有3個步驟
   1. 在/Users/user/Documents/gitScripts目錄下，建立一個git-refresh(你的指令)，檔案不需要副檔名</br>
      ![image]()</br></br>
   2. 將剛剛儲存檔案的路徑加入系統的環境路徑
        export PATH=$PATH:/Users/user/Documents/gitScripts(位置隨資料位置改變)
        </br>
   3. 給予資料可執行的權限(chmod +x git-refresh)並重新載入剛剛輸入的資料
        source ~/.bash_profile
        </br>
# git hooks
  git hooks是一個在某個事件發生時，會自動觸發hooks的腳本，腳本的內容都可以自行定義，你可以想像成每次你做commit，監聽commit的hook就會把你的資料勾過去檢察。而hooks裡面又分為 server(伺服器)端 跟 client(用戶)端 ，而在做git init 或是 git init --bare(遠端儲存庫)後，分別在/.git/hooks 和 /hooks裡面都有已經預設好的腳本，這是 Git 提供給我們的 sample script，讓我們參考用的，script 的檔名就是對應的事件，要使用hooks的時候只要把需要的副檔名(.sample)刪掉可以了。
  
  下面介紹各種hooks的功能以及觸發的時機
  主要分成用戶端和伺服器端兩大類
  
   用戶端hooks
   1. pre-mommit
   
      這個腳本會在每次你執行 git commit 時執行，藉由hook來判斷是否這個commit可以被提交，所以你可以自定義檢查在這個腳本內。
      預設腳本會檢查 句尾是否空白字符、是否有空白的行、行首TAB後緊貼一個空白、文件是否是中文名稱
      
