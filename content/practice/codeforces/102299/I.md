---
title: "CF 102299I - Sòng bạc Sobytiynyy Proyekt"
description: "Chúng ta có (N) cặp ((pi,fi)). Khi đạt được một cặp, người chơi nhận được (pi) rúp và có thể dừng ngay lập tức, trả (fi) rúp. Nếu (fi) âm, trả tiền có nghĩa là nhận thêm tiền. Nếu người chơi đến cặp cuối cùng, việc dừng lại ở đó là bắt buộc."
date: "2026-08-13T08:19:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "I"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 109
verified: true
draft: false
---

[CF 102299I - Sòng bạc Sobytiynyy Proyekt](https://codeforces.com/problemset/problem/102299/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (N) cặp ((p_i,f_i)). Khi đạt được một cặp, người chơi nhận được (p_i) rúp và có thể dừng ngay lập tức, trả (f_i) rúp. Nếu (f_i) âm, trả tiền có nghĩa là nhận thêm tiền. Nếu người chơi đến cặp cuối cùng, việc dừng lại ở đó là bắt buộc. 

Sòng bạc chọn thứ tự của tất cả các cặp trước khi trò chơi bắt đầu. Người chơi sau đó chọn điểm dừng tốt nhất có thể. Nhiệm vụ của chúng ta là chọn thứ tự sao cho lợi ích cuối cùng tối ưu của người chơi càng nhỏ càng tốt và tạo ra lợi ích tối thiểu đó. 

Đối với thứ tự cố định, giả sử cặp (i) đầu tiên đã được chơi. Nếu người chơi dừng lại chính xác ở đó, lợi ích của cô ấy là 

[ 
p_1+p_2+\cdots+p_i-f_i. 
] 

Người chơi có thể chọn bất kỳ vị trí dừng nào, kể cả vị trí cuối cùng. Do đó, đối với một hoán vị cố định, mức tăng tối ưu của người chơi là 

[ 
\max_{1\le i\le N}\left(\sum_{j=1}^{i}p_j-f_i\right). 
] 

Vì vậy, bài toán trở thành bài toán thứ tự thuần túy: sắp xếp các cặp sao cho tối đa các biểu thức tiền tố này là nhỏ nhất. 

Với (N) đến (10^5), việc liệt kê các hoán vị là không thể. Ngay cả thuật toán bậc hai cũng đã thực hiện được khoảng (10^{10}) phép tính trong trường hợp lớn nhất, vượt xa thời gian có sẵn. Chúng ta cần một giải pháp dựa trên việc sắp xếp, mang lại (O(N\log N)). 

Có một số trường hợp khó khăn dễ gây ra giải pháp sai. Một phủ định (f_i) phải được xử lý theo nghĩa đen. Ví dụ,```
1
1 -2
```Người chơi nhận được (1) rồi lại nhận được (2) nên đáp án là (3), không phải (1) hay (-1). Một giải pháp giả định phí luôn không âm sẽ mắc sai lầm này. 

Cặp cuối cùng cũng đặc biệt trong phần mô tả trò chơi nhưng không cần xử lý riêng trong công thức. Ví dụ,```
1
0 1
```buộc người chơi phải trả tiền (1) nên đáp án là (-1). Biểu thức (0-1=-1) đã thể hiện được điều này. Việc coi cặp cuối cùng như thể người chơi có thể đơn giản bỏ qua khoản phí của nó sẽ tạo ra câu trả lời sai. 

Cho phép giá trị 0 của (p_i). Vì```
2
0 1
3 4
```thứ tự hiển thị đưa ra các giá trị tiền tố (-1) và (-1), vì vậy câu trả lời là (-1). Bằng chứng hoặc việc triển khai dựa vào việc tăng nghiêm ngặt số tiền tích lũy ở mỗi vòng sẽ không hợp lệ. 

Cuối cùng, các giá trị (f_i) bằng nhau không yêu cầu quy tắc ràng buộc đặc biệt. Bất kỳ thứ tự nào trong số các giá trị (f_i) bằng nhau đều là tối ưu. Ví dụ,```
3
2 5
1 5
3 5
```có các giá trị tiền tố (-3,-2,1) theo bất kỳ thứ tự nào, vì vậy câu trả lời là (1). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi hoán vị của cặp (N). Đối với mỗi hoán vị, quét từ trái sang phải, duy trì (p) tích lũy và tính giá trị lớn nhất của (tiền tố-f_i). Điều này đúng vì nó đánh giá chính xác mức tăng có sẵn cho người chơi tối ưu cho mọi vị trí dừng có thể, sau đó chọn vị trí tốt nhất. Tuy nhiên, có các hoán vị (N!) và mỗi hoán vị mất (O(N)) thời gian để đánh giá, đưa ra các phép toán (O(N\cdot N!)). Ngay cả ở mức (N=10), con số này đã có khoảng (36) triệu vị trí cơ bản cần kiểm tra và (N=11) nâng con số đó lên khoảng (440) triệu. Tại (N=10^5), cách tiếp cận này không khả thi chút nào. 

Quan sát hữu ích là lợi ích của người chơi có chính xác dưới dạng mục tiêu lập kế hoạch. Hãy coi (p_i) là thời gian xử lý công việc (i) và (f_i) là ngày đáo hạn. Sau khi xử lý (i) công việc đầu tiên, thời gian hoàn thành của nó là tổng tiền tố của (p) và độ trễ của nó là 

[ 
C_i-f_i. 
] 

Chúng ta cần giảm thiểu độ trễ tối đa. Lập luận trao đổi cổ điển nói rằng các công việc nên được xử lý theo thứ tự không giảm dần về ngày đến hạn của chúng, điều này có nghĩa là sắp xếp các cặp theo thứ tự tăng dần (f_i). 

Lý do có thể được suy ra trực tiếp mà không cần dựa vào thuật ngữ lập kế hoạch. Giả sử hai cặp liền kề (a) và (b) hiện được sắp xếp không đúng thứ tự, do đó (f_a>f_b). Gọi (S) là tích lũy (p) trước hai cặp này. Theo thứ tự (a,b), hai giá trị liên quan là 

[ 
S+p_a-f_a 
] 

và 

[ 
S+p_a+p_b-f_b. 
] 

Bởi vì (f_a>f_b) và (p_b\ge0), giá trị thứ hai luôn ít nhất bằng giá trị thứ nhất. Vì vậy cặp này góp phần 

[ 
S+p_a+p_b-f_b 
] 

đến mức tối đa. 

Bây giờ hoán đổi chúng theo thứ tự (b, a). Hai giá trị trở thành 

[ 
S+p_b-f_b 
] 

và 

[ 
S+p_b+p_a-f_a. 
] 

Giá trị đầu tiên không lớn hơn mức tối đa cũ vì (p_a\ge0). Số thứ hai không lớn hơn vì (f_a>f_b), nên phép trừ (f_a) chỉ có thể làm cho nó nhỏ hơn phép trừ (f_b). Do đó, việc hoán đổi một đảo ngược không thể tăng mức tăng tối ưu của người chơi. 

Việc loại bỏ liên tục mọi phép đảo ngược sẽ khiến các cặp được sắp xếp theo thứ tự tăng dần (f_i) và không bao giờ làm cho câu trả lời trở nên tồi tệ hơn. Do đó thứ tự sắp xếp này là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N\cdot N!)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cặp ((p_i,f_i)). Thuộc tính duy nhất xác định thứ tự tương đối của chúng là (f_i), vì vậy chúng ta có thể giữ nguyên từng cặp và sắp xếp theo thành phần thứ hai của nó. 
2. Sắp xếp các cặp theo mức tăng dần (f_i). Đối số trao đổi cho thấy rằng bất cứ khi nào (f) lớn hơn xuất hiện trước (f) nhỏ hơn, việc hoán đổi hai cặp đó không thể tăng mức tăng tối đa có sẵn cho người chơi. Việc lặp lại quá trình này sẽ cho ra thứ tự sắp xếp chính xác. 
3. Quét các cặp được sắp xếp từ trái sang phải và duy trì`prefix`, tổng số tiền nhận được tính đến thời điểm hiện tại. Sau khi thêm một cặp ((p,f)), người chơi có thể dừng lại ở cặp này và nhận được`prefix - f`. 
4. Duy trì tối đa mọi giá trị`prefix - f`. Mức tối đa này là mức tăng tối ưu của người chơi đối với thứ tự đã chọn vì mọi vị trí dừng có thể có đều đã được xem xét. 
5. In tối đa. Vì thứ tự đã được chọn một cách tối ưu nên đây là lợi ích nhỏ nhất mà bất kỳ thứ tự nào cũng có thể cho phép người chơi tối ưu đạt được. 

### Tại sao nó hoạt động 

Đối với bất kỳ thứ tự cố định nào, người chơi có thể dừng ở đúng một vị trí và dừng ở vị trí (i) sẽ nhận giá trị (tiền tố_i-f_i). Do đó mức tăng tối ưu của người chơi chính xác là mức tối đa của các giá trị này. 

Hãy xem xét bất kỳ nghịch đảo liền kề nào trong đó (f_a>f_b). Với hai cặp được sắp xếp là (a,b), giá trị dừng bị ảnh hưởng lớn hơn của chúng là (S+p_a+p_b-f_b). Sau khi hoán đổi chúng, hai giá trị bị ảnh hưởng là (S+p_b-f_b) và (S+p_a+p_b-f_a). Cả hai đều đạt mức tối đa cũ vì (p_a\ge0) và (f_a>f_b). Tất cả các vị trí dừng khác có tổng tiền tố giống hệt nhau và không thay đổi. Do đó việc loại bỏ sự đảo ngược không bao giờ làm tăng mục tiêu. 

Mọi hoán vị có thể được chuyển thành thứ tự không giảm (f) bằng cách loại bỏ nhiều lần các phép nghịch đảo. Vì không có sự hoán đổi nào trong số đó làm tăng mục tiêu nên thứ tự được sắp xếp có giá trị mục tiêu không lớn hơn bất kỳ thứ tự nào khác. Do đó, nó là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pairs = [tuple(map(int, input().split())) for _ in range(n)]

    pairs.sort(key=lambda x: x[1])

    prefix = 0
    answer = None

    for p, f in pairs:
        prefix += p
        value = prefix - f
        if answer is None or value > answer:
            answer = value

    print(answer)

if __name__ == "__main__":
    solve()
```Đầu vào được lưu dưới dạng cặp để việc sắp xếp không bao giờ tách (p_i) khỏi (f_i tương ứng) của nó. Khóa sắp xếp là thành phần thứ hai, tăng dần (f_i).`prefix`được cập nhật trước khi tính toán`value`. Điều này tương ứng với việc người chơi nhận được (p_i) trước khi quyết định có nên ra về ở vòng đó hay không. Tính toán`prefix - f`trước khi thêm (p_i) sẽ đại diện cho một trò chơi khác và tạo ra từng lỗi một.`answer`bắt đầu như`None`thay vì một số lượng lớn được mã hóa cứng. Điều này đặc biệt thuận tiện vì câu trả lời đúng có thể âm, như trong Mẫu 3. Số nguyên Python cũng tự động xử lý tiền tố lớn nhất có thể. Tổng của tất cả (p_i) có thể đạt tới (10^{14}), sẽ vượt quá số nguyên 32 bit trong các ngôn ngữ như C++, mặc dù Python có số nguyên có độ chính xác tùy ý. 

Không có nhánh riêng cho cặp cuối cùng. Giá trị của nó được bao gồm khi quá trình quét đạt tới nó và vì người chơi phải dừng ở đó nên biểu thức`prefix - f`chính xác là mức hoàn trả cuối cùng cần thiết. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, các cặp đầu vào là ((1,2)), ((3,3)) và ((2,1)). Sắp xếp theo (f) cho ((2,1)), ((1,2)), ((3,3)). 

| Cặp | Tiền tố | Tiền tố - f | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| ((2,1)) | 2 | 1 | 1 | 
| ((1,2)) | 3 | 1 | 1 | 
| ((3,3)) | 6 | 3 | 3 | 

Ba mức tăng dừng có thể có của người chơi là (1), (1) và (3). Cô chọn cái lớn nhất, đó là (3). Vì bậc tăng (f) là tối ưu nên không hoán vị nào khác có thể tạo ra mức tăng tối ưu nhỏ hơn. 

Đối với Mẫu 2 chỉ có một cặp ((1,-2)). 

| Cặp | Tiền tố | Tiền tố - f | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| ((1,-2)) | 1 | 3 | 3 | 

Phí âm làm tăng lợi nhuận của người chơi, cho (1-(-2)=3). Khoản thanh toán cuối cùng bắt buộc được xử lý một cách tự nhiên bằng cùng một biểu thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính | 
| Không gian | (O(N)) | Các cặp (N) được lưu trữ để sắp xếp | 

Với (N=10^5), việc sắp xếp yêu cầu so sánh gần đúng (N\log N), điều này rất thực tế. Quá trình quét sau đó diễn ra tuyến tính và mức sử dụng bộ nhớ tỷ lệ thuận với số lượng cặp đầu vào. Tổng tích lũy có thể đạt tới (10^{14}) và số nguyên Python thể hiện nó một cách an toàn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    pairs = [tuple(map(int, input().split())) for _ in range(n)]

    pairs.sort(key=lambda x: x[1])

    prefix = 0
    answer = None

    for p, f in pairs:
        prefix += p
        value = prefix - f
        if answer is None or value > answer:
            answer = value

    return str(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("""3
1 2
3 3
2 1
""") == "3", "sample 1"

assert run("""1
1 -2
""") == "3", "sample 2"

assert run("""2
0 1
3 4
""") == "-1", "sample 3"

# Minimum-size input
assert run("""1
0 0
""") == "0", "single zero pair"

# Equal f values, any internal order is optimal
assert run("""4
2 5
2 5
2 5
2 5
""") == "3", "equal f values"

# Negative f and ordering direction
assert run("""2
1 -1
1 1
""") == "2", "negative f and increasing f order"

# Maximum-size input, also checks large prefix sums
max_case = "100000\n" + "1 0\n" * 100000
assert run(max_case) == "100000", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0`|`0`| Kích thước tối thiểu và giá trị bằng 0 | 
|`4 / 2 5`lặp đi lặp lại |`3`| Giá trị (f_i) bằng nhau và thứ tự ràng buộc tùy ý | 
|`2 / 1 -1 / 1 1`|`2`| Phí âm và hướng sắp xếp tăng dần chính xác | 
| (100000) bản sao của`1 0`|`100000`| Tối đa (N), tổng tiền tố lớn và quét tuyến tính | 

## Vỏ cạnh 

Cặp phí âm duy nhất```
1
1 -2
```được xử lý bằng cách sắp xếp mảng một phần tử và tính toán`prefix = 1`, theo sau là`prefix - f = 1 - (-2) = 3`. Thuật toán không cho rằng việc dừng lại sẽ tốn tiền. Số âm (f) chỉ đơn giản là phần thưởng khi người chơi rời đi. 

Cặp cuối cùng bắt buộc không yêu cầu trường hợp đặc biệt. Vì```
1
0 1
```quá trình quét thu được`prefix = 0`và tính toán`0 - 1 = -1`. Vì đây là vị trí dừng duy nhất có thể nên đáp án chính xác là (-1). Tổng quát hơn, vị trí cuối cùng được bao gồm trong số các biểu thức dừng giống như mọi vị trí trước đó. 

Giá trị 0 (p_i) cũng được xử lý trực tiếp. TRONG```
2
0 1
3 4
```các cặp đã được sắp xếp theo (f). Tiền tố đầu tiên là (0), cho (-1) và tiền tố thứ hai là (3), cho (3-4=-1). Tối đa là (-1). Bằng chứng trao đổi sử dụng (p_i\ge0), do đó thời gian xử lý bằng 0 là hoàn toàn hợp lệ. 

Các giá trị bằng (f_i) không ảnh hưởng đến đối số tham lam. Vì```
4
2 5
2 5
2 5
2 5
```mọi đơn đặt hàng có thể đều tương đương vì tất cả các khoản phí đều giống nhau. Các giá trị tiền tố là (2,4,6,8), do đó mức tăng dừng là (-3,-1,1,3) và câu trả lời là (3). Việc sắp xếp ở đây ổn định hoặc không ổn định mà không ảnh hưởng đến kết quả. 

Các câu trả lời phủ định là hợp lệ và không được nhầm lẫn với việc không có lợi nhuận. Mẫu 3 tạo ra (-1), vì mỗi vị trí dừng sẵn có đều mất đi một rúp. Việc khởi tạo câu trả lời về 0 sẽ biến giá trị này thành (0) một cách không chính xác, đó là lý do tại sao quá trình triển khai khởi tạo nó từ giá trị được tính toán đầu tiên. 

Số tiền tích lũy lớn nhất có thể là (10^5\cdot10^9=10^{14}). Biểu diễn số nguyên của Python xử lý việc này một cách chính xác, trong khi việc triển khai 32 bit sẽ tràn. Thuật toán không bao giờ thực hiện số học trên các giá trị lớn hơn thang đo này, do đó không có vấn đề về số trong Python.
