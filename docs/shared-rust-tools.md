# 共用治理關聯檢查

治理入口保留本專案的簽核、合成資料及 Markdown 來源引用規則，將六組
CSV／JSON 參照關係交給私人 [kairos-rust-tools](https://github.com/JasonLn0711/kairos-rust-tools)。
[設定](../governance-relations.json)持有表格、欄位、fixture 路徑欄位與關聯；
[來源版本](../kairos-tools-source.json)固定完整 SHA。

依[跨電腦安裝](https://github.com/JasonLn0711/kairos-rust-tools/blob/main/docs/installation.md)
取得有權讀取的來源、checkout 固定 SHA 並執行 scripts/install.sh 後，執行：

```sh
python3 scripts/check_governance_registries.py
```

可用 WORKSPACE_VERIFY_BIN 指定獨立安裝版本的完整路徑。缺少工具或回報格式
無效時退出 2，不退回另一份 Python 關聯實作。關聯／領域錯誤退出 1，全部
通過退出 0。此命令是本機治理檢查；沒有更改應用 API、Render/Vercel 流程或
取得私人來源的 CI 權限。只 pull 使用端不會自動安裝 Rust 工具。

一般關聯錯誤使用穩定 rule ID、不回印來源值；結構錯誤會先停止，避免領域
程式繼續解析無效輸入。空表、重複表頭、欄位數不一致、非字串引用陣列及
越界 fixture 路徑的失敗政策，由[共用契約](https://github.com/JasonLn0711/kairos-rust-tools/blob/main/docs/csv-relations.md)
明列。缺少 question_ids/source_ids 仍可視為空陣列；null 不算缺少欄位。

2026-11-08 的 18 組合成案例對照兩份原治理程式，完整 adapter 均維持預期
接受／拒絕，包含四項領域違規。缺少 binary 另確認退出 2。現有資料回報
sources=43、questions=26、api_questions=14、flows=4。這不代表臨床內容或
正式服務已驗收；完整測試工具與後續 v0 整合由
[整合契約](https://github.com/JasonLn0711/kairos-rust-tools/blob/main/docs/triage-governance-consolidation.md)
及[使用端總表](https://github.com/JasonLn0711/kairos-rust-tools/blob/main/docs/integrations.md)接續。
升級／回復一併核對 pin、設定與合成測試，保留領域規則及原始資料。

## 完整流程量測

2026-11-08 同一份合併後資料、相同 Python 載入方式，3 組暖機及 30 組交替
配對量測：舊流程中位數 36.712 ms、新流程 44.628 ms；配對中位數約慢
8.006 ms，95% bootstrap 區間為慢 7.798–8.185 ms。這次採用的收益是六組
關聯規則共用同一實作、可宣告設定與明確失敗處理；沒有整體加速證據。
仍保留 Python 領域解析並新增子程序，成本必須納入後續評估。一次暖快取
量測不能推論能源、維護工時或 LLM token 節省，原始樣本留工具庫 .local。
