---
title: "CF 1654E - Các phép toán số học"
description: "Chúng tôi muốn thay đổi càng ít phần tử mảng càng tốt để mảng cuối cùng trở thành một cấp số cộng. Một cấp số cộng được xác định hoàn toàn bởi hai tham số: giá trị đầu tiên và sai số chung của nó."
date: "2026-06-10T03:43:20+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 1654
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 778 (Div. 1 + Div. 2, based on Technocup 2022 Final Round)"
rating: 2300
weight: 1654
solve_time_s: 160
verified: true
draft: false
---

[CF 1654E - Các phép toán số học](https://codeforces.com/problemset/problem/1654/E) 

**Đánh giá:** 2300 
**Tags:** sức mạnh vũ phu, cấu trúc dữ liệu, đồ thị, toán học 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi muốn thay đổi càng ít phần tử mảng càng tốt để mảng cuối cùng trở thành một cấp số cộng. 

Một cấp số cộng được xác định hoàn toàn bởi hai tham số: giá trị đầu tiên và sai số chung của nó. Nếu chúng ta viết tiến trình như$$a_i = b + d \cdot i$$thì mọi vị trí đã thỏa mãn công thức này đều có thể được giữ nguyên, còn mọi vị trí không thỏa mãn thì phải sửa đổi. 

Điều đó biến vấn đề thành một vấn đề tối đa hóa. Thay vì giảm thiểu trực tiếp số lượng thay đổi, chúng tôi tìm kiếm một cấp số cộng phù hợp với số lượng vị trí lớn nhất. Nếu sự tiến triển đó phù hợp$k$các vị trí còn lại$n-k$các vị trí phải được thay đổi. 

Độ dài mảng có thể đạt tới$10^5$, điều này ngay lập tức loại trừ bất cứ điều gì gần với$O(n^2)$. Một thuật toán bậc hai sẽ thực hiện khoảng$10^{10}$hoạt động vượt xa giới hạn. Chúng ta cần thứ gì đó xung quanh$O(n\sqrt n)$hoặc$O(n \log n)$. 

Một số trường hợp đặc biệt khiến cho việc suy luận ngây thơ trở nên nguy hiểm. 

Coi như```
3
5 5 5
```Mảng đã là một cấp số cộng có sai phân chung$0$. Câu trả lời là$0$. Bất kỳ giải pháp nào chỉ kiểm tra sự khác biệt khác 0 sẽ thất bại ở đây. 

Coi như```
4
1 100000 1 100000
```Tiến trình tối ưu có thể có sự khác biệt chung tích cực hoặc tiêu cực rất lớn. Việc hạn chế tìm kiếm ở những khác biệt nhỏ sẽ bỏ lỡ các câu trả lời hợp lệ. 

Coi như```
1
42
```Một phần tử duy nhất luôn là một cấp số cộng. Câu trả lời phải là$0$. 

Vấn đề tế nhị nhất đến từ giới hạn giá trị. Mọi giá trị ban đầu nằm giữa$1$Và$100000$, nhưng tiến trình đích có thể chứa số âm hoặc giá trị lớn hơn$100000$. Một giải pháp chỉ phải suy luận về những điểm hiện có đã nằm trên một tiến trình chứ không phải về phạm vi của tiến trình cuối cùng. 

## Phương pháp tiếp cận 

Một cách nhìn hình học hữu ích là coi mọi phần tử mảng là một điểm$(i,a_i)$. 

Một cấp số cộng có dạng$$a_i=b+d\,i$$đó chính xác là phương trình của một đường thẳng. Vấn đề trở thành: 

Tìm đường đi qua số điểm lớn nhất$(i,a_i)$. 

Nếu một dòng chứa$k$điểm, những cái đó$k$các vị trí đã khớp với tiến trình được biểu thị bằng dòng đó. 

Ý tưởng vũ phu rất đơn giản. Chọn từng cặp điểm, xác định đường thẳng duy nhất đi qua chúng và đếm xem có bao nhiêu điểm nằm trên đường thẳng đó. Dòng tốt nhất đưa ra câu trả lời. 

Điều này đúng vì mọi cấp số cộng đều tương ứng với một dòng và mỗi dòng được xác định bởi hai điểm. 

Vấn đề là chi phí. có$O(n^2)$cặp, và thậm chí việc lưu trữ hoặc đếm chúng trở nên không thể thực hiện được.$n=10^5$. 

Quan sát quan trọng đến từ giới hạn$a_i \le 100000$. 

Cho phép$B \approx \sqrt{100000}$, Về$320$. 

Chúng tôi chia sẻ sự khác biệt chung$d$thành hai loại. 

Đối với những mái dốc nhỏ,$|d| \le B$, chỉ có khoảng$2B+1$những giá trị có thể. Đối với một cố định$d$,$$a_i = b + d i$$ngụ ý$$b = a_i - d i.$$Tất cả các điểm trên cùng một tiến trình có sự khác biệt$d$có cùng giá trị của$a_i-di$. Chúng ta có thể đếm tần số của$a_i-di$cho mỗi việc nhỏ$d$. 

Đối với những mái dốc lớn,$|d| > B$, điều gì đó đặc biệt sẽ xảy ra. Vì tất cả các giá trị ban đầu đều nằm trong$[1,100000]$,$$|a_j-a_i| \le 100000.$$Nếu hai điểm thuộc một cấp số cộng với$|d|>B$, sau đó$$|j-i| \le \frac{100000}{|d|} < \frac{100000}{B}.$$Lựa chọn$B \approx \sqrt{100000}$làm cho khoảng cách này nhiều nhất là khoảng$320$. 

Vì vậy, mọi cặp điểm thuộc một cấp số có độ dốc lớn phải gần nhau về chỉ số. Chúng ta chỉ cần kiểm tra một cửa sổ ngắn có độ dài$B$trước mỗi vị trí. 

Điều này chuyển vấn đề thành một$O(nB)$giải pháp đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n\sqrt A)$, Ở đâu$A=100000$|$O(\sqrt A)$mức sử dụng băm trung bình | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để$B=\lfloor\sqrt{100000}\rfloor+1$. 
2. Duy trì`best`, số vị trí tối đa đã nằm trên một cấp số cộng chung. 
3. Xử lý mọi khác biệt nhỏ$|d|\le B$. 

Với mỗi số nguyên$d$trong khoảng đó hãy tính$$a_i-di$$cho mọi vị trí$i$. 

Các vị trí thuộc cùng một tiến trình có sự khác biệt chung$d$tạo ra giá trị như nhau. Đếm tần số bằng bản đồ băm và cập nhật`best`. 
4. Xử lý mọi khác biệt lớn$|d|>B$. 

Đối với mọi chỉ số bắt đầu$i$, chỉ kiểm tra các vị trí$$j=i+1,\dots,\min(n-1,i+B).$$Bất kỳ tiến trình có độ dốc lớn hợp lệ nào chứa cả hai$i$Và$j$phải thỏa mãn$$a_j-a_i=d(j-i).$$Nếu như$(a_j-a_i)$chia hết cho$(j-i)$, tính độ dốc nguyên$$d=\frac{a_j-a_i}{j-i}.$$5. Bỏ qua độ dốc với$|d|\le B$, vì chúng đã được xử lý. 
6. Đối với các độ dốc còn lại, hãy đếm xem có bao nhiêu điểm gần đó cho ra kết quả giống nhau$d$với chỉ số bắt đầu cố định$i$. 

Nếu xuất hiện độ dốc$c$lần thì dòng tương ứng chứa$c+1$điểm bao gồm$i$. 

Cập nhật`best`. 
7. Câu trả lời là$$n-\text{best}.$$### Tại sao nó hoạt động 

Mỗi cấp số cộng tương ứng với một dòng$a_i=b+di$. 

Giai đoạn độ dốc nhỏ kiểm tra rõ ràng mọi độ dốc có thể với$|d|\le B$. Đối với độ dốc cố định, các điểm nằm trên cùng một tiến trình chính xác khi chúng có cùng giá trị chặn$a_i-di$, do đó số đếm tần số sẽ đưa ra số vị trí khớp chính xác. 

Đối với các độ dốc lớn, giới hạn giá trị buộc hai điểm khớp bất kỳ phải gần nhau về chỉ số. Khoảng cách chỉ số tối đa có thể nhỏ hơn$100000/B$, nhiều nhất là$B$. Do đó, mọi cặp điểm thuộc một cấp số có độ dốc lớn đều được phát hiện bên trong cửa sổ cục bộ. Khi xử lý chỉ số nhỏ nhất trên tiến trình đó, tất cả các điểm khác trên tiến trình đó đều có cùng độ dốc$d$và số đếm sẽ khôi phục kích thước chính xác của tiến trình. 

Vì mọi tiến trình có thể rơi vào loại độ dốc nhỏ hoặc độ dốc lớn, nên thuật toán tìm thấy số lượng tối đa các vị trí không thay đổi. Số lượng sửa đổi tối thiểu là phần bổ sung của mức tối đa đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n <= 2:
        print(0)
        return

    MAX_A = 100000
    B = int(MAX_A ** 0.5) + 1

    best = 1

    # Small slopes: |d| <= B
    for d in range(-B, B + 1):
        freq = {}
        cur_best = 0

        for i, x in enumerate(a):
            key = x - d * i
            val = freq.get(key, 0) + 1
            freq[key] = val
            if val > cur_best:
                cur_best = val

        if cur_best > best:
            best = cur_best

    # Large slopes: |d| > B
    for i in range(n):
        cnt = {}

        limit = min(n, i + B + 1)

        for j in range(i + 1, limit):
            diff = a[j] - a[i]
            dist = j - i

            if diff % dist != 0:
                continue

            d = diff // dist

            if -B <= d <= B:
                continue

            cnt[d] = cnt.get(d, 0) + 1

        for c in cnt.values():
            if c + 1 > best:
                best = c + 1

    print(n - best)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên liệt kê từng điểm khác biệt nhỏ chung. Đối với mỗi cái, số lượng`a[i] - d * i`đóng vai trò là điểm chặn của đường dây. Các điểm chặn bằng nhau có nghĩa là các điểm tương ứng đã nằm trên cùng một cấp số cộng. 

Giai đoạn thứ hai là nơi khai thác giới hạn giá trị. Đối với một vị trí cố định`i`, chỉ tiếp theo`B`chỉ số cần được kiểm tra Bất kỳ tiến trình nào có độ dốc lớn hơn không thể kéo dài hơn thế vì các giá trị ban đầu bị giới hạn trong một phạm vi chiều rộng`100000`. 

Một lỗi triển khai phổ biến là quên rằng các chỉ mục trong đạo hàm toán học dựa trên 1 trong khi Python sử dụng chỉ mục dựa trên 0. biểu thức`a[i] - d * i`hoạt động hoàn hảo với lập chỉ mục dựa trên 0 vì việc thay đổi tất cả các chỉ số bằng một hằng số chỉ làm dịch chuyển phần chặn và không ảnh hưởng đến sự bình đẳng. 

Một sai lầm dễ mắc phải khác là tính các độ dốc lớn trên toàn cầu. Bản đồ băm phải được xây dựng lại cho mọi chỉ mục bắt đầu`i`. Nếu không, các đường không liên quan có cùng độ dốc sẽ bị trộn lẫn với nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
3 2 7 8 6 9 5 4 1
```Một trong những tiến trình tối ưu là:$$11,10,9,8,7,6,5,4,3$$với sự khác biệt chung$-1$. 

Vì$d=-1$, tính toán$a_i-di=a_i+i$. 

| tôi | một [tôi] | a[i] + tôi | 
| --- | --- | --- | 
| 0 | 3 | 3 | 
| 1 | 2 | 3 | 
| 2 | 7 | 9 | 
| 3 | 8 | 11 | 
| 4 | 6 | 10 | 
| 5 | 9 | 14 | 
| 6 | 5 | 11 | 
| 7 | 4 | 11 | 
| 8 | 1 | 9 | 

Tần số cao nhất là 3, đến từ các giá trị chặn 9 và 11. 

Các sườn dốc khác cũng được kiểm tra. Dòng tối đa được tìm thấy chứa 3 điểm, vì vậy: 

| Số lượng | Giá trị | 
| --- | --- | 
| n | 9 | 
| tốt nhất | 3 | 
| trả lời | 6 | 

Dấu vết cho thấy cách thuật toán tìm kiếm trên tất cả các độ dốc có thể và giữ nhóm lớn nhất các vị trí đã chính xác. 

### Ví dụ 2 

Hãy xem xét```
5
1 3 5 7 9
```Vì$d=2$: 

| tôi | một [tôi] | a[i] - 2i | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 3 | 1 | 
| 2 | 5 | 1 | 
| 3 | 7 | 1 | 
| 4 | 9 | 1 | 

Mọi giá trị đều giống hệt nhau. 

| Số lượng | Giá trị | 
| --- | --- | 
| n | 5 | 
| tốt nhất | 5 | 
| trả lời | 0 | 

Toàn bộ mảng đã nằm trên một cấp số cộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\sqrt{100000})$| Độ dốc nhỏ và độ dốc lớn đều thực hiện$O(nB)$làm việc | 
| Không gian |$O(B)$trung bình | Số lần lưu trữ bản đồ băm cho một độ dốc hoặc một cửa sổ chỉ mục tại một thời điểm | 

Với$B \approx 317$, tổng công việc là khoảng một vài lần$10^7$các thao tác đơn giản, phù hợp thoải mái trong giới hạn cho$n=10^5$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)
    output = io.StringIO()

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = input_data
    sys.stdout = output

    try:
        import math

        input = sys.stdin.readline

        n = int(input())
        a = list(map(int, input().split()))

        if n <= 2:
            print(0)
        else:
            MAX_A = 100000
            B = int(MAX_A ** 0.5) + 1

            best = 1

            for d in range(-B, B + 1):
                freq = {}
                for i, x in enumerate(a):
                    key = x - d * i
                    freq[key] = freq.get(key, 0) + 1
                    best = max(best, freq[key])

            for i in range(n):
                cnt = {}
                for j in range(i + 1, min(n, i + B + 1)):
                    diff = a[j] - a[i]
                    dist = j - i

                    if diff % dist:
                        continue

                    d = diff // dist

                    if -B <= d <= B:
                        continue

                    cnt[d] = cnt.get(d, 0) + 1

                for v in cnt.values():
                    best = max(best, v + 1)

            print(n - best)

        return output.getvalue()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# minimum size
assert run("1\n42\n") == "0\n"

# two elements always form an AP
assert run("2\n5 100\n") == "0\n"

# already arithmetic
assert run("5\n1 3 5 7 9\n") == "0\n"

# all equal
assert run("6\n7 7 7 7 7 7\n") == "0\n"

# one wrong element
assert run("5\n1 3 100 7 9\n") == "1\n"

# large slope progression
assert run("4\n1 100000 199999 299998\n") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`|`0`| Mảng một phần tử | 
|`2 / 5 100`|`0`| Hai phần tử bất kỳ tạo thành một AP | 
|`1 3 5 7 9`|`0`| Đã số học | 
|`7 7 7 7 7 7`|`0`| Sự khác biệt bằng không | 
|`1 3 100 7 9`|`1`| Cần một sửa đổi | 
|`1 100000 199999 299998`|`0`| Xử lý độ dốc lớn | 

## Vỏ cạnh 

Hãy xem xét```
1
42
```Thuật toán ngay lập tức trở lại`0`bởi vì một mảng có độ dài bằng một đã là một cấp số cộng. Không cần xử lý độ dốc. 

Coi như```
6
7 7 7 7 7 7
```Đối với độ dốc$d=0$, mọi giá trị của$a_i-di$bằng$7$. Tần số trở thành$6$, Vì thế`best = 6`và câu trả lời là`0`. Điều này xử lý chính xác sự tiến triển liên tục. 

Coi như```
5
1 3 100 7 9
```Đối với độ dốc$d=2$, các giá trị chặn là: 

| tôi | a[i] - 2i | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 
| 2 | 96 | 
| 3 | 1 | 
| 4 | 1 | 

Sự đánh chặn`1`xuất hiện bốn lần, nghĩa là bốn vị trí đã nằm trong tiến trình$1,3,5,7,9$. Câu trả lời trở thành$5-4=1$. 

Coi như```
4
1 100000 199999 299998
```Sự khác biệt chung là$99999$, lớn hơn nhiều so với$B$. Pha dốc nhỏ không thể phát hiện được nó. Giai đoạn độ dốc lớn kiểm tra các chỉ số lân cận, tính toán độ dốc chính xác từ việc kiểm tra tính chia hết, nhóm các độ dốc bằng nhau lại với nhau và khôi phục tất cả bốn điểm trên cùng một tiến trình. Câu trả lời là`0`.
