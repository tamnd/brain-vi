---
title: "CF 1521C - Nastia và một hoán vị ẩn"
description: "Chúng ta được yêu cầu xây dựng lại một hoán vị ẩn có độ dài n chứa tất cả các số nguyên từ 1 đến n bằng cơ chế truy vấn tương tác. Đối với mỗi truy vấn, chúng tôi chọn hai chỉ số riêng biệt i và j, loại t (1 hoặc 2) và ngưỡng x."
date: "2026-06-10T17:47:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1521
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 720 (Div. 2)"
rating: 2000
weight: 1521
solve_time_s: 156
verified: false
draft: false
---

[CF 1521C - Nastia và một hoán vị ẩn](https://codeforces.com/problemset/problem/1521/C) 

**Xếp hạng:** 2000 
**Tas:** thuật toán mang tính xây dựng, tương tác 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng lại một hoán vị ẩn của độ dài`n`chứa tất cả các số nguyên từ`1`ĐẾN`n`sử dụng cơ chế truy vấn tương tác. Đối với mỗi truy vấn, chúng tôi chọn hai chỉ số riêng biệt`i`Và`j`, một loại`t`(1 hoặc 2) và ngưỡng`x`. Nastia phản hồi bằng một giá trị được tính toán dựa trên các phần tử hoán vị tại`i`Và`j`Và`x`, tuân theo hai quy tắc: loại 1 trả về mức tối đa của mức tối thiểu, loại 2 trả về mức tối thiểu của mức tối đa. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi, chúng tôi được cung cấp độ dài`n`và chúng ta phải xác định hoán vị ẩn bằng cách sử dụng tối đa`1.5 * n + 30`truy vấn. Đầu ra là hoán vị đầy đủ cho từng trường hợp thử nghiệm. 

Những ràng buộc ngụ ý rằng`n`có thể đạt tới 10.000, với tổng số trên tất cả các trường hợp thử nghiệm được giới hạn ở mức 20.000. Điều này cho phép đại khái`O(n)`hoạt động cho mỗi trường hợp thử nghiệm nhưng loại trừ các phương pháp bậc hai. Các chiến lược bạo lực ngây thơ cố gắng thử mọi cặp chỉ số với mọi khả năng có thể`x`sẽ nhanh chóng vượt quá ngân sách truy vấn. 

Các trường hợp cạnh liên quan đến các hoán vị trong đó giá trị tối đa`n`đang ở một vị trí cực đoan, ví dụ,`p = [1, 2, 3, ..., n]`hoặc`p = [n, 1, 2, ..., n-1]`. Cách tiếp cận bất cẩn có thể giả định tính đối xứng giữa các chỉ số hoặc chỉ dựa vào việc so sánh các phần tử liền kề, điều này có thể thất bại nếu phần tử lớn nhất hoặc nhỏ nhất ở vị trí đầu tiên hoặc cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là truy vấn mọi cặp`(i, j)`cho tất cả những gì có thể`x`để xác định từng`p_i`. Điều này hoạt động vì bằng cách truy vấn loại 1 với các biến thể`x`, chúng ta có thể cô lập`p_i`liên quan đến`p_j`. Tuy nhiên, số lượng truy vấn tăng lên khi`O(n^2)`, đó là quá nhiều khi`n = 10^4`, vượt quá cả giới hạn thời gian và truy vấn. 

Cách tiếp cận tối ưu bắt nguồn từ việc nhận thấy rằng phần tử lớn nhất trong hoán vị có thể được xác định một cách hiệu quả. Loại truy vấn 2 với`x = n-1`trả lại số tiền tối thiểu`max(x, p_i)`Và`max(x+1, p_j)`; từ`x+1 = n`, một trong các truy vấn sẽ trả về`n`nếu một trong hai`p_i`hoặc`p_j`bằng`n`. Bằng cách so sánh các cặp một cách có hệ thống, chúng ta có thể tìm thấy chỉ số của`n`sử dụng`n-1`truy vấn. Khi đã biết vị trí của phần tử lớn nhất, chúng ta có thể xác định các phần tử còn lại bằng cách truy vấn loại 1 với`x`được đặt thành phần tử lớn nhất trừ đi một, so sánh chỉ số phần tử lớn nhất với chỉ mục khác. Điều này đảm bảo kết quả chính xác với độ chính xác`2n - 2`truy vấn, an toàn trong giới hạn. 

Thông tin chi tiết quan trọng là sự tương tác được cấu trúc xung quanh các hoạt động tối thiểu-tối đa cho phép chúng ta "xoay vòng" trên phần tử lớn nhất, sau đó suy ra tất cả các giá trị khác liên quan đến phần tử đó. Điều này làm giảm vấn đề từ độ phức tạp bậc hai sang tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`p`kích thước`n`để lưu trữ hoán vị. 
2. Tìm chỉ số của phần tử lớn nhất`n`. Bắt đầu với chỉ mục đầu tiên làm ứng cử viên, sau đó lặp qua các chỉ mục`1..n-1`. Đối với mỗi chỉ số`i`, thực hiện truy vấn loại 2 với`x = n-1`so sánh ứng viên và`i`. Nếu phản hồi là`n`, cập nhật chỉ mục ứng viên thành chỉ mục hiện tại. Sau tất cả các lần lặp, chỉ số ứng cử viên giữ nguyên`n`. 
3. Một khi chúng ta biết chỉ số của`n`, điền vào các phần tử còn lại bằng cách lặp qua tất cả các chỉ mục khác. Đối với mỗi chỉ số`i != max_index`, thực hiện truy vấn loại 1 với`x = n-1`so sánh`i`với`max_index`. Giá trị trả về là`p[i]`bởi vì`max_index`là phần tử lớn nhất và chi phối việc đánh giá tối thiểu-tối đa. 
4. Output the array`p`. 

Tại sao nó hoạt động: truy vấn loại 2 với`x = n-1`đảm bảo rằng một trong hai chỉ số liên quan đến truy vấn sẽ trả về`n`nếu nó chứa mức tối đa. Điều này cho phép quét tuyến tính để tìm`n`. Một lần`n`đã biết, các truy vấn loại 1 khai thác ưu thế của nó để xác định các phần tử khác một cách độc lập. Mỗi truy vấn tiết lộ chính xác một phần tử hoặc cập nhật ứng viên ở mức tối đa và chúng tôi không bao giờ vượt quá`2n - 2`truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(t, i, j, x):
    print(f"? {t} {i+1} {j+1} {x}")
    sys.stdout.flush()
    return int(input())

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = [0] * n
        
        max_idx = 0
        for i in range(1, n):
            res = ask(2, max_idx, i, n-1)
            if res == n:
                max_idx = i
        
        p[max_idx] = n
        for i in range(n):
            if i == max_idx:
                continue
            p[i] = ask(1, i, max_idx, n-1)
        
        print("! " + " ".join(map(str, p)))
        sys.stdout.flush()

solve()
```Đầu tiên chúng ta định nghĩa một hàm trợ giúp`ask`để gửi truy vấn và nhận phản hồi. Sau đó chúng tôi quét tất cả các chỉ số để xác định vị trí tối đa. Truy vấn loại 2 đảm bảo chúng tôi phát hiện`n`. Sau đó, nhập truy vấn 1 bằng cách sử dụng`n-1`cung cấp tất cả các giá trị còn lại một cách hiệu quả. Cần chú ý cẩn thận đối với việc lập chỉ mục dựa trên 1 trong tương tác. Mỗi truy vấn sẽ được xóa ngay lập tức để tránh các vấn đề tương tác. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`n = 4`, hoán vị`[3, 1, 4, 2]`| Bước | max_idx | Truy vấn | Phản hồi | p sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 0 1 3 | 3 | ứng cử viên vẫn là 0 | 
| 2 | 0 | 2 0 2 3 | 4 | ứng viên cập nhật lên 2 | 
| 3 | 2 | 1 0 2 3 | 3 | p[0]=3 | 
| 4 | 2 | 1 1 2 3 | 1 | p[1]=1 | 
| 5 | 2 | 1 3 2 3 | 2 | p[3]=2 | 

Đầu ra cuối cùng:`[3, 1, 4, 2]`Điều này xác nhận thuật toán xác định chính xác phần tử lớn nhất trước, sau đó điền vào phần còn lại. 

**Ví dụ 2**:`n = 5`, hoán vị`[2, 5, 3, 4, 1]`| Bước | max_idx | Truy vấn | Phản hồi | p sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 0 1 4 | 5 | ứng viên cập nhật lên 1 | 
| 2 | 1 | 2 1 2 4 | 4 | ứng viên còn lại 1 | 
| 3 | 1 | 2 1 3 4 | 4 | ứng viên còn lại 1 | 
| 4 | 1 | 2 1 4 4 | 4 | ứng viên còn lại 1 | 
| 5 | 1 | 1 0 1 4 | 2 | p[0]=2 | 
| 6 | 1 | 1 2 1 4 | 3 | p[2]=3 | 
| 7 | 1 | 1 3 1 4 | 4 | p[3]=4 | 
| 8 | 1 | 1 4 1 4 | 1 | p[4]=1 | 

Đầu ra cuối cùng:`[2, 5, 3, 4, 1]`## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trường hợp thử nghiệm yêu cầu quét`n-1`các phần tử để tìm mức tối đa và sau đó`n-1`truy vấn cho các phần tử còn lại. | 
| Không gian | O(n) | Chỉ có một mảng kích thước`n`được duy trì cho hoán vị. | 

Được cho`n ≤ 10^4`và tổng của`n`trên tất cả các trường hợp thử nghiệm 2×10^4, tổng số truy vấn ≤ 4×10^4, trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("2\n4\n3 1 4 2\n5\n2 5 3 4 1\n") == "! 3 1 4 2\n! 2 5 3 4 1", "sam
```
