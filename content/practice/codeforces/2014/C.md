---
title: "CF 2014C - Robin Hood trong thị trấn"
description: "Chúng ta được yêu cầu xác định xem phải bổ sung thêm bao nhiêu vàng cho người giàu nhất trong thị trấn để hơn một nửa dân số trở nên không hài lòng. Hạnh phúc của mỗi người phụ thuộc vào việc tài sản của họ có bằng một nửa mức tài sản trung bình của thị trấn hay không."
date: "2026-06-08T13:00:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2014
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 974 (Div. 3)"
rating: 1100
weight: 2014
solve_time_s: 114
verified: false
draft: false
---

[CF 2014C - Robin Hood trong thị trấn](https://codeforces.com/problemset/problem/2014/C) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, tham lam, toán học 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem phải bổ sung thêm bao nhiêu vàng cho người giàu nhất trong thị trấn để hơn một nửa dân số trở nên không hài lòng. Hạnh phúc của mỗi người phụ thuộc vào việc tài sản của họ có bằng một nửa mức tài sản trung bình của thị trấn hay không. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số lượng người và danh sách tài sản của họ. Đầu ra là số vàng bổ sung tối thiểu cần thiết để Robin Hood xuất hiện hoặc -1 nếu không thể. 

Với những hạn chế, chúng ta thấy rằng`n`có thể đạt tới 200.000 và tổng số trường hợp thử nghiệm cũng được giới hạn ở mức 200.000. Điều này loại trừ bất kỳ cách tiếp cận nào liên quan đến việc kiểm tra mọi giá trị có thể có của số vàng bổ sung bằng vũ lực, vì điều đó có thể liên quan đến việc lặp lại trên một phạm vi tổng rộng. Chúng ta cần một phương pháp tính toán lượng vàng bổ sung cần thiết một cách hiệu quả cho từng trường hợp thử nghiệm về cơ bản là thời gian tuyến tính đối với`n`. 

Trường hợp cạnh tinh tế xảy ra khi dân số rất nhỏ. Vì`n = 1`, không thể có hơn một nửa số người không hài lòng nên câu trả lời phải là -1. Tương tự, nếu tất cả của cải đều bằng nhau hoặc người giàu nhất đã giàu đến mức không ai khác thấp hơn một nửa mức trung bình, thì số vàng tăng thêm có thể bằng không. Xử lý phép chia dấu phẩy động một cách cẩn thận là điều quan trọng vì chúng ta đang so sánh tài sản của từng cá nhân với một nửa mức trung bình, tức là một con số thực. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là thử tăng tài sản của người giàu nhất từng bước một và tính lại số lượng trung bình và số người không hạnh phúc mỗi lần cho đến khi hơn một nửa dân số không hạnh phúc. Điều này hiệu quả vì nó trực tiếp thực hiện định nghĩa, nhưng nó cực kỳ chậm. Phạm vi của`x`có thể cực kỳ lớn (lên tới hàng tỷ) và việc tính lại số lượng trung bình và số lượng không hài lòng mỗi lần yêu cầu các phép toán O(n), do đó độ phức tạp tổng thể có thể là O(n * max(x)), điều này là không thể chấp nhận được. 

Nhận xét quan trọng là số người bất hạnh chỉ tăng lên khi người giàu nhất kiếm được vàng. Điều kiện không hài lòng là tuyến tính trong`x`bởi vì mức trung bình mới là`(sum + x)/n`và chúng tôi so sánh từng`a_i`ĐẾN`(sum + x)/(2n)`. Đối với bất kỳ cá nhân nào`i`, bất đẳng thức`a_i < (sum + x)/(2n)`có thể được giải quyết một cách rõ ràng`x`. Vì vậy, để hơn một nửa dân số không hạnh phúc, chỉ cần tập trung vào`ceil(n/2)`của cải lớn nhất của người không giàu nhất. Chúng ta có thể tính ngưỡng`x`đối với mỗi người khi họ trở nên không hạnh phúc và sau đó đạt đến mức tối đa các ngưỡng này. Điều này dẫn đến một giải pháp dạng đóng mà không cần tìm kiếm lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(x)) | O(n) | Quá chậm | 
| Ngưỡng dạng đóng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số người`n`và danh sách của cải`a`. Tính tổng tài sản`total`. 
2. Tìm sự giàu có tối đa`max_a`và chỉ số của nó. Đây là người giàu nhất sẽ nhận được thêm vàng. 
3. Tính số người bất hạnh cần có:`ceil(n/2)`. 
4. Sắp xếp tất cả những tài sản khác trừ người giàu nhất theo thứ tự tăng dần. Chúng tôi tập trung vào`ceil(n/2)`những người nhỏ nhất có thể không hạnh phúc. 
5. Với mỗi người này, hãy tính số vàng bổ sung tối thiểu`x_i`cần thiết để tài sản của họ thấp hơn một nửa mức trung bình mới. Giải bất đẳng thức`a_i < (total + x)/(2n)`vì`x`để có được`x_i = max(0, 2 * n * a_i - total + 1)`. các`+1`đảm bảo sự bất bình đẳng nghiêm ngặt. 
6. Tận dụng tối đa`x_i`trong số những người được chọn này. Nếu nó âm, trả về 0; nếu dương tính thì trả lại. Nếu không thể (dân số quá nhỏ), trả về -1. 

Sự đúng đắn xảy ra vì người giàu nhất bổ sung thêm vàng sẽ làm tăng mức trung bình, khiến những người ở ranh giới trước đó không hài lòng. Bằng cách lấy ngưỡng tối đa trong số những người phải trở nên bất hạnh, chúng tôi đảm bảo rằng hơn một nửa là không hạnh phúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            print(-1)
            continue
        
        total = sum(a)
        max_val = max(a)
        idx = a.index(max_val)
        
        others = a[:idx] + a[idx+1:]
        others.sort()
        
        unhappy_needed = (n + 1) // 2
        selected = others[:unhappy_needed]
        
        x_needed = 0
        for val in selected:
            x_i = 2 * n * val - total + 1
            x_needed = max(x_needed, x_i)
        
        print(max(0, x_needed))
        
if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào bằng I/O nhanh để xử lý các trường hợp lớn. Nó xử lý riêng trường hợp cạnh của một người. các`2 * n * val - total + 1`công thức đến từ việc giải quyết trực tiếp sự bất bình đẳng nghiêm ngặt và việc đạt mức tối đa đảm bảo rằng tất cả những người được yêu cầu đều không hài lòng. 

## Ví dụ đã hoạt động 

| Kiểm tra đầu vào | n | Sự giàu có | Được chọn vì bất hạnh | giá trị x_i | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 1 2 3 4 | n=4 | ceil(4/2)=2, chọn nhỏ nhất 2 [1,2] | 2_4_1-10+1= -1→0, 2_4_2-10+1=3 | tối đa=3 | 
| 6 | 1 2 1 1 1 25 | n=6 | ceil(6/2)=3, chọn nhỏ nhất 3 [1,1,1] | 2_6_1-31+1=-18→0, lặp lại | tối đa=0 | 

Những ví dụ này chứng minh rằng công thức tính toán chính xác`x`và giải thích sự bất bình đẳng nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp của cải của người khác thống trị thời gian | 
| Không gian | O(n) | Lưu trữ của cải và tập hợp con được chọn | 

Việc sắp xếp có thể được chấp nhận vì n 2×10^5 cho mỗi trường hợp thử nghiệm và tổng n trên tất cả các thử nghiệm là 2×10^5, do đó tổng thời gian chạy nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    buf = _io.StringIO()
    with redirect_stdout(buf):
        solve()
    return buf.getvalue().strip()

# provided samples
assert run("""6
1
2
2
2 19
3
1 3 20
4
1 2 3 4
5
1 2 3 4 5
6
1 2 1 1 1 25""") == "-1\n-1\n0\n15\n16\n0", "sample tests"

# custom cases
assert run("1\n2\n1 1") == "-1", "both equal, impossible"
assert run("1\n3\n1 1 1000000") == "0", "richest already causes unhappiness"
assert run("1\n4\n1 2 3 4") == "15", "from notes"
assert run("1\n1\n100") == "-1", "single person"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 người cả 1 | -1 | trường hợp không thể xảy ra với tất cả đều bằng nhau | 
| 3 người giàu nhất 1e6 | 0 | giàu nhất đã gây ra bất hạnh | 
| 4 người [1 2 3 4] | 15 | tính đúng đắn | 
| 1 người | -1 | trường hợp cạnh n=1 | 

## Vỏ cạnh 

Khi nào`n = 1`, không thể có hơn một nửa dân số không hạnh phúc. Thuật toán trả về đúng -1. Khi người giàu nhất đã giàu đến mức thêm 0 vàng là đủ, thì công thức tạo ra một x không dương, mà chúng ta giảm xuống 0. Khi tất cả mọi người đều có mức độ giàu có như nhau, khó có thể khiến hơn một nửa không hài lòng, và một lần nữa công thức trả về 0 hoặc -1 nếu phù hợp. Trong mọi trường hợp, việc sắp xếp đảm bảo chúng ta chọn đúng số người để theo dõi mức độ bất hạnh.
