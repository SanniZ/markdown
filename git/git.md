# **git 操作**  

--------------------------------------------

## **git format-patch**  
    
### *从change-id 获取指定的patch*  
```
git log --pretty=oneline
8a86d4cc9fa745302eea178c9669a7eca8d92275 added linux
084476343d32219d0dbd1c5f69b5a090f3a4942d how to build .pyc
a33a6e786f9c8fe72cfe43985840d70dac6d851b rebuild arch
53ca6ade9427e1b6ba1cd63a1ab181eb88513b51 upload graph, image and table 

获取'how to build .pyc' and 'rebuild arch':
git format-patch 084476343d32219d0dbd1c5f69b5a090f3a4942d -2

it will create patches:
0001-rebuild-arch.patch
0002-how-to-build-.pyc.patch
```

--------------------------

## **git log**

### *git log -stat*  
用于查看patch修改文件列表  
  
### *git log --pretty=oneline*  
用于将log显示为一列  

--------------------------

## **git difftool  (使用图形界面diff修改内容)**  
  
### *git difftool --cached*  
查看stage中文件  
  
### *git difftool HEAD~ HEAD~2*  
查看HEAD~和HEAD~的文件  
  
-----------------------------

## **git status**  
  
### *git status -s*  
仅显示文件信息  
  
------------------------------