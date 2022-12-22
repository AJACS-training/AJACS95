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
