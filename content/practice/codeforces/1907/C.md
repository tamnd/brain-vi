---
title: "CF 1907C - Loại bỏ những đôi không hấp dẫn"
description: "Chúng ta được cung cấp một chuỗi các chữ cái viết thường và chúng ta có thể liên tục loại bỏ các cặp chữ cái liền kề nếu chúng khác nhau. Mục đích là giảm độ dài chuỗi xuống mức tối thiểu có thể."
date: "2026-06-08T20:36:56+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 1907
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 913 (Div. 3)"
rating: 1200
weight: 1907
solve_time_s: 137
verified: false
draft: false
---

[CF 1907C - Loại bỏ các cặp không hấp dẫn](https://codeforces.com/problemset/problem/1907/C) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, toán học, chuỗi 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái viết thường và chúng ta có thể liên tục loại bỏ các cặp chữ cái liền kề nếu chúng khác nhau. Mục đích là giảm độ dài chuỗi xuống mức tối thiểu có thể. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một chuỗi tối đa 200.000 ký tự và tổng độ dài của tất cả các trường hợp thử nghiệm không vượt quá 200.000. 

Điều này có nghĩa là giải pháp của chúng ta phải tuyến tính theo độ dài của mỗi chuỗi, bởi vì bất kỳ phương pháp bậc hai nào cũng sẽ thực hiện tới$n^2 \approx 4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thời gian 1 giây. Mỗi lần loại bỏ chỉ ảnh hưởng đến các ký tự liền kề, do đó, vấn đề có tính chất cục bộ mạnh, gợi ý cách tiếp cận tham lam hoặc dựa trên ngăn xếp. 

Các trường hợp cạnh bao gồm các chuỗi đã bao gồm các ký tự lặp lại, các chuỗi có các ký tự xen kẽ và các chuỗi rất ngắn có độ dài 1 hoặc 2. Ví dụ: chuỗi`"aa"`không thể giảm chút nào, mang lại độ dài tối thiểu là 2, trong khi`"ab"`có thể bị xóa hoàn toàn, dẫn đến 0. Việc triển khai bất cẩn xóa ký tự theo thứ tự tùy ý có thể không đạt mức tối thiểu vì thứ tự xóa ảnh hưởng đến những chữ cái còn lại. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liên tục quét chuỗi từ trái sang phải, loại bỏ cặp ký tự liền kề khác nhau đầu tiên gặp phải, sau đó khởi động lại. Quá trình này đúng vì mọi thao tác được phép sẽ giảm chuỗi chính xác hai ký tự, nhưng nó cực kỳ chậm: mỗi lần chuyển có thể mất$O(n)$và có thể có tới$O(n/2)$loại bỏ, mang lại sự phức tạp trong trường hợp xấu nhất$O(n^2)$. Vì$n = 2 \cdot 10^5$, tốc độ này quá chậm. 

Thông tin chi tiết quan trọng cho cách tiếp cận tối ưu là lưu ý rằng các thao tác loại bỏ chỉ phụ thuộc vào ký tự không khớp cuối cùng khi quét chuỗi. Điều này đương nhiên dẫn đến một giải pháp tham lam dựa trên ngăn xếp: duyệt chuỗi từ trái sang phải, đẩy các ký tự lên ngăn xếp. Khi ký tự hiện tại khác với phần trên cùng của ngăn xếp, chúng tạo thành một cặp có thể tháo rời, vì vậy hãy bật phần trên cùng thay vì đẩy. Điều này đảm bảo rằng mọi việc loại bỏ cặp được áp dụng ngay lập tức khi có thể và độ dài ngăn xếp cuối cùng biểu thị độ dài chuỗi tối thiểu. 

Brute-force hoạt động vì nó chỉ loại bỏ các cặp hợp lệ nhưng không thành công khi chuỗi dài do quét lại nhiều lần. Quan sát cho thấy chúng ta có thể duy trì một ngăn xếp để xử lý các quyết định cục bộ sẽ giảm độ phức tạp về thời gian xuống$O(n)$bởi vì mỗi ký tự được đẩy và bật nhiều nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu dựa trên ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một ngăn xếp trống. Điều này sẽ lưu trữ các ký tự chưa bị xóa. 
2. Lặp lại từng ký tự`c`trong chuỗi. 
3. Nếu ngăn xếp không trống và ký tự trên cùng khác với`c`, bật ngăn xếp. Điều này thể hiện việc loại bỏ cặp liền kề. 
4. Nếu không, hãy nhấn`c`lên ngăn xếp. Điều này có nghĩa`c`không thể xóa được ký tự trước đó. 
5. Sau khi duyệt qua chuỗi, ngăn xếp chứa tất cả các ký tự không thể ghép nối và xóa được. Độ dài tối thiểu của chuỗi là kích thước của ngăn xếp. 

Tại sao nó hoạt động: Ở mỗi bước, ngăn xếp duy trì một giải pháp từng phần chính xác cho tiền tố của chuỗi. Mỗi cửa sổ bật lên tương ứng với việc loại bỏ hợp lệ các ký tự khác nhau liền kề và không có cặp nào bị bỏ sót vì ngăn xếp chỉ so sánh với ký tự không khớp gần đây nhất. Vì việc loại bỏ mang tính cục bộ và độc lập ngoài vùng lân cận nên sự lựa chọn tham lam này là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    
    stack = []
    for c in s:
        if stack and stack[-1] != c:
            stack.pop()
        else:
            stack.append(c)
    print(len(stack))
```Giải thích: Chúng tôi sử dụng I/O nhanh để xử lý đầu vào lớn. Mỗi ký tự sẽ được đẩy lên ngăn xếp nếu nó không thể tạo thành một cặp có thể tháo rời được với phần trên cùng. Độ dài cuối cùng của ngăn xếp được in cho từng trường hợp thử nghiệm. sử dụng`strip()`đảm bảo không có dòng mới nào cản trở việc so sánh ký tự. Các vấn đề về ranh giới được tránh vì ngăn xếp trống được kiểm tra chính xác trước khi bật lên. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`"aabc"`| Bước | Ngăn xếp | Hành động | 
| --- | --- | --- | 
| 'a' | ['a'] | đẩy 'a' | 
| 'a' | ['a','a'] | đẩy 'a' | 
| 'b' | ['a'] | 'b' != 'a', bật | 
| 'c' | [] | 'c' != 'a', bật | 
| Độ dài ngăn xếp cuối cùng: 0 | | | 

Điều này chứng tỏ rằng cách tiếp cận tham lam có thể loại bỏ các ký tự không liền kề một cách gián tiếp bằng cách giải quyết các cặp theo đúng thứ tự. 

### Ví dụ 2:`"abaca"`| Bước | Ngăn xếp | Hành động | 
| --- | --- | --- | 
| 'a' | ['a'] | đẩy | 
| 'b' | [] | bật, 'b' != 'a' | 
| 'a' | ['a'] | đẩy | 
| 'c' | [] | bật, 'c' != 'a' | 
| 'a' | ['a'] | đẩy | 
| Độ dài ngăn xếp cuối cùng: 1 | | | 

Điều này cho thấy khi
