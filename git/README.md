# Git Learning Note

Git ဆိုင်ရာ ခေါင်းစဉ် ၄ ခုကို ကိုယ့်စကားနဲ့ ပြန်ရေးထားတဲ့ learning note ပါ။ command တွေကို practice repo မှာ တကယ်စမ်းကြည့်ပြီး မှတ်ထားတယ်။

**မာတိကာ**

- [1. Merge Strategies နဲ့ Merge Methods](#s1)
    - [1.0 အရင်ခွဲမှတ်ထားရမယ့်အချက်](#s1-0)
    - [1.1 Merge Strategies (၆ မျိုး)](#s1-1)
        - [1.1.1 `ort` (default)](#s1-1-1)
        - [1.1.2 `octopus`](#s1-1-2)
        - [1.1.3 `ours`](#s1-1-3)
            - [1.1.3.1 `ours` ရလဒ်က ဘယ်လိုဖြစ်လဲ](#s1-1-3-1)
            - [1.1.3.2 `ours` အမြဲ merge commit ဖြစ်တယ်](#s1-1-3-2)
            - [1.1.3.3 `ours` ⚠️ လက်တွေ့သုံးရင် သတိထားရန်](#s1-1-3-3)
            - [1.1.3.4 `-s ours` နဲ့ `-X ours` မတူဘူး](#s1-1-3-4)
        - [1.1.4 `resolve`](#s1-1-4)
        - [1.1.5 `subtree`](#s1-1-5)
            - [1.1.5.1 ပထမဆုံးအကြိမ် ထည့်ပုံ](#s1-1-5-1)
            - [1.1.5.2 အတွင်းမှာ ဘာလုပ်လဲ (command ၃ ကြောင်းနဲ့ လုပ်ရင်)](#s1-1-5-2)
            - [1.1.5.3 Update ယူပုံ](#s1-1-5-3)
        - [1.1.6 `recursive`](#s1-1-6)
    - [1.2 Merge Methods (၃ မျိုး)](#s1-2)
        - [1.2.1 Fast-forward](#s1-2-1)
        - [1.2.2 Merge commit (`--no-ff`)](#s1-2-2)
        - [1.2.3 Squash](#s1-2-3)
        - [1.2.4 Method ၃ ခု နှိုင်းယှဉ်](#s1-2-4)
    - [1.3 Merge Conflict ဖြစ်ရင်](#s1-3)
- [2. Git Branching Cheatsheet](#s2)
    - [Branch ခွဲတာနဲ့ commit လုပ်တာ ဘယ်လိုဖြစ်လဲ](#s2-1)
    - [Remote ရှိတဲ့ repo မှာ ပုံမှန်လုပ်ပုံ](#s2-2)
    - [Branch နှစ်ခု နှိုင်းယှဉ်](#s2-3)
- [3. Time Travel](#s3)
    - [အတိတ်ကို ကြည့်ရန်](#s3-1)
    - [restore / reset / revert](#s3-2)
    - [restore – ဖိုင်ကိုပဲ ပြန်ယူ (commit မရွှေ့ဘူး)](#s3-3)
    - [reset – branch tip ကို ပြန်ရွှေ့](#s3-4)
    - [revert – ပြောင်းပြန်လုပ်တဲ့ commit အသစ်ထည့်](#s3-5)
    - [reflog – reset ပြီး ပျောက်သွားသလိုဖြစ်တဲ့ commit ပြန်ရှာ](#s3-6)
    - [cherry-pick – Commit အဟောင်းထဲက changes ကို ရွေးယူ](#s3-7)
- [4. Rebase vs Merge](#s4)
    - [Rebase ကို ဘယ်အချိန်သုံးမလဲ](#s4-1)
    - [Conflict commands](#s4-2)
    - [Interactive rebase](#s4-3)
    - [Squash merge vs Interactive rebase (မရောနဲ့)](#s4-4)
    - [Push လုပ်တဲ့အခါ](#s4-5)

---

<a id="s1"></a>

## 1. Merge Strategies နဲ့ Merge Methods

<a id="s1-0"></a>

### 1.0 အရင်ခွဲမှတ်ထားရမယ့်အချက်

`git merge` မှာ မေးခွန်းနှစ်ခု ရှိတယ်။ နှစ်ခုကို မရောဖို့ အရေးကြီးတယ်။

| | မေးခွန်း | ရွေးစရာတွေ | Option |
|---|---|---|---|
| **Strategy** | ဖိုင် content ကို ဘယ်လိုတွက်ပြီး ပေါင်းမလဲ | `ort`(default), `octopus`, `ours`, `resolve`, `subtree`, `recursive` | `-s <strategy>` |
| **Method** | History (commit ချိတ်ပုံ) ကို ဘယ်လိုထားမလဲ | fast-forward, merge commit, squash | `--ff-only`, `--no-ff`, `--squash` |

နှစ်ခုကို တွဲသုံးလို့ရတယ်။<br>
e.g strategy က `ours`၊ method က merge commit။

```bash
git merge --no-ff -s ours legacy
```
---

<a id="s1-1"></a>

### 1.1 Merge Strategies (၆ မျိုး)

`git merge -s <strategy> <branch>` နဲ့ ရွေးတယ်။ မရွေးရင် `ort` ကို သုံးတယ်။

| Strategy | အလုပ်လုပ်ပုံ | အသုံးပြုသင့်သည့်အခြေအနေ |
|---|---|---|
| `ort` | common ancestor နဲ့ branch tip နှစ်ခုကို နှိုင်းယှဉ်ပြီး three-way merge လုပ်တယ်။<br>ဖိုင် rename ကိုလည်း သိတယ်။ | ပုံမှန်ပေါင်းတာ အကုန်။ အထူးအကြောင်းမရှိရင် ဒါပဲသုံး(default) |
| `octopus` | Branch အများကို တစ်ခါတည်း ပေါင်းတယ်။ conflict ရှုပ်ရင် ငြင်းတယ်။ | conflict မရှိသလောက်ဖြစ်တဲ့ branch (ဥပမာ docs, tests) အများကို တစ်ပြိုင်တည်း ပေါင်းချင်ရင်။ |
| `ours` | incoming branch ရဲ့ content ကို **လုံးဝမယူဘူး**။ ကိုယ့် tree အတိုင်းပဲထားတယ်။<br>ဒါပေမဲ့ history မှာတော့ "ပေါင်းပြီးသား" လို့ မှတ်ထားတယ်။ | branch အဟောင်းကို content မယူဘဲ "ပေါင်းပြီးပြီ" လို့ မှတ်ချင်ရင်။<br>feature ရဲ့ changes တကယ်လိုချင်ရင် မသုံးရဘူး။ |
| `resolve` | head နှစ်ခုကို three-way merge လုပ်တယ်။ `ort` နဲ့ ဆင်တူပေမဲ့ **rename ကို မသိဘူး**။ | ရိုးရှင်းသော two-head merge အတွက် |
| `subtree` | Tree တစ်ခုကို အခြား tree ၏  **subdirectory အဖြစ်** ညှိပြီး ပေါင်းတယ်။ | Subdirectory အဖြစ် ထည့်ထားသော project ကို ပေါင်းရန် |
| `recursive` | Git အဟောင်းတွေမှာ two-head merge ရဲ့ default ဖြစ်ခဲ့တယ်။<br>**Git 2.50 ကစပြီး `ort` နဲ့ synonym** (နာမည်ပဲကွာ၊ အတူတူ)။ | စာအဟောင်းတွေမှာ တွေ့ရင် `ort` လို့ပဲ မှတ်ထား။ ပုံက [1.1.1 `ort`](#s1-1-1) နဲ့ အတူတူ။ |

<a id="s1-1-1"></a>

#### 1.1.1 `ort` (default)

```bash
git switch main
git merge -s ort feature/login
```

```
မပေါင်းခင်:                         ပေါင်းပြီးနောက်:
A---B---C  main (HEAD)            A---B---C-------M  main (HEAD)
     \                                 \         /
      D---E  feature/login              D-------E  feature/login
```

M ရဲ့ parent က C နဲ့ E နှစ်ခု။ feature/login က မပြောင်းဘူး။

Three-way ဆိုတာ ဘာကို ယှဉ်တာလဲ (base = B၊ ours = C၊ theirs = E):

| ဖိုင် | B (base) | C (main) | E (feature) | M (ရလဒ်) |
|---|---|---|---|---|
| `a.txt` | v1 | v1 | **v2** | **v2** (feature ပြင်ထားတာယူ) |
| `b.txt` | v1 | **v2** | v1 | **v2** (main ပြင်ထားတာယူ) |
| `c.txt` | v1 | **v2** | **v3** | conflict (နှစ်ဘက်လုံးပြင်ထား) |

<a id="s1-1-2"></a>

#### 1.1.2 `octopus`

base A ကနေ docs branch (D) နဲ့ tests branch (T) ကို ခွဲထားတယ်။

```bash
git switch practice/octopus-base
git merge --no-ff -s octopus practice/octopus-docs practice/octopus-tests -m "Merge docs and tests together"
git show --no-patch --format="%h parents: %p" HEAD
```

```
မပေါင်းခင်:                         ပေါင်းပြီးနောက်:
  D  octopus-docs                        D (docs)
 /                                      / \
A    octopus-base (HEAD)         A-----+---M  octopus-base (HEAD)
 \                                      \ /
  T  octopus-tests                       T (tests)
```

M ရဲ့ parent က **၃ ခု** (A, D, T)။ ဒါက `--no-ff` ထည့်ထားလို့။<br> `--no-ff` မပါရင် မလိုတဲ့ ancestor parent ကို ချန်သွားနိုင်လို့ parent အရေအတွက်ကို command တစ်ခုတည်းကြည့်ပြီး မခန့်မှန်းနဲ့။

<a id="s1-1-3"></a>

#### 1.1.3 `ours`

```bash
git switch practice/ours-base
git merge -s ours practice/ours-feature -m "Keep base content"
git diff HEAD^1 HEAD
```

```
မပေါင်းခင်:                         ပေါင်းပြီးနောက်:
A---B  ours-base (HEAD)           A---B-----M  ours-base (HEAD)
 \                                 \       /
  F  ours-feature                   F-----+  ours-feature
```

<a id="s1-1-3-1"></a>

#### 1.1.3.1 `ours` ရလဒ်က ဘယ်လိုဖြစ်လဲ

B ရော F ရော `base.txt` ကို ပြင်ထားတယ်ဆိုပါစို့။

| ဖိုင် | B (ours-base) | F (ours-feature) | M (ရလဒ်) |
|---|---|---|---|
| `base.txt` | B ပြင်ထားတဲ့ version | F ပြင်ထားတဲ့ version | **B ရဲ့ version** (conflict မဖြစ်ဘူး) |
| `feature.txt` | မရှိ | ရှိ | **မရှိ** |

- M ရဲ့ ဖိုင်တွေက B နဲ့ အတူတူပဲ။ ဒါကြောင့် `git diff HEAD^1 HEAD` က ဘာမှမပြဘူး။
- F ကို **content အရ လုံးဝမကြည့်ဘူး**။ နှစ်ဘက်လုံးပြင်ထားတဲ့ ဖိုင်ဆိုရင် conflict စစ်တာမျိုးမလုပ်ဘဲ B ဘက်ကိုပဲ ယူတယ်။
- ဒါပေမဲ့ history မှာတော့ M ရဲ့ parent က B နဲ့ F နှစ်ခုဖြစ်လို့ F ကို **"ပေါင်းပြီးသား"** လို့ မှတ်တယ်။

အတိုချုပ်: `ours` ဆိုတာ **content အရ မပေါင်းဘဲ၊ history အရပဲ ပေါင်းတာ။**

<a id="s1-1-3-2"></a>

#### 1.1.3.2 `ours` အမြဲ merge commit ဖြစ်တယ်

`-s ours` က fast-forward မလုပ်ဘူး။ FF ရနိုင်တဲ့ အခြေအနေမှာတောင် merge commit အမြဲလုပ်တယ်၊ ဒါကြောင့် `--no-ff` ထည့်စရာမလိုဘူး။ ချွင်းချက်က incoming က ကိုယ့် history ထဲ ပါပြီးသားဆိုရင် `Already up to date` ပြပြီး commit မလုပ်ဘူး။

<a id="s1-1-3-3"></a>

#### 1.1.3.3 `ours` ⚠️ လက်တွေ့သုံးရင် သတိထားရန်

**1. F ရဲ့ changes အကုန်ပျောက်မယ်။** F မှာ ပြင်ထားတာ၊ ထည့်ထားတာ အကုန် M ထဲ မဝင်ဘူး။ Conflict ဖြေရှင်းရတာ ရှောင်ချင်လို့ `ours` သုံးရင် F ရဲ့ လုပ်ငန်းအားလုံး ပစ်လိုက်တာဖြစ်သွားမယ်။ မပေါင်းခင် ဘာတွေပျောက်မလဲ အရင်ကြည့်ပါ:

```bash
git log --oneline ours-base..ours-feature     # F မှာပဲရှိတဲ့ commit တွေ
git diff ours-base...ours-feature             # F ရဲ့ ပြောင်းလဲမှုတွေ
```

**2. ပစ်လိုက်တဲ့ changes တွေ နောက်ပိုင်း ပြန်မလာတော့ဘူး။** Git က F ကို "ပေါင်းပြီးသား" လို့ မှတ်ထားလို့ပါ။ ဥပမာ `ours` နဲ့ ပေါင်းပြီးနောက် feature မှာ commit အသစ် F2 ထပ်လုပ်ပြီး ပြန်ပေါင်းကြည့်ရင်:

| Commit | ဖိုင် | ပြန်ပေါင်းတဲ့အခါ |
|---|---|---|
| F1 (`ours` နဲ့ ပေါင်းတုန်းက ရှိပြီးသား) | `f1.txt` | ❌ ဤဥပမာတွင် F1 ၏ changes ကို အလိုအလျောက် ပြန်မယူဘူး |
| F2 (`ours` ပြီးမှ ထပ်လုပ်တာ) | `f2.txt` | ✅ ဝင်လာတယ် |

F1 ၏ changes ကို နောက်မှ ပြန်လိုချင်ရင် မူရင်း F1 ကို ထပ် merge လုပ်ရုံနဲ့ ပြန်မရဘူး။ ရွေးယူချင်သော commit ကို **[cherry-pick](#s3-7)** လုပ်ခြင်း သို့မဟုတ် changes ကို ကိုယ်တိုင်ပြန်အသုံးချခြင်း စသည့်နည်းလမ်းကို စဉ်းစားနိုင်တယ်။

**3. `ours` ကို ဘယ်အချိန်သုံးမလဲ**

| သုံးလို့ရတဲ့ အခြေအနေ | မသုံးသင့်တဲ့ အခြေအနေ |
|---|---|
| F ထဲက လိုချင်တာတွေကို လက်နဲ့ (သို့) cherry-pick နဲ့ B ထဲ ထည့်ပြီးသား၊ F ကို "ပေါင်းပြီးပြီ" လို့ မှတ်ချင်တဲ့အခါ | F ရဲ့ changes တွေ လိုချင်တဲ့အခါ |
| `legacy` လို branch အဟောင်းကို content ပစ်ပြီး history မှာ ပေါင်းပြီးလို့ မှတ်ချင်တဲ့အခါ | Conflict ဖြေရှင်းရတာ ရှောင်ချင်လို့ |
| | ဘာပျောက်မလဲ မသေချာသေးတဲ့အခါ |

<a id="s1-1-3-4"></a>

#### 1.1.3.4 `-s ours` နဲ့ `-X ours` မတူဘူး

| | လုပ်ပုံ |
|---|---|
| `-s ours` | incoming content အကုန်လုံးကို မယူဘူး |
| `-X ours` | `ort` နဲ့ပေါင်းပြီး **conflict ဖြစ်တဲ့နေရာမှာပဲ** ကိုယ့်ဘက်ကိုရွေး။ conflict မဖြစ်တဲ့ incoming changes ကိုတော့ ယူတယ် |

```bash
git merge -s ours legacy                  # content မယူဘူး
git merge -s ort -X ours feature/login    # conflict မှာပဲ ကိုယ့်ဘက်ရွေး
```

<a id="s1-1-4"></a>

#### 1.1.4 `resolve`

```bash
git merge -s resolve feature/login
```

ပုံ (ပေါင်းပုံက `ort` နဲ့ အတူတူ):

```
မပေါင်းခင်:                  ပေါင်းပြီးနောက်:
A---B---C  main (HEAD)     A---B---C-------M  main (HEAD)
     \                          \         /
      D---E  feature             D-------E  feature
```

⚠️ ကွာတာက rename ကိစ္စ:

| အခြေအနေ | `ort` | `resolve` |
|---|---|---|
| main က `old.txt` ကို `new.txt` လို့ rename၊ feature က `old.txt` ကို ပြင် | feature ရဲ့ပြင်ထားတာကို `new.txt` ထဲ ထည့်ပေးနိုင် | rename မသိလို့ conflict ဖြစ်နိုင် |

<a id="s1-1-5"></a>

#### 1.1.5 `subtree`

Library ကို `lib/` folder ထဲ ထည့်ပြီး၊ library ရဲ့ update တွေကို ဆက်ယူတဲ့နည်း။ အောက်ပုံက update ယူပြီးတဲ့အခါ ဘယ်လိုဖြစ်လဲ ပြတာ။

```
မပေါင်းခင်:                                         ပေါင်းပြီးနောက်:
main                     lib (သီးခြား project)      main
├── app.txt              ├── core.txt             ├── app.txt
└── lib/                 └── util.txt             └── lib/
    ├── core.txt (old)                              ├── core.txt(new)
    └── util.txt                                    └── util.txt
```

<a id="s1-1-5-1"></a>

#### 1.1.5.1 ပထမဆုံးအကြိမ် ထည့်ပုံ

```bash
git subtree add --prefix=lib <repo> <ref>    # <repo> = URL/path၊ <ref> = branch/tag
git subtree add --prefix=lib lib             # lib က local branch ဆိုရင်
```

- `lib/` အောက်မှာ ဖိုင်တွေထည့်ပြီး merge commit တစ်ခု လုပ်ပေးတယ် (parent ၂ ခု: main နဲ့ lib)။
- `lib/` ရှိပြီးသားဆိုရင် `prefix 'lib' already exists.` ဆိုပြီး ငြင်းတယ်။
- `--squash` ထည့်ရင် lib ရဲ့ history အကုန်ကို commit တစ်ခုထဲ squash လုပ်ပြီး ယူတယ်။

<a id="s1-1-5-2"></a>

#### 1.1.5.2 အတွင်းမှာ ဘာလုပ်လဲ (command ၃ ကြောင်းနဲ့ လုပ်ရင်)

`subtree add` က ဒီ ၃ ကြောင်းနဲ့ ရလဒ်တူတယ်:

```bash
git merge -s ours --no-commit --allow-unrelated-histories lib    # ၁။ history ချိတ်၊ ဖိုင်မယူသေး
git read-tree --prefix=lib/ -u lib                                # ၂။ ဖိုင်တွေကို lib/ အောက်မှာ ထည့်
git commit -m "Add lib as subtree"                                # ၃။ commit
```

၁ ကြောင်းမှာပါတဲ့ option တွေ:

| Option | ဘာအတွက်လဲ | မပါရင် |
|---|---|---|
| `--allow-unrelated-histories` | Common ancestor မရှိတဲ့ history နှစ်ခု ပေါင်းခွင့်ပြု | `refusing to merge unrelated histories` |
| `-s ours` | `lib` ရဲ့ ဖိုင်တွေကို root မှာ မယူဘူး (history ပဲ ချိတ်) | `core.txt`, `util.txt` က root ထဲ ဝင်လာ |
| `--no-commit` | ၂ ကြောင်းမလုပ်ခင် commit မလုပ်ဘဲ ရပ်ထား | ဖိုင်မပါတဲ့ merge commit ဖြစ်ပြီး commit ၂ ခုဖြစ်တယ် |

၂ ကြောင်းမှာ `--prefix=lib/` ရဲ့ နောက်ဆုံး `/` ပါရမယ်၊ `-u` က working tree ထဲမှာလည်း ဖိုင်ထုတ်ပေးတယ်။ `lib/` ရှိပြီးသားဆိုရင် `Cannot bind` error ဖြစ်တယ်။

ဒီ merge commit ရဲ့ history ချိတ်မှုကြောင့်ပဲ နောက်ပိုင်း update ယူလို့ရတာ။

<a id="s1-1-5-3"></a>

#### 1.1.5.3 Update ယူပုံ

```bash
git subtree pull --prefix=lib <repo> <ref>    # တစ်ကြောင်းတည်း

# (သို့) fetch ပြီး merge
git fetch <repo> <ref>
git merge -s subtree FETCH_HEAD
```

`lib` က local branch ဆိုရင် `git merge -s subtree lib`။ Folder ကို Git က အလိုအလျောက် ခန့်မှန်းတယ်၊ မှားရင် `-X subtree=lib` နဲ့ ကိုယ်တိုင်ပြောပေးလို့ရတယ်။

```bash
git merge -s subtree -X subtree=lib lib
```

> `git subtree` က Git ရဲ့ contrib ထဲက command မို့ install အလိုက် မပါနိုင်ဘူး။ ကိုယ့်စက်မှာ `git subtree` လို့ run ကြည့်ပြီး စစ်ပါ။ အပေါ်က command အားလုံးကို Git 2.34.1 + local path နဲ့ စမ်းထားတယ် (`--squash` ပါ ပါတယ်)။

<a id="s1-1-6"></a>

#### 1.1.6 `recursive`

Git 2.50 ကစပြီး `ort` နဲ့ **အတူတူ** (နာမည်ပဲကွာတယ်)။ ဒါကြောင့် ပေါင်းပုံ၊ မပေါင်းခင်/ပေါင်းပြီးနောက် ပုံ၊ ဖိုင်ရလဒ် အားလုံးအတွက် [1.1.1 `ort`](#s1-1-1) ကို ကြည့်ပါ။

```bash
git merge -s recursive feature/login
```

---

<a id="s1-2"></a>

### 1.2 Merge Methods (၃ မျိုး)

Strategy က content တွက်ပုံ၊ method က **history ထဲမှာ ဘယ်လိုကျန်မလဲ** ဆိုတာ။ Method ၃ မျိုးစလုံးကို အတူတူ ပုံနဲ့ ကြည့်မယ်။

| Method | အလုပ်လုပ်ပုံ | Command |
|---|---|---|
| Fast-forward | Feature ထဲမှာ main ရဲ့ လက်ရှိ history အားလုံး ပါပြီးသားဆိုရင် main ကို feature ရဲ့ နောက်ဆုံး commit အထိ update လုပ်တယ်။ Feature commits တွေ main history ထဲ ပါလာပြီး ဖိုင်တွေလည်း အဲဒီ commit ရဲ့အခြေအနေကို ပြောင်းတယ်။ **Merge commit အသစ်မလိုဘူး**။ | `git merge --ff-only <branch>`  (FF မရရင် ငြင်းတယ်) |
| Merge commit | Branch များ၏ history ကို merge commit ဖြင့် ချိတ်သည်။ FF ဖြစ်နိုင်သည့်အခါလည်း `--no-ff` ဖြင့် merge commit ဖန်တီးနိုင်သည်။ | `git merge --no-ff <branch>` |
| Squash | feature ရဲ့ changes အားလုံးကို staging ထဲ စုထည့်ပေးတယ်။ နောက်မှ ကိုယ်တိုင် ordinary commit `git commit` တစ်ခါလုပ်ရတယ်။ **Merge commit မဟုတ်ဘူး၊ ရိုးရိုး commit တစ်ခု။** | `git merge --squash <branch>` ပြီး `git commit` |


<a id="s1-2-1"></a>

#### 1.2.1 Fast-forward

Main ကနေ feature ခွဲပြီး feature မှာပဲ အလုပ်ဆက်လုပ်ထားတယ်၊ main မှာ သီးခြား commit အသစ် မတိုးထားဘူးဆိုပါစို့။ Feature ရဲ့ history ထဲမှာ main ရဲ့ လက်ရှိ commits အားလုံး ပါပြီးသားဖြစ်နေတယ်။

```text
မပေါင်းခင်:
A---B---C---D  feature/login
    ↑
    main (HEAD)

B = notes.txt ထဲမှာ မူလစာတစ်ကြောင်းရှိ
C = notes.txt ထဲ စာတစ်ကြောင်းထပ်ထည့်
D = login.txt ဖိုင်အသစ်ဖန်တီး
```

Main ပေါ်မှာနေပြီး ပေါင်းမယ်:

```bash
git switch main
git merge --ff-only feature/login
```

လုပ်ပြီးနောက်:

```text
A---B---C---D  main (HEAD), feature/login
```

အလုပ်လုပ်ပုံကို အဆင့်လိုက်ကြည့်ရင်:

1. **Git က history ကိုစစ်တယ်။** D ကနေ parent တွေအတိုင်း နောက်ပြန်လိုက်ရင် C၊ B ကိုတွေ့တယ်။ ဒါကြောင့် main ရဲ့အလုပ်က feature ထဲမှာ ပါပြီးသားဖြစ်ပြီး သီးခြား history နှစ်ခုကို ပြန်ပေါင်းတွက်ရန် မလိုဘူး။
2. **Main ကို D အထိ update လုပ်တယ်။** အရင် main history က A၊ B ဖြစ်ပေမဲ့ အခု A၊ B၊ C၊ D ဖြစ်လာတယ်။ C နဲ့ D က ရှိပြီးသား commits ဖြစ်ပြီး hashes မပြောင်းဘူး။
3. **လက်ရှိဖိုင်တွေကိုလည်း update လုပ်တယ်။** ဒီလို clean working tree နဲ့ စမ်းထားသော ဥပမာမှာ `notes.txt` က စာနှစ်ကြောင်းဖြစ်လာပြီး `login.txt` လည်း main ပေါ်မှာ မြင်ရလာမယ်။ Main နဲ့ feature နှစ်ခုလုံးက D ကိုညွှန်လို့ tracked ဖိုင်အခြေအနေ တူသွားတယ်။

ဒါကြောင့် **“pointer ရွှေ့တယ်” ဆိုတာ main က ဘယ် commit အထိ ရောက်နေပြီလဲဆိုတဲ့ အမှတ်အသားကို B မှ D သို့ ပြောင်းတာ** ပါ။ နာမည်အမှတ်အသားသာ ပြောင်းပြီး ဖိုင်တွေ မပြောင်းဘူးလို့ မဆိုလိုပါ။

**Commit အသစ်မရှိဘူးဆိုတာလည်း main မှာ commits ထပ်မပါလာဘူးလို့ မဆိုလိုပါ။** Feature မှာရှိပြီးသား C နဲ့ D တို့ main history ထဲ ပါလာတယ်။ ပေါင်းရန်အတွက် M ဆိုသော merge commit အသစ်ကိုသာ မဖန်တီးတာပါ။ Feature branch ကလည်း D မှာ ဆက်ရှိနေပြီး အလိုအလျောက် မဖျက်ပါ။

**FF ဘယ်အချိန်ရလဲ:** commit အချိန်၊ အရေအတွက်နဲ့ မဆိုင်ဘူး။ **ကိုယ့် branch ရဲ့ tip က incoming ရဲ့ history ထဲမှာ ပါနေရင်** ရတယ်။

ရတဲ့ပုံ (main = B က feature ရဲ့ history ထဲမှာ ပါတယ်):

```
A---B---C  feature
    ↑
    main
```

မရတဲ့ပုံ (main မှာ D ရှိပြီး feature ရဲ့ history ထဲမှာ D မပါဘူး):

```
      C  feature
     /
A---B---D  main (HEAD)
```

မရတဲ့အခါ `--ff-only` က ငြင်းတယ်၊ `git merge feature` က merge commit (1.2.2) လုပ်ပေးတယ်။ Incoming commits က ကိုယ့် history ထဲ ပါပြီးသားဆိုရင်တော့ `Already up to date` လို့ပဲပြပြီး ဘာမှမလုပ်ဘူး။

<a id="s1-2-2"></a>

#### 1.2.2 Merge commit (`--no-ff`)

```
မပေါင်းခင် (1.2.1 နဲ့ အတူတူ၊ FF ရနိုင်တဲ့ပုံ):          ပေါင်းပြီးနောက်:
A---B  main (HEAD)                            A---B-----------M  main (HEAD)
     \                                             \         /
      C---D  feature/login                          C-------D  feature/login
```

- M ရဲ့ parent က B နဲ့ D၊ branch ခွဲပြီး ပြန်ပေါင်းခဲ့တာ history မှာ မြင်ရတယ်။
- `--no-ff` မသုံးဘဲ main ပါ ရှေ့ရောက်နေရင် (diverged) `git merge feature` လုပ်ရင်လည်း ဒီပုံပဲ ရတယ် (1.1.1 ရဲ့ ပုံ)။

<a id="s1-2-3"></a>

#### 1.2.3 Squash

```bash
git merge --squash feature/login
git commit -m "Add complete feature"
```

```
မပေါင်းခင်:                         ပေါင်းပြီးနောက်:
A---B---C  main (HEAD)            A---B---C---S  main (HEAD)
     \                                 \
      D---E  feature/login              D---E  feature/login
```

- S က D နဲ့ E ရဲ့ changes ကို စုထားတဲ့ commit တစ်ခု။ **Parent က C တစ်ခုတည်း**။
- E နဲ့ S ကြားမှာ မျဉ်းမရှိဘူး၊ feature ရဲ့ မူရင်း commit တွေက မပျက်ဘဲ main history ထဲမှာ ancestry အဖြစ် မချိတ်ထားဘူး။
- ဒါကြောင့် squash ပြီးရင် အဲ့ feature branch ကနေ ဆက်မလုပ်ဘဲ updated main ကနေ branch အသစ်ခွဲတာ ပိုရှင်းတယ်။

<a id="s1-2-4"></a>

#### 1.2.4 Method ၃ ခု နှိုင်းယှဉ်

| | Fast-forward | Merge commit | Squash |
|---|---|---|---|
| Option | `--ff-only` | `--no-ff` | `--squash` |
| Commit အသစ် | မရှိ | ရှိ (M) | ရှိ (S၊ ကိုယ်တိုင် commit လုပ်ရ) |
| Parent | – | ၂ ခု | ၁ ခု |
| Feature commits | main history ထဲ ဝင်လာ | main history ထဲ ဝင်လာ | main history ထဲ မဝင်ဘူး |
| History | တစ်ကြောင်းတည်း | ကွဲပြီးပြန်ပေါင်းတာ မြင်ရ | တစ်ကြောင်းတည်း၊ commit တစ်ခုတည်းအဖြစ် |
| သုံးရင်ကောင်းတာ | feature ရဲ့ commit တွေကို ဒီအတိုင်းထားချင်၊ history ရှင်းချင် | branch ပေါင်းခဲ့တာကို မှတ်တမ်းထားချင် | feature တစ်ခုလုံးကို commit တစ်ခုတည်းနဲ့ မှတ်ချင် |

---

<a id="s1-3"></a>

### 1.3 Merge Conflict ဖြစ်ရင်

Merge မလုပ်ခင် uncommitted changes ကို commit (သို့) stash လုပ်ထား။

```bash
git merge feature/login
git status
```

Conflict ဖြစ်တဲ့ဖိုင်ထဲမှာ ဒီလိုပေါ်လာမယ်:

```
<<<<<<< HEAD
လက်ရှိ branch ရဲ့ စာသား
=======
incoming branch ရဲ့ စာသား
>>>>>>> feature/login
```

ပုံမှန် merge မှာ `HEAD` (ours/current) က ကိုယ့် branch၊ incoming (theirs) က ပေါင်းလာတဲ့ branch။ <br>တစ်ဘက်ပဲရွေးရတာမဟုတ်ဘူး၊ နှစ်ဘက်ပေါင်းပြီး စာသားအသစ်ရေးလည်းရတယ်။ marker တွေ ဖယ်ပြီးရင်:

```bash
git add app.txt
# tests run ကြည့်
git merge --continue
```

- ပြန်ဖျက်ချင်ရင် `git merge --abort`
- Squash conflict ကတော့ ဖြေရှင်းပြီး `git add` + `git commit` ပဲ။ Squash က merge state မသိမ်းလို့ `--continue` / `--abort` မရဘူး။

---

<a id="s2"></a>

## 2. Git Branching Cheatsheet

Branch ဆိုတာ commit တစ်ခုကို ညွှန်တဲ့ pointer တစ်ခုပဲ။ ဒါကြောင့် ခွဲရတာ အရမ်းမြန်တယ်။

| လုပ်ချင်တာ | Command |
|---|---|
| local branch စာရင်း | `git branch` |
| remote-tracking branch စာရင်း | `git branch -r` |
| အကုန်ကြည့် | `git branch -a` |
| upstream တွေပါကြည့် | `git branch -vv` |
| branch ဆောက်ရုံ | `git branch feature/login` |
| ဆောက်ပြီး ကူးမယ် | `git switch -c feature/login` |
| branch ကူး | `git switch main` |
| ရှေ့ branch ပြန်သွား | `git switch -` |
| လက်ရှိ branch နာမည်ပြောင်း | `git branch -m feature/signin` |
| ဖျက် (merge ပြီးမှ) | `git branch -d feature/login` |
| အတင်းဖျက် | `git branch -D feature/login` |
| remote ရဲ့ နောက်ဆုံးအခြေအနေယူ | `git fetch origin` |
| remote branch ကို track လုပ်ပြီး ကူး | `git switch -c feature/login --track origin/feature/login` |
| push လုပ်ပြီး upstream ချိတ် | `git push -u origin feature/login` |
| remote branch ဖျက် | `git push origin --delete feature/login` |

`origin/main` က နောက်ဆုံး fetch လုပ်ခဲ့တုန်းက remote main ဘယ်လိုရှိလဲဆိုတာ local မှာမှတ်ထားတာ။ remote server ရဲ့ အခုလက်ရှိအခြေအနေ မဟုတ်ဘူး။

<a id="s2-1"></a>

### Branch ခွဲတာနဲ့ commit လုပ်တာ ဘယ်လိုဖြစ်လဲ

Branch က commit တစ်ခုကို ညွှန်တဲ့ label၊ `(HEAD)` က အခု ကိုယ်ရှိနေတဲ့ branch။ ဒီ ၂ ခုကို မှတ်ထားရင် အောက်ကပုံတွေ ရှင်းသွားမယ်။

**အဆင့် ၁။ Branch ခွဲရုံ** (`git switch -c feature`)

```
A  ← main, feature (HEAD)
```

- Commit က A တစ်ခုတည်း၊ commit အသစ် မဖြစ်သေးဘူး။
- `main` နဲ့ `feature` ဆိုတဲ့ label ၂ ခုက A ကိုပဲ ညွှန်နေတယ်။
- HEAD က `feature` ပေါ်မှာ။

**အဆင့် ၂။ `feature` မှာ ဖိုင်ပြင်ပြီး commit** (`git commit`)

```
A---B  ← feature (HEAD)
↑
main
```

- Commit B အသစ် ဖြစ်လာတယ် (parent က A)။
- ကိုယ်ရှိနေတဲ့ `feature` ရဲ့ label ပဲ B ဆီ ရွှေ့တယ်။
- `main` က A မှာပဲ ကျန်တယ်။

> Commit လုပ်ရင် **လက်ရှိ branch ရဲ့ label တစ်ခုပဲ** ရှေ့ရွှေ့တယ်။

**အဆင့် ၃။ `main` ကို ပြန်ကူး** (`git switch main`)

```
A---B  ← feature
↑
main (HEAD)
```

- HEAD ပဲ `main` ဆီ ပြန်ရွှေ့တယ်။ Commit တွေ၊ label တွေ မရွှေ့ဘူး။
- ဖိုင်တွေက A ရဲ့ အခြေအနေအတိုင်း ပြန်ဖြစ်တယ်။
- B မှာ သိမ်းထားတာ မပျောက်ဘူး၊ `feature` ပေါ်မှာ ရှိနေတုန်းပဲ။ `git switch feature` နဲ့ ပြန်မြင်ရမယ်။

>⚠️<br>
>Commit မလုပ်ရသေးတဲ့ ပြင်ဆင်မှု (uncommitted changes) တွေကိုတော့ branch အလိုက် သီးသန့်မသိမ်းပေးဘူး၊ <br>
>ကိုယ်နဲ့အတူ ပါလာတတ်တယ်။ branch မပြောင်းခင် commit (သို့) stash လုပ်ထားပါ။

<a id="s2-2"></a>

### Remote ရှိတဲ့ repo မှာ ပုံမှန်လုပ်ပုံ

```bash
git switch main
git fetch origin
git merge --ff-only origin/main
git switch -c feature/login

# app.txt ပြင်ပြီး
git add app.txt
git commit -m "Add login feature"
git push -u origin feature/login
```

`main` နဲ့ `origin/main` ကွဲနေရင် `--ff-only` က ငြင်းမယ်။ အဲ့ဒါဆို history ကို အရင်ကြည့်။

<a id="s2-3"></a>

### Branch နှစ်ခု နှိုင်းယှဉ်

```bash
git log --oneline main..feature/login     # feature မှာပဲရှိတဲ့ commit တွေ
git diff main feature/login               # tip နှစ်ခုကြား ကွာခြားချက်
git diff main...feature/login             # common ancestor ကနေ feature tip အထိ
git log --oneline --graph --decorate --all
```

>⚠️<br>
>`-D` က unmerged work ပျောက်စေနိုင်တယ်။ `-d` က merge ပြီးမပြီး စစ်ပေးတယ်။<br>
>ကိုယ်ရှိနေတဲ့ branch ကိုတော့ ဖျက်လို့မရဘူး။<br>
>remote branch မဖျက်ခင် တခြားသူသုံးနေလားစစ်။

---

<a id="s3"></a>

## 3. Time Travel

Git မှာ time travel ဆိုတာ အတိတ် commit ကြည့်တာ၊ ဖိုင်အဟောင်းပြန်ယူတာ၊ changes ပြန်ဖျက်တာ၊ ပျောက်သွားတဲ့ commit ပြန်ရှာတာ။<br> `<COMMIT_HASH>` / `<BAD_COMMIT>` နေရာမှာ ကိုယ့် history ထဲက hash ထည့်ရမယ်။

<a id="s3-1"></a>

### အတိတ်ကို ကြည့်ရန်

| လုပ်ချင်တာ | Command |
|---|---|
| commit စာရင်း | `git log --oneline` |
| commit တစ်ခုရဲ့ အသေးစိတ် | `git show <COMMIT_HASH>` |
| အဲ့ commit ထဲက ဖိုင်ကြည့် | `git show <COMMIT_HASH>:app.txt` |
| နောက်ဆုံး commit ရဲ့ ပြောင်းလဲမှု | `git diff HEAD~1 HEAD` |
| အတိတ် commit ကို ခဏသွားကြည့် | `git switch --detach <COMMIT_HASH>` |
| main ပြန်သွား | `git switch main` |
| detached HEAD ကနေ work သိမ်း | `git switch -c experiment/old-version` |

- `HEAD~1` = ၁ ဆင့်နောက်၊ `HEAD~2` = ၂ ဆင့်နောက်၊ `HEAD^2` = merge commit ရဲ့ second parent။
- Detached HEAD က error မဟုတ်ဘူး၊ `main` မရွှေ့ဘဲ HEAD ပဲ အတိတ်ကို ရွှေ့တာ။ အဲ့မှာ commit လုပ်ထားရင် ထွက်ခင် branch ဆောက်ပြီး သိမ်းပါ။

<a id="s3-2"></a>

### restore / reset / revert

| Command | ဘာလုပ်လဲ | History ကို ထိလား |
|---|---|---|
| `restore` | ဖိုင်အခြေအနေကို working tree / index ထဲ ပြန်ယူ | branch tip မရွှေ့ဘူး |
| `reset` | branch tip ကို target ဆီရွှေ့၊ mode အလိုက် index / working tree ပါပြောင်း | local history ပြန်ရေးသလိုဖြစ်တယ် |
| `revert` | ပြောင်းပြန်လုပ်တဲ့ commit အသစ်ထည့် | မူရင်း commit တွေ ဆက်ရှိတယ် |

အောက်က ဇယားတွေမှာ ဖိုင်တစ်ခု (`app.txt`) ရဲ့ နေရာ ၃ ခုကို ကြည့်မယ်:

- **HEAD** = branch tip က ညွှန်တဲ့ commit ထဲက ဖိုင်
- **Staging** = `git add` လုပ်ထားတဲ့ ဖိုင်
- **Working** = ကိုယ့် folder ထဲက ဖိုင်အစစ်

<a id="s3-3"></a>

### restore – ဖိုင်ကိုပဲ ပြန်ယူ (commit မရွှေ့ဘူး)

အခြေအနေ: HEAD ရဲ့ `app.txt` = v2။

**၁။ `git restore app.txt`** – ပြင်ထားတာ (unstaged) စွန့်ပစ်

| | မလုပ်ခင် | လုပ်ပြီး |
|---|---|---|
| HEAD | v2 | v2 |
| Staging | v2 | v2 |
| Working | **v3** (ပြင်ထား) | **v2** |

⚠️ ပြင်ထားတာ ပြန်မရတော့ဘူး။

**၂။ `git restore --staged app.txt`** – `git add` လုပ်ထားတာ ပြန်ဖြုတ် (unstage)

| | မလုပ်ခင် | လုပ်ပြီး |
|---|---|---|
| HEAD | v2 | v2 |
| Staging | **v3** (add ထား) | **v2** |
| Working | v3 | v3 (ဖိုင်မပြောင်း) |

**၃။ `git restore --source=<COMMIT_HASH> -- app.txt`** – commit အဟောင်းထဲက ဖိုင်ပြန်ယူ

| | မလုပ်ခင် | လုပ်ပြီး |
|---|---|---|
| HEAD | v2 | v2 |
| Staging | v2 | v2 |
| Working | v2 | **v1b** (အဟောင်းက version) |

Working ထဲပဲ ပြောင်းတာမို့ ထိန်းထားချင်ရင် `git add` + `git commit` ဆက်လုပ်ပါ။

<a id="s3-4"></a>

### reset – branch tip ကို ပြန်ရွှေ့

၃ မျိုးစလုံးမှာ branch tip ရွှေ့ပုံက အတူတူ (HEAD~1 = ၁ ဆင့်နောက်):

```
မလုပ်ခင်:
A---B---C  main (HEAD)

reset လုပ်ပြီး:
A---B  main (HEAD)
     \
      C  (main က မညွှန်တော့)
```

ကွာတာက Staging နဲ့ Working ပါ လိုက်ပြောင်းလား မပြောင်းလား။ အောက်ပါ ဇယားတွေမှာ `= C` ဆိုတာ C commit ထဲက ဖိုင်အတိုင်း၊ `= B` ဆိုတာ B commit ထဲက ဖိုင်အတိုင်း။

**`git reset --soft HEAD~1`** – commit ပဲဖြုတ်၊ changes က staged အတိုင်းကျန်

| | မလုပ်ခင် | လုပ်ပြီး |
|---|---|---|
| HEAD | C | **B** |
| Staging | = C | = C |
| Working | = C | = C |
| အခြေအနေ | clean | C ရဲ့ changes က staged (commit ပြန်လုပ်ဖို့ အဆင်သင့်) |

**`git reset --mixed HEAD~1`** (default) – commit ဖြုတ်၊ changes က working tree မှာကျန် (staged မဟုတ်ဘူး)

| | မလုပ်ခင် | လုပ်ပြီး |
|---|---|---|
| HEAD | C | **B** |
| Staging | = C | **= B** |
| Working | = C | = C |
| အခြေအနေ | clean | C ရဲ့ changes က unstaged (ဖိုင်ထဲမှာ ကျန်) |

**`git reset --hard HEAD~1`** (သို့) `git reset --hard <COMMIT_HASH>` – ဖိုင်ပါ B အတိုင်းပြန်

| | မလုပ်ခင် | လုပ်ပြီး |
|---|---|---|
| HEAD | C | **B** |
| Staging | = C | **= B** |
| Working | = C | **= B** |
| အခြေအနေ | clean | clean (C ရဲ့ changes ပျောက်) |

- အရင် `git branch backup/before-reset` နဲ့ backup branch ထားပါ။ ၃ မျိုးကို ဆက်တိုက်မ run နဲ့၊ တစ်ခုရွေးသုံးပါ။
- ⚠️ `--hard` က tracked changes အကုန်ပယ်တယ်၊ ပြန်ရေးမယ့်နေရာမှာ untracked ဖိုင်ရှိရင်လည်း ဖျက်နိုင်တယ်။ Backup branch က commit မလုပ်ရသေးတဲ့ edit တွေကို မသိမ်းပေးဘူး။

<a id="s3-5"></a>

### revert – ပြောင်းပြန်လုပ်တဲ့ commit အသစ်ထည့်

Shared branch မှာ commit တစ်ခု (B) ကို ပြန်ဖျက်ချင်ရင် သုံးတယ်။ ဥပမာ commit ၃ ခုက ဒီလိုပြင်ထားတယ်ဆိုပါစို့:

| Commit | ပြင်ခဲ့တာ |
|---|---|
| A | `app.txt` = `v1`၊ `other.txt` = `x` (စစချင်း) |
| B | `app.txt` ကို `v1` → `v2` ပြင် |
| C | `other.txt` ကို `x` → `y` ပြင် |

B ကို ပြန်ဖျက်ချင်တယ် (C ကိုတော့ မထိချင်ဘူး):

```bash
git revert <BAD_COMMIT>      # <BAD_COMMIT> = B ရဲ့ hash
```

```
မလုပ်ခင်:
A---B---C  main (HEAD)

revert လုပ်ပြီး:
A---B---C---R  main (HEAD)      R = "Revert B" commit အသစ်
```

ဖိုင်ထဲက တန်ဖိုးတွေ:

| | မလုပ်ခင် | လုပ်ပြီး |
|---|---|---|
| Commit စာရင်း | A, B, C | A, B, C, **R** (`Revert "B"`) |
| `app.txt` (B က ပြင်ခဲ့) | `v2` | **`v1`** (B ပြင်တာ ပြန်ဖျက်ခံရ) |
| `other.txt` (C က ပြင်ခဲ့) | `y` | `y` (မထိဘူး) |

- B နဲ့ C မူရင်းတွေ မပျောက်ဘူး၊ R ဆိုတဲ့ commit အသစ်တစ်ခု တိုးလာတာ။
- Conflict ဖြစ်ရင် ဖြေပြီး `git add` → `git revert --continue` (မလုပ်တော့ရင် `--abort`)။
- Merge commit ကို revert လုပ်ရင် `-m` နဲ့ parent ရွေးရတယ်။

<a id="s3-6"></a>

### reflog – reset ပြီး ပျောက်သွားသလိုဖြစ်တဲ့ commit ပြန်ရှာ

`reflog` = HEAD ရွှေ့ခဲ့တဲ့ မှတ်တမ်း (local မှာပဲ ရှိတယ်)။ `reset --hard` လုပ်ပြီး commit C က `git log` ထဲ မပေါ်တော့ရင်တောင် reflog မှာ hash ကျန်သေးလို့ ပြန်ယူလို့ရတယ်။

အခြေအနေ: `A---B---C` မှာ `git reset --hard HEAD~1` လုပ်လိုက်လို့ C ပျောက်သွားတယ်။

**၁။ reflog ကြည့်** – `git reflog`

```
12377ad HEAD@{0}: reset: moving to HEAD~1     <- အခု (B)
aff9d62 HEAD@{1}: commit: C                   <- ပျောက်သွားတဲ့ C ဒီမှာရှိ
12377ad HEAD@{2}: commit: B
cd3b44a HEAD@{3}: commit (initial): A
```

`git log --oneline` မှာတော့ B, A ပဲ ပေါ်တယ် (C မပေါ်တော့ဘူး)။

**၂။ ပျောက်သွားတဲ့ commit ကို branch နဲ့ ပြန်ချိတ်** – `<COMMIT_HASH>` နေရာမှာ အပေါ်က C ရဲ့ hash (`aff9d62`) ထည့်

```bash
git branch rescue/lost-work <COMMIT_HASH>
```

```
မလုပ်ခင်:
A---B  main (HEAD)
     \
      C  (ဘယ် branch ကမှ မညွှန်ဘူး၊ reflog မှာပဲ ရှိ)

လုပ်ပြီး:
A---B  main (HEAD)
     \
      C  rescue/lost-work     <- C ပြန်ရပြီ
```

**၃။ main ကို C အထိ ပြန်တင်ချင်ရင်** (မတင်ချင်ရင် ဒီအဆင့် မလိုဘူး၊ `rescue/lost-work` ကို ကြည့်ပြီး ရပြီ)

```bash
git switch main
git reset --hard rescue/lost-work
```

`reset` က **ကိုယ်ရောက်နေတဲ့ branch (HEAD ညွှန်တဲ့ branch)** ကိုပဲ ရွှေ့တာ။ `rescue/lost-work` ဆိုတာ "ဘယ်ကိုရွှေ့မလဲ" ဆိုတဲ့ target (C) ကို ညွှန်တဲ့ နာမည်ပဲ။ ဒါကြောင့် `main` ပေါ်မှာ ရှိနေမှ main ရွှေ့တယ်။ `rescue/lost-work` ပေါ်မှာ ရှိနေရင်တော့ မရွှေ့ဘူး (ဘာမှမပြောင်းဘူး)။

```
မလုပ်ခင်:                       လုပ်ပြီး:
A---B  main (HEAD)            A---B---C  main (HEAD), rescue/lost-work     <- main က C ဆီရွှေ့
     \
      C  rescue/lost-work
```

`reset --hard` မသုံးချင်ရင် `git switch main` ပြီး `git merge --ff-only rescue/lost-work` လည်း ရတယ် (main ကို C ဆီ တိုးရုံပဲ၊ ဖိုင်ပျောက်စရာ မရှိ)။

- Reflog က ကိုယ့် machine ပေါ်မှာပဲ ရှိပြီး (clone/push နဲ့ မလိုက်ဘူး) မှတ်တမ်းဟောင်းတွေ ကြာရင် expire ဖြစ်တယ် (default ရက် ၃၀–၉၀)။
- Commit မလုပ်ရသေးတဲ့ edit တွေက reflog မှာ မရှိလို့ `reset --hard` လုပ်ပြီးရင် ပြန်မရဘူး။

<a id="s3-7"></a>

### cherry-pick – Commit အဟောင်းထဲက changes ကို ရွေးယူ

`restore` က ဖိုင် version ပြန်ယူတာ၊ `reflog` က commit ပြန်ရှာတာဖြစ်ပြီး `cherry-pick` က **ရှိပြီးသား commit တစ်ခု၏ changes ကို လက်ရှိ branch ပေါ် ပြန်အသုံးချတာ** ပါ။ ဒါကြောင့် commit အဟောင်းက လိုချင်သည့်ပြောင်းလဲမှုကို ရွေးယူသော Time Travel ဆက်စပ်အကြောင်းအရာအဖြစ် ဒီနေရာမှာ ထည့်ထားတယ်။ Merge strategy တစ်ခုတော့ မဟုတ်ပါ။

**ဘယ်အချိန်သုံးမလဲ?**

- Feature branch တစ်ခုလုံးမယူဘဲ bug fix commit တစ်ခုကိုသာ ယူချင်တဲ့အခါ။
- Development branch မှ fix တစ်ခုကို release branch အဟောင်းသို့ ပြန်ထည့်ချင်တဲ့အခါ (backport)။
- `ours` နဲ့ history ပေါင်းထားပေမဲ့ မယူခဲ့သော change တစ်ခုကို နောက်မှ ရွေးပြန်ယူချင်တဲ့အခါ။

```text
မလုပ်ခင်:                       cherry-pick Y လုပ်ပြီး:
A---B---C  main (HEAD)        A---B---C---Y′  main (HEAD)
 \                             \
  X---Y---Z  feature            X---Y---Z     feature (မပြောင်း)
```

Y′ သည် Y ၏ changes ကို ပြန်အသုံးချထားသော commit အသစ်ဖြစ်ပြီး ဤဥပမာတွင် parent က C ဖြစ်တယ်။ Main ကို Y နဲ့ ancestry အဖြစ် မချိတ်ပါ။ **X နှင့် Z ကို အလိုအလျောက် မယူပါ။** Y က X ၏ code ကို မှီခိုထားလျှင် conflict ဖြစ်ခြင်း သို့မဟုတ် code အလုပ်မလုပ်ခြင်း ဖြစ်နိုင်လို့ dependency နဲ့ tests ကိုစစ်ပါ။

```bash
# မစမီ clean ဖြစ်ကြောင်းစစ်ပြီး destination branch သို့ပြောင်း
git status
git switch main

# ယူချင်သော commit ကို ရှာပြီး changes စစ်
git log --oneline feature
git show <COMMIT_HASH>

# COMMIT_HASH ကို မိမိရွေးထားသော hash ဖြင့် အစားထိုး
git cherry-pick <COMMIT_HASH>
```

Commit တစ်ခုထက်ပိုယူချင်လျှင် `git cherry-pick <COMMIT_HASH1> <COMMIT_HASH2>` ဖြင့် အစီအစဉ်လိုက်ယူနိုင်တယ်။ ပုံမှန်အားဖြင့် commit တစ်ခုစီအတွက် commit အသစ်တစ်ခုစီ ဖန်တီးပေးတယ်၊ အားလုံးကို commit တစ်ခုတည်းအဖြစ် မပေါင်းပါ။

**Conflict ဖြစ်လျှင်:** markers များဖယ်ပြီး လိုချင်သောရလဒ်ကိုပြင်၊ ဖိုင်သိမ်းပြီး:

```bash
git add app.txt
git cherry-pick --continue
```

ပယ်ဖျက်ရန် `git cherry-pick --abort` သုံးပါ။ `app.txt` ကို တကယ် conflict ဖြစ်သောဖိုင်အမည်ဖြင့် အစားထိုးပါ။ ပြီးလျှင် `git status`၊ `git log -1 --oneline` နှင့် သက်ဆိုင်ရာ tests ဖြင့် စစ်ပါ။

---

<a id="s4"></a>

## 4. Rebase vs Merge

Merge က branch history နှစ်ခုကို ချိတ်ပေးတယ်။ Rebase က feature ရဲ့ changes တွေကို base အသစ်ပေါ်မှာ ပြန်အသုံးချတယ်။

```
A---B---C  main
     \
      D---E  feature/login
```

**Merge** (`git switch feature/login` ပြီး `git merge main`)

```
A---B------C  main
     \      \
      D---E---M  feature/login
```

**Rebase** (`git switch feature/login` ပြီး `git rebase main`)

```
A---B---C  main
         \
          D'---E'  feature/login
```

- Merge: D, E မူရင်းအတိုင်းကျန်ပြီး merge commit M ထပ်ဖြစ်တယ်။
- Rebase: D', E' က base အသစ်ပေါ် ပြန်တင်ထားတဲ့ commit တွေမို့ hash အသစ်ရတယ်။ ပြီးသား/ဗလာဖြစ်သွားတဲ့ change တွေ drop ဖြစ်နိုင်လို့ commit အရေအတွက် အမြဲတူမယ်လို့ မဆိုနိုင်ဘူး။

| | Merge | Rebase |
|---|---|---|
| မူရင်း commit | ထိန်းထား | commit အသစ်တွေ (replay) |
| History | ပေါင်းခဲ့တာ မှတ်တမ်းကျန် | ပုံမှန်အားဖြင့် တစ်ကြောင်းတည်း |
| Conflict | ပေါင်းတဲ့အချိန် တစ်ခါတည်းဖြေ | commit တစ်ခုချင်းစီမှာ ဖြေရနိုင် |
| သုံးရင်ကောင်းတာ | shared branch ပေါင်းတာ | ကိုယ့် local feature ကို update / ပြန်စီတာ |
| သတိထားရန် | ပေါင်းပြီး tests စစ် | shared history ကို မလိုဘဲ ပြန်မရေးနဲ့ |

<a id="s4-1"></a>

### Rebase ကို ဘယ်အချိန်သုံးမလဲ

- Feature လုပ်နေတုန်း main ရှေ့ရောက်သွားလို့ နောက်ဆုံး main ပေါ်မှာ စမ်းချင်တဲ့အခါ (merge နဲ့လည်း ရတယ်)။
- PR မတင်ခင် ကိုယ့် commit တွေ (typo fix စတာ) ကို ပြန်စီချင်တဲ့အခါ။
- Team က linear history လိုချင်တဲ့အခါ (team policy လိုက်)။

Remote main နဲ့ update လုပ်ပုံ (working tree clean ဖြစ်ရမယ်):

```bash
git switch feature/login
git fetch origin
git merge origin/main        # merge နည်း
git rebase origin/main       # rebase နည်း (အရင် backup branch ထားပါ)
```

<a id="s4-2"></a>

### Conflict commands

| | Merge | Rebase |
|---|---|---|
| ကြည့် | `git status` | `git status` |
| ပြင်ပြီး stage | `git add app.txt` | `git add app.txt` |
| ဆက်လုပ် | `git merge --continue` | `git rebase --continue` |
| ပယ်ဖျက် | `git merge --abort` | `git rebase --abort` |
| commit ကျော် | – | `git rebase --skip` |

- Rebase မှာ conflict က commit တစ်ခုပြီးတစ်ခု ထပ်ထပ်ဖြစ်နိုင်တယ်၊ ပြင် → add → continue ကို ထပ်လုပ်။ ဖြေပြီးရင် `git commit` သပ်သပ်မလုပ်ရဘူး၊ `--continue` ပဲသုံး။ `--skip` က အဲ့ commit ရဲ့ changes မလိုတော့မှ သုံး။
- ⚠️ Rebase လုပ်တုန်း `ours` / `theirs` က ပြောင်းပြန်: `ours` = upstream ပေါ် အခုအထိ rebase လုပ်ပြီးသားဘက်၊ `theirs` = အခု replay လုပ်နေတဲ့ feature commit ဘက်။

<a id="s4-3"></a>

### Interactive rebase

```bash
git rebase -i HEAD~3
```

| Action | ဘာလုပ်လဲ |
|---|---|
| `pick` | commit ကိုသုံး |
| `reword` | message ပြင် |
| `edit` | ရပ်ပြီး commit ပြင်ခွင့်ပေး |
| `squash` | ရှေ့ commit နဲ့ပေါင်း၊ message တွေ ပြင်နိုင် |
| `fixup` | ရှေ့ commit နဲ့ပေါင်း၊ ဒီ message ကိုပယ် |
| `drop` | commit ဖယ် |

`squash` / `fixup` သုံးဖို့ ရှေ့မှာ commit တစ်ခုရှိရမယ်။ ပထမဆုံး commit ပါ ပြန်စီချင်ရင် `git rebase -i --root`။

စမ်းခဲ့တာ – commit ၃ ခုကို ၂ ခုဖြစ်အောင် ပေါင်းတယ် (editor မှာ အဟောင်းဆုံးက အပေါ်ဆုံး):

```
pick  51a9345 Create login page
pick  61c670e Add validation
fixup fff8b4a Fix validation typo
```

```
မလုပ်ခင်:                            လုပ်ပြီး:
A---B---C---D                       A---B---C′       (C′ message = Add validation)
```

ဖိုင်ရဲ့ နောက်ဆုံးအကြောင်းအရာ မပျောက်ဘူး၊ B မပြောင်းဘူး၊ C′ က hash အသစ်။

<a id="s4-4"></a>

### Squash merge vs Interactive rebase (မရောနဲ့)

Squash merge ပုံက [1.2.3 Squash](#s1-2-3) မှာရှိတယ်။ ခြားနားချက်:

| | Squash merge | Interactive rebase |
|---|---|---|
| ဘယ်မှာ effect ဖြစ်လဲ | destination branch (main) | ပြန်စီတဲ့ feature branch |
| feature မူရင်း commit | မပြောင်း | ပြောင်းသွားတယ် |
| changes အကုန် commit တစ်ခုလား | ဟုတ် (S တစ်ခု၊ parent တစ်ခုတည်း) | ကိုယ်ရွေးတာပဲ ပေါင်းလို့ရ |
| main ထဲရောက်ပြီလား | commit လုပ်ပြီးရင်ရောက် | မရောက်သေး၊ နောက်မှပေါင်းရမယ် |

Main မှာ feature တစ်ခုကို commit တစ်ခုနဲ့ မှတ်ချင်ရင် squash merge၊ commit တစ်ခုချင်းရဲ့ ရည်ရွယ်ချက်ကို ထိန်းချင်ရင် ရိုးရိုး merge၊ feature commit တွေကို သပ်သပ်ရပ်ရပ်စီချင်ရင် interactive rebase။

<a id="s4-5"></a>

### Push လုပ်တဲ့အခါ

```bash
git push -u origin feature/login                       # ပုံမှန်
git push --force-with-lease origin feature/login       # တင်ပြီးသား commit ကို rebase လုပ်ထားရင်၊ team သဘောတူမှ
```

`--force-with-lease` က remote က ကိုယ်မျှော်လင့်ထားတဲ့ အခြေအနေမှာ ရှိမှ overwrite လုပ်တယ်။ ဒါပေမဲ့ တခြားသူ work မပျောက်ဘူးဆိုတာ အာမခံချက်တော့ မဟုတ်ဘူး။ Shared branch ကို rebase မလုပ်ခင် team နဲ့ အရင်ညှိပါ။
