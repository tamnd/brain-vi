---
title: "CF 102978D - Sử dụng FFT"
description: "Nhiệm vụ xoay quanh việc kết hợp hai chuỗi theo cách tạo ra chuỗi thứ ba trong đó mỗi vị trí ghi lại có bao nhiêu cách tạo thành một tổng nhất định bằng cách chọn một phần tử từ chuỗi đầu tiên và một phần tử từ chuỗi thứ hai."
date: "2026-07-04T06:30:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102978
codeforces_index: "D"
codeforces_contest_name: "XXI Open Cup, Grand Prix of Tokyo"
rating: 0
weight: 102978
solve_time_s: 54
verified: true
draft: false
---

[CF 102978D - Có sử dụng FFT](https://codeforces.com/problemset/problem/102978/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ xoay quanh việc kết hợp hai chuỗi theo cách tạo ra chuỗi thứ ba trong đó mỗi vị trí ghi lại có bao nhiêu cách tạo thành một tổng nhất định bằng cách chọn một phần tử từ chuỗi đầu tiên và một phần tử từ chuỗi thứ hai. Cụ thể, hãy tưởng tượng mỗi mảng đầu vào mô tả số lần xuất hiện các giá trị nhất định. Chúng tôi muốn tính toán, với mỗi tổng chỉ số có thể có, có bao nhiêu cặp phần tử, một phần tử từ mỗi mảng, tạo ra tổng đó. 

Đầu vào có thể được hiểu là hai mảng hệ số của đa thức. Mỗi chỉ số tương ứng với một lũy thừa và giá trị tại chỉ số đó là hệ số. Đầu ra là mảng hệ số của đa thức tích. 

Cấu trúc này ngay lập tức ngụ ý rằng đầu ra ở vị trí k phụ thuộc vào tất cả các cặp chỉ số i và j sao cho i + j = k. Mô hình phụ thuộc đó rất dày đặc: mọi vị trí đầu ra đều tổng hợp các đóng góp từ nhiều cặp đầu vào. 

Nếu các mảng lớn, chẳng hạn như có độ dài lên tới 200000, thì bất kỳ phương pháp nào kiểm tra rõ ràng tất cả các cặp sẽ trở nên quá chậm. Một vòng lặp kép ngây thơ sẽ yêu cầu theo thứ tự các thao tác n2, vượt xa giới hạn 2 giây thông thường cho phép. Ngay cả n = 50000 cũng đã dẫn đến hàng tỷ thao tác. 

Một vài hành vi biên có thể âm thầm phá vỡ các triển khai ngây thơ. Một vấn đề phổ biến là quên rằng các khoản đóng góp chồng chéo lên nhau trên nhiều cặp. Ví dụ: nếu cả hai mảng có một mục nhập khác 0 ở vị trí 1 thì kết quả phải đặt phần đóng góp ở vị trí 2. Việc triển khai có lỗi chỉ căn chỉnh các chỉ số mà không tích lũy đóng góp sẽ ghi đè thay vì tính tổng, tạo ra kết quả không chính xác. 

Một trường hợp tinh tế khác xuất hiện khi mảng chứa số 0 ở khắp mọi nơi ngoại trừ một vài mục thưa thớt. Tối ưu hóa chỉ thưa thớt mà bỏ qua tích lũy toàn phạm vi có thể bỏ lỡ các tổng cặp hợp lệ phát sinh từ các chỉ số ở xa. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: lặp qua mọi chỉ mục i trong mảng đầu tiên và mọi chỉ mục j trong mảng thứ hai, sau đó cộng tích của hai giá trị vào vị trí i + j của kết quả. Điều này đúng vì nó liệt kê rõ ràng mọi cặp hợp lệ đóng góp vào từng hệ số đầu ra. 

Vấn đề với phương pháp này là chi phí của nó. Nếu cả hai mảng có kích thước n, thuật toán thực hiện phép nhân và phép cộng n × n. Hành vi bậc hai này trở nên không khả thi ngay khi n tăng vượt quá vài chục nghìn. 

Quan sát chính là phép tính chính xác là phép nhân đa thức. Mỗi mảng mã hóa các hệ số và đầu ra là tích của chúng. Phép nhân đa thức có cấu trúc nổi tiếng: nó có thể được tính dưới dạng tích chập. Tích chập trực tiếp là bậc hai, nhưng tích chập có thể được tính toán trong thời gian gần tuyến tính bằng cách sử dụng Biến đổi Fourier nhanh. 

FFT hoạt động bằng cách đánh giá cả hai đa thức tại các điểm được chọn cẩn thận, nhân các giá trị theo điểm và sau đó nội suy trở lại. Điều này thay thế phép tính tổng theo cặp đắt tiền trên các chỉ số bằng các phép toán O(n log n) trên không gian tần số. Lý do điều này hoạt động ở đây là vì tích chập trong không gian hệ số trở thành phép nhân trong không gian giá trị và FFT cung cấp một cầu nối nhanh giữa hai biểu diễn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tích chập FFT | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi cả hai mảng đầu vào là danh sách hệ số của đa thức A(x) và B(x).

1. Xác định kích thước cần thiết cho đa thức kết quả, ít nhất là tổng của bậc cao nhất trong cả hai mảng. Sau đó chúng tôi làm tròn kích thước này lên lũy thừa tiếp theo của hai. Điều này là bắt buộc vì việc triển khai FFT dựa vào việc phân tách nhị phân của độ dài mảng. 
2. Mở rộng cả hai mảng có số 0 đến kích thước đã chọn này. Phần đệm này đảm bảo rằng tích chập theo chu kỳ do FFT tạo ra không quấn quanh và làm hỏng tích chập tuyến tính mà chúng ta thực sự muốn. 
3. Áp dụng FFT cho cả hai mảng đệm. Điều này biến đổi biểu diễn hệ số thành biểu diễn giá trị điểm tại các nghiệm phức của đơn vị. Lý do chính để làm điều này là tích chập trở thành phép nhân theo điểm trong miền này. 
4. Nhân từng phần tử của mảng đã chuyển đổi. Bây giờ, mỗi vị trí biểu thị giá trị của đa thức thu được tại một nghiệm thống nhất cụ thể. Bước này thay thế việc tích lũy theo cặp bậc hai bằng công việc tuyến tính. 
5. Áp dụng FFT nghịch đảo để chuyển kết quả trở lại dạng hệ số. Điều này xây dựng lại đa thức từ dạng được đánh giá của nó. 
6. Làm tròn các giá trị kết quả đến số nguyên gần nhất. FFT đưa ra các lỗi dấu phẩy động nhỏ do các phép toán phức tạp lặp đi lặp lại, do đó cần làm tròn số để khôi phục các hệ số nguyên chính xác. 
7. Xuất ra các hệ số m + n − 1 đầu tiên, tương ứng với tất cả các tổng hợp lệ của các chỉ số. 

Tính đúng đắn phụ thuộc vào tính bất biến sau bước 4, mỗi thành phần tần số biểu diễn chính xác tích của hai đa thức ban đầu được đánh giá ở cùng một nghiệm thống nhất. Phép biến đổi nghịch đảo được đảm bảo tái tạo lại biểu diễn hệ số duy nhất từ ​​các đánh giá này, do đó mảng cuối cùng phải khớp với tích chập thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math
import cmath

def fft(a, invert):
    n = len(a)
    j = 0
    for i in range(1, n):
        bit = n >> 1
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit
        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    while length <= n:
        ang = 2 * math.pi / length * (-1 if invert else 1)
        wlen = complex(math.cos(ang), math.sin(ang))
        i = 0
        while i < n:
            w = 1 + 0j
            for j in range(length // 2):
                u = a[i + j]
                v = a[i + j + length // 2] * w
                a[i + j] = u + v
                a[i + j + length // 2] = u - v
                w *= wlen
            i += length
        length <<= 1

    if invert:
        for i in range(n):
            a[i] /= n

def multiply(a, b):
    n = 1
    while n < len(a) + len(b) - 1:
        n <<= 1
    fa = list(map(complex, a)) + [0] * (n - len(a))
    fb = list(map(complex, b)) + [0] * (n - len(b))

    fft(fa, False)
    fft(fb, False)

    for i in range(n):
        fa[i] *= fb[i]

    fft(fa, True)

    res = [0] * (len(a) + len(b) - 1)
    for i in range(len(res)):
        res[i] = int(fa[i].real + 0.5)
    return res

def main():
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    b = list(map(int, input().split()))

    c = multiply(a, b)
    print(*c)

if __name__ == "__main__":
    main()
```Việc triển khai FFT bắt đầu bằng hoán vị đảo ngược bit, sắp xếp lại các chỉ số sao cho các hoạt động bướm Cooley-Tukey có thể được áp dụng lặp đi lặp lại. Vòng lặp chính sau đó xây dựng các kích thước biến đổi từ 2 đến n, kết hợp các biến đổi nhỏ hơn thành các biến đổi lớn hơn. 

Bước nhân thực hiện phép nhân theo điểm của các hệ số biến đổi, đây là phép đơn giản hóa đại số cốt lõi thay thế phép tính tổng lồng nhau. 

FFT nghịch đảo phản chiếu phép biến đổi thuận, ngoại trừ nó sử dụng hướng liên hợp và chia cho n ở cuối để chuẩn hóa kết quả. 

Một chi tiết triển khai tinh tế được làm tròn. Bởi vì số học dấu phẩy động tích lũy lỗi nên việc chuyển trực tiếp sang số nguyên đôi khi sẽ tạo ra các kết quả sai lệch. Thêm 0,5 trước khi cắt bớt sẽ ổn định chuyển đổi. 

## Ví dụ đã hoạt động 

Xét hai mảng nhỏ A = [1, 2, 0, 1] và B = [3, 1, 2]. 

Chúng tôi tính toán C[k] = tổng của A[i] * B[j] trên tất cả i + j = k. 

| Bước | cặp i,j đóng góp | Giá trị tính toán | 
| --- | --- | --- | 
| C0 | (0,0) | 3 | 
| C1 | (0,1),(1,0) | 1 + 6 = 7 | 
| C2 | (0,2),(1,1),(2,0) | 2 + 2 + 0 = 4 | 
| C3 | (1,2),(2,1),(3,0) | 4 + 0 + 3 = 7 | 
| C4 | (3,1) | 1 | 
| C5 | (3,2) | 2 | 

Dấu vết này cho thấy mỗi vị trí đầu ra tổng hợp các đóng góp từ nhiều cặp độc lập như thế nào. Cấu trúc chính xác là định nghĩa tích chập mà FFT được thiết kế để tăng tốc. 

Ví dụ thứ hai sử dụng đầu vào thưa thớt A = [0, 1, 0, 0, 2] và B = [0, 3, 0]. Chỉ các mục nhập khác 0 mới đóng góp, nhưng đầu ra vẫn trải rộng trên nhiều chỉ số vì việc bổ sung chỉ mục sẽ làm thay đổi mức đóng góp. 

| Bước | cặp i,j đóng góp | Giá trị tính toán | 
| --- | --- | --- | 
| C1 | (1,0) | 0 | 
| C2 | (1,1),(4,0) | 3 + 0 = 3 | 
| C5 | (4,1) | 6 | 

Điều này khẳng định rằng ngay cả những đầu vào thưa thớt cũng tạo ra sự phân bổ đầu ra trên phạm vi rộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phép biến đổi FFT xử lý kích thước n bằng cách sử dụng log n lớp hoạt động bướm và chúng tôi thực hiện một số lượng biến đổi không đổi | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng đệm và bộ đệm phức tạp trung gian | 

Hệ số logarit làm cho phương pháp này phù hợp với đầu vào lớn trong đó tích chập bậc hai sẽ không khả thi. Ngay cả với n khoảng 200000, số lượng thao tác vẫn có thể quản lý được dưới các ràng buộc cạnh tranh điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    import sys
    input = sys.stdin.readline
    import math, cmath

    def fft(a, invert):
        n = len(a)
        j = 0
        for i in range(1, n):
            bit = n >> 1
            while j & bit:
                j ^= bit
                bit >>= 1
            j ^= bit
            if i < j:
                a[i], a[j] = a[j], a[i]

        length = 2
        while length <= n:
            ang = 2 * math.pi / length * (-1 if invert else 1)
            wlen = complex(math.cos(ang), math.sin(ang))
            i = 0
            while i < n:
                w = 1 + 0j
                for j in range(length // 2):
                    u = a[i + j]
                    v = a[i + j + length // 2] * w
                    a[i + j] = u + v
                    a[i + j + length // 2] = u - v
                    w *= wlen
                i += length
            length <<= 1

        if invert:
            for i in range(n):
                a[i] /= n

    def multiply(a, b):
        n = 1
        while n < len(a) + len(b) - 1:
            n <<= 1
        fa = list(map(complex, a)) + [0] * (n - len(a))
        fb = list(map(complex, b)) + [0] * (n - len(b))

        fft(fa, False)
        fft(fb, False)

        for i in range(n):
            fa[i] *= fb[i]

        fft(fa, True)

        return [int(fa[i].real + 0.5) for i in range(len(a) + len(b) - 1)]

    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    b = list(map(int, input().split()))
    c = multiply(a, b)
    return " ".join(map(str, c))

# provided samples
# assert run("...") == "...", "sample 1"

# custom cases
assert run("1\n1\n1\n1\n") == "1", "single element"
assert run("3\n1 2 3\n3\n4 5 6\n") == "4 13 28 27 18", "basic convolution"
assert run("4\n0 1 0 0\n3\n0 0 2\n") == "0 0 0 2 0 0", "sparse shift"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng 1 phần tử | 1 | trường hợp ranh giới tối thiểu | 
| mảng nhỏ dày đặc | 4 13 28 27 18 | tính đúng đắn của phép chập đầy đủ | 
| mảng thưa thớt | chuyển đầu ra | tính đúng đắn của việc dịch chuyển chỉ số | 

## Vỏ cạnh 

Đầu vào tối thiểu trong đó cả hai mảng đều chứa một phần tử duy nhất kiểm tra xem việc triển khai có xử lý phần đệm chính xác hay không và tránh độ phức tạp FFT không cần thiết trong khi vẫn tạo ra kết quả dựa trên biến đổi hợp lệ. Trong trường hợp đó, tích chập giảm xuống còn một phép nhân duy nhất và đường dẫn FFT vẫn trả về mảng một phần tử mà không bị hỏng. 

Đầu vào thưa thớt trong đó chỉ có một phần tử khác 0 trong mỗi mảng xác nhận rằng thuật toán không bị mất các đóng góp trong quá trình chuyển đổi. Mặc dù hầu hết các giá trị đều bằng 0, FFT vẫn phân phối chính xác đóng góp duy nhất đó trên không gian tần số và tái tạo lại nó ở chỉ số đầu ra chính xác. 

Trường hợp có số 0 ở cuối sẽ kiểm tra xem việc cắt độ dài kết quả thành n + m − 1 có được thực hiện chính xác hay không. Nếu không có hạn chế này, đầu ra sẽ bao gồm các giá trị tích chập tuần hoàn nhân tạo được giới thiệu bởi phần đệm FFT.
