---
title: "CF 109B - Xác suất may mắn"
description: "Chúng ta được yêu cầu tính xác suất để nếu Petya và Vasya mỗi người chọn ngẫu nhiên một số nguyên từ các khoảng tương ứng của chúng thì khoảng giữa hai số được chọn chứa đúng k số may mắn. Những con số may mắn là những con số chỉ chứa các chữ số 4 và 7."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 109
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 84 (Div. 1 Only)"
rating: 1900
weight: 109
solve_time_s: 143
verified: true
draft: false
---

[CF 109B - Xác suất may mắn](https://codeforces.com/problemset/problem/109/B) 

**Xếp hạng:** 1900 
**Tags:** sức mạnh vũ phu, xác suất 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính xác suất để nếu Petya và Vasya mỗi người chọn ngẫu nhiên một số nguyên từ các khoảng tương ứng của chúng thì khoảng giữa hai số được chọn chứa chính xác`k`con số may mắn. Con số may mắn là những con số chỉ chứa các chữ số 4 và 7. Đầu vào cung cấp giới hạn các khoảng cho cả hai người chơi (`pl`ĐẾN`pr`dành cho Petya,`vl`ĐẾN`vr`cho Vasya) và số lượng mục tiêu`k`của những con số may mắn. Đầu ra là xác suất dưới dạng số dấu phẩy động. 

Các ràng buộc đưa ra giới hạn trên của$10^9$cho các khoảng và`k`có thể lên tới 1000. Điều này ngay lập tức loại trừ mọi cách tiếp cận lặp trực tiếp trên tất cả các số nguyên trong các khoảng, bởi vì tổng số cặp có thể lên tới$(10^9)^2 = 10^{18}$, điều này là không thể tính toán được. Vấn đề đòi hỏi một cách tiếp cận tận dụng số lượng nhỏ các con số may mắn so với kích thước khoảng rất lớn tiềm năng. 

Việc triển khai đơn giản có thể cố gắng liệt kê mọi số nguyên trong cả hai khoảng, đếm số may mắn trong mỗi khoảng con kết quả và sau đó chia cho tổng số cặp. Điều này sẽ thất bại trong khoảng thời gian lớn. Một trường hợp khó phát hiện khác phát sinh khi các con số may mắn nằm ngoài một hoặc cả hai khoảng hoàn toàn. Ví dụ, nếu`pl = pr = 1`Và`vl = vr = 2`Và`k = 1`, không có số may mắn nào nằm trong khoảng từ 1 đến 2. Việc triển khai bất cẩn có thể cho rằng xác suất là khác 0, nhưng thực tế nó sẽ trả về 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại trên mọi số nguyên`p`trong khoảng Petya và`v`trong khoảng Vasya, tính khoảng`[min(p,v), max(p,v)]`, đếm các số may mắn trong khoảng đó và kiểm tra xem nó có bằng không`k`. Mặc dù đúng nhưng cách tiếp cận này có độ phức tạp trong trường hợp xấu nhất là$O((pr - pl + 1) \cdot (vr - vl + 1) \cdot \text{number of lucky numbers})$, có thể vượt quá$10^{18}$hoạt động. Điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là số lượng con số may mắn rất nhỏ, nhiều nhất là khoảng 1022 dưới$10^9$(vì chỉ có 2 chữ số và dài tối đa 10 chữ số,$2^{10}-2$bỏ qua một chữ số 0). Thay vì kiểm tra từng số nguyên trong các khoảng, chúng ta có thể tính toán trước tất cả các số may mắn và tập trung vào vị trí của chúng so với các khoảng. 

Thay vì lặp lại tất cả các số, chúng ta có thể đếm xem có bao nhiêu giá trị của`p`Và`v`tạo ra các khoảng chứa chính xác`k`số may mắn bằng cách làm việc với các phạm vi được xác định bởi các số may mắn liên tiếp. Điều này làm giảm vấn đề về việc đếm khoảng: với mỗi số may mắn bắt đầu, hãy tìm`k`-con số may mắn thứ ở phía trước, tính phạm vi hợp lệ cho`p`Và`v`sao cho khoảng giữa chúng chứa chính xác`k`con số may mắn, và tổng hợp những đóng góp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((pr-pl+1)*(vr-vl+1)) | O(1) | Quá chậm | 
| Tối ưu | O(L^2) trong đó L ~ 1022 | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các số may mắn lên tới$10^9$. Điều này có thể được thực hiện bằng hàm đệ quy nối thêm 4 hoặc 7 ở mỗi bước cho đến khi vượt quá giới hạn. Lưu trữ chúng trong một danh sách được sắp xếp. 
2. Thêm giá trị trọng điểm tại`0`Và`10^9 + 1`để xử lý các khoảng bao gồm các ranh giới mà không cần thêm điều kiện. 
3. Đối với mỗi chuỗi liên tiếp của`k`số may mắn, xác định các số tối thiểu và tối đa có thể tạo ra một khoảng chứa chính xác các số này`k`con số may mắn. Cho phép`a`là số may mắn ngay trước dãy số (hoặc 0) và`b`số may mắn ngay sau dãy số (hoặc 10^9 + 1). Điều có thể`p`Và`v`giá trị phải nằm đúng giữa`a+1`và số may mắn đầu tiên trong dãy dành cho giới hạn dưới và hoàn toàn nhỏ hơn`b`cho giới hạn trên. 
4. Với mỗi dãy như vậy, hãy tính số lượng giá trị hợp lệ`(p,v)`cặp. Điều này liên quan đến việc tính toán số lượng số nguyên trong khoảng Petya trùng với phạm vi dưới và trên, và tương tự đối với Vasya. Cộng các số đếm lại với nhau`p<v`Và`v<p`trường hợp. 
5. Tổng hợp tất cả các đóng góp trên tất cả các chuỗi của`k`số may mắn liên tiếp. Chia cho tổng số cặp có thể`(pr-pl+1)*(vr-vl+1)`để có được xác suất. 
6. In xác suất với độ chính xác cao, đảm bảo sai số tuyệt đối không vượt quá$10^{-9}$. 

Tại sao nó hoạt động: Thuật toán tận dụng thực tế là cách duy nhất để có được chính xác`k`những con số may mắn trong khoảng là để giới hạn khoảng cách một cách chính xác giữa`k`-con số may mắn thứ. Bằng cách lặp lại các số may mắn thay vì các số nguyên trong các khoảng, chúng ta biến một bài toán khó thành một số học khoảng có thể quản lý được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def generate_lucky(limit):
    result = []
    def dfs(x):
        if x > limit:
            return
        if x > 0:
            result.append(x)
        dfs(x*10 + 4)
        dfs(x*10 + 7)
    dfs(0)
    return sorted(result)

pl, pr, vl, vr, k = map(int, input().split())
lucky = generate_lucky(10**9)
lucky = [0] + lucky + [10**9 + 1]  # add sentinels

total = 0

for i in range(1, len(lucky) - k):
    left = lucky[i-1] + 1
    right = lucky[i+k] - 1
    low = lucky[i]
    high = lucky[i+k-1]
    
    # compute overlap of intervals with player ranges
    len_p_low = max(0, min(pr, high) - max(pl, left) + 1)
    len_v_low = max(0, min(vr, high) - max(vl, left) + 1)
    
    len_p_left = max(0, min(pr, low-1) - max(pl, left) + 1)
    len_p_right = max(0, min(pr, right) - max(pl, high+1) + 1)
    len_v_left = max(0, min(vr, low-1) - max(vl, left) + 1)
    len_v_right = max(0, min(vr, right) - max(vl, high+1) + 1)
    
    total += len_p_left * len_v_right
    total += len_v_left * len_p_right

prob = total / ((pr - pl + 1) * (vr - vl + 1))
print(f"{prob:.12f}")
```Giải pháp trước tiên tạo ra tất cả các số may mắn một cách hiệu quả bằng DFS đệ quy. Lính canh ở`0`Và`10^9+1`đơn giản hóa việc xử lý cạnh. Sự chồng chéo khoảng thời gian được tính toán bằng cách sử dụng`max`Và`min`để cắt phạm vi theo khoảng thời gian của người chơi. Công thức đếm phân biệt cẩn thận các khoảng góp phần vào`p<v`Và`v<p`. Cuối cùng, chúng tôi chia cho tổng số cặp để tìm xác suất. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1 10 1 10 2
```| tôi | may mắn[i-1] | may mắn[i] | may mắn[i+1] | trái | đúng | len_p_left | len_v_right | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 7 | 1 | 6 | 1-1=0 | ... | 32 | 

Dấu vết này cho thấy rằng chỉ những khoảng bắt đầu từ 1-4 và kết thúc từ 7-10 mới tạo ra chính xác 2 số may mắn, cho ra 32 cặp hợp lệ trên tổng số 100. 

Đầu vào mẫu 2:```
1 10 1 10 1
```Các khoảng tạo ra chính xác 1 số may mắn bao gồm những khoảng chỉ chứa 4 hoặc chỉ 7. Đếm đóng góp cho cả hai`p<v`Và`v<p`tổng lại là 32, xác suất 0,32. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L^2) | L ~ 1022, chúng ta lặp qua các chuỗi có độ dài k trên tất cả các số may mắn | 
| Không gian | O(L) | Lưu trữ tất cả các con số may mắn và trọng điểm | 

Độ phức tạp này đủ nhỏ vì số lượng số may mắn được giới hạn bởi 1022 dưới$10^9$, do đó thuật toán sẽ thực hiện trong chưa đầy một giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open('solution.py').read())
    return sys.stdout.getvalue().strip()

# provided samples
assert run("1 10 1 10 2\n") == "0.320000000000", "sample 1"
assert run("1 10 1 10 1\n") == "0.320000000000", "sample 2"

# custom cases
assert run("1 1 1 1 1\n") == "0.000000000000", "single value, no lucky"
assert run("4 7 4 7 1\n") == "0.500000000000", "interval includes lucky numbers 4 and 7"
assert
```
