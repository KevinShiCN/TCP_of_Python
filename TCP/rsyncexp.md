```bash
#!/usr/bin/expect
set mulu [lindex $argv 0]
set timeout 10
spawn rsync -avzr /backup/$mulu root@192.168.154.40::test
expect Password
send "200410\r"
expect eof
```

