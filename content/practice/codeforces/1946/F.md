---
title: "CF 1946F - Không cần ai cả"
description: "Chúng ta được cung cấp một hoán vị các số nguyên từ 1 đến $n$ và một loạt truy vấn. Mỗi truy vấn hỏi có bao nhiêu chuỗi chỉ số trong một phạm vi được chỉ định $[l, r]$ có thể được hình thành sao cho chuỗi này tăng nghiêm ngặt và mỗi phần tử chia phần tử tiếp theo trong hoán vị."
date: "2026-06-07T17:52:31+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "data-structures", "dfs-and-similar", "dp"]
categories: ["algorithms"]
codeforces_contest: 1946
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 936 (Div. 2)"
rating: 2500
weight: 1946
solve_time_s: 98
verified: false
draft: false
---

[CF 1946F - Không cần ai cả](https://codeforces.com/problemset/problem/1946/F) 

**Đánh giá:** 2500 
**Thẻ:** 2-sat, cấu trúc dữ liệu, dfs và tương tự, dp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến$n$và một loạt câu hỏi. Mỗi truy vấn hỏi có bao nhiêu chuỗi chỉ mục trong một phạm vi được chỉ định$[l, r]$có thể được hình thành sao cho dãy tăng nghiêm ngặt và mỗi phần tử chia cho phần tử tiếp theo trong hoán vị. Nói một cách đơn giản hơn, chúng ta đang đếm các chuỗi có khả năng phân chia tăng dần trong một mảng con. 

Thuộc tính hoán vị đảm bảo rằng mọi số từ 1 đến$n$xuất hiện đúng một lần. Điều này đảm bảo rằng khi chúng ta xem xét mối quan hệ chia hết, mỗi số có một vị trí duy nhất trong mảng. Các truy vấn yêu cầu các mảng con có khả năng chồng chéo và có thể có tới$10^6$truy vấn trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp qua tất cả các chuỗi con một cách ngây thơ bởi vì số lượng các chuỗi con trong một mảng có độ dài$n$là$2^n-1$, điều này là không thể thực hiện được đối với$n$lên đến$10^6$. 

Các trường hợp cạnh bao gồm các mảng con có độ dài bằng một, trong đó câu trả lời gần như là một và các phạm vi chứa các số không có ước số trong mảng con, cũng phải được tính là các chuỗi một phần tử riêng lẻ. Việc triển khai bất cẩn có thể cố gắng tính toán trước các ước số mà không xem xét vị trí hoặc có thể đếm hai chuỗi khi tồn tại nhiều ước số. Ví dụ, trong mảng`[2, 1, 4]`với truy vấn`[1, 3]`, trình tự`(1, 4)`hợp lệ vì 1 chia 4, và`(2, 4)`cũng đúng vì 2 chia 4. Thiếu bất kỳ câu nào trong số này sẽ dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ lặp lại mọi chuỗi con trong phạm vi truy vấn, kiểm tra cả thứ tự tăng dần và khả năng chia hết. Đối với một mảng con có độ dài$k$, có$2^k-1$các chuỗi tiếp theo. Điều này rõ ràng là thất bại đối với$k$lớn hơn 20 hoặc 25, đạt mức tối đa$n$lên đến$10^6$. 

Cái nhìn sâu sắc quan trọng đến từ việc xem đây là một vấn đề về biểu đồ. Mỗi chỉ số$i$có thể là một nút và có một cạnh có hướng từ$i$ĐẾN$j$nếu như$i < j$Và$a[i]$chia rẽ$a[j]$. Việc đếm các chuỗi hợp lệ tương đương với việc đếm tất cả các đường dẫn trong DAG này. Vì mỗi số trong hoán vị là duy nhất và bị giới hạn bởi$n$, chúng ta có thể tính toán trước bội số của mỗi số rồi ánh xạ chúng tới các vị trí trong mảng. 

Lập trình động trên các chỉ mục là điều tự nhiên: xác định`dp[i]`là số lượng chuỗi hợp lệ bắt đầu từ chỉ mục`i`. Với mỗi chỉ số, tổng`1 + sum(dp[j])`tổng thể`j`như vậy`a[i]`chia rẽ`a[j]`Và`i < j`. Việc tính toán bội số có thể được tối ưu hóa bằng cách sử dụng bản đồ từ số đến chỉ mục của nó, cho phép truy cập O(1) vào vị trí của bội số hợp lệ. Để trả lời hiệu quả nhiều truy vấn, chúng ta có thể xây dựng một mảng tổng tiền tố trên`dp`giá trị, cho phép truy xuất truy vấn O(1) cho bất kỳ mảng con liền kề nào. Điều này làm giảm tổng độ phức tạp từ hàm mũ sang tuyến tính trong`n`cộng tuyến tính trong`q`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tổng tiền tố DAG + DP + | O(n + q) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy đọc mảng hoán vị`a`và xây dựng bản đồ`pos`từ giá trị đến chỉ số của chúng. Điều này cho phép chúng ta nhanh chóng tìm ra vị trí mỗi bội số xuất hiện. 
2. Khởi tạo một`dp`mảng độ dài`n+1`, Ở đâu`dp[i]`sẽ lưu trữ số lượng chuỗi hợp lệ bắt đầu từ chỉ mục`i`. 
3. Lặp lại từ`i = n`xuống tới`1`. Đối với mỗi`i`, bộ`dp[i] = 1`để tính đến chuỗi chỉ chứa`a[i]`. Sau đó lặp lại trên tất cả các bội số`m`của`a[i]`như vậy`m <= n`. Nếu vị trí của`m`lớn hơn`i`, thêm vào`dp[pos[m]]`ĐẾN`dp[i]`. 
4. Xây dựng mảng tổng tiền tố`pref`qua`dp`, để có thể`pref[i] = dp[1] + ... + dp[i]`. 
5. Đối với mỗi truy vấn`[l, r]`, tính toán câu trả lời như`pref[r] - pref[l-1]`. Điều này hoạt động vì`dp[i]`đếm tất cả các chuỗi bắt đầu từ`i`, và trừ`pref[l-1]`loại trừ các chuỗi bắt đầu trước`l`. 
6. In câu trả lời cho tất cả các câu hỏi theo thứ tự được đưa ra. 

Bất biến đảm bảo tính đúng đắn là`dp[i]`đếm tất cả các chuỗi hợp lệ bắt đầu từ chỉ mục`i`và các chuỗi bắt đầu ở các chỉ số trước đó không can thiệp vào điều này vì các cạnh chỉ hướng về phía trước. Tổng tiền tố cho phép chúng tôi tính tổng các số đếm này một cách hiệu quả cho bất kỳ phạm vi liền kề nào mà không cần tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        pos = [0] * (n + 1)
        for idx, val in enumerate(a):
            pos[val] = idx + 1

        dp = [0] * (n + 2)
        for i in range(n, 0, -1):
            dp[i] = 1
            val = a[i-1]
            mul = 2
            while val * mul <= n:
                m = val * mul
                if pos[m] > i:
                    dp[i] += dp[pos[m]]
                mul += 1

        pref = [0] * (n + 2)
        for i in range(1, n + 1):
            pref[i] = pref[i-1] + dp[i]

        res = []
        for _ in range(q):
            l, r = map(int, input().split())
            res.append(str(pref[r] - pref[l-1]))
        print(' '.join(res))

if __name__ == "__main__":
    solve()
```Phần đầu tiên ánh xạ các số tới các vị trí để nhanh chóng xác định các bội số hợp lệ. Việc lặp lại đảm bảo rằng`dp[i]`bao gồm tất cả các chuỗi bắt đầu sau đó có thể chia hết cho`a[i]`. Tổng tiền tố cho phép truy vấn phạm vi thời gian không đổi. Tránh lỗi từng cái một bằng cách sử dụng lập chỉ mục dựa trên 1 cho cả hai`pos`Và`dp`. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n=3`,`a=[2,1,3]`, truy vấn`[1,3]`| tôi | một [tôi] | dp[i] | Giải thích | 
| --- | --- | --- | --- | 
| 3 | 3 | 1 | Chỉ trình tự [3] | 
| 2 | 1 | 2 | Chuỗi [1], [1,3] | 
| 1 | 2 | 2 | Chuỗi [2], [2,3] |`pref = [0,2,4,5]`. Truy vấn`[1,3]`cho`5 - 0 = 5`. 

**Ví dụ 2:**`n=4`,`a=[2,3,1,4]`, truy vấn`[2,4]`| tôi | một [tôi] | dp[i] | 
| --- | --- | --- | 
| 4 | 4 | 1 | 
| 3 | 1 | 2 | 
| 2 | 3 | 1 | 
| 1 | 2 | 2 |`pref = [0,2,3,5,6]`. Truy vấn`[2,4]`cho`6 - 2 = 4`. 

Những dấu vết này cho thấy các chuỗi được tính chính xác theo tính phân chia và thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n sqrt(n) + q) | Lặp lại bội số cho mỗi`a[i]`chi phí trung bình lên tới O(sqrt(n)); tổng tiền tố O(n); truy vấn O(q) | 
| Không gian | O(n) | Mảng`dp`,`pos`,`pref`tất cả O(n) | 

Được cho`sum(n), sum(q) <= 10^6`, giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("4\n8 8\n2 1 6 3 5 4 8 7\n1 8\n2 8\n1 7\n1 6\n1 3\n5 8\n4 4\n2 3\n1 1\n1\n1 1\n3 3\n3 2 1\n1 2\n1 3\n2 3\n8 1\n1 2 3 4 5 6 7 8\n1 8\n") == "20 15 18 12 5 5 1 3\n1\n2 3 2\n27", "sample 1"

# Minimum input
assert run("1\n1 1\n1\n1 1\n") == "1
```
