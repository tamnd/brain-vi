---
title: "CF 104157A - Giấy in"
description: "Chúng tôi được cung cấp giới hạn nguồn cung hàng tuần và Michael được phép thực hiện chính xác một lần mua hàng. Hạn chế là số lượng anh ta mua phải là lũy thừa của hai. Trong số tất cả số tiền mua hợp lệ không vượt quá nguồn cung sẵn có, chúng ta cần chọn số tiền lớn nhất."
date: "2026-07-02T01:14:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104157
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 2 (Beginner)"
rating: 0
weight: 104157
solve_time_s: 57
verified: true
draft: false
---

[CF 104157A - Giấy in](https://codeforces.com/problemset/problem/104157/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp giới hạn nguồn cung hàng tuần và Michael được phép thực hiện chính xác một lần mua hàng. Hạn chế là số lượng anh ta mua phải là lũy thừa của hai. Trong số tất cả số tiền mua hợp lệ không vượt quá nguồn cung sẵn có, chúng ta cần chọn số tiền lớn nhất. 

Nói cách khác, chúng ta đang tìm số lớn nhất có dạng$2^k$sao cho nó không vượt quá$n$. đầu vào$n$đại diện cho số lượng giấy tờ tối đa có sẵn trong một tuần và đầu ra là kích thước gói có thể mua lớn nhất theo giới hạn lũy thừa hai. 

Ràng buộc$n \le 10^5$đủ nhỏ để thậm chí việc lặp qua tất cả lũy thừa của hai đến giới hạn này là chuyện nhỏ. Số lũy thừa của hai lên tới$10^5$được giới hạn bởi$\log_2(10^5) \approx 16.6$, do đó, ngay cả việc quét tuyến tính trên lũy thừa cũng là thời gian không đổi trong thực tế. 

Một sai lầm ngây thơ ở đây là hiểu sai nhiệm vụ là cần phân vùng$n$hoặc tối đa hóa sự kết hợp sức mạnh của cả hai. Ví dụ, đưa ra$n = 5$, người ta có thể nghĩ sai$4 + 1$được phép vì cả hai đều là lũy thừa của hai, nhưng vấn đề rõ ràng hạn chế Michael chỉ được mua một lần. Vì vậy, đầu ra phải là một số duy nhất, không phải là sự phân tách. 

Một trường hợp thất bại khó phát hiện khác là dừng ở lũy thừa bậc một của hai lớn hơn hoặc bằng$n$. Vì$n = 5$, lũy thừa tiếp theo của hai là$8$, không hợp lệ vì vượt quá giới hạn, nhưng việc triển khai bất cẩn làm tròn số sẽ trả về câu trả lời không hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê tất cả lũy thừa của hai bắt đầu từ 1 và theo dõi số lớn nhất không vượt quá$n$. Điều này hiệu quả vì lũy thừa của hai tạo thành một dãy tăng dần, do đó việc quét chúng theo thứ tự đảm bảo chúng ta không bỏ sót bất kỳ ứng cử viên nào. Vấn đề với cách tiếp cận này không phải là tính đúng đắn mà là tính tổng quát: nếu được triển khai không hiệu quả hoặc nếu phạm vi lớn hơn nhiều thì việc tạo ra năng lượng nhiều lần có thể trở thành chi phí không cần thiết. 

Tuy nhiên, cấu trúc của bài toán khiến cho việc tối ưu hóa trở nên ngay lập tức. Mỗi số trong chuỗi này nhân đôi số trước đó, vì vậy chúng ta chỉ cần nhân với 2 cho đến khi giá trị tiếp theo vượt quá$n$. Giá trị hợp lệ cuối cùng là câu trả lời. Điều này làm giảm vấn đề xuống một vòng lặp duy nhất có độ dài logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét sức mạnh tuần tự) | O(log n) | O(1) | Đã chấp nhận | 
| Tối ưu (lặp đi lặp lại nhân đôi) | O(log n) | O(1) | Đã chấp nhận | 

Trong thực tế, cả hai đều tương đương ở đây, nhưng phiên bản nhân đôi lặp lại là phiên bản sạch nhất và ít xảy ra lỗi nhất. 

## Hướng dẫn thuật toán 

1. Bắt đầu từ lũy thừa hợp lệ nhỏ nhất của hai, là 1. Điều này luôn an toàn vì ràng buộc đầu vào tối thiểu đảm bảo$n \ge 1$, vậy tồn tại ít nhất một câu trả lời hợp lệ. 
2. Tính lũy thừa tiếp theo của 2 bằng cách nhân đôi giá trị hiện tại. Mỗi bước khám phá ứng cử viên tiếp theo theo thứ tự tăng dần nghiêm ngặt, đảm bảo chúng tôi không bao giờ bỏ qua một khả năng hợp lệ. 
3. Dừng lại khi nhân đôi sẽ tạo ra giá trị lớn hơn$n$. Tại thời điểm đó, giá trị hiện tại là lũy thừa hợp lệ lớn nhất của hai phù hợp với ràng buộc. 
4. Xuất giá trị hợp lệ cuối cùng đạt được trước khi vượt quá$n$. 

Ý tưởng chính là lũy thừa của hai tạo thành một chuỗi đơn điệu, do đó, giá trị tối ưu có thể được tìm thấy bằng cách thực hiện chuỗi này cho đến khi nó phá vỡ tính khả thi. 

### Tại sao nó hoạt động 

Ở mỗi bước, chúng tôi duy trì bất biến rằng giá trị hiện tại là lũy thừa lớn nhất của hai không vượt quá ngưỡng được kiểm tra gần đây nhất. Vì chuỗi lũy thừa của hai tăng dần và không bị gián đoạn nên giá trị đầu tiên vượt quá$n$ngay lập tức ngụ ý giá trị trước đó là ứng cử viên khả thi tối đa. Không có ứng cử viên thay thế nào giữa hai lũy thừa liên tiếp của hai, vì vậy không có giải pháp hợp lệ nào có thể bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    x = 1
    while x * 2 <= n:
        x *= 2
    print(x)

if __name__ == "__main__":
    solve()
```Giải pháp đọc giá trị đầu vào, sau đó lặp lại nhân đôi một biến đang chạy bắt đầu từ 1. Điều kiện vòng lặp`x * 2 <= n`đảm bảo chúng ta chỉ tiến lên trong khi lũy thừa tiếp theo của 2 vẫn có hiệu lực. Một khi điều kiện không thành công,`x`được đảm bảo là lũy thừa có giá trị lớn nhất của hai. 

Một cạm bẫy triển khai phổ biến là sử dụng`x <= n`bên trong điều kiện vòng lặp trong khi cập nhật trước, điều này có thể vô tình vượt quá và yêu cầu quay lui. Điều kiện được chọn sẽ tránh hoàn toàn điều đó bằng cách kiểm tra trước khi cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```| Bước | x | x * 2 <= n | 
| --- | --- | --- | 
| 1 | 1 | Đúng | 
| 2 | 2 | Đúng | 
| 3 | 4 | Đúng | 
| 4 | 8 | Sai | 

Vòng lặp dừng khi nhân đôi 4 sẽ tạo ra 8, vượt quá 5. Giá trị hợp lệ cuối cùng là 4, do đó đầu ra là 4. Điều này xác nhận thuật toán chọn đúng lũy ​​thừa lớn nhất của hai không vượt quá giới hạn. 

### Ví dụ 2 

đầu vào:```
1
```| Bước | x | x * 2 <= n | 
| --- | --- | --- | 
| 1 | 1 | Sai | 

Vòng lặp không bao giờ chạy vì thậm chí tăng gấp đôi 1 cũng sẽ vượt quá giới hạn. Câu trả lời vẫn là 1, điều này đúng vì nó là lũy thừa duy nhất của 2 trong ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi lần lặp lại nhân đôi giá trị nên số bước tỷ lệ thuận với số bit trong n | 
| Không gian | O(1) | Chỉ có một biến số nguyên duy nhất được duy trì | 

Ràng buộc$n \le 10^5$đảm bảo tối đa khoảng 17 lần lặp, đó là thời gian thực sự không đổi trong thực tế và dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    x = 1
    while x * 2 <= n:
        x *= 2
    return str(x)

# provided samples
assert run("5\n") == "4", "sample 1"
assert run("1\n") == "1", "sample 2"

# custom cases
assert run("2\n") == "2", "exact power of two"
assert run("3\n") == "2", "just above power of two"
assert run("16\n") == "16", "exact upper power"
assert run("17\n") == "16", "just above upper power"
assert run("100000\n") == "65536", "large boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 2 | sức mạnh chính xác của hai lần xử lý | 
| 3 | 2 | hành vi sàn đúng giữa các quyền lực | 
| 16 | 16 | khớp chính xác ranh giới | 
| 17 | 16 | chuyển tiếp qua ranh giới quyền lực | 
| 100000 | 65536 | ràng buộc trên đúng đắn | 

## Vỏ cạnh 

cho$n = 1$, thuật toán bắt đầu tại$x = 1$và ngay lập tức thất bại trong điều kiện$x \cdot 2 \le n$, do đó nó trả về 1 mà không cần vào vòng lặp. Điều này xác nhận tính đúng đắn ở ranh giới nhỏ nhất. 

Đối với các giá trị ngay trên lũy thừa hai, chẳng hạn như$n = 33$, chuỗi đi 1, 2, 4, 8, 16, 32 và dừng trước 64. Thuật toán tự nhiên trả về 32, chứng tỏ rằng nó luôn chọn công suất khả thi lớn nhất mà không vượt quá mức.
