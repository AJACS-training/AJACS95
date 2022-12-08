# ブラウザ上でRNA-seqデータを解析する

安水 良明 [Twitter @yyoshiaki](https://twitter.com/yyoshiaki)  
大阪大学 免疫学フロンティア研究センター 実験免疫学  
大阪大学先導的学際研究機構生命医科学融合フロンティア研究部門

## iDEP演習

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


