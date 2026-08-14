---
title: "CF 102416B - Thị trường hiệu quả"
description: "Chúng ta có m công ty và n ngày trong tương lai. Đối với mọi công ty, chúng tôi biết giá cổ phiếu của nó hàng ngày. Dữ liệu đầu vào lưu trữ một công ty trên mỗi hàng, vì vậy mỗi hàng chứa giá của công ty đó từ ngày 1 đến ngày n. Chúng ta bắt đầu với d pound trước ngày đầu tiên được biết."
date: "2026-08-12T20:41:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102416
codeforces_index: "B"
codeforces_contest_name: "Edinburgh Competition 2019"
rating: 0
weight: 102416
solve_time_s: 148
verified: true
draft: false
---

[CF 102416B - Thị trường hiệu quả](https://codeforces.com/problemset/problem/102416/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`m`công ty và`n`những ngày tương lai. Đối với mọi công ty, chúng tôi biết giá cổ phiếu của nó hàng ngày. Đầu vào lưu trữ một công ty trên mỗi hàng, vì vậy mỗi hàng chứa giá của công ty đó từ ngày 1 đến ngày`n`. 

Chúng tôi bắt đầu với`d`pound trước ngày đầu tiên được biết đến. Trong ngày, giá không thay đổi và chúng tôi có thể thực hiện bất kỳ số lượng giao dịch nào. Vì bản thân mẫu yêu cầu đầu tư một số lượng cổ phiếu không nguyên nên chúng ta có thể coi tiền của mình là chia hết liên tục: nếu chúng ta có`x`bảng Anh và chi phí hàng tồn kho`p`, chúng ta có thể đặt tất cả`x`cân vào`x / p`cổ phiếu. Vào cuối ngày`n`, chúng tôi muốn giá trị tiền mặt lớn nhất có thể. 

Hậu quả quan trọng của việc giao dịch không giới hạn trong ngày là chúng ta không cần phải nhớ một danh mục đầu tư phức tạp. Giả sử chúng ta có`x`pound khi bắt đầu quá trình chuyển đổi từ ngày`i`Hôm nay`i+1`, và công ty`j`chi phí`p`vào ngày`i`Và`q`vào ngày`i+1`. Đầu tư mọi thứ vào công ty đó sẽ thay đổi sự giàu có của chúng ta từ`x`ĐẾN`x * q / p`. Công ty tốt nhất đơn giản là công ty có tỷ lệ lớn nhất`q / p`. Giữ tiền dưới dạng tiền mặt cũng là một lựa chọn, với tỷ lệ`1`. 

Số ngày nhiều nhất là 50 và số lượng công ty nhiều nhất là 1000. Một`O(nm)`thuật toán thực hiện tối đa khoảng 50.000 so sánh tỷ lệ, nằm trong giới hạn một giây. Một chiến lược thử mọi chuỗi lựa chọn có thể có của công ty sẽ phát triển theo cấp số nhân với`n`, vì vậy ngay cả giới hạn tương đối nhỏ là 50 ngày cũng khiến việc liệt kê như vậy là không thể. Bản thân đầu vào chỉ chứa`nm <= 50,000`giá cả, do đó việc lưu trữ ma trận hoàn chỉnh là không tốn kém. 

Có một số trường hợp việc triển khai bất cẩn có thể âm thầm thất bại. Thứ nhất, khi chỉ có một ngày thì không có sự chuyển đổi giá nào cả. Ví dụ,```
1 1 25.00
7.50
```có câu trả lời`25.00`. Không có cơ hội để biến số tiền ban đầu thành số tiền lớn hơn, vì ngày cuối cùng cũng là ngày quan sát đầu tiên. Việc triển khai áp dụng một cách mù quáng một chuyển đổi sẽ thực hiện một thao tác không tồn tại. 

Thứ hai, chúng tôi không bắt buộc phải đầu tư. Coi như```
2 1 100.00
5.00 4.00
```Cổ phiếu duy nhất mất giá nên câu trả lời đúng là`100.00`. Giải pháp luôn mua cổ phiếu sẽ thu được sai`80.00`. Tài sản tiền mặt đóng góp một cách hiệu quả vào hệ số nhân của`1`, vì vậy mỗi số nhân hàng ngày ít nhất phải bằng`1`. 

Thứ ba, công ty tốt nhất có thể thay đổi từ ngày này sang ngày khác. Ví dụ,```
3 2 10.00
1.00 10.00 1.00
1.00 1.00 10.00
```Lần chuyển đổi đầu tiên tốt nhất là thông qua công ty 1, nhân số tiền đó với`10`. Lần chuyển đổi thứ hai tốt nhất là thông qua công ty 2, nhân nó với công ty khác`10`, cho`100.00`. Giải pháp chọn một công ty trong suốt thời kỳ sẽ bỏ lỡ khả năng này. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp có thể thử mọi lựa chọn cổ phiếu có thể có cho mỗi lần chuyển đổi giữa các ngày liên tiếp. Đối với mỗi chuỗi hoàn chỉnh, chúng tôi nhân số tiền hiện tại với tỷ lệ giá tương ứng và giữ kết quả lớn nhất. Chúng ta cũng cần lựa chọn duy trì bằng tiền mặt, vì vậy mỗi lần chuyển đổi có nhiều nhất`m + 1`sự lựa chọn. 

Lực lượng tàn bạo này là chính xác bởi vì mọi chuỗi ứng cử viên đều mô tả một cách khả thi để đầu tư số tài sản hiện tại trong mỗi khoảng thời gian hàng ngày. Vấn đề là số lượng trình tự. có`(m + 1)^(n - 1)`trong số chúng, và việc đánh giá một chuỗi mất`O(n)`công việc. Trong trường hợp xấu nhất đây là`O(n(m + 1)^(n - 1))`. Với`n = 50`Và`m = 1000`, đại khái là vậy`49 * 1001^49`, xung quanh`5 * 10^148`những hoạt động cơ bản, hoàn toàn không thể thực hiện được. 

Quan sát loại bỏ hệ số mũ là các chuyển đổi khác nhau không ảnh hưởng lẫn nhau. Khi chúng tôi đến một ngày cụ thể với`x`pound, mọi lịch sử có thể xảy ra trước đó đều được tóm tắt bằng con số duy nhất đó. Đối với lần chuyển đổi tiếp theo, câu hỏi duy nhất là tài sản nào mang lại hệ số nhân lớn nhất từ ​​giá hôm nay đến giá ngày mai. Vì các giao dịch trong ngày không giới hạn cho phép chúng ta bán cổ phiếu cũ và mua ngay cổ phiếu mới, nên việc thay đổi công ty không tốn kém gì ngoài giá niêm yết của họ. 

Dành cho công ty`j`, số nhân từ ngày`i`Hôm nay`i+1`là`price[i+1][j] / price[i][j]`. 

Chúng tôi tận dụng tối đa tỷ lệ này trên tất cả các công ty trở lên`1`để lấy tiền mặt. Sau đó chúng tôi nhân số tài sản hiện tại với mức tối đa đó. Lặp lại điều này một cách độc lập cho tất cả`n - 1`chuyển tiếp mang lại hiệu quả tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n(m+1)^(n-1))`|`O(n)`| Quá chậm | 
| Tối ưu |`O(nm)`|`O(nm)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số ngày`n`, số lượng công ty`m`, và số tiền ban đầu`d`. Lưu trữ`m`hàng giá vì mỗi lần chuyển đổi cần giá của mọi công ty trong hai ngày liên tiếp. 
2. Đặt số tiền hiện tại thành`d`. Trước khi xử lý bất kỳ chuyển đổi nào, đây chính xác là số tiền có sẵn vào đầu ngày 1. 
3. Đối với mỗi cặp ngày liên tiếp`i`Và`i + 1`, kiểm tra mọi công ty. Nếu giá của nó là`p = price[i][j]`Và`q = price[i+1][j]`, tính hệ số nhân`q / p`. 
4. Bắt đầu hệ số nhân tốt nhất cho quá trình chuyển đổi tại`1.0`. Điều này thể hiện việc để lại tất cả tiền dưới dạng tiền mặt. Hãy thay thế nó bất cứ khi nào một cổ phiếu có hệ số nhân lớn hơn. 
5. Nhân số tiền hiện tại với số nhân tốt nhất. Điều này mang lại số tiền tối đa có thể có vào ngày hôm sau, bởi vì tất cả số tiền có thể được chuyển đổi thành tài sản hoạt động tốt nhất cho quá trình chuyển đổi cụ thể này. 
6. Sau khi xử lý chuyển đổi cuối cùng, hãy in số tiền thu được bằng hai chữ số sau dấu thập phân. Khi`n = 1`, vòng lặp không có lần lặp nào, do đó số lượng ban đầu được in không thay đổi. 

### Tại sao nó hoạt động 

Giữ nguyên bất biến đó`money`là số tiền mặt tối đa có thể có vào ngày hiện tại sau khi sử dụng tất cả các cơ hội sinh lời cho đến ngày đó. Hãy xem xét quá trình chuyển đổi tiếp theo. Bất kỳ khoản tiền nào hiện có đều có thể vẫn là tiền mặt, duy trì hệ số nhân của`1`hoặc được chuyển đổi hoàn toàn thành cổ phiếu của một công ty. Nếu công ty`j`được chọn, mỗi pound trở thành`price[i+1][j] / price[i][j]`pound vào ngày hôm sau. Bởi vì các giao dịch tùy ý trong cùng ngày được cho phép, bất kỳ cổ phiếu nào trước đó đều có thể được bán và toàn bộ danh mục đầu tư có thể được chuyển sang bất kỳ công ty nào có tỷ lệ lớn nhất. Vì vậy, sự giàu có tốt nhất có thể có vào ngày hôm sau chính xác là`money * max(1, max_j(price[i+1][j] / price[i][j]))`. Bất biến được giữ nguyên ở mỗi lần chuyển đổi, do đó, sau ngày cuối cùng, số tiền được tính toán là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, money = input().split()
    n = int(n)
    m = int(m)
    money = float(money)

    prices = [list(map(float, input().split())) for _ in range(m)]

    for day in range(n - 1):
        best_ratio = 1.0

        for company in range(m):
            ratio = prices[company][day + 1] / prices[company][day]
            if ratio > best_ratio:
                best_ratio = ratio

        money *= best_ratio

    print(f"{money:.2f}")

if __name__ == "__main__":
    solve()
```Ba giá trị đầu tiên được phân tích cú pháp từ dòng đầu tiên, với`n`Và`m`được chuyển đổi thành số nguyên và chữ hoa ban đầu được chuyển đổi thành số dấu phẩy động. Giá được lưu trữ dưới dạng`prices[company][day]`, khớp trực tiếp với đầu vào theo hàng. 

Vòng lặp bên ngoài chạy từ ngày`0`suốt ngày`n - 2`. Điều này tương ứng chính xác với`n - 1`chuyển tiếp giữa các ngày liên tiếp. sử dụng`range(n - 1)`cũng chính là điều khiến`n = 1`trường hợp này hoạt động tự động vì không có quá trình chuyển đổi nào. 

Đối với mỗi lần chuyển đổi,`best_ratio`bắt đầu lúc`1.0`, không phải tại`0`. Điều này thể hiện lựa chọn không làm gì và giữ tiền dưới dạng tiền mặt. Vì mọi giá cổ phiếu đều dương nên việc chia cho giá luôn an toàn. 

Tỷ lệ được tính toán trước khi cập nhật`money`, vì vậy mọi công ty đều được so sánh bằng cách sử dụng cùng một số vốn ban đầu cho quá trình chuyển đổi đó. Chỉ cập nhật số tiền sau khi tất cả các công ty đã được kiểm tra tương đương với việc chọn công ty tốt nhất và đầu tư toàn bộ số tiền vào đó. 

Ở đây số học dấu phẩy động của Python là đủ vì dung sai đầu ra được yêu cầu là`0.01`, trong khi câu trả lời được đảm bảo là nhiều nhất`10^9`. Python cũng không gặp vấn đề tràn số nguyên có chiều rộng cố định, mặc dù giải pháp này dù sao cũng không cần số nguyên lớn. Định dạng bằng`:.2f`tạo ra hai chữ số thập phân cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4 2 10.00
1.02 1.00 1.00 1.00
4.37 4.81 5.32 6.06
```Hai hàng là lịch sử giá của hai công ty. 

| Chuyển tiếp | Tỷ lệ Công ty 1 | Tỷ lệ Công ty 2 | Hệ số nhân tốt nhất | Tiền | 
| --- | --- | --- | --- | --- | 
| Ngày 1 → 2 |`1.00 / 1.02 ≈ 0.9804`|`4.81 / 4.37 ≈ 1.1007`|`1.1007`|`11.007`| 
| Ngày 2 → 3 |`1.00 / 1.00 = 1`|`5.32 / 4.81 ≈ 1.1060`|`1.1060`|`12.174`| 
| Ngày 3 → 4 |`1.00 / 1.00 = 1`|`6.06 / 5.32 ≈ 1.1391`|`1.1391`|`13.873`| 

Giá trị cuối cùng làm tròn thành`13.87`. Dấu vết cũng cho thấy tại sao số lượng cổ phiếu thực tế không cần phải lưu trữ. Sau mỗi lần chuyển đổi, toàn bộ tài sản hiện tại có thể lại được coi là tiền mặt, sau đó được tái đầu tư một cách tối ưu cho lần chuyển đổi tiếp theo. 

### Xây dựng ví dụ 2 

Hãy xem xét```
3 2 10.00
1.00 10.00 1.00
1.00 1.00 10.00
```| Chuyển tiếp | Tỷ lệ Công ty 1 | Tỷ lệ Công ty 2 | Hệ số nhân tốt nhất | Tiền | 
| --- | --- | --- | --- | --- | 
| Ngày 1 → 2 |`10 / 1 = 10`|`1 / 1 = 1`|`10`|`100`| 
| Ngày 2 → 3 |`1 / 10 = 0.1`|`10 / 1 = 10`|`10`|`1000`| 

Câu trả lời là`1000.00`. Chiến lược tối ưu sẽ thay đổi các công ty giữa hai thời kỳ chuyển đổi. Điều này xác nhận rằng thuật toán không chỉ tập trung vào một cổ phiếu trong toàn bộ thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nm)`| có`n - 1`chuyển tiếp và mỗi chuyển đổi sẽ kiểm tra tất cả`m`các công ty. | 
| Không gian |`O(nm)`| Ma trận giá hoàn chỉnh chứa`m * n`các giá trị dấu phẩy động. | 

Với tối đa 50 ngày và 1000 công ty, thuật toán thực hiện ít hơn 50.000 phép tính tỷ lệ. Ma trận giá cũng chứa tối đa 50.000 giá trị, do đó cả thời gian chạy và mức sử dụng bộ nhớ đều dễ dàng nằm trong giới hạn một giây và 256 MB. 

## Trường hợp thử nghiệm```python
# The following test harness reuses the same solve() logic.
import sys
import io

def solve():
    n, m, money = input().split()
    n = int(n)
    m = int(m)
    money = float(money)

    prices = [list(map(float, input().split())) for _ in range(m)]

    for day in range(n - 1):
        best_ratio = 1.0

        for company in range(m):
            ratio = prices[company][day + 1] / prices[company][day]
            if ratio > best_ratio:
                best_ratio = ratio

        money *= best_ratio

    print(f"{money:.2f}")

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """4 2 10.00
1.02 1.00 1.00 1.00
4.37 4.81 5.32 6.06
"""
) == "13.87", "sample 1"

# Minimum-size input, no transitions and zero initial money
assert run(
    """1 1 0.00
10.00
"""
) == "0.00", "minimum size and zero capital"

# All prices equal, so the amount never changes
assert run(
    """3 2 123.45
5.55 5.55 5.55
2.00 2.00 2.00
"""
) == "123.45", "all prices equal"

# Falling stock, so keeping cash is better than investing
assert run(
    """2 1 100.00
5.00 4.00
"""
) == "100.00", "cash must be an available choice"

# The best company changes between transitions
assert run(
    """3 2 10.00
1.00 10.00 1.00
1.00 1.00 10.00
"""
) == "1000.00", "company switching"

# Maximum dimensions with equal prices
max_case = ["50 1000 1000000.00"]
max_case.extend(["1.00 " * 49 + "1.00"] * 1000)
assert run("\n".join(max_case) + "\n") == "1000000.00", "maximum dimensions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0.00`với một giá |`0.00`| Kích thước tối thiểu và không có bất kỳ chuyển đổi nào | 
| Ba ngày với tất cả giá không đổi |`123.45`| Một số nhân chính xác`1`khiến của cải không thay đổi | 
| Một cổ phiếu giảm từ`5.00`ĐẾN`4.00`|`100.00`| Thuật toán phải được phép giữ tiền mặt | 
| Hai cổ phiếu có lợi nhuận tốt nhất luân phiên |`1000.00`| Công ty tối ưu có thể thay đổi mỗi ngày | 
|`n = 50`,`m = 1000`, tất cả giá`1.00`|`1000000.00`| Kích thước và hiệu suất đầu vào tối đa | 

## Vỏ cạnh 

Khi chỉ có một ngày, không có khoảng thời gian đầu tư. Vì```
1 1 25.00
7.50
```vòng lặp kết thúc`range(n - 1)`trở thành`range(0)`, Vì thế`money`còn lại`25.00`và đầu ra là`25.00`. Điều này tránh được sai lầm phổ biến khi so sánh ngày 1 với ngày 2 không tồn tại. 

Khi mọi cổ phiếu sẵn có đều mất giá, tiền mặt phải đánh bại tất cả. Vì```
2 1 100.00
5.00 4.00
```cổ phiếu duy nhất có tỷ lệ`4 / 5 = 0.8`, trong khi tiền mặt có tỷ lệ`1`. Thuật toán khởi tạo`best_ratio`ĐẾN`1.0`, do đó quá trình chuyển đổi khiến của cải ở mức`100.00`. Việc thực hiện tham lam luôn mua cổ phiếu tốt nhất mà không tính đến tiền mặt sẽ làm giảm vốn một cách không chính xác. 

Khi công ty tối ưu thay đổi, thuật toán sẽ cố tình đánh giá lại tất cả các công ty sau mỗi lần chuyển đổi. TRONG```
3 2 10.00
1.00 10.00 1.00
1.00 1.00 10.00
```lần chuyển đổi đầu tiên chọn công ty 1 và nhân số tiền với`10`. Kết quả`100.00`sau đó được coi là vốn khả dụng cho lần chuyển đổi thứ hai, trong đó công ty 2 có tỷ lệ`10`. Giá trị cuối cùng là`1000.00`. Lựa chọn trước không hạn chế lựa chọn tiếp theo vì việc mua bán trong ngày không bị hạn chế. 

Nếu tất cả các mức giá đều bằng nhau thì mọi tỷ lệ tồn kho đều chính xác`1`, nên vốn không bao giờ thay đổi. Vì```
3 2 123.45
5.55 5.55 5.55
2.00 2.00 2.00
```mọi chuyển đổi đều có hệ số nhân tốt nhất`1`, sản xuất`123.45`khắp. Điều này cũng kiểm tra xem thuật toán không tạo ra lợi nhuận nhân tạo từ các giao dịch lặp lại trong cùng ngày. 

Cuối cùng, vốn ban đầu bằng 0 là vô hại ngay cả khi có cơ hội sinh lời. Vì```
2 1 0.00
1.00 10.00
```cổ phiếu cung cấp một số nhân`10`, Nhưng`0 * 10`vẫn còn`0`. Câu trả lời là`0.00`. Thuật toán tính toán hệ số nhân tối ưu độc lập với lượng vốn, sau đó áp dụng nó cho của cải hiện tại, vì vậy trường hợp biên này không cần xử lý đặc biệt.
