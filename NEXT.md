# NEXT: buffer switcher（最初に作る）

開き済みバッファを popup で絞って切り替える。状態は持たない。シリーズの型を固める1本目。

## やること

- `:Tbs` で `getbufinfo({buflisted: 1})` を候補にする
- 候補表示: `bufnr` ではなく `fnamemodify(name, ':.')`（相対パス）
- `name` が空のバッファ（無名・scratch）は除外（実測で `buflisted` にも入るため）
- 確定: `buffer {bufnr}`（未ロードでも `:buffer` が読み込む）
- キー操作・見た目は tff と同じ

## スコープ外

- バッファの削除・整理（`:bdelete` 等）
- ウィンドウ分割・タブ指定
- フラグ表示（`[+]` / `[RO]`）。必要になったら1条件で足せる

## テスト

- 複数ファイルを `edit` → 行数・絞り込み・Enter後の `bufname('%')` を検証
- 無名バッファが候補に出ないことを検証
- 2本目（jump-viewer）着手時に共通コア切り出しを判断（時期尚早ならコピーのまま）

## 共通の約束（tpf/tff と同じ）

- Vim9のみ・依存なし・KISS・状態最小。`plugin/tbs.vim` + `autoload/tbs.vim` の2ファイル
- 骨格は tff のコピーでよい（popup + filter、先頭行=入力欄、`> `マーカー、border:[]、padding:[0,1,0,1]、20件）
- 罠: def引数名とスクリプト変数の衝突不可(E1168)／テストは `--cmd "set rtp+=$PWD"`／`writefile(/dev/stderr)` 禁止
