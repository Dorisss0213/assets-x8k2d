# Product Trend Watch — 编辑指南

线上看板：https://lab-product-trend-watch.zuru.host
（ZURU LAB TREND WATCH · Category → Trend → Product 结构）

## 怎么加产品 / 改内容

1. **改本地 `product-trend-watch/data.json`**（先 `git pull` 拿最新版）
   - 所有 category、trend、product 数据只存放在此独立数据文件中，不要内嵌到 `index.html`；HTML 只负责加载和渲染。
   - 加产品：在 `products` 数组末尾加一个对象（`id` 必须唯一且为小写+连字符；`features` ≤3 条，品牌名外全英文；`sales` 可选，只放销量/社交/奖项数据；`media` 顺序 = 轮播顺序）：
     ```json
     {
       "id": "brand-product-name",
       "category": "oral-care",
       "trend": "skinification",
       "brand": "品牌名",
       "name": "产品名",
       "features": [
         "<strong>卖点标题</strong> — 描述"
       ],
       "sales": "xxx sold <span class=\"hedge\">(来源)</span>",
       "media": [
         { "type": "video", "src": "CDN链接.mp4", "poster": "CDN链接.webp" },
         { "type": "image", "src": "CDN链接.webp", "alt": "描述" }
       ]
     }
     ```
   - 加新 category / trend：改 `data.json` 的 `categories` / `trends` 数组（id 全局唯一，同名趋势在不同品类下用不同 id，如 `disruptive-format` vs `disruptive-format-snacking`）
2. **图片/视频**：压缩成 webp（图片 ≤1000px 宽）和 ≤5MB 的 mp4，放进 `product-trend-watch/media-webp/`（图）和 `media/`（视频），提交推送；`data.json` 里用 CDN 链接：
   `https://fastly.jsdelivr.net/gh/dorisss0213/beauty-innovation-media@main/product-trend-watch/media-webp/文件名.webp`
3. **本地预览**：`python -m http.server 8899`，浏览器开 `http://127.0.0.1:8899/index.html`
4. **部署**：先 `git push`，再将 `index.html` 和 `data.json` 一起部署到 `lab-product-trend-watch`（两个文件必须同时更新，不能只上传 HTML）。
   （需要 ZURU Deploy 权限 + 本机装 CLI：`curl -fsSL https://deploy.zuru.host/install | sh` 然后 `deploy login`）

## 注意

- **deploy 会整页覆盖线上的 index.html**：动手改之前先 git pull，改完先 git push 再 deploy push——否则别人已部署但未 push 的修改会被你覆盖丢失（2026-11-26 发生过一次，Yuki 的卡片被覆盖，靠服务器媒体恢复）

- `data.json` 中 `products` 的 category/trend 引用必须存在，否则卡片不会渲染
- 图片加载失败会自动回退 raw.githubusercontent.com（jsDelivr 限流 403 时）
- 改完先本地预览再部署
