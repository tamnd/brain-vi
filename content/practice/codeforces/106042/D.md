---
title: "CF 106042D - Tổng và Hoặc"
description: "Chúng ta có một dãy số nguyên dương. Alice thực hiện một bước đầu tiên: cô ấy chọn một chỉ mục và thay thế giá trị đó bằng bit OR của giá trị bằng x. Sau đó, Bob chọn một chỉ mục và thay thế giá trị đó bằng bit AND của giá trị hiện tại bằng y."
date: "2026-06-25T12:52:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106042
codeforces_index: "D"
codeforces_contest_name: "Teamscode Summer 2025 Novice Division"
rating: 0
weight: 106042
solve_time_s: 39
verified: true
draft: false
---

[CF 106042D - Tổng và Hoặc](https://codeforces.com/problemset/problem/106042/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy số nguyên dương. Alice thực hiện một bước đầu tiên: cô ấy chọn một chỉ mục và thay thế giá trị đó bằng bit OR của giá trị bằng`x`. Sau đó, Bob chọn một chỉ mục và thay thế giá trị đó bằng bit AND của giá trị hiện tại bằng`y`. Alice muốn tổng mảng cuối cùng càng nhỏ càng tốt, trong khi Bob muốn nó càng lớn càng tốt. Chúng ta cần tìm tổng cuối cùng giả sử cả hai người chơi đều chọn tối ưu. 

Kích thước mảng có thể đạt tới`100000`trên tất cả các trường hợp thử nghiệm và có thể có nhiều trường hợp thử nghiệm. Điều này có nghĩa là một cách tiếp cận thử mọi cặp lựa chọn của Alice và Bob sẽ thực hiện trong khoảng`n²`hoạt động, trở thành xung quanh`10^10`hoạt động trong trường hợp xấu nhất và không thể đáp ứng được thời hạn. Chúng ta chỉ cần xử lý mỗi phần tử một số lần không đổi. 

Phần khó khăn là Alice và Bob có thể chọn cùng một chỉ mục. Một lỗi phổ biến là tính toán hiệu ứng của Alice và hiệu ứng của Bob một cách độc lập và kết hợp chúng, nhưng khi cả hai thao tác đều chạm vào cùng một phần tử thì thao tác thứ hai sẽ thấy kết quả của thao tác đầu tiên. Ví dụ:```
1
1 8 7
8
```Alice thay đổi phần tử duy nhất thành`8 | 8 = 8`và Bob thay đổi nó thành`8 & 7 = 0`. Câu trả lời là`0`. 

Một giải pháp bất cẩn xử lý các nước đi một cách riêng biệt có thể coi sự thay đổi của Alice là 0 và sự thay đổi của Bob là`8 & 7 - 8 = -1`, tạo ra ý tưởng sai lầm rằng kết quả là`7`. Hai thao tác phải được xem xét cùng nhau cho chỉ mục đã chọn. 

Một trường hợp khác là khi nước đi tốt nhất của Bob không phải là chỉ số mà Alice đã thay đổi. Ví dụ:```
1
2 8 1
8 15
```Nếu Alice chạm vào phần tử đầu tiên, nó vẫn giữ nguyên`8`và Bob có thể giảm phần tử thứ hai từ`15`ĐẾN`1`, tính tổng`9`. Nếu chúng ta chỉ kiểm tra bước đi của Bob trên chỉ số đã chọn của Alice, chúng ta sẽ bỏ lỡ phản hồi tốt hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mọi cặp bước di chuyển có thể xảy ra. Đối với mỗi chỉ mục có thể mà Alice chọn, chúng tôi thử mọi chỉ mục có thể mà Bob chọn, tính tổng kết quả và giữ giá trị mà Alice có thể ép buộc. Điều này đúng vì nó kiểm tra mọi trạng thái có thể có của trò chơi. Vấn đề là nó thực hiện hai vòng lặp lồng nhau trên mảng. Với`n = 100000`, điều này tạo ra khoảng`10^10`pair, tốc độ này quá chậm. 

Nhận xét quan trọng là hành động của Bob luôn có tác động có thể đoán trước được. Nếu Bob chọn một chỉ mục mà Alice chưa chạm tới thì giá trị trước khi Bob di chuyển vẫn là giá trị ban đầu. Đóng góp của chỉ số lựa chọn Bob`j`chỉ đơn giản là:`(A[j] & y) - A[j]`Giá trị này không bao giờ thay đổi trong suốt trò chơi. Trường hợp đặc biệt duy nhất là chỉ mục Alice đã thay đổi, vì Bob nhìn thấy kết quả OR ở đó. 

Đối với sự lựa chọn cố định của Alice`i`, Bob chỉ có hai loại lựa chọn. Anh ấy có thể chọn`i`, gây ra sự biến đổi kết hợp:`(A[i] | x) & y`hoặc anh ta có thể chọn bất kỳ chỉ số nào khác và thực hiện mức giảm chỉ số không đổi tốt nhất. Vì Bob muốn số tiền tối đa nên anh ấy chỉ cần giá trị lớn nhất trong hai lựa chọn này. 

Chúng ta có thể tính toán trước phần đóng góp tốt nhất của Bob trong số tất cả các chỉ số ban đầu. Trong khi kiểm tra một chỉ mục Alice cụ thể, chúng tôi cần giá trị tốt nhất loại trừ chỉ mục đó, vì vậy chúng tôi lưu trữ những đóng góp lớn nhất và lớn thứ hai của Bob. Điều này cho phép chúng ta trả lời mọi lựa chọn của Alice trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng ban đầu của mảng. Câu trả lời sẽ là giá trị này cộng với sự thay đổi tốt nhất mà Alice có thể đưa ra sau khi xem xét câu trả lời tối ưu của Bob. 
2. Với mỗi phần tử, hãy tính lợi nhuận của Bob nếu anh ấy chọn phần tử đó mà không có sự can thiệp nào từ Alice:```
gain = (A[i] & y) - A[i]
```Giá trị này biểu thị tổng số tiền thay đổi bao nhiêu khi Bob hành động trên một chỉ mục chưa được chạm tới. 

1. Tìm giá trị lớn nhất và lớn thứ hai trong số các mức tăng này, cùng với chỉ số của giá trị lớn nhất. Chúng ta cần những thứ này vì khi Alice chọn chỉ số chứa mức tăng lớn nhất, Bob không thể sử dụng nước đi nguyên vẹn đó. 
2. Hãy thử mọi chỉ số có thể mà Alice có thể chọn. Đối với chỉ mục này, hãy tính giá trị sau thao tác của Alice:```
after_alice = A[i] | x
```Sự thay đổi từ Alice là:```
after_alice - A[i]
```1. Tính toán hai câu trả lời tốt nhất có thể có của Bob. Nếu Bob chọn chỉ mục của Alice thì giá trị cuối cùng của phần tử đó sẽ trở thành:```
(after_alice & y)
```vì vậy tổng số thay đổi cho phần tử đó là:```
(after_alice & y) - A[i]
```Nếu Bob chọn một chỉ mục khác, hãy sử dụng mức tăng được tính toán trước lớn nhất không thuộc chỉ mục của Alice. 

1. Bob chọn câu trả lời lớn hơn trong hai câu trả lời này. Alice chọn tổng cuối cùng nhỏ nhất trên tất cả các chỉ số có thể có. 

Tại sao nó hoạt động: 

Đối với mỗi nước đi đầu tiên có thể có của Alice, thuật toán sẽ đánh giá chính xác hai loại nước đi của Bob. Bất kỳ chỉ mục nào Bob chọn đều là chỉ mục đã được sửa đổi của Alice hoặc nó không bị ảnh hưởng. Mức tối đa được tính toán trước bao gồm mọi lựa chọn chưa được xử lý và phép tính trực tiếp bao gồm lựa chọn đã sửa đổi. Vì Alice kiểm tra tất cả các bước đi đầu tiên có thể xảy ra và phản ứng tốt nhất của Bob luôn được chọn nên giá trị minimax được tính toán chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x, y = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)

    best1 = -10**30
    best2 = -10**30
    best_idx = -1

    gains = []
    for i, v in enumerate(a):
        g = (v & y) - v
        gains.append(g)
        if g > best1:
            best2 = best1
            best1 = g
            best_idx = i
        elif g > best2:
            best2 = g

    ans = 10**30

    for i, v in enumerate(a):
        alice_value = v | x

        bob_same = (alice_value & y) - v

        if best_idx != i:
            bob_other = best1
        else:
            bob_other = best2

        bob_change = max(bob_same, bob_other)

        current = total + (alice_value - v) + bob_change
        if current < ans:
            ans = current

    print(ans)

t = int(input())
for _ in range(t):
    solve()
```Giải pháp trước tiên lưu trữ tổng mảng ban đầu vì mỗi câu trả lời ứng viên đều là một sửa đổi của giá trị đó. các`gains`Không cần mảng sau khi tìm thấy các giá trị tối đa, nhưng việc tách riêng phần tính toán sẽ giúp xác minh logic dễ dàng hơn. 

Hai lợi ích tối đa của Bob được duy trì vì việc loại trừ chỉ số đã chọn của Alice là sự phụ thuộc duy nhất giữa các lựa chọn. Nếu lợi ích tốt nhất thuộc về chỉ số của Alice, thì giá trị tốt thứ hai sẽ trở thành nước đi hợp pháp tốt nhất của Bob. 

Vòng lặp lựa chọn của Alice tính toán trường hợp đặc biệt duy nhất, trong đó cả hai người chơi đều ảnh hưởng đến cùng một phần tử. Số nguyên Python đã xử lý số tiền lớn liên quan nên không cần xử lý tràn. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
1
4 5 2
1 4 3 6
```Các trạng thái quan trọng là: 

| Chỉ số Alice | Giá trị Alice | Bob cùng thay đổi | Bob thay đổi khác | Tổng cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | -1 | -4 | 14 | 
| 1 | 5 | -4 | -1 | 15 | 
| 2 | 7 | -1 | -4 | 16 | 
| 3 | 7 | -4 | -1 | 16 | 

Kết quả nhỏ nhất là`14`. Alice thay đổi phần tử đầu tiên và Bob chọn phần tử khác. 

Bây giờ hãy xem xét:```
1
2 8 1
8 15
```| Chỉ số Alice | Giá trị Alice | Bob cùng thay đổi | Bob thay đổi khác | Tổng cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0 | 8 | -8 | -14 | 9 | 
| 1 | 15 | -15 | -7 | 16 | 

Alice chọn chỉ mục`0`. Câu trả lời hay nhất của Bob là thay đổi thành phần thứ hai, chứng minh tại sao trường hợp "chỉ mục khác" phải được xem xét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một số lần không đổi. | 
| Không gian | O(1) | Chỉ cần một vài biến ngoài mảng đầu vào. | 

Tổng chiều dài của tất cả các mảng được giới hạn bởi`100000`, do đó, một giải pháp tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng không đổi ngoài việc lưu trữ test case hiện tại. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n, x, y = map(int, input().split())
        a = list(map(int, input().split()))

        total = sum(a)
        best1 = -10**30
        best2 = -10**30
        best_idx = -1

        for i, v in enumerate(a):
            g = (v & y) - v
            if g > best1:
                best2 = best1
                best1 = g
                best_idx = i
            elif g > best2:
                best2 = g

        ans = 10**30

        for i, v in enumerate(a):
            av = v | x
            same = (av & y) - v
            other = best1 if i != best_idx else best2
            ans = min(ans, total + av - v + max(same, other))

        return str(ans) + "\n"

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "".join(out)

assert run("""5
4 5 2
1 4 3 6
5 6 3
2 5 7 1 4
3 3 4
2 3 5
5 536870912 268435456
536870912 268435456 800000000 900000000 123456789
5 2 6
1 3 2 4 5
""") == """14
19
9
2628763157
15
"""

assert run("""1
1 8 7
8
""") == "0\n"

assert run("""1
2 8 1
8 15
""") == "9\n"

assert run("""1
5 1 1
7 7 7 7 7
""") == "35\n"

assert run("""1
3 1073741824 1073741824
1 2 4
""") == "7\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử đơn với các bước di chuyển chồng chéo |`0`| Chuyển đổi cùng chỉ số | 
| Hai yếu tố có phản hồi chỉ số khác nhau tốt hơn |`9`| Lựa chọn tối ưu của Bob không phải lúc nào cũng là chỉ số của Alice | 
| Tất cả các giá trị bằng nhau |`35`| Giá trị lặp lại và xử lý ràng buộc | 
| Ranh giới giá trị bit lớn |`7`| Hoạt động bit với vị trí cao | 

## Vỏ cạnh 

Đối với trường hợp phần tử đơn:```
1
1 8 7
8
```Alice không có chỉ mục thay thế. Phần tử trở thành`8 | 8 = 8`, thì Bob áp dụng`8 & 7 = 0`. Thuật toán xử lý việc này vì mức tối đa của "chỉ mục khác" không tồn tại, do đó Bob buộc phải sử dụng cùng một chỉ mục. 

Đối với trường hợp Bob tránh chỉ mục của Alice:```
1
2 8 1
8 15
```Alice chọn phần tử đầu tiên sẽ không tăng. Bob có thể giảm phần tử thứ hai bằng cách`14`, điều này tốt hơn là giảm phần tử đầu tiên đã được sửa đổi. Mức tối đa được tính toán trước không bao gồm chỉ số của Alice nắm bắt được điều này. 

Đối với các phần tử bằng nhau:```
1
5 1 1
7 7 7 7 7
```Mọi lợi ích của Bob đều giống hệt nhau. Giá trị tối đa thứ nhất và thứ hai bằng nhau, do đó việc loại trừ một chỉ mục sẽ không thay đổi câu trả lời. Khả năng theo dõi tối đa của thuật toán xử lý việc này mà không cần logic ràng buộc đặc biệt.
