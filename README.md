# Linked Source Routing

用于处理“用户已经给了链接”的起始路由 Skill。

它不负责通用输入理解，也不代替主 Agent 的整体任务判断；它只解决一件事：

- 链接是什么类型
- 应该先拿什么内容
- 该走直取、`web-access` CDP、`video-to-summary`，还是 `browser-troubleshooting`

## 适用场景

- 用户直接贴了文章、文档、GitHub、视频、短视频、动态网页链接
- 主流程还没开始，但需要先决定 acquisition path
- 想把“链接获取层”从研究、视频、网页取数等 Skill 里解耦出来复用

## 路由原则

- 静态页面先走低成本 fetch
- 只要拿到的是 challenge HTML、空壳、局部内容，就立刻升级
- 视频链接直接转给 `video-to-summary`
- JS-heavy / 登录 / 反爬页面直接走 `web-access` CDP

## License

MIT
