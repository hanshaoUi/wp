# gallery 分支

hoperp.com `/gallery/` 用的图片仓库。**这是一个孤儿分支**，和 `main` 没有共同历史，
只放图片，不要往里合并任何别的内容。

```
full/    原图（你只往这里放东西）
thumb/   缩略图，由脚本生成，不要手工放
```

## 图片要求

完整规范在站点仓库的 **`docs/gallery-image-spec.md`**（命名、格式、尺寸、要配哪些文字）。
最容易踩的三条摘在这里：

1. **文件名就是版本号，永远不要覆盖同名文件。** 这些图经 `img.hoperp.com` 提供，
   响应是 `immutable` 缓存一年，覆盖同名文件会让边缘和浏览器长期返回旧图。改图 = 换文件名。
2. **只用小写英文、数字、短横线命名。** Worker 的路径白名单只放行 `[A-Za-z0-9._-]`，
   带中文、空格、括号的文件名会直接 404。推荐 `2026-08-01-neon-alley-1536x1024.webp`。
3. **先推图片，再推站点。** 反了线上会出现一批 404 的图，而一张 404 会让整页
   卡在「100 % LOADING」（模板的加载器要等所有图都成功）。

## 发一批新图

```bash
git checkout gallery
cp ~/新图/*.webp full/
cd <hoperp-site> && npm run gallery -- <本仓库路径>
cd - && git add -A && git commit -m "gallery: add N images" && git push
```

同步脚本会生成缩略图、读出真实像素宽高并写进站点的 `src/_data/gallery.json`
（远程图构建期读不到尺寸，少了宽高滚动加载会一路抖）。之后回站点补 title 和 prompt。

架构和排障见站点仓库的 `docs/gallery.md`。
