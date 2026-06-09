# ai-facing-api-models

[ai-facing-api](https://github.com/mohhh-ok/ai-facing-api) が**ビルド時に取得する埋め込みモデル**の配信用リポジトリ。

本体リポは private なので、モデル（非機密）だけをこの public repo の **Release アセット**として置き、
本体の Dockerfile がビルド時に匿名 `curl` で取得する。これにより:

- 本体の `railway up` に 84MB を毎回乗せない（送信ゼロ）
- private repo にトークンを仕込まない（匿名取得）
- 取得元・sha256 が Dockerfile に記録され、全環境で自動再現

## アセット

| 項目 | 値 |
|---|---|
| ファイル | `dinov2_vits14.onnx`（約 84MB） |
| Release | [`v1`](https://github.com/mohhh-ok/ai-facing-api-models/releases/tag/v1) |
| 取得 URL | `https://github.com/mohhh-ok/ai-facing-api-models/releases/download/v1/dinov2_vits14.onnx` |
| sha256 | `b43bd497e2d9f79722371c3177fb2f92917da84df1db9aece9cdce03abfeea1b` |
| 入力 | `input` `(N, 3, 224, 224)` float32 |
| 出力 | `embedding` = CLS トークン 384 次元 |
| opset | 17 |

## モデルの素性

[DINOv2](https://github.com/facebookresearch/dinov2) ViT-S/14（Meta, Apache-2.0）の派生 ONNX エクスポート。
学習済みの公開基盤モデルそのもので、**機密データは含まない**（サービスの価値である
ラベル付き k-NN データは本体側の `/data` にあり、この onnx には乗っていない）。

## 再現方法

```bash
pip install -r requirements-export.txt   # torch>=2.2
python scripts/export_dinov2_onnx.py --out dinov2_vits14.onnx --opset 17
sha256sum dinov2_vits14.onnx
# => b43bd497e2d9f79722371c3177fb2f92917da84df1db9aece9cdce03abfeea1b
```

`scripts/export_dinov2_onnx.py` は本体リポと同一のもの。torch.hub から
`facebookresearch/dinov2` の `dinov2_vits14` を読み、単一入力 `input` に固定して ONNX 化する。

## 新バージョンを出すとき

1. 上記で onnx を再生成し、新しい sha256 を控える
2. `gh release create v2 dinov2_vits14.onnx --repo mohhh-ok/ai-facing-api-models ...`
3. 本体の `Dockerfile` の URL（`/v1/` → `/v2/`）と sha256 を更新
