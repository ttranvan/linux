# 1. **File system types**

## 1.1 **File System là gì**

Mỗi hệ thống sử dụng cấu trúc riêng, có thể dùng phân cấp để có thể xác định được dữ liệu. Cách lưu trữ và tổ chức như vậy gọi là File system

Một File system là thiết bị mà nó đã được định dạng để lưu trữ file và thư mục.

Hệ thống file Linux bao gồm: CD-ROM, partition của hard disk ...

File system thường được tạo trong quá trình cài đặt hệ điều hành. Nhưng cũng có thể thay đổi cấu trúc file system khi thêm thiết bị hay chỉnh sửa những partition đã tồn tại.

<img src=https://s1.gifyu.com/images/image4e5a1ed81600b10a.png>
## 1.2 *Các thành phần của**  **File system**** :**

- Superblock
- Inode
- Storageblock

- Super Block: là một cấu trúc được tạo tại vị trí bắt đầu file system Nó lưu trữ thông tin về file system như: Thông tin về block-size, free block, thời gian mountcuối cùng của file
- Inode (256 byte): Lưu những thông tin về file và thư mục được tạo ra trong File system . Nhưng chúng không lưu tên file và thư mục thực sự. Mỗi file tạo ra sẽ được phân bổ một inode lưu thông tin sau:

- Loại file và quyền hạn truy cập
- Người sở hữu.
- Kích thước và số hard link đến file
- Ngày và thời gian chỉnh sửa lần cuối cùng.
- Vị trí lưu nội dung file trong File system

- Storageblock: Là vùng lưu dữ liệu thực sự của file và thư mục. Nó chia thành những Data Block. Dữ liệu lưu trữ vào đĩa trong các data block. Mỗi block thường chứa 1024 byte. Ngay khi file chỉ có 1 ký tự thì cũng phải cấp phát 1 block để lưu nó. Không có ký tự kết thúc file.

## 1.3 **File system**  **type**

- **Ext – Extended file system** : Phiên bản đầu tiên của Ext
- **Ext2** : Không hỗ trợ journal , performance tăng lên

Hỗ trợ dung lượng file system 2TB

Phù hợp với các loại lưu trữ ngoài như USB.

- **Ext3** : là ext2 kết hợp với journal , nên perfomance chậm

Ưu điểm : bảo toàn về dữ liệu so với ext2 , resize file system ko dowtime

- **Ext4** : ưu điểm : hỗ trợ file size đến 16TB, sử dụng extent thay cho block mapping của ext2,3 nên tăng perfomance với file lớn, giảm khả năng phân mảnh ,không giới hạn subdirectory , journal checksum : tăng tin cậy cho journal , giảm IO wait để tăng perfomance
- **ReiserFS** : Với nhiều tính năng mới mà file hệ thống Ext khó có thể đạt được. Nhưng vẫn không hỗ trợ đầy đủ hệ thống kernel của Linux. Đạt hiệu suất hoạt động rất cao đối với những file nhỏ như file log, phù hợp với database và Email server.
- **BtrFS** : Đang trong giai đoạn phát triển bởi Oracle và có nhiều tính năng giống với ReiserFS. Đại diện cho B-Tree File System, hỗ trợ tính năng pool trên ổ cứng, tạo và lưu trữ snapshot, nén dữ liệu ở mức độ cao, chống phân mảnh dữ liệu nhanh chóng... được thiết kế riêng biệt dành cho các doanh nghiệp có quy mô lớn.

- **Xfs** Khá tương đồng với Ext4, như hạn chế được tình trạng phân mảnh dữ liệu, không cho phép các snapshot tự động kết hợp với nhau, hỗ trợ nhiều file dung lượng lớn, có thể thay đổi kích thước file dữ liệu... nhưng không thể shrink – chia nhỏ Partition XFS. Phù hợp áp dụng Media Server vì khả năng truyền tải file video rất tốt.

Tuy nhiên, nhiều phiên bản distributor yêu cầu Partition /boot riêng biệt, hiệu suất hoạt động với các file dung lượng nhỏ không bằng được khi so với các định dạng file hệ thống khác, do vậy sẽ không thể áp dụng với mô hình database, email và một vài loại server có nhiều file log

**JFS** Điểm mạnh của JFS là tiêu tốn ít tài nguyên hệ thống, đạt hiệu suất hoạt động tốt với nhiều file dung lượng lớn và nhỏ khác nhau. Các Partition JFS có thể thay đổi kích thước được nhưng lại không thể shrink như ReiserFS và XFS, tuy nhiên nó lại có tốc độ kiểm tra ổ đĩa nhanh nhất so với các phiên bản Ext.

- **Swap** Không phải file system,được sử dụng dưới 1 dạng bộ nhớ ảo và không có cấu trúc file system cụ thể. Không thể kết hợp và đọc dữ liệu được, nhưng lại chỉ có thể được dùng bởi kernel để ghi thay đổi vào ổ đĩa. Thông thường, nó chỉ được sử dụng khi hệ thống thiếu hụt bộ nhớ RAM hoặc chuyển trạng thái của máy tính về chế độ Hibernate.

## 1.4 **Journaling**

<img src=https://s1.gifyu.com/images/image6277553cda1b5cec.png>

Journaling giúp tránh hỏng hệ thống file bằng cách ghi metadata bao gồm các thay đổi của hệ thống file vào journal

Sau khoảng thời gian, những thay đổi này được ghi vào hệ thống file, nếu trường hợp hệ thống tắt đột ngột , OS sẽ kiểm tra journal trong file system khi khởi động và tiếp tục thực hiện

**Journal** mode :

- Journal : toàn bộ dữ liệu và metadata thay đổi sẽ được ghi vào journal trước khi được commit vào file system , làm cho peformance giảm xuống do phải đọc và ghi đến 2 lần
- Ordered : Chỉ ghi các metadata vào journal , dữ liệu sẽ được ghi trực tiếp vào file system. Sau khi ghi dữ liệu xong thì phần metadata tương ứng mới dc ghi vào file system

## 1.5 **Một số lệnh kiểm tra File system type trên hệ thống**

> df -ahT

Lệnh df báo cáo việc sử dụng không gian đĩa của hệ thống ;

Bao gồm toàn bộ các hệ thống tệp giả, trùng lặp, không thể truy cập được : **-a**

Hiển thị size lũy thừa 1024 : **-h**

Loại file system : **-T**


> fsck -N

Lệnh fsck kiểm tra hệ thống tệp Linux và cố gắng sửa chữa trong trường hợp có sự cố

-N không thực thi kiểm tra và sửa chữa , chỉ hiển thị những gì sẽ được thực hiện


> lsblk -f

Liệt kê tên thiết bị, hard disk và Partition

-f hiển thị loại file system


- /etc/fstab là file cấu hình mount file system , loại file system, và các tùy chọn khác.

> cat /etc/fstab

# 2. **Partitioning a disk**

## 2.1 **Giới thiệu về Partition**

Partition là những phân vùng nhỏ được chia ra từ 1 Physical disk . Một hard disk có thể có 1 hoặc nhiều partition. Partition là cách phân chia và quản lý một hard disk đơn giản và hiệu quả (VD 1 partition quan trọng để chứa dữ liệu của OS và 1 partition để chứa dữ liệu người dùng

Dữ liệu trên 1 partition A sẽ được phân tách với dữ liệu trên partition B, mọi thao tác trên partition này sẽ không ảnh hưởng đến partition kia.

Hiện có 3 loại partition chính là: primary, extended và logical :

**Primary partition** : đây là những Partition có thể được dùng để boot hệ điều hành

**Extended partition** : là vùng dữ liệu còn lại khi ta đã phân chia ra các primary partition, extended partition chứa các logical partition trong đó. Mỗi một ổ đĩa chỉ có thể chứa 1 extended edition.

**Logical partition** : các Partition nhỏ nằm trong extended partition, thường dùng để chứa dữ liệu.

## 2.2 **MBR và GPT**

Thông tin về các partition của ổ cứng sẽ được lưu trữ trên MBR (Master Boot Record) hoặc GPT (GUID Partition Table) tùy loại ổ cứng hỗ trợ. Đây là 2 chuẩn cấu hình và quản lý các partition trên ổ cứng. Thông tin được lưu trữ trên đây gồm vị trí và dung lượng của các partition.

- **MBR** là chuẩn phân chia ổ đĩa truyền thống, một ổ đĩa sẽ được chia thành các vùng nhỏ (sector) với dung lượng bằng nhau là 512 bytes. Trên Linux, một ổ đĩa cứng được chia thành nhiều partition với số hiệu như sau: /dev/hda1, /dev/hda2, /dev/sda1, /dev/sda2, /dev/sdb1,… Ta có thể dùng lệnh fdisk hoặc parted để hiển thị thông tin về ổ đĩa dùng MBR trên Linux.

> fdisk -l /dev/sda

<img src=https://s1.gifyu.com/images/image15303b97b712799b.png>
Đối với các ổ cứng kiểu cũ chỉ hỗ trợ MBR thì ta chỉ được phép có tối đa 4 primary partition trên 1 ổ cứng, extended partion cũng được coi là 1 primary partition. MBR sử dụng 32 bit để lưu trữ địa chỉ khối và đối với các đĩa cứng có các sectors 512 byte, MBR xử lý tối đa 2TB (2^32 × 512 byte).

**Boot Record.**

- **GPT** là chuẩn mới hơn, hỗ trợ đến 128 Partition trên 1 ổ đĩa vật lý. Thông tin về các partition sẽ được ghi thành nhiều bản rải rác khắp ổ vật lý. GPT hỗ trợ cơ chế kiểm tra và chỉnh sửa dữ liệu dựa trên CRC (cyclic redundancy check). Nhờ 2 cơ chế này, chuẩn GPT làm giảm tỷ lệ mất mát dữ liệu. GPT sử dụng 64 bit cho địa chỉ khối và cho các đĩa cứng có các sectors 512 byte, kích thước tối đa là 9,4 ZB.

<img src=https://s1.gifyu.com/images/imagef9de4cf53ba435a9.png>

1. **Một số công cụ để Partition ổ đĩa**

- **Fdisk**

**fdisk**  là tiện ích quản lý Partition trên Linux Có thể xem, tạo, thay đổi kích thước, xóa, thay đổi, sao chép và di chuyển các Partition

**fdisk** cho phép tạo tối đa 4 Primary Partition được Linux cho phép với mỗi Partition yêu cầu kích thước tối thiểu 40MB.

**Sử dụng Fdisk :**

- **Xem tất cả các**  **partition hiện có**

Lệnh liệt kê danh sách Partition trên hệ thống đĩa của bạn và các Partition được sắp xếp theo tên /dev của thiết bị như /dev/sda, /dev/sdb, ... Sử dụng lệnh sau để xem tất cả các Partition hiện có trong hệ thống.

> fdisk -l

- **Xem một Partition trên một ổ đĩa cụ thể**

Xem tất cả các Partition trên một đĩa cụ thể, sử dụng lệnh sau để xem tất cả các Partition đĩa trên /dev/sda.

> fdisk -l /dev/sda

- **Xem tất cả các lệnh**  **f**** disk**

Sử dụng lệnh sau để xem tất cả các lệnh fdisk có sẵn

> fdisk /dev/sdb

Nhập m để xem danh sách tất cả các lệnh có sẵn của fdisk có thể sử dụng trên /dev/sdb :

<img src=https://s1.gifyu.com/images/image1c862d6acebf2f7e.png>

- **Tạo Partition mới**

Để tạo Partition mới thực hiện các bước sau :

-- **B1:** Nhập lệnh sau để bắt đầu tạo Partition trên /dev/sdb

> fdisk /dev/sdb


-- **B2** : Nhập **n** để tạo Partition mới sẽ nhắc chỉ định cho một Partition chính hoặc Partition mở rộng. Nhập **p** cho Partition chính hoặc **e** cho Partition mở rộng.


**B3** :Sau đó, bạn sẽ được nhắc nhập số của Partition sẽ được tạo. Bạn có thể nhấn Enter để chấp nhận mặc định.


**B4** :Sau đó, bạn sẽ được nhắc nhập kích thước của Partition sẽ được tạo ví dụ tạo đĩa với size 10 GB. Có thể nhấn Enter để sử dụng tất cả không gian có sẵn.


**B5** : Chạy lệnh  **w**  để lưu các thay đổi


**B6**: Kiểm tra Partition mới đã được tạo chưa


**B7** : Có hai Partition mới được tạo. Sau khi tạo Partition phải thông báo cho OS để cập nhật bảng Partition. Thực hiện lệnh sau đây:

> partprobe /dev/sdb

B8:Định dạng Partition của để sử dụng. Lệnh sau để định dạng Partition sdb1 với **xfs**.

> mkfs.xfs /dev/sdb1

**B9** :Sau khi định dạng thành công Partition mới các Partition đã có thể lưu trữ dữ liệu. Chúng ta phải gắn Partition vào thư mục. 

**B10** : mount /dev/sdb1 vào /data/ bằng lệnh sau :

> mount /dev/sdb1 /data

Kiểm tra sử dụng partition với lệnh sau :

> df -ahT

- **Xóa một Partition**

**B1:** Ngắt kết nối Partition bằng cách xóa mục của Partition đó trong /etc/fstab

**B2:** Ngắt kết nối thư mục /data bằng lệnh

> ummout /data

B3: Xem các Partition hiện có và chạy lệnh sau :


B4 :Nhập d để xóa một Partition. Nhập số của Partition, trường hợp này có 1 Partition /dev/sdb1 nên sẽ mặc định xóa Partition 1 . Lưu các thay đổi bằng cách nhập w.



- **Parted**

**Parted**** :**Hỗtrợ nhiều định dạng Partition table, bao gồm MS-DOS và GPT. Nó cho phép tạo, xóa, thay đổi kích thước, thu nhỏ, di chuyển và sao chép Partition, sắp xếp lại việc sử dụng ổ đĩa và sao chép dữ liệu vào các ổ đĩa mới

**parted** cho phép Partition khi kích thước đĩa lớn hơn 2TB nhưng fdisk không cho phép.

**Sử dụng parted :**

- Xem toàn bộ các Partition hiện có

![](RackMultipart20220717-1-s1ordv_html_d9bffb4eb878c40c.png)

- Cách tạo Partition :

B1: Chọn ổ đĩa muốn tạo Partition , tại đây sử dụng /dev/sdb

Gõ lệnh help để hiển thị danh sách các lệnh

![](RackMultipart20220717-1-s1ordv_html_78a9710e11fe86bb.png)

**B2** : Đặt đơn vị tính bằng GB

![](RackMultipart20220717-1-s1ordv_html_1200b718e29882e2.png)

**B3** : Bắt đầu tạo Partition mới với **mkpart**. Nhập primary cho Partition chính hoặc extended cho Partition mở rộng. Sau đó ta nhập size của Partition :

![](RackMultipart20220717-1-s1ordv_html_e4b0766b49d81429.png)

**B4** : Kiểm tra Patition vừa được tạo:

![](RackMultipart20220717-1-s1ordv_html_278c504054137d9a.png)

- Có thể tạo partition với command

parted [Disk Name] [mkpart] [Partition Type] [Filesystem Type] [Partition Start Size] [Partition End Size]

Tạo Partition thứ 2 :

![](RackMultipart20220717-1-s1ordv_html_f00d0940d79195b8.png)

B5: Định dạng Partition mới thành ext4

![](RackMultipart20220717-1-s1ordv_html_9c0b9349bb019ce0.png)

Kết quả :

![](RackMultipart20220717-1-s1ordv_html_84e371b4c6e80142.png)

- **Tạo Partition mới với toàn bộ không gian còn lại**

![](RackMultipart20220717-1-s1ordv_html_4501aefa33bd6e98.png)

- **Xóa Partition**

Sử dụng **rm** để xóa một partition ; nhập **rm** và number của partition

![](RackMultipart20220717-1-s1ordv_html_917f5f726e45a4d6.png)

- Resize Partition

Kiểm tra không gian còn trống và sử dụng của phân vùng trên ổ đĩa

![](RackMultipart20220717-1-s1ordv_html_ea9d0afbd89b5f03.png)

- Tăng dung lượng partition /dev/sdb1 thêm 2GB ( Trước khi tăng phải umount partition)

![](RackMultipart20220717-1-s1ordv_html_edac46aa81aeb487.png)

- Resizepart chỉ tăng ở Partition label , sử dụng e2fsck với file system (ext2, ext3, ext4) hoặc xfs\_growfs đối với file system (xfs) để resize :

![](RackMultipart20220717-1-s1ordv_html_aadb1fd1d4bf17f5.png)

Kết quả : Phân vùng đã tăng thêm 2GB

![](RackMultipart20220717-1-s1ordv_html_d73df841b926b6c0.png)

1. **Các option cần lưu ý khi phân vùng ổ đĩa**

- Backup data trước khi bắt đầu, bất kỳ thao tác không chính xác có thể loss data
- Không thay đổi phân vùng đang sử dụng
- Fdisk không thực hiện các thay đổi cho đến khi được yêu cầu.
- fdisk không thể sử dụng đối với Partition table (GPT) và nó không hoạt động Partition lớn hơn 2TB.
- MBR chỉ hỗ trợ 4 primary partition , muốn tạo nhiều hơn 4 partition thì phải sử dụng 1 hoặc hơn primary partition để tạo extended partition và tạo logical patition bên trong

1. **Creating file**** systems**

- Sau khi phân vùng ổ đĩa, tiến hành khởi tạo file system cho Partition :

Công cụ để tạo file system : mkfs

- Cấu trúc tạo file system

mkfs [options] device

Sử dụng option –t để chỉ định loại file system , nếu để trống thì mặc định loại file system là ext2

- Để kiểm tra các file system type mà OS hỗ trợ :

![](RackMultipart20220717-1-s1ordv_html_fc4ee19e1ec3b9c8.png)

- Tạo file system với mkfs

![](RackMultipart20220717-1-s1ordv_html_7cafb51b52dd82f1.png)

- Có thể tạo file system với lệnh sau :

![](RackMultipart20220717-1-s1ordv_html_6749aa2ab683ec87.png)

- Trường hợp thay đổi file system type từ ext sang loại khác cần them option –f

![](RackMultipart20220717-1-s1ordv_html_4c8278e232abb8a6.png)

- Tạo swap space

![](RackMultipart20220717-1-s1ordv_html_54fa56c0761386b0.png)

1. **Displaying disk usage**

- **df :** Hiển thị dung lượng ổ đĩa được sử dụng và khả dụng trên hệ thống tệp Linux.

- Cú pháp cơ bản :

**df [options]**

- Df –H : Hiển thị size theo lũy thừa 1024 : dễ dàng hiểu

s ![](RackMultipart20220717-1-s1ordv_html_ebf6a2784fe3eb74.png)

- Có thể xem chỉ một device hoặc 1 thư mục hoặc partition

![](RackMultipart20220717-1-s1ordv_html_b450ef5a6ed0af1.png)

- Kiểm tra dung lượng inode

![](RackMultipart20220717-1-s1ordv_html_3186defcf41d9728.png)

- **du :** Hiển thị dung lượng ổ đĩa được sử dụng bởi thư mục và file

- Cú pháp :

du
 du [options] [directories and/or files]

- Hiển thị dung lượng sử dụng bởi /home , option –h để hiện theo size

![](RackMultipart20220717-1-s1ordv_html_3ddc851be10e29e1.png)

- Sử dụng option **–a** để hiện toàn bộ file , không bao gồm thư mục

![](RackMultipart20220717-1-s1ordv_html_80edcc47b1bced66.png)

- Sử dụng option **–c** để hiện tổng dung lượng của nhiều thư mục; **-s** chỉ hiển thị tổng số đường dẫn
 ![](RackMultipart20220717-1-s1ordv_html_9081f7e50b76f978.png)

- **Btrfs** đối với btrfs file system có thể sử dụng btrfs fi df để xem thông tin sử dụng dung lượng của các mount point

![](RackMultipart20220717-1-s1ordv_html_96ccb6e0e485f41c.png)

1. **Mounting and unmounting file systems**

Tất cả các partition đều được attach vào hệ thống thông qua một mount point , nó sẽ chỉ ra vị trí của file system đó để có thể truy cập và sử dụng

- Để list toàn bộ các file system được attach vào hệ thống :

![](RackMultipart20220717-1-s1ordv_html_9a1819cc5ae5de58.png)

- Để hiển thị mỗi file system type thì them option **–t**

![](RackMultipart20220717-1-s1ordv_html_dde83a154455f3ec.png)

- Để mount file system tới mount point sử dụng syntax :

![](RackMultipart20220717-1-s1ordv_html_97d7a6bd6c3a7314.png)

- **File /etc/fstab** bao gồm các thông tin tự động mount partition.
- Các Partition được mount là tạm thời, các thư mục attach sẽ bị mất khi reboot OS ..Để mount vĩnh viễn thì phải sửa tệp **/etc/fstab**

- Lấy UUID của Partition /dev/sdb1

![](RackMultipart20220717-1-s1ordv_html_3538ebdf0e16688e.png)

- Sửa trong file /etc/fstab, tệp tin fstab có định dạng sau :

![](RackMultipart20220717-1-s1ordv_html_c57b9a30d904a36e.png)

**Device :** Cho biết loại thiết bị (Partition, CD/DVD, USB, ISO image…). Đồng thời cũng cho biết đường dẫn tới file đại diện cho thiết bị (device file)

**Mount Point :** Đường dẫn của mount point, là một thư mục trống được tạo sẵn trong cây thư mục

**File System Type :** Là kiểu filesystem của thiết bị

**Options:** Là các tùy chọn khi mount, nhiều tùy chọn ngăn cách nhau bởi dấu phẩy:

- **auto** : tự động mount thiết bị khi máy tính khởi động
- **noauto** : không tự động mount, nếu muốn sử dụng thiết bị thì sau khi khởi động vào hệ thống bạn cần chạy lệnh mount.
- **user** : cho phép người dùng thông thường được quyền mount.
- **nouser** : chỉ có người dùng root mới có quyền mount.
- **exec** : cho phép chạy các file nhị phân (binary) trên thiết bị.
- **noexec** : không cho phép chạy các file binary trên thiết bị.
- **ro (read-only**): chỉ cho phép quyền đọc trên thiết bị.
- **rw (read-write):** cho phép quyền đọc/ghi trên thiết bị.
- **sync** : thao tác nhập xuất (I/O) trên filesystem được đồng bộ hóa.
- **async** : thao tác nhập xuất (I/O) trên filesystem diễn ra không đồng bộ.
- **defaults** : tương đương với tập các tùy chọn rw, suid, dev, exec, auto, nouser, async

**Dump :** là tùy chọn cho chương trình dump, công cụ sao lưu filesystem. Điền 0: bỏ qua việc sao lưu, 1: thực hiện sao lưu.

**Pass :** là tùy chọn cho chương trình fsck, công cụ dò lỗi trên filesystem. Điền 0: bỏ qua việc kiểm tra, 1: thực hiện kiểm tra

- Điền các thông tin vào file fstab

![](RackMultipart20220717-1-s1ordv_html_3295fbd735853f85.png)

- Lưu lại file và chạy lệnh

![](RackMultipart20220717-1-s1ordv_html_bc7db82068f8213a.png)

![](RackMultipart20220717-1-s1ordv_html_64000860e4245e1c.png)

- Để detach file system đã mount sử dụng umount với syntax:

umount DIRECTORY

umount DEVICE\_NAME

![](RackMultipart20220717-1-s1ordv_html_52f2609a825ec494.png)
