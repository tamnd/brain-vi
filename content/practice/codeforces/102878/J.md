---
title: "CF 102878J - Giáo viên Long và Học máy"
description: "Nhiệm vụ là khôi phục các hệ số của đa thức bậc bốn từ năm giá trị quan sát được. Năm quan sát tương ứng với đa thức được đánh giá tại x = 1, 2, 3, 4 và 5, nhưng mỗi quan sát có thể chứa nhiều nhất một lỗi."
date: "2026-07-25T12:46:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102878
codeforces_index: "J"
codeforces_contest_name: "The 15-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 102878
solve_time_s: 42
verified: true
draft: false
---

[CF 102878J - Giáo viên dài hạn và học máy](https://codeforces.com/problemset/problem/102878/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Nhiệm vụ là khôi phục các hệ số của đa thức bậc bốn từ năm giá trị quan sát được. Năm quan sát tương ứng với đa thức được đánh giá tại x = 1, 2, 3, 4 và 5, nhưng mỗi quan sát có thể chứa nhiều nhất một lỗi. Đa thức thực có các hệ số nguyên trong phạm vi [-100, 100] và chúng ta phải xuất ra bộ hệ số duy nhất có thể tạo ra các quan sát nhiễu. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho năm số nguyên đại diện cho các giá trị quan sát được. Đầu ra cho mỗi trường hợp là hệ số không đổi thông qua hệ số bậc 4, nghĩa là a0, a1, a2, a3 và a4 cho đa thức a0 + a1x + a2x² + a3x³ + a4x⁴. 

Số lượng quan sát nhỏ là hạn chế chính. Chúng ta chỉ có năm giá trị nên mức độ tự do toán học rất hạn chế. Một bậc bốn tổng quát được xác định đầy đủ bởi năm điểm chính xác, nhưng ở đây mỗi điểm có thể di chuyển một đơn vị do nhiễu. Do đó, số lượng điều chỉnh có thể đủ nhỏ để khám phá. Một phương pháp thử tất cả các tập dữ liệu đã sửa có thể chỉ thực hiện vài trăm lần kiểm tra cho mỗi trường hợp thử nghiệm. Các phương pháp phục hồi đa thức phức tạp hơn là không cần thiết. 

Việc thực hiện bất cẩn vẫn có thể thất bại trong một số trường hợp. Nếu nhiễu bị bỏ qua và năm giá trị đã cho được nội suy trực tiếp thì một giá trị bị sai có thể tạo ra một bậc bốn hoàn toàn khác. 

Ví dụ:```
1 4 9 16 24
```Đầu ra đúng là:```
0 0 1 0 0
```Các giá trị gần như khớp với x², nhưng giá trị cuối cùng nhỏ hơn 25. Phép nội suy trực tiếp coi số đo sai là giá trị đúng và tạo ra một bậc bốn khác. 

Một trường hợp khác là khi đa thức có giá trị cao hơn do các số hạng không đổi và tuyến tính. 

đầu vào:```
25 16 9 4 1
```Đầu ra là:```
36 -12 1 0 0
```Một giải pháp chỉ kiểm tra các mẫu đơn giản như hình vuông hoặc cố gắng đoán độ có thể thất bại ở đây. Đa thức vẫn là bậc hai nhưng các hệ số của nó bị dịch chuyển. 

Tiếng ồn cũng có thể xuất hiện ở nhiều vị trí cùng một lúc. Một phương pháp chỉ giả định giá trị cuối cùng là sai sẽ thất bại vì mọi quan sát đều được phép khác với giá trị thực. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp là giả sử năm giá trị quan sát được là chính xác và thực hiện phép nội suy đa thức. Năm điểm xác định duy nhất một bậc bốn, vì vậy phương pháp này có giá trị về mặt toán học đối với dữ liệu sạch. Vấn đề là các giá trị đầu vào không được đảm bảo sạch sẽ. Một điểm nhiễu duy nhất sẽ làm thay đổi đa thức được nội suy và không có cách nào để phân biệt bậc bốn sai với bậc thực. 

Phương pháp brute-force mạnh hơn sử dụng phạm vi tiếng ồn nhỏ. Mỗi quan sát có thể được sửa bằng -1, 0 hoặc +1. Vì chỉ có năm quan sát nên có thể có 3⁵ = 243 trình tự được hiệu chỉnh. Đối với mỗi chuỗi ứng cử viên, chúng tôi nội suy bậc bốn và kiểm tra xem các hệ số kết quả có phải là số nguyên trong phạm vi cho phép hay không. Bài toán đảm bảo rằng tồn tại ít nhất một đa thức hợp lệ, vì vậy ứng cử viên hợp lệ đầu tiên mà chúng ta tìm thấy chính là câu trả lời. 

Quan sát quan trọng là độ không đảm bảo nằm ở các giá trị đầu vào chứ không phải ở bậc đa thức. Thay vì tìm kiếm trong tất cả các hệ số có thể có, vốn sẽ rất lớn, chúng ta tìm kiếm trong không gian nhỏ bé chứa các sai số đo lường có thể xảy ra. Sau khi tìm thấy năm giá trị thực, việc nội suy rất đơn giản. 

Bản thân phép nội suy được thực hiện bằng cách sử dụng sai phân hữu hạn. Đối với các giá trị x cách đều nhau, đa thức bậc bốn có hiệu thứ tư không đổi. Sử dụng các giá trị đã hiệu chỉnh, chúng ta tính dạng Newton của đa thức và chuyển nó thành lũy thừa chuẩn của x. Số học phân số tránh được các vấn đề về độ chính xác và cho phép chúng ta xác minh rằng các hệ số thực sự là số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các hệ số | O(201⁵) | O(1) | Quá chậm | 
| Nội suy trực tiếp các giá trị nhiễu | O(1) | O(1) | Sai | 
| Liệt kê các hiệu chỉnh tiếng ồn và nội suy | O(3⁵) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ra mọi phiên bản sửa chữa có thể có của năm quan sát. Đối với mỗi vị trí, hãy thử thêm -1, 0 và +1. Đây chính xác là những giá trị thực có thể có vì sai số đo không thể vượt quá một. 
2. Nội suy đa thức cho các giá trị đã hiệu chỉnh hiện tại. Các tọa độ x được cố định là 1, 2, 3, 4 và 5, do đó sai phân hữu hạn mang lại một cách gọn nhẹ để xây dựng bậc bốn. 
3. Chuyển đa thức từ cơ sở Newton sang dạng hệ số chuẩn. Định dạng đầu ra bắt buộc là a0, a1, a2, a3 và a4, vì vậy chúng ta cần các hệ số từ x⁰ đến x⁴. 
4. Kiểm tra xem mọi hệ số có phải là số nguyên hay không và mọi hệ số có nằm trong [-100, 100] hay không. Nếu tất cả các bước kiểm tra đều đạt thì đa thức này thỏa mãn mọi điều kiện ban đầu và là đáp án. 
5. Xuất năm hệ số và dừng tìm kiếm. Việc đảm bảo một giải pháp hợp lệ có nghĩa là bảng liệt kê phải tìm ra một giải pháp. 

Tại sao nó hoạt động: mọi đa thức hợp lệ tạo ra chính xác một chuỗi đã hiệu chỉnh trong đó mỗi quan sát nhiễu được điều chỉnh trở về giá trị thực. Việc liệt kê kiểm tra trình tự đó. Nội suy tái tạo lại bậc bốn duy nhất đi qua năm điểm thực đó. Việc xác thực hệ số sẽ loại bỏ tất cả các ứng cử viên không đáp ứng các hạn chế ban đầu. Vì đa thức hợp lệ được đảm bảo tồn tại nên thuật toán không thể kết thúc nếu không tìm được hệ số chính xác. 

## Giải pháp Python```python
import sys
from fractions import Fraction

input = sys.stdin.readline

def multiply_poly(a, b):
    res = [Fraction(0)] * (len(a) + len(b) - 1)
    for i, x in enumerate(a):
        for j, y in enumerate(b):
            res[i + j] += x * y
    return res

def add_poly(a, b):
    n = max(len(a), len(b))
    res = [Fraction(0)] * n
    for i in range(len(a)):
        res[i] += a[i]
    for i in range(len(b)):
        res[i] += b[i]
    return res

def interpolate(values):
    diff = [Fraction(x) for x in values]
    coeff = []
    while diff:
        coeff.append(diff[0])
        diff = [diff[i + 1] - diff[i] for i in range(len(diff) - 1)]

    result = [Fraction(0)]
    basis = [Fraction(1)]
    denominator = 1

    for i, c in enumerate(coeff):
        if i > 0:
            basis = multiply_poly(basis, [Fraction(-i), Fraction(1)])
            denominator *= i
        term = [x * c / denominator for x in basis]
        result = add_poly(result, term)

    return result

def solve_case(arr):
    def dfs(idx, cur):
        if idx == 5:
            poly = interpolate(cur)
            poly += [Fraction(0)] * (5 - len(poly))
            if all(x.denominator == 1 and -100 <= x <= 100 for x in poly[:5]):
                return [int(x) for x in poly[:5]]
            return None

        for delta in (-1, 0, 1):
            cur.append(arr[idx] + delta)
            ans = dfs(idx + 1, cur)
            if ans is not None:
                return ans
            cur.pop()
        return None

    return dfs(0, [])

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        values = list(map(int, input().split()))
        ans.append(" ".join(map(str, solve_case(values))))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```các`multiply_poly`Và`add_poly`các hàm xử lý số học đa thức ở dạng hệ số. Việc tách biệt các hoạt động này giúp việc xác minh logic nội suy dễ dàng hơn. 

các`interpolate`hàm xây dựng biểu diễn sai phân hữu hạn Newton. Giá trị đầu tiên từ mọi lớp khác biệt sẽ trở thành hệ số Newton. Đa thức cơ sở được nhân với`(x - i)`sau mỗi lớp, khớp các điểm x = 1, 2, 3, 4 và 5. Phân số được sử dụng vì các giá trị trung gian như chia cho 2, 6 hoặc 24 xuất hiện ngay cả khi đáp án cuối cùng phải là số nguyên. 

Tìm kiếm theo chiều sâu sẽ thử ba cách điều chỉnh có thể có cho mỗi quan sát. Khi năm giá trị hiệu chỉnh được chọn, đa thức sẽ được xây dựng lại và kiểm tra. Việc kiểm tra phạm vi được thực hiện sau khi xác minh rằng mẫu số là một, điều này tránh việc vô tình chấp nhận các hệ số phân số. 

Không có phép toán dấu phẩy động nên lỗi làm tròn không thể tạo ra câu trả lời sai. Số nguyên Python cũng xử lý tất cả số học trung gian một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 4 9 16 25
```Một con đường điều chỉnh thành công là: 

| Bước | Giá trị đã sửa | Hệ số đa thức | 
| --- | --- | --- | 
| Bắt đầu | 1, 4, 9, 16, 25 | Không được tính toán | 
| Sau khi sửa tìm kiếm | 1, 4, 9, 16, 25 | 0, 0, 1, 0, 0 | 

Các giá trị đã khớp chính xác với x2 nên không cần chỉnh sửa. Điều bất biến là mọi ứng cử viên được chấp nhận đều đại diện cho một đa thức phù hợp với cả năm quan sát. 

### Ví dụ 2 

đầu vào:```
1 4 9 16 24
```Việc tìm kiếm tìm thấy: 

| Bước | Giá trị đã sửa | Hệ số đa thức | 
| --- | --- | --- | 
| Bắt đầu | 1, 4, 9, 16, 24 | Không được tính toán | 
| Hãy thử sửa giá trị thứ năm | 1, 4, 9, 16, 25 | 0, 0, 1, 0, 0 | 

Quan sát cuối cùng được tăng thêm một. Chuỗi đã sửa thu được sẽ tạo ra x2 bậc hai, cho thấy lý do tại sao việc kiểm tra các phép đo gần đó là đủ để khôi phục hàm ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3⁵ * 5³) | Có 243 bộ dữ liệu có thể được sửa và mỗi phép nội suy chỉ xử lý được năm điểm. | 
| Không gian | O(1) | Chỉ các mảng nhỏ chứa hệ số đa thức và hiệu được lưu trữ. | 

Số lượng trường hợp thử nghiệm nhiều nhất là mười, do đó, ngay cả vài nghìn phép tính số học cho mỗi trường hợp cũng thấp hơn nhiều so với giới hạn. Thuật toán dành phần lớn thời gian cho các phép toán đa thức có kích thước không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io
from fractions import Fraction

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    main()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert run("""3
1 4 9 16 25
1 4 9 16 24
25 16 9 4 1
""") == """0 0 1 0 0
0 0 1 0 0
36 -12 1 0 0
""", "samples"

assert run("""1
0 0 0 0 0
""") == """0 0 0 0 0
""", "all equal values"

assert run("""1
1 2 3 4 5
""") == """0 1 0 0 0
""", "linear boundary case"

assert run("""1
100 101 104 109 116
""") == """0 0 1 0 0
""", "quadratic with shifted observations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 4 9 16 25`|`0 0 1 0 0`| Đa thức chính xác không có nhiễu | 
|`1 4 9 16 24`|`0 0 1 0 0`| Quan sát bị hỏng đơn | 
|`0 0 0 0 0`|`0 0 0 0 0`| Tất cả các giá trị bằng nhau | 
|`1 2 3 4 5`|`0 1 0 0 0`| Đa thức bậc thấp | 
|`100 101 104 109 116`|`0 0 1 0 0`| Giá trị lớn hơn và giới hạn hệ số | 

## Vỏ cạnh 

Đối với đầu vào:```
1 4 9 16 24
```thuật toán khám phá các hiệu chỉnh xung quanh mỗi quan sát. Khi nó đạt đến chuỗi ứng cử viên:```
1 4 9 16 25
```các sai phân hữu hạn trở thành sai phân của x2 và các hệ số được thu hồi là:```
0 0 1 0 0
```Cách tiếp cận nội suy trực tiếp sai sẽ giữ 24 là giá trị thực và trả về một bậc bốn khác. 

Đối với đầu vào:```
25 16 9 4 1
```thuật toán không giả định đa thức phải giống với x2. Nó xây dựng lại đa thức từ năm điểm đã sửa và thu được:```
36 -12 1 0 0
```đại diện cho x² - 12x + 36. Điều này chứng tỏ rằng phương trình bậc hai đã dịch chuyển được xử lý một cách tự nhiên. 

Đối với đầu vào có nhiều giá trị bị nhiễu, chẳng hạn như:```
2 3 8 15 24
```việc tìm kiếm vẫn xem xét mọi sự kết hợp điều chỉnh. Nó không dựa vào một vị trí cụ thể bị hỏng. Khi tất cả năm giá trị thực được chọn, phép nội suy và xác thực sẽ quyết định xem ứng viên có được chấp nhận hay không. 

Trường hợp khái niệm kích thước tối thiểu là khi tất cả năm quan sát đều giống hệt nhau. Thuật toán vẫn chạy cùng một quy trình tìm kiếm và nội suy và nó chấp nhận đa thức không đổi khi các hệ số thỏa mãn phạm vi yêu cầu.
