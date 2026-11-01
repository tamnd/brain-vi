---
title: "CF 106443J - Hành Trình Của Nho"
description: "Chúng ta được sắp xếp theo vòng tròn gồm N đỉnh, được gắn nhãn từ 0 đến N − 1 và kích thước bước nhảy cố định S. Bắt đầu từ đỉnh 0, chúng ta liên tục di chuyển về phía trước theo đúng S vị trí modulo N, tạo thành một bước đi xác định vô hạn trên chu trình này."
date: "2026-06-19T17:44:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "J"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 48
verified: true
draft: false
---

[CF 106443J - Hành trình hái nho](https://codeforces.com/problemset/problem/106443/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp theo vòng tròn gồm N đỉnh, được gắn nhãn từ 0 đến N − 1 và kích thước bước nhảy cố định S. Bắt đầu từ đỉnh 0, chúng ta liên tục di chuyển về phía trước theo đúng S vị trí modulo N, tạo thành một bước đi xác định vô hạn trên chu trình này. 

Nhiệm vụ là xác định có bao nhiêu đỉnh riêng biệt xuất hiện trước khi bước đi bắt đầu lặp lại. Nói cách khác, chúng tôi muốn kích thước của tập hợp các trạng thái có thể tiếp cận được trong cấp số cộng mô-đun này. 

Các ràng buộc cho phép N và S lên tới 10^9, do đó, bất kỳ giải pháp nào mô phỏng quy trình từng bước đều không khả thi ngay lập tức. Một mô phỏng đơn giản có thể mất tới N bước trước khi lặp lại, quá lớn so với giới hạn 1 giây. Điều này đẩy chúng ta tới cách giải thích theo lý thuyết số hơn là việc duyệt đồ thị. 

Trường hợp cạnh tinh tế xuất hiện khi S lớn hoặc bằng N. Ví dụ: nếu N = 10 và S = 10, đỉnh tiếp theo luôn là (x + 10) mod 10 = x, vì vậy chúng ta không bao giờ rời khỏi đỉnh 0. Câu trả lời đúng là 1. Một cách triển khai đơn giản giả định ít nhất một nước đi thay đổi vị trí sẽ bị tính quá mức không chính xác ở đây nếu không cẩn thận với hành vi modulo. 

Một trường hợp cạnh khác là khi S = 0 không được các ràng buộc cho phép, nhưng S = 1 tạo ra sự truyền tải đầy đủ tất cả các đỉnh trước khi lặp lại. Điều này giúp neo giữ trực giác ở những thái cực. 

## Phương pháp tiếp cận 

Quy tắc chuyển động xác định hàm xác định trên các dư lượng modulo N: x → x + S mod N. Bắt đầu từ 0, chúng ta tạo ra chuỗi 0, S, 2S, 3S, v.v., tất cả đều lấy theo modulo N. 

Cách tiếp cận bạo lực sẽ mô phỏng lặp đi lặp lại quá trình này, lưu trữ các đỉnh đã truy cập trong tập băm cho đến khi chúng ta quay lại đỉnh đã thấy trước đó. Mỗi bước lấy O(1), nhưng trong trường hợp xấu nhất, khi S và N nguyên tố cùng nhau, chúng ta thăm tất cả N đỉnh trước khi lặp lại. Điều này mang lại thời gian O(N), điều này là không thể khi N đạt tới 10^9. 

Nhận xét quan trọng là đây là bài toán quỹ đạo số học mô-đun cổ điển. Chuỗi kS mod N chu kỳ với chu kỳ bằng k dương nhỏ nhất sao cho kS ≡ 0 (mod N). Điều kiện đó tương đương với việc N chia kS. K nhỏ nhất như vậy là N/gcd(N,S). Điều này xuất phát từ thực tế là S tạo ra một nhóm con của nhóm cộng modulo N, và kích thước quỹ đạo bằng chỉ số của nhóm con đó. 

Vì vậy, thay vì mô phỏng chuyển động, chúng tôi rút gọn bài toán thành việc tính ước số chung lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N) | O(N) | Quá chậm | 
| Công thức dựa trên GCD | O(logN) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán có bao nhiêu đỉnh riêng biệt xuất hiện trong chuỗi x_t = (t · S) mod N bắt đầu từ t = 0. 

1. Đọc số nguyên N và S. Chúng xác định không gian mô-đun và kích thước bước. 
2. Tính g = gcd(N, S). Điều này ghi lại kích thước bước lớn nhất phù hợp đồng đều với độ dài chu kỳ của hệ thống modulo. Theo trực giác, cả N và S đều có thể được thu nhỏ lại theo ước số chung của chúng mà không làm thay đổi cấu trúc của các trạng thái có thể tiếp cận. 
3. Tính đáp án là N // g. Điều này xuất phát từ thực tế là việc bước theo S sẽ phân chia N đỉnh thành các chu kỳ có độ dài bằng nhau được xác định bởi gcd. 
4. Xuất ra N // g là số đỉnh riêng biệt đã truy cập trước khi lặp lại. 

Lý do gcd xuất hiện là việc cộng S nhiều lần cuối cùng sẽ kết thúc chính xác khi chúng ta đạt bội số của N. Số bước nhỏ nhất làm cho kS chia hết cho N là N / gcd(N, S) và đây chính xác là kích thước quỹ đạo. 

### Tại sao nó hoạt động 

Bước đi tạo thành một nhóm con của nhóm cộng modulo N được tạo bởi S. Mọi đỉnh có thể tiếp cận đều có dạng kS mod N. Hai giá trị kS và k'S nằm trên cùng một đỉnh chính xác khi (k − k')S chia hết cho N, nghĩa là N / gcd(N, S) là bước dương tối thiểu trở về 0. Đây là chu kỳ của dãy và do đó là số đỉnh riêng biệt đã được thăm.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def solve():
    n, s = map(int, input().split())
    g = math.gcd(n, s)
    print(n // g)

if __name__ == "__main__":
    solve()
```Giải pháp dựa hoàn toàn vào việc tính toán gcd một cách hiệu quả. Thư viện toán Python triển khai thuật toán Euclid, chạy theo thời gian logarit. Phần còn lại của tính toán là một phép chia duy nhất. 

Một lỗi phổ biến là cố gắng mô phỏng chuyển động một cách rõ ràng hoặc quên rằng chu kỳ bắt đầu từ 0, nghĩa là điều kiện đỉnh lặp lại đầu tiên là khi chúng ta quay lại 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1: N = 14, S = 8 

Chúng ta tính g = gcd(14, 8) = 2, vì vậy câu trả lời là 14/2 = 7. 

| Bước | Đỉnh | 
| --- | --- | 
| 0 | 0 | 
| 1 | 8 mod 14 = 8 | 
| 2 | 16 mod 14 = 2 | 
| 3 | 10 | 
| 4 | 4 | 
| 5 | 12 | 
| 6 | 6 | 
| 7 | 0 | 

Bảng cho thấy sau 7 bước, chúng ta quay về 0 và chính xác 7 đỉnh riêng biệt đã được truy cập trước khi bắt đầu lặp lại. Điều này xác nhận công thức phù hợp với độ dài chu kỳ. 

### Ví dụ 2: N = 2, S = 1 

Chúng ta tính g = gcd(2, 1) = 1, vì vậy câu trả lời là 2. 

| Bước | Đỉnh | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 0 | 

Chúng tôi truy cập cả hai đỉnh trước khi quay lại điểm bắt đầu, khớp toàn bộ phạm vi của chu trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(logN) | bị chi phối bởi tính toán gcd của Euclid | 
| Không gian | O(1) | chỉ một số số nguyên được lưu trữ | 

Các ràng buộc cho phép các giá trị lên tới 10^9, do đó, giải pháp dựa trên gcd logarit sẽ nhanh chóng và tránh mọi mô phỏng trong các chu kỳ lớn. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n, s = map(int, input().split())
    g = math.gcd(n, s)
    return str(n // g)

assert run("2 1\n") == "2"
assert run("14 8\n") == "7"
assert run("10 10\n") == "1"
assert run("1 1\n") == "1"
assert run("12 3\n") == "4"
assert run("1000000000 1\n") == "1000000000"
assert run("1000000000 500000000\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 2 | chu kỳ không tầm thường tối thiểu | 
| 14 8 | 7 | trường hợp gcd hỗn hợp tiêu chuẩn | 
| 10 10 | 1 | tự lặp ngay lập tức | 
| 1 1 | 1 | ranh giới một nút | 
| 12 3 | 4 | chu trình có cấu trúc không đồng nguyên tố | 
| 1e9 1 | 1e9 | truyền tải tối đa | 
| 1e9 5e8 | 2 | giá trị lớn với mức giảm | 

## Vỏ cạnh 

### Trường hợp N = S 

Đối với đầu vào N = 10, S = 10, bước đi luôn ở mức 0 vì mỗi bước di chuyển thêm 0 modulo 10. Gcd là 10, do đó công thức cho 10/10 = 1, phù hợp với thực tế là chỉ có đỉnh bắt đầu được ghé thăm. 

### Trường hợp N và S cùng nguyên tố 

Đối với đầu vào N = 12, S = 5, gcd là 1, vì vậy câu trả lời là 12. Quá trình đi vòng qua tất cả các phần dư trước khi trở về 0, vì không bước nào nhỏ hơn có thể căn chỉnh các bội số S với bội số của N. Thuật toán xác định chính xác phạm vi bao phủ đầy đủ mà không cần mô phỏng.
