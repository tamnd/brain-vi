---
title: "CF 102700G - Bữa tối tuyệt vời"
description: "Chúng ta có (N) học sinh riêng biệt phải xếp thành một hàng. Trong số đó có (M) cặp nạn nhân bắt nạt rời rạc. Đối với mỗi cặp đầu vào ((A,B)), sinh viên (A) phải xuất hiện trước sinh viên (B). Tuyên bố mô tả điều này là (B) không đi trước (A)."
date: "2026-08-08T08:21:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "G"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 350
verified: true
draft: false
---

[CF 102700G - Bữa tối tuyệt vời](https://codeforces.com/problemset/problem/102700/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có\(N\)những học sinh riêng biệt phải xếp thành một hàng. Trong số đó, có\(M\)cặp nạn nhân-bắt nạt rời rạc. Với mỗi cặp đầu vào \((A,B)\), sinh viên\(A\)phải xuất hiện trước học sinh\(B\). Tuyên bố mô tả điều này như\(B\)không đi trước\(A\). 

Đặc tính cấu trúc quan trọng là mỗi học sinh xuất hiện trong nhiều nhất một cặp. Vì thế\(M\)hạn chế liên quan đến\(2M\)học sinh khác nhau. Phần còn lại\(N-2M\)học sinh hoàn toàn không bị hạn chế. 

Nhiệm vụ là đếm mọi hoán vị của\(N\)học sinh đáp ứng được tất cả\(M\)hạn chế đặt hàng và đầu ra tính modulo\(10^9+7\). 

Giá trị của\(N\)có thể đạt được\(10^5\), vì vậy bất cứ điều gì liên quan đến tất cả các hoán vị đều là không thể ngay lập tức. có\(N!\)hoán vị, và thậm chí đối với\(N=20\), con số đó đã vượt xa những gì một chương trình cuộc thi thông thường có thể liệt kê. Với\(N=10^5\)và giới hạn hai giây, giải pháp dự định về cơ bản phải tuyến tính theo\(N\), có lẽ với một yếu tố bổ sung nhỏ tùy thuộc vào\(M\). Ràng buộc thứ hai đặc biệt hữu ích:\(2M\le 2000\), vậy có nhiều nhất\(1000\)hạn chế độc lập. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. Không có hạn chế, ví dụ,```text
1 0
```có chính xác\(1\)sự sắp xếp hợp lệ. Công thức chia cho\(2^M\)nhưng xử lý sai\(M=0\)có thể tạo ra số 0 không chính xác hoặc không tính được nghịch đảo. 

Trường hợp biên có một hạn chế là```text
4 1
1 4
```có\(4!=24\)tổng số sắp xếp, và chính xác một nửa đặt học sinh\(1\)trước sinh viên\(4\), vậy câu trả lời là\(12\). Một giải pháp bất cẩn có thể cố gắng sử dụng số lượng học sinh thực tế trong công thức, mặc dù chỉ có số lượng hạn chế độc lập là quan trọng. 

Cuối cùng, nhiều hạn chế phải liên quan đến các học sinh khác nhau. Ví dụ,```text
6 3
1 2
3 4
5 6
```có ba hạn chế độc lập. Câu trả lời là\(6!/2^3=90\). Việc coi các hạn chế là phụ thuộc sẽ đưa ra số liệu sai. Sự đảm bảo đầu vào rằng mọi điểm cuối xảy ra nhiều nhất một lần chính xác là điều làm cho phép chia đơn giản cho\(2^M\)có hiệu lực. 

Cụm từ "tất cả các giá trị bằng nhau" không mô tả trường hợp đầu vào hợp lệ ở đây, bởi vì mỗi học sinh xuất hiện trong một cặp phải khác với mọi học sinh được ghép đôi khác. Tương đương có ý nghĩa gần nhất là\(M=0\), trong đó tất cả học sinh không bị hạn chế và mọi hoán vị đều hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp sẽ tạo ra mọi hoán vị của\(N\)sinh viên. Đối với mỗi hoán vị, nó có thể xác định vị trí của học sinh trong mỗi cặp nạn nhân bị bắt nạt và kiểm tra xem tất cả các hướng được yêu cầu có chính xác hay không. Điều này đúng vì mọi dòng có thể được xem xét chính xác một lần và một dòng được tính chính xác khi mọi hạn chế được giữ nguyên. 

Vấn đề là số lượng hoán vị. có\(N!\)của chúng và kiểm tra\(M\)các hạn chế đối với mỗi cái sẽ cung cấp thời gian \(O(N!\,M)\). Ngay cả khi chúng tôi đã tối ưu hóa việc kiểm tra để mỗi hoán vị có thể được xác thực trong thời gian không đổi, việc liệt kê\(N!\)hoán vị vẫn sẽ là vô vọng đối với\(N=10^5\). Sự tăng trưởng giai thừa là trở ngại thực sự. 

Cấu trúc của các hạn chế cho chúng ta một quan sát mạnh mẽ hơn nhiều. Chỉ xem xét một cặp \((A,B)\). Trong một hoán vị được chọn thống nhất, chính xác một nửa số hoán vị chiếm vị trí\(A\)trước\(B\), trong khi nửa còn lại đặt\(B\)trước\(A\). Vì vậy, một hạn chế làm giảm số lượng hoán vị hợp lệ theo hệ số chính xác là hai. 

Bởi vì tất cả\(M\)các cặp rời rạc, những hạn chế này có thể được xử lý độc lập. Chính thức hơn, hãy tưởng tượng việc chọn một hướng cho mỗi cặp. có\(2^M\)các mẫu định hướng có thể có. Mỗi mẫu này được thực hiện bằng cùng một số lượng hoán vị. Chúng ta có thể chứng minh điều này bằng một phép song ánh: nếu chúng ta muốn thay đổi hướng của một cặp, hãy hoán đổi danh tính của hai học sinh của nó trong mọi hoán vị. Vì không có học sinh nào thuộc cặp khác nên việc hoán đổi này chỉ làm thay đổi hướng của cặp đó. 

có\(N!\)hoán vị phân bố đều giữa\(2^M\)các mẫu định hướng. Chính xác một mẫu là mẫu mong muốn, cụ thể là mẫu mà mọi kẻ bắt nạt đều đi trước nạn nhân tương ứng. Do đó câu trả lời là\[
\frac{N!}{2^M}.
\]Vì câu trả lời là bắt buộc theo modulo\(10^9+7\), chúng ta không thể thực hiện phép chia số nguyên thông thường. Mô đun là số nguyên tố, vì vậy\(2^M\)có nghịch đảo mô đun. Chúng tôi tính toán\[
N!\cdot (2^M)^{-1}\pmod{10^9+7}.
\]Phương pháp brute-force hoạt động vì nó kiểm tra rõ ràng các đối tượng mà chúng ta muốn đếm, nhưng không thành công vì có quá nhiều đối tượng trong số đó. Quan sát rằng mọi cặp rời rạc đều có hai hướng có thể có kích thước bằng nhau cho phép chúng ta đếm tất cả các hoán vị cùng một lúc và thay thế phép liệt kê bằng một giai thừa và một nghịch đảo mô đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---:|---:|---| 
| Lực lượng vũ phu | \(O(N!\,M)\) | \(O(N+M)\) | Quá chậm | 
| Tối ưu | \(O(N+M)\) | \(O(1)\) phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc\(N\)Và\(M\). Danh tính thực tế của các học sinh được ghép nối không ảnh hưởng đến công thức cuối cùng, bởi vì thuộc tính liên quan duy nhất là\(M\)các cặp rời rạc. 

2. Đọc tất cả\(M\)cặp. Chúng ta không cần phải lưu trữ hoặc kiểm tra số học sinh của họ sau khi đọc. Vai trò của họ chỉ là cho chúng ta biết rằng có\(M\)hạn chế đặt hàng độc lập. 

3. Tính toán\(N!\)modulo\(10^9+7\). Bắt đầu với`fact = 1`và nhân với mọi số nguyên từ\(1\)bởi vì\(N\), lấy số dư sau mỗi lần nhân. 

4. Tính toán\(2^M\)modulo\(10^9+7\). Điều này thể hiện sự\(2^M\)các lớp định hướng có kích thước bằng nhau trong đó tất cả các hoán vị được phân chia. 

5. Tính nghịch đảo mô đun của\(2^M\). Theo định lý nhỏ Fermat, đối với giá trị khác 0\(x\)modulo số nguyên tố\(P=10^9+7\),\[
x^{-1}\equiv x^{P-2}\pmod P.
\]Như vậy nghịch đảo của\(2^M\)thu được với`pow(2, M, P)`theo sau là một phép lũy thừa mô-đun khác hoặc tương đương với`pow(2, M * (P - 2), P)`. Tính mẫu số trước sẽ rõ ràng hơn. 

6. Nhân giai thừa với nghịch đảo đó và lấy kết quả theo modulo\(P\). Điều này mang lại\[
N!\cdot (2^M)^{-1}\pmod P,
\]đó chính xác là số lượng yêu cầu sắp xếp hợp lệ. 

### Tại sao nó hoạt động 

Mỗi hoán vị có một hướng xác định cho mỗi hoán vị\(M\)các cặp rời rạc. Do đó mọi hoán vị đều thuộc đúng một trong\(2^M\)các mẫu định hướng. 

Đối với hai mẫu định hướng bất kỳ, chúng ta có thể chuyển đổi mọi hoán vị thuộc mẫu thứ nhất thành hoán vị thuộc mẫu thứ hai bằng cách hoán đổi hai học sinh trong mỗi cặp có hướng cần thay đổi. Bởi vì các cặp này rời rạc nên các giao dịch hoán đổi này không ảnh hưởng lẫn nhau. Sự biến đổi có thể thuận nghịch nên tất cả\(2^M\)các mẫu chứa chính xác cùng một số hoán vị. 

Vì tất cả\(N!\)hoán vị được phân chia giữa\(2^M\)các lớp có quy mô bằng nhau, mỗi lớp chứa\(N!/2^M\)hoán vị. Sự sắp xếp bắt buộc chính xác là một trong các lớp này, do đó thuật toán trả về số lượng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())

    for _ in range(m):
        input()

    fact = 1
    for x in range(1, n + 1):
        fact = fact * x % MOD

    denominator = pow(2, m, MOD)
    inverse_denominator = pow(denominator, MOD - 2, MOD)

    answer = fact * inverse_denominator % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào tiêu thụ tất cả\(M\)hạn chế, nhưng cố tình không lưu trữ chúng. Việc đảm bảo rằng tất cả các điểm cuối đều khác biệt có nghĩa là các giá trị chính xác của chúng không liên quan sau khi chúng tôi biết rằng đầu vào chứa\(M\)hạn chế. 

Vòng lặp giai thừa chạy qua mỗi học sinh đúng một lần. Lấy mô đun sau mỗi phép nhân giữ cho tất cả các giá trị trung gian bị giới hạn bởi khoảng\(MOD^2\), Python xử lý thoải mái. 

Mẫu số được tính là\(2^M\bmod MOD\). Từ\(M\le1000\), số mũ này rất nhỏ, mặc dù Python có sẵn`pow`cũng thực hiện phép tính lũy thừa mô-đun một cách hiệu quả đối với các số mũ lớn hơn nhiều. 

biểu hiện`pow(denominator, MOD - 2, MOD)`tính toán nghịch đảo mô đun. Điều này hợp lệ vì\(MOD=10^9+7\)là số nguyên tố và`denominator`không chia hết cho\(MOD\). 

Không có vấn đề tràn số nguyên trong Python. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, phép nhân phải sử dụng loại đủ rộng hoặc chiến lược nhân mô-đun. 

Các hạn chế được đọc trước khi tính toán câu trả lời, do đó chương trình cũng xử lý\(M=0\)một cách chính xác. Trong trường hợp đó mẫu số là\(2^0=1\), nghịch đảo của nó cũng là\(1\), và câu trả lời chỉ đơn giản là\(N!\). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```text
4 2
2 1
4 3
```Có bốn sinh viên và hai hạn chế độc lập. Việc đầu tiên yêu cầu học sinh\(2\)xuất hiện trước học sinh\(1\), trong khi cách thứ hai yêu cầu học sinh\(4\)xuất hiện trước học sinh\(3\). 

Việc tính giai thừa và mẫu số được tiến hành như sau. 

| Bước | Biến | Giá trị | 
|---|---|---:| 
| Bắt đầu |`fact`| 1 | 
|\(1!\)|`fact`| 1 | 
|\(2!\)|`fact`| 2 | 
|\(3!\)|`fact`| 6 | 
|\(4!\)|`fact`| 24 | 
| Hạn chế |`m`| 2 | 
| Lớp định hướng |\(2^M\)| 4 | 
| Đếm cuối cùng |\(24/4\)| 6 | 

Hai cặp đưa ra bốn mẫu định hướng có thể. Mỗi mẫu chứa\(24/4=6\)hoán vị, và chính xác một mẫu có\(2\)trước\(1\)Và\(4\)trước\(3\). Vì thế câu trả lời là`6`. 

### Mẫu 2 

Đầu vào là```text
4 1
1 3
```Chỉ có một hạn chế duy nhất là yêu cầu học sinh\(1\)đi trước học sinh\(3\). 

| Bước | Biến | Giá trị | 
|---|---|---:| 
| Bắt đầu |`fact`| 1 | 
|\(1!\)|`fact`| 1 | 
|\(2!\)|`fact`| 2 | 
|\(3!\)|`fact`| 6 | 
|\(4!\)|`fact`| 24 | 
| Hạn chế |`m`| 1 | 
| Lớp định hướng |\(2^M\)| 2 | 
| Đếm cuối cùng |\(24/2\)| 12 | 

Chính xác là một nửa\(24\)hoán vị đặt\(1\)trước\(3\), vậy câu trả lời là`12`. Danh tính thực sự của hai học sinh không quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(N+M+\log MOD)\) | Giai thừa lấy \(O(N)\), giới hạn đọc lấy \(O(M)\) và phép lũy thừa mô-đun lấy \(O(\log MOD)\) phép nhân. | 
| Không gian | \(O(1)\) phụ trợ | Các hạn chế được sử dụng mà không được lưu trữ. | 

Với\(N\le10^5\), vòng lặp giai thừa chỉ thực hiện\(10^5\)lần lặp lại. Số lượng hạn chế nhiều nhất là\(1000\), do đó tổng công dễ dàng nằm trong giới hạn hai giây. Chương trình cũng sử dụng bộ nhớ phụ liên tục ngoài máy móc đầu vào. 

## Trường hợp thử nghiệm 

Khai thác sau phản ánh giải pháp được gửi trong khi cho phép một số cuộc gọi độc lập.```python
import sys
import io

MOD = 10**9 + 7

def solution():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    for _ in range(m):
        input()

    fact = 1
    for x in range(1, n + 1):
        fact = fact * x % MOD

    denominator = pow(2, m, MOD)
    inverse_denominator = pow(denominator, MOD - 2, MOD)

    print(fact * inverse_denominator % MOD)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """4 2
2 1
4 3
"""
).strip() == "6", "sample 1"

# Provided sample 2
assert run(
    """4 1
1 3
"""
).strip() == "12", "sample 2"

# Minimum size: one student, no restrictions
assert run(
    """1 0
"""
).strip() == "1", "minimum size"

# No restrictions: every permutation is valid
assert run(
    """5 0
"""
).strip() == "120", "M = 0"

# Three disjoint restrictions
assert run(
    """6 3
1 2
3 4
5 6
"""
).strip() == "90", "three independent pairs"

# Boundary student numbers: the identities do not affect the count
assert run(
    """4 1
1 4
"""
).strip() == "12", "boundary endpoints"

# Maximum-size configuration allowed by the constraints
n = 100000
m = 1000
pairs = "\n".join(f"{2 * i - 1} {2 * i}" for i in range(1, m + 1))
max_input = f"{n} {m}\n{pairs}\n"

expected = 1
for x in range(1, n + 1):
    expected = expected * x % MOD
expected = expected * pow(pow(2, m, MOD), MOD - 2, MOD) % MOD

assert run(max_input).strip() == str(expected), "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---:|---| 
|`1 0`|`1`| tối thiểu\(N\), không hạn chế, và\(0!\)-phong cách ranh giới | 
|`5 0`|`120`| Không có hạn chế nên mọi hoán vị đều hợp lệ | 
|`6 3`với cặp`(1,2)`,`(3,4)`,`(5,6)`|`90`| Một số hạn chế độc lập | 
|`4 1`với cặp`(1,4)`|`12`| Số lượng sinh viên ranh giới và một hạn chế duy nhất | 
|\(N=100000,\ M=1000\)với\(1000\)cặp rời rạc | mô-đun tính toán\(10^9+7\)| Hạn chế và hiệu suất tối đa | 

Các ràng buộc cấm các điểm cuối lặp lại, do đó, một thử nghiệm có nhiều giá trị cặp giống hệt nhau sẽ không phải là một trường hợp thử nghiệm hợp lệ. Kiểm tra\(M=0\)là cách thích hợp để giải quyết tình trạng không hạn chế, tất cả học sinh đều tự do như nhau. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là\(M=0\). Vì```text
1 0
```thuật toán bỏ qua vòng lặp đọc hạn chế, tính toán\(1!=1\), thu được\(2^0=1\)và nhân với nghịch đảo của nó\(1\). Kết quả là`1`. Tổng quát hơn, đối với`5 0`, kết quả là\(5!=120\), bởi vì không có hạn chế đặt hàng nào cả. 

Trường hợp cạnh thứ hai là một hạn chế duy nhất. Coi như```text
4 1
1 4
```Thuật toán tính toán\(4!=24\). có\(2^1=2\)các hướng có thể có của cặp \((1,4)\) và cả hai hướng đều chứa cùng một số hoán vị. Chia cho hai được`12`. Điều này cũng cho thấy tại sao bản thân số học sinh không nhập được công thức. 

Trường hợp cạnh thứ ba là một số hạn chế độc lập:```text
6 3
1 2
3 4
5 6
```Thuật toán tính toán\(6!=720\)và chia cho\(2^3=8\), cho`90`. Mỗi cặp độc lập đóng góp một nửa hệ số, do đó ba hạn chế cùng đóng góp\(1/8\). 

Trường hợp cạnh cuối cùng là kích thước đầu vào tối đa. Với\(N=100000\)Và\(M=1000\), vẫn chỉ có\(100000\)phép nhân giai thừa và\(1000\)hạn chế tiêu thụ. Thuật toán không bao giờ xây dựng một hoán vị và không bao giờ lưu trữ các cặp, vì vậy thời gian chạy của nó vẫn tuyến tính theo số lượng học sinh và bộ nhớ phụ của nó không đổi. Đây chính xác là thang đo làm cho công thức tính giai thừa phù hợp với các giới hạn đã cho. 
:::
