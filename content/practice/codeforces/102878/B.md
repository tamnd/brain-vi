---
title: "CF 102878B - Vấn đề dư lượng"
description: "Vấn đề yêu cầu chúng tôi trả lời nhiều truy vấn độc lập. Trong mỗi truy vấn, mô đun nguyên tố P được cố định và chúng tôi xác định giá trị f(i,r,P) là số cặp (a,b) modulo P thỏa mãn phương trình mô đun bao gồm a^r, b và b^2."
date: "2026-07-25T12:42:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "B"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 48
verified: true
draft: false
---

[CF 102878B - Vấn đề về dư lượng](https://codeforces.com/problemset/problem/102878/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng tôi trả lời nhiều truy vấn độc lập. Trong mỗi truy vấn, một mô đun nguyên tố`P`là cố định và chúng tôi xác định một giá trị`f(i,r,P)`bằng số lượng cặp`(a,b)`modulo`P`thỏa mãn một phương trình mô đun liên quan đến`a^r`,`b`, Và`b^2`. Câu trả lời cuối cùng không phải là yêu cầu các giá trị riêng lẻ của`f`; nó yêu cầu sản phẩm của`Y`được nâng lên những giá trị này cho mọi`i`từ`1`ĐẾN`N`, modulo`10^9+7`. 

Phép biến đổi hữu ích đầu tiên là kết hợp các số mũ. Từ$$\prod_{i=1}^{N}Y^{f(i,r,P)}=Y^{\sum_{i=1}^{N}f(i,r,P)},$$mỗi truy vấn chỉ yêu cầu tìm một số mũ, tổng của tất cả`f(i,r,P)`các giá trị. 

Số lượng truy vấn có thể đạt tới`100000`, trong khi`P`có thể ở gần`10^9`. Một giải pháp lặp trên tất cả các dư lượng modulo`P`, hoặc thậm chí trên một phần lớn trong số chúng, là không thể. Chúng ta cần một công thức chỉ phụ thuộc vào các tính chất số học nhỏ của`P`,`r`, Và`N`. Sự hạn chế`r <= 3`là đầu mối quan trọng, vì nó cho phép chúng ta suy luận về lũy thừa trong một trường hữu hạn mà không cần thực hiện các phép tính tốn kém. 

Phương trình là$$a^r(b+b^2)^i \equiv b^i \pmod P.$$Các trường hợp biểu thức bị thoái hóa cần xử lý đặc biệt. Nếu như`b=0`, cả hai vế đều bằng 0, nên mọi`a`hoạt động. Nếu như`b=-1`, sau đó`b+b^2=0`Nhưng`b^i`khác 0 nên không`a`hoạt động. Đối với tất cả các giá trị khác, chúng ta có thể chia:$$a^r \equiv (b/(b+b^2))^i.$$Bởi vì`b+b^2=b(b+1)`, điều này trở thành$$a^r \equiv (b+1)^{-i}.$$Vì vậy, vấn đề trở thành việc đếm tần suất một giá trị là một`r`- lũy thừa thứ trong nhóm nhân của trường. 

Việc thực hiện bất cẩn có thể quên các giá trị đặc biệt. Ví dụ, nếu`P=5`,`r=2`, Và`i=1`, truy vấn đầu vào chứa mô-đun`5`. giá trị`b=0`đóng góp năm cặp hợp lệ vì mỗi`a`hoạt động. Nếu chúng ta chỉ đếm các số dư khác 0 và bỏ qua số 0, chúng ta sẽ mất năm cặp này và nhận sai số mũ. 

Một lỗi phổ biến khác là xử lý sai`b=-1`. Vì`P=7`,`r=3`, Và`i=1`, đang chọn`b=6`cho`b+b^2=0`, trong khi`b^i=6`, do đó không có cặp hợp lệ nào từ giá trị này. Đối xử với nó như`b=0`trường hợp sẽ thêm giải pháp không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lặp lại mọi thứ có thể`b`, sau đó đếm số lượng`a`các giá trị thỏa mãn phương trình. Đối với một cố định`b`, chúng tôi có thể kiểm tra mọi`a`và kiểm tra sự phù hợp. Điều này đúng vì nó xem xét chính xác định nghĩa của`f`. Tuy nhiên, một truy vấn có thể yêu cầu khoảng`P^2`kiểm tra, điều này vượt xa những gì có thể khi`P`gần với`10^9`. 

Điều quan trọng là sau khi loại bỏ hai giá trị đặc biệt`b=0`Và`b=-1`, mọi thứ còn lại`b`tương ứng với chính xác một phần tử trường khác 0`x=b+1`khác với`1`. Số lượng các giải pháp$$a^r=t$$trong nhóm nhân chỉ được xác định bởi việc`t`thuộc nhóm con của`r`-quyền lực thứ. 

Vì`r=1`, mọi giá trị đều có một gốc. Vì`r=2`, chính xác một nửa số giá trị khác 0 là bình phương. Vì`r=3`, hoặc mọi giá trị đều có một căn bậc ba duy nhất khi`3`không chia`P-1`hoặc một phần ba các giá trị là lập phương và mỗi lập phương có ba nghiệm khi`3`chia rẽ`P-1`. 

Điều này cho phép chúng ta đếm tất cả những gì có thể`b`các giá trị chỉ sử dụng tính chẵn lẻ hoặc tính chia hết của`i`. 

Vì`r=2`, nếu như`i`là số chẵn, mọi khác 0`x`cho một hình vuông bởi vì`x^i`là một hình vuông. Nếu như`i`là số lẻ, chỉ có giá trị bình phương của`x`công việc. 

Vì`r=3`, nếu như`P-1`không chia hết cho`3`, phép toán khối là một hoán vị nên mọi giá trị đều đúng. Ngược lại, chỉ mỗi giá trị khác 3 thứ ba là lập phương. Mô hình chỉ phụ thuộc vào việc`i`chia hết cho`3`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(P²N) | O(1) | Quá chậm | 
| Tối ưu | O(log MOD) cho mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số mũ`E = sum(f(i,r,P))`thay vì sản phẩm trực tiếp. Câu trả lời cuối cùng là`pow(Y, E, 1e9+7)`. 
2. Tay cầm`r=1`. Mọi khác không`b`ngoại trừ`-1`đóng góp một giá trị`a`, Và`b=0`đóng góp`P`cặp hợp lệ. Giá trị cho mỗi`i`do đó là hằng số. 
3. Tay cầm`r=2`. luôn luôn có`P`giải pháp từ`b=0`. Trong số còn lại`P-2`giá trị, đếm xem có bao nhiêu`(b+1)^(-i)`là những hình vuông. Khi`i`thậm chí là tất cả các giá trị đều hoạt động. Khi`i`là số lẻ, chỉ có thặng dư bậc hai khác 0 mới có tác dụng. 
4. Tay cầm`r=3`. Một lần nữa, hãy bắt đầu với`P`giải pháp từ`b=0`. Nếu như`P-1`không chia hết cho`3`, mọi giá trị khác 0 đều là lập phương. Mặt khác, chỉ một phần ba giá trị khác 0 là lập phương và điều này chỉ xảy ra khi`i`không phải là bội số của`3`. 
5. Nhân các khoản đóng góp với số chỉ số có mỗi thuộc tính. Ví dụ, thay vì lặp qua mọi số chẵn`i`, tính xem có bao nhiêu giá trị chẵn xuất hiện trong`1..N`. 

Tại sao nó hoạt động: 

Sau khi thay thế`x=b+1`, mọi trường hợp khác 0 hợp lệ sẽ hỏi liệu`x^{-i}`thuộc nhóm con của`r`-quyền lực thứ. Trong một nhóm tuần hoàn, việc lấy nghịch đảo không làm thay đổi tư cách thành viên trong một nhóm con, do đó câu hỏi tương đương với việc hỏi liệu`x^i`là một`r`-quyền lực thứ. Kích thước nhóm con của hình vuông và hình khối được cố định bởi`gcd(r,P-1)`, đưa ra các công thức được sử dụng ở trên. Mọi khả năng có thể`b`được bao phủ bởi chính xác một trường hợp, do đó số mũ tổng hợp là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve_query(Y, N, r, P):
    if r == 1:
        exp = N * (2 * P - 2)
        return pow(Y, exp, MOD)

    if r == 2:
        even = N // 2
        odd = N - even

        per_even = P + 2 * (P - 2)
        per_odd = P + (P - 3)

        exp = even * per_even + odd * per_odd
        return pow(Y, exp, MOD)

    if (P - 1) % 3 != 0:
        exp = N * (2 * P - 2)
    else:
        div3 = N // 3
        other = N - div3

        per_div3 = P + 3 * (P - 2)
        per_other = P + (P - 4)

        exp = div3 * per_div3 + other * per_other

    return pow(Y, exp, MOD)

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        Y, N, r, P = map(int, input().split())
        ans.append(str(solve_query(Y, N, r, P)))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên chuyển đổi sản phẩm thành một phép lũy thừa mô-đun duy nhất. Đây là lý do mã không bao giờ cần lưu trữ bất kỳ chuỗi nào`f(i,r,P)`các giá trị. 

Vì`r=1`, số mũ không đổi với mọi`i`. Vì`r=2`, mã phân tách các vị trí chẵn và lẻ vì thuộc tính bình phương thay đổi chính xác theo tính chẵn lẻ của số mũ. Số lượng chỉ số chẵn trong`1..N`là`N//2`, các chỉ số còn lại là số lẻ. 

Vì`r=3`, trước tiên mã sẽ kiểm tra xem bản đồ khối có phải là song ánh hay không. Nếu như`P-1`không chia hết cho`3`, mọi dư lượng khác 0 đều là lập phương. Ngược lại, chỉ số chia hết cho`3`hành xử khác với các chỉ số khác. 

Tất cả số học được giữ dưới dạng số nguyên. Python xử lý các giá trị trung gian lớn một cách an toàn, nhưng số mũ cuối cùng chỉ cần thiết bằng phép lũy thừa mô-đun, chạy theo thời gian logarit. 

## Ví dụ đã hoạt động 

Sử dụng truy vấn mẫu đầu tiên:```
100000 1234567 1 999999937
```Vì`r=1`, mọi`i`đóng góp số tiền như nhau. 

| Bước | r | Đóng góp cho mỗi tôi | Số của tôi | Phần số mũ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 |`2P-2`|`1234567`|`1234567*(2P-2)`| 

Toàn bộ câu trả lời có được bằng cách nâng cao`100000`theo modulo số mũ này`10^9+7`. 

Đối với truy vấn mẫu thứ hai:```
100000 7654321 2 999999929
```Môđun lẻ nên một nửa số dư khác 0 là số bình phương. 

| Bước | tôi gõ | Đếm | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | Ngay cả tôi |`3827160`|`P+2(P-2)`| 
| 2 | Tôi lẻ |`3827161`|`P+(P-3)`| 

Dấu vết này cho thấy tại sao tính chẵn lẻ là thông tin duy nhất cần thiết cho trường hợp bậc hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log MOD) | Mỗi truy vấn thực hiện một số phép tính số học không đổi và một phép lũy thừa mô-đun. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu tùy thuộc vào kích thước đầu vào được lưu trữ. | 

Giới hạn ban đầu cho phép lên tới`100000`truy vấn, vì vậy ngay cả một`O(P)`phương pháp sẽ thất bại. Công việc liên tục trên mỗi truy vấn giúp giải pháp phù hợp với kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def solve_query(Y, N, r, P):
    if r == 1:
        return pow(Y, N * (2 * P - 2), MOD)

    if r == 2:
        even = N // 2
        odd = N - even
        return pow(Y, even * (3 * P - 4) + odd * (2 * P - 3), MOD)

    if (P - 1) % 3:
        return pow(Y, N * (2 * P - 2), MOD)

    return pow(
        Y,
        (N // 3) * (4 * P - 6) + (N - N // 3) * (2 * P - 4),
        MOD
    )

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        Y, N, r, P = map(int, input().split())
        out.append(str(solve_query(Y, N, r, P)))
    return "\n".join(out)

assert run("""4
100000 1234567 1 999999937
100000 7654321 2 999999929
100000 7777777 3 999999893
100000 7777777 3 999999757
""") == """968518472
236321637
770733917
509589974"""

assert run("""1
5 1 1 3
""") == str(pow(5, 4, MOD))

assert run("""1
7 1 2 5
""") == str(pow(7, 7, MOD))

assert run("""1
10 6 2 11
""") == str(pow(10, 6 * (3 * 11 - 4) // 2 + 3 * (2 * 11 - 3), MOD))

assert run("""1
123 100000000 3 1000000007
""") == str(pow(123, 100000000 * (2 * 1000000007 - 2), MOD))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`r=1`, mô đun nhỏ nhất | giá trị tính toán | Xử lý công thức tính trực tiếp | 
|`r=2`, số nguyên tố nhỏ | giá trị tính toán | Kiểm tra việc xử lý số mũ lẻ và chẵn | 
|`r=2`, phạm vi chẵn lẻ hỗn hợp | giá trị tính toán | Bắt lỗi đếm chẵn lẻ | 
| Lớn`N`| giá trị tính toán | Xác nhận lũy thừa logarit | 

## Vỏ cạnh 

cho`b=0`, thuật toán bao gồm cố định`P`cặp hợp lệ trong mọi giá trị của`f`. Ví dụ, với`P=5`,`r=2`, Và`N=1`, phần đóng góp từ dư lượng đơn này là năm cặp và công thức bao gồm nó trước khi tính phần dư lượng còn lại. 

Vì`b=-1`, không có cặp nào được thêm vào. Sự thay thế`x=b+1`đương nhiên loại bỏ trường hợp này vì nó tương ứng với`x=0`, không thuộc nhóm nhân đang được tính. 

Khi`P-1`chia hết cho`3`, số khối thay đổi. Ví dụ, với`P=7`, chỉ một phần ba số dư lượng khác 0 là lập phương và chỉ số chia hết cho`3`phải chia tay vì`x^3`luôn luôn là một khối lập phương trong khi`x`bản thân nó không nhất thiết phải là một. Thuật toán xử lý việc này bằng cách đếm trực tiếp bội số của ba thay vì giả sử mọi chỉ số đều hoạt động giống nhau. 

Khi`N`rất lớn, lặp từ`1`ĐẾN`N`sẽ là không thể. Giải pháp chỉ tính số lượng chỉ số thỏa mãn từng danh mục, chẳng hạn như chỉ số chẵn hoặc bội số của ba, do đó thời gian chạy vẫn độc lập với`N`.
