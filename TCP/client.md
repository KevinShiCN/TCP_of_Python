```bash
import socket
import struct
import json

phone = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
phone.connect(('phone.connect(('server.natappfree.cc', 33516))', 33516))

while True:
    cmd = input('>>>')
    if not cmd:continue
    phone.send(cmd.encode('utf-8'))
    # 接收固定报头
    header_size = struct.unpack('i', phone.recv(4))[0]
    # 解析报头长度
    header_bytes = phone.recv(header_size)
    header_dict = json.loads(header_bytes.decode('utf-8'))
    total_size = header_dict['total_size']
    recv_size = 0
    res = b''
    while recv_size < total_size:
        recv_data = phone.recv(1024)
        res += recv_data
        recv_size += len(recv_data)
    print(res.decode('gbk'))
phone.close()
```

