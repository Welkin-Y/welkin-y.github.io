---
title: "Rename Linux Username"
date: 2023-06-07T22:23:00-04:00
draft: true
tags: ["linux"]
---

# Change Linux username

非 root 用户，所以会用到 `sudo`, 使用 distribution 为 Ubuntu22.04-amd64

1. **log in to a user other than target user**

2. **Change home directory**:
   
    `sudo mv /home/target_old_username /home/target_new_username`

3. **Change `/etc/passwd`**: 
   
    `sudo vim /etc/passwd`

4. **Change `/etc/shadow`**: 
    
    `sudo vim /etc/shadow`

5. **Change `/etc/group`**: 
   
    `sudo vim /etc/group`

附上 chatGPT 的相关回答

`/etc/passwd`: This file contains information about all the system users. You need to hcange the username in the first field and the home directory in the sixth field.

`/etc/shadow`: This file contains password information about all the system users. You need to change the username in the first field and the home directory in the first field.

`/etc/group`: This file contains information about all the system groups. You need to change the old username to the new one in all the lines where it appears.

#### [_A tutorial for vim here._](https://github.com/iggredible/Learn-Vim)


