# Auto scaling 故障排除測試


## 增加 CPU 使用量

可以使用 "[create cpu load in python](https://gist.github.com/tott/3895832)" 這段程式碼，執行 python 程式增加 CPU 使用量

```
$ vim python-cpu.py
```

```python
#!/usr/bin/env python
"""
Produces load on all available CPU cores
"""

from multiprocessing import Pool
from multiprocessing import cpu_count

def f(x):
    while True:
        x*x

if __name__ == '__main__':
    processes = cpu_count()
    print 'utilizing %d cores\n' % processes
    pool = Pool(processes)
```

```shell
# 設定背景執行
$ python python-cpu.py &
[1] 8843

utilizing 1 cores
```

## 監控 CPU 使用狀態

```
$ sudo apt-get install htop
```



## 限制程序 CPU 執行上限

因為 "[create cpu load in python](https://gist.github.com/tott/3895832)" 會將所有 CPU 資源吃滿，可以用 `cpulimit` 來控制程序的 CPU 執行上限

```shell
sudo apt-get install cpulimit
```


```shell
# 限制指定程序 CPU 執行上限為 50 %
$ cpulimit -p 8844 -l 50
Process 8844 detected
```

## 參考資料
* [Google Cloud Load balancer & Autoscaling in ACTION!! (Udemy link below with discount code!) - YouTube](https://www.youtube.com/watch?v=Gn7pGQYkKnA)
