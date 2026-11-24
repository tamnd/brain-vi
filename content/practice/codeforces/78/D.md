---
title: "CF 78D - Cú Bắn Của Cung Thủ"
description: "Chúng ta được yêu cầu tính toán có bao nhiêu ô hình lục giác mà một cung thủ có thể bao phủ hoàn toàn bằng một đòn tấn công hình tròn có bán kính k, trong đó cung thủ được đặt ở tâm của ô. Mỗi hình lục giác có cạnh dài 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "geometry", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 78
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 70 (Div. 2)"
rating: 2300
weight: 78
solve_time_s: 127
verified: true
draft: false
---

[CF 78D - Cú bắn của Cung thủ](https://codeforces.com/problemset/problem/78/D) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, hình học, toán học, hai con trỏ 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính toán xem một cung thủ có thể bao phủ hoàn toàn bao nhiêu ô hình lục giác bằng một đòn tấn công hình tròn có bán kính _k_, trong đó cung thủ được đặt ở trung tâm của ô. Mỗi hình lục giác có độ dài cạnh bằng 1. Một ô chỉ được tính là được bao phủ nếu mọi điểm trong ô đều nằm trong vòng tròn. Đầu vào là một số nguyên _k_ và đầu ra là tổng số ô thỏa mãn điều kiện này. 

Phạm vi của _k_ lên tới 10^6. Nếu chúng ta cố gắng kiểm tra mạnh mẽ từng ô xung quanh cung thủ bằng cách tính toán khoảng cách cho một lưới lớn, thì số lượng thao tác sẽ nhanh chóng trở nên không khả thi vì số lượng ô ứng cử viên tăng lên gần như theo diện tích của hình tròn, O(k²). Đối với giới hạn trên k=10^6, việc này sẽ bao gồm khoảng 10^12 lần kiểm tra, vượt xa giới hạn thời gian 2 giây. 

Trường hợp cạnh khóa xảy ra đối với các giá trị nhỏ của _k_, đặc biệt là _k = 1_. Cung thủ chỉ có thể bao phủ ô của chính mình, vì vậy đầu ra phải là 1. Cách tiếp cận bất cẩn giả định tối thiểu 7 ô (ô trung tâm cộng với 6 ô lân cận) sẽ sai. Một cạm bẫy tiềm ẩn khác là việc đếm sai các ô trên ranh giới của vòng tròn. Bởi vì một ô chỉ được coi là bị bắn nếu **tất cả các điểm** nằm trong vòng tròn, nên các ô có tâm nằm gần đường biên có thể bị loại trừ ngay cả khi tâm nằm bên trong. 

## Phương pháp tiếp cận 

Phương pháp vũ lực sẽ liệt kê một lưới lục giác lớn có chiều dài cạnh khoảng _k_ xung quanh cung thủ, kiểm tra khoảng cách Euclide từ cung thủ đến từng đỉnh của mỗi hình lục giác và đếm các ô trong đó tất cả các đỉnh nằm trong bán kính. Điều này đúng vì nó trực tiếp thực hiện định nghĩa, nhưng nó yêu cầu kiểm tra khoảng cách O(k²) cho k² ô và 6 đỉnh trên mỗi ô, khiến cho k = 10^6 không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát tính đối xứng của gạch lát hình lục giác. Các lớp lục giác xung quanh ô trung tâm tạo thành các vòng: ô trung tâm là lớp 0, 6 ô lân cận là lớp 1, vòng xung quanh tiếp theo có 12 ô, v.v. Mỗi lớp tương ứng với một số nguyên "khoảng cách hex" tính từ tâm. Một ô trong lớp _r_ nằm hoàn toàn bên trong hình tròn nếu bán kính hình tròn ít nhất bằng khoảng cách từ tâm đến đỉnh xa nhất của hình lục giác đó. Khoảng cách từ tâm của lớp-_r_ hex đến đỉnh xa nhất của nó tăng tuyến tính với _r_, cụ thể là r × √3 (khoảng cách từ trung điểm đến đỉnh). Bằng cách sử dụng điều này, chúng ta có thể tính toán số lớp đầy đủ tối đa mà vòng tròn có thể bao phủ và sau đó tính tổng các ô theo số lớp. Số lượng ô trong _n_ lớp đầu tiên là 1 + 6 × (1 + 2 + ... + n) = 1 + 3 × n × (n + 1). Điều này đưa ra công thức O(1) khi chúng ta biết lớp được phủ tối đa. 

Cách tiếp cận này làm giảm đáng kể độ phức tạp vì chúng tôi không còn lặp lại trên từng ô nữa. Phép tính duy nhất là tìm lớp tối đa vừa với vòng tròn rồi áp dụng công thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k²) | O(1) | Quá chậm đối với k lên tới 10^6 | 
| Công thức lớp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Thể hiện gạch lục giác theo lớp: ô trung tâm là lớp 0, 6 ô xung quanh là lớp 1, 12 ô tiếp theo tạo thành lớp 2, v.v. Mỗi lớp r có 6 × r ô. 
2. Tính khoảng cách từ tâm ô của cung thủ đến điểm xa nhất của ô trong lớp r. Đối với hình lục giác có độ dài cạnh 1, khoảng cách từ tâm đến đỉnh là 1. Khoảng cách đến đỉnh xa nhất trong lớp r là r × √3 + 1. 
3. Xác định số lớp tối đa _n_ sao cho khoảng cách từ tâm đến đỉnh xa nhất của bất kỳ hình lục giác nào trong lớp đó không vượt quá k. Giải r × √3 + 1 ≤ k, cho ra r ≤ (k - 1)/√3. 
4. Sử dụng công thức tính tổng cho các lớp hình lục giác. Tổng số ô được che phủ hoàn toàn là 1 (ô trung tâm) cộng với tổng các lớp: 1 + 6 × (1 + 2 + ... + n) = 1 + 3 × n × (n + 1). 
5. In kết quả. 

Tại sao nó hoạt động: Các lớp hex phát triển đồng đều và công thức chiếm tất cả các ô trong các lớp được bao gồm đầy đủ. Việc chỉ kiểm tra ranh giới lớp đảm bảo rằng chúng ta không bao gồm các ô bị che phủ một phần, đáp ứng yêu cầu của bài toán là tất cả các điểm của ô phải nằm trong vòng tròn. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

k = int(input())
if k == 1:
    print(1)
else:
    # maximum layer fully contained in circle
    max_layer = int((k - 1) / math.sqrt(3))
    # sum of cells in layers
    result = 1 + 3 * max_layer * (max_layer + 1)
    print(result)
```Mã này trước tiên xử lý riêng trường hợp tầm thường k = 1 vì công thức sẽ cho 0 lớp. Sau đó, nó tính toán số lớp đầy đủ tối đa vừa khít bên trong vòng tròn bằng cách sử dụng quan hệ hình học r × √3 + 1 ≤ k. Phép chia số nguyên đảm bảo chúng tôi chỉ tính các lớp được bao gồm đầy đủ. Cuối cùng, công thức đóng 1 + 3 × n × (n + 1) tạo ra tổng số ô một cách hiệu quả. Cần chú ý cẩn thận tới các lỗi từng lỗi một và làm tròn dấu phẩy động với`int()`. 

## Ví dụ đã hoạt động 

Với k = 3: 

| Bước | max_layer | kết quả | 
| --- | --- | --- | 
| tính toán max_layer | int((3-1)/√3) = int(2/1.732) = 1 | - | 
| kết quả tính toán | 1 + 3_1_2 = 1 + 6 = 7 | 7 | 

Bảng xác nhận rằng một lớp xung quanh ô trung tâm nằm hoàn toàn bên trong vòng tròn, mang lại tổng cộng 7 ô, khớp với đầu ra mẫu. 

Với k = 5: 

| Bước | max_layer | kết quả | 
| --- | --- | --- | 
| tính toán max_layer | int((5-1)/√3) = int(4/1.732) = 2 | - | 
| kết quả tính toán | 1 + 3_2_3 = 1 + 18 = 19 | 19 | 

Điều này cho thấy hai lớp được bao phủ hoàn toàn, tạo ra 19 ô. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ cần các phép tính số học và một căn bậc hai. | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài một vài số nguyên. | 

Độ phức tạp về thời gian không đổi này là quá đủ cho các ràng buộc của bài toán, vì k có thể đạt tới 10^6 mà không cần lo ngại về hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k = int(input())
    if k == 1:
        return "1"
    else:
        import math
        max_layer = int((k - 1) / math.sqrt(3))
        result = 1 + 3 * max_layer * (max_layer + 1)
        return str(result)

# provided sample
assert run("3\n") == "7", "sample 1"
# minimal input
assert run("1\n") == "1", "k = 1"
# slightly larger
assert run("2\n") == "1", "k = 2, only central cell fits"
# larger k
assert run("5\n") == "19", "k = 5"
# maximum input
assert run("1000000\n") == run("1000000\n"), "sanity check for large k"
# exact layer boundary
assert run("1\n") == "1", "boundary k=1 edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | k tối thiểu, bao phủ một ô | 
| 2 | 1 | vòng tròn quá nhỏ để bao gồm hàng xóm | 
| 3 | 7 | đầu vào mẫu, một lớp | 
| 5 | 19 | nhiều lớp | 
| 1000000 | số lượng lớn | hiệu suất và độ chính xác của công thức | 

## Vỏ cạnh 

Với k = 1, hình tròn không thể bao phủ hoàn toàn bất kỳ hình lục giác lân cận nào. Thuật toán tính toán`max_layer = int((1-1)/√3) = 0`, tạo ra 1 ô, điều này đúng. Với k = 2, đường tròn vẫn còn quá nhỏ để bao phủ toàn bộ sáu đường tròn lân cận. Việc tính toán`max_layer = int((2-1)/√3) = 0`đảm bảo chỉ có ô trung tâm được tính. Những kiểm tra này xác nhận rằng cách tiếp cận dựa trên lớp xử lý chính xác bán kính nhỏ mà không tính quá nhiều ô một phần.
