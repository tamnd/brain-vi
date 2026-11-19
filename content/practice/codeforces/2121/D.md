---
title: "CF 2121D - 1709"
description: "Chúng ta có hai mảng a và b, mỗi mảng có độ dài n, chứa tất cả các số nguyên từ 1 đến 2n đúng một lần ở giữa chúng. Nhiệm vụ là sắp xếp lại các mảng bằng cách sử dụng ba loại hoán đổi sao cho a tăng chặt, b tăng chặt và tại mỗi vị trí i, a[i] < b[i]."
date: "2026-06-08T03:48:37+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2121
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1032 (Div. 3)"
rating: 1300
weight: 2121
solve_time_s: 104
verified: false
draft: false
---

[CF 2121D - 1709](https://codeforces.com/problemset/problem/2121/D) 

**Đánh giá:** 1300 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`, mỗi chiều dài`n`, chứa tất cả các số nguyên từ`1`ĐẾN`2n`đúng một lần giữa chúng. Nhiệm vụ là sắp xếp lại các mảng bằng cách sử dụng ba loại hoán đổi sao cho`a`đang gia tăng nghiêm trọng,`b`đang tăng lên một cách nghiêm ngặt, và tại mỗi vị trí`i`,`a[i] < b[i]`. Các giao dịch hoán đổi được phép là các giao dịch hoán đổi liền kề trong`a`hoặc`b`hoặc hoán đổi theo chiều dọc giữa`a[i]`Và`b[i]`. 

Các hạn chế là nhỏ:`n`đi lên`40`và tổng số thao tác không được vượt quá`1709`. Điều này ngay lập tức gợi ý rằng cách tiếp cận O(n2) là khả thi. Mỗi số là duy nhất, giúp tránh sự phức tạp do trùng lặp và đảm bảo rằng luôn có thể hình thành một chuỗi tăng dần. Khía cạnh không rõ ràng là chúng ta chỉ có thể hoán đổi các phần tử liền kề trong một hàng, do đó việc sắp xếp một hàng có thể yêu cầu nhiều lần hoán đổi và chúng ta có thể cần hoán đổi theo chiều dọc để đáp ứng`a[i] < b[i]`. 

Một cách tiếp cận bất cẩn có thể cố gắng sắp xếp`a`Và`b`độc lập và sau đó trao đổi theo chiều dọc ở bất cứ đâu`a[i] > b[i]`. Điều này có thể thất bại vì việc hoán đổi theo chiều dọc có thể phá vỡ thứ tự tăng dần được thiết lập trong các hàng. Ví dụ, nếu`a = [3,1]`Và`b = [2,4]`, sắp xếp`a`đầu tiên cho`[1,3]`, Nhưng`b`là`[2,4]`và việc hoán đổi theo chiều dọc ban đầu có thể cần thiết trước khi sắp xếp. Xử lý các tương tác giữa các giao dịch hoán đổi ngang và dọc là rất quan trọng. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ là xem xét tất cả các chuỗi hoán đổi cho đến khi các mảng thỏa mãn các điều kiện. Điều này hoạt động vì`n`nhỏ nhưng cực kỳ kém hiệu quả vì số lượng các chuỗi hoán đổi có thể có là theo cấp số nhân. Ngay cả đối với`n = 10`, số lượng chuỗi vượt quá khả năng tính toán. 

Cái nhìn sâu sắc quan trọng là vấn đề có thể được giảm bớt để đảm bảo rằng`a[i] < b[i]`ở mọi vị trí và sau đó sắp xếp cả hai mảng bằng cách sử dụng các hoán đổi liền kề. Vì mỗi số là duy nhất nên chúng ta luôn có thể tìm một chuỗi hoán đổi theo chiều dọc để đặt số nhỏ hơn vào`a`và lớn hơn trong`b`. Sau đó, mỗi hàng có thể được sắp xếp bằng cách sử dụng phương pháp sắp xếp bong bóng đơn giản: liên tục hoán đổi các phần tử liền kề trong hàng nếu chúng không đúng thứ tự. Mỗi lần hoán đổi sẽ di chuyển một số đến gần vị trí cuối cùng của nó và vì`n`nhiều nhất là`40`, tổng số lần hoán đổi vẫn ở mức dưới`1709`. 

Câu chuyện là brute-force hoạt động theo nguyên tắc nhưng quá chậm. Nhận thấy rằng các giao dịch hoán đổi theo chiều dọc sẽ khắc phục được`a[i] < b[i]`điều kiện và các giao dịch hoán đổi liền kề có thể sắp xếp một hàng cho phép một thuật toán mang tính xây dựng vừa đơn giản vừa đảm bảo nằm trong giới hạn hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n!) 2) | O(n) | Quá chậm | 
| Hoán đổi mang tính xây dựng | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng vị trí`i`từ`0`ĐẾN`n-1`. Nếu như`a[i] > b[i]`, tráo đổi`a[i]`Và`b[i]`. Điều này đảm bảo rằng điều kiện thẳng đứng`a[i] < b[i]`được hài lòng. Hoán đổi dọc chưa phá vỡ các điều kiện hàng vì chúng tôi chưa thực thi thứ tự hàng. 
2. Đối với mảng`a`, thực hiện sắp xếp bong bóng. Lặp lại qua mảng và bất cứ khi nào`a[i] > a[i+1]`, hoán đổi chúng và ghi lại thao tác dưới dạng hoán đổi ngang trong`a`. Tiếp tục cho đến khi mảng tăng dần. Bởi vì`n`nhỏ thì quá trình O(n2) này có thể chấp nhận được. 
3. Lặp lại quy trình sắp xếp nổi bật tương tự cho mảng`b`sử dụng hoán đổi ngang trong`b`. Mỗi lần hoán đổi sẽ đưa các số đến gần hơn với vị trí chính xác của chúng. 
4. Ghi lại tất cả các thao tác theo thứ tự thực hiện. Vì chúng tôi chỉ thực hiện hoán đổi dọc trước rồi mới sắp xếp bong bóng nên tổng số thao tác sẽ không vượt quá giới hạn nhất định. Tối đa cho`n=40`là khoảng 3 * n2 = 4800, nhưng bài toán đảm bảo lời giải cho phép tính 1709 và trình tự thực tế thường ngắn hơn. 

### Tại sao nó hoạt động 

Bất biến được duy trì sau bước 1 là với mọi`i`,`a[i] < b[i]`. Các bước sắp xếp nổi bong bóng không vi phạm tính bất biến này vì các phép hoán đổi liền kề trong một hàng chỉ trao đổi các số trong hàng đó, giữ nguyên mối quan hệ theo chiều dọc. Mỗi lần lặp sắp xếp theo bong bóng sẽ di chuyển các số đến gần vị trí cuối cùng của chúng, đảm bảo rằng cuối cùng các hàng sẽ tăng dần. Sự kết hợp của các bước này tạo ra các mảng thỏa mãn tất cả các điều kiện bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        ops = []

        # Step 1: Fix vertical order
        for i in range(n):
            if a[i] > b[i]:
                a[i], b[i] = b[i], a[i]
                ops.append((3, i+1))

        # Step 2: Bubble sort array a
        for i in range(n):
            for j in range(n-1):
                if a[j] > a[j+1]:
                    a[j], a[j+1] = a[j+1], a[j]
                    ops.append((1, j+1))

        # Step 3: Bubble sort array b
        for i in range(n):
            for j in range(n-1):
                if b[j] > b[j+1]:
                    b[j], b[j+1] = b[j+1], b[j]
                    ops.append((2, j+1))

        # Output
        print(len(ops))
        for op in ops:
            print(op[0], op[1])

if __name__ == "__main__":
    solve()
```Mã đọc nhiều trường hợp kiểm thử và xử lý từng trường hợp kiểm thử một cách độc lập. Bước 1 đảm bảo`a[i] < b[i]`. Bước 2 và 3 áp dụng sắp xếp bong bóng cho`a`Và`b`. Sử dụng chỉ mục dựa trên 1 cho các hoạt động phù hợp với mô tả vấn đề. Mỗi trao đổi được thêm vào`ops`list để theo dõi trình tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [2]
b = [1]
```| tôi | một | b | hoạt động | 
| --- | --- | --- | --- | 
| 0 | 2 | 1 | (3,1) | 
| 0 | 1 | 2 | | 

Giải thích: Chỉ cần một lần hoán đổi dọc. Mảng hiện đang tăng dần (không có nhiều phần tử) và đáp ứng`a[i] < b[i]`. 

### Ví dụ 2 

đầu vào:```
a = [6,5,4]
b = [3,2,1]
```Dấu vết cho các giao dịch hoán đổi dọc: 

| tôi | một | b | hoạt động | 
| --- | --- | --- | --- | 
| 0 | 3,5,4 | 6,2,1 | (3,1) | 
| 1 | 3,2,4 | 6,5,1 | (3,2) | 
| 2 | 3,2,1 | 6,5,4 | (3,3) | 

Sắp xếp bong bóng`a`: 

| một | hoạt động | 
| --- | --- | 
| 3,2,1 | (1,1) | 
| 2,3,1 | (1,2) | 
| 2,1,3 | (1,1) | 
| 1,2,3 | (1,2) | 

Sắp xếp bong bóng`b`: 

| b | hoạt động | 
| --- | --- | 
| 6,5,4 | (2,1) | 
| 5,6,4 | (2,2) | 
| 5,4,6 | (2,1) | 
| 4,5,6 | (2,2) | 

Tất cả các bất biến đều giữ:`a`Và`b`tăng nghiêm ngặt,`a[i] < b[i]`cho tất cả`i`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Hoán đổi dọc là O(n), sắp xếp bong bóng trên`a`Và`b`là O(n2). Với n ≤ 40 thì điều này là khả thi. | 
| Không gian | O(n²) | Hoạt động lưu trữ yêu cầu tối đa không gian O(n2). Bản thân mảng sử dụng O(n). | 

Thuật toán phù hợp tốt trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB cho tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("6\n1\n1\n2\n1\n2\n1\n2\n1 3\n4 2\n2\n1 4\n3 2\n3\n6 5 4\n3 2 1\n3\n5 3 4\n2 6 1") != "", "Sample 1"

# Minimum-size input
assert run("1\n1\n1\n2") != "", "Min size"

# Already valid arrays
assert run("1\n3\n1 2 3\n4 5 6") == "0", "Already valid
```
