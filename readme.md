error: remote origin already exists.
解决:git remote rm origin
如果执行 git remote rm origin 报错的话，我们可以手动修改gitconfig文件的内容

    $ vi .git/config
i 修改
:wq 保存退出

git branch -m master source

Git报错解决：OpenSSL SSL_read: Connection was reset, errno 10054 错误解决
git config --global http.sslVerify "false"


