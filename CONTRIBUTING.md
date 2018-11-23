## 帮忙

### 目录

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [重点讲讲 更新](#%E9%87%8D%E7%82%B9%E8%AE%B2%E8%AE%B2-%E6%9B%B4%E6%96%B0)
- [我翻译的结构](#%E6%88%91%E7%BF%BB%E8%AF%91%E7%9A%84%E7%BB%93%E6%9E%84)
- [脚本（2018-11-20）](#%E8%84%9A%E6%9C%AC2018-11-20)
  - [`.mds-list`存放需要翻译的文件路径, 像:](#mds-list%E5%AD%98%E6%94%BE%E9%9C%80%E8%A6%81%E7%BF%BB%E8%AF%91%E7%9A%84%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84-%E5%83%8F)
  - [`sync-en.sh`](#sync-ensh)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

`Issue` or `Pull` 😊

一般来说, 翻译贡献有 三种

- 勘误
- 校对
- 更新

### 重点讲讲 更新

| 原文     | 与日期    | 原文更新             | 更多             |
| -------- | --------- | -------------------- | ---------------- |
| [commit] | 2018 \*\* | ![last commit][last] | [中文翻译][more] |

[commit]: https://github.com/chinanf-boy/chinese-translate-list
[last]: https://img.shields.io/github/last-commit/chinanf-boy/chinese-translate-list.svg
[more]: https://github.com/chinanf-boy/chinese-translate-list

我会在, 翻译 repo 的开头, 放至 源项目的 commit

你可以通过

1. 如果项目留有的 源 md 文件, 通过直接将新的 commit md 文件覆盖, 然后翻译即可

2) `vscode 和 gitlen`

简单, 观察

3. `git diff`

```sh
git diff HEAD 9fdf6d1 readme.md >> my.patch
```

> ⚠️ 注意: 更新 新的 commit 🔗

### 我翻译的结构

| 名           | 曰                                       |
| ------------ | ---------------------------------------- |
| `source/`    | `repo` 的 git clone, 方便同步            |
| `fork/`      | `fork` 给自己之后的 git clone, 方便 Pull |
| `.mds-list`  | 存放 source 要翻译的文件路径             |
| `sync-en.sh` | 自动覆盖(根据`.mds-list`)本库旧版本英文原文md    |
| `readme.md`  | 翻译的 commit 与日期之类的信息           |

> 绝大多数情况, 我会保留英文原文文件

> 当然, 你也可以通过 `git submodule` 来与 英文项目 建立联系

### 脚本（2018-11-20）

#### `.mds-list`存放需要翻译的文件路径, 像:

```
./source/README.md
```

> 一般就存在了，可简单通过 `find source/**/*.md`输出到此文件

#### `sync-en.sh`

<details>

<summary>详细代码</summary>

```sh
cat './.mds-list' | while read line
do
    testseq="zh.md"
    if [[ $line =~ $testseq || "$line" == "" ]]; then
        echo "skip $line"
    else
        source_readme="./source/readme.md"
        lowline=`echo "$line" | awk '{print tolower($0)}'`
        zh=${line//source\//}
        dir=$(dirname $zh)
        # source/readme.md => en.md
        if [[ $lowline == $source_readme ]];then
        filename="en.md"
        else
        filename=$(basename $zh)
        fi
        echo "$line >> $dir/$filename"
        mkdir -p $dir && cp $line "$_/$filename"
    fi
done
```

</details>

<br>

自动将`.mds-list`每个路径,复制到 运行命令目录下，如：

- `source/REAMDE.md` => `./en.md`(防止覆盖了本库的 readme)
- `source/other.md` => `./other.md`

> 若要更新，需要确保运行`source`的`git pull`
