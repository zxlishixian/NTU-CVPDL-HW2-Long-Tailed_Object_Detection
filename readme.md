# HW2 — Long-Tailed Object Detection 作業 README

> 這份 README 為 HW2 — Long-Tailed Object Detection 作業的說明文件，由 HW1 的程式架構改編而來，利用yolo11l架構實現對照片中多種物體的識別，並輸出作業要求的提交格式。從零安裝環境、準備資料夾、執行**訓練**與**推理/產出提交檔**。核心程式在 `HW2.ipynb`
---


## 專案結構

請確保資料夾結構大致如圖所示（檔名可不同，但層級需一致, 其中 output 和 runs 文件夾會由程式自動生成，可以不需要預先創建）

```
.
├─ HW2.ipynb
├─ CVPDL_hw2/
│  ├─ train/
│  │  ├─ img0001.png                 # 訓練影像 ( 00000001,00000002...  png...)
│  │  ├─ img0001.txt                 # 訓練文本               
|  |  :
|  |              
│  └─ test/
│     ├─ img0001.png                 # 測試影像（無標註）
|     ：
|             
├─ (runs/   )                    # Ultralytics 的輸出（訓練/推理結果）
|  |
|  ├─ train/
│  │  ├─ hw2/                
│  │  ├─ hw21/                      
|  |  :
|  |              
│  └─ detect/
│     ├─ predict/
|     └─ val/
|
└─ submission.csv
```
---




## 運行cell2-4 將data中的數據集轉換成yolo架構可接受的格式
這個cell的程式會將標準數據集data文件夾中的文件轉化成yolo架構要求的輸出，正確運行後會在與data同層級目錄下產生一個名為data_yolo的文件夾。
dataset.yaml是未對資料分佈問題做處理的配置文件，cell4在cell3的基礎上產生train_upsampled.txt，針對稀有的類別物體圖片讓其被重複訓練，以此改善long tail問題，並以dataset_upsampled.yaml文件配置
```


-- data_yolo/
    |
    ├─dataset.yaml
    ├─dataset_upsampled.yaml
    |
    ├─ images/
    │  ├─ train/                 
    │  ├─ test/       
    |  └─ val/
    |            
    │  
    └─ label/
       ├─ train/       
       └─ val/                
    
```

建議檢查dataset_upsampled.yaml中的配置是否正確再進行下一步
---



## 運行cell 6 利用yolo架構開始訓練

這個cell的程式運行後正式開始利用data_yolo中的數據集開始訓練，會在與ipynb文件同層目錄下產生run/train文件夾後,在裡面產生hw2，hw21，hw22...文件夾（每訓練一個新模型就會產生一個新的hw2文件夾），hw2文件夾裡面有pt後綴即為訓練好的模型參數配置，還有一些其他記錄訓練過程的文件
---


## 運行cell 7 對驗證集開始測試，並在runs/detect下產生val文件夾，記錄驗證結果的指標
---

## 運行cell 8 產生對test預測的結果與submission文件

請將這行的參數改為你實際載入模型的路徑：

ckpt = Path("/content/runs/train/hw210/weights/best.pt")

這一行可以配置你產生的submission文件的名稱與位置：

out_path = "submission29.csv"

---

## 運行cell 9 將讀取/content/runs/detect/predict中可視化的結果，便於直接判斷該次預測表現是否良好

每一次運行cell 8都會產生一個新的predict(1 2 3...)文件請將這一行的"predict"改為對應名稱:

img_dir = vis_dir / "predict"  # Point to the test prediction directory
---