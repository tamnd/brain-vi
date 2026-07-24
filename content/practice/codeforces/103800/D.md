---
title: "CF 103800D - dòng Gừng"
description: "Chúng ta có một tập hợp các đường thẳng trong mặt phẳng, mỗi đường được mô tả bằng một phương trình có dạng $y = ai x + bi$. Nhiệm vụ là đếm xem có bao nhiêu cặp đường thẳng phân biệt không có thứ tự cắt nhau tại đúng một điểm. Về mặt hình học, hai đường thẳng cắt nhau nếu chúng không song song."
date: "2026-07-02T08:42:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103800
codeforces_index: "D"
codeforces_contest_name: "The 2022 SDUT Summer Trials"
rating: 0
weight: 103800
solve_time_s: 51
verified: true
draft: false
---

[CF 103800D - Dòng gừng](https://codeforces.com/problemset/problem/103800/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta một tập hợp các đường thẳng trong mặt phẳng, mỗi đường được mô tả bằng một phương trình có dạng$y = a_i x + b_i$. Nhiệm vụ là đếm xem có bao nhiêu cặp đường thẳng phân biệt không có thứ tự cắt nhau tại đúng một điểm. 

Về mặt hình học, hai đường thẳng cắt nhau nếu chúng không song song. Đối với các đường ở dạng chặn độ dốc, sự song song xảy ra chính xác khi hệ số góc của chúng bằng nhau, đó là khi$a_i = a_j$. Vấn đề bổ sung thêm một chi tiết nữa một cách rõ ràng: nếu hai đường thẳng giống hệt nhau, nghĩa là cả độ dốc và giao điểm đều khớp nhau, thì chúng không được coi là cặp giao nhau. 

Vì vậy, đầu ra về cơ bản là số lượng cặp$(i, j)$với$i < j$sao cho các đường thẳng không song song và không giống nhau. 

Những hạn chế là$n \le 5 \cdot 10^3$. Điều này đủ nhỏ để$O(n^2)$cách tiếp cận có thể chấp nhận được. Bất kỳ nỗ lực nào kém hơn phương pháp bậc hai, chẳng hạn như kiểm tra từng cặp bằng phép tính nặng hoặc sử dụng cách sắp xếp bên trong các vòng lặp lồng nhau, đều có nguy cơ bị hết thời gian. Tuyến tính hoặc$n \log n$Cách tiếp cận này không thực sự cần thiết nhưng vẫn mong đợi một giải pháp đếm tổ hợp rõ ràng. 

Một trường hợp tinh tế phát sinh khi tồn tại nhiều dòng giống hệt nhau. Ví dụ: nếu đầu vào chứa:```
1 1
1 1
1 1
```cả ba dòng trùng nhau một cách chính xác. Đếm một cách ngây thơ “các cặp không song song” sẽ đếm cả 3 cặp, nhưng không có cặp nào trong số chúng được coi là giao nhau. Câu trả lời đúng là 0 vì các dòng giống nhau bị loại trừ. 

Một trường hợp cạnh khác là khi nhiều đường có cùng độ dốc nhưng có điểm giao cắt khác nhau:```
2 0
2 1
2 2
```Tất cả đều song song nhưng khác biệt nên không có cặp nào giao nhau và câu trả lời là 0. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải bài toán là xét từng cặp đường thẳng và kiểm tra xem chúng có cắt nhau hay không. Hai dòng$y = a_i x + b_i$Và$y = a_j x + b_j$cắt nhau khi và chỉ khi$a_i \ne a_j$. Nếu các hệ số góc bằng nhau, chúng sẽ không bao giờ gặp nhau trừ khi chúng giống hệt nhau và dù sao đi nữa, các đường giống hệt nhau cũng bị loại trừ một cách rõ ràng. 

Điều này dẫn đến một giải pháp bạo lực đơn giản: lặp lại tất cả các cặp$(i, j)$, kiểm tra xem$a_i \ne a_j$, và đếm chúng. Điều này đúng vì mỗi cặp đường thẳng không song song trong mặt phẳng cắt nhau đúng một lần. 

Tuy nhiên, cách tiếp cận này thực hiện$\frac{n(n-1)}{2}$so sánh, tức là khoảng 12,5 triệu phép tính trong trường hợp xấu nhất khi$n = 5000$. Đây đã là ranh giới nhưng vẫn được chấp nhận trong Python nếu được thực hiện ở mức tối thiểu. Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa bằng cách chuyển từ kiểm tra từng cặp sang đếm xem có bao nhiêu cặp bị loại trừ. 

Thay vì đếm trực tiếp các cặp giao nhau, chúng ta đếm tất cả các cặp và trừ đi những cặp không giao nhau. Các cặp không giao nhau chính xác là các cặp đường có độ dốc bằng nhau. Nếu giá trị độ dốc xuất hiện$k$lần, thì nó góp phần$\frac{k(k-1)}{2}$cặp không giao nhau. Vấn đề duy nhất còn lại là các đường giống hệt nhau, nhưng các đường giống hệt nhau đã được đưa vào bên trong các nhóm độ dốc và việc trừ đi tất cả các cặp độ dốc bằng nhau sẽ loại bỏ chính xác cả những đóng góp song song và trùng lặp theo yêu cầu cho định nghĩa bài toán này, vì dù sao thì các đường trùng lặp cũng không bao giờ đóng góp vào giao điểm. 

Do đó, vấn đề giảm xuống còn việc nhóm các đường theo độ dốc và sự kết hợp tổng bên trong mỗi nhóm. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng và nhóm chúng theo độ dốc$a_i$, lưu trữ số lần mỗi độ dốc xuất hiện. Chúng ta chỉ cần đếm vì các điểm chặn không ảnh hưởng đến việc các đường có giao nhau hay không. 
2. Tính tổng số cặp không có thứ tự trong tất cả các dòng bằng cách sử dụng$\frac{n(n-1)}{2}$. Điều này thể hiện số lượng giao điểm tối đa có thể nếu không có đường nào song song. 
3. Đối với từng nhóm dốc có kích thước$k$, tính số cặp không thể giao nhau, đó là$\frac{k(k-1)}{2}$. Trừ đi điều này từ tổng số. 
4. Xuất ra giá trị kết quả. 

Lý do bước 3 hợp lệ là vì bất kỳ cặp đường nào có độ dốc bằng nhau sẽ không bao giờ giao nhau, vì vậy chúng tôi đang loại bỏ chính xác tất cả các cặp không hợp lệ khỏi tập hợp đầy đủ các cặp. 

### Tại sao nó hoạt động 

Mỗi cặp đường thẳng rơi vào đúng một trong hai loại: hệ số góc của chúng khác nhau hoặc hệ số góc của chúng bằng nhau. Nếu các hệ số góc khác nhau thì các đường thẳng cắt nhau đúng một lần. Nếu hệ số góc bằng nhau thì các đường thẳng song song và không bao giờ cắt nhau, bất kể điểm giao nhau, kể cả trường hợp suy biến của các đường giống hệt nhau. Do đó, việc đếm tất cả các cặp và trừ các cặp có cùng độ dốc sẽ tách biệt chính xác các cặp giao nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    n = int(input())
    cnt = defaultdict(int)

    for _ in range(n):
        a, b = map(int, input().split())
        cnt[a] += 1

    total = n * (n - 1) // 2
    bad = 0

    for k in cnt.values():
        bad += k * (k - 1) // 2

    print(total - bad)

if __name__ == "__main__":
    solve()
```Mã này chỉ nhóm các đường theo độ dốc, điều này là đủ vì điểm giao nhau không ảnh hưởng đến sự tồn tại của nút giao. Tổng số cặp được tính một lần và sau đó mỗi nhóm độ dốc đóng góp một thuật ngữ hiệu chỉnh loại bỏ tất cả các cặp song song. 

Một lỗi phổ biến là cố gắng kiểm tra tất cả các cặp một cách rõ ràng, điều này là không cần thiết. Một sai lầm khác là xử lý các đường giống hệt nhau một cách riêng biệt với các đường song song, nhưng cả hai đều được xử lý một cách tự nhiên trong cùng một nhóm độ dốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1
2 2
3 3
```| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | Đếm độ dốc | {1:1, 2:1, 3:1} | 
| 2 | Tổng số cặp | 3 | 
| 3 | Trừ các cặp có cùng độ dốc | 0 | 
| 4 | Trả lời | 3 | 

Mỗi cặp có hệ số góc khác nhau nên mỗi cặp giao nhau đúng một lần. 

### Ví dụ 2 

đầu vào:```
4
1 0
1 1
2 0
2 1
```| Bước | Hành động | Tiểu bang | 
| --- | --- | --- | 
| 1 | Đếm độ dốc | {1:2, 2:2} | 
| 2 | Tổng số cặp | 6 | 
| 3 | Trừ các cặp có cùng độ dốc | 1 + 1 = 2 | 
| 4 | Trả lời | 4 | 

Trong mỗi nhóm độ dốc, các đường thẳng song song và không giao nhau, do đó chỉ các cặp nhóm chéo vẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt đếm độ dốc và một lượt vượt nhóm | 
| Không gian | O(n) | Lưu trữ bản đồ tần số sườn dốc | 

Thuật toán tuyến tính theo số dòng, dễ dàng hiệu quả đối với$n \le 5000$, và trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n = int(sys.stdin.readline())
    cnt = defaultdict(int)

    for _ in range(n):
        a, b = map(int, sys.stdin.readline().split())
        cnt[a] += 1

    total = n * (n - 1) // 2
    bad = 0
    for k in cnt.values():
        bad += k * (k - 1) // 2

    return str(total - bad)

# provided samples (illustrative, as exact samples were not fully specified)
assert run("3\n1 1\n2 2\n3 3\n") == "3"
assert run("4\n1 0\n1 1\n2 0\n2 1\n") == "4"

# custom cases
assert run("1\n5 7\n") == "0", "single line"
assert run("3\n1 1\n1 1\n1 1\n") == "0", "all identical lines"
assert run("3\n1 0\n1 1\n1 2\n") == "0", "all parallel lines"
assert run("3\n1 0\n2 0\n3 0\n") == "3", "all intersecting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 dòng | 0 | trường hợp tối thiểu | 
| các độ dốc giống nhau đều bằng nhau | 0 | xử lý trùng lặp và chồng chéo | 
| cùng một độ dốc chặn khác nhau | 0 | tính đúng đắn song song | 
| sườn dốc khác biệt | 3 | trường hợp giao nhau đầy đủ | 

## Vỏ cạnh 

Đối với một dòng duy nhất như:```
1
10 20
```bản đồ độ dốc chỉ chứa một mục có số 1. Tổng số cặp là 0 và không xảy ra phép trừ nên kết quả chính xác là 0. 

Đối với các dòng hoàn toàn giống hệt nhau:```
3
1 1
1 1
1 1
```nhóm độ dốc là {1:3}. Tổng số cặp là 3, và các cặp xấu trong nhóm cũng là 3, cho câu trả lời cuối cùng là 0. Điều này tránh tính chính xác các đường chồng chéo là giao nhau. 

Đối với nhiều đường thẳng song song nhưng khác biệt:```
4
2 0
2 1
2 2
2 3
```nhóm độ dốc là {2:4}. Tổng số cặp là 6, cặp xấu là 6, đáp án cuối cùng là 0, phản ánh đúng rằng không tồn tại giao điểm nào giữa các đường thẳng song song.
