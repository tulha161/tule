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
--cdrom=/home/tule/Downloads/ISO/CentOS-7-x86_64-Minimal-2009.iso  \
--network=bridge=virbr0,model=virtio \
--disk path=/var/lib/libvirt/images/centos7.qcow2,size=10
```
- Trong đó, các option có ý nghĩa :
	- `--virt-type` : loại ảo hóa 
	- `--os-type` : loại OS
	- `--name` : tên VM
	- `--ram` : ram 
	- `--vcpus` : lượng vcpus cấp cho VM
	- `--os-variant` : chọn biến thế của os, để lấy giá trị osinfo-query os
	- `--cdrom:` Chọn nơi chứa image cài đặt (iso, qcow2,...)
	- `--network:` setup network 
	- `--disk path:` : cấu hình hard disk cho vm, size = 10GB.

## 3.3. Một số thao tác với Virsh command :
- `virsh list ` : list all vm đang bật
- `virsh list --all ` : list all vm in node
- `virsh start [name] ` : khởi động vm [name]
- `virsh autostart [name] ` : khởi động vm chỉ định cùng hệ thống
- `virsh dominfo [name] ` : check info vm 
- `virsh suspend/resume [name] ` : pause/resume vm 
- `virsh destroy [name]` : force shutdown vm
- `virsh undefine [name] ` : xóa vm 
- `virsh edit [name]` : edit vm 
- `virsh dumpxml [name] > name.xml` : dump file xml của vm
- `virsh define name.xml ` : tạo vm từ file xml


## 3.4. CPU pinning :
- Liên kết VCPU của VM với CPU vật lý. Tận dụng hiệu năng của CPU vật lý tốt hơn cho CPU ảo của VM.
- Show vcpupin : 
```
virsh # vcpupin centos7 
 VCPU   CPU Affinity
----------------------
 0      0-3
 1      0-3
```
- Thực hiện pinning CPU ( cú pháp sau pinning Vcpu ID 0 -> Physical CPU ID 0, Vcpu ID 1 -> Physical CPU ID 1 ) 
```
virsh # vcpupin centos7 0 0
virsh # vcpupin centos7 1 1
```
- Kiểm tra lại : 
```
virsh # vcpupin centos7 
 VCPU   CPU Affinity
----------------------
 0      0
 1      1
```

## 3.5. limit CPU 
- listing info : 
```
virsh # vcpucount centos7 
maximum      config         2
maximum      live           2
current      config         2
current      live           2
```
- Trong đó : 
 	- maximum config : chỉ định số Vcpus tối đa có thể được cấp cho vm sau lần restart tiếp theo
 	- maximum live : chỉ định số Vpcpus tối đa có thể được cấp cho vm đang chạy/pause
 	- current config : chỉ định số Vcpus được cấp phát cho vm sau lần restart tiếp theo.
 	- current live : chỉ định số Vcpus được cấp phát cho vm đang chạy/pause.
- Đối với config live, cpu cần hỗ trợ cơ chế *hotplug* thì mới config được. 
- Cú pháp limit cpu : 
```
virsh setvcpus <VM-name> <max-number-of-CPUs> [option]
```
- thêm các arg tương ứng với các trường hợp muốn thay đổi config : 
	- `--config` : thay đổi current config
	- `--config --maximum`: thay đổi current và maximum
	- `--live` : thay đổi live

## 3.6. limit bandwidth : 
- Để limit bw, ta thêm phần config bandwidth vào interface trong file cấu hình xml của VM, cụ thể như sau : 
	- vào thay đổi cấu hình : ` virsh edit centos7 `
	- tìm phần cấu hình `interfaces` thay đổi như sau : 
```
<interface type='bridge'>
      <mac address='52:54:00:cd:1f:44'/>
      <source bridge='virbr0'/>
      <bandwidth>
        <inbound average='1000' peak='1000' burst='1000'/>
        <outbound average='1000' peak='1000' burst='1000'/>
      </bandwidth>
      <model type='virtio'/>
      <address type='pci' domain='0x0000' bus='0x01' slot='0x00' function='0x0'/>
    </interface>
```





