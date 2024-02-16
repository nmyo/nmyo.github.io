### 创建仓库
1. 打开Github,点击头像选择`New repository`创建一个新的仓库
2. 填写仓库先关资料，一般只需要选一个合适的仓库名，然后确保仓库为 `public`
3. 到此GitHub创建仓库完成，下面开始配置PicGo
----
###  PicGo配置方法如下

>1.  首先，我们先要去 Github 创建一个 token:
>>1. 打开 `Settings -> Developer settings -> Personal access tokens`，最后点击 `generate new token`
>>1. 填写及勾选相关信息[^1]，然后点击 `Genetate token` [^2]即可

[^1]:此处填写用途然后勾选repo即可
[^2]:注意它只会显示一次，所以你最好把它复制下来到你的备忘录存好，方便下次使用，否则下次有需要重新新建
>1. 配置 PicGo，依次打开 `图床设置` -> `Github 图床`
>1. 填入仓库名 `nmyo/pictures`
>2. 填入分支 `main`
>3. 填入前面获取的令牌`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`
>4. 指定存储路径 `new/`
>5. 设置CDN`https://cdn.jsdelivr.net/gh/`**你的用户名/你的仓库名**