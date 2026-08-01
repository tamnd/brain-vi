---
title: "CF 102623A - Đại Pháp Sư"
description: "Archmage bắt đầu với một nguồn mana đầy đủ có kích thước n. Trong mỗi giây, anh ta có thể dùng x mana để triệu hồi một Nguyên tố Nước nếu có đủ mana hoặc có thể đợi. Sau quyết định đó, hào quang của anh ta phục hồi y mana, nhưng mana không thể vượt quá n."
date: "2026-08-01T08:55:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "A"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 87
verified: true
draft: false
---

[CF 102623A - Đại pháp sư](https://codeforces.com/problemset/problem/102623/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Archmage bắt đầu với một lượng mana đầy đủ`n`. Trong mỗi giây, anh ta có thể dành`x`mana để triệu hồi một Nguyên tố Nước nếu có đủ mana, hoặc anh ta có thể đợi. Sau quyết định đó, khí chất của anh phục hồi`y`mana, nhưng mana không thể vượt lên trên`n`. 

Nhiệm vụ là tìm số lượng triệu hồi lớn nhất có thể trong lần đầu tiên.`m`giây. Mỗi trường hợp thử nghiệm đưa ra giới hạn năng lượng, số giây khả dụng, chi phí năng lượng của một lần triệu hồi và lượng tái tạo. 

Những hạn chế làm cho khó khăn chính trở nên rõ ràng. Có thể có tới`100000`trường hợp thử nghiệm và mỗi giá trị có thể lớn bằng`10^9`. Một mô phỏng xử lý mỗi giây sẽ yêu cầu tới`10^14`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Giải pháp phải giảm quy trình xuống thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Những trường hợp khó khăn đến từ sự tương tác giữa khả năng hồi phục và giới hạn năng lượng. Mô phỏng trực tiếp thường hoạt động trên các ví dụ nhỏ nhưng không thành công khi`m`là rất lớn hoặc khi Archmage có thể sử dụng vĩnh viễn hoặc phải dựa vào khả năng hồi phục tích lũy. 

Ví dụ, hãy xem xét:```
1
2 5 1 1
```Đầu ra đúng là:```
5
```Một giải pháp bất cẩn có thể cho rằng mana giảm vì mỗi lần triệu hồi đều tốn mana. Tuy nhiên, chi phí và khả năng hồi phục là như nhau nên sau mỗi giây hoàn thành, năng lượng sẽ trở về giá trị như cũ. 

Một trường hợp khác là khi tốc độ hồi phục nhỏ hơn chi phí chính tả:```
1
6 10 4 2
```Đầu ra đúng là:```
6
```Một mô phỏng giả định rằng Archmage nên đợi bất cứ khi nào có thể có thể bỏ sót thực tế là toàn bộ mana ban đầu có thể được chuyển đổi thành nhiều lần sử dụng ngay lập tức trước khi cần phải tái tạo. 

Trường hợp ranh giới cuối cùng là khi số giây là hệ số giới hạn:```
1
100 3 20 5
```Đầu ra đúng là:```
3
```Mặc dù nguồn cung cấp năng lượng đủ lớn để có nhiều phép thuật hơn nhưng chỉ được phép thực hiện ba hành động vì trò chơi kéo dài đúng ba giây. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô phỏng từng giây. Giữ nguyên giá trị năng lượng hiện tại, kiểm tra xem có thể triệu hồi được hay không, trừ đi`x`nếu có thì thêm vào`y`với nắp được áp dụng. Điều này tuân thủ chính xác các quy tắc trò chơi, vì vậy kết quả là chính xác. 

Vấn đề là số giây. Từ`m`có thể đạt được`10^9`và có thể có`10^5`trường hợp thử nghiệm, tổng số giây mô phỏng có thể đạt tới`10^14`. Mô phỏng rất hữu ích để hiểu quy trình, nhưng nó không thể được sử dụng trong giải pháp cuối cùng. 

Quan sát quan trọng là nguồn tài nguyên duy nhất quan trọng là tổng lượng mana có sẵn trong toàn bộ thời gian. Ban đầu Archmage sở hữu`n`mana. Trong thời gian sau`m - 1`phục hồi, nhiều nhất`(m - 1) * y`mana bổ sung có thể được phục hồi. Sự phục hồi của giây cuối cùng xảy ra sau quyết định cuối cùng, vì vậy nó không thể giúp tạo ra một lần triệu hồi khác. 

Khi`x <= y`, mỗi lần triệu hồi sẽ tự trả giá một cách hiệu quả sau khi được khôi phục. Kể từ khi Archmage bắt đầu đầy đủ và`n >= x + y >= x`, anh ấy có thể sử dụng mỗi giây. 

Khi`x > y`, mỗi lần triệu hồi tiêu tốn nhiều năng lượng hơn một lần hồi phục. Tuy nhiên, tổng lượng mana có thể sử dụng cho việc triệu hồi vẫn bị giới hạn bởi lượng mana ban đầu cộng với tất cả các lần phục hồi trước hành động cuối cùng. Chia tổng số này cho`x`đưa ra số lượng triệu tập tối đa có thể. Câu trả lời cũng không thể vượt quá`m`, vì chỉ có`m`giây. 

Do đó, giải pháp tối ưu là tính toán thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu lượng tái sinh`y`ít nhất là chi phí chính tả`x`, đầu ra`m`. Mỗi lần sử dụng sẽ được bù bằng việc phục hồi vào cuối giây, vì vậy Archmage có thể triệu hồi trong mỗi giây có sẵn. 
2. Mặt khác, hãy tính tổng năng lượng có thể sử dụng để triệu hồi như sau:`n + (m - 1) * y`. Năng lượng ban đầu có sẵn trước hành động đầu tiên và chỉ có hành động đầu tiên`m - 1`việc phục hồi có thể góp phần vào việc triệu tập trong tương lai. 
3. Chia tổng số này cho`x`để có được số lần triệu hồi hoàn chỉnh tối đa mà lượng năng lượng sẵn có có thể chi trả. 
4. Giới hạn kết quả bằng`m`, bởi vì ngay cả lượng mana vô hạn cũng không thể tạo ra nhiều hơn một lần triệu hồi mỗi giây. 

Tại sao nó hoạt động: 

Khi nào`x > y`, mọi lần triệu hồi sẽ bị loại bỏ vĩnh viễn`x - y`mana khỏi hệ thống sau khi phục hồi. Thứ tự chờ đợi và thi triển chính xác không làm tăng tổng lượng mana có thể sử dụng. Mana ban đầu và lần đầu tiên`m - 1`phục hồi là ngân sách hoàn chỉnh có sẵn cho tất cả các lệnh triệu tập. Mỗi triệu hồi tiêu thụ chính xác`x`từ ngân sách này nên số lần triệu tập không được vượt quá số nguyên chia của ngân sách cho`x`. Vì Archmage luôn có thể sắp xếp các hành động của mình để sử dụng lượng mana sẵn có này bất cứ khi nào số lượng nằm trong giới hạn, nên giới hạn này có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m, x, y = map(int, input().split())

        if y >= x:
            ans.append(str(m))
        else:
            ans.append(str(min(m, (n + (m - 1) * y) // x)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên tách biệt trường hợp khả năng tái tạo theo kịp tốc độ sử dụng phép thuật. Trong tình huống đó không có lý do gì để bỏ qua một giây. 

Đối với trường hợp khác, biểu thức`n + (m - 1) * y`đại diện cho tổng lượng mana có thể đóng góp cho các lần thi triển. Phép nhân sử dụng`m - 1`còn hơn là`m`bởi vì quá trình phục hồi sau giây cuối cùng xảy ra quá muộn để tạo ra một phép thuật khác. 

Số nguyên Python xử lý giá trị trung gian lớn nhất một cách an toàn. Phép nhân lớn nhất là xung quanh`10^18`, phù hợp thoải mái với loại số nguyên chính xác tùy ý của Python. các`min`hoạt động xử lý ranh giới nơi ngân sách năng lượng được tính toán cho phép nhiều phép thuật hơn số giây có sẵn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
n = 2, m = 2, x = 1, y = 1
```Từ`y >= x`, mỗi giây có thể chứa một lời triệu hồi. 

| Thứ hai | Mana trước khi hành động | Hành động | Mana sau khi phục hồi | Triệu tập | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | Diễn viên | 2 | 1 | 
| 2 | 2 | Diễn viên | 2 | 2 | 

Dấu vết cho thấy trường hợp tái sinh vô hạn. Mana trở về giá trị cũ sau mỗi giây, vì vậy giới hạn thời gian là hạn chế duy nhất. 

Đối với mẫu thứ hai:```
n = 4, m = 4, x = 2, y = 1
```Đây`x > y`, do đó tính toán năng lượng có sẵn sẽ được sử dụng. 

| Biến | Giá trị | 
| --- | --- | 
| Mana ban đầu | 4 | 
| Phục hồi trước giây cuối cùng | 3 | 
| Tổng mana có thể sử dụng | 4 + 3 = 7 | 
| Chi phí chính tả | 2 | 
| Triệu hồi dựa trên mana | 7 // 2 = 3 | 
| Thời hạn | 4 | 
| Trả lời | 3 | 

Dấu vết chứng minh rằng sự phục hồi cuối cùng bị loại trừ. Đếm sai sẽ cho thêm một lần triệu hồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Mỗi test case chỉ sử dụng các phép tính số học | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giải pháp thực hiện công việc liên tục cho mọi trường hợp thử nghiệm, vì vậy`100000`trường hợp được xử lý dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

def solve():
    input = sys.stdin.readline
    t = int(input())
    res = []

    for _ in range(t):
        n, m, x, y = map(int, input().split())
        if y >= x:
            res.append(str(m))
        else:
            res.append(str(min(m, (n + (m - 1) * y) // x)))

    print("\n".join(res))

assert run("""3
2 2 1 1
4 4 2 1
6 10 4 2
""") == "2\n3\n6\n", "samples"

assert run("""1
2 1 1 1
""") == "1\n", "minimum values"

assert run("""1
2000000000 1000000000 1000000000 1
""") == "2\n", "large values"

assert run("""1
100 3 20 5
""") == "3\n", "time limit boundary"

assert run("""1
10 20 3 3
""") == "20\n", "equal cost and regeneration"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1 1`|`2`| Tái sinh bằng chi phí | 
|`2000000000 1000000000 1000000000 1`|`2`| Giá trị số học lớn | 
|`100 3 20 5`|`3`| Số giây giới hạn câu trả lời | 
|`10 20 3 3`|`20`| Mẫu đúc vô hạn | 

## Vỏ cạnh 

Đối với trường hợp tái sinh bằng nhau:```
1
2 5 1 1
```Thuật toán kiểm tra`y >= x`, đó là sự thật. Nó lập tức quay lại`5`. Điều này phù hợp với quy trình thực tế vì mỗi giây tiêu tốn một năng lượng và phục hồi một năng lượng sau đó. 

Đối với trường hợp bùng nổ mana ban đầu:```
1
6 10 4 2
```Thuật toán tính toán:```
(6 + 9 * 2) // 4 = 24 // 4 = 6
```Kết quả là`6`. Năng lượng ban đầu cho phép thực hiện một số lần sử dụng sớm và những lần phục hồi sau đó sẽ bổ sung đủ năng lượng cho các lần triệu hồi bổ sung. Quá trình phục hồi cuối cùng không được bao gồm vì nó xảy ra sau lần sử dụng cuối cùng có thể. 

Đối với trường hợp thời gian lớn:```
1
100 3 20 5
```Việc tính toán cho:```
(100 + 2 * 5) // 20 = 110 // 20 = 5
```Câu trả lời sau đó bị giới hạn bởi`m`, sản xuất`3`. Thuật toán tôn trọng chính xác thực tế là một giây có thể tạo ra nhiều nhất một Nguyên tố Nước.
