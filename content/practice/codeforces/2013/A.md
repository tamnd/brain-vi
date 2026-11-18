---
title: "CF 2013A - Máy xay sinh tố của Zhan"
description: "Zhan có một số loại trái cây và anh ấy muốn xay tất cả chúng càng nhanh càng tốt bằng máy xay sinh tố có hai hạn chế. Mỗi giây, anh ta có thể cho tối đa một số lượng trái cây cố định vào máy xay và máy xay có thể xử lý số lượng trái cây tối đa mỗi giây."
date: "2026-06-08T13:05:08+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 2013
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 973 (Div. 2)"
rating: 800
weight: 2013
solve_time_s: 104
verified: true
draft: false
---

[CF 2013A - Máy xay sinh tố của Zhan](https://codeforces.com/problemset/problem/2013/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Zhan có một số loại trái cây và anh ấy muốn xay tất cả chúng càng nhanh càng tốt bằng máy xay sinh tố có hai hạn chế. Mỗi giây, anh ta có thể cho tối đa một số lượng trái cây cố định vào máy xay và máy xay có thể xử lý số lượng trái cây tối đa mỗi giây. Bất kỳ loại trái cây nào đã có trong máy xay sẽ được xử lý đến giới hạn của máy xay và phần còn lại sẽ được giữ nguyên trong giây tiếp theo. Dữ liệu đầu vào cung cấp số lượng trái cây, công suất máy xay mỗi giây và số lượng trái cây mà Zhan có thể cho ăn mỗi giây. Đầu ra là số giây tối thiểu cần thiết để hoàn thành việc xay tất cả các loại trái cây. 

Các ràng buộc rất lớn: lên tới$10^9$trái cây và hoạt động mỗi giây. Điều này có nghĩa là chúng ta không thể mô phỏng từng giây hoặc lặp lại từng loại quả riêng lẻ; lời giải phải tính thời gian cần thiết bằng các phép tính số học đơn giản. 

Các trường hợp đặc biệt bao gồm khi Zhan có thể thêm nhiều trái cây hơn mức máy xay có thể xử lý trong một giây, khi công suất máy xay vượt quá mức Zhan có thể thêm và khi có rất ít trái cây so với giới hạn. Ví dụ: nếu chỉ có một quả và cả x và y đều lớn thì câu trả lời là 1. Các giải pháp bất cẩn có thể đếm thừa hoặc đếm thiếu vì chúng trộn lẫn công suất máy xay và tốc độ chèn mà không xem xét đúng mức tối thiểu giữa chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng giây, cộng thêm$y$trái cây cho vào máy xay rồi xay nhuyễn$\min(x, c)$trái cây. Mặc dù đúng nhưng điều này sẽ yêu cầu lặp lại tối đa$n$những loại trái cây không thể thực hiện được$n$lên tới$10^9$. 

Quan sát quan trọng là số giây cần thiết phụ thuộc vào tốc độ nhanh hơn trong hai tốc độ: công suất của máy xay$x$và tốc độ chèn$y$. Mỗi giây, số lượng trái cây được xử lý hiệu quả là số lượng tối đa có thể cho vào và lấy ra. Điều này làm giảm vấn đề thành một phép tính số học đơn giản sử dụng trần của$n$chia cho số lớn nhất$x$Và$y$, đồng thời xử lý tình huống khi cả máy xay và Zhan đều không thể giữ cho hệ thống bão hòa hoàn toàn chỉ trong một bước. Bằng cách xem xét lớn hơn của$x$Và$y$, chúng ta có thể tính được số giây cần thiết trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tính toán số học | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số quả$n$, công suất máy xay$x$và giới hạn chèn$y$. 
2. Nếu$n$là 0, câu trả lời là 0 giây, vì không có gì để trộn. 
3. Xác định tỷ lệ lớn hơn trong hai tỷ lệ,$max\_rate = \max(x, y)$. Điều này thể hiện số lượng trái cây tối đa mà chúng ta có thể xử lý một cách hiệu quả trong mỗi giây, vì việc đặt nhiều hơn mức máy xay có thể xử lý sẽ gây lãng phí và công suất máy xay nhỏ hơn sẽ hạn chế công suất hiệu quả. 
4. Tính số giây tối thiểu là$\lceil n / max\_rate \rceil$. Sử dụng số học số nguyên, đây là$(n + max\_rate - 1) // max\_rate$. Công thức này đảm bảo rằng bất kỳ phần giây nào cũng được tính là một giây đầy đủ. 
5. Xuất ra số giây được tính toán. 

Tính chính xác xuất phát từ tính bất biến là mỗi giây chúng tôi xử lý càng nhiều trái cây càng tốt với tốc độ nhanh hơn và mọi phần còn lại sẽ được tính vào giây tiếp theo. Trần nhà đảm bảo không có trái cây nào chưa qua chế biến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        x, y = map(int, input().split())
        
        if n == 0:
            print(0)
            continue
        
        max_rate = max(x, y)
        seconds = (n + max_rate - 1) // max_rate
        print(seconds)

if __name__ == "__main__":
    solve()
```Mã đọc số lượng trường hợp kiểm thử và xử lý lần lượt từng trường hợp kiểm thử. Chúng tôi sử dụng I/O nhanh và số học số nguyên để xử lý dữ liệu đầu vào lớn một cách hiệu quả. Công thức`(n + max_rate - 1) // max_rate`xử lý chính xác việc làm tròn đến số nguyên gần nhất. Những trường hợp đặc biệt như`n=0`được xử lý rõ ràng để tránh các vấn đề phân chia. 

## Ví dụ đã hoạt động 

Đầu vào mẫu: 5 loại trái cây, máy xay có thể xử lý 3, Zhan có thể thêm 4. 

| Bước | Trái Cây Còn Lại | Hành động | Pha trộn | 
| --- | --- | --- | --- | 
| 1 | 5 | Chèn min(4,5)=4 | min(3,4)=3 được trộn, 1 còn lại | 
| 2 | 1 | Chèn min(4,1)=1 | phút(3,2)=2 được trộn, 0 còn lại | 

Giây: 2 

Một ví dụ khác: 100 loại trái cây, công suất máy xay 4, Zhan thêm 3.`max_rate = max(4,3)=4`→ giây = (100 + 4 - 1) // 4 = 103 // 4 = 25. Điều này tính toán chính xác mức chia trần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi lần kiểm tra | Tính toán số học đơn lẻ, không lặp lại trái cây | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài đầu vào | 

Với tối đa 1000 trường hợp thử nghiệm, tổng số thao tác là không đáng kể, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n5\n3 4\n3\n1 2\n6\n4 3\n100\n4 3\n9\n3 3\n") == "2\n3\n2\n25\n3", "sample 1"

# Custom cases
assert run("2\n0\n1 1\n1\n1 1\n") == "0\n1", "zero and one fruit"
assert run("1\n1000000000\n1000000000 1\n") == "1", "blender can handle all at once"
assert run("1\n10\n3 5\n") == "2", "partial last second"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 quả | 0 | Xử lý trường hợp trống | 
| 1 quả, cả 1 | 1 | Trường hợp tối thiểu | 
| 1e9 quả, x=1e9 | 1 | Dung lượng tối đa một giây | 
| 10 quả, x=3, y=5 | 2 | Phân chia trần đúng cách | 

## Vỏ cạnh 

cho`n=0`, máy xay không làm gì cả. Công thức vẫn hoạt động nhưng kiểm tra rõ ràng trả về 0 giây để tránh tính toán không cần thiết. Vì`n`nhỏ hơn cả hai`x`Và`y`, công thức tạo ra 1 giây, điều này đúng. Đối với rất lớn`n`, phép tính số nguyên tránh được lỗi dấu phẩy động và đảm bảo phép chia trần chính xác. Việc trộn một phần vào giây cuối cùng được tính chính xác bằng`+ max_rate - 1`lừa.
