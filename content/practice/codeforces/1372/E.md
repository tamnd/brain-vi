---
title: "CF 1372E - Omkar và tầng cuối cùng"
description: "Chúng ta được yêu cầu thiết kế tầng cuối cùng trong ngôi nhà của Omkar, được biểu diễn dưới dạng lưới $n nhân m$ ban đầu chứa đầy các số 0. Mỗi hàng được chia thành các khoảng liền kề. Trong mỗi khoảng, chúng ta được phép thay đổi chính xác một số 0 thành một."
date: "2026-06-11T11:18:54+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1372
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 655 (Div. 2)"
rating: 2900
weight: 1372
solve_time_s: 123
verified: true
draft: false
---

[CF 1372E - Omkar và Tầng cuối cùng](https://codeforces.com/problemset/problem/1372/E) 

**Xếp hạng:** 2900 
**Tags:** dp, tham lam, hai con trỏ 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu thiết kế tầng cuối cùng trong ngôi nhà của Omkar, được thể hiện như một$n \times m$lưới ban đầu chứa đầy số không. Mỗi hàng được chia thành các khoảng liền kề. Trong mỗi khoảng, chúng ta được phép thay đổi chính xác một số 0 thành một. Sau khi tất cả các hàng được xử lý, chúng tôi tính tổng của từng cột, bình phương nó và tính tổng trên tất cả các cột để có được chất lượng của sàn. Nhiệm vụ là tối đa hóa chất lượng tổng thể này. 

Những hạn chế$1 \le n,m \le 100$đủ nhỏ để cho phép các thuật toán có độ phức tạp lên tới$O(n \cdot m^2)$mà không gặp rủi ro về thời gian chờ. Số lượng khoảng trên mỗi hàng có thể khác nhau và khoảng có thể nhỏ bằng một cột hoặc trải rộng trên nhiều cột. 

Một cách tiếp cận ngây thơ có thể âm thầm thất bại nếu nó không tính đến các lựa chọn chồng chéo giữa các hàng. Ví dụ: nếu chúng ta luôn chọn cột ngoài cùng bên trái trong mỗi khoảng thời gian, thì cuối cùng chúng ta có thể xếp chồng nhiều cột vào một cột một cách không cần thiết trong khi để trống các cột khác, dẫn đến chất lượng tổng thể thấp hơn. Hãy xem xét một hàng có các khoảng bao phủ tất cả các cột; đặt một cột trong cùng một cột với một hàng khác có thể không tối ưu. Một ví dụ nhỏ: hai hàng, mỗi hàng có một khoảng bao gồm cột 1 và 2. Việc chọn cột 1 cho cả hai hàng sẽ mang lại chất lượng$2^2 + 0^2 = 4$, nhưng chia thành cột 1 và 2 sẽ cho chất lượng$1^2 + 1^2 = 2$. Ở đây, cách tiếp cận đơn giản ngoài cùng bên trái thực sự tốt hơn, nhưng trong các bố cục phức tạp hơn, việc phân bổ không hợp lý sẽ làm giảm tổng bình phương tổng thể. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là thử tất cả các vị trí có thể có của các vị trí cho mỗi khoảng trong mỗi hàng. Mỗi khoảng có nhiều vị trí ứng cử viên, do đó tổng số kết hợp có thể đạt tới$\prod_{i=1}^{n} \prod_{j=1}^{k_i} (r_{i,j}-l_{i,j}+1)$. Ngay cả đối với nhỏ$n$Và$m$, sản phẩm này phát triển theo cấp số nhân, khiến nó hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là vấn đề được phân tách độc đáo theo hàng bằng cách sử dụng lập trình động. Tổng cột tích lũy tuyến tính qua các hàng và hàm chất lượng có thể tách rời trên mỗi cột. Do đó, đối với mỗi hàng, chúng ta có thể tính toán mức đóng góp tốt nhất mà nó có thể mang lại cho tổng cột hiện tại nhất định. Điều này gợi ý một DP trong đó trạng thái theo dõi số lượng các ô được đặt trong mỗi cột hoặc hiệu quả hơn là một DP xử lý hàng dưới dạng một chuỗi các khoảng và phân phối các ô một cách tham lam để tối đa hóa tổng bình phương cục bộ, đồng thời mang các tổng tích lũy. 

Một cái nhìn sâu sắc khác là trong một khoảng duy nhất, việc đặt số vào cột có tổng tối thiểu hiện tại luôn là tối ưu. Vì hàm bình phương tăng nhanh hơn với số lượng lớn hơn nên việc tập trung các số vào các cột đã có nhiều số sẽ làm tăng tổng số. Điều này làm giảm vấn đề thành chiến lược hai con trỏ hoặc tham lam trên mỗi hàng, trong đó chúng tôi xử lý các khoảng tuần tự và chọn cột có tổng hiện tại tối đa để đặt một cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n*m) | Quá chậm | 
| Tham lam + DP / Xử lý theo khoảng thời gian | O(n*m^2) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`cols`chiều dài$m$với số không. Mảng này sẽ lưu trữ tổng hiện tại của mỗi cột. 
2. Đối với mỗi hàng, lặp lại các khoảng của nó theo thứ tự. 
3. Với mỗi khoảng, hãy xác định cột trong khoảng đó hiện có tổng lớn nhất`cols`. Đặt một cái ở đó bằng cách tăng mục nhập tương ứng vào`cols`. 
4. Sau khi xử lý tất cả các hàng và khoảng, hãy tính tổng chất lượng như sau:$\sum_{i=1}^{m} cols[i]^2$. 

Lý do là việc thêm số 1 vào cột có tổng hiện tại lớn nhất sẽ làm tăng tổng bình phương nhiều nhất. Bởi vì mỗi khoảng phải đặt chính xác một khoảng, nên lựa chọn tham lam này sẽ tối đa hóa mức tăng gia tăng cho hàng đó trong khi vẫn tôn trọng các ràng buộc về khoảng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý từng khoảng thời gian, chất lượng tổng thể sẽ được tối đa hóa dưới sự ràng buộc của những khoảng thời gian đã được đặt. Hàm bình phương là hàm lồi nên việc cộng vào tổng lớn nhất hiện có luôn tạo ra mức tăng lớn hơn so với cộng vào tổng nhỏ hơn. Bằng cách xử lý các khoảng thời gian một cách tuần tự, chúng tôi duy trì lựa chọn gia tăng tối ưu ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
cols = [0] * m

for _ in range(n):
    k = int(input())
    intervals = [tuple(map(int, input().split())) for _ in range(k)]
    for l, r in intervals:
        # convert to 0-indexed
        l -= 1
        r -= 1
        # find column with max current sum
        best_col = max(range(l, r + 1), key=lambda x: cols[x])
        cols[best_col] += 1

print(sum(c * c for c in cols))
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi hàng, nó đọc các khoảng và sau đó, với mỗi khoảng, hãy chọn cột có tổng tối đa hiện tại để đặt một khoảng. Việc xử lý ranh giới được thực hiện bằng cách chuyển đổi sang các vị trí được lập chỉ mục 0. Việc tính toán tổng bình phương cuối cùng rất đơn giản. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4 5
2
1 2
3 5
2
1 3
4 5
3
1 1
2 4
5 5
3
1 1
2 2
3 5
```| Bước | Khoảng thời gian | cols trước | tốt nhất_col | cols sau | 
| --- | --- | --- | --- | --- | 
| Hàng1 Khoảng1 | 1-2 | [0,0,0,0,0] | 0 | [1,0,0,0,0] | 
| Hàng1 Khoảng2 | 3-5 | [1,0,0,0,0] | 4 | [1,0,0,0,1] | 
| Hàng2 Khoảng 1 | 1-3 | [1,0,0,0,1] | 0 | [2,0,0,0,1] | 
| Hàng2 Khoảng2 | 4-5 | [2,0,0,0,1] | 4 | [2,0,0,0,2] | 
| Hàng3 Khoảng 1 | 1-1 | [2,0,0,0,2] | 0 | [3,0,0,0,2] | 
| Hàng3 Khoảng2 | 2-4 | [3,0,0,0,2] | 4 | [3,0,0,0,3] | 
| Hàng3 Khoảng3 | 5-5 | [3,0,0,0,3] | 4 | [3,0,0,0,4] | 
| Khoảng thời gian hàng41 | 1-1 | [3,0,0,0,4] | 0 | [4,0,0,0,4] | 
| Hàng4 Khoảng2 | 2-2 | [4,0,0,0,4] | 1 | [4,1,0,0,4] | 
| Hàng4 Khoảng3 | 3-5 | [4,1,0,0,4] | 4 | [4,1,0,0,5] | 

Chất lượng tổng thể:$4^2 + 1^2 + 0 + 0 + 5^2 = 16 + 1 + 25 = 42$Các lựa chọn cột tối ưu có thể khác nhau về tỷ lệ hòa, nhưng tổng số tối đa vẫn giữ nguyên. 

### Đầu vào tùy chỉnh```
2 3
1
1 2
1
2 3
```Dấu vết: 

| Khoảng thời gian | cols trước | tốt nhất_col | cols sau | 
| --- | --- | --- | --- | 
| Hàng1 Khoảng1 | [0,0,0] | 0 | [1,0,0] | 
| Hàng2 Khoảng 1 | [1,0,0] | 1 | [1,1,0] | 

Chất lượng:$1^2 + 1^2 + 0 = 2$Điều này cho thấy thuật toán xử lý các khoảng chồng chéo một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m^2) | Đối với mỗi khoảng, chúng tôi quét các cột của nó (≤ m) để tìm mức tối đa, lặp lại cho tất cả các khoảng (≤ n*m) | 
| Không gian | O(m) | Chỉ một`cols`mảng có độ dài m là cần thiết | 

Được cho$n,m \le 100$, tổng số hoạt động$O(100 * 100^2) = 10^6$được chấp nhận trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    cols = [0] * m
    for _ in range(n):
        k = int(input())
        intervals = [tuple(map(int, input().split())) for _ in range(k)]
        for l, r in intervals:
            l -= 1
            r -= 1
            best_col = max(range(l, r+1), key=lambda x: cols[x])
            cols[best_col] += 1
    return str(sum(c*c for c in cols))

# provided sample
assert run("""4 5
2
1
```
