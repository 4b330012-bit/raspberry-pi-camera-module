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
