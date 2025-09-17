## 樹莓派相機模組
## 硬體連接
若要測試相機，您需要連接 HDMI 顯示器或 DIS 顯示器進行預覽。 DSI
介面（顯示器）和 CSI 介面（相機）的介面外觀相同，連接相機時請注意。 CSI 介面位於音訊插孔和 HDMI 連接埠之間。 Pi Zero 系列的 CSI 介面位於電源介面旁。如果您使用計算模組，請檢查載板的實際位置。

連接到 Raspberry Pi 5
將 FPC 線纜的金屬面朝向有線網路端口，然後連接到 CSI 連接埠。 Pi 5 有兩個 CSI 連接埠；任一連接埠皆可連線。

##關於模型
|感光晶片型號                     |支援的 Raspberry Pi 板型號 |支援的驅動程式類型        |
|-------------------------------|-------------------------|-----------------------|
|OV5647	                        |所有 Raspberry Pi 開發板	  |libcamera/Raspicam     |
|OV9281	                        |所有 Raspberry Pi 開發板	  |庫相機                  |
|IMX219（官方樹莓派）	            |所有 Raspberry Pi 開發板	  |libcamera/Raspicam     |
|IMX219（第三方）	                |Raspberry Pi 計算模組	  |庫相機                  |
|IMX290/IMX327	                |所有 Raspberry Pi 開發板	  |庫相機                  |
|IMX378	                        |所有 Raspberry Pi 開發板	  |庫相機                  |
|IMX477（官方樹莓派）             |所有 Raspberry Pi 開發板	  |libcamera/Raspicam     |
|IMX477（第三方）	                |Raspberry Pi 計算模組	  |庫相機                  |
|IMX519	                        |所有 Raspberry Pi 開發板	  |libcamera（需要驅動程式） |
|IMX708（樹莓派相機模組 3）	    |所有 Raspberry Pi 開發板	  |庫相機                  |
|IMX296（Raspberry Pi 全球攝影機）|所有 Raspberry Pi 開發板	  |庫相機                  |

## 測試相機

## 軟體設定

如果您使用的是最新的 Raspberry Pi 相機模組 3 或 Raspberry Pi 全域快門相機，則需要執行以下命令來更新系統（需要網路連線）。
```bibtex
sudo apt-get update -y
sudo apt-get upgrade -y
```
如果只呼叫一個鏡頭，請將相機連接到 CAM1 連接埠。
如果您使用的不是官方的 Raspberry Pi 鏡頭，則需要設定「config.txt」檔案。如果您使用最新的 Bookworm 系統，則需要設定 /boot/firmware/config.txt。

```bibtex
sudo nano /boot/config.txt 
#如果使用 bookworm 系統
sudo nano /boot/firmware/config.txt
```

找到“camera-auto-detect=1”並將其修改為“camera_auto_detect=0”。
在文件最後根據相機型號添加以下設定語句。
|模型	          |設定語句                              |
|---------------|-------------------------------------|
|OV9281	        |dtoverlay=ov9281                     |
|IMX290/IMX327	|dtoverlay=imx290，時脈頻率=37125000    |
|IMX378	        |dtoverlay=imx378                     |
|IMX219	        |dtoverlay=imx219                     |
|IMX477	        |dtoverlay=imx477                     |
|IMX708	        |dtoverlay=imx708                     |

注意：要將IMX290連接到Raspberry Pi 5，需要在指令目錄下新增json文件，操作如下：
```bibtex
sudo wget https://www.waveshare.net/w/upload/7/7a/Imx290.zip 
sudo unzip Imx290.zip 
sudo cp imx290.json /usr/share/libcamera/ipa/rpi/pisp
```
雙眼相機配置

目前CM4載板和Raspberry Pi 5均支援連接兩個相機。
如果要同時連接兩個鏡頭，可以在對應的攝影機配置語句後面加上「cam0」和「cam1」來指定攝影機。
例如imx219連接到cam0接口，ov5647攝影機連接到cam1接口。

```bibtex
dtoverlay=imx219，cam0 
dtoverlay=ov5647，cam1
```
## 測試相機命令

進入樹莓派終端，啟用攝影機預覽：
```bibtex
sudo libcamera-hello -t 0
```

如需關閉預覽窗口，可直接按下「Alt + F4」鍵，或點選「X」關閉。您也可以回到終端機介面，按下「Ctrl + c」結束示範。
注意：如果使用“Camera Module 3”，則自動對焦功能已啟用。
測試雙眼攝像頭

測試雙眼相機時，需要新增「--camera」指定相機，如果不新增此參數，則預設指定「cam0」。
```bibtex
sudo libcamera-hello -t 0 --camera 0 
sudo libcamera-hello -t 0 --camera 1
```
## 前言

查看自己使用的系統是什麼版本，運行sudo cat /etc/os-release查看是否有下面兩個鏡像的信息，然後選擇。

Raspberry Pi OS Bookworm 將相機捕獲應用程式從 libcamera-* 更改為 rpicam-*，目前允許用戶繼續使用舊的 libcamera，但 libcamera 將來會被棄用，因此請盡快使用 rpicam。
如果您使用的是 Raspberry Pi OS Bullseye 系統，請向下捲動頁面以使用本教學的 libcamera-* 部分。

## 遙控相機

在執行最新版本的 Raspberry Pi OS 時，rpicam-apps 已安裝五個基本功能。在這種情況下，官方 Raspberry Pi 相機也會被偵測到並自動啟用。
您可以輸入以下命令來檢查一切是否正常：
```bibtex
rpicam-hello
```

您應該會看到一個攝影機預覽窗口，持續約五秒鐘。
注意：如果您使用的是 Bullseye Raspberry Pi 3 及更早版本，則需要重新啟用 Glamor 以確保 X Windows 硬體加速預覽視窗正常運作。在終端機視窗中輸入指令 sudo raspi-config，然後選擇「進階選項」、「Glamor」和「是」。退出並重新啟動您的 Raspberry Pi。預設情況下，執行 Bullseye 的 Raspberry Pi 3 及更早版本裝置可能未使用正確的顯示驅動程式。請參考 /boot/firmware/config.txt 文件，並確保 dtoverlay=vc4-fkms-v3d 或 dtoverlay=vc4-kms-v3d 目前處於活動狀態。如果您需要更改此設置，請重新啟動。
##調音檔案

Raspberry Pi 的 libcamera 包含針對每種相機模組的調校檔案。文件中的參數會傳遞給演算法和硬件，以產生最佳品質的影像。 libcamera 只能自動確定所使用的影像感測器，而不是整個模組，即使整個模組都會影響「調校」。因此，有時需要覆蓋特定感測器的預設調校檔案。例如
，不含紅外線濾鏡 (NoIR) 版本的感測器需要與標準版本不同的 AWB（白平衡）設置，因此，與 Pi 4 或更早版本的裝置一起使用的 IMX219 NoIR 應按如下方式運作：
```bibtex
rpicam-hello --tuning-file /usr/share/libcamera/ipa/rpi/vc4/imx219_noir.json
```
Raspberry Pi 5 在不同的資料夾中使用不同的調優文件，因此在這裡您將使用：

```bibtex
rpicam-hello --tuning-file /usr/share/libcamera/ipa/rpi/pisp/imx219_noir.json
```

這也意味著使用者可以複製現有的調校檔案並根據自己的喜好進行修改，只要參數 --tuning-file 指向新版本即可。 --
tuning-file 參數與其他命令列選項一樣適用於所有 rpicam-apps。
##rpicam-jpeg

rpicam-jpeg 是一個簡單的靜態影像擷取應用程式。
若要擷取全解析度 JPEG 影像，請使用以下命令。該命令將顯示大約五秒鐘的預覽，然後將全解析度 JPEG 影像擷取到檔案 test.jpg 中。
```bibtex
rpicam-jpeg -o test.jpg
```

-t <duration> 選項用於變更預覽顯示的時長，而 --width 和 --height 選項則用於變更擷取靜態影像的解析度。例如：
```bibtex
rpicam-jpeg -o test.jpg -t 2000 --width 640 --height 480
```

## 曝光控制

所有這些 rpicam-apps 都允許使用者以固定的快門速度和增益運行相機。拍攝一張曝光時間為 20ms、增益為 1.5 倍的影像。此增益將用作感測器內的類比增益，直到達到核心感測器驅動程式允許的最大類比增益。之後，剩餘部分將用作數位增益。

```bibtex
rpicam-jpeg -o test.jpg -t 2000 --shutter 20000 --gain 1.5
```

Raspberry Pi 的 AEC/AGC 演算法支援應用程式定義的曝光補償，允許透過指定的停止次數使影像變暗或變亮。
```bibtex
rpicam-jpeg --ev -0.5 -o darker.jpg 
rpicam-jpeg --ev 0 -o normal.jpg 
rpicam-jpeg --ev 0.5 -o brighter.jpg
```

## 數位增益

數位增益由 ISP 應用，而非感測器。數位增益始終非常接近 1.0，除非：

請求的總增益（透過選項 --gain 或相機調整中的曝光配置檔案）超過了感測器內可用作類比增益的增益。只有所需的額外增益才會用作數位增益。
其中一個顏色增益小於 1（請注意，顏色增益也用作數字增益）。在這種情況下，發布的數位增益將穩定在 1/分鐘（紅色增益，藍色增益）。這意味著其中一個顏色通道（而不是綠色通道）應用了個位數增益。
AEC/AGC 正在改變。當 AEC/AGC 發生變化時，數位增益通常會發生一定程度的變化，以試圖消除任何波動，但很快就會恢復到正常值。

## rpicam-still

它模擬了原始應用程式 raspistill 的許多功能。

```bibtex
rpicam-still -o test.jpg
```

## 編碼器

rpicam-still 允許以多種不同格式儲存檔案。它支援 PNG 和 BMP 編碼。它還允許將檔案儲存為 RGB 或 YUV 像素的二進位轉儲，無需編碼或檔案格式。在後一種情況下，讀取檔案的應用程式必須了解其自身的像素排列。

```bibtex
rpicam-still -e png -o test.png 
rpicam-still -e bmp -o test.bmp 
rpicam-still -e rgb -o test.data 
rpicam-still -e yuv420 -o test.data
```

注意：儲存影像的格式取決於 -e（相當於編碼）選項，並且不會根據輸出檔案名稱自動選擇。

## 原始影像擷取
原始影像是指影像感測器直接產生的影像，未經 ISP（影像訊號處理器）或任何 CPU 核心處理。對於彩色影像感測器，這些影像通常是拜耳格式。請注意，原始影像與我們之前看到的經過處理但未編碼的 RGB 或 YUV 影像有很大不同。
取得原始影像：
```bibtex
rpicam-still --raw --output test.jpg
```
這裡，-r 選項（也稱為 raw）表示捕捉原始影像和 JPEG。實際上，原始影像是生成 JPEG 的原始影像。原始影像以 DNG（Adobe 數位負片）格式儲存，並與許多標準應用程式（例如 draw 或 RawTherapee）相容。原始映像保存為同名但擴展名為 .ng 的文件，因此成為 test.dng。
這些 DNG 檔案包含與影像擷取相關的元數據，包括黑色電平、白平衡資訊以及 ISP 用於產生 JPEG 的色彩矩陣。這使得這些 DNG 檔案將來更方便地與上述某些工具一起進行「手動」原始轉換。使用 exiftool 顯示編碼到 DNG 檔案中的所有元資料：
```
File Name                       : test.dng
Directory                       : .
File Size                       : 24 MB
File Modification Date/Time     : 2021:08:17 16:36:18+01:00
File Access Date/Time           : 2021:08:17 16:36:18+01:00
File Inode Change Date/Time     : 2021:08:17 16:36:18+01:00
File Permissions                : rw-r--r--
File Type                       : DNG
File Type Extension             : dng
MIME Type                       : image/x-adobe-dng
Exif Byte Order                 : Little-endian (Intel, II)
Make                            : Raspberry Pi
Camera Model Name               : /base/soc/i2c0mux/i2c@1/imx477@1a
Orientation                     : Horizontal (normal)
Software                        : rpicam-still
Subfile Type                    : Full-resolution Image
Image Width                     : 4056
Image Height                    : 3040
Bits Per Sample                 : 16
Compression                     : Uncompressed
Photometric Interpretation      : Color Filter Array
Samples Per Pixel               : 1
Planar Configuration            : Chunky
CFA Repeat Pattern Dim          : 2 2
CFA Pattern 2                   : 2 1 1 0
Black Level Repeat Dim          : 2 2
Black Level                     : 256 256 256 256
White Level                     : 4095
DNG Version                     : 1.1.0.0
DNG Backward Version            : 1.0.0.0
Unique Camera Model             : /base/soc/i2c0mux/i2c@1/imx477@1a
Color Matrix 1                  : 0.8545269369 -0.2382823821 -0.09044229197 -0.1890484985 1.063961506 0.1062747385 -0.01334283455 0.1440163847 0.2593136724
As Shot Neutral                 : 0.4754476844 1 0.413686484
Calibration Illuminant 1        : D65
Strip Offsets                   : 0
Strip Byte Counts               : 0
Exposure Time                   : 1/20
ISO                             : 400
CFA Pattern                     : [Blue,Green][Green,Red]
Image Size                      : 4056x3040
Megapixels                      : 12.3
Shutter Speed                   : 1/20
```

我們注意到只有一個校準光源（由 AWB 演算法確定，儘管它始終標記為“D65”），將 ISO 數值除以 100 可得出所使用的模擬增益。
## 超長曝光

為了拍攝長曝光影像，請停用 AEC/AGC 和 AWB，因為這些演算法會強制使用者在收斂時等待很多影格。
禁用它們的方法是提供明確的值。此外，可以使用 immediate 選項跳過拍攝的整個預覽階段。
因此，要執行 100 秒的曝光拍攝，請使用：

```bibtex
rpicam-still -o long_exposure.jpg --shutter 100000000 --gain 1 --awbgains 1,1 --immediate
```

## rpicam-vid

rpicam-vid 可以幫助我們在 Raspberry Pi 裝置上擷取影片。 rpicam-vid 會顯示一個預覽窗口，並將編碼後的位元流寫入指定的輸出。這將產生一個未打包的視訊位元流，該位元流未以任何容器格式（例如 mp4 檔案）打包。

rpicam-vid 使用 H.264 編碼
例如，以下命令將 10 秒的影片寫入名為 test.h264 的檔案中：
```bibtex
rpicam-vid -t 10s -o test.h264
```

您可以使用 VLC 和其他視訊播放器播放結果檔案：

```bibtex
VLC test.h264
```

在 Raspberry Pi 5 上，您可以透過指定輸出檔案的 MP4 檔案副檔名直接輸出為 MP4 容器格式：

```bibtex
rpicam-vid -t 10s -o test.mp4
```

## 編碼器

rpicam-vid 支援動態 JPEG 以及未壓縮和未格式化的 YUV420：

```bibtex
rpicam-vid -t 10000 --codec mjpeg -o test.mjpeg 
rpicam-vid -t 10000 --codec yuv420 -o test.data
```

codec 選項決定輸出格式，而不是輸出檔案的副檔名。 segment
選項將輸出檔案分割成多個段大小的區塊（以毫秒為單位）。透過指定極短的段（1 毫秒），可以方便地將移動的 JPEG 流分解成單一 JPEG 檔案。例如，以下命令將 1 毫秒的段與輸出檔名中的計數器組合，為每個段產生一個新的檔名：

```bibtex
rpicam-vid -t 10000 --codec mjpeg --segment 1 -o test%05d.jpeg
```

## 捕捉高幀率視頻

為了最大限度地減少高幀率（> 60fps）影片的幀丟失，請嘗試以下配置調整：

使用參數--level 4.2將H.264的目標等級設定為4.2
透過將降噪選項設為 cdn_off 來停用軟體顏色降噪處理。
停用 nopreview 的顯示視窗以釋放額外的 CPU 週期。
在 /boot/firmware/config.txt 中設定 force_turbo=1，以確保在視訊擷取期間 CPU 時脈不會受到限制。有關更多信息，請參閱force_turbo文檔。
將ISP輸出解析度參數調整為--width 1280 --height 720或更低即可達到幀率目標。
在 Raspberry Pi 4 上，您可以透過在 /boot/firmware/config.txt 中加入 gpu_freq=550 或更高的頻率來超頻 GPU，從而提升效能。有關詳細信息，請參閱超頻文件。
以下指令示範如何實現1280×720 120fps的影片：

```bibtex
rpicam-vid --level 4.2 --framerate 120 --width 1280 --height 720 --save-pts timestamp.pts -o video.264 -t 10000 --denoise cdn_off -n
```

## Libav 與 picam-vid 的集成

Rpicam-vid 可以使用 ffmpeg/libav 編解碼器後端對音訊和視訊串流進行編碼。您可以將這些流儲存到檔案中，或透過網路進行串流傳輸。
若要啟用 libav 後端，請將 libav 傳遞給 codec 選項：

```bibtex
rpicam-vid --codec libav --libav-format avi --libav-audio --output example.avi
```

## UDP

若要將 Raspberry Pi 用作透過 UDP 傳輸串流視訊的伺服器，請使用下列命令，將 < IP -addr> 佔位符替換為用戶端或多重播放位址的 IP 位址，並將 <port> 佔位符替換為您要用於串流傳輸的連接埠：

```bibtex
rpicam-vid -t 0 --inline -o udp://<ip-addr>:<port>
```

使用 Raspberry Pi 作為客戶端透過 UDP 觀看視訊串流，使用以下命令，將 <port> 佔位符替換為您要串流傳輸的連接埠：

```bibtex
vlc udp://@:<port> :demux=h264
```

或者，在客戶端使用 ffplay 透過以下命令進行串流：

```bibtex
ffplay udp://<ip-addr-of-server>:<port> -fflags nobuffer -flags low_delay -framedrop
```

## TCP

視訊也可以透過 TCP 傳輸。使用 Raspberry Pi 作為伺服器：
```bibtex
rpicam-vid -t 0 --inline --listen -o tcp://0.0.0.0:<port>
```
使用Raspberry Pi作為客戶端透過TCP觀看視訊串流，使用以下命令：

```bibtex
vlc tcp/h264://<ip-addr-of-server>:<port>
```

或者，在客戶端使用以下命令以每秒 30 幀的速度使用 ffplay 流：

```bibtex
ffplay tcp://<ip-addr-of-server>:<port> -vf "setpts=N/30" -fflags nobuffer -flags low_delay -framedrop
```
## RTSP

要使用 VLC 透過 RTSP 傳輸視頻，並使用 Raspberry Pi 作為伺服器，請使用以下命令：
```bibtex
rpicam-vid -t 0 --inline -o - | cvlc stream:///dev/stdin --sout '#rtp{sdp=rtsp://:8554/stream1}' :demux=h264
```

若要使用 Raspberry Pi 作為客戶端查看 RTSP 上的視訊串流，請使用以下命令：
```bibtex
ffplay rtsp://<ip-addr-of-server>:8554/stream1 -vf "setpts=N/30" -fflags nobuffer -flags low_delay -framedrop
```

或在客戶端使用以下命令透過 VLC 進行串流：
```bibtex
vlc rtsp://<ip-addr-of-server>:8554/stream1
```

## rpicam-raw

rpicam-raw 會直接從感應器錄製視頻，並將其作為原始拜耳幀。它不會顯示預覽視窗。若要將兩秒鐘的原始片段錄製到名為 test.raw 的檔案中，請執行以下命令：
```bibtex
rpicam-raw -t 2000 -o test.raw
```

RPICAM-RAW 輸出原始幀，不包含任何格式資訊。該應用程式會將像素格式和圖像大小列印到終端窗口，以幫助用戶解析像素資料。
預設情況下，rpicam-raw 將原始幀輸出到一個可能非常大的檔案中。使用 segments 選項將每個原始幀定向到單獨的文件，並使用 %05d 指令使每個幀的文件名唯一：
```bibtex
rpicam-raw -t 2000 --segment 1 -o test%05d.raw
```

使用快速儲存設備，rpicam-raw 可以以 10fps 的速度將 1,2 百萬像素 HQ 相機的 18MB 幀寫入磁碟。 rpicam-raw 無法將輸出幀格式化為 DNG 檔案；為此，請使用低於 10 的幀速率的 rpicam-still 選項以避免丟幀：

```bibtex
rpicam-raw -t 5000 --width 4056 --height 3040 -o test.raw --framerate 8
```

## rpicam-檢測

注意：Raspberry Pi 作業系統不包含 rpicam-detect。如果您已安裝 TensorFlow Lite，則可以建置 rpicam-detect。執行 cmake 時，請不要忘記傳遞 -DENABLE_TFLITE=1。 rpicam-detect 會顯示一個預覽窗口，並使用 Google MobileNet v1 SSD（單次偵測器）神經網路監控內容，該網路已使用 Coco 資料集訓練，可識別約 80 個類別的物件。 rpicam-detect 可以辨識人、車、貓和許多其他物體。每當 rpicam-detect 偵測到目標物體時，它都會捕捉全解析度 JPEG 影像。然後返回監控預覽模式。有關模型使用的一般信息。例如，當您外出時，您可以照顧您的貓：
```bibtex
rpicam-detect -t 0 -o cat%04d.jpg --lores-width 400 --lores-height 300 --post-process-file object_detect_tf.json --object cat
```

## rpicam參數設定

--help -h 列印所有選項以及每個選項的簡要說明
```bibtex
rpicam-hello-h
```

--version 輸出 libcamera 和 rpicam-apps 的版本字串
```bibtex
rpicam-hello --version
```

範例輸出：

```bibtex
rpicam-apps build: ca559f46a97a 27-09-2021 (14:10:24)
libcamera build: v0.0.0+3058-c29143f7
```

--list-cameras 列出連接到 Raspberry Pi 的攝影機及其可用的感測器模式

```bibtex
rpicam-hello --list-cameras
```

感測器模式的標識符具有以下形式：
```bibtex
S<Bayer order><Bit-depth>_<Optional packing> : <Resolution list>
```
裁切以感測器原生像素（即使在像素合併模式）指定為 (<x>, <y>)/<Width>×<Height>。 (x, y) 指定感測器陣列中寬度 × 高度裁剪視窗的位置。
例如，以下輸出顯示了索引為 0 的 IMX219 感測器和索引為 1 的 IMX477 感測器的資訊：
```
Available cameras
-----------------
0 : imx219 [3280x2464] (/base/soc/i2c0mux/i2c@1/imx219@10)
    Modes: 'SRGGB10_CSI2P' : 640x480 [206.65 fps - (1000, 752)/1280x960 crop]
                             1640x1232 [41.85 fps - (0, 0)/3280x2464 crop]
                             1920x1080 [47.57 fps - (680, 692)/1920x1080 crop]
                             3280x2464 [21.19 fps - (0, 0)/3280x2464 crop]
           'SRGGB8' : 640x480 [206.65 fps - (1000, 752)/1280x960 crop]
                      1640x1232 [41.85 fps - (0, 0)/3280x2464 crop]
                      1920x1080 [47.57 fps - (680, 692)/1920x1080 crop]
                      3280x2464 [21.19 fps - (0, 0)/3280x2464 crop]
1 : imx477 [4056x3040] (/base/soc/i2c0mux/i2c@1/imx477@1a)
    Modes: 'SRGGB10_CSI2P' : 1332x990 [120.05 fps - (696, 528)/2664x1980 crop]
           'SRGGB12_CSI2P' : 2028x1080 [50.03 fps - (0, 440)/4056x2160 crop]
                             2028x1520 [40.01 fps - (0, 0)/4056x3040 crop]
                             4056x3040 [10.00 fps - (0, 0)/4056x3040 crop]
```

--camera 選擇要使用的攝影機。請從可用攝影機清單中指定索引。
```bibtex
rpicam-hello --list-cameras 0 
rpicam-hello --list-cameras 1
```

--config -c 指定一個包含指令參數選項和值的檔案。通常，名為 example_configuration.txt 的檔案以鍵值對的形式指定選項和值，每個選項佔一行。
```bibtex
timeout=99000
verbose=
```
注意：命令列中通常會省略參數前綴“--”。對於缺少值的標誌（例如上例中的 verbose），必須在末尾加上“=”。
然後，您可以執行以下命令來指定 99,000 毫秒的逾時時間和詳細輸出：
```bibtex
rpicam-hello --config example_configuration.txt
```
--time -t，預設延遲5000毫秒
```bibtex
rpicam-hello-t
```
指定應用程式在關閉前運行的時間。這適用於視訊錄製視窗和預覽視窗。捕獲靜態影像時，應用程式會顯示一個預覽窗口，並在輸出捕獲的影像之前等待幾毫秒。
```bibtex
rpicam-hello-t 0
```
--preview 設定桌面或 DRM 預覽視窗的位置（x,y 座標）和大小（w,h 尺寸）。這對從相機請求的影像的解析度或寬高比沒有影響。
以以下逗號分隔的形式傳遞預覽視窗尺寸：x、y、w、h
```bibtex
rpicam-hello --preview 100,100,500,500
```
rpicam-hello --preview 100,100,500,500
```bibtex
rpicam-hello-f
```
--qt-preview 使用 Qt 預覽窗口，比其他選項消耗更多資源，但支援 X 視窗轉送。不相容全螢幕標誌。不接受值。

```bibtex
rpicam-hello --qt-preview
```
--nopreview 使應用程式不顯示預覽視窗。不接受任何值。

```bibtex
rpicam-hello --nopreview
```
|命令	 |描述                                 |
|--------|------------------------------------|
|％框架	 |影格序號                             |
|%幀率	 |瞬間幀率                             |
|%exp	 |拍攝影像的快門速度（以毫秒為單位）       |
|%ag	 |感光晶片控制影像模擬增益               |
|%dg	 |影像訊號增益由ISP控制                 |
|%rg	 |每個像素點紅色分量的增益               |
|%體重	 |每個像素點藍色分量的增益               |
|％重點	 |影像的角點度量，數值越大，影像越清晰     |
|%lp     |目前鏡頭的屈光度（距離以 1/公尺為單位）  |
|%afstate|自動對焦狀態（空閒、掃描、對焦、失敗）   |


