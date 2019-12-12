# 序言

如果近幾年從業於軟體工程，特別是伺服器端和後端系統開發，那麼您很有可能已經被大量關於資料存儲和處理的時髦詞彙轟炸過了： NoSQL！大數據！Web-Scale！分片！最終一致性！ACID！ CAP定理！雲服務！MapReduce！即時！

在最近十年中，我們看到了很多有趣的進展，關於資料庫，分散式系統，以及在此基礎上構建應用程式的方式。這些進展有著各種各樣的驅動力：

* 穀歌，雅虎，亞馬遜，臉書，領英，微軟和推特等互聯網公司正在和巨大的流量/資料打交道，這迫使他們去創造能有效應對如此規模的新工具。
* 企業需要變得敏捷，需要低成本地檢驗假設，需要通過縮短開發週期和保持資料模型的靈活性，快速地回應新的市場洞察。
* 免費和開源軟體變得非常成功，在許多環境中比商務軟體和定制軟體更受歡迎。
* 處理器主頻幾乎沒有增長，但是多核處理器已經成為標配，網路也越來越快。這意味著並行化程度只增不減。
* 即使您在一個小團隊中工作，現在也可以構建分佈在多台電腦甚至多個地理區域的系統，這要歸功於譬如亞馬遜網路服務（AWS）等基礎設施即服務（IaaS）概念的踐行者。
* 許多服務都要求高可用，因停電或維護導致的服務不可用，變得越來越難以接受。

**資料密集型應用（data-intensive applications）**正在通過使用這些技術進步來推動可能性的邊界。一個應用被稱為**資料密集型**的，如果**資料是其主要挑戰**（資料量，資料複雜度或資料變化速度）—— 與之相對的是**計算密集型**，即處理器速度是其瓶頸。

説明資料密集型應用存儲和處理資料的工具與技術，正迅速地適應這些變化。新型資料庫系統（“NoSQL”）已經備受關注，而訊息佇列，緩存，搜索索引，批次處理和流處理框架以及相關技術也非常重要。很多應用組合使用這些工具與技術。

這些生意盎然的時髦詞彙體現出人們對新的可能性的熱情，這是一件好事。但是作為軟體工程師和架構師，如果要開發優秀的應用，我們還需要對各種層出不窮的技術及其利弊權衡有精准的技術理解。為了獲得這種洞察，我們需要深挖時髦詞彙背後的內容。

幸運的是，在技術迅速變化的背後總是存在一些持續成立的原則，無論您使用了特定工具的哪個版本。如果您理解了這些原則，就可以領會這些工具的適用場景，如何充分利用它們，以及如何避免其中的陷阱。這正是本書的初衷。

本書的目標是説明您在飛速變化的資料處理和資料存儲技術大觀園中找到方向。本書並不是某個特定工具的教程，也不是一本充滿枯燥理論的教科書。相反，我們將看到一些成功資料系統的樣例：許多流行應用每天都要在生產中會滿足可擴展性、性能、以及可靠性的要求，而這些技術構成了這些應用的基礎。

我們將深入這些系統的內部，理清它們的關鍵演算法，討論背後的原則和它們必須做出的權衡。在這個過程中，我們將嘗試尋找**思考**資料系統的有效方式 —— 不僅關於它們**如何**工作，還包括它們**為什麼**以這種方式工作，以及哪些問題是我們需要問的。

閱讀本書後，你能很好地決定哪種技術適合哪種用途，並瞭解如何將工具組合起來，為一個良好應用架構奠定基礎。本書並不足以使你從頭開始構建自己的資料庫存儲引擎，不過幸運的是這基本上很少有必要。你將獲得對系統底層發生事情的敏銳直覺，這樣你就有能力推理它們的行為，做出優秀的設計決策，並追蹤任何可能出現的問題。


## 本書的目標讀者

如果你開發的應用具有用於存儲或處理資料的某種伺服器/後端系統，而且使用網路（例如，Web應用，移動應用或連接到互聯網的感測器），那麼本書就是為你準備的。

本書是為軟體工程師，軟體架構師，以及喜歡寫代碼的技術經理準備的。如果您需要對所從事系統的架構做出決策 —— 例如您需要選擇解決某個特定問題的工具，並找出如何最好地使用這些工具，那麼這本書對您尤有價值。但即使你無法選擇你的工具，本書仍將幫助你更好地瞭解所使用工具的長處和短處。

您應當具有一些開發Web應用或網路服務的經驗，且應當熟悉關係型數據庫和SQL。任何您瞭解的非關係型數據庫和其他與資料相關工具都會有所説明，但不是必需的。對常見網路通訊協定如TCP和HTTP的大概理解是有幫助的。程式設計語言或框架的選擇對閱讀本書沒有任何不同影響。

如果以下任意一條對您為真，你會發現這本書很有價值：

* 您想瞭解如何使資料系統可擴展，例如，支援擁有數百萬用戶的Web或移動應用。
* 您需要提高應用程式的可用性（最大限度地減少停機時間），保持穩定運行。
* 您正在尋找使系統在長期運行過程易於維護的方法，即使系統規模增長，需求與技術也發生變化。
* 您對事物的運作方式有著天然的好奇心，並且希望知道一些主流網站和線上服務背後發生的事情。這本書打破了各種資料庫和資料處理系統的內幕，探索這些系統設計中的智慧是非常有趣的。

有時在討論可擴展的資料系統時，人們會說：“你又不在穀歌或亞馬遜，別操心可擴展性了，直接上關係型數據庫”。這個陳述有一定的道理：為了不必要的擴展性而設計程式，不僅會浪費不必要的精力，並且可能會把你鎖死在一個不靈活的設計中。實際上這是一種“過早優化”的形式。不過，選擇合適的工具確實很重要，而不同的技術各有優缺點。我們將看到，關聯式資料庫雖然很重要，但絕不是資料處理的終章。


## 本書涉及的領域

本書並不會嘗試告訴讀者如何安裝或使用特定的套裝軟體或API，因為已經有大量文檔給出了詳細的使用說明。相反，我們會討論資料系統的基石——各種原則與利弊權衡，並探討了不同產品所做出的不同設計決策。

在電子書中包含了線上資源全文的連結。所有連結在出版時都進行了驗證，但不幸的是，由於網路的自然規律，連結往往會頻繁地破損。如果您遇到連結斷開的情況，或者正在閱讀本書的列印副本，可以使用搜尋引擎查找參考文獻。對於學術論文，您可以在Google學術中搜索標題，查找可以公開獲取的PDF檔。或者，您也可以在 https://github.com/ept/ddia-references 中找到所有的參考資料，我們在那兒維護最新的連結。

我們主要關注的是資料系統的**架構（architecture）**，以及它們被集成到資料密集型應用中的方式。本書沒有足夠的空間覆蓋部署，運維，安全，管理等領域 —— 這些都是複雜而重要的主題，僅僅在本書中用粗略的注解討論這些對它們很不公平。每個領域都值得用單獨的書去講。

本書中描述的許多技術都被涵蓋在**大資料（Big Data）**這個時髦詞的範疇中。然而“大資料”這個術語被濫用，缺乏明確定義，以至於在嚴肅的工程討論中沒有用處。這本書使用歧義更小的術語，如“單節點”之於”分散式系統“，或”線上/互動式系統“之於”離線/批次處理系統“。

本書對自由和開源軟體（FOSS）有一定偏好，因為閱讀，修改和執行源碼是瞭解一樣東西詳細工作原理的好方法。開放的平臺也可以降低供應商壟斷的風險。然而在適當的情況下，我們也會討論專利軟體（閉源軟體，軟體即服務 SaaS，或一些在文獻中描述過但未公開發行的公司內部軟體）。

## 本書綱要

本書分為三部分：

1. 在[第一部分](part-i.md)中，我們會討論設計資料密集型應用所賴的基本思想。我們從[第1章](ch1.md)開始，討論我們實際要達到的目標：可靠性，可擴展性和可維護性；我們該如何思考這些概念；以及如何實現它們。在[第2章](ch2.md)中，我們比較了幾種不同的資料模型和查詢語言，看看它們如何適用於不同的場景。在[第3章](ch3.md)中將討論存儲引擎：資料庫如何在磁片上擺放資料，以便能高效地再次找到它。[第4章](ch4.md)轉向資料編碼（序列化），以及隨時間演化的模式。

2. 在[第二部分](part-ii.md)中，我們從討論存儲在一台機器上的資料轉向討論分佈在多台機器上的資料。這對於可擴展性通常是必需的，但帶來了各種獨特的挑戰。我們首先討論複製（[第5章](ch5.md)），分區/分片（[第6章](ch6.md)）和事務（[第7章](ch7.md)）。然後我們將探索關於分散式系統問題的更多細節（[第8章](ch8.md)），以及在分散式系統中實現一致性與共識意味著什麼（[第9章](ch9.md)）。

3. 在[第三部分](part-iii.md)中，我們討論那些從其他資料集衍生出一些資料集的系統。衍生資料經常出現在異構系統中：當沒有單個資料庫可以把所有事情都做的很好時，應用需要集成幾種不同的資料庫，緩存，索引等。在[第10章](ch10.md)中我們將從一種衍生資料的批次處理方法開始，然後在此基礎上建立在[第11章](ch11.md)中討論的流處理。最後，在[第12章](ch12.md)中，我們將所有內容匯總，討論在將來構建可靠，可伸縮和可維護的應用程式的方法。



## 參考文獻與延伸閱讀

本書中討論的大部分內容已經在其它地方以某種形式出現過了 —— 會議演示文稿，研究論文，博客文章，代碼，BUG跟蹤器，郵寄清單，以及工程習慣中。本書總結了不同來源資料中最重要的想法，並在文本中包含了指向原始文獻的連結。 如果你想更深入地探索一個領域，那麼每章末尾的參考文獻都是很好的資源，其中大部分可以免費線上獲取。


## O‘Reilly Safari

[Safari](http://oreilly.com/safari) (formerly Safari Books Online) is a membership-based training and reference platform for enterprise, government, educators, and individuals.

Members have access to thousands of books, training videos, Learning Paths, interac‐ tive tutorials, and curated playlists from over 250 publishers, including O’Reilly Media, Harvard Business Review, Prentice Hall Professional, Addison-Wesley Pro‐ fessional, Microsoft Press, Sams, Que, Peachpit Press, Adobe, Focal Press, Cisco Press, John Wiley & Sons, Syngress, Morgan Kaufmann, IBM Redbooks, Packt, Adobe Press, FT Press, Apress, Manning, New Riders, McGraw-Hill, Jones & Bartlett, and Course Technology, among others.

For more information, please visit http://oreilly.com/safari.


## 致謝

本書融合了學術研究和工業實踐的經驗，融合並系統化了大量其他人的想法與知識。在計算領域，我們往往會被各種新鮮花樣所吸引，但我認為前人完成的工作中，有太多值得我們學習的地方了。本書有800多處引用：文章，博客，講座，文檔等，對我來說這些都是寶貴的學習資源。我非常感謝這些材料的作者分享他們的知識。

我也從與人交流中學到了很多東西，很多人花費了寶貴的時間與我討論想法並耐心解釋。特別感謝 Joe Adler, Ross Anderson, Peter Bailis, Márton Balassi, Alastair Beresford, Mark Callaghan, Mat Clayton, Patrick Collison, Sean Cribbs, Shirshanka Das, Niklas Ekström, Stephan Ewen, Alan Fekete, Gyula Fóra, Camille Fournier, Andres Freund, John Garbutt, Seth Gilbert, Tom Haggett, Pat Hel‐ land, Joe Hellerstein, Jakob Homan, Heidi Howard, John Hugg, Julian Hyde, Conrad Irwin, Evan Jones, Flavio Junqueira, Jessica Kerr, Kyle Kingsbury, Jay Kreps, Carl Lerche, Nicolas Liochon, Steve Loughran, Lee Mallabone, Nathan Marz, Caitie McCaffrey, Josie McLellan, Christopher Meiklejohn, Ian Meyers, Neha Narkhede, Neha Narula, Cathy O’Neil, Onora O’Neill, Ludovic Orban, Zoran Perkov, Julia Powles, Chris Riccomini, Henry Robinson, David Rosenthal, Jennifer Rullmann, Matthew Sackman, Martin Scholl, Amit Sela, Gwen Shapira, Greg Spurrier, Sam Stokes, Ben Stopford, Tom Stuart, Diana Vasile, Rahul Vohra, Pete Warden, 以及 Brett Wooldridge.

更多人通過審閱草稿並提供回饋意見在本書的創作過程中做出了無價的貢獻。我要特別感謝Raul Agepati, Tyler Akidau, Mattias Andersson, Sasha Baranov, Veena Basavaraj, David Beyer, Jim Brikman, Paul Carey, Raul Castro Fernandez, Joseph Chow, Derek Elkins, Sam Elliott, Alexander Gallego, Mark Grover, Stu Halloway, Heidi Howard, Nicola Kleppmann, Stefan Kruppa, Bjorn Madsen, Sander Mak, Stefan Podkowinski, Phil Potter, Hamid Ramazani, Sam Stokes, 以及Ben Summers。當然對於本書中的任何遺留錯誤或難以接受的見解，我都承擔全部責任。

為了幫助這本書落地，並且耐心地處理我緩慢的寫作和不尋常的要求，我要對編輯Marie Beaugureau，Mike Loukides，Ann Spencer和O'Reilly的所有團隊表示感謝。我要感謝Rachel Head幫我找到了合適的術語。我要感謝Alastair Beresford，Susan Goodhue，Neha Narkhede和Kevin Scott，在其他工作事務之外給了我充分地創作時間和自由。

特別感謝Shabbir Diwan和Edie Freedman，他們非常用心地為各章配了地圖。他們提出了不落俗套的靈感，創作了這些地圖，美麗而引人入勝，真是太棒了。

最後我要表達對家人和朋友們的愛，沒有他們，我將無法走完這個將近四年的寫作歷程。你們是最棒的。

