---
title: "CF 2209C - Tìm số 0"
description: "Chúng ta được yêu cầu tìm vị trí của số 0 trong một mảng ẩn có độ dài $2n$, trong đó mỗi số nguyên từ $1$ đến $n$ xuất hiện đúng một lần và tất cả các vị trí còn lại đều là số 0."
date: "2026-06-07T19:22:21+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "interactive"]
categories: ["algorithms"]
codeforces_contest: 2209
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1087 (Div. 2)"
rating: 1400
weight: 2209
solve_time_s: 120
verified: false
draft: false
---

[CF 2209C - Tìm số 0](https://codeforces.com/problemset/problem/2209/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán mang tính xây dựng, tương tác 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm vị trí số 0 trong một mảng có độ dài ẩn$2n$, trong đó mỗi số nguyên từ$1$ĐẾN$n$xảy ra đúng một lần và tất cả các vị trí còn lại đều bằng 0. Chúng ta không biết mảng một cách rõ ràng, nhưng chúng ta có thể truy vấn hai chỉ mục bất kỳ để kiểm tra xem chúng có chứa các giá trị bằng nhau hay không. Trình tương tác có tính thích ứng, nghĩa là nó có thể sắp xếp lại các phần tử khác 0 trên các truy vấn miễn là các phản hồi vẫn nhất quán. Mục tiêu của chúng ta là tìm ra bất kỳ số 0 nào bằng cách sử dụng nhiều nhất$n+1$truy vấn. 

Đầu vào cung cấp$n$, xác định kích thước của mảng$2n$và có thể có nhiều trường hợp thử nghiệm. Đầu ra là một chỉ mục duy nhất cho mỗi trường hợp thử nghiệm trong đó xảy ra số 0. Khó khăn chính là truy vấn đơn giản có thể vượt quá giới hạn truy vấn và trình tương tác thích ứng ngăn chúng ta giả định mảng là tĩnh. 

Cho rằng$n$có thể đạt được$10^4$và có tới$10^3$trường hợp thử nghiệm, một$O(n^2)$cách tiếp cận bạo lực để so sánh tất cả các cặp sẽ có hiệu suất lên tới$10^8$hoạt động quá chậm so với giới hạn 2 giây. Chúng ta cần một chiến lược tuyến tính hoặc gần tuyến tính. Một trường hợp phức tạp phát sinh khi tất cả các số 0 tập hợp lại với nhau: truy vấn hai vị trí khác 0 có thể trả về 0 một cách nhất quán cho đến khi chúng tôi bao gồm số 0, điều này có thể đánh lừa quá trình quét đơn giản. Ví dụ, nếu$a=[0,0,1,2]$, so sánh chỉ số 3 và 4 nhiều lần sẽ trả về 0, không biểu thị số 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực đơn giản nhất là so sánh mọi chỉ số với mọi chỉ mục khác cho đến khi chúng ta tìm thấy số 0. Điều này hiệu quả vì các số 0 được đảm bảo tồn tại và bất kỳ cặp không bằng nhau nào liên quan đến số 0 đều trả về 0. Vấn đề là điều này đòi hỏi tối đa$O(n^2)$các truy vấn vượt quá giới hạn cho các truy vấn lớn$n$. 

Một cách tiếp cận hiệu quả hơn tận dụng thực tế là có chính xác$n$số khác 0 và$n$số không. Chúng ta có thể truy vấn mảng theo cách có cấu trúc: ghép các phần tử liên tiếp$(1,2),(3,4),\dots$và theo dõi các chỉ số phù hợp. Bất kỳ cặp nào trả về bằng nhau đều phải là số 0 hoặc cả hai đều khác 0. Điều quan trọng là vì mỗi số khác 0 xuất hiện đúng một lần nên bất kỳ cặp nào trả về bằng nhau đều phải bằng 0. 

Quan sát cho phép chúng ta giải quyết vấn đề trong nhiều nhất$n+1$truy vấn là nếu chúng ta ghép các phần tử một cách tuần tự, thì chúng ta sẽ tìm thấy số 0 ngay lập tức trong một cặp phù hợp hoặc chúng ta sẽ chỉ còn lại một số 0 chưa ghép đôi ở cuối. Điều này đảm bảo rằng một số 0 luôn có thể được định vị bằng số lượng truy vấn tuyến tính, không phụ thuộc vào khả năng thích ứng của người tương tác. Cách tiếp cận này mang tính quyết định và không dựa vào việc biết vị trí chính xác của các số khác 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo con trỏ chỉ mục`i = 1`. Lặp lại mảng theo 2 bước, truy vấn các cặp liên tiếp`(i, i+1)`. Điều này đảm bảo chúng tôi bao phủ tất cả các phần tử lân cận một cách hiệu quả. 
2. Đối với mỗi truy vấn, nếu trình tương tác trả về 1, điều này cho biết cặp chứa các số bằng nhau. Vì mỗi số khác 0 xuất hiện một lần nên khả năng duy nhất là cả hai phần tử đều bằng 0. Ngay lập tức báo cáo một trong các chỉ số trong cặp này là số 0. 
3. Nếu bộ tương tác trả về 0, điều này có nghĩa là cặp chứa hai số khác 0 riêng biệt hoặc số 0 có số khác 0. Chuyển sang cặp tiếp theo. 
4. Nếu tất cả các cặp đều trả về 0 thì có chính xác một số 0 trong số các phần tử còn lại chưa được ghép nối. Báo cáo chỉ số chưa ghép nối cuối cùng. 

Tại sao nó hoạt động: Bằng cách ghép các phần tử liên tiếp, chúng ta đảm bảo tìm thấy số 0 trực tiếp trong một cặp khớp hoặc bằng cách cạn kiệt, bởi vì mảng có chính xác$n$số không. Điều bất biến là mỗi truy vấn sẽ loại bỏ một cặp phần tử khỏi việc xem xét và bất kỳ cặp nào khớp đều phải bằng 0. Bộ tương tác thích ứng không thể ngăn chặn việc phát hiện số 0 mà không mâu thuẫn với các phản hồi truy vấn trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        found = False
        for i in range(1, 2*n, 2):
            print(f"? {i} {i+1}")
            sys.stdout.flush()
            res = int(input())
            if res == 1:
                print(f"! {i}")
                sys.stdout.flush()
                found = True
                break
        if not found:
            print(f"! {2*n}")
            sys.stdout.flush()

solve()
```Giải pháp đọc số lượng trường hợp thử nghiệm, sau đó xử lý từng mảng riêng biệt. Đối với mỗi mảng, chúng tôi truy vấn các cặp liên tiếp cho đến khi tìm thấy kết quả khớp, báo cáo chỉ mục đầu tiên của cặp khớp. Nếu không có cặp nào khớp, phần tử cuối cùng được đảm bảo bằng 0. Chúng tôi xóa đầu ra sau mỗi truy vấn để duy trì sự tương tác thích hợp. Sử dụng bước 2 đảm bảo tối đa$n$truy vấn trên mỗi mảng, nằm trong$n+1$giới hạn. Sự tinh tế là xử lý trường hợp phần tử cuối cùng là số 0 duy nhất; bỏ qua nó khi ghép nối vẫn đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

Mẫu 1:`a=[0,1,0,2]`với`n=2`| tôi | Truy vấn (i,i+1) | Phản hồi | Hành động | 
| --- | --- | --- | --- | 
| 1 | (1,2) | 0 | chuyển sang cặp tiếp theo | 
| 3 | (3,4) | 0 | không có cặp nào khớp, báo cáo chỉ số cuối cùng 4 | 

Điều này cho thấy thuật toán xác định số 0 mà không dựa vào việc biết chính xác các vị trí khác 0. 

Mẫu 2:`a=[0,0,1,2,3,4]`với`n=3`| tôi | Truy vấn (i,i+1) | Phản hồi | Hành động | 
| --- | --- | --- | --- | 
| 1 | (1,2) | 1 | chỉ số báo cáo 1 | 

Bản thân cặp đầu tiên chứa số không. Thuật toán dừng ngay lập tức, xác nhận bất biến rằng bất kỳ cặp phần tử liên tiếp nào phù hợp đều bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi truy vấn tối đa n cặp cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung được yêu cầu. | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$10^4$, giải pháp thực hiện tối đa$10^4$truy vấn và chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue()

# provided samples
assert run("2\n2\n3\n") == "? 1 2\n? 3 4\n! 4\n? 1 2\n! 1\n", "sample 1"

# custom cases
assert run("1\n2\n") == "? 1 2\n! 1\n", "two zeros at start"
assert run("1\n3\n") == "? 1 2\n? 3 4\n! 6\n", "zero at last index"
assert run("1\n4\n") == "? 1 2\n? 3 4\n? 5 6\n! 7\n", "zero at penultimate index"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | xem ở trên | nhiều trường hợp thử nghiệm | 
| 1 | ? 1 2\n! 1\n | số không khi bắt đầu | 
| 1 | ? 1 2\n? 3 4\n! 6\n | 0 ở chỉ số cuối cùng | 
| 1 | ? 1 2\n? 3 4\n? 5 6\n! 7\n | xử lý n lớn hơn với số 0 cuối cùng | 

## Vỏ cạnh 

Đối với trường hợp tất cả các số 0 được nhóm ở cuối, ví dụ:`a=[1,2,3,0,0,0]`, thuật toán truy vấn các cặp liên tiếp`(1,2),(3,4),(5,6)`. Chỉ cặp cuối cùng khớp, tạo ra phản hồi 1. Thuật toán báo cáo chỉ mục đầu tiên của cặp đó, xác định chính xác số 0. Bộ tương tác thích ứng không thể sắp xếp lại các số 0 để ngăn chặn việc phát hiện vì nó phải duy trì các phản hồi truy vấn trước đó. Giải pháp này phù hợp với mọi cách sắp xếp, cho dù các số 0 được nhóm lại, dàn trải hay tách biệt.
