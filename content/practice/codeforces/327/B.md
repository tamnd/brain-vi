---
title: "CF 327B - Chuỗi Đói"
description: "Chúng ta cần xây dựng một dãy tăng dần gồm n số nguyên dương sao cho không có phần tử nào sau đó có thể chia hết cho bất kỳ phần tử nào trước đó. Đầu vào chứa một số duy nhất n. Chúng ta phải xuất ra bất kỳ chuỗi nào có độ dài n thỏa mãn hai điều kiện."
date: "2026-06-06T09:05:58+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 327
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 191 (Div. 2)"
rating: 1200
weight: 327
solve_time_s: 103
verified: false
draft: false
---

[CF 327B - Trình tự đói](https://codeforces.com/problemset/problem/327/B) 

**Đánh giá:** 1200 
**Thẻ:** toán 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một dãy tăng dần của`n`các số nguyên dương sao cho không có phần tử nào sau đó có thể chia hết cho bất kỳ phần tử nào trước đó. 

Đầu vào chứa một số duy nhất`n`. Chúng ta phải xuất ra bất kỳ chuỗi độ dài nào`n`thỏa mãn hai điều kiện. Đầu tiên, trình tự phải tăng một cách chặt chẽ. Thứ hai, với mỗi cặp vị trí`i < j`, giá trị tại vị trí`j`không được là bội số của giá trị tại vị trí`i`. 

Các ràng buộc rất rộng rãi từ góc độ toán học nhưng lại hạn chế từ góc độ triển khai. Chúng tôi cần tới`100000`số và mỗi số phải có nhiều nhất`10^7`. Với`n = 100000`, bất kỳ thuật toán nào kiểm tra rõ ràng tất cả các cặp sẽ yêu cầu khoảng$$\frac{100000 \cdot 99999}{2} \approx 5 \cdot 10^9$$so sánh, điều này hoàn toàn không thể xảy ra trong vòng một giây. Chúng tôi cần một công trình trực tiếp. 

Nhận xét quan trọng nhất là bài toán không yêu cầu trình tự nhỏ nhất về mặt từ điển hoặc bất kỳ trình tự tối ưu nào. Bất kỳ công trình hợp lệ nào đều được chấp nhận. 

Một lỗi phổ biến là sử dụng các số nguyên liên tiếp bắt đầu từ 1. 

Ví dụ:```
n = 3
```Sự liên tiếp:```
1 2 3
```không hợp lệ vì mọi số đều chia hết cho 1. 

Một ý tưởng hấp dẫn khác là sử dụng các số nguyên liên tiếp bắt đầu từ 2:```
2 3 4
```Điều này vẫn không hợp lệ vì`4`chia hết cho`2`. 

Một trình tự đúng cho`n = 3`có thể là:```
2 3 5
```Không có phần tử nào chia cắt phần tử nào sau này. 

Một vấn đề tế nhị khác là giới hạn trên`10^7`. Công trình tạo ra số lượng rất lớn có thể thỏa mãn điều kiện chia hết nhưng lại vi phạm phạm vi cho phép. 

Ví dụ: lũy thừa của hai:```
2, 4, 8, 16, ...
```đã không đạt được điều kiện phân chia, và thậm chí nếu được sửa đổi, các công trình xây dựng đang phát triển nhanh chóng có nguy cơ vượt quá`10^7`. 

Chúng ta cần một công trình đồng thời đảm bảo trật tự ngày càng tăng, tránh sự chia cắt và nằm trong giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xây dựng chuỗi từng số một. Đối với mỗi giá trị ứng cử viên, chúng tôi sẽ kiểm tra xem nó có chia hết cho bất kỳ số nào đã chọn trước đó hay không. Nếu nó vượt qua tất cả các bước kiểm tra, chúng tôi sẽ thêm nó vào chuỗi. 

Phương pháp này đúng vì nó xác minh rõ ràng thuộc tính bắt buộc. Vấn đề là hiệu quả. Mỗi ứng viên mới có thể cần phải được kiểm tra dựa trên nhiều yếu tố trước đó. Với`n = 100000`, ngay cả một phiên bản được tối ưu hóa cũng thực hiện quá nhiều lần kiểm tra khả năng chia hết. 

Quan sát quan trọng là các số nguyên tố tự động thỏa mãn điều kiện. 

Giả sử chúng ta chọn các số nguyên tố phân biệt theo thứ tự tăng dần:```
p1 < p2 < p3 < ...
```Lấy bất kỳ cặp nào`i < j`. Từ`pj`là số nguyên tố và lớn hơn`pi`, ước số dương duy nhất của`pj`là`1`Và`pj`chính nó. Từ`pi`không phải là những giá trị đó,`pj`không thể chia hết cho`pi`. 

Vì vậy, bất kỳ danh sách tăng dần nào của các số nguyên tố riêng biệt đều tự động là một dãy đói. 

Vấn đề bây giờ trở thành việc tìm`n`số nguyên tố không vượt quá`10^7`. 

Một thực tế nổi tiếng là có nhiều hơn`100000`số nguyên tố dưới đây`10^7`. Trên thực tế, số nguyên tố thứ 100000 là`1299709`, thoải mái dưới mức giới hạn. Do đó tạo ra cái đầu tiên`100000`số nguyên tố là đủ. 

Công cụ tiêu chuẩn là Sàng Eratosthenes. Chạy một cái sàng lên đến khoảng`1.3 × 10^6`dễ dàng tạo ra ít nhất`100000`số nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu (Sàng + n số nguyên tố đầu tiên) | O(M log log M) | O(M) | Đã chấp nhận | 

Đây`M`là giới hạn sàng, khoảng`1.3 × 10^6`. 

## Hướng dẫn thuật toán 

1. Đọc`n`. 
2. Chọn giới hạn sàng đủ lớn để chứa ít nhất`100000`số nguyên tố. Một giới hạn của`1300000`là đủ. 
3. Chạy Sàng Eratosthenes đến giới hạn đó. 
4. Thu thập tất cả các số nguyên tố theo thứ tự tăng dần. 
5. Xuất đầu tiên`n`số nguyên tố. 

Lý do điều này có hiệu quả là vì các số nguyên tố đã tăng lên một cách nghiêm ngặt khi được thu thập từ sàng và không có số nguyên tố nào lớn hơn số nguyên tố khác có thể chia hết cho nó. 

### Tại sao nó hoạt động 

Sàng tạo ra các số nguyên tố theo thứ tự tăng dần nên dãy tăng dần. 

Xem xét hai phần tử được chọn bất kỳ`pi < pj`. Từ`pj`là số nguyên tố nên ước số dương duy nhất của nó là`1`Và`pj`. số`pi`không phải là các ước số đó vì nó là số nguyên tố nhỏ hơn. Kể từ đây`pi`không chia`pj`. 

Vì đối số này đúng cho mọi cặp chỉ số nên chuỗi được tạo thỏa mãn điều kiện chuỗi đói. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    LIMIT = 1300000

    is_prime = [True] * (LIMIT + 1)
    is_prime[0] = is_prime[1] = False

    p = 2
    while p * p <= LIMIT:
        if is_prime[p]:
            for multiple in range(p * p, LIMIT + 1, p):
                is_prime[multiple] = False
        p += 1

    primes = []
    for i in range(2, LIMIT + 1):
        if is_prime[i]:
            primes.append(i)
            if len(primes) == n:
                break

    print(*primes)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc độ dài chuỗi yêu cầu. 

Sàng đánh dấu các số tổng hợp. Đối với mỗi số nguyên tố`p`, mọi bội số bắt đầu từ`p²`được đánh dấu là không nguyên tố. Bắt đầu lúc`p²`tránh được công việc dư thừa vì bội số nhỏ hơn đã được xử lý bởi các số nguyên tố nhỏ hơn. 

Sau khi sàng xong, chúng ta quét các số theo thứ tự tăng dần và thu được các số nguyên tố. Một khi chúng tôi đã tập hợp được`n`số nguyên tố, chúng ta dừng ngay lập tức thay vì quét phần còn lại của mảng. 

Đầu ra bao gồm đầu tiên`n`số nguyên tố. Thứ tự tăng dần của chúng xuất phát trực tiếp từ thứ tự quét. 

Một chi tiết triển khai tinh tế là giới hạn sàng. Chúng ta cần đủ số nguyên tố cho trường hợp xấu nhất`n = 100000`. Một giới hạn của`1300000`an toàn chứa hơn`100000`số nguyên tố nên vòng lặp thu thập luôn thành công. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Số nguyên tố thu thập được: 

| Bước | Thủ tướng tìm thấy | Trình tự | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 3 | 2 3 | 
| 3 | 5 | 2 3 5 | 

Đầu ra:```
2 3 5
```Kiểm tra điều kiện:`3`không chia hết cho`2`.`5`không chia hết cho`2`.`5`không chia hết cho`3`. 

Trình tự là hợp lệ. 

### Ví dụ 2 

đầu vào:```
5
```Số nguyên tố thu thập được: 

| Bước | Thủ tướng tìm thấy | Trình tự | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 3 | 2 3 | 
| 3 | 5 | 2 3 5 | 
| 4 | 7 | 2 3 5 7 | 
| 5 | 11 | 2 3 5 7 11 | 

Đầu ra:```
2 3 5 7 11
```Ví dụ này minh họa bất biến trung tâm: mọi số được chọn đều là số nguyên tố, do đó không có số nguyên tố nào được chọn trước đó có thể chia hết cho số sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log log M) | Sàng Eratosthenes | 
| Không gian | O(M) | Mảng sàng Boolean | 

Đây`M = 1300000`. 

Bộ sàng xử lý khoảng 1,3 triệu số, có thể quản lý dễ dàng trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ so với 256 MB có sẵn. Giải pháp thoải mái phù hợp với các hạn chế. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())

    LIMIT = 1300000
    is_prime = [True] * (LIMIT + 1)
    is_prime[0] = is_prime[1] = False

    p = 2
    while p * p <= LIMIT:
        if is_prime[p]:
            for multiple in range(p * p, LIMIT + 1, p):
                is_prime[multiple] = False
        p += 1

    ans = []
    for i in range(2, LIMIT + 1):
        if is_prime[i]:
            ans.append(str(i))
            if len(ans) == n:
                break

    return " ".join(ans)

# provided sample
assert run("3\n") == "2 3 5"

# minimum size
assert run("1\n") == "2"

# small case
assert run("2\n") == "2 3"

# another small case
assert run("4\n") == "2 3 5 7"

# larger sanity check
out = run("10\n").split()
assert len(out) == 10
assert out[0] == "2"
assert out[-1] == "29"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`2`| Kích thước tối thiểu | 
|`2`|`2 3`| Chuỗi không tầm thường nhỏ nhất | 
|`3`|`2 3 5`| Xây dựng cỡ mẫu | 
|`4`|`2 3 5 7`| Số nguyên tố tăng vẫn còn giá trị | 
|`10`| 10 số nguyên tố đầu tiên | Kiểm tra tính chính xác lớn hơn | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
1
```đầu ra của thuật toán:```
2
```Một chuỗi có độ dài không có cặp nào để kiểm tra nên nó tự động bị đói. 

Đối với một đầu vào nhỏ như```
2
```đầu ra của thuật toán:```
2 3
```Trình tự ngày càng tăng và`3 % 2 != 0`, do đó điều kiện được giữ. 

Để có đầu vào tối đa```
100000
```thuật toán chỉ đơn giản lấy cái đầu tiên`100000`số nguyên tố từ sàng. Số nguyên tố được chọn cuối cùng vẫn ở bên dưới`10^7`, thỏa mãn ràng buộc giá trị. Không có cặp nào vi phạm điều kiện chia hết vì tất cả các số được chọn đều là số nguyên tố phân biệt. 

Một trường hợp thất bại phổ biến đối với các giải pháp không chính xác là sử dụng các số nguyên liên tiếp:```
2 3 4
```Ở đâu`4`chia hết cho`2`. 

Cách xây dựng của chúng tôi hoàn toàn tránh được điều này vì mọi phần tử đều là số nguyên tố và không có số nguyên tố nào lớn hơn số nguyên tố khác có thể chia hết cho nó.
