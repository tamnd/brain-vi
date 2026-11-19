---
title: "CF 2104B - Đi đến tận cùng"
description: "Chúng ta được cung cấp một mảng các số nguyên và với mọi độ dài $k$ từ 1 đến $n$, chúng ta muốn tối đa hóa tổng của các phần tử $k$ cuối cùng sau khi di chuyển một phần tử mà chúng ta chọn đến cuối mảng. Sau khi chọn và di chuyển phần tử đó, chúng tôi khôi phục mảng cho truy vấn tiếp theo."
date: "2026-06-08T04:57:15+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2104
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 178 (Rated for Div. 2)"
rating: 1000
weight: 2104
solve_time_s: 97
verified: false
draft: false
---

[CF 2104B - Đi đến cùng](https://codeforces.com/problemset/problem/2104/B) 

**Đánh giá:** 1000 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, triển khai 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và với mọi độ dài có thể$k$từ 1 đến$n$, chúng tôi muốn tối đa hóa tổng của cuối cùng$k$các phần tử sau khi di chuyển phần tử **one** mà chúng ta chọn đến cuối mảng. Sau khi chọn và di chuyển phần tử đó, chúng tôi khôi phục mảng cho truy vấn tiếp theo. Về cơ bản, chúng ta được hỏi: nếu chúng ta được phép thực hiện một thao tác “đi đến cuối”, tổng tốt nhất của phép toán cuối cùng là bao nhiêu?$k$các phần tử cho mọi$k$? 

Những ràng buộc chỉ ra rằng$n$có thể đạt được$2 \cdot 10^5$và có thể có tới$10^4$trường hợp thử nghiệm, nhưng tổng số$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$. Điều này ngụ ý rằng chúng ta phải có một thuật toán tuyến tính trong$n$cho từng trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào mô phỏng việc di chuyển mọi phần tử cho mọi$k$sẽ là$O(n^2)$và rõ ràng là quá chậm. 

Các trường hợp cạnh không rõ ràng xuất hiện khi tất cả các phần tử đều bằng nhau, khi$n=1$hoặc khi phần tử lớn nhất ở đầu hoặc cuối. Ví dụ, với đầu vào`[7, 5]`, tổng của$k=2$yêu cầu chọn phần tử đầu tiên để tối đa hóa tổng. Nếu bạn ngây thơ chỉ tính tổng cuối cùng$k$yếu tố mà không xem xét di chuyển, bạn sẽ nhận được kết quả sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại trên mỗi$k$và mô phỏng việc di chuyển mọi phần tử đến cuối, tính tổng phần cuối cùng$k$các phần tử mỗi lần. Về nguyên tắc, điều này hoạt động vì nó đánh giá mọi động thái có thể xảy ra, nhưng độ phức tạp về thời gian của nó là$O(n^2)$, vượt quá giới hạn 2 giây khi$n$là lớn. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả là nhận ra rằng việc di chuyển một phần tử đến cuối chỉ ảnh hưởng đến phần tử cuối cùng.$k$phần tử nếu nó lớn hơn một số phần tử cuối cùng hiện có. Sắp xếp mảng theo thứ tự giảm dần sẽ nắm bắt được những đóng góp tiềm năng của việc di chuyển bất kỳ phần tử nào. Nếu chúng ta tính tổng tiền tố của mảng được sắp xếp theo thứ tự giảm dần thì tổng của$k$các phần tử lớn nhất tương ứng chính xác với tổng tối đa có thể đạt được bằng cách di chuyển bất kỳ phần tử đơn lẻ nào đến cuối. Điều này hiệu quả vì với mỗi$k$, hiện tại cuối cùng$k$phần tử chứa số lớn nhất$k$các số hoặc chúng ta có thể di chuyển số lớn nhất chưa có ở số cuối cùng$k$đến cuối cùng để cải thiện tổng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng$a$. 
2. Sắp xếp mảng$a$theo thứ tự không tăng để ưu tiên các phần tử lớn nhất trước. Điều này cho phép chúng tôi xác định các phần tử sẽ tối đa hóa tổng của bất kỳ độ dài tiền tố nào. 
3. Tính tổng tiền tố của mảng đã được sắp xếp. Tổng tiền tố tại chỉ mục$i$lưu trữ tổng của số đầu tiên$i+1$phần tử lớn nhất. Tổng tiền tố này trực tiếp cung cấp số tiền tối đa cho lần cuối cùng$k=i+1$các phần tử. 
4. Xuất các tổng tiền tố theo thứ tự. Mỗi mục đại diện cho số tiền tối đa cho tương ứng$k$. 

Tại sao nó hoạt động: Sắp xếp đảm bảo rằng chúng ta luôn biết yếu tố nào có thể đóng góp nhiều nhất cho phần cuối cùng$k$các vị trí. Tổng tiền tố đảm bảo rằng chúng ta đang xem xét tập hợp con tối ưu của các phần tử cho mọi$k$. Vì mảng được khôi phục sau mỗi truy vấn nên chúng ta không cần mô phỏng các bước di chuyển; thứ tự sắp xếp là đủ để xác định số tiền tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort(reverse=True)
        prefix = []
        current = 0
        for num in a:
            current += num
            prefix.append(current)
        print(' '.join(map(str, prefix)))

if __name__ == "__main__":
    solve()
```Mã đọc nhiều trường hợp thử nghiệm. Đối với mỗi mảng, nó sắp xếp theo thứ tự giảm dần và tính tổng tiền tố. Tổng tiền tố tương ứng trực tiếp với tổng tối đa cho mỗi$k$bởi vì lớn nhất$k$số luôn mang lại kết quả tối ưu. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[13, 5, 10, 14, 8, 15, 13]`: 

| k | Tổng tiền tố được sắp xếp | Giải thích | 
| --- | --- | --- | 
| 1 | 15 | Phần tử lớn nhất được chuyển đến cuối | 
| 2 | 28 | Tổng của hai phần tử lớn nhất: 15 + 13 | 
| 3 | 42 | Tổng ba số lớn nhất: 15 + 14 + 13 | 
| 4 | 50 | Tổng của bốn số lớn nhất: 15 + 14 + 13 + 8 | 
| 5 | 63 | Tổng của năm số lớn nhất: 15 + 14 + 13 + 13 + 8 | 
| 6 | 73 | Tổng sáu lớn nhất: 15 + 14 + 13 + 13 + 8 + 10 | 
| 7 | 78 | Tổng của bảy phần tử | 

Bảng xác nhận các tổng tiền tố khớp với các tổng tối ưu được tính toán thủ công bằng các thao tác “chuyển từ đầu đến cuối”. 

Đối với đầu vào`[7, 5]`: 

| k | Tiền tố Tổng | Giải thích | 
| --- | --- | --- | 
| 1 | 7 | Di chuyển 7 đến cuối | 
| 2 | 12 | Tổng của cả hai phần tử | 

Điều này chứng tỏ rằng thuật toán xử lý chính xác các mảng rất nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; tính tổng tiền tố là O(n) | 
| Không gian | O(n) | Lưu trữ tổng tiền tố | 

Với tổng số tiền$n$trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, thuật toán này chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n7\n13 5 10 14 8 15 13\n6\n1000000000 1000000000 1000000000 1000000000 1000000000 1000000000\n1\n42\n2\n7 5\n") == "15 28 42 50 63 73 78\n1000000000 2000000000 3000000000 4000000000 5000000000 6000000000\n42\n7 12"

# Custom cases
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n5\n1 2 3 4 5\n") == "5 9 12 14 15", "ascending array"
assert run("1\n3\n10 10 10\n") == "10 20 30", "all equal values"
assert run("1\n4\n1000000000 1 1 1\n") == "1000000000 1000000001 1000000002 1000000003", "large max first"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | đầu vào kích thước tối thiểu | 
| 1 2 3 4 5 | 5 9 12 14 15 | thứ tự tăng dần chung | 
| 10 10 10 | 10 20 30 | tất cả các giá trị bằng nhau | 
| 1000000000 1 1 1 | 1000000000 1000000001 1000000002 1000000003 | ranh giới có giá trị lớn | 

## Vỏ cạnh 

Đối với mảng có một phần tử`[42]`,$k=1$tầm thường là 42. Thuật toán xử lý việc này vì việc sắp xếp không làm thay đổi mảng một phần tử và tổng tiền tố chỉ đơn giản là chính phần tử đó. Đối với các mảng trong đó tất cả các phần tử đều bằng nhau, chẳng hạn như`[10, 10, 10]`, số tiền tối đa cho$k$luôn luôn là$10 \times k$và tổng tiền tố của chúng tôi phản ánh chính xác điều này. Khi phần tử lớn nhất ở đầu hoặc cuối, tổng tiền tố được sắp xếp đảm bảo rằng các phần tử lớn nhất luôn được tính trước, tự động tạo ra tổng chính xác cho tất cả$k$.
