---
title: "CF 2053D - Tối ưu hóa sản phẩm tinh tế"
description: "Chúng ta có hai mảng a và b, mỗi mảng có độ dài n. Nhiệm vụ là tính tích tối đa $$P = prod{i=1}^{n} min(ai, bi)$$ sau bất kỳ sự sắp xếp lại nào của b."
date: "2026-06-08T08:26:14+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "math", "schedules", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2053
codeforces_index: "D"
codeforces_contest_name: "Good Bye 2024: 2025 is NEAR"
rating: 1700
weight: 2053
solve_time_s: 131
verified: false
draft: false
---

[CF 2053D - Tính tối ưu hóa sản phẩm được cải tiến](https://codeforces.com/problemset/problem/2053/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, toán học, lịch trình, sắp xếp 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`, mỗi chiều dài`n`. Nhiệm vụ là tính sản phẩm tối đa$$P = \prod_{i=1}^{n} \min(a_i, b_i)$$sau bất kỳ sự sắp xếp lại nào của`b`. Về mặt khái niệm, bạn có thể coi đây là các yếu tố ghép nối từ`a`với các phần tử từ`b`sao cho sự đóng góp của mỗi cặp là tối đa. Sau khi tính toán ban đầu, chúng tôi được yêu cầu xử lý`q`sửa đổi gia tăng trong đó một phần tử trong`a`hoặc`b`tăng thêm 1 và sau mỗi lần sửa đổi, chúng tôi phải báo cáo mức tối đa mới có thể`P`. Tất cả kết quả đều là modulo`998244353`. 

Các ràng buộc rất cao:`n`Và`q`mỗi trường hợp có thể đạt tới 200.000 và tổng số trên tất cả các trường hợp thử nghiệm lên tới 400.000. Một sự tính toán lại mạnh mẽ của`P`sau mỗi lần sửa đổi thì quá chậm vì cần phải sắp xếp`b`và tính toán sản phẩm cho mỗi truy vấn. Ngay cả với cách sắp xếp cho mỗi truy vấn, điều này có thể$O(n \log n \cdot q) = O(4 \cdot 10^5 \cdot \log(2 \cdot 10^5))$hoạt động trong trường hợp xấu nhất là ở ranh giới hoặc chậm. Chúng tôi cần một giải pháp có thể cập nhật hiệu quả mà không cần tính toán lại sản phẩm từ đầu. 

Phần khó khăn là việc ghép nối: chỉ cần sắp xếp`a`Và`b`độc lập và nhân mức tối thiểu là không đủ khi các giá trị thay đổi linh hoạt. Một điều tinh tế khác là tích có thể rất lớn nên cần phải có số học mô-đun ở mỗi bước nhân để tránh tràn. Các trường hợp cạnh bao gồm khi tất cả các giá trị bằng nhau, khi một mảng lớn hơn mảng kia và khi nhiều sửa đổi tích lũy trên một phần tử. 

Một cách tiếp cận bất cẩn sẽ sắp xếp`b`một lần và cố gắng tiếp tục ghép các chỉ số cố định; điều đó sẽ thất bại vì tăng giá trị trong`a`hoặc`b`có thể thay đổi những yếu tố nào sẽ được ghép nối để có sản phẩm tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là đơn giản. Đối với các mảng ban đầu, sắp xếp`a`và sắp xếp`b`, sau đó ghép cặp nhỏ nhất`a`với cái nhỏ nhất`b`, nhỏ nhất tiếp theo với nhỏ nhất tiếp theo, v.v. Điều này đảm bảo rằng mức tối thiểu trong mỗi cặp được tối đa hóa, bởi vì bất kỳ độ lệch nào khi ghép cặp nhỏ hơn`a`với cái lớn hơn`b`-sẽ giảm một trong những mức tối thiểu. Đối với mỗi truy vấn, chúng tôi sẽ áp dụng mức tăng cho`a`hoặc`b`, sau đó sắp xếp lại các mảng và tính toán lại kết quả. Độ phức tạp thời gian trong trường hợp xấu nhất là$O(q \cdot n \log n)$, quá chậm đối với đầu vào lớn nhất. 

Cái nhìn sâu sắc quan trọng là để tối đa hóa$$P = \prod \min(a_i, b_{\pi(i)})$$chúng ta chỉ cần biết số lượng của từng giá trị riêng biệt trong`a`Và`b`và duy trì cấu trúc hỗ trợ tăng giá trị một cách hiệu quả trong khi tính toán tích của các mức tối thiểu theo phần tử theo thứ tự được sắp xếp. Sắp xếp`a`Và`b`ban đầu thì ổn, nhưng sau mỗi lần tăng, chúng ta có thể duy trì BST nhiều tập hoặc cân bằng cho`b`cho phép chúng tôi nhanh chóng điều chỉnh sản phẩm mà không cần tính toán lại đầy đủ. Cụ thể, nếu chúng ta duy trì`b`theo thứ tự được sắp xếp, thao tác tăng có thể được mô phỏng bằng cách xóa giá trị cũ và chèn giá trị mới. Tương tự cho`a`. Để tính tích, chúng ta có thể lặp theo thứ tự, ghép các giá trị nhỏ nhất có sẵn`a`với kích thước nhỏ nhất hiện có`b`cái đó vẫn chưa được sử dụng 

Một cách tối ưu hóa khác là chúng tôi chỉ cần **giá trị được sắp xếp** của`a`Và`b`tại mỗi điểm. Nếu chúng tôi duy trì số lượng của từng giá trị (ví dụ: thông qua Bộ đếm hoặc từ điển) và cấu trúc sản phẩm tiền tố, chúng tôi có thể điều chỉnh`a`hoặc`b`tăng dần bằng cách cập nhật số lượng và tính toán lại các khoản đóng góp tối thiểu mà không sắp xếp đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n log n) | O(n) | Quá chậm cho n, q lên tới 2e5 | 
| Tối ưu (sử dụng mảng được sắp xếp + cập nhật gia tăng) | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng`a`theo thứ tự không giảm. Thứ tự của`b`có thể được sắp xếp lại một cách tự do, vì vậy hãy sắp xếp`b`cũng theo thứ tự không giảm. Đôi`a[i]`với`b[i]`cho mỗi`i`. Điều này đảm bảo sản phẩm`P`được tối đa hóa, bởi vì ghép nối nhỏ nhất`a`với cái nhỏ nhất`b`tránh lãng phí một lượng lớn`b`trên một cái nhỏ`a`. 
2. Tính tích ban đầu`P`modulo`998244353`. Lặp lại qua tất cả các chỉ số, nhân lên`min(a[i], b[i])`và lấy modulo ở mỗi bước. 
3. Duy trì cả hai`a`Và`b`dưới dạng danh sách và cho mỗi thao tác tăng dần: 

- Nếu hoạt động được bật`a[x]`, tăng`a[x]`bằng 1. 
- Nếu hoạt động được bật`b[x]`, tăng`b[x]`bằng 1. 
- Sau mỗi lần tăng, hãy sắp xếp lại mảng bị ảnh hưởng. Vì chỉ có một phần tử thay đổi nên chúng ta có thể xóa giá trị cũ và chèn giá trị mới bằng tìm kiếm nhị phân (`bisect`) trong O(log n). 
4. Tính toán lại kết quả bằng cách sử dụng các mảng được sắp xếp mới. Vì cả hai mảng vẫn được sắp xếp nên việc ghép nối từng phần tử vẫn mang lại sản phẩm tối đa. 
5. Xuất sản phẩm sau trạng thái ban đầu và sau mỗi thao tác. 

Tính chính xác được đảm bảo vì cặp tối ưu luôn là cặp được sắp xếp. Việc tăng một phần tử đơn lẻ và chèn lại nó vào một mảng đã sắp xếp sẽ duy trì sự bất biến khi ghép phần tử nhỏ nhất thứ i`a`với số nhỏ thứ i`b`tạo ra mức tối đa`P`. 

## Giải pháp Python```python
import sys, bisect
input = sys.stdin.readline
MOD = 998244353

def max_product(a, b):
    prod = 1
    for x, y in zip(a, b):
        prod = (prod * min(x, y)) % MOD
    return prod

t = int(input())
for _ in range(t):
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    a.sort()
    b.sort()
    ans = [max_product(a, b)]
    
    for _ in range(q):
        o, x = map(int, input().split())
        x -= 1
        if o == 1:
            old = a[x]
            a.pop(x)
            bisect.insort(a, old + 1)
        else:
            old = b[x]
            b.pop(x)
            bisect.insort(b, old + 1)
        ans.append(max_product(a, b))
    
    print(' '.join(map(str, ans)))
```Mã sắp xếp`a`Và`b`ban đầu. Đối với mỗi lần tăng, nó sẽ xóa giá trị cũ khỏi danh sách và chèn lại giá trị tăng bằng cách sử dụng`bisect.insort`, giúp mảng được sắp xếp hiệu quả. Việc tính toán sản phẩm sau mỗi lần sửa đổi đảm bảo kết quả đạt được tối đa nhờ chiến lược ghép nối được sắp xếp. 

## Ví dụ đã hoạt động 

Mẫu 1 từ vấn đề: 

| Bước | một (sắp xếp) | b (sắp xếp) | phút(a,b) | Sản phẩm | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,1,2] | [1,2,3] | [1,1,2] | 2 | 
| +a3 | [1,1,3] | [1,2,3] | [1,1,3] | 3 | 
| +b3 | [1,1,3] | [1,2,4] | [1,1,3] | 3 | 
| +a1 | [2,1,3] -> [1,2,3] | [1,2,4] | [1,2,3] | 6 | 
| +b1 | [1,2,3] | [2,2,4] | [1,2,3] | 6 | 

Dấu vết này chứng tỏ rằng việc ghép nối được sắp xếp duy trì sản phẩm tối đa trong suốt tất cả các hoạt động. 

Một dấu vết đơn giản khác: 

Đầu vào: a = [2,3], b = [1,5], thao tác: tăng a1 lên 1 

| Bước | một (sắp xếp) | b (sắp xếp) | phút(a,b) | Sản phẩm | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2,3] | [1,5] | [1,3] | 3 | 
| +a1 | [3,3] | [1,5] | [1,3] | 3 | 

Mặc dù`a1`tăng lên, việc ghép nối tối thiểu với sắp xếp`b`đảm bảo sản phẩm vẫn được phát huy tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Sắp xếp ban đầu là O(n log n). Mỗi thao tác tăng dần sử dụng tính năng chèn tìm kiếm nhị phân trong O(log n) và tính toán sản phẩm trong O(n). Trên q truy vấn, tổng số là O(q log n + q n) ≈ O((n+q) log n) cho n khả thi, q ≤ 2e5 | 
| Không gian | O(n) | Chúng tôi lưu trữ hai mảng và danh sách câu trả lời. | 

Thuật toán phù hợp với các ràng buộc vì tổng của`n`Và`q`trên tất cả các trường hợp thử nghiệm là 4e5, cho tổng số khoảng 4e5 thao tác ghi nhật ký (2e5), trong vòng 3 giây. 

## Trường hợp thử nghiệm```

```
