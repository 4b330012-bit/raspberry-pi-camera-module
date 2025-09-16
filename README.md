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
