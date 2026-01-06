# 一、主題： GoogLeNet vs  SqueezeNet
<details>
  <summary></summary>
 * 
 * 1x1 卷積（1x1 Convolution）：在 Inception 模組內部，1x1 卷積被巧妙地用於降維，從而大大減少了計算成本和參數數量，同時增加了網絡的非線性表達能力。
 * 全局平均池化（Global Average Pooling, GAP）：GoogLeNet 使用全局平均池化層取代了傳統 CNN 架構末端龐大的全連接層。這顯著減少了模型參數，有助於防止過擬合。
 * 輔助分類器（Auxiliary Classifiers）：為了緩解訓練深度網絡時可能出現的梯度消失問題，GoogLeNet 在網絡中間層引入了兩個輔助分類器。這些分支在訓練期間提供額外的監督信號，並在推斷時被移除。 
  </details>
  <details>
  <summary>GoogLeNet 的主要創新點包括：</summary>

  這裡是隱藏的內容，可以包含：
  * Inception 模組（Inception Module）：這是該架構的核心組成單元。它在同一層中并行執行多種不同大小的卷積（1x1、3x3、5x5）和最大池化操作，並將所有結果在通道維度上拼接起來。這使得網絡能夠同時捕捉不同尺度的特徵，從而提高了特徵提取的效率和多樣性。
  * [連結](https://github.com)
  * `程式碼`
</details>
<details><summary>SqueezeNet 的核心創新在於其獨特的設計策略和稱為 "Fire Module" 的構建模組。 </summary>
* Fire Module（火模組）: 這是 SqueezeNet 的基本單元，包含兩個層次：
1. Squeeze Layer（壓縮層）: 僅使用 1x1 卷積核來減少輸入通道的數量，從而降低模型的計算成本。
2. Expand Layer（擴展層）: 結合 1x1 和 3x3 卷積核，旨在擴展特徵圖的通道數，同時保持空間資訊。
* 設計策略: 該架構遵循三個關鍵原則來最大化效率：
1. 用 1x1 卷積取代 3x3 卷積：1x1 卷積核的參數量比 3x3 卷積核少九倍。
2. 減少 3x3 卷積的輸入通道數：透過在 3x3 卷積前放置壓縮層來實現。
3. 延遲下採樣：將池化層（下採樣）放置在網路較深層次，以保留更大的激活圖（特徵圖），從而提高分類準確率。
* 無全連接層: SqueezeNet 捨棄了傳統 CNN 最後常用的全連接層，改用全域平均池化層，進一步大幅減少了模型參數。
</details>
