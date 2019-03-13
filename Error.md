# 部分报错的解决方法  

## 1.  
```bash
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
```
解决方法：  
在终端输入：  
```bash
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```


## 2.  
