---
title: "CF 1945C - Nhà Tả và Phải"
description: "Chúng ta có một ngôi làng có n ngôi nhà xếp thành một hàng. Mỗi người dân có một sở thích riêng về phía nào của đường mà họ muốn sống: trái (0) hoặc phải (1)."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1945
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 935 (Div. 3)"
rating: 1200
weight: 1945
solve_time_s: 48
verified: true
draft: false
---

[CF 1945C - Nhà trái và phải](https://codeforces.com/problemset/problem/1945/C) 

**Đánh giá:** 1200 
**Tags:** vũ lực 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một ngôi làng với`n`những ngôi nhà thẳng hàng. Mỗi người dân có một sở thích riêng về phía nào của con phố mà họ muốn sống: bên trái (`0`) hoặc phải (`1`). Các nhà quy hoạch làng muốn đặt một con đường ở đâu đó giữa các ngôi nhà để ít nhất một nửa số cư dân ở mỗi bên hài lòng. Một bên hài lòng nếu số lượng cư dân được hưởng quyền ưu tiên của bên đó ít nhất bằng một nửa số nhà ở bên đó. 

Thực tế, vấn đề yêu cầu chúng ta tìm chỉ mục phân vùng`i`(con đường ở sau nhà`i`) sao cho đoạn có độ dài bên trái`i`có ít nhất`ceil(i/2)`số không và đoạn dài bên phải`n-i`có ít nhất`ceil((n-i)/2)`những cái đó. Trong số tất cả hợp lệ`i`, chúng ta nên chọn nơi gần giữa làng nhất, tức là giảm thiểu`abs(n/2 - i)`, với các mối quan hệ bị phá vỡ bởi cái nhỏ hơn`i`. 

Các ràng buộc cho phép lên tới 300.000 ngôi nhà trong tất cả các trường hợp thử nghiệm và lên tới 20.000 trường hợp thử nghiệm. Một giải pháp đơn giản giúp kiểm tra mọi phần phân tách bằng cách quét toàn bộ các phân đoạn bên trái và bên phải sẽ là O(n^2) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất, quá chậm. Chúng tôi cần giải pháp O(n) cho mỗi trường hợp thử nghiệm hoặc tốt hơn. Các trường hợp cạnh bao gồm tất cả các ngôi nhà thích cùng một phía, đường ở ranh giới (trước ngôi nhà đầu tiên hoặc sau ngôi nhà cuối cùng), hoặc các đoạn có kích thước lẻ và chẵn ảnh hưởng đến trần của một nửa đoạn đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét từng sự phân chia có thể`i`từ 0 đến`n`và đếm số số 0 ở đoạn bên trái và số 0 ở đoạn bên phải. Nếu số đếm thỏa mãn điều kiện trần thì chúng tôi coi là hợp lệ. Cách tiếp cận này hoạt động hợp lý nhưng yêu cầu thao tác O(n^2) đối với các chuỗi lớn vì mỗi lần phân tách yêu cầu quét một phần của mảng, khiến nó không thực tế đối với`n`lên đến 3·10^5. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể tính toán trước các tổng tiền tố. Cho phép`prefix_zeros[j]`là số số 0 ở số đầu tiên`j`những ngôi nhà. Khi đó số số 0 ở đoạn độ dài bên trái`i`là`prefix_zeros[i]`. Số cái ở đoạn dài bên phải`n-i`có thể được bắt nguồn như`(total_ones - prefix_ones[i])`hoặc tương đương`((n - prefix_zeros[n]) - (i - prefix_zeros[i]))`, nhưng đơn giản hơn là tính tổng tiền tố cho các số đó. Với các tổng tiền tố này, việc kiểm tra từng phần phân tách sẽ trở thành O(1) và chúng ta chỉ cần một lần duyệt qua mảng để tìm ra phần phân chia tối ưu. 

Nhận xét rằng điều kiện thỏa mãn chỉ phụ thuộc vào số lượng số 0 và số 1 cho phép chúng ta chuyển đổi các bất đẳng thức trần thành các so sánh số đơn giản bằng cách sử dụng tổng tiền tố. Điều này làm giảm lực lượng bậc hai có khả năng thành thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tổng tiền tố / Quét tuyến tính | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`và chuỗi`a`. Chuyển thành`a`thành một danh sách các số nguyên để tính toán dễ dàng hơn. 
2. Tính tổng tiền tố của các số 0,`prefix_zeros`, Ở đâu`prefix_zeros[i]`là số số 0 ở số đầu tiên`i`những ngôi nhà. 
3. Tính tổng số đơn vị`total_ones`, bằng cách đếm các số trong toàn bộ mảng hoặc bằng cách trừ các số 0 khỏi`n`. 
4. Khởi tạo các biến để theo dõi chỉ số tốt nhất`best_i`và khoảng cách tối thiểu đến trung tâm`min_dist = n`. Chúng tôi sẽ cố gắng giảm thiểu`abs(n/2 - i)`. 
5. Lặp lại tất cả các chỉ số phân chia có thể`i`từ 0 đến`n`. Với mỗi lần chia, đoạn bên trái có độ dài`i`, độ dài đoạn bên phải`n-i`. 
6. Tính số 0 ở bên trái:`left_zeros = prefix_zeros[i]`. Tính số số ở bên phải:`right_ones = total_ones - (i - left_zeros)`. 
7. Kiểm tra xem`left_zeros >= ceil(i/2)`Và`right_ones >= ceil((n-i)/2)`. Sử dụng số học số nguyên cho trần nhà:`ceil(x/2) = (x+1)//2`. 
8. Nếu cả hai điều kiện đều đúng, hãy tính`dist = abs(n/2 - i)`. Nếu như`dist < min_dist`hoặc`dist == min_dist`Và`i < best_i`, cập nhật`best_i`Và`min_dist`. 
9. Sau vòng lặp, xuất ra`best_i`. 

Tại sao nó hoạt động: Tổng tiền tố cho chúng tôi biết ngay lập tức có bao nhiêu số 0 ở bất kỳ phân đoạn bên trái nào và bao nhiêu số 1 ở bất kỳ phân đoạn bên phải nào. Điều kiện trần được xử lý đúng cách với số học số nguyên. Bằng cách quét tất cả`i`từ 0 đến`n`và theo dõi điểm gần nhất ở giữa, chúng tôi đảm bảo giải pháp đáp ứng cả điều kiện công bằng và yêu cầu khoảng cách tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = input().strip()
        arr = [int(ch) for ch in a]

        prefix_zeros = [0]*(n+1)
        for i in range(n):
            prefix_zeros[i+1] = prefix_zeros[i] + (1 if arr[i] == 0 else 0)
        total_ones = n - prefix_zeros[n]

        best_i = 0
        min_dist = n
        for i in range(n+1):
            left_len = i
            right_len = n - i
            left_zeros = prefix_zeros[i]
            right_ones = total_ones - (right_len - (total_ones - (prefix_zeros[n] - prefix_zeros[i])))
            right_ones = total_ones - (i - left_zeros)

            if left_zeros >= (left_len + 1)//2 and right_ones >= (right_len + 1)//2:
                dist = abs(n/2 - i)
                if dist < min_dist or (dist == min_dist and i < best_i):
                    best_i = i
                    min_dist = dist
        print(best_i)

if __name__ == "__main__":
    solve()
```Trong quá trình triển khai này, chúng tôi tính tổng tiền tố cho các số 0. Số lượng những cái ở đoạn bên phải được tính bằng cách lấy tổng số những cái được đếm ở bên trái trừ đi. các`(length + 1)//2`thủ thuật xử lý chính xác việc chia trần. Lặp lại từ 0 đến`n`đảm bảo chúng tôi xem xét tất cả các vị trí có thể có của con đường, kể cả trước ngôi nhà đầu tiên và sau ngôi nhà cuối cùng. Logic phá vỡ ràng buộc đảm bảo chúng tôi chọn chỉ số nhỏ hơn nếu hai vị trí gần trung tâm như nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1: 

Chuỗi đầu vào`101`với`n=3`. 

| tôi | left_len | đúng_len | left_zeros | đúng_ones | trần(left_len/2) | trần(right_len/2) | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 3 | 0 | 2 | 0 | 2 | Có | 
| 1 | 1 | 2 | 0 | 2 | 1 | 1 | Không | 
| 2 | 2 | 1 | 1 | 1 | 1 | 1 | Có | 
| 3 | 3 | 0 | 1 | 0 | 2 | 0 | Không | 

Gần trung tâm nhất`n/2 = 1.5`là`i=2`. đầu ra`2`. 

### Ví dụ 2: 

Chuỗi đầu vào`010111`với`n=6`. 

| tôi | left_len | đúng_len | left_zeros | đúng_ones | trần(left_len/2) | trần(right_len/2) | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 6 | 0 | 4 | 0 | 3 | Có | 
| 1 | 1 | 5 | 1 | 4 | 1 | 3 | Có | 
| 2 | 2 | 4 | 1 | 3 | 1 | 2 | Có | 
| 3 | 3 | 3 | 2 | 2 | 2 | 2 | Có | 
| 4 | 4 | 2 | 2 | 2 | 2 | 1 | Có | 
| 5 | 5 | 1 | 2 | 1 | 3 | 1 | Không | 
| 6 | 6 | 0 | 3 | 0 | 3 | 0 | Có | 

Gần giữa nhất`3`là chỉ số`3`, nhưng tie-break thích chỉ số nhỏ hơn`3`. đầu ra`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tính toán tổng tiền tố là O(n), phân tách quét là O(n) | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Mảng tổng tiền tố có độ dài n+1 | 

Thuật toán tuyến tính theo số lượng nhà trên mỗi trường hợp thử nghiệm. Với tổng số`n`trên tất cả các trường hợp thử nghiệm 3·10^5, điều này dễ dàng phù hợp với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin
```
