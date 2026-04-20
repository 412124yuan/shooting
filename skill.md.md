# **遊戲開發規格書：狙擊特攻 (Sniper Elite JS)**

## **1\. 遊戲核心概觀 (Game Overview)**

* **類型**：2D 側視動作射擊 \+ 第一人稱狙擊機制。  
* **核心玩法**：玩家在畫面上自由移動閃躲敵方彈幕，透過「進入狙擊模式」觸發「子彈時間（慢動作）」來進行精準的即時命中射擊。  
* **風格**：現代 2D 幾何與粒子特效風格。

## **2\. 技術選型 (Tech Stack)**

* **環境**：純 HTML5 / CSS3 / Vanilla JavaScript (ES6+)。  
* **渲染引擎**：Canvas API (2D Context)。  
* **動畫驅動**：requestAnimationFrame。  
* **座標系統**：以視窗寬度 (Window Width) 與高度 (Window Height) 為基準的響應式畫布。

## **3\. 核心機制 (Core Mechanics)**

### **A. 操作與輸入 (Input System)**

* **移動**：鍵盤 WASD 或 方向鍵控制。  
* **射擊**：滑鼠左鍵 (即時命中 Hitscan)。  
* **狙擊模式**：滑鼠右鍵長按 (觸發 Time Scale 縮放)。  
* **後座力**：射擊時玩家會向射擊反方向產生位移。

### **B. 時間縮放系統 (Time Scale System)**

* **正常倍率**：timeScale \= 1.0  
* **慢動作倍率**：timeScale \= 0.15 (可調)  
* **邏輯公式**：所有物理移動必須乘以 dt (Delta Time)，其中 dt \= (currentTime \- lastTime) \* timeScale。

### **C. 視覺效果 (Visual Effects)**

* **即時命中**：不計算彈道，直接判定滑鼠座標與敵方碰撞箱的距離。  
* **螢幕震動 (Screen Shake)**：射擊或受傷時給予畫布隨機位移，並以指數級衰減。  
* **粒子系統 (Particle System)**：  
  * 每個粒子具備 vx, vy (初速)、gravity (重力) 與 life (透明度衰減)。  
  * 應用場景：槍口閃光、擊中閃光、死亡爆炸、道具拾取感。  
* **狙擊鏡遮罩**：使用 arc(..., true) 反向剪裁技術繪製黑色半透明遮罩。

## **4\. 物件導向架構 (Class Structure)**

| 類別 (Class) | 責任說明 | 關鍵屬性/方法 |
| :---- | :---- | :---- |
| Player | 玩家位置、速度、強化等級與繪製。 | update(dt), draw(), powerLevel |
| Enemy | 管理敵人 AI 路徑、生命值與發彈計時。 | shoot(), hp, fireRate |
| EnemyBullet | 敵方彈幕粒子，受 timeScale 影響。 | angle, speed, active |
| Item | 掉落道具，處理漂浮動畫與屬性標記。 | type (speed/damage), active |
| Particle | 獨立的視覺點，負責爆炸特效。 | life, decay, gravity |

## **5\. 碰撞偵測邏輯 (Collision Detection)**

1. **圓形 vs 圓形 (Circle Collision)**：  
   * 應用於：玩家撿道具、玩家受傷判定。  
   * 公式：distance(p1, p2) \< (r1 \+ r2)。  
2. **座標判定 (Point-in-Circle)**：  
   * 應用於：狙擊即時命中。  
   * 判定滑鼠座標是否在敵人的 radius 範圍內。

## **6\. 後續開發路線圖 (Future Roadmap)**

* \[ \] **音效系統**：加入 Web Audio API 處理射擊聲與子彈時間音效變調。  
* \[ \] **高級 AI**：加入追蹤彈 (Homing Bullets) 與階段性 Boss 戰。  
* \[ \] **視覺升級**：將幾何圖形替換為 Sprite Sheets 或 Spine 骨骼動畫。  
* \[ \] **UI/UX**：實作漸層血條、Combo 計數器以及主選單畫面。  
* \[ \] **數據持久化**：使用 localStorage 記錄最高分與解鎖武器。

**文件結尾**