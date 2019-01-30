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
        export PATH=$PATH:/Users/user/Documents/gitScripts(位置資料放的位置改變)
   3. 給予資料可執行的權限(chmod +x git-refresh)並重新載入剛剛輸入的資料
        source ~/.bash_profile
