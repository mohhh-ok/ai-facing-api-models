# ai-facing-api-models (archived)

> **Moved to https://github.com/mohhh-ok/image-facing-api**
>
> 本体リポを public 化したのに伴い、モデル onnx の配信もそちらの Release に統合しました。
> 新しい取得 URL は次の通りです（sha256 は同一）:
>
> ```
> https://github.com/mohhh-ok/image-facing-api/releases/download/v1/dinov2_vits14.onnx
> ```
>
> このリポジトリは履歴保全のため残してありますが、archive 済みで更新されません。

---

## 旧説明（参考）

[image-facing-api](https://github.com/mohhh-ok/image-facing-api)（旧 `ai-facing-api`）が
**ビルド時に取得する埋め込みモデル**の配信用リポジトリでした。

本体リポが private だった頃、モデル（非機密）だけをこの public repo の **Release アセット**として置き、
本体の Dockerfile がビルド時に匿名 `curl` で取得する構成にしていました。本体が public になったため
分離しておく理由が無くなり、配信を本体側に統合しました。

## アセット（履歴）

| 項目 | 値 |
|---|---|
| ファイル | `dinov2_vits14.onnx`（約 84MB） |
| Release | `v1` |
| 旧取得 URL | `https://github.com/mohhh-ok/ai-facing-api-models/releases/download/v1/dinov2_vits14.onnx` |
| sha256 | `b43bd497e2d9f79722371c3177fb2f92917da84df1db9aece9cdce03abfeea1b` |
| 入力 | `input` `(N, 3, 224, 224)` float32 |
| 出力 | `embedding` = CLS トークン 384 次元 |
| opset | 17 |

## モデルの素性

[DINOv2](https://github.com/facebookresearch/dinov2) ViT-S/14（Meta, Apache-2.0）の派生 ONNX エクスポート。
学習済みの公開基盤モデルそのもので、機密データは含みません。
