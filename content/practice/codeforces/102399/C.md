---
title: "CF 102399C - \u0418\u0432\u0430\u043d\u0443\u0448\u043a\u0430-\u0434\u0443\u0440\u0430\u0447\u043e\u043a \u0438 \u0442\u0435\u043e\u0440\u0438\u044f \u0432\u0435\u0440\u043e\u044f\u0442\u043d\u043e\u0441\u0442\u0435\u0439"
description: "Chúng ta có một lưới hình chữ nhật (n lần m) có các ô được tô bằng hai màu. Hai ô là hàng xóm khi chúng có chung một bên. Việc tô màu hợp lệ nếu mỗi ô có nhiều nhất một ô lân cận có cùng màu."
date: "2026-08-11T15:52:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "C"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 134
verified: true
draft: false
---

[CF 102399C - \u0418\u0432\u0430\u043d\u0443\u0448\u043a\u0430-\u0434\u0443\u0440\u0430\u0447\u043e\u043a \u0438 \u0442\u0435\u043e\u0440\u0438\u044f \u0432\u0435\u0440\u043e\u044f\u0442\u043d\u043e\u0441\u0442\u0435\u0439](https://codeforces.com/problemset/problem/102399/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật (n \times m) có các ô được tô bằng hai màu. Hai ô là hàng xóm khi chúng có chung một bên. Việc tô màu hợp lệ nếu mỗi ô có nhiều nhất một ô lân cận có cùng màu. 

Nhiệm vụ là đếm tất cả các màu hợp lệ của lưới và in kết quả theo modulo (10^9+7). Mỗi kích thước lưới có thể đạt tới (100.000), do đó số lượng ô có thể đạt tới (10^{10}). Bất kỳ phương pháp nào xử lý rõ ràng mọi màu sắc có thể đều là vô vọng và ngay cả một phương pháp có trạng thái phụ thuộc theo cấp số nhân vào kích thước nhỏ hơn cũng không thể xử lý lưới (10^5 \times 10^5). Chúng ta cần giảm vấn đề xuống một lượng công việc không đổi trên mỗi chiều. 

Có hai trường hợp nhỏ bộc lộ sai sót trong cách lập luận. Đối với lưới (1\times1), cả hai màu đều hợp lệ, vì vậy câu trả lời là (2). Một phương pháp giả sử mỗi ô có bốn ô lân cận hoặc áp dụng một cách mù quáng phép truy toán bắt đầu từ (n=2), có thể mắc sai lầm này. Đối với lưới (1\times3), các chuỗi hợp lệ đều là các chuỗi nhị phân không có ba ô liên tiếp bằng nhau, tạo ra (6) màu. Một cách tiếp cận bất cẩn chỉ cấm các ô bằng nhau liền kề sẽ chỉ đếm hai chuỗi xen kẽ và bỏ sót các màu như (001). 

Đối với lưới (2\times2), câu trả lời là (6). Bốn ô không thể có cùng một màu và việc tô màu với ba ô cùng một màu và một ô màu kia cũng không hợp lệ vì một trong các ô có màu đa số có hai ô lân cận bằng nhau. Đây là một phép kiểm tra hữu ích vì nó phân biệt điều kiện thực tế, nhiều nhất là một hàng xóm bằng nhau, với điều kiện mạnh hơn và không chính xác là tất cả hàng xóm phải có màu đối lập. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê từng màu trong số (2^{nm}). Đối với mỗi màu, chúng ta có thể kiểm tra từng ô và đếm các ô lân cận có cùng màu với nó. Điều này đúng vì mọi màu sắc có thể đều được xem xét và kiểm tra chính xác theo điều kiện yêu cầu. Công việc trong trường hợp xấu nhất của nó là (O(nm,2^{nm})). Với (n=m=100.000), có (10^{10}) ô và (2^{10^{10}}) cách tô màu có thể, do đó, ngay cả việc viết ra không gian tìm kiếm cũng không thể thực hiện được. 

Quan sát hữu ích là ngừng suy nghĩ trực tiếp về màu sắc và thay vào đó đánh dấu từng cặp ô liền kề có cùng màu. Bởi vì một ô có thể có nhiều nhất một ô lân cận có cùng màu nên các cạnh được đánh dấu này tạo thành một sự khớp: hai cạnh được đánh dấu không bao giờ có thể chia sẻ một ô. 

Bây giờ hãy xem xét hướng của một cạnh như vậy. Giả sử hai ô liền kề theo chiều ngang có cùng màu. Mỗi điểm cuối đã sử dụng hàng xóm có cùng màu duy nhất được phép của nó, vì vậy các ô ngay phía trên chúng, khi chúng tồn tại, phải có màu đối lập. Đối số tương tự truyền bá cặp màu ngang bằng nhau qua mỗi hàng. Do đó, một cạnh cùng màu nằm ngang buộc toàn bộ màu chỉ có các cạnh cùng màu nằm ngang. Một cạnh thẳng đứng cùng màu không thể xuất hiện ở bất kỳ đâu trong màu sắc như vậy, bởi vì cạnh thẳng đứng sẽ truyền theo hướng vuông góc và cuối cùng xung đột với cấu trúc ngang cưỡng bức. Đối số tương tự hoạt động với trao đổi ngang và dọc. Quan sát cấu trúc này là chìa khóa cho toàn bộ giải pháp. 

Do đó, mọi màu hợp lệ đều thuộc về ít nhất một trong hai họ. Trong họ đầu tiên không có các cạnh dọc có màu bằng nhau nên mỗi cột xen kẽ nhau theo chiều dọc. Trong họ thứ hai không có cạnh ngang cùng màu nên mỗi hàng xen kẽ theo chiều ngang. Màu duy nhất thuộc cả hai họ là hai màu bàn cờ.

Hãy xem xét gia đình đầu tiên. Vì mỗi cột xen kẽ theo chiều dọc nên toàn bộ lưới được xác định bởi hàng đầu tiên của nó. Hàng đầu tiên có thể là bất kỳ chuỗi nhị phân nào trong đó ba ô liên tiếp không bao giờ bằng nhau. Khi hàng đầu tiên được chọn, tất cả các hàng còn lại sẽ bị ép luân phiên theo chiều dọc. Nếu màu của ô đầu tiên cố định, hãy gọi (f_k) là số chuỗi có độ dài hợp lệ (k). Hai giá trị đầu tiên là (f_1=1) và (f_2=2). Đối với (k\ge3), sau khi sửa ô đầu tiên, khối cuối cùng có thể mở rộng chuỗi trước đó thêm một ô hoặc hai ô bằng nhau, tạo ra 

[ 
f_k=f_{k-1}+f_{k-2}. 
] 

Bản thân ô đầu tiên có hai màu có thể, vì vậy họ này chứa các lưới (2f_m). 

Theo tính đối xứng, họ không có cạnh ngang cùng màu chứa lưới (2f_n). Giao điểm của chúng bao gồm chính xác hai bàn cờ. Bao gồm-loại trừ mang lại 

2(f_n+f_m-1). 
] 

Đây là đặc tính tiêu chuẩn được sử dụng cho vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm,2^{nm})) | (O(nm)) | Quá chậm | 
| Fibonacci tái diễn | (O(\max(n,m))) | (O(\max(n,m))) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và (m) và đặt (L=\max(n,m)). Chúng ta chỉ cần các giá trị giống Fibonacci ở kích thước lớn hơn. 
2. Xác định (f_1=1) và (f_2=2). Với mọi (i) từ (3) đến (L), hãy tính 
[ 
f_i=f_{i-1}+f_{i-2}\pmod{10^9+7}. 
] 
Điều này đếm các chuỗi nhị phân có độ dài (i) với màu đầu tiên cố định và không có ba màu bằng nhau liên tiếp. 
3. Các màu không có cạnh thẳng đứng cùng màu được tính bằng (2f_m). Hệ số (2) chọn màu của ô đầu tiên, sau đó sự xen kẽ theo chiều dọc sẽ xác định phần còn lại của mỗi cột. 
4. Bằng cách xoay đối số theo (90^\circ), các màu không có cạnh ngang màu nào được tính bằng (2f_n). 
5. Hai họ chồng lên nhau theo đúng hai màu bàn cờ. Trừ hai màu trùng lặp này một lần, thu được 
[ 
2f_n+2f_m-2. 
] 
6. In 
[ 
2(f_n+f_m-1)\pmod{10^9+7}. 
] 

Bất biến đằng sau phép đếm là mọi màu hợp lệ với cặp liền kề có màu bằng nhau chỉ có một hướng có thể có cho các cặp đó. Khi hướng đó được chọn, hướng vuông góc phải xen kẽ và phần tự do còn lại chính xác là chuỗi nhị phân một chiều không có ba màu liên tiếp bằng nhau. Hai hướng có thể có là rời rạc ngoại trừ hai bàn cờ, do đó, công thức loại trừ bao gồm tính mỗi màu hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())
    limit = max(n, m)

    if limit == 1:
        f1 = 1
    else:
        f1, f2 = 1, 2

        for _ in range(3, limit + 1):
            f1, f2 = f2, (f1 + f2) % MOD

    if n == 1:
        fn = 1
    elif n == 2:
        fn = 2
    else:
        a, b = 1, 2
        for _ in range(3, n + 1):
            a, b = b, (a + b) % MOD
        fn = b

    if m == 1:
        fm = 1
    elif m == 2:
        fm = 2
    else:
        a, b = 1, 2
        for _ in range(3, m + 1):
            a, b = b, (a + b) % MOD
        fm = b

    print((2 * (fn + fm - 1)) % MOD)

if __name__ == "__main__":
    solve()
```Phép lặp chỉ có thể được tính một lần tối đa (\max(n,m)), cách này đơn giản hơn và tránh thực hiện hai vòng lặp riêng biệt. Việc triển khai ở trên giữ cho các trường hợp ranh giới rõ ràng nhưng cũng có thể triển khai dựa trên mảng nhỏ gọn. 

Các giá trị (f_1=1) và (f_2=2) không phải là giá trị khởi tạo Fibonacci (1,1) thông thường. Ở đây chúng có ý nghĩa tổ hợp trực tiếp. Với màu đầu tiên cố định, có một chuỗi có độ dài bằng một, trong khi có hai chuỗi có độ dài bằng hai: ô thứ hai có thể có một trong hai màu. 

Số nguyên Python không tràn, nhưng dù sao thì mọi giá trị lặp lại đều giảm modulo (10^9+7). Phép trừ của (1) xảy ra trước phép nhân cuối cùng và việc thêm mô-đun là không cần thiết trong Python vì phép nhân cuối cùng`% MOD`xử lý chính xác giá trị trung gian âm. 

Việc triển khai hiệu quả hơn có thể lưu trữ tất cả các giá trị trong một mảng và đọc trực tiếp (f_n) và (f_m). Phiên bản cuộn chỉ sử dụng bộ nhớ phụ không đổi và đủ dùng khi hai chiều được xử lý riêng biệt. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp (n=2,m=3), phép truy hồi cho ra (f_1=1), (f_2=2) và (f_3=3). 

| Bước | Kích thước | (f_{i-2}) | (f_{i-1}) | (f_i) | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | | 1 | | 
| Ban đầu | 2 | 1 | 2 | | 
| Tái Phát | 3 | 1 | 2 | 3 | 

Hai họ này chứa các màu (2f_2=4) và (2f_3=6). Cả hai đều chứa hai bàn cờ, do đó hợp có (4+6-2=8) màu. 

Đối với ví dụ thứ hai, lấy (n=1,m=3). Các giá trị lặp lại lại là (f_1=1,f_2=2,f_3=3). 

| Bước | Kích thước | (f_{i-2}) | (f_{i-1}) | (f_i) | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | | 1 | | 
| Ban đầu | 2 | 1 | 2 | | 
| Tái Phát | 3 | 1 | 2 | 3 | 

Công thức cho 

[ 
2(f_1+f_3-1)=2(1+3-1)=6. 
] 

Sáu chuỗi đó chính xác là các chuỗi nhị phân có độ dài ba, không bao gồm (000) và (111). Điều này xác nhận rằng đối số hai chiều cũng xử lý chính xác lưới một hàng suy biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\max(n,m))) | Chúng tôi tính toán sự tái phát chỉ lên đến kích thước lớn hơn. | 
| Không gian | (O(1)) | Chỉ cần hai giá trị lặp lại cuối cùng. | 

Kích thước lớn nhất chỉ là (100.000), do đó thuật toán thực hiện khoảng (100.000) bước lặp lại đơn giản. Điều này dễ dàng tương thích với các ràng buộc đã nêu, trong khi mọi sự phụ thuộc theo cấp số nhân vào số lượng ô là hoàn toàn không khả thi. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def solve_data(data: str) -> str:
    n, m = map(int, data.split())

    limit = max(n, m)
    f = [0] * (limit + 1)

    f[1] = 1
    if limit >= 2:
        f[2] = 2

    for i in range(3, limit + 1):
        f[i] = (f[i - 1] + f[i - 2]) % MOD

    answer = 2 * (f[n] + f[m] - 1) % MOD
    return str(answer)

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Provided sample
assert run("2 3\n") == "8", "sample 1"

# Minimum-size grid
assert run("1 1\n") == "2", "single cell has two colors"

# One-dimensional boundary case
assert run("1 3\n") == "6", "binary strings of length 3 without 000 or 111"

# Equal dimensions, catches symmetric counting mistakes
assert run("2 2\n") == "6", "2x2 grid"

# Large boundary case
def expected(n: int, m: int) -> str:
    limit = max(n, m)
    a, b = 1, 2

    if limit == 1:
        fn = fm = 1
    else:
        values = [0, 1, 2]
        for i in range(3, limit + 1):
            values.append((values[-1] + values[-2]) % MOD)
        fn = values[n]
        fm = values[m]

    return str(2 * (fn + fm - 1) % MOD)

assert run("100000 100000\n") == expected(100000, 100000), \
    "maximum equal dimensions"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`2`| Khởi tạo lưới tối thiểu và một chiều | 
|`1 3`|`6`| Ranh giới một chiều và phép truy hồi không ba bằng nhau | 
|`2 2`|`6`| Sự đối xứng và chồng chéo của hai họ định hướng | 
|`100000 100000`| Modulo được tính toán (10^9+7) | Kích thước tối đa và tái phát mô-đun | 

## Vỏ cạnh 

cho`1 1`, phép truy hồi chỉ có (f_1=1). Công thức cho 

[ 
2(1+1-1)=2. 
] 

Chỉ có một ô nên một trong hai màu sẽ tạo ra màu hợp lệ. Sự vắng mặt của hàng xóm được xử lý một cách tự nhiên theo công thức. 

Vì`1 3`, lưới chỉ là một chuỗi nhị phân. Một chuỗi hợp lệ không thể chứa ba ô liên tiếp bằng nhau vì ô ở giữa sẽ có hai ô lân cận bằng nhau. Sáu chuỗi hợp lệ chính xác là các chuỗi được tính bằng (2f_3=6). Công thức cho kết quả tương tự: 

[ 
2(f_1+f_3-1)=2(1+3-1)=6. 
]

Vì`2 2`, (f_2=2), do đó 

[ 
2(f_2+f_2-1)=2(2+2-1)=6. 
] 

Sáu màu hợp lệ bao gồm hai bàn cờ và bốn màu có một cặp ô liền kề bằng nhau. Trường hợp này xác nhận rằng hai họ định hướng phải trùng nhau bởi chính xác hai màu thay vì chỉ được thêm vào. 

Vì`2 3`, phép truy hồi cho (f_2=2) và (f_3=3). Họ cạnh ngang đóng góp (2f_2=4), họ cạnh dọc đóng góp (2f_3=6) và hai bàn cờ xuất hiện ở cả hai họ. Số cuối cùng là (4+6-2=8), khớp với mẫu. 

Vì`100000 100000`, không cần trường hợp toán học đặc biệt nào. Phép truy toán được đánh giá theo modulo (10^9+7) ở mỗi bước, do đó các giá trị vẫn nhỏ và câu trả lời thu được từ hai giá trị giống nhau (f_{100000}). Thời gian chạy vẫn tuyến tính theo (100.000), không tính theo (10^{10}) ô của lưới.
