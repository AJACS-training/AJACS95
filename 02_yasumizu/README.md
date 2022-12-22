# ブラウザ上でRNA-seqデータを解析する

安水 良明 [Twitter @yyoshiaki](https://twitter.com/yyoshiaki)  
大阪大学 免疫学フロンティア研究センター 実験免疫学  
大阪大学先導的学際研究機構生命医科学融合フロンティア研究部門

## [AJACS 84 小野さん資料](https://github.com/AJACS-training/AJACS84/tree/main/01_ono)

## この資料の使い方

以下のコマンドでレポジトリをクローンできます。ブラウザから直接ダウンロードもできます。

```
git clone https://github.com/AJACS-training/AJACS95.git
```

## [iDEP](http://bioinformatics.sdstate.edu/idep/)演習

### 使用するデータ
Cuadrado, Eloy, Maartje van den Biggelaar, Sander de Kivit, Yi Yen Chen, Manon Slot, Ihsane Doubal, Alexander Meijer, Rene A. W. van Lier, Jannie Borst, and Derk Amsen. 2018. “Proteomic Analyses of Human Regulatory T Cells Reveal Adaptations in Signaling Pathways That Protect Cellular Identity.” Immunity 48 (5): 1046–59.e6. [https://doi.org/10.1016/j.immuni.2018.04.008](https://doi.org/10.1016/j.immuni.2018.04.008), [SRA Run Selector](https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA355160&o=acc_s%3Aa)

### ikraを用いた遺伝子発現量定量
[ikra](https://github.com/yyoshiaki/ikra)

#### ikra install

1. docker install [https://www.docker.com/](https://www.docker.com/)
2. clone ikra

```
$ git clone https://github.com/yyoshiaki/ikra.git
```

#### input tableの作成

[SRA Run Selector](https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA355160&o=acc_s%3Aa) よりMetadataをダウンロード。Excelなどでcsvに整形。iDEPのinputとして使用するには、groupA_1, groupA_2など、グループ名+アンダースコア+replicate numberにする必要あり。

#### ikra実行

```
$ ikra ikra_input.csv human --protein-coding
```

出力ファイルは`output.tsv`

### TCR遺伝子を除く

```
$ cat output.tsv | grep -v -E "^TRBV\d*|^TRBD\d*|^TRBJ\d*|^TRDV\d*|^TRDD\d*|^TRDJ\d*|^TRAV\d*|^TRAJ\d*|^TRGV\d*|^TRGJ\d*" > output.rmTCR.tsv
```

## [RaNA-seq](https://ranaseq.eu/)

`count.tsv`をiDEPに読み込ませるための改変。ファイルの先頭にタブを挿入、Ensembl IDsの修正。

```
$ cat counts.tsv | sed -E 's/(ENSG[0-9]+)\.[0-9]+/\1/g' | sed '1s/^/\t/' > counts.mod.tsv
```

### 参考 : RaNA-seq用テストデータの作成

```
$ zcat SRR5058658.fastq.gz | head -n 100000 > SRR5058658.mini.fastq
$ pigz SRR5058658.mini.fastq
```

## [BioJupies](https://maayanlab.cloud/biojupies/)

## 紹介論文

Yasumizu, Yoshiaki, Naganari Ohkura, Hisashi Murata, Makoto Kinoshita, Soichiro Funaki, Satoshi Nojima, Kansuke Kido, et al. 2022. “Myasthenia Gravis-Specific Aberrant Neuromuscular Gene Expression by Medullary Thymic Epithelial Cells in Thymoma.” Nature Communications 13 (1): 4230. [https://doi.org/10.1038/s41467-022-31951-8](https://doi.org/10.1038/s41467-022-31951-8)

## 参考URL

- [nf-core RNAseq](https://nf-co.re/rnaseq/usage)
- [VIRTUS2](https://github.com/yyoshiaki/VIRTUS2)
- [ARCHS4](https://maayanlab.cloud/archs4/)

## Q&A 2022/12/22

### RaNA-seqにアップロードするfastqファイルはトリミング済みのものをアップロードするのでしょうか？それともシーケンサーから得られた生データで大丈夫ですか？
生データです。

### 異なるシーケンサーで読まれた公共データを自前でまとめて解析する際に注意すべきことはあるでしょうか。
バッチ効果などを慎重に評価・補正する必要があります。

### 新しいツールの情報などについては論文を追うしかないのでしょうか。
論文や検索エンジン、学会、TwitterなどのSNSが有用です。

### 今現在まだCPM補正を行うことはあるか
CPMはcount値をmilloin単位に補正した最もシンプルな補正です。幅広く使用されていますが、RNA-seqにおいては遺伝子長さによる補正が適切なのでTPMなどが使用されます。

### edgeRを使っていますが、TMM正規化を行っていればTPM正規化は不要ですか？
TMMのあと、もしくは前にTPM補正をかけることはできません。

### single-cellで低発現遺伝子が心配
公共データを用いたシングルセルデータ再解析をして、実際に発現を確認されるといいかと思います。

### iDEPで実施した解析がRなどのコードでどう書くか出してくれるサービスがiDEP内にはあるのでしょうか。
Rタブからコードのダウンロードができますが、以前試した際にはそれ単体では動かず断念しました。もし可能なら大変便利なので、試されてできそうでしたらぜひ教えて下さい。

### 非モデル生物の場合は？
ikraはマウス・ヒトのみ対応 
ゲノム配列 or トランスクリプト配列 と遺伝子アノテーションを用意し、自力で解析ツールを回すのがよいか。
iDEPは様々な種に対応。

### どのように勉強すれば良い？
本講習会、日本語書籍など参考書が出ています。ただし、解析にあたっては必ず各ツールの公式サイトも見るようにしてください。また、参考文献のmethodも参考にしてください。

### セキュリティの問題は？
iDEPにupするデータはサマリーデータなので、個人情報には当たりませんが、機密情報であることには代わりありません。不安であればsecureな環境で解析してください。

### きれいなグラフを描きたい
iDEPはepsで書き出せるため、illustratorで文字やレイアウトの修正が可能です。私は有効な解析をiDEPで確認した後、論文で使用する場合はRで解析をし直して製図したりします。

### scRNAseqに興味がある
本講習会はバルクRNAseqについてのみです。ただし、single cell解析にあたってはバルクRNAseqの仕組みは理解しておく必要があります。また、実例紹介ではシングルセルとの統合についても簡単にご紹介いたします。

### Macを買えば問題ない？
Intel macはスムーズに解析出来ましたが、Apple silicon Macはツールによっては動きません。製図用や端末としての使用にとどめ、リモートのコンピューターを別途用意するのがよいです。

### 感染症のRNAseq
ヒトに感染するウイルスならVIRTUSというツールをおすすめしています。
https://github.com/yyoshiaki/VIRTUS2

### ゲノムデータとの統合 （vcf）
eQTL, sQTL解析などが出来ますが、これらはno codeでは出来ません。

### Bioinformatics解析はすべて無料で出来て安上がり？
そんなことはありません。リソースの維持や人件費などがかかります。共同研究先に解析を依頼される場合はこのあたりにご注意ください。

