---
title: "CF 102367A - Phân phối bánh"
description: "Chúng ta cần cắt một chiếc bánh thành nhiều miếng có trọng lượng nguyên. Số lượng khách không được biết trước: sẽ chính xác là (A), (B) hoặc (C)."
date: "2026-08-14T02:59:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102367
codeforces_index: "A"
codeforces_contest_name: "Fall 2019 ICPC-style Waterloo Local Contest"
rating: 0
weight: 102367
solve_time_s: 82
verified: true
draft: false
---

[CF 102367A - Phân phối bánh](https://codeforces.com/problemset/problem/102367/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần cắt một chiếc bánh thành nhiều miếng có trọng lượng nguyên. Số lượng khách không được biết trước: sẽ chính xác là (A), (B) hoặc (C). Đối với mỗi khả năng trong số ba khả năng đó, mỗi phần phải có người nhận được xác định trước và sau khi phân phát tất cả các phần, mỗi khách phải nhận được tổng trọng lượng chính xác như nhau. 

Đầu ra mô tả từng phần vật lý bằng bốn con số. Đầu tiên là trọng lượng của nó. Ba số còn lại cho biết ai nhận được quân cờ đó khi có khách (A), (B) hoặc (C). Cùng một phần vật lý có thể được giao cho những người khác nhau trong ba tình huống. 

Bản thân chiếc bánh có thể có tổng trọng lượng bất kỳ từ (1) đến (10^{18}), vì vậy chúng ta có thể tự do lựa chọn tổng trọng lượng thuận tiện. Số lượng khách tối đa là ba (1000). Điều đó làm cho một công trình có (O(A+B+C)) phần hoàn toàn thực tế, trong khi một công trình có tối đa (10^9) phần thì không. Giới hạn 5000 mảnh là hạn chế chính yêu cầu chúng ta tìm kiếm một biểu diễn nén thay vì xây dựng một mảnh trên mỗi gam. 

Trường hợp cạnh hữu ích là (A=B=C=1). Đầu ra đúng chỉ có thể chứa một phần có trọng số 1 được gán cho người 1 trong cả ba trường hợp. Việc xây dựng bất cẩn dựa trên các ranh giới bên trong có thể vô tình tạo ra các phần không có trọng lượng nếu nó không loại bỏ các ranh giới trùng lặp. 

Ví dụ, với đầu vào`1 1 1`, ranh giới duy nhất là 0 và ranh giới cuối cùng là 1. Có chính xác một khoảng, do đó đầu ra là`1`theo sau là`1 1 1 1`. Việc coi ba bộ ranh giới là các mảng riêng biệt mà không trùng lặp có thể đếm sai cùng một khoảng thời gian nhiều lần. 

Một trường hợp ranh giới khác là`1 2 3`. Bội số chung nhỏ nhất là 6. Ba ranh giới chia sẻ bằng nhau là 0 và 6 cho một khách, 0, 3, 6 cho hai khách và 0, 2, 4, 6 cho ba khách. Công đoàn của họ là`0, 2, 3, 4, 6`, sản xuất bốn mảnh. Đầu ra mẫu là một sự thực hiện hợp lệ của chính xác bốn khoảng thời gian đó. Việc triển khai bất cẩn sử dụng phép chia dấu phẩy động cũng có thể xác định sai ranh giới cho các giá trị lớn, do đó mọi ranh giới phải được tính toán bằng số học số nguyên. 

Cuối cùng, các giá trị đầu vào tối đa vẫn vô hại đối với cấu trúc đã chọn. Vì (\operatorname{lcm}(A,B,C)) chia (ABC), bánh luôn có thể được chọn với trọng lượng tối đa (1000^3=10^9), thấp hơn nhiều (10^{18}). Số lượng ranh giới bên trong riêng biệt nhiều nhất là ((A-1)+(B-1)+(C-1)), do đó số lượng mảnh nhiều nhất là (A+B+C-2), nhiều nhất là 2998. Do đó, giới hạn 5000 mảnh vẫn còn dư chỗ đáng kể. 

## Phương pháp tiếp cận 

Cách xây dựng đơn giản là chọn trọng lượng bánh là (L=\operatorname{lcm}(A,B,C)) và biến mỗi gram thành một phần riêng biệt. Có (L) mảnh đơn vị. Đối với kịch bản (A)-khách, chỉ định (L/A) các đơn vị liên tiếp cho mỗi khách. Thực hiện tương tự cho (B) và (C). Điều này đúng vì (L) chia hết cho cả ba số lượng khách, do đó mỗi khách nhận được chính xác (L/A), (L/B) hoặc (L/C) gram tương ứng. 

Vấn đề là số lượng miếng. Trong trường hợp xấu nhất (L) có thể gần bằng (10^9). Ví dụ: (A=997), (B=998) và (C=999) có bội số chung nhỏ nhất (994010994). Do đó, một cấu trúc đơn vị sẽ cần 994010994 dòng đầu ra, vượt xa giới hạn 5000 và vượt xa những gì có thể được xử lý trong một giây. 

Quan sát quan trọng là chúng ta không thực sự cần một phần riêng biệt cho mỗi gam. Đối với số lượng khách cố định, hãy tưởng tượng chiếc bánh là một khoảng liên tục từ 0 đến (L). Nếu có (A) khách, khách 1 cần khoảng từ 0 đến (L/A), khách 2 cần khoảng tiếp theo, v.v. Nơi duy nhất mà người nhận có thể thay đổi là ranh giới 

[ 
0,\frac{L}{A},\frac{2L}{A},\ldots,L. 
] 

Làm tương tự với (B) và (C). Bây giờ hãy lấy sự kết hợp của tất cả các ranh giới này và chỉ cắt chiếc bánh ở đó. 

Mỗi phần kết quả nằm hoàn toàn bên trong một khoảng (A)-khách, một khoảng (B)-khách và một khoảng (C)-khách. Do đó, chúng tôi có thể chỉ định toàn bộ nội dung cho người nhận tương ứng trong mỗi trường hợp. Các mảnh chính xác là sự sàng lọc chung của ba phân vùng bằng nhau. 

Có nhiều nhất (A-1), (B-1) và (C-1) ranh giới bên trong, vì vậy sau khi kết hợp chúng, có nhiều nhất (A+B+C-2) phần cắt bên trong và nhiều nhất (A+B+C-1) phần. Với cả ba giá trị nhiều nhất là 1000, đây là tối đa 2999 miếng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(\tên toán tử{lcm}(A,B,C))) | (O(\tên toán tử{lcm}(A,B,C))) | Quá chậm | 
| Tối ưu | (O((A+B+C)\log(A+B+C))) | (O(A+B+C)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán (L=\operatorname{lcm}(A,B,C)). Điều này mang lại một chiếc bánh có tổng trọng lượng có thể được chia đều cho bất kỳ ai trong số ba lượng khách có thể có. 
2. Tạo một tập hợp chứa 0 và (L). Đây là hai đầu của chiếc bánh. 
3. Với mọi số nguyên (i) từ 1 đến (A-1), hãy chèn (i(L/A)) vào tập hợp. Đây chính xác là những nơi người nhận thay đổi khi có (A) khách. 
4. Lặp lại quy trình tương tự cho (B) và (C), chèn (i(L/B)) và (i(L/C)). Vì (L) chia hết cho cả ba giá trị nên mọi ranh giới đều là số nguyên. 
5. Sắp xếp tập ranh giới kết quả. Các giá trị liên tiếp (x<y) xác định một miếng bánh vật lý có trọng lượng (y-x). Các ranh giới trùng lặp đã được tập hợp loại bỏ nên mọi trọng số đều dương. 
6. Đối với mỗi khoảng bắt đầu tại (x), hãy tính người nhận của nó theo từng kịch bản. Với (A) khách, khoảng chứa (x) thuộc về khách 

[ 
\left\lfloor\frac{x}{L/A}\right\rfloor+1. 
] 

Các công thức tương ứng cho (B) và (C) cho hai người nhận còn lại. Bởi vì (x) bản thân nó là một ranh giới của hợp, nên khoảng không thể vượt qua một ranh giới khác thuộc bất kỳ kịch bản nào. 
7. Ghi trọng lượng của từng kiện hàng theo sau là ba chỉ số người nhận. Tổng trọng lượng chính xác là (L) và số lượng mảnh dưới 5000.

### Tại sao nó hoạt động 

Điều bất biến là mọi phần được tạo ra đều nằm hoàn toàn trong một khoảng chia sẻ bằng nhau cho mỗi trong số ba số lượng khách có thể có. Hãy xem xét phân vùng (A)-guest. Tất cả các ranh giới của nó đều được bao gồm trong tập hợp ranh giới toàn cầu của chúng tôi, vì vậy không phần nào được tạo có thể vượt qua ranh giới (A)-khách. Do đó, mỗi phần thuộc về chính xác một khoảng (A)-khách và chúng tôi gán nó cho khách của khoảng đó. Vì các khoảng đó đều có trọng lượng (L/A), nên mỗi khách (A) đều nhận được chính xác (L/A) gam. Lập luận tương tự áp dụng độc lập cho (B) và (C). Các mảnh là dương vì chúng được hình thành từ các ranh giới được sắp xếp riêng biệt và tổng trọng lượng của chúng là (L). 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def lcm(a, b):
    return a // gcd(a, b) * b

def solve():
    A, B, C = map(int, input().split())

    L = lcm(lcm(A, B), C)

    cuts = {0, L}

    part_a = L // A
    part_b = L // B
    part_c = L // C

    for i in range(1, A):
        cuts.add(i * part_a)

    for i in range(1, B):
        cuts.add(i * part_b)

    for i in range(1, C):
        cuts.add(i * part_c)

    cuts = sorted(cuts)

    ans = []

    for i in range(len(cuts) - 1):
        left = cuts[i]
        right = cuts[i + 1]
        weight = right - left

        a = left // part_a + 1
        b = left // part_b + 1
        c = left // part_c + 1

        ans.append((weight, a, b, c))

    out = [str(len(ans))]
    out.extend(f"{w} {a} {b} {c}" for w, a, b, c in ans)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính trọng lượng chung của bánh. Sự lồng nhau`lcm`cuộc gọi an toàn vì số nguyên Python có độ chính xác tùy ý, mặc dù trong vấn đề này, giá trị kết quả đã ở dưới (10^9). 

Ba`part_*`biến là trọng lượng chính xác mà mỗi khách phải nhận. Chúng cũng đóng vai trò là độ rộng của các khoảng bằng nhau trong ba trường hợp có thể xảy ra. 

các`cuts`bộ là phần trung tâm của công trình. Mọi vị trí có thể thay đổi của người nhận đều được chèn chính xác một lần. Việc sử dụng một tập hợp rất quan trọng vì cùng một ranh giới có thể xuất hiện ở nhiều phân vùng. Ví dụ: khi (A=2) và (B=4), điểm giữa (L/2) cũng là ranh giới sau hai trong số bốn khoảng thời gian (B)-khách. 

Sau khi sắp xếp, các vết cắt liên tiếp sẽ mô tả các phần thực tế. biểu thức`right - left`luôn dương vì các phần cắt trùng lặp đã bị loại bỏ. 

Tính toán người nhận sử dụng phép chia số nguyên thay vì dấu phẩy động. Nếu như`left`nằm trong khoảng (j)-th của phân vùng (A), thì`left // part_a`là (j-1), do đó việc thêm một sẽ chuyển đổi số khoảng dựa trên 0 thành chỉ số một người được yêu cầu. 

Không có vấn đề tràn trong Python. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, số nguyên 64 bit là đủ vì bài toán cho phép rõ ràng tổng số lên tới (10^{18}) và cấu trúc ở đây nhỏ hơn nhiều so với số đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`1 2 3`, trọng lượng bánh thông thường là 

[ 
L=\tên toán tử{lcm}(1,2,3)=6. 
] 

Ba chiều rộng phân vùng là 6, 3 và 2. 

| trái | đúng | cân nặng | Người nhận | Người nhận B | Người nhận C | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 1 | 1 | 1 | 
| 2 | 3 | 1 | 1 | 1 | 2 | 
| 3 | 4 | 1 | 1 | 2 | 2 | 
| 4 | 6 | 2 | 1 | 2 | 3 | 

Đối với một vị khách, người đó nhận được đủ sáu gram. Đối với hai khách, người 1 nhận hai miếng đầu tiên với tổng số tiền là 3 gam, còn người 2 nhận hai miếng cuối cùng với tổng số 3 gam. Đối với ba khách, ba nhóm có trọng số hai, hai và hai. 

Dấu vết cũng chứng minh tại sao ranh giới toàn cầu là đủ. Không có khoảng nào vượt qua bất kỳ ranh giới nào từ bất kỳ phân vùng nào trong ba phân vùng. 

### Dấu vết tùy chỉnh:`2 3 4`đây 

[ 
L=\tên toán tử{lcm}(2,3,4)=12. 
] 

Độ rộng phân vùng là 6, 4 và 3. Sự kết hợp các ranh giới của chúng là`0, 3, 4, 6, 8, 9, 12`. 

| trái | đúng | cân nặng | Người nhận | Người nhận B | Người nhận C | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 1 | 1 | 1 | 
| 3 | 4 | 1 | 1 | 1 | 2 | 
| 4 | 6 | 2 | 1 | 2 | 2 | 
| 6 | 8 | 2 | 2 | 2 | 3 | 
| 8 | 9 | 1 | 2 | 3 | 3 | 
| 9 | 12 | 3 | 2 | 3 | 4 | 

Đối với hai khách, tổng số ba quân đầu tiên (3+1+2=6) và tổng số ba quân cuối cùng (2+1+3=6). Đối với ba khách, tổng số là (4), (4) và (4). Đối với bốn khách, tổng số là (3), (3), (3) và (3). 

Ví dụ chứng minh rằng các mảnh vật lý không cần phải tạo thành các mảnh có kích thước bằng nhau. Kích thước của chúng chỉ cần căn chỉnh với ranh giới của mọi phân vùng bằng nhau có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((A+B+C)\log(A+B+C))) | Nhiều nhất các ranh giới (A+B+C-1) được chèn và sắp xếp | 
| Không gian | (O(A+B+C)) | Tập ranh giới và đầu ra chứa các phần tử (O(A+B+C)) | 

Vì (A,B,C\le1000) nên có ít hơn 3000 sản phẩm được sản xuất. Việc xây dựng chỉ thực hiện vài nghìn phép tính số học và một phép sắp xếp nhỏ, do đó, nó dễ dàng nằm trong giới hạn thời gian 1 giây đã nêu và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Đầu ra văn bản chính xác của một vấn đề mang tính xây dựng không được xác định một cách duy nhất bởi vấn đề đó. Các thử nghiệm sau đây sử dụng cách triển khai xác định ở trên, sau đó xác thực các điều kiện cấu trúc mà mọi đầu ra được chấp nhận phải đáp ứng.```python
import sys
import io
from math import gcd

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        A, B, C = map(int, sys.stdin.readline().split())

        def lcm(a, b):
            return a // gcd(a, b) * b

        L = lcm(lcm(A, B), C)

        cuts = {0, L}
        pa = L // A
        pb = L // B
        pc = L // C

        for i in range(1, A):
            cuts.add(i * pa)
        for i in range(1, B):
            cuts.add(i * pb)
        for i in range(1, C):
            cuts.add(i * pc)

        cuts = sorted(cuts)

        ans = []
        for i in range(len(cuts) - 1):
            x = cuts[i]
            y = cuts[i + 1]
            ans.append((
                y - x,
                x // pa + 1,
                x // pb + 1,
                x // pc + 1
            ))

        out = [str(len(ans))]
        out.extend(f"{w} {a} {b} {c}" for w, a, b, c in ans)
        return "\n".join(out)
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, output: str):
    A, B, C = map(int, inp.split())
    lines = output.strip().splitlines()

    K = int(lines[0])
    assert K == len(lines) - 1
    assert 1 <= K <= 5000

    pieces = []
    total = 0

    for line in lines[1:]:
        w, a, b, c = map(int, line.split())
        assert w > 0
        assert 1 <= a <= A
        assert 1 <= b <= B
        assert 1 <= c <= C
        pieces.append((w, a, b, c))
        total += w

    assert 1 <= total <= 10**18

    for count, column in ((A, 1), (B, 2), (C, 3)):
        sums = [0] * (count + 1)

        for w, a, b, c in pieces:
            person = (a, b, c)[column - 1]
            sums[person] += w

        assert len(set(sums[1:])) == 1

def run(inp: str) -> str:
    out = solve_data(inp)
    validate(inp, out)
    return out

# Provided sample
sample = run("1 2 3")
assert sample == (
    "4\n"
    "2 1 1 1\n"
    "1 1 1 2\n"
    "1 1 2 2\n"
    "2 1 2 3"
), "sample 1"

# Minimum-size input
out = run("1 1 1")
assert out == "1\n1 1 1 1", "minimum case"

# All values equal
out = run("7 7 7")
assert out == "7\n" + "\n".join(
    f"1 {i} {i} {i}" for i in range(1, 8)
), "all equal"

# Boundary-heavy case
out = run("2 3 4")
assert out == (
    "6\n"
    "3 1 1 1\n"
    "1 1 1 2\n"
    "2 1 2 2\n"
    "2 2 2 3\n"
    "1 2 3 3\n"
    "3 2 3 4"
), "boundary case"

# Maximum values
out = run("1000 999 997")
validate("1000 999 997", out)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3`| Bốn mảnh phù hợp với mẫu | Cung cấp mẫu và ranh giới phân vùng chồng chéo | 
|`1 1 1`| Một mảnh nặng 1 | Ranh giới trùng lặp và giá trị tối thiểu | 
|`7 7 7`| Bảy phần đơn vị, mỗi phần được gán cho cùng một chỉ mục trong mọi tình huống | Cả ba phân vùng đều trùng nhau | 
|`2 3 4`| Sáu quân có ranh giới 0, 3, 4, 6, 8, 9, 12 | Một số cách sắp xếp ranh giới khác nhau | 
|`1000 999 997`| Bất kỳ đầu ra nào đáp ứng trình xác nhận | Giá trị đầu vào lớn và giới hạn số lượng mảnh | 

## Vỏ cạnh 

cho`1 1 1`, bội số chung nhỏ nhất là 1 và cả ba vòng lặp tạo ra ranh giới bên trong đều trống. Bộ ranh giới đơn giản là`{0, 1}`. Khoảng duy nhất có trọng số là 1 và cả ba chỉ số người nhận đều là 1. Thuật toán không thể tạo ra phần có trọng số bằng 0 vì nó sắp xếp một tập hợp chứ không phải một danh sách chứa các bản sao lặp lại của cùng một ranh giới. 

Vì`1 2 3`, cái bánh có trọng lượng là 6. Phân vùng (A) không có vết cắt bên trong, phân vùng (B) có vết cắt ở 3, và phân vùng (C) có vết cắt ở 2 và 4. Công đoàn đưa ra`0, 2, 3, 4, 6`, do đó, bốn trọng số đầu ra là 2, 1, 1 và 2. Khách duy nhất (A) nhận được tất cả bốn phần, trong khi hai kịch bản còn lại chia các phần giống nhau đó theo ranh giới riêng của chúng. 

Vì`2 3 4`, chiếc bánh có trọng lượng 12. Các ranh giới (A) là 0, 6, 12, các ranh giới (B) là 0, 4, 8, 12 và các ranh giới (C) là 0, 3, 6, 9, 12. Các ranh giới kết hợp tạo ra sáu miếng. Nhiệm vụ của người nhận của họ được xác định bằng cách chia số nguyên theo chiều rộng 6, 4 và 3, vì vậy mỗi khách sẽ nhận được chính xác 6, 4 hoặc 3 gram trong trường hợp tương ứng. 

Đối với trường hợp lớn`1000 999 997`, bội số chung nhỏ nhất nằm dưới (10^9), nhưng thuật toán không bao giờ tạo ra (10^9) phần. Nó tạo ra tối đa (999+998+996+1=2994) vị trí ranh giới trước khi loại bỏ trùng lặp và do đó tạo ra nhiều nhất là 2993 phần. Mỗi ranh giới là bội số nguyên chính xác của một trong ba chiều rộng phân vùng, do đó không cần số học dấu phẩy động ngay cả ở các giá trị đầu vào lớn nhất được phép.
