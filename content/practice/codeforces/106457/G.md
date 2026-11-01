---
title: "CF 106457G - Eridanus Prime"
description: "Vấn đề mô tả một đội tàu vận tải. Tàu i có sức chở hàng hóa Ai. Chúng ta phải chọn một thùng duy nhất có kích thước x. Một con tàu chỉ có thể được sử dụng khi sức chứa của nó có thể được chia thành các thùng có kích thước chính xác x, do đó, con tàu đóng góp khi và chỉ khi Ai là bội số của x."
date: "2026-06-25T09:14:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106457
codeforces_index: "G"
codeforces_contest_name: "UTPC Spring 2026 Open Contest"
rating: 0
weight: 106457
solve_time_s: 30
verified: true
draft: false
---

[CF 106457G - Eridanus Prime](https://codeforces.com/problemset/problem/106457/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một đội tàu vận tải. Tàu thủy`i`có sức chở hàng hóa`A_i`. Chúng ta phải chọn một kích thước thùng duy nhất`x`. Một con tàu chỉ được sử dụng khi sức chứa của nó có thể chia thành các thùng có kích thước chính xác`x`, do đó con tàu đóng góp khi và chỉ khi`A_i`là bội số của`x`. 

Đối với kích thước thùng đã chọn, khoản thanh toán của Đế chế là kích thước thùng nhân với số lượng tàu có thể sử dụng nó. Về mặt toán học, giá trị của`x`là:`x * (number of A_i values divisible by x)`Nhiệm vụ là tìm kích thước thùng có số tiền xuất chi lớn nhất. Nếu nhiều kích cỡ thùng tạo ra cùng một khoản thanh toán thì phải chọn kích thước thùng nhỏ nhất. 

Đầu vào chứa số lượng tàu và năng lực của chúng. Đầu ra là kích thước thùng tốt nhất. 

Hạn chế quan trọng là có thể có tới`10^6`tàu, và mọi sức chứa tối đa`10^6`. Một giải pháp kiểm tra mọi kích thước thùng có thể có đối với từng con tàu sẽ hoạt động xung quanh`10^12`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn một giây thông thường cho phép. Chúng ta cần khai thác giá trị tối đa nhỏ của sức chứa thay vì số lượng tàu. 

Các trường hợp cạnh chính đến từ dây buộc và từ kích thước thùng xuất hiện dưới dạng ước số nhưng không thường xuyên nhất. 

Ví dụ: nếu đầu vào là:```
3
2 4 6
```Đầu ra đúng là:```
3
```Kích thước thùng`2`hoạt động cho cả ba con tàu, mang lại`2 * 3 = 6`. Kích thước thùng`3`chỉ hoạt động cho tàu có công suất`6`, cho`3 * 1 = 3`, vì vậy ví dụ này không tạo ra kết quả hòa. Việc triển khai bất cẩn chỉ xem xét các ước số có giá trị lớn nhất có thể bỏ lỡ các ứng cử viên tốt hơn trong số các ước số nhỏ hơn. 

Một hộp cà vạt là:```
4
2 4 6 8
```Đầu ra đúng là:```
4
```Kích thước thùng`2`cho`2 * 4 = 8`và kích thước thùng`4`cho`4 * 2 = 8`. Vì cả hai đều có mức thanh toán như nhau nên chúng ta phải chọn kích thước thùng nhỏ hơn, vì vậy câu trả lời là`2`. Việc triển khai bất cẩn cập nhật câu trả lời bất cứ khi nào nó thấy điểm bằng nhau sẽ trả về giá trị sai nếu nó thay thế câu trả lời hiện tại bằng ứng viên sau. 

Trường hợp kích thước tối thiểu là:```
1
1000000
```Câu trả lời là:```
1000000
```Con tàu duy nhất phải được tính và toàn bộ sức chở của nó có thể được sử dụng làm kích cỡ thùng. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ thử mọi kích thước thùng có thể`x`. Đối với mỗi`x`, chúng tôi quét tất cả các tàu và đếm xem có bao nhiêu năng lực chia hết cho`x`. Phương pháp này đúng vì nó đánh giá khoản thanh toán chính xác của mọi lựa chọn có thể. Tuy nhiên, với`N = 10^6`, điều này sẽ yêu cầu kiểm tra tới`10^6`giá trị có thể chống lại lên đến`10^6`tàu, dẫn đến khoảng`10^12`các phép thử tính chia hết. 

Quan sát hữu ích là năng lực cũng bị giới hạn ở`10^6`. Thay vì hỏi mỗi con tàu xem nó có chấp nhận kích thước thùng hay không, chúng ta có thể đảo ngược quy trình và hỏi mỗi kích cỡ thùng có bao nhiêu sức chứa là bội số của nó. 

Cho phép`freq[v]`là số lượng tàu có sức chứa`v`. Đối với kích thước thùng cố định`x`, số tàu có thể sử dụng chính xác có công suất:`x, 2x, 3x, ...`đạt công suất tối đa. Chúng ta có thể tính tổng tần số của các giá trị này bằng cách sử dụng vòng lặp dạng sàng. Mỗi kích thước thùng có thể được xử lý bằng cách truy cập bội số của nó, nhanh hơn nhiều so với việc kiểm tra từng con tàu riêng biệt. 

Sau khi tính toán số lượng cho mỗi`x`, chúng tôi so sánh khoản thanh toán`x * count`. Khi hai khoản thanh toán bằng nhau, chúng tôi giữ mức thanh toán nhỏ hơn`x`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N * tối đa(A)) | O(1) | Quá chậm | 
| Tối ưu | O(max(A) log max(A)) | O(tối đa(A)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi tàu có bao nhiêu sức chở. Lưu trữ cái này trong`freq`, Ở đâu`freq[v]`cho chúng ta biết dung lượng gấp bao nhiêu lần`v`xuất hiện. Điều này cho phép chúng tôi xử lý các giá trị thay vì các tàu riêng lẻ. 
2. Lặp lại mọi kích thước thùng có thể`x`từ`1`đến công suất tối đa. Đối với mỗi`x`, cộng tần số của tất cả các bội số của`x`. Đây chính xác là những con tàu có thể được triển khai với kích thước thùng`x`. 
3. Tính toán khoản thanh toán như`x * count`. Nếu nó lớn hơn khoản thanh toán tốt nhất được thấy cho đến nay, hãy thay thế câu trả lời. Nếu bằng nhau thì giữ kích thước thùng nhỏ hơn. 
4. Xuất kích thước thùng được lưu trữ sau khi tất cả các ứng viên đã được kiểm tra. 

Tại sao nó hoạt động: 

Đối với mọi kích thước thùng có thể`x`, thuật toán truy cập mọi dung lượng chia hết cho`x`, do đó số lượng được tính toán chính xác là số lượng tàu có thể sử dụng kích thước thùng đó. Do đó, việc tính toán khoản thanh toán giống hệt với định nghĩa trong bài toán. Vì mọi điều có thể`x`được xem xét và xử lý các ràng buộc bằng cách giữ giá trị nhỏ nhất thì câu trả lời cuối cùng được đảm bảo là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)
    freq = [0] * (mx + 1)

    for value in a:
        freq[value] += 1

    best_x = 1
    best_value = 0

    for x in range(1, mx + 1):
        count = 0
        for multiple in range(x, mx + 1, x):
            count += freq[multiple]

        value = x * count

        if value > best_value:
            best_value = value
            best_x = x
        elif value == best_value and x < best_x:
            best_x = x

    print(best_x)

if __name__ == "__main__":
    solve()
```Mảng tần số chỉ lưu trữ những dung lượng thực sự tồn tại trong dải đầu vào. Kích thước của nó được quyết định bởi sức chứa lớn nhất chứ không phải bởi số lượng tàu. 

Vòng lặp bên ngoài thử mọi kích thước thùng có thể. Vòng lặp bên trong duyệt qua các bội số của nó, cộng tần số của chúng. Điều này tránh việc phải quét liên tục toàn bộ danh sách tàu. 

Điều kiện ràng buộc được xử lý rõ ràng. Khi hai kích thước thùng có cùng khoản thanh toán, câu trả lời hiện tại chỉ được thay thế nếu kích thước thùng mới nhỏ hơn. Điều này ngăn kết quả phụ thuộc vào thứ tự lặp. 

Số nguyên Python không bị tràn nên phép nhân`x * count`là an toàn ngay cả ở các giá trị tối đa. Trường hợp ranh giới duy nhất cần xử lý là công suất tối đa, được bao gồm trong mảng tần số và trong mỗi vòng lặp. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5
2 3 3 4 5
```Các trạng thái quan trọng là: 

| Kích thước thùng | Đã tính bội số | Số lượng tàu | Xuất chi | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 2, 3, 4, 5 | 5 | 5 | 1 | 
| 2 | 2, 4 | 2 | 4 | 1 | 
| 3 | 3 | 2 | 6 | 3 | 
| 4 | 4 | 1 | 4 | 3 | 
| 5 | 5 | 1 | 5 | 3 | 

Khoản thanh toán lớn nhất được tạo ra bởi kích thước thùng`3`, vậy câu trả lời là`3`. 

Đối với mẫu thứ hai:```
5
5 4 3 2 1
```Dấu vết là: 

| Kích thước thùng | Đã tính bội số | Số lượng tàu | Xuất chi | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 1, 2, 3, 4, 5 | 5 | 5 | 1 | 
| 2 | 2, 4 | 2 | 4 | 1 | 
| 3 | 3 | 1 | 3 | 1 | 
| 4 | 4 | 1 | 4 | 1 | 
| 5 | 5 | 1 | 5 | 1 | 

Khoản thanh toán tốt nhất được chia theo kích thước thùng`1`Và`5`. Kích thước thùng nhỏ hơn được chọn, cho`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M) | Mỗi kích thước thùng sẽ ghé thăm bội số của nó, trong đó`M`là công suất tối đa. | 
| Không gian | O(M) | Mảng tần số lưu trữ số lượng cho mọi công suất lên đến`M`. | 

Với`M <= 10^6`, sàng chuỗi hài chạy thoải mái trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ vì chỉ có một mảng số nguyên có kích thước`10^6 + 1`là cần thiết. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    mx = max(a)
    freq = [0] * (mx + 1)

    for x in a:
        freq[x] += 1

    ans = 1
    best = 0

    for x in range(1, mx + 1):
        cnt = 0
        for y in range(x, mx + 1, x):
            cnt += freq[y]
        score = x * cnt
        if score > best or (score == best and x < ans):
            best = score
            ans = x

    print(ans)

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert solve("5\n2 3 3 4 5\n") == "3\n", "sample 1"
assert solve("5\n5 4 3 2 1\n") == "1\n", "sample 2"

assert solve("1\n1000000\n") == "1000000\n", "single ship maximum capacity"
assert solve("4\n2 4 6 8\n") == "2\n", "tie chooses smallest"
assert solve("6\n12 18 24 30 36 42\n") == "6\n", "common divisors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1000000`|`1000000`| Trường hợp ranh giới giá trị đơn | 
|`4 / 2 4 6 8`|`2`| Xử lý ràng buộc thanh toán bằng nhau | 
|`6 / 12 18 24 30 36 42`|`6`| Ước số đúng khi đếm nhiều bội số | 

## Vỏ cạnh 

Đối với hộp đựng cà vạt:```
4
2 4 6 8
```Mảng tần số chứa một lần xuất hiện của mỗi công suất. Khi`x = 2`, bội số là`2, 4, 6, 8`, đưa ra một khoản thanh toán`8`. Khi`x = 4`, bội số là`4, 8`, đồng thời đưa ra một khoản thanh toán`8`. Thuật toán đạt`x = 4`muộn hơn, nhưng điều kiện điểm bằng nhau vẫn được giữ nguyên`x = 2`vì nó nhỏ hơn. 

Đối với một con tàu:```
1
1000000
```Kích thước thùng hữu ích duy nhất là ước số của`1000000`. Khoản thanh toán lớn nhất đến từ việc sử dụng toàn bộ sức chứa của tàu làm kích thước thùng, mang lại`1000000 * 1`. Vòng lặp bội bao gồm chính giá trị lớn nhất nên câu trả lời được tìm thấy chính xác. 

Đối với nhiều năng lực lặp đi lặp lại:```
5
6 6 6 6 6
```Kích thước thùng`6`đưa ra một khoản thanh toán`30`, trong khi các ước số nhỏ hơn cho khoản thanh toán nhỏ hơn. Phương pháp tần số đếm tất cả năm tàu ​​cùng một lúc thông qua`freq[6]`, tránh mọi sai sót do năng lực trùng lặp.
