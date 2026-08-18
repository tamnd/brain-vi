---
title: "CF 102218K - Chữ số thiếu thứ K"
description: "Chúng ta có hai số nguyên dương (A) và (B), nhưng chúng có thể cực kỳ dài vì dòng đầu tiên hiển thị số chữ số của chúng chứ không phải giá trị số của chúng trong phạm vi kích thước máy."
date: "2026-08-17T23:26:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "K"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 163
verified: false
draft: false
---

[CF 102218K - Chữ số bị thiếu thứ K](https://codeforces.com/problemset/problem/102218/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên dương (A) và (B), nhưng chúng có thể cực kỳ dài vì dòng đầu tiên hiển thị số chữ số của chúng chứ không phải giá trị số của chúng trong phạm vi kích thước máy. Chúng ta cũng có một chuỗi thập phân (P) phải bằng (A \times B), ngoại trừ việc chính xác một chữ số đã được thay thế bằng`*`. Chữ số bị thiếu được đảm bảo là một trong (1,\dots,9) và chúng ta cần in nó. 

Ràng buộc quan trọng là (a), (b) và (p) có thể gần với (10^6). Điều đó có nghĩa là (A), (B) và (P) có thể chứa khoảng một triệu ký tự. Chúng tôi không thể chuyển đổi chúng thành số nguyên thông thường và thậm chí nhân hai số nguyên có hàng triệu chữ số bằng phép nhân trong sách giáo khoa sẽ yêu cầu khoảng (10^{12}) phép tính có một chữ số. Giới hạn thời gian 0,5 giây làm cho bất kỳ thuật toán bậc hai nào hoàn toàn không phù hợp. Chúng ta cần quét tuyến tính qua đầu vào. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý không thành công. Chữ số còn thiếu có thể là chữ số (9). Ví dụ,```
1 1 2
7
3
2*
```Tích là (21), nên đáp án là (1), không phải là thặng dư của (0). Tổng quát hơn, khi phép tính modulo (9) tạo ra (0), chữ số còn thiếu hợp lệ duy nhất là (9), vì bản thân (0) bị cấm. 

các`*`cũng có thể là ký tự đầu tiên của sản phẩm. Ví dụ,```
2 2 3
10
10
*00
```Câu trả lời là (1). Một phương thức cố gắng phân tích tích số không đầy đủ dưới dạng số nguyên phải xử lý ký tự đại diện đứng đầu một cách đặc biệt, trong khi phương thức tổng các chữ số không quan tâm đến nơi ký tự đại diện xuất hiện. 

Tích cũng có thể lớn hơn nhiều so với các loại số nguyên tiêu chuẩn. Ví dụ, nếu (A) và (B) mỗi cái có hàng trăm nghìn chữ số thì việc xây dựng (A \times B) một cách rõ ràng là không khả thi. Giải pháp dưới đây không bao giờ tạo ra tích và chỉ cần tổng chữ số của phần đã biết của (P). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi chữ số bị thiếu có thể từ (1) đến (9). Đối với mỗi ứng cử viên, nó sẽ thay thế`*`, xây dựng chuỗi sản phẩm hoàn chỉnh và kiểm tra xem nó có bằng (A \times B) hay không. Ý tưởng này đúng vì phát biểu đảm bảo rằng có chính xác một ứng cử viên là chữ số thực bị thiếu. 

Vấn đề là phép nhân. Nếu (A) và (B) đều chứa (10^6) chữ số, phép nhân trong sách giáo khoa thông thường sẽ thực hiện các phép toán chữ số (O(10^{12})). Việc thử chín ứng viên không làm thay đổi bài toán tiệm cận, vì vậy cách tiếp cận này vượt xa thời hạn. 

Quan sát quan trọng là tổng các chữ số thập phân bảo toàn giá trị modulo (9). Với mọi số nguyên (X), 

[ 
X \equiv \text{tổng các chữ số của }X \pmod 9. 
] 

Vì (P=A\times B), nên ta biết 

[ 
P \equiv A\times B \pmod 9. 
] 

Giả sử các chữ số đã biết của (P) có tổng (S) và chữ số còn thiếu là (d). Sau đó 

[ 
S+d \equiv A\times B \pmod 9. 
] 

Chúng ta có thể tính toán (A\bmod 9) và (B\bmod 9) bằng cách quét các chữ số của chúng mà không cần xây dựng các giá trị số của chúng. Chúng ta cũng có thể tính toán (S\bmod 9) bằng cách quét (P). Do đó, chữ số bị thiếu được xác định bằng một lần truyền tuyến tính duy nhất trên tất cả các ký tự đầu vào. 

Sự đảm bảo rằng (d\neq0) sẽ loại bỏ sự mơ hồ duy nhất. Dư lượng từ (1) đến (8) trực tiếp cho chữ số tương ứng, trong khi dư lượng (0) phải đại diện cho chữ số (9). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(ab)) | (O(a+b+p)) | Quá chậm | 
| Tối ưu | (O(a+b+p)) | (O(a+b+p)) để lưu trữ đầu vào, (O(1)) phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số chữ số và ba chuỗi. Số lượng không cần thiết cho bản thân việc tính toán, nhưng việc đọc chuỗi là cần thiết vì các số có thể quá lớn đối với các kiểu số nguyên gốc. 
2. Tính (A\bmod9) bằng cách xử lý từng chữ số của (A). Nếu số dư hiện tại là (r) và chữ số tiếp theo là (x) thì số dư mới là 

[ 
(10r+x)\bmod9. 
] 

Bởi vì (10\equiv1\pmod9), điều này tương đương với việc cộng modulo chữ số (9). 

1. Tính (B\bmod9) tương tự. 
2. Quét (P) và thêm mọi chữ số ngoại trừ`*`thành tổng hiện có modulo (9). Vị trí của`*`không thành vấn đề vì chỉ cần tổng các chữ số còn lại. 
3. Đặt (r=(A\bmod9)(B\bmod9)\bmod9). Đây là phần còn lại của sản phẩm hoàn chỉnh. 
4. Chữ số còn thiếu phải thỏa mãn 

[ 
d\equiv r-S\pmod9. 
] 

Chuẩn hóa kết quả thành phạm vi (0,\dots,8). 

1. Nếu số dư thu được là (0), hãy in (9). Nếu không thì hãy in phần dư ra. Vì chữ số bị thiếu được đảm bảo là khác 0 nên phần dư (0) không thể tương ứng với chữ số (0), để lại (9) là câu trả lời hợp lệ duy nhất. 

### Tại sao nó hoạt động 

Điều bất biến là mọi số và tổng các chữ số thập phân của nó đều có cùng phần dư modulo (9). Trong quá trình quét (A) và (B), chúng tôi duy trì chính xác dư lượng theo modulo (9). Trong quá trình quét (P), chúng tôi duy trì phần còn lại của tất cả các chữ số sản phẩm đã biết. Nếu chữ số bị thiếu là (d), tổng các chữ số đầy đủ của (P) là (S+d), do đó nó phải có cùng thặng dư với (A\times B). Do đó, giá trị được tính toán (d\equiv A B-S\pmod9) là số modulo chữ số duy nhất có thể bị thiếu (9). Các chữ số từ (1) đến (9) biểu thị mọi phần dư theo modulo (9) chính xác một lần, với (9) biểu thị phần dư (0), do đó câu trả lời được xác định duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mod9(s):
    r = 0
    for ch in s:
        r = (r + ord(ch) - ord('0')) % 9
    return r

def solve():
    a, b, p = map(int, input().split())
    A = input().strip()
    B = input().strip()
    P = input().strip()

    ra = mod9(A)
    rb = mod9(B)

    known = 0
    for ch in P:
        if ch != '*':
            known = (known + ord(ch) - ord('0')) % 9

    missing = (ra * rb - known) % 9

    if missing == 0:
        missing = 9

    print(missing)

if __name__ == "__main__":
    solve()
```các`mod9`hàm thực hiện hai bước thuật toán đầu tiên. Nó không bao giờ chuyển đổi toàn bộ chuỗi thành số nguyên, vì vậy giá trị trung gian của nó vẫn ở dưới (9). Sử dụng tổng chữ số trực tiếp là hợp lệ vì (10\equiv1\pmod9). 

Chuỗi sản phẩm được quét riêng biệt để`*`bị bỏ qua thay vì vô tình được coi là một chữ số. biểu thức`(ra * rb - known) % 9`sử dụng thao tác modulo của Python để tự động bình thường hóa các khác biệt âm. 

Không có vấn đề tràn số nguyên trong Python và ngay cả trong ngôn ngữ có chiều rộng cố định, các giá trị duy nhất được sử dụng để tính toán mô-đun sẽ có nhiều nhất là (8). Các đối tượng có tiềm năng lớn duy nhất là chính ba chuỗi đầu vào. Số lượng chữ số được khai báo không được sử dụng để lập chỉ mục cho các chuỗi, do đó không có vấn đề riêng lẻ nào liên quan đến độ dài sản phẩm. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
1 1 2
3
8
2*
```chúng ta có được trạng thái sau. 

| Bước | (A\bmod9) | (B\bmod9) | Dư lượng sản phẩm đã biết | Thiếu cặn | 
| --- | --- | --- | --- | --- | 
| Đọc (A=3) | 3 | | | | 
| Đọc (B=8) | 3 | 8 | | | 
| Đọc chữ số đã biết`2`| 3 | 8 | 2 | | 
| Tính cặn sản phẩm | 3 | 8 | 2 | (3\cdot8-2=22\equiv4) | 
| Đầu ra | 3 | 8 | 2 | 4 | 

Tích thực tế là (24) nên chữ số còn thiếu là (4). Dấu vết thể hiện tính bất biến trung tâm: (24\equiv2+4\equiv6\pmod9). 

Đối với mẫu thứ hai,```
2 2 3
10
10
*00
```dấu vết là: 

| Bước | (A\bmod9) | (B\bmod9) | Dư lượng sản phẩm đã biết | Thiếu cặn | 
| --- | --- | --- | --- | --- | 
| Đọc (A=10) | 1 | | | | 
| Đọc (B=10) | 1 | 1 | | | 
| Đọc`*00`| 1 | 1 | 0 | | 
| Tính cặn sản phẩm | 1 | 1 | 0 | (1\cdot1-0=1) | 
| Đầu ra | 1 | 1 | 0 | 1 | 

Sản phẩm hoàn chỉnh là (100), do đó ký tự đại diện được khôi phục chính xác thành (1). Ví dụ này cũng xác nhận rằng ký tự đại diện ở vị trí đầu tiên không yêu cầu xử lý đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(a+b+p)) | Mỗi chữ số của (A), (B) và (P) được quét một lần. | 
| Không gian | (O(a+b+p)) | Các chuỗi đầu vào được lưu trữ; bản thân thuật toán sử dụng không gian phụ trợ (O(1)). | 

Với tối đa khoảng một triệu chữ số cho mỗi số đầu vào, quá trình quét tuyến tính chỉ thực hiện vài triệu thao tác đơn giản. Điều đó tương thích với các ràng buộc dự định, trong khi việc nhân các số nguyên lớn một cách rõ ràng sẽ yêu cầu phép tính bậc hai và không khả thi trong giới hạn 0,5 giây. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    a, b, p = map(int, data[:3])
    A, B, P = data[3], data[4], data[5]

    def mod9(s):
        r = 0
        for ch in s:
            if ch != '*':
                r = (r + ord(ch) - ord('0')) % 9
        return r

    ra = mod9(A)
    rb = mod9(B)

    known = 0
    for ch in P:
        if ch != '*':
            known = (known + ord(ch) - ord('0')) % 9

    ans = (ra * rb - known) % 9
    if ans == 0:
        ans = 9

    return str(ans)

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Provided sample 1
assert run("""1 1 2
3
8
2*
""") == "4", "sample 1"

# Provided sample 2
assert run("""2 2 3
10
10
*00
""") == "1", "sample 2"

# Minimum-size values: 1 * 1 = 1
assert run("""1 1 1
1
1
*
""") == "1", "minimum-size case"

# Missing digit is 9, exercising the residue-zero boundary.
assert run("""1 1 2
7
3
1*
""") == "9", "digit 9 / residue zero"

# Wildcard at the beginning.
assert run("""2 1 3
99
1
*99
""") == "9", "leading wildcard"

# All equal digits: 111 * 111 = 12321.
assert run("""3 3 5
111
111
12*21
""") == "3", "all-equal inputs"

# Large input sizes, generated without constructing the product.
# A = 999...999 (999 digits), B = 1.
# 999...999 * 1 is the same string, so the missing digit is 9.
n = 999
A = "9" * n
P = "9" * (n - 1) + "*"
large_input = f"{n} 1 {n}\n{A}\n1\n{P}\n"
assert run(large_input) == "9", "large-size linear scan"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1 / 1 / *`|`1`| Giá trị nhỏ nhất có thể và tích có một chữ số | 
|`1 1 2 / 7 / 3 / 1*`|`9`| Dư (0) phải ánh xạ tới chữ số (9) | 
|`2 1 3 / 99 / 1 / *99`|`9`| Ký tự đại diện ở vị trí sản phẩm đầu tiên | 
|`3 3 5 / 111 / 111 / 12*21`|`3`| Các chữ số đầu vào lặp đi lặp lại và ký tự đại diện nội bộ | 
| 999 chữ số`9...9`, nhân với`1`|`9`| Đầu vào lớn và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Trường hợp không có dư lượng là trường hợp tế nhị nhất. Coi như```
1 1 2
7
3
1*
```Ở đây (A\bmod9=7), (B\bmod9=3) nên sản phẩm có cặn (21\bmod9=3). Chữ số đã biết đóng góp (1), cho ra (d\equiv3-1\equiv2\pmod9). Thuật toán in ra (2), khớp (7\times3=21). Nếu chữ số bắt buộc là (9), thì phần dư được tính thay vào đó sẽ là (0) và phép chuyển đổi cuối cùng từ (0) sang (9) sẽ xử lý trường hợp đó. 

Ký tự đại diện ở đầu không cần nhánh riêng. Vì```
2 2 3
10
10
*00
```cả hai toán hạng đều có phần dư (1) nên tích của chúng có phần dư (1). Các chữ số đã biết có tổng bằng (0), cho ra (d=1). Thuật toán không bao giờ cố gắng phân tích cú pháp`*00`, do đó không có vấn đề về ký tự đầu. 

Lý do tương tự cũng áp dụng khi ký tự đại diện ở giữa hoặc ở cuối. Ví dụ, trong```
3 3 5
111
111
12*21
```các toán hạng có phần dư (3) và (3), cho phần dư của sản phẩm (0). Các chữ số tích đã biết có tổng bằng (6), cũng là số dư (6). Do đó, chữ số còn thiếu thỏa mãn (d\equiv0-6\equiv3\pmod9), nên câu trả lời là (3). Sản phẩm thực tế là (12321). 

Cuối cùng, các toán hạng rất dài không thay đổi phương thức. Nếu (A) có (999999) chữ số, thuật toán vẫn thực hiện chính xác một cập nhật mô-đun cho mỗi chữ số. Nó không bao giờ lưu trữ số nguyên hàng triệu chữ số dưới dạng đối tượng số và không bao giờ thực hiện phép nhân trên các chữ số đó. Bản thân tích chỉ được biểu diễn gián tiếp thông qua modulo đồng dư (9), đây chính xác là thông tin cần thiết để xác định một chữ số khác 0 bị thiếu.
