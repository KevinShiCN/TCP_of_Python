```bash
#!/usr/bin/python
import socket
import subprocess
import struct
import json

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
phone.bind(('192.168.154.60', 8080))
phone.listen(5)

while True:
    conn, addr = phone.accept()
    print(addr)
    while True:
        try:
            cmd = conn.recv(1024)
            ret = subprocess.Popen(cmd.decode('utf-8'), shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
            correct_msg = ret.stdout.read()
            error_msg = ret.stderr.read()
            total_siza = len(correct_msg) + len(error_msg)
            header_dict = {
                'md5': 'fdsaf2143254f',
                'file name': 'f1.txt',
                'total_size': total_siza
            }
            header_dict_json = json.dumps(header_dict)
            bytes_headers = header_dict_json.encode('utf-8')
            header_size = len(bytes_headers)
            header = struct.pack('i', header_size)
            conn.send(header)
            conn.send(bytes_headers)
            conn.send(correct_msg)
            conn.send(error_msg)
        except Exception as e:
            print(e)
            break

conn.close()
phone.close()
```

