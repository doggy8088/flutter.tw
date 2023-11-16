---
title: 瞭解 Flutter 開發者們的 IDE 使用情況
toc: true
keywords: IDE, Android Studio, VS Code
description: 本文中的見解來自 2022 年第二季度開發者調研，由 JaYoung Lee 和 Ander Dobo 整理撰文。
image:
    path: https://devrel.andfun.cn/devrel/posts/2022/10/PEE2bR.png
---

*作者 / JaYoung Lee, UX Researcher at Google*

Google 的 Flutter 團隊負責建構和維護 Android Studio (基於 IntelliJ-IDEA) 和 Visual Studio Code (VS Code) 的支援。我們將程式碼自動完成、語法高亮、widget 編輯輔助、執行和除錯等功能整合到這些 IDE 外掛中，用於 Flutter 應用開發。Flutter 開發者們從一開始就有在使用這兩種 IDE，Android Studio 以前比 VS Code 更受歡迎，不過 VS Code 近期在 Flutter 開發中的熱度一直在穩步增加，最近甚至超過了 Android Studio，如下文圖中所示。

為了更好地理解 Flutter 開發者在選擇 IDE 時的想法，Flutter UX 團隊在 2022 年 5 月開展了針對此課題的特別調研。

在後文中，"Android Studio" 同時代表 "Android Studio" 和 "IntelliJ-IDEA"。

![△ 圖 1. 從 2021 年 8 月到 2022 年 7 月，使用每個 IDE 的 Flutter 開發者數量。圖中可以看出 VS Code 在過去幾個月中變得更受歡迎。](https://devrel.andfun.cn/devrel/posts/2022/10/3CRi1T.png)

△ 圖 1. 從 2021 年 8 月到 2022 年 7 月，使用每個 IDE 的 Flutter 開發者數量。圖中可以看出 VS Code 在過去幾個月中變得更受歡迎。

## **我們對 IDE 的現有了解**

除了知道 Flutter 開發者在 2022 年初時使用 Android Studio 和 VS Code 的人數各佔一半之外，我們根據之前的調研，對開發者的選擇有了更深入的瞭解。

**Flutter 開發者傾向於使用他們熟悉的 IDE**

在 2019 年第三季度，我們曾詢問 Flutter 開發者為什麼更喜歡他們使用最多的 IDE。最常見的回答是: 這個 IDE "我更熟悉"。

![△ 圖 2. 根據 2019 年第三季度的調研，80% 的 Android Studio 使用者和 61% 的 VS Code 使用者表示，他們選擇 IDE 是因為對其更熟悉。](https://devrel.andfun.cn/devrel/posts/2022/10/eRDu2m.png)

△ 圖 2. 根據 2019 年第三季度的調研，80% 的 Android Studio 使用者和 61% 的 VS Code 使用者表示，他們選擇 IDE 是因為對其更熟悉。

**VS Code 使用者看重在 IDE 中體驗到的速度**

上圖 (圖 2) 中另外值得一提的是，68% 的 VS Code 使用者選擇該 IDE 是因為它比其他 IDE 更快，而只有 15% 的 Android Studio 使用者如此認為。在一個開放式問題中，VS Code 使用者表示他們喜歡該 IDE 是因為它雖輕量卻具有多種擴充程式。

**VS Code 使用者對 Flutter 的 IDE 支援更滿意**

我們還詢問了對 Flutter 的 IDE 支援的滿意度，VS Code 使用者對此更為滿意。(我們記錄了開發者從 IDE 開啟調研問卷時是在使用哪個 IDE。當開發者單擊問卷連結時，我們會告知他們將對此資訊進行記錄。)

![△ 圖 3. 有 93.3% 的 VS Code 使用者對 Flutter 的 IDE 支援感到滿意，只有 85.9% 的 Android Studio 使用者對此感到滿意。](https://devrel.andfun.cn/devrel/posts/2022/10/c3qTBb.png)

△ 圖 3. 有 93.3% 的 VS Code 使用者對 Flutter 的 IDE 支援感到滿意，只有 85.9% 的 Android Studio 使用者對此感到滿意。

當然，Android Studio 的設計目的，是作為面向 Android 開發的完全整合的 IDE，因此它提供更豐富的功能集。開發者們有提到，在 Android Studio 中處理原生 Android 程式碼和使用重構等便利功能很容易。在本文的下一節中，我們將深入探討開發者們的偏好，以及為什麼儘管 Android Studio 有這些優點，開發者對在其中進行 Flutter 開發時仍不太滿意。

## **2022 年第二季度調研結果總結**

上一節的結果讓我們不禁好奇，為什麼 Flutter 開發者更樂意使用 VS Code 支援，而不是 Android Studio 支援。我們想了解 Flutter 開發者真正喜歡 VS Code 的哪些方面。

為了解這一點，我們詢問了將主要 IDE 從一個換成另一個 (既包括從 Android Studio 換成 VS Code，也包括從 VS Code 換成 Android Studio) 的開發者一組問題。我們相信這些開發者可以很好地從他們的視角告訴我們每種 IDE 的獨特價值。

首先，有更多的 Flutter 開發者從 Android Studio 換成 VS Code。

![△ 圖 4. 更多開發者將主要 IDE 從 Android Studio (藍色) 換成 VS Code (青色)，反向改換的人則很少。](https://devrel.andfun.cn/devrel/posts/2022/10/MUECW1.png)

△ 圖 4. 更多開發者將主要 IDE 從 Android Studio (藍色) 換成 VS Code (青色)，反向改換的人則很少。

如下圖所示，轉用 VS Code 的人喜歡它的效能 (82%) 和易用性 (63%)。另一方面，轉用 Android Studio 的人喜歡它的功能 (51%)、與 Flutter 工具的整合 (39%) 以及與原生平台的整合 (27%)。

![△ 圖 5. 轉用其他 IDE 的理由。](https://devrel.andfun.cn/devrel/posts/2022/10/QMEE8p.png)

△ 圖 5. 轉用其他 IDE 的理由。

仍然有大約 23% 的 Flutter 開發者同時使用 VS Code 和 Android Studio。當詢問他們為什麼使用多個 IDE 時，最常見的回答是，VS Code 使用者需要使用 Android Studio 和 Xcode 來實現特定於原生裝置的功能，例如模擬器設定、建構配置、釋出需求 (例如金鑰產生和簽名)，以及開發 Flutter + 原生混合式應用。

![△ 圖 6. 22.5% 的 Flutter 開發者同時使用 VS Code 和 Android Studio。](https://devrel.andfun.cn/devrel/posts/2022/10/p11WTQ.png)

△ 圖 6. 22.5% 的 Flutter 開發者同時使用 VS Code 和 Android Studio。

我們從調研中還了解到，不同國家或地區的 Flutter 開發者偏好不同的 IDE。儘管大多數 Flutter 開發者更偏好 VS Code，但中國的開發者相較於 VSCode (23%) 還是更偏好 Android Studio (56%)。我們發現這很有趣——Flutter 在全球範圍內都有被廣泛採用，但又往往處於不同的開發環境中。無論您來自哪個國家或地區，如果您有任何圍繞這一傾向的故事，歡迎和我們分享。

![△ 圖 7. 不同國家或地區的 IDE 使用偏好。圖表中各個國家或地區至少包含 100 名受訪者。](https://devrel.andfun.cn/devrel/posts/2022/10/QGB3Ob.png)

△ 圖 7. 不同國家或地區的 IDE 使用偏好。圖表中各個國家或地區至少包含 100 名受訪者。

## **結論**

我們的目標是提供實用且完整的開發體驗，最大限度地減少大家開始使用 Flutter 時的不便之處，並最大限度地提高開發者的工作效率。我們將基於上述以及未來的調研結果，為今後 Flutter 的 IDE 支援和文件提供路線圖。我們會先對官方網站的上手指南文件進行小幅更新，以更好地反映上文提到的 IDE 偏好和使用模式。

我們從此次及其他調研中獲取的諸多寶貴見解將確保我們聚焦於正確的領域，從而持續改進 Flutter 開發者體驗。再次感謝參與調研的每一位開發者！如果您有興趣參與未來的 [使用者調研](https://flutter.dev/research-signup)，歡迎在官網上進行註冊。我們將在下個季度和大家分享第三季度的調研結果，請保持關注！
