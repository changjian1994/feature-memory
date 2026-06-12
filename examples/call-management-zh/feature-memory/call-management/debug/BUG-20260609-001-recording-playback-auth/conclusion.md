# Conclusion

## 记忆索引

- Bug ID：`BUG-20260609-001-recording-playback-auth`
- 所属 feature：`call-management`
- 根因分类：`auth/browser-native-request`
- 关键词：`recording`、`401`、`audio`、`auth header`、`blob`

## 当前状态

- 状态：`RESOLVED`
- 最新结论：后端录音代理可用，但浏览器原生播放请求没有携带必需的鉴权头，导致播放失败。

## 已确认事实

- 通过带鉴权的 HTTP 请求调用录音代理接口时，可以拿到音频响应。
- 同一录音通过浏览器原生 `<audio>` 或直接打开 URL 时失败。
- 浏览器原生资源请求没有经过前端 HTTP 拦截器。
- 录音路由必须保持鉴权，不能为了播放方便改成公开接口。

## 修复方式

- 新增前端 API helper，通过带鉴权的 HTTP client 以 blob 形式请求录音文件。
- 使用 blob 创建 object URL，供 `<audio>` 播放和下载。
- 详情关闭或录音列表刷新时回收 object URL。

## 可复用经验

当带鉴权的文件、图片、视频或音频接口在 HTTP client 测试中可用，但浏览器播放或下载返回 `401` 时，优先检查是否是原生资源请求绕过了鉴权拦截器。

## 安全说明

- 不记录对象存储凭证。
- 不把真实生产 object key 写入 feature memory。
- 不粘贴未脱敏的生产响应体或日志原文。
