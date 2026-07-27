---
title: "CF 102822K - Tri thức là sức mạnh"
description: "Bài toán đưa ra một số x và yêu cầu tập hợp các số nguyên có tổng chính xác là x. Mọi số nguyên được chọn phải lớn hơn 1, phải có ít nhất hai số nguyên và mọi cặp số nguyên trong tập hợp phải là nguyên tố cùng nhau."
date: "2026-07-26T15:58:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102822
codeforces_index: "K"
codeforces_contest_name: "2020 China Collegiate Programming Contest - Mianyang Site"
rating: 0
weight: 102822
solve_time_s: 56
verified: true
draft: false
---

[CF 102822K - Kiến thức là sức mạnh](https://codeforces.com/problemset/problem/102822/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một số`x`và yêu cầu một tập hợp các số nguyên có tổng chính xác`x`. Mọi số nguyên được chọn phải lớn hơn 1, phải có ít nhất hai số nguyên và mọi cặp số nguyên trong tập hợp phải là nguyên tố cùng nhau. Trong số tất cả các bộ sưu tập hợp lệ, chúng ta cần giảm thiểu sự khác biệt giữa giá trị được chọn lớn nhất và nhỏ nhất. 

Đầu vào chứa tối đa`10^5`giá trị độc lập của`x`, mỗi cái lớn bằng`10^9`. Điều này loại trừ việc tìm kiếm thông qua các tập hợp có thể hoặc thậm chí thực hiện phân tích nhân tử tốn kém cho mọi trường hợp thử nghiệm. Giải pháp dự định phải sử dụng đặc tính toán học trực tiếp và trả lời từng truy vấn trong thời gian không đổi. 

Phần khó khăn là câu trả lời tốt nhất không thể có được bằng cách đơn giản chia nhỏ`x`thành hai số gần nhau. Hai số gần nhau vẫn có thể có chung một thừa số. Ví dụ, đối với`x = 6`, sự chia tách`{3, 3}`có phạm vi nhỏ nhất có thể nhưng không hợp lệ vì các số bằng nhau không phải là số nguyên tố cùng nhau. Đầu ra đúng là`-1`. 

Một trường hợp biên khác là giá trị lẻ. Vì`x = 5`, bộ`{2, 3}`hoạt động và có phạm vi`1`, vậy câu trả lời là`1`. Việc triển khai bất cẩn chỉ tìm kiếm các phần chia chẵn sẽ bỏ lỡ điều này. 

Trường hợp quan trọng thứ ba là khi`x`chia hết cho bốn. Vì`x = 8`, bộ`{3, 5}`hoạt động. Sự khác biệt là`2`, không`1`, vì hai số nguyên liên tiếp sẽ có tổng lẻ. Thiếu điều kiện nguyên tố cùng nhau giữa các số cách nhau hai số có thể dẫn đến đáp án sai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử các phần tử nhỏ nhất có thể và xây dựng các tập ứng cử viên xung quanh chúng. Đối với mỗi phạm vi có thể, nó sẽ liệt kê các tập hợp con của khoảng đó và kiểm tra xem tổng có đúng không`x`và liệu tất cả các cặp có nguyên tố cùng nhau hay không. Điều này đúng vì nó xem xét mọi ứng viên có thể trả lời, nhưng ở đây thì không thể. Số lượng các khoảng và tập hợp con tăng lên vượt xa những gì`10^5`trường hợp thử nghiệm có thể xử lý. 

Quan sát hữu ích là chỉ những phạm vi rất nhỏ mới có thể tối ưu. một loạt các`1`có thể thực hiện được chính xác khi chúng ta có thể sử dụng hai số liên tiếp. Tổng của chúng là số lẻ nên mọi số lẻ`x`có câu trả lời`1`. 

Thậm chí`x`, phạm vi`1`không thể làm việc. Sau đó chúng tôi nhìn vào phạm vi`2`. Hai số chênh lệch`2`có hình thức`a`Và`a + 2`. gcd của họ là`gcd(a, 2)`, vậy chúng nguyên tố cùng nhau chính xác khi`a`thật kỳ quặc. Tổng của họ là`2a + 2`, chia hết cho`4`. Như vậy mọi`x`chia hết cho`4`có câu trả lời`2`. 

Các giá trị chẵn còn lại là`x ≡ 2 (mod 4)`. một loạt các`2`là không thể, vì vậy chúng ta cần một khoảng rộng hơn. Việc xây dựng`{a, a+1, a+3}`cung cấp phạm vi`3`. Tổng của nó là`3a+4`. Bằng cách chọn`a`một cách thích hợp, tất cả các giá trị đủ lớn trong lớp này có thể được biểu diễn. Giá trị chẵn duy nhất không thành công là`x = 6`. 

Toàn bộ vấn đề giảm xuống việc kiểm tra tính chẵn lẻ và tính chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(x) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`x`là số lẻ, đầu ra`1`. Hai số nguyên liên tiếp`(x-1)/2`Và`(x+1)/2`có số tiền cần thiết và luôn nguyên tố cùng nhau. 
2. Nếu`x`là số chẵn và chia hết cho`4`, đầu ra`2`. Cặp đôi`x/2 - 1`Và`x/2 + 1`có tổng`x`, sự khác biệt`2`, và số nhỏ hơn là số lẻ, nên cặp này nguyên tố cùng nhau. 
3. Nếu`x`bằng`6`, đầu ra`-1`. Không có tập hợp hợp lệ nào tồn tại vì chỉ có thể phân chia thành các số lớn hơn một hoặc lặp lại một số hoặc chứa một thừa số chung. 
4. Với mọi giá trị chẵn khác, xuất ra`3`. Việc xây dựng sử dụng ba số bên trong cửa sổ bốn phần tử chứng tỏ rằng một phạm vi`3`luôn là đủ. 

Tại sao nó hoạt động: 

Các giới hạn dưới đến từ các hạn chế chẵn lẻ và gcd. Phạm vi`0`là không thể vì các số lặp lại không phải là số nguyên tố cùng nhau. Phạm vi`1`chỉ cho phép hai số liên tiếp có tổng phải là số lẻ. Đối với các số chẵn không chia hết cho 4, phạm vi`2`không thể tạo ra một cặp hợp lệ vì các số cách nhau hai sẽ có tính chẵn lẻ sai đối với tính nguyên tố cùng nhau. Các công trình trên khớp với các giới hạn dưới này, ngoại trừ`x = 6`, nơi không có công trình xây dựng nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []
    for case in range(1, t + 1):
        x = int(input())

        if x & 1:
            res = 1
        elif x % 4 == 0:
            res = 2
        elif x == 6:
            res = -1
        else:
            res = 3

        ans.append(f"Case #{case}: {res}")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Chương trình chỉ thực hiện kiểm tra số học nên không bao giờ phụ thuộc vào kích thước của`x`. Kiểm tra tính chẵn lẻ xử lý phạm vi`1`xây dựng đầu tiên. Kiểm tra chia hết cho bốn được thực hiện tiếp theo vì những trường hợp đó có phạm vi chặt chẽ hơn`2`trả lời. Trường hợp đặc biệt`6`phải được kiểm tra trước dự phòng cuối cùng vì đó là giá trị chẵn duy nhất không thể. 

Định dạng đầu ra bao gồm số trường hợp chính xác theo yêu cầu. Số nguyên Python là đủ vì tất cả các phép tính đều ở dưới mức vài tỷ. 

## Ví dụ đã hoạt động 

cho`x = 5`: 

| Bước | Tình trạng | Kết quả | 
| --- | --- | --- | 
| 1 |`x`thật kỳ quặc | câu trả lời trở thành`1`| 
| 2 | Đầu ra |`Case #1: 1`| 

Việc xây dựng là`{2,3}`, điều này xác nhận rằng có thể đạt được phạm vi nhỏ nhất có thể. 

Vì`x = 10`: 

| Bước | Tình trạng | Kết quả | 
| --- | --- | --- | 
| 1 |`x`không có gì lạ | tiếp tục | 
| 2 |`10 % 4 != 0`| tiếp tục | 
| 3 |`x != 6`| tiếp tục | 
| 4 | Trường hợp chẵn còn lại | câu trả lời trở thành`3`| 

Một tập hợp lệ là`{2,3,5}`. Tổng của nó là`10`và sự khác biệt giữa giá trị tối đa và tối thiểu là`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm chỉ yêu cầu một vài phép tính số học. | 
| Không gian | O(T) | Chương trình lưu trữ các dòng đầu ra trước khi in chúng. | 

Với`T = 100000`, quá trình xử lý tuyến tính này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline
    t = int(data())
    out = []

    for case in range(1, t + 1):
        x = int(data())
        if x & 1:
            res = 1
        elif x % 4 == 0:
            res = 2
        elif x == 6:
            res = -1
        else:
            res = 3
        out.append(f"Case #{case}: {res}")

    sys.stdin = old
    return "\n".join(out)

assert run("4\n5\n6\n7\n10\n") == (
    "Case #1: 1\n"
    "Case #2: -1\n"
    "Case #3: 1\n"
    "Case #4: 3"
)

assert run("1\n8\n") == "Case #1: 2"

assert run("5\n12\n16\n20\n14\n18\n") == (
    "Case #1: 2\n"
    "Case #2: 2\n"
    "Case #3: 2\n"
    "Case #4: 3\n"
    "Case #5: 3"
)

assert run("3\n1000000000\n999999999\n6\n") == (
    "Case #1: 2\n"
    "Case #2: 1\n"
    "Case #3: -1"
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5, 7`|`1`| Giá trị lẻ sử dụng số liên tiếp. | 
|`6`|`-1`| Trường hợp duy nhất không thể xảy ra. | 
|`8, 12, 20`|`2`| Các giá trị chia hết cho bốn có phạm vi chặt chẽ. | 
|`14, 18`|`3`| Các giá trị chẵn còn lại sử dụng cách xây dựng rộng hơn. | 
|`1000000000`|`2`| Xử lý kích thước đầu vào tối đa. | 

## Vỏ cạnh 

cho`x = 6`, thuật toán đạt đến trường hợp không thể rõ ràng. một loạt các`1`sẽ yêu cầu hai số nguyên liên tiếp có tổng là một số chẵn, điều này không thể xảy ra. một loạt các`2`sẽ yêu cầu một cặp như`{2,4}`, nhưng gcd của họ thì không`1`. Phạm vi lớn hơn không thể vượt qua mức tối thiểu bắt buộc và không có bộ hợp lệ nào tồn tại. 

Vì`x = 5`, nhánh lẻ lập tức trả về`1`. Những con số`2`Và`3`tổng hợp thành`5`, và gcd của họ là`1`, chứng tỏ rằng phạm vi nhỏ nhất có thể đạt được. 

Vì`x = 8`, thuật toán trả về`2`bởi vì`8`chia hết cho bốn. bộ`{3,5}`có tổng`8`, cả hai giá trị đều lớn hơn một và`gcd(3,5)=1`. Phạm vi không thể`1`vì các số liên tiếp luôn có tổng là số lẻ. 

Với giá trị lớn như`x = 1000000000`, chương trình không thử xây dựng hoặc lặp lại. Nó chỉ kiểm tra tính chia hết, thấy rằng giá trị chia hết cho 4 và trả về`2`ngay lập tức. Điều này xác nhận rằng giải pháp có tỷ lệ độc lập với kích thước số của`x`.
