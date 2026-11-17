---
title: "CF 200D - Ngôn ngữ lập trình"
description: "Chúng ta được cung cấp một tập hợp các khai báo thủ tục và một tập hợp các biến. Mỗi khai báo thủ tục bao gồm một tên và một danh sách các kiểu tham số. Loại tham số có thể là một trong các loại cụ thể int, string hoặc double hoặc có thể là loại ký tự đại diện T."
date: "2026-06-03T16:27:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "expression-parsing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 200
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 126 (Div. 2)"
rating: 1800
weight: 200
solve_time_s: 129
verified: true
draft: false
---

[CF 200D - Ngôn ngữ lập trình](https://codeforces.com/problemset/problem/200/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, phân tích biểu thức, triển khai 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các khai báo thủ tục và một tập hợp các biến. 

Mỗi khai báo thủ tục bao gồm một tên và một danh sách các kiểu tham số. Một loại tham số có thể là một trong những loại cụ thể`int`,`string`, hoặc`double`, hoặc có thể là loại ký tự đại diện`T`. Loại`T`khớp với bất kỳ loại biến nào. 

Sau đó, các cuộc gọi thủ tục được đưa ra. Cuộc gọi chứa tên thủ tục và danh sách các tên biến. Vì mỗi biến đều có một kiểu đã biết nên mỗi lệnh gọi có thể được xem dưới dạng tên thủ tục cùng với danh sách có thứ tự các kiểu đối số. 

Đối với mỗi cuộc gọi, chúng ta phải đếm xem có bao nhiêu thủ tục được khai báo tương thích với nó. 

Một quy trình tương thích nếu đáp ứng được cả ba điều kiện: 

1. Tên giống hệt nhau. 
2. Số lượng tham số giống hệt nhau. 
3. Đối với mọi vị trí, loại tham số thủ tục là`T`hoặc nó chính xác bằng loại đối số tương ứng. 

Khó khăn chính không phải là logic phù hợp. Phần khó khăn là phân tích cú pháp đầu vào một cách chính xác vì các khoảng trắng tùy ý có thể xuất hiện ở hầu hết mọi nơi xung quanh từ khóa, dấu phẩy và dấu ngoặc đơn. 

Những hạn chế là khá nhỏ. Có tối đa 1000 khai báo thủ tục và tối đa 1000 lời gọi. Mỗi danh sách tham số có độ dài tối đa là 5. Việc so sánh đơn giản giữa một lệnh gọi và mọi thủ tục chỉ cần khoảng$$1000 \times 1000 \times 5 = 5 \cdot 10^6$$kiểu so sánh trong trường hợp xấu nhất. 

Năm triệu so sánh chuỗi đơn giản có thể dễ dàng được chấp nhận trong giới hạn 2 giây. Điều này có nghĩa là vấn đề chủ yếu là vấn đề triển khai và phân tích cú pháp chứ không phải là vấn đề tối ưu hóa thuật toán. 

Một số trường hợp đặc biệt có thể âm thầm phá vỡ trình phân tích cú pháp bất cẩn. 

Coi như:```
void  f ( int , T )
```Và```
f(a,b)
```Tuyên bố chứa các khoảng trắng tùy ý xung quanh mỗi mã thông báo. Trình phân tích cú pháp dựa trên các vị trí cố định hoặc phân tách đơn giản bằng dấu cách sẽ không thành công. 

Một trường hợp tế nhị khác là khớp`T`.```
void f(T)
void f(int)

int a
f(a)
```Câu trả lời đúng là:```
2
```bởi vì cả hai tuyên bố đều phù hợp. Một sai lầm phổ biến là điều trị`T`như một loại cụ thể chứ không phải là ký tự đại diện. 

Một trường hợp khác là số lượng tham số khác nhau.```
void f(T)
void f(T,T)

int a
f(a)
```Câu trả lời đúng là:```
1
```Mặc dù`T`khớp với bất cứ thứ gì thì số lượng tham số vẫn phải khớp chính xác. 

Cuối cùng, tên thủ tục có thể chứa các chữ số:```
void foo123(int)
```Trình phân tích cú pháp giả sử số nhận dạng chỉ chứa các chữ cái sẽ từ chối tên hợp lệ một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là phân tích cú pháp tất cả các khai báo, lưu trữ chúng và kiểm tra mọi cuộc gọi đối với mọi khai báo thủ tục. 

Đối với một so sánh duy nhất, chúng tôi kiểm tra ba điều. Tên phải trùng khớp. Số lượng tham số phải khớp nhau. Sau đó, mỗi vị trí tham số phải đáp ứng đẳng thức loại chính xác hoặc ký tự đại diện`T`. 

Cách tiếp cận bạo lực này rõ ràng là đúng vì nó thực hiện định nghĩa về sự phù hợp theo đúng nghĩa đen. Đối với mỗi lệnh gọi, nó sẽ kiểm tra mọi khai báo và đếm chính xác những lệnh thỏa mãn quy tắc. 

Mối quan tâm tự nhiên là hiệu quả. Với 1000 lần khai báo và 1000 lệnh gọi, chúng tôi thực hiện tối đa một triệu so sánh lệnh gọi khai báo. Vì mỗi khai báo chứa tối đa năm tham số nên tổng công việc là khoảng năm triệu lần kiểm tra tham số. Điều đó dễ dàng đủ nhanh. 

Không cần lập chỉ mục phức tạp, sơ đồ băm hoặc tìm kiếm nhị phân. Vấn đề được phân loại theo một số thẻ vì đôi khi có thể tối ưu hóa việc phân tích cú pháp và so khớp các mẫu, nhưng các ràng buộc nhất định khiến việc triển khai trực tiếp hoàn toàn đủ. 

Quan sát thực tế là số lượng tham số được giới hạn ở mức năm. Ngay cả việc quét toàn bộ tất cả các thủ tục cho mọi cuộc gọi vẫn nằm trong giới hạn một cách thoải mái. Khi chúng tôi nhận ra điều này, nhiệm vụ sẽ trở thành việc phân tích cú pháp cẩn thận và triển khai trung thực các quy tắc so khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nkL) | O(n + m) | Đã chấp nhận | 
| Tối ưu | O(nkL) | O(n + m) | Đã chấp nhận | 

Đây$L \le 5$là số lượng tham số tối đa. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các tờ khai thủ tục. 
2. Đối với mỗi khai báo, hãy xóa khoảng trắng xung quanh và phân tích cú pháp: 

- Tên thủ tục 
- danh sách thứ tự các loại tham số 
3. Lưu mỗi khai báo thành một cặp: 

- Tên thủ tục 
- danh sách loại tham số 
4. Đọc tất cả các khai báo biến. 
5. Lưu trữ ánh xạ từ tên biến tới loại biến. 
6. Đối với mỗi lệnh gọi thủ tục, hãy phân tích cú pháp: 

- tên thủ tục được gọi 
- danh sách thứ tự các tên biến 
7. Chuyển đổi tên biến thành kiểu tương ứng bằng cách sử dụng bảng biến. 
8. Khởi tạo câu trả lời cho cuộc gọi này về 0. 
9. Lặp lại mọi khai báo thủ tục lưu sẵn. 
10. Bỏ qua ngay nếu tên khác nhau. 
11. Bỏ qua ngay nếu số lượng thông số khác nhau. 
12. So sánh từng vị trí tương ứng. 

Một vị trí khớp khi kiểu khai báo là`T`hoặc khi nó bằng loại đối số. 
13. Nếu mọi vị trí đều khớp, hãy tăng câu trả lời. 
14. Xuất số đếm cuối cùng của cuộc gọi. 

### Tại sao nó hoạt động 

Đối với mỗi cuộc gọi, thuật toán sẽ kiểm tra mọi thủ tục được khai báo chính xác một lần. Một thủ tục đóng góp vào việc đếm khi và chỉ khi tất cả các điều kiện từ định nghĩa được thỏa mãn. 

Việc kiểm tra tên thực thi các tên thủ tục giống hệt nhau. Việc kiểm tra độ dài thực thi việc đếm tham số giống hệt nhau. Việc so sánh vị trí thực thi quy tắc khớp chính xác mà khai báo có chứa`T`hoặc các loại bê tông bằng nhau. 

Vì mọi khai báo đều được kiểm tra và không có khai báo nào được tính trừ khi tất cả các quy tắc đều đúng, số lượng kết quả chính xác là số lượng thủ tục phù hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_signature(line):
    line = line.strip()

    if line.startswith("void"):
        line = line[4:].strip()

    pos_l = line.find('(')
    pos_r = line.rfind(')')

    name = line[:pos_l].strip()

    inside = line[pos_l + 1:pos_r].strip()

    if inside == "":
        params = []
    else:
        params = [x.strip() for x in inside.split(',')]

    return name, params

def solve():
    n = int(input())

    procedures = []

    for _ in range(n):
        procedures.append(parse_signature(input()))

    m = int(input())

    var_type = {}

    for _ in range(m):
        parts = input().strip().split()
        var_type[parts[1]] = parts[0]

    k = int(input())

    answers = []

    for _ in range(k):
        name, vars_used = parse_signature(input())

        arg_types = [var_type[v] for v in vars_used]

        cnt = 0

        for proc_name, proc_types in procedures:
            if proc_name != name:
                continue

            if len(proc_types) != len(arg_types):
                continue

            ok = True

            for p, a in zip(proc_types, arg_types):
                if p != "T" and p != a:
                    ok = False
                    break

            if ok:
                cnt += 1

        answers.append(str(cnt))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp là phần tinh tế nhất trong quá trình triển khai. 

Cả hai khai báo và gọi đều có chung cấu trúc chung:```
name(type1,type2,...)
```Điểm khác biệt duy nhất là phần khai báo có thể bắt đầu bằng từ khóa`void`. Hàm trợ giúp sẽ loại bỏ tiền tố đó khi có mặt, sau đó trích xuất tên thủ tục và nội dung được phân tách bằng dấu phẩy giữa các dấu ngoặc đơn. 

Mỗi mã thông báo được trích xuất sẽ bị loại bỏ khoảng trắng riêng lẻ. Điều này làm cho trình phân tích cú pháp miễn nhiễm với khoảng cách tùy ý xung quanh dấu phẩy, dấu ngoặc đơn và từ khóa. 

Vòng lặp phù hợp tuân theo định nghĩa trực tiếp. Tên và số lượng tham số được kiểm tra đầu tiên vì chúng cho phép loại bỏ sớm. Chỉ khi đó chúng ta mới so sánh các vị trí tham số tương ứng. 

Quy tắc ký tự đại diện được thực hiện bởi:```
if p != "T" and p != a:
```chỉ từ chối khi tham số khai báo không phải là ký tự đại diện cũng như loại cụ thể chính xác. 

Bởi vì số lượng tham số tối đa chỉ là năm nên không cần tối ưu hóa bổ sung. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
void f(int,T)
void f(T, T)
void foo123(int,double,string,string)
void p(T,double)
3
int a
string s
double x123
5
f(a,a)
f(s,a)
foo(a,s,s)
f(s,x123)
proc(a)
```Thủ tục: 

| Thủ tục | Các loại | 
| --- | --- | 
| f | [int, T] | 
| f | [T, T] | 
| foo123 | [int, double, chuỗi, chuỗi] | 
| p | [T, gấp đôi] | 

Biến: 

| Biến | Loại | 
| --- | --- | 
| một | int | 
| s | chuỗi | 
| x123 | đôi | 

Gọi`f(a,a)`: 

| Ứng viên | Trận đấu | 
| --- | --- | 
| f(int,T) | Có | 
| f(T,T) | Có | 

Trả lời = 2. 

Gọi`f(s,a)`: 

| Ứng viên | Trận đấu | 
| --- | --- | 
| f(int,T) | Không | 
| f(T,T) | Có | 

Trả lời = 1. 

Gọi`foo(a,s,s)`: 

| Ứng viên | Trận đấu | 
| --- | --- | 
| foo123(...) | Tên không khớp | 

Trả lời = 0. 

Dấu vết này cho thấy tên thủ tục phải khớp chính xác và`T`hoạt động như một ký tự đại diện. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
void g(T)
void g(int)
void g(string)
2
int a
string b
2
g(a)
g(b)
```Vì`g(a)`: 

| Ứng viên | Các loại tham số | Trận đấu | 
| --- | --- | --- | 
| g(T) | [T] | Có | 
| g(int) | [int] | Có | 
| g(chuỗi) | [chuỗi] | Không | 

Trả lời = 2. 

cho`g(b)`: 

| Ứng viên | Các loại tham số | Trận đấu | 
| --- | --- | --- | 
| g(T) | [T] | Có | 
| g(int) | [int] | Không | 
| g(chuỗi) | [chuỗi] | Có | 

Trả lời = 2. 

Ví dụ này chứng minh rằng nhiều khai báo có thể khớp với cùng một lệnh gọi cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nkL) | Mọi cuộc gọi đều kiểm tra mọi thủ tục, mỗi thủ tục có tối đa L ≤ 5 so sánh tham số | 
| Không gian | O(n + m) | Thủ tục lưu trữ và bảng biến | 

Với$n \le 1000$,$k \le 1000$, Và$L \le 5$, tổng công việc tối đa là khoảng năm triệu so sánh tham số. Điều này thoải mái phù hợp trong thời hạn. Việc sử dụng bộ nhớ rất nhỏ vì chỉ có các khai báo và ánh xạ kiểu biến được lưu trữ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    def parse_signature(line):
        line = line.strip()

        if line.startswith("void"):
            line = line[4:].strip()

        l = line.find("(")
        r = line.rfind(")")

        name = line[:l].strip()

        inside = line[l + 1:r].strip()

        if inside:
            params = [x.strip() for x in inside.split(",")]
        else:
            params = []

        return name, params

    n = int(input())
    procedures = [parse_signature(input()) for _ in range(n)]

    m = int(input())
    vars_ = {}

    for _ in range(m):
        t, v = input().split()
        vars_[v] = t

    k = int(input())
    ans = []

    for _ in range(k):
        name, vars_used = parse_signature(input())
        arg_types = [vars_[v] for v in vars_used]

        cur = 0

        for pname, ptypes in procedures:
            if pname != name:
                continue

            if len(ptypes) != len(arg_types):
                continue

            ok = True

            for p, a in zip(ptypes, arg_types):
                if p != "T" and p != a:
                    ok = False
                    break

            if ok:
                cur += 1

        ans.append(str(cur))

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return "\n".join(ans)

# sample 1
assert run(
"""4
void f(int,T)
void f(T, T)
void foo123(int,double,string,string)
void p(T,double)
3
int a
string s
double x123
5
f(a,a)
f(s,a)
foo(a,s,s)
f(s,x123)
proc(a)
"""
) == "2\n1\n0\n1\n0"

# minimum case
assert run(
"""1
void f(T)
1
int a
1
f(a)
"""
) == "1"

# parameter count mismatch
assert run(
"""2
void f(T)
void f(T,T)
1
int a
1
f(a)
"""
) == "1"

# wildcard and exact match together
assert run(
"""3
void g(T)
void g(int)
void g(string)
2
int a
string b
2
g(a)
g(b)
"""
) == "2\n2"

# many spaces
assert run(
"""1
  void   abc123 (  int , T )   
2
int x
double y
1
 abc123 ( x , y )
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khai báo đơn với`T`| 1 | Phiên bản hợp lệ tối thiểu | 
| Số lượng tham số khác nhau | 1 | Kết hợp chính xác | 
| Các loại chính xác cộng với ký tự đại diện | 2, 2 | Nhiều khai báo phù hợp | 
| Khoảng trắng quá mức | 1 | Phân tích cú pháp mạnh mẽ | 
| Đầu vào mẫu | 2,1,0,1,0 | Hành vi vấn đề đầy đủ | 

## Vỏ cạnh 

Xem xét các khai báo với số lượng tham số khác nhau:```
2
void f(T)
void f(T,T)
1
int a
1
f(a)
```Cuộc gọi có một đối số. Tuyên bố đầu tiên tồn tại sau khi kiểm tra độ dài. Tuyên bố thứ hai bị từ chối ngay lập tức vì nó mong đợi hai tham số. Các đầu ra thuật toán:```
1
```cái nào đúng. 

Xem xét kết hợp ký tự đại diện:```
2
void f(T)
void f(string)
1
int a
1
f(a)
```Danh sách loại cuộc gọi là`[int]`. 

Vì`f(T)`, việc so sánh thành công vì`T`chấp nhận bất kỳ loại. 

Vì`f(string)`, việc so sánh thất bại vì`string != int`. 

Câu trả lời là:```
1
```phù hợp với quy luật ngôn ngữ. 

Hãy xem xét tên có chứa chữ số:```
1
void foo123(int)
1
int a
1
foo123(a)
```Trình phân tích cú pháp trích xuất`foo123`làm tên mà không áp đặt bất kỳ hạn chế nào chỉ về chữ cái. Tuyên bố khớp với cuộc gọi, tạo ra:```
1
```Điều này xác nhận rằng việc phân tích cú pháp mã định danh xử lý toàn bộ các tên hợp lệ.
