---
title: "CF 125C - Bữa tiệc của người Hobbit"
description: "Chúng tôi có n người hobbit và muốn tạo ra càng nhiều ngày tiệc tùng càng tốt. Mỗi ngày tương ứng với một danh sách khách mời, là một tập hợp con không trống của các hobbit. Danh sách khách mời phải thỏa mãn hai điều kiện."
date: "2026-06-02T16:16:14+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 125
codeforces_index: "C"
codeforces_contest_name: "Codeforces Testing Round 2"
rating: 1600
weight: 125
solve_time_s: 137
verified: true
draft: false
---

[CF 125C - Bữa tiệc của người Hobbit](https://codeforces.com/problemset/problem/125/C) 

**Đánh giá:** 1600 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`hobbit và muốn tạo ra càng nhiều ngày tiệc tùng càng tốt. 

Mỗi ngày tương ứng với một danh sách khách mời, là một tập hợp con không trống của các hobbit. 

Danh sách khách mời phải thỏa mãn hai điều kiện. 

Đối với mỗi cặp ngày khác nhau, phải có ít nhất một hobbit được mời tham gia cả hai ngày. Theo thuật ngữ cố định, cứ hai danh sách khách mời phải giao nhau. 

Đối với mỗi ba ngày khác nhau, không được có một hobbit nào được mời đến cả ba ngày. Một hobbit có thể xuất hiện trong một hoặc hai ngày, nhưng không bao giờ xuất hiện trong ba ngày trở lên. 

Đầu vào chỉ chứa`n`, số lượng người hobbit có sẵn. Chúng ta phải tối đa hóa số ngày`k`, sau đó xuất ra một danh sách khách hợp lệ cho mỗi ngày. 

Ràng buộc`n ≤ 10000`đủ nhỏ để kích thước đầu ra trở thành giới hạn thực sự. Bất kỳ công trình xây dựng nào được chấp nhận đều phải sản xuất tổng cộng tối đa khoảng mười nghìn hobbit. Một giải pháp với`O(n²)`trí nhớ sẽ là không thể bởi vì`10000² = 10^8`các phần tử. Một giải pháp gần như`O(n)`công việc và đầu ra hoàn toàn ổn. 

Phần khó khăn là vấn đề yêu cầu số ngày tối đa có thể chứ không chỉ đơn thuần là một lịch trình hợp lệ. 

Coi như`n = 3`. Một công trình bất cẩn có thể chỉ tạo ra hai ngày:```
1 2
2 3
```Điều này thỏa mãn các điều kiện, nhưng nó không phải là tối đa. Câu trả lời đúng có ba ngày:```
1 2
1 3
2 3
```Một sai lầm dễ mắc phải khác là cho phép một hobbit xuất hiện sau ba ngày. Ví dụ:```
Day 1: {1,2}
Day 2: {1,3}
Day 3: {1,4}
```Mỗi cặp ngày giao nhau, nhưng hobbit`1`thuộc về cả ba ngày, vi phạm điều kiện thứ hai. 

Một cạm bẫy khác là quên rằng mỗi cặp ngày đều phải giao nhau. Ví dụ:```
Day 1: {1,2}
Day 2: {2,3}
Day 3: {3,4}
```Ngày 1 và 3 rời rạc nên việc xây dựng không hợp lệ. 

Thách thức là phải hiểu giới hạn trên mạnh nhất có thể về số ngày và sau đó xây dựng lịch trình để đạt được giới hạn đó. 

## Phương pháp tiếp cận 

Tư duy bạo lực bắt đầu bằng cách xem mỗi ngày như một tập hợp con tùy tiện của người Hobbit. Chúng ta có thể thử các tập hợp con khác nhau và kiểm tra hai điều kiện. Việc xác minh một ứng cử viên yêu cầu kiểm tra tất cả các cặp và bộ ba ngày. Ngay cả khi bằng cách nào đó chúng ta giới hạn bản thân chỉ trong vài trăm ngày thì không gian tìm kiếm của các tập hợp con vẫn theo cấp số nhân`n`, khiến điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng đến từ việc quan sát một người hobbit. 

Bởi vì không người hobbit nào có thể tham dự ba ngày khác nhau nên mỗi người hobbit có thể tham gia nhiều nhất là hai ngày. 

Giả sử một hobbit thuộc về chính xác hai ngày, chẳng hạn như ngày`A`Và`B`. Khi đó người hobbit đó có thể được coi là đại diện cho cặp đôi`(A,B)`. 

Bây giờ hãy xem xét điều kiện đầu tiên. Mỗi cặp ngày đều phải có một người hobbit chung. Vì một hobbit không thể thuộc về ba ngày, nên một hobbit chỉ có thể chứng kiến ​​sự giao nhau của một cặp ngày. Nó không thể đồng thời đóng vai trò là khách chung cho hai cặp khác nhau. 

Nếu có`k`ngày, số cặp ngày là$$\binom{k}{2}.$$Mỗi cặp cần ít nhất một hobbit chuyên dụng. Vì chỉ có`n`người hobbit,$$\binom{k}{2} \le n.$$Điều này ngay lập tức đưa ra giới hạn trên cho`k`. 

Câu hỏi tiếp theo là liệu giới hạn này có thể đạt được hay không. 

Đúng vậy. Lấy`k`ngày. Cho mỗi cặp ngày`(i,j)`, hãy tạo một người hobbit được mời chính xác vào hai ngày đó. Sau đó, mỗi cặp ngày giao nhau vì người hobbit đó, và không có người hobbit nào thuộc về ba ngày vì mỗi ngày được gán cho đúng một cặp. 

Công trình này sử dụng chính xác$$\binom{k}{2}$$hobbit. 

Nếu như`n`lớn hơn, những hobbit còn lại không được sử dụng. Tuyên bố chỉ yêu cầu danh sách khách mời phải là tập hợp con không trống của cư dân, chứ không phải mọi hobbit đều phải xuất hiện. 

Do đó toàn bộ bài toán quy về việc tìm số nguyên lớn nhất`k`thỏa mãn$$\binom{k}{2} \le n.$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng tối ưu | O(k²) | O(k²) | Đã chấp nhận | 

Từ`k`đại khái là`√(2n)`, vì`n = 10000`chúng tôi nhận được`k ≤ 141`, làm`k²`nhỏ xíu. 

## Hướng dẫn thuật toán 

1. Tìm số nguyên lớn nhất`k`như vậy`k(k-1)/2 ≤ n`. 
2. Tạo`k`danh sách khách trống. 
3. Số hobbit bắt đầu từ`1`. 
4. Cho mỗi cặp ngày`(i,j)`với`i < j`, tạo một hobbit mới và thêm nó vào cả hai ngày`i`và ngày`j`. 

Người hobbit này trở thành nhân chứng duy nhất cho thấy hai ngày đó giao nhau. 
5. Sau khi xử lý chính xác tất cả các cặp`k(k-1)/2`hobbit đã được sử dụng. 
6. Đầu ra`k`. 
7. Xuất ra từng danh sách khách mời. 

### Tại sao nó hoạt động 

có`k(k-1)/2`cặp ngày. Việc xây dựng chỉ định một hobbit riêng biệt cho mỗi cặp. 

Mất hai ngày khác nhau. Cặp tương ứng của họ nhận được một hobbit xuất hiện vào đúng hai ngày đó, vì vậy các ngày giao nhau. 

Hãy lấy bất kỳ người hobbit nào. Nó được tạo cho một cặp ngày cụ thể và được chèn vào chính xác hai danh sách khách mời đó. Do đó không có người hobbit nào xuất hiện trong ba ngày khác nhau. 

Việc xây dựng sử dụng chính xác`k(k-1)/2`hobbit. Từ`k`được chọn là giá trị lớn nhất thỏa mãn`k(k-1)/2 ≤ n`, bất kỳ lịch trình nào có nhiều hơn`k`ngày sẽ cần nhiều hơn`n`hobbit, mâu thuẫn với lập luận đếm. Như vậy số ngày là tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    k = 1
    while (k + 1) * k // 2 <= n:
        k += 1

    groups = [[] for _ in range(k)]

    cur = 1
    for i in range(k):
        for j in range(i + 1, k):
            groups[i].append(cur)
            groups[j].append(cur)
            cur += 1

    print(k)
    for g in groups:
        print(*g)

solve()
```The first loop finds the largest feasible number of days. Từ`k`không bao giờ vượt quá khoảng 141, ngay cả một tìm kiếm tuyến tính đơn giản cũng đủ. 

Mảng`groups[i]`lưu trữ danh sách khách mời trong ngày`i`. 

Các vòng lặp lồng nhau lặp lại trong mỗi cặp ngày không có thứ tự. Đối với mỗi cặp, một mã định danh hobbit mới sẽ được tạo và chèn vào cả hai danh sách khách mời tương ứng. Không có mã định danh nào được sử dụng lại. 

Một sai lầm phổ biến là tiếp tục phân công người Hobbit cho đến khi đạt được chính xác`n`. Điều đó là không cần thiết. Việc chứng minh chỉ yêu cầu nhiều nhất`n`hobbit. Người hobbit chưa sử dụng được phép. 

Một điểm tinh tế khác là lập chỉ mục. Ngày được lưu trữ với các chỉ số dựa trên số 0 trong chương trình, nhưng nhãn hobbit bắt đầu từ`1`bởi vì tuyên bố số lượng cư dân từ`1`ĐẾN`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```lớn nhất`k`thỏa mãn`k(k-1)/2 ≤ 4`là`3`. 

| Cặp ngày | Hobbit được giao | 
| --- | --- | 
| (1,2) | 1 | 
| (1,3) | 2 | 
| (2,3) | 3 | 

Kết quả danh sách khách mời: 

| Ngày | Khách | 
| --- | --- | 
| 1 | 1 2 | 
| 2 | 1 3 | 
| 3 | 2 3 | 

Đầu ra:```
3
1 2
1 3
2 3
```Mỗi cặp ngày có chính xác một hobbit và không có hobbit nào xuất hiện trong ba ngày. 

### Ví dụ 2 

đầu vào:```
6
```Giá trị hợp lệ lớn nhất là`k = 4`bởi vì:$$\binom{4}{2}=6.$$| Cặp ngày | Hobbit được giao | 
| --- | --- | 
| (1,2) | 1 | 
| (1,3) | 2 | 
| (1,4) | 3 | 
| (2,3) | 4 | 
| (2,4) | 5 | 
| (3,4) | 6 | 

Danh sách khách trở thành: 

| Ngày | Khách | 
| --- | --- | 
| 1 | 1 2 3 | 
| 2 | 1 4 5 | 
| 3 | 2 4 6 | 
| 4 | 3 5 6 | 

Dấu vết này cho thấy sự bất biến trung tâm: mỗi cặp ngày nhận được một hobbit chung duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k²) | Một lần lặp lại cho mỗi cặp ngày | 
| Không gian | O(k²) | Lưu trữ tất cả các hobbit được chỉ định trong danh sách khách | 
| Kích thước đầu ra | O(k²) | Chính xác`k(k-1)`in lần xuất hiện của khách | 

Từ`k`là số nguyên lớn nhất với`k(k-1)/2 ≤ n`, chúng tôi có`k = O(√n)`. Vì`n = 10000`,`k ≤ 141`, do đó có ít hơn hai mươi nghìn lượt xuất hiện của khách được lưu trữ và in. Điều này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())

    k = 1
    while (k + 1) * k // 2 <= n:
        k += 1

    groups = [[] for _ in range(k)]

    cur = 1
    for i in range(k):
        for j in range(i + 1, k):
            groups[i].append(cur)
            groups[j].append(cur)
            cur += 1

    out = [str(k)]
    for g in groups:
        out.append(" ".join(map(str, g)))

    return "\n".join(out)

# sample
assert run("4\n").splitlines()[0] == "3"

# minimum n
assert run("3\n").splitlines()[0] == "3"

# exact triangular number
assert run("6\n").splitlines()[0] == "4"

# just below next triangular number
assert run("9\n").splitlines()[0] == "4"

# maximum constraint
assert run("10000\n").splitlines()[0] == "141"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`| dòng đầu tiên`3`| Đầu vào tối thiểu được phép | 
|`6`| dòng đầu tiên`4`| Số tam giác chính xác | 
|`9`| dòng đầu tiên`4`| Khả thi lớn nhất`k`được chọn đúng | 
|`10000`| dòng đầu tiên`141`| Xử lý ràng buộc trên | 

## Vỏ cạnh 

Hãy xem xét đầu vào tối thiểu:```
3
```Thuật toán tìm`k = 3`bởi vì`3·2/2 = 3`. Ba cặp ngày nhận hobbit`1`,`2`, Và`3`. Mỗi ngày có đúng hai khách. Việc xây dựng vẫn có hiệu lực ngay cả ở ranh giới nhỏ nhất. 

Coi như:```
5
```Chúng tôi vẫn nhận được`k = 3`bởi vì`4·3/2 = 6 > 5`. Thuật toán chỉ sử dụng ba hobbit mặc dù có năm người. Điều này chứng tỏ rằng những người hobbit không sử dụng là vô hại. Vấn đề không bao giờ đòi hỏi mọi cư dân phải xuất hiện. 

Xét một giá trị chính xác bằng một số tam giác:```
10
```Thuật toán tìm`k = 5`bởi vì`5·4/2 = 10`. Mỗi hobbit có sẵn đều được sử dụng, một chiếc cho mỗi cặp ngày. Đây là trường hợp chặt chẽ nhất có thể đạt được chính xác giới hạn đếm trên. 

Hãy xem xét một giá trị ngay dưới số tam giác tiếp theo:```
9
```Một lỗi thường gặp sẽ chọn sai`k = 5`, nhưng điều đó sẽ cần đến mười hobbit làm chứng. Thuật toán giữ đúng`k = 4`bởi vì`10 > 9`. Bằng chứng tối đa đảm bảo rằng năm ngày là không thể.
