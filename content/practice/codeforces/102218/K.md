---
title: "CF 102218K - Chữ số thiếu thứ K"
description: "Chúng ta có hai số nguyên dương (A) và (B), mỗi số có khả năng chứa tới một triệu chữ số thập phân. Sản phẩm của họ được viết dưới dạng chuỗi thập phân (P), ngoại trừ chính xác một chữ số đã được thay thế bằng . Nhiệm vụ là tìm lại chữ số bị thiếu đó."
date: "2026-08-20T03:38:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "K"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 304
verified: false
draft: false
---

[CF 102218K - Chữ số bị thiếu thứ K](https://codeforces.com/problemset/problem/102218/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 4 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên dương (A) và (B), mỗi số có khả năng chứa tới một triệu chữ số thập phân. Tích của họ được viết dưới dạng một chuỗi thập phân (P), ngoại trừ việc chính xác một chữ số đã được thay thế bằng`*`. Nhiệm vụ là tìm lại chữ số bị thiếu đó. Chữ số bị thiếu được đảm bảo là khác 0. 

Dòng đầu tiên cho biết độ dài của (A), (B) và (P). Hai dòng tiếp theo chứa (A) và (B), và dòng cuối cùng chứa biểu diễn thập phân đã biết một phần của (A \times B). Câu trả lời là một chữ số thập phân phải thay thế`*`. 

Ràng buộc quan trọng là số chữ số, không phải giá trị số của số nguyên. (A) và (B) đều có thể có (10^6) chữ số, do đó việc xây dựng một trong hai số nguyên bằng số học máy thông thường là không thể. Ngay cả một phép nhân cấp lớp cũng sẽ yêu cầu các phép tính chữ số theo thứ tự (10^{12}). Việc thử tất cả chín chữ số có thể bị thiếu và kiểm tra sản phẩm thu được sẽ nhân chi phí đó lên chín. 

Bản thân đầu vào có thể chứa tới khoảng hai triệu chữ số, do đó, một thuật toán quét các chuỗi với số lần không đổi là phù hợp. Một thuật toán yêu cầu phép nhân, chuyển đổi sang kiểu số nguyên có sẵn hoặc phép tính bậc hai trên chuỗi chữ số là không phù hợp. Các số nguyên có độ chính xác tùy ý của Python không thay đổi kết luận này, bởi vì các số nguyên đầu vào có thể lớn hơn nhiều so với các biểu diễn gốc thực tế và việc chuyển đổi các chuỗi thập phân hàng triệu chữ số bản thân nó là công việc không cần thiết. 

Có hai trường hợp đặc biệt thường gây ra lỗi. Đầu tiên, chữ số còn thiếu có thể là (9). Ví dụ,```
1 1 2
9
1
1*
```cho (A \times B = 9), nên câu trả lời là`9`. Một phương pháp chỉ tính kết quả modulo (9) và trả về phần dư trực tiếp sẽ trả về`0`, điều này bị cấm nhưng tương đương về mặt toán học modulo (9). Việc đảm bảo rằng chữ số bị thiếu là khác 0 cho chúng ta hiểu phần dư (0) là chữ số (9). 

Trường hợp cạnh thứ hai là một chữ số bị thiếu ở hai đầu của sản phẩm. Ví dụ,```
2 2 3
10
10
*00
```có sản phẩm`100`, vậy chữ số còn thiếu là`1`. Phương pháp cố gắng suy ra chữ số chỉ sử dụng các ký tự lân cận hoặc coi ngôi sao là chữ số bên trong có thể thất bại ở đây. Đối số mô-đun không phụ thuộc vào vị trí của ngôi sao, do đó, phép tính tương tự áp dụng cho vị trí đầu tiên, cuối cùng hoặc bất kỳ vị trí ở giữa nào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi chữ số khác 0 có thể có từ (1) đến (9), thay thế`*`với chữ số đó và kiểm tra xem chuỗi kết quả có bằng (A \times B) hay không. Điều này đúng vì chữ số bị thiếu đảm bảo là một trong chín giá trị đó nên việc kiểm tra mọi khả năng phải tìm ra câu trả lời. 

Vấn đề là phép nhân. Với tối đa (10^6) chữ số ở cả (A) và (B), phép nhân dài thông thường thực hiện các phép toán chữ số (O(10^{12})) cho một ứng cử viên. Việc kiểm tra chín ứng viên vẫn còn lại khoảng (9 \times 10^{12}) phép tính trong trường hợp xấu nhất. Phép nhân số nguyên lớn phức tạp hơn có thể giảm tiệm cận đó, nhưng bài toán có một tính chất đơn giản hơn nhiều là tránh được phép nhân hoàn toàn. 

Quan sát quan trọng là các số thập phân bảo toàn giá trị modulo (9) khi các chữ số của chúng được tính tổng. Đối với bất kỳ số thập phân (X), 

[ 
X \equiv \sum \text{chữ số}(X) \pmod 9. 
] 

Vì (P = A \times B), nên chúng ta có thể tính 

[ 
P \bmod 9 = (A \bmod 9)(B \bmod 9) \bmod 9. 
] 

Chúng ta có thể tính toán (A \bmod 9) và (B \bmod 9) bằng cách quét các chữ số của chúng mà không cần phải tạo ra các số nguyên khổng lồ. Chúng ta cũng có thể tính phần đóng góp của mọi chữ số đã biết của (P), chỉ để lại chữ số chưa biết (x). 

Giả sử tổng các chữ số đã biết trong (P) là (S). Sau đó 

[ 
x + S \equiv A \times B \pmod 9. 
] 

Như vậy 

[ 
x \equiv (A \times B - S) \pmod 9. 
] 

Điều này mang lại dư lượng từ (0) đến (8). Nếu từ (1) đến (8) thì đó chính xác là chữ số bị thiếu. Nếu là (0), chữ số bị thiếu phải là (9), vì bài toán loại trừ rõ ràng (0). 

Phương pháp brute-force hoạt động vì mọi ứng cử viên đều có thể được kiểm tra dựa trên sản phẩm chính xác, nhưng không thành công khi toán hạng chứa hàng triệu chữ số. Quan sát cho thấy tổng các chữ số thập phân bảo toàn các giá trị modulo (9) cho phép chúng ta thay thế phép nhân cực lớn bằng ba lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(AB)) cho phép nhân cấp lớp, lặp lại tối đa 9 lần | (O(A+B+P)) | Quá chậm | 
| Tối ưu | (O(A+B+P)) | (O(1)) bên cạnh chuỗi đầu vào | Đã chấp nhận | 

Ở đây (A), (B) và (P) trong biểu thức độ phức tạp biểu thị số chữ số, không phải giá trị số của số nguyên. 

## Hướng dẫn thuật toán 

1. Đọc ba độ dài và các chuỗi biểu thị (A), (B) và tích đã biết một phần (P). Độ dài không cần thiết cho toán học, nhưng việc đọc chúng tuân theo định dạng đầu vào. 
2. Tính (A \bmod 9) bằng cách quét từng chữ số của (A) và áp dụng nhiều lần`remainder = (remainder + digit) % 9`. Tính toán tương tự được thực hiện cho (B). Điều này có tác dụng vì chữ số thập phân có giá trị (10^k) và (10 \equiv 1 \pmod 9), do đó mọi lũy thừa của (10) cũng bằng với (1). 
3. Nhân hai số dư theo modulo (9). Kết quả là phần còn lại mà sản phẩm hoàn chỉnh (P) phải có. 
4. Quét (P), thêm mọi chữ số đã biết modulo (9) trong khi bỏ qua`*`. Gọi giá trị kết quả`known`. 
5. Tính số dư của chữ số còn thiếu là`(product_mod - known) % 9`. Hoạt động modulo xử lý trường hợp phép trừ âm. 
6. Nếu dư lượng thu được bằng 0, hãy xuất`9`; nếu không thì xuất ra phần dư. Số dư bằng 0 tương ứng với cả hai chữ số`0`và chữ số`9`, và tuyên bố loại trừ`0`. 

### Tại sao nó hoạt động 

Điều bất biến là mọi chuỗi thập phân được xử lý đều có cùng phần dư modulo (9) là tổng các chữ số được xử lý của nó. Sau khi quét (A) và (B), chúng ta biết chính xác dư lượng của chúng theo modulo (9), do đó, dư lượng sản phẩm của chúng được biết đến từ phép nhân mô đun. Sau khi quét các chữ số đã biết của (P), phần đóng góp duy nhất còn thiếu trong tổng chữ số của nó là chữ số chưa biết (x). Do đó (x + S) phải có chính xác phần dư của tích theo modulo (9). Phần dư được tính toán xác định (x) duy nhất trong số các chữ số được phép (1,\ldots,9), vì chỉ (0) và (9) chia sẻ phần dư bằng 0 và (0) bị cấm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_mod_9(s):
    remainder = 0
    for ch in s:
        if ch != '\n':
            remainder = (remainder + ord(ch) - ord('0')) % 9
    return remainder

def solve():
    a_len, b_len, p_len = map(int, input().split())

    A = input().strip()
    B = input().strip()
    P = input().strip()

    a_mod = digit_mod_9(A)
    b_mod = digit_mod_9(B)

    product_mod = (a_mod * b_mod) % 9

    known_mod = 0
    for ch in P:
        if ch != '*':
            known_mod = (known_mod + ord(ch) - ord('0')) % 9

    missing = (product_mod - known_mod) % 9

    if missing == 0:
        missing = 9

    print(missing)

if __name__ == "__main__":
    solve()
```các`digit_mod_9`hàm thực hiện phép tính tổng chữ số được mô tả trong phần đầu tiên của thuật toán. Giữ phần dư giảm modulo (9) có nghĩa là biến không bao giờ tăng quá một chữ số. 

Hai người gọi tiếp`A`Và`B`cung cấp phần dư của toán hạng. Nhân các phần dư đó sẽ có phần dư của sản phẩm thực tế mà không cần tạo ra sản phẩm đó. 

Lần quét cuối cùng bỏ qua`*`và tích lũy phần dư của tất cả các chữ số sản phẩm đã biết. Trừ phần này khỏi phần dư của sản phẩm sẽ tách ra modulo chữ số bị thiếu (9). 

các`% 9`thao tác sau phép trừ là cần thiết vì Python có thể tạo ra kết quả trung gian âm trước khi lấy modulo. Sự chuyển đổi cuối cùng từ`0`ĐẾN`9`cũng là điều cần thiết. Trả lại phần dư trực tiếp sẽ không thành công bất cứ khi nào chữ số bị thiếu thực tế là (9). 

Không có chuyển đổi số nguyên và không có phép nhân số lớn. Số học có độ chính xác tùy ý của Python chỉ được sử dụng cho các giá trị nhỏ bên dưới (9^2), do đó việc tràn số nguyên không phải là vấn đề. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là:```
1 1 2
3
8
2*
```Việc tính toán tiến hành như sau. 

| Biến | Tiểu bang | 
| --- | --- | 
| (A \bmod 9) | (3) | 
| (B \bmod 9) | (8) | 
| Sản phẩm modulo (9) | (3 \times 8 \bmod 9 = 6) | 
| Đã biết tổng chữ số sản phẩm modulo (9) | (2) | 
| Thiếu chữ số modulo (9) | (6 - 2 = 4) | 
| Trả lời |`4`| 

Sản phẩm thực tế là (3 \times 8 = 24). Phép tính mô-đun đạt đến cùng một chữ số bị thiếu mà không cần tính toán sản phẩm đó một cách rõ ràng. Bất biến hiển thị ở đây vì (2 + 4 = 6), khớp với modulo dư của tích (9). 

Đối với mẫu thứ hai, đầu vào là:```
2 2 3
10
10
*00
```Dấu vết là: 

| Biến | Tiểu bang | 
| --- | --- | 
| (A \bmod 9) | (1) | 
| (B \bmod 9) | (1) | 
| Sản phẩm modulo (9) | (1) | 
| Đã biết tổng chữ số sản phẩm modulo (9) | (0) | 
| Thiếu chữ số modulo (9) | (1) | 
| Trả lời |`1`| 

Ở đây ngôi sao là ký tự đầu tiên của sản phẩm. Vị trí của nó không ảnh hưởng đến phương pháp vì mỗi chữ số thập phân đóng góp giá trị modulo (9), bất kể giá trị vị trí của nó. Sản phẩm là`100`, và phần dư của nó là (1), nên chữ số đầu bị thiếu phải là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(A+B+P)) | Mỗi số đầu vào và chuỗi sản phẩm được quét một lần. | 
| Không gian | (O(A+B+P)) | Các chuỗi đầu vào được lưu trữ, trong khi bản thân thuật toán sử dụng không gian bổ sung (O(1)). | 

Tổng kích thước đầu vào tối đa là vài triệu ký tự, do đó số lần quét tuyến tính không đổi là phù hợp ngay cả trong giới hạn thời gian rất chặt chẽ. Thuật toán không bao giờ phân bổ đại diện của sản phẩm đầy đủ và chỉ sử dụng một số biến số nguyên nhỏ ngoài chuỗi đầu vào, điều này cũng giúp duy trì mức sử dụng bộ nhớ thoải mái trong giới hạn đã nêu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io():
    input = sys.stdin.readline

    a_len, b_len, p_len = map(int, input().split())
    A = input().strip()
    B = input().strip()
    P = input().strip()

    a_mod = 0
    for ch in A:
        a_mod = (a_mod + ord(ch) - ord('0')) % 9

    b_mod = 0
    for ch in B:
        b_mod = (b_mod + ord(ch) - ord('0')) % 9

    product_mod = (a_mod * b_mod) % 9

    known_mod = 0
    for ch in P:
        if ch != '*':
            known_mod = (known_mod + ord(ch) - ord('0')) % 9

    answer = (product_mod - known_mod) % 9
    if answer == 0:
        answer = 9

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve_io()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("""1 1 2
3
8
2*
""") == "4", "sample 1"

assert run("""2 2 3
10
10
*00
""") == "1", "sample 2"

# Minimum-size operands, 1 * 9 = 9
assert run("""1 1 1
1
9
*
""") == "9", "minimum size and missing digit 9"

# Missing digit is 9 and appears in the middle
# 99 * 1 = 99
assert run("""2 1 2
99
1
9*
""") == "9", "residue zero must map to digit 9"

# Missing digit is the first product digit
# 12 * 8 = 96
assert run("""2 1 2
12
8
*6
""") == "9", "leading missing digit"

# All digits equal, product 111 * 3 = 333
assert run("""3 1 3
111
3
3*3
""") == "3", "all-equal digits"

# Larger boundary-style values:
# 999999 * 9 = 8999991
assert run("""6 1 7
999999
9
8*99991
""") == "9", "large values and missing digit 9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`,`9`,`*`|`9`| Kích thước tối thiểu và trường hợp không có cặn đặc biệt | 
|`99`,`1`,`9*`|`9`| Thiếu chữ số cuối cùng bằng`9`| 
|`12`,`8`,`*6`|`9`| Thiếu chữ số đầu tiên | 
|`111`,`3`,`3*3`|`3`| Lặp lại các chữ số bằng nhau | 
|`999999`,`9`,`8*99991`|`9`| Toán hạng lớn hơn và số học tỷ lệ biên | 

## Vỏ cạnh 

Chữ số bị thiếu bằng (9) là trường hợp khó phát hiện nhất. Coi như:```
1 1 1
1
9
*
```Sản phẩm là`9`, có phần dư modulo (9) bằng 0. Phần đóng góp chữ số đã biết cũng bằng 0 vì không có chữ số sản phẩm nào đã biết. Thuật toán tính toán`missing = 0`, sau đó chuyển phần dư đó thành`9`. Điều này đúng vì`0`bị cấm bởi đảm bảo đầu vào. 

Ngôi sao ở đầu sản phẩm không yêu cầu xử lý vị trí đặc biệt. Vì:```
2 1 2
12
8
*6
```sản phẩm là`96`. Chúng ta có (12 \equiv 3 \pmod 9) và (8 \equiv 8 \pmod 9), cho ra sản phẩm còn lại (3 \times 8 \equiv 6). Chữ số đã biết là`6`, do đó chữ số bị thiếu có phần dư (6-6\equiv0). Chữ số duy nhất được phép có phần dư đó là`9`, giao đúng sản phẩm`96`. 

Một ngôi sao ở cuối hành xử giống hệt nhau. Vì:```
1 1 2
3
8
2*
```dư lượng sản phẩm là (6), trong khi chữ số đã biết`2`đóng góp (2). Chữ số còn thiếu là (4) nên thuật toán in ra`4`. 

Các toán hạng lớn được xử lý mà không cần nhân chúng. Nếu (A) chứa một triệu chữ số và (B) chứa một triệu chữ số, thuật toán chỉ duy trì hai số dư trong khoảng (0) đến (8) trong khi quét đầu vào. Lý do tương tự cũng áp dụng cho chuỗi sản phẩm chứa gần hai triệu chữ số. Đây chính xác là quy mô mà cách tiếp cận mô-đun thành công và phép nhân rõ ràng không thành công.
