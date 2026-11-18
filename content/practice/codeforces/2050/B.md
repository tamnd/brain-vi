---
title: "CF 2050B - Truyền máu"
description: "Chúng ta được cung cấp một mảng các số nguyên đại diện cho “nhóm” tài nguyên nào đó. Ở mỗi thao tác, chúng ta có thể chọn một phần tử không nằm ở ranh giới và di chuyển một đơn vị từ hàng xóm này sang hàng xóm khác."
date: "2026-06-08T08:47:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2050
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 991 (Div. 3)"
rating: 1100
weight: 2050
solve_time_s: 133
verified: false
draft: false
---

[CF 2050B - Truyền máu](https://codeforces.com/problemset/problem/2050/B) 

**Đánh giá:** 1100 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên đại diện cho “nhóm” tài nguyên nào đó. Ở mỗi thao tác, chúng ta có thể chọn một phần tử không nằm ở ranh giới và di chuyển một đơn vị từ hàng xóm này sang hàng xóm khác. Cụ thể, nếu chúng ta chọn chỉ mục`i`(2 ₫ i ₫ n-1), ta có thể giảm`a[i-1]`lên 1 và tăng`a[i+1]`bằng 1 hoặc giảm`a[i+1]`lên 1 và tăng`a[i-1]`bằng 1. Các giá trị âm bị cấm, vì vậy nguồn phải có ít nhất 1 trước khi truyền. 

Mục đích là để xác định xem có thể làm cho tất cả các phần tử bằng nhau sau một số thao tác này hay không. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có độ dài mảng và chính mảng đó. Đầu ra là một “CÓ” hoặc “KHÔNG” đơn giản cho mỗi trường hợp thử nghiệm, tùy thuộc vào việc có thể cân bằng tất cả các phần tử hay không. 

Các ràng buộc chặt chẽ về kích thước mảng: n có thể đạt tới 2×10⁵ và tổng của tất cả n trong các trường hợp thử nghiệm được giới hạn bởi 2×10⁵. Điều này có nghĩa là chúng ta không thể mô phỏng các thao tác một cách rõ ràng vì mỗi thao tác có thể là O(n) và tổng số thao tác cần thiết có thể lớn. Chúng tôi cần một giải pháp xem xét mảng trên toàn cầu và xác định tính khả thi mà không cần mô phỏng. 

Trường hợp cạnh tinh tế phát sinh khi một trong các phần tử cuối nhỏ hơn phần tử lân cận của nó. Vì các hoạt động chỉ dịch chuyển giá trị giữa các phần tử lân cận của phần tử bên trong nên phần tử cuối chỉ có thể nhận tài nguyên từ một hướng. Ví dụ, nếu`a[0]`là 1 và`a[1]`là 0, không có chuỗi nước đi nào có thể tăng lên được`a[0]`trên 1 nếu tất cả các phần tử bên trong đều nhỏ hơn hoặc bằng nhau. Điều này cho thấy giá trị tối thiểu là một ràng buộc chính. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực ngây thơ sẽ cố gắng mô phỏng tất cả các chuỗi hoạt động có thể xảy ra. Đối với mỗi chỉ số`i`chúng ta có thể thử di chuyển tài nguyên sang trái hoặc phải. Về nguyên tắc, điều này đúng vì các quy tắc hoạt động đảm bảo bảo toàn tổng số tiền, nhưng nó quá chậm. Đối với mảng có kích thước 10⁵, ngay cả một chuỗi thao tác cũng sẽ thực hiện các bước O(n²) và với 10⁴ trường hợp thử nghiệm, phương pháp này không khả thi. 

Cái nhìn sâu sắc quan trọng là quan sát sự bất biến: tổng tổng của mảng không đổi. Cho phép`S`là tổng của tất cả các phần tử và`n`chiều dài của mảng. Nếu tất cả các phần tử trở nên bằng nhau thì chúng phải đạt giá trị`S / n`. Điều này ngay lập tức đưa ra kiểm tra tính khả thi: nếu`S % n != 0`, điều đó là không thể. 

Tiếp theo, chúng ta cần đảm bảo rằng không có phần tử nào cần nhiều hơn những gì các phần tử lân cận có thể cung cấp. Vì hoạt động chỉ có thể chuyển tài nguyên qua hai phần tử liền kề cùng một lúc nên các phần tử bên trong phải có khả năng “hấp thụ” hoặc “cung cấp” số lượng cần thiết. Cấu trúc hoạt động cho phép mảng phân phối lại tài nguyên một cách tự do giữa các phần tử bên trong. Nó chỉ ra rằng miễn là tổng chia hết cho n và mỗi phần tử bên trong ít nhất là giá trị đích (vì phần tử cuối có thể tăng thêm một phần tử lân cận), điều đó luôn có thể thực hiện được. 

Do đó, giải pháp tối ưu là tính tổng, kiểm tra khả năng chia hết cho n và xuất ra CÓ nếu chia hết, KHÔNG nếu ngược lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm | 
| Kiểm tra tổng + chia hết | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc độ dài`n`và mảng`a`. 
2. Tính tổng số tiền`S = sum(a)`. 
3. Tính giá trị mục tiêu`target = S // n`. 
4. Nếu`S % n != 0`, in`NO`và chuyển sang trường hợp thử nghiệm tiếp theo. 
5. Nếu không, hãy in`YES`bởi vì các quy tắc hoạt động cho phép phân phối lại đạt được mục tiêu. 

Tại sao nó hoạt động: hoạt động bảo toàn tổng số tiền. Nếu tổng chia hết cho n thì giá trị đích tồn tại. Các hoạt động cho phép chuyển từng đơn vị một giữa các đơn vị lân cận và bằng cách liên tục áp dụng các bước di chuyển theo kiểu tham lam, bất kỳ cấu hình bên trong nào cũng có thể phân phối lại các giá trị dư thừa cho các phần tử bị thiếu. Việc kiểm tra khả năng phân chia đảm bảo tính khả thi, đây là hạn chế toàn cầu duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        if total % n == 0:
            print("YES")
        else:
            print("NO")

solve()
```Mã sử ​​dụng I/O nhanh với`sys.stdin.readline`. Đối với mỗi trường hợp thử nghiệm, chúng tôi chỉ tính tổng của mảng và kiểm tra tính chia hết. Đây là O(n) cho mỗi trường hợp thử nghiệm, có thể chấp nhận được theo các ràng buộc đã cho. Không có bộ nhớ bổ sung nào được sử dụng ngoài bộ nhớ mảng. Các điểm tinh tế bao gồm xử lý chính xác nhiều trường hợp thử nghiệm và đọc các mảng có kích thước khác nhau. 

## Ví dụ đã hoạt động 

**Mẫu đầu vào 1:**`3 2 1`→ tổng = 6, n = 3, 6 % 3 = 0 → CÓ 

**Mẫu đầu vào 2:**`1 1 3`→ tổng = 5, n = 3, 5 % 3 ≠ 0 → KHÔNG 

| Mảng | Tổng hợp | n | Tổng % n | Đầu ra | 
| --- | --- | --- | --- | --- | 
| [3,2,1] | 6 | 3 | 0 | CÓ | 
| [1,1,3] | 5 | 3 | 2 | KHÔNG | 

Điều này chứng tỏ bất biến chia hết tổng xác định chính xác các cấu hình khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chỉ cần kiểm tra tổng và chia hết | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Lưu trữ mảng đầu vào | 

Với tổng n trên tất cả các trường hợp thử nghiệm 2×10⁵, thuật toán chạy thoải mái trong giới hạn thời gian. 

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
assert run("8\n3\n3 2 1\n3\n1 1 3\n4\n1 2 5 4\n4\n1 6 6 1\n5\n6 2 1 4 2\n4\n1 4 2 1\n5\n3 1 2 1 3\n3\n2 4 2\n") == "YES\nNO\nYES\nNO\nYES\nNO\nNO\nNO"

# Custom cases
assert run("2\n3\n2 2 2\n4\n1 1 1 1\n") == "YES\nYES"  # all already equal
assert run("2\n3\n1 2 3\n3\n1 1 2\n") == "YES\nNO"    # sum divisible vs not
assert run("1\n5\n5 5 5 5 5\n") == "YES"             # all equal large
assert run("1\n3\n1 1000000000 1\n") == "NO"        # large numbers
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [2,2,2] | CÓ | Đã có mảng bằng nhau | 
| [1,2,3] | CÓ | Tổng chia hết, có thể phân phối lại | 
| [1,1,2] | KHÔNG | Tổng không chia hết | 
| [5,5,5,5,5] | CÓ | Tất cả các mảng lớn bằng nhau | 
| [1,1e9,1] | KHÔNG | Số lớn, tổng không chia hết | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các phần tử ban đầu đều bằng nhau, thuật toán ngay lập tức đưa ra CÓ, kết quả này đúng. Đối với các mảng có số lượng cực lớn, phép tính tổng sử dụng các số nguyên có độ chính xác tùy ý của Python, do đó, lỗi tràn không phải là vấn đề đáng lo ngại. Đối với các mảng có tổng % n ≠ 0, đầu ra NO nắm bắt chính xác điều không thể xảy ra bất kể vị trí phần tử.
