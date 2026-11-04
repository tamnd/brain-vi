---
title: "CF 1137B - Lịch cắm trại"
description: "Chúng tôi được cung cấp hai chuỗi nhị phân. Chuỗi đầu tiên s không phải là lịch trình chúng ta phải xuất trực tiếp. Thay vào đó, nó hoạt động như một tập hợp nhiều ký tự."
date: "2026-06-12T03:57:18+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "hashing", "strings"]
categories: ["algorithms"]
codeforces_contest: 1137
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 545 (Div. 1)"
rating: 1600
weight: 1137
solve_time_s: 111
verified: true
draft: false
---

[CF 1137B - Lịch cắm trại](https://codeforces.com/problemset/problem/1137/B) 

**Đánh giá:** 1600 
**Tags:** tham lam, băm, chuỗi 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi nhị phân. 

Chuỗi đầu tiên`s`không phải là lịch trình chúng ta phải xuất ra trực tiếp. Thay vào đó, nó hoạt động như một tập hợp nhiều ký tự. Chúng ta được phép sắp xếp lại các ký hiệu của nó theo thứ tự bất kỳ, nhưng tổng số`'0'`nhân vật và`'1'`các ký tự phải giữ nguyên như cũ. 

Chuỗi thứ hai`t`là một mẫu mục tiêu. Trong số tất cả các sự sắp xếp lại có thể có của`s`, chúng tôi muốn một cái có số lần tối đa`t`xuất hiện dưới dạng một chuỗi con. 

Đầu ra là bất kỳ sự sắp xếp lại nào của`s`đạt được mức tối đa này. 

Hạn chế chính là chỉ có thứ tự mới có thể thay đổi. Nếu như`s`chứa 100 số 0 và 200 số đơn vị thì đáp án cũng phải chứa đúng 100 số 0 và 200 số đơn vị. 

Độ dài của cả hai dây có thể đạt tới 500.000. Ở quy mô này, các thuật toán bậc hai là hoàn toàn không thể. Thậm chí một$O(n^2)$quét sẽ yêu cầu khoảng$2.5 \times 10^{11}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một giải pháp về cơ bản là tuyến tính trong tổng kích thước đầu vào. 

Khó khăn chính là việc tối đa hóa số lần xuất hiện của chuỗi con không giống như việc tham lam đặt các bản sao của`t`một cách độc lập. Các lần xuất hiện khác nhau có thể trùng lặp. Trên thực tế, sự chồng chéo chính xác là thứ cho phép chúng ta ghép nhiều lần xuất hiện hơn vào một lượng ký tự cố định. 

Coi như:```
t = 10101
```Lần xuất hiện tiếp theo có thể bắt đầu trước khi lần xuất hiện trước đó kết thúc hoàn toàn vì hậu tố`"101"`cũng là tiền tố. Việc bỏ qua sự trùng lặp sẽ gây lãng phí ký tự và tạo ra ít lần xuất hiện hơn. 

Một lỗi dễ mắc phải khác là dừng lại sau khi xây dựng một bản sao của`t`. 

Ví dụ:```
s = 11110000
t = 1100
```Một giải pháp ngây thơ có thể đặt một bản sao:```
1100 + remaining
```Nhưng nếu nguồn lực cho phép, chúng ta nên tiếp tục thêm các bản sao chồng chéo bất cứ khi nào có thể. 

Trường hợp cạnh thứ ba xảy ra khi chúng ta không thể xây dựng dù chỉ một bản sao của`t`. 

Ví dụ:```
s = 1111
t = 000
```Vì không có sẵn số 0 nên mọi sắp xếp đều không có lần xuất hiện nào. Câu trả lời đúng đơn giản là bất kỳ chuỗi nào sử dụng tất cả các ký tự của`s`, chẳng hạn như:```
1111
```Đang cố gắng sao chép một phần của`t`sẽ vi phạm số lượng ký tự. 

## Phương pháp tiếp cận 

Một quan điểm brute-force là xem xét mọi hoán vị của các ký tự của`s`, đếm xem bao nhiêu lần`t`xuất hiện và chọn cái tốt nhất. 

Điều này rõ ràng là đúng vì nó kiểm tra mọi lịch trình hợp lệ. Thật không may, nếu`|s| = 500000`, số lượng hoán vị là vô cùng lớn và hoàn toàn không thể thực hiện được. 

Một ý tưởng mạnh mẽ thực tế hơn là liên tục thêm một bản sao đầy đủ của`t`bất cứ khi nào vẫn còn đủ số không và số một. Điều này tối đa hóa số lần xuất hiện không chồng chéo. 

Lỗ hổng là các lần xuất hiện được phép chồng chéo lên nhau. Giả định:```
t = 10101
```Hậu tố`"101"`bằng tiền tố`"101"`. 

Sau khi viết một lần xuất hiện:```
10101
```chúng ta chỉ cần nối thêm`"01"`để tạo ra một lần xuất hiện khác:```
1010101
```Sự chồng chéo lưu ba ký tự. Vì nguồn cung cấp số 0 và số 1 của chúng tôi bị hạn chế nên việc lưu ký tự đồng nghĩa với việc phù hợp với nhiều lần xuất hiện hơn. 

Quan sát này chỉ trực tiếp đến chức năng tiền tố hoặc tư duy KMP. Nếu chúng ta biết tiền tố thích hợp dài nhất của`t`đó cũng là hậu tố thì sau khi viết 1 lần ta không cần phải viết lại từ đầu. Chúng ta chỉ có thể nối thêm phần đuôi không chồng chéo. 

Cho phép```
k = longest border length of t
```Sau đó:```
tail = t[k:]
```Sau bản sao đầu tiên của`t`, mọi lần xuất hiện bổ sung có thể được tạo bằng cách nối thêm`tail`. 

Vì mỗi đuôi được nối thêm sẽ tạo ra chính xác một lần xuất hiện mới trong khi tiêu thụ số lượng ký tự tối thiểu có thể, nên việc nối thêm đuôi nhiều lần luôn là tối ưu. 

Thuật toán trở thành: 

1. Đếm các số 0 và số 1 có sẵn từ`s`. 
2. Kiểm tra xem một bản sao của`t`có thể được hình thành. 
3. Nếu không, xuất ra tất cả các ký tự tùy ý. 
4. Nếu không thì đặt một bản sao của`t`. 
5. Tính đường viền dài nhất của`t`sử dụng KMP. 
6. Liên tục nối phần đuôi không chồng lên nhau trong khi số lượng ký tự cho phép. 
7. Nối tất cả các ký tự không sử dụng còn lại. 

Mọi thứ đều tuyến tính vì KMP tính toán đường viền theo$O(|t|)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O( | s | + | 

## Hướng dẫn thuật toán 

1. Đếm số số 0 và số 1 có trong`s`. 
2. Đếm số số 0 và số 1 theo yêu cầu của một bản sao đầy đủ`t`. 
3. Nếu`s`không chứa đủ số không hoặc số một để xây dựng dù chỉ một bản sao của`t`, không có sự kiện nào có thể tồn tại được. Xuất tất cả các ký tự còn lại theo thứ tự bất kỳ và dừng lại. 
4. Tính tiền tố KMP của`t`. 
5. Hãy để`k = pi[-1]`. Đây là độ dài của tiền tố thích hợp dài nhất của`t`đó cũng là một hậu tố. 
6. Đính kèm một bản sao hoàn chỉnh của`t`vào câu trả lời và trừ số lượng ký tự của nó khỏi nguồn cung cấp có sẵn. 
7. Xác định:```
tail = t[k:]
```Đây chính xác là phần phải được thêm vào để tạo thêm một lần xuất hiện trong khi sử dụng lại phần chồng lấp. 
8. Đếm các số 0 và số 1 có trong`tail`. 
9. Khi còn đủ ký tự để viết một bản sao khác của`tail`, nối nó và trừ số lượng của nó. 
10. Sau khi không thể thêm đuôi nữa, hãy nối tất cả các số 0 còn lại và sau đó là tất cả các số còn lại. 

### Tại sao nó hoạt động 

Sau lần xuất hiện đầu tiên của`t`đã được viết, mỗi lần xuất hiện trong tương lai nên trùng lặp với lần trước đó càng nhiều càng tốt. Sự chồng chéo pháp lý tối đa giữa hai bản sao liên tiếp chính xác là đường viền dài nhất của`t`. 

Bất kỳ sự trùng lặp nhỏ hơn nào cũng tiêu tốn nhiều ký tự hơn trong khi tạo ra cùng số lần xuất hiện. Vì việc cung cấp số 0 và số 1 là cố định nên việc tiêu thụ ít ký tự hơn cho mỗi lần xuất hiện bổ sung không bao giờ có thể tệ hơn. 

Đường viền KMP mang lại sự chồng chéo lớn nhất có thể. Như vậy mỗi phần được thêm vào`tail`tạo ra một lần xuất hiện mới với chi phí ký tự tối thiểu có thể. Việc lặp đi lặp lại điều này sẽ tối đa hóa số lần xuất hiện phù hợp với số lượng có sẵn. Một lần nữa`tail`không đủ khả năng chi trả, không có cấu trúc thay thế nào có thể tạo ra một lần xuất hiện bổ sung vì mỗi lần xuất hiện đều yêu cầu ít nhất nhiều ký tự mới như phần đuôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
t = input().strip()

cnt0 = s.count('0')
cnt1 = len(s) - cnt0

need0 = t.count('0')
need1 = len(t) - need0

if cnt0 < need0 or cnt1 < need1:
    print('0' * cnt0 + '1' * cnt1)
    sys.exit()

m = len(t)

pi = [0] * m
for i in range(1, m):
    j = pi[i - 1]
    while j > 0 and t[i] != t[j]:
        j = pi[j - 1]
    if t[i] == t[j]:
        j += 1
    pi[i] = j

k = pi[-1]
tail = t[k:]

tail0 = tail.count('0')
tail1 = len(tail) - tail0

ans = [t]

cnt0 -= need0
cnt1 -= need1

while cnt0 >= tail0 and cnt1 >= tail1:
    ans.append(tail)
    cnt0 -= tail0
    cnt1 -= tail1

ans.append('0' * cnt0)
ans.append('1' * cnt1)

print(''.join(ans))
```Phần đầu tiên đếm các tài nguyên sẵn có và kiểm tra xem ngay cả một bản sao của`t`là có thể. Nếu không, mọi sự sắp xếp đều không có lần xuất hiện nào, do đó mọi sự sắp xếp lại hợp lệ đều là tối ưu. 

Việc tính toán hàm tiền tố là KMP tiêu chuẩn. giá trị`pi[-1]`đưa ra đường viền dài nhất của toàn bộ chuỗi. 

Ý tưởng tinh tế là việc xây dựng`tail = t[k:]`. Nếu chiều dài đường viền là`k`, sau đó là người đầu tiên`k`các ký tự của lần xuất hiện tiếp theo đã có sẵn dưới dạng hậu tố của lần xuất hiện trước. Chỉ có hậu tố còn lại phải được thêm vào. 

Vòng lặp nối thêm càng nhiều đuôi càng tốt. Mỗi lần lặp lại tạo ra chính xác một lần xuất hiện mới trong khi sử dụng số lượng ký tự tối thiểu có thể. 

Cuối cùng, các số 0 và số 1 còn sót lại sẽ được thêm vào. Tại thời điểm đó, không thể xảy ra thêm sự cố nào nữa vì chúng tôi đã thất bại trong bài kiểm tra khả năng chi trả cho một cái đuôi khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = 101101
t = 110
```Đếm:```
zeros = 2
ones = 4
```Chức năng tiền tố:```
t = 110
pi = [0,1,0]
```Biên giới dài nhất:```
k = 0
tail = 110
```| Bước | Trả lời | Còn lại 0s | Còn lại 1 giây | 
| --- | --- | --- | --- | 
| Bắt đầu | "" | 2 | 4 | 
| Thêm t đầu tiên | 110 | 1 | 2 | 
| Thêm đuôi | 110110 | 0 | 0 | 
| Kết thúc | 110110 | 0 | 0 | 

Đầu ra:```
110110
```Có hai trường hợp xảy ra`"110"`, bắt đầu ở vị trí 1 và 4. 

### Ví dụ 2 

đầu vào:```
s = 11110000
t = 1010
```Chức năng tiền tố:```
pi = [0,0,1,2]
```Biên giới dài nhất:```
k = 2
tail = 10
```| Bước | Trả lời | Còn lại 0s | Còn lại 1 giây | 
| --- | --- | --- | --- | 
| Bắt đầu | "" | 4 | 4 | 
| Thêm t đầu tiên | 1010 | 2 | 2 | 
| Thêm đuôi | 101010 | 1 | 1 | 
| Thêm đuôi | 10101010 | 0 | 0 | 
| Kết thúc | 10101010 | 0 | 0 | 

Sự chồng chéo sẽ lưu các ký tự và cho phép xuất hiện ba lần thay vì chỉ có hai bản sao không chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O( | t | 

Với độ dài lên tới 500.000, thuật toán tuyến tính dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 512 MB. Giải pháp chỉ thực hiện một vài lần truyền qua chuỗi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    s = input().strip()
    t = input().strip()

    cnt0 = s.count('0')
    cnt1 = len(s) - cnt0

    need0 = t.count('0')
    need1 = len(t) - need0

    if cnt0 < need0 or cnt1 < need1:
        print('0' * cnt0 + '1' * cnt1)
        return

    pi = [0] * len(t)
    for i in range(1, len(t)):
        j = pi[i - 1]
        while j and t[i] != t[j]:
            j = pi[j - 1]
        if t[i] == t[j]:
            j += 1
        pi[i] = j

    k = pi[-1]
    tail = t[k:]

    ans = [t]

    cnt0 -= need0
    cnt1 -= need1

    tail0 = tail.count('0')
    tail1 = len(tail) - tail0

    while cnt0 >= tail0 and cnt1 >= tail1:
        ans.append(tail)
        cnt0 -= tail0
        cnt1 -= tail1

    ans.append('0' * cnt0)
    ans.append('1' * cnt1)

    print(''.join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    global input
    input = sys.stdin.readline

    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# sample
assert run("101101\n110\n") == "110110"

# cannot build even one copy
assert run("1111\n000\n") == "1111"

# single character target
assert run("00111\n1\n") == "11100"

# perfect overlap chain
assert run("11110000\n1010\n") == "10101010"

# minimum size
assert run("0\n0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1111 / 000`|`1111`| Không thể hình thành sự xuất hiện | 
|`00111 / 1`|`11100`| Mục tiêu một ký tự | 
|`11110000 / 1010`|`10101010`| Xây dựng chồng chéo lặp đi lặp lại | 
|`0 / 0`|`0`| Đầu vào nhỏ nhất có thể | 

## Vỏ cạnh 

### Không thể hình thành lần xuất hiện đầu tiên 

đầu vào:```
s = 1111
t = 000
```Số lượng có sẵn:```
zeros = 0
ones = 4
```Yêu cầu:```
zeros = 3
ones = 0
```Lần xuất hiện đầu tiên là không thể. Thuật toán ngay lập tức xuất ra tất cả các ký tự còn lại:```
1111
```Mọi sự sắp xếp đều không có lần xuất hiện nào, vì vậy điều này là tối ưu. 

### Nhắm mục tiêu có đường viền lớn 

đầu vào:```
s = 11110000
t = 1010
```Đường viền dài nhất có chiều dài 2.```
1010
  10
```Thuật toán chỉ nối lại nhiều lần`"10"`thay vì một cái khác đầy đủ`"1010"`. Điều này tạo ra nhiều lần xuất hiện hơn từ cùng một nguồn cung cấp ký tự. 

### Độ dài đường viền bằng 0 

đầu vào:```
s = 101101
t = 110
```Đường viền dài nhất bằng 0, vì vậy:```
tail = t
```Không thể có sự chồng chéo. Thuật toán suy thoái một cách tự nhiên thành việc liên tục đặt các bản sao hoàn chỉnh của`t`, điều này chính xác là tối ưu khi không tồn tại sự chồng chéo. 

### Mục tiêu một ký tự 

đầu vào:```
s = 00111
t = 1
```Giá trị tiền tố-hàm bằng 0 và đuôi là`"1"`. 

Thuật toán đặt càng nhiều`'1'`ký tự càng tốt trước tiên:```
111
```Sau đó nối thêm các số 0 còn lại:```
11100
```Mọi`'1'`vị trí tạo thành một lần xuất hiện, đó là số lượng tối đa có thể đạt được.
