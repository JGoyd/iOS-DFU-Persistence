# Apple Official Documentation vs Observed DFU Behavior

## Apple's Claims 

**1. Apple Support: Restore iPhone**
> "A factory restore **erases the information and settings** on your iPhone...."  
> — [Apple Support: Erase iPhone](https://support.apple.com/en-us/HT201252)

**2. Apple Support: Recovery Mode (DFU equivalent)**
> "When you see the option to Update or Restore, choose **Restore**. Restore reinstalls iOS and **erases all of your data.**"  
> — [Apple Support: If you can't update or restore](https://support.apple.com/en-us/118106)

**3. Apple Training (Explicit)**
> "You can **restore a device** to **erase all data and settings** and return it to factory settings."  
> — [Apple Training: Recovering, Reviving, Restoring](https://it-training.apple.com/support/tutorials/course/sup150/)

## Observed Reality

**1. disk1s8 survives with "protect" flag**  
`mount.txt: apfs...journaled,noatime,**protect**` [EVIDENCE/mount.txt]

**2. 11:45 pre-DFU logs survive restore**  
`mobileactivationdlog.1: "Fri Jan 9 11:45:41 2026"` [EVIDENCE/mobileactivationdlog.1]

**3. TCC.db privacy grants preserved**  
`Health/Photos auth_value=2 (ALLOWED) 14:47:24` [EVIDENCE/TCC.db]

**4. Background processes span DFU boundary**  
`fitnessd/siriactionsd/linkd → 14:46-14:48:25` [EVIDENCE/.bgsql]

**5. NAND controllers active during protection**  
`AppleANS3CGv2Controller(x2) 14:48:24` [EVIDENCE/stacks-2026-01-09-144824.ips]

**Apple Claims "erases all data/settings" → Reality: disk1s8 user volume preserved via hardware protection**
