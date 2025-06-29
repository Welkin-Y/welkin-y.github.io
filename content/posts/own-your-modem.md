---
title: "Own Your Modem"
date: 2025-06-29T18:24:39+08:00
draft: false
---

Device: F7610M
Production date: 202306

1. Record necessary Info && Reset device

- LOID

2. Enable telnet and receive config from ISP

- [zteOnu](https://github.com/Septrum101/zteOnu)

  ```bash
  ./zteOnu --pass "nE%jA@5b" -u "factorymode" --port 80 
  ```

3. Change admin username/password [refer](https://www.right.com.cn/forum/thread-8393194-1-1.html)

 ```bash
sendcmd 1 DB set DevAuthInfo 0 User <username>
sendcmd 1 DB set DevAuthInfo 0 Pass <password>
sendcmd 1 DB save
```

4. Disable tr069 etc. [refer](https://www.right.com.cn/forum/thread-8393194-1-1.html)

 ```bash
sendcmd 1 DB set MgtServer 0 Tr069Enable 0
sendcmd 1 DB set MgtServer 0 PeriodicInformEnable 0
sendcmd 1 DB save
```
