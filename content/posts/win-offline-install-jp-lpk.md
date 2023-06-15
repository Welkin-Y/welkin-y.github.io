---
title: "Manually Install Microsoft IME Japanese"
date: 2023-06-15T02:09:10-04:00
draft: false
---
# Problem description
Cannot install Japanese language package nor Microsoft IME on working computer.
#### Error messages:
> Japanese IME is not ready yet.

> Sorry. we're having trouble getting this feature installed. You can try again later.  
Error code: 0x800F0954.

In a word, the working computer blocks communication with servers that installs language packages. For more detailed reason behind, one can refer to this [blog](https://www.wroot.It/wp/technology/fixing-japanese-and-chinese-ime-problem-en). 

# Requirements:
1. **Admin Rights** (**IMPORTANT!** Without which you can CLOSE this page).
2. Microsoft Windows Client Language Pack for Japanese in a .cab file.
3. Microsoft Features On Demand (FOD) for Japanese language features in some .cab files.
   If one only need basic input of Japanese, a LanguageFeatures-Basic .cab file will be enough.

# My enviroment & process
Env: Windows10 22H2 x64  
1. Request Admin rights
2. Download required .cab files
   - Microsoft-Windows-Client-Language-Pack_x64_ja-jp.cab   
   You can find and download it [HERE](https://ia800807.us.archive.org/view_archive.php?archive=/13/items/mu_windows_10_language_pack_version_1703_update_march_2017_x86_x64_dvd_10204769/mu_windows_10_language_pack_version_1703_update_march_2017_x86_x64_dvd_10204769.iso)
   - Microsoft-Windows-LanguageFeatures-Basic-ja-jp-Package~31bf3856ad364e35~adm64~~.cab  
   You can download and extract it from [HERE](https://software-download.microsoft.com/download/pr/19041.1.191206-1406.vb_release_amd64fre_FOD-PACKAGES_OEM_PT1_amd64fre_MULTI.iso)
3. Press Win+R, type ```lpksetup```, install Microsoft-Windows-Client-Language-Pack_x64_ja-jp.cab.
4. Run Windows PowerShell as administrator, type:  
   ```Add-WindowsPackage -Online -PackagePath <path of Microsoft-Windows-LanguageFeatures-Basic-ja-jp-Package~31bf3856ad364e35~adm64~~.cab>```
   




