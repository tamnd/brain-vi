---
title: "CF 102800A - Hợp âm"
description: "Mỗi trường hợp thử nghiệm mô tả ba nốt nhạc đã được sắp xếp từ cao độ thấp nhất đến cao nhất."
date: "2026-07-27T17:35:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "A"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 60
verified: true
draft: false
---

[CF 102800A - Hợp âm](https://codeforces.com/problemset/problem/102800/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả ba nốt nhạc đã được sắp xếp từ cao độ thấp nhất đến cao nhất. Các nốt có thể thuộc các quãng tám khác nhau, nhưng đầu vào đảm bảo rằng khoảng cách từ nốt đầu tiên đến nốt thứ hai và từ nốt thứ hai đến nốt thứ ba tối đa là 11 nửa cung. Nhiệm vụ của chúng ta là xác định xem ba nốt này có tạo thành hợp âm ba trưởng, hợp âm thứ hay không. 

Thông tin duy nhất quan trọng là số nửa cung giữa các nốt liên tiếp. Một hợp âm ba trưởng có các quãng gồm 4 nửa cung, tiếp theo là 3 nửa cung. Một hợp âm ba thứ có các quãng gồm 3 nửa cung, tiếp theo là 4 nửa cung. Bất kỳ cặp khoảng thời gian nào khác phải được báo cáo là`Dissonance`. 

Số lượng trường hợp thử nghiệm nhiều nhất là 2000, rất ít. Mỗi trường hợp thử nghiệm chỉ chứa ba ghi chú, do đó, ngay cả việc xử lý thời gian liên tục cho mỗi trường hợp cũng là quá đủ. Giải pháp chỉ cần ánh xạ từ tên nốt đến vị trí của chúng trong phạm vi quãng tám và một vài phép tính số học. 

Trường hợp tinh tế đầu tiên là khi các nốt vượt qua ranh giới quãng tám. Ví dụ,```
A C E
```Đầu ra đúng là```
Minor triad
```Ghi chú`C`có chỉ số nhỏ hơn`A`trong một quãng tám nhưng thực tế nó nằm ở quãng tám tiếp theo. Chỉ cần trừ các chỉ số quãng tám sẽ tạo ra giá trị âm. Việc tính quãng phải cộng thêm 12 bất cứ khi nào chỉ số của nốt tiếp theo nhỏ hơn. 

Một sai lầm dễ mắc phải khác là cho rằng mọi chuỗi nốt tăng dần đều có thể được phân loại là một hợp âm. Ví dụ,```
C F A
```Các quãng là 5 và 4 nửa cung, không khớp với định nghĩa hợp âm nào. Đầu ra đúng là```
Dissonance
```Trường hợp cạnh cuối cùng là khi đầu vào giảm dần về mặt âm nhạc về tên nốt nhưng vẫn tăng dần về cao độ do quãng tám.```
E D C
```Đầu ra đúng là```
Dissonance
```Mặc dù`D`Và`C`xuất hiện sớm hơn trong thang màu, chúng thuộc về quãng tám cao hơn. Giải pháp bỏ qua gói quãng tám sẽ tính toán khoảng cách âm không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng trực tiếp định nghĩa âm nhạc. Chuyển đổi mọi nốt sang vị trí của nó trong một quãng tám, tính khoảng cách nửa cung giữa các nốt liên tiếp, điều chỉnh gói quãng tám bất cứ khi nào cần thiết, sau đó so sánh hai khoảng cách với hai mẫu hợp lệ. 

Người ta cũng có thể tưởng tượng việc tạo ra mọi bộ ba chính và bộ ba phụ có thể có và kiểm tra xem dữ liệu đầu vào có khớp với một trong số chúng hay không. Vì chỉ có 12 gốc có thể có và hai loại hợp âm nên điều này vẫn chỉ cần 24 mẫu và đủ nhanh. Mặc dù đúng nhưng nó lại đưa ra các bước tiền xử lý và so sánh không cần thiết. 

Việc tính toán khoảng thời gian trực tiếp đơn giản hơn vì định nghĩa bài toán đã được biểu diễn dưới dạng khoảng cách nửa cung liên tiếp. Khi mỗi ghi chú được ánh xạ tới một số nguyên từ 0 đến 11, mọi trường hợp thử nghiệm sẽ giảm xuống còn hai phép tính khoảng và hai phép so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(24) cho mỗi trường hợp thử nghiệm | O(24) | Được chấp nhận, nhưng không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ánh xạ từ mỗi tên nốt tới vị trí của nó trong thang màu. 
2. Đọc ba nốt nhạc và chuyển chúng sang vị trí số. 
3. Tính quãng từ nốt đầu tiên đến nốt thứ hai. Nếu vị trí thứ hai nhỏ hơn, hãy cộng 12 trước khi trừ để khoảng thời gian biểu thị việc di chuyển lên quãng tám tiếp theo. 
4. Tính khoảng thứ hai theo cách tương tự. 
5. Nếu hai khoảng đó là`(4, 3)`, in`Major triad`. 
6. Mặt khác, nếu các khoảng thời gian là`(3, 4)`, in`Minor triad`. 
7. Nếu không, hãy in`Dissonance`. 

### Tại sao nó hoạt động 

Đầu vào đảm bảo rằng các nốt đã được sắp xếp từ cao độ thấp hơn đến cao độ cao hơn. Điều phức tạp duy nhất là việc di chuyển lên trên có thể vượt qua ranh giới quãng tám. Việc thêm 12 bất cứ khi nào giá trị nốt số giảm đi sẽ tái tạo lại khoảng cách nửa cung hướng lên thực tế. Sau khi tính toán hai khoảng cách đó, loại hợp âm được xác định hoàn toàn bằng định nghĩa bài toán. Vì mọi đầu vào có thể tạo ra chính xác một cặp quãng liên tiếp nên thuật toán luôn phân loại hợp âm một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

pos = {
    "C": 0,
    "C#": 1,
    "D": 2,
    "D#": 3,
    "E": 4,
    "F": 5,
    "F#": 6,
    "G": 7,
    "G#": 8,
    "A": 9,
    "A#": 10,
    "B": 11,
}

def interval(a, b):
    if b < a:
        b += 12
    return b - a

t = int(input())

for _ in range(t):
    n1, n2, n3 = input().split()

    x = pos[n1]
    y = pos[n2]
    z = pos[n3]

    d1 = interval(x, y)
    d2 = interval(y, z)

    if d1 == 4 and d2 == 3:
        print("Major triad")
    elif d1 == 3 and d2 == 4:
        print("Minor triad")
    else:
        print("Dissonance")
```Từ điển lưu trữ thang màu dưới dạng số nguyên từ 0 đến 11. Điều này làm cho phép tính khoảng trở thành phép trừ số nguyên đơn giản thay vì xử lý chuỗi. 

Hàm trợ giúp xử lý phần khó khăn duy nhất trong quá trình triển khai. Khi nốt thứ hai có giá trị số nhỏ hơn, nó thuộc quãng tám tiếp theo, do đó, việc cộng 12 sẽ tạo lại khoảng cách đi lên chính xác. Vì câu lệnh đảm bảo rằng mỗi quãng có nhiều nhất là 11 nửa cung nên việc điều chỉnh này luôn là đủ. 

Mỗi trường hợp thử nghiệm tính toán hai khoảng và so sánh chúng với hai mẫu hợp lệ duy nhất. Không có vấn đề gì vì thang màu được biểu thị bằng các số nguyên liên tiếp và mỗi khoảng được đo trực tiếp dưới dạng chênh lệch giữa các vị trí sau khi điều chỉnh quãng tám. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
C E G
```| Lưu ý | Giá trị số | 
| --- | --- | 
| C | 0 | 
| E | 4 | 
| G | 7 | 

| d1 | d2 | Kết quả | 
| --- | --- | --- | 
| 4 | 3 | Bộ ba chính | 

Các quãng khớp chính xác với định nghĩa của bộ ba chính, do đó thuật toán sẽ in ra`Major triad`. 

### Ví dụ 2 

đầu vào:```
A C E
```| Lưu ý | Giá trị số | 
| --- | --- | 
| A | 9 | 
| C | 0 | 
| E | 4 | 

| d1 | d2 | Kết quả | 
| --- | --- | --- | 
| (12 + 0) - 9 = 3 | 4 | Bộ ba nhỏ | 

Ví dụ này chứng minh tại sao việc điều chỉnh quãng tám là cần thiết. Nếu không thêm 12, khoảng đầu tiên sẽ trở thành không chính xác`-9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện công việc liên tục. | 
| Không gian | O(1) | Chỉ có ánh xạ ghi chú cố định và một vài biến được lưu trữ. | 

Ngay cả với tối đa 2000 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ. Giải pháp dễ dàng thỏa mãn các giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    pos = {
        "C": 0,
        "C#": 1,
        "D": 2,
        "D#": 3,
        "E": 4,
        "F": 5,
        "F#": 6,
        "G": 7,
        "G#": 8,
        "A": 9,
        "A#": 10,
        "B": 11,
    }

    def interval(a, b):
        if b < a:
            b += 12
        return b - a

    t = int(input())
    out = []

    for _ in range(t):
        a, b, c = input().split()
        d1 = interval(pos[a], pos[b])
        d2 = interval(pos[b], pos[c])

        if d1 == 4 and d2 == 3:
            out.append("Major triad")
        elif d1 == 3 and d2 == 4:
            out.append("Minor triad")
        else:
            out.append("Dissonance")

    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    ans = sys.stdout.getvalue()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return ans

assert run(
"""5
C E G
A C E
B D F#
C F A
E D C
"""
) == (
"""Major triad
Minor triad
Minor triad
Dissonance
Dissonance
"""
)

assert run(
"""1
C E G
"""
) == (
"""Major triad
"""
)

assert run(
"""1
A C E
"""
) == (
"""Minor triad
"""
)

assert run(
"""1
C F A
"""
) == (
"""Dissonance
"""
)

assert run(
"""1
B D# F#
"""
) == (
"""Major triad
"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`C E G`|`Major triad`| Hợp âm trưởng chuẩn. | 
|`A C E`|`Minor triad`| Xử lý đúng gói quãng tám. | 
|`C F A`|`Dissonance`| Mẫu khoảng thời gian không hợp lệ. | 
|`B D# F#`|`Major triad`| Hợp âm trưởng vượt qua ranh giới quãng tám. | 

## Vỏ cạnh 

Xem xét đầu vào```
1
A C E
```Các giá trị được ánh xạ là`9`,`0`, Và`4`. Thuật toán phát hiện ra rằng`0 < 9`, cộng 12 và tính khoảng đầu tiên là`3`. Khoảng thứ hai là`4`. Vì cặp khoảng là`(3, 4)`, đầu ra là```
Minor triad
```Điều này xử lý chính xác gói quãng tám. 

Bây giờ hãy xem xét```
1
C F A
```Các giá trị được ánh xạ là`0`,`5`, Và`9`. Các khoảng trở thành`(5, 4)`. Không có mẫu hợp âm hợp lệ nào khớp, do đó thuật toán sẽ in ra```
Dissonance
```Điều này xác nhận rằng chỉ có hai chuỗi khoảng thời gian chính xác được chấp nhận. 

Cuối cùng, hãy xem xét```
1
E D C
```Các giá trị được ánh xạ là`4`,`2`, Và`0`. Thuật toán tính toán các khoảng thời gian như`(10, 10)`sau khi điều chỉnh quãng tám. Không có cặp nào phù hợp`(4, 3)`hoặc`(3, 4)`, vì vậy đầu ra là```
Dissonance
```Điều này chứng tỏ rằng tên nốt giảm dần không gây nhầm lẫn cho việc tính toán quãng vì việc gói quãng tám được xử lý rõ ràng.
