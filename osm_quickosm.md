# QuickOSM

在試試使用下載整份 OSM 資料時，發現這些檔案能夠用 QGIS 開啟，呈現在 QGIS 內，但因為檔案過於龐大，幾乎跑不動，尤其我只是開幾百 M 的檔案而已，若是幾 G 的肯定開不起來。
原本著手研究不開 QGIS GUI，利用獨立腳本執行 PyQGIS，就無需等待開啟 OSM 資料的時間，**直接篩選並輸出**想要的資料。
QGIS 中有一個外掛也能夠開啟 OSM 資料，但只仔細一看，赫然發現該外掛，就能完整的取得設定條件的所有空間資料，且能夠獲得該資料的所有屬性資料。這個外掛就是 **QuickOSM**。

1. 首先在 QGSIS 中的外掛管理，安裝 QuickOSM 並開啟功能，而後就能右鍵在 Toolbars 中看到 QuickOSM，勾選之後就會出現

![tool](C:/OSM/quickosmtool.PNG)

2. 先開啟一張底圖，我選擇開啟 OSM 底圖，**作為接下來要搜尋的範圍(全球)**

![gobel](C:/OSM/osmmap.PNG)

3. 接著打開 QuickOSM，先在快速檢索輸入 **鍵(Key)** 與 **值(Value)**，也就是要搜尋的目標物，並將輸入的範圍選取 **圖層範圍**、**開啟的全球地圖名稱**：

![quick0](C:/OSM/quick0.PNG)



#### 以目標"點"作為示範：  

(一) 再打開進階只勾選 **節點(node)** 與 **點**，接者點選顯示檢索

![quick1](C:/OSM/quick1.PNG)



(二) 就會跳至檢索，接下來將上面的 `timeout="25"` 刪除，並在進階下 {{bbox}}或{{center}} 確認搜尋範圍，是否為選取 **圖層範圍**，雖然在一開始的快速檢索中已經選取，但有時仍然會到這一步驟跑掉，必須檢查

![quick2](C:/OSM/quick2.PNG)



(三) 按下執行檢索，等待搜尋結果呈現後，就能在 Layers 中看到暫存的圖層，最後按下右邊存成 SHP，且必定是 **WGS84 座標系**，即使底圖不是 WGS84

![quick3](C:/OSM/quick3.PNG)



#### 快速做法：直接打開 "檢索"

- 點：複製以下，目標條件只需更改以下 k 及 v，且注意 **搜尋範圍**，及進階之下選取輸出 **點**

```quickosm
<osm-script output="xml">
    <union>
        <query type="node">
            <has-kv k="military" v="airfield"/>
            <bbox-query {{bbox}}/>
        </query>
    </union>
    <union>
        <item/>
        <recurse type="down"/>
    </union>
    <print mode="body"/>
</osm-script>
```

![quickpoint](C:/OSM/quickpoint.PNG)



- way 面：複製以下，並且目標條件只需更改以下 k 及 v，且注意 **搜尋範圍**，及進階之下選取輸出 **多重多邊形**

```quickosm
<osm-script output="xml">
    <union>
        <query type="way">
            <has-kv k="military" v="airfield"/>
            <bbox-query {{bbox}}/>
        </query>
    </union>
    <union>
        <item/>
        <recurse type="down"/>
    </union>
    <print mode="body"/>
</osm-script>
```

![quickway](C:/OSM/quickway.PNG)



- relation 面：複製以下，並且目標條件只需更改以下 k 及 v，且注意 **搜尋範圍**，及進階之下選取輸出 **多重多邊形**

```
<osm-script output="xml">
    <union>
        <query type="relation">
            <has-kv k="military" v="airfield"/>
            <bbox-query {{bbox}}/>
        </query>
    </union>
    <union>
        <item/>
        <recurse type="down"/>
    </union>
    <print mode="body"/>
</osm-script>
```

![quickrelation](C:/OSM/quickrelation.PNG)



#### 輸出結果

![quickresult](C:/OSM/quickresult.PNG)



