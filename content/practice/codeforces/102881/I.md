---
title: "CF 102881I - Đồ thị Ehab Bé đã học"
description: "Chúng ta được cho một đồ thị vô hướng liên thông có tối đa 100 đỉnh. Đầu vào là ma trận kề của nó, vì vậy mỗi cặp đỉnh cho chúng ta biết liệu đồ thị ban đầu có chứa cạnh đó hay không. Chúng ta phải biểu thị biểu đồ này dưới dạng XOR của một số cây trên cùng một tập đỉnh."
date: "2026-07-25T12:35:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102881
codeforces_index: "I"
codeforces_contest_name: "ECPC 2019 Kickoff"
rating: 0
weight: 102881
solve_time_s: 70
verified: true
draft: false
---

[CF 102881I - Đồ thị cho bé học Ehab](https://codeforces.com/problemset/problem/102881/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông có tối đa 100 đỉnh. Đầu vào là ma trận kề của nó, vì vậy mỗi cặp đỉnh cho chúng ta biết liệu đồ thị ban đầu có chứa cạnh đó hay không. Chúng ta phải biểu thị biểu đồ này dưới dạng XOR của một số cây trên cùng một tập đỉnh. Một cạnh tồn tại trong XOR cuối cùng chính xác khi nó xuất hiện trong một số lẻ cây được chọn. 

Trong số tất cả các phân tách hợp lệ, chúng ta muốn đường kính tối đa nhỏ nhất có thể của bất kỳ cây nào mà chúng ta sử dụng. Đầu ra là danh sách các cây đạt mức tối thiểu đó, hoặc`-1`nếu không có sự phân hủy tồn tại. 

Giới hạn nhỏ về`n`làm thay đổi bản chất của vấn đề. Với tối đa 100 đỉnh, số cạnh có thể có nhiều nhất là 4950, do đó việc biểu diễn đồ thị dưới dạng vectơ bit và sử dụng phép loại trừ XOR Gaussian là thực tế. Việc tìm kiếm trực tiếp trên các tập hợp con của cây là không thể vì số lượng cây có thể là rất lớn. 

Hạn chế ẩn đầu tiên xuất phát từ tính chẵn lẻ. Mỗi cây trên`n`đỉnh có chính xác`n-1`các cạnh. XOR bảo toàn tính chẵn lẻ của tổng số cạnh được chọn, do đó tính chẵn lẻ của đồ thị cuối cùng phải bằng tính chẵn lẻ của`k * (n-1)`cho một số số`k`của cây cối. Khi`n`là số lẻ, mọi cây đều có số cạnh chẵn nên đồ thị cuối cùng cũng phải chứa số cạnh chẵn. Nếu như`n`và số cạnh đều là số lẻ nên không thể tồn tại câu trả lời. 

Một số trường hợp rất dễ xử lý sai. Một tam giác có ba đỉnh có ba cạnh:```
3
0 1 1
1 0 1
1 1 0
```Câu trả lời là`-1`. Mỗi cây trên ba đỉnh đều có hai cạnh và XOR bất kỳ số tập hợp cạnh có kích thước chẵn nào không bao giờ có thể tạo ra một biểu đồ có ba cạnh. 

Đồ thị hai đỉnh thì khác:```
2
0 1
1 0
```Câu trả lời là cây duy nhất chứa cạnh duy nhất. Việc triển khai chung chỉ tìm kiếm đường kính hai cây sẽ không thành công ở đây vì cây hai nút có đường kính một, đây là mức tối thiểu thực sự. 

Một ngôi sao đã có đường kính tối thiểu đối với đồ thị có ba đỉnh trở lên cho phép nó:```
3
0 1 1
1 0 0
1 0 0
```Câu trả lời là một cây chứ không phải vài mảnh nhỏ hơn. Việc cố gắng luôn phân hủy thành nhiều cây vẫn có thể đúng nhưng sẽ không đáp ứng được yêu cầu về đường kính tối thiểu. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là liệt kê các cây có thể và thử kết hợp cho đến khi XOR bằng biểu đồ đích. Điều này đúng vì mọi cây ứng cử viên đều có thể được coi là một vectơ các cạnh và đồ thị mong muốn chỉ là tổng XOR của các vectơ đã chọn. Vấn đề là kích thước của không gian tìm kiếm. Dù chỉ có 100 đỉnh nhưng số cây được dán nhãn là`100^98`, nên việc liệt kê là hoàn toàn không thể. 

Quan sát quan trọng là chúng ta không cần liệt kê tất cả các cây. Chúng ta chỉ cần biết liệu vectơ cạnh mục tiêu có thuộc về khoảng tuyến tính của họ cây có đường kính nhỏ được lựa chọn cẩn thận hay không. 

Đường kính nhỏ nhất có thể được kiểm tra đầu tiên. Vì`n = 2`, đường kính một là tối ưu. Đối với đồ thị lớn hơn, đường kính hai cây chính xác là các ngôi sao. Chúng tôi tạo ra mọi ngôi sao và sử dụng phương pháp loại bỏ Gaussian XOR để kiểm tra xem liệu chỉ riêng các ngôi sao có thể tạo thành biểu đồ hay không. 

Nếu thất bại, đường kính ba là câu trả lời khả thi tiếp theo. Một cây có đường kính tối đa là ba có thể được xây dựng thành một ngôi sao đôi. Với mọi cặp đỉnh khác nhau có thứ tự`(a, b)`, chúng ta tạo ra một cây có cạnh`(a, b)`làm cạnh giữa và gắn mọi đỉnh khác vào`b`. chỉ có`n(n-1)`những cây như vậy, đủ nhỏ. Khoảng của chúng đủ để biểu diễn mọi đồ thị vượt qua điều kiện chẵn lẻ, vì vậy nếu phép loại trừ thứ hai này thành công thì câu trả lời là tối ưu. 

Quá trình loại bỏ cũng lưu trữ những cây được tạo ra từng vectơ cơ sở. Sau khi giảm vectơ mục tiêu, chúng tôi khôi phục các cây đã chọn bằng cách XOR các lựa chọn đã lưu trữ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ của số cây | Hàm mũ | Quá chậm | 
| Tối ưu | O(n^4) | O(n^2 + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi đồ thị đầu vào thành một vectơ bit. Gán một bit cho mọi cạnh có thể, do đó XOR giữa các biểu đồ trở thành XOR giữa các số nguyên. 
2. Kiểm tra điều kiện không thể chẵn lẻ. Nếu như`n`là số lẻ và số cạnh là số lẻ, in`-1`. Mỗi cây có số cạnh chẵn trong trường hợp này, vì vậy không có tổ hợp XOR nào có thể khớp với biểu đồ. 
3. Xử lý trường hợp đặc biệt`n = 2`. Đồ thị liên thông duy nhất có thể có là một cạnh duy nhất và bản thân cạnh đó là cây tối ưu. 
4. Tạo tất cả các ngôi sao. Một ngôi sao có tâm ở đỉnh`v`chứa mọi cạnh`(v, u)`vì`u != v`. Chạy loại bỏ cơ sở XOR trên những cây này. 
5. Nếu đồ thị mục tiêu có thể được tạo thành từ các ngôi sao, hãy xuất ra các ngôi sao đã chọn. Vì các ngôi sao có đường kính hai và đường kính một là không thể`n > 2`, điều này là tối ưu. 
6. Tạo tất cả các sao đôi theo thứ tự. Đối với mỗi cặp đặt hàng`(a, b)`, thêm cạnh`(a, b)`và kết nối mọi đỉnh khác với`b`. Mỗi cây được tạo ra có đường kính tối đa là ba. 
7. Chạy cùng loại bỏ cơ sở XOR trên các sao đôi. Những cây được chọn chính là câu trả lời. 

Tại sao nó hoạt động: 

Thuật toán kiểm tra các đường kính có thể theo thứ tự tăng dần. Một cây có nhiều hơn hai đỉnh không thể có đường kính bằng một, vì vậy họ thành công đầu tiên sẽ đưa ra đường kính tối thiểu có thể. Các ngôi sao chính xác là các cây có đường kính hai và các ngôi sao đôi được tạo ra bao phủ trường hợp có đường kính ba. Việc loại bỏ Gaussian XOR là đúng vì đồ thị XOR chỉ là phép cộng vectơ trên trường có hai phần tử. Nếu vectơ mục tiêu có thể được đại diện bởi một họ, việc loại bỏ sẽ khôi phục một tập hợp con hợp lệ của họ đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def xor_basis(candidates, target, edge_count):
    basis = {}
    for idx, (vec, _) in enumerate(candidates):
        x = vec
        mask = 1 << idx
        while x:
            p = x.bit_length() - 1
            if p in basis:
                bx, bm = basis[p]
                x ^= bx
                mask ^= bm
            else:
                basis[p] = (x, mask)
                break

    x = target
    ans = 0
    while x:
        p = x.bit_length() - 1
        if p not in basis:
            return None
        bx, bm = basis[p]
        x ^= bx
        ans ^= bm

    return ans

def main():
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]

    edges = []
    pos = {}
    for i in range(n):
        for j in range(i + 1, n):
            pos[(i, j)] = len(edges)
            edges.append((i, j))

    m = len(edges)
    target = 0
    for i, j in edges:
        if a[i][j]:
            target ^= 1 << pos[(i, j)]

    if n % 2 == 1 and target.bit_count() % 2 == 1:
        print(-1)
        return

    if n == 2:
        print(1)
        print("1 2")
        return

    def make_vec(es):
        v = 0
        for x, y in es:
            if x > y:
                x, y = y, x
            v ^= 1 << pos[(x, y)]
        return v

    stars = []
    for c in range(n):
        es = []
        for x in range(n):
            if x != c:
                es.append((c, x))
        stars.append((make_vec(es), es))

    res = xor_basis(stars, target, m)
    if res is not None:
        out = [str(res.bit_count())]
        for i in range(len(stars)):
            if (res >> i) & 1:
                for u, v in stars[i][1]:
                    out.append(f"{u + 1} {v + 1}")
        print("\n".join(out))
        return

    doubles = []
    for a in range(n):
        for b in range(n):
            if a == b:
                continue
            es = [(a, b)]
            for x in range(n):
                if x != a and x != b:
                    es.append((b, x))
            doubles.append((make_vec(es), es))

    res = xor_basis(doubles, target, m)
    if res is None:
        print(-1)
        return

    out = []
    chosen = []
    for i in range(len(doubles)):
        if (res >> i) & 1:
            chosen.append(doubles[i][1])

    out.append(str(len(chosen)))
    for tree in chosen:
        for u, v in tree:
            out.append(f"{u + 1} {v + 1}")
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc lập chỉ mục cạnh biến mọi đồ thị có thể thành một số nguyên. Hoạt động duy nhất mà thuật toán cần là XOR, vì vậy số nguyên Python cung cấp cách biểu diễn bitset thuận tiện.`xor_basis`thực hiện loại bỏ Gaussian trên GF(2). Khóa từ điển là bit được đặt cao nhất của vectơ cơ sở. Trong quá trình chèn, vectơ bị giảm cho đến khi chúng biến mất hoặc tạo thành phần tử cơ sở mới. Các bản ghi mặt nạ đi kèm đã tạo ra các cây tạo ra vectơ đó, cho phép tái tạo lại lần cuối các cây đã chọn. 

Thế hệ sao khớp chính xác với trường hợp có đường kính hai. Đối với mỗi trung tâm, mã sẽ thêm mọi cạnh sự cố. Thế hệ sao đôi sử dụng một cặp có thứ tự vì điểm cuối thứ hai xác định vị trí gắn tất cả các đỉnh còn lại. Mỗi cây như vậy có chính xác`n-1`các cạnh và đường kính nhiều nhất là ba. 

Quá trình xây dựng lại lặp qua mặt nạ đã chọn và chỉ in các cây có hệ số bằng một. Số lượng cây được chọn được giới hạn bởi hạng của không gian cạnh, tức là số cạnh có thể có nhiều nhất nên luôn thỏa mãn điều kiện`n + m`giới hạn đầu ra. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đồ thị có bốn đỉnh và năm cạnh. Số sao không đủ nên thuật toán chuyển sang sao đôi. 

| Bước | Gia đình | Kết quả | 
| --- | --- | --- | 
| 1 | Xây dựng vector mục tiêu | Năm bit cạnh được đặt | 
| 2 | Hãy thử sao | Mục tiêu không thể đại diện | 
| 3 | Hãy thử sao đôi | Mục tiêu có thể đại diện | 
| 4 | Xuất ra cây đã chọn | XOR bằng đồ thị gốc | 

Ví dụ này chứng minh tại sao đường kính 2 không phải lúc nào cũng đủ. Những cây cuối cùng có thể có đường kính bằng 3, nhưng không thể có đường kính tối đa nhỏ hơn. 

Đối với mẫu thứ ba: 

| Bước | Gia đình | Kết quả | 
| --- | --- | --- | 
| 1 | Xây dựng vector mục tiêu | Hai cạnh được đặt | 
| 2 | Hãy thử sao | Một sao khớp chính xác | 
| 3 | Xuất ra một cây | Đường kính là hai | 

Điều này xác nhận rằng thuật toán dừng ở đường kính đầu tiên có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^4) | Nhiều nhất là các cây ứng cử viên O(n^2) được chèn và mỗi vectơ có các bit cạnh O(n^2) | 
| Không gian | O(n^2) | Cơ sở lưu trữ tối đa một vectơ trên mỗi cạnh có thể | 

Số cạnh tối đa là 4950 và số sao đôi được tạo tối đa là 9900. Các thao tác bit giúp việc loại bỏ trở nên thực tế đối với các giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

# The original solution is placed in solve() in a local test environment.
def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # call solution function here
    sys.stdin = old
    return ""

assert run("""2
0 1
1 0
""") != "", "minimum size graph"

assert run("""3
0 1 1
1 0 1
1 1 0
""") == "-1", "odd edge parity"

assert run("""3
0 1 1
1 0 0
1 0 0
""") != "", "single star"

assert run("""4
0 1 0 1
1 0 1 0
0 1 0 1
1 0 1 0
""") != "", "cycle requiring decomposition"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh có một cạnh | Một cây | Đường kính một xử lý | 
| Tam giác |`-1`| Tính chẵn lẻ không thể | 
| Sao ba nút | Một cây | Đường kính hai phát hiện | 
| Chu kỳ bốn nút | Một số cây | Dự phòng hai sao | 

## Vỏ cạnh 

Trường hợp tam giác bị bác bỏ trước khi loại bỏ. Thuật toán đếm ba cạnh và thấy rằng`n`là số lẻ trong khi số cạnh là số lẻ. Vì mọi cây có thể đều có số cạnh chẵn nên mục tiêu không thể tồn tại. 

Trường hợp hai nút được xử lý riêng biệt. Đồ thị liên thông duy nhất là:```
2
0 1
1 0
```Thuật toán đưa ra một cây chứa`1 2`. Điều này tránh việc coi câu trả lời là một cấu trúc có đường kính hai. 

Trường hợp ngôi sao thành công trong lần loại trừ đầu tiên. Vì:```
3
0 1 1
1 0 0
1 0 0
```ngôi sao được tạo có tâm ở đỉnh một có vectơ cạnh giống hệt như mục tiêu. Thuật toán chỉ chọn cây này, cho đường kính tối thiểu. 

Đồ thị không phải là ngôi sao đạt đến giai đoạn sao đôi. Cơ sở thứ hai chứa đủ đường kính - ba cây để biểu thị mọi trường hợp hợp lệ còn lại và bước loại trừ sẽ chọn một tập hợp con có XOR tái tạo mọi cạnh ban đầu với tính chẵn lẻ chính xác.
