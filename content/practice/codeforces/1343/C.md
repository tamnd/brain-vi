---
title: "CF 1343C - Chuỗi con xen kẽ"
description: "Chúng ta được cho một dãy số nguyên, cả dương và âm, và cần xây dựng một dãy con có các phần tử thay phiên nhau đúng dấu. Trong số tất cả các dãy con đạt được độ dài tối đa có thể, chúng ta được yêu cầu tìm dãy con có tổng lớn nhất."
date: "2026-06-11T15:09:21+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1343
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 636 (Div. 3)"
rating: 1200
weight: 1343
solve_time_s: 79
verified: true
draft: false
---

[CF 1343C - Chuỗi con xen kẽ](https://codeforces.com/problemset/problem/1343/C) 

**Đánh giá:** 1200 
**Tags:** dp, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên, cả dương và âm, và cần xây dựng một dãy con có các phần tử thay phiên nhau đúng dấu. Trong số tất cả các dãy con đạt được độ dài tối đa có thể, chúng ta được yêu cầu tìm dãy con có tổng lớn nhất. Đầu ra cho mỗi trường hợp thử nghiệm là tổng đó. 

Mỗi trường hợp thử nghiệm cung cấp số phần tử theo sau chính chuỗi đó. Các ràng buộc rất đáng kể: tổng của tất cả độ dài chuỗi trong các trường hợp thử nghiệm không vượt quá 200.000, do đó, mọi giải pháp đều phải chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. Các giải pháp bậc hai, chẳng hạn như kiểm tra mọi dãy con có thể, là không khả thi. 

Trường hợp cạnh tinh tế phát sinh khi các phần tử liên tiếp có cùng dấu. Ví dụ: nếu đầu vào là`[1, 2, 3, -1, -2]`, dãy con xen kẽ tối đa sẽ chọn giá trị dương lớn nhất trước khi chuyển sang giá trị âm, sau đó là giá trị âm lớn nhất trước khi chuyển đổi lại. Đơn giản chỉ cần xen kẽ mọi phần tử sẽ không thể tối đa hóa tổng. Một trường hợp phức tạp khác xảy ra khi tất cả các phần tử có cùng dấu, chẳng hạn như`[-1, -2, -3]`, trong đó dãy con xen kẽ dài nhất có độ dài bằng 1 và câu trả lời đúng là phần tử lớn nhất`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tạo ra tất cả các chuỗi con có thể có, kiểm tra dấu thay thế nào và chọn chuỗi có độ dài lớn nhất và sau đó là tổng tối đa. Điều này đúng nhưng hoàn toàn không thực tế: việc tạo ra các chuỗi con là O(2^n) và n có thể lên tới 2 × 10^5. 

Quan sát quan trọng tạo nên một giải pháp hiệu quả là trong một đoạn liền kề của các số có cùng dấu, chúng ta chỉ muốn số lớn nhất. Việc bao gồm các số nhỏ hơn sẽ làm giảm tổng mà không kéo dài độ dài của dãy con xen kẽ. Do đó, chuỗi có thể được nén thành các "khối" gồm các số liên tiếp có cùng dấu và với mỗi khối chúng ta chọn phần tử lớn nhất. Sau khi được nén, chỉ cần tính tổng các phần tử tối đa này theo thứ tự xen kẽ sẽ đạt được giải pháp tối ưu. 

Do đó, cách tiếp cận tối ưu hoạt động theo thời gian tuyến tính bằng cách lặp qua chuỗi, duy trì mức tối đa của khối dấu hiện tại và thêm nó vào tổng bất cứ khi nào xảy ra thay đổi dấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`sum_total`về không. Điều này sẽ lưu trữ tổng của chuỗi xen kẽ đã chọn. 
2. Đặt`current_max`tới phần tử đầu tiên của dãy. Điều này theo dõi phần tử lớn nhất trong khối ký hiệu hiện tại. 
3. Lặp lại chuỗi từ phần tử thứ hai. Đối với mỗi phần tử: 

1. Nếu dấu của nó trùng khớp`current_max`, cập nhật`current_max`đến cái lớn hơn trong hai cái. Điều này đảm bảo chúng tôi chỉ giữ số lớn nhất trong khối ký hiệu hiện tại. 
2. Nếu dấu của nó khác thì thêm`current_max`ĐẾN`sum_total`và thiết lập`current_max`tới phần tử mới này, bắt đầu một khối ký hiệu mới. 
4. Sau khi lặp lại, thêm phần cuối cùng`current_max`ĐẾN`sum_total`. Điều này xử lý khối cuối cùng. 
5. In`sum_total`. 

Tại sao nó hoạt động: điều bất biến là tại bất kỳ thời điểm nào,`current_max`giữ phần tử tối đa của khối dấu liên tiếp hiện tại. Vì các chuỗi con xen kẽ yêu cầu đổi dấu cho từng phần tử nên việc chọn phần tử lớn nhất trong mỗi khối sẽ tối đa hóa tổng mà không làm giảm độ dài chuỗi con. Điều này đảm bảo rằng kết quả là tổng lớn nhất của dãy con xen kẽ có độ dài tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    sum_total = 0
    current_max = a[0]
    
    for i in range(1, n):
        if (a[i] > 0) == (current_max > 0):
            current_max = max(current_max, a[i])
        else:
            sum_total += current_max
            current_max = a[i]
    
    sum_total += current_max
    print(sum_total)
```Mỗi phần theo trực tiếp từ thuật toán.`current_max`giữ giá trị lớn nhất của khối dấu hiện tại. Sự so sánh`(a[i] > 0) == (current_max > 0)`kiểm tra xem phần tử hiện tại có cùng dấu với khối trước đó hay không. Thêm`current_max`ở cuối đảm bảo khối cuối cùng được tính. Điều này tránh được các lỗi riêng lẻ và hoạt động đối với các chuỗi hoàn toàn tích cực hoặc tiêu cực. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
1 2 3 -1 -2
```| tôi | một [tôi] | hiện tại_max |_
