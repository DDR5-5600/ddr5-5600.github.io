+++
date = '2025-09-14T07:45:04+08:00'
draft = false
title = '自動化 dpkg-reconfigure 過程的方法'
+++

前陣子寫了個能自動建立 Debian LiveUSB 的 shell script,  
過程中發現鍵盤沒辦法和 apt 互動被卡在選擇界面只能重啟電腦,  
直接用 `DEBIAN_FRONTEND=noninteractive` 把互動全部壓下去,  
script 是順利執行完了，但這時卻發現無法自訂 wireshark-common 的組態。  
  
最後這樣解決:  
```
echo "wireshark-common wireshark-common/install-setuid boolean true" | debconf-set-selections
dpkg-reconfigure wireshark-common
usermod -aG wireshark kali
```
用 `debconf-set-selections` 強制指定 `debconf` 的選擇,  
然後直接跑一遍 `dpkg-reconfigure` 讓它吃進去,  
唯一需要安裝的軟體套件是 `debconf-utils`
