# Android twrp device tree for Lenovo TB-7305X (mt8765_P)


Works after unlocking bootloader and flashing recovery by fastboot.

```
fastboot flash recovery recovery.img
```

You can use the recovery name instead of <recovery.img>


## Notes
> This device does not a vbmeta partition so you may have to either replace hexcode of string "verify" in boot.img to 0s or flash the disable_dm_verity_force_decrypt.zip afterwards to disable avb

> The kernel does not allow booting into some GSIs (tested, didnt go past boot animation -> lineage-os A14 to A9, arrowos A11, caos-A11, /e/-a10) or the 
