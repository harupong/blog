---
{"dg-publish":true,"permalink":"/Kindleライブラリをセルフホストしたい/","created":"2025-05-30T10:53:49.239+09:00","updated":"2025-06-13T15:37:48.446+09:00"}
---

[[⚒️PROJECTS\|⚒️PROJECTS]]
***
Amazonから買った電子書籍が自由にならないのは癪なので、セルフホストを試みている記録。

## 全体像
![Kindleライブラリをセルフホストしたい 2025-06-11 09.21.37.excalidraw.png|600](/img/user/attachments/Kindle%E3%83%A9%E3%82%A4%E3%83%96%E3%83%A9%E3%83%AA%E3%82%92%E3%82%BB%E3%83%AB%E3%83%95%E3%83%9B%E3%82%B9%E3%83%88%E3%81%97%E3%81%9F%E3%81%84%202025-06-11%2009.21.37.excalidraw.png)

## Calibre環境をセットアップ
すべてを[[EPUB\|EPUB]]に変換するため、ローカル（WindowsとMac）に[[Calibre\|Calibre]]をインストールする。[[Kindle本をCalibreでゴニョゴニョする方法\|Kindle本をCalibreでゴニョゴニョする方法]]はここでは触れない。詳細はChatGPTにでも聞いてみるとよいかと。

自宅サーバーに[[Calibre Web\|Calibre Web]]をセットアップして、EPUBデータをセルフホストする。Kindle Paperwhiteからアクセスするためにはインターネットに公開する必要があるので、[[Pangolin\|Pangolin]]を使ってセキュアに公開する。
## CalibreライブラリをNASへ転送
CalibreライブラリをNASに置いて運用するのは[公式には非推奨](https://manual.calibre-ebook.com/ja/faq.html#i-am-getting-errors-with-my-calibre-library-on-a-networked-drive-nas)。トラブっても泣かない精神でいく。ローカルの複数環境でデータを作成するため、ライブラリデータをNASで共有したい。

Windows環境からは`Robocopy`、Mac環境からは`rsync`でデータを転送する。NAS上のデータを壊したくないのでデータは同期しない。

Calibre WebからNAS上のCalibreライブラリにSMBでアクセスするには、cifsのマウントオプションに"nobrl"を追加する必要があるので注意。参考 [Database errors · Issue janeczku/calibre-web](https://github.com/janeczku/calibre-web/issues/2698#issuecomment-1533893962)
## Kindle PaperwhiteをJailbreak/KOReaderセットアップ
素のKindleではEPUBデータが読み込めないのでJailbreakする。[[Kindle Paperwhite\|Kindle Paperwhite]]をMacにつないで、[Kindle Modding Wiki](https://kindlemodding.org/)の内容にそってポチポチやるとKindleがJailbreakできる。その流れで[[KOReader\|KOReader]]をインストールもおこなう。
## 電子書籍を読む方法
JailbreakしたKindle PaperwhiteのKOReaderをメイン、iPhoneの[Cantook by Aldiko](https://apps.apple.com/jp/app/cantook-by-aldiko/id1476410111)をサブ環境として使う。

どちらもCalibre WebのOPDS経由でデータをダウンロードする。読書進捗の同期は苦難の道なのでひとまずは諦めてしまおう。

[[KOReader\|KOReader]]はネイティブでは縦読みに[対応していない](https://github.com/koreader/koreader/issues/11469)。以下の対策を両方ともおこなうと、擬似的ではあるが縦読みが実現できる。

1. [MyK00L/tategakifont](https://github.com/MyK00L/tategakifont)を使ってフォントを90度回転させる
	- `brew install fontforge`で依存関係が導入できる
2. [plateaukao/koreader\_patch\_vertical\_read](https://github.com/plateaukao/koreader_patch_vertical_read?tab=readme-ov-file)を使ってKOReaderを擬似的に縦書き対応にする

縦書きは明朝体が読みやすいので、Googleが公開していて改変が許可されている[BIZ UD明朝](https://fonts.google.com/specimen/BIZ+UDMincho/license)フォントを90度回転させて導入してみる。

## 次に試したいもの
- [readest/readest: Readest is a modern, feature-rich ebook reader designed for avid readers offering seamless cross-platform access, powerful tools, and an intuitive interface to elevate your reading experience.](https://github.com/readest/readest)
- [stumpapp/stump: A free and open source comics, manga and digital book server with OPDS support (WIP)](https://github.com/stumpapp/stump)

## これも読みたい
- [OK, Reader — 🦄🌈 Brie Carranza](https://brie.dev/ok-reader/)
