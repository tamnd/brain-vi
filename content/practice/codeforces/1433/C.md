---
title: "CF 1433C - Piranha chiếm ưu thế"
description: "Chúng tôi được yêu cầu xác định cá piranha chiếm ưu thế trong một bể cá tuyến tính. Mỗi cá piranha có một kích thước khác nhau và một cá piranha chiếm ưu thế nếu cuối cùng nó có thể ăn tất cả các cá piranha khác bằng cách liên tục ăn thịt một cá piranha liền kề nhỏ hơn nó. Mỗi lần nó ăn, kích thước của nó lại tăng lên một."
date: "2026-06-11T05:03:44+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1433
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 677 (Div. 3)"
rating: 900
weight: 1433
solve_time_s: 505
verified: false
draft: false
---

[CF 1433C - Cá Piranha thống trị](https://codeforces.com/problemset/problem/1433/C) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 8m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định cá piranha chiếm ưu thế trong một bể cá tuyến tính. Mỗi cá piranha có một kích thước khác nhau và một cá piranha chiếm ưu thế nếu cuối cùng nó có thể ăn tất cả các cá piranha khác bằng cách liên tục ăn thịt một cá piranha liền kề nhỏ hơn nó. Mỗi lần nó ăn, kích thước của nó lại tăng lên một. Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định số lượng cá piranha và kích cỡ của chúng. Đầu ra phải là chỉ số dựa trên 1 của bất kỳ cá piranha ưu thế nào hoặc -1 nếu không tồn tại. 

Các ràng buộc ngụ ý rằng việc mô phỏng mạnh mẽ tất cả các chuyển động có thể xảy ra là không khả thi vì tổng số cá piranha trong tất cả các trường hợp thử nghiệm có thể lên tới 300.000. Việc mô phỏng từng trình tự ăn uống có thể có cho mọi cá piranha ứng cử viên sẽ tạo ra độ phức tạp bậc hai hoặc tệ hơn, điều này không thể chấp nhận được trong giới hạn thời gian 2 giây. 

Các trường hợp cạnh không rõ ràng phát sinh khi tất cả cá piranha có cùng kích thước. Trong trường hợp như vậy, không có cá piranha nào có thể ăn thịt cá khác, vì vậy kết quả đầu ra đúng là -1. Một trường hợp khó phát hiện khác là khi kích thước tối đa xuất hiện nhiều lần trong mảng; chỉ mức tối đa có ít nhất một lân cận nhỏ hơn chính nó mới có khả năng chiếm ưu thế. Ví dụ, trong`[5, 5, 4, 3, 5]`, đầu tiên và cuối cùng`5`không thể ăn bất kỳ cá piranha liền kề nào nhỏ hơn, nhưng ở giữa`5`có thể ăn`4`ở bên phải của nó, vì vậy nó là một ứng cử viên hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là thử từng cá piranha như một ứng cử viên và mô phỏng các bước di chuyển của nó cho đến khi nó ăn tất cả các cá piranha khác hoặc bị mắc kẹt. Điều này hiệu quả vì các quy tắc mang tính quyết định: cá piranha chỉ có thể ăn con nhỏ hơn liền kề. Tuy nhiên, đối với một mảng có kích thước`n`, một mô phỏng duy nhất có thể liên quan đến`O(n^2)`các bước và tổng hợp tất cả các trường hợp thử nghiệm sẽ dẫn đến các hoạt động vượt quá`10^10`, quá chậm. 

Thông tin chi tiết quan trọng là chỉ cá piranha có **kích thước tối đa** mới có thể chiếm ưu thế. Một con cá piranha nhỏ hơn mức tối đa không bao giờ có thể tiêu thụ một con cá piranha lớn hơn, vì vậy nó ngay lập tức bị loại. Trong số các loài cá piranha có kích thước tối đa, một cá piranha chỉ có thể chiếm ưu thế nếu nó có **ít nhất một hàng xóm nhỏ hơn nó**. Điều này là do nó cần một điểm khởi đầu để phát triển và nếu không có hàng xóm nhỏ hơn, nó không thể bắt đầu quá trình ăn uống. Với quan sát này, chúng ta chỉ cần quét mảng một lần để tìm giá trị lớn nhất rồi nhìn vào các hàng xóm của nó. Điều này làm giảm độ phức tạp thành tuyến tính trong kích thước mảng cho từng trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| Kiểm tra hàng xóm tối đa + | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số lượng cá piranha`n`và mảng`a`về kích cỡ. 
2. Tìm kích thước lớn nhất trong mảng. 
3. Duyệt mảng để xác định vị trí của một con cá piranha có kích thước tối đa có ít nhất một con lân cận nhỏ hơn nó. 
4. Nếu có một con cá piranha như vậy, hãy xuất chỉ mục dựa trên 1 của nó và ngừng tìm kiếm thêm. 
5. Nếu không tìm thấy cá piranha như vậy sau khi quét mảng, ghi -1. 

Tại sao nó hoạt động: Theo định nghĩa, chỉ cá piranha có kích thước tối đa mới có thể ăn tất cả những con khác. Một con cá piranha có kích thước tối đa không có hàng xóm nhỏ hơn không thể bắt đầu tiêu thụ, vì vậy nó không thể chiếm ưu thế. Bất kỳ cá piranha có kích thước tối đa nào có ít nhất một con hàng xóm nhỏ hơn đều có thể tham lam ăn trái hoặc phải, và sau khi ăn thịt con hàng xóm, nó sẽ phát triển và có thể tiếp tục ăn cho đến khi tất cả các cá piranha khác biến mất. Điều bất biến là ứng viên phải luôn có khả năng bắt đầu ăn uống; thuật toán kiểm tra chính xác điều đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        max_size = max(a)
        
        dominant_index = -1
        for i in range(n):
            if a[i] == max_size:
                if (i > 0 and a[i-1] < max_size) or (i < n-1 and a[i+1] < max_size):
                    dominant_index = i + 1  # 1-based index
                    break
        print(dominant_index)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó tính toán kích thước tối đa và sau đó lặp qua mảng để tìm một con piranha có kích thước tối đa có ít nhất một hàng xóm nhỏ hơn. các`i + 1`điều chỉnh chuyển đổi từ chỉ mục dựa trên 0 sang chỉ mục dựa trên 1 theo yêu cầu của vấn đề. Việc ngắt đảm bảo chúng tôi xuất ra ứng cử viên hợp lệ đầu tiên, mặc dù bất kỳ ứng cử viên hợp lệ nào cũng được chấp nhận. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`[5, 3, 4, 4, 5]`| tôi | một [tôi] | Kiểm tra hàng xóm | Trội? | 
| --- | --- | --- | --- | 
| 0 | 5 | đúng=3 < 5 | Có | 

Con cá piranha đầu tiên có kích thước tối đa và có con hàng xóm nhỏ hơn, vì vậy chỉ số`1`là hợp lệ. Trong giải pháp mẫu, thuật toán chọn chỉ số`3`thay vào đó, nó cũng hoạt động. Nhiều câu trả lời có thể được chấp nhận. 

**Ví dụ 2:**`[1, 1, 1]`| tôi | một [tôi] | Kiểm tra hàng xóm | Trội? | 
| --- | --- | --- | --- | 
| 0 | 1 | đúng=1 | Không | 
| 1 | 1 | trái=1, phải=1 | Không | 
| 2 | 1 | trái=1 | Không | 

Không có cá piranha có kích thước tối đa nào có hàng xóm nhỏ hơn. đầu ra`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét một lần để tìm max và kiểm tra hàng xóm | 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài mảng đầu vào | 

Với tổng số`n`trên tất cả các trường hợp thử nghiệm 300.000, thuật toán chạy thoải mái theo thời gian tuyến tính trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio
    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n5\n5 3 4 4 5\n3\n1 1 1\n5\n4 4 3 4 4\n5\n5 5 4 3 2\n3\n1 1 2\n5\n5 4 3 5 5\n") == "3\n-1\n4\n3\n3\n1", "sample 1"

# Custom cases
assert run("1\n2\n1 2\n") == "2", "two elements, dominant at end"
assert run("1\n3\n3 3 3\n") == "-1", "all equal sizes"
assert run("1\n4\n1 5 5 1\n") == "2", "middle max with left neighbor smaller"
assert run("1\n5\n5 1 5 1 5\n") == "1", "multiple maxima, first with smaller neighbor"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2\n1 2`| 2 | Nhận dạng đúng khi chiếm ưu thế ở cuối | 
|`3\n3 3 3`| -1 | Không có cá piranha chiếm ưu thế khi tất cả đều bình đẳng | 
|`4\n1 5 5 1`| 2 | Chọn chính xác tối đa với hàng xóm nhỏ hơn | 
|`5\n5 1 5 1 5`| 1 | Chọn mức tối đa đầu tiên với hàng xóm hợp lệ | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả cá piranha có cùng kích thước, thuật toán sẽ đưa ra chính xác -1 vì không có cá piranha nào có hàng xóm nhỏ hơn để bắt đầu ăn. Trong các mảng có nhiều mức tối đa, thuật toán luôn tìm mức tối đa đầu tiên với lân cận nhỏ hơn. Đối với kích thước đầu vào tối thiểu`n=2`, việc kiểm tra hoạt động chính xác: nếu một con cá piranha hoàn toàn lớn hơn con kia, nó sẽ được chọn là ưu thế. Để có kích thước đầu vào tối đa, quá trình quét tuyến tính đảm bảo hiệu suất luôn nằm trong giới hạn.
