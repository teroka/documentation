---
layout: default
title: peers
categories: [Reference, Functions, peers]
published: true
alias: reference-functions-peers.html
tags: [reference, communication functions, functions, peers]
---

**Prototype:** `peers(filname, regex, groupsize)`

**Return type:** `slist`

**Description:** Returns a list of peers from the partition to which
the current host belongs, excluding the current host.

This function returns a list of hostnames that may be considered peers
of the current host. Peers are defined according to a list of hosts,
provided in `filename`. This file should contain a list (one per line), 
possible with comments matching `regex`, of fully qualified host names. 
CFEngine breaks up this list into non-overlapping groups of up to `groupsize`, 
each of which has a leader that is the first host in the group.

The current host should belong to this file if it is expected to interact with 
the others. The function returns nothing if the current host does not belong 
to the list.


**Arguments**:

* `filename` : File name of host list, in the range `"?(/.*)`

A path to a file containing the list of hosts.

* `regex` : Comment regex pattern, in the range `.*`

A pattern that matches a legal comment in the file. The regex is [unanchored][unanchored], 
meaning it may match a partial line. Comments are stripped as the file is 
read.

* `arg3` : Peer group size, in the range `0,99999999999`   

A number between 2 and 64 which represents the number of peers in a 
peer-group. An arbitrary limit of 64 is set on groups to avoid nonsensical 
promises.

**Example:**

Example file:

```cf3
     one
     two
     three # this is a comment
     four
     five
     six
     seven
     eight
     nine
     ten
     eleven
     twelve
     etc
```

```cf3
bundle agent peers
{
vars:

  "mygroup" slist => peers("/tmp/hostlist","#.*",4);

  "myleader" string => peerleader("/tmp/hostlist","#.*",4);

  "all_leaders" slist => peerleaders("/tmp/hostlist","#.*",4);

reports:

   "mypeer $(mygroup)";
   "myleader $(myleader)";
   "another leader $(all_leaders)";

}
```
