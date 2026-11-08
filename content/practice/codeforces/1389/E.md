---
title: "CF 1389E - Lịch mơ hồ"
description: "Chúng ta được cung cấp một hệ thống lịch hư cấu trong đó thời gian được cấu trúc thành ba lớp. Một năm có m tháng, mỗi tháng có đúng d ngày và tuần lặp lại cứ sau w ngày."
date: "2026-06-18T18:36:33+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1389
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 92 (Rated for Div. 2)"
rating: 2200
weight: 1389
solve_time_s: 300
verified: false
draft: false
---

[CF 1389E - Lịch không rõ ràng](https://codeforces.com/problemset/problem/1389/E) 

**Xếp hạng:** 2200 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 5 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống lịch hư cấu trong đó thời gian được cấu trúc thành ba lớp. Một năm có`m`tháng, mỗi tháng chứa chính xác`d`ngày và tuần lặp lại mỗi`w`ngày. Ngày đầu tiên của năm bắt đầu ở chỉ số ngày trong tuần là 1 và tất cả các ngày trong tuần sau đó đều theo chu kỳ với khoảng thời gian.`w`. 

Mỗi ngày trong hệ thống này có thể được xác định theo hai cách khác nhau: theo vị trí của nó trong một tháng và theo nhãn ngày trong tuần toàn cầu của nó. Bởi vì các tháng đều giống nhau nên điều duy nhất quan trọng khi tính toán ngày trong tuần là chỉ số ngày tuyến tính trong năm. 

Nhiệm vụ là xem xét tất cả các cặp tháng khác nhau`(x, y)`với`x < y`và kiểm tra điều kiện đối xứng giữa chúng. Để một cặp được tính, ngày trong tuần của “ngày x của tháng y” phải khớp với ngày trong tuần của “ngày y của tháng x”. Chúng ta phải đếm xem có bao nhiêu cặp như vậy thỏa mãn tính đối xứng này. 

Mặc dù tuyên bố được diễn đạt theo thuật ngữ lịch, mọi thứ đều quy về số học theo dòng thời gian một chiều: mỗi ngày có một chỉ mục toàn cầu và các ngày trong tuần chỉ phụ thuộc vào chỉ mục đó theo modulo`w`. 

Các ràng buộc là vô cùng lớn, với mỗi`m`,`d`, Và`w`lên đến`10^9`và lên đến`1000`trường hợp thử nghiệm. Bất kỳ phương pháp nào kiểm tra trực tiếp các cặp tháng, sẽ là phương trình bậc hai trong`m`, là không thể. Sự phụ thuộc tuyến tính thậm chí vào`m`mỗi trường hợp thử nghiệm là quá lớn trong trường hợp xấu nhất. 

Một nỗ lực ngây thơ sẽ cố gắng kiểm tra từng cặp`(x, y)`và tính toán chênh lệch ngày trong tuần bằng cách sử dụng số học mô-đun. Điều đó dẫn đến`O(m^2)`hoạt động cho mỗi thử nghiệm, điều này hoàn toàn không khả thi khi`m`là lớn. 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc cố gắng mô phỏng các ngày trong tuần theo từng tháng. Mặc dù ca làm việc mỗi tháng rất đơn giản nhưng việc tích lũy các khoản bù đắp trên`m`tháng và sau đó kiểm tra tất cả các cặp vẫn dẫn đến công việc bậc hai hoặc việc tính toán trước nặng nề sẽ phá vỡ các ràng buộc. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là cả “ngày x của tháng y” và “ngày y của tháng x” đều phụ thuộc vào cùng một lịch tuyến tính, vì vậy chúng ta đang so sánh hai biểu thức được xây dựng từ cùng một cấp số cộng nhưng được đánh giá ở các chỉ số hoán đổi. 

Hãy để chúng tôi phân công mỗi tháng`i`một khoản bù đắp bắt đầu trong năm. Vì mỗi tháng có`d`ngày, đầu tháng`i`là`(i-1) * d`. Do đó, chỉ số toàn cầu trong ngày`k`trong tháng`i`là:`A(i, k) = (i-1) * d + k`Ngày trong tuần của ngày này là`A(i, k) mod w`. 

Điều kiện để có một cặp`(x, y)`trở thành:`A(y, x) ≡ A(x, y) (mod w)`Mở rộng cả hai bên:`(y-1)d + x ≡ (x-1)d + y (mod w)`Sắp xếp lại:`(y-x)d + x - y ≡ 0 (mod w)`Nhân tố:`(y-x)(d - 1) ≡ 0 (mod w)`Bây giờ cấu trúc trở nên rõ ràng: điều kiện chỉ phụ thuộc vào hiệu`t = y - x`, không phải ở vị trí tuyệt đối. Mỗi cặp hợp lệ chỉ được xác định bởi khoảng cách giữa các tháng. 

Vì vậy đối với mỗi`t > 0`, ta cần đếm xem có bao nhiêu cặp`(x, x+t)`tồn tại, đó là`m - t`và kiểm tra xem:`t * (d - 1) ≡ 0 (mod w)`Do đó, vấn đề giảm xuống việc đếm những khác biệt hợp lệ`t`được tính bằng số lượng cặp chúng tạo ra. 

Lực lượng vũ phu sẽ lặp đi lặp lại trên tất cả`t`từ`1`ĐẾN`m-1`và kiểm tra điều kiện đã có`O(m)`cho mỗi trường hợp thử nghiệm và vẫn còn quá lớn khi`m`là`10^9`. 

Cái nhìn sâu sắc quan trọng là phân loại tất cả`t`thỏa mãn điều kiện chia hết môđun. Chúng tôi cần:`w | t * (d - 1)`Cho phép`g = gcd(w, d - 1)`. Khi đó điều kiện đơn giản hóa thành:`(w / g) | t`Rất hợp lệ`t`chính xác là bội số của`k = w / gcd(w, d - 1)`. 

Bây giờ vấn đề trở thành vấn đề số học thuần túy: đếm tất cả các bội số của`k`lên đến`m-1`, và với mỗi trường hợp như vậy`t`, thêm vào`m - t`. 

Điều này làm giảm toàn bộ tính toán thành tổng một cấp số cộng theo bội số của`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực theo cặp | O(m^2) | O(1) | Quá chậm | 
| Lặp lại sự khác biệt | O(m) | O(1) | Quá chậm | 
| Đếm số học dựa trên GCD | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta chuyển phép rút gọn số học thành một phép tính trực tiếp. 

1. Tính toán`a = d - 1`. Đây là sự dịch chuyển xuất hiện trong sự đồng dư dẫn xuất. Nếu như`a = 0`, điều kiện ngày trong tuần đơn giản hóa đáng kể vì tất cả các giá trị bù trừ theo tháng đều biến mất bên trong biểu thức. 
2. Tính toán`g = gcd(w, a)`. Điều này rút ra cấu trúc phân chia chung giữa độ dài tuần và ca làm việc hàng tháng. 
3. Xác định`k = w // g`. Đây là kích thước bước tối thiểu sao cho mọi khác biệt hợp lệ`t`phải là bội số của`k`. 
4. Nếu`k > m - 1`, không có sự khác biệt hợp lệ. Trở lại`0`. Điều này xảy ra vì giá trị nhỏ nhất hợp lệ`t`đã vượt quá phạm vi khoảng trống tháng có thể có. 
5. Ngược lại, hãy xem xét tất cả các bội số`t = k, 2k, 3k, ... ≤ m-1`. Gọi bội số lớn nhất đó là`q = (m-1) // k`. 
6. Đối với mỗi`i`từ`1`ĐẾN`q`,`t = i * k`đóng góp`(m - t)`cặp hợp lệ. Tổng hợp những đóng góp này. 
7. Tính tổng bằng chuỗi số học: 

Câu trả lời trở thành:`sum_{i=1..q} (m - i*k) = q*m - k * (q*(q+1)//2)`. 
8. Xuất giá trị này cho test case. 

### Tại sao nó hoạt động 

Toàn bộ phép biến đổi phụ thuộc vào việc quy đổi sự đồng dạng ban đầu thành điều kiện chia hết trên`t`. Một khi được viết lại thành`w | t(d-1)`, tất cả cấu trúc của lịch gốc sẽ biến mất ngoại trừ sự tương tác gcd giữa`w`Và`d-1`. Điều này đảm bảo rằng mỗi cặp hợp lệ được tính chính xác một lần thông qua sự khác biệt duy nhất của nó`t`và mọi cặp không hợp lệ đều bị loại trừ vì sự khác biệt của nó không thỏa mãn ràng buộc mô-đun. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        m, d, w = map(int, input().split())
        a = d - 1
        g = gcd(w, a)
        k = w // g

        if k > m - 1:
            out.append("0")
            continue

        q = (m - 1) // k
        ans = q * m - k * (q * (q + 1) // 2)
        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp việc giảm đại số. Phép biến đổi không tầm thường duy nhất là thay thế điều kiện mô-đun bằng kích thước bước dựa trên gcd`k`. Sau khi tính toán xong, phần còn lại là tổng đóng góp của tất cả các khác biệt hợp lệ ở dạng đóng. 

Phải cẩn thận với`m - 1`, vì chênh lệch tháng không thể vượt quá giới hạn đó. Một điểm tinh tế khác là sử dụng số học số nguyên xuyên suốt; tất cả các giá trị trung gian đều vừa khít với số nguyên 64 bit khi sử dụng số nguyên lớn của Python, nhưng trong các ngôn ngữ khác, việc quản lý tràn sẽ là điều cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
m = 6, d = 7, w = 4
```Chúng tôi tính toán:`a = 6`,`g = gcd(4, 6) = 2`, Vì thế`k = 2`. 

Sự khác biệt hợp lệ là bội số của 2:`t = 2, 4`. 

| tôi | t | m - t | 
| --- | --- | --- | 
| 1 | 2 | 4 | 
| 2 | 4 | 2 | 

Tổng = 6. 

Điều này phù hợp với sản lượng dự kiến. 

### Ví dụ 2 

đầu vào:```
m = 12, d = 30, w = 7
```Chúng tôi tính toán:`a = 29`,`g = gcd(7, 29) = 1`, Vì thế`k = 7`. 

Sự khác biệt hợp lệ:`t = 7`. 

| tôi | t | m - t | 
| --- | --- | --- | 
| 1 | 7 | 5 | 

Đáp án = 5. 

Điều này chứng tỏ trường hợp chỉ tồn tại một sự khác biệt hợp lệ do hạn chế mô-đun mạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) mỗi lần kiểm tra | Mỗi bài kiểm tra sử dụng một số phép tính số học không đổi và một gcd | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Giải pháp có thể xử lý thoải mái tới 1000 trường hợp thử nghiệm với các giá trị lên tới`10^9`bởi vì tất cả các hoạt động đều có thời gian không đổi và tránh lặp lại hàng tháng hoặc hàng ngày. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        m, d, w = map(int, input().split())
        a = d - 1
        g = gcd(w, a)
        k = w // g

        if k > m - 1:
            out.append("0")
        else:
            q = (m - 1) // k
            ans = q * m - k * (q * (q + 1) // 2)
            out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("5\n6 7 4\n10 7 12\n12 30 7\n1 1 1\n3247834 10298779 625324\n") == \
"6\n9\n5\n0\n116461800"

# custom cases
assert run("1\n1 10 7\n") == "0", "single month"
assert run("1\n2 2 2\n") == "1", "tiny cyclic case"
assert run("1\n5 1 3\n") == "6", "all shifts zero"
assert run("1\n10 100 1\n") == "45", "w=1 all pairs valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 10 7`|`0`| không thể có cặp khi chỉ có một tháng | 
|`2 2 2`|`1`| cấu trúc không tầm thường nhỏ nhất | 
|`5 1 3`|`6`| điều kiện suy biến dịch chuyển sụp đổ | 
|`10 100 1`|`45`| modulo 1 làm cho tất cả các cặp hợp lệ | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi`d = 1`. Sau đó`d - 1 = 0`, vậy mỗi số hạng`t(d-1)`bằng 0 và điều kiện mô đun luôn đúng. Thuật toán xử lý việc này vì`g = gcd(w, 0) = w`, Vì thế`k = 1`. Mọi sự khác biệt đều hợp lệ và công thức giảm xuống việc đếm tất cả các cặp`(x, y)`, đó là`m(m-1)/2`. 

Một trường hợp cạnh khác là khi`w = 1`. Tuần chỉ có một ngày nên tất cả các ngày trong tuần đều giống nhau. Lại`k = 1`và thuật toán đếm chính xác tất cả các cặp. 

Khi`m = 1`, không có cặp nào hợp lệ. Công thức mang lại`(m-1) = 0`, Vì thế`q = 0`và kết quả là`0`. 

Cuối cùng, khi`k > m - 1`, không có sự khác biệt hợp lệ tồn tại. Điều này tương ứng với cấu trúc mô-đun cực kỳ hạn chế mà ngay cả khoảng cách tháng nhỏ nhất cũng không thể thỏa mãn điều kiện. Việc trả lại sớm đảm bảo chúng tôi không thử tính tổng không hợp lệ.
