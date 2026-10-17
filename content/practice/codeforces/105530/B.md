---
title: "CF 105530B - MEX mô-đun"
description: "Chúng ta đang xem xét tất cả số dư được tạo ra bằng cách sửa một số $n$ và chia nó cho mọi số nguyên $i$ từ $1$ đến $n$. Điều này đưa ra một tập hợp các giá trị có dạng $n bmod i$."
date: "2026-06-23T22:58:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105530
codeforces_index: "B"
codeforces_contest_name: "Metropolitan University Inter University Programming Contest - Sylhet Division 2024"
rating: 0
weight: 105530
solve_time_s: 52
verified: true
draft: false
---

[CF 105530B - MEX mô-đun](https://codeforces.com/problemset/problem/105530/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem xét tất cả số dư được tạo ra bằng cách sửa một số$n$và chia nó cho mọi số nguyên$i$từ$1$ĐẾN$n$. Điều này đưa ra một tập hợp các giá trị có dạng$n \bmod i$. Mỗi cách chọn số chia sẽ tạo ra số dư giữa$0$Và$i - 1$, do đó, trên tất cả các ước số, chúng tôi thu thập một tập hợp các số nguyên có cấu trúc nhưng không tùy ý. 

Nhiệm vụ là xác định số nguyên không âm nhỏ nhất không bao giờ xuất hiện trong số phần dư này. Nói cách khác, chúng ta đang tính MEX của tập hợp$\{ n \bmod i \mid 1 \le i \le n \}$. 

Cấu trúc ràng buộc cực kỳ chặt chẽ. Vì đầu vào là một số nguyên nên mọi nghiệm lên tới$O(n)$hoặc thậm chí$O(\sqrt{n})$đủ nhanh và thách thức thực sự không phải là tính toán mà là nhận dạng mẫu trong tập hợp phần dư. 

Một cách tiếp cận ngây thơ sẽ tính toán rõ ràng tất cả các giá trị$n \bmod i$, chèn chúng vào một tập hợp, sau đó quét lên từ 0 cho đến khi tìm thấy số nguyên còn thiếu đầu tiên. Điều này hoạt động một cách máy móc, nhưng nó che giấu cấu trúc và có thể đánh lừa trực giác về những giá trị thực sự có thể tiếp cận được. 

Một cạm bẫy tinh vi xuất hiện khi suy luận xem liệu các giá trị lớn có thể xuất hiện dưới dạng số dư hay không. Ví dụ, người ta có thể giả định không chính xác rằng các giá trị gần bằng$n$có thể xuất hiện do mô đun thay đổi theo$i$, nhưng trên thực tế, mọi số dư đều nhỏ hơn số chia của nó, điều này hạn chế giá trị tối đa có thể có theo nghĩa tổng thể. 

Ví dụ, khi$n = 5$, tập hợp là:$5 \bmod 1 = 0$,$5 \bmod 2 = 1$,$5 \bmod 3 = 2$,$5 \bmod 4 = 1$,$5 \bmod 5 = 0$, 

vậy MEX là$3$. 

Câu trả lời đúng hóa ra chỉ phụ thuộc vào số lượng số nguyên nhỏ được đảm bảo xuất hiện chứ không phụ thuộc vào sự phân bố chính xác của tất cả các số dư. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp tính toán mọi phần còn lại$n \bmod i$vì$i$từ$1$ĐẾN$n$. Điều này tạo ra$n$các giá trị, sau đó chúng tôi chèn chúng vào một mảng hoặc tập hợp boolean và tìm kiếm tuyến tính số nguyên còn thiếu nhỏ nhất. Điều này đúng vì nó xây dựng một cách thấu đáo bộ mục tiêu. 

Chi phí của phương pháp này là tuyến tính trong$n$, cả để tạo phần dư và để quét. Đó nhiều nhất là về$2n$hoạt động, điều này tốt cho các ràng buộc điển hình nhưng không cung cấp cái nhìn sâu sắc về cấu trúc. 

Quan sát quan trọng là tập số dư có ngưỡng rõ ràng xung quanh$\lceil n/2 \rceil$. Mọi số nguyên nhỏ hơn ngưỡng này có thể được hiển thị dưới dạng số dư của một số ước số nào đó, trong khi bản thân ngưỡng đó không bao giờ được đảm bảo là có mặt. Điều này tạo ra một tiền tố liền kề của các giá trị có thể đạt được bắt đầu từ 0 và MEX chính xác là giá trị đầu tiên bên ngoài tiền tố này. 

Brute-force hoạt động vì nó trực tiếp liệt kê tất cả các phần dư, nhưng nó trở nên không cần thiết khi chúng ta nhận ra rằng các giá trị có thể tiếp cận tạo thành một khoảng liên tục. Vấn đề giảm xuống việc xác định độ dài của khoảng này thay vì xây dựng nó một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(n)$| Đã chấp nhận | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp tối ưu dựa trên việc nhận biết cấu trúc của phần dư có thể tiếp cận được. 

1. Tính toán$k = \lceil n/2 \rceil$. Giá trị này đóng vai trò như một ranh giới ngăn cách phần dư được đảm bảo với phần dư không thể. 
2. Trở về$k$như câu trả lời. Đây là số nguyên đầu tiên không thể được biểu diễn dưới dạng$n \bmod i$cho bất kỳ$i \le n$. 

Lý do bước này hợp lệ xuất phát từ cách hoạt động của việc giảm mô-đun: các giá trị nhỏ có thể được hình thành bằng cách chọn các ước số chỉ lớn hơn một chút so với phần còn lại mà chúng ta muốn, trong khi các giá trị ở hoặc trên ngưỡng không thể tồn tại trong ràng buộc$n \bmod i < i$cho tất cả$i$. 

### Tại sao nó hoạt động 

Với mọi số nguyên$m < \lceil n/2 \rceil$, chúng ta có thể xây dựng một ước số$i = n - m$. Sự lựa chọn này đảm bảo rằng$n \bmod i = m$, từ$n = 1 \cdot (n - m) + m$. điều kiện$i \le n$giữ tự động vì$m > 0$ngụ ý$i < n$, và cho$m = 0$, chúng ta có thể lấy$i = n$. 

Điều này cho thấy mọi giá trị dưới đây$\lceil n/2 \rceil$là có thể đạt được. Mặt khác, bất kỳ giá trị nào$m \ge \lceil n/2 \rceil$sẽ yêu cầu một số chia$i \le n - m \le n/2$, nhưng cách xây dựng như vậy không còn đảm bảo rằng số dư bằng$m$và trên thực tế, cấu trúc số học ngăn chặn việc bao phủ tất cả các giá trị lớn hơn một cách liên tiếp. Tập hợp có thể truy cập tạo thành tiền tố chính xác$[0, \lceil n/2 \rceil - 1]$, vậy MEX là$\lceil n/2 \rceil$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
print((n + 1) // 2)
```Toàn bộ giải pháp giảm xuống việc tính toán mức trần của một nửa$n$. biểu hiện$(n + 1) // 2$xử lý rõ ràng cả trường hợp chẵn và lẻ. Không cần có cấu trúc dữ liệu hoặc vòng lặp bổ sung vì cấu trúc có thể truy cập được xác định hoàn toàn bằng lý luận số học thay vì liệt kê. 

Một lỗi phổ biến ở đây là cố gắng mô phỏng phần dư hoặc xây dựng một tập hợp, điều này không cần thiết và có nguy cơ gây nhầm lẫn về giới hạn. Giải pháp chỉ phụ thuộc vào hành vi chia số nguyên. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 5$. 

Ta tính số dư: 

| tôi | n mod tôi | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 1 | 
| 5 | 0 | 

Bộ này là$\{0, 1, 2\}$. Số nguyên còn thiếu đầu tiên là$3$, phù hợp$\lceil 5/2 \rceil = 3$. 

Điều này xác nhận rằng tất cả các giá trị bên dưới ranh giới đều xuất hiện, còn bản thân ranh giới thì không. 

Bây giờ hãy xem xét$n = 8$. 

| tôi | n mod tôi | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 2 | 
| 4 | 0 | 
| 5 | 3 | 
| 6 | 2 | 
| 7 | 1 | 
| 8 | 0 | 

Tập hợp trở thành$\{0, 1, 2, 3\}$. MEX là$4$, khớp$\lceil 8/2 \rceil = 4$. 

Những ví dụ này cho thấy tiền tố có thể truy cập tăng chính xác lên tới một nửa$n$, và không có gì ngoài nó lấp đầy khoảng trống ngay sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ cần một phép tính số học duy nhất | 
| Không gian |$O(1)$| Không sử dụng bộ lưu trữ phụ | 

Giải pháp chạy trong thời gian không đổi bất kể kích thước đầu vào, thấp hơn nhiều so với giới hạn của bất kỳ môi trường lập trình cạnh tranh nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    print((n + 1) // 2)
    return sys.stdout.getvalue().strip()

# simple checks
assert run("1\n") == "1", "n=1"
assert run("2\n") == "1", "n=2"
assert run("5\n") == "3", "example"
assert run("8\n") == "4", "even case"
assert run("1000000000\n") == "500000000", "large case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | ranh giới tối thiểu | 
| 2 | 1 | trường hợp không tầm thường nhỏ nhất | 
| 5 | 3 | trường hợp lẻ tiêu chuẩn | 
| 8 | 4 | trường hợp chẵn tiêu chuẩn | 
| 1000000000 | 500000000 | ổn định đầu vào lớn | 

## Vỏ cạnh 

cho$n = 1$, số dư duy nhất là$0$, vậy MEX là$1$. Công thức$(n + 1) // 2$cho$1$, khớp trực tiếp. 

Vì$n = 2$, số dư là$0$Và$0$, vậy MEX là$1$. Công thức cho$1$, phù hợp với cấu trúc đã đặt. 

Đối với các giá trị lớn của$n$, không có bước lặp nào được thực hiện nên không có nguy cơ tràn hoặc suy giảm hiệu suất. Việc tính toán vẫn là một phép toán số học duy nhất bất kể độ lớn.
