## 樹莓派相機模組
## 硬體連接
若要測試相機，您需要連接 HDMI 顯示器或 DIS 顯示器進行預覽。 DSI
介面（顯示器）和 CSI 介面（相機）的介面外觀相同，連接相機時請注意。 CSI 介面位於音訊插孔和 HDMI 連接埠之間。 Pi Zero 系列的 CSI 介面位於電源介面旁。如果您使用計算模組，請檢查載板的實際位置。

連接到 Raspberry Pi 5
將 FPC 線纜的金屬面朝向有線網路端口，然後連接到 CSI 連接埠。 Pi 5 有兩個 CSI 連接埠；任一連接埠皆可連線。

##關於模型
感光晶片型號	支援的 Raspberry Pi 板型號	支援的驅動程式類型
OV5647	所有 Raspberry Pi 開發板	libcamera/Raspicam
OV9281	所有 Raspberry Pi 開發板	庫相機
IMX219（官方樹莓派）	所有 Raspberry Pi 開發板	libcamera/Raspicam
IMX219（第三方）	Raspberry Pi 計算模組	庫相機
IMX290/IMX327	所有 Raspberry Pi 開發板	庫相機
IMX378	所有 Raspberry Pi 開發板	庫相機
IMX477（官方樹莓派）	所有 Raspberry Pi 開發板	libcamera/Raspicam
IMX477（第三方）	Raspberry Pi 計算模組	庫相機
IMX519	所有 Raspberry Pi 開發板	libcamera（需要驅動程式）
IMX708（樹莓派相機模組 3）	所有 Raspberry Pi 開發板	庫相機
IMX296（Raspberry Pi 全球攝影機）	所有 Raspberry Pi 開發板	庫相機
