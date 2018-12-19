Building your own container w/o docker
```sh
cd /btrfs
sudo -s
```
That is an empty dir

# make sure mount points are private
```sh
mount --make-private
# Create dir for img and containers
mkdir -p images containers
# Create alpine image
btrfs subvol create images/alpine
# Get image running
CID=$(docker run -d alpine true)
echo $CID
# Get tarball of container
docker export $CID | tar -C images/alpine -xf-
# Create snapshot of image
btrfs subvol snapshot images/alpine container/tupperware
# Create a file
touch containers/tupperwre/THIS_IS_TUPPERWARE
ls containers/tupperware

chroot contaiers/tupperware sh
# I this apline container
exit

# use NS (All except user)
unshare --mount --uts --ipc --net --pid --fork bash
# Above commands doesn output anthing
hostname tupperware
exec bash
# Now in tupperware container (begining)
ps 
# Could see only procs inside container

# No PID 0, still has system view.
pidof unshare
kill <some proc>
# Can't kill the proc, as its in separate NS, we re in NS other than this PID
# lets mount /proc
mount -t proc none /proc
ps
# Now we only see process inside

cd /btrfs
ps faux
# Now lets remove this /proc
umount /proc/

# Now lets get into FS of our container
cd btrfs/containers/tupperware
# pivot_root
mkdir olderoot
pivot_root . oldroot/
# Above doesn't work, You should be at the top of your FS hierarchy

cd /
# Bindmount Tupperware container dir as mount
mount --bind /btrfs/containers/tupperware/ /btrfs/containers/tupperware
# And then mount that at top of btrfs hierarchy
mount --move /btrfs/containers/tupperware /btrfs/
cd /btrfs
pivot_root . oldroot/
cd /
mount -t proc node /proc
ps

mount
# Still have tons of mounts from system
umount -a
# But it also remove /proc
mount -t proc none /proc
umount /oldroot/
umount -l /oldroot/
mount
# Now just one mount

# Now network
ping 4.2.2.1
#Goto host
sudo -i
# Find PD of container
pidof unshare
6902
CPID=6902
# Peer interfaces
ip link add name h6902 type veth peer name c6902

# back to container
ifconfig
# we have network

# back to host
ip link set c$CPID netns $CPID
# now new interface showed up in container
ifconfig -a

# at Guest
ip link set h$CPID master docker0 up

# in Contaner
# up the network
ip link set lo up
# set name of network as eth0, so it will look like ethernet network to container
ip link set c6902 name eth0 up
# Lets add a IP Address, Random
ip addr add 172.17.42.3/16 dev eth0
# Lets add a Default gateway
ip route add default via 172.17.42.1
# Still cant pong these
ping 4.2.2.1
ping 8.8.8.8
ping 172.17.42.1

# But can ping machine 
ping 172.17.0.1

# Guest System
ip link ls
ip adr ls dev docker0 

# Last Step to enter into container
exec chroot / sh
# Complicated handoff
```
#### Haven't talked about following
===========
- cgroup
- devices
- capabilites
- selinux
- seccomp integration + Accessibility
===========
