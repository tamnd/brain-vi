---
title: "CF 1715A - Thị trường chéo"
description: "Cửa hàng là một lưới n × m. Stanley bắt đầu ở góc trên bên trái và muốn đến góc dưới bên phải. Megan bắt đầu ở góc dưới bên trái và muốn đến góc trên bên phải. Di chuyển đến một ô lân cận tốn một đơn vị năng lượng."
date: "2026-06-09T19:54:10+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1715
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 816 (Div. 2)"
rating: 800
weight: 1715
solve_time_s: 58
verified: false
draft: false
---

[CF 1715A - Thị trường chéo](https://codeforces.com/problemset/problem/1715/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 58s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cửa hàng là một`n × m`lưới. Stanley bắt đầu ở góc trên bên trái và muốn đến góc dưới bên phải. Megan bắt đầu ở góc dưới bên trái và muốn đến góc trên bên phải. 

Di chuyển đến một ô lân cận tốn một đơn vị năng lượng. Megan để lại một cánh cổng trong mỗi phòng giam mà cô đến thăm. Từ bất kỳ ô nào chứa cổng, một trong hai người có thể tiêu tốn một đơn vị năng lượng để nhảy đến bất kỳ ô cổng nào khác. Hai người có thể tự do di chuyển theo bất kỳ thứ tự nào, vì bản thân thời gian là vô giá. Chúng ta phải tìm tổng năng lượng tối thiểu mà cả hai tiêu thụ cùng nhau. 

Mỗi trường hợp thử nghiệm chỉ chứa các kích thước của lưới. Câu trả lời là một con số đại diện cho năng lượng tổng hợp tối thiểu. 

Kích thước có thể lớn như`10^5`, và có thể có tới`1000`trường hợp thử nghiệm. Một giải pháp khám phá lưới hoặc chạy các thuật toán đồ thị sẽ quá đắt. Ngay cả một tìm kiếm theo chiều rộng đầu tiên trên một`100000 × 100000`lưới sẽ liên quan đến xung quanh`10^10`tế bào, điều này hoàn toàn không thể thực hiện được. Vì kích thước đầu vào rất nhỏ nên câu trả lời phải đến từ quan sát toán học trực tiếp và chạy trong thời gian không đổi cho mỗi trường hợp kiểm thử. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Khi lưới chỉ chứa một ô,```
1 1
```cả hai người đều đã đứng ở điểm đến của mình nên câu trả lời là```
0
```Một công thức trừ đi một cách mù quáng sẽ tạo ra một giá trị âm. 

Lưới một hàng cần được chăm sóc đặc biệt. Ví dụ,```
1 5
```Hai người chỉ cần đi dọc theo hàng. Cổng không mang lại lợi ích gì vì chỉ có một con đường. Câu trả lời là```
5
```Sử dụng công thức tổng quát mà không xét đến trường hợp này sẽ cho```
4
```điều đó là không chính xác. 

Vấn đề tương tự cũng xuất hiện đối với lưới một cột.```
5 1
```Câu trả lời là một lần nữa```
5
```vì mỗi người phải đi qua cột. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp nhất là xem lưới dưới dạng biểu đồ và tìm kiếm lịch trình di chuyển và dịch chuyển tối ưu. Vì các cổng xuất hiện linh hoạt và cả hai người có thể di chuyển theo thứ tự tùy ý nên không gian trạng thái trở nên rất lớn. Trạng thái sẽ cần mô tả vị trí của cả hai người và ô nào đã chứa cổng. Ngay cả đối với kích thước lưới vừa phải, số lượng khả năng sẽ bùng nổ. 

Cách tiếp cận bạo lực là đúng vì nó xem xét rõ ràng mọi chuỗi hành động có thể xảy ra, nhưng độ phức tạp của nó tăng theo cấp số nhân. Với lưới chứa tới`10^10`tế bào, cách tiếp cận như vậy là vô vọng. 

Quan sát quan trọng là chỉ có hình dạng của lưới mới quan trọng. Không có cổng thông tin, mỗi người cần```
(n - 1) + (m - 1)
```các bước, đưa ra tổng số```
2(n + m - 2).
```Cổng cho phép một người sử dụng lại một phần của tuyến đường đã được Megan khám phá. Mỗi lần chúng ta đổi hướng khi đi ngang qua hình chữ nhật, chúng ta thực sự sẽ mất đi một đơn vị so với việc di chuyển theo đường thẳng. Một hình chữ nhật yêu cầu hai hướng thay đổi, vì vậy các cổng lưu chính xác một đơn vị so với tổng số ban đầu. 

Do đó, đối với các lưới có ít nhất hai hàng và hai cột, câu trả lời sẽ trở thành```
2(n + m - 2) - max(n, m) + 1
```đơn giản hóa để```
2(min(n, m) - 1) + max(n, m).
```Một hình thức thậm chí còn sạch hơn là```
2(n + m - 2) - (min(n, m) - 1).
```Sau khi đơn giản hóa,```
answer = 2(n + m - 2) - (min(n, m) - 1)
       = n + m + max(n, m) - 2.
```Đối với một hàng hoặc một cột, cổng thông tin không giúp ích được gì và câu trả lời chỉ đơn giản là```

```
