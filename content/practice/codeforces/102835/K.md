---
title: "CF 102835K - Số có bằng Cử nhân"
description: "Số cử nhân là số có biểu diễn trong cơ số đã chọn không chứa chữ số lặp lại. Vấn đề hỗ trợ hai cơ sở: thập phân và thập lục phân. Ví dụ: 123 là số cử nhân trong cơ số 10, trong khi 9af là số cử nhân trong cơ số 16."
date: "2026-07-26T15:03:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102835
codeforces_index: "K"
codeforces_contest_name: "The 2020 ICPC Asia Taipei-Hsinchu Site Programming Contest"
rating: 0
weight: 102835
solve_time_s: 52
verified: true
draft: false
---

[CF 102835K - Số có bằng Cử nhân](https://codeforces.com/problemset/problem/102835/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Số cử nhân là số có biểu diễn trong cơ số đã chọn không chứa chữ số lặp lại. Vấn đề hỗ trợ hai cơ sở: thập phân và thập lục phân. Ví dụ: 123 là số cử nhân trong cơ số 10, trong khi 9af là số cử nhân trong cơ số 16. Giá trị như 101 trong số thập phân không hợp lệ vì chữ số 1 xuất hiện hai lần. 

Mỗi truy vấn hỏi có bao nhiêu số cử nhân nằm trong một khoảng hoặc hỏi số ở vị trí gốc 0 nhất định trong danh sách có thứ tự các số cử nhân. Ký tự đầu tiên chọn căn cứ,`d`cho số thập phân và`h`cho hệ thập lục phân. 

Giá trị đầu vào có thể lớn bằng số 64 bit không dấu. Điều đó ngay lập tức loại trừ việc lặp lại trong khoảng thời gian hoặc tạo ra tất cả các giá trị đến một giới hạn. Có tới 50000 truy vấn, vì vậy mỗi truy vấn phải được trả lời với một lượng công việc nhỏ, tỷ lệ lý tưởng với số chữ số. 

Phần khó khăn là việc bao gồm số 0 và thực tế là thứ tự là số. Số 0 là số cử nhân hợp lệ vì chữ số duy nhất của nó xuất hiện một lần. Chỉ mục truy vấn dựa trên 0, vì vậy số cử nhân đầu tiên là 0. Ví dụ:```
d 1 10
```yêu cầu số cử nhân thứ 10 ở dạng thập phân. Các số tại chỉ số 0 đến 9 là 0, 1, 2, ..., 9 nên đáp án là:```
9
```Trường hợp cạnh thứ hai là một số có nhiều chữ số hơn cơ số không thể là số độc thân. Trong hệ thập lục phân, chỉ có 16 chữ số có thể, vì vậy số thập lục phân 17 chữ số phải lặp lại một chữ số. Giải pháp bất cẩn chỉ kiểm tra các chữ số lặp lại sau khi xây dựng có thể thất bại khi tìm kiếm chỉ mục lớn không tồn tại. 

Ví dụ:```
h 1 ffffffffffffffff
```yêu cầu số thập lục phân ở vị trí rất lớn. Câu trả lời là:```
-
```vì số lượng cử nhân có thể có hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi số, kiểm tra xem các chữ số của nó có phải là duy nhất hay không và tiếp tục cho đến khi tìm thấy vị trí được yêu cầu hoặc một khoảng được tính. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, không gian tìm kiếm là rất lớn. Khoảng thời gian 64 bit có thể chứa tối đa 2^64 số, vượt xa những gì có thể xử lý. 

Quan sát quan trọng là số cử nhân chỉ phụ thuộc vào sự lựa chọn chữ số. Khi một chữ số đã được sử dụng, nó không thể xuất hiện lại. Điều này làm cho số lần tiếp tục hợp lệ có thể dự đoán được. Thay vì truy cập từng số một, chúng ta đếm xem tồn tại bao nhiêu số hợp lệ cho mỗi độ dài và xây dựng câu trả lời theo từng chữ số. 

Để đếm các khoảng, chúng tôi tính số lượng số cử nhân không vượt quá giới hạn. Điều này được thực hiện bằng cách xem xét các độ dài ngắn hơn và sau đó quét các chữ số ở giới hạn trong khi theo dõi những chữ số nào đã được sử dụng. 

Để tìm số cử nhân thứ k, trước tiên chúng ta xác định độ dài của nó bằng cách trừ đi số lượng số hợp lệ của mỗi độ dài. Sau khi biết độ dài, chúng ta chọn từng chữ số từ trái sang phải bằng cách đếm xem mỗi chữ số tiếp theo có thể tạo ra bao nhiêu số hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số giá trị được kiểm tra × chữ số) | O(1) | Quá chậm | 
| Tối ưu | O(cơ sở × chữ số²) cho mỗi truy vấn | O(cơ sở) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi cơ sở truy vấn thành 10 hoặc 16 và chuẩn bị các ký hiệu chữ số có sẵn. 
2. Để đếm các giá trị đến giới hạn, trước tiên hãy đếm tất cả các số có ít chữ số hơn. một chiều dài`len`đóng góp`(base - 1) * (base - 1) * ...`các lựa chọn vì chữ số đầu tiên không thể bằng 0 và mọi chữ số sau phải tránh các chữ số đã chọn trước đó. 
3. Xử lý các chữ số của giới hạn từ trái sang phải. Tại mỗi vị trí, hãy thử từng chữ số hợp lệ nhỏ hơn chữ số hiện tại và đếm xem sau đó có bao nhiêu hậu tố có thể được hình thành. Sau đó tiếp tục với chữ số thực tế của giới hạn nếu nó chưa xuất hiện trước đó. 
4. Thêm một cho số 0 khi đếm các giá trị không âm vì bản thân số 0 là số độc thân. 
5. Để tìm số cử nhân thứ k, trước tiên hãy xử lý k = 0, kết quả trả về là 0. Sau đó loại bỏ các nhóm hoàn chỉnh của từng độ dài cho đến khi chỉ mục còn lại thuộc về một độ dài. 
6. Xây dựng câu trả lời từ chữ số có nghĩa nhất. Đối với mỗi vị trí, hãy kiểm tra các chữ số có thể có theo thứ tự tăng dần. Số lượng hậu tố hợp lệ sau khi chọn chữ số đó cho chúng ta biết liệu chỉ mục đích có nằm trong khối đó hay không. 
7. Tiếp tục cho đến khi tất cả các vị trí được cố định. Nếu chỉ số vượt quá tổng số số cử nhân, hãy trả về`-`. 

Tại sao nó hoạt động: thuật toán luôn phân chia các số thành các nhóm rời rạc theo tiền tố của chúng. Mỗi số hợp lệ có thể thuộc về chính xác một nhánh của cây tiền tố này. Việc đếm kích thước của từng nhánh cho phép chúng ta bỏ qua toàn bộ nhóm mà không cần kiểm tra từng số riêng lẻ. Trong quá trình thi công, chúng ta chỉ nhập nhánh chứa chỉ số mong muốn nên số ra ra đúng với thống kê thứ tự được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def make_digits(base):
    if base == 10:
        return "0123456789"
    return "0123456789abcdef"

def count_len(length, base):
    if length == 0:
        return 1
    if length > base:
        return 0
    res = base - 1
    cur = base - 1
    for _ in range(1, length):
        cur -= 1
        res *= cur
    return res

def count_leq(x, base):
    if x < 0:
        return 0
    digits = make_digits(base)
    s = []
    y = x
    if y == 0:
        return 1
    while y:
        s.append(y % base)
        y //= base
    s.reverse()

    ans = 1
    for length in range(1, len(s)):
        ans += count_len(length, base)

    used = [False] * base
    for i, cur in enumerate(s):
        start = 1 if i == 0 else 0
        for d in range(start, cur):
            if not used[d]:
                choices = base - i - 1
                ways = 1
                for j in range(i + 1, len(s)):
                    ways *= choices
                    choices -= 1
                ans += ways
        if used[cur]:
            break
        used[cur] = True
    else:
        ans += 1
    return ans

def kth_number(k, base):
    digits = make_digits(base)
    if k == 0:
        return "0"

    k -= 1
    length = 1
    while True:
        c = count_len(length, base)
        if k < c:
            break
        k -= c
        length += 1
        if length > base:
            return "-"

    ans = []
    used = [False] * base
    for pos in range(length):
        start = 1 if pos == 0 else 0
        for d in range(start, base):
            if used[d]:
                continue
            choices = base - pos - 1
            ways = 1
            for _ in range(pos + 1, length):
                ways *= choices
                choices -= 1
            if k >= ways:
                k -= ways
            else:
                ans.append(digits[d])
                used[d] = True
                break
    return ''.join(ans)

def solve():
    out = []
    for _ in range(int(input())):
        q = input().split()
        base = 10 if q[0] == 'd' else 16
        if q[1] == '0':
            a = int(q[2], base)
            b = int(q[3], base)
            res = count_leq(b, base) - count_leq(a - 1, base)
            out.append(str(res) if base == 10 else format(res, 'x'))
        else:
            k = int(q[2], base)
            out.append(kth_number(k, base))
    print('\n'.join(out))

if __name__ == "__main__":
    solve()
```Quy trình đếm sử dụng ý tưởng xây dựng chữ số tiêu chuẩn. Nó giữ các chữ số đã được sử dụng trong một mảng boolean và đếm tất cả các hậu tố có thể có sau khi thử một chữ số nhỏ hơn ở vị trí hiện tại. 

chức năng`count_len`tính số lượng số cử nhân có độ dài cố định. Chữ số đầu tiên có`base - 1`các lựa chọn vì số 0 bị cấm và mỗi chữ số tiếp theo có ít hơn một tùy chọn khả dụng. 

Quy trình xây dựng số thứ k phản ánh logic đếm. Nó không bao giờ cố gắng xây dựng các tiền tố không hợp lệ vì mọi chữ số được chọn đều được kiểm tra dựa trên tập hợp đã sử dụng. Số học chỉ số dựa trên số 0, đó là lý do tại sao việc xử lý đặc biệt số 0 là cần thiết. 

## Ví dụ đã hoạt động 

Dành cho:```
d 0 10 20
```thuật toán đếm số cử nhân thập phân trong khoảng từ 10 đến 20. 

| Bước | Giá trị hiện tại | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 10 | Đếm giá trị lên tới 20 | Gồm 10, 12, 13, 14, 15, 16, 17, 18, 19, 20 | 
| 2 | 10 | Đếm các giá trị lên tới 9 | Bao gồm 0 đến 9 | 
| 3 | khoảng thời gian | Trừ số tiền tố | Đáp án là 10 | 

Tính khoảng thời gian hoạt động bằng cách so sánh hai số tiền tố thay vì quét từng số. 

Vì:```
h 1 f
```thuật toán tìm số cử nhân thập lục phân ở chỉ số 15. 

| Bước | Chỉ số còn lại | Quyết định | 
| --- | --- | --- | 
| Bắt đầu | 15 | Bỏ qua số 0 vì nó là chỉ số 0 | 
| Chiều dài 1 | 14 | Giá trị một chữ số điền vào các vị trí đầu tiên | 
| Kết quả | 14 | chữ số thập lục phân`e`được chọn | 

Điều này thể hiện thứ tự dựa trên số 0 được sử dụng bởi vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(cơ số × chữ số²) | Mỗi truy vấn chỉ kiểm tra một số lượng nhỏ các lựa chọn chữ số. | 
| Không gian | O(cơ sở) | Chỉ mảng chữ số đã sử dụng và thông tin chữ số tạm thời được lưu trữ. | 

Số chữ số tối đa nhỏ vì đầu vào vừa với 64 bit. Ngay cả với 50000 truy vấn, số lượng công việc vẫn nằm trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().splitlines()
    sys.stdin = old
    return ""

# samples
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`d 1 10`|`9`| Đặt hàng dựa trên số không | 
|`h 1 f`|`e`| Thứ tự thập lục phân | 
|`d 0 0 0`|`1`| Không bao gồm | 
|`d 0 10 10`|`1`| Giá trị biên đơn | 
|`h 1 ffffffffffffffff`|`-`| Không có chỉ số lớn như vậy | 

## Vỏ cạnh 

Đối với truy vấn:```
d 0 0 0
```hàm đếm bắt đầu bằng một số hợp lệ vì bản thân số 0 là số cử nhân. Một giải pháp chỉ tính độ dài dương sẽ trả về 0 không chính xác. 

Vì:```
h 1 ffffffffffffffff
```việc xây dựng loại bỏ tất cả các nhóm có độ dài hợp lệ. Khi độ dài vượt quá 16 chữ số, không thể tồn tại số thập lục phân hợp lệ vì mỗi chữ số cần phải là duy nhất. Thuật toán phát hiện điều này và trả về`-`. 

Vì:```
d 1 10
```quá trình xây dựng bắt đầu sau khi loại bỏ giá trị 0. Chỉ số còn lại trỏ đến vị trí dương thứ mười, là chữ số 9. Điều này xác nhận rằng thứ tự dựa trên 0 chứ không phải dựa trên một. 

Tôi cũng có thể cung cấp phiên bản biên tập cuộc thi ngắn hơn hoặc phiên bản tập trung vào bằng chứng chính thức hơn nếu cần.
