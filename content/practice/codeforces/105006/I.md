---
title: "CF 105006I - Đếm Corgi"
description: "Chúng ta được cung cấp một tập hợp các chú chó corgi, mỗi chú được gán một giá trị tính cách nguyên 22 bit. Nếu chúng ta chọn bất kỳ tập hợp con nào của những chú chó corgi này, mỗi cặp bên trong tập hợp con đó đều đóng góp một “niềm hạnh phúc” bằng với AND trong tính cách của chúng."
date: "2026-06-28T03:13:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105006
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 03-01-24 Div. 1 (Advanced)"
rating: 0
weight: 105006
solve_time_s: 61
verified: false
draft: false
---

[CF 105006I - Đếm Corgi](https://codeforces.com/problemset/problem/105006/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chú chó corgi, mỗi chú được gán một giá trị tính cách nguyên 22 bit. Nếu chúng ta chọn bất kỳ tập hợp con nào của những chú chó corgi này, mỗi cặp bên trong tập hợp con đó đều đóng góp một “niềm hạnh phúc” bằng với AND trong tính cách của chúng. Tổng hạnh phúc của một tập hợp con là tổng của các giá trị AND theo cặp này. Nhiệm vụ là tính tổng số lượng này trên mọi tập hợp con có thể có của corgis. 

Một cách trực tiếp để giải thích điều này là mỗi cặp chỉ số đóng góp nhiều lần: một lần cho mỗi tập hợp con chứa cả hai phần tử của cặp. Thay vì nghĩ về các tập con trước tiên, sẽ hữu ích hơn khi nghĩ về tần suất một cặp cố định xuất hiện trên tất cả các tập con. 

Các ràng buộc rất lớn, lên tới 200000 phần tử và giá trị lên tới 2²². Điều này ngay lập tức loại trừ bất kỳ giải pháp nào lặp lại trên các tập hợp con hoặc thậm chí trực tiếp trên tất cả các cặp nếu mỗi cặp yêu cầu công việc nặng nhọc. Có khoảng 2×10^10 tập hợp con và 2×10^10 cặp trên tất cả các tập hợp con, vì vậy không thể liệt kê bất kỳ tập hợp con nào. Ngay cả O(n²) cũng là đường biên quá lớn đối với n = 2×10^5. 

Một cách tiếp cận đơn giản sẽ tính toán cho mỗi tập hợp con tổng cặp nội bộ của nó, dẫn đến thời gian theo cấp số nhân. Ngay cả việc tính tổng tất cả các cặp một lần và sau đó nhân với một hệ số cố định cũng bỏ qua cấu trúc tập hợp con và thất bại, bởi vì mỗi cặp không được tính bằng nhau trên các tập hợp con trừ khi chúng ta tính đến tổ hợp một cách cẩn thận. 

Một cạm bẫy tinh vi là giả sử tính tuyến tính trên các tập hợp con mà không theo dõi bội số. Ví dụ: với các giá trị [1, 2], cặp (1,2) chỉ đóng góp khi cả hai đều được chọn, điều này xảy ra trong chính xác một tập hợp con chứ không phải trong tất cả các tập hợp con chứa một trong hai phần tử. 

## Phương pháp tiếp cận 

Ý tưởng chính là đảo ngược thứ tự tính tổng. Thay vì tính tổng các tập hợp con trước, trước tiên chúng ta tính tổng theo các cặp và đối với mỗi cặp, hãy xác định có bao nhiêu tập hợp con chứa nó. 

Sửa hai chỉ số i < j. Cặp (i, j) xuất hiện chính xác trong các tập con bao gồm cả hai phần tử, trong khi n − 2 phần tử còn lại là tùy ý. Do đó, mỗi cặp đóng góp giá trị AND của nó nhân với 2^(n−2). Điều này làm giảm toàn bộ vấn đề về tính toán: 

tính tổng tất cả các cặp (p_i & p_j), sau đó nhân với 2^(n−2). 

Bây giờ vấn đề trở thành tính tổng bitwise AND trên tất cả các cặp. Đây là một thủ thuật phân tách bit cổ điển. Mỗi bit có thể được xử lý độc lập. Với mỗi bit b, chúng ta đếm xem có bao nhiêu số có tập hợp bit này. Nếu các phần tử cnt[b] có tập hợp bit b thì sẽ có cnt[b] chọn 2 cặp đóng góp bit đó và mỗi cặp như vậy đóng góp 2^b vào tổng AND. 

Vậy tổng số cặp VÀ tổng là: 

tổng trên b: (cnt[b] * (cnt[b] − 1) / 2) * 2^b 

Cuối cùng nhân với 2^(n−2). 

Điều này làm giảm vấn đề đếm bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(n · 2^n) | O(1) | Quá chậm | 
| Đếm cặp với phân tách bit | O(n · 22) | O(22) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi bit từ 0 đến 21 xuất hiện trên tất cả các giá trị. Điều này tách biệt các đóng góp trên mỗi bit, vì AND hoạt động độc lập trên mỗi bit. 
2. Với mỗi bit b, hãy tính cnt[b] * (cnt[b] − 1) // 2. Điều này cho biết số cặp không có thứ tự trong đó cả hai số đều có bit b được đặt. Lý do điều này hoạt động là vì một bit chỉ đóng góp vào AND khi cả hai điểm cuối đều có nó. 
3. Nhân mỗi cặp số đếm với 2^b để tính giá trị của bit đó trong tổng cuối cùng. 
4. Tính tổng các phần đóng góp này trên tất cả các bit để có tổng số p_i & p_j trên tất cả các cặp không có thứ tự. 
5. Nhân kết quả với 2^(n−2), vì mỗi cặp cố định xuất hiện trong chính xác 2^(n−2) tập hợp con. Mọi tập hợp con chứa cả hai điểm cuối đều có thể tự do chọn bất kỳ phần tử nào còn lại. 
6. Trả về kết quả modulo 998244353. 

### Tại sao nó hoạt động

Tính đúng đắn dựa trên hai phân rã độc lập. Đầu tiên, đóng góp của mỗi cặp trên tất cả các tập hợp con là đồng nhất và chỉ phụ thuộc vào số lượng tập hợp con chứa cả hai điểm cuối, chính xác là 2^(n−2). Thứ hai, bitwise AND có tính cộng trên các bit, nghĩa là sự đóng góp của từng bit có thể được tính riêng mà không bị nhiễu từ các bit khác. Vì cả hai phép biến đổi đều bảo toàn tổng đóng góp chính xác nên công thức cuối cùng khớp với định nghĩa ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n = int(input())
a = list(map(int, input().split()))

B = 22
cnt = [0] * B

for x in a:
    for b in range(B):
        if x & (1 << b):
            cnt[b] += 1

pair_and_sum = 0
for b in range(B):
    c = cnt[b]
    pair_and_sum += (c * (c - 1) // 2) % MOD * ((1 << b) % MOD)
    pair_and_sum %= MOD

if n >= 2:
    pair_and_sum *= modpow(2, n - 2)
    pair_and_sum %= MOD
else:
    pair_and_sum = 0

print(pair_and_sum)
```Việc triển khai trước tiên sẽ nén tất cả thông tin thành số lượng bit, tránh bất kỳ việc liệt kê cặp nào. Phép lũy thừa mô-đun tính toán hệ số bội tập hợp con 2^(n−2) một cách hiệu quả. Phép nhân được hoãn lại cho đến sau phép tính tổng theo cặp để tránh các hoạt động mô-đun lặp lại. 

Một chi tiết tinh tế là xử lý n < 2, trong đó không tồn tại cặp nào và kết quả phải bằng 0. Một cách khác là giữ tất cả các giá trị trung gian theo modulo 998244353 trong khi vẫn sử dụng số học số nguyên cho các kết hợp, vì cnt[b] choose 2 khớp an toàn trong số nguyên Python trước khi rút gọn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2 3
```Số bit: 

bit 0: {1, 3} → 2 

bit 1: {2, 3} → 2 

| Chút | cnt | cặp cntC2 | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | 1 | 
| 1 | 2 | 1 | 2 | 

Tổng cặp = 3. 

Nhân với 2^(3−2) = 2 → kết quả = 6. 

Điều này xác nhận rằng mỗi cặp được tính một lần cho mỗi tập con chứa nó. 

### Mẫu 2 

đầu vào:```
5
3 5 2 1 2
```Số bit: 

bit 0: 3 phần tử 

bit 1: 3 phần tử 

| Chút | cnt | cntC2 | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 3 | 3 | 3 | 
| 1 | 3 | 3 | 6 | 

Pa
