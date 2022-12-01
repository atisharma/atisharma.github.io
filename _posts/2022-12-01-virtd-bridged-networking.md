---
layout: post
title: Bridged networking with virtd and nixos
categories: [sysadmin, virtd, nix]
---

## Bridged networking in virtd

This gives each vm its own IP without involving a separate subnet / IP range.

### nix config

{% highlight nix %}
networking.bridges.bridge0.interfaces = [ "bond0" ];
networking.interfaces.bridge0 = {
    useDHCP = false;
    ipv4.addresses = [ {
        "address" = "192.168.1.116";
        "prefixLength" = 24;
    }];
};
{% endhighlight %}

### virsh network

Create the following file, `bridged.xml`.
{% highlight xml %}
<network>
  <name>bridged</name>
  <uuid>00000000-0000-0000-0000-000000000000</uuid>
  <forward mode='bridge'/>
  <bridge name='bridge0'/>
</network>
{% endhighlight %}

Then define the network and connect it to the system

{% highlight bash %}
virsh --connect qemu:///system net-define bridged.xml
virsh net-autostart bridged
{% endhighlight %}

### References
[NixOS: Headless Home Assistant VM](https://myme.no/posts/2021-11-25-nixos-home-assistant.html)

