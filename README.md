# 誰是牛頭王 Take 6!

**Programming design II**
* Final project
* Due to **6/29 AM 00:05**
* Teacher: 紀博文

---


## 一、基本資料

* 姓名: Yu-Chen Lin
* 系級: CSIE 111
* 學號: 40771131H


---


## 二、檔案

|File name|Describe|
|:--|:--|
|40771131H_project.c|主程式|
|40771131H_contest.h|function prototype & contest的函式|
|Makefile|complier the project|
|README.md|說明|

* 檔案中皆有註解！
* 40771131H_contest.h可提供include => contest.c

---

## 三、How to run?


### (一) Demo

![](https://i.imgur.com/GOcQS9m.gif)

### (二) Sample Tutorial

```shell
$ make
$ ./40771131H_project
```

接著直接按照介面提示即可進行遊戲

---

## 四、規則

Source: http://www.swanpanasia.com/products/take-6

![](https://i.imgur.com/ZmygTkf.gif)


### (一) 普通規則

1. 手牌依據project.pdf規定為104張牌 (非說明書的 人數 )，數值範圍為1-104(唯一)
2. 每位玩家會拿到十張手牌，十個回合所有玩家手牌出完遊戲結束
3. 每張牌依據其數字擁有其牛頭數：
    * 牛頭數7：55
    * 牛頭數5：11, 22, 33, 44, 66, 77, 88, 99
    * 牛頭數3：10, 20, 30, 40, 50, 60, 70, 80, 90, 100
    * 牛頭數2：5, 15, 25, 35, 45, 65, 75, 85, 95
    * 其餘數值為牛頭數1
4. 牌面共四排，每排至多可放5張
5. 起初牌面會有4張排頭，各玩家挑選一張手牌覆蓋，待所有玩家出完，同時翻開牌面，並由數字小至大放入各排內，牌會依據其數字，放入「各排最後一張最接近的數字」，若比任意一排數字還要小，則玩家必須選擇一排，將整排的牌拿走，該玩家所出的排當作該排的新排頭
6. 若放置牌時，該排已經有5張牌，則該玩家必須拿走整排，並以該玩家所出的牌作為新排頭
7. 最終依牛頭數由「少至多」排名

### (二) 變體規則

因在線上遊玩該款遊戲，有時候一個不小心就會變成真正的「牛頭王」，因此思考，也許遊戲勝利規則改為「拿最多牛頭數」勝利，在過程中的策略就會幾乎相反，可是這樣只是單純的修改遊戲(反過來而已)。

如果說，設定拿指定的牛頭數以上勝利，若未超過則按照原本的規則計算勝利狀態，那麼，玩到一半就會有人「走偏」開始往吃牌路線前進，他有他的策略，而一些人則按照「儘量少吃牌」路線前進，由於兩種不同策略的考量，遊戲的牌面將會更難預測，亦可大大增進不確定性以及刺激程度。

因此，在此規則，原則上所有規則同「普通規則」，在計算排名時，假使指定為，而某玩家其牛頭數 ，則按照牛頭數多至少排名，若玩家未符合此條件，則按照牛頭少至多排名，此外，為符合「誰是牛頭王」的名稱，只要得了狂牛症則排名會在未得狂牛症的玩家前面。

即在本規則中，大於等於x玩家稱為「狂牛症」，吃越少則狂牛症指數越低。
→拿超多牛頭數，拿很少牛頭數都有可能獲勝。

#### ──example──
狂牛症變身條件：大於等於30  (自行指定)\
A 玩家：牛頭數 35 =>	勝利指數：35-30 = 5  [狂牛症]\
B 玩家：牛頭數 20 => 	勝利指數：20\
C 玩家：牛頭數 5 	=> 	勝利指數：5\
D 玩家：牛頭數 12 => 	勝利指數：12

則排名： **A(5) = C(5) > D(12) > B(20)**

> p.s 若想要線上嘗試take 6!，可以在下列網址遊玩：\
> URL: https://boardgamearena.com/lobby

---

## 五、使用者介面設計

### (一) 草圖

![](https://i.imgur.com/mIZ0BjX.png)




### (二) 正式流程圖

![](https://i.imgur.com/pnRmfUi.png)

---

## 六、函式設計

contest 限定函式
```cpp
void CSIE_40771131H_setup(int32_t id);
void CSIE_40771131H_deal(const int32_t cards[10]);

int32_t CSIE_40771131H_pick\
	(const int32_t table[4][5], const int32_t score[2],\
	const int32_t last[8], const int32_t card[10]);

int32_t CSIE_40771131H_choose\
	(const int32_t table[4][5], int32_t card);
```

project 函式內容

* input：牌的數值
* output：該數值所代表的牛頭數
* 特例：數值0以及-1輸出為0
```cpp
int32_t CSIE_40771131H_transform(int32_t card);
```

* input：牌面、(空)牌面中每排最後一個的位置、(空)每排的牛頭數總和 →從給定牌面，計算後兩個的數值並存入
```cpp
void CSIE_40771131H_determine_last_index_index\
	(const int32_t table[4][5], int32_t last_index[4],\
		int32_t score[4]);
```

* input：全部卡的陣列、陣列大小 →配置陣列內容為index+1
```cpp
void __setup_shuffle(int32_t card[], int32_t n);
```
	
* input：全部卡的陣列、陣列大小 →從n-1到1，每次都隨機取一個index和第i項交換 →洗牌
```cpp
void range_shuffle(int32_t card[], int32_t n);
```
	
* input：玩家序號、所有手牌陣列、手牌數 →印出指定玩家手牌以及相對應的牛頭數
```cpp
void print_player_hand\
	(int32_t player, int32_t **hand_card, int32_t n);
```
	
* input：牌面、列數、行數 →印出牌面的點數、牛頭數，每列尾巴列出總牛頭數、排尾位置
```cpp
void print_table_status\
	(int32_t table[4][5], int32_t row, int32_t col);
```


* input：列數、行數 →用雙重指標建造一個二維空間
* output：該二維空間的指標位置
```cpp
int32_t **declared_two_pointer(int32_t row, int32_t col);
```

	
* input：手牌、數值
* output：該數值在手牌中的位置，沒找到則回傳-1
```cpp
int32_t look_up_hand_index(int32_t card[10], int32_t val);
```
	
	
* input：牌面、手牌
* output：第一級的電腦決策後選擇的牌(數值)
```cpp
int32_t Agent_pick_level_1
	(int32_t table[4][5], int32_t card[10]);
```
	
	
	
* input：牌面、牌的數值
* output：第一級的電腦決策後選擇要吃掉的列
```cpp
int32_t Agent_choose_level_1\
	(int32_t table[4][5], int32_t card);
```
	
* input：牌面、手牌
* output：第二級的電腦決策後選擇的牌(數值)
```cpp
int32_t Agent_pick_level_2\
	(int32_t table[4][5], int32_t card[10]);
```
	
* input：牌面、牌的數值
* output：第二級的電腦決策後選擇要吃掉的列
```cpp
int32_t Agent_choose_level_2\
	(int32_t table[4][5], int32_t card);
```

* input：牌面、玩家點數、玩家序號、列 →讓指定玩家吃掉指定的列(牛頭數增加)、清除該列J
```cpp
void pick_one_row\
	(int32_t table[4][5], int32_t score[], \
	int32_t player, int32_t row);
```
	
* input：牌面、玩家點數、玩家序號、手牌、數值 →根據數值找到「該放的列」，若小於所有列的數值則回傳-1 →若該列已有五張牌，則以使用者的牌作為排頭，並讓該使用者吃掉整排 →其餘則直接加一張牌在列尾巴
* output：回傳應該放的「列」，若無則回傳-1
```cpp
int32_t put_one_card\
	(int32_t table[4][5], int32_t score[], \
	int32_t player, int32_t card[10], int32_t val);
```
	
* input：手牌、數值 →從手牌中找到指定數值，並清除掉
* output：若找不到數值則回傳-1，成功輸出1

```cpp
int32_t put_one_card_and_clear_hand\
	(int32_t card[10], int32_t val);
```


* input：牌面、玩家序號、手牌、數值、列序號、牌面每列最後的index →將牌放置到指定的列尾巴，並從玩家手牌中清除此牌
* output：成功回傳1，失敗回傳-1

```cpp
int32_t put_one_card_in_table_row\
	(int32_t table[4][5], int32_t player, \
	int32_t card[10], int32_t val, int32_t row, \
	int32_t table_last[]);
```
	
* 列出各式表單
```cpp
void print_meau_start_game();
void print_meau_game_setting();
void print_meau_agent_setting();
void print_meau_store();
void print_meau_sort_hand();
```

**其它**

(結構為 index和所在的列序號)
```cpp
typedef struct _last_status{
	int32_t last;
	int32_t row;
}last_status;
```

(qsort根據last的數值排序)
```cpp
int32_t compare(const void *a, const void *b){
	if (((last_status *) a)->last > \
		((last_status *)b)->last) return 1;
	return 0;
}
```

(cards和id的紀錄)
```cpp
int32_t CSIE_40771131H_cards[10] = {0};
int32_t CSIE_40771131H_id =0;
```

**其它擴增函式**

其它撰寫有關讀檔、存檔和電腦分級的函式，在程式檔內有註解介紹

---

## 七、特殊功能

* 存檔、讀檔功能
* 普通規則、特殊規則可選擇
* 電腦AI共三層分級
* 接受2-10人遊玩

