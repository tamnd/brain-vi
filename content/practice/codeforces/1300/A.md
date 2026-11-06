---
title: "CF 1300A - Khác không"
description: "Chúng ta được cho một mảng các số nguyên và muốn làm cho hai thuộc tính đồng thời đúng: tổng của các phần tử không được bằng 0 và tích của các phần tử không được bằng 0. Hoạt động duy nhất được phép là tăng từng phần tử lên một."
date: "2026-06-11T18:19:56+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1300
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 618 (Div. 2)"
rating: 800
weight: 1300
solve_time_s: 42
verified: true
draft: false
---

[CF 1300A - Khác 0](https://codeforces.com/problemset/problem/1300/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và muốn làm cho hai thuộc tính đồng thời đúng: tổng của các phần tử không được bằng 0 và tích của các phần tử không được bằng 0. Hoạt động duy nhất được phép là tăng từng phần tử lên một. Nhiệm vụ là tìm số lượng tối thiểu các phép toán tăng như vậy để thỏa mãn cả hai thuộc tính. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bắt đầu bằng kích thước của mảng, theo sau là các phần tử mảng. Giới hạn rất nhỏ: tối đa 100 phần tử trên mỗi mảng, mỗi phần tử nằm trong khoảng từ -100 đến 100 và nhiều nhất là 1000 trường hợp thử nghiệm. Điều này có nghĩa là một$O(n)$giải pháp cho mỗi trường hợp thử nghiệm sẽ đủ nhanh. 

Các trường hợp biên xuất hiện khi có số 0 trong mảng. Số 0 trong mảng làm cho kết quả bằng 0, số này phải được cố định bằng ít nhất một gia số trên mỗi số 0. Một trường hợp tinh vi khác xảy ra nếu tổng của mảng bằng 0 sau khi sửa tất cả các số 0. Ví dụ: một mảng như [0, -1, 1] có tổng bằng 0 và tích bằng 0. Nếu chúng ta tăng số 0 lên 1 thì tích sẽ khác 0, nhưng tổng bây giờ là 1, do đó cả hai điều kiện đều được thỏa mãn. Nói chung, sau khi xử lý các số 0, chúng ta phải kiểm tra xem bản thân tổng có bằng 0 hay không, vì việc tăng dần các số 0 có thể không tự động sửa tổng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ mô phỏng việc tăng từng số 0 riêng lẻ và sau đó kiểm tra mọi sự kết hợp của các số gia bổ sung để cố định tổng. Điều này đúng nhưng không cần thiết vì bài toán có thể được giải thích bằng toán học. Với$n \le 100$và các giá trị bị giới hạn, số lượng thao tác có vẻ nhỏ nhưng việc mô phỏng mọi điều chỉnh tổng có thể yêu cầu công việc theo cấp số nhân, điều này không hợp lý nếu xét đến thông tin chi tiết đơn giản hơn có sẵn. 

Cái nhìn sâu sắc quan trọng là tách biệt hai ràng buộc. Bất kỳ số 0 nào trong mảng đều phải được tăng lên để sửa kết quả. Cho phép`zeros`là số không; chúng ta cần ít nhất`zeros`hoạt động. Sau khi tăng tất cả các số 0, chúng ta có thể tính tổng của mảng kết quả. Nếu tổng này bằng 0, chúng ta cần thêm một mức tăng thêm ở bất kỳ đâu để phá vỡ tổng từ 0. Nếu tổng đã khác 0 thì không cần thực hiện thêm thao tác nào. Điều này làm giảm giải pháp đếm số 0 và kiểm tra tổng đã điều chỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Đếm Số Không + Điều Chỉnh Tổng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`steps`về không. Điều này sẽ theo dõi tổng số gia tăng cần thiết. 
2. Đếm số lượng số 0 trong mảng. Mỗi số 0 yêu cầu một số gia để làm cho tích khác không, vì vậy hãy cộng số này vào`steps`. 
3. Tính tổng của mảng sau khi giả sử tăng tất cả các số 0 lên một. Cho phép`sum_after`được số tiền này. 
4. Nếu`sum_after`bằng 0, tăng`steps`bởi một. Điều này giải thích cho tình huống việc sửa chữa sản phẩm tạo ra tổng bằng 0. 
5. In`steps`như là kết quả của trường hợp thử nghiệm này. 

Tại sao nó hoạt động: Mỗi số 0 phải được tăng lên để đạt được tích khác 0, điều mà thuật toán đảm bảo. Khi các số 0 được xử lý, cách duy nhất để tổng có thể giữ nguyên bằng 0 là nếu các phần tử khác 0 bị loại bỏ hoàn toàn. Thêm một số gia nữa vào bất cứ đâu sẽ phá vỡ điều này, đảm bảo cả tổng và tích đều khác 0. Thuật toán đếm chính xác số lượng gia số tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    zeros = a.count(0)
    steps = zeros
    
    sum_after = sum(a) + zeros
    if sum_after == 0:
        steps += 1
    
    print(steps)
```Phần đầu tiên đọc số lượng trường hợp kiểm thử và lặp lại chúng. Đối với mỗi mảng
