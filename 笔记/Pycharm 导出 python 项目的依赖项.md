### 实用小技巧】Pycharm 导出 python 项目的依赖项 requirements.txt

`方法一：直接使用命令`

```shell
pip freeze > requirements.txt
```

`方法二：通过pipreqs`

```shell
pip install pipreqs
pipreqs ./
```

`通过以上两种方法即可导出项目的依赖库，当移植到其他环境中时，可通过以下命令安装：`

```shell
pip install -r requirements.txt
```
