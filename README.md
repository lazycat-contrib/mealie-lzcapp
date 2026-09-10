# Mealie for LazyCat

Mealie is a self-hosted recipe manager and meal planner for the whole family. Import recipes by URL or create them with the visual editor.

主页：https://github.com/mealie-recipes/mealie/

## 使用

要求懒猫微服 1.5.0 或更新版本，目标架构 amd64。首次登录账号为 `changeme@example.com`，密码为 `MyPassword`，请登录后修改默认账号和密码。

保留 `ALLOW_SIGNUP=false`、UID/GID 1000、`TZ=America/Anchorage` 和 `1000M` 内存限制。`BASE_URL` 自动使用应用实际 HTTPS 域名。内部 HTTP 服务监听 9000，由懒猫入口转发，无需占用宿主 9925 端口。

`/lzcapp/var/data` 对应容器 `/app/data`，存放数据库、食谱、图片和备份。使用上游默认 SQLite，无需额外数据库服务。镜像入口负责切换运行用户和设置目录权限。

保留手动登录，不添加文件选择器；继承镜像自带的健康检查。

## 构建与发布

```sh
mkdir -p dist
lzc-cli project release -o dist/application.lpk
lzc-cli lpk info dist/application.lpk
```

仅发布喵喵商店，官方商店关闭。使用 `ghcr.1ms.run` 加速镜像，自动更新时校验 amd64 摘要与上游一致。初始版本 3.25.1，每日跟踪三段式稳定版本。

工作流引用组织级 `APPSTORE_URL`、`APPSTORE_TOKEN` 及可选的 `PRIVATE_STORE_GROUP_CODES`。喵喵商店引用 GitHub Release 文件 `community.lazycat.app.mealie-v<version>.lpk` 及其 SHA256。

图标由用户提供，应用代码与许可见上游。构建与发布验证不替代微服上的登录、食谱导入和持久化实测。
