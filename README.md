# releases
firmware and system images for OTA updates


# Firmware Manifest 說明

此專案包含一個 `manifest.json` 檔案，用來描述不同硬體平台的 **韌體更新資訊**。  
設備在進行 OTA（Over-The-Air）更新時，可以透過此檔案來判斷是否有新版本需要下載與更新。


## 檔案結構

### 範例 `PRK/manifest.json`
```json
{
  "manifest_version": "1.0",
  "files": [
    {
      "id": "PRK|ESP32|firmware",
      "model_name": "PRK",
      "hardware_name": "ESP32",
      "file_type": "firmware",
      "file_version": "0.1.0",
      "file_url": "",
      "sha256": "",
      "release_date": "2025-09-25"
    },
    {
      "id": "PRK|MSPM0G3507|firmware",
      "model_name": "PRK",
      "hardware_name": "MSPM0G3507",
      "file_type": "firmware",
      "file_version": "0.1.0",
      "file_url": "",
      "sha256": "",
      "release_date": "2025-09-25"
    }
  ]
}
````

## 欄位說明

| 欄位名稱                  | 說明                                            |
| ------------------------ | ----------------------------------------------- |
| **manifest_version**     | Manifest 檔案版本，方便後續格式更新時做版本控管    |
| **files**                | 韌體檔案清單，每個物件描述一個對應的韌體版本        |

### `files` 內部欄位

| 欄位名稱            | 說明                                                                             |
| ------------------ | -------------------------------------------------------------------------------- |
| **id**             | 檔案唯一 ID（組合：`(model_name)\|(hardware_name)\|(file_type)`                    |
| **model_name**     | 產品/裝置型號（例如：`PRK`）                                                       |
| **hardware_name**  | 硬體平台名稱（例如：`ESP32`、`MSPM0G3507`）                                        |
| **file_type**      | 檔案類型（目前主要是 `firmware`，未來可擴充如 `image`、`apk`）                      |
| **file_version**   | 韌體版本號，建議採用 [Semantic Versioning](https://semver.org/lang/zh-TW/) 格式    |
| **file_url**       | 韌體檔案下載位置（HTTP/HTTPS URL）                                                 |
| **sha256**         | 檔案的 SHA256 雜湊值，用於完整性驗證，防止檔案被竄改                                 |
| **release_date**   | 韌體發布日期（格式：YYYY-MM-DD），跟韌體上傳日期不同                                  |


## 使用方式

1. 設備在啟動或定期檢查更新時，先下載 `manifest.json`。
2. 比對設備自身的 `model_name` 與 `hardware_name`，找到對應的項目。
3. 檢查 `file_version` 是否較新。
4. 下載 `file_url` 指向的韌體檔案。
5. 驗證下載後的檔案 SHA256 是否與 `sha256` 欄位一致。
6. 通過驗證後，執行韌體更新。


## 範例應用情境

* **ESP32 裝置** 會讀取 `PRK|ESP32|firmware` 條目 → 取得最新韌體 URL 與版本。
* **MSPM0G3507 裝置** 會讀取 `PRK|MSPM0G3507|firmware` 條目 → 驗證並更新。


## 未來擴充

* 支援多種類型檔案（如 bootloader、image、配置檔）。
* 增加 **簽名檔 (signature)** 欄位，強化安全性。
* 引入 **最低支援版本 (min_supported_version)**，確保裝置不會跨版本更新失敗。


