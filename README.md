### nfs

```
go-nfs /home/gogs/ 8084
s3nfsd serve --addr :8888
```

```

mount -o port=8084,mountport=8084,nfsvers=3,noacl,tcp -t nfs 192.168.9.27:/home/gogs /home/gogs
umount /home/gogs
umount -l /home/gogs
mount -o port=8084,mountport=8084,nfsvers=3,noacl,tcp -t nfs 10.0.16.5:/ /home/s3

nfs 挂载时可选参数：
　　timeo ：如果超时，客户端等待的时间，以十分之一秒计算
　　retrans ：超时尝试的次数
　　bg ：后台挂载
　　hard ：如果server端没有响应，那么客户端一直尝试挂载
　　wsize ：写块大小
　　rsize ：读块大小
　　intr ：可以中断不成功的挂载
　　noatime ：不更新文件的 inode 访问时间，可以提高速度
　　async ：异步读写

mount  -t nfs -o nolock, rsize=1024,wsize=1024,timeo=15 192.168.0.124:/home/admin/rootfs /mnt



df -h
```

### goreleaser  

```
goreleaser init 
```

```
go mod vendor
goreleaser --snapshot --skip-publish --rm-dist
```
