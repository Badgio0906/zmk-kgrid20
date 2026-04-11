

# ⌨️ kgrid20

## 🛠️ Current Configuration 
20キー（10列×4行）分割レイアウトの完全復旧版。

## 📂 Branch Strategy
- **main**: 安定版


## ✨ Special Features
- **Split Layout**: `k_grid.json` の調整により、中央に1列分のスペースを配置。


## 🔨 Hardware Mod
- **Weight**: 内部に鉛板を追加。
- **Damping**: ポロンシートによる消音加工。
- **Keycaps**: Chocfox LAK (Low Profile)

---

## 1. 現在の物理的な状態（正常動作中）

| 部位 | 役割 | 接続先 / 設定 |
|:---:|:---:|:---|
| **左手** | 親機 (Central) | `ZMK_SPLIT_ROLE_CENTRAL=y` / PC接続担当 |
| **右手** | 子機 (Peripheral) | `ZMK_SPLIT_ROLE_CENTRAL=n` / 左手接続担当 |

- **ペアリング状態**: 左右間の同期 ✅ / PCとのBluetooth接続 ✅
- **マトリクス**: 左右それぞれ5列×4行（計20キー）すべて反応確認済み。

---

## 2. GitHub 上の設定ファイル構成

### `config/boards/shields/k_grid/Kconfig.defconfig`
左手を「親」、右手を「子」として明確に役割分担させる設定を導入。これによりBluetoothの認識率が大幅に向上しました。

## 3. 今後のキーマップ変更の流れ

基本的に**左手（親機）の設定を書き換えるだけ**で、左右両方のキー配置を更新可能です。

1. GitHub 上の `.keymap` ファイルを編集（全40スロット分）
2. Commit & Push → 自動ビルド
3. `k_grid_left.uf2` をダウンロードして左手に書き込む
4. 完了！（右手は触らなくても配置が変わります）

## 4. ポインティングデバイス（マウス機能）の有効化

このキーボードでは、キー入力に加えてマウス操作を行うために  
ZMKのポインティング機能を有効化しています。

```conf
CONFIG_ZMK_POINTING=y
```

詳細なキー割り当ては `kgrid20.keymap` を参照してください。

---

## 5. メンテナンス・トラブルシューティング

### ✅ 通常のキーマップ変更
- `.keymap` を修正してPush。
- 生成された **`left.uf2` だけを左手に書き込む**（右手の書き換えは不要）。

### 🔄 接続が不安定になった場合（リセット儀式）
1. PC側のBluetooth設定から「kgrid20」を一度削除。
2. 左手・右手両方に `settings_reset.uf2` を書き込む。
3. 左手に `left.uf2`、右手に `right.uf2` をそれぞれ書き込む。
4. 左右を至近距離に置いて同時起動し、ペアリングを待つ。

---

## MIT License

This project is licensed under the MIT License.
