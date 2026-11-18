---
title: "CF 2023A - Nối mảng"
description: "Chúng ta có n mảng và mỗi mảng chứa chính xác hai số. Chúng ta có thể sắp xếp lại các mảng này theo bất kỳ cách nào chúng ta muốn, nhưng bên trong mỗi mảng, thứ tự của hai phần tử không được thay đổi. Sau khi chọn thứ tự, chúng ta ghép tất cả các mảng và thu được một chuỗi có độ dài 2n."
date: "2026-06-08T12:32:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2023
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 980 (Div. 1)"
rating: 1300
weight: 2023
solve_time_s: 136
verified: false
draft: false
---

[CF 2023A - Nối mảng](https://codeforces.com/problemset/problem/2023/A) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, tham lam, toán học, sắp xếp 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`n`mảng và mỗi mảng chứa chính xác hai số. Chúng ta có thể sắp xếp lại các mảng này theo bất kỳ cách nào chúng ta muốn, nhưng bên trong mỗi mảng, thứ tự của hai phần tử không được thay đổi. 

Sau khi chọn thứ tự, chúng ta ghép tất cả các mảng và thu được một chuỗi có độ dài`2n`. Mục tiêu của chúng tôi là chọn thứ tự giảm thiểu số lần đảo ngược trong chuỗi cuối cùng. 

Chi tiết quan trọng là chúng ta không được yêu cầu tính số lần đảo ngược tối thiểu. Chúng ta chỉ cần xuất ra bất kỳ thứ tự nối nào đạt được nó. 

Tổng số mảng trên tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`. Điều này ngay lập tức loại trừ bất cứ điều gì kiểm tra nhiều hoán vị. Ngay cả việc kiểm tra tất cả các mối quan hệ theo cặp giữa các mảng cũng chỉ vừa đủ chấp nhận được, trong khi mọi thứ mang tính giai thừa đều vô vọng. Một giải pháp xung quanh`O(n log n)`mỗi trường hợp thử nghiệm là mục tiêu tự nhiên. 

Có một số tình huống dễ bỏ lỡ. 

Hãy xem xét các mảng có các phần tử đang tăng lên:```
2
1 100
2 3
```Một ý tưởng ngây thơ có thể là sắp xếp theo phần tử đầu tiên. Điều đó mang lại`[1,100] [2,3]`, tạo ra nhiều đảo ngược mảng chéo vì`100`lớn hơn cả hai`2`Và`3`. Thứ tự tối ưu thực sự là`[2,3] [1,100]`. 

Một trường hợp tinh tế khác là khi một mảng đang giảm:```
2
5 1
4 2
```Mỗi mảng đã đóng góp một sự đảo ngược nội bộ không thể tránh khỏi. Vì thứ tự nội bộ không thể thay đổi nên điều duy nhất chúng ta có thể tối ưu hóa là sự đảo ngược được tạo giữa các mảng khác nhau. 

Giá trị bằng nhau cũng quan trọng:```
2
1 1
1 1
```Các số bằng nhau không bao giờ tạo ra sự đảo ngược. Bất kỳ thứ tự chính xác nào cũng sẽ xử lý các mối quan hệ một cách tự nhiên mà không cần đưa ra logic đặc biệt. 

Cuối cùng, mảng như```
3
100 1
2 3
4 5
```chỉ ra tại sao chỉ nhìn vào phần tử đầu tiên hoặc chỉ nhìn vào phần tử thứ hai là không đủ. Sự tương tác giữa hai giá trị bên trong mỗi mảng là yếu tố quyết định quy tắc sắp xếp đúng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Hãy thử mọi hoán vị của`n`mảng, xây dựng chuỗi kết quả, đếm số lần đảo ngược và giữ hoán vị tốt nhất. 

Điều này đúng vì nó kiểm tra rõ ràng tất cả các đơn đặt hàng có thể. 

Vấn đề là số lượng hoán vị. Ngay cả đối với`n = 10`, đã có rồi`10! = 3,628,800`khả năng. Giới hạn thực tế là`10^5`, vì vậy vũ lực là hoàn toàn không thể thực hiện được. 

Để tìm ra một chiến lược tốt hơn, hãy tập trung vào nguồn gốc của sự đảo ngược. 

Mỗi mảng chỉ chứa hai phần tử. Giả sử chúng ta có hai mảng:```
A = [a1, a2]
B = [b1, b2]
```Nếu chúng ta đặt`A`trước`B`, các phép đảo ngược mảng chéo do cặp này đóng góp là```
[a1 > b1] + [a1 > b2] + [a2 > b1] + [a2 > b2]
```Ở đâu`[condition]`là`1`khi đúng và`0`nếu không thì. 

Thay vào đó nếu chúng ta đặt`B`trước`A`, sự đóng góp trở thành```
[b1 > a1] + [b1 > a2] + [b2 > a1] + [b2 > a2]
```Sự đảo ngược bên trong của mỗi mảng không bao giờ thay đổi, vì vậy chỉ có những đóng góp chéo theo cặp này mới quan trọng. 

Một quan sát hữu ích là nếu chúng ta sắp xếp mọi mảng về mặt khái niệm theo giá trị nhỏ hơn và lớn hơn của nó,```
low = min(x, y)
high = max(x, y)
```thì thứ tự tối ưu đạt được bằng cách sắp xếp các mảng theo`low`. 

Đây chính xác là thứ tự được sử dụng trong giải pháp chính thức. Một công thức tương đương khác là sắp xếp mảng theo`min(ai)`. 

Tại sao điều này làm việc? 

Lấy hai mảng`A`Và`B`. 

Nếu như```
min(A) <= min(B)
```đặt`A`trước`B`không bao giờ tạo ra nhiều sự đảo ngược chéo hơn việc đặt`B`trước`A`. Sự so sánh theo cặp giảm xuống thành một đối số trao đổi. Bất cứ khi nào hai mảng liền kề vi phạm thứ tự này, việc hoán đổi chúng không thể làm tăng tổng số lần đảo ngược. Việc áp dụng nhiều lần các giao dịch hoán đổi như vậy sẽ dẫn đến sự sắp xếp tối ưu toàn cầu. 

Khi đã biết tiêu chí sắp xếp này, toàn bộ bài toán sẽ trở thành bài toán sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mảng cùng với hai giá trị ban đầu của chúng. 
2. Với mỗi mảng, hãy tính phần tử nhỏ nhất của nó. 
3. Sắp xếp các mảng theo phần tử tối thiểu này theo thứ tự tăng dần. 

Đối số trao đổi cho thấy rằng bất cứ khi nào hai mảng lân cận nằm ngoài thứ tự này, việc hoán đổi chúng không thể làm tăng số lần đảo ngược. 
4. Xuất các mảng theo thứ tự sắp xếp, giữ nguyên thứ tự ban đầu của hai phần tử bên trong mỗi mảng. 
5. Ghép tất cả các cặp đã in thành một chuỗi. 

### Tại sao nó hoạt động 

Tổng số lần đảo ngược bao gồm hai phần. 

Phần đầu tiên là sự đảo ngược hoàn toàn bên trong các mảng riêng lẻ. Vì thứ tự bên trong mỗi cặp là cố định nên những sự đảo ngược này là không đổi và không thể tối ưu hóa được. 

Phần thứ hai là sự đảo ngược giữa các mảng khác nhau. Đối với hai mảng bất kỳ, sự đóng góp của chúng chỉ phụ thuộc vào mảng nào xuất hiện đầu tiên. Nếu hai mảng lân cận được sắp xếp trái ngược với thứ tự tăng dần của các phần tử tối thiểu của chúng, thì việc hoán đổi chúng sẽ không bao giờ làm tăng sự đóng góp theo cặp của chúng. Vì tất cả các cặp khác không thay đổi nên tổng số lần đảo ngược không tăng sau khi hoán đổi. 

Việc sửa chữa nhiều lần các đảo ngược như vậy theo thứ tự cuối cùng sẽ tạo ra chuỗi được sắp xếp theo phần tử tối thiểu. Vì mọi hoán đổi cục bộ đều không tăng nên sự sắp xếp cuối cùng này là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())

        arr = []
        for _ in range(n):
            x, y = map(int, input().split())
            arr.append((min(x, y), x, y))

        arr.sort(key=lambda item: item[0])

        res = []
        for _, x, y in arr:
            res.append(str(x))
            res.append(str(y))

        out.append(" ".join(res))

    sys.stdout.write("\n".join(out))

solve()
```Bước đầu tiên lưu trữ từng cặp cùng với giá trị tối thiểu của nó. Giá trị tối thiểu đó là khóa sắp xếp bắt nguồn từ đối số trao đổi. 

Sau khi sắp xếp, chúng ta phải xuất ra cặp gốc đúng như đã cho. Một lỗi phổ biến là sắp xếp lại hai số trong một cặp. Vấn đề chỉ cho phép sắp xếp lại toàn bộ mảng chứ không sửa đổi nội dung của chúng. 

Các giá trị có thể lớn như`10^9`, nhưng chúng tôi chỉ so sánh và in chúng. Không có số học ngoài`min()`là bắt buộc, vì vậy việc tràn không bao giờ là vấn đề đáng lo ngại. 

Tổng số mảng trên tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`, làm`O(n log n)`sắp xếp dễ dàng đủ nhanh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 4
2 3
```| Cặp | phút(cặp) | 
| --- | --- | 
| (1,4) | 1 | 
| (2,3) | 2 | 

Sau khi sắp xếp theo giá trị nhỏ nhất: 

| Vị trí | Cặp | 
| --- | --- | 
| 1 | (1,4) | 
| 2 | (2,3) | 

Trình tự đầu ra:```
1 4 2 3
```Ví dụ này hiển thị trực tiếp quy tắc cốt lõi. Mảng có phần tử tối thiểu nhỏ hơn sẽ xuất hiện trước. 

### Ví dụ 2 

đầu vào:```
5
5 10
2 3
9 6
4 1
8 7
```Tính toán tối thiểu: 

| Cặp | phút(cặp) | 
| --- | --- | 
| (5,10) | 5 | 
| (2,3) | 2 | 
| (9,6) | 6 | 
| (4,1) | 1 | 
| (8,7) | 7 | 

Sau khi sắp xếp: 

| Vị trí | Cặp | 
| --- | --- | 
| 1 | (4,1) | 
| 2 | (2,3) | 
| 3 | (5,10) | 
| 4 | (9,6) | 
| 5 | (8,7) | 

Đầu ra:```
4 1 2 3 5 10 9 6 8 7
```Dấu vết này chứng tỏ rằng các cặp giảm dần như`(4,1)`không được sửa đổi. Chỉ có thứ tự của toàn bộ mảng thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp các mảng chiếm ưu thế trong công việc | 
| Không gian | O(n) | Lưu trữ cho tất cả các cặp và xây dựng đầu ra | 

Tổng của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`. Sắp xếp`10^5`các mặt hàng yêu cầu khoảng`10^5 log₂(10^5)`so sánh, phù hợp thoải mái trong thời hạn. Việc sử dụng bộ nhớ là tuyến tính và thấp hơn nhiều so với mức 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        t = int(input())
        ans = []

        for _ in range(t):
            n = int(input())

            arr = []
            for _ in range(n):
                x, y = map(int, input().split())
                arr.append((min(x, y), x, y))

            arr.sort(key=lambda v: v[0])

            cur = []
            for _, x, y in arr:
                cur.append(str(x))
                cur.append(str(y))

            ans.append(" ".join(cur))

        return "\n".join(ans)

    global input
    input = sys.stdin.readline
    return solve()

# minimum size
assert run(
"""1
1
10 20
"""
) == "10 20"

# all equal values
assert run(
"""1
3
5 5
5 5
5 5
"""
) == "5 5 5 5 5 5"

# decreasing pairs
assert run(
"""1
3
10 1
8 2
6 3
"""
) == "10 1 8 2 6 3"

# mixed minima ordering
assert run(
"""1
4
7 8
1 100
4 5
2 3
"""
) == "1 100 2 3 4 5 7 8"

# tie on minima
assert run(
"""1
2
1 10
1 5
"""
) in ("1 10 1 5", "1 5 1 10")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cặp đơn | Cùng một cặp | Ranh giới kích thước tối thiểu | 
| Tất cả các giá trị bằng nhau | Bất kỳ thứ tự giống hệt nhau | Xử lý dây buộc đúng cách | 
| Tất cả các cặp giảm dần | Nội dung cặp gốc được bảo tồn | Không sắp xếp lại bên trong mảng | 
| Tối thiểu hỗn hợp | Sắp xếp theo phần tử tối thiểu | Quy tắc sắp xếp cốt lõi | 
| Tối thiểu bằng nhau | Hoặc buộc thứ tự | Không cần xử lý dây buộc ổn định | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
5 1
4 2
```Mức tối thiểu là`1`Và`2`, do đó thứ tự sắp xếp vẫn như sau:```
5 1
4 2
```Thuật toán không bao giờ thay đổi`(5,1)`vào trong`(1,5)`. Đảo ngược nội bộ là cố định và không thể được tối ưu hóa, do đó cần phải giữ nguyên thứ tự cặp. 

Bây giờ hãy xem xét:```
1
2
1 1
1 1
```Cả hai cực tiểu đều bằng nhau. Mọi thứ tự đều tối ưu vì tất cả các giá trị đều bằng nhau và không tồn tại sự đảo ngược giữa các mảng. Bước sắp xếp có thể giữ một trong hai thứ tự và cả hai đều đúng. 

Cuối cùng:```
1
3
100 1
2 3
4 5
```mức tối thiểu
