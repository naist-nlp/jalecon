# JaLeCoN
[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC_BY--NC--SA_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

JaLeCoN is a Dataset of **Ja**panese **Le**xical **Co**mplexity for **N**on-Native Readers. It can be used to train or evaluate Japanese lexical complexity prediction models.

Please cite the following paper in publications using this dataset. You can also refer to it for details about the data and a baseline experiment.

```
@inproceedings{ide2023,
  title     = "Japanese Lexical Complexity for Non-Native Readers: A New Dataset",
  booktitle = "Proceedings of the Eighteenth Workshop on Innovative Use of {NLP} for Building Educational Applications",
  author    = "Ide, Yusuke and Mita, Masato and Nohejl, Adam and Ouchi, Hiroki and Watanabe, Taro",
  month     = July,
  year      = 2023,
  publisher = "Association for Computational Linguistics",
}
```

## Overview

`ck.txt`, `non_ck.txt`
- These files contain data for each L1 group. See also [Data Format](#data-format) below.

`cv_splits.json`
- This file contains cross-validation splits. See also [Cross-Validation Splits](#cross-validation-splits) below.

`mwe_list`
- This directory contains manually collected lists of ordinary MWEs in `mwe.tsv` and proper noun MWEs in `proper_mwe.tsv`. Sequences of SUWs that match any MWEs in the lists are chunked in the step of MWE annotation (see Section 3.3 of our paper). `|` in the MWEs denotes a boundary between SUWs.

## Data Format

The files `ck.txt` and `non_ck.txt` are composed of sentences, each of which has the following format (terminated by a blank line):

```
<ID>\t<sentence_num>\t<sentence>
<word_1>
[…]
<word_n>

```

Example:
```
news-1-1	1	ネット|情報|から|危機|発生|を|瞬時|に|検知|、|デマ|も|判定|（|JBpress|）|-|Yahoo|!|ニュース
ネット	0:1	-	7	0	0	0	0.0
情報	1:2	-	6	1	0	0	0.047
[…]
-	16:17	-	7	0	0	0	0.0
Yahoo!ニュース	17:20	P	-	-	-	-	-

```

The first line is in the format `<ID>\t<sentence_num>\t<sentence>`:

- `ID` is the unique ID of the sentence in the format `<genre>-<block>-<line>`, where `genre` identifies the genre (`news` or `gov`); `block` is a unit annotated by the same set of annotators; `line` is the line number in the block.
- `sentence_num` is the sentence number in the sequence to which the sentence belongs. This is useful for knowing where each sequence starts and ends.
- `sentence` is the sentence itself segmented into SUWs (delimited with `|`).

The subsequent `<word_i>` lines provide information about each word in the sentence in the format `<word>\t<position>\t<ignore>\t<easy>\t<not_easy>\t<diff>\t<very_diff>\t<complexity>`:

- `position` consists of the start and end offsets of the word separated with a colon.
- `ignore` contains `P` or `Q` when the word should be ignored. Specifically, `P` means that the word is a proper noun; `S` means that an error related to segmentation occurred in the word.
- `easy`, `not_easy`, `diff`, and `very_diff` are the numbers of annotators who assigned the respective label to the word.
- `complexity` is the complexity score of the word for the L1 group.

When the word is to be ignored, the fields `easy`, `not_easy`, `diff`, `very_diff`, and `complexity` each contain `-`.

## Cross-Validation Splits

The file `cv_splits.json` contains cross-validation splits that we have used for the experiment in our paper, and which can therefore be used to replicate it or to easily compare our results with other systems.

We have used:
- 5 folds,
- stratification by genre (News and Government),
- grouping by sequence of sentences (see Section 3 in the paper).

Genres are thus equally represented in each fold, and a sequence of sentences is never split between a training set and a validation set.

`cv_splits.json` has the following form:

```
[
	[split1_train, split1_validation],
	[split2_train, split2_validation],
	[split3_train, split3_validation],
	[split4_train, split4_validation],
	[split5_train, split5_validation]
]
```

Each split's training or validation set is a list of IDs (first tab-separated fields of the sentence lines in the dataset files).

## Data Sources

### News Genre

We used data from the Japanese-English development set of [the WMT22 General Machine Translation Task](https://www.statmt.org/wmt22/translation-task.html) (Kocmi et al. 2022[^1]).

[^1]: Tom Kocmi, Rachel Bawden, Ondřej Bojar, Anton Dvorkovich, Christian Federmann, Mark Fishel, Thamme Gowda, Yvette Graham, Roman Grund- kiewicz, Barry Haddow, Rebecca Knowles, Philipp Koehn, Christof Monz, Makoto Morishita, Masaaki Nagata, Toshiaki Nakazawa, Michal Novák, Martin Popel, and Maja Popović. 2022. Findings of the 2022 conference on machine translation (WMT22). In Proceedings of the Seventh Conference on Machine Translation (WMT), pages 1–45, Abu Dhabi, United Arab Emirates (Hybrid). Association for Computational Linguistics

### Government Genre

Website of the Japan Meteorological Agency (気象庁ウェブサイト):

https://www.jma.go.jp/jma/kishou/tyoukan/2023/dg_20230105.html
https://www.jma.go.jp/jma/kishou/tyoukan/2022/dg_20221221.html
https://www.jma.go.jp/jma/kishou/tyoukan/2022/dg_20221116.html
https://www.jma.go.jp/jma/kishou/tyoukan/2022/dg_20221018.html

Website of the Japan Tourism Agency (観光庁ウェブサイト):

https://www.mlit.go.jp/kankocho/page01_000728.html
https://www.mlit.go.jp/kankocho/page01_000723.html
https://www.mlit.go.jp/kankocho/page01_000718.html
https://www.mlit.go.jp/kankocho/page01_000715.html
https://www.mlit.go.jp/kankocho/page01_000712.html

Website of the Ministry of Justice (法務省ウェブサイト):

https://www.moj.go.jp/hisho/kouhou/hisho08_00386.html
https://www.moj.go.jp/hisho/kouhou/hisho08_00381.html
https://www.moj.go.jp/hisho/kouhou/hisho08_00380.html
https://www.moj.go.jp/hisho/kouhou/hisho08_00375.html
https://www.moj.go.jp/hisho/kouhou/hisho08_00344.html
https://www.moj.go.jp/hisho/kouhou/hisho08_00341.html

Website of the Ministry of Foreign Affairs (外務省ウェブサイト):

https://www.mofa.go.jp/mofaj/press/kaiken/kaiken22_000066.html
https://www.mofa.go.jp/mofaj/press/kaiken/kaiken24_000174.html
https://www.mofa.go.jp/mofaj/press/kaiken/kaiken4_001075.html
https://www.mofa.go.jp/mofaj/press/kaiken/kaiken24_000165.html
https://www.mofa.go.jp/mofaj/press/kaiken/kaiken24_000164.html

Website of the Ministry of Health, Labour and Welfare (厚生労働省ウェブサイト):

https://www.mhlw.go.jp/stf/kaiken/daijin/0000194708_00527.html
https://www.mhlw.go.jp/stf/kaiken/daijin/0000194708_00525.html
https://www.mhlw.go.jp/stf/kaiken/daijin/0000194708_00522.html
https://www.mhlw.go.jp/stf/kaiken/daijin/0000194708_00516.html
https://www.mhlw.go.jp/stf/kaiken/daijin/0000194708_00515.html
https://www.mhlw.go.jp/stf/kaiken/daijin/0000194708_00514.html

## License

This work is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[cc-by-nc-sa]: https://creativecommons.org/licenses/by-nc-sa/4.0/
