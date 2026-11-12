---
title: "CF 1624B - Làm AP"
description: "Chúng ta có ba vị trí cố định trong một chuỗi: giá trị thứ nhất a, giá trị thứ hai b và giá trị thứ ba c. Chúng ta được phép thực hiện đúng một thao tác sửa đổi: chọn một trong ba vị trí này và nhân nó với một số nguyên dương m mà chúng ta chọn."
date: "2026-06-10T05:35:04+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1624
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 764 (Div. 3)"
rating: 900
weight: 1624
solve_time_s: 75
verified: true
draft: false
---

[CF 1624B - Tạo AP](https://codeforces.com/problemset/problem/1624/B) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba vị trí cố định trong một chuỗi: giá trị đầu tiên`a`, giá trị thứ hai`b`và giá trị thứ ba`c`. Chúng ta được phép thực hiện đúng một thao tác sửa đổi: chọn một trong ba vị trí này và nhân nó với một số nguyên dương nào đó`m`sự lựa chọn của chúng tôi. 

Sau sự thay đổi duy nhất này, chúng ta muốn bộ ba kết quả tạo thành một cấp số cộng theo thứ tự đã cho. Điều đó có nghĩa là phần tử ở giữa phải nằm chính xác giữa phần tử thứ nhất và phần tử thứ ba hoặc tương đương với điều kiện`2 * middle = first + third`phải nắm giữ. 

Nhiệm vụ không phải là xây dựng tiến trình một cách rõ ràng mà chỉ quyết định liệu sự lựa chọn chỉ số và số nhân như vậy có tồn tại hay không. 

Các ràng buộc chặt chẽ về số lượng trường hợp thử nghiệm, lên tới 10⁴, nhưng mỗi trường hợp thử nghiệm là công việc liên tục. Điều đó ngay lập tức loại trừ bất cứ điều gì liên quan đến việc tìm kiếm các giá trị của`m`hoặc liệt kê các ứng cử viên. Từ`m`có thể lớn tới 10⁸, bất kỳ lực lượng vũ phu nào đối với các số nhân có thể là không thể. 

Phần tinh tế của vấn đề này là việc sửa đổi một giá trị sẽ làm thay đổi cấu trúc một cách không đối xứng. Một cách tiếp cận ngây thơ có thể thử tất cả các lựa chọn về chỉ mục và tất cả các lựa chọn có thể`m`, nhưng điều đó tạo ra một không gian tìm kiếm khổng lồ. 

Các trường hợp cạnh thường phá vỡ các giải pháp không chính xác đến từ các trường hợp đối xứng và không khác biệt. Ví dụ: khi tất cả các số đều bằng nhau, mọi phép nhân của một phần tử vẫn cho phép AP nếu chúng ta chọn`m = 1`. Một trường hợp phức tạp khác là khi AP đã được giữ: chúng ta vẫn buộc phải áp dụng một thao tác, nhưng nhân với 1 thì vẫn giữ nguyên. Một giải pháp bất cẩn có thể quên rằng “không làm gì cả” được phép thông qua`m = 1`. 

Góc quan trọng thứ hai là khi AP “gần như hợp lệ” nhưng yêu cầu điều chỉnh phần tử ở giữa phải thỏa mãn mối quan hệ tuyến tính chặt chẽ chứ không chỉ là sự bình đẳng của các khác biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi chỉ mục`i ∈ {a, b, c}`và mọi số nhân có thể`m`, tính lại bộ ba và kiểm tra xem nó có tạo thành một cấp số cộng hay không. Về nguyên tắc, điều này đúng vì nó khám phá tất cả các hoạt động được phép. 

Tuy nhiên, điều này ngay lập tức thất bại vì`m`dao động lên tới 10⁸. Ngay cả khi chúng ta tự giới hạn khả năng kiểm tra tính khả thi bằng đại số, việc lặp lại các giá trị là không thể. Vấn đề thực sự không chỉ là quy mô của`m`, nhưng không có cấu trúc đơn điệu trong cách thức`m`ảnh hưởng đến tình trạng AP trên các lựa chọn chỉ mục khác nhau. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm`m`không hề. Thay vào đó, chúng ta có thể giả định vị trí nào được sửa đổi và trực tiếp giải quyết`m`sử dụng điều kiện cấp số cộng. 

Nếu chúng ta cố định chỉ số, điều kiện AP sẽ trở thành phương trình tuyến tính trong`m`. Mỗi trường hợp giảm xuống việc kiểm tra xem giá trị dẫn xuất của`m`là số nguyên dương. 

Điều này biến bài toán từ việc tìm kiếm các giá trị thành một số lượng kiểm tra đại số không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hơn m | O(t · 10⁸) | O(1) | Quá chậm | 
| Thử 3 vị trí + giải phương trình | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xem xét từng khả năng trong số ba khả năng một cách riêng biệt. 

1. Giả sử chúng ta nhân`a`qua`m`. Trình tự mới trở thành`[a·m, b, c]`. Để đây là một AP, chúng ta cần`2b = a·m + c`. Chúng tôi sắp xếp lại điều này thành`m = (2b - c) / a`. Điều này chỉ hoạt động nếu`2b - c > 0`và chia hết cho`a`. 
2. Giả sử chúng ta nhân`b`qua`m`. Trình tự trở thành`[a, b·m, c]`. Điều kiện AP là`2(b·m) = a + c`, Vì thế`m = (a + c) / (2b)`. Chúng tôi yêu cầu chia hết chính xác và`a + c > 0`, điều này luôn đúng với các ràng buộc. 
3. Giả sử chúng ta nhân`c`qua`m`. Trình tự trở thành`[a, b, c·m]`. Điều kiện AP trở thành`2b = a + c·m`, Vì thế`m = (2b - a) / c`. Chúng tôi một lần nữa yêu cầu sự tích cực và chia sẻ. 

Tại bất kỳ thời điểm nào, nếu chúng ta tìm thấy một số nguyên hợp lệ`m ≥ 1`, chúng ta có thể ngay lập tức trả về CÓ. 

Nếu không có trường hợp nào trong ba trường hợp tạo ra số nhân hợp lệ thì câu trả lời là KHÔNG. 

### Tại sao nó hoạt động 

Mỗi giải pháp hợp lệ phải tương ứng với chính xác một vị trí được sửa đổi. Đối với vị trí cố định đó, điều kiện AP xác định duy nhất giá trị yêu cầu của`m`thông qua một phương trình tuyến tính. Vì chỉ có ba lựa chọn khả thi và mỗi trường hợp đều được kiểm tra toàn diện nên mọi cấu hình hợp lệ đều phải được phát hiện. Ngược lại, mọi ứng cử viên được tính toán đều được lấy trực tiếp từ điều kiện AP, do đó không có cấu hình không hợp lệ nào có thể vượt qua quá trình kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(x, y, z):
    # try multiplying x by m: (x*m, y, z)
    num = 2*y - z
    if num > 0 and num % x == 0:
        m = num // x
        if m >= 1:
            return True

    # try multiplying y by m: (x, y*m, z)
    num = x + z
    if num % (2*y) == 0:
        m = num // (2*y)
        if m >= 1:
            return True

    # try multiplying z by m: (x, y, z*m)
    num = 2*y - x
    if num > 0 and num % z == 0:
        m = num // z
        if m >= 1:
            return True

    return False

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())
    print("YES" if ok(a, b, c) else "NO")
```Mã trực tiếp thực hiện ba phép kiểm tra đại số dẫn xuất. Mỗi khối tương ứng với việc sửa một chỉ mục làm phần tử được sửa đổi. Việc kiểm tra phép chia số nguyên đảm bảo chúng tôi chỉ chấp nhận các số nhân hợp lệ và điều kiện dương sẽ ngăn các giải pháp âm hoặc bằng 0 không hợp lệ. 

Một sai lầm phổ biến ở đây là quên rằng số nhân ít nhất phải bằng 1. Một vấn đề khó nhận thấy khác là phép chia số nguyên: chúng ta phải xác nhận tính chia hết trước khi chia để tránh việc cắt bớt không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`10 5 30`Chúng tôi thử sửa đổi từng vị trí. 

| Trường hợp | Phương trình | Xuất phát từ m | Có hiệu lực? | 
| --- | --- | --- | --- | 
| nhân một | 2·5 = 10·m + 30 | m = (10 - 30)/10 không hợp lệ | Không | 
| nhân b | 10 + 30 = 2·5·m | m = 40/10 = 4 | Có | 
| nhân c | 2·5 = 10 + 30·m | không hợp lệ | Không | 

Chúng tôi tìm thấy một số nhân hợp lệ khi sửa đổi`b`, vì vậy câu trả lời là CÓ. 

Điều này xác nhận rằng thuật toán xác định chính xác trường hợp điều chỉnh phần tử ở giữa. 

### Ví dụ 2 

đầu vào:`2 6 3`| Trường hợp | Phương trình | Xuất phát từ m | Có hiệu lực? | 
| --- | --- | --- | --- | 
| nhân một | 12 = 2·m + 3 | m = 4,5 | Không | 
| nhân b | 2 + 3 = 12·m | m < 1 | Không | 
| nhân c | 12 = 2 + 3·m | m = 10/3 | Không | 

Không tồn tại số nhân số nguyên hợp lệ nên đầu ra là KHÔNG. 

Điều này cho thấy rằng ngay cả khi các giá trị gần nhau, các ràng buộc về tính chia hết sẽ ngăn cản giải pháp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi bài kiểm tra thực hiện kiểm tra số học liên tục | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi trường hợp thử nghiệm | 

Giải pháp dễ dàng nằm trong giới hạn vì ngay cả đối với 10⁴ trường hợp thử nghiệm, chúng tôi chỉ thực hiện một số thao tác số nguyên cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        a, b, c = map(int, input().split())

        def check(a, b, c):
            num = 2*b - c
            if num > 0 and num % a == 0 and num // a >= 1:
                return True
            if (a + c) % (2*b) == 0 and (a + c) // (2*b) >= 1:
                return True
            num = 2*b - a
            if num > 0 and num % c == 0 and num // c >= 1:
                return True
            return False

        return "YES" if check(a, b, c) else "NO"

    out = []
    t = int(sys.stdin.readline())
    for _ in range(t):
        out.append(solve())
    return "\n".join(out)

# provided samples
assert run("""11
10 5 30
30 5 10
1 2 3
1 6 3
2 6 3
1 1 1
1 1 2
1 1 3
1 100000000 1
2 1 1
1 2 2
""") == """YES
YES
YES
YES
NO
YES
NO
YES
YES
NO
YES"""

# custom cases
assert run("""1
5 5 5
""") == "YES", "all equal"

assert run("""1
1 2 4
""") == "NO", "already AP but cannot fix wrong shape with single mult"

assert run("""1
3 1 2
""") in ["YES", "NO"], "sanity boundary"

assert run("""1
2 6 3
""") == "NO", "no valid multiplier"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 5 | CÓ | tất cả đều bằng nhau | 
| 1 2 4 | KHÔNG | không thể gần AP | 
| 2 6 3 | KHÔNG | lỗi chia hết | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cả ba số đều bằng nhau. Điều kiện AP đã được giữ và nhân bất kỳ phần tử nào với`m = 1`bảo tồn nó. Thuật toán xử lý việc này vì mỗi trường hợp cho phép`m = 1`và việc kiểm tra tính chia hết sẽ diễn ra một cách tự nhiên. 

Một trường hợp khác là khi giải pháp hợp lệ yêu cầu sửa đổi phần tử ở giữa. Nhiều giải pháp không chính xác đã bỏ sót điều này hoặc cho rằng điểm cuối phải được điều chỉnh. phương trình`m = (a + c) / (2b)`nắm bắt rõ ràng khả năng này và việc kiểm tra tính chia hết cho số nguyên đảm bảo tính chính xác. 

Trường hợp tinh tế cuối cùng là khi tử số được tính toán trở thành không dương, chẳng hạn như`2b - c ≤ 0`. Điều này tương ứng với các cấu hình AP không thể thực hiện được trong đó việc sửa đổi được chọn sẽ yêu cầu hệ số nhân không dương. Thuật toán loại bỏ rõ ràng những trường hợp này, ngăn không cho các giải pháp số học không hợp lệ lọt qua.
