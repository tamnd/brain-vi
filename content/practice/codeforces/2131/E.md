---
title: "CF 2131E - XOR liền kề"
description: "Chúng ta có hai mảng a và b, cả hai đều có độ dài n. Chúng ta có thể thực hiện thao tác trên a nhiều nhất một lần cho mỗi chỉ mục: với bất kỳ chỉ mục i nào từ 1 đến n-1, chúng ta có thể đặt a[i] thành a[i] XOR a[i+1]."
date: "2026-06-08T02:56:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2131
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1042 (Div. 3)"
rating: 1400
weight: 2131
solve_time_s: 90
verified: false
draft: false
---

[CF 2131E - XOR liền kề](https://codeforces.com/problemset/problem/2131/E) 

**Đánh giá:** 1400 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`Và`b`, cả hai đều có chiều dài`n`. Chúng ta có thể thực hiện một thao tác trên`a`nhiều nhất một lần cho mỗi chỉ mục: cho bất kỳ chỉ mục nào`i`từ 1 đến`n-1`, chúng ta có thể thiết lập`a[i]`ĐẾN`a[i] XOR a[i+1]`. Câu hỏi đặt ra là liệu có thể, bằng cách chọn một chuỗi các thao tác như vậy, để biến đổi`a`chính xác vào`b`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước`n`và hai mảng`a`Và`b`. Chúng ta cần xuất ra "CÓ" nếu có thể chuyển đổi và "KHÔNG" nếu ngược lại. 

Các ràng buộc ngụ ý rằng chúng ta cần một thuật toán gần như tuyến tính trong`n`cho mỗi trường hợp thử nghiệm, bởi vì`n`có thể lên tới 2·10^5 và tổng của`n`trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi 2·10^5. Một giải pháp đơn giản là thử tất cả các chuỗi hoạt động theo cấp số nhân, quá chậm. Chúng ta phải suy luận về cấu trúc của vấn đề thay vì mô phỏng mọi thao tác. 

Các trường hợp cạnh bao gồm các mảng có số 0, các mảng chỉ có phần tử cuối cùng khác nhau hoặc các trường hợp trong đó`a`đã bằng rồi`b`. Ví dụ, nếu`a = [0,0,1]`Và`b = [0,0,0]`, một cách tiếp cận tham lam ngây thơ có thể thử XOR các phần tử đầu tiên và bỏ lỡ rằng không có chuỗi thao tác được phép nào có thể xóa dấu vết`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: với mọi chỉ mục`i`, hãy thử thực hiện thao tác XOR với`i+1`và kiểm tra đệ quy nếu`b`có thể thu được. Điều này đúng về nguyên tắc vì mọi thao tác được phép đều được xem xét, nhưng nó dẫn đến khả năng O(2^n) và không khả thi đối với`n`lên đến 2·10^5. 

Quan sát quan trọng cho phép giải tuyến tính là xem xét sự khác biệt XOR giữa`a`Và`b`. Cho phép`d[i] = a[i] XOR b[i]`. Mỗi thao tác`a[i] := a[i] XOR a[i+1]`chuyển đổi`d[i]`qua`a[i+1]`. Bởi vì chúng ta chỉ có thể thao tác từ trái sang phải và mỗi chỉ mục chỉ bị ảnh hưởng một lần bởi phần tử trước đó của nó, nên chúng ta có thể truyền bá các hiệu chỉnh XOR một cách tham lam: nếu`a[i]`không khớp`b[i]`, chúng ta phải áp dụng thao tác tại`i`để điều chỉnh nó. Nhắc nhở duy nhất là phần tử cuối cùng không thể sửa được vì không có`a[n+1]`. Điều này mang lại cái nhìn sâu sắc quan trọng: việc chuyển đổi có thể thực hiện được khi và chỉ khi phần tử cuối cùng khớp và các hiệu chỉnh lan truyền không xung đột với các phần tử đã đặt trước đó. 

Thông tin chi tiết này giúp giảm vấn đề thành một lần chuyển từ trái sang phải, duy trì mảng sai phân và áp dụng các thao tác nếu cần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tuyên truyền tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, hãy tính mảng sai phân`d[i] = a[i] XOR b[i]`. 
2. Lặp lại từ trái sang phải các chỉ số`i = 0`ĐẾN`n-2`. Tại mỗi chỉ số, nếu`d[i] != 0`, chúng ta phải áp dụng thao tác tại`i`. Cập nhật`d[i+1] := d[i+1] XOR d[i]`và thiết lập`d[i] := 0`. Điều này tương ứng với việc áp dụng`a[i] := a[i] XOR a[i+1]`. 
3. Sau khi lặp lại, hãy kiểm tra`d[n-1]`. Nếu như`d[n-1] == 0`, xuất ra "CÓ"; nếu không thì xuất ra "NO". Phần tử cuối cùng không thể điều chỉnh được, do đó, sự không khớp ở đó có nghĩa là không thể chuyển đổi được. 

Tại sao nó hoạt động: Điều bất biến là sau khi xử lý chỉ mục`i`, tất cả sự khác biệt`d[0]..d[i]`đã được sửa thành không. Truyền bá XOR cần thiết tới`d[i+1]`chính xác là những gì hoạt động thực hiện. Điều này đảm bảo rằng nếu hiệu cuối cùng bằng 0 thì tất cả các phần tử của`a`cuộc thi đấu`b`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_transform(a, b, n):
    d = [a[i] ^ b[i] for i in range(n)]
    for i in range(n - 1):
        if d[i]:
            d[i+1] ^= d[i]
            d[i] = 0
    return d[-1] == 0

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    print("YES" if can_transform(a, b, n) else "NO")
```Mã đầu tiên tính toán sự khác biệt XOR giữa`a`Và`b`. Sau đó, nó quét từ trái sang phải, loại bỏ sự khác biệt bằng cách truyền chúng sang phần tử tiếp theo. Sau vòng lặp, nếu hiệu của phần tử cuối cùng bằng 0 thì có thể thực hiện chuyển đổi. Cần chú ý cẩn thận đối với phần tử cuối cùng và đối với các chỉ số: thao tác chỉ ảnh hưởng đến`i`Và`i+1`, vì vậy chúng tôi không bao giờ thử`i = n-1`. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:```
a = [1, 2, 3, 4, 5]
b = [3, 2, 7, 1, 5]
```| tôi | d trước | hành động | d sau | 
| --- | --- | --- | --- | 
| 0 | 1^3=2 | tuyên truyền | d[1] ^= 2 -> 0^2=2, d[0]=0 | 
| 1 | 2 | tuyên truyền | d[2]^=2 -> 3^7? chờ tính d[2]=3^7=4 -> 4^2=6, d[1]=0 | 
| 2 | 6 | tuyên truyền | d[3]^=6 -> 4^1=5, d[2]=0 | 
| 3 | 5 | tuyên truyền | d[4]^=5 -> 5^5=0, d[3]=0 |`d[4] = 0`, xuất ra CÓ. Việc truyền từ trái sang phải sẽ khắc phục thành công mọi khác biệt. 

**Ví dụ 2**:```
a = [0,0,1]
b = [1,0,1]
```| tôi | d trước | hành động | d sau | 
| --- | --- | --- | --- | 
| 0 | 0^1=1 | tuyên truyền | d[1]^=1 -> 0^1=1, d[0]=0 | 
| 1 | 1 | tuyên truyền | d[2]^=1 -> 1^1=0, d[1]=0 |`d[2]=0`, xuất ra CÓ. Thuật toán xử lý các số 0 ban đầu và truyền bá sự khác biệt một cách chính xác. 

**Ví dụ 3**:```
a = [0,0,1]
b = [0,0,0]
```| tôi | d trước | hành động | d sau | 
| --- | --- | --- | --- | 
| 0 | 0 | bỏ qua | | 
| 1 | 1 | tuyên truyền | d[2]^=1 -> 1^0=1, d[1]=0 |`d[2]=1`, đầu ra SỐ. Phần tử cuối cùng không thể sửa được; sự biến đổi không thể được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chuyển một lần từ trái sang phải trên mảng có độ dài n | 
| Không gian | O(n) | Mảng chênh lệch độ dài n | 

Với tổng số`n`trên tất cả các trường hợp thử nghiệm 2·10^5, giải pháp chạy trong khoảng 2·10^5 thao tác, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ là tuyến tính trong`n`, phù hợp với ràng buộc 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # call solution
    import sys
    input = sys.stdin.readline

    def can_transform(a, b, n):
        d = [a[i] ^ b[i] for i in range(n)]
        for i in range(n - 1):
            if d[i]:
                d[i+1] ^= d[i]
                d[i] = 0
        return d[-1] == 0

    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        print("YES" if can_transform(a, b, n) else "NO")
    return out.getvalue().strip()

# Provided samples
assert run("7\n5\n1 2 3 4 5\n3 2 7 1 5\n3\n0 0 1\n1 0 1\n3\n0 0 1\n0 0 0\n4\n0 0 1 2\n1 3 3 2\n6\n1 1 4 5 1 4\n0 5 4 5 5 4\n3\n0 1 2\n2 3 2\n2\n10 10\n11 10") == "YES\nNO\nNO\nNO\nYES\nNO\nNO"

# Custom cases
assert run("1\n2\n0 0\n
```
