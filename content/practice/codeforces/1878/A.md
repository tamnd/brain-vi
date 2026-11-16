---
title: "CF 1878A - Daytona có giá bao nhiêu?"
description: "Bài toán hỏi liệu, với một mảng số nguyên và số nguyên đích $k$, có tồn tại ít nhất một mảng con liền kề trong đó $k$ là phần tử xuất hiện thường xuyên hơn bất kỳ phần tử nào khác hay không."
date: "2026-06-08T22:50:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1878
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 900 (Div. 3)"
rating: 800
weight: 1878
solve_time_s: 106
verified: true
draft: false
---

[CF 1878A - Daytona có giá bao nhiêu?](https://codeforces.com/problemset/problem/1878/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán hỏi liệu, cho một mảng số nguyên và số nguyên đích$k$, tồn tại ít nhất một mảng con liền kề trong đó$k$là phần tử xuất hiện thường xuyên hơn bất kỳ phần tử nào khác. Mỗi trường hợp thử nghiệm cho biết kích thước của mảng$n$, mục tiêu$k$, và mảng$a$chính nó. Câu trả lời cho mỗi trường hợp thử nghiệm là "CÓ" nếu phân đoạn đó tồn tại và "KHÔNG" nếu ngược lại. 

Các hạn chế là nhỏ:$n$nhiều nhất là 100 và$t$, số lượng ca kiểm thử lên tới 1000. Mỗi phần tử$a_i$Và$k$cũng nhiều nhất là 100. Điều này có nghĩa là$O(n^2)$Cách tiếp cận cho một trường hợp thử nghiệm duy nhất, sẽ khám phá tất cả các mảng con, có thể chấp nhận được về mặt tính toán vì trường hợp xấu nhất sẽ liên quan đến$1000 \cdot 100^2 = 10^7$hoạt động, có thể quản lý trong giới hạn thời gian 1 giây. 

Các trường hợp khó phát hiện khi$k$chỉ xảy ra một lần trong mảng hoặc xuất hiện ở cuối hoặc đầu. Ví dụ, nếu$a = [1, 2, 3]$Và$k = 3$, mảng con duy nhất chứa$k$là$[3]$. Trong trường hợp như vậy, thuật toán phải xác định chính xác rằng mảng con một phần tử đủ điều kiện, như$k$tầm thường là yếu tố phổ biến nhất ở đó. Một trường hợp khác cần xem xét là khi$k$xuất hiện nhiều lần nhưng luôn được phân tách bằng các phần tử khác sao cho không có mảng con nào cho phép$k$chiếm ưu thế về số lượng; xử lý chính xác các cửa sổ có độ dài tối thiểu sẽ đảm bảo giải pháp mạnh mẽ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ lặp lại trên tất cả các mảng con có thể có của$a$và đếm tần số của từng phần tử trong mảng con đó, sau đó kiểm tra xem$k$có số lượng cao nhất. Đối với một mảng có độ dài$n$, có khoảng$n(n+1)/2$mảng con và việc đếm tần số trong mỗi mảng con có thể mất tới$O(n)$hoạt động, dẫn đến$O(n^3)$mỗi trường hợp thử nghiệm. Cách tiếp cận này đúng nhưng không hiệu quả ngay cả đối với$n = 100$, mang lại$10^6$hoạt động cho mỗi trường hợp thử nghiệm và$10^9$trong trường hợp xấu nhất trong tất cả các trường hợp thử nghiệm. 

Một quan sát quan trọng làm giảm vấn đề một cách đáng kể:$k$chỉ cần xảy ra trong một mảng con cùng với nhiều nhất một lần xuất hiện khác của$k$gần đó để thống trị các nước láng giềng. Cụ thể hơn, nếu$k$xuất hiện ít nhất hai lần và hai lần xuất hiện ở vị trí$i$Và$j$với$j - i \le 2$, sau đó$k$có thể là thường xuyên nhất trong các vị trí bao phủ mảng con$i$bởi vì$j$. Một lần xuất hiện là đủ nếu mảng có độ dài bằng 1 hoặc nếu không có phần tử nào khác xung quanh cạnh tranh với nó. 

Giải pháp tối ưu tận dụng điều này bằng cách quét mảng và kiểm tra khoảng cách giữa các lần xuất hiện của$k$. Nếu như$k$xuất hiện ít nhất hai lần trong khoảng cách hai phần tử thì ta có thể trả lời ngay là “CÓ”. Ngược lại, chúng tôi kiểm tra xem$k$xảy ra ít nhất một lần trong mảng: nếu nó chỉ xảy ra một lần và độ dài mảng là một thì câu trả lời là "CÓ"; nếu mảng dài hơn, một mảng bị cô lập$k$là đủ, vì ngay cả trong mảng con hai phần tử, nó có thể xuất hiện thường xuyên nhất nếu phần tử kia khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$,$k$, và mảng$a$. 
2. Đếm xem bao nhiêu lần$k$xuất hiện trong mảng. Nếu nó xuất hiện 0 lần, hãy in "NO" và chuyển sang test case tiếp theo. 
3. Nếu mảng chứa đúng một phần tử, in ra "CÓ" nếu phần tử đó là$k$. 
4. Nếu không, hãy lặp qua mảng và kiểm tra từng cặp chỉ số$i$Và$j$Ở đâu$i < j$Và$j - i \le 2$. Nếu cả hai vị trí đều chứa$k$, in ngay "CÓ". 
5. Nếu không tìm thấy cặp như vậy nhưng$k$tồn tại trong mảng, in "CÓ" vì mảng con một phần tử chứa$k$đủ điều kiện. 
6. Nếu không đáp ứng được điều kiện nào, hãy in "KHÔNG". 

Tại sao nó hoạt động: Điều bất biến ở đây là bất kỳ mảng con nào có độ dài chứa một$k$được tự động hợp lệ. Đối với các mảng con dài hơn, việc kiểm tra khoảng cách đảm bảo rằng$k$có thể chiếm ưu thế ít nhất hai phần tử liền kề, tạo thành một mảng con hợp lệ. Thuật toán xác định chính xác tất cả các mảng con tối thiểu trong đó$k$có thể là cách thường xuyên nhất, tránh việc đếm tất cả các mảng con không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    count_k = a.count(k)
    
    if count_k == 0:
        print("NO")
        continue
    
    if n == 1:
        print("YES")
        continue
    
    found = False
    for i in range(n):
        if a[i] != k:
            continue
        for j in range(i, min(i + 3, n)):
            if a[j] == k and j - i <= 2:
                found = True
                break
        if found:
            break
    
    print("YES" if found else "NO")
```Giải pháp này trước tiên đếm số lần xuất hiện của$k$. Nếu như$k$vắng mặt thì câu trả lời là “KHÔNG”. Mảng một phần tử rất dễ xử lý. Vòng lặp lồng nhau kiểm tra các cặp trong một cửa sổ nhỏ (độ dài tối đa là 3) để xem liệu$k$có thể thống trị tại địa phương. Giới hạn trên của vòng lặp bên trong đảm bảo chúng ta không vượt quá giới hạn mảng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
5 4
1 4 3 4 1
```| tôi | j | a[i..j] | Kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | [4,3] | k xuất hiện một lần, dài 2, chiếm ưu thế | CÓ | 

Điều này cho thấy một mảng con hợp lệ [4,3] trong đó 4 là phổ biến nhất. 

**Đầu vào mẫu 2:**```
4 1
2 3 4 4
```| tôi | j | a[i..j] | Kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [2] | k=1 không có mặt | KHÔNG | 
| ... | ... | ... | không chứa k | KHÔNG | 

Không có phân mảng nào chứa 1, do đó thuật toán sẽ đưa ra kết quả "KHÔNG" một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi trường hợp kiểm thử sẽ quét mảng một lần và kiểm tra các cửa sổ nhỏ có tối đa 3 phần tử. | 
| Không gian | O(n) | Lưu trữ mảng cho từng trường hợp thử nghiệm. | 

Được cho$t \le 1000$Và$n \le 100$, nhiều nhất$10^5$hoạt động xảy ra, trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        count_k = a.count(k)
        if count_k == 0:
            print("NO")
            continue
        if n == 1:
            print("YES")
            continue
        found = False
        for i in range(n):
            if a[i] != k:
                continue
            for j in range(i, min(i + 3, n)):
                if a[j] == k and j - i <= 2:
                    found = True
                    break
            if found:
                break
        print("YES" if found else "NO")
    return output.getvalue().strip()

# provided samples
assert run("7\n5 4\n1 4 3 4 1\n4 1\n2 3 4 4\n5 6\n43 5 60 4 2\n2 5\n1 5\n4 1\n5 3 3 1\n1 3\n3\n5 3\n3 4 1 5 5\n") == \
"YES\nNO\nNO\nYES\nYES\nYES\nYES", "Sample tests"

# custom cases
assert run("2\n1 1\n1\n3 2\n1 2 2\n") == "YES\nYES", "single-element and duplicate k"
assert run("1\n5 5\n1 2 3 4 5\n") == "YES", "k at end"
assert run("1\n4 10\n1 2 3 4\n") == "NO", "k absent"
assert run("1\n3 2\n2 1 2\n") == "
```
