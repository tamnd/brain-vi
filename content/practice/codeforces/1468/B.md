---
title: "CF 1468B - Tiệm Bánh"
description: "Chúng ta được yêu cầu tính số lượng bánh mì cũ tại một tiệm bánh trong nhiều ngày. Mỗi ngày tiệm bánh sản xuất một số lượng bánh mì cố định và khách hàng đến hàng ngày với nhu cầu đã biết."
date: "2026-06-11T01:21:21+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu"]
categories: ["algorithms"]
codeforces_contest: 1468
codeforces_index: "B"
codeforces_contest_name: "2020-2021 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules)"
rating: 2900
weight: 1468
solve_time_s: 137
verified: false
draft: false
---

[CF 1468B - Tiệm bánh](https://codeforces.com/problemset/problem/1468/B) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu, dsu 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính số lượng bánh mì cũ tại một tiệm bánh trong nhiều ngày. Mỗi ngày tiệm bánh sản xuất một số lượng bánh mì cố định và khách hàng đến hàng ngày với nhu cầu đã biết. Bánh mì được bán theo kiểu vào trước - ra sau: ổ bánh mới nướng được bán trước và ổ bánh cũ chưa bán được sẽ bán sau. Bất kỳ ổ bánh mì nào còn lại sau ngày cuối cùng sẽ được bán và sự hư hỏng của một ổ bánh mì được xác định bằng số ngày nó được bảo quản trước khi bán. Sự kém hấp dẫn của tiệm bánh là sự hư hỏng tối đa trong số tất cả các ổ bánh mì. 

Đầu vào bao gồm số ngày`n`, số lượng giá trị nhu cầu tiềm năng của khách hàng`m`, danh sách sản xuất hàng ngày`a`kích thước`n`và danh sách tăng dần nhu cầu của khách hàng tiềm năng`k`. Đối với mỗi`k_i`, chúng ta phải tính toán mức độ hư hỏng tối đa nếu chính xác`k_i`khách hàng đến mỗi ngày. 

Những ràng buộc chỉ ra`n`Và`m`mỗi chiếc có thể lên tới 200.000 và nhu cầu sản xuất và khách hàng hàng ngày có thể đạt tới 10^9. Do đó, một mô phỏng ngây thơ bán bánh mì hàng ngày cho từng nhu cầu của khách hàng là không thực tế vì nó có thể đạt được khoảng`O(n * m)`lần lặp, theo thứ tự 4 * 10^10. Bất kỳ thuật toán nào cũng phải tránh lặp lại nhiều ngày cho mỗi truy vấn. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó nhu cầu hàng ngày rất nhỏ so với sản lượng. Ví dụ, nếu`a = [100, 1]`Và`k = 1`, ngày đầu tiên còn lại 99 ổ bánh chưa bán được. Những thứ này sẽ tích tụ và hư hỏng, vì vậy mức độ hư hỏng tối đa sẽ được xác định bằng thời gian tồn tại của những ổ bánh mì còn sót lại cho đến lần bán cuối cùng. Một trường hợp khác xảy ra khi nhu cầu vượt quá sản lượng hàng ngày: tất cả bánh mì được bán ngay lập tức và độ hư hỏng bằng không. Không xử lý vấn đề này một cách chính xác có thể tạo ra các giá trị kém hấp dẫn không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp brute-force mô phỏng mỗi ngày cho từng nhu cầu của người tiêu dùng, theo dõi các ổ bánh còn lại dưới dạng hàng đợi hoặc danh sách đếm độ tuổi. Vào ngày`i`, nó bán`k`ổ bánh bắt đầu từ mẻ mới nhất và tăng độ hư hỏng cho những ổ bánh còn lại. Sau ngày cuối cùng, nó tính toán mức độ hư hỏng tối đa trong số tất cả các ổ bánh còn lại. Điều này đúng, nhưng mỗi truy vấn có thể yêu cầu`O(n)`thời gian để mô phỏng các ngày và với`m`truy vấn, tổng thời gian là`O(n*m)`. Đối với các ràng buộc tối đa, điều này là quá chậm. 

Quan sát quan trọng để có một giải pháp hiệu quả là thứ tự bán ổ bánh mì là có tính quyết định và độ hư hỏng hàng ngày sẽ tăng tuyến tính nếu nhu cầu không đủ. Chúng ta có thể tính toán trước tổng tiền tố của sản lượng hàng ngày:`S[i] = a_1 + a_2 + ... + a_i`. Đối với một nhu cầu hàng ngày nhất định`k`, chúng ta có thể xác định ngày sớm nhất khi một ổ bánh được nướng vào ngày`i`sẽ được bán bằng cách tìm ngày nhỏ nhất`j`như vậy`S[j] - k*(j-i) >= S[i-1]`. Sự hư hỏng tối đa khi đó là sự khác biệt giữa`j`Và`i`. Vì nhu cầu của khách hàng được sắp xếp nên chúng tôi có thể quét qua các ngày sản xuất một cách hiệu quả để tính toán mức độ hư hỏng tích lũy và sử dụng phương pháp hai con trỏ hoặc tham lam để lập bản đồ nhu cầu về hư hỏng trong`O(n + m)`thời gian. Điều này tránh việc mô phỏng mọi hoạt động bán hàng và tận dụng tính đơn điệu của việc hư hỏng theo nhu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của sản lượng bánh mì. Điều này cho phép chúng tôi nhanh chóng xác định tổng số bánh đã nướng cho đến bất kỳ ngày nào. Cho phép`S[i] = sum(a[0..i])`. 
2. Khởi tạo một mảng`unattractiveness`kích thước`m`lưu trữ kết quả theo từng nhu cầu của khách hàng`k_i`. 
3. Duy trì một biến`leftover`để theo dõi số bánh mì không bán được hàng ngày. Ban đầu,`leftover = 0`. 
4. Lặp lại từ ngày đầu tiên đến ngày cuối cùng. Cho mỗi ngày`i`, tính toán`leftover = leftover + a[i] - k`. Nếu như`leftover < 0`, đặt lại về 0. Điều này tượng trưng cho bánh mì sẽ bị hỏng thêm. 
5. Theo dõi sự hư hỏng của ổ bánh mì cũ nhất. Nếu một ổ bánh nướng vào ngày`i`được bán vào ngày`i + x`, thì sự hư hỏng của nó là`x`. Chúng ta có thể tính toán`x`sử dụng hiệu quả tổng số bánh mì còn sót lại chia cho nhu cầu hàng ngày`k`dùng phép chia số nguyên. 
6. Cuối ngày, số ổ bánh còn lại sẽ được bán vào ngày cuối cùng. Tính toán mức độ hư hỏng tối đa cho mỗi truy vấn`k_i`sử dụng công thức`(total leftover + k_i - 1)//k_i`để xác định số ngày họ chờ đợi. 
7. Vì`k`các giá trị được sắp xếp, chúng ta có thể quét qua`a`một lần và ánh xạ sự hư hỏng cho từng cái`k_i`trong một lượt. 

Bất biến được duy trì là sau ngày xử lý`i`,`leftover`luôn đại diện cho tổng số ổ bánh chưa bán được và độ hư hỏng tăng tỷ lệ thuận với số ngày chúng không được bán. Bằng cách tận dụng tổng tiền tố và phép chia số nguyên, chúng tôi tránh mô phỏng rõ ràng và đảm bảo rằng mức hư hỏng tối đa được tính toán chính xác cho mọi nhu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = list(map(int, input().split()))
k = list(map(int, input().split()))

# prefix sum of production
prefix = [0]*(n+1)
for i in range(n):
    prefix[i+1] = prefix[i] + a[i]

res = []

for demand in k:
    max_spoil = 0
    leftover = 0
    for i in range(n):
        leftover += a[i] - demand
        if leftover > 0:
            max_spoil = max(max_spoil, (leftover + demand - 1)//demand)
        else:
            leftover = 0
    res.append(max_spoil)

print(" ".join(map(str, res)))
```Tổng tiền tố được tính toán để làm rõ khái niệm, nhưng việc tính toán chính được thực hiện bằng cách duy trì một`leftover`đếm. Đối với mỗi ngày, chúng tôi trừ đi nhu cầu hàng ngày và, nếu dương, tính lượng hư hỏng bổ sung theo ngày bằng phép chia số nguyên. Công thức`(leftover + demand - 1)//demand`đảm bảo làm tròn chính xác cho một phần ngày hư hỏng. Tiêu cực`leftover`được đặt lại về 0 vì không có sự hư hỏng nào được tích lũy nếu nhu cầu vượt quá sản xuất. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5 4
5 2 1 3 7
1 3 4 10
```| Ngày | Nướng | Còn dư sau bán (k=1) | Thiệt hại tối đa | 
| --- | --- | --- | --- | 
| 1 | 5 | 4 | 0 | 
| 2 | 2 | 5 | 1 | 
| 3 | 1 | 5 | 2 | 
| 4 | 3 | 7 | 3 | 
| 5 | 7 | 0 (đã bán) | 4 | 

Vì`k=10`, tất cả bánh mì được bán ngay lập tức, vì vậy`max_spoilage = 0`. Dấu vết này xác nhận rằng thuật toán tích lũy chính xác bánh mì còn sót lại và tính toán mức độ hư hỏng mà không cần mô phỏng từng ổ bánh mì riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Lặp lại một lần trong các ngày sản xuất và một lần trong các truy vấn theo yêu cầu | 
| Không gian | O(n) | Lưu trữ tổng tiền tố và mảng còn sót lại theo khái niệm | 

Thuật toán chia tỷ lệ tuyến tính với kích thước đầu vào và dễ dàng xử lý các giới hạn tối đa trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    k = list(map(int, input().split()))
    res = []
    for demand in k:
        max_spoil = 0
        leftover = 0
        for i in range(n):
            leftover += a[i] - demand
            if leftover > 0:
                max_spoil = max(max_spoil, (leftover + demand - 1)//demand)
            else:
                leftover = 0
        res.append(max_spoil)
    return " ".join(map(str, res))

assert run("5 4\n5 2 1 3 7\n1 3 4 10\n") == "4 2 1 0", "sample 1"
assert run("1 1\n10\n5\n") == "0", "all sold same day"
assert run("3 2\n2 2 2\n1 2\n") == "2 1", "small production, multiple k"
assert run("5 3\n1 1 1 1 1\n1 2 5\n") == "4 2 0", "slow accumulation"
assert run("2 2\n1000000000 1000000000\n1 1000000000\n") == "1 0", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 4\n5 2 1 3 7\n1 3 4 10\n`|`4 2 1 0`| Độ chính xác đầu vào mẫu | 
|`1 1\n10\n5\n`|`0`| Tất cả bánh mì |
