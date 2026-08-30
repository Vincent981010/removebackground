# 去背工作台

自動去背 + 手動筆刷(恢復 / 抹除)的單頁網頁工具。AI 去背模型與執行程式已經整包放在 `assets/` 資料夾裡,**不會向任何第三方伺服器發送請求或下載檔案**,所有運算都在使用者的瀏覽器裡進行。

## 檔案結構

```
index.html                          網頁本體
assets/onnxruntime-web/             AI 推論引擎(onnxruntime-web,~12MB)
assets/isnet-anime-model/           AI 模型(isnet-anime,動漫插畫專用,~168MB,切成多個分片)
assets/isnet-general-model/         AI 模型(isnet-general-use,真人/一般照片專用,~171MB,切成多個分片)
```

這個版本會自動判斷照片是「動漫插畫」還是「真人照片」,並自動選用對應的模型:

- **動漫插畫** → [isnet-anime](https://github.com/danielgatis/rembg)
- **真人 / 一般照片** → [isnet-general-use](https://github.com/danielgatis/rembg)

判斷方式是分析照片的色彩豐富度與色塊平坦程度(不需要額外下載模型,運算很快),不保證每次都準。如果判斷錯誤,可以在網頁上的「模型」下拉選單手動指定要用哪個模型,再重新執行一次自動去背。

只有實際用到的那個模型才會被瀏覽器下載,不會兩個一起載入,所以不會讓網頁變得特別慢 —— 但因為兩個模型都放進了 repo 裡,整個專案資料夾變大了(約 350MB),`git push` 上傳時間會拉長。

部署或搬動這個工具時,請務必讓 `assets/` 資料夾跟 `index.html` 保持在同一層、路徑不變,否則自動去背會找不到模型檔案。

## 部署到 GitHub Pages

1. 把整個資料夾(含 `assets/`)推到你的 GitHub repo。
2. 到 repo 的 Settings → Pages,把 Source 設成你放這些檔案的分支 / 資料夾。
3. 等 Pages 建置完成後,用它給的網址開啟即可,不需要任何額外設定。

## 本機測試(不透過 GitHub)

直接用滑鼠雙擊 `index.html` **無法**正常載入模型,這是瀏覽器對 `file://` 開啟頁面的安全限制(會擋掉讀取旁邊資料夾檔案的請求),跟這個工具本身無關。要在本機測試,請在這個資料夾裡起一個簡易伺服器,例如:

```bash
# 若電腦有裝 Python
python3 -m http.server 8000
```

然後瀏覽器開啟 `http://localhost:8000` 即可,效果跟部署到 GitHub Pages 後完全一樣。

## 授權

- `assets/onnxruntime-web/`:微軟 [onnxruntime-web](https://github.com/microsoft/onnxruntime),**MIT 授權**。
- `assets/isnet-anime-model/`:模型與程式邏輯參考 [danielgatis/rembg](https://github.com/danielgatis/rembg) 的 isnet-anime 模型,**MIT 授權**。

兩者都是寬鬆授權,個人或商業用途都可以直接使用,附上授權原文供參考。
