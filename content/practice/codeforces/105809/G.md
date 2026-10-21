---
title: "CF 105809G - Trò chơi bi"
description: "Chúng ta có một tập hợp các viên bi, mỗi viên chứa một số nguyên. Một người chơi, Sebastian, lấy từng viên bi có số lẻ. Người chơi còn lại, Sebastiàn, lấy mọi viên bi có số chẵn. Người chiến thắng chỉ được xác định bằng số viên bi mà mỗi người chơi nhận được."
date: "2026-06-25T15:29:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105809
codeforces_index: "G"
codeforces_contest_name: "Code Rush 2025"
rating: 0
weight: 105809
solve_time_s: 36
verified: true
draft: false
---

[CF 105809G - Trò chơi bi](https://codeforces.com/problemset/problem/105809/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các viên bi, mỗi viên chứa một số nguyên. Một người chơi, Sebastian, lấy từng viên bi có số lẻ. Người chơi còn lại, Sebastiàn, lấy mọi viên bi có số chẵn. 

Người chiến thắng chỉ được xác định bằng số viên bi mà mỗi người chơi nhận được. Nếu số điểm bằng nhau, Sebastian sẽ thắng hòa. Chúng ta phải xuất`"Sebastian"`nếu Sebastiàn có nhiều viên bi hơn Sebastian. Nếu không chúng tôi xuất ra`"Notbastian"`. 

Đầu vào bao gồm một số nguyên`n`, theo sau là`n`giá trị đá cẩm thạch. Giá trị thực tế có thể lớn bằng`10^9`, nhưng chỉ có tính chẵn lẻ của chúng là quan trọng. Từ`n`nhiều nhất là`10^6`, bất kỳ thuật toán nào kiểm tra từng viên bi một lần đều dễ dàng đủ nhanh, trong khi bất kỳ thuật toán nào đắt hơn đáng kể so với thời gian tuyến tính sẽ không cần thiết. Với một triệu con số, một`O(n)`quá trình quét thực hiện khoảng một triệu thao tác, điều này không đáng kể trong giới hạn. 

Nguồn gốc của sai lầm chính là quy tắc hòa. 

Coi như:```
4
1 1 2 2
```Có hai viên bi lẻ và hai viên bi chẵn. Vì số phiếu bằng nhau nên Sebastian thắng, nghĩa là Sebastiàn không giữ được tên. Đầu ra đúng là:```
Notbastian
```Việc thực hiện bất cẩn để kiểm tra`even_count >= odd_count`sẽ in sai`"Sebastian"`. 

Một trường hợp dễ bỏ sót khác là khi tất cả các viên bi đều thuộc về một người chơi.```
3
2 4 6
```Sebastiàn lấy được cả ba viên bi và Sebastian không lấy được viên nào, vì vậy kết quả đúng là:```
Sebastian
```Tương tự:```
3
1 3 5
```cho```
Notbastian
```bởi vì Sebastiàn không nhận được viên bi nào. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng chính xác các quy tắc. Đối với mỗi viên bi, hãy xác định xem số của nó là chẵn hay lẻ và tăng số đếm tương ứng. Cuối cùng, so sánh hai quầy theo điều kiện thắng của trò chơi. 

Người ta có thể tưởng tượng một cách giải thích mạnh mẽ lưu trữ rõ ràng các viên bi của Sebastian và các viên bi của Sebastiàn trong các mảng riêng biệt và sau đó so sánh kích thước của chúng. Điều này đúng vì kết quả trận đấu chỉ phụ thuộc vào số viên bi mà mỗi người chơi nhận được. Tuy nhiên, dung lượng lưu trữ bổ sung là không cần thiết vì chỉ có số lượng mới quan trọng. 

Quan sát quan trọng là bản thân các giá trị của đá cẩm thạch không bao giờ ảnh hưởng đến kết quả ngoài mức tương đương. Chúng ta không quan tâm liệu một viên bi có chứa`2`hoặc`1000000000`, chỉ liệu nó có chẵn hay không. Một khi chúng ta nhận ra điều này, toàn bộ vấn đề sẽ giảm xuống việc đếm số chẵn và số lẻ. 

Sau khi đếm, Sebastiàn chỉ giữ tên khi số viên bi chẵn lớn hơn số viên bi lẻ. Mọi tình huống khác, bao gồm cả sự ràng buộc, đều tạo ra`"Notbastian"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưu trữ viên bi của cả hai người chơi một cách rõ ràng | O(n) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Đếm số chẵn và tỷ lệ cược | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và`n`giá trị đá cẩm thạch. 
2. Khởi tạo`even_count = 0`Và`odd_count = 0`. 
3. Quét qua mọi giá trị bằng đá cẩm thạch. 
4. Nếu giá trị chẵn thì tăng`even_count`; nếu không thì tăng`odd_count`. 
5. Sau khi xử lý tất cả các viên bi, hãy so sánh số lượng. 
6. Nếu`even_count > odd_count`, in`"Sebastian"`vì Sebastiàn có nhiều viên bi hơn. 
7. Nếu không thì in`"Notbastian"`. Điều này bao gồm trường hợp hòa, trong đó Sebastian thắng theo luật. 

### Tại sao nó hoạt động 

Mỗi viên bi thuộc về chính xác một người chơi chỉ dựa trên tính chẵn lẻ. Đếm số chẵn sẽ biết số viên bi mà Sebastiàn nhận được, đếm số lẻ sẽ biết số Sebastian nhận được. Người chiến thắng được xác định hoàn toàn bằng cách so sánh các số liệu này với các mối quan hệ được trao cho Sebastian. Vì thuật toán tính toán chính xác hai đại lượng này và áp dụng quy tắc so sánh đã nêu nên nó luôn tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

even_count = 0

for x in a:
    if x % 2 == 0:
        even_count += 1

odd_count = n - even_count

if even_count > odd_count:
    print("Sebastian")
else:
    print("Notbastian")
```Giải pháp thực hiện một lần duyệt qua mảng và đếm các giá trị chẵn. Vì mỗi viên bi là chẵn hoặc lẻ nên số lẻ có thể được tính là`n - even_count`. 

Sự so sánh ở cuối là nghiêm ngặt. Đây là chi tiết triển khai quan trọng nhất. sử dụng`>=`sẽ trao mối quan hệ không chính xác cho Sebastiàn, mâu thuẫn với tuyên bố. 

Không có vấn đề tràn tồn tại vì số lượng không bao giờ vượt quá`n`, nhiều nhất là`10^6`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 2 4
```| Đá cẩm thạch | Chẵn lẻ | chẵn_count | lẻ_count | 
| --- | --- | --- | --- | 
| 1 | lẻ | 0 | 1 | 
| 2 | Thậm chí | 1 | 1 | 
| 2 | Thậm chí | 2 | 1 | 
| 4 | Thậm chí | 3 | 1 | 

So sánh cuối cùng:`3 > 1`. 

Đầu ra:```
Sebastian
```Điều này chứng tỏ trường hợp chiến thắng bình thường của Sebastiàn, người nhận được nhiều viên bi hơn Sebastian. 

### Ví dụ 2 

đầu vào:```
4
1 1 2 2
```| Đá cẩm thạch | Chẵn lẻ | chẵn_count | lẻ_count | 
| --- | --- | --- | --- | 
| 1 | lẻ | 0 | 1 | 
| 1 | lẻ | 0 | 2 | 
| 2 | Thậm chí | 1 | 2 | 
| 2 | Thậm chí | 2 | 2 | 

So sánh cuối cùng:`2 = 2`. 

Đầu ra:```
Notbastian
```Ví dụ này nêu bật quy tắc hòa. Mặc dù cả hai người chơi đều nhận được số viên bi như nhau nhưng Sebastian vẫn thắng hòa nên Sebastiàn phải đổi tên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi viên bi được kiểm tra đúng một lần | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Với tối đa`10^6`viên bi, quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    a = list(map(int, input().split()))

    even_count = sum(x % 2 == 0 for x in a)
    odd_count = n - even_count

    if even_count > odd_count:
        return "Sebastian\n"
    return "Notbastian\n"

# provided samples
assert run("4\n1 2 2 4\n") == "Sebastian\n", "sample 1"
assert run("4\n1 1 2 2\n") == "Notbastian\n", "sample 2"

# custom cases
assert run("1\n2\n") == "Sebastian\n", "single even marble"
assert run("1\n1\n") == "Notbastian\n", "single odd marble"
assert run("5\n2 4 6 8 10\n") == "Sebastian\n", "all even"
assert run("5\n1 3 5 7 9\n") == "Notbastian\n", "all odd"
assert run("6\n1 2 3 4 5 6\n") == "Notbastian\n", "tie case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2`|`Sebastian`| Kích thước tối thiểu, thậm chí là đá cẩm thạch | 
|`1 / 1`|`Notbastian`| Kích thước tối thiểu, đá cẩm thạch lẻ | 
| Tất cả các giá trị chẵn |`Sebastian`| Sebastiàn nhận từng viên bi | 
| Tất cả các giá trị lẻ |`Notbastian`| Sebastiàn không nhận được gì | 
| Số lẻ và số chẵn bằng nhau |`Notbastian`| Xử lý dây buộc đúng cách | 

## Vỏ cạnh 

Hãy xem xét tình huống hòa:```
4
1 1 2 2
```Thuật toán đếm hai viên bi lẻ và hai viên bi chẵn. Từ`even_count > odd_count`là sai, nó in`"Notbastian"`. Điều này hoàn toàn phù hợp với quy tắc hòa đặc biệt. 

Hãy xem xét trường hợp Sebastiàn nhận được mọi viên bi:```
3
2 4 6
```Quá trình quét tạo ra`even_count = 3`Và`odd_count = 0`. Việc so sánh thành công nên kết quả là:```
Sebastian
```Hãy xem xét thái cực ngược lại:```
3
1 3 5
```Quá trình quét tạo ra`even_count = 0`Và`odd_count = 3`. Việc so sánh không thành công nên kết quả đầu ra là:```
Notbastian
```Những trường hợp này đề cập đến khía cạnh tế nhị duy nhất của vấn đề, sự khác biệt giữa thắng đậm và hòa. Phương pháp đếm xử lý tất cả chúng một cách tự nhiên.
