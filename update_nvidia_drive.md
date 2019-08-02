Update Nvidia Dirver on several machines
========================================

1. Gen a list of machines:

```user@domain
user@domain
user@domain
.
.
.
user@domain
user@domain
```

2. [Optional download the driver]

3. Check if the driver is installed

```bash
LIST_FILE=gpu_all.lst
for m in $(<$LIST_FILE); do
    sshpass -p "password" ssh -o StrictHostKeyChecking=no $m "hostname;nvidia-smi";
done
```

4. Install the driver on the missing ones

```bash
for m in $(<$LIST_FILE); do
    sshpass -p "password" ssh -o StrictHostKeyChecking=no $m \
    "hostname;
    if ( lsmod | grep -q nvidia ) then 
        echo "Driver OK";
    else
        sudo nvidia-uninstall; sudo service lightdm stop; sudo <DIRVER> -s" &
    fi
done
```
