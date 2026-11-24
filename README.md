# cr-desktop-firmware

**CR-Desktop 的韌體更新清單與韌體檔的公開發布位置。** CR-Desktop 連上手錶後，從這裡查有沒有新版、去哪裡下載、下載到的檔案對不對。

這裡只放發布出去的東西：清單、簽章，以及放在 Release 附件的韌體檔。沒有韌體原始碼。

## App 讀的網址

清單與簽章（固定網址，App 連上手錶後讀）：

```
https://raw.githubusercontent.com/Brad-Cat/cr-desktop-firmware/main/firmware.json
https://raw.githubusercontent.com/Brad-Cat/cr-desktop-firmware/main/firmware.json.sig
```

韌體檔的網址由清單給（Release 附件），App 不自己組。清單不存在（404）時，App 當作「沒有更新」。

## 東西放哪裡

| 內容 | 位置 |
|---|---|
| `firmware.json`、`firmware.json.sig` | git 追蹤的檔案（要有版本歷史） |
| 完整韌體檔、差分檔 | **GitHub Release 的附件**，絕不 commit 進 repo |

## 安全

- `firmware.json` 用 Ed25519 簽章，簽的是檔案的原始位元組。App 內建兩把公鑰，先驗簽章才讀內容；**簽完之後不能再改清單任何一個字元**，改了要重新簽。
- 每份清單有發布序號。App 記得看過的最大序號，序號比較小的清單一律不用，所以撤回一個版本要發一份序號更大的新清單，不能把舊清單放回來。
- 私鑰不在這個 repo，也不在任何 repo。
- 下載的韌體檔，App 會再核對大小、SHA-256，並檢查檔案本身是不是給這支手錶的平台、品牌與版本。

## 怎麼發布

步驟在 CR-Desktop repo 的 `docs/phase2/release-howto.md`（`tools/fw_release.py` 的 build、table、sign、commands）。**先上傳 Release 附件，再推清單**：清單一公開，App 就會開始下載附件。

## 已知限制：Windows

清單是 Mac 版與 Windows 版共用的。到 2026-11-24 為止，韌體更新只在 **Mac** 上用真錶驗收過，CR-Desktop 第一次公開發行也只出 Mac 版。

**Windows 版發行之前，一定要先做下面其中一件：**

1. 在 Windows 上驗收韌體更新：CR4 家族（EOC04）走 `cr4-oad-acceptance.md` 第 2、3 項，CR5 系列（EOC05）走 `cr5-diff-acceptance.md` 的差分與 Windows 打包版。
2. 在 App 端讓 Windows 不提供這些平台的更新。CR1／CR4P（EOC01、EOC4P）在 Windows 上已經不提供更新。

細節記在 CR-Desktop repo 的 `docs/phase2/followups.md`「第一份正式韌體清單」。
