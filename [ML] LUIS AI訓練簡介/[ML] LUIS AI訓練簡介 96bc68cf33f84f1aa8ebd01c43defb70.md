# [ML] LUIS.AI訓練簡介

- 進入[https:\\luis.ai](https://luis.ai)後，使用Microsoft Account登入開始使用
- 先建立app，接下來在app中開始建立Entity以及Intent
- 以下圖為例，定義查IUI Full Lot Report的Intent
- 並且建立該Intent會使用的各項Entity，如LotID等
- 接下來在Intent中，手動輸入不童的訓練用語句。其中需要人工標示訓練語句中的Entity
- 建立完成後，按下右上的Train，訓練完成後可以使用Test，開始輸入語句進行測試
- 回傳為score最高的Intent，並且將該驗證測試語句中的各Entity列出
- Machine Learning類型的Entity意為複合型(Composite)的Entity(eg., 複合型的例子有地址，包含縣市鄉鎮等)，不需要人工手動標示，可以自動學習出來
- 注意：實際應用時，就等於是從語句中知道要使用何種api以及該api必須參數；因此需要把該驗證語句的所有Entity都列舉出來才能夠應用

![%5BML%5D%20LUIS%20AI%E8%A8%93%E7%B7%B4%E7%B0%A1%E4%BB%8B%2096bc68cf33f84f1aa8ebd01c43defb70/LUIS.AI.jpg](%5BML%5D%20LUIS%20AI%E8%A8%93%E7%B7%B4%E7%B0%A1%E4%BB%8B%2096bc68cf33f84f1aa8ebd01c43defb70/LUIS.AI.jpg)

![%5BML%5D%20LUIS%20AI%E8%A8%93%E7%B7%B4%E7%B0%A1%E4%BB%8B%2096bc68cf33f84f1aa8ebd01c43defb70/LUIS.AI_IUI-1.jpg](%5BML%5D%20LUIS%20AI%E8%A8%93%E7%B7%B4%E7%B0%A1%E4%BB%8B%2096bc68cf33f84f1aa8ebd01c43defb70/LUIS.AI_IUI-1.jpg)

![%5BML%5D%20LUIS%20AI%E8%A8%93%E7%B7%B4%E7%B0%A1%E4%BB%8B%2096bc68cf33f84f1aa8ebd01c43defb70/LUIS.AI_IUI-2.jpg](%5BML%5D%20LUIS%20AI%E8%A8%93%E7%B7%B4%E7%B0%A1%E4%BB%8B%2096bc68cf33f84f1aa8ebd01c43defb70/LUIS.AI_IUI-2.jpg)

![%5BML%5D%20LUIS%20AI%E8%A8%93%E7%B7%B4%E7%B0%A1%E4%BB%8B%2096bc68cf33f84f1aa8ebd01c43defb70/LUIS.AI_IUI-3.jpg](%5BML%5D%20LUIS%20AI%E8%A8%93%E7%B7%B4%E7%B0%A1%E4%BB%8B%2096bc68cf33f84f1aa8ebd01c43defb70/LUIS.AI_IUI-3.jpg)