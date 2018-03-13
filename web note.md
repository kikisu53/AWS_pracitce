網路通訊協定：OSI & TCP/IP

第三層 ip  source ip port - destant ip port - message => data link layer （第二層）
route ip認址 dhcp
第四層  tcp 可靠網路連線，整理package
一到六層 broadcast => 不安全
五到七層。純文字協定 http samp pop3…
http協定：connection less, session less，空白行代表段落結束
Https: x509加密

一層 定義傳輸資料的協定 只要可以透過物理變化明確定義0,1者
例如 RJ45 雙絞線 消除對其他線的干擾 迴轉半徑最小10 太長時電阻會抵銷高電位導致訊號消失，光纖 光速3*10^8 光纖材質折射率 1.48 => 光纖速度 2*10^8，全反射，折射後高低峰差距變下
藍芽 802.11(wifi) 等等
Hub 管到第一層的機器

二層 ip & mac 取得與告知 only broadcast 
switch 管道第二層的機器：丟到正確的mac，理論上不能切網段，，但arp table會記得mac的位置，所以

三層 routing. 給定Denstination，透過route table 決定要用哪一個實體方式傳出，不保證順序正確換安全性。package. 例如 IP
Router 是一台管到第三層機器，裡面有routing table：可以切網段
routing table 為了區別不同網段，決定封包要往哪邊丟

四層  在一個管道中，區分不同關係的訊習且不互相干擾。frame. 封包訊息的確認與重組 例如 TCP 定義傳輸方式。TCP slow stat, session。UDP獨立傳訊 ICMP IGMP
管到第四層的機器 gateway (但不完全正確）

PPPoE point to point over ethernet: 撥接前169.254.x.x，撥接時broadcast router，取得public ip，relay 內部電腦會知道自己的public ip

虛擬網路 router會修改封包，使得封包可以傳送到正確的位置。虛擬網路的電腦不知道自己的public ip，以為自己的虛擬IP就是public ip。 Internet ip
NAT network address translation 
(S)NAT+MAQURADS
DNAT port-founding

5~7 解讀資料。網路通訊協定  雙工：可以同時聽說，半雙工：無法同時聽說


問題：同一個區域裡面的其他使用者如何知道其他使用者使用的port？
nat會自己修正已被佔用的port
nat是功能


AWS VPC
一個網路：定義網段
子資源：
subnet：一定屬於某格az, 不能重複，必須包含在vpc網段中
security group：可跨多個az,管理四層管理權，預設不允許任何通訊，只能設定allow，source可以設定其他sg。綁在ENI上，四層的東西。允許反向，也就是回傳的封包會被允許。
Network ACL：哪一個網段可以access哪一個網段，綁在subnet上，可以allow/deny，source
三層的東西，規則號碼越小越優先，比對到就結束。回傳的封包不會被允許，需要自行設定。

封包傳送 source, denstination -> 接收者回傳 source = destination, destination = source

routing table：一個subnet 可以只一台route table
Destination -> gateway 可以設定 igw, vat/eni, nat gw, peering, vgw(VPN, dx), blackhole
Igw: 一個vpc最多可以綁一個igw，NAT(DMZ)
NAT/ENI: 用機器做nat, potable SNAT - MASK
NAT gw: AWS提供的方案，主動見一台機器當ＮＡＴ，檔掉在重啟，綁定eip，跟某個public subnet。 
EIP: 讓igw可以做nat。AWS從ip pool 中取得。
public Ip：關機後會收回，下次開機取得的可能不同。
peering: 
VGW: router, ipv6, vpn透過網路建立虛擬的tunnel, d(t?)x是透過線路
VPCE: VPCN point，不知到ip的igw
Blackhole: 設一台eni並綁定route table，在砍掉eni

鳥哥nat

保留五個ip.  0 Netwrok, 1 gateway, 2 dns,  3 reserved, 
255 boradcast (AWS not allowed, but reserved)

127.0.0.0/32 load back 1.0
10.24.0.0/24 local eth0
0.0.0.0/0 10.24.0.1 eth0 (default)

每個都比對，符合的prefix越長的優先(only for aws)
Linux 從上往下看，符合的就走


 
VPN connection
igw
vgw
Peering


TCP: frame 保證傳達且有序號 
 3-way handshake, 4-way handshake, 
Syn ->                       fin(代表話說完了)->
  <-syn, ack+1            <- ack(收到fin)
Ack+2 ->                     <-fin(代表話說完了)
<- +2                         ack(收到fin)->
如果沒收到，ack最後一個收到的
Reset ：不說也不聽 => 暴力斷線

Flag: 1byte
Syn,ack是flag


Telent(23)：sudo tty, 透過網路模擬出本機上的介面 => 裸奔(沒有加密的封包在網路上)
FTP 也是裸奔
SSH: 對稱是加密AES, chacha(google), session key不儲存且該session結束後便消失。
SSL: 第二個s事tcp連線， => TLS(微軟加入後)
Key exchange: 無法監聽得知使用的key，且不需要使用public和private key.
RSA:  
Zn = {0,1,2~9} => Zn*為Ｚ的乘法反元素(一個恕誠上自己後mo定理：

DH:a -> A=P^a%M. —————  b->B=P^b%M => 監聽者無法從A,B逆推a,b，雙方可以透過 P^ab做為溝通的鑰匙。因為B^a = A^b = P^ab                    

HTTPS
HTTP裸奔？



192.168.0.1/24 => and 運算 相同同一區
.0  送出
.255 broadcast
.? getway 實務

https://www.phpini.com/linux/chmod-command
http://chi15036-blog.logdown.com/posts/306374-aws-availability-zones-introduced 
http://akuma1.pixnet.net/blog/post/291725322-%EF%BC%88%E4%BA%8C%EF%BC%89ec2%EF%BC%88elastic-compute-cloud%EF%BC%89%EF%BC%8D%EF%BC%8Daws%E7%B6%93%E9%A9%97%E6%95%99%E5%AD%B8
http://allenchien.logdown.com/posts/729685-aws-study-notes-2-create-ec2-host


AWS amazon web service （雲端）
global infrastructure
1 region at least two availability zones (AZ, hdtc) at least one 機房
選region 優先考量法規，次考量費用跟延遲時間 (104選用東京，練習用奧客良）
CDN edge locations

傳統從建構機房開始 IDC?
Iaas(infrastructure as a service) ：供應vm，其餘由使用者建構
Paas(Platform as a service)  供應商已建構好網路平台，使用者部署應用程式即可．
Saas(Software as a service)：軟體租用服務，例如gmail企業版


DNS: 提供domain name和 IP address的轉換。從尾端開始解析 => 樹狀圖
台灣可以在twnic查詢相關資料

ip/16 => ip和16個1做and運算，結果相同的同一段，16稱為netmask
子網路切割速算法
子網段數目 ＝2^(新mask - 原mask)，每一段 Ip 數量 ＝ 2^(32 - 新mask)
Private subset for 內網
10.0.0.0/8 172.16/12 = 172.16.0.0/16 ~ 172.31.0.0/16
192.168/16 = 192.168.0.0/24 ~ 192.168.255.0/24

Router 連接不同段網路，每段網路會有一張網卡，每張網卡都有一個ip（腳），getway: router 的腳
一般來說，getway和自己的電腦同網段．

防火牆：阻擋ip, port等東西，跟防毒沒關係，因為防火牆無法讀取內容
第一代：只能看到第三層，兩邊都要自己設定
第二代：stateful filters，只要設定一邊，另一邊會自己對應
第三代WAF：可以看到第七層，可以追蹤狀態

NAT: network address translation 對外ip

macpass 密碼: 每個網站都產生一組密碼，但使用時只需要輸入同一組密碼。



104awsdev10.signin.aws.amazon.com/console
aws帳號密碼：id，hcqp.su.md.y s.b.shift.n

https://104awsdev27.signin.aws.amazon.com/console

https://home.clifflu.net/cast

Lab 1 建立一台對外連線的電腦
Service -> Networking -> VPC(虛擬機房)
右上角點選區域（不同區域價格不同，延遲的時間也不同），subnet一開始就要決定在哪一個az，之後無法更改，也無法跨az使用。
左邊Your VPCs -> Create VPC -> 輸入name跟IPv4如10.128.0.0/16，（Tenancy選Default代表共用機器，Dedicated代表專用機器，相對費用也很高）。
左邊Subnets -> Create Subnet -> 輸入name，選擇自己的VPC，Availability Zone任選一個，同一個帳號下的同一個zone才會是同一個機房 ，不同帳號下不保證是同一個機房（跨機房費用較高），IPv4輸入子網段如10.128.1.0/24。
左邊Internet gateways：如果VPC要連上Internet，需要開啟這一個功能，任意名稱皆可。選取getway，滑鼠右鍵，選擇attach VPC，選擇自己的VPC。
左邊Route Table：Create Route Table -> 輸入任意名稱，選擇自己的VPC -> 點選自己的subname -> 下方Routes  -> Edit -> 輸入 Destination 0.0.0.0/0(連接外網)，選擇Target igw… -> subnet associations 綁定。
＊ subnet裡面所有資源共享同一張route table。aws裡面會預設local route table，跟一般觀念不同，這張表格會讓子網段可以相連，也會讓所有網段都可以連上網路，因此要新建table。一般預設table不修改，因為子網段沒有綁table時，會自己綁定預設table，如果預設table修改後權限太大，會有安全性問題。
Service -> Compute -> EC2 (建議開新視窗，可以對照資訊）
Create Instance -> 選擇要使用的作業系統 -> 選擇免費的 -> 選擇Network -> add tag -> key =‘Name’，key=‘之前使用的名字’ -> 其他都先用預設值，其中一個步驟要把ip enable (事後可以到elastic ips補，注意要到的ip是變動的，且沒有使用要收費） -> 下載 create key-vale pair(輸入之前使用的名字），一定要下載，以後無法再下載，遺失的話整個系統都不能使用。
使用結束後，可以按滑鼠右鍵，先把機器停掉，避免收費．
使用終端機測試機器可否正常連線使用
切換到儲存key的資料夾
ssh -i kikisu-test.pem(key的檔案名稱) ec2-user@13.58.43.13(ec2上顯示的public ip)

刪除VPC之前，要先把裡面的COMPUT類刪除，因為VPC不會主動刪除，因此在刪除VPC的時候會出錯。

Lab 2 建立一個vpc，一台對外電腦，一台內部電腦，不可上網
建立vpc如Lab1
步驟3. 因為有兩台電腦，要起兩個subnets且Availability Zone兩台要選相同的。ip 10.128.1.0/24 & 10.128.10.0/24
因為內部電腦不可直接上網，要透過NAT getway => subnet 要選 public，因為NAT必須要能夠對外聯繫。NAT Getway建置需要數分鐘，如果Status顯示失敗，就在重新起一個。
步驟4不用做。
步驟5 建兩個 route table，對外電腦跟Lab1一樣，內部電腦在下面routes的target要選擇nat ＝> 透過nat上網。
建立pc
先做security group：public的部分name跟description自訂，選自己的，type為SSH，source = My IP。private的部分，source = customer 輸入 public security 的group id::0 例sg-00114a69
*這個地方如果從vpc做，會找不到myip的選項，另開視窗搜尋myip即可。
建立EC2，Bastion (協助內網機器連接外網的跳板機器，可連外網) create instances: Network = 自己的VPC，subnet = Public，Auto assign Public IP = Enable。記得下載密碼
使用終端機測試是否可以連上該電腦，如Lab1 步驟C，成功後可以輸入sudo yum -y update(更新機器的指令)，測試是否可連上外部網路。exit退出機器，回到自己的電腦。
把建立pubilic ec2時得到的key丟到bastion server，終端機指令scp -i kikisu-test.pem(登入檔案) kikisu-test.pem(要複製檔案來源) ec2-user@13.58.98.254(bastion server public ip):~。
在起一台EC2，create instances: Network = 自己的VPC，subnet = private，Auto assign Public IP = Disable。step 6 assign a security group: selecting security group。記得下載密碼。

 成功後，一樣用終端機測試。


Lab 3 建置兩套Lab2，放在不同機房
因為雲端建置新主機很快，所以只要建置一個Bastion，等出狀況在建構另一台即可，兩台web server在不同機房AZ(available zone)，並透過load balancing分配資源給兩台web server。另外設定auto scaling，讓雲端系統可以根據web server的cpu狀況自動增減web server。
網路段
VPC一台，綁定IGT(internet gateways)
subnet四個：建議ip可以採用11,12,21,22分別代表az和lab
建議兩個AZ各自設一個NAT，以免其中一個AZ掛掉 => 需要兩個NAT。除非透過load balancing控制。
三張rounting table：public共用一張(下面route: destination=0.0.0.0/0, target=igw；subnet associations綁定兩個public)，private走各自的NAT，各一張rounting table，(下面route: destination=0.0.0.0/0, target=對應的nat, subnet associations綁定各自對應的subnet)。
四張防火牆security group：兩張ssh => 一張給bastion
兩台電腦在其中一個AZ，一公一私
EC2 -> load balancers


while true; do curl http://10.128.22.214/stress.php; done
從bastion測試server的壓力

AWS: rounting table 一般跟機器綁一起，但在AWS中跟getway綁一起，又package會先到hyperviser層，因此會先在這層的過濾。


serverless framework



