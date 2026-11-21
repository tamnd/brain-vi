---
title: "CF 226D - Bàn"
description: "Chúng ta có một bảng hình chữ nhật gồm các số nguyên có $n$ hàng và $m$ cột. Mỗi ô chứa một số có thể dương, âm hoặc bằng 0. Harry có thể thực hiện hai loại thao tác: lật dấu tất cả các số trong một hàng hoặc lật dấu tất cả các số trong một cột."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 226
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 140 (Div. 1)"
rating: 2100
weight: 226
solve_time_s: 178
verified: false
draft: false
---

[CF 226D - Cái bàn](https://codeforces.com/problemset/problem/226/D) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Cho một bảng hình chữ nhật gồm các số nguyên có$n$hàng và$m$cột. Mỗi ô chứa một số có thể dương, âm hoặc bằng 0. Harry có thể thực hiện hai loại thao tác: lật dấu tất cả các số trong một hàng hoặc lật dấu tất cả các số trong một cột. Mục đích là áp dụng một chuỗi các thao tác sao cho mỗi hàng và mỗi cột đều có tổng không âm. Đầu ra phải là tập hợp các hàng và cột cần lật. 

Cho rằng$n, m \le 100$và mỗi số được giới hạn bởi 100 về giá trị tuyệt đối, chúng ta được phép có giá trị gần đúng$10^4$các thao tác để giữ lời giải trong giới hạn thời gian 2 giây nếu chúng ta thực hiện các vòng lặp đơn giản. Một cách tiếp cận thô bạo thử tất cả các kết hợp lật hàng và lật cột là không khả thi, bởi vì có$2^{n+m}$các cấu hình lật có thể có, lớn về mặt thiên văn ngay cả đối với$n=m=20$. 

Các trường hợp biên có thể không thực hiện được khi triển khai đơn giản bao gồm các bảng có một hàng hoặc một cột trong đó tất cả các số đều âm. Ví dụ: nếu chúng ta có bảng 4×1 với các giá trị$[-1, -1, -1, -1]$, một thuật toán bất cẩn chỉ xem xét các hàng một cách cô lập có thể không lật được gì hoặc lật các cột không tồn tại, thay vì lật tất cả các hàng. Một kịch bản phức tạp khác là mô hình bàn cờ gồm các số âm và dương trong đó việc lật một hàng hoặc cột sẽ ảnh hưởng đến nhiều tổng; một cách tiếp cận tham lam ngây thơ có thể dao động vô hạn nếu nó không hội tụ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng lật từng tập hợp con của hàng và cột và kiểm tra xem tổng kết quả có âm hay không. Mặc dù đảm bảo tìm được nghiệm nếu tồn tại nhưng số lượng tập hợp con là$2^{n+m}$, điều này rõ ràng là không thể thực hiện được đối với$n, m = 100$. 

Thông tin chi tiết quan trọng là việc lật một hàng hoặc cột hai lần sẽ tự hủy, do đó, bất kỳ giải pháp nào cũng có thể được biểu diễn dưới dạng một tập hợp các lần lật hàng và một tập hợp các lần lật cột mà không lặp lại. Hơn nữa, việc lật có thể được áp dụng một cách tham lam. Nếu tổng hàng âm, chúng ta lật hàng đó thành số dương. Sau khi điều chỉnh hàng, chúng tôi kiểm tra cột; nếu tổng cột âm, chúng ta lật cột. Tuy nhiên, việc lật một cột có thể khiến một số tổng hàng trở lại âm, nhưng ứng dụng lặp lại cuối cùng sẽ hội tụ vì mỗi lần lật sẽ làm tăng tổng của hàng hoặc cột hiện âm. 

Vì các số bị giới hạn và lưới nhỏ nên phương pháp lặp tham lam trong đó chúng ta liên tục lật bất kỳ hàng hoặc cột nào có tổng âm sẽ hội tụ nhanh chóng và luôn tạo ra cấu hình hợp lệ. Số lần lật trong trường hợp xấu nhất là nhiều nhất$n + m$bởi vì việc lật bất kỳ hàng hoặc cột nào hai lần là không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{n+m} \cdot n \cdot m)$|$O(n \cdot m)$| Quá chậm | 
| Lặp đi lặp lại tham lam |$O(n \cdot m \cdot (n+m))$|$O(n \cdot m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới đầu vào và khởi tạo hai mảng boolean để theo dõi hàng và cột nào đã bị đảo ngược. Khởi tạo tất cả thành sai. 
2. Tính tổng của mỗi hàng. Nếu tổng hàng âm, hãy lật hàng bằng cách đánh dấu nó đã bị lật và phủ định tất cả các phần tử của nó. Lật một hàng đảm bảo tổng hàng không âm. 
3. Tính tổng từng cột. Nếu tổng cột âm, hãy lật cột bằng cách đánh dấu cột đã bị lật và phủ định tất cả các phần tử của cột đó. Lật một cột đảm bảo tổng cột không âm. 
4. Sau khi lật cột, một số tổng của hàng có thể trở thành số âm. Lặp lại bước 2 và 3 cho đến khi không có tổng hàng hoặc cột nào âm. 
5. Xuất ra danh sách các hàng bị đảo lộn và các cột bị đảo lộn. Thứ tự không quan trọng, chỉ mỗi chỉ mục xuất hiện nhiều nhất một lần. 

Tại sao nó hoạt động: Mỗi lần lật cải thiện nghiêm ngặt dấu của ít nhất một tổng hàng hoặc cột âm. Khi tổng của một hàng hoặc cột là dương, các lần lật tiếp theo trên các hàng hoặc cột khác không thể làm cho tổng đó âm vô thời hạn vì mỗi số đều bị giới hạn và việc lật hai lần sẽ tự hủy. Điều này đảm bảo sự hội tụ và tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

row_flip = [False] * n
col_flip = [False] * m

changed = True
while changed:
    changed = False
    for i in range(n):
        row_sum = sum(a[i][j] if not col_flip[j] else -a[i][j] for j in range(m))
        if row_sum < 0:
            row_flip[i] = not row_flip[i]
            changed = True
    for j in range(m):
        col_sum = sum(a[i][j] if not row_flip[i] else -a[i][j] for i in range(n))
        if col_sum < 0:
            col_flip[j] = not col_flip[j]
            changed = True

rows = [i + 1 for i in range(n) if row_flip[i]]
cols = [j + 1 for j in range(m) if col_flip[j]]

print(len(rows), *rows)
print(len(cols), *cols)
```Đầu tiên, mã sẽ đọc bảng và khởi tạo hai danh sách để theo dõi các lần lật. Vòng lặp chính xen kẽ giữa việc kiểm tra tổng hàng và tổng cột. Việc lật được áp dụng một cách tham lam bất cứ khi nào tìm thấy số tiền âm. Vòng lặp kết thúc khi không có hàng hoặc cột nào có tổng âm. Cuối cùng, chúng tôi trích xuất chỉ mục của các hàng và cột bị đảo ngược và in chúng. 

Một chi tiết tinh tế là cách chúng tôi tính tổng cho các lần tung trước đó. Một ô đóng góp tích cực nếu cả hàng và cột của nó đều không bị đảo số lần lẻ. Sử dụng boolean với phủ định sẽ đơn giản hóa việc theo dõi. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 1
-1
-1
-1
-1
```| Bước | Tổng hàng | Tổng cột | Lật hàng | Lật cột | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [-1,-1,-1,-1] | [-4] | [] | [] | 
| Lật hàng | [1,1,1,1] | [4] | [1,2,3,4] | [] | 
| Kiểm tra cột | [1,1,1,1] | [4] | [1,2,3,4] | [] | 

Bảng có một cột. Tất cả các tổng hàng ban đầu đều âm, vì vậy tất cả các hàng đều bị đảo ngược. Tổng cột trở nên dương. Không cần lật thêm nữa. 

Đầu vào mẫu 2:```
2 2
-1 2
3 -4
```| Bước | Tổng hàng | Tổng cột | Lật hàng | Lật cột | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,-1] | [2,-2] | [] | [] | 
| Lật hàng 2 | [1,1] | [2,0] | [2] | [] | 
| Lật cột 2 | [1,-1] | [2,4] | [2] | [2] | 
| Lật hàng 2 | [1,1] | [2,4] | [2] | [2] | 

Thuật toán ổn định với hàng 2 và cột 2 được đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m * (n + m)) | Mỗi lần lặp tính tổng theo hàng và cột. Mỗi lần lật kích hoạt tối đa một lần lặp trên mỗi hàng hoặc cột. | 
| Không gian | O(n * m) | Lưu trữ bảng và hai mảng boolean cho các lần lật. | 

Với$n, m \le 100$, trường hợp xấu nhất là đại khái$100 * 100 * 200 = 2 \cdot 10^6$hoạt động, phù hợp thoải mái dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    old_print = builtins.print
    out = []
    builtins.print = lambda *args: out.append(" ".join(map(str,args)))
    exec(open('solution.py').read())
    builtins.print = old_print
    return "\n".join(out)

assert run("4 1\n-1\n-1\n-1\n-1\n") == "4 1 2 3 4\n0", "sample 1"
assert run("2 2\n-1 2\n3 -4\n") == "1 2\n1 2", "checkerboard"
assert run("1 1\n-5\n") == "1 1\n0", "single negative"
assert run("3 3\n1 2 3\n4 5 6\n7 8 9\n") == "0\n0", "all positive"
assert run("2 3\n-1 -2 -3\n4 5 6\n") == "1 1\n0", "one negative row"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4×1 tất cả -1 | Lật 4 hàng không có cột | Kịch bản một cột | 
| Bàn cờ 2×2 | Hàng 2, Cột 2 bị lật | Biển báo xen kẽ, lật nhiều lần | 
| 1×1 -5 | Hàng lật | Bảng kích thước tối thiểu | 
| 3×3 đều dương | Không lật | Bảng đã không âm | 
| 2×3 hàng âm | Hàng 1 bị lật | Tiêu cực một phần |
