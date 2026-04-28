# ⌨️ kgrid20改

> Zephyr 4.1対応・ZMK Studio対応版

---

## 🛠️ Current Configuration

20キー（10列×4行）分割レイアウト。Zephyr 4.1対応・ZMK Studio対応版。

---

## 📂 Branch Strategy

| ブランチ | 説明 |
|:---:|:---|
| **main** | 安定版 |

---

## ✨ Special Features

| 機能 | 説明 |
|:---|:---|
| **Split Layout** | `k_grid.json` の調整により、中央に1列分のスペースを配置 |
| **ZMK Studio対応** | USBケーブル接続でキーマップをリアルタイム変更可能 |
| **Zephyr 4.1対応** | 最新のZMKファームウェアに対応 |

---

## 🔨 Hardware Mod

| 項目 | 内容 |
|:---|:---|
| **Weight** | 内部に鉛板を追加 |
| **Damping** | ポロンシートによる消音加工 |
| **Keycaps** | Chocfox LAK (Low Profile) |

---

## 1. 現在の物理的な状態（正常動作中）

| 部位 | 役割 | 接続先 / 設定 |
|:---:|:---:|:---|
| **左手** | 親機 (Central) | `ZMK_SPLIT_ROLE_CENTRAL=y` / PC接続担当 |
| **右手** | 子機 (Peripheral) | `ZMK_SPLIT_ROLE_CENTRAL=n` / 左手接続担当 |

- **ペアリング状態**: 左右間の同期 ✅ / PCとのBluetooth接続 ✅
- **マトリクス**: 左右それぞれ5列×4行（計20キー）すべて反応確認済み

---

## 2. ファームウェアについて

### 🆕 Zephyr 4.1対応

元のkgrid20からZephyr 4.1対応のため以下の変更を加えています。

| 変更点 | 変更前 | 変更後 |
|:---|:---|:---|
| ボード名 | `xiao_ble` | `xiao_ble//zmk` |
| ビルド方式 | 独自ワークフロー | ZMK公式ワークフロー |
| レイアウト定義 | `zmk,matrix-transform` | `zmk,physical-layout` |
| 追加ファイル | なし | `k_grid-layouts.dtsi` |

### 🆕 ZMK Studio対応

USBケーブルでPCと接続した状態で [ZMK Studio](https://zmk.studio/) を使うことでキーマップをリアルタイムに変更できます。**ファームウェアの書き直しは不要です。**

```conf
CONFIG_ZMK_STUDIO=y
CONFIG_ZMK_STUDIO_LOCKING=n
```

`build.yaml` にも以下を追加しています。

```yaml
snippet: studio-rpc-usb-uart
```

> ⚠️ **注意**: ZMK Studioでキーマップを変更した後にファームウェアを書き直すと、ZMK Studioでの変更が失われます。元の設定に戻したい場合はZMK Studio内の「Restore Stock Settings」から行ってください。

---

## 3. GitHub上の設定ファイル構成

```
config/
├── boards/shields/k_grid/
│   ├── Kconfig.defconfig       # 左右の役割分担設定
│   ├── Kconfig.shield
│   ├── k_grid.dtsi             # 物理レイアウト参照に変更
│   ├── k_grid-layouts.dtsi     # 🆕 ZMK Studio用物理レイアウト定義
│   ├── k_grid_left.overlay
│   └── k_grid_right.overlay
├── k_grid.conf                 # ZMK Studio設定を追加
├── k_grid_left.conf
├── k_grid_right.conf
├── k_grid.keymap
└── west.yml
```

### `k_grid-layouts.dtsi`（新規追加）
ZMK Studio対応のための物理レイアウト定義ファイルです。各キーの物理的な位置情報を定義しています。

### `Kconfig.defconfig`
左手を「親」、右手を「子」として明確に役割分担させる設定を導入。これによりBluetoothの認識率が大幅に向上しました。

---

## 4. キーマップ変更の流れ

### ✅ ZMK Studioを使う場合（推奨）

```
USBケーブルで左手をPCに接続
　　↓
Google ChromeでZMK Studioを開く
　　↓
キーマップを変更して保存
　　↓
完了！（ファームウェアの書き直し不要）
```

### ✅ ファイルを直接編集する場合

```
GitHub上の .keymap ファイルを編集
　　↓
Commit & Push → 自動ビルド
　　↓
k_grid_left.uf2 と k_grid_right.uf2 をダウンロード
　　↓
左右それぞれに書き込む
```

---

## 5. ポインティングデバイス（マウス機能）の有効化

このキーボードでは、キー入力に加えてマウス操作を行うためにZMKのポインティング機能を有効化しています。

```conf
CONFIG_ZMK_POINTING=y
```

詳細なキー割り当ては `k_grid.keymap` を参照してください。

---

## 6. メンテナンス・トラブルシューティング

### ✅ ZMK Studioでキーマップを変更する場合

- USBケーブルで左手を接続してZMK Studioを使う
- ファームウェアの書き直し不要

### ✅ ファームウェアを書き直す場合

- `.keymap` を修正してPush
- 生成された `k_grid_left.uf2` と `k_grid_right.uf2` を左右それぞれに書き込む

### 🔄 接続が不安定になった場合（リセット手順）

1. PC側のBluetooth設定から「kgrid20」を一度削除
2. 左手・右手両方に `settings_reset.uf2` を書き込む
3. 左手に `k_grid_left.uf2`、右手に `k_grid_right.uf2` をそれぞれ書き込む
4. 左右を至近距離に置いて同時起動し、ペアリングを待つ

---

## MIT License

This project is licensed under the MIT License.
