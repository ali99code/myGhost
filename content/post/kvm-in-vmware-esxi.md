+++
title = "KVM in VMware Esxi"
date = 2016-11-27T14:31:00Z
author = "Alibaba"
categories = ["KVM","VMware"]
tags = ["KVM","VMware"]
description = ""
slug = "kvm-in-vmware-esxi"
draft = false

+++

# KVM install
KVM 全名為 **Kernel-Based Virtual Machine**,是一個用於Linux核心中的虛擬化基礎設施。
本單元主要演示安裝KVM過程,主要利用VMware Esxi 平台建立Netsed hypervisors環境,即是 在 Esxi 平台建立一個 虛擬機 ,在此虛擬機安裝KVM ,再由kvm 建立一台虛擬機。

## 環境需求
- 實體機的 CPU 需支持 Intel (**Intel Virtualization Technology, Intel VT-x**); AMD (**AMD Secure Virtual Machine, AMD**)
-  VMware ESXi 5 或 6 
-  Ubuntu 16.04 Desktop LTS ,[下載點](https://www.ubuntu-tw.org/modules/tinyd0/"target="_blank)

## 步驟
1. 在esxi 平台上建立虛擬機，主要關鍵,勾選硬體虛擬化項目**向Guest OS 公開硬體協助虛擬化**,此操作可以在 **ESXi Web Client or ESXi Host Client**下完成。
![如圖示說明](/content/images/2016/11/GuestoS1.GIF)
2.網路部分,需要將VSS Security Policies的**Promiscuous mode** 設為允許。此功能相當於HUB,將port 收到的封包,會廣播向其它的Port傳送
![Promiscuous mode](/content/images/2016/11/VM_Network_PortGroup_Security.GIF)

3.之後,虛擬機安裝OS ,Ubuntu 16.04 Desktop LTS
4.之後,安裝完OS。先確定以下項目,每項都必須確認無誤後,才能做下一步
* VM是否支援CPU虛擬化
```
grep -E -i 'vmx|svm' /etc/cpuinfo
```
* 確認kvm是否安奘
```
apt-get install cpu-checker
kvm-ok
```
* 確認是否有載入KVM 相關模組
```
lsmod|grep kvm
```
5. 安裝 libvirt,及看使用者(這邊以 andy)是否在指定群組
```
apt-get install qemu-kvm libvirt-bin
grep andy /etc/group
```
```
adm:x:4:syslog,andy
cdrom:x:24:andy
sudo:x:27:andy
dip:x:30:andy
plugdev:x:46:andy
lpadmin:x:113:andy
andy:x:1000:
sambashare:x:128:andy
libvirtd:x:130:andy
```
6. 安裝 virt-manager , bridge-utils 
```
apt-get install virt-manager bridge-utils
```
7. 開始執行virt-manager 建立虛擬機,會跳出以下圖示
```
$ sudo virt-manager
```
![virt-manager](/content/images/2016/11/kvm-virt-manager.GIF)
## 參考資料
- [Nested ESXi 6 in a Lab](http://www.vladan.fr/nested-esxi-6-in-a-lab/"target="_blank)
- [Fedora 17 KVM於ESXi 5.1參數設定與安裝VMTools](http://www.ntueees.tp.edu.tw/wordpress/?p=2981"target="_blank)
