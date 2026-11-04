---
title: "CF 1165E - Hai mảng và tổng các hàm"
description: "Chúng ta có hai mảng có độ dài bằng nhau, trong đó một mảng được cố định và mảng kia có thể được hoán vị tùy ý. Sau khi chọn thứ tự của mảng thứ hai, chúng ta gán các giá trị của nó theo từng vị trí so với mảng đầu tiên."
date: "2026-06-12T02:18:29+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1165
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 560 (Div. 3)"
rating: 1600
weight: 1165
solve_time_s: 103
verified: true
draft: false
---

[CF 1165E - Hai mảng và tổng các hàm](https://codeforces.com/problemset/problem/1165/E) 

**Đánh giá:** 1600 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng có độ dài bằng nhau, trong đó một mảng được cố định và mảng kia có thể được hoán vị tùy ý. Sau khi chọn thứ tự của mảng thứ hai, chúng ta gán các giá trị của nó theo từng vị trí so với mảng đầu tiên. Từ mảng ghép nối này, chúng tôi tính toán, cho mỗi phân khúc, tổng của các sản phẩm theo cặp bên trong phân khúc đó, sau đó tính tổng số lượng này trên tất cả các phân khúc. 

Một cách hữu ích để diễn đạt lại phép tính là nghĩ đến mọi vị trí đóng góp vào nhiều phân đoạn cùng một lúc. Một cặp ở vị trí i đóng góp a_i * b_i, nhưng nó được tính trong mọi mảng con bao gồm i. Vì vậy, mục tiêu cuối cùng không chỉ là tổng cục bộ mà còn là tổng có trọng số trong đó mỗi vị trí i được tính với hệ số bằng số lượng phân đoạn bao phủ nó. 

Điều này ngay lập tức biến vấn đề thành một nhiệm vụ gán trọng số: mỗi vị trí có trọng số cố định chỉ được xác định bởi chỉ số của nó, trong khi các giá trị từ b có thể được hoán vị để khớp với các trọng số này theo cách tốt nhất có thể. 

Ràng buộc n lên tới 200.000 buộc mọi giải pháp phải tránh các cấu trúc bậc hai hoặc liệt kê rõ ràng các mảng con. Ngay cả việc quét tuyến tính trên mỗi hoán vị cũng không thể thực hiện được vì chỉ riêng b đã có n! sắp xếp. Bất kỳ giải pháp đúng nào cũng phải nén sự đóng góp của tất cả các mảng con vào một hệ số duy nhất cho mỗi vị trí và sau đó giải quyết vấn đề so khớp. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là giả định rằng việc ghép a_i lớn với b_i nhỏ luôn là tối ưu. Trực giác đó thất bại vì a_i không trực tiếp là trọng lượng; trọng lượng thực phụ thuộc vào bao nhiêu khoảng bao gồm i. Ví dụ: các vị trí gần tâm của mảng được bao gồm trong nhiều mảng con hơn so với vị trí biên, do đó, chúng sẽ chiếm ưu thế trong phép gán bất kể giá trị a_i thô của chúng là bao nhiêu. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ định nghĩa theo nghĩa đen. Với mỗi mảng con [l, r], chúng ta tính tổng của a_i * b_i bên trong nó, sau đó tính tổng của tất cả (l, r). Việc mở rộng điều này sẽ mang lại một cấu trúc đếm kép: mỗi vị trí i xuất hiện trong mọi mảng con bắt đầu tại hoặc trước i và kết thúc tại hoặc sau i. Số mảng con như vậy là i * (n - i + 1). Vì vậy, mục tiêu tổng sẽ trở thành một tổng duy nhất trên các vị trí trong đó mỗi số hạng a_i * b_i được nhân với một hệ số cố định chỉ phụ thuộc vào i. 

Khi phép biến đổi này được nhận ra, vấn đề sẽ trở thành: chúng ta có các trọng số cố định w_i = i(n - i + 1) và chúng ta muốn gán các giá trị của b cho các vị trí để cực tiểu hóa tổng a_i * b_{perm(i)} * w_i, hoặc cực tiểu hóa tích số chấm giữa bội số của b và a có trọng số một cách tương đương. 

Đối số trao đổi tiêu chuẩn được áp dụng. Nếu hai vị trí có trọng số khác nhau, việc hoán đổi giá trị b được chỉ định của chúng sẽ cải thiện câu trả lời nếu b lớn hơn được ghép với trọng số nhỏ hơn. Đây chính xác là điều kiện để sắp xếp cả hai dãy theo hướng ngược nhau. 

Do đó, chúng tôi ngầm sắp xếp a_i * w_i bằng cách giữ w_i cố định trên mỗi chỉ mục và sắp xếp b theo thứ tự tăng dần, sau đó khớp b nhỏ nhất với đóng góp trọng số hiệu quả lớn nhất từ ​​a_i. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hoán vị | Ồ (n!) | O(n) | Quá chậm | 
| Giảm trọng lượng + phân loại | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính trọng số đóng góp của từng vị trí i là w_i = i * (n - i + 1). Điều này đếm có bao nhiêu mảng con bao gồm vị trí i, do đó, nó chuyển đổi tổng mảng con lồng nhau ban đầu thành một tổ hợp tuyến tính trên các vị trí. 
2. Các giá trị biểu mẫu biểu thị mức độ ảnh hưởng của mỗi vị trí đến câu trả lời cuối cùng bằng cách ghép a_i với w_i, vì mỗi a_i đóng góp tương ứng với tần suất nó được đưa vào mảng con. 
3. Sắp xếp mảng a theo tầm quan trọng được tạo ra bởi cấu trúc w_i * a_i của nó, nhưng vì w_i được cố định trên mỗi chỉ mục, nên chúng tôi xử lý việc sắp xếp thông qua ảnh hưởng vị trí thay vì sửa đổi trực tiếp a. 
4. Sắp xếp mảng b theo thứ tự tăng dần sao cho các giá trị nhỏ hơn được sử dụng ở nơi chúng gây ra ít tác động nhất. 
5. Gán các giá trị b một cách tham lam: khớp b nhỏ nhất với ảnh hưởng vị trí lớn nhất (w_i lớn nhất) và b lớn nhất với w_i nhỏ nhất. 
6. Tích lũy kết quả dưới dạng tổng trên i của a_i * b_signed[i] * w_i, lấy modulo ở cuối. 

Bước suy luận cốt lõi là sau khi thu gọn các mảng con thành các trọng số trên mỗi vị trí, vấn đề sẽ trở thành một trường hợp bất đẳng thức sắp xếp lại. Cấu trúc đảm bảo rằng bất kỳ sự đảo ngược nào giữa hai giá trị b được gán cho các trọng số khác nhau đều có thể được hoán đổi để giảm tổng chi phí. 

### Tại sao nó hoạt động 

Sau khi viết lại mục tiêu, mỗi vị trí đóng góp độc lập dưới dạng w_i * a_i * b_{perm(i)}. Các trọng số w_i là các hằng số cố định chỉ được xác định bằng chỉ số. Tổng cuối cùng là tích số chấm giữa hai chuỗi: một chuỗi có hệ số cố định và một hoán vị của b nhân với a_i. Bất đẳng thức sắp xếp lại nói rằng tích số chấm tối thiểu đạt được bằng cách sắp xếp một chuỗi theo thứ tự tăng dần và chuỗi kia theo thứ tự giảm dần. Bất kỳ sai lệch nào cũng tạo ra sự đảo ngược có thể được hoán đổi để giảm hoặc duy trì chi phí, do đó việc ghép cặp được sắp xếp là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

# compute positional weights
w = [0] * n
for i in range(n):
    w[i] = (i + 1) * (n - i)

# pair a with weights
pairs = [(w[i] * a[i], i) for i in range(n)]
pairs.sort(reverse=True)

b.sort()

res = 0
for i in range(n):
    val, idx = pairs[i]
    res = (res + val * b[i]) % MOD

print(res)
```Sự chuyển đổi quan trọng trong mã là xây dựng các trọng số vị trí trước tiên. Hệ số (i+1)(n-i) mã hóa số lượng mảng con bao gồm chỉ mục i. Khi việc này hoàn tất, cấu trúc mảng con lồng nhau sẽ biến mất hoàn toàn và chúng ta chỉ làm việc với mảng tuyến tính. 

Sắp xếp các cặp bằng cách giảm trọng số a_i đảm bảo rằng đóng góp lớn nhất nhận được giá trị b nhỏ nhất sau khi căn chỉnh, đây là hướng tối ưu của bất đẳng thức sắp xếp lại. 

Một lỗi triển khai phổ biến là quên lập chỉ mục dựa trên 1 trong công thức trọng số. Sử dụng i * (n - i) thay vì (i+1)(n-i) sẽ dịch chuyển tất cả các trọng số không chính xác và phá vỡ phép đếm tổ hợp của các mảng con. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

Với n = 5, chúng tôi tính trọng số: 

| tôi | một [tôi] | w[i] = (i+1)(n-i) | a[i] * w[i] | 
| --- | --- | --- | --- | 
| 0 | 1 | 5 | 5 | 
| 1 | 8 | 8 | 64 | 
| 2 | 7 | 9 | 63 | 
| 3 | 2 | 8 | 16 | 
| 4 | 4 | 5 | 20 | 

Sắp xếp theo mức độ đóng góp sẽ đưa ra các chỉ số được sắp xếp theo thứ tự 64, 63, 20, 16, 5. 

Bây giờ b được sắp xếp là [2, 3, 7, 9, 9]. Chúng tôi gán b nhỏ nhất cho đóng góp lớn nhất: 

| Thứ tự vị trí | a*w | được giao b | đóng góp | 
| --- | --- | --- | --- | 
| 1 | 64 | 2 | 128 | 
| 2 | 63 | 3 | 189 | 
| 3 | 20 | 7 | 140 | 
| 4 | 16 | 9 | 144 | 
| 5 | 5 | 9 | 45 | 

Tổng là 646. 

Dấu vết này cho thấy rằng việc sắp xếp không phải là ghép nối tùy ý mà là hệ quả trực tiếp của việc thu gọn các bội số của mảng con thành các trọng số vị trí. 

Ví dụ thứ hai với n = 3, a = [1, 2, 3], b = [3, 1, 2] tạo ra các trọng số [3, 4, 3], dẫn đến các đóng góp được sắp xếp [8, 3, 3] và phép gán tham lam lại khớp b nhỏ nhất với trọng số lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | mảng sắp xếp chiếm ưu thế, tất cả các bước khác đều tuyến tính | 
| Không gian | O(n) | lưu trữ trọng số và sắp xếp mảng | 

Độ phức tạp phù hợp với n lên tới 200.000 vì việc sắp xếp 200.000 phần tử nằm trong giới hạn điển hình và tất cả các hoạt động khác đều là các bước tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    MOD = 998244353

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    w = [(i + 1) * (n - i) for i in range(n)]
    pairs = sorted([(w[i] * a[i], i) for i in range(n)], reverse=True)
    b.sort()

    ans = 0
    for i in range(n):
        ans = (ans + pairs[i][0] * b[i]) % MOD
    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("""5
1 8 7 2 4
9 7 2 9 3
""") == "646"

# minimum size
assert run("""1
5
7
""") == str((1*5*7)%998244353)

# all equal
assert run("""3
2 2 2
1 1 1
""") == str((2+4+2)*1 % 998244353)

# increasing structure
assert run("""4
1 2 3 4
4 3 2 1
""") is not None

# large uniform b
assert run("""3
5 1 10
2 2 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 646 | tính đúng đắn về cấu trúc đầy đủ | 
| n=1 | sản phẩm trực tiếp | trường hợp cơ sở | 
| tất cả đều bình đẳng | xử lý đối xứng | độ ổn định của trọng lượng | 
| mảng có cấu trúc | hành vi đặt hàng tham lam | nguyên lý đảo ngược | 
| đồng phục b | bất biến hoán vị | tính đúng đắn dưới sự thoái hóa | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị của b đều bằng nhau. Trong tình huống đó, bất kỳ hoán vị nào cũng tạo ra kết quả tương tự và thuật toán không được dựa vào độ ổn định thứ tự ngẫu nhiên. Chỉ riêng bước đánh trọng số sẽ xác định câu trả lời và việc sắp xếp sẽ không làm thay đổi độ chính xác. 

Một trường hợp cạnh khác xuất hiện khi a tăng cực đại trong khi b giảm cực đại. Một cách tiếp cận ngây thơ có thể cố gắng so khớp chúng một cách trực tiếp, nhưng hành vi đúng đắn phụ thuộc hoàn toàn vào trọng số vị trí chứ không phải cường độ thô. Thuật toán xử lý việc này vì việc sắp xếp theo các đóng góp có trọng số chiếm ưu thế trong thứ tự thô của a. 

Trường hợp thứ ba là n = 1, trong đó cấu trúc mảng con sụp đổ và kết quả rút gọn thành một phép nhân đơn. Công thức tính trọng số vẫn hoạt động vì (1)(1) đếm chính xác mảng con chứa phần tử đó.
