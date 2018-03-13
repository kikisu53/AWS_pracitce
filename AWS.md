# VPC (virtual private cloud，虛擬機房)
## 建立VPC的步驟
1.	切換右上角的區域，即選擇虛擬機房的所在地。
    a.	不同區域價格不同，延遲的時間也不同。
    b.	每個區域至少有兩個AZ(availability zones)，每個AZ至少有一個機房。
    c.	簡單來說，一個AZ就是一個AWS的實體機房組。
2.	搜尋VPC服務 → Your VPCs → Create VPC
3.	Name tag: 自訂。IPv4 CIDR block: 虛擬網段和遮罩，可用遮罩/16~/28。
    Tenancy: Default(共用機器)，Dedicated(專用機器，費用高很多)。
4.	Create VPC
5.	State: available，代表建立成功。

## 建立VPC相關的資訊
1.	VPC建立後，可以看到以下資訊。
    ![VPC Summary](/VPC/summary.png)
2.	DHCP options set
    ![DHCP Summary](/VPC/dhcp.png)
3.	Route table(default)
    a.	VPC中Subnets預設的routing table：只要沒有綁定routing table的subnet，一律使用此routing table，因此不要變更此張default routing table。
    b.	Summary - Explicitly Associated With：綁定0個subnet，可到subnet associations修改。
        Summary - Main：yes表default routing table。
    ![Route Table Summary](/VPC/route_summary.png)
    c.	Routes：要傳送給10.0.x.x(Destination)的訊息，都走local。換句話說，10.0.x.x的電腦都是鄰居，不需要上internet。
    ![Route Default](/VPC/route_default.png)
4.	Network ACL
    a.	Associated with：綁定的subnet為0，可到subnet associations修改。
    ![Route Default](/VPC/Network_ACL_summary.png)
    b.	Inbound Rules：傳入訊息的限制，Type表可以用哪種通訊協定進來，Source表可以允許哪些來源的訊息近來。
    ![Route Default](/VPC/Network_ACL_in.png)
    c.	Outbound Rules：傳出訊息的限制，Type表可以用哪種通訊協定出去，Destination表可以發送的目的地。
    ![Route Default](/VPC/Network_ACL_out.png)
5.	Security Groups(SG)
    a.	Group description：該SG為VPC default的SG。
    ![Route Default](/VPC/sg_summary.png)
    b.	Inbound Rules和Outbound Rules參考Network ACL說明。
    ![Route Default](/VPC/sg_in.png)
    ![Route Default](/VPC/sg_out.png)
 
 
問題
ACL和SG的差異：都是防火牆
ACL的inbound rules和outbound rules? 還有上面allow下面deny =>???
subnet 少5個ip，除了頭尾跟gateway
security group: private source customer = public sg id不通，改成0.0.0.0/0可通？


建立對外連線Internet Gateway
1.	VPC服務(左邊選單) → Internet Gateways → Create Internet Gateway
2.	Name tag: 自訂。
3.	Attach to VPC → 選擇要綁訂的VPC → Yes, Attach。
4.	State: attached，代表綁訂成功。


建立子網段Subnet


 
EC2 (Elastic Compute Cloud，伺服器)



 
補充
虛擬網段和遮罩
