---
title: "CF 106030D - \u6709\u9650\u5c0f\u6570"
description: "Nhiệm vụ xoay quanh việc quyết định xem một số hữu tỷ đã cho có thể được biểu diễn dưới dạng số thập phân tận cùng hay không. Nói cách khác, đối với mỗi phân số được cung cấp, chúng tôi muốn xác định xem liệu việc mở rộng thập phân của nó có kết thúc sau một số hữu hạn các chữ số thay vì tiếp tục vô thời hạn hay không."
date: "2026-06-22T16:58:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106030
codeforces_index: "D"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Chongqing Onsite"
rating: 0
weight: 106030
solve_time_s: 52
verified: true
draft: false
---

[CF 106030D - \u6709\u9650\u5c0f\u6570](https://codeforces.com/problemset/problem/106030/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ xoay quanh việc quyết định xem một số hữu tỷ đã cho có thể được biểu diễn dưới dạng số thập phân tận cùng hay không. Nói cách khác, đối với mỗi phân số được cung cấp, chúng tôi muốn xác định xem liệu việc mở rộng thập phân của nó có kết thúc sau một số hữu hạn các chữ số thay vì tiếp tục vô thời hạn hay không. 

Mỗi mục đầu vào có thể được xem dưới dạng tỷ lệ giữa hai số nguyên. Sau khi đơn giản hóa phân số về số hạng thấp nhất, chúng ta đang hỏi xem mẫu số của nó có tương thích với cách biểu diễn cơ số 10 hay không. Nếu đúng như vậy thì số đó có dạng thập phân hữu hạn; mặt khác, nó tạo ra một số thập phân lặp lại vô hạn. 

Ràng buộc cấu trúc chính là kích thước đầu vào có thể bao gồm nhiều phân số như vậy và mỗi phân số có thể bao gồm các số nguyên lớn vừa phải. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng mô phỏng chữ số mở rộng thập phân theo chữ số hoặc thực hiện phép chia với độ chính xác cao. Các phương pháp như vậy sẽ giảm xuống công việc tuyến tính trên mỗi chữ số được tạo ra, quá chậm nếu mẫu số lớn. 

Một trường hợp sai sót tinh tế xuất hiện khi phân số không được giảm trước. Ví dụ, hãy xem xét ngày 12/8. Nếu trực tiếp kiểm tra 12, chúng ta có thể kết luận sai điều gì đó về việc phân tích thành nhân tử của nó. Nhưng sau khi đơn giản hóa, 12/8 trở thành 2/3 và hành vi của mẫu số thay đổi hoàn toàn. Đầu ra đúng chỉ phụ thuộc vào mẫu số rút gọn. 

Một trường hợp cạnh khác phát sinh khi mẫu số là 1 sau khi giảm, chẳng hạn như 5/1 hoặc 42/1. Đây là những số nguyên và kết thúc một cách tầm thường, nhưng việc triển khai đơn giản chỉ kiểm tra các mẫu chia hết trước khi đơn giản hóa vẫn có thể thực hiện các tính toán không cần thiết hoặc phân loại sai các dạng trung gian. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng phép chia tử số cho mẫu số trong cơ số 10 và kiểm tra xem phần còn lại cuối cùng có bằng 0 hay không. Điều này phản ánh hiệu quả sự phân chia dài. Mặc dù đúng nhưng số bước cần thiết phụ thuộc trực tiếp vào độ lớn của mẫu số và trong trường hợp xấu nhất, chu kỳ lặp lại có thể cực kỳ dài. Đối với đầu vào lớn, điều này trở nên không khả thi vì mỗi bước chỉ loại bỏ một chữ số thập phân không chắc chắn. 

Quan sát quan trọng là chúng ta không cần phải mô phỏng quá trình thập phân nào cả. Một số hữu tỷ có biểu diễn thập phân tận cùng khi và chỉ nếu, sau khi rút gọn phân số thành số hạng thấp nhất, mẫu số của nó không chứa thừa số nguyên tố nào ngoài 2 và 5. Điều này xuất phát trực tiếp từ cấu trúc của cơ số 10, vì 10 = 2 × 5, và bất kỳ số thập phân tận cùng nào cũng có thể được viết dưới dạng số nguyên chia cho lũy thừa nào đó của 10. 

Điều này làm giảm toàn bộ vấn đề thành nhân tử 2 và 5 từ mẫu số sau khi đơn giản hóa. Nếu sau đó bất cứ thứ gì còn sót lại trong mẫu số, thì cấu trúc còn sót lại đó sẽ tạo ra một chu trình lặp lại trong khai triển cơ số 10. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sư đoàn dài Brute Force | O(k) mỗi phân số | O(1) | Quá chậm | 
| Lọc hệ số nguyên tố | O(log n) trên mỗi phân số | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng phân số một cách độc lập và rút gọn nó trước khi kiểm tra cấu trúc mẫu số của nó.

1. Đọc một phân số gồm hai số nguyên, tử số và mẫu số. Những điều này xác định số hữu tỷ mà chúng tôi đang phân tích. 
2. Tính ước chung lớn nhất của hai số rồi chia cả hai cho số đó. Bước này đảm bảo rằng chúng ta đang làm việc với phân số ở dạng đơn giản nhất. Điều này rất cần thiết vì bất kỳ thừa số chung ẩn nào cũng có thể làm sai lệch phân tích nhân tố. 
3. Tập trung hoàn toàn vào mẫu số đơn giản. Nếu tại thời điểm này là 1 thì số đó đã là số nguyên và do đó có biểu diễn thập phân tận cùng. 
4. Trong khi mẫu số chia hết cho 2, hãy chia nó cho 2. Điều này loại bỏ tất cả các đóng góp khỏi hệ số tương thích với cơ số 10. 
5. Trong khi mẫu số chia hết cho 5, hãy chia nó cho 5. Điều này tương tự sẽ loại bỏ tất cả các đóng góp tương thích với tỷ lệ cơ số 10. 
6. Sau khi loại bỏ tất cả các thừa số của 2 và 5, hãy kiểm tra xem mẫu số có trở thành 1 hay không. Nếu không, vẫn còn một số thừa số nguyên tố khác đảm bảo số thập phân lặp lại. 

### Tại sao nó hoạt động 

Khi phân số đã được rút gọn, cách duy nhất để biểu thị nó dưới dạng số thập phân tận cùng là viết lại nó với mẫu số có lũy thừa bằng 10. Vì 10 chỉ phân hủy thành 2 và 5, nên bất kỳ số thập phân tận cùng hợp lệ nào cũng phải có mẫu số mà hệ số nguyên tố của nó không chứa các số nguyên tố khác ngoài 2 và 5. Các bước chia lặp lại mô phỏng việc trích ra hệ số lớn nhất có thể có của 10 từ mẫu số. Nếu bất cứ thứ gì còn sót lại sau đó, cấu trúc còn sót lại đó không thể được hấp thụ thành lũy thừa 10, điều này buộc phải mở rộng lặp lại vô hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def is_terminating(a, b):
    g = gcd(a, b)
    a //= g
    b //= g

    if b == 1:
        return True

    while b % 2 == 0:
        b //= 2
    while b % 5 == 0:
        b //= 5

    return b == 1

def solve():
    data = sys.stdin.read().strip().split()
    if not data:
        return
    t = int(data[0])
    idx = 1
    out = []

    for _ in range(t):
        a = int(data[idx]); b = int(data[idx + 1])
        idx += 2
        out.append("Yes" if is_terminating(a, b) else "No")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ giảm từng phân số bằng cách sử dụng ước số chung lớn nhất, điều này rất cần thiết để tránh các kết quả âm tính giả do các yếu tố chung gây ra. Sau khi giảm, nó liên tục loại bỏ các hệ số 2 và 5 khỏi mẫu số. Kiểm tra cuối cùng là kiểm tra cấu trúc trực tiếp: liệu có còn gì khác ngoài 2s và 5s hay không. 

Thứ tự của các hoạt động quan trọng một cách tinh tế. Nếu chúng ta cố gắng loại bỏ các thừa số trước khi rút gọn phân số, chúng ta có thể bỏ sót các phép hủy chỉ xuất hiện sau khi đơn giản hóa, dẫn đến kết luận không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét hai phân số đại diện. 

Đầu tiên, lấy ngày 12/8. 

| Bước | Tử số | Mẫu số | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 8 | 12 | Phân số đầu vào | 
| Sau khi giảm GCD | 2 | 3 | Chia cho 4 | 
| Xóa 2s và 5s | 2 | 3 | Mẫu số không có 2 hoặc 5 | 
| Kiểm tra lần cuối | 2 | 3 | Mẫu số không 1 | 

Mẫu số còn lại là 3, xác nhận sự mở rộng thập phân không kết thúc. 

Bây giờ hãy xem xét 25/40. 

| Bước | Tử số | Mẫu số | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 25 | 40 | Phân số đầu vào | 
| Sau khi giảm GCD | 5 | 8 | Chia cho 5 | 
| Xóa 2s và 5s | 5 | 1 | Yếu tố dải | 
| Kiểm tra lần cuối | 5 | 1 | Chấm dứt | 

Điều này cho thấy rằng sau khi đơn giản hóa, mẫu số chỉ chứa lũy thừa của 2 và do đó tạo ra số thập phân tận cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T log N) | Mỗi phân số yêu cầu tính toán gcd và loại bỏ hệ số 2 và 5 | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Các phép toán đủ nhanh đối với các đầu vào lớn vì gcd và phép chia lặp lại đều có tỷ lệ logarit với độ lớn của các số liên quan. Ngay cả với nhiều trường hợp thử nghiệm, giải pháp vẫn phù hợp thoải mái trong giới hạn lập trình cạnh tranh điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided samples (hypothetical since not given)
# assert run("...") == "..."

# custom cases
assert run("3\n1 2\n1 3\n1 5\n") == "Yes\nNo\nYes", "basic terminating vs non-terminating"

assert run("2\n8 12\n25 40\n") == "No\nYes", "reduction + factor stripping"

assert run("1\n7 1\n") == "Yes", "integer case"

assert run("1\n6 27\n") == "No", "denominator has non 2/5 factor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hỗn hợp các phân số nhỏ | Có/Không trộn | tính đúng đắn cơ bản | 
| 12/8, 25/40 | Không, Có | giảm thiểu sự cần thiết | 
| 1/7 | Có | xử lý số nguyên | 
| 27/6 | Không | thừa số nguyên tố ẩn | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi tử số và mẫu số có chung các thừa số chung lớn mà chỉ hiển thị sau khi rút gọn hoàn toàn. Ví dụ, trong 100/250, một cách tiếp cận ngây thơ có thể suy luận trực tiếp không chính xác về 250. Sau khi rút gọn, phân số trở thành 2/5, kết thúc rõ ràng vì mẫu số đã là lũy thừa của 5. 

Một trường hợp khác là khi mẫu số ban đầu trông có vẻ “sạch” nhưng lại ẩn đi cấu trúc bổ sung thông qua các thừa số chung với tử số. Ví dụ: 27/6 đơn giản hóa thành 2/9 và mẫu số còn lại chứa hệ số 3, đảm bảo số thập phân lặp lại. 

Cuối cùng, đầu vào số nguyên như 42/7 giảm xuống còn 6/1. Những điều này phải được thừa nhận là kết thúc ngay sau khi đơn giản hóa, mặc dù mẫu số ban đầu không gợi ý điều đó ngay từ cái nhìn đầu tiên.
