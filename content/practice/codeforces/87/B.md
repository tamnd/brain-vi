---
title: "CF 87B - Vasya và các loại"
description: "Ngôn ngữ trong bài toán này chỉ có hai loại cơ sở thực sự là void và errtype. Mọi loại khác được xác định thông qua typedef và mọi truy vấn đều yêu cầu chúng tôi đánh giá một biểu thức loại bằng typeof. Một biểu thức kiểu được xây dựng từ tên cơ sở cộng với một số toán tử và &."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 87
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 73 (Div. 1 Only)"
rating: 1800
weight: 87
solve_time_s: 116
verified: true
draft: false
---

[CF 87B - Vasya và các loại](https://codeforces.com/problemset/problem/87/B) 

**Đánh giá:** 1800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ngôn ngữ trong bài toán này chỉ có hai loại cơ sở thực,`void`Và`errtype`. Mọi loại khác được xác định thông qua`typedef`và mọi truy vấn đều yêu cầu chúng ta đánh giá một biểu thức kiểu với`typeof`. 

Một biểu thức kiểu được xây dựng từ tên cơ sở cộng với một số`*`Và`&`các nhà khai thác. Các quy tắc có chủ ý khác với C. 

Đang bổ sung`*`có nghĩa là “con trỏ tới”. Đang bổ sung`&`có nghĩa là “sự phản đối”. Hội thảo giảm độ sâu con trỏ xuống một. Nếu chúng ta cố gắng dereference đơn giản`void`, kết quả trở thành`errtype`. 

Loại đặc biệt`errtype`đang hấp thụ. Một khi một biểu thức trở thành`errtype`, thêm`*`hoặc`&`giữ nó bằng`errtype`. 

Chi tiết chính là mức độ ưu tiên của nhà điều hành. Dereference có mức độ ưu tiên thấp hơn so với việc tạo con trỏ. Điều đó có nghĩa là:```
&T* = T
```không```
(&T)*
```Vì vậy, khi chúng tôi phân tích một biểu thức kiểu, tất cả`*`về mặt khái niệm các toán tử xảy ra trước tất cả`&`các nhà khai thác. 

Đầu vào là một chuỗi các thao tác được xử lý theo thứ tự. MỘT`typedef A B`hoạt động lưu trữ ý nghĩa hiện tại của`A`vào tên`B`. Định nghĩa lại trong tương lai về tên được sử dụng bên trong`A`không ảnh hưởng hồi tố`B`. 

MỘT`typeof A`truy vấn yêu cầu chúng tôi đánh giá`A`ngay bây giờ và in dạng chuẩn của nó:```
void*****
```hoặc`errtype`. 

Những hạn chế là rất nhỏ. Có tối đa 100 phép toán và mỗi chuỗi toán tử chứa tối đa 10 phép toán rõ ràng`*`Và`&`biểu tượng. Ngay cả khi chuỗi typedef trở nên dài thì tổng khối lượng công việc vẫn rất nhỏ. Bất kỳ giải pháp nào có xử lý tuyến tính hoặc bậc hai cho mỗi truy vấn đều dễ dàng phù hợp. 

Phần khó khăn không phải là hiệu suất, mà là mô hình hóa hệ thống kiểu một cách trung thực. 

Một lỗi phổ biến là đánh giá các toán tử từ trái sang phải thay vì tôn trọng quyền ưu tiên. 

Coi như:```
typedef void* p
typeof &p*
```Việc thực hiện bất cẩn có thể hiểu điều này là:```
(&p)*
```trở thành:```
(&void*)* = void*
```Nhưng ngôn ngữ định nghĩa:```
&p* = &(void**) = void*
```Kết quả đúng là`void*`. 

Một trường hợp tinh tế khác là ngữ nghĩa ảnh chụp nhanh typedef. 

Coi như:```
typedef void* a
typedef a b
typedef void a
typeof b
```Đầu ra đúng là:```
void*
```bởi vì`b`lưu trữ ý nghĩa của`a`tại thời điểm xác định. Nó không tham chiếu`a`một cách năng động. 

Một trường hợp cạnh nguy hiểm khác là sự lan truyền của`errtype`. 

Coi như:```
typedef &void bad
typeof bad*****
```Hội thảo`void`ngay lập tức sản xuất`errtype`và một khi điều đó xảy ra, mọi toán tử sau này vẫn để nguyên như vậy`errtype`. 

Đầu ra đúng là:```
errtype
```Một cách triển khai ngây thơ giữ độ sâu con trỏ âm hoặc cố gắng tiếp tục thao tác`void`sau thất bại có thể âm thầm đưa ra những câu trả lời sai. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là biểu diễn mọi loại theo nghĩa đen dưới dạng một chuỗi, chẳng hạn như:```
void****
```Sau đó, với mỗi truy vấn, hãy liên tục mở rộng tên typedef cho đến khi đạt được loại cụ thể. Sau đó, áp dụng từng toán tử một. 

Điều này hiệu quả vì ngữ nghĩa của ngôn ngữ rất đơn giản và các ràng buộc rất nhỏ. Ngay cả khi một loại mở rộng qua một số lớp typedef, tổng cộng chỉ có 100 thao tác. 

Vấn đề với cách trình bày này là nó khuyến khích việc phân tích cú pháp không chính xác. Quy tắc ưu tiên giữa`*`Và`&`trở nên lúng túng khi thao tác với chuỗi thô. Rất dễ vô tình xử lý các toán tử theo thứ tự văn bản thay vì thứ tự ngữ nghĩa. 

Quan sát quan trọng là mọi loại không có lỗi hợp lệ được xác định hoàn toàn bởi một số nguyên: số lớp con trỏ ở trên cùng của`void`. 

Ví dụ:```
void      -> depth 0
void*     -> depth 1
void***   -> depth 3
```Hội thảo giảm độ sâu đi một. Tạo con trỏ tăng độ sâu lên một. 

Nếu độ sâu trở thành âm, kết quả sẽ trở thành`errtype`. 

Điều này biến đổi toàn bộ vấn đề thành số học số nguyên. 

Khi phân tích một biểu thức:```
&&name***
```độ ưu tiên của toán tử có nghĩa là:```
((name***)) with two dereferences afterward
```Vì vậy chúng ta chỉ cần:```
final_depth = base_depth + stars - amps
```Nếu loại cơ sở đã có`errtype`hoặc độ sâu cuối cùng trở thành âm, kết quả là`errtype`. 

Cách biểu diễn này cũng xử lý ngữ nghĩa ảnh chụp nhanh typedef một cách tự nhiên. Khi xác định tên loại mới, chúng tôi chỉ cần lưu trữ độ sâu số nguyên được đánh giá của nó tại thời điểm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · L) | O(n · L) | Được chấp nhận nhưng dễ bị lỗi | 
| Tối ưu | O(n · L) | O(n) | Đã chấp nhận | 

Đây,`L`là độ dài biểu thức tối đa. 

## Hướng dẫn thuật toán 

1. Tạo một từ điển lưu trữ ý nghĩa của từng loại đã biết. 

Chúng tôi đại diện:```
void -> 0
errtype -> -1
```Mọi số nguyên không âm đều có nghĩa là số`*`các ký hiệu ở trên`void`. giá trị`-1`đại diện cho`errtype`. 
2. Đối với mỗi thao tác, hãy chia dòng thành các mã thông báo. 

Hoạt động là:```
typedef A B
```hoặc:```
typeof A
```3. Để đánh giá một biểu thức kiểu, hãy tách nó thành ba phần: 

hàng đầu`&`biểu tượng, 

dấu vết`*`biểu tượng, 

và tên loại lõi. 

Ví dụ:```
&&ptr***
```trở thành:```
amps = 2
name = ptr
stars = 3
```4. Tra cứu ý nghĩa hiện tại của tên loại lõi. 

Nếu tên không tồn tại, hãy coi nó như`errtype`. 
5. Nếu loại cơ sở đã có`errtype`, trở lại`errtype`ngay lập tức. 

Ngôn ngữ xác định:```
errtype* = errtype
&errtype = errtype
```6. Ngược lại tính:```
depth = base_depth + stars - amps
```Điều này khớp chính xác với quy tắc ưu tiên mà tất cả`*`toán tử áp dụng trước tất cả`&`các nhà khai thác. 
7. Nếu`depth < 0`, trở lại`errtype`. 

Độ sâu âm có nghĩa là chúng tôi đã cố gắng loại bỏ sự tham chiếu đơn giản`void`. 
8. Đối với một`typedef`, lưu trữ độ sâu được đánh giá dưới tên mới. 

Điều này lưu trữ ảnh chụp nhanh được đánh giá đầy đủ của loại tại thời điểm xác định. 
9. Đối với một`typeof`, in:```
errtype
```nếu kết quả là`-1`, nếu không thì in:```
void + '*' * depth
```### Tại sao nó hoạt động 

Điều bất biến là mọi tên loại được lưu trữ luôn ánh xạ tới dạng chuẩn được đánh giá đầy đủ của nó. 

Loại không có lỗi chỉ được biểu thị bằng độ sâu con trỏ ở trên`void`. Vì các thao tác ngôn ngữ chỉ tăng hoặc giảm độ sâu con trỏ nên cách biểu diễn này nắm bắt tất cả thông tin cần thiết. 

Ưu tiên của toán tử được giữ nguyên vì mọi biểu thức được đánh giá là:```
base + all pointers - all dereferences
```điều này hoàn toàn tương đương với quy tắc ngôn ngữ mà việc hủy đăng ký có mức độ ưu tiên thấp hơn so với việc tạo con trỏ. 

Bởi vì typedef lưu trữ đánh giá độ sâu thay vì tham chiếu tượng trưng, ​​​​việc định nghĩa lại sau này không thể ảnh hưởng đến các loại được xác định trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def evaluate(expr, types):
    amps = 0
    i = 0

    while i < len(expr) and expr[i] == '&':
        amps += 1
        i += 1

    stars = 0
    j = len(expr) - 1

    while j >= i and expr[j] == '*':
        stars += 1
        j -= 1

    name = expr[i:j + 1]

    if name not in types:
        return -1

    base = types[name]

    if base == -1:
        return -1

    depth = base + stars - amps

    if depth < 0:
        return -1

    return depth

def solve():
    n = int(input())

    types = {
        "void": 0,
        "errtype": -1
    }

    out = []

    for _ in range(n):
        parts = input().split()

        if parts[0] == "typedef":
            expr = parts[1]
            new_name = parts[2]

            types[new_name] = evaluate(expr, types)

        else:
            expr = parts[1]

            result = evaluate(expr, types)

            if result == -1:
                out.append("errtype")
            else:
                out.append("void" + "*" * result)

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`evaluate`chức năng thực hiện toàn bộ hệ thống loại. 

Đầu tiên nó được tính là dẫn đầu`&`ký hiệu và dấu vết`*`biểu tượng. Mọi thứ ở giữa là tên loại thực tế. Phương pháp phân tích cú pháp này khớp trực tiếp với ngữ pháp ngôn ngữ và tự động tôn trọng quyền ưu tiên của toán tử. 

Các cửa hàng từ điển đánh giá độ sâu thay vì các biểu thức tượng trưng. Đây là chi tiết triển khai giúp duy trì ngữ nghĩa của ảnh chụp nhanh typedef. Nếu một loại sau đó được xác định lại, các mục được lưu trữ trước đó sẽ không thay đổi vì chúng đã chứa độ sâu số nguyên cuối cùng của chúng. 

Giá trị đặc biệt`-1`đại diện cho`errtype`. Việc sử dụng một giá trị trọng điểm duy nhất làm cho tất cả các quy tắc truyền bá trở nên đơn giản. Ngay khi đánh giá đạt đến`-1`, mọi hoạt động trong tương lai vẫn giữ nguyên`-1`. 

Lỗi triển khai phổ biến nhất là xử lý các toán tử theo thứ tự văn bản. Giải pháp này tránh hoàn toàn điều đó bằng cách chuyển đổi biểu thức thành:```
base + stars - amps
```Một điểm tinh tế khác là xử lý các tên không xác định. Tuyên bố nói rằng họ ngay lập tức trở thành`errtype`, do đó người đánh giá trả về`-1`khi một cái tên không có trong từ điển. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
typedef void* ptv
typeof ptv
typedef &&ptv node
typeof node
typeof &ptv
```Dấu vết: 

| Hoạt động | Loại cơ sở | Sao | Ampe | Độ sâu kết quả | Lưu trữ / Xuất | 
| --- | --- | --- | --- | --- | --- | 
| typedef void* ptv | 0 | 1 | 0 | 1 | ptv = 1 | 
| loại ptv | 1 | 0 | 0 | 1 | trống* | 
| nút typedef &&ptv | 1 | 0 | 2 | -1 | nút = loại lỗi | 
| nút loại | lỗi | 0 | 0 | lỗi | lỗi | 
| loại &ptv | 1 | 0 | 1 | 0 | trống | 

Thao tác thứ ba thể hiện việc hủy tham chiếu quá nhiều lần.`ptv`bằng`void*`, Vì thế`&&ptv`cố gắng hủy đăng ký hai lần và giảm xuống dưới độ sâu bằng 0. 

### Ví dụ tùy chỉnh 

đầu vào:```
6
typedef void** a
typedef &a b
typedef b* c
typeof a
typeof b
typeof c
```Dấu vết: 

| Hoạt động | Loại cơ sở | Sao | Ampe | Độ sâu kết quả | Lưu trữ / Xuất | 
| --- | --- | --- | --- | --- | --- | 
| typedef void** a | 0 | 2 | 0 | 2 | a = 2 | 
| typedef &a b | 2 | 0 | 1 | 1 | b = 1 | 
| typedef b* c | 1 | 1 | 0 | 2 | c = 2 | 
| loại a | 2 | 0 | 0 | 2 | trống** | 
| loại b | 1 | 0 | 0 | 1 | trống* | 
| loại c | 2 | 0 | 0 | 2 | trống** | 

Ví dụ này xác nhận rằng typedef lưu trữ các giá trị được đánh giá.`b`trở thành`void*`, Và`c`trở thành`void**`độc lập với những thay đổi sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L) | Mỗi thao tác quét biểu thức của nó một lần | 
| Không gian | O(n) | Từ điển lưu trữ tối đa một mục nhập cho mỗi typedef | 

Đây,`L`là độ dài biểu thức tối đa. 

Với tối đa 100 thao tác và biểu thức rất ngắn, thời gian chạy rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def evaluate(expr, types):
        amps = 0
        i = 0

        while i < len(expr) and expr[i] == '&':
            amps += 1
            i += 1

        stars = 0
        j = len(expr) - 1

        while j >= i and expr[j] == '*':
            stars += 1
            j -= 1

        name = expr[i:j + 1]

        if name not in types:
            return -1

        base = types[name]

        if base == -1:
            return -1

        depth = base + stars - amps

        if depth < 0:
            return -1

        return depth

    n = int(input())

    types = {
        "void": 0,
        "errtype": -1
    }

    out = []

    for _ in range(n):
        parts = input().split()

        if parts[0] == "typedef":
            types[parts[2]] = evaluate(parts[1], types)
        else:
            result = evaluate(parts[1], types)

            if result == -1:
                out.append("errtype")
            else:
                out.append("void" + "*" * result)

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""5
typedef void* ptv
typeof ptv
typedef &&ptv node
typeof node
typeof &ptv
"""
) == "void*\nerrtype\nvoid"

# minimum input
assert run(
"""1
typeof void
"""
) == "void"

# undefined type
assert run(
"""1
typeof abc
"""
) == "errtype"

# typedef snapshot semantics
assert run(
"""4
typedef void* a
typedef a b
typedef void a
typeof b
"""
) == "void*"

# errtype propagation
assert run(
"""3
typedef &void bad
typedef bad***** x
typeof x
"""
) == "errtype"

# precedence check
assert run(
"""2
typedef void* p
typeof &p*
"""
) == "void*"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`typeof void`|`void`| Truy vấn hợp lệ nhỏ nhất | 
|`typeof abc`|`errtype`| Xử lý kiểu không xác định | 
| Xác định lại`a`sau khi xác định`b`|`void*`| Ngữ nghĩa ảnh chụp nhanh Typedef | 
| sử dụng`bad*****`sau lỗi |`errtype`| lan truyền lỗi | 
|`typeof &p*`|`void*`| Đúng thứ tự ưu tiên của toán tử | 

## Vỏ cạnh 

Xem xét các loại không xác định:```
1
typeof abc
```Người đánh giá trích dẫn:```
name = abc
```Từ`abc`vắng mặt trong từ điển, đánh giá sẽ trả về ngay lập tức`-1`, in ra là:```
errtype
```Điều này phù hợp với tuyên bố rằng các loại không xác định sẽ tự động trở thành`errtype`. 

Bây giờ hãy xem xét việc hội thảo quá mức:```
2
typedef void p
typeof &p
```

`p`có chiều sâu`0`. Truy vấn áp dụng một sự hủy đăng ký:```
0 - 1 = -1
```Độ sâu âm không hợp lệ nên kết quả trở thành:```
errtype
```Thuật toán xử lý việc này một cách rõ ràng thông qua`depth < 0`kiểm tra. 

Cuối cùng, hãy xem xét việc xác định lại typedef:```
5
typedef void* a
typedef a b
typedef void a
typeof a
typeof b
```Dấu vết là: 

| Bước | một | b | 
| --- | --- | --- | 
| sau typedef đầu tiên | 1 | chưa được xác định | 
| sau typedef thứ hai | 1 | 1 | 
| sau khi xác định lại | 0 | 1 | 

Đầu ra trở thành:```
void
void*
```bởi vì thuật toán lưu trữ độ sâu được đánh giá đầy đủ bên trong từ điển.`b`giữ giá trị cũ ngay cả sau đó`a`những thay đổi.
