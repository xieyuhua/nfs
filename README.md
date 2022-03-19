### nfs

``
./go-nfs /home/gogs/ 8084
``

``
mount -o port=8084,mountport=8084,nfsvers=3,noacl,tcp -t nfs 192.168.9.27:/home/gogs /home/gogs
umount /home/gogs
``

### goreleaser  

``
goreleaser init 
``

``
go mod vendor
goreleaser --snapshot --skip-publish --rm-dist
``
