# SMTP 命令

1.  HELO

    > 启动识别发件人服务器的对话，通常后跟域名
2.  EHLO

    > 启动会话的替代命令，表明服务器正在使用扩展 SMTP 协议
3.  MAIL FROM

    > 使用此 SMTP 命令开始操作，发件人在 “From”字段说明源电子邮件地址，并开始电子邮件传输
4.  RCPT TO

    > 标识电子邮件的收件人；如果有多个，命令只是逐地址重复
5.  SIZE

    > 此 SMTP 命令通知远程服务器有关附加电子邮件的估计大小，还可以用户报告服务器接受的消息的最大大小
6.  DATA

    > 使用 DATA 命令传输电子邮件内容，通常后面跟着服务器给出的 354 回复代码，允许开始实际传输
7.  VRFY

    > 要求服务器验证特定电子邮件地址或用户名是否存在
8.  TURN

    > 用于反转客户端和服务器之间的角色，而不需要允许新的连接
9.  AUTH

    > 使用 AUTH 命令，客户端向服务器验证自己，并提供用户名和密码，这是保证安全传输的另一层安全性
10. RSET

    > 通知服务器正在运行的电子邮件传输将被终止，但是 SMTP 会话不会关闭
11. EXPN

    > 要求确认邮件列表的标识
12. QUIT

    > 终止 SMTP 会话

参考文章 --> [链接](https://serversmtp.com/smtp-commands/)
