---
layout: post
title: Installing void on a VM using libvirtd/virt-manager
categories: [sysadmin, virtd, void]
---


Void vm image requires uefi boot.

{% highlight bash %}
virt-install -n void \
    --description "Void Linux" \
    --boot uefi \
    --osinfo voidlinux \
    --cdrom=void-live-x86_64-20210930.iso \
    --ram 2048 \
    --vcpus 2 \
    --disk size=20 \
    --graphics vnc,listen=0.0.0.0 \
    --network bridge:virbr0
{% endhighlight %}

To start, stop or force-stop the vm,

{% highlight bash %}
virsh start void
virsh shutdown void
virsh destroy --domain void
{% endhighlight %}

To delete the vm,

{% highlight bash %}
virsh undefine --domain void --remove-all-storage
{% endhighlight %}

Then install as normal.
