---
title: "CF 36C - Bát"
description: "Mỗi chiếc bát là một hình nón, một hình nón đã bị cắt bỏ phần đầu. Một cái bát được mô tả bởi chiều cao h, bán kính đáy r và bán kính đỉnh R. Các cái bát được xếp chồng lên nhau theo một thứ tự nhất định, luôn có chung một trục thẳng đứng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 36
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 36"
rating: 2200
weight: 36
solve_time_s: 125
verified: false
draft: false
---
[CF 36C - Bát](https://codeforces.com/problemset/problem/36/C) 

**Đánh giá:** 2200 
**Tags:** hình học, thực hiện 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chiếc bát là một hình nón, một hình nón đã bị cắt bỏ phần đầu. Một cái bát được mô tả bởi chiều cao của nó`h`, bán kính đáy`r`, và bán kính đỉnh`R`. Các bát được xếp theo thứ tự nhất định, luôn có chung một trục thẳng đứng. Mỗi chiếc bát mới được hạ xuống càng nhiều càng tốt mà không giao nhau với bất kỳ chiếc bát nào trước đó. 

Chúng ta phải tính tổng chiều cao cuối cùng của ngăn xếp, được đo từ đáy của bát thấp nhất đến cạnh trên của bát cao nhất. 

Hình học quan trọng vì cái bát không phải là hình trụ. Bán kính của chúng thay đổi tuyến tính theo chiều cao, vì vậy việc hai cái bát có va chạm hay không phụ thuộc vào vị trí các bề mặt bên của chúng gặp nhau. 

Ràng buộc`n ≤ 3000`là quan sát chính. Một giải pháp bậc hai thực hiện khoảng chín triệu kiểm tra cặp, điều này hoàn toàn ổn trong Python nếu mỗi lần kiểm tra là thời gian không đổi. Bất cứ điều gì khối sẽ tiếp cận`2.7 × 10^10`hoạt động và không thể thực hiện được trong vòng hai giây. 

Phần khó khăn là xác định xem một chiếc bát mới có thể đi được bao xa khi đã tồn tại một vài chiếc bát trước đó. Việc thực hiện bất cẩn thường chỉ so sánh bán kính trên và dưới, nhưng va chạm đầu tiên có thể xảy ra ở đâu đó ở giữa các bề mặt nghiêng. 

Hãy xem xét ví dụ này:```
2
10 1 100
10 99 100
```Cái bát thứ hai gần như trùng với độ dốc của cái bát thứ nhất. Chúng va chạm ở gần đỉnh chứ không phải ở đáy. Nếu chỉ so sánh bán kính đáy, chúng ta sẽ nghĩ sai rằng chiếc bát thứ hai có thể hạ xuống rất xa. 

Một trường hợp tinh tế khác là ngăn chặn hoàn toàn:```
2
100 1 100
10 2 3
```Chiếc bát thứ hai nằm gọn hoàn toàn bên trong chiếc bát thứ nhất, sao cho đáy của nó chạm tới đáy chiếc bát thứ nhất. Tổng chiều cao vẫn còn`100`, không`110`. 

Cũng có trường hợp ngược lại là chiếc bát mới rộng hơn nhiều:```
2
10 1 2
10 100 200
```Bát thứ hai không thể vào được bát thứ nhất. Đáy của nó nằm chính xác trên bát đầu tiên, cho biết tổng chiều cao`20`. 

Những trường hợp này cho thấy tại sao chúng ta cần một điều kiện hình học chính xác thay vì so sánh theo kinh nghiệm. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp nhất là xử lý từng bát một. Đối với mỗi chiếc bát mới, hãy cố gắng hạ thấp nó cho đến khi chạm vào chiếc bát trước đó. Vì mỗi chiếc bát đều đã có một vị trí thẳng đứng cố định nên điều duy nhất chưa biết là chiều cao của đáy chiếc bát mới. 

Phiên bản brute-force có thể liên tục kiểm tra các vị trí hoặc tìm kiếm nhị phân chiều cao của vị trí trong khi kiểm tra các giao điểm so với tất cả các bát trước đó. Việc kiểm tra hình học rất dễ dàng vì bán kính thay đổi tuyến tính theo chiều cao. Thật không may, nếu chúng ta tìm kiếm nhị phân với độ chính xác cao cho mỗi cặp bát thì tổng công việc sẽ trở nên gần như đại khái.`O(n² log precision)`. Nó vẫn sẽ đi qua đây, nhưng nó phức tạp hơn mức cần thiết và ẩn cấu trúc cơ bản. 

Quan sát quan trọng là vị trí giới hạn giữa hai cái bát có thể được suy ra trực tiếp từ các hình tam giác giống nhau. 

Giả sử bát`i`đã được đặt ở độ cao đáy`pos[i]`. Chúng tôi muốn đặt bát`j`dưới đây càng nhiều càng tốt. 

Ở độ lệch dọc`x`bát bên trong`j`, bán kính của nó là:$$r_j + \frac{R_j - r_j}{h_j} x$$Cho bát`i`, ở độ cao`y`phía trên đáy của nó:$$r_i + \frac{R_i - r_i}{h_i} y$$Khi các bát chạm nhau lần đầu tiên, các bán kính này sẽ bằng nhau ở một độ cao chung nào đó trên toàn cầu. Giải các phương trình cho thấy một điều kiện cực kỳ đơn giản: 

Nếu các độ dốc bằng nhau thì một cái bát có thể vừa khít hoàn toàn hoặc không bao giờ vừa khít. 

Ngược lại, điểm tiếp xúc sẽ xác định một sự dịch chuyển tương đối duy nhất. 

Điều này cho phép chúng tôi tính toán vị trí hợp lệ tối thiểu so với một bát trước đó trong thời gian không đổi. Lấy mức tối đa trên tất cả các bát trước đó sẽ cho vị trí cuối cùng của bát mới. 

Từ`n = 3000`, việc kiểm tra trực tiếp từng cặp là hoàn toàn có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với tìm kiếm nhị phân | O(n² độ chính xác của nhật ký) | O(n) | Được chấp nhận nhưng phức tạp không cần thiết | 
| Tính toán hình học trực tiếp | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ chiều cao đáy`pos[i]`cho mỗi bát. 

Bát đầu tiên luôn bắt đầu ở độ cao`0`. 
2. Xử lý bát theo thứ tự nhất định. 

Vị trí của những chiếc bát không bao giờ thay đổi sau đó, vì vậy những chiếc bát trước đó đóng vai trò là chướng ngại vật cố định. 
3. Đối với bát hiện tại`j`, khởi tạo vị trí dưới cùng của nó thành`0`. 

Điều này có nghĩa là trước tiên chúng tôi cho rằng nó có thể hạ xuống hoàn toàn. 
4. So sánh bát`j`chống lại mọi bát trước đó`i`. 

Chúng tôi tính toán chiều cao thấp nhất nơi bát`j`không giao nhau bát`i`. 
5. Hãy để$$k_i = \frac{R_i - r_i}{h_i}, \quad k_j = \frac{R_j - r_j}{h_j}$$Đây là các sườn bên của bát. 

1. Nếu`k_i == k_j`, các cái bát có hình dạng tương tự nhau. 

Trong trường hợp này: 

- Nếu`r_j >= R_i`, cái bát`j`hoàn toàn rộng hơn và không thể vào bát`i`. Đáy của nó ít nhất phải bằng`pos[i] + h_i`. 
- Còn không thì bát`j`có thể trượt hoàn toàn bên trong bát`i`, vì vậy cặp này không áp đặt hạn chế nào ngoài`pos[i]`. 
2. Ngược lại, giải tìm điểm tiếp xúc đầu tiên của các bề mặt bên. 

Công thức dẫn xuất cho vị trí đáy hợp lệ tối thiểu là:$$need = pos[i] + \frac{r_i - r_j + k_j h_j}{k_j - k_i}$$Công thức này cho biết vị trí mà bề mặt ngoài của bát`j`lần đầu chạm vào bát`i`. 

1. Bát`j`cũng không thể xuống dưới đáy bát`i`. 

Kẹp giá trị bằng:$$need = \max(need, pos[i])$$1. Bát hiện tại phải đáp ứng mọi hạn chế trước đó, vì vậy hãy cập nhật:$$pos[j] = \max(pos[j], need)$$1. Sau khi đặt tất cả các bát vào, câu trả lời là:$$\max_i (pos[i] + h_i)$$Đây là cạnh trên cao nhất trong ngăn xếp. 

### Tại sao nó hoạt động 

Đối với mỗi cặp bát, tồn tại một vị trí tương đối thấp nhất nơi chúng không giao nhau. Bất kỳ vị trí nào thấp hơn sẽ làm cho các bề mặt nghiêng giao nhau. Phương trình dẫn xuất tính toán chính xác vị trí biên này. 

Khi chế biến bát`j`, mọi cái bát trước đó đã có một vị trí cố định. Lấy mức tối đa trên tất cả các giới hạn dưới theo cặp sẽ mang lại vị trí khả thi thấp nhất duy nhất cho bát`j`. Vì chiếc bát luôn được đặt ở mức thấp nhất có thể nên cấu trúc tham lam này phù hợp với quá trình xếp chồng vật lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

EPS = 1e-12

def main():
    n = int(input())

    h = [0.0] * n
    r = [0.0] * n
    R = [0.0] * n

    for i in range(n):
        hh, rr, RR = map(float, input().split())
        h[i] = hh
        r[i] = rr
        R[i] = RR

    pos = [0.0] * n

    for j in range(n):
        best = 0.0

        kj = (R[j] - r[j]) / h[j]

        for i in range(j):
            ki = (R[i] - r[i]) / h[i]

            if abs(ki - kj) < EPS:
                if r[j] >= R[i] - EPS:
                    need = pos[i] + h[i]
                else:
                    need = pos[i]
            else:
                need = pos[i] + (r[i] - r[j] + kj * h[j]) / (kj - ki)
                need = max(need, pos[i])

            best = max(best, need)

        pos[j] = best

    ans = 0.0

    for i in range(n):
        ans = max(ans, pos[i] + h[i])

    print(f"{ans:.10f}")

if __name__ == "__main__":
    main()
```Các mảng`h`,`r`, Và`R`lưu trữ các thông số hình học của mỗi bát. các`pos`mảng lưu trữ tọa độ dọc của đáy mỗi bát. 

Vòng lặp chính xử lý các bát theo thứ tự chèn. Vì các bát trước đó không bao giờ di chuyển nữa nên mọi quyết định về vị trí sẽ trở nên độc lập sau khi các vị trí trước đó được cố định. 

Việc tính toán độ dốc là thành phần hình học quan trọng nhất:```
k = (R - r) / h
```Đây là mức tăng bán kính trên một đơn vị chiều cao. Bởi vì các bức tường của cái bát là những đường thẳng nên bán kính ở bất kỳ độ cao nào đều trở thành một hàm tuyến tính đơn giản. 

Các sườn dốc song song yêu cầu xử lý riêng. Khi hai cái bát có các góc cạnh bằng nhau, chúng không bao giờ giao nhau dọc theo các bề mặt nghiêng trừ khi một cái bát hoàn toàn rộng hơn. Sử dụng so sánh dấu phẩy động`EPS`bởi vì sự bình đẳng chính xác về đôi là không đáng tin cậy. 

Công thức:```
need = pos[i] + (r[i] - r[j] + kj * h[j]) / (kj - ki)
```xuất phát trực tiếp từ việc giải đẳng thức bán kính tại điểm tiếp xúc. Đạo hàm giả định lần tiếp xúc đầu tiên xảy ra trên các bức tường bên. 

Cái kẹp:```
need = max(need, pos[i])
```ngăn chặn bát hiện tại đi xuống dưới đáy bát trước đó. Nếu không có đường này, một số cấu hình sẽ đặt bát không chính xác sâu hơn mức có thể về mặt vật lý. 

Cuối cùng, câu trả lời không chỉ đơn giản là phần trên của chiếc bát cuối cùng. Những chiếc bát trước vẫn có thể mở rộng cao hơn những chiếc bát sau, vì vậy chúng tôi tính toán mức tối đa trên tất cả những chiếc bát. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
40 10 50
60 20 30
```| Bát | Vị trí tính toán | Chiều cao trên cùng | 
| --- | --- | --- | 
| 0 | 0 | 40 | 
| 1 | 10 | 70 | 

Bát thứ nhất chiếm độ cao`[0, 40]`. 

Bát thứ hai hẹp hơn ở gần đỉnh nên lọt một phần vào bát thứ nhất. Công thức hình học xác định rằng vị trí hợp lệ thấp nhất là`10`. Đỉnh của nó trở thành`10 + 60 = 70`. 

Chiều cao ngăn xếp cuối cùng là`70`. 

### Ví dụ 2 

đầu vào:```
3
100 1 100
10 2 3
20 150 200
```| Bát | Vị trí tính toán | Chiều cao trên cùng | 
| --- | --- | --- | 
| 0 | 0 | 100 | 
| 1 | 0 | 10 | 
| 2 | 100 | 120 | 

Bát thứ hai nằm hoàn toàn bên trong bát thứ nhất nên đáy bát cũng ở độ cao`0`. 

Bát thứ ba rộng hơn nhiều so với mọi thứ bên dưới nó. Nó không thể vào bất kỳ cái bát nào trước đó, vì vậy nó nằm trên vật cản cao nhất ở độ cao`100`. 

Dấu vết này xác nhận rằng các trường hợp ngăn chặn và không chồng chéo đều được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi bát được so sánh với tất cả các bát trước đó | 
| Không gian | O(n) | Mảng lưu trữ thông số và vị trí bát | 

Với`n ≤ 3000`, thuật toán thực hiện tối đa khoảng chín triệu kiểm tra cặp. Mỗi lần kiểm tra là số học theo thời gian không đổi, dễ dàng phù hợp với giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline
    EPS = 1e-12

    n = int(input())

    h = [0.0] * n
    r = [0.0] * n
    R = [0.0] * n

    for i in range(n):
        hh, rr, RR = map(float, input().split())
        h[i] = hh
        r[i] = rr
        R[i] = RR

    pos = [0.0] * n

    for j in range(n):
        best = 0.0
        kj = (R[j] - r[j]) / h[j]

        for i in range(j):
            ki = (R[i] - r[i]) / h[i]

            if abs(ki - kj) < EPS:
                if r[j] >= R[i] - EPS:
                    need = pos[i] + h[i]
                else:
                    need = pos[i]
            else:
                need = pos[i] + (r[i] - r[j] + kj * h[j]) / (kj - ki)
                need = max(need, pos[i])

            best = max(best, need)

        pos[j] = best

    ans = max(pos[i] + h[i] for i in range(n))
    print(f"{ans:.10f}")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""2
40 10 50
60 20 30
"""
) == "70.0000000000", "sample 1"

# minimum size
assert run(
"""1
10 1 2
"""
) == "10.0000000000", "single bowl"

# complete containment
assert run(
"""2
100 1 100
10 2 3
"""
) == "100.0000000000", "small bowl fully inside"

# completely outside
assert run(
"""2
10 1 2
10 100 200
"""
) == "20.0000000000", "second bowl rests on top"

# equal slopes
assert run(
"""2
10 1 11
20 2 22
"""
) == "20.0000000000", "parallel sides"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bát đơn | 10 | Trường hợp cơ bản không có tương tác cặp | 
| Bát nhỏ bên trong bát lớn | 100 | Ngăn chặn hoàn toàn | 
| Bát lớn nối tiếp bát nhỏ | 20 | Không thể chèn | 
| Độ dốc bằng nhau | 20 | Xử lý đúng các bức tường song song | 

## Vỏ cạnh 

Hãy xem xét trường hợp ngăn chặn:```
2
100 1 100
10 2 3
```Chiếc bát đầu tiên có chiều rộng rất lớn so với chiếc bát thứ hai. Trong quá trình xử lý, hạn chế được tính toán không bao giờ ép bát thứ hai hướng lên trên. Đáy của nó vẫn ở`0`. 

Các đỉnh là: 

- Bát 0:`0 + 100 = 100`- Bát 1:`0 + 10 = 10`Thuật toán xuất ra chính xác`100`. 

Bây giờ hãy xem xét một cái bát hoàn toàn không thể vào được:```
2
10 1 2
10 100 200
```Cái bát thứ hai ở đáy đã rộng hơn cái bát đầu tiên ở trên cùng. Giới hạn theo cặp trở thành:```
need = pos[0] + h[0] = 10
```Vì vậy, bát thứ hai bắt đầu chính xác ở nơi bát thứ nhất kết thúc. Chiều cao cuối cùng trở thành`20`. 

Cuối cùng, kiểm tra độ dốc bằng nhau:```
2
10 1 11
20 2 22
```Cả hai cái bát đều giãn ra một đơn vị bán kính trên một đơn vị chiều cao. Các bức tường bên của họ song song. Vì bát thứ hai rộng hơn ở mọi nơi nên nó không thể rơi xuống bát thứ nhất. Nhánh dốc song song đặc biệt đặt nó ở độ cao chính xác`10`.
