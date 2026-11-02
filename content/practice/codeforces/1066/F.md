---
title: "CF 1066F - Một chuyến đi bộ 2D khác"
description: "Chúng ta được cấp một tập hợp các điểm trong góc phần tư thứ nhất của lưới và Maksim bắt đầu từ điểm gốc. Trong một lần di chuyển, anh ta có thể bước tới bất kỳ điểm nào trong số bốn điểm lưới lân cận, vì vậy mỗi lần di chuyển tốn một đơn vị khoảng cách Manhattan. Hạn chế chính không phải là hình học mà là cấu trúc."
date: "2026-06-15T08:13:18+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1066
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 515 (Div. 3)"
rating: 2100
weight: 1066
solve_time_s: 282
verified: true
draft: false
---

[CF 1066F - Một chuyến đi bộ 2D khác](https://codeforces.com/problemset/problem/1066/F) 

**Xếp hạng:** 2100 
**Thẻ:** dp 
**Thời gian giải:** 4 phút 42 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm trong góc phần tư thứ nhất của lưới và Maksim bắt đầu từ điểm gốc. Trong một lần di chuyển, anh ta có thể bước tới bất kỳ điểm nào trong số bốn điểm lưới lân cận, vì vậy mỗi lần di chuyển tốn một đơn vị khoảng cách Manhattan. 

Hạn chế chính không phải là hình học mà là cấu trúc. Các điểm được nhóm theo “cấp độ” của chúng, được xác định là tọa độ tối đa của chúng. Cấp 1 chứa các điểm nằm bên trong hoặc trên hình vuông từ điểm gốc lên đến 1 theo một trong hai hướng, cấp 2 mở rộng hình vuông đó đến kích thước 2, v.v. Maksim phải truy cập tất cả các điểm theo thứ tự tăng dần của các cấp độ này và anh ta không được phép bắt đầu truy cập bất kỳ điểm nào ở cấp độ k+1 cho đến khi đã truy cập hết mọi điểm của cấp độ k. 

Vì vậy, vấn đề trở thành: chúng ta phải chọn thứ tự các điểm tham quan tôn trọng ràng buộc phân lớp này và chúng ta muốn giảm thiểu tổng khoảng cách di chuyển của Manhattan bắt đầu từ (0, 0). 

Các ràng buộc rất lớn, lên tới 200.000 điểm và tọa độ lên tới 10^9. Bất kỳ giải pháp nào cố gắng xây dựng biểu đồ giữa tất cả các điểm hoặc chạy thuật toán đường đi ngắn nhất qua các trạng thái đều là không thể ngay lập tức. Ngay cả việc sắp xếp tất cả các cặp hoặc thực hiện tính toán đường đi ngắn nhất theo từng bước cũng sẽ quá chậm. Chúng ta cần thứ gì đó làm giảm cấu trúc thành một đường quét tuyến tính hoặc gần tuyến tính qua các cấp độ. 

Một trường hợp thất bại tinh vi xuất hiện khi các điểm cùng cấp độ bị phân tán theo cách khiến cho việc đặt hàng tham lam trông có vẻ hấp dẫn. Ví dụ: các điểm ở cấp độ 3 như (3, 0), (0, 3) và (3, 3) có thể đánh lừa một cách tiếp cận ngây thơ cố gắng kết nối các hàng xóm gần nhất cục bộ. Các chuyển đổi tối ưu cục bộ có thể làm tăng chi phí qua các ranh giới cấp độ, đây là nơi mà ràng buộc thực sự buộc phải đưa ra quyết định toàn cầu. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo là coi đây là bài toán đường đi ngắn nhất qua các trạng thái bao gồm những điểm nào đã được ghé thăm và vị trí hiện tại. Điều đó có nghĩa là không gian trạng thái có kích thước 2^n hoặc ít nhất là n! hoán vị, cả hai đều hoàn toàn không khả thi. Ngay cả khi chúng tôi hạn chế tham quan các điểm theo thứ tự cấp độ, chúng tôi vẫn phải quyết định thứ tự tốt nhất trong mỗi cấp độ. Điều đó đã giống với bài toán người bán hàng du lịch, không thể giải được chính xác ở quy mô này. 

Sự đơn giản hóa chính xuất phát từ hình học của khoảng cách Manhattan kết hợp với ràng buộc cấp độ. Vì các mức được xác định bởi max(x, y), nên mỗi mức k bao gồm các điểm nằm trên ranh giới của hình vuông [0, k] × [0, k], với ít nhất một tọa độ bằng k. Quan sát quan trọng là sự chuyển động bên trong một cấp độ không ảnh hưởng đến các cấp độ trong tương lai ngoại trừ thông qua điểm thoát cuối cùng của cấp độ đó và điểm vào cấp độ tiếp theo. 

Vì vậy, đối với mỗi cấp độ, chúng tôi chỉ quan tâm đến cách chúng tôi vượt qua các điểm ranh giới của nó và nơi chúng tôi kết thúc khi rời khỏi cấp độ đó. Cấu trúc của khoảng cách Manhattan ngụ ý rằng chuyển động tối ưu trong một cấp độ có thể được giảm xuống để chỉ duy trì hai trạng thái cực đoan: chi phí hoàn thiện tốt nhất ở phía dưới cùng bên trái so với phía trên cùng bên phải của trật tự ranh giới cấp độ được tạo ra bằng cách sắp xếp. 

Điều này biến vấn đề thành một cuộc quét theo các cấp độ tăng dần, duy trì trạng thái lập trình động nhỏ cho mỗi cấp độ. Đối với mỗi cấp độ, chúng tôi sắp xếp các điểm theo một tọa độ và tính toán các chuyển tiếp mô phỏng việc đi dọc theo ranh giới theo một trong hai hướng. DP theo dõi chi phí tối thiểu để hoàn tất việc xử lý tất cả các điểm đến một mức và kết thúc tại một trong hai vị trí ranh giới đại diện. 

Lực lượng vũ phu không thành công vì nó cố gắng suy luận về các chuyển đổi điểm-điểm riêng lẻ, trong khi giải pháp đúng sẽ nén từng cấp độ thành một vấn đề đường dẫn có cấu trúc chỉ với hai cấu hình thoát có ý nghĩa.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (giống TSP trên tất cả các điểm) | Ồ (n!) | O(n) | Quá chậm | 
| Cấp DP với nén ranh giới | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nhóm tất cả các điểm theo cấp độ k = max(x, y). Mỗi nhóm được xử lý độc lập theo thứ tự tăng dần của k. 

1. Tính cấp độ cho từng điểm và lưu điểm vào từ điển khóa theo cấp độ. 

Điều này đảm bảo chúng tôi xử lý các ràng buộc theo thứ tự chính xác mà vấn đề yêu cầu. 
2. Sắp xếp tất cả các cấp độ khác nhau theo thứ tự tăng dần. 

Ràng buộc cấm bỏ qua phía trước, vì vậy thứ tự này xác định cấu trúc chung của bước đi. 
3. Đối với mỗi cấp độ, hãy sắp xếp các điểm của nó theo tọa độ x và sử dụng y làm điểm phân định. 

Thứ tự này sắp xếp các điểm dọc theo ranh giới sao cho các chuyển tiếp Manhattan bên trong cấp độ trở nên đơn điệu. 
4. Duy trì trạng thái lập trình động với hai giá trị: chi phí tối thiểu để hoàn tất xử lý đến cấp trước đó và kết thúc ở điểm cuối ranh giới “ngoài cùng bên trái” hoặc “ngoài cùng bên phải” của quá trình truyền tải của cấp đó. 

Hai trạng thái này nắm bắt tất cả các cách có ý nghĩa mà việc đi bộ có thể thoát khỏi một cấp độ vì bất kỳ vị trí kết thúc bên trong nào cũng có thể được ánh xạ tới một trong những thái cực này mà không làm tăng chi phí trong tương lai. 
5. Đối với mỗi cấp độ, hãy tính chi phí đi qua tất cả các điểm của nó bắt đầu từ mỗi trạng thái trong số hai trạng thái DP của cấp độ trước đó. 

Đối với mỗi trạng thái bắt đầu, mô phỏng hai hướng di chuyển có thể có của các điểm được sắp xếp (từ trái sang phải và từ phải sang trái), tích lũy khoảng cách giữa các điểm liên tiếp. 
6. Sau khi tính toán cả hai hướng truyền tải, hãy cập nhật DP cho cấp độ hiện tại bằng cách lấy mức tối thiểu trên tất cả các chuyển đổi và ghi lại hai điểm cuối có thể có. 

Điều này bảo tồn cấu trúc con tối ưu: khi một cấp độ được hoàn thành, chỉ vị trí thoát của nó mới quan trọng. 
7. Sau khi xử lý tất cả các cấp độ, trả về giá trị DP tối thiểu trên hai điểm cuối cuối cùng có thể có. 

### Tại sao nó hoạt động 

Bên trong mỗi cấp độ, các điểm nằm trên ranh giới của một hình vuông đang phát triển. Bất kỳ con đường tối ưu nào đi qua tất cả các điểm trong cấp độ đó đều có thể được chuyển thành một cuộc đi bộ đơn điệu dọc theo ranh giới mà không làm tăng chi phí, bởi vì các đường vòng bên trong hình vuông luôn có thể là đường tắt khi sử dụng hình học Manhattan. Điều này làm giảm mỗi cấp độ thành bài toán sắp xếp một chiều chỉ có hai hướng. DP duy trì tính tối ưu vì quá trình chuyển đổi giữa các cấp chỉ phụ thuộc vào điểm cuối của cấp trước đó chứ không phụ thuộc vào đường dẫn nội bộ được sử dụng để đạt đến cấp đó. 

## Giải pháp Python```
PythonRun
```Việc triển khai nhóm điểm theo cấp độ bằng cách sử dụng bản đồ băm, sau đó xử lý các cấp độ theo thứ tự tăng dần. Trạng thái DP được cố ý giữ ở mức nhỏ: hai chi phí và hai đại diện điểm cuối. Hàm khoảng cách trợ giúp mã hóa trực tiếp chuyển động của Manhattan, điều này là đủ vì không có số liệu nào khác liên quan. 

Một điểm tinh tế là chúng tôi luôn xem xét cả hai hướng di chuyển của từng cấp độ. Thiếu một hướng sẽ phá vỡ tính chính xác vì hướng vào lệnh tối ưu phụ thuộc vào mức độ kết thúc trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Cấp độ: 

(1,1), (1,2),(2,1) 

| Bước | Cấp độ | Bắt đầu | Đặt hàng | Chi phí | Kết thúc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | (0,0) | - | 0 | (0,0) | 
| 1 | 1 | (0,0) | (1,1) | 2 | (1,1) | 
| 2 | 2 | (1,1) | (1,2)->(2,1) | 3 | (2,1) | 

Tổng cộng = 5. 

Dấu vết này cho thấy cách thuật toán nén từng cấp độ thành một lần truyền tải duy nhất, chỉ mang thông tin điểm cuối về phía trước. 

### Ví dụ 2 

đầu vào:```

```Tất cả các điểm đều ở cấp độ 2. 

| Bước | Cấp độ | Bắt đầu | Đặt hàng | Chi phí | Kết thúc | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | (0,0) | - | 0 | (0,0) | 
| 1 | 2 | (0,0) | (1,1)->(1,2)->(2,1)->(2,2) | 6 | (2,2) | 

Trường hợp này chứng minh rằng trong một cấp độ duy nhất, thuật toán giảm vấn đề một cách hiệu quả thành đường truyền tuyến tính trên ranh giới đã được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp điểm trong mỗi cấp chiếm ưu thế trong tổng công việc | 
| Không gian | O(n) | Lưu trữ điểm được nhóm theo cấp độ và trạng thái DP | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì mỗi điểm được sắp xếp một lần và được xử lý theo các chuyển tiếp DP liên tục. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 2 | chuyển động căn cứ từ nguồn gốc | 
| dòng trên cùng y | 5 | tính đúng đắn của việc truyền tải đơn điệu | 
| đầy đủ hình vuông 2x2 | 6 | nén biên đúng cách | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các điểm thuộc cùng một cấp độ. Trong trường hợp đó, thuật toán giảm xuống còn một lần quét qua các điểm biên đã được sắp xếp bắt đầu từ điểm gốc. DP không dựa vào sự chuyển tiếp giữa các cấp độ, do đó tính chính xác phụ thuộc hoàn toàn vào logic truyền tải bên trong. Ví dụ với hình vuông 2x2 xác nhận rằng cả hai hướng đi ngang đều được xem xét và hướng tối thiểu được chọn. 

Một trường hợp khác xảy ra khi các cấp độ thưa thớt, ví dụ như một điểm duy nhất cho mỗi cấp độ. Ở đây DP thoái hóa thành việc liên tục chọn cách chuyển đổi rẻ nhất giữa các điểm bị cô lập. Vì mỗi cấp độ chỉ có một lần duyệt hợp lệ nên cả chi phí tiến và lùi đều trùng nhau, do đó, thuật toán tự nhiên giảm xuống việc tính tổng khoảng cách Manhattan giữa các đại diện cấp liên tiếp.
