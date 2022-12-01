---
layout: post
title: Setting up a macvtap bridge to a vm
categories: [sysadmin, virtd]
---

Create the following file, `macvtap.xml`.
{% highlight xml %}
<network>
  <name>macvtap-bridge</name>
  <forward mode="bridge">
    <interface dev="bond0"/>
  </forward>
</network>
{% endhighlight %}

Note the host can't communicate with the guest using macvtap.
The trick is to attach this macvtap-bridge network AFTER the default bridge network.

{% highlight bash %}
virsh net-define /notes/libvirtd/macvtap.xml
virsh net-autostart macvtap-bridge
virsh net-start macvtap-bridge
virsh attach-interface vm-name network macvtap-bridge --current
virsh edit vm-name      # to remove other nw interface
{% endhighlight %}

### References
[scottlowe](https://blog.scottlowe.org/2016/02/09/using-kvm-libvirt-macvtap-interfaces/)
[nodinrogers](https://www.nodinrogers.com/post/2022-01-06-enabling-kvm-host-to-vm-communcation/)
[kernelnewbies](https://virt.kernelnewbies.org/MacVTap)
