---
title: "CF 82A - Cola đôi"
description: "Năm người đứng xếp hàng theo một thứ tự cố định: Sheldon, Leonard, Penny, Rajesh, Howard. Bất cứ khi nào người đứng đầu mua cola, người đó sẽ ngay lập tức tạo một bản sao của chính họ và cả hai bản sao sẽ chuyển về phía sau hàng đợi. Hàng đợi tiếp tục tăng mãi mãi."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 82
codeforces_index: "A"
codeforces_contest_name: "Yandex.Algorithm 2011: Qualification 2"
rating: 1100
weight: 82
solve_time_s: 90
verified: true
draft: false
---

[CF 82A - Double Cola](https://codeforces.com/problemset/problem/82/A) 

**Đánh giá:** 1100 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Năm người đứng xếp hàng theo một thứ tự cố định: Sheldon, Leonard, Penny, Rajesh, Howard. 

Bất cứ khi nào người đứng đầu mua cola, người đó sẽ ngay lập tức tạo một bản sao của chính họ và cả hai bản sao sẽ chuyển về phía sau hàng đợi. Hàng đợi tiếp tục tăng mãi mãi. 

Chúng tôi được cấp số`n`, đại diện cho`n`-thứ cola được bán. Nhiệm vụ là xác định xem người nào uống cola đó. 

Ràng buộc`n ≤ 10^9`làm thay đổi hoàn toàn bản chất của vấn đề. Mô phỏng trực tiếp sẽ liên tục đẩy và bật các tên từ hàng đợi, nhưng sau đủ bước, hàng đợi sẽ trở nên khổng lồ. Ngay cả một thuật toán tuyến tính với một thao tác trên mỗi cola cũng sẽ yêu cầu tới một tỷ thao tác, vượt xa giới hạn thời gian 1 giây. 

Quan sát quan trọng là quá trình phát triển theo từng lớp. Ban đầu mỗi người xuất hiện một lần. Sau cùng năm người uống một lần, mỗi người xuất hiện hai lần. Sau đó, mỗi cái xuất hiện bốn lần, rồi tám lần, v.v. Hàng đợi mở rộng về mặt hình học, có nghĩa là chúng ta có thể bỏ qua toàn bộ khối thay vì mô phỏng từng đồ uống riêng lẻ. 

Có một số chỗ dễ mắc lỗi. 

Lỗi phổ biến đầu tiên là trộn lẫn chỉ mục dựa trên 0 và dựa trên 1. Bài toán số cola bắt đầu từ 1. Ví dụ: 

đầu vào:```
1
```Đầu ra đúng:```
Sheldon
```Nếu chúng ta vô tình đối xử`n`dựa trên 0, chúng tôi sẽ dịch chuyển mọi câu trả lời theo một vị trí. 

Một trường hợp tinh tế khác xảy ra chính xác ở ranh giới nhóm. Hãy xem xét 5 đồ uống đầu tiên: 

| Số đồ uống | Người | 
| --- | --- | 
| 1 | Sheldon | 
| 2 | Leonard | 
| 3 | Đồng xu | 
| 4 | Rajesh | 
| 5 | Howard | 

Sau đó, khối tiếp theo bắt đầu với hai Sheldon: 

| Số đồ uống | Người | 
| --- | --- | 
| 6 | Sheldon | 
| 7 | Sheldon | 

Nếu logic trừ bỏ qua một nhóm bổ sung, các giá trị như`n = 6`hoặc`n = 10`trở nên sai lầm. 

Giá trị lớn là một mối nguy hiểm khác. Vì số lượng tăng gấp đôi mỗi vòng nên việc triển khai sử dụng logarit dấu phẩy động có thể gây ra các vấn đề về độ chính xác gần các ranh giới. Số học số nguyên an toàn hơn và đơn giản hơn. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản sử dụng hàng đợi. 

Chúng tôi bắt đầu với:```
[Sheldon, Leonard, Penny, Rajesh, Howard]
```Ở mỗi bước, chúng tôi xóa người đứng đầu, ghi lại rằng họ đã uống cola, sau đó dán hai bản sao của chính người đó vào phía sau. 

Điều này hoạt động vì nó khớp chính xác với các quy tắc của quy trình. Sau khi thực hiện đủ các thao tác, cuối cùng chúng ta cũng đạt được`n`-đồ uống thứ 

Vấn đề là tốc độ tăng trưởng. Mỗi đồ uống sẽ tăng kích thước hàng đợi lên một. Nếu như`n = 10^9`, chúng ta sẽ cần khoảng một tỷ hoạt động xếp hàng và bộ nhớ khổng lồ. Điều đó là không thể trong giới hạn. 

Cấu trúc của hàng đợi mang lại cho chúng ta điều gì đó tốt hơn nhiều. 

Hãy suy nghĩ xem mỗi tên xuất hiện liên tiếp bao nhiêu lần trong một giai đoạn. 

Lúc đầu:```
Sheldon Leonard Penny Rajesh Howard
```Mỗi người xuất hiện một lần. 

Sau khi mọi người uống một lần:```
Sheldon Sheldon Leonard Leonard Penny Penny ...
```Mỗi người bây giờ xuất hiện hai lần. 

Sau đó:```
Sheldon Sheldon Sheldon Sheldon ...
```Mỗi người xuất hiện bốn lần. 

Vì vậy, quá trình hoạt động theo khối: 

| Giai đoạn | Bản sao mỗi người | Tổng số đồ uống trong giai đoạn | 
| --- | --- | --- | 
| 0 | 1 | 5 | 
| 1 | 2 | 10 | 
| 2 | 4 | 20 | 
| 3 | 8 | 40 | 

Thay vì mô phỏng từng đồ uống, chúng ta có thể trừ toàn bộ giai đoạn. 

Giả định`n = 52`. 

Các giai đoạn tiêu thụ:```
5 + 10 + 20 = 35
```Vì vậy, uống 52 nằm trong giai đoạn tiếp theo, trong đó mỗi người xuất hiện 8 lần liên tiếp. 

Bên trong giai đoạn đó:```
52 - 35 = 17
```8 vị trí đầu tiên thuộc về Sheldon, 8 vị trí tiếp theo thuộc về Leonard và 8 vị trí tiếp theo thuộc về Penny. 

Vị trí 17 rơi vào phân khúc của Penny. 

Điều này làm giảm vấn đề liên tục tăng gấp đôi kích thước khối cho đến khi chúng tôi xác định được pha chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Quá chậm | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu năm tên theo thứ tự ban đầu. 
2. Hãy để`group = 1`. Điều này có nghĩa là mỗi người hiện xuất hiện một lần liên tiếp. 
3. Tính xem có bao nhiêu đồ uống thuộc giai đoạn hiện tại. Vì có 5 người và mỗi người xuất hiện`group`lần, kích thước pha là`5 * group`. 
4. Trong khi`n`lớn hơn kích thước pha hiện tại, hãy trừ toàn bộ pha khỏi`n`và gấp đôi`group`. 

Điều này có tác dụng vì mọi giai đoạn đã hoàn thành sẽ biến mất hoàn toàn trước khi giai đoạn tiếp theo bắt đầu. 
5. Sau khi vòng lặp kết thúc,`n`nằm bên trong giai đoạn hiện tại. 
6. Trong giai đoạn này, mỗi người chiếm giữ chính xác`group`các vị trí liên tiếp. 
7. Chuyển đổi vị trí thành chỉ mục người bằng cách sử dụng:```
index = (n - 1) // group
```8. Xuất tên tại chỉ mục đó. 

### Tại sao nó hoạt động 

Mỗi giai đoạn chứa tất cả đồ uống trong đó mỗi người xuất hiện cùng số lần liên tiếp. 

Nếu như`group = 2^k`, thì pha đó chứa:```
[Sheldon repeated group times]
[Leonard repeated group times]
[Penny repeated group times]
[Rajesh repeated group times]
[Howard repeated group times]
```Việc trừ các giai đoạn trước đó là an toàn vì những đồ uống đó diễn ra hoàn toàn trước khi giai đoạn hiện tại bắt đầu. 

Khi chúng tôi xác định được pha chính xác, chia cho`group`cho chúng tôi biết phân khúc của người nào chứa đồ uống mong muốn. Vì các tên vẫn giữ nguyên thứ tự trong mọi giai đoạn nên chỉ số được tính toán luôn khớp với câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    names = ["Sheldon", "Leonard", "Penny", "Rajesh", "Howard"]

    group = 1

    while n > 5 * group:
        n -= 5 * group
        group *= 2

    index = (n - 1) // group
    print(names[index])

solve()
```Giải pháp bắt đầu bằng cách lưu trữ năm tên theo thứ tự vì thứ tự đó không bao giờ thay đổi trong suốt quá trình. 

Biến`group`đại diện cho số lượng bản sao liên tiếp mà mỗi người có trong giai đoạn hiện tại. Ban đầu mỗi người chỉ xuất hiện một lần, vì vậy`group = 1`. 

Vòng lặp loại bỏ toàn bộ các giai đoạn cùng một lúc. Ví dụ, khi`group = 4`, pha hiện tại chứa`5 * 4 = 20`đồ uống. Nếu như`n`lớn hơn 20, chúng tôi biết đồ uống mong muốn không thể thuộc giai đoạn này, vì vậy chúng tôi trừ tất cả 20 đồ uống và chuyển sang giai đoạn tiếp theo trong đó mỗi lần đếm sẽ tăng gấp đôi. 

Biểu thức:```
(n - 1) // group
```là chi tiết ranh giới quan trọng nhất. 

Giả định`group = 4`. 

Sau đó các vị trí:```
1 2 3 4
```thuộc về Sheldon,```
5 6 7 8
```thuộc về Leonard, v.v. 

Trừ 1 trước khi chia số nguyên sẽ chuyển đổi chính xác vị trí dựa trên 1 thành chỉ mục khối dựa trên 0. 

Nếu không có`-1`, ranh giới chính xác sẽ thất bại. Ví dụ: vị trí 4 sẽ ánh xạ không chính xác tới Leonard thay vì Sheldon. 

Thuật toán chỉ sử dụng số học số nguyên, do đó không có vấn đề về độ chính xác của dấu phẩy động ngay cả đối với các giá trị rất lớn của`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Bước | n | nhóm | Kích thước pha | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 1 | 5 |`n <= 5`, dừng lại | 
| Cuối cùng | 1 | 1 | 5 | chỉ số = (1 - 1) // 1 = 0 | 

Đầu ra:```
Sheldon
```Ví dụ này cho thấy đầu vào nhỏ nhất có thể. Câu trả lời nằm ở giai đoạn đầu tiên nên không có phép trừ nào xảy ra. 

### Ví dụ 2 

đầu vào:```
52
```| Bước | n | nhóm | Kích thước pha | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 52 | 1 | 5 | giai đoạn trừ | 
| Sau lần trừ thứ 1 | 47 | 2 | 10 | giai đoạn trừ | 
| Sau phép trừ thứ 2 | 37 | 4 | 20 | giai đoạn trừ | 
| Sau phép trừ thứ 3 | 17 | 8 | 40 | dừng lại | 
| Cuối cùng | 17 | 8 | 40 | chỉ số = (17 - 1) // 8 = 2 | 

chỉ mục`2`tương ứng với Penny. 

Đầu ra:```
Penny
```Dấu vết này cho thấy cách thuật toán bỏ qua toàn bộ phần của hàng đợi thay vì mô phỏng từng đồ uống riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) |`group`nhân đôi mỗi lần lặp | 
| Không gian | O(1) | chỉ một vài biến được lưu trữ | 

Từ`group`tăng theo cấp số nhân, vòng lặp chỉ chạy khoảng 30 lần ngay cả khi`n = 10^9`. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())

    names = ["Sheldon", "Leonard", "Penny", "Rajesh", "Howard"]

    group = 1

    while n > 5 * group:
        n -= 5 * group
        group *= 2

    index = (n - 1) // group
    print(names[index])

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("1\n") == "Sheldon\n", "sample 1"

# boundary of first phase
assert run("5\n") == "Howard\n", "end of first phase"

# start of second phase
assert run("6\n") == "Sheldon\n", "start of doubled block"

# inside larger doubled group
assert run("52\n") == "Penny\n", "larger phase test"

# very large input
assert run("1000000000\n") == "Penny\n", "large input handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`Sheldon`| đầu vào tối thiểu | 
|`5`|`Howard`| chính xác kết thúc giai đoạn đầu tiên | 
|`6`|`Sheldon`| chuyển sang giai đoạn tiếp theo | 
|`52`|`Penny`| bỏ qua nhiều giai đoạn | 
|`1000000000`|`Penny`| hiệu suất đầu vào lớn và độ chính xác | 

## Vỏ cạnh 

Hãy xem xét ranh giới giữa các giai đoạn. 

đầu vào:```
5
```Giai đoạn đầu tiên có kích thước 5 vì mỗi người trong số năm người xuất hiện một lần. 

Điều kiện vòng lặp:```
while n > 5 * group
```không thực thi vì`5 > 5`là sai. 

Sau đó:```
index = (5 - 1) // 1 = 4
```Chỉ số 4 tương ứng với Howard. 

Đầu ra:```
Howard
```sử dụng`>=`thay vì`>`sẽ trừ sai toàn bộ pha và đưa ra câu trả lời sai. 

Bây giờ hãy xem xét lần uống đầu tiên của giai đoạn tiếp theo. 

đầu vào:```
6
```Ban đầu:```
group = 1
phase size = 5
```Từ`6 > 5`, chúng tôi trừ:```
n = 1
group = 2
```Bây giờ giai đoạn hiện tại là:```
Sheldon Sheldon Leonard Leonard ...
```Chúng tôi tính toán:```
index = (1 - 1) // 2 = 0
```ánh xạ chính xác tới Sheldon. 

Trường hợp này xác nhận rằng quá trình chuyển pha được xử lý đúng cách. 

Cuối cùng, hãy xem xét một giá trị rất lớn. 

đầu vào:```
1000000000
```Vòng lặp tăng gấp đôi`group`nhiều lần:```
1, 2, 4, 8, 16, ...
```Chỉ sau khoảng 30 lần lặp, pha chính xác đã được tìm thấy. 

Không có hàng đợi nào được tạo nên việc sử dụng bộ nhớ không đổi và quá trình thực thi vẫn nhanh ngay cả khi bị hạn chế tối đa.
