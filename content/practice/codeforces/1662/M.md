---
title: "CF 1662M - Sắp xếp chai"
description: "Chúng ta phải xếp một hàng n chai rượu. Mỗi chai có màu đỏ (R) hoặc màu trắng (W). Mọi nhà phê bình đều muốn tìm một số phân đoạn chai liền kề có nội dung khớp với cặp được yêu cầu (r, w), trong đó r là số chai màu đỏ và w là số chai màu trắng trong đó…"
date: "2026-06-10T02:53:06+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1662
codeforces_index: "M"
codeforces_contest_name: "SWERC 2021-2022 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 0
weight: 1662
solve_time_s: 289
verified: false
draft: false
---

[CF 1662M - Sắp xếp chai](https://codeforces.com/problemset/problem/1662/M) 

**Đánh giá:** - 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 4 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta phải xây dựng một hàng`n`chai rượu vang. Mỗi chai có màu đỏ (`R`) hoặc trắng (`W`). 

Mọi nhà phê bình đều muốn tìm một số phân đoạn chai liền kề có nội dung phù hợp với cặp được yêu cầu`(r, w)`, Ở đâu`r`là số chai màu đỏ và`w`là số chai màu trắng trong đoạn đó. Các nhà phê bình khác nhau có thể chọn các phân khúc khác nhau. Nhiệm vụ của chúng tôi là xây dựng một sự sắp xếp duy nhất cho phép mọi cặp được yêu cầu xuất hiện dưới dạng số lượng màu của ít nhất một khoảng liền kề. 

Nếu không sắp xếp độ dài`n`có thể đáp ứng đồng thời tất cả các yêu cầu, chúng ta phải in`IMPOSSIBLE`. 

Những hạn chế là nhỏ. Cả hai`n`Và`m`nhiều nhất là`100`. Điều này ngay lập tức gợi ý rằng việc liệt kê tốn kém trên tất cả các độ dài có thể hoặc tất cả số lượng màu có thể có là khả thi. Những gì còn lại là tìm ra quan sát cấu trúc để giảm không gian tìm kiếm đủ để giúp việc xây dựng trở nên dễ dàng. 

Một điểm tinh tế là các nhà phê bình không chỉ định các vị trí khoảng thời gian. Họ chỉ xác định có bao nhiêu chai màu đỏ và trắng phải xuất hiện trong một khoảng thời gian nào đó. Chúng ta có thể tự do lựa chọn khoảng thời gian sau này, miễn là nó tồn tại. 

Xem xét một yêu cầu`(3, 2)`. Nhà phê bình không yêu cầu một đoạn cụ thể có độ dài 5. Bất kỳ khoảng nào chứa chính xác ba chai màu đỏ và hai chai màu trắng đều được chấp nhận. 

Một sai lầm dễ mắc phải khác là quên rằng độ dài khoảng thời gian đã được cố định theo yêu cầu. một yêu cầu`(r, w)`yêu cầu một khoảng độ dài`r + w`. Nếu như`r + w > n`, yêu cầu không bao giờ có thể được thỏa mãn. 

Ví dụ:```
n = 4
request = (3, 2)
```Độ dài khoảng cần thiết là`5`, nhưng toàn bộ sự sắp xếp chỉ có bốn chai. Câu trả lời đúng là`IMPOSSIBLE`. 

Trường hợp thứ hai không rõ ràng xảy ra khi yêu cầu chỉ yêu cầu một màu.```
n = 3
requests:
(0, 2)
(0, 3)
```Sự sắp xếp`WWW`hoạt động vì các khoảng có độ dài hai và ba chứa chính xác số lượng chai màu trắng cần thiết. 

Một giải pháp bất cẩn khẳng định mọi khoảng thời gian hợp lệ phải chứa cả hai màu sẽ từ chối trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ tạo ra mọi chuỗi có độ dài có thể`n`và kiểm tra xem mọi yêu cầu có xuất hiện trong khoảng thời gian của nó hay không. Vì có`2^n`các chuỗi có thể, điều này trở nên không thể ngay cả đối với các giá trị vừa phải của`n`. 

Quan sát chính xuất phát từ việc xem yêu cầu thực sự chứa thông tin gì. 

một yêu cầu`(r, w)`chỉ quan tâm đến hai đại lượng: 

1. Độ dài khoảng`L = r + w`. 
2. Số chai màu đỏ trong khoảng đó. 

Giả sử chúng ta xây dựng một chuỗi bao gồm một số chai màu đỏ theo sau là một số chai màu trắng:```
RRRR...RWWWW...W
```Gọi tổng số chai màu đỏ là`R`. 

Hãy xem xét bất kỳ độ dài khoảng nào`L`. 

Là một cửa sổ trượt có chiều dài`L`di chuyển từ ngoài cùng bên trái sang ngoài cùng bên phải, số lượng chai màu đỏ bên trong cửa sổ thay đổi dần dần. Nó bắt đầu lúc`min(R, L)`và kết thúc tại`max(0, L - (n - R))`. 

Hơn nữa, mọi giá trị số nguyên trong phạm vi đó đều xuất hiện. 

Do đó, với tổng số chai màu đỏ cố định`R`, một yêu cầu`(r, w)`khả thi khi và chỉ khi:```
L = r + w <= n
```Và```
max(0, L - (n - R)) <= r <= min(R, L)
```Đây là sự đơn giản hóa quan trọng. 

Thay vì xây dựng một cách sắp xếp tùy ý, chúng ta chỉ cần chọn một giá trị`R`giữa`0`Và`n`. Một lần`R`đã cố định, sự sắp xếp```
R repeated R times
W repeated (n - R) times
```đã nhận ra mọi số lượng màu đỏ có thể đạt được trong mỗi khoảng thời gian. 

chỉ có`n + 1`những lựa chọn có thể có của`R`, nhiều nhất là`101`. 

Chúng ta có thể chỉ cần kiểm tra từng giá trị ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Liệt kê tất cả các chuỗi | O(2^n · n²) | O(n) | Quá chậm | 
| Hãy thử mọi tổng số màu đỏ | O(nm) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số chai màu đỏ có thể`R`từ`0`ĐẾN`n`, xét sự sắp xếp gồm`R`chai màu đỏ theo sau`n - R`chai màu trắng. 

2. Đối với mỗi yêu cầu`(r, w)`, tính độ dài khoảng cần thiết:```
   L = r + w
   ```3. Nếu`L > n`, yêu cầu này không thể phù hợp với sự sắp xếp. Từ chối giá trị này của`R`. 

4. Tính số chai màu đỏ tối thiểu có thể có trong bất kỳ khoảng thời gian nào`L`:```
   low = max(0, L - (n - R))
   ```Điều này xảy ra khi khoảng được đẩy càng xa càng tốt vào vùng màu trắng. 

5. Tính số chai màu đỏ tối đa có thể có trong bất kỳ khoảng thời gian nào`L`:```
   high = min(R, L)
   ```Điều này xảy ra khi khoảng thời gian được đẩy càng xa càng tốt vào vùng màu đỏ. 

6. Kiểm tra xem số màu đỏ được yêu cầu có thỏa mãn hay không:```
   low <= r <= high
   ```Nếu không, hãy loại bỏ giá trị này của`R`. 

7. Nếu mọi yêu cầu đều thành công, hãy xuất ra:```
   "R" * R + "W" * (n - R)
   ```8. Nếu không có giá trị của`R`công việc, đầu ra`IMPOSSIBLE`. 

### Tại sao nó hoạt động 

Trong một chuỗi gồm một khối chai màu đỏ, theo sau là một khối chai màu trắng, mỗi đoạn có độ dài cố định sẽ cắt khối màu đỏ ở một phần liền kề. 

Khi khoảng thời gian trượt, số lượng chai màu đỏ thay đổi tối đa một chai ở mỗi bước. Do đó, số lượng màu đỏ có thể tạo thành một khoảng nguyên hoàn chỉnh từ giá trị tối thiểu có thể đạt được đến giá trị tối đa có thể đạt được. 

Các công thức cho`low`Và`high`mô tả chính xác những thái cực đó. 

Một yêu cầu được đáp ứng chính xác khi số lượng màu đỏ yêu cầu của nó nằm trong khoảng đó. Vì mọi số nguyên giữa các cực trị đều xảy ra nên không cần thêm điều kiện nào. 

Kiểm tra tất cả các giá trị của`R`kiểm tra mọi kích thước có thể có của khối màu đỏ. Nếu tồn tại sự sắp xếp hợp lệ ở dạng đặc biệt này thì thuật toán sẽ tìm thấy nó. Quan sát mang tính xây dựng chứng minh rằng sự sắp xếp như vậy tồn tại bất cứ khi nào có bất kỳ giải pháp nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n, m = map(int, input().split())
        req = [tuple(map(int, input().split())) for _ in range(m)]

        found = None

        for R in range(n + 1):
            ok = True

            for r, w in req:
                L = r + w

                if L > n:
                    ok = False
                    break

                low = max(0, L - (n - R))
                high = min(R, L)

                if not (low <= r <= high):
                    ok = False
                    break

            if ok:
                found = "R" * R + "W" * (n - R)
                break

        if found is None:
            answers.append("IMPOSSIBLE")
        else:
            answers.append(found)

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo đặc tính toán học. 

Vòng ngoài thử mọi số chai màu đỏ có thể. Vòng lặp bên trong xác minh tất cả các yêu cầu phê bình đối với lựa chọn đó. 

Các công thức cho`low`Và`high`là phần không tầm thường duy nhất. Chúng mã hóa số lượng màu đỏ nhỏ nhất và lớn nhất có thể đạt được theo một khoảng độ dài`L`bên trong một chuỗi có dạng:```
RRR...RWWW...W
```Không cần liệt kê khoảng thời gian. 

Bản thân cấu trúc chỉ đơn giản là tiền tố của chai màu đỏ, theo sau là hậu tố của chai màu trắng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
requests:
(1, 0)
(3, 2)
(2, 2)
```Thử`R = 3`. 

| Yêu cầu | L | thấp | cao | Bắt buộc r | hợp lệ | 
|----------|---|------|------|-------------|--------| 
| (1,0) | 1 | 0 | 1 | 1 | Có | 
| (3,2) | 5 | 3 | 3 | 3 | Có | 
| (2,2) | 4 | 2 | 3 | 2 | Có | 

Tất cả các yêu cầu đều được thông qua. 

Các đầu ra thuật toán:```
RRRWW
```Các khoảng có độ dài bốn và năm nhận được số lượng cần thiết. 

### Ví dụ 2 

đầu vào:```
n = 4
requests:
(2,1)
(1,1)
(0,3)
```Thử`R = 2`. 

| Yêu cầu | L | thấp | cao | Bắt buộc r | hợp lệ | 
|----------|---|------|------|-------------|--------| 
| (2,1) | 3 | 1 | 2 | 2 | Có | 
| (1,1) | 2 | 0 | 2 | 1 | Có | 
| (0,3) | 3 | 1 | 2 | 0 | Không | 

Giá trị này không thành công. 

Thử tất cả các giá trị của`R`không hiển thị thỏa mãn mọi yêu cầu, vì vậy câu trả lời là:```
IMPOSSIBLE
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(nm) cho mỗi trường hợp thử nghiệm | Hãy thử mọi kích thước khối màu đỏ và xác minh tất cả các yêu cầu | 
| Không gian | O(m) | Lưu trữ các yêu cầu | 

Từ`n`Và`m`nhiều nhất là cả hai`100`, công việc trong trường hợp xấu nhất chỉ là khoảng mười nghìn lượt kiểm tra cho mỗi trường hợp kiểm thử. Điều này là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    from collections import deque

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        req = [tuple(map(int, input().split())) for _ in range(m)]

        found = None

        for R in range(n + 1):
            ok = True

            for r, w in req:
                L = r + w

                if L > n:
                    ok = False
                    break

                low = max(0, L - (n - R))
                high = min(R, L)

                if not (low <= r <= high):
                    ok = False
                    break

            if ok:
                found = "R" * R + "W" * (n - R)
                break

        ans.append(found if found is not None else "IMPOSSIBLE")

    return "\n".join(ans)

# provided sample
assert run(
"""3
5 3
1 0
3 2
2 2
4 3
2 1
1 1
0 3
3 2
0 2
0 3
"""
) == "RRRWW\nIMPOSSIBLE\nWWW"

# minimum size
assert run(
"""1
1 1
1 0
"""
) == "R"

# all white requests
assert run(
"""1
5 2
0 2
0 5
"""
) == "WWWWW"

# impossible because interval too long
assert run(
"""1
3 1
2 2
"""
) == "IMPOSSIBLE"

# full-length interval requirement
assert run(
"""1
4 1
2 2
"""
) == "RRWW"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| n=1, yêu cầu (1,0) | R | Trường hợp hợp lệ nhỏ nhất | 
| Yêu cầu (0,2), (0,5) | WWWWW | Xây dựng màu trắng tinh khiết | 
| n=3, yêu cầu (2,2) | KHÔNG THỂ | Khoảng thời gian dài hơn mảng | 
| n=4, yêu cầu (2,2) | RRWW | Yêu cầu khoảng thời gian toàn mảng | 

## Vỏ cạnh 

Hãy xem xét:```
n = 3
request = (2, 2)
```Độ dài khoảng thời gian sẽ là bốn. Không có khoảng có độ dài bốn tồn tại trong một mảng có độ dài ba. Thuật toán phát hiện ngay điều này thông qua điều kiện`L > n`và đầu ra`IMPOSSIBLE`. 

Coi như:```
n = 5
requests:
(0, 2)
(0, 5)
```Lựa chọn`R = 0`mang lại:```
WWWWW
```Đối với độ dài hai và độ dài năm, số màu đỏ duy nhất có thể có là 0. Cả hai yêu cầu đều được đáp ứng. 

Coi như:```
n = 5
request = (5, 0)
```Lựa chọn`R = 5`mang lại:```
RRRRR
```Khoảng duy nhất có độ dài 5 chứa chính xác 5 chai màu đỏ, vì vậy yêu cầu được thỏa mãn. 

Coi như:```
n = 4
request = (2, 2)
```Độ dài khoảng yêu cầu bằng toàn bộ mảng. Thuật toán chọn`R = 2`, sản xuất`RRWW`. Toàn bộ mảng chứa chính xác hai chai màu đỏ và hai chai màu trắng, phù hợp với yêu cầu. 
::: 

Bài xã luận này phát triển việc xây dựng từ quan sát đếm khoảng thời gian và dẫn trực tiếp đến giải pháp được chấp nhận.
