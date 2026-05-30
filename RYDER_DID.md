# 第 15 組 Ontology 交接文件

## 已完成項目

第 15 組的 ontology instance layer 已建立於
`ontology/group-ontology.ttl`。

- 定義第 15 組的 ontology URI 與 `g15:` namespace。
- 加入 ontology metadata 與六位組員資料。
- 將 `g15:toyBlockCollectionTask` 宣告為選定的 manipulation task。
- 建立影片中觀察到的三個積木：
  - `g15:block01`：藍色積木
  - `g15:block02`：綠色積木
  - `g15:block03`：紅色積木
- 將 `g15:basket01` 建模為用於收集積木的白色收納箱。
- 加入其他預定義任務所需的 baseline instances：
  `g15:blueCup01`、`g15:pinkCup01`、`g15:knife01`、`g15:fork01` 與
  `g15:plate01`。
- 為任務相關 instances 加入可讀的 labels、comments、object labels 與
  task roles。

## 刻意省略項目

- 尚未加入 pose-frame identifiers，因為目前沒有官方 frame 名稱。
- 尚未將積木上的視覺標記納入建模，因為目前無法確認 marker 類型與 ID。

## 下一步注意事項

下一位組員應實作並驗證 reasoning 與 query workflow，包含：

- 加入或確認作業 PDF 第 11 節描述的 `cap:GraspableObject` OWL inference
  pattern。
- 將 inferred graph 匯出為 `ontology/inferred-results.ttl`。
- 加入必要的 SPARQL query 並儲存執行結果。
- 準備最後要求的 repository 結構，包含
  `ontology/imports/course-affordance.ttl`。

執行 reasoning 前，應先檢查老師提供的 `course-affordance.ttl`。該檔案
最後的 `cap:Basket` 附近疑似有格式錯誤，而且目前未包含必要的
`cap:GraspableObject` axiom。
