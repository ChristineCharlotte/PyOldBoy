# shutil模块
高级的文件、文件夹、压缩包 处理模块

shutil.copyfileobj(fsrc, fdst[, length])
将文件内容拷贝到另一个文件中

```python
import shutil
shutil.copyfileobj(open('old.xml', 'r'), open('new.xml', 'w'))
```


shutil.copyfile(src, dst)
拷贝文件

```python
import shutil
shutil.copyfile('f1.log', 'f2.log')  # 目标文件无需存在
```

shutil.copymode(src, dst)
仅拷贝权限。内容、组、用户均不变

```python
import shutil
shutil.copymode('f1.log', 'f2.log')  # 目标文件必须存在
```


shutil.copystat(src, dst)
仅拷贝状态的信息，包括：mode
bits, atime, mtime, flags

```python
import shutil
shutil.copystat('f1.log', 'f2.log')  # 目标文件必须存在
```


shutil.copy(src, dst)
拷贝文件和权限

```python
import shutil
shutil.copy('f1.log', 'f2.log')
```


shutil.copy2(src, dst)
拷贝文件和状态信息

```python
import shutil
shutil.copy2('f1.log', 'f2.log')
```


shutil.ignore_patterns(*patterns)
shutil.copytree(src, dst, symlinks=False, ignore=None)
递归的去拷贝文件夹

```python
import shutil
shutil.copytree('folder1', 'folder2',
                ignore=shutil.ignore_patterns('*.pyc', 'tmp*'))  # 目标目录不能存在，注意对folder2目录父级目录要有可写权限，ignore的意思是排除 
```

拷贝软连接
```python
import shutil
shutil.copytree('f1', 'f2', symlinks=True, ignore=shutil.ignore_patterns('*.pyc', 'tmp*'))

# 通常的拷贝都把软连接拷贝成硬链接，即对待软连接来说，创建新的文件
```

shutil.rmtree(path[, ignore_errors[, onerror]])
递归的去删除文件

```python
import shutil
shutil.rmtree('folder1')
```

shutil.move(src, dst)
递归的去移动文件，它类似mv命令，其实就是重命名。

```python
import shutil
shutil.move('folder1', 'folder3')
```


shutil.make_archive(base_name, format, ...)
创建压缩包并返回文件路径，例如：zip、tar


base_name： 压缩包的文件名，也可以是压缩包的路径。只是文件名时，则保存至当前目录，否则保存至指定路径，
- 如 data_bak = > 保存至当前路径
- 如： / tmp / data_bak = > 保存至 / tmp /
- format： 压缩包种类，“zip”, “tar”, “bztar”，“gztar”
- root_dir： 要压缩的文件夹路径（默认当前目录）
- owner： 用户，默认当前用户
- group： 组，默认当前组
- logger： 用于记录日志，通常是logging.Logger对象

```python
#将 /data 下的文件打包放置当前程序目录
import shutil
ret = shutil.make_archive("data_bak", 'gztar', root_dir='/data')
   
#将 /data下的文件打包放置 /tmp/目录
import shutil
ret = shutil.make_archive("/tmp/data_bak", 'gztar', root_dir='/data') 
```

shutil 对压缩包的处理是调用 ZipFile 和 TarFile 两个模块来进行的，详细：
```python
import zipfile

# 压缩
z = zipfile.ZipFile('laxi.zip', 'w')
z.write('a.log')
z.write('data.data')
z.close()

# 解压
z = zipfile.ZipFile('laxi.zip', 'r')
z.extractall(path='.')
z.close()
```

```python
import tarfile

# 压缩
t=tarfile.open('/tmp/egon.tar','w')
t.add('/test1/a.py',arcname='a.bak')
t.add('/test1/b.py',arcname='b.bak')
t.close()


# 解压
t=tarfile.open('/tmp/egon.tar','r')
t.extractall('/egon')
t.close()
```