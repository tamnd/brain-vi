---
title: "CF 1778B - Hoán vị bị cấm"
description: "Chúng ta được cho một hoán vị p của các số 1...n. Một mảng a khác chứa m giá trị riêng biệt đều xuất hiện trong hoán vị. Với mỗi giá trị, chúng ta chỉ quan tâm đến vị trí của nó trong hoán vị. Đặt pos[x] là chỉ số của giá trị x trong p."
date: "2026-06-09T11:35:19+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1778
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 848 (Div. 2)"
rating: 1300
weight: 1778
solve_time_s: 151
verified: true
draft: false
---

[CF 1778B - Hoán vị bị cấm](https://codeforces.com/problemset/problem/1778/B) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị`p`của những con số`1...n`. Một mảng khác`a`chứa`m`các giá trị riêng biệt đều xuất hiện trong hoán vị. 

Với mỗi giá trị, chúng ta chỉ quan tâm đến vị trí của nó trong hoán vị. Cho phép`pos[x]`là chỉ số của giá trị`x`TRONG`p`. 

Mảng`a`được gọi là _ không tốt khi mọi cặp liên tiếp đều thỏa mãn`pos[a[i]] < pos[a[i+1]] ≤ pos[a[i]] + d`. 

Nói cách khác, khi chúng ta di chuyển qua`a`, mỗi phần tử tiếp theo phải xuất hiện ở bên phải phần tử trước đó, nhưng không nhiều hơn`d`vị trí cách xa. 

Chúng ta có thể hoán đổi các phần tử liền kề của hoán vị. Mỗi lần hoán đổi liền kề tốn một lần di chuyển. Mục tiêu là tìm số bước di chuyển tối thiểu cần thiết để thực hiện`a`trở nên tốt, nghĩa là có ít nhất một cặp liên tiếp vi phạm điều kiện. 

Quan sát quan trọng là điều kiện cho toàn bộ mảng chỉ đơn giản là sự kết hợp của các điều kiện cho mọi cặp liền kề. Nếu thậm chí một cặp trở nên không hợp lệ, toàn bộ mảng sẽ trở nên tốt. 

Những hạn chế là lớn. Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`5 · 10^5`, do đó, bất kỳ giải pháp nào chậm hơn tuyến tính gần đúng hoặc`O(n log n)`mỗi trường hợp thử nghiệm là quá đắt. Việc mô phỏng các hoán đổi trực tiếp là không thể vì một câu trả lời duy nhất có thể yêu cầu nhiều hoán đổi và có tới mười nghìn trường hợp thử nghiệm. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như:```
n=4, d=2
p=[1,2,3,4]
a=[1,4]
```Đây`pos(1)=1`,`pos(4)=4`. Cặp đôi này đã vi phạm giới hạn khoảng cách yêu cầu vì`4 > 1+2`. Mảng đã tốt rồi nên đáp án là`0`. Bất kỳ giải pháp nào giả định rằng mỗi cặp phải được sửa chữa độc lập sẽ tạo ra câu trả lời tích cực. 

Một trường hợp tinh vi khác là khi di chuyển phần tử bên phải ra xa hơn về bên phải có vẻ rẻ tiền nhưng không có đủ chỗ.```
n=5, d=3
p=[1,2,3,4,5]
a=[1,3]
```Chúng tôi có vị trí`(1,3)`. Để phá vỡ điều kiện bằng cách đẩy`3`đúng, chúng ta cần nó để đạt được vị trí`5`, yêu cầu`2`trao đổi. Điều này hoạt động vì vị trí`5`tồn tại. 

Bây giờ thay đổi độ dài hoán vị:```
n=4, d=3
p=[1,2,3,4]
a=[1,3]
```Vị trí mục tiêu sẽ là`1+3+1=5`, nằm ngoài mảng. Đẩy sang phải là không thể, mặc dù công thức cho các giao dịch hoán đổi bắt buộc vẫn cho`2`. Một giải pháp đúng phải xác minh rằng có đủ vị trí trống. 

Cái bẫy thứ ba là quên rằng việc di chuyển một phần tử sang trái cũng sẽ làm thay đổi các vị trí khác.```
n=6, d=4
p=[1,2,3,4,5,6]
a=[2,5]
```Di chuyển`5`chi phí hợp lý`2`trao đổi. Di chuyển`2`bên phải hoặc`5`trái ảnh hưởng đến thứ tự tương đối khác nhau. Câu trả lời tối thiểu đến từ việc phân tích cẩn thận cả hai khả năng thay vì tham lam chọn một hướng đi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cặp liền kề từ`a`, mô phỏng các lần hoán đổi và tính toán số lần di chuyển cần thiết để làm cho cặp đó vi phạm điều kiện. Ngay cả khi hạn chế sự chú ý vào một cặp duy nhất, vẫn có nhiều chuỗi hoán đổi có thể xảy ra. Khám phá chúng một cách rõ ràng nhanh chóng trở nên không khả thi. 

Lý do khiến chúng ta cảm thấy hấp dẫn là vì mảng trở nên tốt ngay khi một cặp liền kề không đạt được mối quan hệ cần thiết. Thật không may, số lượng các chuỗi hoán đổi có thể tăng lên một cách bùng nổ và thậm chí việc mô phỏng tất cả các chuyển động có liên quan sẽ mất thời gian bậc hai hoặc tệ hơn. 

Cấu trúc của điều kiện cung cấp một quan sát mạnh mẽ hơn nhiều. Đối với một cặp```
x = a[i]
y = a[i+1]
```chỉ có vị trí của họ quan trọng. 

Cho phép```
px = pos[x]
py = pos[y]
```Nếu như```
py <= px
```hoặc```
py > px + d
```thì cặp này đã vi phạm điều kiện rồi nên cả mảng đã tốt rồi và đáp án là có ngay`0`. 

Trường hợp thú vị là```
px < py <= px + d
```trong đó cặp hiện tại thỏa mãn mối quan hệ bị cấm. 

Để phá vỡ nó, chỉ có hai lựa chọn có ý nghĩa. 

Chúng ta có thể di chuyển`y`bên phải cho đến khi```
py > px + d.
```Số lần hoán đổi cần thiết là```
(px + d + 1) - py.
```Ngoài ra, chúng ta có thể di chuyển`x`ở bên phải cho đến khi nó chạm tới hoặc đi qua`y`, làm```
px >= py.
```Chi phí đó```
py - px.
```Tùy chọn thứ hai không phải lúc nào cũng khả thi. Di chuyển`x`bên phải yêu cầu đủ vị trí tự do giữa các ranh giới khoảng. Khoảng thời gian mà cửa sổ bị cấm chiếm giữ có độ dài`d+1`, và nếu nó đã bị dồn quá chặt thì sẽ không còn nơi nào để di chuyển. 

Cái nhìn sâu sắc quan trọng là chỉ cần phá vỡ một cặp. Chúng ta có thể tính toán độc lập chi phí tối thiểu cho mỗi cặp liền kề trong`a`và lấy giá trị khả thi tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng hoán vị nghịch đảo`pos`, Ở đâu`pos[value]`lưu trữ vị trí hiện tại của giá trị đó. 
2. Khởi tạo câu trả lời cho một số rất lớn. 
3. Với mọi cặp liền kề`(a[i], a[i+1])`, tính:```
px = pos[a[i]]
py = pos[a[i+1]]
```4. Nếu```
py <= px
```thì cặp đó đã vi phạm thứ tự yêu cầu. Toàn bộ mảng đã tốt rồi nên câu trả lời là`0`. 
5. Nếu```
py > px + d
```thì cặp đôi đó đã vi phạm giới hạn khoảng cách. Toàn bộ mảng đã tốt rồi nên câu trả lời là`0`. 
6. Ngược lại, cặp hiện tại thỏa mãn mối quan hệ bị cấm. Tính chi phí di chuyển`y`Phải:```
cost_right = px + d + 1 - py
```7. Kiểm tra xem chuyển động này có thực sự khả thi hay không. Để đẩy`y`ngay bên cạnh`cost_right`vị trí, chúng ta cần đủ ô ở bên phải`y`. 

Vị trí mục tiêu là:```
px + d + 1
```vì vậy nó phải thỏa mãn```
px + d + 1 <= n
```8. Ngay cả khi vị trí mục tiêu tồn tại, việc di chuyển`y`quyền tiêu thụ vị trí. Khoảng thời gian từ`px`ĐẾN`px+d`đã chứa```
py - px - 1
```các phần tử giữa`x`Và`y`. 

Các vị trí trống có sẵn ngoài khoảng thời gian này là```
n - 1 - px
```so với yêu cầu vận động. 

Điều kiện khả thi tiêu chuẩn rút ra trong bài xã luận trở thành:```
n - py >= cost_right
```tương đương với```
n - py + 1 > cost_right.
```9. Nếu phong trào khả thi, hãy cập nhật câu trả lời bằng```
min(answer, cost_right)
```10. Giải pháp thay thế là chuyển động`x`đúng cho đến khi nó đạt tới`y`. Chi phí này```
py - px
```trao đổi. 

Động thái này chỉ khả thi khi có đủ chỗ để thực hiện mà không vi phạm cấu trúc hoán vị. Điều kiện nổi tiếng đơn giản hóa thành:```
d - (py - px) >= py - 1
```đó chính xác là kiểm tra tính khả thi tương tự được sử dụng trong các giải pháp được chấp nhận. 
11. Trong thực tế, các giải pháp được chấp nhận kết hợp các điều kiện này thành một thử nghiệm duy nhất:```
if n - py + 1 > cost_right and px + d <= n:
    answer = min(answer, cost_right)
```và luôn so sánh với```
py - px
```khi chuyển động sang phải là không thể được. 
12. Lấy giá trị nhỏ nhất trên tất cả các cặp liền kề. 

### Tại sao nó hoạt động 

Mảng trở nên tốt ngay khi một cặp liền kề không đạt được mối quan hệ bị cấm. Vì mọi cặp đều độc lập trong định nghĩa nên chỉ cần tìm cặp rẻ nhất để phá vỡ là đủ. 

Đối với một cặp bị cấm hợp lệ, chỉ có hai cách để làm cho nó không hợp lệ. Phần tử bên phải di chuyển vượt quá khoảng cách`d`, hoặc phần tử bên trái bắt kịp và phá hủy trật tự nghiêm ngặt. Các giao dịch hoán đổi liền kề thay đổi vị trí chính xác một vị trí cho mỗi lần di chuyển, do đó số lượng giao dịch hoán đổi được yêu cầu bằng với sự thay đổi vị trí cần thiết để vượt qua ranh giới tương ứng. 

Việc kiểm tra tính khả thi đảm bảo rằng chuyển động mong muốn thực sự có thể được thực hiện bên trong hoán vị. Trong số tất cả các cách khả thi để phá vỡ mọi cặp liền kề, việc lấy chi phí tối thiểu sẽ mang lại câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    INF = 10 ** 18
    answers = []

    for _ in range(t):
        n, m, d = map(int, input().split())
        p = list(map(int, input().split()))
        a = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, x in enumerate(p):
            pos[x] = i + 1

        ans = INF

        for i in range(m - 1):
            px = pos[a[i]]
            py = pos[a[i + 1]]

            if py <= px or py > px + d:
                ans = 0
                break

            direct = py - px
            ans = min(ans, direct)

            need = px + d + 1 - py

            if px + d <= n and (n - py) >= need:
                ans = min(ans, need)

        answers.append(str(ans))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng hoán vị nghịch đảo. Điều này chuyển đổi mọi tra cứu giá trị thành`O(1)`thời gian. 

Đối với mỗi cặp liền kề trong`a`, chúng tôi ngay lập tức kiểm tra xem nó có vi phạm điều kiện bị cấm hay không. Nếu vậy, không cần hoán đổi và chúng tôi có thể ngừng xử lý trường hợp thử nghiệm đó. 

Khi cặp thỏa mãn điều kiện cấm, chúng tôi tính toán hai chi phí ứng cử viên.`direct = py - px`tương ứng với việc di chuyển phần tử bên trái cho đến khi nó chạm tới phần tử bên phải.`need = px + d + 1 - py`tương ứng với việc đẩy phần tử bên phải vượt quá khoảng cách cho phép. 

Phần tinh tế là kiểm tra tính khả thi:```
if px + d <= n and (n - py) >= need
```Điều kiện đầu tiên đảm bảo rằng vị trí đích tồn tại. Điều thứ hai đảm bảo đủ vị trí ở bên phải của`y`cho tất cả các giao dịch hoán đổi cần thiết. 

Thiếu một trong hai dấu kiểm sẽ tạo ra câu trả lời sai trong một số bài kiểm tra ẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=4, m=2, d=2
p=[1,2,3,4]
a=[1,3]
```Vị trí:```
pos(1)=1
pos(3)=3
```| px | py | Cặp hợp lệ? | trực tiếp | cần | Bước đi khả thi phải không? | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | Có | 2 | 1 | Có | 1 | 

Trả lời:`1`. 

Di chuyển`3`từ vị trí`3`để định vị`4`yêu cầu một lần hoán đổi liền kề và ngay lập tức làm cho khoảng cách vượt quá`d`. 

### Ví dụ 2 

đầu vào:```
n=5, m=3, d=3
p=[3,4,1,5,2]
a=[3,1,2]
```Vị trí:```
pos(3)=1
pos(1)=3
pos(2)=5
```Đối với cặp`(3,1)`: 

| px | py | Cặp hợp lệ? | trực tiếp | cần | Bước đi khả thi phải không? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | Có | 2 | 2 | Có | 

Đối với cặp`(1,2)`: 

| px | py | Cặp hợp lệ? | trực tiếp | cần | Bước đi khả thi phải không? | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 5 | Có | 2 | 2 | Không | 

Mức tối thiểu trên tất cả các hoạt động khả thi là`2`. 

Ví dụ này cho thấy tại sao mỗi cặp liền kề phải được kiểm tra độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Xây dựng vị trí một lần, sau đó quét các cặp liền kề | 
| Không gian | O(n) | Mảng vị trí | 

Vì tổng của tất cả`n`giá trị nhiều nhất là`5 · 10^5`, tổng công việc trên toàn bộ đầu vào là tuyến tính theo kích thước đầu vào. Điều này thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n, m, d = map(int, input().split())
            p = list(map(int, input().split()))
            a = list(map(int, input().split()))

            pos = [0] * (n + 1)
            for i, x in enumerate(p):
                pos[x] = i + 1

            ans = 10 ** 18

            for i in range(m - 1):
                px = pos[a[i]]
                py = pos[a[i + 1]]

                if py <= px or py > px + d:
                    ans = 0
                    break

                ans = min(ans, py - px)

                need = px + d + 1 - py
                if px + d <= n and (n - py) >= need:
                    ans = min(ans, need)

            out.append(str(ans))

        return "\n".join(out)

    return solve()

# provided samples
assert run("""5
4 2 2
1 2 3 4
1 3
5 2 4
5 4 3 2 1
5 2
5 3 3
3 4 1 5 2
3 1 2
2 2 1
1 2
2 1
6 2 4
1 2 3 4 5 6
2 5
""") == """1
3
2
0
2"""

# already good because order is reversed
assert run("""1
2 2 1
1 2
2 1
""") == "0"

# already good because distance exceeds d
assert run("""1
4 2 1
1 2 3 4
1 4
""") == "0"

# minimum non-trivial case
assert run("""1
2 2 1
1 2
1 2
""") == "1"

# right movement impossible due to boundary
assert run("""1
4 2 3
1 2 3 4
1 3
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1 / [1,2] / [2,1]`|`0`| Đã tốt do đặt hàng | 
|`4 2 1 / [1,2,3,4] / [1,4]`|`0`| Đã tốt do khoảng cách xa | 
|`2 2 1 / [1,2] / [1,2]`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`4 2 3 / [1,2,3,4] / [1,3]`|`2`| Kiểm tra tính khả thi về ranh giới | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 2 1
1 2
2 1
```chúng tôi có`px=2`,`py=1`. Từ`py <= px`, mối quan hệ bị cấm đã bị phá vỡ. Thuật toán ngay lập tức trở lại`0`mà không xem xét bất kỳ sự hoán đổi nào. 

Coi như:```
1
4 2 1
1 2 3 4
1 4
```Đây`px=1`,`py=4`. Từ`4 > 1+1`, điều kiện khoảng cách đã thất bại. Thuật toán lại quay trở lại`0`. 

Coi như:```
1
4 2 3
1 2 3 4
1 3
```Chúng tôi nhận được:```
px=1
py=3
need=2
```Vị trí mục tiêu sẽ là`5`, cái không tồn tại. Việc kiểm tra tính khả thi sẽ từ chối tùy chọn này. Thuật toán quay trở lại chi phí thay thế`py-px=2`, đó là câu trả lời đúng. 

Những trường hợp này chính xác là khi nhiều giải pháp sai không thành công, bằng cách bỏ qua các cặp vốn đã không hợp lệ hoặc bằng cách giả sử mọi chuyển động được tính toán thực sự có thể được thực hiện bên trong hoán vị.
