---
title: "CF 989A - Sự hòa quyện của mùa xuân"
description: "Chúng ta được cung cấp một chuỗi ngắn đại diện cho một hàng ô. Mỗi ô trống hoặc chứa chính xác một bông hoa thuộc một trong ba loại, được mã hóa thành các chữ cái A, B và C."
date: "2026-06-17T00:43:16+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 989
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 487 (Div. 2)"
rating: 900
weight: 989
solve_time_s: 76
verified: true
draft: false
---

[CF 989A - Sự hòa quyện của mùa xuân](https://codeforces.com/problemset/problem/989/A) 

**Xếp hạng:** 900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn đại diện cho một hàng ô. Mỗi ô trống hoặc chứa chính xác một bông hoa thuộc một trong ba loại, được mã hóa thành các chữ cái A, B và C. Khi một bông hoa “chết”, nó sẽ biến mất khỏi ô của chính nó và lan màu sang các ô lân cận bên trái và bên phải của nó. Nếu một người hàng xóm ở ngoài chuỗi, mức chênh lệch đó sẽ bị bỏ qua. Nhiều bông hoa có thể chết theo bất kỳ thứ tự nào và ảnh hưởng của chúng sẽ tích lũy. 

Câu hỏi đặt ra là liệu có tồn tại chuỗi cái chết nào đó sao cho tại một thời điểm nào đó, ít nhất một ô chứa cả ba màu cùng một lúc hay không. Một tế bào có thể tích lũy màu sắc từ các loại hoa khác nhau; chúng tôi chỉ quan tâm liệu cả ba màu riêng biệt có thể xuất hiện ở cùng một vị trí hay không sau một số kết hợp chênh lệch. 

Độ dài chuỗi tối đa là 100, do đó, bất kỳ giải pháp nào kiểm tra tất cả các cấu hình cục bộ có liên quan hoặc mô phỏng các kết hợp nhỏ đều đủ nhanh. Một lực lượng mạnh mẽ đối với tất cả các tập hợp con hoa sẽ rất nhỏ vì có tối đa 100 vị trí, nhưng ngay cả điều đó cũng không cần thiết. Cấu trúc của sự lan truyền về cơ bản làm cho vấn đề trở nên cục bộ. 

Trường hợp cạnh tinh tế là khi cả ba màu đều tồn tại nhưng cách nhau quá xa để có thể gặp nhau trong một ô duy nhất. Ví dụ: nếu tất cả hoa A bị cô lập ở ngoài cùng bên trái và B và C ở ngoài cùng bên phải thì sự lan tỏa không bao giờ tạo ra sự chồng chéo ba lần. Một trường hợp khác là khi một trong các màu bị thiếu hoàn toàn, ví dụ: "A.B.A", trong đó dù chúng ta kích hoạt cái chết bằng cách nào, chúng ta không bao giờ có thể tạo ra cả ba màu. 

## Phương pháp tiếp cận 

Quan điểm brute-force là coi mọi tập hợp con của các bông hoa là tập hợp "chết", tính toán mô hình trải rộng kết quả và kiểm tra xem có ô nào tích lũy A, B và C hay không. Đối với một tập hợp con cố định, mỗi cái chết đóng góp nhiều nhất hai lần cập nhật lân cận, do đó việc mô phỏng một tập hợp con có giá O(n). Có 2^n tập hợp con, vì vậy điều này trở thành O(n 2^n), điều này hoàn toàn không khả thi ngay cả với n = 100. 

Quan sát quan trọng là chúng ta thực sự không cần mô phỏng động lực học hoặc trật tự. Một ô cuối cùng sẽ chứa một màu khi và chỉ nếu ban đầu nó chứa bông hoa đó hoặc nó nằm cạnh ít nhất một bông hoa có màu đó mà cuối cùng sẽ chết. Vì chúng ta được phép chọn những bông hoa nào sẽ chết, điều quan trọng là liệu có tồn tại những vị trí có thể đóng góp chung cả ba màu vào một ô mục tiêu hay không. 

Sửa ô i làm điểm gặp gỡ tiềm năng. Đối với mỗi màu, chúng ta chỉ quan tâm liệu có tồn tại ít nhất một lần xuất hiện của màu đó ở các vị trí i − 1, i hoặc i + 1 hay không. Nếu một bông hoa có màu đó tồn tại ở bất kỳ đâu trong vùng lân cận này, chúng ta có thể buộc nó chết đi và gửi màu đó vào i. Nếu không có bông hoa như vậy tồn tại thì màu sắc đó không bao giờ có thể xuất hiện trong tôi dưới bất kỳ chuỗi thao tác nào. Do đó, vấn đề giảm xuống còn việc kiểm tra xem vị trí i nào đó có đủ ba màu trong vùng lân cận đóng của nó hay không. 

Điều này làm giảm toàn bộ quá trình động thành một cửa sổ kiểm tra trượt có kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi quét mọi vị trí của chuỗi và coi nó như một ô ứng cử viên nơi cả ba màu có thể gặp nhau.

1. Đối với mỗi chỉ mục i trong chuỗi, chúng ta kiểm tra chuỗi con được hình thành bởi các vị trí i − 1, i và i + 1, bỏ qua các chỉ mục bên ngoài ranh giới mảng. Điều này nắm bắt tất cả các nguồn có thể có thể đóng góp trực tiếp cho i. 
2. Chúng tôi theo dõi A, B và C nào xuất hiện trong vùng lân cận địa phương này. Mỗi nhân vật đóng góp màu sắc riêng của mình ngay lập tức, và bất kỳ bông hoa hiện tại nào sau đó cũng có thể bị buộc phải chết để đóng góp màu sắc của mình cho i. 
3. Nếu cả ba màu xuất hiện trong vùng lân cận này, chúng ta ngay lập tức kết luận rằng tôi có thể được pha trộn hoàn chỉnh và đạt được thành công. 
4. Nếu chúng ta kiểm tra xong tất cả các vị trí mà không tìm được một lân cận như vậy thì chúng ta kết luận rằng điều đó là không thể. 

Logic phụ thuộc vào thực tế là bất kỳ màu nào không có trong vùng lân cận bán kính-1 đều không thể đến được ô trung tâm, bởi vì chỉ các ô liền kề mới có thể đóng góp cánh hoa. 

### Tại sao nó hoạt động 

Đối với bất kỳ ô cố định i nào, cách duy nhất để một màu có thể xuất hiện trong i là bông hoa đã ở i hoặc nó ở i − 1 hoặc i + 1 và được chọn để chết. Những bông hoa ở xa hơn không bao giờ có thể đóng góp trực tiếp cho i, vì sự lan truyền chỉ đến được những bông hoa lân cận. Do đó, tập hợp các màu có thể xuất hiện trong i chính xác là tập hợp các màu có trong khoảng [i − 1, i + 1]. Nếu sự kết hợp này chứa A, B và C, chúng ta có thể sắp xếp cái chết của những bông hoa tương ứng để đảm bảo cả ba đều đóng góp vào i. Nếu không, ít nhất một màu sẽ không thể truy cập được cho ô đó trong mọi chuỗi có thể, do đó không có cấu trúc hợp lệ nào tồn tại. 

## Giải pháp Python```
import
```
