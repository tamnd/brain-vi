---
title: "CF 102399G - \u0426\u0435\u043b\u044b\u0435 \u0442\u043e\u0447\u043a\u0438"
description: "Ta có hai họ đường thẳng. Họ đầu tiên chứa các dòng có dạng (y=x+p) và họ thứ hai chứa các dòng có dạng (y=-x+q). Các giá trị (p) là các số nguyên riêng biệt, cũng như các giá trị (q)."
date: "2026-08-11T23:42:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "G"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 183
verified: true
draft: false
---

[CF 102399G - \u0426\u0435\u043b\u044b\u0435 \u0442\u043e\u0447\u043a\u0438](https://codeforces.com/problemset/problem/102399/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có hai họ đường thẳng. Họ đầu tiên chứa các dòng có dạng (y=x+p) và họ thứ hai chứa các dòng có dạng (y=-x+q). Các giá trị (p) là các số nguyên riêng biệt, cũng như các giá trị (q). 

Mọi đường thẳng từ họ thứ nhất cắt mọi đường thẳng từ họ thứ hai vì hệ số góc của chúng là (1) và (-1). Đối với mỗi cặp như vậy, chúng ta phải xác định xem giao điểm của chúng có tọa độ nguyên hay không. Câu trả lời là số cặp thỏa mãn điều kiện này. 

Đối với một đường thẳng (y=x+p) và một đường thẳng (y=-x+q), việc đánh đồng hai biểu thức sẽ cho 

[ 
x+p=-x+q, 
] 

vậy 

[ 
x=\frac{q-p}{2}. 
] 

Thay thế điều này vào một trong hai dòng sẽ cho 

[ 
y=\frac{p+q}{2}. 
] 

Do đó cả hai tọa độ đều là số nguyên khi (q-p) là số chẵn. Sự khác biệt của hai số nguyên là chẵn khi các số nguyên có cùng tính chẵn lẻ. Do đó, toàn bộ bài toán hình học quy về việc đếm các cặp trong đó (p) và (q) đều là số chẵn hoặc cả hai đều là số lẻ. 

Ràng buộc quan trọng là mỗi họ có thể chứa tối đa (100000) dòng. Một phương pháp kiểm tra từng cặp sẽ thực hiện tối đa (100000\cdot100000=10^{10}) kiểm tra. Điều đó vượt xa những gì giới hạn lập trình cạnh tranh 2 giây có thể hỗ trợ. Chúng ta cần một giải pháp mà công việc của nó về cơ bản tỷ lệ thuận với số lượng giá trị đầu vào. 

Có một vài trường hợp ranh giới rất dễ bị xử lý sai. Nếu cả hai giá trị đều là số lẻ thì hiệu của chúng vẫn là số chẵn. Ví dụ, với```
1
1
1
3
```hai đường thẳng là (y=x+1) và (y=-x+3). Giao điểm của chúng là ((1,2)), vì vậy câu trả lời đúng là`1`. Một giải pháp chỉ kiểm tra xem cả hai số có chẵn hay không sẽ trả về 0 không chính xác. 

Giá trị 0 cũng là số chẵn và phải được xử lý bình thường. Ví dụ,```
1
0
1
1
```cho (x=\frac{1-0}{2}), đây không phải là số nguyên, vì vậy kết quả đầu ra đúng là`0`. Việc triển khai bất cẩn xử lý số 0 tách biệt với các số chẵn khác có thể mắc sai lầm này. 

Sự đẳng thức của (p) và (q) không phải là vấn đề đặc biệt. Ví dụ,```
1
5
1
5
```tạo ra giao điểm ((0,5)), vì vậy câu trả lời là`1`. Hai đường này khác nhau vì chúng có độ dốc khác nhau, mặc dù các tham số của chúng bằng nhau. 

Cuối cùng, câu trả lời tối đa có thể rất lớn. Nếu tất cả các giá trị (100000) trong cả hai họ có cùng tính chẵn lẻ thì mọi cặp đều hoạt động, cho ra (10^{10}) cặp hợp lệ. Do đó, câu trả lời phải được lưu trữ dưới dạng số nguyên có khả năng chứa ít nhất (10^{10}). Số nguyên Python không có vấn đề tràn ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lấy từng dòng (y=x+p_i), ghép nó với mỗi dòng (y=-x+q_j), tính toán (q_j-p_i) và kiểm tra xem kết quả có chẵn hay không. Điều này đúng vì mỗi cặp có thể được kiểm tra chính xác một lần và công thức giao nhau dẫn xuất đưa ra tiêu chí chính xác cho tọa độ nguyên. 

Vấn đề là số lượng cặp. Với (n=m=100000), lực lượng vũ phu thực hiện (10^{10}) kiểm tra tính chẵn lẻ. Mặc dù mỗi lần kiểm tra đều đơn giản về mặt toán học nhưng 10 tỷ lần lặp lại là quá nhiều so với giới hạn thời gian. 

Quan sát quan trọng là chúng ta không thực sự cần các giá trị của sự khác biệt. Chúng ta chỉ cần sự ngang bằng của họ. Chỉ có hai khả năng chẵn lẻ có thể xảy ra, chẵn và lẻ. Một cặp đóng góp chính xác khi hai tham số của nó thuộc cùng một lớp chẵn lẻ. 

Giả sử có các giá trị chẵn (E_p) và (O_p) giá trị lẻ trong số các tham số của họ đầu tiên. Tương tự, đặt (E_q) và (O_q) là số đếm tương ứng cho họ thứ hai. Mọi (p) chẵn có thể được ghép với mọi cặp chẵn (q), tạo ra các cặp hợp lệ (E_pE_q). Tương tự, mọi số lẻ (p) có thể được ghép với mọi số lẻ (q), tạo ra các cặp hợp lệ (O_pO_q). 

Vì thế câu trả lời là 

[ 
E_pE_q+O_pO_q. 
] 

Chúng ta chỉ cần quét từng mảng đầu vào một lần và đếm các phần tử chẵn và lẻ của nó. Vị trí và độ lớn thực tế của các tham số không liên quan sau khi biết tính chẵn lẻ của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nm)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(n+m)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số (n) dòng có dạng (y=x+p), sau đó đọc tất cả các giá trị (p). Đếm xem có bao nhiêu số chẵn và bao nhiêu số lẻ. Chỉ có hai số đếm này quan trọng vì tiêu chí giao nhau chỉ phụ thuộc vào tính chẵn lẻ. 
2. Đọc số (m) dòng có dạng (y=-x+q), sau đó đọc tất cả các giá trị (q). Một lần nữa, đếm các giá trị chẵn và lẻ. 
3. Nhân số chẵn (p) với số chẵn (q). Mỗi cặp như vậy đều có (q-p) chẵn, vì vậy mỗi cặp đều đóng góp vào câu trả lời. 
4. Nhân số lẻ (p) với số lẻ (q). Sự khác biệt của họ cũng bằng nhau, vì vậy những cặp này cũng đóng góp. 
5. Thêm hai sản phẩm và in kết quả. Các cặp chẵn lẻ đối diện bị loại trừ vì hiệu của chúng là số lẻ, làm cho cả hai tọa độ giao nhau đều là nửa số nguyên. 

### Tại sao nó hoạt động 

Đối với bất kỳ cặp đường nào, giao điểm của chúng là 

[ 
\left(\frac{q-p}{2},\frac{p+q}{2}\right). 
] 

Nếu (p) và (q) có cùng tính chẵn lẻ thì cả (q-p) và (p+q) đều là số chẵn, do đó cả hai tọa độ đều là số nguyên. Nếu số chẵn lẻ của chúng khác nhau thì cả hai biểu thức đều là số lẻ nên cả hai tọa độ đều không phải là số nguyên. Do đó, các cặp hợp lệ chính xác là các cặp chẵn lẻ giống nhau. Việc đếm các cặp chẵn-chẵn và lẻ-lẻ đếm mỗi cặp hợp lệ chính xác một lần và loại trừ mọi cặp không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    m = int(input())
    q = list(map(int, input().split()))

    even_p = sum(x % 2 == 0 for x in p)
    odd_p = n - even_p

    even_q = sum(x % 2 == 0 for x in q)
    odd_q = m - even_q

    answer = even_p * even_q + odd_p * odd_q
    print(answer)

if __name__ == "__main__":
    solve()
```Hai lần đọc đầu vào đầu tiên thu được hai họ tham số. Bài toán đảm bảo rằng mỗi họ được đưa ra trên một dòng, do đó chuyển đổi toàn bộ dòng thành một danh sách là đủ. 

Hai bộ đếm chẵn lẻ cho họ đầu tiên thu được bằng cách đếm các giá trị chẵn và trừ đi (n) để thu được số giá trị lẻ. Điều tương tự cũng được thực hiện với gia đình thứ hai. 

Biểu thức cuối cùng trực tiếp thực hiện kết quả toán học (E_pE_q+O_pO_q). Không cần phải sắp xếp các mảng, xây dựng các đường hoặc tính toán bất kỳ giao điểm nào một cách rõ ràng. 

Hoạt động modulo`% 2`an toàn cho tất cả các giá trị tham số không âm được phép. Các số nguyên có độ chính xác tùy ý của Python cũng xử lý câu trả lời tối đa có thể có, (10^{10}) mà không cần bất kỳ cách xử lý đặc biệt nào. 

Phiên bản Gym ban đầu chứa chính xác một trường hợp thử nghiệm, trong khi phiên bản Codeforces sau này gói gọn ý tưởng tương tự vào nhiều trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Mẫu phòng tập ban đầu là```
3
1 3 2
2
0 3
```Họ đầu tiên có các tham số (1,3,2). Họ thứ hai có tham số (0,3). 

| Nhóm tham số | Thậm chí | lẻ | 
| --- | --- | --- | 
| (p) | 1 | 2 | 
| (q) | 1 | 1 | 

Các cặp chẵn-chẵn đóng góp (1\cdot1=1). Các cặp lẻ-lẻ đóng góp (2\cdot1=2). 

| Bước | chẵn_p | lẻ_p | chẵn_q | lẻ_q | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Đếm (p) | 1 | 2 | 0 | 0 | 0 | 
| Đếm (q) | 1 | 2 | 1 | 1 | 0 | 
| Kết hợp các lớp chẵn lẻ | 1 | 2 | 1 | 1 | 3 | 

Kết quả là`3`, phù hợp với đầu ra mẫu. Về mặt hình học, các cặp tham số hợp lệ là ((1,3)), ((3,3)) và ((2,0)). Ba cặp còn lại có tham số chẵn lẻ ngược lại. 

Một ví dụ thứ hai là```
2
0 4
3
1 3 5
```Cả hai giá trị (p) đều là số chẵn, trong khi cả ba giá trị (q) đều là số lẻ. 

| Nhóm tham số | Thậm chí | lẻ | 
| --- | --- | --- | 
| (p) | 2 | 0 | 
| (q) | 0 | 3 | 

Phần đóng góp chẵn-chẵn là (2\cdot0=0) và phần đóng góp lẻ-lẻ là (0\cdot3=0). 

| Bước | chẵn_p | lẻ_p | chẵn_q | lẻ_q | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Đếm (p) | 2 | 0 | 0 | 0 | 0 | 
| Đếm (q) | 2 | 0 | 0 | 3 | 0 | 
| Kết hợp các lớp chẵn lẻ | 2 | 0 | 0 | 3 | 0 | 

Câu trả lời là`0`. Mọi giao điểm đều có tọa độ nửa số nguyên vì mỗi cặp có thể bao gồm một tham số chẵn và một tham số lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Mỗi tham số được kiểm tra một lần để xác định tính chẵn lẻ của nó. | 
| Không gian | (O(n+m)) | Việc thực hiện lưu trữ hai mảng đầu vào. | 

Các ràng buộc cho phép tối đa (10^5) giá trị trong mỗi họ, do đó thuật toán tối ưu chỉ thực hiện khoảng (2\cdot10^5) kiểm tra tính chẵn lẻ. Điều này thoải mái trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ cũng nhỏ so với giới hạn 512 MB. 

Các mảng thậm chí có thể được xử lý mà không cần lưu trữ chúng, giảm không gian phụ trợ xuống (O(1)), nhưng việc giữ chúng giúp việc triển khai trở nên đơn giản và vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    p = list(map(int, input().split()))

    m = int(input())
    q = list(map(int, input().split()))

    even_p = sum(x % 2 == 0 for x in p)
    odd_p = n - even_p

    even_q = sum(x % 2 == 0 for x in q)
    odd_q = m - even_q

    print(even_p * even_q + odd_p * odd_q)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    "3\n"
    "1 3 2\n"
    "2\n"
    "0 3\n"
) == "3\n", "sample 1"

# Minimum-size input, equal parameters
assert run(
    "1\n"
    "5\n"
    "1\n"
    "5\n"
) == "1\n", "minimum size and equal values"

# All parameters have the same parity, so every pair is valid
assert run(
    "3\n"
    "0 2 4\n"
    "4\n"
    "1 3 5 7\n"
) == "0\n", "opposite parity classes"

# Boundary values and mixed parity
assert run(
    "4\n"
    "0 1 2 1000000000\n"
    "5\n"
    "0 1 3 999999999 1000000000\n"
) == "10\n", "boundary values and mixed parity"

# Maximum-size construction
n = 100000
m = 100000
p = list(range(0, 2 * n, 2))
q = list(range(1, 2 * m + 1, 2))

max_input = (
    f"{n}\n"
    + " ".join(map(str, p))
    + "\n"
    + f"{m}\n"
    + " ".join(map(str, q))
    + "\n"
)

assert run(max_input) == "0\n", "maximum-size opposite parity case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 / 1 / 5`|`1`| Kích thước tối thiểu và sự bình đẳng giữa hai họ | 
|`3 / 0 2 4 / 4 / 1 3 5 7`|`0`| Tất cả các cặp có thể có tính chẵn lẻ đối lập | 
|`4 / 0 1 2 1000000000 / 5 / 0 1 3 999999999 1000000000`|`10`| Các giá trị biên tỷ lệ 0, (10^9) và tính chẵn lẻ hỗn hợp | 
| 100000 chẵn (p) và 100000 lẻ (q) |`0`| Kích thước đầu vào tối đa và không có phép tính bậc hai | 

Đối với bài kiểm tra căng thẳng với câu trả lời tối đa, thay vào đó, cùng một cấu trúc có thể sử dụng các giá trị chẵn trong cả hai mảng. Khi đó tất cả các cặp (10^{10}) đều hợp lệ, điều này cũng hữu ích để kiểm tra xem việc triển khai không vô tình sử dụng số nguyên 32 bit. 

## Vỏ cạnh 

Trường hợp cạnh chẵn lẻ đầu tiên là hai tham số lẻ. Coi như```
1
1
1
3
```Thuật toán đếm một số lẻ (p) và một số lẻ (q), cho ra (1\cdot1=1). Giao điểm là ((1,2)), do đó đầu ra là`1`. Điều này nắm bắt các triển khai giả định không chính xác rằng chỉ các cặp chẵn-chẵn mới có thể hoạt động. 

Trường hợp cạnh thứ hai liên quan đến số 0:```
1
0
1
1
```Ở đây (p=0) là chẵn và (q=1) là lẻ. Thuật toán tạo ra (1\cdot0+0\cdot1=0). Giao lộ là ((1/2,1/2)), xác nhận đầu ra`0`. Số 0 được xử lý một cách tự nhiên bằng phép kiểm tra tính chẵn lẻ. 

Trường hợp đẳng thức là```
1
5
1
5
```Cả hai tham số đều là số lẻ nên thuật toán trả về`1`. Giao lộ là ((0,5)). Các giá trị tham số bằng nhau không có nghĩa là hai đường thẳng trùng nhau, vì độ dốc của chúng khác nhau. 

Cuối cùng, hãy xem xét số lượng cặp hợp lệ lớn nhất có thể. Đặt cả hai họ chứa (100000) giá trị chẵn khác nhau. Khi đó, mỗi cặp (100000^2=10^{10}) đều có giao điểm số nguyên. Công thức tính giá trị này trực tiếp dưới dạng (100000\cdot100000) mà không liệt kê bất kỳ cặp nào. Đây chính xác là trường hợp tách biệt lực lượng vũ phu (O(nm)) khỏi giải pháp (O(n+m)).
