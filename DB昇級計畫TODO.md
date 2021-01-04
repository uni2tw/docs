將DB昇級Windows 2016, SQL Server 2016計劃

第一階段
1. 將 web 連線字串，從17alwayson 改成連 Ironman , 並確認web是否正常寫入。
2. 報表伺服器，連線字串改成連 Ironman，並確認相關服務是否正常。
3. 停止 Magneto Sql Server
4. 在Ironman 移除 alwayson group - 17alwayson
5. 重灌 Magneto 到 windows server 2016, 加入ad後，用ad帳號登入後，安裝 sql server 2016 ,  failover clustering，並做完windows update

第2階段-1
6. 暫停 Ironman 的定時備份機制
7. 手動進行資料備份與還原，對Ironman做full backup 並還原到 Magneto (還原為 No Recovery)
第2階段-2(此階段需要停止服務，需告知相關人員)
8. Ironman切換成 singlue user  模式，防止新資料的寫入。
9. 手動進行資料備份與還原，對Ironman做差異備份，並還原到 Magneto，讓Magneto的資料與Ironman是資料是一致的(還原為 No Recovery)
10. 將Magneto從No Recovery 設定成 Recovery 模式，可進行讀寫。(RESTORE DATABASE ... WITH RECOVERY)
11. 將 web 連線字串，從 Ironman 改成 Magneto，並重啟web服務讓連線確認生效。需確認web正常。
12. 將報表伺服器連線字串改成Magneto

第3階段-
13. 重灌Ironman 到 windows server 2016, 加入ad後，用ad帳號登入後，安裝 sql server 2016 ,  failover clustering
14. 設定新的Failover Clustering，設定見證模式(File Share)
15. Magneto建立新的alwayson group - 17alwayson2020
16. Magneto建立新的alwayson, 加入ironman 節點，並將4個database 依續建立

