資料庫系統期末專題報告
------------------

# 虎尾美食推薦系統

## 組別與成員
**組別**：第14組  
**成員**：  
- 41043104 郭俞汎 
- 41043146 張哲維  
- 41143237 陳昱霖  
- 41143254 鄭名男


## 實作Demo
https://drive.google.com/drive/folders/1dE6DKWtguK_YtDJdcWRwP2tqN0rkKKVq?usp=sharing  

## 總覽
虎尾美食推薦系統旨在幫助使用者根據個人喜好尋找虎尾地區的合適餐廳。使用者可以註冊、登入、選擇喜好類別，並接收個人化的餐廳推薦。管理員可以管理餐廳資訊、喜好類別，並查看會員數量統計，但無法存取個人用戶資料。

## 目錄
1. [應用情境](#應用情境)
2. [系統需求說明](#系統需求說明)
3. [使用案例](#使用案例)
4. [資料概念層模型](#資料概念層模型)
5. [ER Diagram](#ER_Diagram)
6. [資料庫Schema](#資料庫schema)
7. [View SQL](#view-sql)
8. [使用者權限SQL](#使用者權限sql)


## 應用情境

### 情境 1: 初來乍到
- **情境描述**：小明和他的三個朋友剛從台北開車到虎尾渡假，這是他們第一次來虎尾，對當地的美食店家一無所知。到了晚上，大家都開始肚子餓了，想要找間不錯的餐廳用餐。
- **系統應用**：
  1. 小明打開虎尾美食推薦系統，點選註冊新會員，填寫基本資料完成註冊。
  2. 登入後，系統詢問小明的喜好類別，小明勾選了「燒肉」、「丼飯」和「甜食」三個類別。
  3. 系統根據小明的喜好，過濾和分析虎尾所有餐廳的菜色類別，很快產生一份推薦名單。
  4. 透過打開推薦清單中的 Google 地圖連結，小明看到「色鼎燒肉」這家店的介紹：提供燒肉、丼飯和甜點等多種菜色，營業到晚上 11 點，離小明下榻的旅館也不太遠。
  5. 小明點開看到具體位置和路線導航後，決定就帶著三個朋友前往這家餐廳用餐。

### 情境 2: 熟門熟路
- **情境描述**：老王是虎尾科大的學生，住在虎尾兩年了，他不喜歡每天都吃同樣的餐點，但卻想不到要吃什麼，詢問同學也得到了「不知道欸？你要吃什麼」的回答。
- **系統應用**：
  1. 老王打開虎尾美食推薦系統，登入後系統詢問老王的喜好類別，老王發現自己在上次選擇了「火鍋」這個類別。
  2. 老王覺得火鍋已經吃過了，取消掉火鍋的選項，改選擇了「麵類」。
  3. 系統根據老王的喜好，過濾和分析虎尾所有餐廳的菜色類別，很快產生一份推薦名單。
  4. 在推薦清單中，老王看到「客家庄」、「熬鍋燒」等店家，他們都有販賣麵類，透過打開推薦清單中的 Google 地圖連結後發現「熬鍋燒」比較便宜，於是老王選擇了「熬鍋燒」。
  5. 老王找了朋友們一起騎車去吃「熬鍋燒」。

## 系統需求說明

### 系統概述
本系統旨在幫助使用者找尋虎尾地區的美食店家，根據其個人喜好進行推薦。系統主要功能包括管理員登入、會員登入、美食店家資料管理、會員喜好類別輸入、推薦項目清單產生等。

### 功能需求
1. **會員管理**：
   - 使用者會員註冊
   - 使用者會員登入
   - 管理員查看會員數量 (無法查看會員個人資料)
2. **美食店家管理**：
   - 管理員新增、刪除、更新店家資訊
   - 使用者瀏覽店家列表
3. **喜好類別管理**：
   - 管理員新增、刪除喜好類別
   - 管理員為店家新增類別
   - 使用者設定自己的喜好類別
4. **推薦項目生成**：
   - 系統根據會員的喜好類別，隨機產生符合類別的 2 個推薦店家
5. **安全性**：
   - 密碼獨立資料表儲存
   - 一般管理員無法查看會員個人資料

## 使用案例

### 1. 使用者角色(會員)
![image](https://github.com/aasd0/database/blob/main/user.jpg)

在此系統中，會員的使用過程包含以下幾個步驟：
- **註冊帳號**：會員需要在系統上註冊，這涉及使用者資料的儲存，對應資料庫中「Member」實體。
- **登入系統**：會員可以透過註冊的帳號密碼登入，對應「Member」與「Password」實體的存取。
- **選擇喜好**：會員可以自行選擇喜好類別，對應「Category」與「Preference」實體的關聯與修改。
- **進行推薦**：會員可以點擊推薦功能，對應「Restaurant」與「Recommendation」實體的關聯與新增。

### 2. 管理員角色(餐廳資料管理者)
![image](https://github.com/aasd0/database/blob/main/administrator.jpg)

在此系統中，管理員的使用過程包含以下幾個步驟：
- **查看會員數量**：餐廳資料管理者可以在系統上查看會員數量，這涉及對「Member」實體數量的存取。
- **更新店家資訊**：餐廳資料管理者可以在系統上更新店家的相關資訊，這涉及對「Restaurant」、「Hours」與「resCate」實體的修改。
- **更新喜好類別**：餐廳資料管理者可以在系統上新增、刪除喜好類別，這涉及對「Category」實體的修改。

## 資料概念層模型

### 資料表

#### 1. 會員 (Member) 資料表
| 欄位名稱     | 說明           | 資料型態         | 是否為空 | 值域                                                                 |
|--------------|----------------|------------------|----------|----------------------------------------------------------------------|
| mId          | 會員 ID         | INT AUTO_INCREMENT | N        | 主鍵，自動遞增                                                     |
| mName        | 姓名            | VARCHAR(12)      | N        | 2~12 字元，限中文字或英文字母 |
| mAccount     | 帳號            | VARCHAR(20)      | N        | 6~20 字元，限英文與數字組合       |
| mEmail       | 電子郵件        | VARCHAR(50)      | N        | Email 格式， '^[a-zA-Z0-9.]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$' |
| mPhone       | 手機            | CHAR(10)         | Y        | 台灣手機格式，以 09 開頭後面接 8 個數字 |
| mAddress     | 地址            | VARCHAR(64)      | Y        | 最長 64 字元的文字                                                |
| mCreateDate  | 建立日期        | DATE             | N        | 格式為 yyyy-mm-dd ，未設定時自動填入當前系統日期    |

#### 2. 喜好類別 (Category) 資料表
| 欄位名稱 | 說明         | 資料型態     | 是否為空 | 值域                                                                 |
|--------------|----------------|------------------|----------|----------------------------------------------------------------------|
| cId      | 類別 ID      | INT AUTO_INCREMENT | N        | 主鍵，自動遞增                                                     |
| cName    | 類別名稱     | VARCHAR(16)  | N        | 1~16 字元，限中文字或英文字母                                                             |

#### 3. 營業時間 (Hours) 資料表
| 欄位名稱   | 說明                 | 資料型態     | 是否為空 | 值域                                                                 |
|--------------|----------------|------------------|----------|----------------------------------------------------------------------|
| rHoursId   | 營業時間 ID          | INT          | N        | 從 1 開始遞增的整數                                    |
| day        | 星期                 | CHAR(3)      | N        |只能是固定的 Mon, Tue, Wed, Thu, Fri, Sat, Sun  這幾種          |
| start_hr   | 開始小時             | TINYINT      | N        | 0~23（24 小時制），整數                                   |
| start_min  | 開始分鐘             | TINYINT      | N        | 0~59，整數                                           |
| end_hr     | 結束小時             | TINYINT      | N        | 0~23（24 小時制），整數                              |
| end_min    | 結束分鐘             | TINYINT      | N        | 0~59，整數                                   |

#### 4. 餐廳 (Restaurant) 資料表
| 欄位名稱   | 說明           | 資料型態         | 是否為空 | 值域                                                                 |
|--------------|------------------|------------------|----------|----------------------------------------------------------------------|
| rId        | 店家 ID          | INT AUTO_INCREMENT | N        | 主鍵，自動遞增                                                     |
| rName      | 店名             | VARCHAR(30)      | N        | 1~30 字元，中文或英文皆可                                |
| rAddress   | 地址             | VARCHAR(64)      | N        | 最長 64 字元的文字                                             |
| rPhone     | 店家電話         | VARCHAR(10)         | Y        |  056 開頭後面 6 個數字的虎尾市話 或 09 開頭後面 8 個數字的行動電話          |
| rHoursId   | 營業時間 ID      | INT              | Y        | 參考 hours.rHoursId                     |
| rLink      | GoogleMap連結    | VARCHAR(100)     | Y        | URL 格式，含 https://maps.app.goo.gl/ + 英文與數字的組合  |

#### 5. 店家類別對應表 (resCate)
| 欄位名稱 | 說明       | 資料型態 | 是否為空 | 值域                                              |
|--------------|-----------------|------------------|----------|----------------------------------------------------------------------|
| rId      | 店家 ID    | INT      | N        | 參考 restaurant.rId |
| cId      | 類別 ID    | INT      | N        | 參考 category.cId |

#### 6. 會員喜好類別表 (Preference)
| 欄位名稱 | 說明       | 資料型態 | 是否為空 | 值域                                              |
|--------------|----------------|------------------|----------|----------------------------------------------------------------------|
| mId      | 會員 ID    | INT      | N        | 參考 member.mId |
| cId      | 類別 ID    | INT      | N        | 參考 category.cId |

#### 7. 推薦清單表 (Recommendation)
| 欄位名稱 | 說明         | 資料型態 | 是否為空 | 值域                                                                 |
|--------------|----------------|------------------|----------|----------------------------------------------------------------------|
| recId    | 推薦 ID      | INT AUTO_INCREMENT | N        | 主鍵，自動遞增                                                     |
| mId      | 會員 ID      | INT      | N        | 參考 member.mId                   |
| rIdA     | 推薦店家 A ID | INT      | N        | 參考 restaurant.rId       |
| rIdB     | 推薦店家 B ID | INT      | N        | 參考 restaurant.rId         |
| recDate  | 推薦日期     | DATE     | N        | 格式為 yyyy-mm-dd，未設定時自動填入當前系統日期   |

## ER_Diagram
![image](https://github.com/gyufan/database/blob/main/ER%20Diagram.jpg)

#### 1. 會員 (Member) 資料表屬性
- 會員ID (mId)
- 會員帳號 (mAccount)
- 姓名 (mName)
- 電子郵件 (mEmail)
- 連絡電話 (mPhone)
- 常用地址 (mAddress)
- 建立日期 (mCreateDate)

#### 2. 喜好類別 (Category) 資料表屬性
- 類別ID (cId)
- 類別名稱 (cName)

#### 3. 營業時間 (Hours) 資料表屬性
- 營業時間ID (rHoursId)
- 營業星期 (day)
- 營業開始時 (start_hr)
- 營業開始分 (start_min)
- 營業結束時 (end_hr)
- 營業結束分 (end_min)

#### 4. 餐廳 (Restaurant) 資料表屬性
- 店家ID (rId)
- 店家名稱 (rName)
- 店家地址 (rAddress)
- 店家電話 (rPhone)
- 營業時間ID (rHoursId)
- Google Map 連結 (rLink)

#### 5. 店家類別 (resCate) 資料表屬性
- 店家ID (rId)
- 類別ID (cId)

#### 6. 會員喜好類別 (Preference) 資料表屬性
- 會員ID (mId)
- 類別ID (cId)

#### 7. 推薦清單 (Recommendation) 資料表屬性
- 推薦ID (recId)
- 會員ID (mId)
- 店家1 ID (rIdA)
- 店家2 ID (rIdB)
- 推薦日期 (recDate)

#### 8. 關聯
- 「Member」與「Preference」實體有一對一 (1..1) 的關係，每個使用者對應到一整組喜好。
- 「Preference」與「Category」實體有一對多 (1..n) 的關係，每組使用者喜好對應到多個喜好類別。
- 「Preference」與「Recommendation」實體有一對多 (1..n) 的關係，每組使用者喜好可以產生多個推薦清單。
- 「Restaurant」與「Hours」實體有一對多 (1..n) 的關係，每個店家有不同的營業時段。
- 「Restaurant」與「Recommendation」實體有一對一 (1..1) 的關係，每個店家對應到一組推薦清單。
- 「Restaurant」與「resCate」實體有一對多 (1..n) 的關係，每個店家對應到多個類別。
- 「resCate」與「Category」實體有一對多 (1..n) 的關係，每個店家類別對應到多個喜好類別。

## 資料庫Schema

以下是創建資料庫表的 SQL：

```sql
-- 會員 (Member) 資料表
CREATE TABLE Member (
    mId INT NOT NULL AUTO_INCREMENT,
    mName VARCHAR(12) NOT NULL,
    mAccount VARCHAR(20) NOT NULL,
    mEmail VARCHAR(50) NOT NULL,
    mPhone CHAR(10),
    mAddress VARCHAR(64),
    mCreateDate DATE NOT NULL DEFAULT CURRENT_DATE,
    CHECK (mAccount REGEXP '^[a-zA-Z0-9._]{6,20}$'),
    CHECK (CHAR_LENGTH(mName) >= 2),
    CHECK (mEmail REGEXP '^[a-zA-Z0-9.]+@[a-zA-Z0-9.]+\.[a-zA-Z]{2,}$'),
    CHECK (mPhone IS NULL OR mPhone REGEXP '^09[0-9]{8}$'),
    CHECK (mAddress IS NULL OR CHAR_LENGTH(mAddress) >= 1),
    PRIMARY KEY (mId),
    UNIQUE (mAccount),
    UNIQUE (mEmail)
);

-- 喜好類別 (Category) 資料表
CREATE TABLE Category (
    cId INT NOT NULL AUTO_INCREMENT,
    cName VARCHAR(16) NOT NULL,
    PRIMARY KEY (cId),
    CHECK (CHAR_LENGTH(cName) >= 1)
    UNIQUE (cName)
);

-- 營業時間 (Hours) 資料表
CREATE TABLE Hours (
    rHoursId INT NOT NULL,
    day CHAR(3) NOT NULL,
    start_hr NUMERIC(2) CHECK (start_hr >= 0 AND start_hr <= 24),
    start_min NUMERIC(2) CHECK (start_min >= 0 AND start_min < 60),
    end_hr NUMERIC(2) CHECK (end_hr >= 0 AND end_hr <= 24),
    end_min NUMERIC(2) CHECK (end_min >= 0 AND end_min < 60),
    PRIMARY KEY (rHoursId, day, start_hr, start_min, end_hr, end_min),
    CHECK (day IN ('Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'))
);

-- 餐廳 (Restaurant) 資料表
CREATE TABLE Restaurant (
    rId INT NOT NULL AUTO_INCREMENT,
    rName VARCHAR(30) NOT NULL,
    rAddress VARCHAR(64) NOT NULL,
    rPhone VARCHAR(10),
    rHoursId INT,
    rLink VARCHAR(100),
    PRIMARY KEY (rId),
    FOREIGN KEY (rHoursId) REFERENCES Hours(rHoursId) ON DELETE SET NULL,
    CHECK (CHAR_LENGTH(rName) >= 1),
    CHECK (CHAR_LENGTH(rAddress) >= 1),
    CHECK (rLink IS NULL OR rLink REGEXP '^https://maps\.app\.goo\.gl/.*$'),
    CHECK (rPhone IS NULL OR rPhone REGEXP '^056[0-9]{6}|09[0-9]{8}$')
);
--觸發器 "" 換 null,
DELIMITER //
CREATE TRIGGER restaurant_before_insert
BEFORE INSERT ON Restaurant
FOR EACH ROW
BEGIN
    IF NEW.rPhone = "" THEN
        SET NEW.rPhone = NULL;
    END IF;
END //
DELIMITER ;

-- 店家類別 (resCate) 資料表
CREATE TABLE resCate (
    rId INT NOT NULL,
    cId INT NOT NULL,
    PRIMARY KEY (rId, cId),
    FOREIGN KEY (rId) REFERENCES Restaurant(rId) ON DELETE CASCADE,
    FOREIGN KEY (cId) REFERENCES Category(cId) ON DELETE CASCADE
);

-- 會員喜好類別 (Preference) 資料表
CREATE TABLE Preference (
    mId INT NOT NULL,
    cId INT NOT NULL,
    PRIMARY KEY (mId, cId),
    FOREIGN KEY (mId) REFERENCES Member(mId) ON DELETE CASCADE,
    FOREIGN KEY (cId) REFERENCES Category(cId) ON DELETE CASCADE
);

-- 推薦清單 (Recommendation) 資料表
CREATE TABLE Recommendation (
    recId INT NOT NULL AUTO_INCREMENT,
    mId INT NOT NULL,
    rIdA INT NOT NULL,
    rIdB INT NOT NULL,
    recDate DATE NOT NULL DEFAULT CURRENT_DATE,
    PRIMARY KEY (recId),
    FOREIGN KEY (mId) REFERENCES Member(mId) ON DELETE CASCADE,
    FOREIGN KEY (rIdA) REFERENCES Restaurant(rId) ON DELETE CASCADE,
    FOREIGN KEY (rIdB) REFERENCES Restaurant(rId) ON DELETE CASCADE
);

```

### 範例資料

```sql
-- 會員 (Member) 範例資料
INSERT INTO Member (mAccount, mName, mEmail, mPhone, mAddress, mCreateDate) VALUES
    ('yf0612', '郭俞汎', '41043104@nfu.edu.tw', '0900787955', '雲林縣虎尾鎮新生路74號', '2025-04-27'),
    ('pp9988', '無名氏', '94879487@gmail.com', '0987987987', '雲林縣虎尾鎮文化路87號', '2025-04-27'),
    ('chen.meiling', '陳美玲', 'chen.meiling@gmail.com', '0921789584', '雲林縣虎尾鎮中正路376號', '2025-05-13');

-- 喜好類別 (Category) 範例資料
INSERT INTO Category (cName) VALUES
    ('沙拉'), ('丼飯'), ('咖哩'), ('炸物'), ('羹類');

-- 營業時間 (Hours) 範例資料
INSERT INTO Hours (rHoursId, day, start_hr, start_min, end_hr, end_min) VALUES
    (1, 'Mon', 11, 30, 14, 30),
    (1, 'Thu', 11, 30, 14, 30),
    (2, 'Tue', 11, 0, 14, 0),
    (2, 'Wed', 11, 0, 14, 0),
    (3, 'Tue', 6, 30, 12, 30);

-- 餐廳 (Restaurant) 範例資料
INSERT INTO Restaurant (rName, rAddress, rPhone, rHoursId, rLink) VALUES
    ('樂飯樂販', '雲林縣虎尾鎮自強街31號', '056310673', 1, 'https://maps.app.goo.gl/4ZT4XMCax78Y1ANt5'),
    ('第8間咖哩', '雲林縣虎尾鎮文化路88號', '056311288', 2, 'https://maps.app.goo.gl/xS93PNQkDyuNdNx9A'),
    ('虎尾美貞肉羹麵', '雲林縣虎尾鎮中山路23巷2-2號', '', 3, 'https://maps.app.goo.gl/29f6CWfwcQXAKyPp9'),
    ('川息拉麵丼飯', '雲林縣虎尾鎮林森路二段111號', '0921794642', 4, 'https://maps.app.goo.gl/ZhUZZnqRQzkYf4YQ9');

-- 店家類別 (resCate) 範例資料
INSERT INTO resCate (rId, cId) VALUES
    (1, 1), (1, 2), (1, 3), (2, 1), (2, 3);

-- 會員喜好類別 (Preference) 範例資料
INSERT INTO Preference (mId, cId) VALUES
    (2, 10), (2, 20), (2, 24), (4, 12);

-- 推薦清單 (Recommendation) 範例資料
INSERT INTO Recommendation (mId, rIdA, rIdB, recDate) VALUES
    (2, 34, 48, '2025-05-01'),
    (2, 5, 79, '2025-05-01'),
    (5, 1, 49, '2025-05-02');

```

## View SQL

### 會員View

### 1. 會員喜好 View：`user_preferences`

**用途**：彙整會員的喜好類別。

```sql
SELECT 
    m.mID,
    m.mName,
    m.mAccount,
    GROUP_CONCAT(c.cName SEPARATOR ', ') AS preferences
FROM member m
LEFT JOIN preference p ON m.mID = p.mID
LEFT JOIN category c ON p.cID = c.cID
GROUP BY m.mID, m.mName, m.mAccount;
```
---
### 2. 營業中餐廳 View：`open_restaurants`

**用途**：查詢目前時間仍在營業的餐廳。

```sql
SELECT DISTINCT
    r.rID,
    r.rName,
    r.rAddress,
    r.rPhone,
    r.rLink
FROM restaurant r
JOIN hours h ON r.rHoursID = h.rHoursID
WHERE h.day = LEFT(DAYNAME(NOW()), 3)
AND (
    (h.start_hr < HOUR(NOW()) OR 
     (h.start_hr = HOUR(NOW()) AND h.start_min <= MINUTE(NOW())))
    AND
    (h.end_hr > HOUR(NOW()) OR 
     (h.end_hr = HOUR(NOW()) AND h.end_min >= MINUTE(NOW())))
);
```
---
### 3. 推薦候選餐廳 View：`recommendation_candidates`

**用途**：計算會員與餐廳間的喜好配對數量（供推薦邏輯使用）。

```sql
SELECT 
    p.mID,
    r.rID,
    r.rName,
    r.rAddress,
    r.rPhone,
    r.rLink,
    COUNT(DISTINCT rc.cID) AS matching_categories
FROM preference p
JOIN resCate rc ON p.cID = rc.cID
JOIN restaurant r ON rc.rID = r.rID
GROUP BY p.mID, r.rID, r.rName, r.rAddress, r.rPhone, r.rLink
ORDER BY p.mID, matching_categories DESC;
```
---
### 4. 推薦記錄 View：`recent_recommendations`

**用途**：顯示推薦歷史記錄與對應餐廳名稱。

```sql
SELECT 
    rec.recID,
    m.mName,
    m.mAccount,
    r1.rName AS restaurant_1,
    r2.rName AS restaurant_2,
    rec.recDate
FROM recommendation rec
JOIN member m ON rec.mID = m.mID
JOIN restaurant r1 ON rec.rIDA = r1.rID
JOIN restaurant r2 ON rec.rIDB = r2.rID
ORDER BY rec.recDate DESC, rec.recID DESC;
```

---

### 5. 餐廳詳情 View：`restaurant_details`

**用途**：整合餐廳的類別與營業時段顯示。

```sql
SELECT 
    r.rID,
    r.rName,
    r.rAddress,
    r.rPhone,
    r.rLink,
    GROUP_CONCAT(DISTINCT c.cName SEPARATOR ', ') AS categories,
    GROUP_CONCAT(DISTINCT 
        CONCAT(h.day, ' ', 
               LPAD(h.start_hr, 2, '0'), ':', LPAD(h.start_min, 2, '0'), 
               '-', 
               LPAD(h.end_hr, 2, '0'), ':', LPAD(h.end_min, 2, '0'))
        SEPARATOR '; ') AS business_hours
FROM restaurant r
LEFT JOIN resCate rc ON r.rID = rc.rID
LEFT JOIN category c ON rc.cID = c.cID
LEFT JOIN hours h ON r.rHoursID = h.rHoursID
GROUP BY r.rID, r.rName, r.rAddress, r.rPhone, r.rLink;
```
---
### 管理員View

### 6. 系統統計 View：`admin_statistics`

**用途**：顯示會員總數、餐廳數、類別數與推薦記錄數。

```sql
SELECT 
    (SELECT COUNT(*) FROM member) AS total_members,
    (SELECT COUNT(*) FROM restaurant) AS total_restaurants,
    (SELECT COUNT(*) FROM category) AS total_categories,
    (SELECT COUNT(*) FROM recommendation) AS total_recommendations;
```

---
## 使用者權限SQL

```sql
-- 1. 建立會員使用者帳號
CREATE USER 'user_general'@'%' IDENTIFIED BY 'userpassword123';

-- 2. 會員使用者對 huwei_food 資料庫中的操作權限
GRANT SELECT, INSERT, UPDATE, DELETE ON huwei_food.Preference TO 'user_general'@'%';
GRANT SELECT ON huwei_food.recommendation TO 'user_general'@'%';

-- 3. 授權會員使用者查詢view
GRANT SELECT ON huwei_food.user_preferences TO 'user_general'@'%';
GRANT SELECT ON huwei_food.open_restaurants TO 'user_general'@'%';
GRANT SELECT ON huwei_food.recommendation_candidates TO 'user_general'@'%';
GRANT SELECT ON huwei_food.recent_recommendations TO 'user_general'@'%';
GRANT SELECT ON huwei_food.restaurant_details TO 'user_general'@'%';

-- 4. 建立餐廳資料管理者帳號
CREATE USER 'user_admin'@'%' IDENTIFIED BY 'userpassword789';

-- 5. 餐廳資料管理者對 huwei_food 資料庫中的操作權限
GRANT SELECT, INSERT, UPDATE, DELETE ON huwei_food.category TO 'user_admin'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON huwei_food.restaurant TO 'user_admin'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON huwei_food.rescate TO 'user_admin'@'%';
GRANT SELECT, INSERT, UPDATE, DELETE ON huwei_food.hours TO 'user_admin'@'%';

-- 6. 授權餐廳資料管理者查詢view
GRANT SELECT ON huwei_food.admin_statistics TO 'user_general'@'%';

FLUSH PRIVILEGES;

```




