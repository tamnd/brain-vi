---
title: "CF 1920A - Thỏa mãn các ràng buộc"
description: "Chúng ta được yêu cầu xác định có bao nhiêu số nguyên thỏa mãn một tập hợp ràng buộc. Mỗi trường hợp thử nghiệm bao gồm nhiều ràng buộc trên một số nguyên $k$. Các ràng buộc thuộc ba loại: $k$ ít nhất phải bằng $x$, $k$ phải nhiều nhất là $x$, và $k$ không được bằng $x$."
date: "2026-06-08T19:28:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1920
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 919 (Div. 2)"
rating: 800
weight: 1920
solve_time_s: 123
verified: true
draft: false
---

[CF 1920A - Thỏa mãn các ràng buộc](https://codeforces.com/problemset/problem/1920/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định có bao nhiêu số nguyên thỏa mãn một tập hợp ràng buộc. Mỗi trường hợp thử nghiệm bao gồm nhiều ràng buộc trên một số nguyên duy nhất$k$. Các ràng buộc được chia thành ba loại:$k$ít nhất phải có$x$,$k$phải nhiều nhất$x$, Và$k$không được bằng$x$. Mục tiêu của chúng ta là đếm tất cả các số nguyên thỏa mãn đồng thời tất cả các ràng buộc này. 

Các ràng buộc thuộc loại "ít nhất$x$" cho chúng tôi giới hạn dưới cho$k$. Giới hạn dưới mạnh nhất xuất phát từ việc lấy tối đa tất cả các ràng buộc như vậy. Tương tự, các ràng buộc thuộc loại "nhiều nhất$x$" đưa ra giới hạn trên và giới hạn trên chặt chẽ nhất là mức tối thiểu của các ràng buộc này. Nhập "không bằng$x$" các ràng buộc chỉ đơn giản là loại bỏ các số nguyên nhất định khỏi khoảng được xác định bởi giới hạn dưới và giới hạn trên. 

Vì đầu vào đảm bảo ít nhất một ràng buộc loại 1 và một ràng buộc loại 2 nên tập hợp các số nguyên khả thi luôn hữu hạn. Tuy nhiên, nếu giới hạn dưới vượt quá giới hạn trên sau khi xử lý tất cả các ràng buộc loại 1 và loại 2 thì không có số nguyên nào thỏa mãn các ràng buộc đó. Một cách tiếp cận đơn giản để kiểm tra mọi số nguyên giữa các giới hạn là khả thi đối với phạm vi nhỏ nhưng sẽ thất bại đối với phạm vi lớn vì mức tối đa$x$có thể$10^9$. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó tất cả các số nguyên trong khoảng bị loại trừ bởi ràng buộc loại 3 hoặc khi khoảng giảm xuống còn một số duy nhất. Ví dụ, nếu các ràng buộc là$k \ge 5$,$k \le 5$, Và$k \neq 5$, không có số nguyên nào thỏa mãn điều kiện, tạo ra 0. Cách tiếp cận bất cẩn có thể tính đúng khoảng nhưng quên trừ các loại trừ loại 3 hoặc trừ quá nhiều nếu chúng nằm ngoài khoảng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên mọi số nguyên từ giới hạn dưới tối thiểu đến giới hạn trên tối đa, kiểm tra từng số nguyên theo tất cả các ràng buộc loại 3. Điều này có hiệu quả vì các ràng buộc rất đơn giản, nhưng nó trở nên không khả thi khi độ dài khoảng tiến đến gần$10^9$. Đối với một trường hợp thử nghiệm duy nhất, việc này sẽ mất tới$10^9$hoạt động, vượt xa giới hạn 1 giây. 

Điều quan trọng là chúng ta không cần phải kiểm tra từng số nguyên riêng lẻ. Một khi chúng ta biết giới hạn dưới tối đa$L$và giới hạn trên tối thiểu$R$, các giá trị có thể có của$k$tạo thành một khoảng liền kề$[L, R]$. Ràng buộc loại 3 chỉ loại bỏ các số nguyên cụ thể khỏi khoảng này. Do đó, câu trả lời chỉ đơn giản là độ dài của khoảng trừ đi số giá trị loại 3 nằm bên trong nó. Điều này làm giảm vấn đề xuống còn một vài thao tác tối đa/phút và quét tuyến tính đơn giản qua các ràng buộc loại 3. Giải pháp thu được có độ phức tạp về thời gian$O(n)$cho mỗi trường hợp thử nghiệm, hiệu quả được đưa ra$n \le 100$Và$t \le 500$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((R-L+1) * n) | O(1) | Quá chậm đối với R-L lớn | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai biến$L$Và$R$để theo dõi khoảng thời gian khả thi. Bộ$L$đến 1 và$R$ĐẾN$10^9$, giá trị tối thiểu và tối đa có thể có của$k$. 
2. Lặp lại tất cả các ràng buộc. Đối với các ràng buộc loại 1, hãy cập nhật$L$đến mức tối đa của dòng điện$L$và giá trị đã cho. Điều này đảm bảo giới hạn dưới phản ánh ràng buộc "ít nhất" mạnh nhất. Đối với các ràng buộc loại 2, hãy cập nhật$R$đến mức tối thiểu của hiện tại$R$và giá trị đã cho. Điều này thực thi ràng buộc "nhiều nhất" chặt chẽ nhất. 
3. Tập hợp tất cả các ràng buộc loại 3 thành một tập hợp. Đây là những giá trị mà$k$không thể lấy. 
4. Nếu khoảng kết quả$[L, R]$không hợp lệ (tức là$L > R$), ngay lập tức trả về 0 vì không có số nguyên nào có thể thỏa mãn giới hạn dưới và giới hạn trên trái ngược nhau. 
5. Ngược lại, hãy bắt đầu bằng cách đếm các số nguyên trong khoảng như sau:$R - L + 1$. Đối với mỗi giá trị trong bộ loại 3, hãy kiểm tra xem nó có nằm trong khoảng không. Nếu có, hãy giảm số lượng. Các giá trị ngoài khoảng không ảnh hưởng đến câu trả lời. 
6. Trả về số cuối cùng là số nguyên thỏa mãn mọi ràng buộc. 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến$L$Và$R$luôn phản ánh các giới hạn chặt chẽ nhất được áp đặt bởi các ràng buộc loại 1 và 2. Ràng buộc loại 3 chỉ loại bỏ các số cụ thể khỏi khoảng. Vì tập hợp các giá trị loại 3 là hữu hạn và khoảng là hữu hạn nên việc đếm độ dài và loại bỏ các phần tử không hợp lệ sẽ đưa ra câu trả lời chính xác mà không thiếu hoặc đếm kép bất kỳ số nguyên nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    L, R = 1, 10**9
    forbidden = set()
    for _ in range(n):
        a, x = map(int, input().split())
        if a == 1:
            L = max(L, x)
        elif a == 2:
            R = min(R, x)
        else:
            forbidden.add(x)
    if L > R:
        print(0)
        continue
    count = R - L + 1
    for x in forbidden:
        if L <= x <= R:
            count -= 1
    print(count)
```Phần đầu tiên đọc số lượng trường hợp thử nghiệm. Biến$L$Và$R$theo dõi khoảng thời gian khả thi, được khởi tạo ở phạm vi rộng nhất có thể. Bộ bị cấm lưu trữ tất cả các ràng buộc loại 3, cho phép kiểm tra tư cách thành viên nhanh chóng. Sau khi xử lý tất cả các ràng buộc, khoảng thời gian có thể trở nên không hợp lệ; trong trường hợp đó, chúng tôi in 0. Ngược lại, chúng tôi tính số số nguyên trong khoảng và trừ đi mọi giá trị bị cấm nằm trong khoảng đó. Điều này tránh việc lặp lại trong toàn bộ khoảng thời gian có thể rất lớn. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên: 

| Ràng buộc | L | R | Bị cấm | 
| --- | --- | --- | --- | 
| 1 3 | 3 | 10^9 | {} | 
| 2 10 | 3 | 10 | {} | 
| 3 1 | 3 | 10 | {1} | 
| 3 5 | 3 | 10 | {1,5} | 

Độ dài khoảng là$10-3+1 = 8$. Các giá trị bị cấm trong khoảng là {5}, do đó số đếm cuối cùng là$8-1=7$. Điều này phù hợp với đầu ra mẫu. 

Đầu vào mẫu thứ hai: 

| Ràng buộc | L | R | Bị cấm | 
| --- | --- | --- | --- | 
| 1 5 | 5 | 10^9 | {} | 
| 2 4 | 5 | 4 | {} | 

Đây$L > R$, vì vậy câu trả lời là 0. Điều này chứng tỏ thuật toán xử lý chính xác các giới hạn mâu thuẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ràng buộc được xử lý một lần. Các giá trị loại 3 được kiểm tra theo khoảng thời gian tuyến tính. | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Lưu trữ tập hợp giá trị loại 3 bị cấm. Các biến khác là không gian không đổi. | 

Được cho$t \le 500$Và$n \le 100$, các phép toán trong trường hợp xấu nhất là 500 * 100 = 50.000, dễ dàng trong vòng 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())
    return output.getvalue().strip()

# provided samples
assert run("6\n4\n1 3\n2 10\n3 1\n3 5\n2\n1 5\n2 4\n10\n3 6\n3 7\n1 2\n1 7\n3 100\n3 44\n2 100\n2 98\n1 3\n3 99\n6\n1 5\n2 10\n1 9\n2 2\n3 2\n3 9\n5\n1 1\n2 2\n3 1\n3 2\n3 3\n6\n1 10000\n2 900000000\n3 500000000\n1 100000000\n3 10000\n3 900000001") == "7\n0\n90\n0\n0\n800000000"

# minimum interval, excluded
assert run("1\n3\n1 5\n2 5\n3 5") == "0", "single value excluded"

# interval of size 1, no exclusions
assert run("1\n2\n1 42\n2 42") == "1", "single value allowed"

# interval of size 2, one exclusion
assert run("1\n3\n1 10\n2 11\n3 10") == "1", "lower bound excluded"

# large interval, no exclusions
assert run("1\n2\n1 1\n2 1000000000") == str(10**9), "max size interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5,2 5,3 5 | 0 | Khoảng thời gian giảm xuống còn một giá trị |
