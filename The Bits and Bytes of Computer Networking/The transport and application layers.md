# The transport layer
## The transport layer
Chức năng chính của tầng vận chuyển
-  Đảm bảo truyền dữ liệu đáng tin cậy qua mạng.
-  Thực hiện đa kênh (multiplexing) và giải đa kênh (demultiplexing) để quản lý lưu lượng mạng.

Đa kênh và giải đa kênh
-  Đa kênh: cho phép một nút mạng gửi dữ liệu đến nhiều dịch vụ nhận khác nhau.
-  Giải đa kênh: phân phối dữ liệu đến đúng dịch vụ nhận trên nút đích

Vai trò của cổng (ports) và địa chỉ socket
-  Cổng là số 16 bit dùng để định hướng lưu lượng đến dịch vụ cụ thể trên máy tính.
-  Địa chỉ socket là sự kết hợp giữa địa chỉ IP và cổng.
> Ví dụ: 10.1.1.100:80 cho dịch vụ HTTP.

> Ví dụ về ứng dụng đa kênh
> -  Một máy chủ có thể chạy nhiều dịch vụ như web (cổng 80), FTP (cổng 21), mail, in ấn, chia sẻ file.
> -  Điều này giúp một máy chủ phục vụ nhiều ứng dụng cùng lúc nhờ vào đa kênh và giải đa kênh.

## Dissection of a TCP Segment
Cấu trúc của đoạn TCP
-  Đoạn TCP bao gồm phần header TCP và phần dữ liệu (data section).
-  Header TCP chứa nhiều trường thông tin quan trọng để quản lý kết nối và truyền dữ liệu.

Các trường chính trong header TCP
-  Source port và destination port: xác định cổng nguồn và đích để phân biệt các kết nối và dịch vụ.
-  Sequence number và acknowledgment number: theo dõi vị trí và xác nhận các đoạn dữ liệu trong chuỗi truyền.
-  Data offset: cho biết độ dài của header TCP để xác định vị trí bắt đầu dữ liệu.
-  Control flags: 6 bit dùng để điều khiển các trạng thái và hành vi của kết nối TCP.

Các trường hỗ trợ và kiểm tra
-  TCP window: xác định phạm vi các số thứ tự có thể gửi trước khi cần xác nhận.
-  Checksum: kiểm tra lỗi dữ liệu trọng toàn bộ đoạn TCP.
-  Urgent pointer và options: các trường ít được sử dụng, phục vụ cho các tính năng đặc biệt hoặc điều khiển nâng cao.
-  Padding: thêm các bit 0 để đảm bảo dữ liệu bắt đầu đúng vị trí mong muốn.

Các cờ điều khiển TCP
- Có 6 cờ chính trong header TCO: URG (urgent), ACK (acknowledged), PSH (push), RST (reset), SYN (synchronize), FIN (finish).
- Mỗi cờ có chức năng riêng

> Ví dụ: SYN dùng để bắt đầu kết nối. FIN để kết thúc kết nối.

Quá trình thiết lập kết nối TCO (Three-way-handshake)
-  Máy A gửi segment với cờ SYN để bắt đầu kết nối và gửi số thứ tự.
-  Máy B phẩn hồi với segment có cờ SYN và ACK để đồng ý và xác nhận số thứ tự.
-  Máy A gửi segment bới cờ ACK để xác nhận lại, kết nối được thiết lập.

Quá trình đóng kết nối TCP (Four-way-handshake)
-  Máy muốn đóng kết nối gửi segment với cờ FIN.
-  Máy kia nhận phản hồi với cờ ACK để xác nhận.
-  Máy kia cũng gửi FIN khi sắn sàng đóng.
-  Máy đầu tiên phản hồi với ACK, kết nối đóng hoàn toàn.

## TCP and UDP Packets
How the packet is sending data between the client and the server?

Client: SYN packet sender
The client sends a SYN packet to the server via TCP. It tells the server where it wants to connect, and provides the initial sequence number.

SYN packet
The packet is carrying the header and data for the server. The header has information about the origin and the destination IP addresses for the packet. The data is the payload of the packet.

Server
SYN packet receiver.

Client
SYN/ACK packet receiver

SYN/ACK packet
The SYN packet is acknowledged with the server;s Initial Sequence Number.

Server: SYN/ACK packet sender
The server sends a SYN/ACK packet telling the client it got the request.

Client
CLient responds with ACK. finishing the connection.

Agree
Both sides agree to the packet's delivery.

Server
This completes the TCP three-way handshake. All systems are ready for the transfer.

Client: UDP packet sender
The client sends a packet to the server via UDP, UDP does not include SYN-ACK or handshake.

UDP packet
The packet is carrying the header and data for the server. The header has information about the origin and destination IP addresses for the packet. The data is the payload of the packet.

UDP packet receiver
Packet arrives at the server. There is no handshake.

## TCP socket states
Khái niệm về socket TCP
- Socket là điểm kết nối thực tế trong một kết nối TCP tiềm năng, được khởi tạo bởi chương trình.
- Port là khái niệm ảo, có thể gửi đến bất kỳ port nào, nhưng chỉ nhận phản hồi nếu có socket mở trên port đó.

Các trạng thái phổ biển của socket TCO
- Listen: Socket sẵn sáng và đang chờ kết nối đến, thường ở phái server.
- SYM_SENT: Yêu cầu đồng bộ đã được gửi nhưng kết nối chưa thiết lập thường ở phía client.
- SYN_RECEIVED: Socket ở trạng thái listen nhận yêu cầu động bộ và gửi SYN/ACK, chờ ACK cuối cùng từ client, thường ở phía server.

Trạng thái kết nối và đóng kết nối
- Established: Kết nối TCP đã thiết lập và hai bên có thể gửi dữ liệu cho nhau, xuất hiện ở cả client và server.
- FIN_WAIT: đã gửi tín hiệu kết thúc (FIN) nhưng chưa nhận được xác nhận (ACK) từ phía đối diện.
- CLOSE_WAIT: Kết nối đã đóng ở tầng TCP nhưng ứng dụng chưa giải phóng socket.
- Closed: Kết nối đã hoàn toàn kết thúc, không thể giao tiếp thêm.

Lưu ý về sự khác biệt hệ điều hành
- Tên và trạng thái socket có thể khác nhau tùy hệ điều hành vì không thuộc phạm vi định nghĩa chuẩn của TCP.
- Khi xử lý sự cố, cần kiểm tra định nghĩa trạng thái socket cụ thể của hệ thống đang làm việc.

## Connection-oriented and Connectionless Protocols
Giao thức kết nối hướng (TCP)
- TCP thiết lập kết nối và đảm bảo mọi dữ liệu được truyền đi đều được xác nhận (acknowledged).
- TCP sử dụng số thứ tự (sequence numbers) để sắp xếp lại dữ liệu nếu các đoạn dữ liệu đến không theo thứ tự.

Tính năng và vai trò của TCP
- TCP chịu trách nhiệm phát hiện lỗi và quyết định gửi lại dữ liệu nếu cần, trong khi các tầng thấp hơn như IP và Ethernet chỉ loại bỏ dữ liệu lỗi mà không gửi lại.
- Việc thiết lập, duy trì và kết thúc kết nối tạo ra overhead lớn nhưng đảm bảo dữ liệu đến đích một cách chính xác.

Giao thức không kết nối (UDP)
- UDP không thiết lập kết nối và không có cơ chế xác nhận dữ liệu đã nhận.
- UDP phù hợp với các ứng dụng không yêu cầu độ tin cậy cao như truyền video trực tiếp, nơi mất một vài gói dữ liệu không ảnh hưởng lớn đến trải nghiệm người dùng.

## System Ports versus Ephemeral Ports
Phân loại cổng mạng
- Cổng là số 16-bit dùng để định danh dịch vụ mạng trên máy tính.
- Có 3 loại cổng do IANA phân chia: Cổng hệ thống (1-1023), Cổng người dùng (1024-49151), và Cổng tạm thời (49152-65535).

Cách TCP sử dụng cổng và socket
-  TCP dùng cổng để thiết lập kết nối mạng và truyền dữ liệu giữa dịch vụ (server) và khách hàng (client).
-  Socket là cổng đang được dịch vụ kích hoạt để lắng nghe yêu cầu dữ liệu.

Bảo mật và tính toàn vẹn dữ liệu
-  TCP đảm bảo dữ liệu được truyền chính xác qua xác nhận và kiểm tra checksum.
-  Cổng có thể là điểm xâm nhập của phần mềm độc hại, nên cần dùng tường lửa và chỉ mở cổng khi cần thiết để bảo vệ mạng.

## Firewalls
Khái niệm về tường lửa
-  Tường lửa là thiết bị hoặc chương trình dùng để chặn các lưu lượng mạng dựa trên các tiêu chí nhất định nhằm bảo vệ mạng.
-  Tường lửa có thể hoạt động ở nhiều tầng khác nhau của mô hình mạng, phổ biến nhất là tầng vận chuyển (transportation layer).

Chức năng và cách hoạt động của tường lửa
-  Tường lửa ở tầng vận chuyển thường được cấu hình để cho phép hoặc chặn lưu lượng đến các cổng (ports) cụ thể.
-  Ví dụ, một doanh nghiệp nhỏ có thể dùng tường lửa để cho phép truy cập cổng 80 (web server) từ bên ngoài nhưng chặn các cổng khác để bảo vệ tài liệu nội bộ.

Hình thức triển khai tường lửa
-  Tường lửa có thể là thiết bị mạng độc lập hoặc là phần mềm chạy trên các thiết bị khác như router hoặc máy chủ.
-  Hầu hết các hệ điều hành hiện đại đều tích hợp chức năng tường lửa để kiểm soát lưu lượng mạng ngay trên từng máy chủ hoặc máy tính cá nhân.

# The application layer
## The application layer
Lớp ứng dụng trong mô hình mạng
-  Lớp ứng dụng là nơi các ứng dụng thực sự giao tiếp và trao đổi dữ liệu với nhau.
-  Dữ liệu ứng dụng được đóng gói trong phần payload của các phân đoạn TCP.

Giao thức và tính đa dạng
-  Có rất nhiều giao thức khác nhau ở lớp ứng dụng, không giống như các lớp thấp hơn có ít giao thức phổ biến hơn.
-  Các giao thức này được chuẩn hóa để đảm bảo các ứng dụng khác nhau có thể tương tác với nhau.

> Ví dụ về giao thức HTTP
> -  Trình duyệt web (client) và máy chủ web (server) phải sử dụng cùng một giao thức HTTP để giao tiếp.
> -  Các trình duyệt và máy chủ khác nhau (Chrome, Safari, Microsoft IIS, Apache, nginx) đều tuân theo chuẩn HTTP để đảm bảo tương thích.

## The Application Layer and the OSI Model
Mô hình mạng năm lớp và mô hình OSI
-  Mô hình năm lớp thường được sử dụng trong thực tế để hiểu các khía cạnh cơ bản của mạng.
-  Mô hình OSI có bảy lớp, được định nghĩa chặt chẽ và thường dùng trong học thuật và các chứng chỉ mạng.

Các lớp bổ sung trong mô hình OSI
-  Lớp phiên (Session Layer) chịu trách nhiệm quản lý giao tiếp giữa các ứng dụng và lớp vận chuyển.
-  Lớp trình bày (Presentation Layer) đảm bảo dữ liệu ứng dụng được hiểu đúng, xử lý các chức năng như mã hóa và nén dữ liệu.

Sự khác biệt và ứng dụng
-  Mô hình năm lớp gộp các chức năng của lớp phiên và lớp trình bày vào lớp ứng dụng để đơn giản hóa.
-  Hiểu mô hình OSI giúp nắm vững kiến thức mạng cơ bản và chuẩn bị cho các chứng chỉ mạng chuyên sâu.

## All the Layers Working in Unison
Mô hình mạng và địa chỉ IP
-  Ba mạng được giả định: Mạng A (10.1.1.0/24), Mạng B (192.168.1.0/24), Mạng C (172.16.1.0/24).
-  Hai router kết nối các mạng: Router A giữa Mạng A và B, Router B giữa Mạng B và C.

Quá trình truyền dữ liệu từ máy khách đến máy chủ
-  Máy tính 1 (client) trên Mạng A gửi yêu cầu tới máy tính 2 (server) trên Mạng C qua địa chỉ IP 172.16.1.100.
-  Máy tính 1 xác định địa chỉ MAC của gateway (Router A) qua ARP để gửi gói tin ra ngoài mạng cục bộ.
-  Gói tin TCP được tạo với các trường cần thiết (cổng nguồn, cổng đích, số thứ tự, cờ SYN) và được đóng gói qua các lớp IP và Ethernet.

Chuyển tiếp và xử lý gói tin qua các thiết bị mạng
-  Router A nhận gói, kiểm tra checksum, giảm TTL, tìm đường đi tiếp theo và gửi gói tới Router B.
-  Router B thực hiện tương tự, gửi gói tới máy tính 2 trên Mạng C.
-  Máy tính 2 nhận gói, kiểm tra các lớp, xác nhận cổng đích (80) và chuẩn bị phản hồi TCP.

Tổng quan
-  Quá trình này minh họa sự phối hợp giữa các lớp mạng (Ethernet, IP, TCP) và các thiết bị (máy tính, router, switch) để truyền dữ liệu.
-  Việc thiết lập kết nối TCP (ba bước bắt tay) được thực hiện qua nhiều lần truyền gói tin tương tự.

