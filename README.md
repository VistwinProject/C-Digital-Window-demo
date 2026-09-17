# C 區 數位窗景 — 展示版

**只有編譯輸出,原始碼在私有 repo:** [`VistwinProject/C-Digital-Window`](https://github.com/VistwinProject/C-Digital-Window)

沿用 org 既有的 `x-controller-demo` 慣例。之所以另開一個 repo,是因為這個 org 是 free 方案、private repo 不支援 Pages,而把主 repo 改成 public 需要 owner 權限。

| 路徑 | 內容 |
|---|---|
| `/` | 影片背景窗景(Next.js 靜態匯出) |
| `/breeze/` | shader 微風背景(Vite / Three.js) |

## 按鍵

| 鍵 | 作用 |
|---|---|
| `1`–`6` | 切換背景影片 |
| `A` / `S` / `D` / `F` / `G` | 精簡卡片 / 儀表板 / 詳細版 / 無框版 / 面板版 |
| `L` / `R` | 雙螢幕裁切:顯示完整畫布的左半 / 右半(只在直式視口生效) |
| `W` | 疊出實體窗框的框料,佈場對位用 |
| `I` | 狀態角標(這台是哪一半、同步有沒有連上) |
| `Q` | 切換無濾鏡的備援玻璃 |

## 這裡看不到的東西

**跨裝置同步不會運作。** https 頁面連不上 `ws://`,而 GitHub Pages 只能給靜態檔、架不了 WebSocket server。雙螢幕展場要從展場機器以 http 提供頁面,並跑 `npm run sync`。這份是給團隊與業主預覽用的。

## 影片放哪裡

**影片不進 git**，掛在本 repo 的 [`videos`](../../releases/tag/videos) Release。Pages 的 workflow 會在建置時下載。

之所以這樣做：影片放進 git 的話，每換一支就多一份約 50MB 的 blob，而且**永遠刪不掉**。先前用「分支直接發布」時歷史已經累積到 13 份影片 blob、遠端 208MB。Release 附件不算 git 歷史。

**換影片**：覆蓋 Release 附件就好，git 不會動。

```bash
gh release upload videos 7.mp4 --clobber --repo VistwinProject/C-Digital-Window-demo
gh workflow run deploy.yml --repo VistwinProject/C-Digital-Window-demo
```

支數變動時要同步改 workflow 裡的 `VIDEO_COUNT`（那是刻意寫死的檢查：抓不到就讓建置失敗，因為靜默變成黑背景比壞掉更難發現）。

## 怎麼更新程式

在私有 repo 建置後把產物覆蓋到這裡（**不要**把 `videos/` 一起帶進來，`.gitignore` 已經擋了）：

```bash
# apps/window
NEXT_PUBLIC_BASE_PATH=/C-Digital-Window-demo npm run build -w @baopu-c/window
# apps/breeze
npm run build -w @baopu-c/breeze -- --base=/C-Digital-Window-demo/breeze/
```

`apps/window/out` 放根目錄、`apps/breeze/dist` 放 `breeze/`。


