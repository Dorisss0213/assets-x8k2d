# Product Trend Watch — 编辑指南

线上看板：https://lab-product-trend-watch.zuru.host
（ZURU LAB TREND WATCH · Category → Trend → Product 结构）

## 怎么加产品 / 改内容

1. **改本地 `product-trend-watch/index.html`**（先 `git pull` 拿最新版）
   - 加产品：在 `PRODUCTS` 数组末尾加一个对象（照抄现有卡片的字段结构）：
     ```js
     {
       id: "brand-product-name",          // 唯一，小写+连字符
       category: "oral-care",             // 必须是 CATEGORIES 里已有的 id
       trend: "skinification",            // 必须是 TRENDS 里已有的 id
       brand: "品牌名",
       name: "产品名",
       features: [                        // ≤3 条，品牌名外全英文
         "<strong>卖点标题</strong> — 描述",
       ],
       sales: "xxx sold <span class=\"hedge\">(来源)</span>",  // 可选，只放销量/社交数据
       media: [                           // 图片/视频，顺序 = 轮播顺序
         { type: "video", src: "CDN链接.mp4", poster: "CDN链接.webp" },
         { type: "image", src: "CDN链接.webp", alt: "描述" },
       ],
     }
     ```
   - 加新 category / trend：改顶部 `CATEGORIES` / `TRENDS` 数组（id 全局唯一，同名趋势在不同品类下用不同 id，如 `disruptive-format` vs `disruptive-format-snacking`）
2. **图片/视频**：压缩成 webp（图片 ≤1000px 宽）和 ≤5MB 的 mp4，放进 `product-trend-watch/media-webp/`（图）和 `media/`（视频），提交推送；HTML 里用 CDN 链接：
   `https://fastly.jsdelivr.net/gh/dorisss0213/beauty-innovation-media@main/product-trend-watch/media-webp/文件名.webp`
3. **本地预览**：`python -m http.server 8899`，浏览器开 `http://127.0.0.1:8899/index.html`
4. **部署**：`git push` 后执行 `deploy push index.html lab-product-trend-watch`
   （需要 ZURU Deploy 权限 + 本机装 CLI：`curl -fsSL https://deploy.zuru.host/install | sh` 然后 `deploy login`）

## 注意

- `PRODUCTS` 里的 category/trend 引用必须存在，否则卡片不会渲染
- 图片加载失败会自动回退 raw.githubusercontent.com（jsDelivr 限流 403 时）
- 改完先本地预览再部署
