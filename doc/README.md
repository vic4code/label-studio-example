# Verdict NLP

<!-- [Installation](#Installation) -->

<h4 style="text-align: left;">
	<a href=#Installation> Installation </a> |
	<a href=#Quickstart> Quickstart </a> |
	<a href=#Details> Details </a> |
	<a href=#Contact> Contact </a>
</h4>


法院判決書是法律系統中至關重要的文件，其中包含了法官對案件的裁決和法律解釋。然而，判決書通常包含大量的文本信息，對於法律專業人員和研究人員來說，有效提取和分類這些信息是一項具有挑戰性的任務。這個專案旨在應用 Information Extraction 和 Text Classification 技術，自動處理中文法院判決書，以提供更高效的法律文檔分析工具。

<a name="Installation"></a>
<h1>Installation</h1>

## 環境
- python >= 3.８

## 建立 python 環境
- 安裝 miniconda，可參考[官方文件](https://docs.conda.io/projects/miniconda/en/latest/):
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```
- 創建並進入 python 環境:
```
conda create -n <name> python=3.8
conda activate <name>
```

## python package 安装

根據機器硬體設備安裝對應的套件（**擇一**）：

**GPU Version：**
```Shell
pip install -r requirements.txt
```
**CPU Version：**
```Shell
pip install -r requirements-cpu.txt
```
注意：參數檔案需根據您使用的裝置手動更改。例如使用 CPU，則模型參數檔內 `device` 請設定為 `cpu`。

<a name="Quickstart"></a>
<h1>Quickstart</h1>

- 移至專案目錄並設定 `PYTHONPATH`:
```
cd prjt-verdict-nlp
export PYTHONPATH="$PWD/src"
```
## Data Preparation
### Label Studio Data

- Information Extraction
  - 將 Label Studio 所輸出的 JSON 檔案放至 `./data/modeling/information_extraction/label_studio/` 資料夾內，並且命名為 `input_example.json` (或改 dataprep.yaml 參數檔的輸入資料名稱)。

- Text Classification:
  - UTC: 將 Label Studio 所輸出的 JSON 檔案放至 `./data/modeling/text_classification/label_studio/` 資料夾內，並且命名為 `utc_input_example.json` (或改 dataprep.yaml 參數檔的輸入資料名稱)。
  - UIE (Optional): 將 Label Studio 所輸出的 JSON 檔案放至 `./data/modeling/text_classification/label_studio/` 資料夾內，並且命名為 `uie_input_example.json` (或改 dataprep.yaml 參數檔的輸入資料名稱)。


### Inference Data
- 將判決書資料 `infer_example.json` 放至 `./data/inference/`

## Modeling

### Information Extraction

- 將 Label Studio Data 轉成模型輸入格式， dataset 將存於 `./data/modeling/text_classification/model_train`:
```Shell
python -m judge information_extraction dataprep
```
- 模型訓練，模型 checkpoint 將存於 `./models/information_extraction/checkpoint/model_best`:
```Shell
python -m judge information_extraction train
```
- 模型評估，評估結果將存於 `./models/information_extraction/checkpoint/model_best/test_results`:
```Shell
python -m judge information_extraction eval
```

### Text Classifciation

- 將 Label Studio Data 轉成模型輸入格式：
```Shell
python -m judge text_classification utc_dataprep
```

<details>
    <summary>UIE 預處理文本 (Optional): </summary>

- 將 Label Studio Data 轉成模型輸入格式：
```Shell
python -m judge text_classification uie_dataprep
```
- 模型訓練，模型 checkpoint 將存於 `./models/text_classification/uie/checkpoints/model_best`:
```Shell
python -m judge text_classification uie_train
```
- 模型評估，評估結果將存於 `./models/information_extraction/uie/checkpoints/model_best/test_results`:
```Shell
python -m judge text_classification uie_eval
```
- UIE 預處理文本，先將 `dataprep.yaml` 中的 `data_file_or_path_to_inference` 路徑設定為 `./data/modeling/text_classification/model_train/utc/dataset`，結果將存於 `./data/modeling/text_classification/model_train/uie_processed_dataset`:
```Shell
python -m judge information_extraction uie_infer
```

</details>

- 模型訓練，模型 checkpoint 將存於 `./models/text_classification/utc/checkpoints/model_best`:
```Shell
python -m judge text_classification utc_train
```
- 模型評估，評估結果將存於 `./models/text_classification/utc/checkpoints/model_best/test_results`:
```Shell
python -m judge text_classification utc_eval
```

## Inference

### Information Extraction

-  模型推論，結果將存於 `./reports/information_extraction/inference_results`:
```Shell
python -m judge information_extraction infer
```
### Text Classifciation
-  模型推論，結果將存於 `./reports/text_classification/inference_results`:
```Shell
python -m judge text_classification utc_infer
```

## Merge Results

合併兩專案結果需先確認：
- 確認兩者任務都推論成功，且推論資料來源完全相同。
- 在 `merge_config.yaml` 中確認欲合併資料路徑與檔名是否正確（金額擷取任務檔名包含時間，請確認與 .yaml 一致）。

確認後正確後，執行：

```Shell
python -m judge merge
```

結果將存於 `./reports/merge_results` 內。

<a name="Details"></a>
<h1>Details</h1>

詳細的訓練方法與參數細節可以依照各自任務做參考：

- Information Extraction 請參考<a href="./src/judge/information_extraction/README.md"> <u>這裡</u> </a>。
- Text Classification 請參考<a href="./src/judge/text_classification/README.md"> <u>這裡</u> </a>。

<a name="Contact"></a>
<h1>Contact</h1>

如果您有任何疑問、建議或反饋，歡迎隨時聯絡我們的團隊成員！

- 地點：
  - 松仁總行：臺北市信義區松仁路7號3樓
- 聯絡人：
  - 陳奕惟（Victor）：
    - 信箱：[victorchen@cathayholdings.com.tw](victorchen@cathayholdings.com.tw)
    - 電話：(02) 27165090#7507
  - 江柏學（Luka）：
    - 信箱：[lukajiang@cathayholdings.com.tw](lukajiang@cathayholdings.com.tw)
    - 電話：(02) 27165090#7019

