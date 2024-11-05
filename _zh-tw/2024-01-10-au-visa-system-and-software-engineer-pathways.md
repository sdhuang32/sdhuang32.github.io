---
title: "澳洲技術移民分享 (1) 簽證系統簡介與軟體工程師路線考量"
excerpt: "跟大家分享澳洲簽證系統大方向以及適合軟體工程師的策略選擇"
classes: wide
tags:
  - Skilled Migration
  - Australia
  - Visa
toc: true
date: 2024-01-11
last_modified_at: 2024-11-05
---

## 前言

澳洲移民 (目標: 取得永久居留簽證，成為 "永久居民" "Permanent Resident"，簡稱 PR) 可以透過[許多不同的簽證類別](https://immi.homeaffairs.gov.au/visas/permanent-resident/visa-options)，有錢可以投資移民，有子女、配偶可以擔保的話可以走家庭類移民，擁有澳洲產業所需要的高技術工作者則可以走技術移民，本文將會分享適合軟體工程師 (或在澳洲更廣泛的稱呼 "IT") 的技術移民路線與策略選擇。

*聲明: 本人是軟體工程師而非移民律師/代辦，文章內容為自身經驗以及查閱相關資訊消化後之成果，我盡力確保資訊直至文章最後更新日止為正確，若見錯誤還請留言或郵件告知。若有疑問歡迎詢問，我會盡力回答，不過還是建議前往各官方網站查證最新資訊，以及諮詢專業移民律師/代辦。*


## 概述

澳洲的技術移民簽證，簡單講分成兩個主要路線: "打分邀請" 跟 "雇主擔保"，共通的宗旨就是希望招攬年輕英文好又高技術的從業者來為澳洲貢獻 GDP。(為求簡要，以下省略了專門給偏遠地區的簽證選項如 [491][491], [191][191], [494][494] 等，大致的概念是偏遠地區的簽證門檻比非偏遠的低一些，條件是必須在[偏遠地區](https://immi.homeaffairs.gov.au/visas/working-in-australia/regional-migration/eligible-regional-areas)求學或居住一段時間，其他的申請流程跟非偏遠的簽證大致相同)

### 打分邀請

這個路線是 "聯邦政府" ([189 visa][189]) 或 "州政府" ([190 visa][190]) 擔保你獲得永久居留，可以帶伴侶及小孩一同申請並獲簽，流程是所有申請人到系統填[意向 (EOI, Expression of Interest)](https://immi.homeaffairs.gov.au/visas/working-in-australia/skillselect/expression-of-interest) 輸入你的各項條件，政府再按照分數高低邀請申請人來申請該簽證，獲邀的申請人這時才透過 [ImmiAccount][5] 填具真正的簽證申請表以及上傳所有證明文件，讓移民局驗證你的條件是否符合要求並且都跟當初 EOI 宣告的相同或更好，若是則批簽。

需要注意你宣告的分數必須是在提交 EOI 的當下所持有的，而不能是 "預期在一段時間後移民局邀請我的時候將會擁有的"，例如兩週後英文一定可以考到幾分、一個月後就會完成學業、一個月後工作滿 N 年然後就先預支分數這種，就算收到收到邀請也會在之後遞交正式申請時被拒簽的。

可以利用[移民局官方分數試算](https://immi.homeaffairs.gov.au/help-support/tools/points-calculator)，就可以估算自己能拿多少分，還有了解移民局在乎哪些項目、想加分需要往什麼方向努力。

並可以參考[移民局官方統計數據](https://api.dynamic.reports.employment.gov.au/anonap/extensions/hSKLS02_SkillSelect_EOI_Data/hSKLS02_SkillSelect_EOI_Data.html)，看一下自己的職業想受邀需要幾分。

打分邀請不是我親身經歷的路線，因此研究不多也無法提供申請過程細節，歡迎加入台灣開發者在澳洲社群的 [Facebook][1]、[Discord][2]，澳洲職場人社群的 [Facebook][3]、[Discord][4] 跟眾多前輩討論。

個人的看法，這條路的大優點是不用仰賴雇主，也就是當前不需要有工作也能取得身份，不過競爭激烈，尤其在雪、墨等首府城市獲邀分數甚高，全世界所有人不管境內境外、留學生還是多年工作經驗者都會來申請，申請人居於被動，只能湊分等待政府邀請，不確定的時程對許多人是心理煎熬，對英文檢定分數要求極高，持有澳洲當地學歷與工作經驗加分較多因此對留學生較有利。

### 雇主擔保

這個路線則是澳洲註冊的合格雇主來擔保你獲得永久居留 ([186 visa][186])，可以帶伴侶及小孩一同申請並獲簽，這個路線不需要算分數，只要跟雇主談好讓他贊助你即可，流程是雇主先向移民局提交一份 "雇主提名" 申請，出具文件寫明他們要提名你從事某某職位，並且證明企業營運、財務狀況良好並且有真實需求 (真實業務需求與職缺，打了付費廣告卻找不到合意的當地人選) 要雇用外國人，接著簽證申請人再從 [ImmiAccount][5] 填具簽證申請表並上傳所有證明文件，讓移民局驗證雇主跟申請人的條件是否符合要求，若是則批簽。

雇主擔保是我經歷的路線 (我是副申請人)，因此能夠提供比較多細節。先簡單敘述簽證門檻: 45 歲 (含) 以下，至少三年全職的相關工作經驗 ("相關" 指的是工作內容高度符合你被提名的 [ANZSCO 職業別](https://www.abs.gov.au/statistics/classifications/anzsco-australian-and-new-zealand-standard-classification-occupations/latest-release)，例如 261313 Software Engineer，詳細定義可以在這網頁中搜尋及查閱。海外工作經驗是否被認可、是否需要當地資格證照請依據各職業查證)，英文達到 [Competent English](https://immi.homeaffairs.gov.au/help-support/meeting-our-requirements/english-language/competent-english) (雅思四個 6 以上或 PTE 四個 50 以上)，取得合格的職業鑑定 (臨時工作簽 [482][482] 兩年後轉 [186 TRT](https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/employer-nomination-scheme-186/temporary-residence-transition-stream) 則不需要)，還有最重要也最難的部分就是有一個願意贊助你的雇主。

個人的看法，這條路由於英文要求較低 (Competent English 在打分路線是加 0 分...)，對資深從業人員較有利，只要技術實力堅強，順利通過面試，雇主有擔保資格，就會幫你搞定簽證問題，高端人才從海外面試直接拿 [186 DE][186DE] 空降都是有可能的 (一些跨國大廠內轉 relocation 也屬此類)，尤其軟體工程師的海外工作經驗可以直接被認可，不需要當地學歷也不需要當地證照或政府資格，是很容易移動的職業，且申請人居於主動，一旦雇主同意，下簽指日可待。但關鍵也正難在 "有一個願意贊助的雇主" 這一項，因為 1. 澳洲雇主普遍偏好已經有 PR 有當地工作經驗的人，而且畢竟贊助要額外多花一筆錢，因此你必須強過一般當地人不少且英文不能太差 2. 有些雇主願意贊助臨時工作簽 482 沒有問題，但就是不願意贊助 186 DE/TRT，因為 482 相當於綁約讓你只能為他工作 (雖然新政已打算放寬 482 轉換雇主的限制)，一旦拿到 186 成為 PR 就代表隨時可能失去這個員工。也就是這條路不單是實力至上，很多時候還很需要機緣的，我們能做的只有靠更多更豐富的工作經驗跟主動爭取來平衡無常的運氣。

個人建議想尋找雇主擔保的話，以軟體工作而言，至少在台灣是 5 年以上經驗的 Senior Engineer 且工作內容圍繞 web service 會比較有把握，不用管職缺 JD 有沒有寫贊助簽證，直接海投，填申請表的時候都會問你持有的是什麼簽證，如果他們不考慮贊助是根本不會有下文的，所以只要公司願意跟你面試就表示有機會。以軟體類職缺而言，明說能夠贊助的公司不多，可能只有 Atlassian, Canva, Amazon 這些大廠會明確地這樣表態，還有 StackOverflow 上面有些公司也會明寫可以贊助，不過數量不多，但事實上也有蠻多公司其實可以贊助，只是沒事的話不主動提 (畢竟不會有企業優先傾向聘用外國人)，只能海投看看然後希望可以碰到有緣的雇主，用實力說服他了。

## 軟體工程師細部路線

大神級直接一步到位拿 PR:
- **858 Global Talent** [1](https://immi.homeaffairs.gov.au/visas/working-in-australia/visas-for-innovation/global-talent-independent-program) [2](https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/global-talent-visa-858): 全球人才簽證，擁有世界級成就且有推薦人推薦，特別是在澳洲政府想要的領域，高精尖人士可考慮。
- **[186 DE][186DE]**
- **[189][189]/[190][190]**

一般人循序漸進:
- **[482][482] -> [186 DE/TRT][186]**: 資深從業者可考慮海外直接投履歷或內推，有機會直接拿 482 工作簽 (軟體工程師為 4 年簽) 來澳工作。粗略估計，總時間成本: 2 年+; 金錢成本: 低; 風險: 若無擔保頂多暫時不赴澳。482 期間再找機會說服雇主 186 DE 或[待滿兩年 (25 Nov 2023 以前要待滿三年) 走 186 TRT](https://immi.homeaffairs.gov.au/news-media/archive/article?itemId=1136)。若雇主只承諾 482 不願承諾 186，則持有 482 期間也有被裁員、雇主倒閉、最終不願擔保 186 等風險，若必須轉換雇主也須於時限內完成。建議 482 期間不斷明示暗示旁敲側擊 186 的可能性，可以直接 186 DE 就不要乖乖等做滿兩年再轉 186 TRT。
- **[417][417] -> [186 DE][186DE]**: 417 打工度假簽起手，直接境內找軟體工作，417 不限制產業與職位且可馬上上工，雖有[同一雇主不得超過六個月](https://immi.homeaffairs.gov.au/what-we-do/whm-program/specified-work-conditions/6-month-work-limitation)的限制，時程很趕，但只要雇主願意，足以證明實力並開始跑 186 DE 流程。台灣人滿 31 歲前適用。粗略估計，總時間成本: 1 年+; 金錢成本: 低; 風險: 417 找擔保若沒做好準備或運氣不佳，有可能找不到擔保，不過一旦找到，只要跟雇主談好，若趕不上直接 186 也可先 482。
- **[417][417] -> [482][482] -> [186 DE/TRT][186]**: 乘上，許多資深台灣開發者前輩走的路線，417 找軟體工作並轉換為 482 工作簽，再接 186 DE 或待滿兩年接 186 TRT，時程較為充裕。台灣人滿 31 歲前適用。粗略估計，總時間成本: 2-3 年+; 金錢成本: 低; 風險: 乘上，417 找擔保需妥善準備，再加上持有 482 期間可能的變數。
- **[417][417] -> [500 學生簽][500] -> [485 畢業工作簽][485] -> [189][189]/[190][190](/[491][491]->[191][191])**: 通常是打工度假後喜歡澳洲，想繼續留下來工作生活，轉學生簽然後再拿畢業工作簽 (學士 2 年碩士 3 年)，在念書跟工作期間想盡辦法提升分數，PY (職業年)，英文想辦法拼雅思 8/PTE 79，甚至住偏遠地區，NAATI 加分等，然後申請 189, 190(/491->191)，工作年資足夠以後也可同時爭取 482/186。台灣人滿 31 歲前適用。粗略估計，總時間成本: 6 年+; 金錢成本: 高; 風險: 因為已唸書貢獻 GDP 了因此可保證 485 進行後續營運，但是費時長所以也有政策變動或分數變化風險，小心夜長夢多。
- **[417][417] -> [500][500] -> [485][485] -> [482][482] -> [186 DE/TRT][186]**: 乘上，粗略估計，總時間成本: 6 年+; 金錢成本: 高; 風險: 485 後若能轉 482 則風險下降，可雙管齊下投 189/190 EOI 並探詢雇主 186 可能性。
- **[500][500] -> [485][485] -> [189][189]/[190][190](/[491][491]->[191][191])**: 乘上，粗略估計，總時間成本: 5 年+; 金錢成本: 高; 風險: 見上。
- **[500][500] -> [485][485] -> [482][482] -> [186 DE/TRT][186]**: 乘上，粗略估計，總時間成本: 5 年+; 金錢成本: 高; 風險: 見上。

## 關於[留學代辦、移民代辦、移民律師](https://www.mara.gov.au/get-help-visa-subsite/FIles/giving_immigration_assistance_in_australia_english.pdf)與面對簽證申請時的觀念

**留學代辦**通常不收顧問費/代辦費，目標是把你跟某校某學程媒合上以賺取**校方給的佣金**，不完全是為你服務的，他們的利益跟你自身的利益 (畢業後能找到還算滿意的工作、拿到 PR) 可能不完全相同，建議多方查證，勿盡信，以自身需求為核心判斷各選項是否合適。

**移民代辦**收取顧問費/代辦費來代替你提交簽證申請，作為中間人，向你要個人資訊以及相關證明文件，幫你填申請表以及上傳附件到 [ImmiAccount][5]，處理和移民局的書信往來，及一些簡單問題。不過若牽涉到比較進階的法律問題，他們可能就沒有辦法處理了，這時就需要尋求 **移民律師** 的幫助，他們才是真正熟讀移民法且有能力替你打官司的人，尤其是如果曾經有過被拒簽、被驅逐、逾期拘留、其他犯罪史，或不服簽證審批結果想要上訴[仲裁庭](https://www.aat.gov.au/)，或個人學經歷較為複雜而擔心所有這些可能的風險，或許從一開始就選擇移民律師會是一個比較好的選項，當然收費也會比較貴就是了。

另外，想要特別強調的觀念是，在申請簽證時即便找了代辦或律師，申請人還是要[對所有提交的文件正確性負全責](https://immi.homeaffairs.gov.au/help-support/who-can-help-with-your-application/using-a-migration-agent#content-index-4)，也就是如果因為提交了錯誤或具誤導性的資訊而導致簽證被拒簽或者下簽後被取消，想要以 "代辦出錯" 的理由上訴要求撤銷處分，是非常困難的 (花錢花時間找好律師還未必成功)。移民局會預期所有資料都是經過你的同意、充分溝通確認後提交的，道義上你也不應該期待代辦會完全站在你的角度來準備所有資料，畢竟這是自己未來要持有的簽證，任何人都只是幫你或給意見而已，千萬別下意識認為代辦比自己有經驗一定是對的，建議一定要親自查證相關資訊，才能夠驗證代辦說的是否正確、合法，也一定要要求代辦把他填好的申請表在送出前給你看過，自己審核一遍所有細節。過程中若代辦真的出錯讓你很不爽，可以[提交抱怨](https://www.mara.gov.au/get-help-with-a-visa/help-from-registered-agents/steps-to-choose/complain)給移民代辦主管機關。

我個人曾碰過代辦只會當搬運工，我提供的初始文件不符合規範他也沒辨認出來，幫我填的申請表有欄位跟我提供的資訊不同等狀況，也聽過有人分享國籍從 Taiwan 被寫成 China 等問題。尤其移民局對申請人的身份 (Identity) 正確性、文件是否誤導或造假看得非常重，有任何這類狀況都可能對當前簽證或未來的申請造成[不良影響例如數年內拒簽](https://immi.homeaffairs.gov.au/help-support/meeting-our-requirements/providing-accurate-information)。其實各簽證的要求在移民局官方網頁都公開寫得非常清楚，甚至有一個 "Step by step" 的 tab 告訴你流程與細節，也可以直接到 [ImmiAccount][5] 裡面點選你要的簽證，自行填表 (不確定的欄位先暫時亂填)，親自走過一輪就會知道需要準備哪些跟缺少哪些資料了，若很確定自己學經歷符合資格且背景單純，DIY 申請都是可以的。

[491]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/skilled-work-regional-provisional-491>
[191]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/skilled-regional-191>
[494]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/skilled-employer-sponsored-regional-494>
[500]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/student-500>
[485]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/temporary-graduate-485>
[189]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/skilled-independent-189>
[190]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/skilled-nominated-190>
[417]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/work-holiday-417>
[482]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/temporary-skill-shortage-482>
[186]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/employer-nomination-scheme-186>
[186DE]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/employer-nomination-scheme-186/direct-entry-stream>

[1]: <https://www.facebook.com/groups/tw.devs.in.au/>
[2]: <https://discord.gg/23KQEcE>
[3]: <https://www.facebook.com/groups/australiacareerforum>
[4]: <https://discord.gg/cnwFRQaabz>
[5]: <https://online.immi.gov.au/ola/app>
