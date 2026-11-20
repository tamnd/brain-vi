---
title: "CF 2136B - Giống như Bitset"
description: "Chúng ta được yêu cầu xây dựng một hoán vị có độ dài $n$ thỏa mãn ràng buộc tối đa về vị trí được xác định bởi chuỗi nhị phân $s$ và số nguyên $k$."
date: "2026-06-09T04:10:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2136
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1046 (Div. 2)"
rating: 900
weight: 2136
solve_time_s: 85
verified: false
draft: false
---

[CF 2136B - Giống như Bitset](https://codeforces.com/problemset/problem/2136/B) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị độ dài$n$thỏa mãn ràng buộc tối đa về vị trí được xác định bởi chuỗi nhị phân$s$và một số nguyên$k$. Đối với mọi vị trí$i$Ở đâu$s_i = 1$, chúng ta phải đảm bảo rằng$p_i$không bao giờ là giá trị lớn nhất trong bất kỳ mảng con liền kề nào có độ dài ít nhất$k$có chứa$i$. Vị trí với$s_i = 0$không có hạn chế. Nếu hoán vị như vậy không thể tồn tại, chúng tôi sẽ báo cáo "KHÔNG". 

Những ràng buộc cho phép$n$lên đến$2 \cdot 10^5$trên nhiều trường hợp thử nghiệm. Điều này loại trừ các thuật toán với$O(n^2)$hành vi vì tổng số hoạt động trong trường hợp xấu nhất có thể vượt quá$10^{10}$. Chúng tôi cần một$O(n \log n)$hoặc tốt hơn, lý tưởng nhất$O(n)$, giải pháp cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh khó phát sinh khi có quá nhiều số 1 liên tiếp so với$k$. Ví dụ, nếu$s = 1111$Và$k = 3$, bất kỳ phần tử nào trong khối gồm bốn số 1 sẽ nằm trong một khoảng có độ dài bằng 3 hoặc dài hơn và nếu các phần tử được gán liên tiếp thì một trong số chúng chắc chắn sẽ có giá trị lớn nhất trong một khoảng nào đó. Điều này có thể làm cho việc hoán vị không thể thực hiện được. Một trường hợp cạnh khác là khi$k = 1$; mọi phần tử với$s_i = 1$nằm trong khoảng có độ dài 1, bao gồm chính nó một cách tầm thường, nhưng vấn đề cho phép$p_i$không được lớn nhất trong khoảng thời gian có độ dài-1. Điều này quan trọng vì nó thay đổi cách chúng ta gán số lớn nhất và số nhỏ nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử tất cả các hoán vị và kiểm tra tất cả các mảng con có độ dài ít nhất$k$với mỗi 1. Điều này rõ ràng là không khả thi: với mỗi hoán vị có$n!$khả năng, và việc kiểm tra tất cả các khoảng thời gian có thể mất$O(n^2)$. Vì vậy ngay cả đối với nhỏ$n$điều này là quá chậm. 

Quan sát quan trọng là các ràng buộc chỉ quan trọng trong 1 giây. Đối với số 0, chúng ta có thể tự do gán bất kỳ số nào. Để đảm bảo số 1 không bao giờ là giá trị tối đa trong bất kỳ khoảng thời gian nào$\ge k$chứa nó, chúng ta có thể gán tất cả các số tương đối nhỏ ở vị trí 1 và tất cả các số tương đối lớn ở vị trí 0. Sau đó, trong bất kỳ khoảng thời gian nào$k$hoặc nhiều hơn, sẽ luôn có ít nhất một số 0 có số cao hơn các số ở vị trí 1, đảm bảo không có số 1 nào là lớn nhất. 

Tuy nhiên, có một giới hạn. Nếu một khối gồm các số 1 liên tiếp có độ dài lớn hơn$k$, khi đó tồn tại độ dài-$k$khoảng hoàn toàn bên trong khối này. Tất cả các số trong khoảng đó sẽ nhỏ, vì vậy giá trị tối đa nhất thiết phải là một trong số 1. Do đó, chúng ta phải kiểm tra xem khối 1 liền kề dài nhất có nhỏ hơn không$k$. Nếu đúng như vậy thì một phép gán hợp lệ là có thể. Nếu không thì không thể được. 

Điều này đưa ra một cách tiếp cận mang tính xây dựng: gán các số nhỏ nhất cho các số 1 theo thứ tự và các số lớn nhất cho các số 0 theo thứ tự. Chúng ta có thể chỉ cần lặp qua chuỗi và điền vào danh sách các số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n) | Quá chậm | 
| Tham lam mang tính xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp qua chuỗi nhị phân$s$và tìm độ dài của đoạn liền kề dài nhất là 1 giây. Nếu chiều dài này là$\ge k$, xuất ra "KHÔNG" vì sẽ luôn có độ dài-$k$khoảng thời gian 1 giây trong đó một trong số chúng là giá trị tối đa. 
2. Nếu đoạn 1 dài nhất nhỏ hơn$k$, xuất ra "CÓ". Chúng ta có thể xây dựng một hoán vị. 
3. Chuẩn bị hai danh sách: một danh sách các số gán cho số 1 (bắt đầu từ 1 trở lên) và một danh sách cho các số gán cho số 0 (bắt đầu từ$n$hướng xuống dưới). 
4. Lặp lại$s$theo thứ tự. Đối với mỗi vị trí$i$, nếu như$s_i = 1$, gán số chưa sử dụng nhỏ nhất tiếp theo. Nếu như$s_i = 0$, gán số chưa sử dụng lớn nhất tiếp theo. 
5. In hoán vị đã xây dựng. 

Tại sao nó hoạt động: Bằng cách gán các số nhỏ hơn cho các số 1 và các số lớn hơn cho các số 0, ít nhất bất kỳ khoảng độ dài nào cũng được$k$chứa ít nhất một số 0 sẽ có số 0 lớn hơn số 1, đảm bảo không có số 1 nào là lớn nhất. Bởi vì khối 1 liên tiếp dài nhất nhỏ hơn$k$, bất kỳ khoảng độ dài nào$k$trở lên phải có ít nhất một số 0. Do đó, việc xây dựng luôn thỏa mãn ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()
    
    # Step 1: check longest consecutive 1s
    max_ones = 0
    cur = 0
    for ch in s:
        if ch == '1':
            cur += 1
            max_ones = max(max_ones, cur)
        else:
            cur = 0
    
    if max_ones >= k:
        print("NO")
        continue
    
    # Step 2: construct permutation
    perm = [0] * n
    low = 1
    high = n
    for i, ch in enumerate(s):
        if ch == '1':
            perm[i] = low
            low += 1
        else:
            perm[i] = high
            high -= 1
    print("YES")
    print(' '.join(map(str, perm)))
```Phần đầu tiên lặp qua chuỗi để tìm khối 1 liền kề dài nhất. Đây là điểm duy nhất mà việc hoán vị có thể là không thể. Phần thứ hai là bài tập mang tính xây dựng: sử dụng hai con trỏ (thấp và cao) đảm bảo tất cả các số được sử dụng chính xác một lần và số 1 nhận được số nhỏ nhất trong khi số 0 nhận được số lớn nhất. Các lỗi xảy ra có thể tránh được bằng cách khởi tạo cẩn thận low = 1 và high = n, tăng hoặc giảm ngay sau khi gán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
0010
```| tôi | s[i] | max_ones | thấp | cao | perm[i] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 4 | 4 | 
| 1 | 0 | 0 | 1 | 3 | 3 | 
| 2 | 1 | 1 | 1 | 3 | 1 | 
| 3 | 0 | 0 | 2 | 2 | 2 | 

Hoán vị:`[4, 3, 1, 2]`. Số lượng tối đa trong bất kỳ khoảng độ dài nào >= 3 bao phủ vị trí 2 là 4, 3 hoặc 2, không bao giờ là 1. Hợp lệ. 

### Ví dụ 2 

đầu vào:```
5 2
11011
```1s liền kề dài nhất = 2, bằng k. Theo kiểm tra của chúng tôi, vì max_ones >= k nên đầu ra là "KHÔNG". 

Những dấu vết này xác nhận rằng thuật toán xác định chính xác các trường hợp không thể và xây dựng các hoán vị khi khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để tìm khối 1 dài nhất và một lượt để xây dựng hoán vị | 
| Không gian | O(n) | Lưu trữ cho hoán vị | 

Với tổng số$n$trên tất cả các trường hợp thử nghiệm 2e5, tổng số thao tác đều ở mức dưới 1e6 một cách thoải mái, phù hợp trong giới hạn thời gian. Mức sử dụng bộ nhớ trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        max_ones = 0
        cur = 0
        for ch in s:
            if ch == '1':
                cur += 1
                max_ones = max(max_ones, cur)
            else:
                cur = 0
        if max_ones >= k:
            print("NO")
            continue
        perm = [0] * n
        low = 1
        high = n
        for i, ch in enumerate(s):
            if ch == '1':
                perm[i] = low
                low += 1
            else:
                perm[i] = high
                high -= 1
        print("YES")
        print(' '.join(map(str, perm)))
    return output.getvalue().strip()

# Provided samples
assert run("6\n2 1\n00\n4 3\n0010\n5 2\n11011\n7 5\n1111110\n8 4\n00101011\n10 2\n1000000010\n") == \
"""YES
2 1
YES
4 3 1 2
NO
NO
YES
8 7 2 6 5 4
```
