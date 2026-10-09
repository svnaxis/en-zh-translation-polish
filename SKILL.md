---
name: en-zh-translation-polish
description: Translate English passages into natural Chinese or polish Chinese translations against the source; supports bilingual output with Taiwanese conventions.
license: MIT
allowed-tools: Read Write Edit Bash
metadata:
  version: 1.2.0
  tags: [translation, traditional-chinese, english-to-chinese, 英譯中, 潤色, 中英對照, localization, 臺灣中文]
---

# 英譯中翻譯潤色 (English→Chinese Translate & Polish)

把英文譯成**忠實、道地的中文**並產出**英中對照**譯文。汲取余光中《論中文的常態與變態》、思果《翻譯研究》等臺灣翻譯名家的思想精髓，在完整、準確傳達原文的前提下，**以讀者為中心、依文本類型調節歸化尺度、發揮中文意合優勢、兼顧音韻節奏與臺灣在地語言規範**。

**忠實性優先，適用於所有檔位。** 歸化自由度只調整表達方式，不授權增刪事實、改變邏輯關係、說話者、限定條件或語氣強度。原文的比喻、誇張和個人語氣應保留其作用，不額外強化。外部知識用於消歧，不能補寫成原文的陳述；原文未展開的資訊不自行補齊。使用者明確要求摘要、節譯或改寫時按該範圍交付，並說明處理方式。

## 依請求選擇交付與檢查範圍

聊天中的短段落翻譯直接在回覆中交付，使用者要求檔案時才寫入檔案；全文或專案翻譯沿用下述預設檔案契約。使用者明確選擇純譯文、雙語或已有目標檔案時按其要求，不重複確認。先保留數字、術語、引文與限定條件，再依文體選擇必要的潤色檢查；短文本不必逐項展示診斷過程或通讀所有參考表。研究論文、文學和正式文件的專項邊界繼續適用。

## 獨立使用與檔案輸入

本技能可獨立安裝使用，無需額外安裝其他翻譯技能。輸入為 PDF 或其他檔案時，先使用當前宿主可用的讀取工具獲取原文；保留使用者要求的正文、圖表說明、公式、引文和參考文獻。無法可靠提取的內容應說明缺口，不將翻譯任務轉交給使用者未安裝的技能。

## 交付物

預設產出**一個**英中對照 Markdown 檔案；使用者要求「全中文版」時再以程式派生。

1. **`<名稱> 翻譯(中英對照).md`** — 唯一真源。每段**英文原文作為 blockquote (`> `)**，緊跟其下是中文譯文。
2. （按需）**`<名稱> 翻譯(全中文).md`** — 脫離對照檔案**用程式機械派生**（刪除 `> ` 行），不要重打中文。
   ```bash
   python - "<名稱> 翻譯(中英對照).md" "<名稱> 翻譯(全中文).md" <<'PY'
   import sys, re
   src, dst = sys.argv[1], sys.argv[2]
   lines = open(src, encoding='utf-8').read().split('\n')
   kept = [ln for ln in lines if not ln.lstrip().startswith('>')]
   open(dst,'w',encoding='utf-8').write(re.sub(r'\n{3,}','\n\n','\n'.join(kept)).strip()+'\n')
   PY
   ```
   > 順序要求：**先執行階段 6 的標點規範化，再派生全中文版**——派生程式原樣繼承標點，不會再糾正。

若輸入只是聊天裡的一小段（幾句話），直接在回覆裡給對照即可，不必寫入檔案；成篇文章或使用者指明儲存才建立檔案。命名沿用源檔名，無工具前綴（除非使用者要求）。目標檔案已存在則更新而非新建。

## 工作流（嚴格按序；每段譯文都要走完潤色與核查）

長文、多受訪者或圖文穿插文本，依原有章節、發言者或連續小段分批翻譯並就地核查；合併後檢查全篇順序、缺段和術語一致性。程式只組裝已核查內容。短文本直接完成，不強制分塊或另建檢查檔案。

### 階段 0 — 文本分析：定歸化檔位 ★必做第一步
先回答：**這是硬文本還是軟文本？屬紐馬克哪一類？自由度大致幾分 (1–10)？** 詳見 `reference/text-analysis-and-qa.md` 小節 A。

- **偏硬（自由度 1–3）**：政治／法律／經貿／科技／契約／說明書。→ 貼原文邏輯，選詞精準，**準確 > 流暢**，少介入，術語用固定譯法。某些「西化」是為精確付出的合理代價。
- **偏中 (4–6)**：教科書／新聞／一般論述。→ 平衡，預設交際翻譯。
- **偏軟 (7–9)**：文學／散文／評論／演講／廣告／宣傳／行銷。→ 在保留原意與語氣強度的前提下，靈活調整句法、措辭與修辭呈現，追求自然與表達效果。
- **混合文本**：訪談、新聞、產業綜述按段落或資訊功能判斷；技術陳述、具體舉例與受訪者歸屬始終精確保留，口語和比喻可自然轉述。
- 別忘**文本外因素**：翻譯目的、讀者是誰。檔位一句話寫進給使用者的說明裡。

### 階段 1 — 理解 → 脫離語言外殼
先理清原文事實、實體、指代、否定、條件和語氣，再**打破英文句法外殼，以中文思維重新組織**；如思果所言「翻譯是換話，不是換字」。遇到結構糾纏的片語用「解包袱法」理清隱含語意關係（見 `reference/techniques.md`）。

### 階段 2 — 依檔位初譯
以**意合優先**起譯：能不用連接詞就不用，長句拆成流水短句，被動轉主動，去動詞名詞化（余光中批評之「進行討論」、「作出貢獻」等虛浮動詞），修飾語切忌盲目堆疊在名詞前。

### 階段 3 — 潤色診斷（核心）逐段過三張表
1. **翻譯腔／惡性西化病症** — 對照 `reference/translationese-symptoms.md`，依余光中「中文的常態與變態」逐條排查並修：形合遷移／連接詞冗餘、長前置修飾語、「的的不休」、劣質被動、動詞名詞化與虛動詞、代名詞氾濫、複數「們」字成災、習語硬譯、語意關係隱含不清。
2. **技巧庫** — 用 `reference/techniques.md` 的手段修：詞性轉換、增／減詞、分句／合句、語序移位、修飾子句轉狀語／獨立句、正反譯、被動轉化。
3. **隱喻決策** — 基本／圖像圖式隱喻（旅程、大小、冷熱…）可直譯；文化專屬隱喻揉合或捨棄；直譯≠應直譯，看檔位（`reference/text-analysis-and-qa.md` 小節 B）。

### 階段 4 — 音韻節奏打磨
朗讀出聲。實踐余光中**「白以為常，文以應變」**的語言哲學：以純淨白話為基調，融入四字結構、雙音節、「偶字易適奇字難平」、對偶排比與文言凝練。並列詞組盡量配偶字（「飛機小，便宜」→「體積不大，造價便宜」）。**但軟文本才放開；硬文本點到為止。**

### 階段 5 — 準確性質檢
對照 `reference/text-analysis-and-qa.md` 小節 C 逐項核查：擅自改結構／移焦點、漏譯情態虛詞、擅自添詞、搭配、指代、邏輯關係、專名術語數字、語域對齊。

潤色後重新讀取對應原段，執行雙向核查：

- **原文 → 譯文**：每項主張、例子、列舉、限定及圖表說明都有對應，專名與數字完整準確。
- **譯文 → 原文**：每個事實性陳述均有對應原文依據；受訪者之間、正文與圖說之間不得移接陳述，標題和署名同樣核對來源。

單獨核對情態與修飾語：可能性不能改成頻率或確定性判斷，例如表示可能的 `can` 不譯成「往往」；不無依據添加「確實」「主要」等強調或範圍限定，`only if` 的必要條件不得改成充分條件。

段落數量、英文引用存在、實體齊全和標點通過均不能替代語意核查。發現問題時定位原文與譯文中的對應位置，修復後複查受影響段落；不以「已全面檢查」的自述代替核對，不要求逐字或逐句形式上的一一對應。

### 階段 6 — 中文標點規範化 ★（機械執行，寫入檔案後必跑）
依**臺灣教育部《重訂標點符號手冊》**標準執行，中文譯文裡的標點必須是正體中文全形，不能殘留英文半形。規則：
- `, : ; ? !` → `，：；？！`；句末用 `。`；並列用頓號 `、`；破折號 `——`（佔兩字寬）；刪節號 `……`（佔兩字寬，六點置中）。
- 圓括號 `( )` → 全形 `（ ）`：**僅當緊鄰中文字元時才轉**（包裹中文內容、含人名／術語原文注，如「道德受體地位（moral patienthood）」）；純英文括注（如電影演職員 `(Harold D. Schuster, 1937)`）保持半形。
- **引號規範（臺灣標準）**：
  - **外層一律使用單引號 `「` `」`**；**內層巢狀使用雙引號 `『` `』`**。
  - 將直引號 `"` 依開閉配對轉為 `「` 與 `」`；將西式彎引號 `“` `”` 歸一為 `「` `」`；`‘` `’` 歸一為 `『` `』`。
- 書名號使用雙角引號 `《 》`（書籍、雜誌、期刊）；篇名號使用單角引號 `〈 〉`（單篇論文、篇章、詩歌、歌曲）。

**以下一律保持半形，絕不轉換：** ① 英文原文 blockquote（`> ` 行）整段不動；② Markdown 連結 `[文字](url)` 的括號與 URL；③ 中文行內嵌入的整句／片語英文，其內部標點——如被引用的英文原句 `…one nation under God, indivisible, …`；④ 數字／縮寫／版本號裡的點：`4.3`、`Word 2010`、`A.I.`、`L. M.`；⑤ 程式碼／公式塊。

判定原理：**逗號類與圓括號都只在緊鄰中文字元（前或後）時才轉**；直雙引號 `"` 與西式引號按開閉依序映射至「」與『』。寫入檔案後跑下方程式機械正規化並自檢（`殘留` 必須為 0）：

```bash
python - "<名稱> 翻譯(中英對照).md" <<'PY'
import re, sys
src = sys.argv[1]
lines = open(src, encoding='utf-8').read().split('\n')
CJK_EXTRA = set('「」『』·—…《》〈〉【】、。，！？；：（）“”‘’')
def is_cjk(c):
    if not c: return False
    o = ord(c)
    return (0x3400<=o<=0x9fff) or (0x3000<=o<=0x303f) or (0xff00<=o<=0xffef) or (c in CJK_EXTRA)

SMAP = {',':'，', ';':'；', ':':'：', '?':'？', '!':'！', '(':'（', ')':'）'}
LINK = re.compile(r'\[[^\]]*\]\([^)]*\)')

def fix(line):
    store = []
    line = LINK.sub(lambda m: store.append(m.group(0)) or '\x00%d\x00'%(len(store)-1), line)
    out = []
    sop = True   # 單引號「」交替（臺灣標準外層引號）
    dop = True   # 雙引號『』交替（臺灣標準內層引號）
    for i, c in enumerate(line):
        if c == '"' or c in '“”':
            out.append('「' if sop else '」')
            sop = not sop
        elif c in '‘’':
            out.append('『' if dop else '』')
            dop = not dop
        elif c == "'":
            # 判斷是否為英文縮寫 (如 don't, it's)，前後為英文字母則保持半形
            prev_c = line[i-1] if i > 0 else ''
            next_c = line[i+1] if i + 1 < len(line) else ''
            if prev_c.isalpha() and next_c.isalpha():
                out.append("'")
            else:
                out.append('『' if dop else '』')
                dop = not dop
        else:
            out.append(c)
    ch = out
    n = len(ch)
    for i, c in enumerate(ch):
        if c in SMAP and (is_cjk(ch[i-1] if i else '') or is_cjk(ch[i+1] if i+1<n else '')):
            ch[i] = SMAP[c]
    line = ''.join(ch)
    return re.sub(r'\x00(\d+)\x00', lambda m: store[int(m.group(1))], line)

def skip(ln):
    s = ln.strip()
    return s == '' or ln.lstrip().startswith('>') or s == '---'

res = [ln if skip(ln) else fix(ln) for ln in lines]
open(src, 'w', encoding='utf-8').write('\n'.join(res))

def residual(ln):
    t = LINK.sub('', ln)
    return sum(1 for m in re.finditer(r'[,;:?!()]', t)
               if is_cjk(t[m.start()-1] if m.start() else '') or is_cjk(t[m.start()+1] if m.start()+1<len(t) else ''))

bad = sum(residual(ln) for ln in res if not skip(ln))
print('殘留半形標點:', bad)
PY
```
聊天裡直接給的小段對照（未寫入檔案）同樣要手動套用上述規則。

### 階段 7 — 輸出英中對照
依下方格式逐段配對；成篇則寫入檔案；給使用者附一句「檔位判定 + 主要潤色取捨」。

## 英中對照輸出格式

**按段落配對**：英文原段作為 blockquote，緊接中文譯文。

```markdown
> It is a truth universally acknowledged that a single man in possession of a
> good fortune must be in want of a wife.

凡是有錢的單身漢，總想娶位太太，這是一條舉世公認的真理。
```

規則：
- 中文譯文用中文全形標點；半形標點只留給英文原文、連結、行內英文詞句、數字縮寫、程式碼（詳見階段 6，寫入檔案後用程式自檢）。
- 只清理提取噪聲（接回斷行連字號、刪除頁首頁尾頁碼），**不要**從中文反推英文。
- 標題作中文 `##`/`###`，不加 blockquote；可選地把英文標題作為上一行 blockquote。
- 不該被程式刪除、需保留進全中文版的內容（圖片、表格、標題塊）**不要** blockquote。
- 公式／程式碼用共享代碼塊，不按語言重複。

## 潤色邊界 ★（防止方法論被過度使用）

借鑑余光中、思果等名家之戒飭，務必守住兩個反模式：
- **不要過度歸化**：余光中言「翻譯是戴著鐐銬跳舞」，切莫為了追求表面的「通順流利」而抹煞原文的風格特徵、術語精度或文化標記。西化是一個**連續體**，可接受度隨文本價值變化——硬文本／科技法律文本能容納更多異化以確保邏輯無誤（見 `reference/translationese-symptoms.md` 第六節）。
- **不要堆砌四字成語**：音韻節奏是水到渠成的潤色手段而非目的，思果亦再三告誡「不可為文造情、濫用成語」，為辭害意只會顯得浮誇失真。
- 不確定「該改到多狠」時，回到階段 0 的檔位：**自由度越低，越克制**。

## 範例

輸入（英文）：

```
The confidence that the West would remain a dominant force in the 21st century is giving way to a sense of foreboding.
```

輸出（英中對照；階段 0 判定：偏硬／政論，自由度≈3，準確優先、克制音韻）：

> The confidence that the West would remain a dominant force in the 21st century is giving way to a sense of foreboding.

西方曾篤信自己將在 21 世紀穩居主導地位，如今這份自信，正讓位於一種隱隱的不祥之感。

## 致謝與理論源流

本 skill 的程式碼、提示詞與組織方式以 **MIT 許可**發布，可自由使用、修改、再分發。

本方法論之中文診斷與修辭規範，核心承襲余光中《論中文的常態與變態》《翻譯乃大道》、思果《翻譯研究》《翻譯新究》等臺灣翻譯大師之論述精華，並融合功能翻譯學派（如 Peter Newmark 文本功能理論）與當代英譯中在地化實務。標點符號標準全面遵照臺灣教育部《重訂標點符號手冊》。謹向深耕中文純淨與翻譯之道的前輩學者致敬。
