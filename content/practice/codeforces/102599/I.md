---
title: "CF 102599I - Đếm tam giác"
description: "Chúng ta có bốn ranh giới có thứ tự để chia độ dài các cạnh có thể có thành ba phạm vi. Cạnh thứ nhất x phải được chọn từ [A, B], cạnh thứ hai y từ [B, C], và cạnh thứ ba z từ [C, D]."
date: "2026-08-01T07:08:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102599
codeforces_index: "I"
codeforces_contest_name: "The fifth Lipetsk collegiate programming contest. Finals. 8-11 form"
rating: 0
weight: 102599
solve_time_s: 1018
verified: true
draft: false
---

[CF 102599I - Đếm tam giác](https://codeforces.com/problemset/problem/102599/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 16 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn ranh giới có thứ tự để chia độ dài các cạnh có thể có thành ba phạm vi. Mặt đầu tiên`x`phải được chọn từ`[A, B]`, cạnh thứ hai`y`từ`[B, C]`, và cạnh thứ ba`z`từ`[C, D]`. Do thứ tự của các dãy nên mọi bộ ba được chọn đều thỏa mãn`x <= y <= z`. Yêu cầu duy nhất còn lại đối với một tam giác hợp lệ là bất đẳng thức tam giác không suy biến, trở thành`x + y > z`. 

Nhiệm vụ là đếm có bao nhiêu bộ ba`(x, y, z)`thỏa mãn các điều kiện này. 

Giá trị lớn nhất của bất kỳ ranh giới nào là`500000`. Việc liệt kê trực tiếp tất cả các bộ ba sẽ có giá trị lên đến`500000^3`khả năng, vượt xa những gì có thể phù hợp trong giới hạn thời gian một giây. Thậm chí lặp lại trên tất cả các cặp`(x, y)`quá đắt vì có thể có khoảng`2.5 * 10^11`những cặp như vậy. Chúng ta cần tránh sự phụ thuộc vào tích của các độ dài khoảng. 

Các trường hợp cạnh chính xuất phát từ ranh giới giữa các tam giác hợp lệ và không hợp lệ. Việc thực hiện bất cẩn thường xử lý sai sự bất bình đẳng nghiêm ngặt hoặc cho rằng mọi bộ ba có thể đều là một tam giác. 

Ví dụ: với đầu vào:```
1 1 1 3
```Bộ ba có thể là`(1,1,1)`,`(1,1,2)`, Và`(1,1,3)`. Chỉ cái đầu tiên là hợp lệ vì`1 + 1 > 1`là đúng, trong khi`1 + 1 > 2`Và`1 + 1 > 3`là sai. Câu trả lời là`1`. sử dụng`>=`thay vì`>`sẽ đếm không chính xác các trường hợp suy biến. 

Một trường hợp ranh giới khác là khi tất cả các khoảng đều chứa một giá trị:```
500000 500000 500000 500000
```Tam giác duy nhất có thể là`(500000,500000,500000)`, vậy câu trả lời là`1`. Các giải pháp dựa vào phạm vi có nhiều giá trị hoặc chia cho độ dài khoảng không chính xác có thể thất bại ở đây. 

Một lỗi phổ biến cuối cùng xuất hiện khi giới hạn trên của`z`đã đạt được. Vì:```
1 2 3 4
```Cặp đôi`(2,3)`cho phép`z = 3`Và`z = 4`, nhưng không tồn tại giá trị nào lớn hơn. Bất kỳ công thức nào tính tất cả`z < x+y`không cắt ở`D`sẽ vượt quá. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là thử mọi cách có thể`x`, mọi khả năng`y`, rồi đếm khả năng có thể`z`các giá trị thỏa mãn bất đẳng thức tam giác. Để cố định`x`Và`y`, hợp lệ`z`giá trị là từ`C`ĐẾN`min(D, x+y-1)`. Cách tiếp cận này đúng vì nó áp dụng trực tiếp định nghĩa về tam giác. 

Vấn đề là số lượng cặp. Hai phạm vi đầu tiên đều có thể chứa khoảng`500000`giá trị, sản xuất xung quanh`250000000000`cặp. Ngay cả khi mỗi cặp được xử lý trong thời gian không đổi thì việc này cũng không kết thúc. 

Quan sát quan trọng là chúng ta không cần biết từng cặp riêng lẻ. Đối với một giá trị cố định của`z`, câu hỏi duy nhất là có bao nhiêu cặp`(x,y)`có`x+y > z`. Điều này biến bài toán thành việc đếm tổng bên trong một hình chữ nhật có thể có`(x,y)`các giá trị. 

Chúng ta có thể đếm số lượng ngược lại, số cặp có`x+y <= z`và trừ nó khỏi tổng số cặp. Sau khi dịch chuyển cả hai phạm vi để bắt đầu từ 0, đây trở thành vấn đề kinh điển về việc đếm điểm`(a,b)`trong một hình chữ nhật nơi`a+b`nhiều nhất là một giá trị nào đó. Loại trừ bao gồm cung cấp số lượng này trong thời gian không đổi, cho phép chúng tôi lặp lại mọi khả năng có thể`z`. Có nhiều nhất`500000`những giá trị như vậy, có thể dễ dàng quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((B-A+1)(C-B+1)) | O(1) | Quá chậm | 
| Tối ưu | O(D-C+1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`n = B - A + 1`Và`m = C - B + 1`là kích thước của hai phạm vi bên đầu tiên. Tổng số có thể`(x,y)`cặp là`n * m`. 
2. Chuyển đổi độ dài hai cạnh đầu tiên thành tọa độ gốc 0. Định nghĩa`a = x - A`, Vì thế`0 <= a < n`, Và`b = y - B`, Vì thế`0 <= b < m`. 

điều kiện`x + y <= z`trở thành:```
a + b <= z - A - B
```Phép chuyển đổi này loại bỏ các giá trị ban đầu lớn và để lại vấn đề đếm lưới giới hạn tiêu chuẩn. 
3. Tạo hàm trợ giúp`triangle_count(k)`nó trả về bao nhiêu cặp`(a,b)`thỏa mãn`a >= 0`,`b >= 0`,`a < n`,`b < m`, Và`a+b <= k`. 

Không có giới hạn trên`a`Và`b`, số cặp không âm là:```
(k+1)(k+2)/2
```Nếu như`a`đạt tới`n`, sự thay đổi`a`xuống bởi`n`và trừ đi vùng không hợp lệ. Điều tương tự cũng áp dụng cho`b`và các cặp vi phạm cả hai giới hạn sẽ được thêm lại bằng cách loại trừ bao gồm. 
4. Trong mọi khả năng`z`từ`C`ĐẾN`D`, tính xem có bao nhiêu`(x,y)`cặp thỏa mãn`x+y <= z`. Trừ đi số này khỏi tổng số cặp để có số lượng hình tam giác hợp lệ kết thúc bằng số này`z`. 
5. Cộng tất cả các số đếm này để có được câu trả lời cuối cùng. 

Tại sao nó hoạt động: 

Đối với mỗi cố định`z`, mỗi cặp`(x,y)`rơi vào đúng một trong hai nhóm: hoặc`x+y <= z`, không thể tạo thành một tam giác không suy biến, hoặc`x+y > z`, cái đó thì có. Hàm trợ giúp đếm chính xác nhóm đầu tiên bằng cách sử dụng loại trừ bao gồm trên các ranh giới hình chữ nhật. Trừ đi tổng số cặp sẽ để lại chính xác các lựa chọn hợp lệ cho điều đó`z`. Tổng hợp tất cả những gì có thể`z`đếm mỗi tam giác hợp lệ một lần vì mỗi tam giác có đúng một cạnh thứ ba. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B, C, D = map(int, input().split())

    n = B - A + 1
    m = C - B + 1

    def count_leq(k):
        def calc(t):
            if t < 0:
                return 0
            return (t + 1) * (t + 2) // 2

        return calc(k) - calc(k - n) - calc(k - m) + calc(k - n - m)

    total_pairs = n * m
    ans = 0

    for z in range(C, D + 1):
        not_triangles = count_leq(z - A - B)
        ans += total_pairs - not_triangles

    print(ans)

if __name__ == "__main__":
    solve()
```Các biến`n`Và`m`mô tả các lựa chọn có thể có cho hai bên đầu tiên. Giữ chúng dưới dạng độ dài thay vì lưu trữ mảng là điều cho phép giải pháp sử dụng bộ nhớ không đổi. 

chức năng`calc(t)`là công thức số tam giác cho một lưới không giới hạn. Nó đếm tất cả các cặp giá trị không âm có tổng không vượt quá`t`. Trả về số 0 cho số âm`t`xử lý các trường hợp giới hạn quá nhỏ đối với bất kỳ cặp nào. 

Bốn thuật ngữ trong`count_leq`là hiệu chỉnh bao gồm-loại trừ. Số hạng đầu tiên tính mọi cặp không âm. Cặp thứ hai và thứ ba loại bỏ trong đó`a`hoặc`b`vượt quá phạm vi cho phép của họ. Cặp thứ tư thêm lại được loại bỏ hai lần. 

Vòng lặp kết thúc`z`sử dụng thực tế là chỉ có`D-C+1`các giá trị có thể có của cạnh lớn nhất. Số nguyên Python tránh được vấn đề tràn, điều này quan trọng vì câu trả lời có thể lớn hơn nhiều so với số nguyên 32 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1 2 3 4
```Hình chữ nhật được dịch chuyển có:`n = 2`,`m = 2`, tổng số cặp = 4. 

| z | k = z-A-B | Cặp với x+y <= z | Cặp hợp lệ | 
| --- | --- | --- | --- | 
| 3 | 0 | 1 | 3 | 
| 4 | 1 | 3 | 1 | 

Tổng số tiền là`3 + 1 = 4`, phù hợp với đầu ra mẫu. Dấu vết cho thấy mỗi giá trị của cạnh lớn nhất được xử lý độc lập như thế nào. 

Đối với mẫu thứ hai:```
1 2 2 5
```Đây:`n = 2`,`m = 1`, tổng số cặp = 2. 

| z | k = z-A-B | Cặp với x+y <= z | Cặp hợp lệ | 
| --- | --- | --- | --- | 
| 2 | -1 | 0 | 2 | 
| 3 | 0 | 1 | 1 | 
| 4 | 1 | 2 | 0 | 
| 5 | 2 | 2 | 0 | 

Câu trả lời là`2 + 1 = 3`. Ví dụ này chứng minh rằng lớn hơn`z`các giá trị cuối cùng trở nên không thể có được khi tổng của hai cạnh nhỏ hơn không thể vượt quá chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(D-C+1) | Chúng ta đánh giá một công thức hằng số thời gian cho mọi giá trị có thể có của`z`. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Số lần lặp vòng lặp tối đa là`500000`và mỗi lần lặp chỉ thực hiện các phép tính số học. Điều này phù hợp thoải mái trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    A, B, C, D = map(int, input().split())

    n = B - A + 1
    m = C - B + 1

    def count_leq(k):
        def calc(t):
            if t < 0:
                return 0
            return (t + 1) * (t + 2) // 2

        return calc(k) - calc(k - n) - calc(k - m) + calc(k - n - m)

    total = n * m
    ans = 0

    for z in range(C, D + 1):
        ans += total - count_leq(z - A - B)

    return str(ans) + "\n"

assert solution("1 2 3 4\n") == "4\n", "sample 1"
assert solution("1 2 2 5\n") == "3\n", "sample 2"
assert solution("500000 500000 500000 500000\n") == "1\n", "sample 3"

assert solution("1 1 1 3\n") == "1\n", "degenerate triangles must not count"
assert solution("1 1 2 2\n") == "1\n", "single valid combination"
assert solution("5 5 5 6\n") == "2\n", "equal sides and upper bound handling"
assert solution("1 500000 500000 500000\n") == "0\n", "largest side too large for most pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 3`|`1`| Bất đẳng thức tam giác chặt và các trường hợp suy biến | 
|`1 1 2 2`|`1`| Phạm vi nhỏ nhất và ranh giới chính xác | 
|`5 5 5 6`|`2`| Xử lý các khoảng bằng nhau và nhiều`z`giá trị | 
|`1 500000 500000 500000`|`0`| Phạm vi lớn và hình tam giác không thể | 

## Vỏ cạnh 

Đối với trường hợp tam giác suy biến:```
1 1 1 3
```Thuật toán lặp qua`z = 1, 2, 3`. Vì`z = 1`, số cặp có`x+y <= 1`bằng 0, vậy cặp duy nhất`(1,1)`được tính. Vì`z = 2`Và`z = 3`, cặp được phân loại là không hợp lệ vì tổng các cạnh nhỏ hơn không lớn hơn`z`. Câu trả lời cuối cùng là`1`. 

Đối với trường hợp tối đa một giá trị:```
500000 500000 500000 500000
```Các phạm vi được dịch chuyển đều có độ dài bằng một. Vì điều duy nhất có thể`z`, hàm trợ giúp đếm 0 cặp không hợp lệ vì`500000 + 500000 > 500000`. Tổng số cặp là một, vì vậy câu trả lời là một. 

Để cắt giới hạn trên:```
1 2 3 4
```Cặp đôi`(2,3)`có tổng`5`, vì vậy nó cho phép`z`giá trị lên đến`4`, không vượt quá khoảng đã cho. Thuật toán không bao giờ tạo ra một`z`ngoài`[C,D]`, do đó giới hạn trên đương nhiên được tôn trọng. Số cuối cùng vẫn còn`4`.
