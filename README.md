# auto
A repo with infomation regarding CentOS and Automotive.

## Generating CentOS Automotive image with hirte and qm packages
This is temporary workaround until the packages get officially
included.

#### Host machine
```
$ cat /etc/fedora-release
Fedora release 38 (Thirty Eight)
```

```
$ getenforce
Enforcing
```

#### Installing required packages and copying config files
```
sudo dnf install -y osbuild osbuild-tools osbuild-ostree 
git clone https://gitlab.com/CentOS/automotive/sample-images
git clone https://github.com/dougsland/auto
pushd auto
sudo cp org.osbuild.fedora38 /usr/lib/osbuild/runners/ 
cp hirte.mpp.yml ../sample-images/osbuild-manifests/images/
popd
```

Please note, the trick in the yml file is below.  
In this example, we are adding repos from copr (`mperina/hirte` and `podman-next`).
```
  <snip>
  - - id: copr-hirte-snapshot
      baseurl: https://download.copr.fedorainfracloud.org/results/mperina/hirte-snapshot/centos-stream-9-$arch
  - - id: podman-next
      baseurl: https://download.copr.fedorainfracloud.org/results/rhcontainerbot/podman-next/centos-stream+epel-next-9-$arch/
  packages:
    mpp-join:
    - mpp-eval: image_rpms
    - mpp-eval: extra_rpms
    - - podman
      - containernetworking-plugins
      - curl
      - hirte
      - qm
  <snip>
```
#### Building AutoOS
During the build process, the password for the root user will be requested.
```
cd sample-images/osbuild-manifests/
make cs9-qemu-hirte-regular.x86_64.qcow2

$ ls *.qcow2
cs9-qemu-hirte-regular.x86_64.qcow2
```

Afer the vm is generated, use the following command to run in the console.  
Please note, the default user and password is: 'root` and `password`:
```
./runvm --nographics cs9-qemu-hirte-regular.x86_64.qcow2
```
**To exit from qemu**: `CTRL-a c` and then `quit` into the qemu terminal

### References
https://sigs.centos.org/automotive/building/#installing-osbuild-on-centos-stream-8
https://gist.github.com/michael131468/ba35263a14143547d26f6f7d691a6a30
https://gitlab.com/lrossett/sample-images/-/commits/hirte-snapshot
