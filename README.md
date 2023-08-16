### nfs

``
./go-nfs /home/gogs/ 8084
./s3nfsd serve --addr :8888
``

``

mount -o port=8084,mountport=8084,nfsvers=3,noacl,tcp -t nfs 192.168.9.27:/home/gogs /home/gogs
umount /home/gogs
mount -o port=8084,mountport=8084,nfsvers=3,noacl,tcp -t nfs 10.0.16.5:/ /home/s3
df -h
``

### goreleaser  

``
goreleaser init 
``

``
go mod vendor
goreleaser --snapshot --skip-publish --rm-dist
``
