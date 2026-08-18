---
title: "CF 102317A - Hùng vĩ 10"
description: "Vấn đề yêu cầu chúng tôi xử lý một số cầu thủ bóng rổ. Mỗi người chơi có chính xác ba số liệu thống kê, chẳng hạn như điểm, số lần bật lại hoặc hỗ trợ. Một số liệu thống kê được tính là \"double\" khi nó ít nhất là 10."
date: "2026-08-17T10:13:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "A"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 59
verified: true
draft: false
---

[CF 102317A - Hùng vĩ 10](https://codeforces.com/problemset/problem/102317/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng tôi xử lý một số cầu thủ bóng rổ. Mỗi người chơi có chính xác ba số liệu thống kê, chẳng hạn như điểm, số lần bật lại hoặc hỗ trợ. Một số liệu thống kê được tính là "kép" khi nó ít nhất là 10. Đối với mỗi người chơi, chúng ta phải in ba số liệu thống kê ban đầu, sau đó mô tả xem người chơi có 0, một, hai hay ba số liệu thống kê như vậy hay không. 

Bốn mô tả có thể là`zilch`cho số liệu thống kê bằng 0 đạt tới 10,`double`cho chính xác một,`double-double`cho đúng hai, và`triple-double`cho cả ba. Đầu vào chứa số lượng người chơi dương, theo sau là ba số liệu thống kê số nguyên cho mỗi người chơi. Mỗi thống kê nằm trong khoảng từ 0 đến 100. 

Các ràng buộc làm cho việc này trở thành một lần quét tuyến tính trực tiếp. Chỉ có ba giá trị cho mỗi người chơi, vì vậy việc xử lý một người chơi sẽ mất một lượng công việc không đổi. Tuyên bố không áp đặt giới hạn trên đối với số lượng người chơi, nhưng ngay cả một số lượng rất lớn người chơi cũng chỉ yêu cầu công việc không đổi trên mỗi người chơi, đưa ra thuật toán O(n) trong đó n là số lượng người chơi. Không có lý do gì để xem xét việc sắp xếp, lập trình động, thuật toán đồ thị hoặc bất kỳ phép toán bậc hai nào. 

Trường hợp ranh giới chính là giá trị 10. Vì 10 được tính là gấp đôi nên việc so sánh phải là`>= 10`, không`> 10`. Ví dụ,```
1
10 0 0
```sản xuất```
10 0 0
double
```Việc thực hiện bất cẩn bằng cách sử dụng`> 10`sẽ in sai`zilch`. 

Ranh giới đối diện là 9, không được tính. Ví dụ,```
1
9 9 9
```sản xuất```
9 9 9
zilch
```Việc chỉ kiểm tra xem giá trị có dương hay không sẽ phân loại người chơi này không chính xác. 

Tất cả ba số liệu thống kê có thể đủ điều kiện cùng một lúc. Ví dụ,```
1
10 20 30
```sản xuất```
10 20 30
triple-double
```Chuỗi độc lập`if`các câu in ra câu trả lời ngay lập tức cho thống kê đủ điều kiện đầu tiên có thể dừng lại ở`double`. Cách tiếp cận đúng sẽ tính tất cả số liệu thống kê đủ điều kiện trước khi chọn mô tả. 

Trường hợp số 0 cũng có vấn đề. Vì```
1
0 1 9
```đầu ra là```
0 1 9
zilch
```Người chơi vẫn có ba số liệu thống kê hợp lệ, nhưng không có số liệu nào đạt đến ngưỡng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản đã là tối ưu về mặt tiệm cận. Đối với mỗi người chơi, hãy kiểm tra từng số liệu thống kê một và đếm xem có bao nhiêu người thỏa mãn`value >= 10`. Bởi vì có chính xác ba số liệu thống kê nên điều này yêu cầu tối đa ba phép so sánh cho mỗi người chơi, theo sau là một lựa chọn trong số bốn chuỗi có thể. Đối với n người chơi, trường hợp xấu nhất chính xác là so sánh ngưỡng 3n, cộng với công việc O(n) để tạo đầu ra. 

Không có kỹ thuật tiệm cận thực sự nhanh hơn để khám phá ở đây. Câu trả lời phụ thuộc độc lập vào từng thống kê trong ba số liệu thống kê, vì vậy thuật toán ít nhất phải đọc các giá trị đầu vào. Quan sát quan trọng là toàn bộ trạng thái của người chơi có thể được biểu thị bằng một số nguyên nhỏ, số lượng thống kê đạt tới 10. Sau khi biết số lượng đó, thông báo bắt buộc sẽ xuất hiện trực tiếp. 

Do đó, các phương pháp tiếp cận bạo lực và tối ưu đều giống nhau trong quá trình quét tuyến tính. Việc tối ưu hóa hữu ích mang tính khái niệm chứ không phải tiệm cận: thay vì viết bốn trường hợp riêng biệt dựa trên các giá trị thực tế, hãy quy mỗi người chơi về số lượng thống kê đủ điều kiện. Điều đó làm cho điều kiện biên trở nên rõ ràng và ngăn chặn các điều kiện chồng chéo tạo ra thông báo sai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng người chơi. Điều này cho chúng ta biết chính xác có bao nhiêu nhóm gồm ba số liệu thống kê phải được xử lý. 
2. Đối với mỗi người chơi, đọc ba số liệu thống kê và khởi tạo bộ đếm về 0. Bộ đếm sẽ biểu thị số lần nhân đôi mà người chơi này kiếm được. 
3. Kiểm tra từng thống kê trong số ba số liệu thống kê theo ngưỡng 10 bằng cách sử dụng`>=`. Tăng bộ đếm bất cứ khi nào số liệu thống kê ít nhất là 10. Cần phải so sánh toàn diện vì giá trị chính xác là 10 đủ điều kiện. 
4. Chọn tin nhắn tương ứng với bộ đếm. Tổng số 0 bản đồ tới`zilch`, 1 đến`double`, 2 đến`double-double`, và 3 đến`triple-double`. 
5. In ba số liệu thống kê ban đầu theo sau là thông báo đã chọn. In một dòng trống sau kết quả của người chơi, khớp với định dạng đầu ra được yêu cầu. 

Bất biến rất đơn giản: sau khi xử lý bất kỳ tiền tố nào của ba số liệu thống kê, bộ đếm bằng chính xác số lượng thống kê được xử lý ít nhất là 10. Mỗi thống kê đóng góp một cho bộ đếm khi và chỉ khi nó đủ điều kiện, vì vậy sau khi cả ba số liệu thống kê đã được xử lý, bộ đếm chính xác là số nhân đôi. Ánh xạ cuối cùng từ 0 đến 3 là đầy đủ nên thông báo được chọn phải chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    output = []

    names = ["zilch", "double", "double-double", "triple-double"]

    for _ in range(n):
        stats = list(map(int, input().split()))

        doubles = sum(value >= 10 for value in stats)
        output.append(f"{stats[0]} {stats[1]} {stats[2]}\n{names[doubles]}")

    sys.stdout.write("\n\n".join(output) + "\n\n")

if __name__ == "__main__":
    solve()
```các`names`mảng trực tiếp mã hóa bốn câu trả lời có thể. Chỉ số của nó là số lượng thống kê thỏa mãn ngưỡng, do đó`names[doubles]`tránh được một chuỗi câu lệnh có điều kiện dài hơn. 

Python xử lý`True`là 1 và`False`bằng 0, điều này làm cho`sum(value >= 10 for value in stats)`một cách nhỏ gọn để đếm số liệu thống kê đủ điều kiện. Việc so sánh vẫn mang tính bao hàm nên 10 được xử lý chính xác. 

Số liệu thống kê gốc được lưu trữ để có thể sao chép chính xác theo thứ tự yêu cầu. Vì chỉ có ba giá trị cho mỗi người chơi nên giá trị này sử dụng bộ nhớ không đổi cho mỗi người chơi. các`output`list lưu trữ kết quả đầu ra được tạo cho tất cả người chơi trước khi ghi nó một lần, điều này cũng giúp I/O hoạt động hiệu quả khi có nhiều người chơi. 

Không có vấn đề tràn số nguyên vì mọi thống kê đầu vào tối đa là 100 và bộ đếm chỉ có thể là 0, 1, 2 hoặc 3.`"\n\n"`cung cấp cho khối đầu ra của mọi người chơi dòng trống cần thiết, bao gồm cả người chơi cuối cùng. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào chứa bốn người chơi. Quá trình xử lý có thể được theo dõi như sau. 

| Người chơi | Thống kê | Giá trị đủ điều kiện |`doubles`| Tin nhắn | 
| --- | --- | --- | --- | --- | 
| 1 | 5 0 8 | không | 0 |`zilch`| 
| 2 | 30 10 50 | 30, 10, 50 | 3 |`triple-double`| 
| 3 | 20 5 20 | 20, 20 | 2 |`double-double`| 
| 4 | 5 100 6 | 100 | 1 |`double`| 

Kết quả đầu ra là```
5 0 8
zilch

30 10 50
triple-double

20 5 20
double-double

5 100 6
double
```Ví dụ này thực hiện mọi giá trị có thể có của bộ đếm, từ 0 đến 3. Nó cũng xác nhận rằng chính xác 10 đủ điều kiện vì chỉ số ở giữa của người chơi thứ hai là 10. 

Ví dụ hữu ích thứ hai tập trung vào chính ngưỡng đó và các giá trị ngay bên dưới và bên trên nó.```
3
9 10 11
0 0 0
10 10 10
```| Người chơi | Thống kê | Sau lần kiểm tra đầu tiên | Sau lần kiểm tra thứ hai | Sau lần kiểm tra thứ ba | Tin nhắn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 9 10 11 | 0 | 1 | 2 |`double-double`| 
| 2 | 0 0 0 | 0 | 0 | 0 |`zilch`| 
| 3 | 10 10 10 | 1 | 2 | 3 |`triple-double`| 

Đầu ra tương ứng là```
9 10 11
double-double

0 0 0
zilch

10 10 10
triple-double
```Người chơi đầu tiên thể hiện cả hai mặt của ranh giới trong một hàng. Giá trị 9 bị từ chối, trong khi 10 và 11 được chấp nhận. Người chơi thứ ba xác nhận rằng ba giá trị đủ điều kiện tạo ra`triple-double`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi người trong số n người chơi được kiểm tra chính xác ba số liệu thống kê, do đó công việc của mỗi người chơi là không đổi. | 
| Không gian | O(n) | Việc triển khai lưu trữ kết quả đầu ra được tạo cho tất cả người chơi trước khi ghi nó. Bản thân thuật toán yêu cầu O(1) không gian phụ cho mỗi người chơi. | 

Bài toán có số lượng tính toán không đổi cho mỗi người chơi, do đó thời gian chạy tăng tuyến tính theo số lượng người chơi. Ngay cả khi đầu vào chứa số lượng người chơi rất lớn, thuật toán chỉ thực hiện ba lần kiểm tra ngưỡng cho mỗi người chơi và không thực hiện bất kỳ lần quét lồng nhau nào. Bản thân các giá trị thống kê rất nhỏ nên bộ nhớ được thuật toán sử dụng không phụ thuộc vào độ lớn của chúng. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    output = []

    names = ["zilch", "double", "double-double", "triple-double"]

    for _ in range(n):
        stats = list(map(int, input().split()))
        doubles = sum(value >= 10 for value in stats)
        output.append(f"{stats[0]} {stats[1]} {stats[2]}\n{names[doubles]}")

    sys.stdout.write("\n\n".join(output) + "\n\n")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """4
5 0 8
30 10 50
20 5 20
5 100 6
"""
) == (
    """5 0 8
zilch

30 10 50
triple-double

20 5 20
double-double

5 100 6
double

"""
), "sample 1"

# Minimum-size input
assert run(
    """1
0 0 0
"""
) == (
    """0 0 0
zilch

"""
), "minimum-size case"

# All values equal to the threshold
assert run(
    """1
10 10 10
"""
) == (
    """10 10 10
triple-double

"""
), "threshold is inclusive"

# Values immediately around the threshold
assert run(
    """3
9 9 9
9 10 11
11 9 10
"""
) == (
    """9 9 9
zilch

9 10 11
double-double

11 9 10
double-double

"""
), "boundary values"

# Large stress case
large_n = 100000
large_input = str(large_n) + "\n" + ("100 0 0\n" * large_n)
large_expected = ("100 0 0\ndouble\n\n" * large_n)
assert run(large_input) == large_expected, "large input"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0 0`|`zilch`| Đầu vào có kích thước tối thiểu và số liệu thống kê đủ điều kiện bằng 0 | 
|`1 / 10 10 10`|`triple-double`| Ngưỡng bao gồm và cả ba đủ điều kiện | 
|`3 / 9 9 9`,`9 10 11`,`11 9 10`|`zilch`,`double-double`,`double-double`| Các giá trị ngay bên dưới, tại và trên ranh giới | 
| 100000 người chơi với`100 0 0`|`double`cho mọi người chơi | Đầu vào lớn và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Trường hợp ranh giới nguy hiểm nhất chính xác là 10. Đối với```
1
10 0 0
```thuật toán kiểm tra 10 đầu tiên, đánh giá`10 >= 10`là đúng và thay đổi`doubles`từ 0 đến 1. Các giá trị còn lại không đủ điều kiện nên số đếm cuối cùng là 1 và đầu ra là`double`. Một triển khai sử dụng`>`sẽ đưa ra câu trả lời sai. 

Ranh giới dưới hoạt động đối xứng. Vì```
1
9 9 9
```mỗi so sánh`9 >= 10`là sai, để bộ đếm ở mức 0. Đầu ra là`zilch`. Điều này phát hiện các hoạt động triển khai vô tình kiểm tra xem một thống kê có dương tính hay không thay vì liệu nó có đạt đến ngưỡng yêu cầu hay không. 

Trường hợp đủ điều kiện là```
1
10 10 10
```Bộ đếm trở thành 1 sau giá trị đầu tiên, 2 sau giá trị thứ hai và 3 sau giá trị thứ ba. Tra cứu cuối cùng là`names[3]`, mang lại`triple-double`. Giải pháp in ngay khi tìm thấy số liệu thống kê đủ điều kiện đầu tiên sẽ không thành công ở đây. 

Trường hợp không đủ điều kiện là```
1
0 0 0
```Bộ đếm vẫn bằng 0 trong suốt quá trình quét, vì vậy`names[0]`cho`zilch`. Điều này cũng xác nhận rằng số 0 là giá trị thống kê bình thường và không yêu cầu xử lý đầu vào đặc biệt. 

Cuối cùng, bản thân định dạng đầu ra có thể khiến giải pháp đúng không thành công. Đối với nhiều người chơi, số liệu thống kê ban đầu của mỗi người chơi phải được theo sau bởi thông báo của nó và một dòng trống ngăn cách các khối đầu ra. Giải pháp này xây dựng một khối hoàn chỉnh cho mỗi người chơi và nối các khối đó với hai ký tự dòng mới, ngăn không cho số liệu thống kê và thông báo từ những người chơi khác nhau vô tình bị trộn lẫn.
