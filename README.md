
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
100.86.2.1:80  mount.xinan1.zos.ctyun.cn

# 刷新 DNS 缓存（CentOS 7/8）
sudo systemctl restart NetworkManager

# 验证解析
ping xinan1.zos.ctyun.cn
nslookup mount.xinan1.zos.ctyun.cn


s3fs mount /s3fs/data -o passwd_file=~/.passwd-s3fs -o url=http://100.86.2.1  -o dbglevel=info -f -o curldbg
s3fs mount /s3fs/data -o passwd_file=./.passwd-s3fs -o url=https://xinan1.zos.ctyun.cn  -o dbglevel=info -f -o curldbg

umount /s3fs/data
umount -l /s3fs/data


推荐配置（无性能影响）

s3fs mybucket /mnt/s3 -o passwd_file=~/.passwd-s3fs \
    -o url=https://correct-endpoint.com \
    -o max_stat_cache_size=100000 \
    -o stat_cache_expire=900 \
    -o enable_noobj_cache \
    -o parallel_count=15 \
    -o multireq_max=20

# 提高性能的核心参数
-o parallel_count=20      # 增加并行传输线程数
-o multireq_max=100       # 提高批量请求数量
-o max_stat_cache_size=200000  # 增大元数据缓存
-o enable_noobj_cache     # 缓存空目录查询结果
-o use_cache=/tmp/s3cache # 启用本地磁盘缓存
-o stat_cache_expire=900  # 元数据缓存过期时间(秒)

异步日志
# 减少同步写入开销
s3fs mount /s3fs/data -o passwd_file=./.passwd-s3fs -o url=https://xinan1.zos.ctyun.cn 2>&1 | rotatelogs /s3fs/log/s3fs_%Y%m%d.log 10M


一、生产环境推荐

# 完全禁用调试日志(最高性能)
s3fs mount /s3fs/data -o passwd_file=./.passwd-s3fs -o url=https://xinan1.zos.ctyun.cn -o dbglevel=err

二、平衡方案(保留必要日志

# 只记录警告和错误
s3fs mount /s3fs/data -o passwd_file=./.passwd-s3fs -o url=https://xinan1.zos.ctyun.cn -o dbglevel=warn


观察  iostat -x 1  查看磁盘I/O情况
使用  iftop  监控网络带宽使用
检查  dmesg  是否有相关错误
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
