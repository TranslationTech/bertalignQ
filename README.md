# Bertalign

An automatic mulitlingual sentence aligner.

Bertalign is designed to facilitate the construction of multilingual parallel corpora and translation memories, which have a wide range of applications in translation-related research such as corpus-based translation studies, contrastive linguistics, computer-assisted translation, translator education and machine translation.

## Approach

Bertalign uses [sentence-transformers](https://github.com/UKPLab/sentence-transformers) to represent source and target sentences so that semantically similar sentences in different languages are mapped onto similar vector spaces. Then a two-step algorithm based on dynamic programming is performed: 1) Step 1 finds the 1-1 alignments for approximate anchor points; 2) Step 2 limits the search path to the anchor points and extracts all the valid alignments with 1-many, many-1 or many-to-many relations between the source and target sentences.

## Performance

According to our experiments, Bertalign achieves more accurate results on [Text+Berg](./text+berg), a publicly available German-French parallel corpus, than the traditional length-, dictionary-, or MT-based alignment methods as reported in [Thompson & Koehn (2019)](https://aclanthology.org/D19-1136/)

## Languges Supported

Alignment between 25 languages: Catalan (ca), Chinese (zh), Czech (cs), Danish (da), Dutch (nl), English(en), Finnish (fi), French (fr), German (de), Greek (el), Hungarian (hu), Icelandic (is), Italian (it), Lithuanian (lt), Latvain (lv), Norwegian (no), Polish (pl), Portuguese (pt), Romanian (ro), Russian (ru), Slovak (sk), Slovenian (sl), Spanish (es), Swedish (sv), and Turkish (tr).

## Installation

Please see [requirements.txt](./requirements.txt) for installation. 

### You can also install Bertalign and run the examples directly in a [Google Colab notebook](https://colab.research.google.com/drive/123GhXwgwmQp1F5SVZ74_uIgyxo6hLRq0?usp=sharing).

## Basic example

Just import *Bertalign* and initialize it with the source and target text, which will detect the source and target language automatically and split both texts into sentences. Then invoke the method *align_sents()*  to align sentences and print out the result with *print_sents()*.

```python
from bertalign import Bertalign
```

```python
src = """兩年以後，大興安嶺。
“順山倒咧——”
隨著這聲嘹亮的號子，一棵如巴特農神廟的巨柱般高大的落葉松轟然倒下，葉文潔感到大地抖動了一下。她拿起斧頭和短鋸，開始去除巨大樹身上的枝丫。每到這時，她總覺得自己是在為一個巨人整理遺體。她甚至常常有這樣的想象：這巨人就是自己的父親。兩年前那個悽慘的夜晚，她在太平間為父親整理遺容時的感覺就在這時重現。巨松上那綻開的樹皮，似乎就是父親軀體上累累的傷痕。
內蒙古生產建設兵團的六個師四十一個團十多萬人就分佈在這遼闊的森林和草原之間。剛從城市來到這陌生的世界時，很多兵團知青都懷著一個浪漫的期望：當蘇修帝國主義的坦克叢集越過中蒙邊境時，他們將飛快地武裝起來，用自己的血肉構成共和國的第一道屏障。事實上，這也確實是兵團組建時的戰略考慮之一。但他們渴望的戰爭就像草原天邊那跑死馬的遠山，清晰可見，但到不了眼前，於是他們只有墾荒、放牧和砍伐。這些曾在“大串聯”中燃燒青春的年輕人很快發現，與這廣闊天地相比，內地最大的城市不過是個羊圈；在這寒冷無際的草原和森林間，燃燒是無意義的，一腔熱血噴出來，比一堆牛糞涼得更快，還不如後者有使用價值。但燃燒是他們的命運，他們是燃燒的一代。於是，在他們的油鋸和電鋸下，大片的林海化為荒山禿嶺；在他們的拖拉機和康拜因（聯合收割機）下，大片的草原被犁成糧田，然後變成沙漠。
葉文潔看到的砍伐只能用瘋狂來形容，高大挺拔的興安嶺落葉松、四季常青的樟子松、亭亭玉立的白樺、聳入雲天的山楊、西伯利亞冷杉，以及黑樺、柞樹、山榆、水曲柳、鑽天柳、蒙古櫟，見什麼伐什麼，幾百把油鋸如同一群鋼鐵蝗蟲，她的連隊所過之處，只剩下一片樹樁。
整理好的落葉松就要被履帶拖拉機拖走了，在樹幹另一頭，葉文潔輕輕撫摸了一下那嶄新的鋸斷面，她常常下意識地這麼做，總覺得那是一處巨大的傷口，似乎能感到大樹的劇痛。她突然看到，在不遠處樹樁的鋸斷面上，也有一隻在輕輕撫摸的手，那手傳達出的心靈的顫抖，與她產生了共振。那手雖然很白皙，但能夠看出是屬於男性的。葉文潔抬頭，看到撫摸樹樁的人是白沐霖，一個戴眼鏡的瘦弱青年，他是兵團《大生產報》的記者，前天剛到連隊來採訪。葉文潔看過他寫的文章，文筆很好，其中有一種與這個粗放環境很不協調的纖細和敏感，令她很難忘。"""

tgt = """Two years later, the Greater Khingan Mountains
“Tim-ber…”
Following the loud chant, a large Dahurian larch, thick as the columns of the Parthenon, fell with a thump, and Ye Wenjie felt the earth quake.
She picked up her ax and saw and began to clear the branches from the trunk. Every time she did this, she felt as though she were cleaning the corpse of a giant. Sometimes she even imagined the giant was her father. The feelings from that terrible night two years ago when she cleaned her father’s body in the mortuary would resurface, and the splits and cracks in the larch bark seemed to turn into the old scars and new wounds covering her father.
Over one hundred thousand people from the six divisions and forty-one regiments of the Inner Mongolia Production and Construction Corps were scattered among the vast forests and grasslands. When they first left the cities and arrived at this unfamiliar wilderness, many of the corps’ “educated youths”—young college students who no longer had schools to go to—had cherished a romantic wish: When the tank clusters of the Soviet Revisionist Imperialists rolled over the Sino-Mongolian border, they would arm themselves and make their own bodies the first barrier in the Republic’s defense. Indeed, this expectation was one of the strategic considerations motivating the creation of the Production and Construction Corps.
But the war they craved was like a mountain at the other end of the grassland: clearly visible, but as far away as a mirage. So they had to content themselves with clearing fields, grazing animals, and chopping down trees.
Soon, the young men and women who had once expended their youthful energy on tours to the holy sites of the Chinese Revolution discovered that, compared to the huge sky and open air of Inner Mongolia, the biggest cities in China’s interior were nothing more than sheep pens. Stuck in the middle of the cold, endless expanse of forests and grasslands, their burning ardor was meaningless. Even if they spilled all of their blood, it would cool faster than a pile of cow dung, and not be as useful. But burning was their fate; they were the generation meant to be consumed by fire. And so, under their chain saws, vast seas of forests turned into barren ridges and denuded hills. Under their tractors and combine harvesters, vast tracts of grasslands became grain fields, then deserts.
Ye Wenjie could only describe the deforestation that she witnessed as madness. The tall Dahurian larch, the evergreen Scots pine, the slim and straight white birch, the cloud-piercing Korean aspen, the aromatic Siberian fir, along with black birch, oak, mountain elm, Chosenia arbutifolia—whatever they laid eyes on, they cut down. Her company wielded hundreds of chain saws like a swarm of steel locusts, and after they passed, only stumps were left.
The fallen Dahurian larch, now bereft of branches, was ready to be taken away by tractor. Ye gently caressed the freshly exposed cross section of the felled trunk. She did this often, as though such surfaces were giant wounds, as though she could feel the tree’s pain. Suddenly, she saw another hand lightly stroking the matching surface of the stump a few feet away. The tremors in that hand revealed a heart that resonated with hers. Though the hand was pale, she could tell it belonged to a man.
She looked up. It was Bai Mulin. A slender, delicate man who wore glasses, he was a reporter for the Great Production News, the corps’ newspaper. He had arrived the day before yesterday to gather news about her company. Ye remembered reading his articles, which were written in a beautiful style, sensitive and fine, ill suited to the rough-hewn environment."""
```

```python
aligner = Bertalign(src, tgt)
aligner.align_sents()
```

    Source language: Chinese, Number of sentences: 21
    Target language: English, Number of sentences: 32
    Embedding source and target text using LaBSE ...
    Performing first-step alignment ...
    Performing second-step alignment ...
    Finished! Successfuly aligning 21 Chinese sentences to 32 English sentences

```python
aligner.print_sents()
```

    两年以后，大兴安岭。
    Two years later, the Greater Khingan Mountains
    
    “顺山倒咧——”
    “Tim-ber…”
    
    隨著這聲嘹亮的號子，一棵如巴特農神廟的巨柱般高大的落葉松轟然倒下，葉文潔感到大地抖動了一下。
    Following the loud chant, a large Dahurian larch, thick as the columns of the Parthenon, fell with a thump, and Ye Wenjie felt the earth quake.
    
    她拿起斧頭和短鋸，開始去除巨大樹身上的枝丫。
    She picked up her ax and saw and began to clear the branches from the trunk.
    
    每到這時，她總覺得自己是在為一個巨人整理遺體。
    Every time she did this, she felt as though she were cleaning the corpse of a giant.
    
    她甚至常常有這樣的想象：這巨人就是自己的父親。
    Sometimes she even imagined the giant was her father.
    
    兩年前那個悽慘的夜晚，她在太平間為父親整理遺容時的感覺就在這時重現。 巨松上那綻開的樹皮，似乎就是父親軀體上累累的傷痕。
    The feelings from that terrible night two years ago when she cleaned her father’s body in the mortuary would resurface, and the splits and cracks in the larch bark seemed to turn into the old scars and new wounds covering her father.
    
    內蒙古生產建設兵團的六個師四十一個團十多萬人就分佈在這遼闊的森林和草原之間。
    Over one hundred thousand people from the six divisions and forty-one regiments of the Inner Mongolia Production and Construction Corps were scattered among the vast forests and grasslands.
    
    剛從城市來到這陌生的世界時，很多兵團知青都懷著一個浪漫的期望：當蘇修帝國主義的坦克叢集越過中蒙邊境時，他們將飛快地武裝起來，用自己的血肉構成共和國的第一道屏障。
    When they first left the cities and arrived at this unfamiliar wilderness, many of the corps’ “educated youths”—young college students who no longer had schools to go to—had cherished a romantic wish: When the tank clusters of the Soviet Revisionist Imperialists rolled over the Sino-Mongolian border, they would arm themselves and make their own bodies the first barrier in the Republic’s defense.
    
    事實上，這也確實是兵團組建時的戰略考慮之一。
    Indeed, this expectation was one of the strategic considerations motivating the creation of the Production and Construction Corps.
    
    但他們渴望的戰爭就像草原天邊那跑死馬的遠山，清晰可見，但到不了眼前，於是他們只有墾荒、放牧和砍伐。
    But the war they craved was like a mountain at the other end of the grassland: clearly visible, but as far away as a mirage. So they had to content themselves with clearing fields, grazing animals, and chopping down trees.
    
    這些曾在“大串聯”中燃燒青春的年輕人很快發現，與這廣闊天地相比，內地最大的城市不過是個羊圈；在這寒冷無際的草原和森林間，燃燒是無意義的，一腔熱血噴出來，比一堆牛糞涼得更快，還不如後者有使用價值。
    Soon, the young men and women who had once expended their youthful energy on tours to the holy sites of the Chinese Revolution discovered that, compared to the huge sky and open air of Inner Mongolia, the biggest cities in China’s interior were nothing more than sheep pens. Stuck in the middle of the cold, endless expanse of forests and grasslands, their burning ardor was meaningless. Even if they spilled all of their blood, it would cool faster than a pile of cow dung, and not be as useful.
    
    但燃燒是他們的命運，他們是燃燒的一代。
    But burning was their fate; they were the generation meant to be consumed by fire.
    
    於是，在他們的油鋸和電鋸下，大片的林海化為荒山禿嶺；在他們的拖拉機和康拜因（聯合收割機）下，大片的草原被犁成糧田，然後變成沙漠。
    And so, under their chain saws, vast seas of forests turned into barren ridges and denuded hills. Under their tractors and combine harvesters, vast tracts of grasslands became grain fields, then deserts.
    
    葉文潔看到的砍伐只能用瘋狂來形容，高大挺拔的興安嶺落葉松、四季常青的樟子松、亭亭玉立的白樺、聳入雲天的山楊、西伯利亞冷杉，以及黑樺、柞樹、山榆、水曲柳、鑽天柳、蒙古櫟，見什麼伐什麼，幾百把油鋸如同一群鋼鐵蝗蟲，她的連隊所過之處，只剩下一片樹樁。
    Ye Wenjie could only describe the deforestation that she witnessed as madness. The tall Dahurian larch, the evergreen Scots pine, the slim and straight white birch, the cloud-piercing Korean aspen, the aromatic Siberian fir, along with black birch, oak, mountain elm, Chosenia arbutifolia—whatever they laid eyes on, they cut down. Her company wielded hundreds of chain saws like a swarm of steel locusts, and after they passed, only stumps were left.
    
    整理好的落葉松就要被履帶拖拉機拖走了，在樹幹另一頭，葉文潔輕輕撫摸了一下那嶄新的鋸斷面，她常常下意識地這麼做，總覺得那是一處巨大的傷口，似乎能感到大樹的劇痛。
    The fallen Dahurian larch, now bereft of branches, was ready to be taken away by tractor. Ye gently caressed the freshly exposed cross section of the felled trunk. She did this often, as though such surfaces were giant wounds, as though she could feel the tree’s pain.
    
    她突然看到，在不遠處樹樁的鋸斷面上，也有一隻在輕輕撫摸的手，那手傳達出的心靈的顫抖，與她產生了共振。
    Suddenly, she saw another hand lightly stroking the matching surface of the stump a few feet away. The tremors in that hand revealed a heart that resonated with hers.
    
    那手雖然很白皙，但能夠看出是屬於男性的。
    Though the hand was pale, she could tell it belonged to a man.
    
    葉文潔抬頭，看到撫摸樹樁的人是白沐霖，一個戴眼鏡的瘦弱青年，他是兵團《大生產報》的記者，前天剛到連隊來採訪。
    She looked up. It was Bai Mulin. A slender, delicate man who wore glasses, he was a reporter for the Great Production News, the corps’ newspaper. He had arrived the day before yesterday to gather news about her company.
    
    葉文潔看過他寫的文章，文筆很好，其中有一種與這個粗放環境很不協調的纖細和敏感，令她很難忘。
    Ye remembered reading his articles, which were written in a beautiful style, sensitive and fine, ill suited to the rough-hewn environment.

## Batch processing & evaluation

The following example shows how to use Bertalign to align the Text+Berg corpus, and evaluate its performance with gold standard alignments. The evaluation script [eval.py](./bertalign/eval.py) is based on [Vecalign](https://github.com/thompsonb/vecalign).

Please see [aligner.py](./bertalign/aligner.py) for more options to configure Bertalign.

```python
import os
from bertalign import Bertalign
from bertalign.eval import * 
```

```python
src_dir = 'text+berg/de'
tgt_dir = 'text+berg/fr'
gold_dir = 'text+berg/gold'
```

```python
test_alignments = []
gold_alignments = []
for file in os.listdir(src_dir):
    src_file = os.path.join(src_dir, file).replace("\\","/")
    tgt_file = os.path.join(tgt_dir, file).replace("\\","/")
    src = open(src_file, 'rt', encoding='utf-8').read()
    tgt = open(tgt_file, 'rt', encoding='utf-8').read()

    print("Start aligning {} to {}".format(src_file, tgt_file))
    aligner = Bertalign(src, tgt, is_split=True)
    aligner.align_sents()
    test_alignments.append(aligner.result)

    gold_file = os.path.join(gold_dir, file)
    gold_alignments.append(read_alignments(gold_file))
```

    Start aligning text+berg/de/001 to text+berg/fr/001
    Source language: German, Number of sentences: 137
    Target language: French, Number of sentences: 155
    Embedding source and target text using LaBSE ...
    Performing first-step alignment ...
    Performing second-step alignment ...
    Finished! Successfuly aligning 137 German sentences to 155 French sentences
    
    Start aligning text+berg/de/002 to text+berg/fr/002
    Source language: German, Number of sentences: 293
    Target language: French, Number of sentences: 274
    Embedding source and target text using LaBSE ...
    Performing first-step alignment ...
    Performing second-step alignment ...
    Finished! Successfuly aligning 293 German sentences to 274 French sentences
    
    Start aligning text+berg/de/003 to text+berg/fr/003
    Source language: German, Number of sentences: 95
    Target language: French, Number of sentences: 100
    Embedding source and target text using LaBSE ...
    Performing first-step alignment ...
    Performing second-step alignment ...
    Finished! Successfuly aligning 95 German sentences to 100 French sentences
    
    Start aligning text+berg/de/004 to text+berg/fr/004
    Source language: German, Number of sentences: 107
    Target language: French, Number of sentences: 112
    Embedding source and target text using LaBSE ...
    Performing first-step alignment ...
    Performing second-step alignment ...
    Finished! Successfuly aligning 107 German sentences to 112 French sentences
    
    Start aligning text+berg/de/005 to text+berg/fr/005
    Source language: German, Number of sentences: 36
    Target language: French, Number of sentences: 40
    Embedding source and target text using LaBSE ...
    Performing first-step alignment ...
    Performing second-step alignment ...
    Finished! Successfuly aligning 36 German sentences to 40 French sentences
    
    Start aligning text+berg/de/006 to text+berg/fr/006
    Source language: German, Number of sentences: 126
    Target language: French, Number of sentences: 131
    Embedding source and target text using LaBSE ...
    Performing first-step alignment ...
    Performing second-step alignment ...
    Finished! Successfuly aligning 126 German sentences to 131 French sentences
    
    Start aligning text+berg/de/007 to text+berg/fr/007
    Source language: German, Number of sentences: 197
    Target language: French, Number of sentences: 199
    Embedding source and target text using LaBSE ...
    Performing first-step alignment ...
    Performing second-step alignment ...
    Finished! Successfuly aligning 197 German sentences to 199 French sentences

```python
scores = score_multiple(gold_list=gold_alignments, test_list=test_alignments)
log_final_scores(scores)
```

     ---------------------------------
    |             |  Strict |    Lax  |
    | Precision   |   0.932 |   0.987 |
    | Recall      |   0.941 |   0.991 |
    | F1          |   0.936 |   0.989 |
     ---------------------------------

## Citation

Lei Liu & Min Zhu. 2022. Bertalign: Improved word embedding-based sentence alignment for Chinese–English parallel corpora of literary texts, *Digital Scholarship in the Humanities*. [https://doi.org/10.1093/llc/fqac089](https://doi.org/10.1093/llc/fqac089).

## Licence

Bertalign is released under the [GNU General Public License v3.0](./LICENCE)

## Credits

##### Main Libraries

* [sentence-transformers](https://github.com/UKPLab/sentence-transformers)

* [faiss](https://github.com/facebookresearch/faiss)

* [sentence-splitter](https://github.com/mediacloud/sentence-splitter)

##### Other Sentence Aligners

* [Hunalign](http://mokk.bme.hu/en/resources/hunalign/)

* [Bleualign](https://github.com/rsennrich/Bleualign)

* [Vecalign](https://github.com/thompsonb/vecalign)

## Todo List

- Try the [CNN model](https://tfhub.dev/google/universal-sentence-encoder-multilingual/3) for sentence embeddings
* Develop a GUI for Windows users
