---
title: "CF 102448E - Ai cũng thích acai"
description: "Mỗi nhà hàng đưa cho Gabriel một số nguyên (k), biểu thị số lượng açaí tối đa anh ta có thể cho vào bát của mình. Anh ta muốn số nguyên dương lớn nhất không vượt quá (k) có các ước số thích hợp cộng chính xác với chính số đó. Nếu không tồn tại số đó thì câu trả lời là (-1)."
date: "2026-08-09T02:06:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "E"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 483
verified: true
draft: false
---

[CF 102448E - Mọi người đều yêu thích acai](https://codeforces.com/problemset/problem/102448/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 3 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi nhà hàng đưa cho Gabriel một số nguyên (k), biểu thị số lượng açaí tối đa anh ta có thể cho vào bát của mình. Anh ta muốn số nguyên dương lớn nhất không vượt quá (k) có các ước số thích hợp cộng chính xác với chính số đó. Nếu không tồn tại số đó thì câu trả lời là (-1). 

Ví dụ, (6) là hoàn hảo vì các ước số thực sự của nó là (1,2,3) và (1+2+3=6). Do đó, nhà hàng cung cấp (8) có thể cung cấp một bát hoàn hảo có kích thước (6), trong khi nhà hàng cung cấp (5) không thể cung cấp bất kỳ bát hoàn hảo nào. 

Dữ liệu đầu vào có thể chứa tối đa (2\cdot10^6) nhà hàng và tối đa mỗi số tiền là (2\cdot10^6). Số lượng truy vấn đủ lớn nên việc thực hiện lý thuyết số đáng kể riêng biệt cho từng nhà hàng là không khả thi. Ngay cả một bài kiểm tra số hoàn hảo (O(\sqrt{k})) cũng sẽ yêu cầu khoảng (1400) kiểm tra số chia cho một số gần giới hạn trên và việc tìm kiếm qua nhiều ứng cử viên sẽ nhân chi phí đó lên đáng kể. 

Thực tế quan trọng là phạm vi đó rất nhỏ so với thang đo xuất hiện các số hoàn hảo. Các số hoàn hảo đầu tiên là (6,28,496,8128), trong khi số tiếp theo là (33,550,336), đã vượt xa mức tối đa của bài toán là (2\cdot10^6). 

Cũng không có số hoàn hảo lẻ nào ẩn trong phạm vi này. Trên thực tế, sự tồn tại của số hoàn hảo lẻ vẫn là một bài toán mở, nhưng bất kỳ số nào như vậy đều đã được chứng minh là lớn hơn (10^{1500}). Do đó, các số hoàn hảo duy nhất liên quan đến bài toán này là chính xác (6,28,496,8128). 

Một số ranh giới nhỏ rất dễ bị xử lý sai. Đối với đầu vào`1`, câu trả lời là`-1`, vì (1) không có ước số dương thích hợp nên tổng ước số của nó là (0). Đối với đầu vào`6`, câu trả lời là`6`, vì bản thân giới hạn trên có thể hoàn hảo. Đối với đầu vào`7`, câu trả lời vẫn là`6`, bởi vì chúng ta cần số hoàn hảo lớn nhất nhỏ hơn hoặc bằng số tiền của nhà hàng chứ không phải số hoàn hảo nhỏ hơn nó. Cuối cùng, đối với đầu vào`2000000`, câu trả lời là`8128`, vì số hoàn hảo tiếp theo đã nằm ngoài phạm vi cho phép. 

Ví dụ, đầu vào```
1
1
```phải sản xuất```
-1
```Việc triển khai bất cẩn coi (1) là hoàn hảo vì mọi số đều chia hết cho chính nó sẽ sử dụng định nghĩa ước số sai. 

Tương tự,```
1
6
```phải sản xuất```
6
```Việc triển khai chỉ tìm kiếm số hoàn hảo nằm dưới (k) sẽ trả về không chính xác (-1). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xử lý từng nhà hàng một cách độc lập. Bắt đầu từ (k), chúng ta có thể kiểm tra (k,k-1,k-2,\ldots) cho đến khi tìm được số hoàn hảo. Để kiểm tra một ứng cử viên (x), chúng ta có thể liệt kê các ước số lên đến (\sqrt{x}), cộng cả hai thành viên của mỗi cặp ước số. Điều này đúng vì mọi ước số thực sự đều xuất hiện trực tiếp trong phạm vi đó hoặc được ghép với một ước số bổ sung. 

Vấn đề là số lượng công việc lặp đi lặp lại. Đối với truy vấn có (k=2\cdot10^6), quá trình tìm kiếm chỉ dừng khi đạt đến (8128), do đó, nó sẽ kiểm tra khoảng (2\cdot10^6-8128) ứng viên. Mỗi ứng viên thực hiện (O(\sqrt{x})) công việc, đưa ra khoảng 

[ 
\sum_{x=8129}^{2\cdot10^6}\sqrt{x}, 
] 

đó là khoảng (1.9\cdot10^9) số lần lặp chia cho chỉ một truy vấn trong trường hợp xấu nhất. Với (2\cdot10^6) nhà hàng, công việc trong trường hợp xấu nhất là ở mức (10^{15}), vượt xa thời hạn. 

Lực lượng vũ phu hoạt động vì việc kiểm tra một ứng cử viên cho chúng tôi biết chính xác liệu ứng cử viên đó có hoàn hảo hay không, nhưng nó không thành công vì các ứng cử viên tương tự được phát hiện lại cho hàng triệu truy vấn. Việc quan sát thấy toàn bộ phạm vi đầu vào chỉ chứa bốn số hoàn hảo sẽ thay đổi hoàn toàn vấn đề. Thay vì tìm kiếm xuống theo từng nhà hàng, chúng tôi lưu trữ danh sách đã sắp xếp 

[ 
[6,28,496,8128]. 
] 

Với mỗi (k), chúng ta chỉ cần phần tử lớn nhất của danh sách này nhiều nhất là (k). Vì danh sách chứa bốn phần tử nên đây thực sự là thời gian không đổi. 

Tìm kiếm nhị phân thuận tiện vì nó thể hiện trực tiếp truy vấn dưới dạng "tìm số hoàn hảo ngoài cùng bên phải không vượt quá (k)". Chỉ với bốn giá trị, ngay cả quét tuyến tính cũng đủ nhanh, nhưng tìm kiếm nhị phân làm cho điều kiện biên trở nên rõ ràng và giữ cho cách tiếp cận tổng quát nếu danh sách được tính toán trước lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nM^{3/2})) trong trường hợp xấu nhất | (O(1)) | Quá chậm | 
| Tối ưu | (O(n\log 4)=O(n)) | (O(1)) | Đã chấp nhận | 

Ở đây (M=2\cdot10^6). Giải pháp tối ưu là tuyến tính một cách hiệu quả về số lượng nhà hàng. 

## Hướng dẫn thuật toán 

1. Lưu trữ các số hoàn hảo duy nhất có thể xuất hiện trong phạm vi số lượng cho phép:`6`,`28`,`496`, Và`8128`. Số hoàn hảo tiếp theo là (33,550,336), nên không còn gì cần xem xét. 
2. Đọc số tiền (k) của nhà hàng hiện tại. Chúng ta cần số hoàn hảo được lưu trữ lớn nhất thỏa mãn (p\le k). 
3. Sử dụng`bisect_right`trong danh sách số hoàn hảo đã sắp xếp. Nó trả về vị trí ngay sau tất cả các giá trị nhỏ hơn hoặc bằng (k). Di chuyển một vị trí sang trái sẽ cho chính xác số hoàn hảo hợp lệ lớn nhất. 
4. Nếu vị trí kết quả là âm thì (k<6), do đó không có bát hoàn hảo nào tồn tại và chúng tôi xuất ra`-1`. Ngược lại, xuất ra số hoàn hảo ở vị trí đó. 
5. Lặp lại thao tác tra cứu có kích thước không đổi tương tự cho tất cả các nhà hàng và viết câu trả lời theo thứ tự ban đầu. 

### Tại sao nó hoạt động 

Điều bất biến là danh sách được lưu trữ chứa mọi số hoàn hảo có thể là câu trả lời cho một giá trị đầu vào. Đối với bất kỳ số tiền nhà hàng nào (k), câu trả lời hợp lệ phải là một trong bốn số đó và không được vượt quá (k).`bisect_right`chọn số lớn nhất như vậy, do đó giá trị trả về vừa hợp lệ vừa tối đa. Nếu mọi số hoàn hảo được lưu trữ đều vượt quá (k), thì không có câu trả lời hợp lệ nào tồn tại và`-1`là đúng. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

# The only perfect numbers <= 2 * 10^6.
PERFECT = (6, 28, 496, 8128)

def solve():
    n = int(input())
    out = bytearray()

    for _ in range(n):
        k = int(input())
        pos = bisect_right(PERFECT, k) - 1

        if pos < 0:
            out.extend(b"-1\n")
        else:
            out.extend(str(PERFECT[pos]).encode())
            out.append(10)

    sys.stdout.write(out.decode())

if __name__ == "__main__":
    solve()
```Bộ dữ liệu`PERFECT`được sắp xếp, được yêu cầu bởi`bisect_right`. Vì bộ dữ liệu chỉ có bốn phần tử nên tìm kiếm nhị phân chỉ thực hiện tối đa một vài phép so sánh bất kể kích thước của (k). 

Phép trừ một sau`bisect_right`xử lý ranh giới trên một cách chính xác. Nếu (k=6),`bisect_right`trả lại vị trí sau`6`, do đó vị trí trước chứa`6`chính nó. Nếu (k=7), nó trả về cùng một vị trí chèn và phần tử trước đó vẫn`6`. Nếu (k=5), nó trả về 0, do đó trừ đi 1 sẽ được`-1`. 

Sản lượng được tích lũy trong một`bytearray`thay vì lưu trữ hai triệu chuỗi Python trong một danh sách. Điều này giữ cho mức sử dụng bộ nhớ ở mức thấp và giảm số lượng thao tác đầu ra. Các số nguyên có kích thước tùy ý của Python cũng loại bỏ mọi lo ngại về tràn số nguyên, mặc dù tất cả các giá trị ở đây đủ nhỏ để các số nguyên máy thông thường đã đủ. 

Đầu vào được xử lý từng dòng một thay vì sử dụng`read().split()`. Với hai triệu truy vấn, việc cụ thể hóa đồng thời mọi mã thông báo đầu vào sẽ tiêu tốn nhiều bộ nhớ hơn mức cần thiết. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, danh sách số hoàn hảo là`[6, 28, 496, 8128]`. 

| Nhà hàng | (k) |`bisect_right(PERFECT, k)`| Vị trí sau`-1`| Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 8 | 1 | 0 | 6 | 
| 2 | 5 | 0 | -1 | -1 | 

Với (k=8), chỉ`6`nhiều nhất là số tiền có sẵn, vì vậy câu trả lời là`6`. Với (k=5), ngay cả số hoàn hảo nhỏ nhất cũng quá lớn, nên đáp án là`-1`. Điều này thể hiện cả truy vấn tiền nhiệm thông thường và ranh giới "không có giá trị hợp lệ". 

Đối với Mẫu 2, ví dụ thứ hai của câu lệnh là một nhà hàng có (k=5). 

| Nhà hàng | (k) |`bisect_right(PERFECT, k)`| Vị trí sau`-1`| Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | -1 | -1 | 

Việc tìm kiếm không vô tình xem xét`6`, bởi vì`bisect_right`chỉ bao gồm các giá trị nhiều nhất là (5). Đây chính xác là sự bất đẳng thức mà bài toán yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log 4)=O(n)) | Mỗi nhà hàng thực hiện tìm kiếm nhị phân trên bốn giá trị cố định. | 
| Không gian | (O(1)) không gian phụ | Danh sách số hoàn hảo được cố định và dung lượng lưu trữ đầu ra tỷ lệ thuận với kích thước đầu ra được yêu cầu. | 

Với (n\le2\cdot10^6), thuật toán chỉ thực hiện một số so sánh cho mỗi nhà hàng. Không có phép liệt kê số chia, phân tích nhân tử, sàng lọc hoặc tìm kiếm theo truy vấn trong phạm vi số nguyên. Quá trình xử lý trước lý thuyết số có kích thước cố định giúp giải pháp dễ dàng tương thích với giới hạn thời gian 3 giây và giới hạn bộ nhớ 256 MB. Giới hạn vấn đề ban đầu là (2\cdot10^6) nhà hàng và (2\cdot10^6) là số tiền tối đa. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_right

PERFECT = (6, 28, 496, 8128)
input = sys.stdin.readline

def solve():
    n = int(input())
    out = bytearray()

    for _ in range(n):
        k = int(input())
        pos = bisect_right(PERFECT, k) - 1

        if pos < 0:
            out.extend(b"-1\n")
        else:
            out.extend(str(PERFECT[pos]).encode())
            out.append(10)

    sys.stdout.write(out.decode())

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided sample 1
assert run("2\n8\n5\n") == "6\n-1\n", "sample 1"

# Provided sample 2
assert run("1\n5\n") == "-1\n", "sample 2"

# Minimum-size input
assert run("1\n1\n") == "-1\n", "minimum value"

# Exact perfect numbers and their immediate successors
assert run(
    "8\n"
    "6\n"
    "7\n"
    "28\n"
    "29\n"
    "496\n"
    "497\n"
    "8128\n"
    "8129\n"
) == (
    "6\n"
    "6\n"
    "28\n"
    "28\n"
    "496\n"
    "496\n"
    "8128\n"
    "8128\n"
), "perfect-number boundaries"

# All-equal values
assert run("5\n2000000\n2000000\n2000000\n2000000\n2000000\n") == (
    "8128\n8128\n8128\n8128\n8128\n"
), "all equal"

# Maximum number of restaurants
maximum_input = "2000000\n" + "2000000\n" * 2000000
maximum_output = "8128\n" * 2000000
assert run(maximum_input) == maximum_output, "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`-1`| Số tiền nhỏ nhất có thể và không có số hoàn hảo. | 
|`6, 7, 28, 29, 496, 497, 8128, 8129`|`6, 6, 28, 28, 496, 496, 8128, 8128`| Kết quả trùng khớp chính xác và số liền trước ngay sau mỗi số hoàn hảo. | 
| Năm bản sao của`2000000`| Năm bản sao của`8128`| Các giá trị trên số hoàn hảo có liên quan lớn nhất và các truy vấn lặp lại. | 
| Hai triệu bản sao của`2000000`| Hai triệu bản sao của`8128`| Kích thước đầu vào tối đa, xử lý đầu ra và các giá trị hoàn toàn bằng nhau. | 

## Vỏ cạnh 

Với số tiền nhỏ nhất, hãy xem xét```
1
1
```

`bisect_right`trả lại`0`vì mọi số hoàn hảo đều lớn hơn`1`. Sau khi trừ đi một, vị trí là`-1`, do đó, thuật toán đầu ra`-1`. Điều này tránh việc xử lý không chính xác số đó như một ước số thích hợp. 

Để có một số hoàn hảo chính xác, hãy xem xét```
1
28
```Vị trí chèn được trả về bởi`bisect_right`là vị trí sau`28`. Trừ một lựa chọn`28`, vì vậy đầu ra là```
28
```Việc sử dụng`bisect_right`, còn hơn là`bisect_left`, là điều làm cho sự bình đẳng hoạt động chính xác. 

Đối với một giá trị ngay trên một số hoàn hảo, hãy xem xét```
1
29
```Việc tìm kiếm vẫn chọn`28`, bởi vì`29`bản thân nó không hoàn hảo và câu trả lời bắt buộc không được vượt quá số tiền của nhà hàng. Đầu ra là```
28
```Với số tiền tối đa có thể,```
1
2000000
```việc tìm kiếm đạt tới`8128`. Số hoàn hảo tiếp theo là (33,550,336), do đó phạm vi đầu vào không cho phép số hoàn hảo lớn hơn. Đầu ra là```
8128
```Cuối cùng, để có số lượng nhà hàng tối đa, mọi truy vấn đều độc lập. Ngay cả khi tất cả hai triệu nhà hàng đều có chính xác`2000000`, mỗi truy vấn chỉ thực hiện tìm kiếm nhị phân bốn phần tử. Thuật toán không bao giờ chia tỷ lệ theo khoảng cách số giữa`k`và số hoàn hảo gần nhất, là thuộc tính làm cho lời giải đủ nhanh cho đầu vào lớn nhất.
