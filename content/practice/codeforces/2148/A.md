---
title: "CF 2148A - Chuỗi siêu phàm"
description: "Bài toán cho chúng ta một số x và độ dài n và yêu cầu chúng ta xây dựng một chuỗi bằng cách xen kẽ x và -x, bắt đầu bằng x. Sau đó chúng tôi được yêu cầu tính tổng của tất cả các phần tử trong chuỗi đó."
date: "2026-06-08T01:13:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "hashing", "math"]
categories: ["algorithms"]
codeforces_contest: 2148
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1050 (Div. 4)"
rating: 800
weight: 2148
solve_time_s: 67
verified: true
draft: false
---

[CF 2148A - Trình tự siêu phàm](https://codeforces.com/problemset/problem/2148/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, băm, toán học 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán cho chúng ta một con số`x`và một chiều dài`n`và yêu cầu chúng tôi xây dựng một chuỗi bằng cách xen kẽ`x`Và`-x`, bắt đầu bằng`x`. Sau đó chúng tôi được yêu cầu tính tổng của tất cả các phần tử trong chuỗi đó. Về mặt khái niệm, điều này giống như việc lật dấu của`x`từng bước và cộng các giá trị lại với nhau. 

Ví dụ, nếu`x = 2`Và`n = 5`, trình tự là`2, -2, 2, -2, 2`. Tổng hợp những điều này mang lại`2`. Nếu như`n = 4`, trình tự`2, -2, 2, -2`tổng cộng`0`. 

Các ràng buộc rất nhỏ: cả hai`x`Và`n`tối đa là 10 và có thể có tới 100 trường hợp thử nghiệm. Điều này có nghĩa là ngay cả một mô phỏng trực tiếp xây dựng trình tự một cách rõ ràng cũng sẽ chạy nhanh chóng. Tuy nhiên, bài toán cũng có thể được giải bằng toán học mà không cần xây dựng trình tự, điều này giúp đơn giản hóa lời giải và giảm nguy cơ xảy ra từng lỗi một. 

Một trường hợp cạnh tinh tế phát sinh khi`n`thật kỳ quặc. Trình tự sau đó kết thúc bằng`x`, để lại một tổng khác 0. Nếu như`n`là số chẵn thì dãy có số bằng nhau`x`Và`-x`, dẫn đến tổng bằng 0. Việc triển khai bất cẩn luôn tính tổng các cặp hoặc bỏ qua phần tử cuối cùng có thể tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi trường hợp thử nghiệm, hãy xây dựng trình tự một cách rõ ràng và tính tổng tất cả các phần tử. Điều này hoạt động vì`n`nhiều nhất là 10. Trong Python, điều này liên quan đến việc lặp lại`n`lần và luân phiên thêm`x`Và`-x`đến tổng số đang chạy. Tổng số thao tác trong trường hợp xấu nhất là 100 trường hợp thử nghiệm × 10 phần tử trên mỗi chuỗi = 1000 lần bổ sung, con số này không đáng kể. 

Cách tiếp cận tối ưu quan sát một mô hình. Mỗi cặp liên tiếp`(x, -x)`tổng bằng không. Vì vậy, nếu`n`là số chẵn, tổng sẽ tự động bằng 0. Nếu như`n`là số lẻ, tổng bằng`x`bởi vì tất cả các cặp đều bị hủy và phần bổ sung`x`cuối cùng vẫn còn. Cái nhìn sâu sắc này làm giảm vấn đề thành một kiểm tra đơn giản về tính chẵn lẻ của`n`và có thể bổ sung thêm`x`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × t) | O(n) | Được chấp nhận do những hạn chế nhỏ | 
| Tối ưu | O(t) | O(1) | Được chấp nhận, đơn giản và an toàn nhất | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Điều này xác định có bao nhiêu trình tự chúng ta cần xử lý. 
2. Với mỗi test case, hãy đọc`x`Và`n`. 
3. Kiểm tra xem`n`là chẵn. Nếu đúng như vậy thì tổng bằng 0 vì tất cả các cặp`(x, -x)`hủy nhau. 
4. Nếu`n`là số lẻ, tổng bằng`x`bởi vì tất cả các cặp đều bị hủy và phần bổ sung`x`cuối cùng đóng góp vào tổng số. 
5. In kết quả cho từng test. 

Tại sao nó hoạt động: Thuật toán dựa vào tính bất biến mà mọi cặp phần tử liên tiếp`(x, -x)`tổng bằng không. Bằng cách ghép nối các phần tử theo cách này, vấn đề sẽ giảm xuống còn việc kiểm tra xem một phần tử chưa được ghép nối`x`vẫn còn, điều này xảy ra chính xác khi`n`thật kỳ quặc. Không có phần tử chuỗi nào được tính hai lần hoặc bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, n = map(int, input().split())
    if n % 2 == 0:
        print(0)
    else:
        print(x)
```Dòng đầu tiên ghi số lượng test case. Đối với mỗi trường hợp thử nghiệm,`x`Và`n`được đọc. Séc`n % 2 == 0`xác định xem độ dài chuỗi là chẵn hay lẻ. Điều kiện này trực tiếp triển khai logic rút ra trong hướng dẫn thuật toán. Không có vòng lặp nào trên các phần tử chuỗi vì chúng tôi tận dụng thuộc tính chẵn lẻ. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** Đầu vào`x = 1, n = 4`| Bước | n % 2 | Tổng hợp | 
| --- | --- | --- | 
| Kiểm tra tính chẵn lẻ | 4 % 2 = 0 | 0 | 

Trình tự`1, -1, 1, -1`tổng bằng không. Thuật toán của chúng tôi xuất ra chính xác`0`. 

**Ví dụ 2:** Đầu vào`x = 2, n = 5`| Bước | n % 2 | Tổng hợp | 
| --- | --- | --- | 
| Kiểm tra tính chẵn lẻ | 5% 2 = 1 | 2 | 

Trình tự`2, -2, 2, -2, 2`có bốn phần tử bị hủy và một phần tử còn lại`2`. Đầu ra thuật toán của chúng tôi`2`như mong đợi. 

Những dấu vết này xác nhận rằng cách tiếp cận dựa trên tính chẵn lẻ xử lý chính xác cả độ dài chuỗi chẵn và lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu thao tác in và kiểm tra tính chẵn lẻ theo thời gian liên tục. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng ngoài các biến vô hướng. | 

Được cho`t ≤ 100`Và`n ≤ 10`, giải pháp này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    t = int(input())
    for _ in range(t):
        x, n = map(int, input().split())
        print(0 if n % 2 == 0 else x)
    
    return out.getvalue().strip()

# Provided samples
assert run("4\n1 4\n2 5\n3 6\n4 7\n") == "0\n2\n0\n4", "sample 1"

# Custom cases
assert run("3\n1 1\n5 10\n7 3\n") == "1\n0\n7", "min and max n"
assert run("2\n10 2\n10 9\n") == "0\n10", "even and odd large x"
assert run("1\n5 5\n") == "5", "odd n simple"
assert run("1\n3 6\n") == "0", "even n simple"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, 5 10, 7 3 | 1, 0, 7 | Xử lý n tối thiểu, n tối đa, xử lý n lẻ | 
| 10 2, 10 9 | 0, 10 | Thậm chí n kết quả là 0, n lẻ kết quả x | 
| 5 5 | 5 | Xử lý đúng số lẻ n tầm trung | 
| 3 6 | 0 | Xử lý đúng ngay cả n tầm trung | 

## Vỏ cạnh 

Đối với chuỗi nhỏ nhất`n = 1`, nhập`x = 1`tạo ra trình tự`[1]`. Thuật toán của chúng tôi thấy rằng`n % 2 == 1`và đầu ra`1`, khớp với tổng. Đối với dãy lớn nhất`n = 10`, nhập`x = 10`sản xuất`[10, -10, 10, -10, 10, -10, 10, -10, 10, -10]`. Thuật toán của chúng tôi thấy`n % 2 == 0`và đầu ra`0`, điều đó đúng. Điều này chứng tỏ thuật toán xử lý cả hai ranh giới mà không lặp qua chuỗi.
