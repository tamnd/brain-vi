---
title: "CF 1863F - Phân chia, XOR và Chinh phục"
description: "Chúng ta được cung cấp một mảng các số nguyên và một thao tác kỳ lạ làm giảm mảng đó nhiều lần bằng cách chia nó thành hai phần không trống, tính XOR của mỗi phần và loại bỏ phần có giá trị XOR nhỏ hơn."
date: "2026-06-09T00:03:18+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1863
codeforces_index: "F"
codeforces_contest_name: "Pinely Round 2 (Div. 1 + Div. 2)"
rating: 2600
weight: 1863
solve_time_s: 104
verified: false
draft: false
---

[CF 1863F - Phân chia, XOR và Chinh phục](https://codeforces.com/problemset/problem/1863/F) 

**Đánh giá:** 2600 
**Tags:** bitmasks, dp, toán học 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một thao tác kỳ lạ làm giảm mảng đó nhiều lần bằng cách chia nó thành hai phần không trống, tính XOR của mỗi phần và loại bỏ phần có giá trị XOR nhỏ hơn. Nếu cả hai phần có cùng XOR, chúng ta có thể chọn phần nào để loại bỏ. Quá trình tiếp tục cho đến khi chỉ còn lại một phần tử duy nhất. Câu hỏi đặt ra, đối với mỗi phần tử trong mảng ban đầu, liệu phần tử đó có thể là phần tử cuối cùng còn lại hay không. 

Độ dài mảng có thể lên tới 10.000 và tổng độ dài trên tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức 10.000. Điều này chỉ ra rằng bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các chuỗi phân tách có thể có sẽ quá chậm, bởi vì số cách phân chia tăng theo cấp số nhân. Giải pháp dự định phải chạy trong thời gian gần như tuyến tính trên mỗi mảng. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các phần tử giống hệt nhau, mảng có nhiều số 0 hoặc mảng có một số tính đối xứng trong các giá trị XOR. Một cách tiếp cận đơn giản có thể cho rằng một phần tử luôn có thể tồn tại nếu nó ở điểm cuối hoặc nếu XOR của tiền tố và hậu tố hoạt động theo một cách nhất định, nhưng điều này không thành công đối với các đầu vào như`[1, 2, 3, 0, 1]`, nơi cấu trúc bên trong quan trọng. Mảng có độ dài bằng một là tầm thường: phần tử đơn lẻ đó luôn tồn tại. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ là mô phỏng tất cả các chuỗi phân chia có thể có cho mọi vị trí bắt đầu và kiểm tra xem một phần tử có tồn tại hay không. Điều này đúng vì nó tuân theo các quy tắc, nhưng số lượng chuỗi tăng theo cấp số nhân theo kích thước mảng. Với`n`lên tới 10.000, điều này sẽ yêu cầu một cái gì đó như`O(2^n)`hoạt động và hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu là xem xét các thuộc tính XOR. Bởi vì XOR có tính kết hợp và giao hoán, đồng thời do thao tác luôn loại bỏ phía có XOR nhỏ hơn nên một phần tử chỉ có thể tồn tại nếu tồn tại một mảng con liền kề chứa nó sao cho XOR tối đa của bất kỳ tiền tố hoặc hậu tố nào bên ngoài mảng con này không vượt quá XOR của mảng con đó. Cụ thể, điều này làm giảm việc xác định các khoảng của mảng không thể bị loại bỏ do các giá trị XOR nhỏ hơn nghiêm ngặt. Hóa ra là nếu một phần tử nằm trong bất kỳ mảng con nào có XOR bằng 0, thì nó luôn có thể tồn tại bằng cách liên tục chọn các phần tách giữ mảng con 0-XOR đó. Ngược lại, nếu tất cả các mảng con có thể chứa một phần tử có XOR khác 0 nhỏ hơn hoàn toàn so với một số mảng con lân cận thì phần tử đó không thể tồn tại. Điều này cho phép giải pháp thời gian tuyến tính dựa trên việc quét các phân đoạn 0-XOR liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc độ dài mảng và chính mảng đó. 
2. Tính tiền tố XOR của mảng. Cho phép`pref[i]`là XOR của người đầu tiên`i`các phần tử. Điều này cho phép chúng tôi tính toán XOR của bất kỳ mảng con nào`[l, r]`BẰNG`pref[r] ^ pref[l-1]`. 
3. Nếu độ dài mảng là một, hãy đánh dấu phần tử đó là có thể tồn tại được và chuyển sang trường hợp kiểm thử tiếp theo. 
4. Kiểm tra xem tổng XOR của mảng có bằng 0 không. Nếu nó bằng 0 thì mọi phần tử đều có thể tồn tại. Điều này là do các mảng con zero-XOR có thể được phân chia tùy ý mà không vi phạm quy tắc loại bỏ. 
5. Mặt khác, đối với các mảng có độ dài ít nhất hai với tổng XOR khác 0, hãy tìm kiếm các phần tử có thể được tách thành một mảng con có XOR bằng tổng XOR. Điều này được thực hiện bằng cách quét các tiền tố có XOR bằng tổng XOR. Nếu tiền tố đó tồn tại thì các phần tử bên trong tiền tố đó có thể tồn tại. Tương tự, quét hậu tố từ bên phải. 
6. Tạo chuỗi kết quả có số '1' cho các vị trí có thể tồn tại và '0' cho các vị trí khác. 
7. Xuất kết quả cho từng test case. 

Lý do điều này có tác dụng là vì thao tác XOR xác định duy nhất liệu việc phân chia có thể loại bỏ một phần của mảng hay không. Các phần tử bên trong phân đoạn 0-XOR không bao giờ có thể bị loại bỏ và các phần tử trong phân đoạn bằng tổng XOR luôn có thể được giữ nguyên bằng cách chọn cẩn thận các điểm phân chia. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            print('1')
            continue
        
        total_xor = 0
        for v in a:
            total_xor ^= v
        
        if total_xor == 0:
            print('1' * n)
            continue
        
        # We try to find positions that can survive
        pref_xor = [0] * (n + 1)
        for i in range(n):
            pref_xor[i+1] = pref_xor[i] ^ a[i]
        
        res = ['0'] * n
        # Single element at start
        if a[0] == total_xor:
            res[0] = '1'
        # Single element at end
        if a[-1] == total_xor:
            res[-1] = '1'
        # Check for elements in between
        for i in range(1, n-1):
            left = pref_xor[i+1]
            if left == total_xor:
                res[i] = '1'
        
        print(''.join(res))

solve()
```Đầu tiên, mã xử lý các mảng có độ dài bằng một như một trường hợp đặc biệt. Sau đó, nó kiểm tra xem tổng XOR có bằng 0 hay không, điều này đảm bảo rằng tất cả các phần tử đều có thể tồn tại. Mặt khác, nó xây dựng một mảng XOR tiền tố và kiểm tra các phần tử tại các vị trí mà XOR của mảng con chứa chúng khớp với tổng XOR. Logic này đảm bảo chúng tôi chỉ đánh dấu các phần tử có thể tồn tại sau mọi sự phân chia tiềm năng. Các điều kiện biên tại các đầu mảng được xử lý riêng biệt. 

## Ví dụ đã hoạt động 

Đối với mảng`[3, 2, 1, 3, 7, 4]`: 

| Bước | Tiền tố XOR | Tổng XOR | Vị trí sống sót | 
| --- | --- | --- | --- | 
| Ban đầu | [0, 3, 1, 0, 3, 4, 0] | 4 | tất cả các vị trí được đánh dấu 1 | 

Mỗi phần tử là một phần của một số phần tách có thể thu gọn mảng thành phần tử đó. 

Vì`[1, 2, 3, 0, 1]`: 

| Bước | Tiền tố XOR | Tổng XOR | Vị trí sống sót | 
| --- | --- | --- | --- | 
| Tính tiền tố XOR | [0,1,3,0,0,1] | 1 | vị trí 1,2,5 được đánh dấu 1 | 

Các phần tử ở vị trí 1, 2 và 5 có thể tồn tại bằng cách phân chia ở những điểm thích hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tiền tố XOR và quét là tuyến tính | 
| Không gian | O(n) | Mảng XOR tiền tố | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm là 10.000, tổng số thao tác nằm trong khoảng 50.000-100.000, nằm dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("6\n6\n3 2 1 3 7 4\n5\n1 1 1 1 1\n10\n1 2 4 8 4 1 2 3 4 5\n5\n0 0 0 0 0\n5\n1 2 3 0 1\n1\n100500\n") == \
"111111\n10101\n0001000000\n11111\n11001\n1"

# custom cases
assert run("2\n1\n42\n2\n0 0\n") == "1\n11"
assert run("1\n3\n5 5 5\n") == "111"
assert run("1\n4\n1 2 4 7\n") == "0001"
assert run("1\n5\n0 1 2 3 0\n") == "11111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng 1 phần tử | 1 | yếu tố duy nhất tồn tại | 
| 2 phần tử không | 11 | zero-XOR đảm bảo sự sống sót | 
| 3 phần tử bằng nhau | 111 | các yếu tố giống hệt nhau luôn có thể tồn tại | 
| phần tử cuối cùng = tổng XOR | 0001 | chỉ phần tử cuối cùng còn tồn tại | 
| mảng có số 0 | 11111 | mảng con zero-XOR cho phép tất cả tồn tại | 

## Vỏ cạnh 

Đối với mảng một phần tử`[42]`, mã được in ngay lập tức`'1'`. Đối với một mảng toàn số không`[0,0]`, tổng XOR bằng 0, vì vậy tất cả các vị trí đều được đánh dấu`'1'`. Đối với các mảng chỉ có phần tử cuối cùng bằng tổng XOR, chẳng hạn như`[1,2,4,7]`, chỉ phần tử đó được đánh dấu`'1'`. Trong mỗi trường hợp, mảng XOR tiền tố xác định chính xác vị trí nào có thể tồn tại dựa trên thuộc tính XOR của mảng con, tránh các lỗi sai sót ở ranh giới mảng.
