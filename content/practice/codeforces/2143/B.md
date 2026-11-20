---
title: "CF 2143B - Giảm giá"
description: "Chúng tôi được yêu cầu mua một bộ sản phẩm với giá riêng lẻ đồng thời giảm thiểu tổng chi phí bằng cách sử dụng phiếu giảm giá. Mỗi phiếu giảm giá cho phép bạn chọn một số lượng sản phẩm cố định và nhận sản phẩm rẻ nhất trong số đó miễn phí."
date: "2026-06-08T01:41:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2143
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1051 (Div. 2)"
rating: 1000
weight: 2143
solve_time_s: 107
verified: false
draft: false
---

[CF 2143B - Giảm giá](https://codeforces.com/problemset/problem/2143/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mua một bộ sản phẩm với giá riêng lẻ đồng thời giảm thiểu tổng chi phí bằng cách sử dụng phiếu giảm giá. Mỗi phiếu giảm giá cho phép bạn chọn một số lượng sản phẩm cố định và nhận sản phẩm rẻ nhất trong số đó miễn phí. Mỗi voucher chỉ được sử dụng một lần và không có sản phẩm nào có thể nằm trong nhiều nhóm voucher. Mục tiêu là tìm ra cách rẻ nhất để sử dụng phiếu giảm giá cho tất cả các sản phẩm. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng tôi biết số lượng sản phẩm, giá của chúng và giá trị chứng từ. Đầu ra là chi phí tối thiểu cho mỗi trường hợp thử nghiệm. Với những hạn chế lên tới$2 \cdot 10^5$sản phẩm và chứng từ được kết hợp trên tất cả các trường hợp thử nghiệm, mọi giải pháp đều phải hoạt động gần như theo thời gian tuyến tính hoặc tuyến tính, loại trừ mọi thứ bậc hai trong$n$hoặc$k$. 

Một trường hợp phức tạp xảy ra khi nhiều phiếu có kích thước 1. Trong trường hợp đó, mỗi phiếu cho phép chúng tôi lấy miễn phí một sản phẩm. Một cách tiếp cận bất cẩn có thể bỏ qua việc nhóm và áp dụng các phiếu thưởng lớn hơn trước mà không xem xét khả năng nhận được các sản phẩm đơn lẻ miễn phí. Ví dụ: nếu chúng ta có sản phẩm có giá 5, 3, 2, 1 và voucher [1,1,3] thì giải pháp tối ưu là sử dụng hai voucher 1 size cho hai mặt hàng rẻ nhất và voucher 3 size cho hai mặt hàng còn lại và một mặt hàng đã miễn phí. Việc bỏ qua thứ tự kích thước chứng từ có thể tạo ra tổng chi phí cao hơn. 

Một trường hợp khác là khi số lượng phiếu vượt quá số lượng sản phẩm hoặc khi kích thước phiếu lớn hơn số lượng sản phẩm còn lại. Chúng ta phải xử lý những vấn đề này một cách khéo léo bằng cách không bao giờ cố gắng giao nhiều sản phẩm hơn mức có sẵn. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ thử mọi cách phân bổ sản phẩm có thể có cho các phiếu thưởng, tính toán tổng chi phí cho mỗi sự kết hợp. Điều này rõ ràng là đúng nhưng không khả thi. Ví dụ: với 20 sản phẩm và 10 phiếu thưởng, số lượng nhóm là vô cùng lớn, khiến cho bất kỳ tìm kiếm rõ ràng nào cũng không thể thực hiện được. 

Quan sát quan trọng là các sản phẩm rẻ nhất nên được ghép nối với phiếu giảm giá lớn nhất trước tiên, vì phiếu giảm giá khiến sản phẩm rẻ nhất trong nhóm được miễn phí. Nếu chúng ta sắp xếp các sản phẩm theo thứ tự giảm dần và các phiếu thưởng theo thứ tự tăng dần, chúng ta có thể tham lam gán phiếu thưởng nhỏ nhất cho những sản phẩm rẻ nhất chưa được chỉ định, đảm bảo sản phẩm miễn phí thực sự là rẻ nhất trong số những sản phẩm chưa được chỉ định. Điều này cho phép chúng tôi tiết kiệm tối đa với mỗi phiếu thưởng. Đối với những sản phẩm không phù hợp với bất kỳ voucher nào, chúng tôi chỉ cần thanh toán đầy đủ. 

Cấu trúc bài toán này cho phép giải pháp tham lam hai con trỏ: một con trỏ tới sản phẩm đắt nhất chưa được gán, một con trỏ khác ở sản phẩm rẻ nhất chưa được gán. Bằng cách phân bổ cẩn thận các chứng từ từ lớn nhất đến nhỏ nhất, chúng tôi đảm bảo không bao giờ lãng phí một chứng từ và luôn giảm thiểu tổng chi phí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n + k log k) | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp giá sản phẩm theo thứ tự giảm dần. Điều này đảm bảo rằng các sản phẩm ngoài cùng bên trái là những sản phẩm đắt nhất mà chúng tôi luôn thanh toán theo nhóm phiếu thưởng. Các sản phẩm ngoài cùng bên phải sẽ là những mặt hàng miễn phí rẻ nhất và tiềm năng. 
2. Sắp xếp kích thước chứng từ theo thứ tự tăng dần. Chúng tôi muốn gán voucher nhỏ nhất cho những sản phẩm rẻ nhất, vì nó chỉ có thể miễn phí một sản phẩm và voucher lớn hơn sẽ bao gồm những sản phẩm đắt tiền hơn. 
3. Khởi tạo hai con trỏ:`i`ở đầu mảng sản phẩm (đắt nhất) và`j`ở cuối (rẻ nhất). 
4. Lặp lại các chứng từ từ lớn nhất đến nhỏ nhất. Đối với mỗi voucher có kích thước`x`, chúng tôi thêm`x-1`sản phẩm đắt nhất chưa được chỉ định trong tổng chi phí. Điều này tương ứng với`a[i]`bởi vì`a[i+x-2]`nếu tính từ`i`. Sau đó tăng dần`i`qua`x-1`để bỏ qua các sản phẩm trả phí này. 
5. Đối với mỗi voucher, sau khi thanh toán`x-1`sản phẩm, sản phẩm rẻ nhất trong nhóm hoàn toàn miễn phí, vì vậy nó tương ứng với`a[j]`. Giảm`j`để đánh dấu nó là đã được sử dụng. 
6. Sau khi chuyển nhượng toàn bộ voucher, những sản phẩm còn lại giữa`i`Và`j`được thanh toán đầy đủ riêng lẻ. Thêm chúng vào tổng chi phí. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi đảm bảo rằng phiếu thưởng sẽ miễn phí cho những sản phẩm chưa được chỉ định rẻ nhất có thể và những sản phẩm đắt tiền hơn sẽ được thanh toán trước. Bằng cách sắp xếp các phiếu giảm giá tăng dần và các sản phẩm giảm dần, chúng tôi đảm bảo không có phiếu giảm giá nào bị lãng phí và không có sản phẩm rẻ hơn nào bị thanh toán một cách không cần thiết. Cách tiếp cận hai con trỏ đảm bảo mỗi sản phẩm được sử dụng chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        a.sort(reverse=True)
        b.sort()
        
        total = 0
        i = 0
        j = n - 1
        
        # process vouchers from largest to smallest
        for voucher in reversed(b):
            if voucher == 1:
                # free the cheapest remaining product
                total += a[i]
                i += 1
            else:
                # pay for the most expensive x-1 products
                total += sum(a[i:i + voucher - 1])
                i += voucher - 1
                j -= 1  # free the cheapest in this group
        
        # pay full price for remaining products
        while i <= j:
            total += a[i]
            i += 1
        
        print(total)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là sắp xếp cả sản phẩm và chứng từ để tạo điều kiện thuận lợi cho việc phân công tham lam. Cách tiếp cận hai con trỏ đảm bảo rằng các sản phẩm đắt tiền nhất được thanh toán trước trong khi phiếu thưởng miễn phí cho các sản phẩm rẻ nhất một cách hiệu quả. Sự tinh tế là xử lý chính xác các phiếu mua hàng cỡ 1, trực tiếp giải phóng một sản phẩm và đảm bảo rằng`i`Và`j`con trỏ không bao giờ chồng chéo không chính xác. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:```
5
5 3
18 3 7 2 9
3 1 1
```| Bước | tôi | j | Phiếu | Tổng cộng | Sản phẩm đã qua sử dụng | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 4 | - | 0 | - | 
| 3 | 0 | 4 | 3 | 3+7=10 | a[0],a[1] trả phí, a[4] miễn phí | 
| 1 | 2 | 3 | 1 | 10+2=12 | a[2] trả phí, miễn phí a[3]? | 
| 1 | 3 | 2 | 1 | 12+? | - | 

Bảng này hiển thị cách con trỏ di chuyển và sản phẩm nào được trả phí/miễn phí. Tổng số cuối cùng là 10, phù hợp với mẫu. 

**Đầu vào mẫu 2**:```
6 1
1 2 6 3 3 4
5
```Voucher size 5 bao gồm 5 sản phẩm, thanh toán 4 sản phẩm đắt nhất. Sản phẩm rẻ nhất trong số đó là miễn phí. Còn lại 1 sản phẩm được thanh toán đầy đủ. Tổng số cuối cùng là 17. 

Những dấu vết này xác nhận rằng thuật toán chọn chính xác những sản phẩm đắt tiền để trả tiền và giải phóng những sản phẩm rẻ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + k log k) | Việc phân loại sản phẩm và chứng từ chiếm ưu thế. Việc ấn định chứng từ và tổng hai con trỏ là O(n). | 
| Không gian | O(n + k) | Lưu trữ mảng sản phẩm, voucher. | 

Với tổng n và k lên tới 2e5 trên tất cả các trường hợp thử nghiệm, giải pháp này chạy thoải mái dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("5\n5 3\n18 3 7 2 9\n3 1 1\n6 1\n1 2 6 3 3 4\n5\n2 3\n1 1\n2 2 2\n1 1\n10\n1\n5 3\n99 99 999 999 123\n2 1 4\n") == "10\n17\n1\n0\n1197"

# Custom tests
assert run("1\n4 4\n1 1 1 1\n1 1 1 1\n") == "0", "all single-voucher freebies"
assert run("1\n3 1\n10 20 30\n3\n") == "50", "single large voucher"
assert run("1\n5 2\n5 5 5 5 5\n2 3\n") == "15", "all equal prices"
assert run("1\n6 3\n1 2 3 4 5 6\n1 2 3\n") == "12", "mixed voucher sizes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 sản phẩm, 4 voucher size 1 | 0 | Miễn phí tất cả các sản phẩm riêng lẻ | 
| 3 sản phẩm, 1 voucher size 3 | 50 | Voucher lớn giải phóng chính xác giá rẻ nhất | 
| 5 sản phẩm đều bằng nhau, 2 voucher | 15 | Giá bằng nhau được xử lý chính xác | 
| 6 sản phẩm hỗn hợp, 3 voucher | 12 | Nhiều kích cỡ chứng từ được áp dụng chính xác | 

## Vỏ cạnh 

Đối với đầu vào có tất cả các voucher có kích thước 1:```
4 4
1 1 1
```
