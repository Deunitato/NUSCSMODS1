---
layout: post
published: true
title: CS4226 - Project Assignment
subtitle: Project Assignment
---
Github Repository: Link

Locations of files:
- Controller: /pox/misc/controller.py
- topology: main location as long as it is in the same directory as `topology.in`


# Task 1 - Setting up the topology

Setting up the topology using the topology.in file. The only file you need to enter is 


```python
# Add switch
 for x in range(1, int(switch) + 1):
      sconfig = {'dpid': "%016x" % x}
      self.addSwitch('s%d' % x, **sconfig)
 # Add hosts
 for y in range(1, int(host) + 1):
      self.addHost('h%d' % y)

 # Add Links
 for x in range(int(link)):
      info = linksInfo[x].split(',')
      host = info[0]
      switch = info[1]
      bandwidth = int(info[2])
      self.addLink(host, switch, bw=bandwidth)
```

# Task 4 - Firewall

Output:

![CS4226_task4_1.png]({{site.baseurl}}/img/CS4226_task4_1.png)


# Task 5

Printing out all the links setup:
![image_2021-11-07_192105.png]({{site.baseurl}}/img/image_2021-11-07_192105.png)