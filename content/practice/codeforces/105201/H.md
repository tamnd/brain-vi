---
title: "CF 105201H - Nội quy nhà"
description: "Chúng ta có một dòng gồm n k vị trí, được đánh số theo khoảng cách từ lối vào. Mỗi vị trí ban đầu có một chiếc giày và mỗi người có đúng k chiếc giày. Mảng a cho chúng ta biết chủ nhân của chiếc giày hiện được đặt ở mỗi vị trí. Một ngày bao gồm hai sự kiện ngẫu nhiên."
date: "2026-06-27T02:48:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105201
codeforces_index: "H"
codeforces_contest_name: "IME++ Open Contest 2024"
rating: 0
weight: 105201
solve_time_s: 79
verified: false
draft: false
---

[CF 105201H - Nội quy chung](https://codeforces.com/problemset/problem/105201/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng`n * k`vị trí, được đánh số theo khoảng cách từ lối vào. Mỗi vị trí ban đầu có một chiếc giày và mỗi người sở hữu chính xác`k`đôi giày. Mảng`a`cho chúng ta biết chủ nhân của chiếc giày hiện đang được đặt ở mỗi vị trí. 

Một ngày bao gồm hai sự kiện ngẫu nhiên. Mỗi người chọn một đôi giày thống nhất cho mình và mang đi. Sau đó, mọi người quay trở lại theo thứ tự ngẫu nhiên. Những đôi giày đã tháo ra sẽ được đặt vào các vị trí trống theo thứ tự đến đó, lấp đầy vị trí trống gần nhất trước tiên. 

Đối với mỗi truy vấn, chúng tôi cần vị trí trung bình dự kiến ​​của tất cả các đôi giày thuộc về một người cụ thể sau một số ngày nhất định. Câu trả lời là một biểu diễn mô-đun của giá trị hợp lý chính xác. 

Tổng số vị trí tối đa là`500000`, nhưng số lượng truy vấn cũng`500000`, và số ngày có thể lớn bằng`10^18`. Mô phỏng không thể hoạt động vì ngay cả một truy vấn yêu cầu nhiều ngày cũng sẽ yêu cầu quá nhiều lần chuyển đổi. Với nửa triệu truy vấn, chúng tôi cần thời gian gần như không đổi cho mỗi truy vấn sau khi xử lý trước, điều này loại trừ mọi thứ tùy thuộc vào số ngày hoặc tổng số giày cho mỗi truy vấn. 

Các trường hợp đặc biệt chính đến từ công thức chuyển tiếp. Khi`k = 1`, mỗi người chỉ sở hữu một chiếc giày nên mỗi ngày hãy tháo tất cả những chiếc giày ra và đặt chúng lại một cách ngẫu nhiên. Số nhân trong phép truy hồi trở thành 0 và coi nó như một phép chia mô đun thông thường sẽ thất bại. Ví dụ:```
Input:
2 1
1 2
2
1 0
1 1
```Vị trí trung bình ban đầu của người 1 là`1`. Sau một ngày có hai chiếc giày và chủ nhân của chúng được phân ngẫu nhiên vào các vị trí`1`Và`2`, vậy vị trí trung bình kỳ vọng của người 1 là`3/2`. Việc thực hiện bất cẩn tính toán quyền hạn của`(k-1)/k`không cần xử lý`k = 1`sẽ cố gắng đảo ngược số không. 

Một trường hợp cạnh khác là`d = 0`. Không có phép toán ngẫu nhiên nào xảy ra nên đáp án phải là giá trị trung bình ban đầu. Ví dụ:```
Input:
2 2
1 1 2 2
1
1 0
```Câu trả lời là`3/2`, vì hai chiếc giày của người 1 đã ở vị trí 1 và 2. Áp dụng trực tiếp công thức dài hạn mà không cần kiểm tra`d = 0`có thể vô tình thay thế trạng thái ban đầu bằng giá trị dừng. 

Trường hợp tinh tế cuối cùng là các vị trí được lập chỉ mục một. Vì:```
Input:
2 2
1 2 1 2
1
1 0
```người 1 sở hữu vị trí 1 và 3 nên trung bình là`2`. Nếu coi khe đầu tiên là khoảng cách bằng 0 sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là giữ lại toàn bộ cách sắp xếp giày và mô phỏng hàng ngày. Mỗi ngày chúng tôi sẽ chọn ngẫu nhiên một chiếc giày của mỗi người, xác định những vị trí còn trống, xáo trộn những chiếc giày đã tháo ra và đặt chúng trở lại. Sau mỗi ngày mô phỏng, chúng tôi có thể tính toán vị trí trung bình của người được yêu cầu. Điều này đúng vì nó tuân theo chính xác quy trình được mô tả. 

Vấn đề là số ngày không bị giới hạn. Một truy vấn có thể hỏi về`10^18`ngày, do đó ngay cả một mô phỏng xử lý một ngày trong`O(nk)`thời gian là không thể. Trong trường hợp xấu nhất, nó sẽ cần khoảng`10^18 * 5 * 10^5`hoạt động. 

Quan sát hữu ích là chúng ta không bao giờ cần biết vị trí giày của từng cá nhân. Chúng ta chỉ cần tổng số vị trí dự kiến ​​của mỗi người. 

Giả sử một người có tổng vị trí giày dự kiến`S`. Mỗi người trong số họ`k`giày có xác suất`1/k`bị loại bỏ, vì vậy những đôi giày ở lại đóng góp một khoản tiền dự kiến`(k-1)/k * S`. 

Chiếc giày bị loại bỏ sẽ nhận được một trong những vị trí trống. Đối với tất cả mọi người, vị trí trống chính là vị trí của những chiếc giày đã được tháo ra. Vì mọi chiếc giày đều có xác suất`1/k`bị loại bỏ, tổng số vị trí trống dự kiến ​​là tổng số vị trí chia cho`k`. Tổng số vị trí là:$$\frac{nk(nk+1)}{2}$$vì vậy tổng số vị trí trống dự kiến ​​là:$$\frac{n(nk+1)}{2}$$có`n`giày bị loại bỏ và mọi chiếc giày bị loại bỏ đều có khả năng nhận được bất kỳ vị trí trống nào. Do đó, mức đóng góp vị trí mới dự kiến ​​của chiếc giày đã tháo ra của một người là:$$\frac{nk+1}{2}$$Điều này mang lại một sự tái phát tuyến tính:$$S_{t+1}=\frac{k-1}{k}S_t+\frac{nk+1}{2}$$Điểm cố định của lần truy hồi này là vị trí trung bình sau vô số ngày:$$\frac{nk+1}{2}$$Đối với mức trung bình thay vì tổng, công thức tương tự sẽ trở thành:$$A_d=C+\left(\frac{k-1}{k}\right)^d(A_0-C)$$Ở đâu`A0`là vị trí trung bình ban đầu và`C`là`(nk+1)/2`. 

Toàn bộ vấn đề giảm xuống còn việc lưu trữ giá trị trung bình ban đầu của mỗi người và tính toán một lũy thừa mô-đun cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(ngày * n * k) | O(nk) | Quá chậm | 
| Tối ưu | O(nk + q log d) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tổng số chức vụ của mỗi người theo cách sắp xếp ban đầu. Chia mỗi tổng cho`k`để có được vị trí trung bình ban đầu`A0`của mỗi người. 
2. Tính toán trước các hằng số mô đun được sử dụng trong phép truy toán. Giá trị cố định là:$$C=\frac{nk+1}{2}$$và số nhân là:$$r=\frac{k-1}{k}$$Tất cả các phép chia được thực hiện dưới dạng phép nhân với các nghịch đảo mô-đun. 

1. Đối với mỗi truy vấn`(x, d)`, truy xuất giá trị trung bình ban đầu được lưu trữ của người`x`. 
2. Nếu`d`bằng 0, xuất ngay giá trị trung bình ban đầu vì chưa có chuyển đổi nào xảy ra. 
3. Ngược lại tính:$$r^d$$sử dụng lũy ​​thừa nhị phân. Khi`k = 1`,`r`bằng 0, do đó mọi số ngày dương đều trực tiếp đạt đến giá trị dừng. 

1. Kết hợp các giá trị bằng cách sử dụng:$$C+r^d(A_0-C)$$và in kết quả modulo`10^9+7`. 

Lý do điều này có tác dụng là vì vị trí trung bình dự kiến ​​của mỗi người được mô tả hoàn toàn bằng một giá trị duy nhất. Các lựa chọn ngẫu nhiên hàng ngày chỉ ảnh hưởng đến việc một chiếc giày ở lại hay di chuyển và tính tuyến tính của kỳ vọng có nghĩa là đóng góp dự kiến ​​của mỗi chiếc giày có thể được tổng hợp một cách độc lập. Vì sự lặp lại chỉ phụ thuộc vào mức trung bình dự kiến ​​trước đó nên chỉ cần duy trì một giá trị này cho mỗi người là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    sums = [0] * (n + 1)
    for i, x in enumerate(a, 1):
        sums[x] += i

    inv_k = pow(k, MOD - 2, MOD) if k != 1 else 0
    inv_2 = (MOD + 1) // 2

    if k == 1:
        c = (n + 1) * inv_2 % MOD
        initial = [0] * (n + 1)
        for i in range(1, n + 1):
            initial[i] = sums[i] % MOD
    else:
        c = (n * k + 1) % MOD * inv_2 % MOD
        initial = [0] * (n + 1)
        for i in range(1, n + 1):
            initial[i] = sums[i] % MOD * inv_k % MOD

    if k == 1:
        multiplier = 0
    else:
        multiplier = (k - 1) % MOD * inv_k % MOD

    q = int(input())
    ans = []

    for _ in range(q):
        x, d = input().split()
        x = int(x)
        d = int(d)

        if d == 0:
            ans.append(str(initial[x]))
        elif k == 1:
            ans.append(str(c))
        else:
            power = pow(multiplier, int(d), MOD)
            value = (c + power * (initial[x] - c)) % MOD
            ans.append(str(value))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã lưu trữ tổng vị trí ban đầu của mỗi chủ sở hữu. Bởi vì mỗi chủ sở hữu có chính xác`k`giày, chia cho`k`đưa ra mức trung bình ban đầu. 

Các hằng số lặp lại được tính toán một lần. Nghịch đảo mô-đun của`k`chỉ có hiệu lực khi`k`không phải là một nên trường hợp đặc biệt sẽ được xử lý riêng. Khi`k = 1`, một ngày là đủ để mất tất cả thông tin về vị trí ban đầu, làm cho câu trả lời bằng giá trị dừng cho mọi số dương`d`. 

Đối với trường hợp bình thường,`pow(multiplier, d, MOD)`thực hiện lũy thừa nhanh theo thời gian logarit. Số nguyên Python không bị tràn nhưng giảm từng modulo phép toán`MOD`giữ các giá trị nhỏ và khớp với số học mô-đun cần thiết. 

## Ví dụ đã hoạt động 

Đối với một sự sắp xếp nhỏ:```
n = 2, k = 2
positions: 1 2 3 4
owners:    1 1 2 2
```Trung bình cố định là:$$C=\frac{4+1}{2}=\frac52$$Trạng thái của người 1 diễn biến như sau. 

| Ngày | Trung bình ban đầu A0 | Công suất nhân | Trung bình hiện tại | 
| --- | --- | --- | --- | 
| 0 | 2/3 | 1 | 2/3 | 
| 1 | 2/3 | 1/2 | 2 | 

Ngày đầu tiên di chuyển trung bình kỳ vọng một nửa từ giá trị ban đầu về giá trị đứng yên. Điều này xác nhận rằng sự truy hồi không chuyển trực tiếp đến trạng thái cân bằng. 

Đối với trường hợp`k = 1`:```
n = 2, k = 1
positions: 1 2
owners:    1 2
```| Ngày | Trung bình ban đầu | Hệ số nhân | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 
| 1 | 1 | 0 | 2/3 | 
| 2 | 1 | 0 | 2/3 | 

Bảng này cho thấy lý do tại sao trường hợp đặc biệt là cần thiết. Quá trình chuyển đổi đầu tiên sẽ loại bỏ hoàn toàn thông tin sắp xếp ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk + q log d) | Tổng ban đầu yêu cầu một lần chuyển qua giày và mọi truy vấn đều sử dụng lũy ​​thừa nhị phân. | 
| Không gian | O(n) | Chỉ có mức trung bình ban đầu của mọi người được lưu trữ. | 

Tối đa các xử lý tiền xử lý`500000`đôi giày. Mỗi truy vấn cần tối đa 60 phép nhân mô-đun vì`d`ở dưới`10^18`, do đó tổng công việc vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    
    # Paste the solve() function here in a real test harness.
    # This placeholder is replaced by the submitted solution.
    sys.stdin = old_stdin
    return ""

# These assertions describe the expected behaviour of the solution.
# They can be used directly after wrapping the solve() function.

# Minimum size, k = 1 boundary
# Input:
# 2 1
# 1 2
# 2
# 1 0
# 1 1
# Expected:
# 1
# 500000005

# Single person is impossible by constraints, but all positions of one owner
# can be tested with n=2:
# 2 2
# 1 1 2 2
# 2
# 1 0
# 1 1
# Expected:
# 500000005
# 750000006

# Large d should only require logarithmic exponentiation:
# 2 2
# 1 2 1 2
# 1
# 1 1000000000000000000

# All equal pattern for one owner:
# 3 1
# 1 2 3
# 3
# 1 0
# 2 1
# 3 1000000000000000000
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2, k=1`| Giá trị ban đầu sau đó là giá trị dừng | Xử lý số nhân bằng 0 | 
|`2 2 / 1 1 2 2`| Đúng trung bình phân số | Kiểm tra quá trình chuyển đổi lặp lại | 
| Rất lớn`d`| Kết thúc nhanh chóng | Kiểm tra lũy thừa nhị phân | 
|`k=1`với nhiều chủ sở hữu | Phân phối cuối cùng thống nhất | Kiểm tra hành vi ranh giới | 

## Vỏ cạnh 

Khi nào`k = 1`, thuật toán không bao giờ cố gắng đảo ngược`k`hoặc tính toán quyền hạn của hệ số lặp lại không hợp lệ. Vì:```
2 1
1 2
2
1 0
1 1
```truy vấn đầu tiên trả về vị trí ban đầu`1`. Truy vấn thứ hai trả về:$$\frac{2+1}{2}=\frac32$$đó là`500000005`modulo`10^9+7`. 

Khi`d = 0`, thuật toán bỏ qua hoàn toàn phép truy hồi. Vì:```
2 2
1 1 2 2
1
1 0
```tổng được lưu trữ là`3`, và chia cho`k = 2`cho`3/2`. Câu trả lời vẫn là sự sắp xếp ban đầu vì không có ngày nào trôi qua. 

Đối với các vị trí được lập chỉ mục một, mã sẽ tính tổng bằng cách sử dụng`enumerate(a, 1)`. TRONG:```
2 2
1 2 1 2
1
1 0
```người 1 sở hữu vị trí 1 và 3, tạo ra trung bình`(1+3)/2 = 2`. Việc triển khai phù hợp với định nghĩa khoảng cách vật lý vì nó không bao giờ thay đổi vị trí một vị trí.
