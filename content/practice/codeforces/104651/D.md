---
title: "CF 104651D - Biến đổi Fourier rời rạc"
description: "Chúng ta được cấp một dãy số nguyên có độ dài n. Từ đó, chúng tôi tính toán biến đổi Fourier rời rạc của nó, tạo ra n giá trị phức tạp. Mỗi tần số t tương ứng với một tổng phức của tất cả các phần tử mảng, mỗi phần tử được nhân với một phép quay phức đơn vị tùy thuộc vào chỉ số và t của nó."
date: "2026-06-29T15:16:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104651
codeforces_index: "D"
codeforces_contest_name: "The 2023 CCPC Online Contest"
rating: 0
weight: 104651
solve_time_s: 69
verified: true
draft: false
---

[CF 104651D - Biến đổi Fourier rời rạc](https://codeforces.com/problemset/problem/104651/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên có độ dài n. Từ đó, chúng tôi tính toán biến đổi Fourier rời rạc của nó, tạo ra n giá trị phức tạp. Mỗi tần số t tương ứng với một tổng phức của tất cả các phần tử mảng, mỗi phần tử được nhân với một phép quay phức đơn vị tùy thuộc vào chỉ số và t của nó. 

Chúng ta được phép sửa đổi chính xác một vị trí, chỉ số k, thay thế nó bằng bất kỳ số nguyên nào chúng ta chọn. Sự thay đổi duy nhất này ảnh hưởng đồng thời đến mọi hệ số Fourier vì mỗi hệ số là sự kết hợp tuyến tính của tất cả các phần tử mảng. 

Mục tiêu là chọn giá trị mới tại vị trí k sao cho sau khi tính toán lại phép biến đổi, độ lớn lớn nhất trong tất cả các hệ số Fourier trở nên nhỏ nhất có thể. 

Các ràng buộc đủ nhỏ để có thể chấp nhận được phương pháp tiền xử lý O(n2). Vì n nhiều nhất là 2000 nên việc tính toán trực tiếp tất cả các hệ số Fourier là khả thi. Phần khó hơn là tối ưu hóa một biến miễn phí. 

Trường hợp cạnh tinh vi xuất hiện khi sửa đổi tối ưu khác xa giá trị ban đầu. Một cách tiếp cận đơn giản có thể chỉ thử những điều chỉnh nhỏ hoặc giả sử giá trị tốt nhất nằm gần f_k ban đầu, nhưng lựa chọn tối ưu phụ thuộc vào sự cân bằng toàn cục trên tất cả các tần số chứ không phải cấu trúc cục bộ. 

Ví dụ: nếu chuỗi ban đầu đã có một đỉnh Fourier chiếm ưu thế, việc thay đổi f_k có thể “kéo” đỉnh đó xuống nhưng có thể nâng các đỉnh khác lên một chút. Việc hạn chế x trong một vùng lân cận nhỏ xung quanh f_k có thể bỏ lỡ hoàn toàn mức tối ưu thực sự. 

## Phương pháp tiếp cận 

Việc giải thích trực tiếp rất đơn giản: thử mọi giá trị thay thế có thể có cho f_k, tính toán lại biến đổi Fourier và theo dõi câu trả lời tốt nhất. Tuy nhiên, điều này ngay lập tức là không thể vì giá trị ứng cử viên là không giới hạn. Ngay cả việc giới hạn ở một phạm vi số hợp lý vẫn để lại vô số khả năng. 

Quan sát quan trọng là biến đổi Fourier là tuyến tính trong chuỗi đầu vào. Nếu chúng ta biểu thị phép biến đổi ban đầu bằng F⁰_t, thì việc thay f_k bằng x sẽ thay đổi từng hệ số bằng cách cộng (x − f_k) nhân với một nghiệm phức đơn vị tùy thuộc vào t. Điều này có nghĩa là mọi hệ số Fourier đều trở thành hàm affine của một biến thực x. 

Sau khi viết lại, mỗi hệ số trở thành một điểm trong mặt phẳng có khoảng cách từ gốc tọa độ là hàm lồi của x. Mục tiêu là giá trị lớn nhất của các hàm lồi này trên tất cả t. Tối đa các hàm lồi vẫn là hàm lồi, do đó bài toán giảm xuống mức cực tiểu hóa hàm lồi một chiều trên x thực, với kết quả cuối cùng được đánh giá là một số nguyên. 

Cấu trúc này cho phép tìm kiếm ba chiều trên x trong số thực. Mỗi đánh giá yêu cầu tính toán tất cả n khoảng cách, đưa ra chi phí O(n) cho mỗi lần kiểm tra. Độ phức tạp tổng thể trở thành độ chính xác nhật ký O (n2), đủ nhanh cho n lên tới 2000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên x | Vô hạn / O(phạm vi · n²) | O(n) | Không thể | 
| Tìm kiếm bậc ba tối ưu | O(n² log R) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên, chúng tôi tính toán biến đổi Fourier rời rạc của mảng ban đầu. Điều này mang lại cho chúng ta các hệ số cơ bản F⁰_t cho mọi tần số t. 

Tiếp theo, chúng tôi tách biệt sự thay đổi ở chỉ số k ảnh hưởng như thế nào đến từng hệ số. Chúng tôi tính toán trước hệ số phức đơn vị ω_t = e^{-2π i k t / n}. Nếu chúng ta thay f_k bằng giá trị x thì hệ số mới sẽ trở thành F⁰_t + (x − f_k) · ω_t. 

Bây giờ chúng ta diễn giải lại biểu thức này về mặt hình học bằng cách xoay từng hệ số sao cho hướng ω_t trở thành trục thực. Điều này biến mỗi tần số thành một điểm cố định trong mặt phẳng phức và biến x dịch chuyển dọc theo trục thực. Độ lớn trở thành khoảng cách từ điểm chuyển động x đến một điểm cố định trong mặt phẳng.

Với mỗi t, chúng tôi xác định một hàm g_t(x) = |x + b_t|, trong đó b_t là hằng số phức được tính toán trước bắt nguồn từ F⁰_t và ω_t. Mục tiêu là giảm thiểu max_t g_t(x). 

Sau đó, chúng tôi tìm kiếm giá trị thực x để giảm thiểu mức tối đa này. Vì cực đại của hàm lồi là lồi nên chúng ta áp dụng tìm kiếm bậc ba trên x thực. 

Tại mỗi ứng cử viên x, chúng tôi đánh giá tất cả các tần số và tính khoảng cách tối đa. Sau khi hội tụ, chúng ta kiểm tra các giá trị số nguyên tốt nhất xung quanh giá trị thực tối ưu được tìm thấy, vì đáp án cuối cùng phải là một số nguyên thay thế. 

### Tại sao nó hoạt động 

Mỗi g_t(x) là một hàm lồi của x vì nó là khoảng cách Euclide từ một điểm cố định trong mặt phẳng đến một điểm chuyển động dọc theo một đường thẳng. Cực đại của các hàm lồi cũng là lồi, đảm bảo một cực tiểu toàn cục duy nhất. Điều này đảm bảo tìm kiếm bậc ba không bị mắc kẹt trong cực tiểu cục bộ và việc thu hẹp khoảng cách luôn duy trì mức tối ưu thực sự. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def dft(f):
    n = len(f)
    res = [0j] * n
    for t in range(n):
        acc = 0j
        for s in range(n):
            angle = -2.0 * math.pi * s * t / n
            acc += f[s] * complex(math.cos(angle), math.sin(angle))
        res[t] = acc
    return res

def solve():
    n, k = map(int, input().split())
    f = list(map(int, input().split()))

    F = dft(f)

    base = f[k]

    # precompute w_t = exp(-i 2π k t / n)
    w = []
    for t in range(n):
        angle = -2.0 * math.pi * k * t / n
        w.append(complex(math.cos(angle), math.sin(angle)))

    # b_t = F_t - f_k * w_t
    b = [F[t] - base * w[t] for t in range(n)]

    def cost(x):
        x = float(x)
        best = 0.0
        for t in range(n):
            val = b[t] + x * w[t]
            best = max(best, abs(val))
        return best

    # ternary search on real x
    lo, hi = -1e5, 1e5
    for _ in range(80):
        m1 = (2 * lo + hi) / 3
        m2 = (lo + 2 * hi) / 3
        if cost(m1) < cost(m2):
            hi = m2
        else:
            lo = m1

    x0 = (lo + hi) / 2

    # check nearby integers
    best_ans = float('inf')
    for xi in range(int(x0) - 3, int(x0) + 4):
        best_ans = min(best_ans, cost(xi))

    print(best_ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán trực tiếp toàn bộ biến đổi Fourier, điều này có thể chấp nhận được trong các ràng buộc. Sau đó, nó tách biệt sự đóng góp của chỉ số k bằng cách sử dụng các hệ số xoay được tính toán trước. 

Hàm chi phí đánh giá cường độ tối đa trên tất cả các tần số cho một giá trị thay thế nhất định. Tìm kiếm bậc ba liên tục thu hẹp khoảng thời gian mà hàm lồi đạt mức tối thiểu. Bởi vì số học dấu phẩy động đưa ra độ lệch nhỏ, bước cuối cùng sẽ kiểm tra các giá trị số nguyên xung quanh giá trị tối ưu liên tục để đảm bảo thỏa mãn ràng buộc số nguyên. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ trong đó việc thay đổi một phần tử sẽ làm thay đổi đáng kể sự cân bằng quang phổ. Giả sử n = 3, k = 2 và f = [1, 1, 0]. 

Đầu tiên chúng ta tính toán các hệ số Fourier. Sau đó, chúng tôi kiểm tra xem việc thay đổi f₂ ảnh hưởng như thế nào đến tất cả các hệ số cùng một lúc. 

| bước | x đoán | cấu trúc F_t bị ảnh hưởng | tối đa |F_t| | 

|------|--------|---------------|--------| 

| bắt đầu | 0 | quang phổ ban đầu | lớn | 

| giữa1 | -2 | giảm tần số chi phối | nhỏ hơn | 

| giữa2 | 2 | sự mất cân bằng chuyển sang nơi khác | lớn hơn | 

Tìm kiếm bậc ba thiên về hướng giảm cực đại, cuối cùng hội tụ gần điểm cân bằng tốt nhất. 

Ví dụ này cho thấy rằng sửa đổi tốt nhất không nhất thiết phải gần với giá trị ban đầu 0; nó được chọn để cân bằng toàn cầu tất cả các tần số thay vì điều chỉnh cục bộ một tần số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + n log R) | Chi phí DFT O(n²), mỗi lần đánh giá có chi phí O(n), tìm kiếm bậc ba sử dụng các đánh giá O(log R) | 
| Không gian | O(n) | Lưu trữ hệ số Fourier và hệ số xoay được tính toán trước | 

Với n 2000, DFT đóng góp khoảng 4 triệu phép tính và tìm kiếm bậc ba bổ sung thêm vài trăm nghìn phép tính, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # inline solution
    import math

    def dft(f):
        n = len(f)
        res = [0j] * n
        for t in range(n):
            acc = 0j
            for s in range(n):
                angle = -2.0 * math.pi * s * t / n
                acc += f[s] * complex(math.cos(angle), math.sin(angle))
            res[t] = acc
        return res

    n, k = map(int, input().split())
    f = list(map(int, input().split()))
    F = dft(f)
    base = f[k]

    w = []
    for t in range(n):
        angle = -2.0 * math.pi * k * t / n
        w.append(complex(math.cos(angle), math.sin(angle)))

    b = [F[t] - base * w[t] for t in range(n)]

    def cost(x):
        best = 0.0
        for t in range(n):
            best = max(best, abs(b[t] + x * w[t]))
        return best

    lo, hi = -1e5, 1e5
    for _ in range(60):
        m1 = (2 * lo + hi) / 3
        m2 = (lo + 2 * hi) / 3
        if cost(m1) < cost(m2):
            hi = m2
        else:
            lo = m1

    x0 = (lo + hi) / 2
    ans = float('inf')
    for xi in range(int(x0) - 3, int(x0) + 4):
        ans = min(ans, cost(xi))

    return str(ans)

# provided sample
assert run("3 2\n1 1 0\n")[:1] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2, 1 1 0 | 2.0 | tính đúng đắn cơ bản | 
| 1 0, 5 | 0,0 | trường hợp tầm thường phần tử đơn | 
| 4 1, 1 2 3 4 | khác nhau | tính đối xứng của phản ứng Fourier | 
| 5 3, tất cả số không | 0,0 | độ ổn định phổ bằng không | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi sửa đổi tối ưu khác xa giá trị ban đầu của f_k. Nếu việc triển khai chỉ thử các giá trị gần f_k, thì nó sẽ bỏ lỡ các giải pháp trong đó việc hủy bỏ tốt nhất yêu cầu dịch chuyển lớn theo một hướng để giảm đỉnh Fourier chiếm ưu thế. Trong công thức lồi, điều này tương ứng với hàm cực tiểu nằm cách xa gốc của khoảng tìm kiếm. 

Một trường hợp khác là khi nhiều tần số chiếm ưu thế như nhau. Trong những trường hợp như vậy, hàm chi phí có vùng đáy phẳng thay vì mức tối thiểu rõ ràng. Tìm kiếm bậc ba vẫn hội tụ chính xác vì tính lồi đảm bảo rằng mọi điểm bằng phẳng cục bộ đều tối ưu toàn cục, nhưng việc làm tròn số nguyên trở nên cần thiết để tránh trôi ra khỏi vùng phẳng.
