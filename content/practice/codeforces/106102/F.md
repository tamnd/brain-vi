---
title: "CF 106102F - Roulette chỗ ngồi trên máy bay"
description: "Hành khách lần lượt lên máy bay. Mỗi hành khách có một hàng cố định và một chỗ ngồi cố định ở hàng đó. Trong một hàng, ghế số 1 cạnh cửa sổ và ghế m cạnh lối đi. Hành khách luôn đi vào từ phía lối đi và đi về phía chỗ ngồi được chỉ định của họ."
date: "2026-06-25T11:49:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106102
codeforces_index: "F"
codeforces_contest_name: "AGM 2025, Final Round, Day 1"
rating: 0
weight: 106102
solve_time_s: 44
verified: true
draft: false
---

[CF 106102F - Roulette chỗ ngồi trên máy bay](https://codeforces.com/problemset/problem/106102/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hành khách lần lượt lên máy bay. Mỗi hành khách có một hàng cố định và một chỗ ngồi cố định ở hàng đó. 

Trong một hàng, chỗ ngồi`1`ở cạnh cửa sổ và chỗ ngồi`m`nằm cạnh lối đi. Hành khách luôn đi vào từ phía lối đi và đi về phía chỗ ngồi được chỉ định của họ. 

Giả sử một hành khách muốn có chỗ ngồi`s`. Trong khi đi từ lối đi về phía chỗ ngồi đó, họ vượt qua tất cả những chỗ ngồi có số lượng lớn hơn`s`. Nếu một số ghế đã có người ngồi, hành khách ngồi phải đứng dậy để người mới đi qua. Mỗi hành động thường trực đều góp phần`1`để trả lời. 

Nhiệm vụ là tính tổng số lần hành khách đứng lên trong toàn bộ quá trình lên máy bay. 

Số lượng hàng ghế có thể lớn tới`10^9`, nhưng chỉ`k ≤ 10^5`hành khách thực sự có mặt. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào phụ thuộc vào tất cả các hàng hoặc tất cả số ghế đều không thể thực hiện được. Dữ liệu liên quan duy nhất là số ghế đã có người xuất hiện trong đầu vào. 

Một điểm tinh tế là sự tương tác không bao giờ xảy ra giữa các hàng khác nhau. Một hành khách đi đến chỗ ngồi theo hàng`r`chỉ ảnh hưởng đến những người đã ngồi cùng hàng. 

Hãy xem xét một hàng có hành khách đến theo thứ tự sau:```
seat 5
seat 2
```Khi hành khách thứ hai đến, ngồi`5`đã có người ngồi và nằm giữa lối đi và chỗ ngồi`2`, vậy hành khách ngồi ở ghế`5`đứng một lần. Sự đóng góp là`1`. 

Bây giờ hãy xem xét:```
seat 2
seat 5
```Hành khách thứ hai chỉ đi tới chỗ ngồi`5`, nằm cạnh lối đi. Không ai cản đường. Sự đóng góp là`0`. 

Việc thực hiện bất cẩn có thể tính số ghế đã sử dụng có số ghế nhỏ hơn thay vì số ghế lớn hơn và thu được kết quả ngược lại. 

Một sai lầm dễ mắc phải khác là trộn lẫn hành khách từ các hàng ghế khác nhau. 

Ví dụ:```
row 1 seat 5
row 2 seat 4
row 1 seat 2
```Chỉ có hành khách ở hàng ghế`1`, ghế`5`vấn đề cho lần đến cuối cùng. Hành khách xếp hàng`2`không liên quan vì các hàng độc lập. 

Câu trả lời đúng là`1`, không`2`. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp rất đơn giản. 

Đối với mỗi hành khách đến, hãy quét tất cả hành khách đã ngồi trước đó trong cùng một hàng và đếm xem có bao nhiêu ghế có số ghế lớn hơn. Mỗi hành khách như vậy phải đứng một lần. 

Điều này đúng vì hành khách ngồi chặn lối đi chính xác khi ghế của họ nằm giữa lối đi và ghế đích. 

Vấn đề là thời gian chạy. Với`k = 100000`, việc kiểm tra tất cả hành khách trước đó cho mỗi lần đến đòi hỏi khoảng```
1 + 2 + ... + 99999 ≈ 5 × 10^9
```so sánh trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là, trong một hàng cố định, chúng ta đang đếm các nghịch đảo. 

Giả sử thứ tự đến của các ghế trong một hàng là```
a1, a2, a3, ...
```Khi hành khách`ai`đến, mỗi chỗ ngồi trước đó lớn hơn`ai`gây ra một sự kiện đứng lên. 

Vậy phần đóng góp của hàng này chính xác là số cặp```
i < j and ai > aj
```đó là số lượng đảo ngược cổ điển. 

Khi vấn đề được giảm xuống bằng cách đếm nghịch đảo, chúng ta có thể xử lý từng hàng một cách độc lập bằng cây Fenwick. Vì số chỗ ngồi có thể lớn bằng`10^9`, chúng tôi nén số ghế thực sự xuất hiện trong hàng. 

Tổng số hành khách trên tất cả các hàng nhiều nhất là`100000`, do đó độ phức tạp tổng thể trở thành`O(k log k)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k²) | O(k) | Quá chậm | 
| Tối ưu | O(k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả hành khách theo thứ tự lên máy bay. 
2. Nhóm số ghế theo hàng trong khi vẫn giữ nguyên thứ tự đến trong mỗi hàng. 
3. Đối với mỗi hàng, thu thập tất cả số ghế xuất hiện trong hàng đó và sắp xếp chúng để xây dựng ánh xạ nén tọa độ. 
4. Tạo cây Fenwick có kích thước bằng số ghế riêng biệt trong hàng đó. 
5. Xử lý hành khách của hàng đó theo thứ tự đến. 
6. Đối với chỗ ngồi hiện tại`s`, tìm vị trí nén của nó`p`. 
7. Hãy để`seen`là số lượng hành khách đã được xử lý ở hàng này. 
8. Truy vấn cây Fenwick xem có bao nhiêu chỗ ngồi được xử lý có chỉ số nén`≤ p`. 
9. Số ghế trước đó lớn hơn nhiều so với`s`là```
seen - count(≤ s)
```Thêm giá trị này vào câu trả lời. 
10. Chèn chỗ ngồi`s`vào cây Fenwick và tiếp tục. 
11. Tính tổng các phần đóng góp từ tất cả các hàng và in kết quả. 

### Tại sao nó hoạt động 

Đối với hành khách được chỉ định chỗ ngồi`s`, những người duy nhất phải đứng là những hành khách đã ngồi ở ghế có số lượng lớn hơn trong cùng một hàng. Đó chính xác là những chỗ ngồi nằm giữa lối đi và`s`. 

Khi xử lý một hàng theo thứ tự đến, mỗi sự kiện đứng lên tương ứng với một cặp hành khách trong đó hành khách trước ngồi ở số ghế lớn hơn hành khách sau. Những cặp như vậy chính xác là sự đảo ngược của dãy số ghế của hàng đó. 

Cây Fenwick duy trì tất cả các chỗ ngồi đã được xử lý trước đó. Ở mỗi bước, nó sẽ đếm xem có bao nhiêu ghế trước đó lớn hơn ghế hiện tại, chính xác là số lần đứng chờ do hành khách này gây ra. Tổng các giá trị này trên tất cả các hàng sẽ cho ra tổng số cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        while idx <= self.n:
            self.bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        while idx > 0:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

def solve():
    n, m, k = map(int, input().split())

    rows = {}

    for _ in range(k):
        r, s = map(int, input().split())
        rows.setdefault(r, []).append(s)

    answer = 0

    for seats in rows.values():
        vals = sorted(set(seats))
        comp = {v: i + 1 for i, v in enumerate(vals)}

        fw = Fenwick(len(vals))
        seen = 0

        for seat in seats:
            pos = comp[seat]

            not_greater = fw.sum(pos)
            greater = seen - not_greater

            answer += greater

            fw.add(pos, 1)
            seen += 1

    print(answer)

solve()
```Từ điển`rows`lưu trữ, đối với mỗi hàng, chuỗi số ghế theo thứ tự lên máy bay. Việc duy trì trật tự là cần thiết vì sự đảo ngược phụ thuộc vào thứ tự đến. 

Đối với mỗi hàng, số ghế được nén theo tọa độ. Số ghế ban đầu có thể lớn bằng`10^9`, nhưng chỉ thứ tự tương đối mới quan trọng đối với việc đếm nghịch đảo. 

Cây Fenwick lưu trữ tần số của các ghế đã được xử lý trước đó. Khi xử lý chỗ ngồi,`fw.sum(pos)`đưa ra số ghế trước đó có số ghế nhỏ hơn hoặc bằng số ghế hiện tại. Trừ cái này khỏi`seen`để lại chính xác số ghế trước đó lớn hơn. 

Câu trả lời có thể đạt được một cách đại khái```
100000 × 99999 / 2
```vì vậy nó phải được lưu trữ ở dạng số nguyên có khả năng 64 bit. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 5 3
1 5
1 2
1 4
```Thứ tự chỗ ngồi ở hàng duy nhất:```
[5, 2, 4]
```| Hành khách | Chỗ ngồi | Đã từng thấy | Ghế lớn hơn trước đó | Đã thêm | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 0 | 0 | 
| 2 | 2 | 1 | 1 | 1 | 
| 3 | 4 | 2 | 1 | 1 | 

Tổng số câu trả lời:```
2
```Hành khách thứ hai bị chặn chỗ ngồi`5`. Hành khách thứ ba cũng bị chặn chỗ ngồi`5`, nhưng không theo chỗ ngồi`2`. 

### Ví dụ 2 

đầu vào:```
2 10 5
1 8
2 7
1 3
2 2
1 5
```Trình tự hàng 1:```
[8, 3, 5]
```Trình tự hàng 2:```
[7, 2]
```Đối với hàng 1: 

| Hành khách | Chỗ ngồi | Ghế lớn hơn trước đó | 
| --- | --- | --- | 
| 1 | 8 | 0 | 
| 2 | 3 | 1 | 
| 3 | 5 | 1 | 

Đóng góp = 2. 

Đối với hàng 2: 

| Hành khách | Chỗ ngồi | Ghế lớn hơn trước đó | 
| --- | --- | --- | 
| 1 | 7 | 0 | 
| 2 | 2 | 1 | 

Đóng góp = 1. 

Câu trả lời cuối cùng:```
3
```Ví dụ này chứng minh rằng các hàng được xử lý độc lập và số lần đảo ngược của chúng được cộng lại với nhau một cách đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k) | Mỗi hành khách tham gia vào một bản cập nhật Fenwick và một truy vấn | 
| Không gian | O(k) | Dữ liệu hành khách được lưu trữ, mảng nén và cây Fenwick | 

Từ`k ≤ 100000`,`O(k log k)`đủ nhanh một cách dễ dàng. Việc sử dụng bộ nhớ là tuyến tính theo số lượng hành khách và vẫn ở mức thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, val):
            while idx <= self.n:
                self.bit[idx] += val
                idx += idx & -idx

        def sum(self, idx):
            res = 0
            while idx:
                res += self.bit[idx]
                idx -= idx & -idx
            return res

    n, m, k = map(int, input().split())

    rows = {}
    for _ in range(k):
        r, s = map(int, input().split())
        rows.setdefault(r, []).append(s)

    ans = 0

    for seats in rows.values():
        vals = sorted(set(seats))
        comp = {v: i + 1 for i, v in enumerate(vals)}

        fw = Fenwick(len(vals))
        seen = 0

        for seat in seats:
            p = comp[seat]
            ans += seen - fw.sum(p)
            fw.add(p, 1)
            seen += 1

    return str(ans) + "\n"

assert run("1 5 1\n1 3\n") == "0\n", "single passenger"

assert run(
    "1 5 3\n"
    "1 5\n"
    "1 2\n"
    "1 4\n"
) == "2\n", "basic inversion counting"

assert run(
    "2 10 5\n"
    "1 8\n"
    "2 7\n"
    "1 3\n"
    "2 2\n"
    "1 5\n"
) == "3\n", "multiple rows"

assert run(
    "1 5 5\n"
    "1 1\n"
    "1 2\n"
    "1 3\n"
    "1 4\n"
    "1 5\n"
) == "0\n", "already increasing"

assert run(
    "1 5 5\n"
    "1 5\n"
    "1 4\n"
    "1 3\n"
    "1 2\n"
    "1 1\n"
) == "10\n", "maximum inversions for five elements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một hành khách | 0 | Kích thước tối thiểu | 
|`[5,2,4]`trong một hàng | 2 | Logic đảo ngược cơ bản | 
| Nhiều hàng | 3 | Hàng độc lập | 
| Tăng chỗ ngồi | 0 | Không có sự kiện độc lập | 
| Giảm chỗ ngồi | 10 | Số lần đảo ngược tối đa | 

## Vỏ cạnh 

### Hành khách ở các hàng ghế khác nhau không được tương tác 

đầu vào:```
2 10 3
1 5
2 4
1 2
```Dãy hàng 1 là`[5, 2]`, tạo ra một sự đảo ngược. 

Trình tự hàng 2 là`[4]`, tạo ra sự đảo ngược bằng không. 

Thuật toán lưu trữ các hàng riêng biệt nên hành khách ở hàng 2 không bao giờ ảnh hưởng đến hàng 1. Câu trả lời cuối cùng là:```
1
```### Số ghế lớn hơn sẽ chặn số ghế nhỏ hơn 

đầu vào:```
1 10 2
1 2
1 8
```Sự liên tiếp:```
[2, 8]
```Không có sự đảo ngược vì`2 < 8`. 

Hành khách thứ hai ngồi gần lối đi và không vượt qua ghế`2`. Truy vấn Fenwick trả về số ghế lớn hơn trước đó bằng 0, vì vậy câu trả lời là:```
0
```### Nhiều bộ chặn cho một hành khách 

đầu vào:```
1 10 3
1 9
1 7
1 2
```Khi ngồi`2`đến nơi, cả hai ghế`9`Và`7`đang có người ngồi và nằm giữa lối đi và chỗ ngồi`2`. 

Các cặp nghịch đảo là:```
(9, 7)
(9, 2)
(7, 2)
```Tổng số câu trả lời:```
3
```Thuật toán đếm cả ba lần đảo ngược, khớp chính xác với số lượng sự kiện chờ.
