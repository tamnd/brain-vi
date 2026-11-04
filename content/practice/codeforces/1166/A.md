---
title: "CF 1166A - Lớp học im lặng"
description: "Chúng tôi được cung cấp danh sách tên học sinh và chúng tôi được phép chia những học sinh này thành hai lớp. Mỗi học sinh phải đến đúng một trong hai phòng và một trong hai phòng có thể trống."
date: "2026-06-13T08:52:52+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1166
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 561 (Div. 2)"
rating: 900
weight: 1166
solve_time_s: 103
verified: true
draft: false
---

[CF 1166A - Lớp học im lặng](https://codeforces.com/problemset/problem/1166/A) 

**Xếp hạng:** 900 
**Tags:** tổ hợp, tham lam 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp danh sách tên học sinh và chúng tôi được phép chia những học sinh này thành hai lớp. Mỗi học sinh phải đến đúng một trong hai phòng và một trong hai phòng có thể trống. 

Một cặp học sinh sẽ trở nên “hay nói chuyện” nếu họ có cùng chữ cái bắt đầu và được xếp vào cùng một phòng. Đối với mỗi phòng, chúng tôi đếm xem có bao nhiêu cặp như vậy tồn tại trong số các học sinh trong đó và chúng tôi tính tổng số tiền đóng góp của cả hai phòng. Sự đóng góp của một phòng được xác định hoàn toàn bằng số lượng học sinh có cùng chữ cái đầu tiên trong đó, vì chỉ những học sinh đó mới có thể tạo thành các cặp hợp lệ. 

Nhiệm vụ là chọn cách phân chia sao cho tổng số cặp ban đầu giống nhau ở cả hai phòng càng nhỏ càng tốt. 

Các hạn chế là nhỏ, tối đa là 100 sinh viên. Điều này ngay lập tức gợi ý rằng ngay cả một giải pháp bậc hai hoặc hơi tổ hợp cũng có thể vượt qua dễ dàng, nhưng cấu trúc của bài toán cho phép một cách tiếp cận tính tham lam thậm chí còn đơn giản hơn. 

Một sai lầm ngây thơ là thử phân công từng học sinh mà không nhận ra rằng điều đó chỉ được tính cho mỗi chữ cái đầu tiên. Một cạm bẫy phổ biến khác là giả định rằng việc trộn các chữ cái giữa các phòng sẽ tương tác với nhau, trong khi trên thực tế, mỗi chữ cái đều góp phần độc lập vào câu trả lời cuối cùng. 

Ví dụ: nếu tất cả các tên bắt đầu bằng các chữ cái riêng biệt thì bất kỳ sự phân tách nào cũng sẽ cho ra 0 cặp. Nếu tất cả các tên bắt đầu bằng cùng một chữ cái thì quyết định duy nhất là làm thế nào để phân chia đồng đều nhiều phần đó giữa hai phòng. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử tất cả các bài tập có thể có của n học sinh thành hai nhóm. Mỗi học sinh có hai lựa chọn nên có thể có 2^n cách chia. Đối với mỗi phần chia, chúng tôi sẽ tính toán số cặp trong cả hai phòng bằng cách nhóm học sinh theo chữ cái đầu tiên và tổ hợp tổng. Điều này đúng nhưng tăng theo cấp số nhân và với n lên tới 100 thì điều đó hoàn toàn không khả thi. 

Quan sát quan trọng là sự tương tác không xảy ra giữa các chữ cái khác nhau. Nếu chúng ta sửa một chữ cái như 'a' và đếm xem có bao nhiêu học sinh bắt đầu bằng 'a', thì nhóm đó sẽ đóng góp độc lập với tất cả các chữ cái khác. Tổng đáp án chỉ là tổng đóng góp của mỗi chữ cái. 

Vì vậy, vấn đề rút gọn thành thế này: với mỗi chữ cái, giả sử chúng ta có c học sinh bắt đầu bằng chữ cái đó. Chúng ta phải chia chúng thành hai nhóm có kích thước a và c - a. Số cặp trò chuyện được đóng góp bởi bức thư này là C(a, 2) + C(c - a, 2). Chúng tôi muốn chọn a để giảm thiểu biểu thức này. 

Cấu trúc của hệ số nhị thức làm cho bài toán này trở thành một bài toán tối thiểu hóa lồi trên các số nguyên. Tổng được giảm thiểu khi hai nhóm cân bằng nhất có thể. Theo trực giác, việc xếp học sinh thành hai nửa gần bằng nhau sẽ làm giảm số lượng các cặp bên trong, bởi vì các cặp tăng trưởng bậc hai theo quy mô nhóm. 

Do đó, đối với mỗi chữ cái, chúng ta chỉ cần chia số đếm thành sàn (c/2) và trần (c/2), tính toán đóng góp theo cặp và tính tổng trên tất cả các chữ cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(26 + n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng tôi đếm có bao nhiêu học sinh bắt đầu với mỗi chữ cái trong bảng chữ cái. Điều này biến đổi đầu vào thành tối đa 26 giá trị tần số độc lập. 

Đối với mỗi tần số chữ cái c, chúng tôi quyết định cách phân chia nó cho hai lớp học. Cách phân chia tối ưu là làm cho hai phần bằng nhau nhất có thể vì bất kỳ sự mất cân bằng nào cũng làm tăng số lượng cặp bên trong theo phương trình bậc hai.

1. Xây dựng một mảng tần số freq có kích thước 26 trong đó freq[i] lưu trữ bao nhiêu tên bắt đầu bằng chữ cái thứ i. Điều này cô lập các vấn đề con độc lập trên mỗi chữ cái. 
2. Khởi tạo câu trả lời thành 0, vì chúng tôi sẽ tích lũy đóng góp từ mỗi chữ cái riêng biệt. 
3. Với mỗi tần số c trong tần số, tính xem có bao nhiêu học sinh vào phòng thứ nhất là a = c // 2 và phòng thứ hai là b = c - a. Điều này đảm bảo sự phân chia được cân bằng nhất có thể. 
4. Thêm C(a, 2) + C(b, 2) vào câu trả lời, trong đó C(x, 2) = x(x - 1) // 2. Việc này tính các cặp trong mỗi phòng cho chữ cái đó. 
5. Xuất ra câu trả lời tích lũy cuối cùng sau khi xử lý tất cả các chữ cái. 

### Tại sao nó hoạt động 

Mỗi chữ cái tạo thành một tập hợp độc lập gồm các mục giống hệt nhau và các cặp chỉ được hình thành trong các nhóm chữ cái giống hệt nhau. Vì không có sự tương tác giữa các chữ cái nên việc giảm thiểu tổng số tương đương với việc giảm thiểu sự đóng góp của từng chữ cái riêng biệt. Đối với số c cố định, hàm C(x,2) + C(c-x,2) được giảm thiểu khi x càng gần c/2 càng tốt do tính đối xứng và tăng trưởng lồi của số hạng bậc hai. Điều này đảm bảo rằng sự phân chia cân bằng tham lam tạo ra mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def comb2(x):
    return x * (x - 1) // 2

def solve():
    n = int(input().strip())
    freq = [0] * 26

    for _ in range(n):
        s = input().strip()
        freq[ord(s[0]) - ord('a')] += 1

    ans = 0
    for c in freq:
        a = c // 2
        b = c - a
        ans += comb2(a) + comb2(b)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén dữ liệu đầu vào thành tần số chữ cái, đây là thông tin duy nhất quan trọng đối với việc hình thành cặp. Chức năng trợ giúp`comb2`tính toán số lượng các cặp không có thứ tự trong một nhóm một cách hiệu quả mà không cần vòng lặp. 

Bước chia tách`a = c // 2`Và`b = c - a`thực thi sự cân bằng tối ưu. Không cần phải xem xét các phân phối thay thế vì bất kỳ độ lệch nào cũng làm tăng một bên nhiều hơn làm giảm bên kia và sự tăng trưởng bậc hai của số lượng cặp đảm bảo điều này là dưới mức tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
jorge
jose
oscar
jerry
```Tần số là: 

- j: 3 
- ồ: 1 
- những người khác: 0 

Đối với chữ 'j', c = 3 cho ra a = 1, b = 2. 

| Thư | c | một | b | C(a,2) | C(b,2) | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| j | 3 | 1 | 2 | 0 | 1 | 1 | 

Tổng số câu trả lời là 1. 

Điều này cho thấy rằng chỉ có chữ cái đầu tiên được lặp lại nhiều mới góp phần tạo nên kết quả cuối cùng. 

### Ví dụ 2 

đầu vào:```
6
aa
ab
ac
ad
ae
af
```Tần số: 

- một: 6 

Chia cho ta a = 3, b = 3. 

| Thư | c | một | b | C(a,2) | C(b,2) | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| một | 6 | 3 | 3 | 3 | 3 | 6 | 

Ở đây kết quả được xác định hoàn toàn bằng cách cân bằng việc chia đều. 

Điều này chứng tỏ rằng ngay cả khi tất cả học sinh có chung tên ban đầu, chiến lược tối ưu vẫn là phân vùng đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 26) | Một lượt đếm tên, một lượt đếm bảng chữ cái | 
| Không gian | O(26) | Mảng tần số cố định | 

Thời gian chạy tuyến tính theo số lượng học sinh và không phụ thuộc vào bất kỳ phép liệt kê tổ hợp nào, phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input().strip())
    freq = [0] * 26
    for _ in range(n):
        s = input().strip()
        freq[ord(s[0]) - ord('a')] += 1

    ans = 0
    for c in freq:
        a = c // 2
        b = c - a
        ans += a * (a - 1) // 2 + b * (b - 1) // 2

    print(ans)

# provided sample
assert run("""4
jorge
jose
oscar
jerry
""") == "1"

# all distinct
assert run("""3
alice
bob
carol
""") == "0"

# all same letter even
assert run("""4
aa
ab
ac
ad
""") == "2"

# all same letter odd
assert run("""5
aa
ab
ac
ad
ae
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các tên viết tắt riêng biệt | 0 | không có cặp nào hình thành | 
| cùng một lá thư thậm chí chia | 2 | hành vi phân vùng cân bằng | 
| cùng một chữ chia lẻ | 4 | xử lý sàn/trần đúng cách | 

## Vỏ cạnh 

Đối với một học sinh, mảng tần số có một mục nhập khác 0 là 1. Phép chia trở thành 1 và 0, đồng thời cả C(1,2) và C(0,2) đều bằng 0, do đó đầu ra là 0. Thuật toán xử lý vấn đề này một cách trực tiếp vì phép chia số nguyên tạo ra a = 0 và b = 1 hoặc ngược lại tùy theo thứ tự và cả hai trường hợp đều cho kết quả bằng 0. 

Khi tất cả học sinh có cùng một giá trị ban đầu, chẳng hạn như c = 100, thuật toán luôn chia thành 50 và 50. Giá trị được tính toán trở thành C(50,2) + C(50,2), khớp chính xác với phân bố tối thiểu có thể có. Bất kỳ sự phân chia không đồng đều nào sẽ làm tăng một số hạng bậc hai nhiều hơn so với việc giảm số hạng kia, do đó trường hợp cân bằng vẫn tối ưu theo quy tắc được triển khai.
