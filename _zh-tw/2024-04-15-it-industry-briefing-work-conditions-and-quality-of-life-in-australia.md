---
title: "澳洲技術移民分享 (2) IT 產業概況、勞動條件、生活品質"
excerpt: "跟大家分享自己對澳洲 IT 產業的粗略觀察以及在當地工作生活起來大概會長什麼樣子"
classes: wide
tags:
  - Skilled Migration
  - Australia
toc: true
date: 2024-04-15
last_modified_at: 2024-04-15
---

*聲明: 本人是軟體工程師而非移民律師/代辦，文章內容為自身經驗以及查閱相關資訊消化後之成果，我盡力確保資訊直至文章最後更新日止為正確，若見錯誤還請留言或郵件告知。若有疑問歡迎詢問，我會盡力回答，不過還是建議前往各官方網站查證最新資訊，以及諮詢專業移民律師/代辦。*

## 產業與職缺分佈

一言以蔽之，澳洲的軟體工程工作機會大部分都是架構在雲上的 web services，像台灣那樣軟硬體整合一起賣的科技製造業、IC 設計/製造、FPGA、IoT 裝置服務等等較少，因此韌體類、嵌入式系統、Kernel、實體伺服器管理、路由器、無線網路這類的工作也較為稀少，也就是職稱/職缺內容關鍵字數量大概是這樣: frontend, backend, fullstack, data, AI/ML, QA/testing, DevOps, SRE, platform, cloud, cyber security > > > android, iOS, embedded, firmware, kernel, system admin, DBA, hypervisor, baremetal, wireless。職缺中的雲平台數量分佈: AWS > Azure > > GCP。

就我體感，澳洲重視實際工作經驗勝過學歷或證照，也蠻重視技能的直接符合度。我之前的一位澳洲人主管年齡跟我一樣大，但他高中畢業就出來工作一路從 IT Helpdesk 做上來，當我主管的時候已是超過 10 年經驗的 Lead Cloud Engineer，我則處在 6 年左右經驗 Mid-Senior 的位置而已。當然他是土生土長當地人，有文化因素，也跟簽證有關係，外國人想來工作連大學學歷都沒有的話鐵定是非常困難，我想表達的意思是許多台灣人會認為來唸書補學歷對找工作有幫助，我認為不見得，若是為了轉專業或簽證目的那某種程度同意，但若是台灣已是資工學士學歷，一定是多累積幾年實務經驗會對找工作更有幫助。 甚至，軟體工程師申請 [482][482] 工作簽不需要 [ACS 職業鑑定](https://www.acs.org.au/msa.html) (ACS 會要求學歷)，而 482 簽證本身並不要求申請人必須是相關科系畢業，也就是所有非本科的軟體工程師只要有 2 年全職工作經驗且能提供證明文件 (reference letter, payslips 等) 就符合簽證申請資格。這個事實已經過台灣開發者在澳洲社群 ([Facebook][1], [Discord][2]) 的多位前輩親身驗證過了，也就是說唯一需要努力的事情就是用實力說服雇主聘你而已，學歷是真不必要。

再以我自己作為 DevOps Engineer 的情境來舉例，假設去應徵一間做 customer facing web service 的公司，同為雲平台，公司用 AWS 但你只用過別家的話就是吃虧，有證照也沒用，除非非常資深然後可以在面試中用流暢的口才跟實際案例證明你的其他雲經驗是 transferable。而且會要求維運 Production 的經驗，假設他們用 Kubernetes 然後你只是自己玩過 Kubernetes 或說在 Non-production 用過或是公司內部系統用過，那被刷掉的機會就很大了。如果有 PR (永居簽證) 的話還好說，假如需要公司贊助的話難度只會更高。所以如果沒身分的話我會選擇先在台灣找個用 AWS, Kubernetes, Terraform 等主流平台的公司磨練幾年，至少成為 5 年以上經驗的 Senior 再來澳。

## 求職平台

我個人認為最多機會以及最好用的是 [LinkedIn](https://www.linkedin.com/) 以及 [Seek](https://www.seek.com.au/)，歡迎上此二平台搜尋工作機會親身感受當地職缺重視的技能。人還沒到澳洲也可以把 LinkedIn profile 的地區改到澳洲，這邊獵人頭風氣非常盛行，可能會有獵人頭直接傳訊息或打電話聯繫。不過若還沒有能工作的長期簽證，獵人頭幫助有限，畢竟是比較不好賣的商品，可以跟他們聊天練練英文，了解當前市場狀態及薪水就好，建議還是主動投公司自己開出來的職缺會更有機會。

## 薪資範圍

以下是我個人觀察到的本薪 (base salary) 區間，單位為"澳幣/年"，不包括法定雇主提撥養老金、分紅、股票或其他激勵獎金，依地點、產業、公司規模等變因可能有不小的差異:

- Graduate: 50K - 80K
- Junior (約 1-3 年經驗): 60K - 100K
- Mid level (約 2-5 年經驗): 80K - 140K
- Senior (約 5 年以上經驗): 130K - 200K
- Lead/Staff/Principal: 160K - $$$

## 企業文化

可以參考[另一篇拙作](/zh-tw/cultural-experience-of-software-industry-in-taiwan-and-australia)。

## 勞動條件與生活品質

既然考慮來澳洲工作，工作生活的條件與平衡自然是考量重點，以下是一些事實及多少帶有偏見的個人心得，請謹慎服用並自行查證，以及參考其他資訊分享。

### 法定基礎勞工權益

在全世界範圍內都相當優秀，我這提幾項最重要的:
- [每年帶薪特休 20 天][20]，無論年資。同一間公司服務特別久的人額外還有 [Long Service Leave](https://www.fairwork.gov.au/leave/long-service-leave)。
- [每年帶薪病假/照顧假 10 天][10]，無論年資。雇主可要求提供證明，但實務上偶爾一天或兩天的病假我從來沒碰過或聽過被要求提供的案例。
- [雇主須額外提撥薪水 11% 進到個人退休金帳戶][11]。

### 工時

[法定週工時 38 小時][38]，因此每天工時就是原則上朝九晚五，一個重點是午餐被視為包括在工時之內，法定其實是說勞雇雙方可在合約中約定 unpaid breaks 啦，但我還沒看過有公司真的硬性規定的，同時由於軟體工程師通常都不需要打卡或記錄工時，因此我願稱之為 "文化上的彈性安排"，想要前後挪移日工時也可以 (例如 8-16, 7-15, 10-18)，而且其實除非中午聚餐，否則很多人都是簡單吃個三明治或自己帶的便當，就繼續工作了，從沒看過有人在午睡的，我感受到的氛圍是大家都想緊湊地把工作趕快在工時內完成，然後就早點下班回家，運動、接小孩、陪家人等等做自己的事情去了，沒在拖泥帶水的。

### 地緣因素

身為離亞洲最近的英語系 ([五眼聯盟](https://zh.wikipedia.org/wiki/%E4%BA%94%E7%9C%BC%E8%81%AF%E7%9B%9F)) 國家，時差最多 3 小時，飛機直飛東岸大城約 8.5 - 9.5 小時為五眼聯盟最少，不論電話聯絡、返鄉探親、做生意都很方便。 順帶一提雖然語言文化不是，但澳洲其實幾乎快可以算是亞洲國家了，因為地緣關係，不管是經濟活動還是文化交流都跟亞洲國家來往密切，移民也大半是亞裔，亞洲餐廳與亞洲超市甚多。

### 氣候

[沿海氣候宜人且多元](https://en.wikipedia.org/wiki/Climate_of_Australia)，喜歡什麼樣的氣候都可以在澳洲找到適合自己的，以工作機會較多的東部沿海來說，最主要就是年日照時數長，且較為乾燥，不會濕黏不舒服，但是須注意皮膚保養以免乾裂，且晴天時太陽非常烈因此務必防曬乳及太陽眼鏡不離身，否則皮膚眼睛很快就會壞掉。海岸線風景漂亮，沙灘眾多，水上活動很多也很盛行，然後整片大陸都相當平坦，[最高峰僅 2228 m](https://en.wikipedia.org/wiki/Mount_Kosciuszko)，喜愛大山的朋友就很遺憾了，[滑雪還是可以的不過地點比較侷限](https://en.wikipedia.org/wiki/Skiing_in_Australia)。

### 花費感受

凡是不需要人工及服務的東西，例如超市中的食品、民生用品、能大量生產的商品 (包括手機電腦等電子產品) 價格都跟台灣差不多，因此在家自己煮以及自己動手解決生活大小事可以省下大量花費，房價所得比對普通打工人來說還是比台灣好很多，車價有時候可比台灣便宜數十萬台幣。不過如果是需要別人幫你服務的事情，或尋求專業知識技術的協助，那麼費用就馬上變得相當驚人，外食、按摩、水電維修、庭院草木修剪、各種仲介/顧問/管理、律師會計師等等的費用至少是台灣兩三倍或以上，深刻印證了澳洲是全世界法定基礎工資最高([Ref 1](https://zh.wikipedia.org/wiki/%E5%90%84%E5%9C%8B%E6%9C%80%E4%BD%8E%E5%B7%A5%E8%B3%87%E5%88%97%E8%A1%A8), [Ref 2](https://www.fairwork.gov.au/pay-and-wages/minimum-wages#national))，非常重視人工及專業價值的地方。

### 稅務

大部分的稅例如個人所得税、企業所得稅、消費稅 (GST)、健保費都是全國統一，由聯邦收取及管理，不像美加那樣所得稅、消費稅及醫療由各州/省自訂。我個人十分欣賞澳洲的做法，因為從民眾的角度來看這讓我們的生活比較簡單、無腦，不需要因為換州居住就要改變申報所得稅的方式及税率，也不會因為跨州居住旅行有醫療需求時還得換醫保卡之類跟州政府交涉，也不需要在看到商品標價的時候自行計算加上該州消費稅以及小費後的結果，順帶一提澳洲沒有強迫小費文化，因此商品標價是真正的所見即所得，以上這些都這讓人民的財務管理容易許多，心理壓力也輕上許多。

跟工作最相關的個人所得稅採取階梯稅率，[這個網站](https://paycalculator.com.au/)有好用的計算機可以多加利用。如果買房則會產生印花稅、地價稅等等州稅，各州税率、收取條件差異頗大，請參閱各州官方網站。

### 交通

交通的部分一言以蔽之就是比台灣好上百倍。不管到哪裡都一定有[**實體人行道**](https://twitter.com/TransportforNSW/status/1765558841855574361)，車速雖不像日本那麼溫和但禮讓行人同樣是每個人內化並實踐在每一天的常識，機車很少，道路工程良好且合理，善用車道寬度、減速丘、圓環、行人庇護島等措施來控制車速及保障行人安全，還有例如 school zone 在上學放學限速 40km/hr 這樣特別保護學生的措施，以及嚴格的駕照核發、升級制度 (例如[新州 Learner -> P1 -> P2 -> Full license](https://www.service.nsw.gov.au/guide/getting-a-nsw-driver-licence))，都讓整體的交通環境安全許多。

### 醫療

澳洲醫療體系實行分級制，也就是除了真正的緊急情況直接去急診室外，任何疑難雜症都得先去看家庭醫生 (GP, General Practitioner)，僅有當他無法處理時，才會轉介你去看專科醫生或者去醫院，沒有 GP 的轉介信是沒法直接去專科診所或醫院掛號的，如果直接殺去急診室但被判斷沒有立即的危險，就會把你晾著慢慢處理，這樣才能有效分配資源給真正需要的病人。公私立院所並存，公立醫院手術需要排隊較久的時間，想快的話則可以去私立醫院。

還沒有當地健保時可買大公司如 [Bupa](https://www.bupa.com.au/health-insurance/cover/overseas-visitors), [Medibank](https://www.medibank.com.au/overseas-health-insurance/) 的私人健康保險，跟大部分公立私立醫院還有家庭醫生診所都有合作 (須確認該院所是否接受你的保險)，給付處方藥，抽血驗尿 X 光等檢查也有涵蓋 (須確認該檢驗所是否接受你的保險)。 能夠講中文的 GP 有很多，可以在 [Healthengine](https://healthengine.com.au/) 上面設篩選條件去找最適合你的診所。

### 其他

- 使用公制單位，公里公斤公升等，無需額外適應。
- 較少難民、槍枝、毒品問題: 沒有陸路與其他國家連接，非法移民難以偷渡，旅遊簽、學生簽審批嚴格。 自從 1996 發生在塔州的[亞瑟港槍擊案](https://zh.wikipedia.org/wiki/%E4%BA%9A%E7%91%9F%E6%B8%AF%E6%9E%AA%E5%87%BB%E6%A1%88)後，澳洲從此頒布嚴格的槍枝管理法案，只有打獵或射擊俱樂部等用途可合法申請持有。 毒品部分就我所知，最前衛的首都領地對於持有少量自用大麻、古柯鹼、海洛因、冰毒、搖頭丸有除罪化的措施 (依然違法但不到刑事犯罪，只罰錢及勒戒，[參考](https://www.abc.net.au/news/2023-10-28/canberra-drug-decriminalisation-laws-begin-today/103032128))，其他大部分州則是連持有大麻都不行。

[20]: <https://www.fairwork.gov.au/leave/annual-leave#how-much-annual-leave-an-employee-gets>
[10]: <https://www.fairwork.gov.au/leave/sick-and-carers-leave#paid-sick-leave>
[11]: <https://www.ato.gov.au/businesses-and-organisations/super-for-employers/paying-super-contributions/how-much-super-to-pay#ato-Workouthowmuchtopay>
[38]: <https://www.fairwork.gov.au/employment-conditions/hours-of-work-breaks-and-rosters/hours-of-work#maximum-weekly-hours>
[482]: <https://immi.homeaffairs.gov.au/visas/getting-a-visa/visa-listing/temporary-skill-shortage-482/medium-term-stream>

[1]: <https://www.facebook.com/groups/tw.devs.in.au/>
[2]: <https://discord.gg/23KQEcE>
