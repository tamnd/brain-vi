---
title: "CF 102267C - Búp bê Matryoshka"
description: "Chúng ta có một con búp bê Matryoshka lớn nhất có kích thước nguyên S. Nếu một con búp bê được đặt bên trong một con búp bê khác, con búp bê bên trong phải có kích thước tối đa bằng 1/X kích thước của con búp bê bên ngoài."
date: "2026-08-19T03:14:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "C"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 350
verified: false
draft: false
---

[CF 102267C - Búp bê Matryoshka](https://codeforces.com/problemset/problem/102267/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có búp bê Matryoshka lớn nhất có kích thước nguyên`S`. Nếu một con búp bê được đặt bên trong một con búp bê khác thì con búp bê bên trong tối đa phải bằng`1/X`kích thước bên ngoài của con búp bê. Chúng tôi muốn chuỗi lồng dài nhất có thể, vì vậy câu hỏi đặt ra là có bao nhiêu búp bê có thể xuất hiện từ kích thước ban đầu`S`xuống kích thước số nguyên dương nhỏ nhất. 

Giả sử con búp bê hiện tại có kích thước`A`. Kích thước lớn nhất có thể có của con búp bê tiếp theo là`floor(A / X)`. Chọn bất cứ thứ gì nhỏ hơn không bao giờ có thể giúp ích được, bởi vì con búp bê tiếp theo lớn hơn ít nhất sẽ để lại nhiều chỗ cho mọi con búp bê sau này. Việc lặp lại lựa chọn này sẽ mang lại chuỗi dài nhất có thể. 

Các ràng buộc đủ nhỏ để số học logarit trở thành mục tiêu tự nhiên. Từ`X >= 2`, mỗi bước lồng nhau sẽ giảm kích thước ít nhất hai lần. Bắt đầu từ nhiều nhất`10^9`, có thể có ít hơn 31 lần giảm trước khi kích thước trở thành 0. Quét tuyến tính trên tất cả các kích thước lên tới`10^9`sẽ là quá đắt đối với giới hạn một giây, trong khi`O(log S)`giải pháp chỉ thực hiện vài chục lần lặp. 

Có một số trường hợp ranh giới trong đó việc triển khai có thể gây ra từng lỗi một. Vì`S = 1, X = 2`, câu trả lời là`1`, bởi vì bản thân con búp bê ban đầu cũng có giá trị, nhưng không có kích thước nguyên dương nào có thể nhét vừa vào bên trong nó. Một vòng lặp bất cẩn chỉ tính các phép chia thành công có thể xuất ra`0`. 

Vì`S = 10, X = 2`, chuỗi tối ưu là`10 -> 5 -> 2 -> 1`, cho`4`. Việc triển khai sử dụng phép chia thông thường mà không lấy tầng một cách rõ ràng là an toàn trong Python vì phép chia số nguyên đã có tầng, nhưng việc sử dụng phép chia dấu phẩy động và chuyển đổi sau này có thể gây ra những rủi ro về độ chính xác không cần thiết. 

Vì`S = 9, X = 2`, chuỗi là`9 -> 4 -> 2 -> 1`, một lần nữa cho`4`. Kích thước thứ hai là`4`, không`4.5`, vì kích thước búp bê phải là số nguyên. Bất kỳ cách tiếp cận nào coi kích thước là số thực đều có thể tạo ra độ dài chuỗi không chính xác. 

Khi`S = X = 10^9`, chuỗi là`10^9 -> 1`, vậy câu trả lời là`2`. Một sai lầm phổ biến là yêu cầu con búp bê tiếp theo phải có kích thước nhỏ hơn rất nhiều so với kích thước của nó.`S / X`, nhưng điều kiện cho phép bằng nhau nên kích thước`1`là hợp lệ. 

## Phương pháp tiếp cận 

Một phương pháp brute-force trực tiếp có thể tìm kiếm con búp bê tiếp theo qua mọi kích thước nguyên có thể có. Đối với kích thước hiện tại`A`, nó có thể kiểm tra tất cả các ứng cử viên từ`1`bởi vì`floor(A / X)`và xác định cái nào lớn nhất và hợp lệ. Điều này đúng vì mọi lựa chọn có thể có cho con búp bê tiếp theo đều được xem xét và việc lấy lựa chọn hợp lệ lớn nhất sẽ tạo ra sự tiếp tục tốt nhất. 

Vấn đề là số lượng công việc không cần thiết. Với`S = 10^9`Và`X = 2`, tìm kiếm như vậy sẽ kiểm tra đại khái`floor(10^9 / 2) + floor(10^9 / 4) + floor(10^9 / 8) + ...`ứng viên trên toàn bộ chuỗi. Số tiền này là`10^9 - popcount(10^9) = 999,999,987`, vì vậy trường hợp xấu nhất về cơ bản là một tỷ lần lặp. Điều đó không thể vừa vặn thoải mái trong giới hạn một giây. 

Phương pháp brute-force hoạt động vì nó đang tìm kiếm một không gian trong đó kích thước tiếp theo tốt nhất là hiển nhiên sau khi tất cả các ứng cử viên đã được xem xét. Quan sát quan trọng là chúng ta thực sự không cần phải tìm kiếm không gian đó. Đối với kích thước hiện tại`A`, mọi kích thước hợp lệ tiếp theo tối đa là`floor(A / X)`và việc chọn chính xác mức tối đa đó sẽ mang lại điểm xuất phát lớn nhất có thể cho những con búp bê còn lại. Do đó, vấn đề giảm xuống còn việc áp dụng phép chia số nguyên nhiều lần cho`X`. 

Bắt đầu với`S`, chúng ta đếm con búp bê hiện tại, thay thế kích thước của nó bằng`S // X`và tiếp tục trong khi kích thước mới là dương. Từ`X`ít nhất là`2`, số lần lặp là logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(S)`trong trường hợp xấu nhất |`O(1)`| Quá chậm | 
| Tối ưu |`O(log_X S)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`S`Và`X`. Kích thước búp bê hiện tại ban đầu là`S`, và câu trả lời bắt đầu từ`1`bởi vì bản thân con búp bê lớn nhất đã là một phần của chuỗi tổ. 
2. Trong khi kích thước hiện tại ít nhất là`X`, thay thế nó bằng`current // X`và tăng câu trả lời. 

Chỉ chia khi`current >= X`tương đương với việc kiểm tra xem một con búp bê số nguyên dương khác có thể nhét vừa vào bên trong hay không. Nếu như`current < X`, sau đó`floor(current / X) = 0`, và kích thước`0`không phải là búp bê nên dây xích phải dừng lại. 
3. In câu trả lời tích lũy. 

Tính đúng đắn rút ra từ bất biến rằng trước mỗi lần lặp,`current`là kích thước lớn nhất có thể có của con búp bê ở độ sâu đó trong số tất cả các chuỗi tối ưu. Từ một con búp bê có kích thước`current`, không có con búp bê bên trong hợp lệ nào có thể vượt quá`floor(current / X)`và kích thước chính xác đó có giá trị bất cứ khi nào nó dương. Việc chọn kích thước tiếp theo tối đa có thể không thể làm giảm số lượng búp bê tối đa có thể đạt được sau này, bởi vì mọi ràng buộc sau này đều đơn điệu đối với kích thước hiện tại. Do đó, mọi bộ phận đều tạo ra con búp bê tiếp theo tốt nhất có thể và vòng lặp dừng chính xác khi không tồn tại con búp bê tích cực bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    S, X = map(int, input().split())

    ans = 1
    while S >= X:
        S //= X
        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Biến`ans`bắt đầu lúc`1`bởi vì con búp bê ban đầu được tính ngay cả khi không thể đặt con búp bê nào nhỏ hơn vào bên trong nó. Tay cầm này`S = 1`một cách chính xác. 

điều kiện`S >= X`là kiểm tra ranh giới rõ ràng. Nếu nó giữ được,`S // X`ít nhất là`1`, vậy là tồn tại một con búp bê có kích thước dương khác. Nếu nó không giữ được thì thương số sẽ bằng 0 và việc lồng ghép phải dừng lại. 

Bản cập nhật sử dụng phép chia số nguyên trực tiếp. Vì kích thước là số nguyên,`S // X`chính xác là kích thước số nguyên hợp lệ lớn nhất cho con búp bê tiếp theo. Không cần số học dấu phẩy động. 

Số nguyên Python cũng loại bỏ mọi lo ngại về tràn, mặc dù các giá trị đầu vào đã đủ nhỏ để số nguyên có dấu 32 bit sẽ đủ cho các giá trị được lưu trữ. 

Đầu vào chỉ chứa một ca kiểm thử, do đó không có vòng lặp ca kiểm thử bên ngoài. yêu cầu`sys.stdin.readline`thiết lập vẫn được sử dụng cho đầu vào lập trình cạnh tranh nhanh và thông thường. 

## Ví dụ đã hoạt động 

### Mẫu 1:`S = 10, X = 2`Con búp bê hiện tại có thể được thay thế nhiều lần bằng kích thước nguyên lớn nhất vừa với bên trong nó. 

| Bước | Kích thước hiện tại | Kích thước mới | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 10 | 5 | 2 | 
| 2 | 5 | 2 | 3 | 
| 3 | 2 | 1 | 4 | 
| Dừng lại | 1 | Không thể | 4 | 

Chuỗi kết quả là`10 -> 5 -> 2 -> 1`. Khi kích thước hiện tại đạt đến`1`, nó nhỏ hơn`X = 2`, do đó một con búp bê số nguyên dương khác không thể vừa. Câu trả lời là`4`. 

### Ví dụ tùy chỉnh:`S = 9, X = 2`| Bước | Kích thước hiện tại | Kích thước mới | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 9 | 4 | 2 | 
| 2 | 4 | 2 | 3 | 
| 3 | 2 | 1 | 4 | 
| Dừng lại | 1 | Không thể | 4 | 

Ví dụ này thực hiện ranh giới số nguyên. Con búp bê bên trong đầu tiên có kích thước`9 // 2 = 4`, không`5`, vì yêu cầu là`2 * inner <= outer`. Chuỗi là`9 -> 4 -> 2 -> 1`, cho`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(log_X S)`| Mỗi lần lặp lại chia kích thước hiện tại cho`X`, Và`X >= 2`. | 
| Không gian |`O(1)`| Chỉ có kích thước, số chia và câu trả lời hiện tại được lưu trữ. | 

Với`S <= 10^9`Và`X >= 2`, có tối đa 30 phép chia trước khi đạt giá trị`1`. Do đó, thuật toán chỉ thực hiện vài chục lần lặp ngay cả trong trường hợp xấu nhất, dễ dàng khớp trong giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    S, X = map(int, data)

    ans = 1
    while S >= X:
        S //= X
        ans += 1

    return str(ans)

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Provided sample
assert run("10 2\n") == "4", "sample 1"

# Minimum possible S
assert run("1 2\n") == "1", "minimum size"

# Exact powers of X
assert run("8 2\n") == "4", "exact divisibility"

# Non-divisible boundary, catches incorrect rounding
assert run("9 2\n") == "4", "floor division boundary"

# Equal maximum values
assert run("1000000000 1000000000\n") == "2", "maximum equal values"

# Maximum S with minimum X
assert run("1000000000 2\n") == "30", "maximum chain length"

# X larger than S
assert run("5 10\n") == "1", "no inner doll"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`1`| Kích thước tối thiểu và thực tế là con búp bê ban đầu có giá trị | 
|`8 2`|`4`| Quyền hạn chính xác và sự bình đẳng ở mọi ranh giới phân chia | 
|`9 2`|`4`| Chia tầng số nguyên thay vì chia số thực | 
|`1000000000 1000000000`|`2`| Giá trị tối đa và ranh giới đẳng thức cho phép | 
|`1000000000 2`|`30`| Số lần lặp tối đa theo các ràng buộc | 
|`5 10`|`1`| Trường hợp không thể đặt búp bê bên trong | 

## Vỏ cạnh 

cho`S = 1, X = 2`, thuật toán bắt đầu bằng`ans = 1`Và`S = 1`. điều kiện`1 >= 2`là sai, do đó vòng lặp không thực thi và kết quả là`1`. Điều này ngăn ngừa sai lầm phổ biến khi chỉ đếm những con búp bê bên trong thay vì đếm con búp bê lớn nhất ban đầu. 

Vì`S = 9, X = 2`, bản cập nhật đầu tiên là`9 // 2 = 4`. Các cập nhật sau đây được`4 // 2 = 2`Và`2 // 2 = 1`. Thuật toán dừng ở`1`và trả về`4`. Điều này chứng tỏ tại sao thương số phải được coi là số nguyên, vì chuỗi giá trị thực giả định liên quan đến`4.5`không được phép. 

Vì`S = X = 10^9`, điều kiện ban đầu`10^9 >= 10^9`là đúng, do đó thuật toán chấp nhận một con búp bê bên trong có kích thước`10^9 // 10^9 = 1`và tăng câu trả lời cho`2`. Điều kiện tiếp theo không thành công vì`1 < 10^9`. Do đó, đầu ra là`2`, xác nhận rằng điều kiện biên cho phép một con búp bê bên trong chính xác bằng`S / X`. 

Vì`S = 5, X = 10`, điều kiện ban đầu`5 >= 10`là sai. Thuật toán ngay lập tức trở lại`1`, điều này đúng vì bất kỳ kích thước số nguyên dương bên trong nào cũng sẽ phải lớn nhất là`floor(5 / 10) = 0`. 

Đối với chuỗi lớn nhất có thể,`S = 10^9, X = 2`, thuật toán thực hiện 29 phép chia thành công trước khi đạt tới`1`, vậy con búp bê ban đầu cộng với 29 con búp bê bên trong đó sẽ cho`30`. Ngay cả trường hợp cực đoan này cũng chỉ yêu cầu nhiều phép toán logarit, đó là lý do chính khiến giải pháp đáp ứng thoải mái các ràng buộc.
