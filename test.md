# 一、主題： GoogLeNet vs  SqueezeNet

  <details>
  <summary>GoogLeNet 的主要創新點包括：</summary>

  * ` Inception 模組（Inception Module）：`這是該架構的核心組成單元。它在同一層中并行執行多種不同大小的卷積（1x1、3x3、5x5）和最大池化操作，並將所有結果在通道維度上拼接起來。這使得網絡能夠同時捕捉不同尺度的特徵，從而提高了特徵提取的效率和多樣性。
  * `1x1 卷積（1x1 Convolution）：`在 Inception 模組內部，1x1 卷積被巧妙地用於降維，從而大大減少了計算成本和參數數量，同時增加了網絡的非線性表達能力。
  * `全局平均池化（Global Average Pooling, GAP）：` GoogLeNet 使用全局平均池化層取代了傳統 CNN 架構末端龐大的全連接層。這顯著減少了模型參數，有助於防止過擬合。
  * ` 輔助分類器（Auxiliary Classifiers）：` 為了緩解訓練深度網絡時可能出現的梯度消失問題，GoogLeNet 在網絡中間層引入了兩個輔助分類器。這些分支在訓練期間提供額外的監督信號，並在推斷時被移除。 
</details>

<details>
   <summary>SqueezeNet 的主要創新點包括：</summary>
  
  * SqueezeNet 的核心創新在於其獨特的設計策略和稱為 ` "Fire Module" `  的構建模組。
    <details> 
      <summary> Fire Module（火模組）: 這是 SqueezeNet 的基本單元，包含兩個層次：</summary>
      
    1. `Squeeze Layer（壓縮層）:` 僅使用 1x1 卷積核來減少輸入通道的數量，從而降低模型的計算成本。.
    2. `Expand Layer（擴展層）:` 結合 1x1 和 3x3 卷積核，旨在擴展特徵圖的通道數，同時保持空間資訊。
       
    </details>  
    <details> 
      <summary> 設計策略: 該架構遵循三個關鍵原則來最大化效率： </summary>
        
      1. `用 1x1 卷積取代 3x3 卷積：` 1x1 卷積核的參數量比 3x3 卷積核少九倍。
      2. `減少 3x3 卷積的輸入通道數：` 透過在 3x3 卷積前放置壓縮層來實現。
      3. `延遲下採樣：` 將池化層（下採樣）放置在網路較深層次，以保留更大的激活圖（特徵圖），從而提高分類準確率。
    </details>

  - `無全連接層:`SqueezeNet 捨棄了傳統 CNN 最後常用的全連接層，改用全域平均池化層，進一步大幅減少了模型參數。

</details>

# 二、收集資料：
### 原始資料
利用Google蒐集相關圖片並存在一固定資料夾
<details><summary>展開圖片</summary><img width="865" height="366" alt="image" src="https://github.com/user-attachments/assets/c6297c8a-fb0b-40ee-9ae4-02f18efcbdd6" /></details>

### 格式轉換
用 $\color{red}{\text{Xnview}}$ 的批次轉換將圖片轉成MatLab可以使用的 $\color{red}{\text{jpg}}$ 檔案，並裁切成可使用的大小
<details><summary>展開圖片</summary><img width="865" height="485" alt="image" src="https://github.com/user-attachments/assets/0f64c9e4-5349-45da-9c14-d0ad9b2bc853" /></details>

### 資料重新命名
將圖片統一重新命名，預防出現MatLab不支援的檔案名稱
<details><summary>展開圖片</summary><img width="865" height="469" alt="image" src="https://github.com/user-attachments/assets/7b4729d8-1e78-4495-9510-50008bb3064b" />
<img width="865" height="472" alt="image" src="https://github.com/user-attachments/assets/0c3a5d9d-007c-435c-b254-1ec2cab8874d" /></details>

# 三、建立模型挖掘資料：
### 導入數據
透過對資料進行隨機增強，可以有效增加訓練資料量。資料增強還能使網路對影像資料中的失真具有不變性，所以我使圖片沿著  $\color{red}{\text{x}}$  軸進行隨機反射，在  $\color{red}{\text{[-90,90]}}$  度範圍內進行隨機旋轉，並在  $\color{red}{\text{[1,2]}}$  範圍內進行隨機縮放。
<details><summary>展開圖片</summary><img width="865" height="436" alt="image" src="https://github.com/user-attachments/assets/a0935fbc-24f6-4185-8f7d-51ccd2eb17e9" /></details>

### 數據視覺化
這 $\color{red}{\text{70%}}$ 是用來訓練的資料
<details><summary>展開圖片</summary><img width="865" height="465" alt="image" src="https://github.com/user-attachments/assets/f2d5c250-33c5-442d-8138-275f05f9c729" /></details>

這 $\color{red}{\text{30%}}$ 是用來測試的資料
<details><summary>展開圖片</summary><img width="865" height="464" alt="image" src="https://github.com/user-attachments/assets/e836f707-8352-496c-9ab9-fa9735185de4" /></details>

## GoogLeNet
### 取代輸入層
使輸入層讀入的資料符合我們輸入的資料
<details><summary>展開圖片</summary><img width="865" height="428" alt="image" src="https://github.com/user-attachments/assets/6264b24f-11a8-4647-82b8-c68dd46b8d4d" /></details>

### 取代最後一個可學習層
將  $\color{red}{\text{NumFilters}}$ 變更為新資料中的類別數量 $\color{red}{\text{4}}$ 。 調整學習率，使新層的學習速度比遷移層的學習速度更快，將  $\color{red}{\text{WeightLearnRateFactor}}$  和  $\color{red}{\text{BiasLearnRateFactor}}$ 設定 為  $\color{red}{\text{10}}$ 。
<details><summary>展開圖片</summary><img width="865" height="430" alt="image" src="https://github.com/user-attachments/assets/a7b2d61d-e4cb-4611-8661-62a8ac19e744" /></details>

### 替換輸出層
對於新的輸出層，無需進行設定 OutputSize，訓練時，深度網路設計器會根據資料自動設定該層的輸出類別。
<details><summary>展開圖片</summary><img width="865" height="350" alt="image" src="https://github.com/user-attachments/assets/a4ecf86d-2751-4d6f-8f18-d96d19e67d81" /></details>

### 檢查網路
檢查網路是否已準備好進行訓練
訓練網路
將  $\color{red}{\text{InitialLearnRate}}$ 設定 為  $\color{red}{\text{0.0001}}$ ，   $\color{red}{\text{ValidationFrequency}}$ 設定為 $\color{red}{\text{4}}$ ，   $\color{red}{\text{MaxEpochs}}$  設定為  $\color{red}{\text{8}}$ 。由於有  $\color{red}{\text{168}}$ 個觀測值，將  $\color{red}{\text{MiniBatchSize}}$ 設為  $\color{red}{\text{42}}$  以便均勻分配訓練數據
<details><summary>展開圖片</summary><img width="865" height="517" alt="image" src="https://github.com/user-attachments/assets/87814ebd-7655-40b4-a56c-67beecd91cf9" /></details>

深度網路設計器可讓您視覺化和監控訓練進度

<details><summary>展開圖片</summary><img width="568" height="633" alt="image" src="https://github.com/user-attachments/assets/5ab8d704-7d37-4620-b398-5d4f2b0549d8" /></details>

## SqueezeNet
### 取代輸入層
使輸入層讀入的資料符合我們輸入的資料
<details><summary>展開圖片</summary><img width="865" height="563" alt="image" src="https://github.com/user-attachments/assets/000979c0-e153-4da9-8d0e-3fa8a05d8fe9" /></details>

### 取代最後一個可學習層
將  $\color{red}{\text{NumFilters}}$ 變更為新資料中的類別數量 $\color{red}{\text{4}}$ 。 調整學習率，使新層的學習速度比遷移層的學習速度更快，將  $\color{red}{\text{WeightLearnRateFactor}}$  和設定 $\color{red}{\text{BiasLearnRateFactor}}$  為  $\color{red}{\text{10}}$ 。
<details><summary>展開圖片</summary><img width="865" height="447" alt="image" src="https://github.com/user-attachments/assets/1b01170a-a476-423a-a685-45c5ea82bc15" /></details>

### 替換輸出層
對於新的輸出層，無需進行設定 OutputSize，訓練時，深度網路設計器會根據資料自動設定該層的輸出類別。
<details><summary>展開圖片</summary><img width="865" height="419" alt="image" src="https://github.com/user-attachments/assets/6a804c6c-5fd8-4aba-8cff-517137c59cf7" /></details>

### 檢查網路
檢查網路是否已準備好進行訓練
<details><summary>展開圖片</summary><img width="865" height="512" alt="image" src="https://github.com/user-attachments/assets/337a905b-73f7-457c-8d42-77794aaa5e54" /></details>

### 訓練網路
將  $\color{red}{\text{InitialLearnRate}}$ 設定 為  $\color{red}{\text{0.0001}}$ ，  $\color{red}{\text{ValidationFrequency}}$ 設定為 $\color{red}{\text{4}}$ ，   $\color{red}{\text{MaxEpochs}}$  設定為  $\color{red}{\text{8}}$ 。由於有  $\color{red}{\text{168}}$ 個觀測值，將  $\color{red}{\text{MiniBatchSize}}$ 設為   $\color{red}{\text{42}}$ 以便均勻分配訓練數據

<details><summary>展開圖片</summary><img width="568" height="633" alt="image" src="https://github.com/user-attachments/assets/23ba4fc1-245e-429b-949c-4379f0e29634" /></details>

深度網路設計器可讓您視覺化和監控訓練進度
<details><summary>展開圖片</summary><img width="865" height="578" alt="image" src="https://github.com/user-attachments/assets/e9afb172-f580-4a53-a5d9-7353950bfcf3" /></details>

# 四、實驗結果
|GoogLeNet| SqueezeNet|
| :--- | :--- |
|<details open><summary>展開圖片</summary><img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/cec14697-bd8a-467d-86b6-0607e106b2e9" /></details>|<details open><summary>展開圖片</summary><img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/fa08a0b7-29fb-4aed-bd74-5309632f838d" /></details>|
|<details open><summary>展開圖片</summary><img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/ad46719e-a95f-4890-a780-5c78ba0f7163" /></details>|<details open><summary>展開圖片</summary><img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/79fc44e2-8541-4c06-b0a1-ba5897933b5b" /></details>|
|<details open><summary>展開圖片</summary><img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/4543a159-7a56-48d1-9542-f032232f9932" /></details>|<details open><summary>展開圖片</summary><img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/6147e002-af11-46b2-80a6-cb9a3a6e6b38" /></details>|

# 五、實驗結果討論
 找圖片時，要找的物品要偏向圖片中間，不然圖片剪切過後物品會變的不完整 ，無法在訓練中使用。
訊聯時最好多訓練幾次直到成功率比較高的時候再存下來用，不然拿去測試時出現的配對程度都比較低，有些時候還會配對錯。就結果而言，SqueezeNet的效果比較好，它的訓練結果和便是時的匹配度都比GoogLeNet高。
# 六、心得感想
經過這些課程我學到了很多關於深度學習的知識，例如：深度學習一般會用70%的資料來訓練，而另外30%用來測試、卷積層是讓機器判讀照片時，用一些過濾器去看照片中的某些特徵，然後把這張圖片各處的這個特徵的強度記錄在它自己的記分板上、池化層是在每一個選區裡選出該池最大的數字，當作代表這個區域某個特徵的強度，加上最後實際製作一個深度學習的辨別線統，使我對深度學習的運行方式以及其專有名詞更加了解。
