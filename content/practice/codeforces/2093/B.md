---
title: "CF 2093B - Con Số Đắt"
description: "Chúng ta được cung cấp một số thập phân dưới dạng một chuỗi. Chúng tôi có thể xóa bất kỳ tập con chữ số nào của nó trong khi vẫn giữ nguyên thứ tự tương đối của các chữ số còn lại. Chuỗi chữ số kết quả phải đại diện cho một số dương, mặc dù cho phép các số 0 đứng đầu."
date: "2026-06-08T05:37:58+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2093
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1016 (Div. 3)"
rating: 900
weight: 2093
solve_time_s: 108
verified: true
draft: false
---

[CF 2093B - Con số đắt đỏ](https://codeforces.com/problemset/problem/2093/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số thập phân dưới dạng một chuỗi. Chúng tôi có thể xóa bất kỳ tập con chữ số nào của nó trong khi vẫn giữ nguyên thứ tự tương đối của các chữ số còn lại. Chuỗi chữ số kết quả phải đại diện cho một số dương, mặc dù cho phép các số 0 đứng đầu. 

Đối với mỗi số kết quả, giá trị của nó được xác định là:$$\text{cost}=\frac{\text{number value}}{\text{sum of its digits}}$$Trong số tất cả các dãy con hợp lệ, chúng ta muốn có chi phí nhỏ nhất có thể. Sau khi đạt được chi phí tối thiểu đó, chúng tôi muốn giữ lại càng nhiều chữ số càng tốt vì đầu ra yêu cầu số lần xóa tối thiểu. 

Số đầu vào có thể chứa tối đa 100 chữ số, vì vậy nó lớn hơn nhiều so với các loại số nguyên tiêu chuẩn. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào dựa trên việc chuyển đổi dãy con thành số nguyên thực tế đều có rủi ro. Số lượng ca kiểm thử nhiều nhất là 1000, có nghĩa là chúng ta cần thời gian gần với thời gian tuyến tính cho mỗi ca kiểm thử. Vì mỗi chuỗi có độ dài tối đa là 100, thậm chí một$O(n^2)$giải pháp dễ dàng đủ nhanh. 

Phần khó khăn là hiểu được chi phí tối thiểu có thể thực sự là bao nhiêu. 

Một sai lầm dễ mắc phải là cho rằng việc giữ nhiều chữ số khác 0 sẽ có ích. Coi như:```
n = 12
```Các dãy con hợp lệ có thể có là`1`,`2`, Và`12`. 

Các chi phí là:$$1/1=1,\quad 2/2=1,\quad 12/(1+2)=4$$Chi phí tối thiểu là 1, đạt được bằng một chữ số khác 0. Câu trả lời đúng là xóa 1. 

Một trường hợp tinh tế khác là khi số 0 đứng đầu trở nên hữu ích:```
n = 1005
```Giữ`0005`là không thể vì trật tự phải được duy trì, nhưng phải giữ`005`là hợp lệ. Chi phí của nó là$$5/(0+0+5)=1$$Kết quả có ba chữ số, không phải một. Giải pháp chỉ tìm kiếm một chữ số sẽ bỏ lỡ số lần xóa tối ưu. 

Cạm bẫy phổ biến thứ ba là quên rằng số cuối cùng phải dương:```
n = 1000
```Chỉ giữ số 0 là không hợp lệ vì số kết quả bằng 0. Dãy con hợp lệ tốt nhất là`0001`? Lệnh đó không tồn tại. Sự lựa chọn tốt nhất là`1000`,`100`,`10`, hoặc`1`. Chi phí tối thiểu đạt được bằng cách`1`, và chúng ta chỉ có thể giữ lại một chữ số. Câu trả lời là 3 lần xóa. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi chuỗi con của chuỗi đã cho. Đối với một số có độ dài 100, có$$2^{100}$$những hậu quả có thể xảy ra, hoàn toàn không thể thực hiện được. Ngay cả đối với độ dài 60, điều này đã vượt xa những gì có thể xử lý được. 

Brute-force hoạt động về mặt khái niệm vì chúng ta có thể tính toán chi phí của mọi dãy con hợp lệ và chọn dãy tốt nhất. Vấn đề là không gian tìm kiếm tăng theo cấp số nhân. 

Để tìm ra cách tiếp cận tốt hơn, chúng ta cần hiểu cấu trúc của hàm chi phí. 

Giả sử dãy con có tổng các chữ số$S$và giá trị số$V$. Vì số đó là dương nên$V \ge S$. 

Tại sao điều này luôn đúng? Một số thập phân$$d_k10^k+d_{k-1}10^{k-1}+\cdots+d_0$$ít nhất là$$d_k+d_{k-1}+\cdots+d_0$$vì mọi lũy thừa của mười đều ít nhất bằng 1. 

Điều này mang lại$$\frac{V}{S}\ge 1$$cho mọi dãy con hợp lệ. 

Vì vậy, chi phí tối thiểu có thể không bao giờ có thể dưới 1. 

Khi nào sự bình đẳng giữ được? 

Chúng tôi cần$V=S$. Cách duy nhất để một số thập phân bằng tổng các chữ số của nó là khi tồn tại chính xác một chữ số khác 0 và mọi chữ số được giữ còn lại đều là số 0 đứng đầu. Ví dụ:```
5
05
005
0007
```Tất cả những thứ này đều có giá 1. 

Khi chúng ta biết chi phí tối thiểu có thể đạt được luôn là 1 thì nhiệm vụ sẽ thay đổi hoàn toàn. Chúng tôi không còn tối ưu hóa một phần nữa. Chúng ta chỉ cần dãy con dài nhất có giá trị bằng 1. 

Dãy con như vậy phải chứa chính xác một chữ số khác 0 và tất cả các chữ số được giữ lại khác phải bằng 0 trước nó. Bất kỳ số 0 nào sau chữ số khác 0 đó sẽ đóng góp một vị trí thập phân và tăng giá trị số mà không làm tăng tổng các chữ số, làm cho chi phí lớn hơn 1. 

Vì vậy, đối với mọi vị trí chữ số khác 0, chúng ta có thể tạo thành một dãy con giá 1 bao gồm: 

1. Bất kỳ số 0 nào trước vị trí đó. 
2. Chính chữ số khác 0 đó. 

Chiều dài của nó bằng:$$(\text{number of zeros before it}) + 1$$Chúng tôi muốn chiều dài tối đa như vậy. 

Nếu độ dài chuỗi ban đầu là$n$và độ dài tối đa có thể giữ được là$L$, thì câu trả lời là:$$n-L$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gọi độ dài của sợi dây là$n$. 
2. Quét chuỗi từ trái sang phải trong khi vẫn duy trì số lượng số 0 đã thấy cho đến nay. 
3. Bất cứ khi nào chữ số hiện tại là`'0'`, tăng bộ đếm số 0. 
4. Bất cứ khi nào chữ số hiện tại khác 0, hãy tính độ dài của chuỗi con giá 1 dài nhất kết thúc ở chữ số này. 

Độ dài đó là:$$\text{zeros\_before}+1$$bởi vì chúng ta có thể giữ mọi số 0 trước đó và chữ số khác 0 này. 
5. Theo dõi độ dài tối đa như vậy trên tất cả các vị trí khác 0. 
6. Sau khi quét, hãy để độ dài tối đa này là$L$. 
7. Đầu ra:$$n-L$$kể từ khi chúng tôi giữ$L$chữ số và xóa phần còn lại. 

### Tại sao nó hoạt động 

Mọi số dương đều thỏa mãn$V \ge S$, do đó mọi chi phí hợp lệ ít nhất là 1. Một dãy con đạt được chi phí 1 chính xác khi giá trị số của nó bằng tổng các chữ số của nó. 

Đối với biểu diễn thập phân, điều đó chỉ xảy ra khi có một chữ số khác 0 và mọi chữ số được giữ khác nằm trước nó và bằng 0. Bất kỳ chữ số bổ sung nào khác 0 đều làm tăng giá trị nhiều hơn tổng các chữ số. Bất kỳ số 0 nào sau chữ số khác 0 sẽ tạo thêm một vị trí thập phân và cũng làm tăng giá trị. 

Đối với một chữ số cố định khác 0, việc giữ tất cả các số 0 trước đó sẽ tối đa hóa độ dài chuỗi con trong khi vẫn giữ nguyên giá 1. Kiểm tra mọi vị trí khác 0 sẽ tìm ra chuỗi con giá 1 dài nhất có thể. Vì chi phí 1 là chi phí tối thiểu toàn cục nên việc tối đa hóa độ dài giữa các chuỗi con như vậy sẽ giảm thiểu số lần xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    s = input().strip()

    zeros = 0
    best = 0

    for ch in s:
        if ch == '0':
            zeros += 1
        else:
            best = max(best, zeros + 1)

    print(len(s) - best)
```Biến`zeros`lưu trữ bao nhiêu số 0 đã xuất hiện trước vị trí hiện tại. 

Bất cứ khi nào chúng ta gặp một chữ số khác 0, chúng ta biết rằng mọi số 0 trước đó có thể được giữ cùng với chữ số này để tạo thành một dãy con giá 1. Độ dài kết quả là`zeros + 1`. 

Biến`best`lưu trữ độ dài lớn nhất như vậy trên toàn bộ chuỗi. 

Cuối cùng, nếu`best`các chữ số có thể được giữ lại trong khi đạt được chi phí tối thiểu có thể, sau đó tất cả các chữ số còn lại phải được loại bỏ. Câu trả lời là`len(s) - best`. 

Một chi tiết tinh tế là các số 0 xuất hiện sau chữ số khác 0 đã chọn sẽ không bao giờ được tính. Chúng không thể thuộc dãy con giá 1 vì chúng sẽ tăng giá trị số trong khi vẫn giữ nguyên tổng các chữ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
13700
```| Vị trí | Chữ số | Số Không Trước | Độ dài ứng viên | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 
| 1 | 3 | 0 | 1 | 1 | 
| 2 | 7 | 0 | 1 | 1 | 
| 3 | 0 | 1 | - | 1 | 
| 4 | 0 | 2 | - | 1 | 

Dãy con giá 1 dài nhất có độ dài 1. Chúng tôi giữ bất kỳ một chữ số nào khác 0 và xóa bốn chữ số còn lại. 

Trả lời:$$5-1=4$$### Ví dụ 2 

đầu vào:```
102030
```| Vị trí | Chữ số | Số Không Trước | Độ dài ứng viên | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 
| 1 | 0 | 1 | - | 1 | 
| 2 | 2 | 1 | 2 | 2 | 
| 3 | 0 | 2 | - | 2 | 
| 4 | 3 | 2 | 3 | 3 | 
| 5 | 0 | 3 | - | 3 | 

Lựa chọn tốt nhất là dãy sau`003`, được tạo bởi hai số 0 đứng trước và chữ số`3`. 

Độ dài của nó là 3, vì vậy:$$6-3=3$$việc xóa là cần thiết. 

Ví dụ này chứng minh tại sao số 0 đứng đầu lại quan trọng. Chúng tăng số lượng chữ số được giữ mà không ảnh hưởng đến chi phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần quét từ trái sang phải của chuỗi | 
| Không gian |$O(1)$| Chỉ có một vài quầy được lưu trữ | 

Với tối đa 100 chữ số cho mỗi trường hợp thử nghiệm và tối đa 1000 trường hợp thử nghiệm, tổng khối lượng công việc là rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        s = input().strip()

        zeros = 0
        best = 0

        for ch in s:
            if ch == '0':
                zeros += 1
            else:
                best = max(best, zeros + 1)

        out.append(str(len(s) - best))

    return "\n".join(out)

# provided samples
assert run("4\n666\n13700\n102030\n7\n") == "2\n4\n3\n0"

# minimum size
assert run("1\n1\n") == "0"

# many leading zeros before final non-zero after deletions
assert run("1\n1005\n") == "1"

# all digits non-zero
assert run("1\n99999\n") == "4"

# off-by-one around final position
assert run("1\n0001\n") == "0"

# length 100 boundary
assert run("1\n" + "1" + "0" * 99 + "\n") == "99"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Đầu vào hợp lệ nhỏ nhất | 
|`1005`|`1`| Các số 0 đứng đầu có thể được giữ trước một chữ số khác 0 | 
|`99999`|`4`| Không có số 0, chỉ có thể giữ lại một chữ số | 
|`0001`|`0`| Toàn bộ chuỗi đã có giá 1 | 
|`1`theo sau là 99 số không |`99`| Ranh giới độ dài tối đa và hành vi ở cuối | 

## Vỏ cạnh 

### Một số chỉ chứa một chữ số khác 0 

đầu vào:```
1000
```Quét: 

| Chữ số | Số Không Trước | Ứng viên | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 0 | 1 | - | 
| 0 | 2 | - | 
| 0 | 3 | - |`best = 1`, vậy câu trả lời là`4 - 1 = 3`. 

Giữ bất kỳ dấu 0 nào sau chữ số`1`sẽ sản xuất`10`,`100`, hoặc`1000`, tất cả đều có giá lớn hơn 1. 

### Số 0 đứng đầu có lợi 

đầu vào:```
1005
```Quét: 

| Chữ số | Số Không Trước | Ứng viên | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 0 | 1 | - | 
| 0 | 2 | - | 
| 5 | 2 | 3 |`best = 3`, tương ứng với dãy con`005`. 

Câu trả lời là`4 - 3 = 1`. 

Điều này cho thấy tại sao việc tối đa hóa độ dài chuỗi con trong số các chuỗi con có giá 1 lại quan trọng. 

### Nhiều chữ số khác 0 

đầu vào:```
12
```Quét: 

| Chữ số | Số Không Trước | Ứng viên | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 0 | 1 |`best = 1`, vậy câu trả lời là`2 - 1 = 1`. 

Giữ cả hai chữ số mang lại chi phí$12/3 = 4$, tệ hơn chi phí tối thiểu có thể đạt được là 1. 

### Toàn bộ số đã có chi phí tối thiểu 

đầu vào:```
0007
```Quét: 

| Chữ số | Số Không Trước | Ứng viên | 
| --- | --- | --- | 
| 0 | 1 | - | 
| 0 | 2 | - | 
| 0 | 3 | - | 
| 7 | 3 | 4 |`best = 4`, vậy câu trả lời là`4 - 4 = 0`. 

Toàn bộ số đã bao gồm các số 0 đứng đầu, theo sau là một chữ số khác 0, mang lại giá trị bằng 1 và không cần xóa.
