---
title: "CF 102388B - Sao"
description: "Một ngôi sao tồn tại ở mọi điểm có hai tọa độ là số nguyên. Đối với mỗi trường hợp thử nghiệm, chúng ta có hai điểm cuối của một đoạn thẳng và chúng ta cần đếm mọi điểm tọa độ nguyên nằm trên đoạn đó, bao gồm cả cả hai điểm cuối."
date: "2026-08-16T08:52:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102388
codeforces_index: "B"
codeforces_contest_name: "SUFE ICPC Team Formation Test"
rating: 0
weight: 102388
solve_time_s: 107
verified: false
draft: false
---

[CF 102388B - Sao](https://codeforces.com/problemset/problem/102388/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Một ngôi sao tồn tại ở mọi điểm có hai tọa độ là số nguyên. Đối với mỗi trường hợp thử nghiệm, chúng ta có hai điểm cuối của một đoạn thẳng và chúng ta cần đếm mọi điểm tọa độ nguyên nằm trên đoạn đó, bao gồm cả cả hai điểm cuối. 

Tọa độ có thể lớn bằng (10^9) theo cả hai hướng. Do đó, sự khác biệt giữa hai tọa độ điểm cuối có thể đạt tới (2\cdot10^9). Do đó, một giải pháp truy cập mọi tọa độ có thể có dọc theo phân đoạn có thể yêu cầu khoảng hai tỷ lần lặp chỉ cho một trường hợp thử nghiệm. Vì có thể có tới 100 ca kiểm thử và giới hạn thời gian chỉ là một giây nên giải pháp dự định phải thực hiện một lượng số học không đổi cho mỗi ca kiểm thử thay vì phụ thuộc vào độ dài hình học của đoạn. 

Có một số trường hợp một công thức trực tiếp có thể dễ dàng mắc sai lầm. Ví dụ: nếu cả hai điểm cuối giống hệt nhau```
1
5 -3 5 -3
```câu trả lời là`1`, bởi vì đoạn này bao gồm một ngôi sao. Một công thức như`gcd(dx, dy)`không có trận chung kết`+1`sẽ trả về không chính xác bằng 0. 

Đoạn ngang là một trường hợp ranh giới hữu ích khác:```
1
2 7 6 7
```Các ngôi sao được che phủ là ((2,7),(3,7),(4,7),(5,7),(6,7)), vì vậy câu trả lời là`5`. Trường hợp dọc hoạt động giống hệt nhau. Việc triển khai bất cẩn khi chia cho độ dốc có thể dẫn đến chia cho 0, mặc dù số lượng điểm mạng được xác định hoàn toàn rõ ràng. 

Tọa độ âm cũng không yêu cầu xử lý hình học đặc biệt. Vì```
1
-2 -2 2 2
```các điểm mạng là ((-2,-2),(-1,-1),(0,0),(1,1),(2,2)), cho`5`. Hiệu có thể âm nên ước số chung lớn nhất phải được tính từ giá trị tuyệt đối của chúng. 

Cuối cùng, một đoạn có thể có chênh lệch tọa độ rất lớn nhưng chỉ chứa hai điểm cuối của nó. Ví dụ,```
1
0 0 1000000000 1
```có câu trả lời`2`, vì tọa độ thay đổi có gcd (1). Giải pháp dựa trên độ dài đoạn sẽ hoàn toàn bỏ qua sự khác biệt này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê các ứng cử viên có tọa độ nguyên dọc theo đoạn. Ví dụ: nếu các điểm cuối có tọa độ (x) khác nhau, chúng ta có thể kiểm tra mọi số nguyên (x) giữa chúng, xác định điểm tương ứng trên dòng và đếm điểm đó khi tọa độ (y) của nó cũng là một số nguyên. Điều này đúng vì mỗi điểm mạng trên một đoạn không thẳng đứng có chính xác một tọa độ nguyên (x), do đó mọi điểm mạng có thể đều được xem xét. 

Vấn đề là số lần lặp lại. Một phân đoạn từ ((-10^9,-10^9)) đến ((10^9,10^9)) chứa (2\cdot10^9+1) điểm mạng và việc quét theo tọa độ có thể yêu cầu khoảng hai tỷ lần lặp cho trường hợp thử nghiệm đó. Với 100 trường hợp kiểm thử, trường hợp xấu nhất đạt tới khoảng (2\cdot10^{11}) lần lặp, vượt xa giới hạn một giây. 

Quan sát quan trọng là các điểm mạng trên một đoạn được cách đều nhau theo các bước tọa độ nguyên. hãy để 

[ 
dx = x_1-x_0,\qquad dy=y_1-y_0. 
] 

Giả sử đạt được điểm mạng từ điểm cuối đầu tiên bằng cách di chuyển (k) các bước nguyên giống hệt nhau. Bước này phải có dạng 

[ 
\left(\frac{dx}{g},\frac{dy}{g}\right), 
] 

ở đâu 

[ 
g=\gcd(|dx|,|dy|). 
] 

Đây là bước số nguyên nhỏ nhất vẫn đạt đến điểm cuối khác. Bắt đầu từ điểm cuối đầu tiên, chúng ta có thể thực hiện chính xác (g) các bước như vậy để đến điểm cuối thứ hai. Do đó, số điểm đã ghé thăm là (g+1), vì điểm bắt đầu cũng là một ngôi sao. 

Ví dụ: từ ((1,2)) đến ((3,6)), hiệu tọa độ là ((2,4)). gcd của họ là (2), vì vậy bước nguyên thủy là ((1,2)). Các điểm là ((1,2)), ((2,4)) và ((3,6)), cho (2+1=3). 

Phương pháp brute-force hoạt động vì nó tìm kiếm rõ ràng các điểm nguyên này, nhưng không thành công khi phạm vi tọa độ quá lớn. Việc quan sát thấy tất cả các điểm mạng hợp lệ được tạo ra bằng cách áp dụng lặp đi lặp lại bước số nguyên nhỏ nhất sẽ làm giảm toàn bộ vấn đề xuống còn một phép tính gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(\max( | dx | , | dy | ))) | (O(1)) | Quá chậm | 
| Tối ưu | (O(\log(\max( | dx | , | dy | )))) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai điểm cuối ((x_0,y_0)) và ((x_1,y_1)), sau đó tính toán (dx=x_1-x_0) và (dy=y_1-y_0). Những khác biệt này mô tả đầy đủ khoảng cách di chuyển của phân khúc theo chiều ngang và chiều dọc. 
2. Lấy giá trị tuyệt đối của cả hai hiệu và tính toán (g=\gcd(|dx|,|dy|)). Gcd cho chúng ta biết có bao nhiêu bước tọa độ nguyên bằng nhau khớp chính xác với tổng độ dịch chuyển. 
3. Trả lại (g+1). Có (g) khoảng cách giữa các điểm mạng liên tiếp và một chuỗi các khoảng (g) chứa (g+1) điểm cuối. 
4. Nếu cả hai điểm cuối trùng nhau thì (dx=dy=0) và (\gcd(0,0)) được Python coi là (0)`math.gcd`. Công thức vẫn cho (0+1=1), đó chính xác là câu trả lời mong muốn. 

### Tại sao nó hoạt động 

Đặt (g=\gcd(|dx|,|dy|)). Vì (g) chia cả hai hiệu tọa độ, nên vectơ 

[ 
\left(\frac{dx}{g},\frac{dy}{g}\right) 
] 

có tọa độ nguyên. Việc lặp lại vectơ này (k) lần từ điểm cuối đầu tiên sẽ cho 

[ 
\left(x_0+k\frac{dx}{g},\ y_0+k\frac{dy}{g}\right) 
] 

với mọi số nguyên (k) từ (0) đến (g), do đó có ít nhất (g+1) điểm mạng trên đoạn thẳng. 

Bước này là nguyên thủy vì hai thành phần sau khi chia cho (g) là nguyên tố cùng nhau. Bước dương nhỏ hơn với tọa độ nguyên không thể di chuyển dọc theo cùng một đường mà vẫn đạt đến điểm cuối một cách chính xác. Do đó không có điểm mạng bổ sung giữa các điểm được tạo liên tiếp. Tập hợp đầy đủ các điểm mạng chính xác là các điểm (g+1) thu được cho (k=0,1,\ldots,g), chứng tỏ rằng câu trả lời là (\gcd(|dx|,|dy|)+1). 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        x0, y0, x1, y1 = map(int, input().split())

        dx = abs(x1 - x0)
        dy = abs(y1 - y0)

        ans.append(str(gcd(dx, dy) + 1))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc số lượng phân đoạn và lưu trữ mỗi câu trả lời dưới dạng một chuỗi để tất cả đầu ra có thể được ghi cùng một lúc. Điều này giữ cho chi phí đầu vào và đầu ra ở mức nhỏ, mặc dù tốc độ tính toán thực tế đã cực kỳ nhanh. 

Đối với mỗi đoạn, sự khác biệt tọa độ tuyệt đối được tính toán trước khi gọi`gcd`. Dấu hiệu của sự khác biệt xác định hướng của đoạn nhưng không ảnh hưởng đến số lượng điểm mạng nằm trên đó. 

sử dụng`gcd(dx, dy) + 1`cũng xử lý các đoạn ngang, dọc và có độ dài bằng 0 mà không có các nhánh riêng biệt. Đối với đoạn ngang,`dy`bằng 0 và gcd trở thành`dx`. Đối với một đoạn thẳng đứng, điều tương tự cũng xảy ra với`dx`bằng không. Đối với các điểm cuối giống hệt nhau, cả hai đối số đều bằng 0 và Python trả về`gcd(0, 0) = 0`. 

Số nguyên Python có độ chính xác tùy ý, do đó chênh lệch tọa độ lên tới (2\cdot10^9) và kết quả trả lời lên tới (2\cdot10^9+1) không yêu cầu xử lý tràn. Trong các ngôn ngữ có loại số nguyên có chiều rộng cố định, số nguyên 64 bit là quá đủ. 

các`+1`chỉ được áp dụng sau khi gcd được tính toán. Gcd đếm số bước bằng nhau giữa các điểm mạng, trong khi số lượng được yêu cầu đếm chính các điểm đó, do đó số điểm lớn hơn số bước. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên chứa một đoạn có độ dài bằng 0. Đối với đầu vào`1 1 1 1`, sự khác biệt đều bằng không. 

| (x_0) | (y_0) | (x_1) | (y_1) | (dx) | (dy) | gcd | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 | 

Gcd bằng 0 vì không có sự dịch chuyển nào cả. Phân đoạn vẫn chứa điểm cuối duy nhất của nó, vì vậy việc thêm một điểm cuối sẽ cho kết quả chính xác. 

Đối với đoạn mẫu từ ((1,2)) đến ((3,6)), tọa độ thay đổi là (2) và (4). gcd của chúng là (2), nghĩa là đoạn này có thể được chia thành hai bước nguyên thủy bằng nhau. 

| (x_0) | (y_0) | (x_1) | (y_1) | (dx) | (dy) | gcd | Bước nguyên thủy | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 6 | 2 | 4 | 2 | ((1,2)) | 3 | 

Bắt đầu từ ((1,2)), một bước nguyên thủy đạt đến ((2,4)) và một bước khác đạt tới ((3,6)). Ba ngôi sao thu được phù hợp với công thức (2+1=3). 

Mẫu thứ ba, ((-1,-1)) đến ((50,101)), chứng minh rằng dấu của tọa độ không quan trọng. 

| (x_0) | (y_0) | (x_1) | (y_1) | (dx) | (dy) | gcd | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| -1 | -1 | 50 | 101 | 51 | 102 | 51 | 52 | 

Bước nguyên thủy là ((1,2)), do đó có 51 khoảng bằng nhau và 52 điểm mạng. Việc lấy sự khác biệt tuyệt đối cho phép tính toán giống nhau bất kể hướng của đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T\log C)) | Mỗi trường hợp thử nghiệm thực hiện một phép tính gcd Euclide trên các giá trị nhiều nhất (2\cdot10^9), trong đó (C) là chênh lệch tọa độ tối đa. | 
| Không gian | (O(T)) | Việc triển khai lưu trữ các chuỗi đầu ra trước khi viết chúng. | 

Với (T\le100), thuật toán chỉ thực hiện vài nghìn phép tính số học trong trường hợp xấu nhất. Phạm vi tọa độ ảnh hưởng đến tính toán gcd logarit thay vì khiến quá trình quét tỷ lệ với độ dài phân đoạn, do đó giải pháp phù hợp trong giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def solve():
    input = sys.stdin.readline
    t = int(input())
    ans = []

    for _ in range(t):
        x0, y0, x1, y1 = map(int, input().split())
        ans.append(str(gcd(abs(x1 - x0), abs(y1 - y0)) + 1))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("""\
4
1 1 1 1
1 2 3 6
-1 -1 50 101
-10000 1 1000000000 0
""") == """\
1
3
52
2
""", "sample"

# Minimum-size and all-equal coordinates
assert run("""\
1
0 0 0 0
""") == "1\n", "zero-length segment"

# Horizontal and vertical segments
assert run("""\
2
2 7 6 7
-3 -5 -3 4
""") == """\
5
10
""", "horizontal and vertical segments"

# gcd = 1, despite a very large coordinate difference
assert run("""\
1
0 0 1000000000 1
""") == "2\n", "only endpoints are lattice points"

# Maximum possible displacement in both coordinates
assert run("""\
1
-1000000000 -1000000000 1000000000 1000000000
""") == "2000000001\n", "maximum-size diagonal"

# Negative direction and a nontrivial gcd
assert run("""\
1
10 10 -2 -2
""") == "7\n", "negative coordinate direction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 0`|`1`| Đoạn có độ dài bằng 0 và`gcd(0, 0)`. | 
|`2 7 6 7`|`5`| Phân đoạn ngang và số lượng điểm cuối. | 
|`-3 -5 -3 4`|`10`| Đoạn thẳng đứng có tọa độ âm. | 
|`0 0 1000000000 1`|`2`| Phạm vi lớn với gcd bằng 1, phát hiện các lỗi dựa trên độ dài. | 
|`-1000000000 -1000000000 1000000000 1000000000`|`2000000001`| Sự khác biệt tọa độ tối đa và câu trả lời lớn. | 
|`10 10 -2 -2`|`7`| Hướng âm và gcd lớn hơn một. | 

## Vỏ cạnh 

Đối với một đoạn có độ dài bằng 0, chẳng hạn như```
1
5 -3 5 -3
```thuật toán tính toán (dx=0) và (dy=0). gcd của Python trả về 0, vì vậy giá trị cuối cùng là (0+1=1). Điểm cuối duy nhất chính xác là một điểm lưới, do đó không cần có trường hợp đặc biệt nào trong quá trình triển khai. 

Đối với một đoạn ngang như```
1
2 7 6 7
```sự khác biệt là (dx=4) và (dy=0). Gcd là (4) và câu trả lời là (5). Bước cơ bản là ((1,0)), tạo ra năm ngôi sao từ (x=2) đến (x=6). Một đoạn thẳng đứng hoạt động thông qua phép tính tương tự với vai trò của (x) và (y) được hoán đổi. 

Đối với một phân đoạn có các điểm cuối có chênh lệch tọa độ rất lớn nhưng có chênh lệch tọa độ là nguyên tố cùng nhau,```
1
0 0 1000000000 1
```gcd là (1), nên câu trả lời là (2). Các điểm mạng duy nhất là hai điểm cuối. Trường hợp này chứng minh tại sao số điểm mạng phụ thuộc vào gcd của hiệu tọa độ, chứ không phụ thuộc vào độ dài Euclide hoặc độ lớn của riêng từng hiệu tọa độ. 

Đối với tọa độ âm và hướng ngược lại,```
1
10 10 -2 -2
```sự khác biệt thô là (-12) và (-12), nhưng giá trị tuyệt đối của chúng có gcd (12). Thuật toán trả về (13), tương ứng với các điểm ((10,10),(9,9),\ldots,(-2,-2)). Lấy các giá trị tuyệt đối trước gcd sẽ loại bỏ hướng khỏi phép tính trong khi vẫn giữ nguyên số bước mạng. 

Đối với đường chéo có kích thước tối đa,```
1
-1000000000 -1000000000 1000000000 1000000000
```cả hai sự khác biệt tọa độ đều có giá trị tuyệt đối (2\cdot10^9). Gcd của họ cũng là (2\cdot10^9), vì vậy câu trả lời là (2\cdot10^9+1=2000000001). Kết quả rất lớn nhưng vẫn thu được bằng một lần tính toán gcd thay vì hàng tỷ lần lặp.
