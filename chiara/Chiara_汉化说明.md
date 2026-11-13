# 琪娅拉（Chiara）汉化说明

模组作者：octavian@rosenranken.de｜汉化范围：全部 tra 文本 + tp2 乱码修复

## 一、文本范围

tra 层模组，英文源在 `Languages\english\`，共 **13 个有内容的 tra + 1 个 0 字节空文件**，合计 **232 条**，已全部译为简体中文写入 `Languages\schinese\`（目录原本存在但为空）：

| 文件 | 条目 | 内容 |
|---|---|---|
| `V#Chiara.tra` | 30 | 神殿区下水道初遇、守卫追捕、真相澄清、招募与再会 |
| `V#Guard.tra` | 27 | 守卫委托调查「下水道里的吸血鬼」、交任务、识破同行 |
| `CHIA_INT.tra` | 28 | 与艾黎谈心、与艾德温互怼、简的萝卜汁长篇大论、与安诺门争论亡灵善恶、与马尔西斯相关段落、最终战表态 |
| `Int_Chiara.tra` | 30 | 城内见闻、影贼势力、马尔西斯招募与对话、波迪抉择 |
| `NPC.tra` | 20 | 雅各布（血供货人）全部对话 |
| `V#ChiaP.tra` | 15 | 离队／归队、狼形态告别、约定下水道再会 |
| `V#ChiaP` 系列之外 | — | 见下 |
| `setup.tra` | 60 | 组件名、姓名、BIO、全套喊话与语音（`[chiaraX]` 标记）、三把剑／箭／面具／手套／血袋／药剂／血之能力名称与说明 |
| `V#Relaed.tra` | 4 | 血贩雷莱德 |
| `add_duer.tra` | 8 | 酒馆里买兽血 |
| `add_wirt.tra` | 6 | 找雷莱德之前的打听 |
| `v#chia.tra` | 2 | 野外渴血触发 |
| `ChiaraPatch.tra` | 1 | 修复音效包提示 |
| `v#jacob.tra` | 0 | 原文件就是 0 字节，按原样保留空文件 |

`.d`、`.cre`、`.itm`、`.spl`、`.baf` 未做改动（唯一例外见第三节），`Languages\english`、`deutsch`、`french` 原样保留，因此不需要 `.bak`。

## 二、编码

按要求输出 **ANSI（GBK / cp936）无 BOM、CRLF**。

- 原英文 tra 编码不统一：`Int_Chiara.tra` 是 cp1252（原文里有 `´power´` 之类的单字节引号），其余是 UTF-8；读取时分别按原编码解码，再统一转 GBK，不会二次乱码。
- tp2 里有 `HANDLE_CHARSETS infer_charset = 1`、`tra_path = %MOD_FOLDER%/Languages`、`reload_array = setup`，EE/EET 安装时会按语言名推断 GBK 并自动转 UTF-8，所以 ANSI 是安全的。
- 已确认输出**无全角空格（U+3000）、无不间断空格（U+00A0）**——两者是 ANSI→UTF-8 后 WeiDU 报 `LEXER ERROR: invalid character` 的常见元凶。

## 三、tp2 乱码修复

原 `Setup-Chiara.tp2` 里有 9 个非 ASCII 字节（德文变音 `ü`/`ä`/`ö`，cp1252 编码）。在 UTF-8 编辑器里看就是 `Blutf�higkeiten` 这种乱码，而 WeiDU 按 ANSI 读又会和 UTF-8 混用出岔子。做法是让它变成**纯 7-bit ASCII 文件**（ASCII 同时是合法 UTF-8，任何编辑器读都正常，WeiDU 读也不会有编码歧义）：

1. **目录改名**：`Blutfähigkeiten` → `Blutfaehigkeiten`（唯一被 tp2 引用的带变音路径，只有 tp2 引用它，改后 4 处 `COPY` 全部对齐）。
2. **注释转写**：`Voraussetzungen für die Installation` → `fuer`、`Bilder für Gegenstände` → `fuer / Gegenstaende`、`Banter-BCS-Ergänzungen` → `Ergaenzungen`、`nach Installation öffnen` → `oeffnen`。德文注释保留，只把变音字母转写。
3. **顺手修掉一个会让安装出错的空 `USING`**：原文件在
   `COMPILE ~chiara/Chiara_Patch/ChiaraPatch.d~` 后面跟了一行空的 `USING`。该 `.d` 的对白是**内嵌写在脚本里**的（不走 tra），留着这行空 `USING` 只会让 WeiDU 把下一行当参数。已删除。

修复后校验：非 ASCII 字节 9 → 0，全文 CRLF，其余内容逐行 diff 一致（仅上述 9 处改动）。

另外，`Chiara_Patch\ChiaraPatch.d` 里那句玩家能看到的德文台词 `Ich richte nun die Soundsets!` 已译为「我现在把音效包设好了！」。因为在 EE 里 `.d` 编译出的字符串直接进 TLK、按 UTF-8 解读，所以这个文件改写为 **UTF-8 无 BOM** 保存（这是本次唯一的非 tra 改动，别的地方仍是纯 ASCII/ANSI）。

## 四、安装

`Setup-Chiara.tp2` 的 `LANGUAGE ~Schinese~ ~schinese~ ~chiara/Languages/schinese/setup.tra~` 分支本来就有，文件已补齐，直接运行 `Setup-Chiara.exe` 选 **Schinese** 即可。

## 五、译名与文风

- Chiara → **琪娅拉**；全名 Chiara Marna Rhie-Tara de Velastan → 琪娅拉·玛尔娜·蕾-塔拉·德·维拉斯塔
- Jacob / Jacob Dorjion → 雅各布 / 雅各布·多吉昂｜Relaed 雷莱德｜Malthis 马尔西斯
- Aerie 艾黎｜Edwin Odesseiron 艾德温·奥德赛伦｜Jan（简·詹森）简｜Anomen 安诺门｜Yoshimo 吉野
- Athkatla 阿斯卡特拉｜Temple District 神殿区｜Copper Coronet 铜冠酒馆｜Sword Coast 剑湾｜Faerûn 费伦｜Shadow Thieves 影贼｜City Guard 城市守卫｜Bodhi 波迪｜Irenicus 艾瑞尼卡斯
- Helm 海姆｜Clangeddin 克兰格汀｜Tymora 泰摩拉｜Loviatar 罗维塔｜Cyric 希瑞克

语气：琪娅拉是贵族出身、说辞端严又带忧郁的吸血鬼，句式保留长句与「我——要——叫——我——夫人」这种强调；简·詹森照原文保留跑题、堆形容词、越扯越远的活宝腔；艾德温保留自大狂的括号独白。

## 六、校验结果

232/232 条全部译出，无英文残留（仅 `Rosenranken` 论坛名与占位符 `CHARNAME` 等保留）：

| 检查项 | 结果 |
|---|---|
| 条目数 / id 集合 | 13 个文件逐一对齐，`v#jacob.tra` 空文件对空文件 |
| `~` 配对 | 56/56、2/2、60/60、40/40、30/30、60/60、54/54、2/2、8/8、16/16、12/12、120/120、4/4 全一致 |
| 尾部 `[chiara…]` 语音标记 | 全部保留原位（含 `setup.tra` 22 条语音） |
| 占位符 `<CHARNAME>` | 逐条同数 |
| 空条目 | `setup.tra` 的 `@32/@33/@34/@45/@46` 按原文字面保持空串 |
| 编码 | GBK 可无损解码、无 BOM、全 CRLF、无全角／不间断空格 |
