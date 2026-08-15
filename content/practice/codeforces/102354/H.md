---
title: "CF 102354H - Thách Thức Trọng Lực"
description: "Chúng ta có số lượng vệ tinh chẵn xung quanh điểm gốc. Mỗi vệ tinh được mô tả bằng một góc cực nguyên, khoảng cách từ gốc và khối lượng. Không có hai vệ tinh nào có chung một góc, vì vậy mỗi vị trí góc nguyên chứa nhiều nhất một vệ tinh."
date: "2026-08-14T12:25:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102354
codeforces_index: "H"
codeforces_contest_name: "2018-2019 Summer Petrozavodsk Camp, Oleksandr Kulkov Contest 2"
rating: 0
weight: 102354
solve_time_s: 466
verified: false
draft: false
---

[CF 102354H - Thách thức trọng lực](https://codeforces.com/problemset/problem/102354/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có số lượng vệ tinh chẵn xung quanh điểm gốc. Mỗi vệ tinh được mô tả bằng một góc cực nguyên, khoảng cách từ gốc và khối lượng. Không có hai vệ tinh nào có chung một góc, vì vậy mỗi vị trí góc nguyên chứa nhiều nhất một vệ tinh. 

Elphaba chọn một tia bắt đầu từ gốc tọa độ. Cô ấy cần mọi lực hấp dẫn dọc theo tia đó để hướng chính xác dọc theo tia đó, ở mọi khoảng cách dương tính từ gốc tọa độ. Đầu ra là tập hợp tất cả các hướng tia như vậy, được đo bằng cung giây. Vì một đường thẳng có hai tia đối nhau nên một trục đối xứng hợp lệ sẽ cho hai hướng ra cách nhau 64.800 giây cung. 

Công thức vật lý có vẻ phức tạp nhưng tính chất liên quan lại đơn giản hơn nhiều. Đối với một đường thẳng ứng cử viên đi qua điểm gốc, mọi vệ tinh ở một bên của đường thẳng phải có một vệ tinh phù hợp ở phía bên kia, có khoảng cách chính xác đến điểm gốc và cùng khối lượng. Sự phản xạ trên đường truyền phải bảo toàn cấu hình vệ tinh có trọng số hoàn chỉnh. Đây là sự giảm thiểu trung tâm của vấn đề. Quan sát tính đối xứng tương tự cũng nhằm mục đích đơn giản hóa bài toán ban đầu. 

Miền góc chỉ chứa 129.600 vị trí số nguyên. Mặc dù câu lệnh cho phép (n) tối đa (2\cdot10^5), tính duy nhất của các góc nguyên thực sự ngụ ý (n\le129600). Thuật toán bậc hai vẫn yêu cầu so sánh khoảng (1,7\cdot10^{10}) ở đầu vào lớn nhất có thể, vượt xa giới hạn hai giây. Chúng ta cần một thuật toán tuyến tính hoặc gần tuyến tính trong miền góc cố định. 

Có ba điểm dễ khiến việc triển khai sai không thành công. Đầu tiên, trục có thể nằm chính xác trên một vệ tinh. Vì```
2
1 0 1
1 64800 1
```cấu hình có trục phản xạ tại (0^\circ) và (90^\circ), nhưng trục (0^\circ) đi qua cả hai vệ tinh và bị cấm. Chỉ có dòng (90^\circ) tồn tại, cho```
2
32400.0000000
97200.0000000
```Trình kiểm tra đối xứng không loại bỏ rõ ràng các vệ tinh cố định sẽ đưa ra bốn hướng không chính xác. 

Thứ hai, trục đối xứng không nhất thiết phải có góc nguyên. Với```
2
1 0 1
1 1 1
```hai vệ tinh được phản xạ vào nhau qua đường thẳng ở (0,5) giây cung. Đầu ra đúng là```
2
0.5000000000
64800.5000000000
```Một giải pháp chỉ lưu trữ câu trả lời dưới dạng số nguyên cung giây sẽ làm mất trục này. 

Thứ ba, các góc bao quanh ở (129600). Với```
2
1 1 1
1 129599 1
```các vệ tinh đối xứng nhau về góc (0), nên câu trả lời là```
2
0.0000000000
64800.0000000000
```Việc triển khai xử lý (1) và (129599) cách xa nhau thay vì liền kề trên vòng tròn có thể bỏ lỡ tính đối xứng này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực bắt đầu từ đặc tính hình học. Đối với mọi trục phản xạ có thể, hãy phản xạ mọi vệ tinh và kiểm tra xem một vệ tinh có cùng bán kính và khối lượng có tồn tại ở góc phản xạ hay không. Có thể có (129600) vị trí trục kép và mỗi lần kiểm tra có thể kiểm tra (n) vệ tinh, cho ra (O(129600n)), tức là khoảng (1,7\cdot10^{10}) hoạt động ở kích thước tối đa. Ngay cả việc tạo các trục ứng cử viên từ các cặp vệ tinh và kiểm tra trực tiếp từng ứng cử viên cũng có cùng một nút thắt cổ chai bậc hai. 

Brute-force hoạt động vì sự phản chiếu chính xác là điều kiện chúng ta cần, nhưng nó liên tục kiểm tra gần như cùng một cấu hình. Quan sát quan trọng là miền góc là một vòng tròn nhỏ cố định có chiều dài 129.600. Chúng ta có thể đưa thông tin vệ tinh hoàn chỉnh vào một mảng được lập chỉ mục theo góc. Mỗi phần tử mảng chứa cặp ((\rho,m)), trong khi một góc trống sẽ có một điểm đánh dấu đặc biệt. 

Bây giờ bài toán trở thành tổ hợp thuần túy. Giả sử trục phản xạ có góc gấp đôi (s), nghĩa là góc thực của nó là (s/2). Một vệ tinh ở góc (x) bị phản xạ tới 

[ 
s-x \pmod {129600}. 
] 

Do đó cấu hình đối xứng chính xác khi 

[ 
A[x]=A[s-x\bmod129600] 
] 

với mọi vị trí góc (x). 

Xác định một mảng tròn đảo ngược 

[ 
B[x]=A[-x\bmod129600]. 
] 

Khi đó điều kiện đối xứng trở thành 

[ 
A[x]=B[x-s\bmod129600]. 
] 

Nói cách khác, (A) phải bằng một phép dịch chuyển theo chu kỳ của (B). Tìm mọi phép dịch chuyển tuần hoàn trong đó hai chuỗi bằng nhau là một bài toán so khớp chuỗi tuyến tính tiêu chuẩn. Chúng ta có thể sao chép (B), tìm kiếm (A) bên trong (B+B) bằng KMP và thu được mọi trục phản xạ có thể có trong thời gian (O(129600+n)). 

Sau khi tìm được trục đối xứng, ta vẫn phải thực hiện yêu cầu đường bay không chứa vệ tinh. Nếu (s) lẻ thì phương trình 

[ 
2x=s\pmod{129600} 
] 

không có nghiệm nguyên nên không có vệ tinh nào có thể nằm trên trục. Nếu (s) chẵn thì hai vị trí góc cố định là 

[ 
x=\frac{s}{2} 
] 

và 

[ 
x=\frac{s}{2}+64800. 
] 

Nếu một trong hai vị trí chứa vệ tinh thì trục đối xứng đó sẽ bị từ chối. 

Cuối cùng, một đường hợp lệ có (các) góc gấp đôi biểu thị hai hướng bay. Các góc nhân đôi của chúng là (s) và (s+129600), vì vậy các góc thực tế của chúng là (s/2) và (s/2+64800). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot129600)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n+129600)) | (O(129600)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng (A) có độ dài (L=129600). Tại vị trí (\varphi_i), lưu trữ cặp ((\rho_i,m_i)). Lưu trữ một giá trị trống đặc biệt ở mọi góc độ mà không cần vệ tinh. Cặp này phải chứa cả bán kính và khối lượng vì sự phản xạ phải bảo toàn vệ tinh thực sự chứ không chỉ là vị trí góc của nó. 
2. Xây dựng mảng tròn đảo ngược (B) bằng cách đặt 

[ 
B[x]=A[-x\bmod L]. 
] 

Ở dạng mảng, đây là (A[0]), tiếp theo là (A[L-1]), (A[L-2]), v.v. cho đến (A[1]). Việc lập chỉ mục chính xác này là thứ biến sự phản ánh thành một sự thay đổi theo chu kỳ. 

1. Xây dựng hàm tiền tố KMP cho (A). Hàm tiền tố cho phép chúng ta tìm mọi lần xuất hiện của (A) bên trong một chuỗi khác theo thời gian tuyến tính mà không cần khởi động lại phép so sánh sau khi không khớp. 
2. Quét chuỗi (B+B), nhưng chỉ xem xét các lần xuất hiện bắt đầu tại các vị trí (0,\ldots,L-1). Nếu (A) bắt đầu ở vị trí (p), thì 

[ 
A[x]=B[p+x]=A[-p-x]. 
] 

So sánh điều này với (A[x]=A[s-x]), chúng ta nhận được 

[ 
s\equiv-p\pmod L. 
] 

Vì vậy, mỗi trận đấu KMP sẽ mang lại cho một ứng cử viên góc trục gấp đôi (s=(-p)\bmod L).

1. Loại bỏ (các) ứng cử viên nếu nó chẵn và vị trí cố định (s/2) hoặc (s/2+L/2) chứa vệ tinh. Đó chính xác là những điểm nằm trên đường bay được đề xuất. 
2. Đối với mỗi (các) còn lại, hãy thêm (các) hướng bay gấp đôi và (s+L). Việc lưu trữ các góc được nhân đôi sẽ tránh hoàn toàn số học dấu phẩy động trong quá trình thuật toán và cũng xử lý chính xác các câu trả lời nửa cung giây. 
3. Sắp xếp tất cả các hướng nhân đôi và in mỗi hướng chia cho hai. Góc nhân đôi chẵn được in dưới dạng số nguyên có phần phân số bằng 0, trong khi góc nhân đôi lẻ kết thúc bằng`.5`. 

Tại sao nó hoạt động: một đường bay hợp lệ có thành phần lực hấp dẫn bằng 0 vuông góc với đường bay tại mọi điểm trên đó. Xem xét tọa độ trong đó đường ứng cử viên là trục (x). Một vệ tinh tại ((a,b)) đóng góp thành phần vuông góc tỷ lệ với 

[ 
\frac{m b}{((x-a)^2+b^2)^{3/2}}. 
] 

Để tổng này biến mất đối với mọi (x), đóng góp đơn lẻ do mọi vệ tinh ngoài trục tạo ra phải bị triệt tiêu bởi vệ tinh phản xạ tại ((a,-b)), có cùng khối lượng và do đó có cùng bán kính và cặp khối lượng. Do đó, mỗi đường hợp lệ là một trục đối xứng phản xạ của cấu hình vệ tinh có trọng số. Ngược lại, nếu cấu hình đối xứng thì mỗi cặp vệ tinh phản xạ sẽ tạo ra các lực vuông góc bằng nhau và ngược chiều trên trục, do đó tổng lực song song với trục ở mọi nơi. Bước KMP tìm thấy chính xác các đối xứng phản xạ đó và kiểm tra vị trí cố định sẽ loại bỏ chính xác các trục bị cấm chứa vệ tinh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

L = 129600
EMPTY = (-1, -1)

def solve():
    n = int(input())

    a = [EMPTY] * L

    for _ in range(n):
        rho, phi, mass = map(int, input().split())
        a[phi] = (rho, mass)

    # B[x] = A[-x mod L].
    b = [a[0]] + a[:0:-1]

    # KMP prefix function for pattern A.
    pi = [0] * L
    j = 0

    for i in range(1, L):
        while j and a[i] != a[j]:
            j = pi[j - 1]
        if a[i] == a[j]:
            j += 1
        pi[i] = j

    candidates = []

    # Search A inside B+B.
    # We only need starts p in [0, L-1], so the text needs 2L-1 elements.
    j = 0

    for i in range(2 * L - 1):
        value = b[i] if i < L else b[i - L]

        while j and value != a[j]:
            j = pi[j - 1]

        if value == a[j]:
            j += 1

        if j == L:
            p = i - L + 1
            if p < L:
                s = (-p) % L

                # If s is even, these are the two fixed angular positions.
                if s % 2 == 0:
                    x = s // 2
                    y = x + L // 2
                    if a[x] != EMPTY or a[y] != EMPTY:
                        j = pi[j - 1]
                        continue

                candidates.append(s)

            j = pi[j - 1]

    # Each reflection axis gives two opposite flight directions.
    directions = []
    for s in candidates:
        directions.append(s)
        directions.append(s + L)

    directions.sort()

    out = [str(len(directions))]
    for d in directions:
        if d & 1:
            out.append(f"{d // 2}.5000000000")
        else:
            out.append(f"{d // 2}.0000000000")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai lưu trữ toàn bộ cấu hình góc trong một mảng có độ dài 129.600. Một bộ dữ liệu ((\rho,m)) là đủ để xác định thông tin vệ tinh cần thiết cho sự phản chiếu, vì góc đã được biểu thị bằng chỉ số mảng. 

Việc xây dựng`a[0] + a[:0:-1]`xứng đáng được quan tâm. Phần tử mong muốn tại chỉ mục (x) là (A[-x\bmod L]), do đó chỉ số 0 vẫn ở phía trước và các phần tử còn lại xuất hiện theo thứ tự ngược lại. Một điều bình thường`a[::-1]`sẽ đặt (A[L-1]) ở chỉ số 0 và thay vào đó sẽ biểu thị sự phản ánh đã dịch chuyển. 

Hàm tiền tố KMP sử dụng trực tiếp đẳng thức tuple. Số nguyên Python có thể giữ bán kính và khối lượng đầu vào mà không bị tràn và không cần số học liên quan đến (\rho_i) hoặc (m_i) sau khi xây dựng mảng. 

Quét KMP sử dụng vị trí văn bản (2L-1). Một mẫu đầy đủ xuất hiện bắt đầu từ vị trí (p<L) kết thúc ở (p+L-1), do đó các vị trí xuyên qua (2L-2) là đủ. Sự chuyển đổi`s = (-p) % L`suy ra trực tiếp từ mối quan hệ giữa sự trùng khớp theo trình tự đảo ngược và sự phản chiếu. 

Bài kiểm tra điểm cố định tách biệt với bài kiểm tra tính đối xứng. Một cấu hình thực sự có thể đối xứng xung quanh một đường trong khi có các vệ tinh nằm trên đường đó. Elphaba không thể sử dụng dòng như vậy nên những ứng cử viên đó phải bị loại bỏ. 

Đầu ra được thể hiện bằng các góc nhân đôi cho đến khi định dạng cuối cùng. Điều này tránh hoàn toàn việc làm tròn dấu phẩy động. Cụ thể, một trục tại (0,5) cung giây được biểu thị bằng góc nhân đôi (1) và in chính xác như`0.5000000000`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với hai vệ tinh ở góc (0) và (64800), mảng góc chứa hai mục giống hệt nhau đối diện nhau. Mảng hình tròn đảo ngược giống hệt với mảng ban đầu nên KMP tìm thấy hai kết quả trùng khớp theo chu kỳ. 

| Xuất phát KMP (p) | Trục nhân đôi (s=(-p)\bmod L) | Vệ tinh cố định | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 0 | Góc 0 và 64800 | Không | 
| 64800 | 64800 | Không có | Có | 

Ứng cử viên (s=0) đại diện cho trục hoành, nhưng cả hai vệ tinh đều nằm trực tiếp trên trục hoành. Ứng cử viên (s=64800) đại diện cho trục tung, không có vệ tinh trên đó. Hai hướng bay của nó là (64800/2=32400) và ((64800+129600)/2=97200). 

### Ví dụ nửa cung giây 

Hãy xem xét```
2
1 0 1
1 1 1
```Trục đối xứng hợp lệ nằm giữa hai vị trí bị chiếm giữ. 

| Xuất phát KMP (p) | Trục nhân đôi | Vị trí cố định | hợp lệ | 
| --- | --- | --- | --- | 
| 129599 | 1 | Không có | Có | 

Ở đây (s=1), nên góc trục là (1/2=0,5). Hướng ngược lại của nó là (0,5+64800=64800,5). 

Dấu vết này chứng tỏ tại sao góc nhân đôi lại hữu ích. Không cần tính toán dấu phẩy động để khám phá hoặc so sánh câu trả lời nửa số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+129600)) | Việc điền chi phí mảng góc (O(n)), trong khi KMP xử lý các mảng có độ dài cố định (129600) theo thời gian tuyến tính. | 
| Không gian | (O(129600)) | Mảng góc, hàm tiền tố, ứng viên và câu trả lời đều sử dụng không gian tuyến tính trong miền góc. | 

Giá trị tối đa hiệu dụng (n) là 129.600 vì tất cả các góc đầu vào là các số nguyên riêng biệt trong phạm vi chính xác là 129.600 vị trí. Do đó, thuật toán chỉ thực hiện vài trăm nghìn phép tính mảng cộng với so sánh KMP, vừa vặn với giới hạn hai giây. 

## Trường hợp thử nghiệm```python
import sys
import io

L = 129600
EMPTY = (-1, -1)

def solve_case(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = [EMPTY] * L

    for _ in range(n):
        rho, phi, mass = map(int, input().split())
        a[phi] = (rho, mass)

    b = [a[0]] + a[:0:-1]

    pi = [0] * L
    j = 0

    for i in range(1, L):
        while j and a[i] != a[j]:
            j = pi[j - 1]
        if a[i] == a[j]:
            j += 1
        pi[i] = j

    candidates = []
    j = 0

    for i in range(2 * L - 1):
        value = b[i] if i < L else b[i - L]

        while j and value != a[j]:
            j = pi[j - 1]

        if value == a[j]:
            j += 1

        if j == L:
            p = i - L + 1

            if p < L:
                s = (-p) % L

                if s % 2 == 0:
                    x = s // 2
                    y = x + L // 2
                    if a[x] != EMPTY or a[y] != EMPTY:
                        j = pi[j - 1]
                        continue

                candidates.append(s)

            j = pi[j - 1]

    directions = []
    for s in candidates:
        directions.append(s)
        directions.append(s + L)

    directions.sort()

    out = [str(len(directions))]
    for d in directions:
        if d & 1:
            out.append(f"{d // 2}.5000000000")
        else:
            out.append(f"{d // 2}.0000000000")

    sys.stdin = old_stdin
    input = old_input

    return "\n".join(out)

# Provided sample.
sample1 = """\
2
1 0 1
1 64800 1
"""

assert solve_case(sample1) == """\
2
32400.0000000000
97200.0000000000
""", "sample 1"

# Minimum-size input with a half-arc-second symmetry axis.
case2 = """\
2
1 0 1
1 1 1
"""

assert solve_case(case2) == """\
2
0.5000000000
64800.5000000000
""", "half-arc-second axis"

# Boundary wrap-around: angles 1 and 129599 are reflections around angle 0.
case3 = """\
2
1 1 1
1 129599 1
"""

assert solve_case(case3) == """\
2
0.0000000000
64800.0000000000
""", "circular boundary"

# Four equally spaced identical satellites.
# The axes through the satellites are forbidden.
case4 = """\
4
1 0 1
1 32400 1
1 64800 1
1 97200 1
"""

assert solve_case(case4) == """\
4
16200.0000000000
48600.0000000000
81000.0000000000
113400.0000000000
""", "fourfold symmetry with forbidden axes"

# Maximum possible number of distinct angular positions.
# Every angle is occupied by an identical satellite.
# Exactly the odd doubled axes avoid all occupied fixed positions.
parts = ["129600"]
for phi in range(L):
    parts.append(f"1 {phi} 1")

max_case = "\n".join(parts) + "\n"
max_out = solve_case(max_case)
max_lines = max_out.splitlines()

assert max_lines[0] == "129600", "maximum number of valid directions"
assert len(max_lines) == 129601, "maximum output size"
assert max_lines[1] == "0.5000000000", "first maximum-case direction"
assert max_lines[-1] == "129599.5000000000", "last maximum-case direction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`vệ tinh ở góc 0 và 64800 | 2 hướng | Cung cấp mẫu và loại bỏ trục chứa vệ tinh | 
|`2`vệ tinh ở góc 0 và 1 | 0,5 và 64800,5 | Câu trả lời nửa giây | 
|`2`vệ tinh ở góc 1 và 129599 | 0 và 64800 | Bao bọc xung quanh | 
| Bốn vệ tinh giống hệt nhau ở 0, 32400, 64800, 97200 | 16200, 48600, 81000, 113400 | Nhiều đối xứng và trục cấm | 
| 129600 vệ tinh giống hệt nhau ở mọi góc độ | 129600 chỉ đường | Kích thước miền góc tối đa và kích thước đầu ra tối đa | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là cấu hình đối xứng có trục đi qua các vệ tinh. Vì```
2
1 0 1
1 64800 1
```ứng cử viên (s=0) là một đối xứng phản xạ thực sự vì cả hai vị trí chiếm giữ đều được cố định bởi sự phản xạ. Kiểm tra vị trí cố định nhìn thấy các vệ tinh ở vị trí (0) và (64800) nên ứng viên bị loại. Ứng viên còn lại (s=64800) không có vị trí chiếm cố định và tạo ra hai hướng hợp lệ (32400) và (97200). 

Trường hợp cạnh thứ hai là hướng nửa số nguyên. Vì```
2
1 0 1
1 1 1
```sự dịch chuyển theo chu kỳ phù hợp sẽ cho (s=1). Vì (s) là số lẻ nên không có vị trí góc nguyên nào thỏa mãn (2x=s) nên không có vệ tinh nào có thể nằm trên trục. Thuật toán lưu trữ hướng nhân đôi (1), sau đó in (1/2=0,5) và cũng in hướng ngược lại (64800,5). 

Vỏ cạnh thứ ba được bao bọc theo góc cạnh. Vì```
2
1 1 1
1 129599 1
```vị trí phản xạ của góc (1) quanh trục (0) là 

[ 
0-1\equiv129599\pmod{129600}. 
] 

Mảng đảo ngược hình tròn và kết hợp KMP hoạt động theo mô-đun đường tròn góc hoàn chỉnh, do đó, sự đối xứng này được tìm thấy mà không cần trường hợp đặc biệt cho các góc gần bằng 0. Các hướng kết quả là (0) và (64800). 

Trường hợp cạnh cuối cùng là một vòng tròn góc được chiếm hoàn toàn. Với một vệ tinh giống hệt nhau ở mọi góc nguyên, mọi (các) trục kép lẻ đều là một đối xứng phản xạ hợp lệ vì nó hoán đổi các vị trí nguyên theo cặp và không có vị trí nguyên cố định. Mọi chẵn đều có vị trí chiếm giữ cố định và bị cấm. Có 64.800 giá trị lẻ của (s) trong ([0,129600)), mỗi giá trị tạo ra hai hướng ngược nhau, do đó đầu ra chứa chính xác 129.600 hướng. Đây cũng là đầu ra lớn nhất có thể được miền góc cho phép.
