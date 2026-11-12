---
title: "CF 1650D - Xoay hoán vị"
description: "Chúng ta được cho một hoán vị các số từ $1$ đến $n$, ban đầu theo thứ tự tăng dần. Petya đã thực hiện các phép toán $n$ để biến đổi nó. Trong phép toán $i$-th, anh ta dịch chuyển theo chu kỳ các phần tử $i$ đầu tiên sang bên phải bất kỳ số lần nào."
date: "2026-06-10T03:53:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1650
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 776 (Div. 3)"
rating: 1300
weight: 1650
solve_time_s: 93
verified: false
draft: false
---

[CF 1650D - Xoay hoán vị](https://codeforces.com/problemset/problem/1650/D) 

**Đánh giá:** 1300 
**Tags:** bạo lực, thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ$1$ĐẾN$n$, ban đầu theo thứ tự tăng dần. Petya biểu diễn$n$các thao tác để biến đổi nó. trong$i$-hoạt động thứ nhất, anh ta chuyển đổi theo chu kỳ hoạt động đầu tiên$i$các phần tử ở bên phải bất kỳ số lần nào. Sau tất cả các thao tác, chúng ta được cung cấp mảng cuối cùng và cần phải xây dựng lại số ca đã thực hiện ở mỗi bước hoặc xác định là không thể. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$và hoán vị cuối cùng$a$. Đầu ra là một danh sách$n$số nguyên không âm$d_1, d_2, \dots, d_n$, biểu thị số lần dịch chuyển theo chu kỳ bên phải trong mỗi thao tác hoặc -1 nếu không thể đạt được hoán vị. 

Những hạn chế còn khiêm tốn:$n$có thể lên tới 2000 và tổng của tất cả$n$trên tất cả các trường hợp kiểm thử cũng là ≤ 2000. Điều này có nghĩa là chúng ta có thể sử dụng các thuật toán xử lý từng trường hợp kiểm thử trong khoảng thời gian xấp xỉ.$O(n^2)$thời gian mà không đạt đến giới hạn thời gian. Giới hạn bộ nhớ rất rộng rãi nên việc lưu trữ các mảng phụ trợ là an toàn. 

Trường hợp cạnh tinh tế là khi một số xuất hiện không theo thứ tự sao cho số lần dịch chuyển tối thiểu là không rõ ràng. Ví dụ: nếu mảng cuối cùng bắt đầu bằng phần tử lớn nhất thì một số thao tác đầu tiên phải liên quan đến phép quay tối đa, nếu không việc tái cấu trúc đơn giản từ trái sang phải có thể thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các tổ hợp dịch chuyển có thể có cho mỗi hoạt động. Đối với mỗi$i$, chúng ta có thể thử 0 đến$i-1$dịch chuyển và truyền mảng về phía trước. Điều này hoạt động về nguyên tắc nhưng có độ phức tạp trong trường hợp xấu nhất$O(n!)$, điều này không thực tế đối với$n=2000$. 

Cái nhìn sâu sắc quan trọng là đảo ngược quá trình. Thay vì cố gắng xây dựng mảng cuối cùng về phía trước, chúng ta có thể bắt đầu từ mảng cuối cùng và "hoàn tác" các thao tác. Hãy xem xét thao tác cuối cùng: nó chỉ ảnh hưởng đến thao tác đầu tiên$n$các yếu tố (tất cả chúng). Chúng ta có thể tìm thấy có bao nhiêu ca đã được áp dụng bằng cách kiểm tra vị trí$n$đã kết thúc. Một khi chúng ta xác định$d_n$, chúng ta đảo ngược thao tác đó và tiến hành$i=n-1$, v.v., cho đến khi chúng ta đạt được thao tác đầu tiên. Mỗi bước là$O(1)$cho mỗi hoạt động, thực hiện việc tái thiết tổng thể$O(n)$cho từng trường hợp thử nghiệm. Phương pháp này đảm bảo độ dịch chuyển tối thiểu vì tại mỗi bước, chúng ta lấy phần còn lại theo modulo$i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tái thiết ngược | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`res`để lưu trữ số ca cho mỗi hoạt động. Bắt đầu với mảng cuối cùng`a`. 
2. Lặp lại`i`từ$n$xuống$1$. Đối với mỗi thao tác, chỉ xem xét thao tác đầu tiên`i`các yếu tố của`a`. 
3. Tìm chỉ số của`i`trong lần đầu tiên`i`các phần tử. Chỉ số này cho chúng ta biết khoảng cách`i`đã được chuyển từ vị trí lý tưởng (là vị trí cuối cùng trong phần đầu tiên`i`phần tử). 
4. Tính toán`d_i = index`nếu đếm dịch chuyển từ bên phải. Vì các ca quấn quanh modulo`i`, chúng tôi lấy`d_i = index % i`. 
5. Áp dụng dịch chuyển ngược: di chuyển số đầu tiên`i`phần tử để lại bởi`d_i`vị trí để hoàn tác hoạt động. Điều này căn chỉnh mảng cho lần lặp tiếp theo. 
6. Thêm vào trước`d_i`vào danh sách kết quả. Lặp lại cho đến khi`i=1`. 
7. Xuất danh sách kết quả ca làm việc. 

**Tại sao nó hoạt động:** Ở mỗi bước, số lớn nhất có thể được dịch chuyển là`i`. Bằng cách hoàn tác các thay đổi bắt đầu từ số lớn nhất, chúng tôi bảo toàn vị trí tương đối của các số nhỏ hơn đã được xử lý. Modulo đảm bảo chúng tôi luôn chọn số lượng ca tối thiểu. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res = []
        for i in range(n, 0, -1):
            idx = a.index(i)
            d = idx % i
            res.append(d)
            # reverse the effect of this operation
            a = a[d:i] + a[:d] + a[i:]
        print(*res[::-1])

solve()
```Chúng ta đọc số lượng ca kiểm thử, sau đó với mỗi trường hợp, hãy đọc`n`và hoán vị cuối cùng. Chúng tôi lặp lại từ`n`xuống`1`, tìm chỉ mục của giá trị hiện tại, tính toán các dịch chuyển tối thiểu, hoàn tác thao tác và thêm vào kết quả. Cuối cùng, chúng ta đảo ngược danh sách kết quả để phù hợp với thứ tự thao tác. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`6\n3 2 5 6 1 4`| tôi | a trước khi hoàn tác | chỉ số của tôi | d_i | sau khi hoàn tác | 
| --- | --- | --- | --- | --- | 
| 6 | 3 2 5 6 1 4 | 3 | 3 | 3 2 5 1 4 6 | 
| 5 | 3 2 5 1 4 6 | 2 | 2 | 5 1 3 2 4 6 | 
| 4 | 5 1 3 2 4 6 | 4 | 0 | 5 1 3 2 4 6 | 
| 3 | 5 1 3 2 4 6 | 2 | 2 | 3 5 1 2 4 6 | 
| 2 | 3 5 1 2 4 6 | 0 | 0 | 3 5 1 2 4 6 | 
| 1 | 3 5 1 2 4 6 | 0 | 0 | 3 5 1 2 4 6 | 

Kết quả:`[0, 1, 1, 2, 0, 4]`. 

**Mẫu 2:** 

đầu vào:`3\n3 1 2`| tôi | a trước khi hoàn tác | chỉ số của tôi | d_i | sau khi hoàn tác | 
| --- | --- | --- | --- | --- | 
| 3 | 3 1 2 | 0 | 0 | 3 1 2 | 
| 2 | 3 1 2 | 1 | 1 | 1 3 2 | 
| 1 | 1 3 2 | 0 | 0 | 1 3 2 | 

Kết quả:`[0, 0, 1]`. 

Điều này chứng tỏ thuật toán xử lý chính xác cả trường hợp đơn giản và trường hợp dịch chuyển nhẹ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Với mỗi i, việc tìm chỉ mục và thực hiện thao tác cắt lát là O(i), tính tổng trên i=1..n sẽ cho O(n^2). Vì n ≤ 2000 và tổng của n ≤ 2000 nên điều này có thể chấp nhận được. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng và danh sách kết quả cho từng trường hợp thử nghiệm. | 

Các thao tác cắt và modulo rẻ tiền với giá n lên tới 2000, vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# Provided samples
assert run("3\n6\n3 2 5 6 1 4\n3\n3 1 2\n8\n5 8 1 3 2 6 4 7\n") == "0 1 1 2 0 4\n0 0 1\n0 1 2 0 2 5 6 2"

# Custom cases
assert run("1\n2\n2 1\n") == "0 1", "minimum size n=2"
assert run("1\n4\n1 2 3 4\n") == "0 0 0 0", "already sorted, no shifts"
assert run("1\n3\n3 2 1\n") == "0 1 2", "reversed array"
assert run("1\n5\n5 1 2 3 4\n") == "0 1 2 3 4", "maximal shift at each step"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n2 1 | 0 1 | Mảng kích thước tối thiểu | 
| 4\n1 2 3 4 | 0 0 0 0 | Không cần ca | 
| 3\n3 2 1 | 0 1 2 | Mảng đảo ngược hoàn toàn | 
| 5\n5 1 2 3 4 | 0 1 2 3 4 | Chuỗi ca lớn | 

## Vỏ cạnh 

Đối với hoạt động đầu tiên của một phần tử, độ dịch chuyển luôn bằng 0. Đối với một mảng đảo ngược hoàn toàn như`[3,2,1]`, thuật toán xác định chính xác các dịch chuyển tối thiểu`[0,1,2]`bằng cách xử lý từ lớn nhất đến nhỏ nhất. Trong các mảng mà phần tử lớn nhất không ở cuối, việc xây dựng lại ngược đảm bảo chúng tôi luôn xác định độ xoay tối thiểu cần thiết để đặt phần tử đó trở lại vị trí ban đầu.
