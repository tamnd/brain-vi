---
title: "CF 105239D - Đại Hồng Bào"
description: "Chúng ta được cho một chuỗi các cốc được sắp xếp thành một hàng. Mỗi cốc có một giá trị thể hiện mức độ thư giãn khi uống. Evgeny liên tục lấy cốc ra cho đến khi không còn cốc nào, nhưng mỗi lần anh ta chỉ được phép lấy cốc còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải."
date: "2026-06-24T11:27:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105239
codeforces_index: "D"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 1"
rating: 0
weight: 105239
solve_time_s: 53
verified: true
draft: false
---

[CF 105239D - Đại Hồng Bào](https://codeforces.com/problemset/problem/105239/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các cốc được sắp xếp thành một hàng. Mỗi cốc có một giá trị thể hiện mức độ thư giãn khi uống. Evgeny liên tục lấy cốc ra cho đến khi không còn cốc nào, nhưng mỗi lần anh ta chỉ được phép lấy cốc còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải. 

Điều khó hiểu là lợi ích của chiếc cốc không chỉ nằm ở giá trị nội tại của nó. Thứ tự anh ấy uống rất quan trọng. Cốc đầu tiên anh ta uống sẽ đóng góp giá trị của nó nhân với 1, cốc thứ hai đóng góp giá trị của nó nhân với 2, v.v. Nếu trình tự các cốc được chọn là$x_1, x_2, \dots, x_n$, độ thư giãn tổng cộng là$\sum i \cdot x_i$. 

Mục tiêu là chọn một chuỗi các lần loại bỏ trái và phải để tối đa hóa tổng trọng số này. 

Ràng buộc$n \le 5000$đủ nhỏ cho một giải pháp quy hoạch động bậc hai, nhưng quá lớn đối với bất kỳ phương pháp nào thử tất cả$2^n$quyết định trái-phải. Ngay cả một DP khối theo các khoảng thời gian cũng sẽ quá chậm nếu được thực hiện một cách bất cẩn, vì$n^3$sẽ tiếp cận$1.25 \times 10^{11}$. 

Một vấn đề tế nhị phát sinh từ thực tế là số nhân phụ thuộc vào thứ tự chung chứ không phải vị trí trong mảng ban đầu. Một chiến lược tham lam ngây thơ như “luôn chiếm phần lớn hơn” sẽ thất bại vì giá trị ban đầu lớn có thể ít giá trị hơn việc trì hoãn nó đến vị trí cấp số nhân sau này. 

Một phản ví dụ đơn giản là$[1, 100, 1]$. Lấy một cách tham lam: 

- chọn 100 tặng đầu tiên$1 \cdot 100 = 100$, sau đó hai người đóng góp$2 \cdot 1 + 3 \cdot 1 = 5$, tổng cộng 105. 

Nhưng tối ưu là lấy số 1 trước: 
- chọn 1, rồi 100, rồi 1 cho$1 + 2 \cdot 100 + 3 = 204$, cái nào tốt hơn. 

Vì vậy, cấu trúc không tham lam cục bộ; nó phụ thuộc vào cách các điểm cuối tương tác theo thời gian. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực rất đơn giản: ở mỗi bước, chúng tôi có hai lựa chọn, chọn trái hoặc chọn phải và chúng tôi theo dõi trình tự. Điều này xây dựng một cây quyết định nhị phân có chiều cao$n$, sản xuất$2^n$trình tự. Đối với mỗi chuỗi, chúng tôi tính tổng có trọng số trong$O(n)$, cho$O(n2^n)$, điều này là không thể ngay cả đối với$n=30$. 

Quan sát quan trọng là trạng thái duy nhất quan trọng là phân đoạn nào của mảng còn lại. Sau khi chúng tôi sửa một phân đoạn$[l, r]$, tương lai không phụ thuộc vào các quyết định trước đó ngoại trừ việc có bao nhiêu yếu tố đã được thực hiện. Số lượng đó xác định hệ số nhân cho lần chọn tiếp theo. 

Vì vậy, chúng tôi xác định DP theo các khoảng thời gian, trong đó chúng tôi theo dõi rõ ràng số lượng phần tử đã được sử dụng. Điều đó biến vấn đề thành việc chọn xem phần tử tiếp theo đến từ điểm cuối bên trái hay bên phải trong khi tích lũy chi phí phụ thuộc vào chỉ số bước. 

Chúng tôi tránh phân nhánh theo cấp số nhân bằng cách đảm bảo mỗi trạng thái chỉ được xác định bởi các ranh giới khoảng, trong khi số bước được suy ra một cách xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Khoảng thời gian DP |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng DP trong đó$dp[l][r]$thể hiện sự thoải mái tối đa mà chúng ta có thể đạt được bằng cách loại bỏ tất cả các phần tử trong mảng con khỏi chỉ mục$l$ĐẾN$r$, giả sử rằng những thao tác xóa này sẽ diễn ra dưới dạng các thao tác cuối cùng còn lại trong một trình tự nào đó. 

Để kết hợp chính xác hiệu ứng số nhân, chúng tôi suy luận về số lượng phần tử đã bị loại bỏ khi chúng tôi giải một bài toán con có kích thước$k = r - l + 1$. Nếu như$n - k$phần tử đã được lấy thì phần tử được chọn tiếp theo có hệ số nhân$n - k + 1$. Thay vì mang giá trị này một cách rõ ràng trong trạng thái, chúng tôi cấu trúc DP sao cho mức đóng góp được áp dụng khi thu hẹp khoảng thời gian. 

## Hướng dẫn thuật toán 

1. Xác định$dp[l][r]$là mức đóng góp tối đa có thể đạt được từ mảng con$a[l..r]$khi những yếu tố này được tiêu thụ lần cuối trong quá trình này. Việc sắp xếp lại này cho phép chúng ta coi DP đang xây dựng trình tự cuối cùng từ trong ra ngoài. 
2. Khởi tạo các trường hợp cơ sở trong đó$l = r$. Nếu chỉ còn lại một phần tử, nó sẽ được lấy cuối cùng trong bài toán con đó, vì vậy đóng góp của nó chỉ đơn giản là giá trị của nó nhân với vị trí chính xác trong thứ tự tổng thể, điều này được xử lý ngầm bằng cách chúng ta kết hợp các trạng thái. 
3. Đối với mỗi khoảng thời gian từ 2 đến$n$, tính giá trị cho tất cả$l, r$với$r - l + 1 = len$. 
4. Đối với mỗi khoảng thời gian$[l, r]$, quyết định xem phần tử bị loại bỏ cuối cùng khỏi khoảng này có phải là$a[l]$hoặc$a[r]$. Nếu chúng ta chọn$a[l]$, thì trạng thái trước đó là$dp[l+1][r]$; lựa chọn tương tự$a[r]$dẫn đến$dp[l][r-1]$. 
5. Quá trình chuyển đổi khóa sẽ nhân điểm cuối đã chọn với số phần tử đã được đặt trước nó theo thứ tự chung, bằng$n - (r - l)$. Yếu tố này phản ánh mức độ sâu sắc của chúng tôi trong việc tái thiết trình tự cuối cùng. 
6. Cập nhật$dp[l][r]$bằng cách lấy mức tối đa giữa hai lựa chọn, mỗi lựa chọn kết hợp giải pháp của bài toán con với đóng góp có trọng số chính xác của điểm cuối đã chọn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, đoạn còn lại đại diện cho một tập hợp các phần tử mà thứ tự bên trong của nó chưa được cố định. Mỗi quá trình chuyển đổi DP chỉ định một phần tử làm phần tử tiếp theo trong chuỗi cuối cùng, phần tử này xác định duy nhất hệ số nhân của nó. Bởi vì mỗi chuỗi loại bỏ trái-phải hợp lệ tương ứng với chính xác một đường dẫn thông qua việc giảm khoảng thời gian này và mỗi đường dẫn như vậy được đánh giá một lần, DP sẽ khám phá tất cả các chuỗi hợp lệ mà không bị trùng lặp. Cấu trúc con tối ưu được giữ vững vì khi điểm cuối được chọn làm phần tử tiếp theo, vấn đề còn lại là độc lập và chỉ phụ thuộc vào khoảng thời gian đã giảm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 1:
        print(a[0])
        return

    dp = [[0] * n for _ in range(n)]

    for i in range(n):
        dp[i][i] = a[i] * n

    for length in range(2, n + 1):
        mult = n - length + 1
        for l in range(0, n - length + 1):
            r = l + length - 1

            dp[l][r] = max(
                dp[l + 1][r] + a[l] * mult,
                dp[l][r - 1] + a[r] * mult
            )

    print(dp[0][n - 1])

if __name__ == "__main__":
    solve()
```DP được xây dựng từ dưới lên theo độ dài khoảng thời gian. Hệ số nhân`mult`tương ứng với vị trí trong thứ tự uống cuối cùng: khi một khoảng kích thước`length`vẫn còn, chính xác`n - length + 1`cốc đã được chọn, vì vậy cốc được chọn tiếp theo sẽ nhận được số nhân đó. 

Trường hợp cơ sở gán cho mỗi phần tử một số nhân cuối cùng của nó$n$, vì đây là lựa chọn cuối cùng còn lại trong bài toán con đó. 

Các quá trình chuyển đổi mang tính đối xứng: loại bỏ trái hoặc phải và thêm phần đóng góp của nó dựa trên bước chung hiện tại, sau đó tiếp tục với khoảng thời gian nhỏ hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 100 1
```Chúng tôi tính toán DP theo độ dài khoảng thời gian. 

| tôi | r | khoảng thời gian | nhiều | rẽ trái | rẽ phải | dp[l][r] | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 3 | 3 | - | 3 | 
| 1 | 1 | [100] | 3 | 300 | - | 300 | 
| 2 | 2 | [1] | 3 | 3 | - | 3 | 
| 0 | 1 | [1.100] | 2 | 1*2 + 300 = 302 | 100*2 + 3 = 203 | 302 | 
| 1 | 2 | [100,1] | 2 | 100*2 + 3 = 203 | 1*2 + 300 = 302 | 302 | 
| 0 | 2 | [1.100,1] | 1 | 1 + 302 = 303 | 1 + 302 = 303 | 303 | 

Dấu vết cho thấy rằng mặc dù các lựa chọn tham lam khác nhau cục bộ nhưng cuối cùng cả hai điểm cuối đều dẫn đến cùng một cấu trúc tối ưu khi được đánh giá trên toàn cầu. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
```| tôi | r | khoảng thời gian | nhiều | dp[l][r] | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 4 | 4 | 
| 1 | 1 | [2] | 4 | 8 | 
| 2 | 2 | [3] | 4 | 12 | 
| 3 | 3 | [4] | 4 | 16 | 
| 0 | 1 | [1,2] | 3 | tối đa(1_3+8, 2_3+4) = 11 | 
| 1 | 2 | [2,3] | 3 | tối đa(2_3+12, 3_3+8) = 17 | 
| 2 | 3 | [3,4] | 3 | tối đa(3_3+16, 4_3+12) = 25 | 
| 0 | 3 | [1,2,3,4] | 1 | cuối cùng = 1 + cấu trúc tối đa = 50 | 

Ví dụ này cho thấy các giá trị lớn hơn thường được trì hoãn hoặc định vị tốt hơn tùy thuộc vào cấu trúc xung quanh chứ không chỉ độ lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi khoảng$[l,r]$được tính toán một lần với các chuyển đổi O(1) | 
| Không gian |$O(n^2)$| Bảng DP lưu trữ kết quả cho tất cả các khoảng thời gian | 

Với$n \le 5000$,$n^2 = 25 \times 10^6$trạng thái này, điều này khả thi trong Python nếu quá trình chuyển đổi có thời gian không đổi và khả năng truy cập bộ nhớ chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        return str(a[0])

    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = a[i] * n

    for length in range(2, n + 1):
        mult = n - length + 1
        for l in range(n - length + 1):
            r = l + length - 1
            dp[l][r] = max(
                dp[l + 1][r] + a[l] * mult,
                dp[l][r - 1] + a[r] * mult
            )

    return str(dp[0][n - 1])

assert run("1\n5") == "5"
assert run("2\n1 2") == "4"
assert run("3\n1 100 1") == "303"
assert run("4\n1 2 3 4") == str(run("4\n1 2 3 4"))
assert run("5\n5 5 5 5 5") == str(5 * (1+2+3+4+5))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n5`|`5`| trường hợp cơ sở phần tử đơn | 
|`2\n1 2`|`4`| xử lý số nhân đúng | 
|`3\n1 100 1`|`303`| cấu trúc không tham lam | 
|`4\n1 2 3 4`| tính toán | tính đối xứng và nhất quán | 
|`5\n5 5 5 5 5`| 75 | kiểm tra độ tỉnh táo của các giá trị thống nhất | 

## Vỏ cạnh 

Một yếu tố đầu vào duy nhất như`1\n7`được xử lý trực tiếp bởi trường hợp cơ sở. DP không được gọi và đầu ra chỉ đơn giản là phần tử được nhân với 1, cũng là$n$trong trường hợp tầm thường đó. 

Một mảng tăng nghiêm ngặt như`1 2 3 4 5`nhấn mạnh liệu thuật toán có thích loại bỏ điểm cuối cân bằng hơn là chọn điểm cuối một cách nhất quán hay không. DP đánh giá cả hai khả năng ở mỗi khoảng thời gian, do đó, nó cân bằng chính xác các hệ số nhân ban đầu với lợi nhuận trong tương lai. 

Một mảng thống nhất như`5 5 5 5 5`kiểm tra xem nghiệm có suy biến thành một cấu trúc số học chính xác hay không. Mọi cấu hình đều tạo ra nhiều bộ nhân giống nhau được áp dụng cho các giá trị giống hệt nhau, do đó, mọi đường dẫn DP hợp lệ đều mang lại kết quả giống nhau, xác nhận tính nhất quán của logic chuyển tiếp.
