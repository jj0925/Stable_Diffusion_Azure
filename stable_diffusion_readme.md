# 🧐 Stable Diffusion Docker - Azure 雲端部署專案

本專案是基於 [Stable Diffusion WebUI Docker](https://github.com/AbdBarho/stable-diffusion-webui-docker) 進行改造與優化，目的是能夠更彈性地在不同設備與雲端平台（如 Azure 虛擬機）上部署，無論有沒有 GPU 都能運行！

---

## 🚀 專案特色

- 🐳 基於 Docker 架構，支援一鍵部署
- ☁️ 支援 Azure 雲端部署（CPU / GPU 都 OK）
- 📆 整合多種自定義模型（可替換路徑 / 模型格式）
- 🖼️ 可選用 AUTOMATIC1111 WebUI 或 ComfyUI 作為前端介面
- 🧱 對新手友善的分步教學：從建機器、安裝驅動到啟動 UI

---

## 📚 使用教學總覽

以下將一步步引導你從零開始，在 Azure 上部署完整的 Stable Diffusion Web UI 環境，支援 CPU 或 GPU 兩種部署方式。

---

### ① 建立 Azure 虛擬機、選擇 GPU 機型

#### ✅ 登入 Azure 並進入「虛擬機」

1. 前往 [https://portal.azure.com](https://portal.azure.com)
2. 點選左側「虛擬機 (Virtual Machines)」
3. 點「建立」 → 「虛擬機」

#### ✅ 選擇合適的微稱與大小

| 類型  | 建議機型              | 描述                            |
| --- | ----------------- | ----------------------------- |
| GPU | `Standard_NC6`    | 有 NVIDIA Tesla K80 GPU，適合圖形處理 |
| CPU | `Standard_D2s_v3` | 僅 CPU，適合低需求測試                 |

4. 微稱：Ubuntu Server 20.04 LTS
5. 大小：點「變更大小」搜尋並選擇機型
6. 驗證方式：建議用 SSH 金鑰或設定密碼
7. 點「檢閱 + 建立」

---

### ② 安裝 NVIDIA 驅動 & Docker

#### 登入 VM

```bash
ssh <username>@<your-public-ip>
```

#### 安裝 NVIDIA 驅動 (GPU 用戶)

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600

sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /"
sudo apt update
sudo apt install -y cuda-drivers

sudo reboot
```

#### 確認 GPU 機器裝置

```bash
nvidia-smi
```

---

#### 安裝 Docker & Compose (CPU/GPU 通用)

```bash
curl -fsSL https://get.docker.com | sudo bash
sudo usermod -aG docker $USER
sudo apt install -y docker-compose
```

---

### ③ 部署本專案

```bash
git clone https://github.com/jj0925/Stable_Diffusion_Azure.git
cd Stable_Diffusion_Azure
```

---

### ④ 啟動 Stable Diffusion WebUI

#### ✅ CPU 版

```bash
docker compose -f docker-compose.cpu.yml up -d
```

> CPU 版會停用 GPU 技術，可使 `--no-half` 、 `--precision full` 參數

---

#### ✅ GPU 版

```bash
docker compose -f docker-compose.gpu.yml up -d
```

> 確認 compose 中有 GPU 承諾和 runtime: nvidia 設定

---

### ⑤ 開啟 WebUI

開啟瀏覽器，輸入下列 IP

```
http://<VM-IP>:7860
```

即可使用圖像生成介面囉！

---

## 🔹 進階功能

- 🎨 模型可載入 `/data/models/` 自定義 LoRA / ControlNet
- 🔄 支援 ComfyUI 作為另類 UI
- 🔐 公開部署請加入密碼註冊

---

## 📝 License

MIT License 基於 AbdBarho 版 Stable Diffusion Docker 進行擴充

---

幫助評估：歡迎 star / PR / 問題分享，感謝你看到這裡～

