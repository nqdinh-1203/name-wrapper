# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Introduction](#introduction)
  - [The ENS Name Wrapper](#the-ens-name-wrapper)
    - [Registry Owners with the Name Wrapper](#registry-owners-with-the-name-wrapper)
    - [States of an ENS Name](#states-of-an-ens-name)
  - [Example - Step By Step](#example---step-by-step)
    - [Wrapped Subnames](#wrapped-subnames)
    - [Burning Parent-Controlled Fuses](#burning-parent-controlled-fuses)
    - [Burning the “Parent Cannot Control” Fuse](#burning-the-parent-cannot-control-fuse)
    - [Emancipated Subnames](#emancipated-subnames)
    - [Burning the “Cannot Unwrap” Fuse](#burning-the-cannot-unwrap-fuse)
    - [Locked Subnames](#locked-subnames)
    - [Burning Owner-Controlled Fuses](#burning-owner-controlled-fuses)
    - [Burning the “Cannot Burn Fuses” Fuse](#burning-the-cannot-burn-fuses-fuse)
    - [All Fuses Frozen](#all-fuses-frozen)
  - [Example - Parent Locks Subname All-In-One](#example---parent-locks-subname-all-in-one)
  - [Example - Multiple Subnames in Various States](#example---multiple-subnames-in-various-states)
  - [Name Wrapper Fuses](#name-wrapper-fuses)
  - [Name Wrapper Expiry](#name-wrapper-expiry)
  - [Approved Operators](#approved-operators)

## Introduction
Trước khi chúng tôi đi sâu vào **Name Wrapper**, hãy tóm tắt ngắn gọn một số khái niệm ENS cơ bản sẽ cung cấp một số context hữu ích.

**Registry** là contract cốt lõi ở trung tâm của giải pháp ENS. Tất cả các tra cứu ENS bắt đầu bằng cách truy vấn registry. Registry không chỉ dành cho các tên second-level cua .eth (như `name.eth`) mà còn cho tất cả các tên ENS (các tên phụ như `sub.name.eth` và cả các tên DNS như `domain.xyz`). Chủ sở hữu tên trong registry còn được gọi là Controller hoặc Manager của tên.

**.eth Registrar** dành riêng cho đăng ký tên second-level .eth. Đây thực chất là một registrar cho tên miền phụ cho TLD (top-level domain) .eth. Khi bạn đăng ký một tên .eth, registrar sẽ cấp cho bạn một ERC-721 NFT.

Chủ sở hữu của NFT đó còn được gọi là Registrant hoặc Owner của tên đó. Nó cũng có thể được sử dụng để đòi lại quyền sở hữu đối với tên trong core registry (nói cách khác, Registrant có thể ghi đè lên Controller).

![Alt text](image.png)

Có thể thấy, không có Name Wrapper, hiện tại chỉ có các tên sencond-level .eth là NFT. Theo mặc định, tên phụ và miền DNS không có NFT được liên kết với chúng, trừ khi hợp đồng tùy chỉnh được tạo cho mục đích đó.

![Alt text](image-1.png)

## The ENS Name Wrapper
![Alt text](image-2.png)

Name Wrapper bao bọc một tên ENS thành một ERC-1155 NFT mới. Bất kỳ tên ENS nào cũng có thể được bao bọc, cho dù đó là tên .eth hay tên miền DNS hay thậm chí là bất kỳ tên phụ nào!

Bạn có thể lấy tên `name.eth`…

![Alt text](image-3.png)

Và biến nó thành ERC-1155 NFT được bọc!

![Alt text](image-4.png)

Hãy lấy subname `sub1.name.eth` của chúng ta từ phía trên và bọc nó!

![Alt text](image-5.png)

Bây giờ tên phụ cũng là một ERC-1155 NFT được bao bọc!

Những dòng bạn nhìn thấy đại diện cho cái gọi là "permission fuses" cho tên ENS được bao bọc của bạn, mà tôi sẽ giải thích một chút.

![Alt text](image-6.png)

### Registry Owners with the Name Wrapper
Khi bạn bọc một tên, bạn đang đặt Owner thành Name Wrapper contract. Đổi lại, Name Wrapper phát hành một ERC-1155 NFT và chuyển nó cho bạn.

Đối với tên sencond-level .eth, bạn chuyển Registrant / NFT (trong công ty đăng ký tên miền .eth) sang Name Wrapper contract khi bạn bao bọc. Name Wrapper cũng sẽ tự động lấy lại Owner trong core Registry.

Trước đây, không có NFT nào cả (ít nhất là đối với tên con và tên DNS):

![Alt text](image-7.png)\
*Registry Controller of an unwrapped subname*

Sau khi bạn đặt tên, về mặt kỹ thuật, Owner trong Registry hiện là Name Wrapper contract. Điều đó có nghĩa là bạn không thể tương tác trực tiếp với ENS Registry cho tên đó vì bạn không phải là Owner.

Tuy nhiên, Name Wrapper contrat cung cấp tất cả các method cần thiết để bạn vẫn có thể tương tác với Registry. Bạn có thể chuyển quyền sở hữu, set reolver/ttl và thậm chí tạo tên phụ giống như bình thường. Trên hết, bất kỳ tên phụ nào bạn tạo cũng sẽ được tự động bọc theo mặc định!

![Alt text](image-8.png)\
*Registry Controller of a wrapped subname is now the Name Wrapper contract*

### States of an ENS Name
Ngoài tất cả các chức năng mà tên có được khi trở thành một ERC-1155 NFT chính thức, Name Wrapper cũng cung cấp một số tính năng dành riêng cho ENS.

Nó thực hiện điều này với một hệ thống *cầu chì*. Mỗi *cầu chì* đại diện cho một số loại quyền cho tên. Khi bạn đốt *cầu chì*, bạn thường thu hồi một số loại quyền. Trong giao diện người dùng ENS Manager, điều này có sẵn trong phần "Permission" của tên.

Ví dụ: theo mặc định khi đặt tên, bạn có thể tự do chuyển NFT, giống như bạn có thể làm với các NFT khác. Tuy nhiên, nếu *cầu chì* “Cannot Transfer” bị cháy, thì NFT sẽ không thể chuyển nhượng được. Trong ENS Manager UI, bạn sẽ thực hiện việc này bằng cách thu hồi quyền “Can send this name”.

Như tên của nó, một khi bạn đốt cháy cầu chì, bạn không thể hoàn tác hành động đó, ít nhất là cho đến khi hết hạn.

**Expiry** là khoảng thời gian các cầu chì hoạt động. Thời hạn của tên do parent owner đặt. Đối với tên second-level .eth, ngày hết hạn được đặt thành cùng thời hạn từ .eth registrar. Khi hết hạn, tất cả các cầu chì được đặt lại. Ngoài ra, nếu tên của bạn được *giải phóng* hoặc *bị khóa*, thì bạn sẽ mất quyền sở hữu tên khi hết hạn. Phần sau đó đi sâu vào chi tiết hơn về expiry.

Ngoài ra còn có các cầu chì đặc biệt xác định mối quan hệ giữa tên "cha" và "con" hoặc thay đổi trạng thái của tên được bọc. Một tên có thể ở một trong những trạng thái sau:

- **Unregistered**: Tên thậm chí chưa được đăng ký/tạo hoặc đã hết hạn.
- **Unwrapped**: Tên tồn tại và chưa hết hạn (trong trường hợp tên second-level .eth). Name Wrapper contract không có quyền sở hữu tên. Bạn sở hữu tên trong Registry và/hoặc .eth registrar.
- **Wrapped**: Name Wrapper contract có quyền sở hữu tên (trong Registry/Registrar). Đổi lại, bạn được cấp một ERC-1155 NFT, điều này chứng minh rằng bạn là chủ sở hữu thực sự. Bạn có thể unwrap tên bất kỳ lúc nào, thao tác này sẽ burn ERC-1155 NFT và trả lại quyền sở hữu trong Registry/Registrar cho bạn. Nếu tên của bạn là một tên phụ như `sub.name.eth`, thì owner của `name.eth` về mặt kỹ thuật có thể thay thế tên phụ đó và chuyển nó cho một owner khác. Ngoài ra, parent owner có thể đốt cầu chì *parent-controlled* trong tên của bạn.
- **Emancipated**: owner của parent name không còn có thể thay thế tên này hoặc đốt thêm bất kỳ *cầu chì* trong đó. Tất cả các tên second-level .eth (như `name.eth`) sẽ tự động được đưa vào trạng thái **Giải phóng** khi được bọc. Owner vẫn có thể *unwrap* và *rewrap* tên.
- **Locked**: Tên không còn có thể được unwrap. Owner có thể đốt cầu chì *owner-controlled* trong nó. Các cầu chì cho tên phụ (`sub1.name.eth`) của tên này (`name.eth`) cũng có thể bị đốt.

![Alt text](image-9.png)\
*State Machine for the ENS Name Wrapper*

## Example - Step By Step
### Wrapped Subnames
Giả sử bạn có `name.eth` và `sub1.name.eth`, cả hai đều được bọc. Bạn đã *lock* `name.eth`, nhưng chưa có cầu chì nào bị đốt trên tên phụ.

Tên phụ được **Bọc** nhưng chưa được **Giải phóng**, vì vậy parent owner vẫn có toàn quyền kiểm soát. Kiểm tra sơ đồ bên dưới, xem có mỏ neo ở phía trên bên trái của tên phụ không? Điều này có nghĩa là parent owner vẫn có thể kiểm soát tên phụ. Nó có thể đốt cầu chì *Parent-Controlled* hoặc thậm chí có thể thay thế hoàn toàn tên phụ nếu muốn.

![Alt text](image-10.png)

### Burning Parent-Controlled Fuses
Khi ở trạng thái này, parent owner có thể đốt cầu chì *Parent-Controlled* trong khi tiếp tục có toàn quyền kiểm soát tên phụ.

Có tổng số 3 cầu chì *Parent-Controlled* được xác định trước, và sau đó có 13 cầu chì không xác định mà bạn có thể sử dụng theo ý muốn. Một cách để nghĩ về những cầu chì này là "đặc quyền" cho owner tên phụ.

***Ví dụ***: nếu bạn đang sử dụng tên phụ ENS để cấp vé cho một sự kiện, thì bạn có thể sử dụng các cầu chì *Parent-Controlled* này để mở khóa các đặc quyền, chẳng hạn như khả năng đổi một số quà tặng hoặc tham gia một sự kiện đặc biệt (thậm chí có thể tự động tích hợp với một trong những khóa cửa thông minh đó khi người đó chạm vào thẻ truy cập!).

Tôi sẽ cho thấy một trong các cầu chì *Parent-Controlled* đang bị đốt trong sơ đồ bên dưới. Trong trường hợp này, đó là cầu chì *CAN_EXTEND_EXPIRY*, cho phép owner tên phụ kéo dài thời hạn sử dụng của chính họ (có một phần sau sẽ đề cập đến tất cả các cầu chì và tác dụng của chúng).

![Alt text](image-11.png)

### Burning the “Parent Cannot Control” Fuse
Để giải phóng tên phụ, hãy đốt cầu chì "Parent Cannot Control" đặc biệt.

Sau khi đốt, parent sẽ không thể đốt thêm cầu chì nào nữa. Chủ sở hữu của parent name cũng sẽ không thể thay thế tên phụ.

Hãy nhớ rằng mỏ neo ở trên cùng bên trái của sơ đồ? Đốt cầu chì PCC sẽ thổi bay điều đó! Không có cái mỏ neo đó, child sẽ rời xa parent và cắt đứt tất cả những sợi dây còn lại. Vì vậy, parent owner sẽ không còn quyền kiểm soát đối với child này!

Ngoài ra, thấy cầu chì khác đi xuống bên dưới PCC k? Nó cũng sẽ "phá" một phần bức tường, trao cho chủ sở hữu tên phụ quyền truy cập vào “hộp diêm” của chính họ! Giờ đây, owner tên phụ sẽ có thể tự đốt cầu chì.

![Alt text](image-12.png)

### Emancipated Subnames
Tên phụ `sub1.name.eth` hiện đã được giải phóng.

Parent không còn có thể đốt bất kỳ cầu chì nào hoặc thay thế tên phụ (cho đến khi hết hạn).

Mặc dù vậy, tên phụ vẫn chưa bị **Lock**, vì vậy owner của tên phụ vẫn có thể *unwrap* tên nếu họ muốn! Nếu tên được *unwrap* ra và sau đó được *rewrap*, mọi thứ sẽ vẫn ở trạng thái **Giải phóng** giống như các ngòi nổ đã được đốt cháy.

Vì tên này chưa được **Lock** nên owner của tên phụ không thể đốt bất kỳ cầu chì nào khác (ngoài "Cannot Unwrap"). Owner cũng không thể đốt bất kỳ cầu chì nào trên bất kỳ tên phụ nào của chính nó.

Xem phần bên dưới hiện có màu xanh lá cây? Giờ đây, parent đã đốt cháy PCC, owner có quyền truy cập vào “hộp diêm” của riêng mình và có thể bắt đầu chơi với lửa!

Bạn cũng có thể nhận thấy rằng có thể truy cập cầu chì cho “Cannot Unwrap” từ trong cùng phần này…

![Alt text](image-13.png)

### Burning the “Cannot Unwrap” Fuse
Để **Lock** tên, đốt cầu chì "Cannot Unwrap" đặc biệt.

Cầu chì chỉ có thể bị đốt cháy nếu cầu chì PCC (Parent Cannot Control) đã bị đốt lần đầu tiên bởi parent. Nói cách khác, nó chỉ có thể bị đốt cháy nếu tên đó đã được **Giải phóng**.

Bạn có thấy bó thuốc nổ có chữ "CU" bên cạnh không? Nó sẽ nổ khi bạn đốt cầu chì "Cannot Unwrap". Nếu bạn theo các cầu chì khác được gắn vào nó, bạn sẽ thấy rằng nó cũng sẽ làm nổ một số thứ khác mà tôi sẽ đề cập tiếp theo.

![Alt text](image-14.png)

### Locked Subnames
Tên phụ `sub1.name.eth` hiện đã bị khóa. Điều này có nghĩa là tên không còn có thể được *unwrap*.

Bây giờ tên đã bị **Lock**, owner có thể đốt các cầu chì "Owner-Controlled" khác. Chủ sở hữu cũng có thể đốt cầu chì cho bất kỳ tên phụ nào của riêng mình.

Có tổng số 7 cầu chì "Owner-Controlled" được xác định trước và sau đó có 9 cầu chì không xác định mà bạn có thể sử dụng theo cách mình muốn.

![Alt text](image-15.png)

### Burning Owner-Controlled Fuses
Ví dụ: bạn có thể đốt "Cannot Transfer" và "Cannot Set Resolver".

Giờ đây, tên không thể được chuyển nhượng/bán và resolver contract không còn có thể bị ghi đè cho đến khi hết hạn.

![Alt text](image-16.png)

### Burning the “Cannot Burn Fuses” Fuse
Nếu bạn đốt cầu chì đặc biệt "Cannot Burn Fuses”", thì không thể đốt cầu chì nào nữa trong tên.

Parent owner có thể đốt cầu chì này trên tên phụ để đảm bảo rằng một số quyền vẫn "được mở".

Owner của tên cũng có thể chọn đốt nó. Ví dụ: nếu tên sử dụng subdomain registrar, owner có thể để “Cannot Create Subname” không cháy, sau đó đốt cầu chì "Cannot Burn Fuses" để đảm bảo rằng tên phụ mới luôn có thể được đăng ký.

Bạn có thấy gói thuốc nổ còn lại có chữ “CBF” bên cạnh không? Khi điều đó xảy ra, tất cả các Cầu chì do owner kiểm soát khác sẽ không thể truy cập được. Tôi đã khoanh tròn nó màu xanh bên dưới:

![Alt text](image-17.png)

### All Fuses Frozen
Cầu chì "Cannot Burn Fuses" hiện đã bị đốt cháy.

Các cầu chì cho tên hiện đã bị đóng băng hoàn toàn cho đến khi hết hạn. Bất kỳ cầu chì nào đã cháy trước đó sẽ vẫn ở trạng thái đã cháy đó, nhưng bây giờ không thể đốt cháy cầu chì nào khác trên tên này.

Tuy nhiên, owner vẫn có thể đốt cháy cầu chì của bất kỳ tên phụ nào, giả sử rằng nó vẫn chưa giải phóng chúng.

![Alt text](image-18.png)

## Example - Parent Locks Subname All-In-One
Ví dụ: giả sử bạn muốn giải phóng và khóa một tên miền phụ, đồng thời đốt cầu chì "Parent-Controlled" và đốt một số cầu chì "Owner-Controller"?

Để làm điều đó, bạn sẽ đốt "Parent Cannot Control" và "Cannot Unwrap" và tất cả các cầu chì khác mà bạn muốn, tất cả trong một giao dịch.

Hãy nhớ rằng, một khi PCC bị đốt cháy, parent không thể đốt cháy thêm cầu chì nào nữa.

Ngoài ra, nếu bạn muốn đốt bất kỳ cầu chì "Owner-Controller" nào, bạn cũng phải đốt CU (Cannot Unwrap). Bạn có thể nhận thấy "CU Lock" màu tím trên sơ đồ. Điều đó chỉ ra rằng để đốt cháy bất kỳ cầu chì nào trong số đó, bạn cũng phải đốt cháy CU.

Ngoài ra, cầu chì CU chỉ có thể được đốt cháy nếu PCC đã được đốt cháy hoặc được đốt cháy cùng một lúc. Tương tự, có "PCC Lock" màu tím trên sơ đồ. Điều đó chỉ ra rằng để đốt CU, bạn cũng phải đốt PCC.

![Alt text](image-19.png)

Sau khi đốt cháy các cầu chì đó, tên phụ hiện đã bị Lock.

Ngoài ra, cầu chì "Can Extend Expiry" đã bị cháy và cầu chì "Cannot Transfer" cũng bị cháy.

Owner tên phụ vẫn có khả năng đốt các cầu chì khác và tạo tên phụ mới của riêng mình trong trường hợp này. Parent có thể đã quyết định hạn chế mọi thứ hơn nữa bằng cách đốt các cầu chì khác, như "Cannot Create Subname" hoặc "Cannot Burn Fuses".

Nhưng bây giờ parent đã đốt PCC, nó đã từ bỏ quyền kiểm soát tên phụ này và nó không còn có thể đốt cháy bất kỳ thứ gì khác.

![Alt text](image-20.png)

Vì vậy, chúng ta vừa xem cách bạn có thể lấy một tên phụ được bọc hiện có và **Giải phóng/Khóa** nó, với các cầu chì bị đốt cháy, tất cả chỉ trong một bước.

Bạn biết đấy, điều tương tự cũng có thể được thực hiện khi bạn tạo một tên con hoàn toàn mới!

Nếu tên của bạn được bao bọc và bạn tạo một tên phụ mới bên dưới tên đó, thì tên phụ đó cũng sẽ được bao bọc theo mặc định. Đồng thời, trong cùng một transaction, bạn cũng có thể truyền vào danh sách cầu chì mong muốn của mình để đốt (và hết hạn).

Vì vậy, bạn không cần phải tạo một tên phụ và sau đó đốt các cầu chì riêng biệt. Bạn có thể làm mọi thứ trong một giao dịch.

## Example - Multiple Subnames in Various States
## Name Wrapper Fuses
## Name Wrapper Expiry
## Approved Operators

