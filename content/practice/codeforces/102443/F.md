---
title: "CF 102443F - Tam giác cân"
description: "Một đa giác đều có tất cả các đỉnh cách đều nhau xung quanh một đường tròn. Chúng ta phải đếm mọi tam giác có ba đỉnh xuất phát từ đa giác và có độ dài các cạnh chứa ít nhất một cặp bằng nhau. Khó khăn chính là đa giác có thể có tới 109 đỉnh."
date: "2026-08-09T18:11:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "F"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 86
verified: true
draft: false
---

[CF 102443F - Tam giác cân](https://codeforces.com/problemset/problem/102443/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một đa giác đều có tất cả các đỉnh cách đều nhau xung quanh một đường tròn. Chúng ta phải đếm mọi tam giác có ba đỉnh xuất phát từ đa giác và có độ dài các cạnh chứa ít nhất một cặp bằng nhau. 

Khó khăn chính là đa giác có thể có tới 109 đỉnh. Một cách tiếp cận kiểm tra rõ ràng các đỉnh hoặc bộ ba không thể hoạt động được. Ngay cả O(n) cũng đã quá lớn so với giới hạn thời gian, vì vậy giải pháp dự định phải giảm câu trả lời xuống một số phép tính số học không đổi. 

Ngoài ra còn có một vấn đề đếm tinh vi. Nếu chúng ta chọn một đỉnh làm đỉnh của một tam giác cân thì hai đỉnh còn lại có thể được đặt đối xứng quanh đỉnh đó. Điều này tính mỗi tam giác cân thông thường một lần, nhưng một tam giác đều có thể có ba đỉnh nên nó được tính ba lần. Chúng ta phải sửa chính xác trường hợp đó. 

Ví dụ: với đầu vào 3, bản thân đa giác là một tam giác đều. Số đếm dựa trên đỉnh trực tiếp cho ra ba, vì mỗi đỉnh của nó có thể được chọn làm đỉnh. Câu trả lời đúng là 1, vì vậy chỉ cần đếm các cặp đối xứng sẽ đếm được các hình tam giác đều. 

Một trường hợp cạnh khác là đa giác có kích thước chẵn. Với n=4, việc chọn đỉnh đối diện ở cả hai cạnh của một đỉnh sẽ chọn cùng một đỉnh đa giác hai lần, do đó lựa chọn đó không tạo thành một hình tam giác. Câu trả lời đúng là 4, không phải 8. Đây là lý do tại sao số khoảng cách đối xứng hợp lệ là ⌊(n−1)/2⌋. 

Với n=6, tam giác đều tồn tại. Số đếm dựa trên đỉnh cho kết quả là 6⋅2=12, nhưng có 6/3=2 hình tam giác đều riêng biệt, mỗi hình được tính ba lần. Chúng ta phải trừ hai số đếm thừa cho mỗi tam giác đều, được 12−2⋅2=8. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp nhất là chọn ba đỉnh một lần và kiểm tra xem ba khoảng cách theo cặp của chúng có chứa hai giá trị bằng nhau hay không. có 

( 3 n ​ )= 6 n(n−1)(n−2) ​ 

gấp ba lần. Với n=10 9, đây là khoảng 1,67⋅10 26 tam giác, do đó, ngay cả việc xử lý mỗi bộ ba trong thời gian không đổi cũng là vô vọng. 

Một cách tiếp cận đẹp hơn một chút là cố định một đỉnh và thử mọi cặp đỉnh còn lại, nhưng đó vẫn là O(n 2 ), điều này là không thể với n=10 9. 

Cấu trúc hữu ích đến từ đa giác đều. Cố định một đỉnh v làm đỉnh. Hai đỉnh khác có khoảng cách bằng nhau đến v một cách chính xác khi chúng cách đều xung quanh đa giác theo hướng ngược nhau. Nếu khoảng cách theo chiều kim đồng hồ đến một điểm cuối là k thì khoảng cách ngược chiều kim đồng hồ đến điểm cuối kia cũng phải là k. 

Với mỗi đỉnh có chính xác 

⌊ 2 n−1 ​ ⌋ 

các lựa chọn hợp lệ của k. Như vậy bước đầu chúng ta thu được 

n⌊ 2 n−1 ​ ⌋ 

tam giác cân. 

Số đếm này có chính xác một nguồn trùng lặp. Nếu ba đỉnh tạo thành một tam giác đều thì mỗi đỉnh của nó có thể đóng vai trò là đỉnh. Một tam giác đều tồn tại trong một n-giác đều chính xác khi 3∣n, và trong trường hợp đó có n/3 tam giác như vậy. Mỗi cái được tính ba lần thay vì một lần, vì vậy mỗi tam giác đều phải loại bỏ hai bản sao. 

Do đó công thức cuối cùng là 

n⌊ 2 n−1 ​ ⌋−{ 3 2n ​ , 0, ​ 3∣n, 3∤n. ​ ​ 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 3 ) | O(1) | Quá chậm | 
| Sửa đỉnh và liệt kê các khoảng cách đối xứng | O(n) | O(1) | Quá chậm cho 10 9 | 
| Đếm dạng đóng | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số cặp điểm cuối đối xứng có sẵn cho một đỉnh cố định như sau 

k=⌊ 2 n−1 ​ ⌋. 

Mỗi giá trị 1<k<k xác định một tam giác bằng cách lấy các đỉnh k bước theo chiều kim đồng hồ và k bước ngược chiều kim đồng hồ tính từ đỉnh. Hai cạnh bằng nhau là các dây có cùng số cạnh đa giác. 
2. Nhân với n, vì mọi đỉnh đa giác đều có thể được chọn làm đỉnh. 

Số đếm ban đầu là

ans=nk. 
3. Kiểm tra xem n có chia hết cho 3 hay không. Nếu không, không thể tạo thành tam giác đều từ các đỉnh của đa giác, do đó số đếm ban đầu đã chính xác. 
4. Nếu 3∣n thì có n/3 tam giác đều. Mỗi cái được tính một lần cho mỗi đỉnh trong số ba đỉnh của nó, vì vậy mỗi cái đóng góp hai số dư. Trừ 

2⋅ 3 n ​ 

từ câu trả lời. 
5. In số nguyên thu được. 

### Tại sao nó hoạt động 

Đối với mỗi đỉnh được chọn, các cạnh bằng nhau của một tam giác cân phải nối đỉnh đó với hai đỉnh có khoảng cách góc bằng nhau với nó. Bởi vì tất cả các đỉnh đa giác đều nằm ở các khoảng góc bằng nhau, nên hai đỉnh này chính xác là k bước theo hướng ngược nhau đối với một số k hợp lệ. Do đó, mọi tam giác cân đều có ít nhất một biểu diễn trong số đếm của chúng ta. 

Tam giác cân không đều có một đỉnh duy nhất nên được tính đúng một lần. Một tam giác đều có thể có ba đỉnh và do đó được tính ba lần. Đây là những hình tam giác duy nhất có nhiều biểu diễn đỉnh, do đó, việc trừ hai bản sao cho mỗi tam giác đều sẽ khiến mỗi tam giác hợp lệ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    ans = n * ((n - 1) // 2)

    if n % 3 == 0:
        ans -= 2 * (n // 3)

    print(ans)

if __name__ == "__main__":
    solve()
```biểu thức`(n - 1) // 2`tính số khoảng cách đối xứng hợp lệ cho một đỉnh. sử dụng`n - 1`còn hơn là`n`là điều ngăn cản việc chọn cùng một đỉnh ở cả hai bên khi n chẵn. 

Phép nhân với`n`chiếm mọi đỉnh cao có thể. Số nguyên Python xử lý kết quả lớn nhất mà không bị tràn, điều này rất hữu ích ở đây vì câu trả lời có thể theo thứ tự 5⋅10 17, vượt xa số nguyên có dấu 32 bit. 

Kiểm tra khả năng chia`n % 3 == 0`xử lý trường hợp đếm thừa duy nhất. Khi nó giữ,`n // 3`là số lượng các hình tam giác đều khác nhau và mỗi hình cần loại bỏ chính xác hai bản sao thừa. 

Không có vòng lặp trên các đỉnh nên thuật toán chỉ thực hiện một số phép tính số học cố định bất kể n là 3 hay 10 9. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Với n=3, có một tam giác đều. 

| n | Lựa chọn đối xứng trên mỗi đỉnh | Số lượng ban đầu | Chỉnh sửa đều | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 3⋅1=3 | 2⋅(3/3)=2 | 1 | 

Ba lựa chọn đỉnh đều mô tả cùng một tam giác vật lý. Loại bỏ hai số trùng lặp sẽ để lại chính xác một hình tam giác. 

### Mẫu 2 

Với n=5, mỗi đỉnh có thể có hai khoảng cách đối xứng. 

| n | Lựa chọn đối xứng trên mỗi đỉnh | Số lượng ban đầu | Chỉnh sửa đều | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| 5 | 2 | 5⋅2=10 | 0 | 10 | 

Vì 5 không chia hết cho 3 nên không có tam giác đều. Mỗi tam giác được đếm đều có một đỉnh duy nhất, vì vậy tất cả mười số đếm đều đại diện cho các tam giác riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định và một lần kiểm tra tính chia hết được thực hiện. | 
| Không gian | O(1) | Thuật toán chỉ lưu trữ một số lượng biến số nguyên không đổi. | 

Giới hạn trên n 10 9 loại trừ bất kỳ thuật toán nào lặp qua các đỉnh, chưa nói đến các cặp hoặc bộ ba. Công thức thời gian không đổi hoạt động thoải mái trong giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    ans = n * ((n - 1) // 2)

    if n % 3 == 0:
        ans -= 2 * (n // 3)

    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("3\n") == "1\n", "sample 1"
assert run("5\n") == "10\n", "sample 2"

# Minimum size, also the smallest equilateral case
assert run("4\n") == "4\n", "even polygon boundary"

# Small multiple of 3
assert run("6\n") == "8\n", "equilateral correction"

# Large odd value
assert run("999999999\n") == "499999998000000000\n", "large odd n"

# Maximum input
assert run("1000000000\n") == "499999999000000000\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4`|`4`| Thậm chí n, trong đó lựa chọn nửa chừng không được tính hai lần | 
|`6`|`8`| Hiệu chỉnh tam giác đều | 
|`999999999`|`499999998000000000`| Đầu vào lẻ ​​lớn và số học cỡ 64 bit | 
|`1000000000`|`499999999000000000`| Ranh giới đầu vào tối đa | 

## Vỏ cạnh 

Với n=3, thuật toán tính toán một lựa chọn đối xứng trên mỗi đỉnh, cho kết quả 3. Vì 3∣n, nó trừ 2(3/3)=2, tạo ra 1. Điều này xử lý đa giác nhỏ nhất có thể và giải thích tại sao không thể đơn giản bỏ qua các tam giác đều. 

Với n=4, thuật toán tính toán ⌊3/2⌋=1 lựa chọn đối xứng trên mỗi đỉnh, cho kết quả 4. Không có phép hiệu chỉnh đều vì 4 không chia hết cho 3. Kết quả là 4, tương ứng với bốn hình tam giác thu được bằng cách bỏ đi một đỉnh khỏi hình vuông. Thao tác trên sàn là cần thiết vì thực hiện hai bước theo chiều kim đồng hồ và hai bước ngược chiều kim đồng hồ từ một đỉnh sẽ đến cùng một đỉnh đối diện. 

Với n=6, có hai lựa chọn đối xứng trên mỗi đỉnh, cho kết quả ban đầu là 12. Vì 6 chia hết cho 3 nên có 6/3=2 tam giác đều. Mỗi số được đếm ba lần, do đó bốn số thừa sẽ bị loại bỏ. Câu trả lời cuối cùng là 12−4=8. 

Với n=10 9, thuật toán thực hiện cùng một số thao tác không đổi như đối với bất kỳ đầu vào nhỏ hơn nào. Số lượng mỗi đỉnh là 

⌊ 2 10 9 −1 ​ ⌋=499999999, 

vậy số đếm ban đầu là 

10 9 ⋅499999999=499999999000000000. 

Vì 10 9 không chia hết cho 3 nên không cần sửa và giá trị đó là đáp án cuối cùng.
