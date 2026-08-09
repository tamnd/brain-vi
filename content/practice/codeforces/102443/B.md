---
title: "CF 102443B - Chặn tầm nhìn"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta có hai đoạn thẳng không giao nhau, được gọi là (a) và (b), cùng với một vectơ chỉ hướng khác 0 (vec v). Chúng ta cần quyết định xem một số điểm (A) trên (a) có thể di chuyển từ (A) theo hướng (vec v) và chạm vào một điểm (B) nào đó trên (b) hay không."
date: "2026-08-08T12:46:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "B"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 123
verified: true
draft: false
---

[CF 102443B - Chặn chế độ xem](https://codeforces.com/problemset/problem/102443/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng ta có hai đoạn thẳng không giao nhau, được gọi là (a) và (b), cùng với một vectơ chỉ hướng khác 0 (\vec v). Chúng ta cần quyết định xem một số điểm (A) trên (a) có thể di chuyển từ (A) theo hướng (\vec v) và chạm vào một điểm (B) nào đó trên (b) hay không. 

Tương tự, chúng ta cần các điểm (A\in a) và (B\in b) sao cho 

[ 
B-A=t\vec v 
] 

đối với một số (t > 0). Tính dương tuyệt đối xuất phát từ thực tế là hai đoạn thẳng không cắt nhau nên (A) và (B) không thể cùng một điểm. 

Mỗi trường hợp thử nghiệm chứa tám tọa độ cho hai điểm cuối của đoạn và hai tọa độ cho hướng xem. Có thể có tới (50.000) trường hợp thử nghiệm độc lập. Tất cả các tọa độ đều có giá trị tuyệt đối nhiều nhất là (10^6), do đó, giải pháp tổng thể (O(n)) hoặc (O(n\log n)) dễ dàng đủ nhanh, nhưng thuật toán thực hiện tìm kiếm lớn bên trong mỗi phân đoạn là không phù hợp. 

Khó khăn chính là (A) và (B) là các điểm thực tùy ý trên các đoạn. Chỉ kiểm tra các điểm cuối là không đủ. Ví dụ,```
1
0 0 4 4 1 3 3 1 1 1
```có hướng ((1,1)). Các cặp điểm cuối không đưa ra hướng mong muốn, nhưng hai đoạn chứa các điểm có cùng tọa độ vuông góc và một điểm như vậy trên đoạn thứ hai nằm trước điểm tương ứng trên đoạn đầu tiên. Giải pháp chỉ kiểm tra bốn cặp điểm cuối có thể bỏ sót điều này. 

Cái bẫy thứ hai là hướng đi. Coi như```
1
0 0 1 0 -3 0 -2 0 1 0
```Các đoạn nằm trên cùng một đường ngang nhưng đoạn thứ hai nằm sau đoạn thứ nhất khi nhìn theo hướng ((1,0)). Câu trả lời đúng là`No`. Một bài kiểm tra chỉ kiểm tra xem hai đoạn có nằm trên cùng một đường hay không sẽ trả lời sai`Yes`. 

Trường hợp cạnh thứ ba xảy ra khi một đoạn song song với hướng nhìn. Ví dụ,```
1
0 0 1 0 2 0 2 1 1 0
```có đoạn đầu tiên song song với (\vec v). Câu trả lời đúng là`Yes`, vì điểm ((1,0)) có thể di chuyển sang phải và chạm tới ((2,0)) trên đoạn thứ hai. Bất kỳ công thức nào chia cho hiệu tọa độ vuông góc của đoạn đầu tiên đều phải xử lý riêng mẫu số 0. 

Cuối cùng, các đoạn có thể chỉ có một tọa độ vuông góc chung. Ví dụ,```
1
0 0 1 0 2 0 2 1 1 0
```có đúng tình trạng này. Việc coi sự chồng chéo của các khoảng dự kiến ​​là khoảng mở thay vì khoảng đóng sẽ làm mất điểm hợp lệ tại ranh giới. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là tham số hóa cả hai phân đoạn và tìm kiếm thông qua các vị trí có thể có của (A). Đối với một (A) cố định, chúng ta có thể kiểm tra xem tia (A+t\vec v), (t\geq0), có cắt nhau (b) hay không. Khó khăn là tham số của (A) là liên tục. Vị trí lấy mẫu (K) mang lại công việc (O(K)) cho mỗi lần kiểm tra hoặc (50.000K) mẫu trong trường hợp xấu nhất. Ngay cả với (K=10^5), đó là mẫu (5\cdot10^9). Về cơ bản hơn, lấy mẫu hữu hạn không phải là một thuật toán chính xác vì điểm chặn hợp lệ có thể nằm giữa hai mẫu. 

Công thức chính xác ban đầu trông giống như một hệ thống có ba biến liên tục, vị trí trên (a), vị trí trên (b) và quãng đường di chuyển dọc theo (\vec v). Việc giải quyết hệ thống đó một cách riêng biệt cho mọi cấu hình có thể là phức tạp không cần thiết. Quan sát hữu ích là điều kiện hướng có hai phần độc lập. Độ dịch chuyển (B-A) không được có thành phần nào vuông góc với (\vec v) và thành phần của nó dọc theo (\vec v) phải dương. 

Xác định hai tọa độ cho bất kỳ điểm nào (P=(x,y)): 

[ 
q(P)=\operatorname{cross}(\vec v,P)=v_x y-v_y x 
] 

và 

[ 
p(P)=\tên toán tử{dot}(\vec v,P)=v_xx+v_yy. 
] 

Giá trị (q) đo vị trí vuông góc với hướng nhìn, đến một tỷ lệ không đổi. Giá trị (p) đo vị trí dọc theo hướng xem, cũng có tỷ lệ không đổi. 

Bây giờ giả sử (B-A=t\vec v). Sau đó 

# \operatorname{cross}(\vec v,B-A) 

# \operatorname{cross}(\vec v,t\vec v) 

1. 

] 

Vì vậy (A) và (B) phải có cùng tọa độ (q). 

Đồng thời, 

# \operatorname{dot}(\vec v,B-A) 

t|\vec v|^2. 
] 

Vì (t>0), nên chúng ta cần (p(B)>p(A)). Vì các phân đoạn được đảm bảo không giao nhau nên sự bằng nhau không thể xảy ra đối với một cặp hợp lệ, do đó việc kiểm tra (p(B)\geq p(A)) cũng an toàn. 

Các giá trị (q) của một đoạn tạo thành một khoảng vì (q) tuyến tính dọc theo đoạn đó. Do đó, hai phân đoạn chỉ có thể chặn nhau nếu các khoảng (q) của chúng trùng nhau. 

Bên trong phần chồng chéo đó, điểm của mỗi đoạn có tọa độ (q) cho trước được xác định bằng phép nội suy tuyến tính. Do đó, (p_A(q)) và (p_B(q)) là các hàm tuyến tính. Sự khác biệt của họ 

[ 
d(q)=p_B(q)-p_A(q) 
] 

cũng là tuyến tính. Hàm tuyến tính trên một khoảng kín đạt cực đại tại một trong hai điểm cuối. Do đó, chúng ta chỉ phải kiểm tra hai điểm cuối của khoảng (q) trùng nhau. 

Đây là mức giảm chính. Bài toán tồn tại liên tục trở thành hai phép so sánh chính xác của các số hữu tỷ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lấy mẫu Brute Force | (O(K)) mỗi lần kiểm tra | (O(1)) | Quá chậm và không chính xác | 
| Phương pháp chiếu tối ưu | (O(1)) mỗi lần kiểm tra | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi điểm cuối của cả hai đoạn thẳng, hãy tính tọa độ vuông góc của nó (q=\operatorname{cross}(\vec v,P)) và tọa độ song song của nó (p=\operatorname{dot}(\vec v,P)). Hai giá trị số nguyên này mô tả hoàn toàn điểm cho mục đích của bài toán này. 
2. Đối với mỗi phân đoạn, lấy giá trị tối thiểu và tối đa của hai giá trị (q) của nó. Đây chính xác là phạm vi tọa độ vuông góc được bao phủ bởi đoạn đó. 
3. Tính giao điểm của hai phạm vi (q). Nếu điểm cuối bên trái lớn hơn điểm cuối bên phải thì các phạm vi sẽ rời nhau, do đó không có cặp điểm nào có thể có tọa độ (q) bằng nhau. Câu trả lời là ngay lập tức`No`. 
4. Đặt phần chồng lấp là ([L,R]). Đối với một giá trị cụ thể (q=t) bên trong phạm vi của một phân đoạn, hãy khôi phục tọa độ (p) của nó bằng phép nội suy tuyến tính. Nếu điểm cuối của đoạn có tọa độ ((q_0,p_0)) và ((q_1,p_1)), thì 

[ 
p(t)= 
\frac{p_0(q_1-t)+p_1(t-q_0)} 
{q_1-q_0}. 
] 

Nếu (q_0=q_1), đoạn này song song với (\vec v), do đó tọa độ (q) của nó không đổi và tọa độ (p) của nó tại (q) duy nhất có liên quan chỉ đơn giản là điểm cuối (p) của nó. 

1. Đánh giá cả hai đoạn tại (q=L). So sánh tọa độ (p) của chúng. Chúng ta cần (p_B(L)\geq p_A(L)). 
2. Đánh giá cả hai phân đoạn tại (q=R) và thực hiện so sánh tương tự. Vì (p_B(q)-p_A(q)) là tuyến tính trên phần chồng lấp, nếu nó không âm ở bất kỳ đâu thì nó không âm ở một trong hai điểm cuối này. 
3. So sánh các giá trị nội suy bằng phép nhân chéo thay vì dấu phẩy động. Nếu 

[ 
p_A=\frac{n_A}{d_A}, 
\qquad 
p_B=\frac{n_B}{d_B}, 
] 

với mẫu số dương thì 

[ 
p_B\geq p_A 
] 

tương đương với 

[ 
n_Bd_A\geq n_Ad_B. 
] 

Số nguyên Python có độ chính xác tùy ý, vì vậy những sản phẩm này an toàn. 

Tại sao nó hoạt động: với mỗi tọa độ vuông góc (q) trong phần chồng lên nhau, có một điểm tương ứng trên mỗi đoạn, ngoại trừ đoạn thẳng song song với (\vec v) chỉ có một điểm có thể có (q). Điều kiện để hai điểm nằm trên cùng một tia nhìn chính xác là (p_B(q)>p_A(q)). Sự khác biệt của hai tọa độ (p) là tuyến tính theo (q), do đó, giá trị cực đại của nó trên phần chồng lấp đạt được tại (L) hoặc (R). Thuật toán kiểm tra cả hai, do đó nó tìm thấy một cặp hợp lệ chính xác khi có một cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(data):
    ax1, ay1, ax2, ay2, bx1, by1, bx2, by2, vx, vy = data

    def coords(x, y):
        # q: coordinate perpendicular to v
        # p: coordinate along v
        q = vx * y - vy * x
        p = vx * x + vy * y
        return q, p

    a0 = coords(ax1, ay1)
    a1 = coords(ax2, ay2)
    b0 = coords(bx1, by1)
    b1 = coords(bx2, by2)

    aq0, ap0 = a0
    aq1, ap1 = a1
    bq0, bp0 = b0
    bq1, bp1 = b1

    left = max(min(aq0, aq1), min(bq0, bq1))
    right = min(max(aq0, aq1), max(bq0, bq1))

    if left > right:
        return False

    def value_at(q0, p0, q1, p1, q):
        if q0 == q1:
            return p0, 1

        den = q1 - q0
        num = p0 * (q1 - q) + p1 * (q - q0)

        if den < 0:
            den = -den
            num = -num

        return num, den

    for q in (left, right):
        an, ad = value_at(aq0, ap0, aq1, ap1, q)
        bn, bd = value_at(bq0, bp0, bq1, bp1, q)

        if bn * ad >= an * bd:
            return True

    return False

def main():
    t = int(input())
    out = []

    for _ in range(t):
        data = list(map(int, input().split()))
        out.append("Yes" if solve_case(data) else "No")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`coords`Hàm thực hiện phép biến đổi tọa độ trung tâm. Tích chéo cho tọa độ vuông góc, trong khi tích chấm cho tọa độ dọc theo hướng xem. Không cần chuẩn hóa theo (|\vec v|) vì việc nhân hệ tọa độ với thang đo dương chung không làm thay đổi so sánh. 

Bốn dòng tiếp theo tính toán hai khoảng (q) và giao nhau. Các điểm cuối được bao gồm vì điểm chặn được phép là điểm cuối của một trong hai phân đoạn. 

các`value_at`Hàm thực hiện nội suy tuyến tính chính xác. Mẫu số được chuẩn hóa thành dương để tất cả các phép so sánh sau này có cùng hướng. Trường hợp đặc biệt`q0 == q1`là bắt buộc vì một đoạn song song với hướng nhìn có tọa độ vuông góc không đổi. 

Vòng lặp cuối cùng chỉ kiểm tra`left`Và`right`. Đây không phải là các điểm mẫu tùy ý. Chúng là điểm cuối của toàn bộ phạm vi tọa độ vuông góc khả thi và sự khác biệt giữa hai tọa độ song song là tuyến tính trên phạm vi đó. 

Không có số học dấu phẩy động ở bất kỳ đâu trong giải pháp. Điều này quan trọng vì hai phân đoạn có thể được phân tách bằng một sự khác biệt chính xác rất nhỏ sau khi chiếu và so sánh dấu phẩy động có thể thay đổi câu trả lời. Các giá trị trung gian lớn nhất lớn hơn nhiều so với (10^6), nhưng số nguyên của Python tự động tăng lên nên không có vấn đề tràn. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên sử dụng```
0 2 1 1 2 2 3 1 1 1
```Ở đây (\vec v=(1,1)), do đó (q=y-x) và (p=x+y). 

| Số lượng | Đoạn (a) | Đoạn (b) | 
| --- | --- | --- | 
| Điểm cuối đầu tiên ((q,p)) | ((2,2)) | ((0,4)) | 
| Điểm cuối thứ hai ((q,p)) | ((0,2)) | ((-2,4)) | 
| (q)-phạm vi | ([0,2]) | ([-2,0]) | 
| Chồng chéo | ([0,0]) | ([0,0]) | 
| (p_A(0)) | (2) | | 
| (p_B(0)) | | (4) | 
| (p_B-p_A) | | (2>0) | 
| Trả lời | |`Yes`| 

Sự chồng lấp bao gồm tọa độ vuông góc duy nhất (q=0). Tại tọa độ đó, đoạn (a) đóng góp (p=2), trong khi đoạn (b) đóng góp (p=4). Do đó, đoạn thứ hai xa hơn dọc theo hướng xem và một điểm trên (a) có thể di chuyển dọc theo ((1,1)) để tới (b). 

Trường hợp mẫu thứ hai là```
0 2 1 1 2 2 3 1 -1 -1
```Bây giờ (\vec v=(-1,-1)), cho (q=x-y) và (p=-x-y). 

| Số lượng | Đoạn (a) | Đoạn (b) | 
| --- | --- | --- | 
| Điểm cuối đầu tiên ((q,p)) | ((-2,-2)) | ((0,-4)) | 
| Điểm cuối thứ hai ((q,p)) | ((0,-2)) | ((2,-4)) | 
| (q)-phạm vi | ([-2,0]) | ([0,2]) | 
| Chồng chéo | ([0,0]) | ([0,0]) | 
| (p_A(0)) | (-2) | | 
| (p_B(0)) | | (-4) | 
| (p_B-p_A) | | (-2<0) | 
| Trả lời | |`No`| 

Các phân đoạn hình học tương tự đang được xem theo hướng ngược lại. Các tọa độ vuông góc vẫn gặp nhau, nhưng đoạn thứ hai hiện nằm sau đoạn thứ nhất trong tọa độ (p). Bài kiểm tra định hướng là điều làm thay đổi câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi trường hợp thử nghiệm thực hiện một số lượng không đổi tích chéo, tích chấm, nội suy và so sánh số nguyên | 
| Không gian | (O(1)) phụ trợ | Chỉ một số tọa độ nguyên không đổi được lưu trữ cho một trường hợp thử nghiệm | 

Với tối đa (50.000) trường hợp kiểm thử, thuật toán chỉ thực hiện một lượng số học không đổi cho mỗi trường hợp, do đó tổng công việc là tuyến tính theo số lượng trường hợp kiểm thử. Bản thân đầu vào là nguồn dữ liệu chủ yếu và giải pháp phù hợp thoải mái với giới hạn 2 giây và 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def blocking(data):
    ax1, ay1, ax2, ay2, bx1, by1, bx2, by2, vx, vy = data

    def coords(x, y):
        return vx * y - vy * x, vx * x + vy * y

    aq0, ap0 = coords(ax1, ay1)
    aq1, ap1 = coords(ax2, ay2)
    bq0, bp0 = coords(bx1, by1)
    bq1, bp1 = coords(bx2, by2)

    left = max(min(aq0, aq1), min(bq0, bq1))
    right = min(max(aq0, aq1), max(bq0, bq1))

    if left > right:
        return False

    def value_at(q0, p0, q1, p1, q):
        if q0 == q1:
            return p0, 1

        den = q1 - q0
        num = p0 * (q1 - q) + p1 * (q - q0)

        if den < 0:
            den = -den
            num = -num

        return num, den

    for q in (left, right):
        an, ad = value_at(aq0, ap0, aq1, ap1, q)
        bn, bd = value_at(bq0, bp0, bq1, bp1, q)

        if bn * ad >= an * bd:
            return True

    return False

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input_fn = sys.stdin.readline

    t = int(input_fn())
    ans = []

    for _ in range(t):
        data = list(map(int, input_fn().split()))
        ans.append("Yes" if blocking(data) else "No")

    return "\n".join(ans)

# Provided sample
sample = """\
2
0 2 1 1 2 2 3 1 1 1
0 2 1 1 2 2 3 1 -1 -1
"""
assert run(sample) == "Yes\nNo", "provided sample"

# Minimum-size case: two horizontal, disjoint segments, second is ahead.
assert run("""\
1
0 0 1 0 2 0 3 0 1 0
""") == "Yes", "minimum-size positive case"

# Same line, but the second segment is behind the first.
assert run("""\
1
0 0 1 0 -3 0 -2 0 1 0
""") == "No", "wrong direction"

# Perpendicular projections do not overlap.
assert run("""\
1
0 0 1 0 0 2 1 2 1 0
""") == "No", "disjoint perpendicular projections"

# First segment is parallel to the viewing direction.
assert run("""\
1
0 0 1 0 2 0 2 1 1 0
""") == "Yes", "parallel segment with a boundary witness"

# Equal direction components, testing a non-axis-aligned direction.
assert run("""\
1
0 0 1 1 2 2 3 3 1 1
""") == "Yes", "equal direction components"

# Maximum number of test cases.
one = "0 0 1 0 2 0 3 0 1 0\n"
large_input = "50000\n" + one * 50000
large_output = run(large_input)
assert large_output.count("Yes") == 50000, "maximum number of tests"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1 0 2 0 3 0 1 0`|`Yes`| Trường hợp dương tính kích thước tối thiểu | 
|`0 0 1 0 -3 0 -2 0 1 0`|`No`| Định hướng đúng theo hướng nhìn | 
|`0 0 1 0 0 2 1 2 1 0`|`No`| Các khoảng tọa độ vuông góc rời nhau | 
|`0 0 1 0 2 0 2 1 1 0`|`Yes`| Mẫu số nội suy bằng 0 và chồng chéo ranh giới | 
|`0 0 1 1 2 2 3 3 1 1`|`Yes`| Hướng không thẳng hàng với các thành phần bằng nhau | 
| 50.000 trường hợp hợp lệ lặp lại | 50.000`Yes`dòng | Số lượng test case tối đa | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là ranh giới tọa độ vuông góc. Coi như```
1
0 0 1 0 2 0 2 1 1 0
```Với (\vec v=(1,0)), ta có (q=y) và (p=x). Phân đoạn (a) có phạm vi (q) ([0,0]), trong khi phân đoạn (b) có phạm vi (q) ([0,1]). Sự chồng chéo của chúng chính xác là (q=0). Tại tọa độ đó, (a) có (p=0) đến (1), trong khi (b) có (p=2) nên phép so sánh thành công và câu trả lời là`Yes`. Khoảng thời gian đóng là cần thiết ở đây. 

Trường hợp cạnh thứ hai là một đoạn song song với hướng nhìn. Trong cùng một đầu vào, đoạn (a) có (q_0=q_1=0). Thủ tục nội suy sẽ phát hiện điều này trước khi chia. Tọa độ (p) của nó được lấy trực tiếp từ điểm cuối và thuật toán so sánh nó với điểm mà đoạn (b) đạt tới (q=0). Điều này mang lại`Yes`không có phép chia nào cho số 0. 

Trường hợp cạnh thứ ba là hai đoạn thẳng thẳng hàng trỏ sai thứ tự:```
1
0 0 1 0 -3 0 -2 0 1 0
```Cả hai phạm vi (q) đều là ([0,0]), do đó chỉ điều kiện vuông góc nói lên rằng có thể tồn tại sự căn chỉnh. Tuy nhiên, tại (q=0), đoạn đầu tiên có các giá trị (p) từ (0) đến (1), trong khi đoạn thứ hai có các giá trị (p) từ (-3) đến (-2). Đoạn thứ hai nằm sau đoạn thứ nhất, vì vậy (p_B<p_A) ở mọi nơi và câu trả lời là`No`. 

Trường hợp cạnh thứ tư là các hình chiếu vuông góc rời nhau hoàn toàn:```
1
0 0 1 0 0 2 1 2 1 0
```Đoạn đầu tiên có (q=0), trong khi đoạn thứ hai có (q=2). Giao điểm của các phạm vi trống, do đó thuật toán dừng trước khi nội suy và trả về`No`. Điều này tránh việc vô tình xử lý các phân đoạn có tọa độ chiếu gần đó nhưng khác nhau được căn chỉnh. 

Trường hợp cạnh thứ năm là ranh giới khoảng cách bằng 0 của phép thử hướng. Trong bài toán này, các đoạn được đảm bảo không giao nhau, vì vậy nếu (q_A=q_B) và (p_A=p_B), các điểm thực sự sẽ giống hệt nhau, mâu thuẫn với đảm bảo đầu vào. Do đó, việc triển khai có thể sử dụng`>=`trong phép so sánh cuối cùng mà không vô tình chấp nhận độ dịch chuyển có độ dài bằng 0. Đối với lớp đầu vào đã cho, mọi cặp được chấp nhận đều có độ dịch chuyển thực sự dương dọc theo (\vec v). 

Nếu bạn muốn, tôi cũng có thể biến bài này thành một bài xã luận ngắn hơn theo phong cách Codeforces với cùng bằng chứng và mã nhưng ít trình bày hơn.
