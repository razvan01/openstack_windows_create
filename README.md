# openstack_windows_create
# How I've created a windows VM into OpenStack without kvm, virtualbox and others?

How to do that?
Easy and dirty way :)

# Pre-requirements are:
- VirtIO iso already imported in OpenStack
- Windows iso already imported in OpenStack

# Steps:
1) Import Windows iso to OpenStack: *glance image-create --name "Windows 7 SP1 x64" --visibility=public --container-format bare --disk-format=iso --file win7.iso*
2) Import virtio iso to OpenStack: *glance image-create --name "VirtIO" --visibility=public --container-format bare --disk-format=iso --file virtio-win.iso*
3) Create a volume from virtio iso: *openstack volume create --image virtio_iso_id --size 1 --availability-zone nova VirtIO*
4) Boot Windows instance from Windows iso with a blank wolume and VirtIO volume attached: *nova boot --image windows_iso_id --block-device source=blank,dest=volume,size=20,shutdown=preserve --block-device id=virtio_volume_id,source=volume,dest=volume,size=5,type=cdrom --nic net-id=network_id  --flavor flavor_id  Win7x64*
5) Go to Openstack dashboard and open machine console and start install process. When install ask for hdd nothing will appear but is ok, we are smart and will trick it thank to virtio volume already attached :D now we will "hit" the "Load driver" button and there maybe will show missing drivers or not. in my case Windows hate me to much and nothing show in missing pannel but that's ok for now. "Shot" the browse button and install needed drivers (i've installed all because i had a lot of free time). After installing drivers there will appear your hdd and install process can continue like is on a phisical machine
6) Latest step if you want is to install cloud-init but i will skip this because i don't care about :D

# VirtIO download url:
- Stable version: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso
- Latest version: https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win.iso
