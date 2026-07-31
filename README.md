# gallery 分支

hoperp.com `/gallery/` 用的图片仓库。**这是一个孤儿分支**，和 `main` 没有共同历史，
只放图片，不要往里合并任何别的内容。

```
full/    原图
thumb/   缩略图（长边 900，webp q78）
```

两条不能破的约定：

1. **文件名即版本。** 这些图经 `img.hoperp.com` 提供，响应是 `immutable` 一年。
   改图必须换文件名，覆盖同名文件会让边缘和浏览器长期返回旧图，而且很难排查。
2. **thumb/ 不要手工放。** 由 hoperp-site 的 `npm run gallery -- <本仓库路径>` 生成，
   同时会把真实像素宽高写进站点的 `src/_data/gallery.json`（远程图构建期读不到尺寸，
   少了宽高滚动加载会一路抖）。

发一批新图：

```bash
git checkout gallery
cp ~/新图/*.webp full/
cd <hoperp-site> && npm run gallery -- <本仓库路径>
cd -  && git add -A && git commit -m "gallery: add N images" && git push
```

细节见 hoperp-site 的 `docs/gallery.md`。
