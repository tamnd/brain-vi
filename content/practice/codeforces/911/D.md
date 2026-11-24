---
title: "CF 911D - Đếm nghịch đảo"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n, nghĩa là mọi số trong phạm vi đó xuất hiện đúng một lần theo một thứ tự nào đó. Sự đảo ngược là một cặp vị trí trong đó chỉ số lớn hơn giữ số nhỏ hơn chỉ số nhỏ hơn, thực chất là một "rối loạn cục bộ"."
date: "2026-06-13T00:29:39+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 911
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 35 (Rated for Div. 2)"
rating: 1800
weight: 911
solve_time_s: 275
verified: true
draft: false
---

[CF 911D - Đếm nghịch đảo](https://codeforces.com/problemset/problem/911/D) 

**Đánh giá:** 1800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 4 phút 35 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị các số nguyên từ 1 đến _n_, nghĩa là mọi số trong phạm vi đó xuất hiện chính xác một lần theo một thứ tự nào đó. Đảo ngược là một cặp vị trí trong đó chỉ số lớn hơn giữ số nhỏ hơn chỉ số nhỏ hơn, thực chất là một "rối loạn" cục bộ. Ví dụ: trong hoán vị [4, 1, 3, 2], số 1 ở chỉ số 2 nhỏ hơn số 4 ở chỉ số 1, tạo thành một đảo ngược. 

Nhiệm vụ là xử lý nhiều truy vấn, mỗi truy vấn đảo ngược một phân đoạn của hoán vị. Sau mỗi lần đảo chiều, chúng ta không cần số lần đảo ngược chính xác mà chỉ cần số lần đảo ngược là số lẻ hay số chẵn. 

Những hạn chế là tinh tế. Độ dài hoán vị _n_ lên tới 1500, đủ nhỏ để thực hiện các phép tính bậc hai trong _n_ nếu cần. Tuy nhiên, số lượng truy vấn _m_ rất lớn, lên tới 2·10^5. Sự kết hợp này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại các phép đảo ngược từ đầu sau mỗi truy vấn, vì ngay cả số lần đảo ngược O(n^2) cho mỗi truy vấn cũng sẽ yêu cầu khoảng 4,5·10^11 thao tác trong trường hợp xấu nhất. Chúng tôi cần một cách tiếp cận để xử lý các truy vấn ở mức tồi tệ nhất là O(1) hoặc O(n). 

Các trường hợp cạnh phát sinh từ các đoạn rất ngắn hoặc các đoạn có độ dài bằng một. Việc đảo ngược một đoạn có độ dài 1 sẽ không làm thay đổi số lượng đảo ngược. Một hoán vị đã theo thứ tự tăng dần hoặc giảm dần đòi hỏi phải xử lý cẩn thận để đảm bảo các lần lật chẵn lẻ được tính toán chính xác. Ví dụ: đảo ngược toàn bộ hoán vị của kích thước 2 sẽ chuyển một lần đảo ngược duy nhất, chuyển tính chẵn lẻ từ chẵn sang lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: sau khi đọc hoán vị, chúng ta có thể lặp lại từng cặp chỉ mục cho mỗi truy vấn và đếm số lần đảo ngược. Điều này hiệu quả vì định nghĩa đảo ngược rất rõ ràng, nhưng nó trở nên không thực tế ngay khi chúng tôi có hơn vài nghìn truy vấn vì mỗi số lượng sẽ là O(n^2). 

Thông tin chuyên sâu quan trọng giúp mở ra giải pháp hiệu quả đến từ việc tập trung vào tính chẵn lẻ thay vì số lượng đảo ngược chính xác. Hãy xem điều gì sẽ xảy ra khi chúng ta đảo ngược một mảng con có độ dài _k_. Mỗi cặp phần tử trong mảng con hoán đổi thứ tự tương đối của chúng chính xác một lần. Có các cặp _k·(k-1)/2_ trong một đoạn có độ dài _k_. Do đó, tính chẵn lẻ của số nghịch đảo trong đoạn đảo khi và chỉ khi _k·(k-1)/2_ là số lẻ. Điều này làm giảm mỗi truy vấn thành việc tính toán xem giá trị số học đơn giản này là số lẻ hay số chẵn, điều này có thể được thực hiện trong thời gian không đổi. 

Chúng tôi không cần theo dõi những phần tử cụ thể nào được trao đổi; điều bất biến là tính chẵn lẻ của tổng số đảo ngược chỉ thay đổi khi số cặp phần tử trong đoạn đảo ngược là số lẻ. Quan sát này biến đổi một vấn đề dường như vốn có O(n^2) trên mỗi truy vấn thành O(1) trên mỗi bản cập nhật chẵn lẻ truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m·n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n^2 + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, đếm số lần đảo ngược ban đầu trong hoán vị. Điều này được thực hiện bằng cách lặp qua tất cả các cặp`(i, j)`với`i > j`và kiểm tra xem`a[i] < a[j]`. Chỉ lưu trữ tính chẵn lẻ: chẵn hoặc lẻ. 
2. Đối với mỗi truy vấn có phân đoạn`[l, r]`, tính độ dài của đoạn đó là`k = r - l + 1`. 
3. Tính số cặp trong đoạn đó là`k * (k - 1) / 2`. Kiểm tra xem số này là số lẻ hay số chẵn. Nếu lẻ, lật tính chẵn lẻ hiện tại của nghịch đảo. Nếu chẵn thì giữ nguyên số chẵn lẻ. 
4. Xuất ra "lẻ" hoặc "chẵn" dựa trên giá trị chẵn lẻ được cập nhật. 

Tại sao nó hoạt động: Tính chẵn lẻ của các nghịch đảo trong hoán vị chỉ bị ảnh hưởng bởi các hoán đổi có thứ tự đảo ngược trong phân đoạn. Mỗi cặp trong phân đoạn đảo ngược đóng góp chính xác một lần hoán đổi vào số lần đảo ngược. Do đó, thông tin liên quan duy nhất cho truy vấn là số lượng các cặp như vậy là số lẻ hay số chẵn, điều này đảm bảo cập nhật tính chẵn lẻ là chính xác cho mọi hoán vị và bất kỳ độ dài truy vấn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
m = int(input())

# compute initial inversion parity
parity = 0  # 0 = even, 1 = odd
for i in range(n):
    for j in range(i):
        if a[i] < a[j]:
            parity ^= 1  # flip parity

for _ in range(m):
    l, r = map(int, input().split())
    k = r - l + 1
    if (k * (k - 1) // 2) % 2 == 1:
        parity ^= 1
    print("odd" if parity else "even")
```Vòng lặp kép đầu tiên tính toán chính xác số chẵn lẻ ban đầu. Sử dụng XOR với 1 sẽ lật tính chẵn lẻ bất cứ khi nào một phép hoán đổi đóng góp một số lẻ. Quá trình xử lý truy vấn là thời gian không đổi: tính độ dài, tính số cặp, kiểm tra tính chẵn lẻ và lật. Không có sai sót nào kể từ khi chúng tôi tính toán`k = r - l + 1`chính xác cho các chỉ số bao gồm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
1 2 3
2
1 2
2 3
```| Bước | Độ dài đoạn k | Cặp k*(k-1)/2 | Lật mặt? | Chẵn lẻ | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | 0 | - | 
| Truy vấn 1 [1,2] | 2 | 1 | Có | 1 | lẻ | 
| Truy vấn 2 [2,3] | 2 | 1 | Có | 0 | thậm chí | 

Điều này chứng tỏ việc lật chẵn lẻ chính xác cho các phân đoạn tối thiểu. 

Đầu vào mẫu 2:```
4
1 2 4 3
4
2 4
1 3
1 4
3 3
```| Bước | Độ dài đoạn k | Cặp k*(k-1)/2 | Lật mặt? | Chẵn lẻ | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | 1 | - | 
| Truy vấn 1 [2,4] | 3 | 3 | Có | 0 | thậm chí | 
| Truy vấn 2 [1,3] | 3 | 3 | Có | 1 | lẻ | 
| Truy vấn 3 [1,4] | 4 | 6 | Không | 1 | lẻ | 
| Truy vấn 4 [3,3] | 1 | 0 | Không | 1 | lẻ | 

Bảng này cho thấy các phép đảo ngược một phần tử không ảnh hưởng đến tính chẵn lẻ và các phép đảo chiều có độ dài chẵn lớn hơn có thể không ảnh hưởng đến tính chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 + m) | Tính chẵn lẻ đảo ngược ban đầu mất O(n^2), mỗi truy vấn là O(1) | 
| Không gian | O(n) | Chỉ lưu trữ mảng hoán vị | 

Với n 1500 và m 2·10^5, bước đầu tiên O(n^2) là khoảng 2,25·10^6 thao tác, có thể chấp nhận được. Xử lý truy vấn trong O(1) mỗi thao tác là 2·10^5 thao tác, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    parity = 0
    for i in range(n):
        for j in range(i):
            if a[i] < a[j]:
                parity ^= 1
    for _ in range(m):
        l, r = map(int, input().split())
        k = r - l + 1
        if (k * (k - 1) // 2) % 2 == 1:
            parity ^= 1
        output.append("odd" if parity else "even")
    return "\n".join(output)

# provided samples
assert run("3\n1 2 3\n2\n1 2\n2 3\n") == "odd\neven", "sample 1"
assert run("4\n1 2 4 3\n4\n2 4\n1 3\n1 4\n3 3\n") == "even\nodd\nodd\nodd", "sample 2"

# custom cases
assert run("1\n1\n1\n1 1\n") == "even", "single element"
assert run("2\n2 1\n1\n1 2\n") == "even", "two element swap flips parity"
assert run("5\n5 4 3 2 1\n3\n1 5\n2 4\n3 3\n") == "even\nodd\nodd", "descending with mixed queries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1\n1\n1\n1 1\n" | thậm chí | Phân đoạn phần tử đơn không nên lật chẵn lẻ | 
| "2\n2 1\n1\n1 2\n" | thậm chí | Hai phần tử đảo ngược tính chẵn lẻ từ số lẻ ban đầu | 
| "5\n5 4 3 2 1\n3\n1 5\n2 4\n3 3\n" | chẵn, lẻ, lẻ | Đảo ngược ban đầu lớn, chiều dài hỗn hợp |
