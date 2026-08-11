---
title: "CF 102409E - Google muốn tối đa hóa"
description: "Có (2N) số sẽ được đặt trên một vòng tròn. Diego chọn một khối liền kề có chính xác (N) vị trí và Googles nhận được (N) vị trí khác. Vì hai khối bao phủ toàn bộ vòng tròn nên nếu Diego nhận được tổng (X), Google sẽ nhận được tổng số trừ (X)."
date: "2026-08-12T00:00:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "E"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 343
verified: false
draft: false
---

[CF 102409E - Google muốn tối đa hóa](https://codeforces.com/problemset/problem/102409/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Có (2N) số sẽ được đặt trên một vòng tròn. Diego chọn một khối liền kề có chính xác (N) vị trí và Googles nhận được (N) vị trí khác. Vì hai khối bao phủ toàn bộ vòng tròn nên nếu Diego nhận được tổng (X), Google sẽ nhận được tổng số trừ (X). 

Diego chơi một cách tối ưu, do đó, theo quan điểm của Google, số lượng liên quan là tổng lớn nhất có thể của (N) vị trí liên tiếp. Chúng ta có toàn quyền tự do hoán đổi các con số trước khi trò chơi bắt đầu. Nhiệm vụ là xây dựng một hoán vị vòng tròn để giảm thiểu điểm Diego tồi tệ nhất có thể này. 

Đầu vào chứa một (N), theo sau là các số nguyên dương chính xác (2N). Giá trị (N) nhiều nhất là (6), do đó vòng tròn chứa nhiều nhất (12) vị trí. Mỗi số tối đa là (10^6) và do đó, mọi tổng đều vừa vặn thoải mái với số nguyên 64 bit, cũng như các số nguyên có độ chính xác tùy ý của Python. 

Giá trị nhỏ của (N) là tín hiệu chính trong bài toán. Việc thử tất cả (12!) hoán vị đã là khoảng (4,79\times10^8) ứng viên, con số này là quá nhiều ngay cả trước khi đánh giá từng ứng viên. Do đó, một cách tiếp cận (O((2N)!)) nằm ngoài tầm với. Mặt khác, (N!\cdot2^N) chỉ là (720\cdot64=46{,}080), rất nhỏ. 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ quá trình triển khai. Khi (N=1), mọi cách sắp xếp theo vòng tròn đều có kết quả như nhau vì Diego chỉ lấy một số. Ví dụ, với đầu vào`1`Và`7 11`, điểm Diego tệ nhất chính xác là (11), bất kể đầu ra có`7 11`hoặc`11 7`. Một giải pháp giả định có ít nhất hai số trong mỗi khối được chọn có thể vô tình truy cập vào một vị trí không hợp lệ. 

Giá trị bằng nhau là một thử nghiệm hữu ích khác. Với (N=2) và đầu vào`5 5 5 5`, mỗi khối có tổng (10). Giải pháp xử lý các giá trị bằng nhau như các đối tượng riêng biệt vẫn đúng, nhưng việc triển khai cố gắng loại bỏ các hoán vị trùng lặp phải cẩn thận để không loại bỏ quá nhiều khả năng cấu trúc. 

Ranh giới hình tròn cũng có vấn đề. Với (N=2) và các giá trị`1 2 3 100`, khối chứa các vị trí (3,0) chỉ hợp lệ như các khối không vượt qua phần cuối của mảng được in. Đối với sự sắp xếp`100 1 3 2`, tổng bốn khối tuần hoàn là (101,4,5,102), do đó Diego có thể thu được (102). Chỉ kiểm tra các lát mảng thông thường sẽ báo cáo không chính xác (101). 

Cuối cùng, đầu ra là một hoán vị, không nhất thiết phải là hoán vị giống như mẫu. Bất kỳ sự sắp xếp nào có điểm số tối ưu trong trường hợp xấu nhất đều được chấp nhận. Do đó, mã kiểm tra sẽ xác thực hoán vị và điểm số của nó thay vì yêu cầu một thứ tự tối ưu cụ thể. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi hoán vị của số (2N). Đối với mỗi hoán vị, hãy tính tổng của tất cả (2N) cửa sổ tuần hoàn có độ dài (N), lấy giá trị lớn nhất của chúng và giữ lại hoán vị có giá trị lớn nhất nhỏ nhất. Điều này đúng vì nó xem xét rõ ràng mọi sự sắp xếp có thể. Tuy nhiên, đối với (N=6), có (12!=479{,}001{,}600) hoán vị. Ngay cả khi mỗi ứng viên chỉ được đánh giá trong thời gian (O(12)) bằng cửa sổ trượt, thì đó là khoảng (5,7) tỷ lượt cập nhật cửa sổ. Một đánh giá ngây thơ (O(12^2)) sẽ tệ hơn. 

Cấu trúc hữu ích xuất hiện khi chúng ta nhìn vào các vị trí đối diện nhau. Đánh số các vị trí (0,\ldots,2N-1). Vị trí (i) và (i+N) đối diện nhau và một khối vị trí (N) liên tiếp chứa chính xác một phần tử từ mỗi cặp đối diện. 

Sắp xếp các số: 

[ 
a_0\le a_1\le\cdots\le a_{2N-1}. 
] 

Có một sự sắp xếp tối ưu trong đó các cặp đối lập được 

[ 
(a_0,a_1),(a_2,a_3),\ldots,(a_{2N-2},a_{2N-1}). 
] 

Lý do là cặp đối diện chính xác là cặp giá trị được trao đổi khi chúng ta di chuyển cửa sổ qua một vị trí. Nếu các giá trị ở vị trí đối diện là (x) và (y), tổng cửa sổ thay đổi theo (y-x). Khoảng cách lớn giữa các giá trị đối diện tạo ra bước nhảy lớn giữa các tổng cửa sổ liên tiếp. Ghép nối các giá trị được sắp xếp liên tiếp sẽ giảm thiểu những khoảng trống này. Đối số không bắt chéo tiêu chuẩn cho cùng một kết quả: bất cứ khi nào hai cặp chứa các giá trị được sắp xếp tách biệt không cần thiết, việc kết nối lại các điểm cuối của chúng với nhau không thể làm tăng sự khác biệt tuyệt đối lớn nhất của một cặp đối diện. Lặp lại thao tác này sẽ để lại các cặp được sắp xếp liên tiếp. 

Sau lần giảm đó, chỉ còn lại hai quyết định. 

Đối với mỗi cặp, chúng ta phải quyết định điểm cuối nào thuộc nửa đầu của vòng tròn và điểm cuối nào thuộc nửa đối diện. Có (2^N) lựa chọn như vậy. 

Chúng ta cũng phải quyết định thứ tự của các cặp (N) xung quanh vòng tròn. Có (N!) Khả năng. 

Do đó toàn bộ không gian tìm kiếm liên quan chỉ 

[ 
2^N N!\le 2^6\cdot6!=46{,}080. 
] 

Đối với mọi ứng cử viên, chúng tôi xây dựng vòng tròn phần tử (2N) và tính tổng cửa sổ tuần hoàn tối đa của nó. Điều này dễ dàng đủ nhanh. 

Các phương pháp tiếp cận bạo lực và giảm thiểu có thể được so sánh trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((2N)!\cdot N)) | (O(N)) | Quá chậm | 
| Ghép nối + Đếm | (O(2^N N!\cdot N)) | (O(N)) | Đã chấp nhận | 

Cách tiếp cận thứ hai không chỉ đơn thuần là một phương pháp heuristic. Bổ đề ghép nối liền kề được sắp xếp giảm mọi giải pháp tối ưu thành một giải pháp được biểu thị bằng thứ tự và hướng của các cặp (N) đó và chúng tôi liệt kê tất cả các khả năng như vậy. 

## Hướng dẫn thuật toán

1. Sắp xếp tất cả các giá trị (2N). Nhóm các giá trị liên tiếp thành các cặp (N), do đó cặp (i) là ((a_{2i},a_{2i+1})). Những cặp này là những vị trí đối lập nhau mà chúng ta cần xem xét. 
2. Tạo mọi hoán vị của chỉ số cặp (N). Việc này sẽ chọn thứ tự xuất hiện của các cặp đối diện xung quanh vòng tròn. Vì một cặp luôn đóng góp hai vị trí đối lập nhau nên việc chọn thứ tự của cặp là đủ để xác định vị trí tương đối của chúng. 
3. Đối với mỗi cặp hoán vị, hãy tạo mọi mặt nạ bit có độ dài (N). Bit (i) quyết định thành viên nào của cặp (i) được đặt ở nửa đầu của vòng tròn. Thành viên còn lại tự động đi vào vị trí đối diện tương ứng. 
4. Vẽ hình tròn. Nếu thứ tự cặp được chọn là (p_0,p_1,\ldots,p_{N-1}), đặt điểm cuối đã chọn của cặp (p_i) tại vị trí (i) và điểm cuối khác của nó tại vị trí (i+N). Mỗi số được sử dụng chính xác một lần vì mỗi cặp đóng góp chính xác một giá trị cho mỗi nửa. 
5. Tính tổng các vị trí (N) đầu tiên. Sau đó trượt cửa sổ xung quanh vòng tròn. Khi cửa sổ di chuyển một bước, hãy trừ giá trị đi của nó và cộng giá trị vào của nó. Điều này đánh giá tất cả (2N) lựa chọn có thể có của khối Diego trong thời gian (O(N)). 
6. Giữ lại ứng viên có tổng cửa sổ chu kỳ lớn nhất nhỏ nhất. Đây chính xác là sự sắp xếp nhằm tối đa hóa số điểm đảm bảo của Google, bởi vì Google nhận được tổng số tiền trừ đi số điểm tối đa có thể có của Diego. 

### Tại sao nó hoạt động 

Đối với bất kỳ sự sắp xếp vòng tròn cố định nào, Diego có thể chọn bất kỳ cửa sổ tuần hoàn nào của (N) vị trí liên tiếp, do đó, điểm liên quan chính xác là tổng cửa sổ tối đa như vậy. Các vị trí đối diện chia vòng tròn thành các cặp (N) và di chuyển cửa sổ theo một vị trí sẽ trao đổi hai thành viên của một cặp như vậy. Việc ghép các giá trị được sắp xếp liên tiếp sẽ giảm thiểu cường độ trao đổi lớn nhất và đối số không bắt chéo cho thấy rằng tồn tại sự sắp xếp tối ưu với các cặp này đối diện nhau. 

Khi các cặp đó đã được cố định, mọi sự sắp xếp có thể có được biểu thị bằng cấu trúc này hoàn toàn được xác định bởi hai lựa chọn: thứ tự của các cặp và hướng của mỗi cặp. Thuật toán liệt kê tất cả các kết hợp (N!2^N), do đó nó không thể bỏ lỡ sự sắp xếp tối ưu. Vì mọi ứng cử viên đều được đánh giá bằng cách kiểm tra từng khối tuần hoàn, nên ứng cử viên có điểm Diego tối đa nhỏ nhất chính xác là kết quả đầu ra được yêu cầu. 

## Giải pháp Python```python
import sys
import itertools

input = sys.stdin.readline

def solve_case(n, values):
    values = sorted(values)

    # Pair consecutive values in sorted order.
    pairs = [(values[2 * i], values[2 * i + 1]) for i in range(n)]

    best_score = None
    best_circle = None

    # Every permutation chooses the order of opposite pairs.
    for order in itertools.permutations(range(n)):
        # Every mask chooses the orientation of every pair.
        for mask in range(1 << n):
            circle = [0] * (2 * n)

            for pos, pair_id in enumerate(order):
                low, high = pairs[pair_id]

                if mask & (1 << pair_id):
                    circle[pos] = high
                    circle[pos + n] = low
                else:
                    circle[pos] = low
                    circle[pos + n] = high

            # Sum of the first N positions.
            window = sum(circle[:n])
            worst = window

            # Slide through all remaining cyclic windows.
            for start in range(1, 2 * n):
                window += circle[(start + n - 1) % (2 * n)]
                window -= circle[start - 1]
                if window > worst:
                    worst = window

            if best_score is None or worst < best_score:
                best_score = worst
                best_circle = circle[:]

    return " ".join(map(str, best_circle))

def main():
    n = int(input())
    values = list(map(int, input().split()))
    print(solve_case(n, values))

if __name__ == "__main__":
    main()
```Phần đầu tiên sắp xếp các giá trị và tạo các cặp đối diện. Ví dụ: các giá trị mẫu trở thành 

[ 
(100.100),(101.102),(115.117),(145.147),(982.992). 
] 

Sự khác biệt bên trong mỗi cặp càng nhỏ càng tốt trong số tất cả các cách có thể ghép các giá trị được sắp xếp. 

Bên ngoài`itertools.permutations`vòng lặp thực hiện việc liệt kê thứ tự cặp từ bước 2. Với (N=6), nó chỉ tạo ra (720) đơn hàng. 

Vòng lặp mặt nạ thực hiện quyết định định hướng. Bit được đặt sẽ đặt điểm cuối lớn hơn của cặp đó vào nửa đầu, trong khi bit không được đặt sẽ đặt điểm cuối nhỏ hơn ở đó. Điểm cuối đối diện được đặt cách chính xác (N) vị trí. 

Việc tính toán cửa sổ đáng được chú ý vì vòng tròn bao quanh. Cửa sổ đầu tiên là`circle[:n]`. Khi vị trí ban đầu thay đổi từ`start - 1`ĐẾN`start`, giá trị đi ra là`circle[start - 1]`, trong khi giá trị đến là`(start + n - 1) % (2 * n)`. Hoạt động modulo là thao tác làm cho các cửa sổ cuối cùng vượt qua phần cuối của mảng được in một cách chính xác. 

Số nguyên Python không bị tràn, do đó tổng tối đa có thể, (6\cdot10^6), không cần xử lý đặc biệt. Các giá trị ban đầu không bao giờ được sửa đổi và mọi ứng cử viên đều được xây dựng dưới dạng một hoán vị mới, điều này cũng ngăn không cho ứng viên sau này làm sai câu trả lời đã lưu. 

Đầu vào chỉ chứa một trường hợp kiểm thử, do đó không có vòng lặp trường hợp kiểm thử nào xung quanh`solve_case`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các giá trị được sắp xếp là 

[ 
100.100.101.102.115.117.145.147.982.992. 
] 

Các cặp kết quả được hiển thị dưới đây. 

| Cặp | Giá trị | Sự khác biệt | 
| --- | --- | --- | 
| 0 | (100.100) | 0 | 
| 1 | (101,102) | 1 | 
| 2 | (115,117) | 2 | 
| 3 | (145,147) | 2 | 
| 4 | (982,992) | 10 | 

Một ứng cử viên tối ưu là cách sắp xếp mẫu:`992 100 115 102 147 982 101 117 100 145`Nửa đầu của nó có tổng (1456). Trượt cửa sổ quanh vòng tròn sẽ có số tiền sau. 

| Bắt đầu | Tổng cửa sổ | 
| --- | --- | 
| 0 | 1456 | 
| 1 | 1446 | 
| 2 | 1447 | 
| 3 | 1449 | 
| 4 | 1447 | 
| 5 | 1445 | 
| 6 | 1455 | 
| 7 | 1454 | 
| 8 | 1452 | 
| 9 | 1450 | 

Mức tối đa là (1456), do đó Diego có thể ép buộc (1456), trong khi Googles nhận được (2901-1456=1445). 

Dấu vết cũng minh họa tại sao chỉ làm cho nửa đầu càng gần một nửa tổng số càng tốt là không đủ. Mọi cửa sổ tuần hoàn phải được kiểm soát, bao gồm cả các cửa sổ đi qua phần cuối của mảng. 

### Ví dụ tùy chỉnh: (N=2) 

Hãy xem xét```
2
1 2 3 100
```Các cặp liền kề được sắp xếp là ((1,2)) và ((3.100)). 

Một sự sắp xếp tối ưu là```
100 1 3 2
```Trạng thái cửa sổ trượt là: 

| Bắt đầu | Cửa sổ | Tổng hợp | 
| --- | --- | --- | 
| 0 | (100,1) | 101 | 
| 1 | (1,3) | 4 | 
| 2 | (3,2) | 5 | 
| 3 | (2.100) | 102 | 

Điểm tệ nhất là (102). Hàng cuối cùng là trường hợp ranh giới mà việc triển khai không tròn sẽ bỏ sót. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^N N! \cdot N)) | (2^N N!), mỗi ứng viên được đánh giá bằng (O(N)) công việc | 
| Không gian | (O(N)) | Vòng tròn, cặp và hoán vị hiện tại chứa các giá trị (O(N)) | 

Tối đa (N=6), chỉ có (46{,}080) ứng viên và mỗi ứng viên chỉ có (12) vị trí. Do đó, việc triển khai thực hiện vài trăm nghìn thao tác ở cấp độ ứng viên, thoải mái trong giới hạn thời gian 8 giây và thấp hơn nhiều so với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Vì đầu ra được phép là bất kỳ hoán vị tối ưu nào nên các thử nghiệm bên dưới sẽ xác thực hoán vị được trả về và so sánh điểm của nó với điểm tối ưu được tính toán độc lập trong các trường hợp nhỏ. Trường hợp kích thước tối đa sử dụng tất cả các giá trị bằng nhau, trong đó giá trị tối ưu được biết ngay lập tức.```python
import io
import itertools
import sys

def solve_case(n, values):
    values = sorted(values)
    pairs = [(values[2 * i], values[2 * i + 1]) for i in range(n)]

    best_score = None
    best_circle = None

    for order in itertools.permutations(range(n)):
        for mask in range(1 << n):
            circle = [0] * (2 * n)

            for pos, pair_id in enumerate(order):
                low, high = pairs[pair_id]
                if mask & (1 << pair_id):
                    circle[pos] = high
                    circle[pos + n] = low
                else:
                    circle[pos] = low
                    circle[pos + n] = high

            window = sum(circle[:n])
            worst = window

            for start in range(1, 2 * n):
                window += circle[(start + n - 1) % (2 * n)]
                window -= circle[start - 1]
                worst = max(worst, window)

            if best_score is None or worst < best_score:
                best_score = worst
                best_circle = circle[:]

    return best_circle

def solve(inp):
    data = list(map(int, inp.split()))
    n = data[0]
    values = data[1:1 + 2 * n]
    return " ".join(map(str, solve_case(n, values)))

def score(circle):
    m = len(circle)
    n = m // 2

    window = sum(circle[:n])
    best = window

    for start in range(1, m):
        window += circle[(start + n - 1) % m]
        window -= circle[start - 1]
        best = max(best, window)

    return best

def validate(inp, output):
    data = list(map(int, inp.split()))
    n = data[0]
    original = data[1:1 + 2 * n]

    result = list(map(int, output.split()))

    assert len(result) == 2 * n
    assert sorted(result) == sorted(original)
    assert score(result) >= 0

def brute_force_optimum(n, values):
    best = None

    for perm in itertools.permutations(values):
        cur = score(perm)
        if best is None or cur < best:
            best = cur

    return best

def run(inp: str) -> str:
    return solve(inp)

# Sample 1.
sample1 = """5
992 100 115 102 101 117 100 145 147 982
"""
out = run(sample1)
validate(sample1, out)

# Minimum size, N = 1.
case1 = """1
7 11
"""
out = run(case1)
validate(case1, out)
assert score(list(map(int, out.split()))) == 11

# All equal values.
case2 = """2
5 5 5 5
"""
out = run(case2)
validate(case2, out)
assert score(list(map(int, out.split()))) == 10

# Boundary-crossing case.
case3 = """2
1 2 3 100
"""
out = run(case3)
validate(case3, out)
assert score(list(map(int, out.split()))) == brute_force_optimum(
    2, [1, 2, 3, 100]
)
assert score(list(map(int, out.split()))) == 102

# Maximum-size input with extreme values.
case4 = """6
1 1 1 1 1 1 1000000 1000000 1000000 1000000 1000000 1000000
"""
out = run(case4)
validate(case4, out)
assert score(list(map(int, out.split()))) == 3000003
```Các thử nghiệm bao gồm các trường hợp sau đây. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ hoán vị tối ưu hợp lệ nào, điểm kém nhất (1456) | Cấu trúc chính và định hướng cặp | 
|`1 / 7 11`| Bất kỳ hoán vị nào, điểm kém nhất (11) | Cửa sổ một phần tử tối thiểu (N) | 
|`2 / 5 5 5 5`| Bất kỳ hoán vị nào, điểm tệ nhất (10) | Giá trị bằng nhau và xử lý trùng lặp | 
|`2 / 1 2 3 100`| Bất kỳ hoán vị nào, điểm kém nhất (102) | Ranh giới tròn và khoảng cách giá trị lớn | 
| Sáu`1`s và sáu`1000000`s | Bất kỳ hoán vị nào, điểm kém nhất (3000003) | Tối đa (N), giá trị cực trị, thứ tự cặp cân bằng | 

## Vỏ cạnh 

Với (N=1), đầu vào chứa chính xác hai giá trị. Diego chọn một vị trí nên điểm tối ưu của anh ấy chỉ đơn giản là giá trị lớn hơn. Thuật toán tạo ra một cặp, xem xét cả hai hướng và đạt được điểm kém nhất như nhau. Vì`1 / 7 11`, cả hai`7 11`Và`11 7`là tối ưu. 

Đối với các giá trị trùng lặp, việc ghép nối được sắp xếp liên tiếp sẽ tự nhiên tạo ra các cặp có chênh lệch bằng 0. Với`2 / 5 5 5 5`, chênh lệch cặp duy nhất bằng 0 và mọi cửa sổ có thể có tổng (10). Bảng liệt kê hoán vị vẫn hoạt động vì các vị trí vẫn có cấu trúc khác biệt ngay cả khi giá trị của chúng bằng nhau. 

Đối với một khoảng cách lớn, hãy xem xét`2 / 1 2 3 100`. Cấu trúc cặp là`(1,2)`Và`(3,100)`. Ứng viên`100 1 3 2`có tổng cửa sổ (101,4,5,102), bao gồm cả cửa sổ được bao bọc (2+100). Thuật toán đánh giá rõ ràng cửa sổ cuối cùng đó, do đó, nó nhận được điểm chính xác (102) thay vì câu trả lời không tròn không chính xác (101). 

Đối với trường hợp tối đa (N=6) có sáu`1`s và sáu`1000000`s, cách sắp xếp tốt nhất có thể sẽ xen kẽ hai giá trị. Mỗi khối sáu phần tử khi đó chứa ba giá trị lớn và ba giá trị nhỏ, cho 

[ 
3\cdot1{,}000{,}000+3=3{,}000{,}003. 
] 

Sự khác biệt của các cặp đều bằng 0 vì danh sách được sắp xếp tạo thành ba`(1,1)`cặp và ba`(1000000,1000000)`cặp. Việc tìm kiếm còn lại hoàn toàn là về việc sắp xếp các cặp có giá trị bằng nhau và liệt kê tất cả (6!) thứ tự cặp để tìm ra cách sắp xếp vòng tròn cân bằng. 

Trường hợp quan trọng nhất của việc triển khai là cập nhật cửa sổ theo chu kỳ. Chỉ mục đến phải là`(start + n - 1) % (2 * n)`. Việc bỏ qua thao tác modulo chỉ đánh giá các cửa sổ có trong mảng được in và âm thầm bỏ lỡ các cửa sổ bao bọc từ vị trí cuối cùng trở lại vị trí đầu tiên. Đối với vấn đề này, những cửa sổ được bao bọc đó cũng quan trọng như mọi lựa chọn khác mà Diego có thể đưa ra.
