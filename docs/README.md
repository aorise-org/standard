# 研发规范

公司针对研发工作、研发流程的规范和约束。

## 开发规范

- [UI规范](https://github.com/aorise-org/standard/blob/master/standard/UI规范.md)
- [Android开发环境配置](https://github.com/aorise-org/standard/blob/master/standard/Android开发环境配置.md)
- [Android代码规范](https://github.com/aorise-org/standard/blob/master/standard/Android代码规范.md)
- [IOS开发环境配置](https://github.com/aorise-org/standard/blob/master/standard/IOS开发环境配置.md)
- [IOS代码规范](https://github.com/aorise-org/standard/blob/master/standard/IOS代码规范.md)
- [~~APIs规范~~](https://github.com/aorise-org/standard/blob/master/standard/APIs规范.md)

## 流程规范

NA

## 版本规范

版本格式采用语义化版本号表示：主版本号.次版本号.修订号 `MAJOR.MINOR.PATCH`，版本号递增规则如下：  

> 主版本号 `MAJOR`：当你做了不兼容的 API 修改；    
> 次版本号 `MINOR`：当你做了向下兼容的功能性新增；   
> 修订号 `PATCH`：当你做了向下兼容的问题修正。   


例如APP发布版本的文件名字规范：
**aorise-[项目名字]-[版本号]-[日期]-[类型].后缀**       
例如 `v1.0.0`版本的apk文件命名规则：`aorise-education-v1.0.0-20160921-release.apk`  

## Git仓库规范

Git仓库命名建议：**[类型]-[描述|可选]-[项目名字]**    
例如android教育项目建议git仓库名字：`android-education`

Git分支规范：git仓库一定有两个常驻分支 `master` `develop`。随着项目的不断进行，可能还会有许多`release`分支，release分支采用对应量产版本的第一个语义化版本号表示，例如第一个正式发布的量产版本的分支: `v1.0.0`分支。

分支功能介绍：
- `master`分支：非工作分支，merge当前即将发布的分支的代码，配合服务器每日构建和版本发布。
- `develop`分支：工作分支，基本上项目的开发工作全部基于develop分支进行。
- `release`分支：各个进入稳定迭代周期或者量产状态的分支，主要服务于准备上线或者已经上线的代码的维护和BUG修改。

分支名字  |  是否直接发布版本 |  是否工作区间 
--|:---:|:---:
master  |  是 |  否
develop  | 否  |  是
release | 否  |  是


## 附录

- [easy-mock](https://easy-mock.com): 第三方接口服务
- [aorise-easy-mock](http://10.16.3.26:7300/): 公司第三方接口服务