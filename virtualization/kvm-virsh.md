# KVM - Virsh

## 1. KVM là gì ? 
- KVM (kernel-base Virtual Machine) là một phần mềm ảo hóa cho các hệ điều hành chạy nhân Linux, dành cho CPU hỗ trợ công nghệ ảo hóa Intel-VT/ AMD-V. KVM biến máy host/server trở thành một hypervisor.

## 2. Install KVM Ubuntu 20.04
- Kiểm tra tính năng VT ( Virtualization Technology ) đã được enable chưa, nếu chưa thì có thể enable trong BIOS : 
```
tule@tule:~$ lscpu | grep Virtualization 
Virtualization:                  VT-x
```
- Kiểm tra Hardware có hỗ trợ ảo hóa không, nếu lệnh sau trả về = 0 thì không hỗ trợ : 
```
tule@tule:~$ egrep -c "svm|vmx" /proc/cpuinfo
8
```

- Cài đặt các gói sau : 
```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
```

- Enable dịch vụ 
```
systemctl enable --now libvirtd
```


## 3. Virsh command : 

## 3.1. Khái niệm : 
- **Libvirt** là một bộ các phần mềm mà cung cấp các cách thuận tiện để quản lý máy ảo và các chức năng của ảo hóa. Những phần mềm này bao gồm một thư viện API daemon (libvirtd) và các gói tiện tích giao diện dòng lệnh (virsh).
- Mục đích chính của Libvirt là cung cấp một cách duy nhất để quản lý ảo hóa từ các nhà cung cấp và các loại hypervisor khác nhau. Ví dụ, dòng lệnh virsh list có thể được sử dụng để liệt kê ra các máy ảo đang tồn tại cho một số hypervisor được hỗ trợ (KVM, Xen, Vmware ESX, … ). Không cần thiết phải sử dụng một tool xác định cho từng hypervisor.

## 3.2. Create vm from image : 
- Tạo VM mới : 
```
virt-install \
--virt-type=kvm \
--os-type=Linux \
--name centos7 \
--ram 1024 \
--vcpus=2 \
--os-variant=centos7.0 \
--hvm \
--extra-args "console=ttyS0" \
--location=/var/lib/libvirt/boot/CentOS-7-x86_64-Minimal-2009.iso  \
--network=bridge=virbr0,model=virtio \
--disk path=/var/lib/libvirt/images/centos7.qcow2,size=10,bus=virtio,format=qcow2
```
- Trong đó, các option có ý nghĩa :
	- `--virt-type` : loại ảo hóa 
	- `--os-type` : loại OS
	- `--name` : tên VM
	- `--ram` : ram 
	- `--vcpus` : lượng vcpus cấp cho VM
	- `--os-variant` : chọn biến thế của os, để lấy giá trị osinfo-query os
	- `--hvm` : sử dụng full virtualization
	- `--extra-args:` set accept console từ cli
	- `--location:` Chọn nơi chứa image cài đặt (iso, qcow2,...)
	- `--network:` setup network 
	- `--disk path:` hard disk cho vm

