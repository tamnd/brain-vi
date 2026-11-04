---
title: "CF 1154B - Làm cho họ bình đẳng"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Chúng ta phải chọn một giá trị D không âm và sau đó, độc lập với từng phần tử, cộng D, trừ D hoặc giữ nguyên. Mục tiêu là làm cho mọi phần tử trở thành cùng một giá trị sau các thao tác này."
date: "2026-06-12T02:46:46+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1154
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 552 (Div. 3)"
rating: 1200
weight: 1154
solve_time_s: 43
verified: false
draft: false
---

[CF 1154B - Làm cho họ bình đẳng](https://codeforces.com/problemset/problem/1154/B) 

**Đánh giá:** 1200 
**Thẻ:** toán 
**Thời gian giải:** 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Chúng ta phải chọn một giá trị không âm`D`và sau đó, độc lập với từng phần tử, hãy thêm`D`, trừ`D`, hoặc để nguyên không thay đổi. 

Mục tiêu là làm cho mọi phần tử trở thành cùng một giá trị sau các thao tác này. Trong số tất cả các lựa chọn hợp lệ, chúng ta cần lựa chọn nhỏ nhất có thể`D`. Nếu không như vậy`D`tồn tại, chúng tôi in`-1`. 

Hạn chế chính là giống nhau`D`phải được sử dụng cho mọi phần tử. Chúng ta có thể tự do lựa chọn các thao tác khác nhau cho các phần tử khác nhau, nhưng mức độ thay đổi là cố định. 

Những hạn chế là rất nhỏ. Mảng chứa tối đa 100 phần tử và mọi giá trị tối đa là 100. Ngay cả những cách tiếp cận khá bạo lực cũng sẽ phù hợp một cách thoải mái trong giới hạn thời gian. Thách thức không phải là tính hiệu quả mà là việc nhận ra cấu trúc toán học quyết định liệu giải pháp có tồn tại hay không. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một mảng trong đó tất cả các giá trị đều bằng nhau:```
3
5 5 5
```Câu trả lời đúng là:```
0
```Không cần thay đổi, vì vậy giá trị tối thiểu hợp lệ`D`là số không. Một giải pháp luôn tính toán sự khác biệt giữa các giá trị riêng biệt có thể trả về giá trị dương không chính xác. 

Bây giờ hãy xem xét một mảng có chính xác hai giá trị riêng biệt:```
2
2 8
```Câu trả lời đúng là:```
3
```Chúng ta có thể chuyển cả hai số thành 5 bằng cách cộng và trừ 3. Một cách tiếp cận bất cẩn có thể trả về toàn bộ chênh lệch 6 thay vì kích thước phép toán được yêu cầu là 3. 

Một trường hợp quan trọng khác là:```
3
1 4 8
```Câu trả lời đúng là:```
-1
```Không có đơn`D`cho phép cả ba số đạt đến một giá trị chung. Chỉ nhìn vào sự khác biệt theo cặp là không đủ. 

Cuối cùng, hãy xem xét:```

```Câu trả lời là:```

```Giá trị ở giữa 4 đã bằng mục tiêu. Giá trị 1 và 7 di chuyển chính xác bằng 3. Điều này minh họa rằng một số phần tử có thể không thay đổi trong khi các phần tử khác được sửa đổi. 

## Phương pháp tiếp cận 

Một giải pháp brute-force có thể thử mọi giá trị có thể của`D`và kiểm tra xem tất cả các số có thể được chuyển đổi thành một mục tiêu chung hay không. 

Vì các giá trị mảng tối đa là 100, nên bất kỳ giá trị có ý nghĩa nào`D`cũng phải nhỏ. Đối với mỗi ứng viên`D`, chúng tôi có thể tạo tất cả các giá trị có thể truy cập cho mọi phần tử và kiểm tra xem liệu có tồn tại một giá trị chung hay không. Điều này hoạt động vì không gian tìm kiếm rất nhỏ. 

Hạn chế là nó hoàn toàn bỏ qua cấu trúc toán học của bài toán. Mặc dù nó có thể vượt qua những hạn chế này nhưng nó không giải thích được tại sao lại có một giải pháp. 

Quan sát quan trọng là mọi phần tử chỉ có thể kết thúc ở một trong ba vị trí:```
a[i] - D
a[i]
a[i] + D
```Giả sử giá trị chung cuối cùng là`x`. 

Khi đó mọi số ban đầu phải là một trong:```
x - D
x
x + D
```Điều đó có nghĩa là toàn bộ mảng có thể chứa tối đa ba giá trị riêng biệt. Nếu có bốn số khác nhau trở lên thì không thể giải ngay được. 

Hãy để các giá trị riêng biệt được sắp xếp. 

Nếu chỉ có một giá trị riêng biệt thì câu trả lời hiển nhiên là`0`. 

Nếu có hai giá trị riêng biệt, hãy gọi chúng`a < b`. 

Khi`b - a`chẵn thì ta có thể chọn```
D = (b - a) / 2
```và di chuyển cả hai
