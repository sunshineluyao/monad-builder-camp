---
timezone: UTC+8
---

# james_1022

**GitHub ID:** ARZER-TW

**Telegram:** 

## Self-introduction

Web3 暑期实习计划 - Monad Buidler Camp

## Notes

<!-- Content_START -->
# 2026-07-06
<!-- DAILY_CHECKIN_2026-07-06_START -->
今天簡單的掃過了 Monad 文件裡整理的與 Ethereum 差異清單，記幾個有感的點。

最值得想的是收費模型：Monad 按 gas limit 收費而不是實際用量，總扣款是 value + gas\_bid × gas\_limit。第一眼覺得反直覺，但回推架構就通了——非同步執行下，共識階段還不知道實際 gas 用量，只能拿 limit 當上限防 DoS。等於架構選擇的成本反映在收費上，以後在 Monad 上預估 gas limit 要抓緊，不能像以太坊那樣隨手開大。

Reserve Balance 是同個脈絡的產物：為了保證進共識的交易付得起錢,所以鏈上會看到「已上鏈但因餘額不足而 revert」的交易，gas 照扣。文件強調這不算協議層差異（以太坊本來就有 revert 交易上鏈），但跟一般預期確實不同。

其他幾點簡記：

-   合約大小上限 128 KB（以太坊 24 KB），init code 上限 256 KB
    
-   部分 opcode 和預編譯重新定價，反映 Monad 優化後的資源稀缺性變化
    
-   原生支援 secp256r1 (P256) 預編譯（位址 0x0100，EIP-7951），可直接在鏈上驗 WebAuthn / passkey 簽章，做帳戶抽象不用再用合約硬刻 P256 燒 gas
    
-   不支援 EIP-4844 blob 交易
    
-   沒有全域 mempool，交易只轉發給接下來幾個 leader。交易傳播模型跟以太坊完全不同，對做套利的人影響很大
    
-   EIP-7702 委託過的 EOA 有兩個限制：餘額不能低於 10 MON（綁 Reserve Balance 規則）、被當合約呼叫時禁用 CREATE/CREATE2
    
-   吞吐量太高導致 full node 不提供任意歷史狀態查詢
    

整體感想：這些差異幾乎都能回推到同一個根源——非同步、平行執行的架構。gas limit 收費、Reserve Balance、local mempool、砍歷史狀態，全是執行與共識解耦後為了讓系統撐住的配套。EVM 相容只是介面層，底下的資源模型已經是另一套東西。要搬合約過去，gas 預估和任何依賴 mempool 行為的邏輯都得重新驗，本地開發記得用官方客製的 Monad Foundry 對齊鏈上行為。
<!-- DAILY_CHECKIN_2026-07-06_END -->
<!-- Content_END -->
