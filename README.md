# Github 原始文件代理

-   Github 原始文件代理
-   私有仓库认证代理，多用户 (仓库) 独立 Token 认证
-   最大可能保持原站点功能（缓存控制，协商头，断点续传）

## 部署方式

-   **部署 Workers**：`创建 Workers` --> `复制` [src/index.js](https://github.com/pierreteam/cfworker-github-proxy/blob/main/src/index.js) --> `粘贴到 _worker.js` --> `保存并部署`

-   **部署 Workers (Wrangler CLI)**: `Clone 项目` --> 执行命令 `npm install`，安装依赖 --> 执行命令 `npm run deploy`，部署

-   **部署 Workers (GitHub Action)**：`Fork 项目` --> `配置 secrets` --> `运行 GitHub Action`

-   **部署 Workers (GitHub Action) 快捷部署**：

    [![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/pierreteam/cfworker-github-proxy)

## 参数配置

进入 Cloudflare 管理面板，修改环境变量为自己的值，保存并重新部署；如果无特殊要求可跳过

| 环境变量名 | 可选值                                        | 说明         | 示例                    |
| ---------- | --------------------------------------------- | ------------ | ----------------------- |
| HomeMode   | default<br>rewrite<br>redirect                | 主页伪装模式 | "default"               |
| HomePage   | 网址；必须带协议头<br>`https://` 或 `http://` | 主页         | "https://www.baidu.com" |

| 秘密      | 可选值                                    | 说明   | 示例                                                 |
| --------- | ----------------------------------------- | ------ | ---------------------------------------------------- |
| Password  | URL 规范允许的字符                        | 口令   | "666666"                                             |
| AuthTable | URL 规范允许的字符<br>分隔符 `:` `;` 除外 | 授权表 | "user1:token1;user2/repo1:token2;user2/repo2:token3" |

## 自定义域名

三种自动路由方式：

-   **自定义域名**

    进入 Cloudflare 管理面板添加自定义**域名**；

    > 域名格式 `<自定义域名>`

-   **自定义路由** + **CNAME 记录**，实现优选 **（无本地 DNS 服务时，推荐）**

    进入 Cloudflare 管理面板添加自定义**路由**；

    > 路由格式 `<自定义域名>/*`

    进入 Cloudflare DNS 解析添加 CNAME 记录，把自定义域名解析为优选域名；

-   **自定义域名/路由** + **本地 DNS 服务**，实现优选 **（有本地 DNS 服务时，推荐）**

    进入 Cloudflare 管理面板添加自定义**域名/路由**；

    > 域名格式 `<自定义域名>`；路由格式 `<自定义域名>/*`；

    在本地的 DNS 服务添加 CNAME 记录，把自定义域名解析为优选域名；
