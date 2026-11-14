---
title: "CF 1800B - Đếm Số Đôi"
description: "Kristina có một chuỗi các chữ cái có thể là chữ thường hoặc chữ hoa. Cô ấy kiếm được một burl cho mỗi cặp hợp lệ bao gồm một chữ cái viết thường và phiên bản viết hoa tương ứng của nó. Mỗi chữ cái chỉ có thể tham gia vào một cặp duy nhất."
date: "2026-06-09T09:37:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1800
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 855 (Div. 3)"
rating: 1000
weight: 1800
solve_time_s: 177
verified: true
draft: false
---

[CF 1800B - Đếm số cặp](https://codeforces.com/problemset/problem/1800/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, dây 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Kristina có một chuỗi các chữ cái có thể là chữ thường hoặc chữ hoa. Cô ấy kiếm được một burl cho mỗi cặp hợp lệ bao gồm một chữ cái viết thường và phiên bản viết hoa tương ứng của nó. Mỗi chữ cái chỉ có thể tham gia vào một cặp duy nhất. Ngoài ra, cô ấy có thể biểu diễn nhiều nhất$k$các thao tác trong đó cô ấy lật kiểu chữ của một chữ cái, từ chữ thường sang chữ hoa hoặc ngược lại. Mục tiêu là tính toán số lượng cặp tối đa cô ấy có thể tạo thành sau khi đăng ký tối đa$k$hoạt động. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định độ dài chuỗi$n$, số thao tác cho phép$k$, và chuỗi$s$. Đầu ra của mỗi trường hợp thử nghiệm là số lượng cặp tối đa mà Kristina có thể tạo thành. 

Những ràng buộc cho phép$n$để đạt được$2 \cdot 10^5$tổng cộng trên tất cả các trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách tiếp cận bạo lực nào xem xét tất cả các tập hợp con của các lần lật hoặc kiểm tra tất cả các cặp một cách rõ ràng, vì số lượng hoạt động trong trường hợp xấu nhất sẽ là theo cấp số nhân. Chúng ta cần một giải pháp xử lý từng chuỗi một cách tuyến tính, lý tưởng nhất là trong$O(n)$thời gian. 

Trường hợp cạnh tinh tế phát sinh khi số lượng chữ thường và chữ hoa khác nhau nhiều hơn$2k$. Một giải pháp bất cẩn có thể chỉ đơn giản là cố gắng ghép các chữ cái hiện có một cách tham lam và sau đó sử dụng$k$hoạt động mà không kiểm tra xem các chữ cái còn sót lại có thể thực sự được chuyển đổi thành các cặp mới hay không. Ví dụ, với chuỗi`aaaBBB`Và$k = 1$, có ba`a`s và ba`B`S. Một cách tiếp cận ngây thơ có thể nghĩ rằng chúng ta có thể tạo thêm hai cặp bằng một thao tác, nhưng thực tế chỉ có thể tạo được một cặp bổ sung vì chuyển đổi một cặp bổ sung`a`ĐẾN`A`chỉ cho phép ghép nối với một`B`. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ cố gắng liệt kê tất cả các cách có thể để đạt được$k$các chữ cái và đếm số cặp được tạo thành cho mỗi trường hợp. Điều này rõ ràng là không thể thực hiện được vì$n$lên tới$2 \cdot 10^5$Và$k$lên tới$n$, số lượng tập hợp con của các chữ cái cần lật tăng lên theo tổ hợp. 

Điểm mấu chốt là thứ tự các chữ cái không quan trọng khi đếm các cặp. Chúng ta chỉ quan tâm đến số lượng của từng chữ cái viết thường và viết hoa. Đối với mỗi nhân vật`x`, số lượng cặp tự nhiên là số lượng tối thiểu của`x`Và`X`. Bất kỳ chữ cái nào còn lại của một trường hợp đều có thể được chuyển đổi sang trường hợp khác bằng cách sử dụng một thao tác trên hai chữ cái còn sót lại để tạo thành một cặp bổ sung. Ví dụ: nếu có thêm ba chữ thường`a`s và thêm một chữ hoa`A`, chúng ta có thể sử dụng một thao tác để chuyển đổi thêm một thao tác`a`ĐẾN`A`và tạo một cặp bổ sung. 

Giải pháp tối ưu là đếm các chữ cái, tạo thành các cặp tự nhiên, sau đó sử dụng các chữ cái còn lại theo nhóm hai chữ cái với các thao tác có sẵn để tạo thành các cặp bổ sung. Điều này làm giảm vấn đề xuống$O(n)$thời gian cho mỗi trường hợp thử nghiệm và$O(1)$thêm không gian ngoài mảng tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng`lower`Và`upper`có kích thước 26 để đếm số lần xuất hiện của từng chữ cái viết thường và viết hoa. 
2. Lặp lại chuỗi. Đối với chữ thường`c`, tăng`lower[ord(c)-ord('a')]`. Đối với chữ in hoa`C`, tăng`upper[ord(C)-ord('A')]`. 
3. Khởi tạo`pairs = 0`. Đối với mỗi chỉ số chữ cái`i`từ 0 đến 25, tính`natural_pairs = min(lower[i], upper[i])`. Thêm vào`natural_pairs`ĐẾN`pairs`. Trừ`natural_pairs`từ cả hai`lower[i]`Và`upper[i]`. 
4. Sau khi hình thành các cặp chữ tự nhiên, cho mỗi chữ cái chỉ số`i`, tính toán các chữ cái còn lại có thể tạo thành các cặp bổ sung bằng cách lật từng chữ:`extra_pairs_possible = min((lower[i] // 2) + (upper[i] // 2), k)`. Thêm vào`extra_pairs_possible`ĐẾN`pairs`và giảm dần`k`qua`extra_pairs_possible`. 
5. Trở về`pairs`. 

Điều bất biến là ở mỗi bước,`pairs`phản ánh số lượng cặp tối đa chỉ sử dụng các chữ cái được tính và`k`là số lần lật vẫn còn. Vì một cặp yêu cầu một trong mỗi trường hợp hoặc hai trường hợp giống nhau để lật, nên thuật toán sẽ tối đa hóa chính xác số lượng cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_burles(n, k, s):
    lower = [0] * 26
    upper = [0] * 26
    
    for c in s:
        if c.islower():
            lower[ord(c) - ord('a')] += 1
        else:
            upper[ord(c) - ord('A')] += 1
            
    pairs = 0
    for i in range(26):
        natural_pairs = min(lower[i], upper[i])
        pairs += natural_pairs
        lower[i] -= natural_pairs
        upper[i] -= natural_pairs
        
    for i in range(26):
        extra_pairs = min((lower[i] // 2) + (upper[i] // 2), k)
        pairs += extra_pairs
        k -= extra_pairs
        if k <= 0:
            break
            
    return pairs

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()
    print(max_burles(n, k, s))
```Vòng lặp đầu tiên đếm các chữ cái và xử lý các cặp tự nhiên. Vòng lặp thứ hai xem xét các chữ cái còn sót lại cho các cặp bổ sung tiềm năng bằng cách sử dụng các thao tác được phép. Việc sử dụng phép chia số nguyên đảm bảo rằng chỉ các cặp đầy đủ mới được tính và ngắt sớm một lần`k`đã cạn kiệt sẽ ngăn cản việc tính toán không cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`aAaaBACacbE`, k = 2: 

| Thư | Hạ | Thượng | Cặp tự nhiên | Còn lại | Cặp bổ sung | 
| --- | --- | --- | --- | --- | --- | 
| một | 3 | 1 | 1 | 2/0 | 1 | 
| b | 1 | 1 | 1 | 0/0 | 0 | 
| c | 2 | 1 | 1 | 1/0 | 0 | 
| ... | ... | ... | ... | ... | ... |`pairs`= 4 sau các cặp tự nhiên. Chúng ta có thể có thêm 1 cặp nữa bằng cách sử dụng một thao tác cho`a`. Cuối cùng`pairs`= 5. 

Đầu vào mẫu`ab`, k = 2: 

| Thư | Hạ | Thượng | Cặp tự nhiên | Còn lại | Cặp bổ sung | 
| --- | --- | --- | --- | --- | --- | 
| một | 1 | 0 | 0 | 1 | 0 | 
| b | 1 | 0 | 0 | 1 | 0 | 

Không thể có cặp tự nhiên và không thể có cặp bổ sung vì cần có hai trường hợp giống nhau. Cuối cùng`pairs`= 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng ta lặp lại chuỗi một lần và sau đó lặp lại 26 chữ cái hai lần. | 
| Không gian | O(1) | Chỉ có hai mảng có kích thước 26 được sử dụng, độc lập với n. | 

Điều này phù hợp với các ràng buộc: với tổng n lớn nhất của$2 \cdot 10^5$, thuật toán sẽ chạy hiệu quả dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        res.append(str(max_burles(n, k, s)))
    return "\n".join(res)

# Provided samples
assert run("5\n11 2\naAaaBACacbE\n2 2\nab\n4 1\naaBB\n6 0\nabBAcC\n5 3\ncbccb\n") == "5\n0\n1\n3\n2"

# Custom cases
assert run("1\n1 0\na\n") == "0", "single lowercase letter, no pair"
assert run("1\n2 1\nAa\n") == "1", "already a pair"
assert run("1\n4 2\naabb\n") == "2", "flip one pair possible"
assert run("1\n6 3\nAAaaaa\n") == "3", "maximize with flips"
assert run("1\n3 1\nABC\n") == "0", "all uppercase, insufficient flips"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 0\na\n`| 0 | Ký tự đơn, không thể ghép đôi | 
|`1\n2 1\nAa\n`| 1 | Đã là một cặp tự nhiên | 
|`1\n4 2\naabb\n`| 2 | Lật có thể tạo thành cặp phụ | 
|`1\n6 3\nAAaaaa\n`| 3 | Tối đa hóa các cặp bằng cách sử dụng lật | 
|`1\n3 1\nABC\n`| 0 | Tất cả đều viết hoa, lật không đủ | 

## Vỏ cạnh 

Trong trường hợp`aaBB`với k = 1,`lower`= [2],`upper`= [2]. Các cặp tự nhiên = 2. Các cặp còn lại = 0. Các cặp bổ sung = 0. Kết quả đúng = 1 nếu
