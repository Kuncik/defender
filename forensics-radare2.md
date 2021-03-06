# radare2

## bootstrap radare
```
# radare2 ./tsm-b1
[0x08048110]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze len bytes of instructions for references (aar)
[x] Analyze function calls (aac)
[ ] [*] Use -AA or aaaa to perform additional experimental analysis.
[x] Constructing a function name for fcn.* and sym.func.* functions (aan))
```

## print entrypoint
```
[0x08048110]> ie
[Entrypoints] ie
vaddr=0x08048110 paddr=0x00000110 baddr=0x08048000 laddr=0x00000000 haddr=0x00000018 type=program
```

## search string
```
[0x08048110]> / honeypot\x00
Searching 9 bytes from 0x08048000 to 0x0a86bf94: 68 6f 6e 65 79 70 6f 74 00
Searching 9 bytes in [0x8048000-0xa86bf94]
hits: 1
0x081c5167 hit0_0 .cpuinfofree -mhoneypot\\u00007880 7690 189 0 .
```

## search reference
```
[0x08048110]> /c 0x081c5167
[0x08048110]> /c 0x81c5167
0x0804ca67   # 10: mov dword [obj.ssh_type], 0x81c5167
0x0804ca90   # 10: mov dword [obj.ssh_type], 0x81c5167
```


## analyze function list

```
[0x08048110]> afl | grep sym.is_
0x0804829a   12 139          sym.is_duplicate
0x0804d648    9 128          sym.is_virtual
0x0804d6c8    8 105          sym.is_honeypot_wget
0x0804d732    4 69           sym.is_honeypot_zfunction
0x0804d778    9 169          sym.is_busybox
0x0804d8ae   21 535          sym.is_honeypot_cpu
```

## seek function and disassemble, write some pseudo-code
```
[0x0804a19a]> s funcname

[0x0804a19a]> seek main
[0x0804a19a]> pdf
[0x0804a19a]> pdc
```

## seek xrefs for function

```
[0x0804cde6]> axt sym.is_honeypot_cpu
null 0x804d6c8 push ebp in sym.is_honeypot_wget
call 0x804ca83 call sym.is_honeypot_cpu in sym.ssh_procesate
```

## visual mode

```
VV .. enter visual mode
ARROWS .. move the graph
;[gX]  .. all round the graph are jump shortcuts
u/U    .. unseek, redo seek (upon making jump)
r      .. refresh the graph, but also somehow toggles function jump shortcuts ;[gX]
p/P    .. rotate graph modes (normal, display offsets, minigraph, summary)
V      .. toggle basicblock / call graphs
c      .. toggle cursor mode; can move whole graph, can move selected node around
```

## references

* https://www.megabeets.net/a-journey-into-radare-2-part-1/
* https://r2wiki.readthedocs.io/en/latest/options/capv/visual-mode/vv-help/
