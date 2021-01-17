# [ML] 透過Docker在本地端使用LUIS模型進行語意分析

微軟提供LUIS.AI可以讓programmer直接幫自己的聊天機器人加入使用語意分析做出了解使用者對話功能，但是對於傳統製造業等還是會有將對話訊息傳上雲端造成隱私問題，此篇文章則說明如何將已經訓練好的LUIS.AI模型下載回來，並且使用Docker在本地端帶起讓其他App直接使用。

- 從Microsoft Azure建立資源服務
- 從docker hub取得容器映像檔

    使用Ubuntu 2004作為docker運行環境，輸入以下指令抓下Microsoft Azure Cognitive service docker container image

    docker pull [mcr.microsoft.com/azure-cognitive-services/language/luis:latest](http://mcr.microsoft.com/azure-cognitive-services/language/luis:latest)

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled.png)

- 從LUIS.AI匯出已封裝的應用程式

    在LUIS.AI中，MANAGE → VERSIONS → 0.1 → Export → Export for containers

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%201.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%201.png)

- 使用Docker執行已訓練好的模型
    1.  此例使用Ubuntu啟動，先另外新增input以及output目錄，並將下載的gz檔案放在input中

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%202.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%202.png)

    2. 使用以下命令啟動

    docker run --rm -it -p 127.0.0.1:5000:5000 --memory 4g --cpus 1 --mount type=bind,src=/home/chihsuchen/LUIS_TEST/input,target=/input --mount type=bind,src=/home/chihsuchen/LUIS_TEST/output,target=/output [mcr.microsoft.com/azure-cognitive-services/luis:latest](http://mcr.microsoft.com/azure-cognitive-services/luis:latest) Eula=accept Billing=https://xxx.api.cognitive.microsoft.com/ ApiKey=<你的API key>

    至於endpoint & api key要如何查詢，參考下圖；在Azure portal中，在已經建立完成的myluisaaitest資源中，找到api key & endpoint

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%203.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%203.png)

    啟動完成

    ps. 不之為何原因出現以下fatal訊息

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%204.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%204.png)

    接著使用browser啟動，輸入http://localhost:5000，顯示以下畫面

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%205.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%205.png)

    接著在點入主畫面中的"service description"，顯示以下可用api列表進行測試

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%206.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%206.png)

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%207.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%207.png)

    但是會出現以下"no model found with the given application id"，再回到docker console，會出現在/input目錄下沒有該model檔案

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%208.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%208.png)

    此時缺少檔案就直接手動補上

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%209.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%209.png)

    接著關閉docker cognitive service正在執行的process，再重新啟動一次

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2010.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2010.png)

    可以正常得到以下回覆訊息，不過上面的查詢並不精確，回傳Intent=None

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2011.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2011.png)

    再試一次，查詢輸入以下，q=lotid k1234 full lot report

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2012.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2012.png)

    回覆以下訊息

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2013.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2013.png)

    以下回覆訊息中顯示

    Intent = IUI_Inquiry

    Entity也有找出LotID以及full lot report等項目

    {
    "query": "lotid k1234 full lot report",
    "topScoringIntent": {
    "intent": "IUI_Inquiry",
    "score": 0.9802714
    },
    "entities": [
    {
    "entity": "lotid",
    "type": "Doubt",
    "startIndex": 0,
    "endIndex": 4,
    "score": 0.8182307
    },
    {
    "entity": "k1234 full lot report",
    "type": "FullLotReport",
    "startIndex": 6,
    "endIndex": 26,
    "score": 0.638464868
    },
    {
    "entity": "k1234",
    "type": "LotID",
    "startIndex": 6,
    "endIndex": 10,
    "score": 0.6507329
    }
    ]
    }

- LUIS.AI訓練

    建立一Intent之後，在其中輸入要做訓練的問句範例，並手動標示entity

    ![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2014.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2014.png)

參考：

[安裝和執行適用于 LUIS 的 Docker 容器 - Azure Cognitive Services](https://docs.microsoft.com/zh-tw/azure/cognitive-services/luis/luis-container-howto?tabs=v3)

[透過容器化使用LUIS在地端進行語意分析](https://studyhost.blogspot.com/2021/01/luis.html?fbclid=IwAR3aRCKyafTofwXD8cvrySN_Csk0iqM54FqGeYoEgj7hCi-0df4SdCpdLlg)

[Getting started with LUIS Containers](https://pauliom.com/2019/02/03/getting-started-with-luis-containers/)

![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2015.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2015.png)

![%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2016.png](%5BML%5D%20%E9%80%8F%E9%81%8EDocker%E5%9C%A8%E6%9C%AC%E5%9C%B0%E7%AB%AF%E4%BD%BF%E7%94%A8LUIS%E6%A8%A1%E5%9E%8B%E9%80%B2%E8%A1%8C%E8%AA%9E%E6%84%8F%E5%88%86%E6%9E%90%204e693bae67f9456587ceddfa76fa5e4b/Untitled%2016.png)