---
title: "python-graphviz: A New Way of Looking At The Data of Things"
date: 2014-07-13
tags:
  - python
---
At my new job (and some of my personal projects) I found myself looking at a ton of text. Upon reviewing all this data, which could be anything from json, csv or just plain text, my eyes tend to bleed. It can quite quickly become confusion as to which data I’ve looked at or how that data relates to everything else.

<!--more-->

With all of that being said, it was time to find a new way of looking at things.  This is where python-graphviz has come in handy.  The first thing I needed to do (after spending some time googling and then finally learning about graphviz anyway) was to find a way to normalize my data.  

In an attempt to keep things nice and simple we are just going to look at some basic data in a csv. 

**The CSV Data**

```
FC_EVENT,jar_cache234234343.tmp,WindowsXP
FC_EVENT,jar_cache543454345.tmp, Windows7
FC_EVENT,~spawn098343434.tmp,WindowsXP
FC_EVENT,~spawn093565831.tmp,Windows7
FD_EVENT,fred.txt,WindowsXP
FD_EVENT,wilma.txt,Windows7
FD_EVENT,wilma.txt,WindowsXP
FD_EVENT,betty.txt,WindowsXP
FD_EVENT,betty.txt,Windows7
FD_EVENT,betty.txt,WindowsXP
```

**The Python Code**

```
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import os
import hashlib
import argparse
from graphviz import Digraph

def setup():
    parser = argparse.ArgumentParser()
    parser.add_argument("-f", "--file", action="store", dest="file", required=True, help="File to create dot file from")
    global args
    args = parser.parse_args()
    
def main():
    setup()
    fp = open('%s' % args.file, 'r')
    fp_lines = fp.readlines()
    fp.close()
    
    normalized_data = []
    
    for line in fp_lines:
        if not line.strip().startswith("#") and line.strip() != "":
            line_data = line.strip().split(",")
            tmp_data = {
                "action": line_data[0],
                "file": line_data[1],
                "os": line_data[2],
            }
            normalized_data.append(tmp_data)
                
    dot = Digraph(comment='No Comments Here')
    dot.node_attr['shape'] = 'box'
    dot.graph_attr['ranksep'] = '1.5'
    dot.graph_attr['nodesep'] = '0.8'
    dot.graph_attr['splines'] = "ortho"
    n = 0
    lookup = {}
    existing_edges = []
                
    if normalized_data:
        for root in sorted(normalized_data):
            for itm in root.items():
                if not (itm[1] in lookup):
                    lookup[itm[1]] = n
                    dot.node('n%d' % n, '%s' % itm[1])
                    n += 1

            t_hash = hashlib.md5()
            t_hash.update("%s%s" % (root["action"], root["file"]))
            if not t_hash.hexdigest() in existing_edges:
                    existing_edges.append(t_hash.hexdigest())
                    dot.edge('n%s' % lookup[root["action"]], 'n%s' % lookup[root["file"]])
                    
            t_hash = hashlib.md5()
            t_hash.update("%s%s" % (root["file"], root["os"]))
            if not t_hash.hexdigest() in existing_edges:
                    existing_edges.append(t_hash.hexdigest())
                    dot.edge('n%s' % lookup[root["file"]], 'n%s' % lookup[root["os"]])
                    
            fp = open('%s.dot' % os.path.basename(args.file), 'w')
            fp.write(dot.source)
            fp.close()
            
if __name__ == "__main__":
    main()
```

**The Pretty Results**

{{< figure src="/2014-07-13-a-new-way-of-looking-at-the-data-of-things/results.png" title="" >}}<br>
 
**The Results**

The following results can be saved as a .dot file and then opened up into graphviz directy in order to see the pretty chart we just made.

```
// No Comments Here
digraph {
	graph [nodesep=0.8 ranksep=1.5 splines=ortho]
	node [shape=box]
		n0 [label=FC_EVENT]
		n1 [label=WindowsXP]
		n2 [label="jar_cache234234343.tmp"]
			n0 -> n2
			n2 -> n1
		n3 [label=" Windows7"]
		n4 [label="jar_cache543454345.tmp"]
			n0 -> n4
			n4 -> n3
		n5 [label=Windows7]
		n6 [label="~spawn093565831.tmp"]
			n0 -> n6
			n6 -> n5
		n7 [label="~spawn098343434.tmp"]
			n0 -> n7
			n7 -> n1
		n8 [label=FD_EVENT]
		n9 [label="betty.txt"]
			n8 -> n9
			n9 -> n5
			n9 -> n1
		n10 [label="fred.txt"]
			n8 -> n10
			n10 -> n1
		n11 [label="wilma.txt"]
			n8 -> n11
			n11 -> n5
			n11 -> n1
}
```

**Summary**

This is a VERY simple example of what you can do with python-graphviz.  I would also like to mention that I used hashlib to help make sure multiple nodes of the same thing did not get created which wasn't really needed in this example, but may prove useful later on with larger datasets.        

