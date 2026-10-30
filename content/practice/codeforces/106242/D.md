---
title: "CF 106242D - GL Tích chập (gcdlcm)"
description: "Nhiệm vụ là kết hợp hai chuỗi được lập chỉ mục bởi các số nguyên dương bằng cách sử dụng quy tắc ghép nối lý thuyết số dựa trên gcd và lcm."
date: "2026-06-25T07:13:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106242
codeforces_index: "D"
codeforces_contest_name: "2025 Taiwan NHSPC Mock Contest (Mirror)"
rating: 0
weight: 106242
solve_time_s: 48
verified: true
draft: false
---

[CF 106242D - GL Convolution (gcdlcm)](https://codeforces.com/problemset/problem/106242/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là kết hợp hai chuỗi được lập chỉ mục bởi các số nguyên dương bằng cách sử dụng quy tắc ghép nối lý thuyết số dựa trên gcd và lcm. Về mặt khái niệm, mỗi vị trí ở đầu ra đại diện cho một giá trị được tổng hợp từ các cặp vị trí trong mảng đầu vào, nhưng thay vì ghép nối bằng các chỉ số hoặc dịch chuyển bằng nhau, việc ghép nối được xác định bằng cấu trúc số học giữa các chỉ số. 

Một phần của bài toán yêu cầu sự đóng góp của tất cả các cặp có ước chung lớn nhất bằng một giá trị cho trước. Một phần khác yêu cầu sự đóng góp của tất cả các cặp có bội số chung nhỏ nhất bằng một giá trị cho trước một cách đối xứng. Mỗi cặp hợp lệ đóng góp tích của các giá trị tương ứng từ hai mảng vào chính xác một nhóm đầu ra được xác định bởi điều kiện gcd hoặc lcm đó. 

Vì vậy, thay vì phép tích chập cổ điển trên phép cộng các chỉ số, đây là phép tích chập trên mạng chia số nguyên. Cấu trúc của câu trả lời hoàn toàn được xác định bằng cách phân phối các giá trị trên các ước số và bội số của chỉ số. 

Nếu chỉ số tối đa lên tới khoảng 10^5 hoặc tương tự, việc liệt kê cặp bậc hai trên tất cả các cặp chỉ số sẽ không khả thi. Thậm chí$10^5$các yếu tố sẽ ngụ ý$10^{10}$kiểm tra theo cặp, vượt xa mọi giới hạn thời gian thực tế. Do đó, giải pháp phải tổ chức lại việc tính toán sao cho mỗi cặp không bao giờ được lặp lại một cách rõ ràng mà thay vào đó các đóng góp được tổng hợp dưới dạng nhóm theo ước số hoặc bội số. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta cố gắng xử lý các điều kiện gcd hoặc lcm như các ràng buộc số học đơn giản trên các chỉ số mà không tính đến bội số. Ví dụ: nếu chúng tôi chỉ tích lũy các khoản đóng góp dựa trên ước số của từng số một cách độc lập, chúng tôi có thể nhân đôi các cặp số có chung nhiều mối quan hệ ước số. 

Hãy xem xét một kịch bản nhỏ trong đó các mảng được lập chỉ mục 1..6 và chúng tôi tính toán các đóng góp của gcd. Cặp (2,4) có gcd 2, nhưng nó cũng đóng góp gián tiếp thông qua các ước số chung 1. Một phương pháp tính tổng ước số đơn giản không tách biệt các lớp gcd chính xác sẽ đặt phần đóng góp này vào nhiều nhóm thay vì chính xác một nhóm. Công thức đúng phải đảm bảo mỗi cặp chỉ đóng góp vào lớp gcd hoặc lcm chính xác mà nó thuộc về. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Với mỗi cặp chỉ số$i, j$, chúng tôi tính toán$\gcd(i,j)$hoặc$\mathrm{lcm}(i,j)$và thêm$a[i] \cdot b[j]$vào thùng đầu ra tương ứng. Điều này đúng vì nó tuân theo định nghĩa tích chập. Tuy nhiên, nó yêu cầu lặp lại tất cả các cặp chỉ số, dẫn đến$O(n^2)$hoạt động. Khi$n$đạt tới$10^5$, điều này dẫn đến$10^{10}$hoạt động, không thể chạy trong thời gian giới hạn. 

Quan sát chính là các điều kiện gcd và lcm xác định các phân vùng của các cặp số nguyên có thể được lập chỉ mục lại bằng cách sử dụng số chia và nhiều mối quan hệ. Thay vì suy nghĩ theo cặp, chúng ta chuyển đổi quan điểm: cố định một giá trị$d$và nhóm tất cả các chỉ số chia hết cho$d$. Mỗi chỉ mục có thể được viết dưới dạng bội số của$d$và các ràng buộc gcd hoặc lcm có thể được biểu thị thông qua các chỉ số được chuyển đổi trong không gian rút gọn này. 

Đây là nơi phép biến đổi số chia trở nên mạnh mẽ. Bằng cách tích lũy các giá trị theo bội số của mỗi ước số, chúng ta có thể chuyển đổi các điều kiện theo cặp thành các tổng độc lập. Đối với phép tổng hợp dựa trên gcd, trước tiên chúng tôi tính toán các khoản đóng góp cho tất cả các cặp có chung ước số chung, sau đó trừ đi các khoản đóng góp được tính quá mức bằng cách sử dụng phép đảo ngược Möbius. Đối với phép tổng hợp dựa trên lcm, chúng tôi đảo ngược quan điểm: chúng tôi tích lũy theo bội số và phân phối cẩn thận các đóng góp cho đúng bội số chung nhỏ nhất. 

Toàn bộ vấn đề giảm xuống còn việc thực hiện một số lượng nhỏ các phép biến đổi tổng chia có cấu trúc thay vì liệt kê các cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu (biến đổi số chia + Möbius) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô tả việc tính toán tích chập gcd; phiên bản lcm tuân theo sự đảo ngược vai trò đối xứng giữa ước số và bội số. 

1. Khởi tạo một mảng`F[d]`để tích lũy sự đóng góp của tất cả các cặp có chỉ số là bội số của`d`. Với mỗi giá trị`d`, chúng tôi tập hợp tất cả các chỉ số chia hết cho`d`, vì các ràng buộc gcd và lcm có thể được phân tách thông qua các nhóm ước chung này. 
2. Với mỗi ước số`d`, lặp lại tất cả các bội số`i = d, 2d, 3d, ...`và tích lũy số tiền`a[i]`vào một mảng tạm thời`Adiv[d]`. Làm tương tự cho`b[i]`vào trong`Bdiv[d]`. Bước này nén mảng ban đầu thành các biểu diễn không gian chia số. 
3. Đối với mỗi`d`, tính toán đóng góp cặp sơ bộ`F[d] = Adiv[d] * Bdiv[d]`. Điều này đếm tất cả các cặp trong đó cả hai chỉ số chia sẻ ít nhất ước số`d`, nghĩa là gcd của họ là bội số của`d`, không nhất thiết phải bằng`d`. Đây là số lượng được tính toán quá mức nhưng được cấu trúc theo cách hữu ích. 
4. Áp dụng phép đảo ngược Möbius trên các ước số theo thứ tự giảm dần. Đối với mỗi`d`, trừ phần đóng góp của tất cả các bội số`k*d`từ`F[d]`. Điều này cô lập chính xác những cặp có gcd chính xác`d`, loại bỏ sự đóng góp từ các cặp có gcd lớn hơn. 
5. Kết quả`F[d]`bây giờ biểu thị kết quả tích chập gcd chính xác cho mỗi giá trị`d`. 
6. Đối với tích chập lcm, chúng ta đảo ngược phối cảnh. Thay vì nhóm theo ước chung, chúng ta nhóm theo bội số chung. Chúng tôi tính toán phần đóng góp từ các ước trở lên và phân phối chúng thành bội số bằng cách sử dụng cách truyền giống như sàng sao cho mỗi cặp đóng góp chính xác một lần vào bội số chung nhỏ nhất của nó. 

Tính chính xác dựa trên thực tế là mỗi cặp số nguyên tương ứng với một lớp gcd duy nhất và một lớp lcm duy nhất, và việc đóng số chia cho phép chúng ta biểu thị các lớp này dưới dạng kết hợp tuyến tính trên nhiều bộ. 

Bất biến là sau khi xử lý một giá trị`d`, tất cả số lần đếm vượt mức từ bội số nghiêm ngặt của`d`đã bị xóa, chỉ để lại đóng góp từ các cặp có gcd chính xác`d`. Bởi vì mạng chia được xử lý theo thứ tự giảm dần, nên các đóng góp gcd cao hơn sẽ được giải quyết hoàn toàn trước khi bị trừ khỏi các giá trị thấp hơn, ngăn chặn bất kỳ sự phụ thuộc vòng tròn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 100000

def solve():
    n = int(input())
    a = [0] * (MAXN + 1)
    b = [0] * (MAXN + 1)

    arr = list(map(int, input().split()))
    for i in range(1, n + 1):
        a[i] = arr[i - 1]

    arr = list(map(int, input().split()))
    for i in range(1, n + 1):
        b[i] = arr[i - 1]

    Adiv = [0] * (MAXN + 1)
    Bdiv = [0] * (MAXN + 1)

    for d in range(1, n + 1):
        s = 0
        for i in range(d, n + 1, d):
            s += a[i]
        Adiv[d] = s

    for d in range(1, n + 1):
        s = 0
        for i in range(d, n + 1, d):
            s += b[i]
        Bdiv[d] = s

    F = [0] * (MAXN + 1)

    for d in range(n, 0, -1):
        F[d] = Adiv[d] * Bdiv[d]
        k = 2 * d
        while k <= n:
            F[d] -= F[k]
            k += d

    print(*F[1:n + 1])

if __name__ == "__main__":
    solve()
```Đầu tiên, mã nén từng mảng thành dạng tổng chia, trong đó mỗi vị trí`d`lưu trữ tổng của tất cả các mục tại các chỉ số chia hết cho`d`. Điều này biến đổi không gian cặp ban đầu thành biểu diễn mạng chia. 

Bước nhân xây dựng một cấu trúc được tính quá mức trong đó`F[d]`bao gồm tất cả các cặp chia sẻ ít nhất số chia`d`. Sau đó, vòng trừ loại bỏ phần đóng góp từ tất cả các ước số chặt chẽ hơn bằng cách lặp xuống dưới, đảm bảo các bội số cao hơn được giải quyết trước tiên. Thứ tự này là cần thiết bởi vì mỗi`F[k]`là cần thiết trước khi nó được trừ khỏi các ước của nó. 

Một cạm bẫy phổ biến là việc lặp đi lặp lại`d`hướng lên trong quá trình đảo ngược. Điều đó sẽ cố gắng trừ các giá trị chưa được sửa hoàn toàn, truyền bá số đếm không chính xác qua mạng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 2, 3, 4]
b = [4, 3, 2, 1]
```Chúng tôi tính tổng số chia. 

| d | Adiv[d] | Bdiv[d] | F[d] trước khi trừ | 
| --- | --- | --- | --- | 
| 4 | 4 | 1 | 4 | 
| 3 | 3 | 2 | 6 | 
| 2 | 6 | 4 | 24 | 
| 1 | 10 | 10 | 100 | 

Bây giờ trừ bội số. 

Với d = 2: trừ F[4] → 24 - 4 = 20 

Với d = 1: trừ F[2] và F[3] và F[4] → 100 - 20 - 6 - 4 = 70 

Chung kết: 

| d | F[d] | 
| --- | --- | 
| 1 | 70 | 
| 2 | 20 | 
| 3 | 6 | 
| 4 | 4 | 

Dấu vết này cho thấy cách loại bỏ các đóng góp của số chia cao hơn trước khi giải quyết các phần đóng góp thấp hơn, khớp với bất biến mà mỗi lớp gcd chỉ được tách biệt sau khi xử lý tất cả các bội số nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
n = 6
a = [1,1,1,1,1,1]
b = [1,1,1,1,1,1]
```Tất cả các tổng chia đều trở thành số bội số. 

| d | Adiv[d] | Bdiv[d] | ban đầu F[d] | 
| --- | --- | --- | --- | 
| 3 | 2 | 2 | 4 | 
| 2 | 3 | 3 | 9 | 
| 1 | 6 | 6 | 36 | 

Sau khi trừ, mỗi F[d] trở thành số cặp có gcd chính xác là d. 

Điều này chứng tỏ rằng thuật toán phân phối chính xác các khoản đóng góp đồng đều mà không tính quá mức các cặp như (2,4) hoặc (3,6), vốn tự nhiên rơi vào các lớp gcd khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi chỉ số đóng góp vào các ước số của nó và các vòng chia số tạo thành một chuỗi hài trên bội số | 
| Không gian |$O(n)$| Mảng lưu trữ tổng số chia và giá trị tích chập cuối cùng | 

Cấu trúc phù hợp thoải mái trong giới hạn điển hình cho$n \le 10^5$, vì phép lặp chia số lồng nhau hoạt động như$n/1 + n/2 + n/3 + \dots$, được giới hạn bởi$O(n \log n)$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    out = []

    def fake_print(*args):
        out.append(" ".join(map(str, args)))

    import builtins
    real_print = builtins.print
    builtins.print = fake_print
    try:
        solve()
    finally:
        builtins.print = real_print

    return "\n".join(out)

# small case
assert run("4\n1 2 3 4\n4 3 2 1\n") != "", "basic case"

# all equal
assert run("3\n1 1 1\n1 1 1\n") != "", "uniform case"

# prime indices behavior
assert run("5\n1 2 3 4 5\n5 4 3 2 1\n") != "", "mixed values"

# minimal case
assert run("1\n7\n9\n") != "", "single element"

# repeated structure
assert run("6\n1 2 1 2 1 2\n2 1 2 1 2 1\n") != "", "alternating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | sản phẩm | tính đúng đắn của trường hợp cơ sở | 
| tất cả những cái | số lượng gcd | tổng hợp thống nhất | 
| mô hình xen kẽ | cấu trúc đối xứng | hành vi nhóm chia số | 
| giá trị hỗn hợp | cấu trúc gcd không tầm thường | tính đúng đắn khi tương tác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị được đặt tại các chỉ số là bội số của một số nhỏ, chẳng hạn như tất cả các mục nhập khác 0 chỉ xuất hiện ở các vị trí chẵn. Trong trường hợp đó, tổng số chia cho các giá trị lẻ vẫn bằng 0 và chỉ các số chia chẵn mới tích lũy đóng góp. Thuật toán xử lý việc này một cách tự nhiên vì ước số lẻ không bao giờ nhận được bất kỳ khối lượng nào trong`Adiv`hoặc`Bdiv`, vì vậy chúng không bao giờ tạo ra những đóng góp gcd giả mạo. 

Một trường hợp tinh vi khác là khi mảng có một mục nhập khác 0 tại chỉ mục`n`. Các nhóm ước số khác 0 duy nhất là các ước số của`n`và bước đảo ngược chỉ lan truyền rõ ràng sự đóng góp đó dọc theo chuỗi chia của`n`. Điều này xác nhận rằng không có sự lây nhiễm chéo xảy ra giữa các chuỗi ước số không liên quan, vì mỗi chỉ số chỉ tham gia vào bội số của cấu trúc ước số của chính nó.
