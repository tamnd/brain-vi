---
title: "CF 104968G - Cắt Pizza"
description: "Chúng ta được cung cấp một tập hợp lớn các điểm khác biệt trên một lưới. Mỗi điểm tượng trưng cho một lát pepperoni và chúng ta được yêu cầu dựng một đường thẳng sao cho đường thẳng đó đi rất gần với nhiều điểm trong số này."
date: "2026-06-28T06:48:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104968
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 2 (Beginner)"
rating: 0
weight: 104968
solve_time_s: 28
verified: false
draft: false
---

[CF 104968G - Cắt Pizza](https://codeforces.com/problemset/problem/104968/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp lớn các điểm khác biệt trên một lưới. Mỗi điểm tượng trưng cho một lát pepperoni và chúng ta được yêu cầu dựng một đường thẳng sao cho đường thẳng đó đi rất gần với nhiều điểm trong số này. “Rất gần” được coi một cách hiệu quả là “chính xác trên đường thẳng” cho đến sai số bằng số, vì vậy yêu cầu thực sự là tìm một đường chứa ít nhất ⌊n / 8⌋ điểm. 

Đầu ra là một đường có dạng Ax + By = C. Bất kỳ điểm nào có vế trái ước tính có giá trị cực kỳ gần với C đều được coi là nằm trên đường thẳng. Vì vậy, nhiệm vụ giảm xuống còn việc tìm một đường thẳng liên quan đến ít nhất một phần số điểm, cụ thể là ít nhất một phần tám. 

Các ràng buộc rất lớn, lên tới 100000 điểm. Một giải pháp kiểm tra tất cả các cặp điểm và đếm các bộ ba thẳng hàng sẽ yêu cầu các phép toán O(n^2), vượt xa những gì khả thi trong hai giây. Ngay cả các cách tiếp cận O(n sqrt n) cũng có rủi ro trừ khi được tối ưu hóa nhiều, do đó cấu trúc của vấn đề phải được khai thác. 

Một dạng lỗi tinh vi xuất hiện trong phép đếm hình học đơn giản. Nếu người ta cố gắng liệt kê tất cả các dòng được xác định bởi các cặp điểm và đếm xem có bao nhiêu điểm nằm trên mỗi dòng thì cách tiếp cận đó đúng nhưng mang tính bậc hai về số cặp và sẽ không chia tỷ lệ. Một cạm bẫy phổ biến khác là so sánh độ dốc dấu phẩy động, điều này gây ra các vấn đề về độ chính xác. Ở đây, dung sai đầu ra ẩn việc kiểm tra đẳng thức chính xác, nhưng việc xây dựng vẫn cần phải chính xác về số học số nguyên. 

## Phương pháp tiếp cận 

Chiến lược vũ phu sẽ là xem xét từng cặp điểm, tạo thành một đường duy nhất đi qua chúng và đếm xem có bao nhiêu điểm nằm trên đường đó. Đối với mỗi dòng ứng viên, chúng tôi xác minh tất cả các điểm và theo dõi mức tối đa. Điều này đúng vì bất kỳ dòng nào chứa k điểm sẽ được tạo bởi bất kỳ cặp k chọn 2 nào trong số các điểm đó. Vấn đề là chi phí: có các cặp O(n^2) và mỗi lần xác minh đều có chi phí O(n), cho ra O(n^3) trong trường hợp xấu nhất hoặc O(n^2) nếu chúng ta sử dụng lại hàm băm nhưng vẫn quá lớn đối với n = 10^5. 

Quan sát cấu trúc quan trọng là chúng ta không cần phải tìm đường phổ biến nhất trên toàn cầu một cách xác định. Chúng ta chỉ cần tìm bất kỳ dòng nào chứa một phần điểm đủ lớn. Nếu một đường “nặng” chứa ít nhất n/8 điểm thì việc chọn một điểm ngẫu nhiên sẽ có xác suất không nhỏ để rơi vào đường đó. Khi chúng ta chọn một điểm từ đường nặng, vấn đề sẽ giảm xuống việc tìm vectơ chỉ hướng thường xuyên nhất từ ​​điểm neo đó. 

Đối với một điểm neo cố định, mọi điểm khác đều xác định hướng dốc. Tất cả các điểm nằm trên cùng một đường thông qua mỏ neo đều có cùng độ dốc (theo tỷ lệ). Vì vậy, nếu chúng ta chọn một điểm neo chính xác, đường đúng sẽ trở thành một bài toán đếm tần số đơn giản trên các vectơ chỉ hướng đã được chuẩn hóa. Việc lặp đi lặp lại việc neo ngẫu nhiên này một số lần không đổi khiến rất có khả năng chúng ta chạm vào một điểm từ đường nặng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) hoặc O(n2·log n) | O(n) | Quá chậm | 
| Neo ngẫu nhiên + đếm độ dốc | O(n) dự kiến ​​mỗi lần thử | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào việc lấy mẫu ngẫu nhiên các điểm neo và đếm tần số của vectơ chỉ hướng. 

1. Chọn một điểm ngẫu nhiên làm ứng viên neo. 

Trực giác cho thấy bất kỳ tập hợp cộng tuyến đủ lớn nào cũng có xác suất được lấy mẫu đáng chú ý. 
2. Đối với mỏ neo này, hãy tính vectơ chỉ hướng cho tất cả các điểm khác. 

Mỗi điểm khác xác định một vectơ (dx, dy). Các điểm trên cùng một đường đi qua mỏ neo tạo ra các vectơ tỷ lệ. 
3. Chuẩn hóa mỗi vectơ hướng thành một biểu diễn số nguyên chuẩn.

Chúng ta chia cho gcd(dx, dy) và sửa quy ước dấu sao cho các hướng tương đương được băm giống hệt nhau. Điều này ngăn cản việc coi các hướng ngược nhau như các đường khác nhau. 
4. Đếm số lần xuất hiện của từng hướng chuẩn hóa. 

Hướng thường xuyên nhất tương ứng với đường đi qua mỏ neo chứa nhiều điểm nhất. 
5. Nếu tần số tốt nhất ít nhất là ⌊n/8⌋ − 1, hãy xây dựng lại đường truyền bằng cách sử dụng neo và một điểm từ hướng đó và xuất nó. 

Chúng tôi trừ đi một vì bản thân mỏ neo không có trong danh sách chỉ đường. 
6. Lặp lại nhiều lần neo ngẫu nhiên cho đến khi thành công. 

Vì một dòng hợp lệ chứa ít nhất n/8 điểm nên việc chọn một điểm từ nó thành công với xác suất ít nhất là 1/8 cho mỗi lần thử. 

### Tại sao nó hoạt động 

Xét một dòng L hợp lệ chứa k ≥ n/8 điểm. Mỗi điểm trên L đều có khả năng được chọn làm điểm neo như nhau. Với xác suất ít nhất k/n ≥ 1/8, mỏ neo nằm trên L. Khi điều đó xảy ra, tất cả k − 1 điểm còn lại trên L có chung hướng chuẩn hóa từ mỏ neo
