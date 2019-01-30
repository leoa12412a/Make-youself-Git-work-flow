# Git-Alias-Commands-Hooks
# 1.git alias
  alias的目的在簡化程式碼，例如你可以把status設定成st<br>
  方法很簡單只需要在git內輸入 $ git config --global alias.st status<br>
  這個輸入相當於直接輸入在.gitconfig裡面(.gitconfig的位置可以使用git config --global --edit來查找)<br>
  ![image](https://github.com/leoa12412a/Make-youself-Git-work-flow/blob/master/gitconfig.PNG)</br></br>
# 2.git custom commands
   custom command顧名思義就是自定義屬於自己的指令，關於如何設定自己的指令有3個步驟
   1. 在/Users/user/Documents/gitScripts目錄下，建立一個git-refresh(你的指令)，檔案不需要副檔名</br>
      ![image](https://github.com/leoa12412a/Make-youself-Git-work-flow/blob/master/git_refresh.PNG)</br></br>
   2. 將剛剛儲存檔案的路徑加入系統的環境路徑
        export PATH=$PATH:/Users/user/Documents/gitScripts(位置隨資料位置改變)
        </br>
   3. 給予資料可執行的權限(chmod +x git-refresh)並重新載入剛剛輸入的資料
        source ~/.bash_profile
        </br>
# git hooks
  git hooks是一個在某個事件發生時，會自動觸發hooks的腳本，腳本的內容都可以自行定義，你可以想像成每次你做commit，監聽commit的hook就會把你的資料勾過去檢查。而hooks裡面又分為 server(伺服器)端 跟 client(用戶)端 ，而在做git init 或是 git init --bare(遠端儲存庫)後，分別在/.git/hooks 和 /hooks裡面都有已經預設好的腳本，這是 Git 提供給我們的 sample script，讓我們參考用的，script 的檔名就是對應的事件，要使用hooks的時候只要把需要的副檔名(.sample)刪掉可以了。<br>
  ![image](https://github.com/leoa12412a/Make-youself-Git-work-flow/blob/master/hookfile.PNG)</br></br>
  
  
  
  下面介紹各種hooks的功能以及觸發的時機
  主要分成用戶端和伺服器端兩大類


   用戶端hooks
   
   1. pre-mommit
   
        這個腳本會在每次你執行 git commit 時執行，藉由hook來判斷是否這個commit可以被提交，所以你可以自定義檢查在這個腳本內。
        預設腳本會檢查 句尾是否空白字符、是否有空白的行、行首TAB後緊貼一個空白、文件是否是中文名稱
        ![image](https://github.com/leoa12412a/Make-youself-Git-work-flow/blob/master/pre-commit.PNG)</br></br>
      
   2. prepare-commit-msg
   
        觸發時機是在通過pre_commit之後，準備提交commit的資訊之前，可以在此規範團隊的commit資訊
     
   3. commit-msg
   
        commit-msg 跟 prepare-commit-msg 很像，會在使用者提交commit之後才觸發，也可以提醒使用者提交的資訊是否符合規範
   
   4. post-commit(目前版本已經沒有)
   
        觸發的時機是在commit結束後，這個腳本不會影響提交，但是可以在你做完提交後自動運行post-commit內的指令，比方說:每次
        提交都必須向老闆報告，就可以使用這個腳本來傳送email來報告。
    
   5. pre-rebase 
   
        會觸發在git rebase 之前，以確保rebase後不會出現一些不好的錯誤
         ![image](https://github.com/leoa12412a/Make-youself-Git-work-flow/blob/master/pre_rebase.PNG)</br></br>
        如果你想禁止 git rebase 的話，只要在腳本內輸入exit 1
        ![image](https://github.com/leoa12412a/Make-youself-Git-work-flow/blob/master/no_rebase.PNG)</br></br>
        
   6.  applypatch-msg
   
       指令是由git am調用，存有將用的補丁(patch)的提交消息，用途是在規範patch提交的消息，跟prepare-commit-msg類似
       
   7.  pre-applypatch
       
       指令是由git am調用，是在補丁應用但是還沒有commit之前運行的，git提供的腳本只是簡單的調用pre-commit，目的是為了讓git am 和
       git commit執行前都會經過pre-commit，所以要自定義pre-applypatch請修改pre-commit
       ![image](https://github.com/leoa12412a/Make-youself-Git-work-flow/blob/master/pre-applypatch.PNG)</br></br>
       
   8.  fsmonitor-watchman
   
   9.  pre-push
   
       pre-push會在已經更新remote資料但並未傳送資料時被調用，你可以在開始推送前利用此腳本來檢查你的提交
   
   遠端伺服器hooks
   
   遠端的hook除了要刪除副檔名(.sample)外，還需要給予可執行權限 chmod +x hooks  (hooks資料夾)
   
   1.  pre-receive
       
       如果有人向server推送提交(push)就會觸發腳本，可利用此腳本對於使用者提交至中央儲存庫(server)的資料進行規範以及審查
       EX:
       可以使用權限來限制使用者的提交權限
       可以防範一些複雜的合併
       要求提交必須依照某種模式或格式(提交必須簽名...)
       防止一些有關資訊安全或是敏感的字詞添加到server
       如果一次多個引用(References 或 refs)提交的話，會拒絕提交    
       
   2.  update
       
       update 會在 pre-receive 後被調用，實際的用法跟前者也雷同，一樣是在更新server前被調用，不同的是它可以分別被每個推送上來的
       引用(refs)調用，比如說:使用者推送到嘗試推送到4個分支，updata會被執行四次，如果其中一個被拒絕只會是該項目被拒絕，不影響其他的推送
            
   3.  post-update
   
       post-update會在成功被push上去以後才被調用，適合用來發送修改訊息，比起在post-commit發送在post-update發送會更好。
       
       ![image](https://github.com/leoa12412a/Make-youself-Git-work-flow/blob/master/3hook.PNG)</br></br>
       
