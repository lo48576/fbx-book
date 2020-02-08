# 参考文献

* [FBX binary file format specification — Blender Developers Blog](https://code.blender.org/2013/08/fbx-binary-file-format-specification/)
    + FBX 7.4 のバイナリ形式の文法について網羅的な解説。
    + 本書ではこの記事とは一部異なる用語を用いている (特に node record, null record, property など)。
    + 一部に古い情報 (FBX 7.5 でヘッダが変化したことなど。コメントで指摘されている) や誤った情報 (真偽値のバイナリ表現について) がある。
* FBX SDK のリファレンス
    + バージョン毎に URI が異なる、スクリプトやヘッダ・フッタマシマシで滅茶苦茶重いなどいろいろ難があるが、一応公式 SDK の公式ドキュメント。
    + 基礎的な概念が解説されていることがあるので、 (C++ リファレンスではない方の文書に) 一通り目を通すことをおすすめしたい。
* [A quick tutorial about the FBX ASCII format – Banex Developer Blog](https://banexdevblog.wordpress.com/2014/06/23/a-quick-tutorial-about-the-fbx-ascii-format/)
    + FBX テキスト形式の文法とスキーマの一部についての解説。
    + `ReferenceInformationType` と `MappingInformationType` についての解説が図付きで特にわかりやすい。
      ジオメトリ (メッシュ) を解釈したいならこの図は必見。
