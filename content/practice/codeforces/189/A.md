---
title: "CF 189A - Cắt Ruy Băng"
description: "Chúng ta có một dải ruy băng có tổng chiều dài n. Mỗi mảnh cắt phải có một trong chính xác ba chiều dài cho phép: a, b hoặc c. Mục tiêu không chỉ là chia dải băng thành công mà còn là tối đa hóa tổng số mảnh chúng ta thu được."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp"]
categories: ["algorithms"]
codeforces_contest: 189
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 119 (Div. 2)"
rating: 1300
weight: 189
solve_time_s: 91
verified: true
draft: false
---

[CF 189A - Cắt ruy băng](https://codeforces.com/problemset/problem/189/A) 

**Đánh giá:** 1300 
**Tags:** vũ lực, dp 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dải ruy băng có tổng chiều dài`n`. Mỗi mảnh cắt phải có một trong chính xác ba chiều dài cho phép:`a`,`b`, hoặc`c`. Mục tiêu không chỉ là chia dải băng thành công mà còn là tối đa hóa tổng số mảnh chúng ta thu được. 

Ví dụ: nếu chiều dài ruy băng là`5`và độ dài cho phép là`5`,`3`, Và`2`, cắt nó thành một đoạn dài`5`hoạt động, nhưng cắt nó thành từng mảnh`3 + 2`tạo ra hai mảnh, cái nào tốt hơn. 

Ràng buộc`n ≤ 4000`đủ nhỏ để lập trình động. Một thuật toán có khoảng`O(n)`hoặc`O(n^2)`hoạt động dễ dàng phù hợp trong giới hạn. Một phép đệ quy mạnh mẽ thực sự khám phá tất cả các chuỗi cắt có thể vẫn sẽ chấm dứt đối với các giới hạn này, nhưng nó lặp lại các bài toán con giống nhau nhiều lần và trở nên tốn kém một cách không cần thiết. 

Phần khó khăn là không phải mọi độ dài trung gian đều có thể truy cập được. Việc triển khai bất cẩn có thể vô tình coi các trạng thái không thể là hợp lệ và xây dựng câu trả lời từ chúng. 

Hãy xem xét ví dụ này:```
7 5 2 5
```Câu trả lời đúng là`2`, bởi vì chúng ta có thể cắt`5 + 2`. 

Giả sử một mảng DP được khởi tạo bằng số không. Sau đó nêu`dp[1]`sẽ xuất hiện không chính xác có thể truy cập được với giá trị`0`và các quá trình chuyển đổi sau này có thể xây dựng các giải pháp giả mạo từ nó. Các trạng thái không thể thực hiện được phải được đánh dấu rõ ràng, thường bằng`-1`hoặc âm vô cùng. 

Một trường hợp tinh vi khác xuất hiện khi tất cả các độ dài đều bằng nhau:```
9 3 3 3
```Câu trả lời đúng là`3`. Một số triển khai cố gắng loại bỏ các độ dài trùng lặp hoặc sử dụng logic phân nhánh không cần thiết làm phức tạp quá trình chuyển đổi. Vấn đề trở nên đơn giản hơn nhiều nếu chúng ta coi cả ba độ dài là như nhau. 

Thêm một trường hợp khó khăn nữa là khi câu trả lời tối ưu yêu cầu nhiều vết cắt nhỏ thay vì vết cắt hợp lệ lớn hơn:```
8 5 3 2
```Các phân tách hợp lệ bao gồm`5 + 3`với hai mảnh và`2 + 2 + 2 + 2`với bốn mảnh. Ở đây, một chiến lược tham lam luôn chiếm phần lớn nhất sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi trình tự cắt có thể. Từ một dải ruy băng dài`x`, chúng tôi cố gắng cắt bỏ một cách đệ quy`a`,`b`, Và`c`, sau đó giải độ dài còn lại. 

Phép đệ quy đương nhiên là đúng vì mọi trình tự cắt hợp lệ đều được khám phá. Vấn đề là sự lặp lại. Nếu chúng ta tính toán câu trả lời tốt nhất cho độ dài`20`, sau này chúng ta có thể tính toán lại nó từ một đường dẫn khác. Số lượng cuộc gọi đệ quy tăng theo cấp số nhân trong trường hợp xấu nhất. 

Vì`n = 4000`, độ phức tạp theo cấp số nhân là quá lớn. Ngay cả một cái gì đó như`3^30`hoạt động đã không thể thực hiện được trong vòng một giây. 

Quan sát quan trọng là vấn đề có các vấn đề con chồng chéo. Câu trả lời tốt nhất cho chiều dài ruy băng`x`chỉ phụ thuộc vào câu trả lời tốt nhất trong thời gian dài`x-a`,`x-b`, Và`x-c`. 

Điều đó làm cho vấn đề này trở thành một vấn đề lập trình động cổ điển. 

Chúng tôi xác định:```
dp[i] = maximum number of pieces that can form length i
```Nếu độ dài không thể được hình thành, chúng tôi đánh dấu nó là không thể. 

Từ một chiều dài có thể tiếp cận`i`, chúng tôi thử mở rộng nó bằng cách sử dụng từng kích thước cắt được phép. Mỗi lần chuyển đổi sẽ thêm một phần nữa. 

Bởi vì mỗi trạng thái chỉ phụ thuộc vào độ dài nhỏ hơn nên chúng ta có thể tính toán các câu trả lời lặp đi lặp lại từ`0`lên đến`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | Ngăn xếp đệ quy O(n) | Quá chậm | 
| Lập trình động tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng DP có kích thước`n + 1`. 

Mỗi vị trí`dp[i]`lưu trữ số lượng mảnh tối đa có thể tạo ra tổng chiều dài`i`. 
2. Khởi tạo tất cả các trạng thái không thể thực hiện được. 

Chúng tôi sử dụng`-1`cho các trạng thái không thể bởi vì`0`là một câu trả lời hợp lệ chỉ cho độ dài`0`. 
3. Đặt`dp[0] = 0`. 

Một dải ruy băng có chiều dài`0`cần 0 mảnh, vì vậy đây là trạng thái bắt đầu của chúng tôi. 
4. Lặp lại tất cả các độ dài từ`0`ĐẾN`n`. 

Đối với mỗi độ dài có thể tiếp cận, hãy thử thêm một đoạn độ dài nữa`a`,`b`, Và`c`. 
5. Đối với mỗi chiều dài cắt`x`TRONG`{a, b, c}`:

Nếu như`i + x ≤ n`, cập nhật:```
dp[i + x] = max(dp[i + x], dp[i] + 1)
```Chúng tôi thêm một mảnh vì chúng tôi đang sử dụng một mảnh ruy băng bổ sung. 
6. Sau khi xử lý tất cả các trạng thái, in`dp[n]`. 

Tuyên bố đảm bảo tồn tại ít nhất một lần cắt hợp lệ, vì vậy câu trả lời cuối cùng sẽ luôn có sẵn. 

### Tại sao nó hoạt động 

Bất biến DP là:```
dp[i] always stores the maximum number of pieces for total length i
```Chúng ta bắt đầu với trạng thái chắc chắn đúng duy nhất,`dp[0] = 0`. 

Bất cứ khi nào chúng tôi mở rộng trạng thái có thể truy cập bằng một lần cắt được phép, chúng tôi sẽ tạo một phân tách dải băng hợp lệ khác. Vì mỗi quá trình chuyển đổi sẽ thêm chính xác một phần, nên việc lấy mức tối đa sẽ bảo toàn được sự phân tách tốt nhất có thể cho mỗi độ dài. 

Mọi trình tự cắt hợp lệ đều tương ứng với một chuỗi chuyển tiếp nào đó trong DP. Vì tất cả các chuyển đổi như vậy đã được khám phá nên câu trả lời tối ưu cho`n`được đảm bảo sẽ được tìm thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b, c = map(int, input().split())

    dp = [-1] * (n + 1)
    dp[0] = 0

    cuts = [a, b, c]

    for i in range(n + 1):
        if dp[i] == -1:
            continue

        for x in cuts:
            if i + x <= n:
                dp[i + x] = max(dp[i + x], dp[i] + 1)

    print(dp[n])

solve()
```Mảng DP được khởi tạo bằng`-1`vì vậy các trạng thái không thể truy cập được tách biệt rõ ràng với các trạng thái hợp lệ. Điều này tránh việc vô tình xây dựng các chuyển đổi từ độ dài không thể. 

Trường hợp cơ sở`dp[0] = 0`đại diện cho một dải băng trống trước khi thực hiện bất kỳ vết cắt nào. 

Vòng lặp bên ngoài xử lý mọi chiều dài ruy băng có thể tiếp cận được. Vòng lặp bên trong thử cả ba lần cắt được phép. Nếu việc thêm phần cắt vẫn nằm trong giới hạn, chúng tôi sẽ cập nhật trạng thái đích với số lượng phần tốt nhất được thấy cho đến nay. 

Một chi tiết triển khai tinh tế là bỏ qua các trạng thái trong đó`dp[i] == -1`. Nếu không có sự kiểm tra này, các trạng thái không thể thực hiện được sẽ tạo ra các chuyển đổi không chính xác. 

Một chi tiết khác đang sử dụng`max`. Các phân tách khác nhau có thể đạt đến tổng chiều dài như nhau và chúng ta phải giữ lại phân tách tạo ra nhiều mảnh nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5 3 2
```| Hiện hành`i`|`dp[i]`| Chuyển tiếp | Trạng thái cập nhật | 
| --- | --- | --- | --- | 
| 0 | 0 | +5 | dp[5] = 1 | 
| 0 | 0 | +3 | dp[3] = 1 | 
| 0 | 0 | +2 | dp[2] = 1 | 
| 2 | 1 | +3 | dp[5] = 2 | 
| 3 | 1 | +2 | dp[5] = 2 | 

Giá trị cuối cùng:```
dp[5] = 2
```Dấu vết cho thấy tại sao việc tối đa hóa là cần thiết. Chiều dài`5`đầu tiên được tiếp cận trực tiếp bằng một phần, sau đó được cải thiện bằng cách sử dụng`3 + 2`. 

### Ví dụ 2 

đầu vào:```
8 5 3 2
```| Hiện hành`i`|`dp[i]`| Chuyển tiếp | Trạng thái cập nhật | 
| --- | --- | --- | --- | 
| 0 | 0 | +5 | dp[5] = 1 | 
| 0 | 0 | +3 | dp[3] = 1 | 
| 0 | 0 | +2 | dp[2] = 1 | 
| 2 | 1 | +2 | dp[4] = 2 | 
| 4 | 2 | +2 | dp[6] = 3 | 
| 6 | 3 | +2 | dp[8] = 4 | 

Giá trị cuối cùng:```
dp[8] = 4
```Ví dụ này chứng minh tại sao các chiến lược tham lam lại thất bại. Sử dụng mảnh lớn nhất trước tiên chỉ tạo ra hai mảnh, trong khi các vết cắt nhỏ hơn lặp đi lặp lại sẽ tạo ra bốn mảnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi độ dài xử lý chính xác ba lần chuyển tiếp | 
| Không gian | O(n) | Mảng DP lưu trữ một giá trị trên mỗi chiều dài ruy băng | 

Với`n ≤ 4000`, thuật toán chỉ thực hiện khoảng`3 * 4000`chuyển tiếp, đó là tầm thường trong thời hạn. Việc sử dụng bộ nhớ cũng rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, a, b, c = map(int, input().split())

    dp = [-1] * (n + 1)
    dp[0] = 0

    for i in range(n + 1):
        if dp[i] == -1:
            continue

        for x in (a, b, c):
            if i + x <= n:
                dp[i + x] = max(dp[i + x], dp[i] + 1)

    print(dp[n])

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("5 5 3 2\n") == "2", "sample 1"

# minimum-size valid input
assert run("1 1 1 1\n") == "1", "minimum case"

# all lengths equal
assert run("9 3 3 3\n") == "3", "all equal lengths"

# greedy would fail here
assert run("8 5 3 2\n") == "4", "many small cuts are optimal"

# boundary-style combination
assert run("7 5 2 5\n") == "2", "mixed reachable states"

# larger input
assert run("4000 1 2 3\n") == "4000", "maximum n with unit cuts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`|`1`| Đầu vào hợp lệ tối thiểu | 
|`9 3 3 3`|`3`| Độ dài cắt trùng lặp | 
|`8 5 3 2`|`4`| Cách tiếp cận tham lam thất bại | 
|`7 5 2 5`|`2`| Các trạng thái trung gian bất khả thi được xử lý chính xác | 
|`4000 1 2 3`|`4000`| Kích thước ranh giới tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào này:```
7 5 2 5
```DP bắt đầu bằng:```
dp[0] = 0
```Từ chiều dài`0`, chúng ta có thể đạt tới độ dài`5`Và`2`.```
dp[5] = 1
dp[2] = 1
```Từ chiều dài`2`, thêm cái khác`5`đạt tới`7`:```
dp[7] = 2
```Những trạng thái không thể xảy ra như`1`,`3`, Và`4`duy trì`-1`, do đó không có chuyển tiếp không hợp lệ nào được tạo ra từ chúng. 

Bây giờ hãy xem xét kích thước cắt bằng nhau:```
9 3 3 3
```Thuật toán vẫn hoạt động bình thường vì mỗi lần chuyển đổi được xử lý độc lập. Các bản cập nhật trở thành:```
dp[3] = 1
dp[6] = 2
dp[9] = 3
```Các giá trị trùng lặp không yêu cầu xử lý đặc biệt. 

Cuối cùng, hãy xem xét trường hợp tham lam thất bại:```
8 5 3 2
```Lựa chọn chiến lược tham lam`5`đầu tiên chỉ sản xuất:```
5 + 3 = 8
```với hai mảnh. 

DP khám phá tất cả các phân tách có thể tiếp cận, bao gồm cả các phân tách lặp lại`2`cắt giảm:```
2 + 2 + 2 + 2 = 8
```trong đó chính xác cho bốn phần.
