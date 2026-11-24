---
title: "CF 96B - Con số may mắn (dễ)"
description: "Chúng ta cần tìm số nhỏ nhất lớn hơn hoặc bằng một số nguyên cho trước sao cho: 1. Mỗi chữ số là 4 hoặc 7. 2. Số 4 bằng số 7. Những con số này được gọi là những con số siêu may mắn. Ví dụ: 47 hợp lệ vì nó chứa một số 4 và một số 7."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 96
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 77 (Div. 2 Only)"
rating: 1300
weight: 96
solve_time_s: 291
verified: true
draft: false
---

[CF 96B - Những con số may mắn (dễ)](https://codeforces.com/problemset/problem/96/B) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, mặt nạ bit, lực lượng vũ phu 
**Thời gian giải:** 4 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta cần tìm số nhỏ nhất lớn hơn hoặc bằng một số nguyên cho trước sao cho: 

1. Mỗi chữ số đều là`4`hoặc`7`. 
2. Số lượng`4`s bằng số lượng`7`S. 

Những con số này được gọi là những con số siêu may mắn. 

Ví dụ,`47`hợp lệ vì nó chứa một`4`và một`7`. số`7744`cũng hợp lệ vì nó có hai chữ số. số`4447`không hợp lệ vì số lượng khác nhau. 

Đầu vào chứa một số nguyên duy nhất`n`, lên đến`10^9`. Chúng ta phải xuất ra con số siêu may mắn nhỏ nhất ít nhất`n`. 

Ràng buộc trên`n`là quan sát chính. Từ`10^9`có nhiều nhất 10 chữ số, đáp án nào cũng chỉ có một số nhỏ chữ số. Việc tìm kiếm mạnh mẽ trên các số nguyên thông thường vẫn sẽ rất tệ vì số may mắn rất thưa thớt. Kiểm tra mọi số từ`n`trở lên có thể yêu cầu hàng triệu hoặc hàng tỷ lần lặp. 

Cấu trúc của những con số may mắn làm thay đổi hoàn toàn vấn đề. Một số hợp lệ chỉ có thể chứa`4`Và`7`và độ dài phải chẵn vì số lượng phải khớp. Với tối đa 10 chữ số, tổng số ứng viên là rất nhỏ. 

Trong một thời gian dài`2k`, chúng ta chỉ cần chọn cái nào`k`vị trí chứa`4`. Các vị trí còn lại trở thành`7`. Số chuỗi như vậy là:$$\binom{2k}{k}$$Đối với độ dài lên tới 10 chữ số:$$\binom{2}{1} + \binom{4}{2} + \binom{6}{3} + \binom{8}{4} + \binom{10}{5} = 2 + 6 + 20 + 70 + 252 = 350$$Tổng cộng chỉ có 350 ứng cử viên tồn tại. Nó đủ nhỏ để trực tiếp tạo ra mọi khả năng. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ các giải pháp bất cẩn. 

Nếu như`n = 1`, câu trả lời là`47`. Một cách tiếp cận tham lam cố gắng giữ nguyên số chữ số có thể thất bại vì không tồn tại số siêu may mắn có 1 chữ số. 

Nếu như`n = 999999999`, câu trả lời trở thành`4444477777`, có nhiều chữ số hơn`n`. Bất kỳ giải pháp nào bị giới hạn ở số chữ số ban đầu sẽ bỏ lỡ điều này. 

Nếu như`n`bản thân nó đã là siêu may mắn rồi, chẳng hạn như`4477`, chúng ta phải trả lại nó không thay đổi. Sử dụng một cách nghiêm ngặt`>`so sánh thay vì`>=`sẽ bỏ qua nó một cách không chính xác. 

Một trường hợp tế nhị khác là việc sắp xếp từ điển. Giả định`n = 4500`. Các ứng cử viên có 4 chữ số hợp lệ là:`4477, 4747, 4774, 7447, 7474, 7744`Câu trả lời là`4747`. Một công trình tham lam ngây thơ có thể gặp khó khăn sau khi chọn sai chữ số đầu. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là bắt đầu từ`n`và kiểm tra từng số nguyên một. 

Đối với mỗi số, chúng ta sẽ kiểm tra các chữ số của nó và kiểm tra xem: 

1. Mỗi chữ số là`4`hoặc`7`. 
2. Số lượng của`4`Và`7`cuộc thi đấu. 

Điều này hoạt động hợp lý, nhưng hiệu suất là thảm họa. Con số may mắn vô cùng thưa thớt. Xung quanh`10^9`, chúng ta có thể cần quét hàng triệu số nguyên thông thường trước khi tìm số nguyên hợp lệ tiếp theo. Với khả năng lên tới một tỷ giá trị, cách tiếp cận này không thực tế. 

Quan sát quan trọng là bản thân không gian trả lời rất nhỏ. 

Con số siêu may mắn: 

1. Chỉ sử dụng chữ số`4`Và`7`. 
2. Có chiều dài chẵn. 
3. Chứa đúng một nửa`4`s rưỡi`7`S. 

Thay vì tìm kiếm qua tất cả các số nguyên, chúng ta có thể tạo trực tiếp mọi ứng cử viên hợp lệ. 

Đối với mỗi độ dài chẵn: 

1. Chọn vị trí cho`4`S. 
2. Điền vào các vị trí còn lại bằng`7`S. 
3. Chuyển dãy số thu được thành số. 

Vì độ dài tối đa chúng ta cần chỉ là 10 chữ số nên tổng số ứng viên hợp lệ chỉ là 350. 

Sau khi tạo chúng: 

1. Sắp xếp danh sách. 
2. Tìm giá trị đầu tiên ít nhất`n`. 

Điều này chuyển đổi vấn đề từ việc tìm kiếm một phạm vi số lớn sang tìm kiếm một tập hợp nhỏ được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời - n) | O(1) | Quá chậm | 
| Tối ưu | O(350 log 350) | O(350) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Tạo tất cả các số siêu may mắn có độ dài`2, 4, 6, 8, 10`. 

Chúng tôi không bao giờ cần độ dài dài hơn bởi vì`n ≤ 10^9`và câu trả lời tiếp theo có thể có trong vòng 10 chữ số. 
3. Đối với mỗi chiều dài chẵn`L`, tạo tất cả các mặt nạ nhị phân từ`0`ĐẾN`(1 << L) - 1`. 

Mỗi bit xác định liệu một vị trí có chứa`4`hoặc`7`. 
4. Đếm xem có bao nhiêu bit được đặt trong mặt nạ. 

Một con số siêu may mắn hợp lệ phải chứa chính xác`L / 2`chữ số bằng`4`. 
5. Xây dựng số chữ số theo chữ số. 

Nếu một bit được thiết lập, hãy đặt`4`. Nếu không thì đặt`7`. 
6. Chuyển đổi chuỗi đã xây dựng thành số nguyên và lưu trữ nó. 
7. Sắp xếp tất cả các ứng cử viên được tạo ra. 

Sắp xếp đảm bảo thứ tự số tăng dần. 
8. Quét danh sách đã sắp xếp và xuất ra số đầu tiên`x`như vậy`x >= n`. 

### Tại sao nó hoạt động 

Mỗi con số siêu may mắn đều có: 

1. Chiều dài chẵn. 
2. Chính xác một nửa các chữ số của nó bằng`4`. 
3. Tất cả các chữ số còn lại bằng`7`. 

Thuật toán tạo ra mọi chuỗi thỏa mãn các điều kiện này với mọi độ dài phù hợp. Không có ứng cử viên hợp lệ nào bị bỏ qua. 

Sau khi sắp xếp, giá trị đầu tiên lớn hơn hoặc bằng`n`chính xác là câu trả lời hợp lệ nhỏ nhất. Vì danh sách ứng viên đầy đủ và được sắp xếp theo thứ tự nên thuật toán không thể bỏ sót số hợp lệ nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    lucky = []

    for length in range(2, 11, 2):
        half = length // 2

        for mask in range(1 << length):
            if bin(mask).count("1") != half:
                continue

            s = []

            for i in range(length):
                if (mask >> i) & 1:
                    s.append('4')
                else:
                    s.append('7')

            lucky.append(int("".join(s)))

    lucky.sort()

    for x in lucky:
        if x >= n:
            print(x)
            return

solve()
```Giải pháp trực tiếp thực hiện chiến lược thế hệ. 

Vòng lặp bên ngoài lặp lại trên tất cả các độ dài chẵn từ 2 đến 10. Đó là độ dài duy nhất có thể có cho các số siêu may mắn trong bài toán này. 

Mỗi bitmask đại diện cho một phép gán chữ số. Một bit được đặt có nghĩa là chữ số`4`, trong khi một bit không được đặt có nghĩa là chữ số`7`. Việc kiểm tra:```
bin(mask).count("1") != half
```lọc ra những mặt nạ không chứa chính xác một nửa`4`S. 

Trước tiên, số được tạo dưới dạng một chuỗi vì cách nối chữ số đơn giản hơn và ít xảy ra lỗi hơn. 

Việc sắp xếp là cần thiết vì mặt nạ không tạo ra các số theo thứ tự số một cách tự nhiên. 

Một chi tiết tinh tế là sự so sánh:```
if x >= n:
```Sự bình đẳng rất quan trọng. Nếu như`n`đã là siêu may mắn rồi, chúng ta phải trả lại nó ngay lập tức. 

Một điểm tinh tế khác là câu trả lời có thể có nhiều chữ số hơn`n`. Ví dụ:```
999999999 -> 4444477777
```Việc tạo tất cả độ dài lên tới 10 đảm bảo trường hợp này được xử lý chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4500
```Tạo số siêu may mắn gồm 4 chữ số: 

| Ứng viên | >= 4500? | 
| --- | --- | 
| 4477 | Không | 
| 4747 | Có | 
| 4774 | Có | 
| 7447 | Có | 
| 7474 | Có | 
| 7744 | Có | 

Ứng cử viên hợp lệ đầu tiên là`4747`. 

Đầu ra:```
4747
```Dấu vết này cho thấy tại sao việc sắp xếp lại quan trọng. Ứng viên đầu tiên thỏa mãn điều kiện sẽ tự động là câu trả lời hợp lệ tối thiểu. 

### Ví dụ 2 

đầu vào:```
4477
```Ứng viên liên quan: 

| Ứng viên | >= 4477? | 
| --- | --- | 
| 4477 | Có | 
| 4747 | Có | 
| 4774 | Có | 

Thuật toán dừng ngay tại`4477`. 

Đầu ra:```
4477
```Điều này khẳng định tầm quan trọng của việc sử dụng`>=`còn hơn là`>`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(350 log 350) | Tối đa 350 ứng viên hợp lệ được tạo và sắp xếp | 
| Không gian | O(350) | Danh sách toàn bộ số siêu may mắn được lưu trữ | 

Thời gian chạy thực tế là rất nhỏ. Ngay cả việc tạo ra mọi mặt nạ có độ dài lên tới 10 cũng chỉ cần vài nghìn lần lặp. Điều này dễ dàng phù hợp với giới hạn 2 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    lucky = []

    for length in range(2, 11, 2):
        half = length // 2

        for mask in range(1 << length):
            if bin(mask).count("1") != half:
                continue

            s = []

            for i in range(length):
                if (mask >> i) & 1:
                    s.append('4')
                else:
                    s.append('7')

            lucky.append(int("".join(s)))

    lucky.sort()

    for x in lucky:
        if x >= n:
            print(x)
            return

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("4500\n") == "4747\n", "sample 1"

# minimum input
assert run("1\n") == "47\n", "minimum case"

# already super lucky
assert run("4477\n") == "4477\n", "already valid"

# boundary before next candidate
assert run("4748\n") == "4774\n", "next larger candidate"

# maximum-style edge
assert run("999999999\n") == "4444477777\n", "needs longer length"

# exact smallest super lucky
assert run("47\n") == "47\n", "smallest valid number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`47`| Không tồn tại số siêu may mắn 1 chữ số | 
|`4477`|`4477`| Trường hợp bình đẳng | 
|`4748`|`4774`| Sửa lỗi lựa chọn ứng viên tiếp theo | 
|`999999999`|`4444477777`| Trả lời có thể cần thêm chữ số | 
|`47`|`47`| Số siêu may mắn hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
```Thuật toán tạo ra tất cả các số siêu may mắn có 2 chữ số trước tiên:`47`Và`74`. 

Sau khi sắp xếp,`47`là giá trị đầu tiên thỏa mãn`x >= 1`, vậy đáp án là:```
47
```Trường hợp này xác nhận rằng thuật toán xử lý chính xác các tình huống trong đó câu trả lời có nhiều chữ số hơn đầu vào. 

Bây giờ hãy xem xét:```
999999999
```Không có số siêu may mắn 8 chữ số hoặc 9 chữ số nào có thể thỏa mãn điều kiện vì: 

1. Những con số siêu may mắn yêu cầu độ dài chẵn. 
2. Tất cả các ứng viên có 8 chữ số đều nhỏ hơn`999999999`. 

Thuật toán tiếp tục tạo ra các ứng viên có 10 chữ số. Nhỏ nhất là:```
4444477777
```Vì nó là ứng cử viên hợp lệ đầu tiên lớn hơn hoặc bằng đầu vào nên nó sẽ trở thành câu trả lời. 

Cuối cùng, hãy xem xét:```
4477
```Các ứng viên được tạo ra bao gồm`4477`chính xác. Trong quá trình quét: 

| Ứng viên | Tình trạng | 
| --- | --- | 
| 4477 |`4477 >= 4477`là đúng | 

Thuật toán in`4477`ngay lập tức. Điều này xác nhận việc so sánh là toàn diện, khớp chính xác với yêu cầu của bài toán.
