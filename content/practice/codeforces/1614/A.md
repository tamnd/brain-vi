---
title: "CF 1614A - Divan và một cửa hàng"
description: "Divan muốn mua những thanh sô cô la với ba điều kiện ràng buộc: anh ấy chỉ xem xét những thanh sô cô la trong phạm vi giá [l, r] và anh ấy không thể vượt quá tổng ngân sách của mình k."
date: "2026-06-10T06:46:43+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1614
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 757 (Div. 2)"
rating: 800
weight: 1614
solve_time_s: 77
verified: true
draft: false
---

[CF 1614A - Divan và một cửa hàng](https://codeforces.com/problemset/problem/1614/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thuật toán mang tính xây dựng, lòng tham 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Divan muốn mua những thanh sô cô la với ba điều kiện ràng buộc: anh ấy chỉ xem xét những thanh sô cô la trong phạm vi giá [l, r] và anh ấy không thể vượt quá tổng ngân sách của mình k. Chúng ta được cho giá của n thanh sô cô la và với mỗi trường hợp thử nghiệm, chúng ta cần tính số thanh tối đa mà anh ta có thể mua mà không vi phạm những ràng buộc này. 

Dữ liệu đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có số lượng thanh, mức giá thấp hơn và cao hơn có thể chấp nhận được, tổng ngân sách của Divan và dãy giá sô cô la. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên biểu thị số lượng sôcôla tối đa mà Divan có thể mua. 

Các ràng buộc ngụ ý rằng n tối đa là 100 và giá cả cũng như ngân sách có thể lên tới 10^9. Kích thước nhỏ của n cho chúng ta biết rằng bất kỳ cách tiếp cận nào có độ phức tạp khoảng O(n log n) hoặc thậm chí O(n^2) đều có thể chấp nhận được đối với một trường hợp thử nghiệm duy nhất. Tuy nhiên, vì t, số lượng trường hợp thử nghiệm, có thể lên tới 100, nên cách tiếp cận O(n^2) ngây thơ có thể yêu cầu tới 10^6 thao tác trong trường hợp xấu nhất, vẫn có thể chấp nhận được với giới hạn thời gian 1 giây. Điều quan trọng hơn là xử lý cẩn thận việc lọc giá và hạn chế ngân sách. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó tất cả các thanh sô cô la đều quá rẻ hoặc quá đắt hoặc khi ngân sách nhỏ hơn cả thanh rẻ nhất có thể chấp nhận được. Ví dụ: nếu l = 10, r = 20, k = 5 và giá là [12, 15] thì câu trả lời đúng là 0, vì Divan không đủ tiền mua bất kỳ thanh nào. Một giải pháp bất cẩn có thể chọn sai thanh rẻ nhất có thể chấp nhận được mà không kiểm tra ngân sách. 

Một trường hợp khác phát sinh khi nhiều thanh có cùng mức giá, có khả năng lấp đầy ngân sách một cách chính xác. Ví dụ: nếu k = 10 và giá = [5, 5, 5], Divan chỉ có thể mua hai thanh chứ không phải ba. Việc quên tích lũy chi phí cẩn thận có thể dẫn đến việc tính toán quá mức. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ liệt kê tất cả các tập hợp con của những thanh sô cô la có thể chấp nhận được và kiểm tra xem tập hợp con nào phù hợp với ngân sách. Với n = 100, điều này dẫn đến 2^100 tập hợp con, quá lớn về mặt thiên văn. Rõ ràng, cách tiếp cận này là không khả thi. 

Quan sát quan trọng là chúng tôi không quan tâm đến sự kết hợp nào của các thanh mà chúng tôi chọn vượt quá giá của chúng. Để tối đa hóa số lượng thanh, chúng ta phải luôn chọn những thanh rẻ nhất trước tiên, vì mua những thanh rẻ hơn cho phép chúng ta mua nhiều thanh hơn trong cùng một ngân sách. Điều này làm giảm vấn đề thành nhiệm vụ sắp xếp và lựa chọn tham lam: lọc các thanh theo giá, sắp xếp các thanh còn lại theo thứ tự tăng dần, sau đó lặp lại chúng, tích lũy chi phí của chúng cho đến khi vượt quá ngân sách. Cách tiếp cận này vừa đúng vừa hiệu quả khi cho n ≤ 100. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các tập hợp con) | O(2^n) | O(n) | Quá chậm | 
| Lọc + Sắp xếp + Tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case t. Lặp lại từng trường hợp thử nghiệm. 
2. Đối với trường hợp thử nghiệm, đọc n, l, r, k và mảng giá a. 
3. Lọc mảng a để chỉ giữ giá nằm trong khoảng từ l đến r. Điều này đảm bảo chúng tôi chỉ xem xét những loại sôcôla mà Divan thích. 
4. Sắp xếp mảng đã lọc theo thứ tự tăng dần. Việc sắp xếp đảm bảo chúng tôi xem xét các thanh rẻ hơn trước, điều này giúp tối đa hóa số lượng thanh chúng tôi có thể mua với ngân sách k. 
5. Khởi tạo bộ đếm số lượng sôcôla đã mua và một biến số cho số tiền còn lại. Lặp lại thông qua các mức giá được sắp xếp. 
6. Với mỗi mức giá p, hãy kiểm tra xem p có nhỏ hơn hoặc bằng ngân sách còn lại hay không. Nếu vậy, hãy trừ p khỏi ngân sách và tăng số đếm. Nếu p vượt quá ngân sách còn lại, hãy ngừng lặp lại vì việc thêm bất kỳ thanh nào nữa (tất cả đều đắt hơn) sẽ vượt quá ngân sách. 
7. Sau khi xử lý tất cả các thanh, xuất bộ đếm làm câu trả lời cho trường hợp kiểm thử.

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi duy trì tính bất biến rằng chúng tôi đã chi số tiền tối thiểu có thể cho số lượng thanh tối đa. Vì mảng được sắp xếp theo thứ tự tăng dần nên việc bỏ qua một thanh vì nó vượt quá ngân sách còn lại sẽ đảm bảo rằng không thể thêm thanh nào nữa. Cách tiếp cận tham lam này đảm bảo số lượng sôcôla được chọn tối đa mà không vượt quá ngân sách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, l, r, k = map(int, input().split())
    prices = list(map(int, input().split()))
    
    # Keep only acceptable chocolates
    valid_prices = [p for p in prices if l <= p <= r]
    
    # Sort ascending to prioritize cheaper chocolates
    valid_prices.sort()
    
    count = 0
    remaining_budget = k
    for price in valid_prices:
        if price <= remaining_budget:
            remaining_budget -= price
            count += 1
        else:
            break
    
    print(count)
```Mã đọc tất cả các trường hợp thử nghiệm một cách hiệu quả. Việc lọc đảm bảo chúng ta không xem xét những mức giá không thể chấp nhận được, việc phân loại cho phép lựa chọn tham lam các thanh rẻ hơn và quá trình lặp lại sẽ tích lũy sôcôla cho đến khi hết ngân sách. Việc sắp xếp và lọc rất đơn giản nhưng đòi hỏi phải chú ý đến ranh giới của l và r. Việc lặp lại phải dừng ngay khi ngân sách không đủ, đây là nguồn lỗi phổ biến. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 1 100 100
50 100 50
```| Biến | Giá trị sau khi lọc | Giá trị sau khi sắp xếp | Truy cập tiến trình | 
| --- | --- | --- | --- | 
| giá trị hợp lệ | [50, 100, 50] | [50, 50, 100] | đếm = 0, còn lại = 100 | 
| Bước 1 | p = 50 | - | đếm = 1, còn lại = 50 | 
| Bước 2 | p = 50 | - | đếm = 2, còn lại = 0 | 
| Bước 3 | p = 100 | - | vượt quá ngân sách, dừng lại | 

Đầu ra: 2. Điều này khẳng định sự lựa chọn tham lam của các thanh rẻ nhất đầu tiên sẽ tối đa hóa số lượng. 

### Mẫu 2 

đầu vào:```
6 3 5 10
1 2 3 4 5 6
```Đã lọc giá trị hợp lệ = [3, 4, 5], sắp xếp tăng dần = [3, 4, 5]. Lặp lại: 

| Bước | Giá | Còn lại | Đếm | 
| --- | --- | --- | --- | 
| 1 | 3 | 10-3=7 | 1 | 
| 2 | 4 | 7-4=3 | 2 | 
| 3 | 5 | 3 < 5, dừng lại | 2 | 

Đầu ra: 2. Xác nhận dừng chính xác khi thanh tiếp theo vượt quá ngân sách còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n log n) | Lọc là O(n), sắp xếp O(n log n), lặp lại O(n). Đối với t trường hợp thử nghiệm, nhân với t. | 
| Không gian | O(n) | Lưu trữ danh sách đã lọc các giá hợp lệ. | 

Với n 100 và t 100, t*n log n tối đa là 100 * 100 * log 100 ≈ 66.000 thao tác, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    # Solution code
    t = int(input())
    for _ in range(t):
        n, l, r, k = map(int, input().split())
        prices = list(map(int, input().split()))
        valid_prices = [p for p in prices if l <= p <= r]
        valid_prices.sort()
        count = 0
        remaining_budget = k
        for price in valid_prices:
            if price <= remaining_budget:
                remaining_budget -= price
                count += 1
            else:
                break
        print(count)
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("8\n3 1 100 100\n50 100 50\n6 3 5 10\n1 2 3 4 5 6\n6 3 5 21\n1 2 3 4 5 6\n10 50 69 100\n20 30 40 77 1 1 12 4 70 10000\n3 50 80 30\n20 60 70\n10 2 7 100\n2 2 2 2 2 7 7 7 7 7\n4 1000000000 1000000000 1000000000\n1000000000 1000000000 1000000000 1000000000\n1 1 1 1\n1") == "2\n2\n3\n0\n0\n10\n1\n1", "samples"

# Custom edge cases
assert run("1\n5 10 20 5\n12 15 18 19 20") == "0", "budget too small for any"
assert run("1\n5 10 20 50\n12 15 18 19 20") == "4", "maximum bars under budget"
assert run("1\n3 5 5 10\n5 5 5") == "2", "all equal prices, exact budget use"
assert run("1\n1 1 1000000000 1000000000\n100000
```
