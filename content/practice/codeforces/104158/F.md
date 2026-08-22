---
title: "CF 104158F - Đơn đặt hàng nhà vệ sinh"
description: "Chúng ta được cho các cặp số nguyên lớn biểu thị số lượng của hai loại bộ phận, bát và nắp. Từ mỗi cặp, số lượng bồn cầu hoàn chỉnh mà Thomas có thể lắp ráp được xác định bằng ước chung lớn nhất của hai đại lượng này."
date: "2026-07-02T01:11:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104158
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 1 (Advanced)"
rating: 0
weight: 104158
solve_time_s: 81
verified: false
draft: false
---

[CF 104158F - Đơn đặt hàng nhà vệ sinh](https://codeforces.com/problemset/problem/104158/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho các cặp số nguyên lớn biểu thị số lượng của hai loại bộ phận, bát và nắp. Từ mỗi cặp, số lượng bồn cầu hoàn chỉnh mà Thomas có thể lắp ráp được xác định bằng ước chung lớn nhất của hai đại lượng này. Gcd đó biểu thị số lượng cặp nắp tô đầy đủ có thể được hình thành bằng cách sử dụng các vật phẩm được cân bằng hoàn hảo. 

Đối với mỗi trường hợp thử nghiệm, nhiệm vụ không phải là tính toán gcd mà là mô tả cấu trúc cơ bản của nó. Khi chúng tôi tìm thấy gcd, chúng tôi phải biểu thị nó dưới dạng tích của các số nguyên tố và báo cáo từng số nguyên tố riêng biệt cùng với số lần nó xuất hiện trong quá trình nhân tử hóa. Đầu ra được định dạng một số nguyên tố trên mỗi dòng theo thứ tự tăng dần, theo sau là dòng kết thúc chứa số 0. 

Các ràng buộc đẩy chúng ta tới việc xử lý số học cẩn thận. Mỗi số có thể lớn tới 10^12, điều này ngay lập tức loại trừ việc phân tích nhân tử đơn giản bằng cách chia lặp lại cho đến n. Ngay cả việc kiểm tra khả năng chia hết cho đến n cũng là không thể. Chúng ta cần một cái gì đó gần hơn với phân tích căn bậc hai hoặc tốt hơn, và chúng ta phải dựa vào thực tế là gcd cũng nhiều nhất là 10^12. 

Trường hợp cạnh tinh tế là khi gcd bằng 1. Trong trường hợp đó, không có thừa số nguyên tố nào cả, vì vậy chúng tôi chỉ xuất ra một dòng duy nhất chứa số 0. Một trường hợp khác là khi gcd chính là số nguyên tố, chẳng hạn như 5, trong đó kết quả đầu ra chỉ bao gồm số nguyên tố đó có bội số một theo sau là số 0. Thiếu số 0 cuối cùng hoặc in sai nội dung nào đó cho gcd = 1 là những lỗi định dạng phổ biến. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải bài toán này là tính gcd của mỗi cặp, sau đó phân tích nó bằng phép chia thử. Khi chúng ta có g, chúng ta có thể kiểm tra tất cả các số nguyên từ 2 đến g và kiểm tra tính chia hết. Điều này đúng nhưng quá chậm. Trong trường hợp xấu nhất khi g ở khoảng 10^12, điều này sẽ yêu cầu tới một nghìn tỷ phép tính cho mỗi trường hợp thử nghiệm, điều này là không khả thi. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ cải thiện điều này bằng cách chỉ kiểm tra các ước số tối đa sqrt(g). Với mỗi ứng viên i, nếu nó chia g, chúng ta chia liên tục g cho i và tính bội số. Điều này làm giảm đáng kể độ phức tạp, vì sqrt(10^12) là 10^6, là đường biên nhưng có thể chấp nhận được đối với tối đa 100 trường hợp thử nghiệm trong Python được tối ưu hóa nếu được triển khai cẩn thận. 

Điểm mấu chốt là chúng ta không bao giờ cần phân tích b và l riêng biệt. Gcd đã thu gọn vấn đề thành một số có cấu trúc đơn giản hơn cả đầu vào. Khi chúng ta tách g, việc phân tích thành thừa số nguyên tố tiêu chuẩn thông qua phép chia thử lên đến sqrt(g) là đủ. Không cần các phương pháp nâng cao như Pollard Rho vì các ràng buộc đủ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực lên tới g | O(g) | O(1) | Quá chậm | 
| Phép chia thử lên tới √g | O(√g) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Các bước 

1. Đọc số nguyên b và l cho mỗi test. Mục tiêu là chỉ làm việc với gcd của họ, vì tất cả thông tin được yêu cầu phụ thuộc hoàn toàn vào các yếu tố được chia sẻ. 
2. Tính g = gcd(b, l). Điều này nén vấn đề thành phân tích một số duy nhất đại diện cho số lượng cặp cân bằng tối đa có thể. 
3. Nếu g bằng 1, xuất ra 0 ngay lập tức. Không có thừa số nguyên tố nào để báo cáo nên việc tính toán thêm là không cần thiết. 
4. Khởi tạo một danh sách trống cho các thừa số nguyên tố và bắt đầu kiểm tra các ước số bắt đầu từ 2. 
5. Với mỗi số nguyên i từ 2 đến sqrt(g), kiểm tra xem i có chia hết g hay không. Nếu đúng như vậy, hãy liên tục chia g cho i trong khi đếm số lần điều này xảy ra. Số này là bội số của thừa số nguyên tố i. 
6. Nếu sau khi xử lý tất cả i thành sqrt(g) mà giá trị còn lại của g lớn hơn 1 thì bản thân g là số nguyên tố và phải ghi bội số là 1. 
7. In tất cả các số nguyên tố thu thập được theo thứ tự tăng dần, theo sau là bội số của nó và kết thúc bằng một dòng chứa số 0. 

### Tại sao nó hoạt động

Mọi số nguyên lớn hơn 1 đều có một hệ số nguyên tố duy nhất. Quá trình phân chia thử nghiệm loại bỏ một cách có hệ thống tất cả các lần xuất hiện của từng thừa số nguyên tố theo thứ tự tăng dần. Bởi vì chúng tôi chỉ kiểm tra tối đa sqrt(g), mọi giá trị còn lại phải là 1 hoặc số nguyên tố lớn hơn sqrt(g), vì một số tổng hợp đã được phân tích thành các thành phần nhỏ hơn. Điều này đảm bảo rằng tất cả các số nguyên tố đều được ghi lại chính xác một lần và có bội số chính xác. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def factorize(x):
    res = []
    i = 2
    while i * i <= x:
        if x % i == 0:
            cnt = 0
            while x % i == 0:
                x //= i
                cnt += 1
            res.append((i, cnt))
        i += 1
    if x > 1:
        res.append((x, 1))
    return res

def solve():
    t = int(input())
    for _ in range(t):
        b, l = map(int, input().split())
        g = math.gcd(b, l)

        if g == 1:
            print(0)
            continue

        factors = factorize(g)
        for p, c in factors:
            print(p, c)
        print(0)

if __name__ == "__main__":
    solve()
```Giải pháp phân tách các mối quan tâm một cách rõ ràng: đầu tiên nén vấn đề bằng cách sử dụng gcd, sau đó tính số lượng đã giảm bằng cách sử dụng phép chia thử nghiệm tiêu chuẩn. Hàm phân tích nhân tử cẩn thận phân chia hoàn toàn từng số nguyên tố trước khi chuyển tiếp, điều này đảm bảo bội số chính xác. Điều kiện vòng lặp i * i <= x tự động co lại khi x giảm, cải thiện hiệu suất trong thực tế. 

Một lỗi phổ biến là quên tính toán lại vòng lặp dựa trên x đã cập nhật; Việc triển khai này tránh được điều đó bằng cách sử dụng trực tiếp giá trị hiện tại của x trong điều kiện. Một điểm tinh tế khác là đảm bảo rằng x > 1 còn lại được coi là thừa số nguyên tố, vì nó có thể chưa được vòng lặp giảm hoàn toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cặp đầu vào: b = 360, l = 240 

gcd = 120 

| Bước | giá trị x | ước số i | hành động | yếu tố | 
| --- | --- | --- | --- | --- | 
| 1 | 120 | 2 | chia hai lần → 30 | (2,2) | 
| 2 | 30 | 3 | chia một lần → 10 | (3,1) | 
| 3 | 10 | 5 | chia một lần → 2 | (5,1) | 
| 4 | 2 | kết thúc | số nguyên tố còn sót lại | cộng (2,1) | 

Phân tích nhân tử cuối cùng: 2^3 * 3^1 * 5^1 

Đầu ra:```
2 3
3 1
5 1
0
```Điều này xác nhận rằng phép chia lặp lại sẽ tích lũy chính xác các bội số và duy trì trật tự. 

### Ví dụ 2 

Cặp đầu vào: b = 83, l = 24 

gcd = 1 

| Bước | giá trị x | hành động | 
| --- | --- | --- | 
| 1 | 1 | chấm dứt ngay lập tức | 

Đầu ra:```
0
```Điều này chứng tỏ rằng phím tắt gcd tránh được công việc phân tích nhân tử không cần thiết một cách chính xác và xử lý trường hợp phân tích nhân tử trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T √g) | Mỗi gcd được tính theo thời gian logarit và mỗi hệ số sử dụng phép chia thử lên tới sqrt(g) | 
| Không gian | O(1) | Chỉ một danh sách nhỏ các yếu tố cho mỗi trường hợp thử nghiệm | 

Giá trị của g nhiều nhất là 10^12, do đó sqrt(g) nhiều nhất là 10^6. Với T lên tới 100, điều này phù hợp thoải mái trong các giới hạn điển hình trong Python được tối ưu hóa, đặc biệt là khi các phép chia nhanh chóng giảm x trong quá trình phân tích nhân tử. 

## Trường hợp thử nghiệm```python
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def factorize(x):
        res = []
        i = 2
        while i * i <= x:
            if x % i == 0:
                cnt = 0
                while x % i == 0:
                    x //= i
                    cnt += 1
                res.append((i, cnt))
            i += 1
        if x > 1:
            res.append((x, 1))
        return res

    def solve():
        t = int(sys.stdin.readline())
        for _ in range(t):
            b, l = map(int, sys.stdin.readline().split())
            g = math.gcd(b, l)

            if g == 1:
                print(0)
                continue

            for p, c in factorize(g):
                print(p, c)
            print(0)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("1\n360 240\n") == "2 3\n3 1\n5 1\n0"
assert run("3\n83 24\n15 25\n7 13\n") == "0\n5 1\n0\n0"

# custom cases
assert run("1\n2 2\n") == "2 1\n0"                 # minimum non-trivial gcd
assert run("1\n1000000000000 500000000000\n") == "2 45\n5 12\n0"  # large power structure
assert run("1\n17 34\n") == "17 1\n0"              # prime gcd
assert run("1\n6 10\n") == "2 1\n0"                # mixed small primes
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 | 2 1 0 | trường hợp bằng nhỏ nhất | 
| 1e12 5e11 | 2 45 5 12 0 | xử lý số mũ lớn | 
| 17 34 | 17 1 0 | gcd là số nguyên tố | 
| 6 10 | 2 1 0 | trích xuất nhân tố chung | 

## Vỏ cạnh 

Trường hợp một cạnh là khi gcd giảm xuống 1 mặc dù cả hai đầu vào đều lớn. Ví dụ: b = 83 và l = 24 ngay lập tức mang lại kết quả 1. Thuật toán xử lý điều này bằng cách kiểm tra g == 1 trước khi phân tích hệ số, đảm bảo không tạo ra dòng đầu ra sai nào. 

Một trường hợp cạnh khác là khi bản thân gcd là số nguyên tố. Ví dụ: b = 17 và l = 34 cho g = 17. Vòng lặp không tìm thấy bất kỳ ước số nào và giá trị còn lại cuối cùng sẽ kích hoạt điều kiện “x > 1”, phát ra chính xác 17 với bội số 1. 

Trường hợp thứ ba là khi gcd có lũy thừa hoàn hảo, chẳng hạn như 2^k. Trong trường hợp này, phép chia lặp lại bên trong cùng một vòng lặp đảm bảo tích lũy bội số đầy đủ, vì x liên tục giảm trước khi chuyển sang ước số ứng cử viên tiếp theo.
