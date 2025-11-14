# TODO

現在の実装を安定的に保ちながら、段階的な RSMD ロードマップを追跡しています。

## 近期のPR

1. PR1 – 見出し範囲拡張 + シングルパス化
   - 見出し収集の範囲を h1〜h3 に拡張し、それぞれにスラグを割り当てる。
   - 生成された HTML に `id="slug"` を挿入する（例: `<h2 id="...">`）。
   - 現状の「HTML 生成 + 生テキスト再パース」による 2 パス構成を見直し、
     - `pulldown-cmark` のイベントストリーム１回の走査で
       - HTML 生成
       - 見出し収集
       を同時に行うように最適化する（ドキュメントで約束している single-pass 方針との整合性を取る）。

2. PR2 – wasm-bindgen 最小限エクスポート
   - `wasm32-unknown-unknown` をターゲットとする際に既存のクレートに対して `render_wasm` を公開する。
   - `wasm-pack build --target bundler` を成功させる。
   - 基本的なエラー伝搬と戻り値シリアライズ（`RenderResult` 相当）を確認する。

3. PR3 – Node スモークテスト
   - WASM バンドルを読み込み `render("# A")` をアサートする Vitest を含む `examples/node/` を追加する。
   - `pnpm test:node` を CI に組み込み、Node 経由の最小パスが常に動作することを保証する。

4. PR4 – ブラウザスモークテスト
   - `examples/web/`（例: Vite）を追加し、ブラウザ上で WASM レンダーパスを実行する Playwright テストを追加する。
   - 少なくとも 1 つの簡単な Markdown 入力に対して、HTML 出力と見出し情報の双方を検証する。

5. PR5 – README 現実チェック
    - README を RSMD MVP に焦点を当てるよう更新し、ハイブリッド計画を「将来の作業」に移動する。
    - 現状の実装が
      - どこまで single-pass か
      - どこが暫定的な 2 パス実装なのか
      を明示し、性能目標と今後の最適化 PR（PR1 など）との対応関係を書いておく。
    - Unicode バージョン表記（16.0）や CJK 対応スコープを README / DESIGN 両方で揃える。
    - クイックスタートガイド（Rust / WASM / Node / Web それぞれの最小例）を追加する。

6. PR6 – 最小限 CLI（オプション）
    - 見出し出力用の `--json` 付き `stdin` → `stdout` CLI を提供し、README に文書化する。
    - CI で簡単な CLI スモークテスト（`echo "# A" | rsmd-cli --json`）を回す。

## バックログ

- h4〜h6 見出しと TOC ユーティリティのサポート。
- HTML 対応範囲（リスト、コードブロック、リンクなど）を小さな増分で拡張。
- リグレッション用のスナップショット / プロパティテストを追加。
- エラータイプと public ABI を安定化。
- WASM vs. ネイティブバイナリのパッケージング戦略を評価。
- setext 見出しや GFM 拡張など、CommonMark 準拠レベルをどこまで目指すかのポリシーを DESIGN に明文化。
