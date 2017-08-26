---
layout:     post
title:      "Python标准日志模块"
subtitle:   ""
date:       2017-07-20 09:46:11
author:     "Alvey"
header-img: "img/home-bg-o.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Python
---

>- 模块：logging
 - 用途：脚本运行状况写入日志和输出到控制台


#### logging日志函数
---
```
# -*- coding:utf-8 -*-
import logging
import os


def setlogger(information):

    logger = logging.getLogger('logname')
    logger.setLevel(logging.DEBUG)

    # 创建一个handler，用于写入日志文件
    fh = logging.FileHandler(os.path.join(os.getcwd(), 'yourlog.txt'))
    fh.setLevel(logging.DEBUG)

    # 再创建一个handler，用于输出到控制台
    ch = logging.StreamHandler()
    ch.setLevel(logging.DEBUG)

    # 定义handler的输出格式
    formatter = logging.Formatter('%(asctime)s - %(module)s - %(levelname)s - %(message)s')
    fh.setFormatter(formatter)
    ch.setFormatter(formatter)

    # 给logger添加handler
    logger.addHandler(fh)
    logger.addHandler(ch)

    # 记录一条日志
    logger.info(information)

    # 移除Handler，否则会重复打印日志
    logger.removeHandler(ch)
    logger.removeHandler(fh)

    return logger

setlogger("this is the log infomation example")
```

#### log级别高低顺序
- logger.setlevel(loggging.DEBUG)
    - NOTSET < DEBUG < INFO < WARNING < ERROR < CRITICAL
    - 举个栗子：如果设定级别为WARNING，logger.waring("infomation")，那么只有大于WARNING的 ERROR和CRITICAL可以写入日志，其余小于它的则会忽略。

#### 日志重复打印问题
- 问题描述：比如执行
```
setlogger("111111")
setlogger("222222")
setlogger("333333")
```

- 生成的log中会有以下神奇的效果：
```
2017-07-20 15:57:06,729 - temp_2 - INFO - 111111
2017-07-20 15:57:11,730 - temp_2 - INFO - 222222
2017-07-20 15:57:11,732 - temp_2 - INFO - 222222
2017-07-20 15:57:12,736 - temp_2 - INFO - 333333
2017-07-20 15:57:17,740 - temp_2 - INFO - 333333
2017-07-20 15:57:17,842 - temp_2 - INFO - 333333
```

- 问题原因及解决：
    1. 每次创建不同name的logger，每次都是新logger，不会有添加多个handler的问题。（ps:这个办法太笨）
    2. 像上面一样每次记录完日志之后，调用removeHandler(）把这个logger里的handler移除掉
    3. 在log方法里做判断，如果这个logger已有handler，则不再添加handler
    4. 与方法2一样，不过把用pop把logger的handler列表中的handler移除

- 方法2
```
logger.removeHandler(ch)
logger.removeHandler(fh)
```

- 方法3
```
    if not logger.handlers:

        # 创建一个handler，用于写入日志文件
        fh = logging.FileHandler(os.path.join(os.getcwd(), 'csi_log.txt'))
        fh.setLevel(logging.DEBUG)

        # 再创建一个handler，用于输出到控制台
        ch = logging.StreamHandler()
        ch.setLevel(logging.DEBUG)

        # 定义handler的输出格式
        formatter = logging.Formatter('%(asctime)s - %(module)s - %(levelname)s - %(message)s')
        fh.setFormatter(formatter)
        ch.setFormatter(formatter)

        # 给logger添加handler
        logger.addHandler(fh)
        logger.addHandler(ch)
```

- 方法4：注意如果add了多个handler，这里需多次pop，或者可以直接为handlers列表赋空值
```
logger.handlers.pop()
```
