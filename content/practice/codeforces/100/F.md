---
title: "CF 100F - Đa thức"
description: "Chúng ta được cho một đa thức đã được phân tích thành thừa số tuyến tính: $$p(x) = (x + a1)(x + a2)dots(x + an)$$ Nhiệm vụ là khai triển tích này và in đa thức ở dạng lũy ​​thừa giảm dần thông thường: $$x^n + b1x^{n-1} + dots + bn$$ Phần khó khăn không phải là bản thân khai triển."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "F"
codeforces_contest_name: "Unknown Language Round 3"
rating: 1800
weight: 100
solve_time_s: 146
verified: true
draft: false
---

[CF 100F - Đa thức](https://codeforces.com/problemset/problem/100/F) 

**Đánh giá:** 1800 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đa thức đã được phân tích thành thừa số tuyến tính:$$p(x) = (x + a_1)(x + a_2)\dots(x + a_n)$$Nhiệm vụ là khai triển tích này và in đa thức ở dạng lũy ​​thừa giảm dần thông thường:$$x^n + b_1x^{n-1} + \dots + b_n$$Phần khó khăn không phải là việc mở rộng. Phần khó khăn là định dạng kết quả chính xác theo yêu cầu. 

Mỗi thuật ngữ phải xuất hiện dưới dạng`C*X^K`, nhưng được rút ngắn bất cứ khi nào có thể. hệ số`1`trước khi các số hạng không cố định phải biến mất, số mũ`1`phải được in như chỉ`X`, số mũ`0`phải bỏ qua`X`hoàn toàn và các số hạng có hệ số`0`tuyệt đối không được xuất hiện. Biển hiệu cũng phải được in gọn, không có khoảng trống thừa. 

Mức độ nhiều nhất là 9, cực kỳ nhỏ. Ngay cả các thuật toán có hành vi hàm mũ cũng có thể tồn tại ở đây, nhưng không có lý do gì để sử dụng chúng. Phương pháp nhân đa thức đơn giản thực hiện trong vài trăm phép tính. 

Các quy tắc định dạng là nơi xảy ra hầu hết các câu trả lời sai. Bản thân đa thức rất dễ tính toán. 

Một trường hợp cạnh tinh tế xuất hiện khi các hệ số trung gian trở thành 0. Ví dụ:```
2
-1
1
```Đa thức là:$$(x-1)(x+1)=x^2-1$$các`x`thuật ngữ biến mất hoàn toàn. Việc thực hiện bất cẩn có thể in`X^2+0*X-1`, không hợp lệ. 

Một trường hợp nguy hiểm khác là hệ số bằng`1`hoặc`-1`. Coi như:```
1
1
```Đa thức là:$$x+1$$Đầu ra đúng là:```
X+1
```In ấn`1*X+1`quá dài dòng và bị từ chối. 

Định dạng số mũ cũng có vấn đề. Ví dụ:```
2
0
0
```mang lại:$$x^2$$Đầu ra phải là:```
X^2
```không`X^2+0*X+0`. 

Đa thức không đổi cũng cần xử lý đặc biệt. Ví dụ:```
1
-3
```sản xuất:$$x-3$$Thuật ngữ cố định không có`X`, trong khi số hạng tuyến tính không có số mũ. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là nhân từng thừa số một theo đúng nghĩa đen trong khi lưu trữ mọi hệ số một cách rõ ràng. 

Giả sử hiện tại chúng ta biết các hệ số của:$$(x+a_1)(x+a_2)\dots(x+a_k)$$Sau đó nhân với`(x + a_{k+1})`tạo ra đa thức tiếp theo. Mỗi hệ số cũ góp phần tạo thành hai hệ số mới, một hệ số được dịch chuyển theo độ do nhân với`x`và một không thay đổi vì nhân với`a_{k+1}`. 

Điều này hiệu quả vì phép nhân đa thức phân phối tự nhiên trên các số hạng. 

Một giải pháp bạo lực thực sự ngây thơ sẽ liệt kê mọi tập hợp con các yếu tố góp phần vào mọi mức độ. Vì hệ số của$x^{n-k}$là tổng của tất cả các sản phẩm của`k`đã chọn`a_i`, cách tiếp cận đó cần kiểm tra tất cả các tập hợp con. Với`n ≤ 9`, thậm chí$2^9 = 512$các tập hợp con có thể chấp nhận được, nhưng việc triển khai trở nên phức tạp không cần thiết. 

Phương pháp nhân tăng dần rõ ràng hơn và có quy mô tốt hơn nhiều về mặt khái niệm. Mỗi phép nhân chỉ chạm đến bậc đa thức hiện tại, do đó độ phức tạp trở thành bậc hai. 

Quan sát quan trọng là việc nhân với đa thức tuyến tính chỉ làm thay đổi các hệ số lân cận. Nếu như:$$P(x)=c_0+c_1x+\dots+c_dx^d$$sau đó:$$P(x)(x+a)= ac_0+(c_0+ac_1)x+\dots+c_dx^{d+1}$$Sự chuyển đổi cục bộ này làm cho việc xây dựng đa thức động trở nên tự nhiên. 

Sau khi tính toán các hệ số, thách thức còn lại là định dạng xác định. Vì câu lệnh đảm bảo một đầu ra hợp lệ duy nhất nên chúng ta phải tuân theo chính xác các quy tắc biểu diễn ngắn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê tập hợp con | O(2^n \cdot n) | O(2^n) | Đã chấp nhận | 
| Phép nhân đa thức lũy tiến | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các giá trị`a_i`. 
2. Bắt đầu với đa thức không đổi`1`. 

Ta lưu trữ các hệ số theo thứ tự tăng dần. Ban đầu:$$p(x)=1$$vậy mảng hệ số là:```
[1]
```3. Đối với mọi yếu tố`(x + a)`tạo một mảng hệ số mới lớn hơn một độ. 

Nếu hệ số hiện tại của$x^i$là`c`, thì: 

nhân với`a`đóng góp`c * a`đến mức độ`i`. 

nhân với`x`đóng góp`c`đến mức độ`i+1`. 
4. Thay đa thức cũ bằng đa thức mới sau khi xử lý từng thừa số. 

Sau khi tất cả các thừa số được xử lý, mảng chứa mọi hệ số của đa thức mở rộng. 
5. Duyệt các hệ số từ bậc cao nhất đến bậc thấp nhất và xây dựng chuỗi câu trả lời. 
6. Bỏ qua các hệ số bằng 0. 

Thuật ngữ có hệ số`0`tuyệt đối không được xuất hiện. 
7. Xử lý biển báo cẩn thận. 

Thuật ngữ in đầu tiên không được bắt đầu bằng`+`. 

Các thuật ngữ phủ định nên bắt đầu bằng`-`. 
8. Định dạng từng thuật ngữ dưới dạng pháp lý ngắn gọn nhất. 

Đối với bằng cấp`0`, chỉ in hệ số. 

Đối với bằng cấp`1`, in`X`thay vì`X^1`. 

Đối với hệ số`1`Và`-1`với các thuật ngữ không cố định, bỏ phần số. 
9. In chuỗi cuối cùng. 

### Tại sao nó hoạt động 

Sau khi xử lý lần đầu`k`các yếu tố, mảng hệ số thể hiện chính xác:$$(x+a_1)(x+a_2)\dots(x+a_k)$$Bất biến này ban đầu đúng vì đa thức là`1`. 

Khi nhân với`(x+a_{k+1})`, mọi số hạng đều đóng góp chính xác vào đa thức mới thông qua tính phân phối. Nhân với`a_{k+1}`bảo toàn bậc trong khi nhân với`x`tăng độ lên một. Vì mỗi đóng góp được thêm vào đúng một lần nên các hệ số thu được là chính xác. 

Giai đoạn định dạng là chính xác vì mỗi hệ số được dịch theo quy tắc dạng ngắn nhất của câu lệnh và các số hạng 0 bị bỏ qua hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def format_term(coef, deg, first):
    if coef == 0:
        return ""

    sign = ""
    if coef < 0:
        sign = "-"
    elif not first:
        sign = "+"

    val = abs(coef)

    if deg == 0:
        body = str(val)
    else:
        if val == 1:
            coef_part = ""
        else:
            coef_part = str(val) + "*"

        if deg == 1:
            body = coef_part + "X"
        else:
            body = coef_part + f"X^{deg}"

    return sign + body

def solve():
    n = int(input())
    a = [int(input()) for _ in range(n)]

    # coefficients by increasing degree
    poly = [1]

    for x in a:
        nxt = [0] * (len(poly) + 1)

        for deg, coef in enumerate(poly):
            nxt[deg] += coef * x
            nxt[deg + 1] += coef

        poly = nxt

    parts = []
    first = True

    for deg in range(n, -1, -1):
        term = format_term(poly[deg], deg, first)

        if term:
            parts.append(term)
            first = False

    print("".join(parts))

solve()
```Các hệ số đa thức được lưu theo thứ tự tăng dần vì nó đơn giản hóa việc chuyển đổi trong quá trình nhân. Khi xử lý một yếu tố`(x + a)`, mỗi hệ số hiện tại đóng góp vào đúng hai vị trí trong mảng tiếp theo. 

Bản cập nhật:```
nxt[deg] += coef * x
nxt[deg + 1] += coef
```đến trực tiếp từ phép nhân phân phối. 

Chức năng định dạng cô lập tất cả các quy tắc đầu ra ở một nơi. Điều này tránh được những trường hợp đặc biệt rải rác và làm cho việc lập luận về tính chính xác trở nên dễ dàng hơn. 

Chi tiết dễ mắc lỗi nhất là xử lý hệ số`1`Và`-1`. Ví dụ:```
1*X^2
```không hợp lệ vì cách biểu diễn ngắn nhất phải bỏ qua`1`. 

Một điểm tinh tế khác là xử lý dấu hiệu. Thuật ngữ khẳng định đầu tiên không được bắt đầu bằng`+`, trong khi mọi số hạng dương sau đó thì phải. 

Vòng lặp in độ từ lớn nhất đến nhỏ nhất để đa thức xuất hiện ở dạng chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
-1
1
```Chúng tôi tính toán:$$(x-1)(x+1)$$| Bước | Yếu tố | Hệ số đa thức | 
| --- | --- | --- | 
| Bắt đầu | - |`[1]`| 
| Sau đó`x-1`|`-1`|`[-1, 1]`| 
| Sau đó`x+1`|`1`|`[-1, 0, 1]`| 

Mảng hệ số có nghĩa là:$$-1 + 0x + 1x^2$$các`x`số hạng biến mất vì hệ số của nó bằng 0. 

Đầu ra cuối cùng:```
X^2-1
```Dấu vết này chứng tỏ tại sao việc bỏ qua hệ số 0 là bắt buộc. 

### Ví dụ 2 

đầu vào:```
3
1
1
1
```Chúng tôi tính toán:$$(x+1)^3$$| Bước | Yếu tố | Hệ số đa thức | 
| --- | --- | --- | 
| Bắt đầu | - |`[1]`| 
| Sau lần đầu tiên |`1`|`[1, 1]`| 
| Sau giây |`1`|`[1, 2, 1]`| 
| Sau thứ ba |`1`|`[1, 3, 3, 1]`| 

Các hệ số tương ứng với:$$1 + 3x + 3x^2 + x^3$$In từ mức độ cao nhất trở xuống mang lại:```
X^3+3*X^2+3*X+1
```Ví dụ này xác nhận rằng các hệ số được tích lũy chính xác qua các phép nhân lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi phép nhân chạm vào tất cả các hệ số hiện tại | 
| Không gian | O(n) | Chỉ đa thức hiện tại được lưu trữ | 

Với`n ≤ 9`, thuật toán chỉ thực hiện vài chục phép tính số học. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def format_term(coef, deg, first):
        if coef == 0:
            return ""

        sign = ""
        if coef < 0:
            sign = "-"
        elif not first:
            sign = "+"

        val = abs(coef)

        if deg == 0:
            body = str(val)
        else:
            if val == 1:
                coef_part = ""
            else:
                coef_part = str(val) + "*"

            if deg == 1:
                body = coef_part + "X"
            else:
                body = coef_part + f"X^{deg}"

        return sign + body

    n = int(input())
    a = [int(input()) for _ in range(n)]

    poly = [1]

    for x in a:
        nxt = [0] * (len(poly) + 1)

        for deg, coef in enumerate(poly):
            nxt[deg] += coef * x
            nxt[deg + 1] += coef

        poly = nxt

    parts = []
    first = True

    for deg in range(n, -1, -1):
        term = format_term(poly[deg], deg, first)

        if term:
            parts.append(term)
            first = False

    return "".join(parts)

# provided sample
assert run("2\n-1\n1\n") == "X^2-1", "sample 1"

# minimum size
assert run("1\n1\n") == "X+1", "single factor positive"

# zero coefficients in middle
assert run("2\n0\n0\n") == "X^2", "middle and constant zero"

# repeated values
assert run("3\n1\n1\n1\n") == "X^3+3*X^2+3*X+1", "binomial expansion"

# negative coefficient formatting
assert run("1\n-3\n") == "X-3", "constant negative"

# maximum degree style case
assert run("9\n0\n0\n0\n0\n0\n0\n0\n0\n0\n") == "X^9", "highest degree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`X+1`| Hệ số bỏ qua`1`| 
|`2 0 0`|`X^2`| Loại bỏ các điều khoản bằng 0 | 
|`3 1 1 1`|`X^3+3*X^2+3*X+1`| Tích lũy hệ số đúng | 
|`1 -3`|`X-3`| Định dạng liên tục | 
| Chín số không |`X^9`| Xử lý mức độ tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
2
-1
1
```Đa thức trở thành:$$(x-1)(x+1)=x^2-1$$Trong quá trình nhân mảng hệ số trở thành:```
[-1, 0, 1]
```Thuật toán bỏ qua số hạng bậc 1 vì hệ số của nó bằng 0. Đầu ra cuối cùng là:```
X^2-1
```Điều này tránh được hình thức không hợp lệ`X^2+0*X-1`. 

Bây giờ hãy xem xét:```
1
1
```Mảng hệ số là:```
[1, 1]
```Khi định dạng thuật ngữ bậc 1, thuật toán phát hiện hệ số`1`và loại bỏ tiền tố số. Đầu ra trở thành:```
X+1
```thay vì`1*X+1`. 

Cuối cùng, hãy xem xét:```
2
0
0
```Đa thức là:$$x^2$$Mảng hệ số là:```
[0, 0, 1]
```Cả hai thuật ngữ mức độ thấp hơn đều được bỏ qua. Thuật toán chỉ in:```
X^2
```đó là sự đại diện ngắn nhất được yêu cầu.
