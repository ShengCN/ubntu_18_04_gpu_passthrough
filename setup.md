# GPU Passthrough Ubuntu 18.4

## System Scripts

### Grub
Modify **/etc/default/grub** 

``` Bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash intel_iommu=on vfio-pci.ids=10de:1e87,10de:10f8,10de:1ad8,10de:1ad9"
```
Then, update grub:  ``update-grub``

### Modprob
**/etc/modprob.d/vfio.conf**
``` Bash
options vfio-pci ids=10de:1e87,10de:10f8,10de:1ad8,10de:1ad9 
softdep nvidia pre: vfio-pci 
softdep nouveau pre: vfio-pci
```

``` Bash
options vfio-pci ids=10de:17c2,10de:0fb0
softdep nvidia pre: vfio-pci 
softdep nouveau pre: vfio-pci
```

Then, update models:  ``update-initramfs -u``

### Modules
**/etc/modules**
``` Bash
vfio vfio_iommu_type1 vfio_pci ids=10de:1e87,10de:10f8,10de:1ad8,10de:1ad9 
```

``` Bash
vfio vfio_iommu_type1 vfio_pci ids=10de:17c2,10de:0fb0
```

**/etc/initramfs-tools/modules**
``` Bash
vfio vfio_iommu_type1 vfio_pci ids=10de:1e87,10de:10f8,10de:1ad8,10de:1ad9 
```

``` Bash
vfio vfio_iommu_type1 vfio_pci ids=10de:17c2,10de:0fb0
```

## DEBUG CMDs
**DMAR**: 
```Bash
dmesg | grep -E "DMAR|IOMMU" 
```

**vifo**:
```Bash
dmesg | grep -i vfio
```
**show PCI binding points**
```Bash
lspci -nn | grep -i nvidia 
```

After rebooting, **show driver in use**: 
```Bash
lspci -nnv
``` 

## Installing required packages

``` Bash
sudo apt install libvirt-daemon-system libvirt-clients qemu-kvm qemu-utils virt-manager ovmf
```

## virtio
https://docs.fedoraproject.org/en-US/quick-docs/creating-windows-virtual-machines-using-virtio-drivers/#virtio-win-direct-downloads 


## NVIDIA

```xml
...
<features>
	<hyperv>
		...
		<vendor_id state='on' value='whatever'/>
		...
	</hyperv>
	...
	<kvm>
	    <hidden state='on'/>
	</kvm>
</features>
...

```