# Kikitori（聞き取り）

Windows で会議を録音して、文字起こし・要約まで行うアプリです。録音と文字起こしは PC の中で行います。

このリポジトリはインストーラーの配布専用です（ソースコードは置いていません）。

## 動く環境
- Windows 10 / 11（64 ビット）
- GPU のドライバー（NVIDIA・AMD・Intel のどれか。文字起こしを GPU で動かします）
- GPU のメモリ 4GB 以上が目安（文字起こしと検索に使います。要約を Ollama で行う場合は、別にモデルに応じたメモリが必要です）
- 空き容量 約 2GB（文字起こしのモデルなど）
- 初回の準備のときだけ、インターネット

## 入れ方
1. [Releases](../../releases/latest) から `Kikitori_<版>_x64-setup.exe` をダウンロードして開きます
2. 「Windows によって PC が保護されました」と出たら、［詳細情報］→［実行］で進めます（コード署名をしていないため出ます）
3. 起動したら保存先を選びます。続けて「準備」画面が出るので、［まとめて入れる］を押します
   - 文字起こしの道具（speech.cpp）とモデル（約 1.3GB）を、それぞれの配布元から直接ダウンロードし、中身を確かめてから置きます
4. 要約には Claude Code を使います（入っていなければ、準備画面の［Claude Code の入れ方を開く］から）。使わない場合は、設定で別の AI や「要約しない」を選べます（下の「要約について」を参照）

## 要約について（費用）
- **録音・文字起こし・検索は無料**です。PC の中だけで動きます
- **要約には AI を使います。** 既定の Claude Code は、Claude の有料プランか API の従量課金が必要です（[料金](https://claude.com/pricing)）
- 私の環境では試していませんが、設定で、ほかの AI（各社の CLI や API）も選べるようになっています。料金はそれぞれの AI のプランによります
- **無料で使いたい場合**は次のどちらかを選べます
  - [Ollama](https://ollama.com)（PC の中で動く AI）: 必要な GPU のメモリは、選ぶモデルによって変わります。小さいモデルほど軽く動きますが、要約の質は下がります。一般に、Claude などの有料 AI より質は落ちます
  - 「要約しない」: 文字起こしまで行います

## ダウンロードしたファイルを確かめる（任意）
PowerShell で次を実行し、Release の説明に書いてある SHA-256 と同じか確かめられます。

```powershell
Get-FileHash .\Kikitori_<版>_x64-setup.exe -Algorithm SHA256
```

## 更新
新しい版が出ると、アプリのホーム画面に「新しい版があります［更新する］」と出ます。更新ファイルは署名してあり、署名が合わないものは入りません。

## 使っている道具とモデル
次のものは Kikitori に含めていません。「準備」画面で、それぞれの配布元から直接ダウンロードします。

| 用途 | 名前 | 作者 | ライセンス |
|---|---|---|---|
| 文字起こし・話者の聞き分け | [speech.cpp](https://github.com/nyosegawa/speech.cpp) | Sakasegawa | MIT |
| 文字起こしのモデル | [parakeet-tdt_ctc-0.6b-ja](https://huggingface.co/nvidia/parakeet-tdt_ctc-0.6b-ja)（[GGUF 版](https://huggingface.co/sakasegawa/parakeet-tdt_ctc-0.6b-ja-GGUF)） | NVIDIA | CC-BY-4.0 |
| 話者の聞き分けのモデル | [Nemotron-3-Diarization](https://huggingface.co/nvidia/Nemotron-3-Diarization)（[GGUF 版](https://huggingface.co/sakasegawa/Nemotron-3-Diarization-GGUF)） | NVIDIA | OpenMDW-1.1 |
| 検索 | [llama.cpp](https://github.com/ggml-org/llama.cpp) | ggml-org | MIT |
| 検索のモデル | [ruri-v3-310m](https://huggingface.co/cl-nagoya/ruri-v3-310m)（[GGUF 版](https://huggingface.co/Targoyle/ruri-v3-310m-GGUF)） | 名古屋大学 | Apache-2.0 |
| 音声の圧縮（任意） | [FFmpeg](https://ffmpeg.org)（[LGPL 版](https://github.com/BtbN/FFmpeg-Builds)） | FFmpeg 開発者 | LGPL |

## プライバシー
- 録音した音声と文字起こしは、選んだ保存先（PC の中）にだけ置きます
- 要約のときは、選んだ AI（既定は Claude Code）に文字起こしが送られます。PC の中だけで完結させたい場合は、設定で Ollama を選べます
- 会議を録音するときは、参加している人に伝えて、同意を取ってください
