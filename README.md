# auto

## Host machine
```
$ cat /etc/fedora-release
Fedora release 38 (Thirty Eight)
```

```
$ getenforce
Enforcing
```

### Installing required packages and copying config files
```
sudo dnf install -y osbuild osbuild-tools osbuild-ostree 
git clone https://gitlab.com/CentOS/automotive/sample-images
git clone https://github.com/dougsland/auto
pushd auto
sudo cp org.osbuild.fedora38 /usr/lib/osbuild/runners/ 
cp hirte.mpp.yml samples-images/osbuild-manifests/images/hirte.mpp.yml
popd
```

### Building AutoOS
```
cd sample-images
make cs9-qemu-hirte-regular.x86_64.qcow2
```

After the vm is generated, use the following command, `user: root` and `pass: password`:
```
./runvm --nographics cs9-qemu-hirte-regular.x86_64.qcow2
```
**To exit from qemu**: `CTRL-a c` and then `quit` into the qemu terminal

### References
https://sigs.centos.org/automotive/building/#installing-osbuild-on-centos-stream-8
https://gist.github.com/michael131468/ba35263a14143547d26f6f7d691a6a30
https://gitlab.com/lrossett/sample-images/-/commits/hirte-snapshot
