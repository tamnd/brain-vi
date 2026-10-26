---
title: "CF 106106I - \u041d\u0435\u043c\u043d\u043e\u0436\u043a\u043e \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0438"
description: "Nhiệm vụ yêu cầu một quan sát toán học đơn giản. Cho một số k và cần tìm số nguyên dương lớn nhất n sao cho n không lớn hơn k và giá trị của n đúng bằng giai thừa của nó."
date: "2026-06-25T11:41:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106106
codeforces_index: "I"
codeforces_contest_name: "\u0423\u0440\u0430\u043b\u044c\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u042e\u043d\u0438\u043e\u0440\u044b 2024"
rating: 0
weight: 106106
solve_time_s: 26
verified: true
draft: false
---

[CF 106106I - \u041d\u0435\u043c\u043d\u043e\u0436\u043a\u043e \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0438](https://codeforces.com/problemset/problem/106106/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu một quan sát toán học đơn giản. Chúng tôi được cấp một số`k`và cần tìm số nguyên dương lớn nhất`n`như vậy`n`không lớn hơn`k`và giá trị của`n`chính xác bằng giai thừa của nó. Giai thừa của một số là tích của tất cả các số nguyên dương nhỏ hơn số đó. Vấn đề ban đầu là từ Codeforces Gym 106106, vấn đề I, "Немножко математики". 

Đầu vào chỉ chứa một số nguyên`k`, với`1 ≤ k ≤ 10^5`. Vì giới hạn trên nhỏ nên có thể mô phỏng trực tiếp, nhưng quan sát hữu ích hơn là các giai thừa tăng cực kỳ nhanh. Sau một vài giá trị đầu tiên,`n!`trở nên lớn hơn nhiều so với`n`, nên không thể có nhiều ứng viên để kiểm tra. 

Các số nguyên dương duy nhất thỏa mãn`n = n!`là`1`Và`2`. Vì`n = 1`, giai thừa là`1! = 1`. Vì`n = 2`, giai thừa là`2! = 2`. Đối với mọi`n ≥ 3`, giai thừa chứa một phép nhân với ít nhất`2`, và kết quả thực sự lớn hơn`n`. 

Các trường hợp chính là khi`k`chính xác là một trong những giá trị hợp lệ này hoặc khi nó nằm giữa chúng. 

Đối với đầu vào:```
1
```đầu ra đúng là:```
1
```Một giải pháp bất cẩn chỉ kiểm tra xem`k`bản thân nó thỏa mãn điều kiện có thể thất bại nếu nó cho rằng luôn có câu trả lời nhỏ hơn. 

Đối với đầu vào:```
3
```đầu ra đúng là:```
2
```giá trị`3`không hợp lệ vì`3! = 6`, do đó đáp án phải chuyển xuống số hợp lệ trước đó. 

Đối với đầu vào:```
100000
```đầu ra đúng là:```
2
```Phép tính giai thừa bạo lực có thể tràn trong các ngôn ngữ có số nguyên có kích thước cố định nếu nó tiếp tục nhân một cách không cần thiết, ngay cả khi câu trả lời đã được biết trước. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp qua mọi số từ`1`ĐẾN`k`, tính giai thừa của nó và ghi nhớ số lớn nhất có giai thừa bằng chính nó. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa của một câu trả lời hợp lệ. Tuy nhiên, mặc dù`k`chỉ là`100000`, việc tính toán giai thừa nhiều lần là không cần thiết. Giá trị giai thừa trở nên rất lớn rất nhanh và việc lưu trữ hoặc tính toán các giá trị này sẽ làm tăng thêm độ phức tạp mà không cung cấp thông tin hữu ích. 

Quan sát quan trọng là phương trình`n = n!`là cực kỳ hạn chế. Một lần`n`đạt tới`3`, giai thừa ngay lập tức trở nên lớn hơn:```
3! = 6 > 3
```Đối với mỗi số lớn hơn, khoảng cách chỉ tăng lên. Điều này để lại chính xác hai câu trả lời có thể:`1`Và`2`. 

Vấn đề sau đó trở thành một so sánh đơn giản. Nếu như`k`ít nhất là`2`, giá trị hợp lệ lớn nhất hiện có là`2`. Ngược lại, câu trả lời duy nhất có thể là`1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * k) trong một mô phỏng giai thừa đơn giản | O(1) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị`k`. 
2. Kiểm tra xem`k`ít nhất là`2`. giá trị`2`là hợp lệ bởi vì`2! = 2`và không có số nguyên dương nào lớn hơn có thể thỏa mãn phương trình. 
3. Nếu`k`nhỏ hơn`2`, đầu ra`1`, bởi vì`1! = 1`là giá trị hợp lệ duy nhất còn lại. 

Tại sao nó hoạt động: hàm giai thừa chỉ có điểm cố định tại`1`Và`2`giữa các số nguyên dương. Khi con số đạt tới`3`, nhân với tất cả các số nguyên dương trước đó sẽ làm cho giai thừa lớn hơn số ban đầu mãi mãi. 

## Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là chỉ có hai số nguyên dương trong đó số đó và giai thừa của nó bằng nhau. Các giá trị`1`Và`2`có thể được xác minh trực tiếp. Đối với mọi`n ≥ 3`, chúng ta có:```
n! = 1 * 2 * 3 * ... * n
```Sản phẩm chứa đựng những yếu tố`1 * 2 * ... * (n - 1)`, ít nhất đã là vậy rồi`2`cho các giá trị này và nhân với`n`làm cho giai thừa lớn hơn`n`. Vì vậy không có giá trị nào lớn hơn`2`có thể là một câu trả lời Thuật toán chỉ chọn giữa hai số hợp lệ có thể có nên luôn trả về số lớn nhất không vượt quá`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    if k >= 2:
        print(2)
    else:
        print(1)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo quan sát toán học. điều kiện`k >= 2`bao gồm mọi trường hợp có thể có câu trả lời`2`. Nếu không thì,`k`phải là`1`, và câu trả lời cũng là`1`. 

Không có tính toán giai thừa trong mã. Điều này tránh được phép nhân không cần thiết và loại bỏ mọi nguy cơ tràn số nguyên trong các ngôn ngữ có loại số nguyên nhỏ hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```| Bước | k | Tình trạng | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 4 |`4 >= 2`là đúng | 2 | 

giá trị`2`là hợp lệ và là câu trả lời lớn nhất có thể không vượt quá`4`. 

### Ví dụ 2 

đầu vào:```
2
```| Bước | k | Tình trạng | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 2 |`2 >= 2`là đúng | 2 | 

Điều này kiểm tra ranh giới trong đó số hợp lệ lớn nhất chính xác bằng đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một so sánh được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp dễ dàng phù hợp với các giới hạn vì nó thực hiện một lượng công việc không đổi bất kể giá trị đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    k = int(input())
    if k >= 2:
        return "2\n"
    return "1\n"

# provided samples
assert solution("4\n") == "2\n", "sample 1"
assert solution("2\n") == "2\n", "sample 2"
assert solution("10\n") == "2\n", "sample 3"

# custom cases
assert solution("1\n") == "1\n", "minimum value"
assert solution("3\n") == "2\n", "first invalid factorial value"
assert solution("100000\n") == "2\n", "maximum input"
assert solution("2\n") == "2\n", "boundary value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Xử lý đầu vào nhỏ nhất có thể | 
|`3`|`2`| Xác nhận rằng các giá trị sau`2`bị từ chối | 
|`100000`|`2`| Xử lý đầu vào lớn nhất được phép | 
|`2`|`2`| Kiểm tra ranh giới chính xác của hai câu trả lời hợp lệ | 

## Vỏ cạnh 

cho`k = 1`, thuật toán kiểm tra`k >= 2`, sai và trả về`1`. Điều này đúng vì`1! = 1`, và không có số nguyên dương nào nhỏ hơn. 

Vì`k = 3`, thuật toán trả về`2`ngay lập tức. Mặc dù`3`đủ lớn để xem xét, nó không thể được sử dụng vì`3! = 6`. Sự so sánh với`2`nắm bắt giá trị hợp lệ lớn nhất bên dưới nó. 

Vì`k = 100000`, thuật toán không cố gắng tính bất kỳ giai thừa nào. Nó trở lại`2`sau một lần so sánh, tránh sự tăng trưởng không cần thiết của các giá trị giai thừa trong khi vẫn cho kết quả chính xác.
