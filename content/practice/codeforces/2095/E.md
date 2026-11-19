---
title: "CF 2095E - Số lượng cặp"
description: "Vấn đề này xuất phát từ Cuộc thi Ngày Cá tháng Tư. Bí quyết quan trọng là ký hiệu được hiển thị là ⊕ không thực sự là XOR theo bit. Liên kết gắn liền với biểu tượng dẫn đến một câu đố mà lời giải cho thấy rằng ⊕ nên được hiểu là phép nhân."
date: "2026-06-08T05:31:04+07:00"
tags: ["codeforces", "competitive-programming", "*special", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2095
codeforces_index: "E"
codeforces_contest_name: "April Fools Day Contest 2025"
rating: 0
weight: 2095
solve_time_s: 92
verified: true
draft: false
---

[CF 2095E - Số lượng cặp](https://codeforces.com/problemset/problem/2095/E) 

**Đánh giá:** - 
**Tags:** *đặc biệt, lý thuyết số 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề này xuất phát từ Cuộc thi Ngày Cá tháng Tư. Bí quyết chính là biểu tượng được hiển thị là`⊕`thực tế không phải là XOR theo bit. Liên kết gắn liền với biểu tượng dẫn đến một câu đố mà lời giải của nó tiết lộ rằng`⊕`nên được hiểu là phép nhân. Một khi điều đó được phát hiện, tình trạng sẽ trở thành$$(a_i a_j)(a_i^2 a_j^2)\equiv k \pmod p,$$điều đó đơn giản hóa thành$$a_i^3 a_j^3 \equiv k \pmod p.$$Vì vậy, chúng ta được cho một tập hợp các số phân biệt modulo một số nguyên tố$p$, và chúng ta phải đếm các cặp có khối lập phương nhân với nhau$k$modulo$p$. 

Kích thước mảng đạt$3 \cdot 10^5$, điều này ngay lập tức loại trừ việc kiểm tra tất cả các cặp. Một giải pháp bậc hai sẽ thực hiện gần như$4.5 \cdot 10^{10}$kiểm tra cặp trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log p)$. 

Một trường hợp tế nhị là$k=0$. Từ$p$là nguyên tố, sản phẩm$a_i^3 a_j^3$bằng 0 modulo$p$khi và chỉ khi ít nhất một trong hai số bằng 0. Vì tất cả các giá trị mảng đều khác nhau nên số 0 chỉ có thể xuất hiện một lần. 

Coi như:```
3 5 0
0 1 2
```Các cặp hợp lệ là$(0,1)$Và$(0,2)$, vậy đáp án là 2 

Việc triển khai bất cẩn tính toán một cách mù quáng các nghịch đảo mô-đun sẽ thất bại ở đây vì số 0 không có nghịch đảo. 

Một trường hợp quan trọng khác là khi$k \neq 0$nhưng một số phần tử mảng bằng 0.```
3 5 1
0 1 2
```Không có cặp nào chứa số 0 có thể thỏa mãn phương trình vì tích của nó luôn bằng 0 modulo$p$. Những phần tử như vậy phải được bỏ qua trong khi khớp với các giá trị khác 0. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cặp$(i,j)$, tính toán$$a_i^3 a_j^3 \bmod p$$và so sánh nó với$k$. Điều này đúng vì nó trực tiếp kiểm tra điều kiện. Thật không may, có$$\frac{n(n-1)}2$$cặp. Với$n=3\cdot10^5$, đó là về$4.5\cdot10^{10}$séc. 

Cấu trúc của phương trình gợi ý một hướng đi tốt hơn. Cho phép$$x_i = a_i^3 \bmod p.$$Điều kiện trở thành$$x_i x_j \equiv k \pmod p.$$Đối với một cố định$x_j\neq 0$, đối tác được yêu cầu được xác định duy nhất:$$x_i \equiv k \cdot x_j^{-1} \pmod p.$$Từ$p$là số nguyên tố, mọi số dư khác 0 đều có số nghịch đảo. Điều này chuyển đổi vấn đề từ kiểm tra cặp sang đếm tần số. 

Chúng tôi xử lý mảng từ trái sang phải. Đối với giá trị hiện tại$x_j$, chúng tôi tính toán phần dư phải xuất hiện trước đó. Bản đồ băm lưu trữ số lần mỗi khối đã được nhìn thấy. Số lượng dư yêu cầu chính xác là số cặp hợp lệ kết thúc ở vị trí$j$. 

Tình huống đặc biệt duy nhất là$k=0$, trong đó nghịch đảo không thể sử dụng được và phương trình có đặc tính đơn giản hơn nhiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log p)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng đầu vào. 
2. Tay cầm$k=0$riêng. 

Phương trình trở thành$$a_i^3 a_j^3 \equiv 0 \pmod p.$$Từ$p$là số nguyên tố, điều này xảy ra chính xác khi một phần tử bằng 0. 
3. Nếu có số 0 thì mọi cặp liên quan đến phần tử đó đều hợp lệ. 

Bởi vì tất cả các giá trị mảng đều khác nhau nên có nhiều nhất một giá trị 0. Câu trả lời là$n-1$. 
4. Nếu số 0 vắng mặt và$k=0$, không có cặp hợp lệ nào tồn tại. 
5. Đối với$k\neq0$, tạo bản đồ băm`cnt`lưu trữ tần số của dư lượng khối đã được xử lý. 
6. Quét mảng từ trái sang phải. 
7. Bỏ qua giá trị 0. 

Một cặp số 0 không thể tạo ra tích số khác 0 theo modulo$p$. 
8. Tính toán$$x = a_i^3 \bmod p.$$9. Tính nghịch đảo mô đun$$x^{-1} \equiv x^{p-2} \pmod p$$sử dụng định lý Fermat. 
10. Dư lượng đối tác yêu cầu là$$need = k \cdot x^{-1} \bmod p.$$11. Thêm`cnt[need]`để trả lời. 

Mỗi lần xuất hiện trước đây của`need`tạo thành một cặp hợp lệ với phần tử hiện tại. 
12. Chèn dư lượng hiện tại`x`vào bản đồ. 
13. Sau khi xử lý tất cả các phần tử, xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Đối với mọi phần tử khác 0, chúng ta lưu trữ modulo phần dư lập phương của nó$p$. Khi xử lý vị trí$j$, bản đồ chứa chính xác phần còn lại đến từ các vị trí$i<j$. 

phương trình$$x_i x_j \equiv k \pmod p$$tương đương với$$x_i \equiv k x_j^{-1} \pmod p$$bởi vì$x_j\neq0$. Do đó, mọi đối tác hợp lệ trước đó đều tương ứng với một lần xuất hiện của phần dư`need`, và mỗi lần xuất hiện của`need`tạo ra một cặp hợp lệ. Mỗi cặp được tính chính xác một lần khi điểm cuối bên phải của nó được xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, p, k = map(int, input().split())
    a = list(map(int, input().split()))

    if k == 0:
        if 0 in a:
            print(n - 1)
        else:
            print(0)
        return

    cnt = {}
    ans = 0

    for v in a:
        if v == 0:
            continue

        x = pow(v, 3, p)
        inv = pow(x, p - 2, p)
        need = (k * inv) % p

        ans += cnt.get(need, 0)
        cnt[x] = cnt.get(x, 0) + 1

    print(ans)

if __name__ == "__main__":
    main()
```Khối đầu tiên xử lý trường hợp đặc biệt$k=0$. Vì tất cả các giá trị đều khác nhau nên nhiều nhất chỉ có thể tồn tại một số 0. Nếu đúng như vậy thì mọi phần tử khác sẽ tạo thành một cặp hợp lệ với nó. 

Đối với trường hợp chung, bản đồ lưu trữ tần số của phần dư khối. biểu hiện`pow(v, 3, p)`tính toán$v^3 \bmod p$không tràn. biểu hiện`pow(x, p - 2, p)`tính nghịch đảo mô đun bằng định lý Fermat, định lý này hợp lệ vì$p$là số nguyên tố và$x\neq0$. 

Thứ tự quan trọng. Chúng tôi truy vấn bản đồ trước khi chèn phần dư hiện tại. Điều đó đảm bảo chỉ có cặp với$i<j$được tính và ngăn chặn việc tự ghép nối. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 2
0 1 2
```Từ$k\neq0$, chúng tôi sử dụng thuật toán chính. 

| Giá trị | Dư lượng khối$x$| Cần | Số trước | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | bỏ qua | - | - | 0 | 
| 1 | 1 | 2 | 0 | 0 | 
| 2 | 2 | 1 | 1 | 1 | 

Câu trả lời cuối cùng:`1`. 

Dấu vết này cho thấy cách bản đồ biến tìm kiếm cặp thành tra cứu. Khi xử lý`2`, số dư cần tìm là`1`, đã xuất hiện một lần. 

### Mẫu 2 

đầu vào:```
6 11 2
1 3 5 6 7 8
```Dư lượng khối theo modulo 11 là: 

| Giá trị | Dư lượng khối | 
| --- | --- | 
| 1 | 1 | 
| 3 | 5 | 
| 5 | 4 | 
| 6 | 7 | 
| 7 | 2 | 
| 8 | 6 | 

Trình tự xử lý: 

| Giá trị | Dư lượng$x$| Cần | Số trước | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 0 | 
| 3 | 5 | 7 | 0 | 0 | 
| 5 | 4 | 6 | 0 | 0 | 
| 6 | 7 | 5 | 1 | 1 | 
| 7 | 2 | 1 | 1 | 2 | 
| 8 | 6 | 4 | 1 | 3 | 

Câu trả lời cuối cùng:`3`. 

Ba số gia tương ứng chính xác với ba cặp hợp lệ được đề cập trong câu lệnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log p)$| Mỗi phần tử thực hiện một số lũy thừa mô-đun không đổi | 
| Không gian |$O(n)$| Bản đồ băm của dư lượng khối | 

Với$n=3\cdot10^5$Và$p\le10^9$, lũy thừa mô-đun chỉ cần khoảng 30 phép nhân cho mỗi lệnh gọi. Độ phức tạp thu được vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n, p, k = map(int, input().split())
    a = list(map(int, input().split()))

    if k == 0:
        print(n - 1 if 0 in a else 0)
        return

    cnt = {}
    ans = 0

    for v in a:
        if v == 0:
            continue

        x = pow(v, 3, p)
        inv = pow(x, p - 2, p)
        need = (k * inv) % p

        ans += cnt.get(need, 0)
        cnt[x] = cnt.get(x, 0) + 1

    print(ans)

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old
    return out.getvalue().strip()

# provided samples
assert run("3 3 2\n0 1 2\n") == "1", "sample 1"
assert run("6 11 2\n1 3 5 6 7 8\n") == "3", "sample 2"

# custom cases
assert run("2 5 0\n0 1\n") == "1", "single zero pair"
assert run("2 5 0\n1 2\n") == "0", "k=0 without zero"
assert run("2 5 1\n1 1\n") != "0" if False else True  # distinctness violated by statement
assert run("3 5 1\n0 1 2\n") == "0", "zero ignored when k!=0"
assert run("2 2 1\n0 1\n") == "0", "smallest prime"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 5 0 / 0 1`|`1`| Xử lý đặc biệt$k=0$| 
|`2 5 0 / 1 2`|`0`| Không có hiện tại | 
|`3 5 1 / 0 1 2`|`0`| Zero không thể đóng góp khi$k\neq0$| 
|`2 2 1 / 0 1`|`0`| Số nguyên tố được phép nhỏ nhất | 

## Vỏ cạnh 

Hãy xem xét:```
3 5 0
0 1 2
```Thuật toán ngay lập tức đi vào$k=0$chi nhánh. Vì số 0 tồn tại nên nó trả về$n-1=2$. Các cặp hợp lệ là$(0,1)$Và$(0,2)$, khớp chính xác với điều kiện toán học. 

Bây giờ hãy xem xét:```
3 5 0
1 2 3
```Một lần nữa$k=0$nhánh được sử dụng, nhưng không có nhánh nào. Không có tích nào có thể trở thành số 0 modulo một số nguyên tố trừ khi một thừa số bằng 0, vì vậy câu trả lời là`0`. 

Cuối cùng:```
3 5 1
0 1 2
```Giá trị 0 bị bỏ qua trong quá trình xử lý. Các cặn còn lại được kiểm tra bình thường. Vì không có cặp nào thỏa mãn phương trình thặng dư mục tiêu nên câu trả lời là`0`. Việc bỏ qua số 0 sẽ tránh việc cố gắng tính nghịch đảo của số 0, số này sẽ không được xác định.
