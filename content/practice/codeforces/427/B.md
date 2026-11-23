---
title: "CF 427B - Chuyển tù"
description: "Chúng tôi có một hàng tù nhân và mỗi tù nhân có một giá trị về mức độ nghiêm trọng của tội phạm. Chúng ta phải chọn đúng c tù nhân liên tiếp để chuyển đi. Mọi tù nhân trong phân đoạn được chọn phải có mức độ nghiêm trọng tối đa là t."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 427
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 244 (Div. 2)"
rating: 1100
weight: 427
solve_time_s: 93
verified: true
draft: false
---

[CF 427B - Chuyển tù](https://codeforces.com/problemset/problem/427/B) 

**Đánh giá:** 1100 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng tù nhân và mỗi tù nhân có một giá trị về mức độ nghiêm trọng của tội phạm. Chúng ta phải lựa chọn chính xác`c`tù nhân liên tiếp để chuyển giao. Mọi tù nhân trong phân đoạn đã chọn phải có mức độ nghiêm trọng tối đa`t`. 

Nhiệm vụ chỉ đơn giản là đếm xem có bao nhiêu đoạn dài liền kề nhau`c`thỏa mãn hạn chế này. 

Đầu vào cung cấp số lượng tù nhân, mức độ nghiêm trọng tối đa cho phép và độ dài phân đoạn được yêu cầu. Sau đó, chúng tôi nhận được một loạt các mức độ nghiêm trọng. Đầu ra là một số nguyên duy nhất, số lượng các phân đoạn liền kề hợp lệ. 

Những hạn chế ngay lập tức loại trừ những cách tiếp cận tốn kém. Số lượng tù nhân có thể lên tới`2 * 10^5`, có nghĩa là một`O(n^2)`thuật toán sẽ thực hiện khoảng`4 * 10^10`trong trường hợp xấu nhất, vượt xa giới hạn cho một bài toán 1 giây. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính. 

Bản thân các giá trị có thể lớn bằng`10^9`, nhưng điều đó không quan trọng lắm vì chúng ta không bao giờ thực hiện phép tính trên chúng ngoài việc so sánh với`t`. 

Có một số trường hợp khó bỏ sót. 

Một lỗi phổ biến là quên rằng các phân đoạn phải liền kề nhau. Ví dụ:```
5 3 2
1 5 2 3 1
```Các phân đoạn hợp lệ là: 

-`[2,3]`-`[3,1]`Câu trả lời là`2`, không`4`. Một giải pháp bất cẩn có thể đếm tối đa tất cả tù nhân có giá trị`3`và cố gắng hình thành sự kết hợp từ chúng, bỏ qua yêu cầu liền kề. 

Một trường hợp tế nhị khác xảy ra khi một tù nhân xấu chia mảng thành các vùng độc lập.```
7 2 3
1 2 1 5 1 1 1
```Ba tù nhân đầu tiên từ một phân đoạn hợp lệ. Ba tù nhân cuối cùng từ một người khác. Bất kỳ đoạn nào đi qua`5`không hợp lệ. Câu trả lời đúng là`2`. 

Một lỗi thường xuyên xảy ra khi độ dài khối hợp lệ bằng chính xác`c`.```
4 3 4
1 2 3 1
```Toàn bộ mảng là hợp lệ, vì vậy câu trả lời là`1`. Một số triển khai sử dụng không chính xác`length - c`thay vì`length - c + 1`. 

Một trường hợp quan trọng khác là khi không có tù nhân nào thỏa mãn giới hạn.```
5 1 2
3 4 5 6 7
```Không có phân đoạn nào hoạt động, vì vậy câu trả lời là`0`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi kiểm tra mọi đoạn chiều dài liền kề`c`, sau đó kiểm tra xem tất cả các giá trị bên trong phân đoạn đó có nhiều nhất không`t`. 

có`n - c + 1`các phân đoạn có thể. Việc kiểm tra một phân đoạn mất`O(c)`thời gian, do đó tổng độ phức tạp trở thành`O((n - c + 1) * c)`. Trong trường hợp xấu nhất, cả hai`n`Và`c`có thể là về`2 * 10^5`, đưa ra một cách đại khái`4 * 10^10`hoạt động. Như vậy là quá chậm. 

Brute-force hoạt động vì điều kiện cho một phân đoạn rất đơn giản: mọi phần tử bên trong nó phải thỏa mãn một giới hạn. Vấn đề là các phân đoạn lân cận chồng chéo lên nhau rất nhiều, nhưng kẻ tấn công liên tục kiểm tra lại các phần tử giống nhau. 

Quan sát quan trọng là các tù nhân không hợp lệ tách hoàn toàn mảng thành các vùng hợp lệ độc lập. Giả sử chúng ta quét mảng và chỉ tập trung vào các đoạn liên tiếp trong đó mọi giá trị nhiều nhất`t`. 

Ví dụ:```
1 2 1 5 1 1 1
```giá trị`5`chia mảng thành:```
[1 2 1]   and   [1 1 1]
```Bên trong một khối có độ dài hợp lệ`L`, mọi mảng con liền kề có độ dài`c`được tự động hợp lệ. Số mảng con như vậy là:```
L - c + 1
```cung cấp`L >= c`. 

Điều này biến vấn đề thành việc đếm độ dài của các khối hợp lệ tối đa. 

Chúng tôi quét một lần qua mảng trong khi duy trì độ dài hiện tại của các tù nhân được chấp nhận liên tiếp. Khi chúng ta gặp một giá trị lớn hơn`t`, khối hiện tại kết thúc và chúng tôi thêm phần đóng góp của nó. 

Điều này mang lại một tuyến tính`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × c) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo biến trả lời`ans = 0`. 
2. Duy trì bộ đếm`length`đại diện cho khối liên tiếp hiện tại trong đó tất cả các mức độ nghiêm trọng đều cao nhất`t`. 
3. Duyệt mảng từ trái sang phải. 
4. Nếu mức độ nghiêm trọng hiện tại cao nhất`t`, tăng`length`vì khối hợp lệ vẫn tiếp tục. 
5. Nếu mức độ nghiêm trọng hiện tại lớn hơn`t`, khối hợp lệ hiện tại kết thúc. 

Nếu như`length >= c`, thì khối này đóng góp`length - c + 1`các phân đoạn hợp lệ. 

Thêm giá trị này vào câu trả lời, sau đó đặt lại`length = 0`. 
6. Sau khi kết thúc quá trình truyền tải, hãy xử lý khối cuối cùng. 

Bước này là cần thiết vì mảng có thể kết thúc khi vẫn ở trong khối hợp lệ. 
7. In câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến`length`luôn bằng kích thước của hậu tố tối đa hiện tại chỉ bao gồm các tù nhân được chấp nhận. 

Bất cứ khi nào chúng ta gặp một tù nhân không hợp lệ, không có phân đoạn hợp lệ nào có thể vượt qua vị trí đó. Điều này có nghĩa là mọi phân đoạn hợp lệ phải nằm hoàn toàn bên trong một khối hợp lệ. Đối với một khối có chiều dài`L`, mọi cửa sổ liền kề có kích thước`c`là hợp lệ và số lượng cửa sổ như vậy là chính xác`L - c + 1`. 

Vì thuật toán xử lý mọi khối hợp lệ chính xác một lần và đếm tất cả độ dài-`c`cửa sổ bên trong nó, mỗi phân đoạn hợp lệ được tính một lần và chỉ một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, t, c = map(int, input().split())
a = list(map(int, input().split()))

ans = 0
length = 0

for x in a:
    if x <= t:
        length += 1
    else:
        if length >= c:
            ans += length - c + 1
        length = 0

if length >= c:
    ans += length - c + 1

print(ans)
```Việc thực hiện trực tiếp theo ý tưởng đếm khối. 

Biến`length`lưu trữ kích thước của vùng hợp lệ liên tiếp hiện tại. Mỗi lần chúng ta đọc một giá trị không vượt quá`t`, chúng tôi mở rộng khu vực thêm một. 

Khi một giá trị vượt quá`t`, chúng tôi biết khu vực hiện tại đã kết thúc. Nếu kích thước của nó ít nhất là`c`, thì nó chứa các cửa sổ hợp lệ. Số cửa sổ là`length - c + 1`. 

trận chung kết`if`sau vòng lặp rất dễ bị quên. Không có nó, bất kỳ khối hợp lệ nào kết thúc ở phần tử cuối cùng sẽ không bao giờ được tính. 

Tất cả các phép tính đều vừa vặn thoải mái bên trong các số nguyên Python. Ngay cả trong trường hợp lớn nhất, câu trả lời nhiều nhất là`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 3
2 3 1 1
```| Vị trí | Giá trị | Có hiệu lực (`<= t`) | Hiện hành`length`| Đã thêm vào`ans`| Tổng cộng`ans`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | Có | 1 | 0 | 0 | 
| 1 | 3 | Có | 2 | 0 | 0 | 
| 2 | 1 | Có | 3 | 0 | 0 | 
| 3 | 1 | Có | 4 | 0 | 0 | 

Sau khi truyền tải:```
length = 4
4 - 3 + 1 = 2
```Câu trả lời cuối cùng:```
2
```Các phân đoạn hợp lệ là:```
[2,3,1]
[3,1,1]
```Ví dụ này chứng minh rằng một khối hợp lệ có thể chứa nhiều phân đoạn hợp lệ chồng chéo. 

### Ví dụ 2 

đầu vào:```
7 2 3
1 2 1 5 1 1 1
```| Vị trí | Giá trị | Có hiệu lực (`<= t`) | Hiện hành`length`| Đã thêm vào`ans`| Tổng cộng`ans`| 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | Có | 1 | 0 | 0 | 
| 1 | 2 | Có | 2 | 0 | 0 | 
| 2 | 1 | Có | 3 | 0 | 0 | 
| 3 | 5 | Không | 0 | 1 | 1 | 
| 4 | 1 | Có | 1 | 0 | 1 | 
| 5 | 1 | Có | 2 | 0 | 1 | 
| 6 | 1 | Có | 3 | 0 | 1 | 

Sau khi truyền tải:```
length = 3
3 - 3 + 1 = 1
```Câu trả lời cuối cùng:```
2
```Dấu vết này cho thấy một giá trị không hợp lệ sẽ chia mảng thành các vùng độc lập như thế nào. Không có phân đoạn hợp lệ nào có thể vượt qua vị trí chứa`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tù nhân được xử lý đúng một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Quét tuyến tính`2 * 10^5`các yếu tố dễ dàng đủ nhanh trong thời gian giới hạn. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ mảng đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, t, c = map(int, input().split())
    a = list(map(int, input().split()))

    ans = 0
    length = 0

    for x in a:
        if x <= t:
            length += 1
        else:
            if length >= c:
                ans += length - c + 1
            length = 0

    if length >= c:
        ans += length - c + 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("4 3 3\n2 3 1 1\n") == "2", "sample 1"

# minimum size
assert run("1 5 1\n3\n") == "1", "single valid prisoner"

# no valid segment
assert run("5 1 2\n3 4 5 6 7\n") == "0", "all prisoners invalid"

# entire array valid
assert run("5 10 3\n1 2 3 4 5\n") == "3", "all windows valid"

# exact block size equals c
assert run("4 3 4\n1 2 3 1\n") == "1", "off-by-one check"

# multiple separated blocks
assert run("7 2 3\n1 2 1 5 1 1 1\n") == "2", "split by invalid prisoner"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 1 / 3`|`1`| Kích thước đầu vào tối thiểu | 
|`5 1 2 / 3 4 5 6 7`|`0`| Không có phân đoạn hợp lệ | 
|`5 10 3 / 1 2 3 4 5`|`3`| Toàn bộ mảng hợp lệ | 
|`4 3 4 / 1 2 3 1`|`1`| Xử lý khối có độ dài chính xác | 
|`7 2 3 / 1 2 1 5 1 1 1`|`2`| Tách theo giá trị không hợp lệ | 

## Vỏ cạnh 

Hãy xem xét trường hợp một tù nhân xấu chia mảng:```
7 2 3
1 2 1 5 1 1 1
```Quá trình quét xây dựng một khối có độ dài hợp lệ`3`, rồi gặp nhau`5`. Từ`5 > 2`, thuật toán cho biết thêm:```
3 - 3 + 1 = 1
```Sau đó, nó đặt lại độ dài khối về 0 và bắt đầu đếm lại. Khối cuối cùng cũng có chiều dài`3`, đóng góp khác`1`. Tổng số trở thành`2`. 

Bây giờ hãy xem xét trường hợp ranh giới chính xác:```
4 3 4
1 2 3 1
```Toàn bộ mảng tạo thành một khối có độ dài hợp lệ`4`. Từ`length == c`, công thức cho:```
4 - 4 + 1 = 1
```Điều này đếm chính xác phân đoạn hợp lệ duy nhất bao gồm toàn bộ mảng. 

Cuối cùng, hãy xem xét một mảng hoàn toàn không hợp lệ:```
5 1 2
3 4 5 6 7
```Mọi giá trị đều vượt quá`t`, do đó thuật toán được đặt lại nhiều lần`length`về không. Không có khối nào đạt đến kích thước`2`, vậy đáp án vẫn là`0`.
