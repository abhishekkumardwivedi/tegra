# tegra

## Boot flow
```
[0000.125] [L4T TegraBoot] (version 00.00.2018.01-l4t-80a468da)
[0000.131] Processing in cold boot mode Bootloader 2
..
[0000.269] Read GPT from (4:0)
..
[0000.327] Using GPT Primary to query partitions
[0000.332] Loading Tboot-CPU binary
..
[0000.352] Bootloader load address is 0xa0000000, entry address is 0xa0000258
[0000.359] Bootloader downloaded successfully.
[0000.363] Downloaded Tboot-CPU binary to 0xa0000258
..
[0000.386] Read GPT from (4:0)
..
[0000.398] Loading NvTbootBootloaderDTB
..
[0000.520] Loading NvTbootKernelDTB
..
[0000.641] Loading cboot binary
..
[0000.712] Bootloader load address is 0x92c00000, entry address is 0x92c00258
..
[0000.768] BoardId: 3448
..
[0000.782] Read GPT from (4:0)
[0000.790] Using GPT Primary to query partitions
[0000.818] Verifying SC7EntryFw in OdmNonSecureSBK mode
..
[0000.879] SC7EntryFw header found loaded at 0xff700000
..
[0001.073] Bpmp FW successfully loaded
..
[0001.433] Bootloader DTB loaded at 0x83000000
..
[0001.495] Welcome to L4T Cboot
[0001.498] 
[0001.499] Cboot Version: 00.00.2018.01-t210-4686ce50
..
[0001.777] board ID = D78, board SKU = 3
[0001.780] Skipping Z3!
..
[0007.241] kfs_getparname: name = LNX
[0007.244] Loading kernel from LNX
[0007.343] load kernel from storage
```
