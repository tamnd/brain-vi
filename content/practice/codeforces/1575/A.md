---
title: "CF 1575A - Một vấn đề sắp xếp khác"
description: "Chúng tôi có một bộ sưu tập các tựa sách riêng biệt, tất cả đều có cùng độ dài. Các cuốn sách không được sắp xếp theo thứ tự từ điển thông thường. Khi so sánh hai tiêu đề, chúng tôi quét từ trái sang phải cho đến khi tìm thấy vị trí đầu tiên mà chúng khác nhau."
date: "2026-06-10T11:01:24+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1575
codeforces_index: "A"
codeforces_contest_name: "COMPFEST 13 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1100
weight: 1575
solve_time_s: 767
verified: false
draft: false
---

[CF 1575A - Một vấn đề sắp xếp khác](https://codeforces.com/problemset/problem/1575/A) 

**Đánh giá:** 1100 
**Thẻ:** cấu trúc dữ liệu, sắp xếp, chuỗi 
**Thời gian giải:** 12 phút 47 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các tựa sách riêng biệt, tất cả đều có cùng độ dài. Các cuốn sách không được sắp xếp theo thứ tự từ điển thông thường. 

Khi so sánh hai tiêu đề, chúng tôi quét từ trái sang phải cho đến khi tìm thấy vị trí đầu tiên mà chúng khác nhau. Nếu vị trí đó là số lẻ thì chữ cái nhỏ hơn sẽ đứng trước. Nếu vị trí đó là số chẵn thì chữ cái lớn hơn sẽ đứng trước. 

Nhiệm vụ là xuất ra các chỉ mục gốc của sách sau khi sắp xếp chúng theo thứ tự tùy chỉnh này. 

Ràng buộc$n \cdot m \le 10^6$là quan sát chính. Tổng số lượng dữ liệu chuỗi tối đa là một triệu ký tự. Bất kỳ giải pháp nào liên tục so sánh các chuỗi theo từng ký tự trong bộ so sánh tùy chỉnh đều có thể trở nên tốn kém. Giải pháp chuyển đổi từng chuỗi một lần rồi sử dụng tính năng sắp xếp tích hợp sẵn của ngôn ngữ sẽ phù hợp một cách thoải mái trong giới hạn. Phức tạp xung quanh$O(nm)$tiền xử lý cộng$O(n \log n)$việc phân loại là hoàn toàn có thể chấp nhận được. 

Một lỗi phổ biến là so sánh các vị trí chẵn không chính xác. Tuyên bố sử dụng các vị trí dựa trên 1. Vị trí 2, 4, 6,... đang giảm dần. Trong Python, chuỗi dựa trên 0, vì vậy các chỉ số 1, 3, 5, ... tương ứng với các vị trí chẵn. 

Coi như:```
2 2
AA
AB
```Ở vị trí thứ 2, chúng ta so sánh giảm dần. Từ$B > A$, chuỗi`AB`nên đến trước`AA`. Việc coi vị trí 2 là tăng dần sẽ tạo ra thứ tự sai. 

Một trường hợp tinh tế khác xảy ra khi sai phân đầu tiên xuất hiện ở vị trí chẵn:```
2 3
ABC
AAC
```Sự khác biệt đầu tiên là ở vị trí 2. Vì vị trí 2 giảm dần nên`ABC`đến trước`AAC`, mặc dù thứ tự từ điển thông thường sẽ đặt`AAC`Đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thực hiện trực tiếp quy tắc so sánh và sắp xếp bằng cách sử dụng bộ so sánh đó. Mỗi so sánh có thể quét tới$m$nhân vật. Yêu cầu sắp xếp$O(n \log n)$so sánh, cho$O(nm \log n)$thời gian. Với tổng số một triệu ký tự, điều này vẫn có thể nặng hơn mức cần thiết, đặc biệt là ở những ngôn ngữ có bộ so sánh tùy chỉnh đắt tiền. 

Cấu trúc của thứ tự gợi ý một ý tưởng rõ ràng hơn. Tại các vị trí lẻ, chúng tôi muốn thứ tự bảng chữ cái thông thường. Ở các vị trí chẵn, chúng tôi muốn thứ tự bảng chữ cái đảo ngược. 

Giả sử chúng ta biến đổi mọi ký tự ở vị trí chẵn. Thay thế$$A \leftrightarrow Z,\quad B \leftrightarrow Y,\quad C \leftrightarrow X,\ldots$$Sau phép biến đổi này, ký tự gốc lớn hơn ở vị trí chẵn sẽ trở thành ký tự được biến đổi nhỏ hơn. So sánh giảm dần ở các vị trí chẵn được chuyển thành so sánh tăng dần thông thường. 

Khi mọi chuỗi được chuyển đổi theo cách này, thứ tự tùy chỉnh sẽ trở thành thứ tự từ điển chuẩn chính xác trên các chuỗi được chuyển đổi. Sau đó chúng ta có thể sắp xếp bình thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bộ so sánh Brute Force |$O(nm \log n)$|$O(1)$thêm | Được chấp nhận nhưng không cần thiết | 
| Biến đổi + sắp xếp thông thường |$O(nm + n\log n)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các chuỗi và ghi nhớ các chỉ số ban đầu của chúng. 
2. Đối với mỗi chuỗi, hãy xây dựng một phiên bản được chuyển đổi. 
3. Giữ nguyên các ký tự ở vị trí lẻ. 
4. Đối với các ký tự ở vị trí chẵn, thay thế chúng bằng gương bảng chữ cái của chúng:$$c \mapsto \text{'Z'} - (c-\text{'A'})$$Điều này chuyển đổi so sánh giảm dần thành so sánh tăng dần. 
5. Bảo quản cặp`(transformed_string, original_index)`. 
6. Sắp xếp tất cả các cặp theo chuỗi đã chuyển đổi. 
7. Xuất các chỉ số gốc theo thứ tự đã sắp xếp. 

### Tại sao nó hoạt động 

Lấy hai chuỗi$a$Và$b$. Cho phép$k$là vị trí đầu tiên mà chúng khác nhau. 

Nếu như$k$là số lẻ, phép biến đổi giữ nguyên cả hai ký tự. Thứ tự của chúng trong so sánh từ điển thông thường chính xác là thứ tự mà bài toán yêu cầu. 

Nếu như$k$chẵn, phép biến đổi sẽ thay thế từng ký tự bằng gương của nó trong bảng chữ cái. Ký tự gốc lớn hơn sẽ trở thành ký tự được biến đổi nhỏ hơn. Do đó, so sánh giảm dần mà bài toán yêu cầu sẽ trở thành so sánh tăng dần sau khi chuyển đổi. 

Vị trí khác nhau đầu tiên vẫn giữ nguyên, do đó các chuỗi được chuyển đổi so sánh theo thứ tự từ điển thông thường một cách chính xác khi các chuỗi ban đầu so sánh theo thứ tự kết thúc tăng dần. Do đó, việc sắp xếp các chuỗi biến đổi sẽ tạo ra thứ tự chính xác được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def transform(s):
    chars = list(s)
    for i in range(1, len(chars), 2):  # 0-based indices => even positions in statement
        chars[i] = chr(ord('Z') - (ord(chars[i]) - ord('A')))
    return ''.join(chars)

def solve():
    n, m = map(int, input().split())

    arr = []
    for idx in range(1, n + 1):
        s = input().strip()
        arr.append((transform(s), idx))

    arr.sort()

    print(*[idx for _, idx in arr])

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý tạo ra cách thể hiện được biến đổi của mọi tiêu đề. Vòng lặp bắt đầu tại chỉ mục`1`bởi vì Python sử dụng lập chỉ mục dựa trên 0 và các vị trí chẵn của vấn đề tương ứng với các chỉ mục`1, 3, 5, ...`. 

Việc chuyển đổi sử dụng gương bảng chữ cái. Ví dụ,`A`trở thành`Z`,`B`trở thành`Y`, Và`Z`trở thành`A`. 

Sau khi chuyển đổi, không cần bộ so sánh tùy chỉnh. Thứ tự chuỗi gốc của Python đã khớp với thứ tự mong muốn. Đầu ra cuối cùng sử dụng các chỉ số gốc được lưu trữ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
AA
AB
BB
BA
AZ
```Chuyển đổi: 

| Bản gốc | Chuyển đổi | Chỉ mục | 
| --- | --- | --- | 
| AA | AZ | 1 | 
| AB | AY | 2 | 
| BB | BỞI | 3 | 
| BA | BZ | 4 | 
| AZ | AA | 5 | 

Sắp xếp theo chuỗi được chuyển đổi: 

| Xếp hạng | Chuyển đổi | Chỉ mục | 
| --- | --- | --- | 
| 1 | AA | 5 | 
| 2 | AY | 2 | 
| 3 | AZ | 1 | 
| 4 | BỞI | 3 | 
| 5 | BZ | 4 | 

Đầu ra:```
5 2 1 3 4
```Điều này cho thấy cách so sánh giảm dần ở vị trí 2 được chuyển đổi thành sắp xếp tăng dần thông thường. 

### Ví dụ 2 

đầu vào:```
3 3
ABC
AAC
BAC
```Chuyển đổi: 

| Bản gốc | Chuyển đổi | 
| --- | --- | 
| ABC | AYC | 
| AAC | AZC | 
| BẮC | BZC | 

Các chuỗi biến đổi được sắp xếp: 

| Chuyển đổi | Chỉ mục gốc | 
| --- | --- | 
| AYC | 1 | 
| AZC | 2 | 
| BZC | 3 | 

Đầu ra:```
1 2 3
```Hai chuỗi đầu tiên khác nhau ở vị trí 2. Vì vị trí 2 giảm dần nên`B`nên đến trước`A`, đó chính xác là những gì thứ tự chuyển đổi đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm + n\log n)$| Chuyển đổi tất cả các ký tự một lần, sau đó sắp xếp$n$phím | 
| Không gian |$O(nm)$| Lưu trữ các chuỗi đã chuyển đổi | 

Tổng kích thước đầu vào tối đa là$10^6$nhân vật. Xử lý mỗi ký tự một lần là không tốn kém. Sắp xếp$n$phím chuyển đổi dễ dàng phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())

    arr = []
    for idx in range(1, n + 1):
        s = input().strip()

        chars = list(s)
        for i in range(1, len(chars), 2):
            chars[i] = chr(ord('Z') - (ord(chars[i]) - ord('A')))

        arr.append(("".join(chars), idx))

    arr.sort()
    return " ".join(str(idx) for _, idx in arr)

# provided sample
assert solve_io(
"""5 2
AA
AB
BB
BA
AZ
"""
) == "5 2 1 3 4"

# minimum size
assert solve_io(
"""1 1
A
"""
) == "1"

# first difference at even position
assert solve_io(
"""2 2
AA
AB
"""
) == "2 1"

# odd-position comparison dominates
assert solve_io(
"""3 2
BA
AZ
BZ
"""
) == "2 1 3"

# longer strings
assert solve_io(
"""3 3
ABC
AAC
BAC
"""
) == "1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi đơn |`1`| Kích thước đầu vào tối thiểu | 
|`AA`,`AB`|`2 1`| Xử lý đúng các vị trí chẵn giảm dần | 
|`BA`,`AZ`,`BZ`|`2 1 3`| Vị trí lẻ vẫn sử dụng thứ tự tăng dần bình thường | 
| Ba dây dài 3 |`1 2 3`| Hành vi vị trí chẵn/lẻ hỗn hợp | 

## Vỏ cạnh 

Xem xét đầu vào```
2 2
AA
AB
```Sự khác biệt đầu tiên xảy ra ở vị trí 2. Quy tắc có thứ tự giảm dần, vì vậy`AB`phải đến trước`AA`. Bản đồ chuyển đổi`AA -> AZ`Và`AB -> AY`. Từ`AY < AZ`, việc sắp xếp thông thường sẽ trả về thứ tự đúng. 

Coi như```
2 3
ABC
AAC
```Sự khác biệt đầu tiên xảy ra ở vị trí 2. Sự so sánh ban đầu là`B`so với`A`, và vị trí 2 đang giảm dần. Sau khi chuyển đổi, các chuỗi trở thành`AYC`Và`AZC`. Việc so sánh được chuyển đổi bây giờ diễn ra theo thứ tự tăng dần thông thường và vẫn đặt chuỗi chính xác lên đầu tiên. 

Coi như```
2 4
AZAZ
AZAY
```Sự khác biệt đầu tiên xuất hiện ở vị trí 4, một vị trí giảm dần khác. Phép biến đổi chỉ thay đổi vị trí 2 và 4, giữ nguyên vị trí của ký tự khác nhau đầu tiên. Thứ tự vẫn đúng vì mọi so sánh ở vị trí chẵn đều bị đảo ngược một cách nhất quán.
