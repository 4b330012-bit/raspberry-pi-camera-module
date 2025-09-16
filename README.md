## 樹莓派相機模組
## 硬體連接
若要測試相機，您需要連接 HDMI 顯示器或 DIS 顯示器進行預覽。 DSI
介面（顯示器）和 CSI 介面（相機）的介面外觀相同，連接相機時請注意。 CSI 介面位於音訊插孔和 HDMI 連接埠之間。 Pi Zero 系列的 CSI 介面位於電源介面旁。如果您使用計算模組，請檢查載板的實際位置。

連接到 Raspberry Pi 5
將 FPC 線纜的金屬面朝向有線網路端口，然後連接到 CSI 連接埠。 Pi 5 有兩個 CSI 連接埠；任一連接埠皆可連線。

##關於模型
|感光晶片型號                     |支援的 Raspberry Pi 板型號	|支援的驅動程式類型        |
|-------------------------------|-------------------------|-----------------------|
|OV5647	                        |所有 Raspberry Pi 開發板	  |libcamera/Raspicam     |
|OV9281	                        |所有 Raspberry Pi 開發板	  |庫相機                  |
|IMX219（官方樹莓派）	            |所有 Raspberry Pi 開發板	  |libcamera/Raspicam     |
|IMX219（第三方）	                |Raspberry Pi 計算模組	    |庫相機                  |
|IMX290/IMX327	                |所有 Raspberry Pi 開發板	  |庫相機                  |
|IMX378	                        |所有 Raspberry Pi 開發板	  |庫相機                  |
|IMX477（官方樹莓派）             |所有 Raspberry Pi 開發板	  |libcamera/Raspicam     |
|IMX477（第三方）	                |Raspberry Pi 計算模組	    |庫相機                  |
|IMX519	                        |所有 Raspberry Pi 開發板	  |libcamera（需要驅動程式） |
|IMX708（樹莓派相機模組 3）	      |所有 Raspberry Pi 開發板	  |庫相機                  |
|IMX296（Raspberry Pi 全球攝影機）|所有 Raspberry Pi 開發板	  |庫相機                  |

##測試相機

##軟體設定

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
##測試相機命令

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
##前言

查看自己使用的系統是什麼版本，運行sudo cat /etc/os-release查看是否有下面兩個鏡像的信息，然後選擇。

Raspberry Pi OS Bookworm 將相機捕獲應用程式從 libcamera-* 更改為 rpicam-*，目前允許用戶繼續使用舊的 libcamera，但 libcamera 將來會被棄用，因此請盡快使用 rpicam。
如果您使用的是 Raspberry Pi OS Bullseye 系統，請向下捲動頁面以使用本教學的 libcamera-* 部分。

##遙控相機

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

##曝光控制

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

##數位增益

數位增益由 ISP 應用，而非感測器。數位增益始終非常接近 1.0，除非：

請求的總增益（透過選項 --gain 或相機調整中的曝光配置檔案）超過了感測器內可用作類比增益的增益。只有所需的額外增益才會用作數位增益。
其中一個顏色增益小於 1（請注意，顏色增益也用作數字增益）。在這種情況下，發布的數位增益將穩定在 1/分鐘（紅色增益，藍色增益）。這意味著其中一個顏色通道（而不是綠色通道）應用了個位數增益。
AEC/AGC 正在改變。當 AEC/AGC 發生變化時，數位增益通常會發生一定程度的變化，以試圖消除任何波動，但很快就會恢復到正常值。

##rpicam-still

它模擬了原始應用程式 raspistill 的許多功能。

