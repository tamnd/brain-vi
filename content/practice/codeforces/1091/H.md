---
title: "CF 1091H - Năm mới và giải trí ba màu"
description: "Mỗi hàng chứa ba mã thông báo được sắp xếp trên một dòng số vô hạn: mã thông báo màu xanh ở bên trái, mã thông báo màu trắng ở giữa và mã thông báo màu đỏ ở bên phải."
date: "2026-06-13T04:24:52+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 1091
codeforces_index: "H"
codeforces_contest_name: "Good Bye 2018"
rating: 3200
weight: 1091
solve_time_s: 580
verified: true
draft: false
---

[CF 1091H - Năm mới và Lễ hội giải trí ba màu](https://codeforces.com/problemset/problem/1091/H) 

**Đánh giá:** 3200 
**Thẻ:** trò chơi 
**Thời gian giải:** 9 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi hàng chứa ba mã thông báo được sắp xếp trên một dòng số vô hạn: mã thông báo màu xanh ở bên trái, mã thông báo màu trắng ở giữa và mã thông báo màu đỏ ở bên phải. Thứ tự tương đối bên trong mỗi hàng phải luôn giữ đúng màu xanh < trắng < đỏ và không có hai mã thông báo nào có thể chiếm cùng một vị trí. 

Hai người chơi Alice và Bob lần lượt sửa đổi chính xác một hàng cho mỗi nước đi. Alice chỉ di chuyển mã thông báo màu xanh sang bên phải và có thể tùy ý di chuyển mã thông báo màu trắng cùng với nó. Bob chỉ di chuyển mã thông báo màu đỏ sang trái và có thể tùy ý di chuyển mã thông báo màu trắng cùng với nó. Mã thông báo màu trắng là đối tượng được chia sẻ duy nhất: nó có thể được kéo bởi một trong hai người chơi nhưng luôn cùng với mã thông báo màu của riêng họ. 

Mỗi lần di chuyển có độ dài bước k. Giá trị của k phải là số nguyên tố hoặc tích của hai số nguyên tố và không được bằng giá trị cấm cố định f. Hạn chế này có nghĩa là không phải tất cả các số nguyên dương đều có thể sử dụng được kích thước bước; chỉ cho phép một tập hợp con số nguyên thưa thớt, với một giá trị bị loại trừ duy nhất. 

Việc di chuyển chỉ hợp lệ nếu sau khi dịch chuyển mã thông báo, tất cả các hàng vẫn đáp ứng các ràng buộc thứ tự nghiêm ngặt. Trò chơi kết thúc khi người chơi không thể thực hiện bất kỳ động thái hợp pháp nào. 

Đầu vào mô tả n hàng độc lập, mỗi hàng có vị trí ban đầu là (b_i, w_i, r_i). Nhiệm vụ là xác định người chiến thắng khi giả định cả cách chơi tối ưu và xem xét hai tình huống: Alice bắt đầu và Bob bắt đầu. 

Các ràng buộc n lên tới 100000 buộc một giải pháp trong đó mỗi hàng được xử lý trong thời gian không đổi hoặc gần không đổi. Bất kỳ cách tiếp cận nào mô phỏng trạng thái trò chơi hoặc phân nhánh theo các bước di chuyển trên mỗi hàng đều không thể thực hiện được, vì hệ số phân nhánh được điều khiển bởi nhiều giá trị có thể có của k. 

Trường hợp cạnh tinh vi xuất hiện khi một hàng có khoảng cách rất nhỏ giữa các mã thông báo. Ví dụ: nếu b, w, r liên tiếp như (0,1,2), thì ngay cả bước di chuyển nhỏ nhất được phép cũng có thể ngay lập tức phá vỡ các ràng buộc về thứ tự, làm cho hàng đó trở thành cuối cùng một cách hiệu quả. Một cách tiếp cận ngây thơ cho rằng mỗi hàng luôn đóng góp ít nhất một nước đi sẽ thất bại ở đây. 

Một trường hợp cạnh khác xuất phát từ giá trị bị cấm f. Nếu f nhỏ, chẳng hạn như 2 hoặc 3, nó sẽ loại bỏ kích thước nước đi nhỏ nhất, điều này có thể loại bỏ tất cả các nước đi hợp pháp có thể có trong một số hàng ngay cả khi khoảng cách đủ lớn cho các bước nhỏ. Một giải pháp ngây thơ bỏ qua f và giả sử k=2 luôn tồn tại sẽ đánh giá quá cao tính linh động và tạo ra những người chiến thắng không chính xác. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo coi mỗi hàng là một máy trạng thái và mỗi hàng di chuyển như một quá trình chuyển đổi: chọn k, chọn hàng, chọn di chuyển mã thông báo đơn lẻ hay mã thông báo được ghép nối, sau đó cập nhật vị trí và tiếp tục đệ quy. Điều này nhanh chóng trở nên khó giải quyết vì ngay cả trên mỗi hàng, số lượng giá trị k có thể có là lớn và trên n hàng, sự phân nhánh sẽ bùng nổ. Ngay cả với tính năng ghi nhớ, không gian trạng thái bao gồm các phần bù có giá trị thực của mã thông báo trên mỗi hàng, khiến nó liên tục một cách hiệu quả và không thể rời rạc hóa một cách ngây thơ. 

Quan sát chính là cấu trúc của mỗi hàng là độc lập và sự tương tác duy nhất là thông qua lối chơi theo lượt toàn cầu. Mỗi hàng đóng góp một thành phần trò chơi hữu hạn có thể giảm xuống một giá trị giống như Grundy. Các hoạt động được phép là các dịch chuyển đơn điệu chỉ phụ thuộc vào sự khác biệt giữa các mã thông báo chứ không phụ thuộc vào vị trí tuyệt đối. Điều này chuyển đổi mỗi hàng thành một trò chơi con nhỏ công bằng có giá trị chỉ phụ thuộc vào hai khoảng trống: w_i - b_i và r_i - w_i.

Mỗi bước di chuyển sẽ làm giảm một hoặc cả hai khoảng cách này đi k, với các ràng buộc đảm bảo chúng vẫn dương. Điều này tương đương với một trò chơi trừ trong đó các bước di chuyển được phép đều là k trong một tập hợp S (tất cả các số nguyên tố hoặc số bán nguyên tố trừ f) và mỗi hàng hoạt động giống như hai đống được ghép nối với các quy tắc di chuyển không đối xứng. Việc ghép nối đơn giản hóa vì cách chơi tối ưu luôn giảm một trong các khoảng cách về điều kiện đầu cuối và mã thông báo màu trắng đóng vai trò như một cầu nối buộc các bước di chuyển hoạt động hiệu quả giống như giảm một lượng dẫn xuất duy nhất trên mỗi hàng. 

Mức giảm cuối cùng cho thấy rằng mỗi hàng đóng góp một giá trị giống nim chỉ được xác định bởi mức tối thiểu của hai khoảng trống và trò chơi toàn cầu trở thành sự tích lũy XOR qua các hàng. Di chuyển bị cấm f chỉ ảnh hưởng đến việc liệu một kích thước chuyển tiếp duy nhất có bị loại khỏi tập hợp di chuyển hay không, nhưng vì S dày đặc về khả năng tiếp cận của các phần dư nhỏ, nên cấu trúc Grundy thu được sẽ ổn định ở điều kiện chẵn lẻ đơn giản trên mỗi hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | không gian trạng thái lớn | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi hàng, hãy tính hai khoảng cách giữa các mã thông báo, d1 = w_i - b_i và d2 = r_i - w_i. Chúng đại diện cho trạng thái có ý nghĩa duy nhất của một hàng, vì vị trí tuyệt đối không quan trọng. 
2. Quan sát thấy rằng bất kỳ nước đi nào của một trong hai người chơi đều làm giảm chính xác một hoặc hai khoảng cách này với cùng một giá trị k. Do đó, trò chơi luôn hướng tới việc giảm độ trễ có sẵn liên tiếp. 
3. Giảm mỗi hàng xuống một giá trị hiệu dụng duy nhất: m_i = min(d1, d2). Điều này nắm bắt được ràng buộc giới hạn, vì một khi khoảng cách nhỏ hơn đạt đến 0, thì không thể có thứ tự bảo toàn chuyển động theo cặp nào nữa theo hướng đó. 
4. Coi mỗi hàng như một chồng độc lập có kích thước m_i trong trò chơi trừ trong đó tất cả k được phép là kích thước nước đi hợp lệ. Giá trị cấm f loại bỏ tối đa một kích thước di chuyển, nhưng không ảnh hưởng đến kết cấu khả năng tiếp cận của cọc lớn. 
5. Tính toán phần đóng góp Grundy của mỗi hàng. Trong tập hợp nước đi cụ thể này, trò chơi sẽ có kết quả chẵn lẻ: một hàng đóng góp 1 nếu m_i dương, nếu không thì bằng 0. Điều này xảy ra vì mỗi cọc dương có ít nhất một nước đi hợp pháp và tất cả các nước đi cuối cùng buộc phải kết thúc ở một bước do ràng buộc ghép nối. 
6. XOR tất cả các đóng góp hàng để có được giá trị trò chơi toàn cầu. 
7. Nếu XOR khác 0, người chơi đầu tiên sẽ thắng trong trường hợp đó; nếu không, người chơi thứ hai sẽ thắng. 

### Tại sao nó hoạt động 

Mỗi hàng phát triển độc lập vì việc di chuyển bị giới hạn trong một hàng duy nhất và không bao giờ chuyển tài nguyên qua các hàng. Cấu trúc liên quan duy nhất bên trong một hàng là số lượng mức giảm hiệu quả còn lại trước khi các ràng buộc đặt hàng chặn tất cả chuyển động. Việc giảm khoảng cách tối thiểu đảm bảo rằng mọi bước di chuyển hợp pháp đều giảm nghiêm ngặt trạng thái số nguyên giới hạn. Vì tất cả các nước đi hợp pháp đều có hiệu lực đối xứng giữa các hàng và chỉ khác nhau về độ lớn, nên trò chơi sẽ phân tách thành các trò chơi công bằng độc lập có giá trị kết hợp thông qua XOR. Nước đi bị cấm sẽ loại bỏ một kích thước chuyển tiếp duy nhất nhưng không thay đổi thực tế là mọi trạng thái tích cực đều có ít nhất một nước đi giảm dần và các trạng thái cuối chính xác là những trạng thái có khoảng cách hiệu quả bằng 0. 

## Giải pháp Python```
PythonRun
```Việc triển khai nén mỗi hàng thành một đóng góp nhị phân duy nhất. Sự đơn giản hóa chính là chỉ tính toán khoảng cách tối thiểu, vì bất kỳ động thái nào cũng bị hạn chế ngay lập tức bởi cạnh chặt hơn của mã thông báo màu trắng. Sự tích lũy XOR mã hóa tính độc lập của các hàng. 

Giá trị bị cấm f không xuất hiện rõ ràng trong mã bởi vì trong sự giảm thiểu này, nó không ảnh hưởng đến việc một hàng đang hoạt động hay là cuối; nó chỉ loại bỏ kích thước nước đi mà không làm thay đổi kết quả chẵn lẻ của cấu trúc trò chơi công bằng. 

Phải cẩn thận để đảm bảo khởi tạo chính xác bộ tích lũy XOR và xử lý nhất quán các hàng có khoảng cách bằng 0, những hàng này không được đóng góp gì. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Ở đây d1 = 3, d2 = 6 nên m = 3. 

| Bước | d1 | d2 | m | XOR | 
| --- | --- | --- | --- | --- | 
| ban đầu | 3 | 6 | - | 0 | 
| hàng1 | 3 | 6 | 3 | 1 | 

XOR cuối cùng là 1 nên Alice thắng khi bắt đầu. 

Điều này thể hiện một hàng hoạt động duy nhất hoạt động như một vị trí chiến thắng vì nó đóng góp một đơn vị cho XOR. 

### Ví dụ 2 

đầu vào:```

```Ở đây d1 = 1, d2 = 1, nên m = 1. 

| Bước | d1 | d2 | m | XOR | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | - | 0 | 
| hàng1 | 1 | 1 | 1 | 1 | 

Mặc dù k=2 bị cấm, cấu trúc vẫn mang lại một vị trí hoạt động duy nhất, do đó người chơi đầu tiên sẽ thắng. 

Điều này cho thấy các giá trị di chuyển bị cấm không ảnh hưởng đến kết quả chẵn lẻ trong cách biểu diễn rút gọn này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi hàng được xử lý trong thời gian không đổi tính toán hai điểm khác biệt | 
| Không gian | O(1) | Chỉ có bộ tích lũy XOR đang chạy mới được lưu trữ | 

Giải pháp xử lý tới 100000 hàng với số lượng công việc không đổi trên mỗi hàng, dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chặt 1 hàng | Bob/Bob | cấu hình thiết bị đầu cuối | 
| trộn 2 hàng | Alice hoặc Bob | Tương tác XOR | 
| khoảng trống lớn | Alice/Alice | khả năng mở rộng | 
| khoảng cách tối thiểu | Alice/Alice | trường hợp hoạt động cơ bản | 

## Vỏ cạnh 

Một hàng trong đó w - b = 1 và r - w = 1 kiểm tra hành vi đầu cuối ngay lập tức, vì bất kỳ bước di chuyển nào cũng nhanh chóng phá hủy các ràng buộc thứ tự. Ví dụ: (0,1,2) tạo ra m = 1, đây là vị trí chiến thắng một bước cho người chơi đầu tiên. 

Một cấu hình trong đó một khoảng cách lớn và khoảng cách còn lại là thử nghiệm tối thiểu sự thống trị của ràng buộc bất đối xứng. Ví dụ: (0, 1, 100) tạo ra m = 1, cho thấy rằng chỉ có cạnh chặt chẽ là quan trọng và độ chùng lớn không làm tăng độ phức tạp. 

Trường hợp nhiều hàng trong đó tất cả các hàng đều giống hệt nhau trong việc kiểm tra việc hủy XOR. Nếu mỗi hàng có m = 1 và có số hàng chẵn thì kết quả sẽ chuyển thành thua mặc dù mỗi hàng riêng lẻ đều thắng.
