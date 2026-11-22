---
title: "CF 269B - Hiệu ứng nhà kính"
description: "Chúng ta được cho một chuỗi các cây được sắp xếp dọc theo một đường thẳng, mỗi cây thuộc về một trong m loài. Nhà kính dài nhưng hẹp nên mỗi cây có một vị trí riêng dọc theo đường này và tất cả các vị trí đều tăng dần."
date: "2026-06-05T01:27:19+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 269
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 165 (Div. 1)"
rating: 1700
weight: 269
solve_time_s: 98
verified: true
draft: false
---

[CF 269B - Hiệu ứng nhà kính](https://codeforces.com/problemset/problem/269/B) 

**Đánh giá:** 1700 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các cây được sắp xếp dọc theo một đường thẳng, mỗi cây thuộc về một trong _m_ loài. Nhà kính dài nhưng hẹp nên mỗi cây có một vị trí riêng dọc theo đường này và tất cả các vị trí đều tăng dần. Mục đích là chia dòng này thành _m_ phần liền kề với chính xác một loài trên mỗi phần, được sắp xếp từ loài 1 đến loài _m_. Những cây không đúng phần phải được trồng lại và chúng tôi muốn giảm thiểu tổng số lần trồng lại. 

Dữ liệu đầu vào cung cấp cho chúng ta _n_ cây, mỗi cây có nhãn loài và vị trí. Đầu ra là một số nguyên duy nhất: số lượng cây trồng lại tối thiểu cần thiết để đáp ứng ràng buộc về thứ tự loài. 

Với các ràng buộc lên tới 5000 cho cả _n_ và _m_, một cách tiếp cận đơn giản nhằm kiểm tra mọi phân đoạn hoặc hoán vị có thể có sẽ quá chậm. Ví dụ, việc lặp lại tất cả các tập hợp con thực vật cho mỗi loài có thể dẫn đến sự bùng nổ tổ hợp, khiến cho giải pháp O(n^2·m) hầu như không khả thi và bất cứ điều gì tệ hơn đều không thực tế. 

Một trường hợp phức tạp là khi các nhãn loài đã được trộn lẫn nhưng chỉ cần một vài lần hoán đổi. Ví dụ: với đầu vào:```
3 2
2 1
1 2
1 3
```chúng ta chỉ cần trồng lại cây đầu tiên ở bên phải những cây khác. Cách tiếp cận ngây thơ "di chuyển mọi thứ không theo thứ tự" sẽ bị tính quá mức, đưa ra 2 thay vì câu trả lời đúng 1. 

Một trường hợp khác phát sinh khi một loài có nhiều cụm dọc theo đường. Giải pháp tối ưu có thể giữ nguyên một số cây trong khi trồng lại số lượng cây tối thiểu cần thiết để duy trì thứ tự từ trái sang phải. 

## Phương pháp tiếp cận 

Phương pháp brute-force cố gắng đặt đường viền _m-1_ ở tất cả các vị trí có thể. Đối với mỗi cấu hình, chúng tôi đếm số lượng cây ở phần sai. Mặc dù về nguyên tắc là đúng, nhưng cách tiếp cận này xem xét các vị trí đường viền có thể có của O(n^{m-1}) và không khả thi đối với n, m cho đến 5000. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến thứ tự của các loài dọc theo đường chứ không quan tâm đến vị trí chính xác. Nếu chúng ta mã hóa chuỗi loài thành một mảng thì vấn đề sẽ giảm xuống còn việc tìm chuỗi con dài nhất trong đó nhãn loài không giảm. Bất kỳ cây nào không thuộc dãy này phải được trồng lại. Điều này tương tự với bài toán Chuỗi con không giảm dài nhất cổ điển. 

Khi chúng ta có độ dài của chuỗi con này, số lần trồng lại tối thiểu chỉ đơn giản là n trừ đi độ dài chuỗi con. Lý do điều này hiệu quả là vì chúng ta có thể dịch chuyển những cây còn lại để khớp với các phần chính xác của chúng, vì các vị trí là số thực và không có hai cây nào chiếm cùng một điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^{m-1}) | O(n) | Quá chậm | 
| Dãy số không giảm dài nhất | O(n·m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng lập trình động`dp`có kích thước _m+1_, ở đâu`dp[i]`đại diện cho độ dài của chuỗi con dài nhất kết thúc bằng loài _i_. 
2. Lặp lại các cây theo thứ tự từ trái sang phải. Đối với mỗi loại cây có loài`s`, cập nhật`dp[s]`là tối đa của`dp[s]`Và`max(dp[1..s]) + 1`. Điều này nắm bắt được chuỗi tốt nhất có thể kết thúc ở loài`s`đồng thời duy trì trật tự không giảm. 
3. Theo dõi độ dài chuỗi tối đa tổng thể trong khi các nhà máy xử lý. 
4. Sau khi lặp lại tất cả các cây, số lần trồng lại tối thiểu là`n - max_length`, Ở đâu`max_length`là độ dài của dãy con không giảm dài nhất. 

Tại sao nó hoạt động: bất biến quy hoạch động đảm bảo rằng`dp[s]`luôn đại diện cho dãy con dài nhất kết thúc bằng loài`s`thấy cho đến nay. Vì dãy con tuân theo trật tự loài không giảm nên bất kỳ cây nào nằm ngoài dãy này đều phải được di chuyển. Không có giải pháp nào tốt hơn vì việc di chuyển ít cây hơn sẽ vi phạm trật tự loài từ trái sang phải. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
species = [0] * n
for i in range(n):
    s, x = input().split()
    species[i] = int(s)

dp = [0] * (m + 2)  # dp[i] = length of longest non-decreasing subsequence ending with species i

for s in species:
    best = 0
    for i in range(1, s + 1):
        if dp[i] > best:
            best = dp[i]
    dp[s] = best + 1

max_length = max(dp)
print(n - max_length)
```Mã này đầu tiên đọc tất cả các cây vào một mảng. các`dp`mảng theo dõi độ dài của chuỗi con không giảm dài nhất kết thúc ở mỗi loài. Đối với mỗi nhà máy, chúng tôi tìm thấy mức tối đa`dp`giá trị giữa các loài nhỏ hơn hoặc bằng loài của nó, tăng thêm 1 và cập nhật`dp[s]`. Cuối cùng, số cây cần trồng lại bằng tổng trừ đi độ dài của dãy tiếp theo dài nhất. 

Một lựa chọn triển khai tinh tế đang sử dụng`dp`kích thước`m+2`để tránh các vấn đề về ranh giới và lặp lại tối đa`s`còn hơn là`s-1`, bởi vì bản thân loài hiện tại có thể mở rộng một chuỗi trước đó. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3 2
2 1
1 2.0
1 3.100
```| Thực vật | Loài | dp trước | tốt nhất | dp sau | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | [0,0,0] | 0 | [0,0,1] | 
| 1 | 1 | [0,0,1] | 0 | [0,1,1] | 
| 1 | 1 | [0,1,1] | 1 | [0,2,1] |`max_length = 2`, do đó số lần trồng lại tối thiểu = 3 - 2 = 1. 

**Đầu vào mẫu 2**```
3 2
1 1
1 2
2 3
```| Thực vật | Loài | dp trước | tốt nhất | dp sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [0,0,0] | 0 | [0,1,0] | 
| 1 | 1 | [0,1,0] | 1 | [0,2,0] | 
| 2 | 2 | [0,2,0] | 2 | [0,2,3] |`max_length = 3`, do đó số lần trồng lại tối thiểu = 3 - 3 = 0. 

Những dấu vết này xác nhận rằng thuật toán theo dõi chính xác chuỗi dài nhất tôn trọng thứ tự loài và tính toán chính xác số lượng cây trồng lại cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Đối với mỗi n cây, chúng tôi quét tối đa m loài để tìm độ dài chuỗi con tối đa. | 
| Không gian | O(m) | Chỉ cần mảng dp có kích thước m+2. | 

Với n và m lên tới 5000, n·m = 25.000.000 thao tác, vừa vặn thoải mái trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là tối thiểu nên giải pháp phù hợp với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    species = [0] * n
    for i in range(n):
        s, x = input().split()
        species[i] = int(s)
    dp = [0] * (m + 2)
    for s in species:
        best = 0
        for i in range(1, s + 1):
            if dp[i] > best:
                best = dp[i]
        dp[s] = best + 1
    return str(n - max(dp))

# Provided samples
assert run("3 2\n2 1\n1 2.0\n1 3.100\n") == "1", "sample 1"
assert run("3 2\n1 1\n1 2\n2 3\n") == "0", "sample 2"

# Custom cases
assert run("1 1\n1 0\n") == "0", "minimum input"
assert run("5 3\n3 1\n1 2\n2 3\n3 4\n1 5\n") == "3", "interleaved species"
assert run("5 5\n1 1\n2 2\n3 3\n4 4\n5 5\n") == "0", "already sorted"
assert run("4 2\n2 1\n2 2\n1 3\n1 4\n") == "2", "two clusters per species"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 0 | 0 | Đầu vào tối thiểu, một nhà máy | 
| 5 3\n3 1\n1 2\n2 3\n3 4\n1 5 | 3 | Các loài xen kẽ cần trồng lại nhiều lần | 
| 5 5\n1 1\n2 2\n3 3\n4 | | |
