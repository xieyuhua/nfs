
### s3fs-fuse

```
sudo yum install epel-release
sudo yum install s3fs-fuse

echo ACCESS_KEY_ID:SECRET_ACCESS_KEY > ${HOME}/.passwd-s3fs
chmod 600 ${HOME}/.passwd-s3fs


# 使用文本编辑器打开 hosts 文件
sudo vi /etc/hosts

# 在文件末尾添加以下格式的内容（示例）
100.86.2.1:80  xinan1.zos.ctyun.cn
100.86.2.1:80  Bucket.xinan1.zos.ctyun.cn

# 刷新 DNS 缓存（CentOS 7/8）
sudo systemctl restart NetworkManager

# 验证解析
ping xinan1.zos.ctyun.cn
nslookup Bucket.xinan1.zos.ctyun.cn


s3fs Bucket /s3fs/data -o passwd_file=~/.passwd-s3fs -o url=http://100.86.2.1  -o dbglevel=info -f -o curldbg
s3fs Bucket /s3fs/data -o passwd_file=./.passwd-s3fs -o url=https://xinan1.zos.ctyun.cn  -o dbglevel=info -f -o curldbg

umount /s3fs/data
umount -l /s3fs/data
```


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
