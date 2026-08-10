---
title: "CF 102392J - Đồ thị và chu trình"
description: "Chúng ta có một đồ thị vô hướng đầy đủ trên một số lẻ (n) đỉnh. Mỗi cạnh (frac{n(n-1)}2) của nó đều có trọng số dương. Chúng ta phải phân chia tất cả các cạnh thành các mảng chu kỳ."
date: "2026-08-10T19:37:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 83
verified: true
draft: false
---

[CF 102392J - Đồ thị và Chu trình](https://codeforces.com/problemset/problem/102392/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng đầy đủ trên một số lẻ (n) đỉnh. Mỗi cạnh (\frac{n(n-1)}2) của nó đều có trọng số dương. 

Chúng ta phải phân chia tất cả các cạnh thành các mảng chu kỳ. Các cạnh liên tiếp trong một mảng phải có chung một đỉnh và hai phần chuyển tiếp xung quanh mỗi cạnh phải sử dụng các điểm cuối khác nhau, do đó mảng mô tả một đường khép kín xuyên qua biểu đồ. Một mảng chu kỳ được phép xem lại một đỉnh. Điều quan trọng là các cạnh của nó được sắp xếp theo chu kỳ và mỗi cạnh liền kề với một cạnh thông qua mỗi điểm cuối của nó. 

Đối với hai cạnh liên tiếp (e_i,e_{i+1}), phần đóng góp của trọng số của chúng càng lớn. Giá của một mảng chu kỳ là tổng của những đóng góp này xung quanh toàn bộ mảng. Đầu ra được yêu cầu là tổng giá tối thiểu có thể có trên một phân vùng của mọi cạnh biểu đồ thành các mảng như vậy. 

Sự kỳ lạ của (n) là nguyên nhân cấu trúc khiến bài toán trở nên đơn giản. Mọi đỉnh đều có bậc (n-1), là bậc chẵn. Vì một cạnh đi vào một đỉnh trong mảng chu kỳ phải được ghép với chính xác một cạnh rời khỏi đỉnh đó, nên tất cả các cạnh liên quan đến một đỉnh có thể được chia thành các cặp. 

Có nhiều nhất (999) đỉnh, nhưng đồ thị dày đặc. Với (n=999), nó chứa 

[ 
\frac{999\cdot998}{2}=498501 
] 

các cạnh. Điều này ngay lập tức loại trừ các thuật toán liệt kê các chu trình, phân tách chu trình hoặc ghép nối tùy ý. Ngay cả một thứ tự tuần hoàn duy nhất của tất cả các cạnh cũng có khả năng xấp xỉ (498500!/2). Giải pháp dự định cần khai thác thực tế là mục tiêu có thể được phân tách bằng các đỉnh. 

Trọng số có thể lớn bằng (10^9), vì vậy câu trả lời không khớp với số nguyên có dấu 32 bit. Trong trường hợp hoàn toàn bằng nhau lớn nhất, câu trả lời là (498501) và với các trọng số riêng lẻ lớn hơn, nó có thể theo thứ tự (10^{15}). Các số nguyên Python tự động xử lý việc này, trong khi việc triển khai C++ sẽ cần`long long`. 

Trường hợp một cạnh nhỏ là (n=3). Mỗi đỉnh có đúng hai cạnh liên tiếp nên chỉ có một cặp có thể có ở mỗi đỉnh. Ví dụ,```
3
1 2 1
2 3 1
1 3 1
```có câu trả lời (3). Một giải pháp giả sử mỗi đỉnh có ít nhất bốn cạnh liên quan hoặc thực hiện vòng lặp ghép nối không chính xác có thể bỏ lỡ trường hợp này. 

Một trường hợp quan trọng khác là trọng số bằng nhau. Vì```
3
1 2 5
2 3 5
1 3 5
```câu trả lời là (15). Mỗi cạnh kề có giá trị (5) và tam giác có ba cạnh kề. Việc triển khai bất cẩn cố gắng phân biệt các cạnh theo trọng số có thể vô tình loại bỏ các cạnh có trọng số bằng nhau hoặc chỉ đếm một trong số chúng. 

Trường hợp thứ ba liên quan đến trọng lượng lớn. Vì```
3
1 2 1000000000
2 3 1000000000
1 3 1000000000
```câu trả lời là (3000000000). Sử dụng số nguyên 32 bit sẽ tràn. Bản thân thuật toán chỉ thực hiện phép cộng, vì vậy việc triển khai chỉ cần sử dụng loại số nguyên có khả năng chứa tổng cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng liệt kê các phân chia chu kỳ có thể xảy ra, tính giá của từng chu kỳ và giữ mức tối thiểu. Điều này đúng vì mọi sự phân chia hợp lệ cuối cùng sẽ được xem xét. Vấn đề là số lượng khả năng. Nếu (m=\frac{n(n-1)}2), thì thậm chí chỉ xem xét một chu trình chứa tất cả (m) cạnh sẽ cho ((m-1)!/2) thứ tự tuần hoàn riêng biệt trong một cài đặt vô hướng. Đối với (n=999), (m=498501), vì vậy điều này vượt xa mọi thứ có thể thực hiện được. Việc liệt kê một số chu trình hoặc thử tất cả các cặp ở các đỉnh thậm chí còn tệ hơn. 

Quan sát hữu ích là giá có thể được gán cục bộ cho các đỉnh. 

Xét một cạnh (e=(u,v)). Bên trong mảng chu kỳ của nó, nó được so sánh với chính xác một cạnh lân cận thông qua (u) và chính xác một cạnh lân cận thông qua (v). Do đó, tại đỉnh (u), các cạnh tới được chia thành các cặp, trong đó mỗi cặp đóng góp tối đa hai trọng số của nó. Mọi đóng góp giá trong toàn bộ quá trình phân chia chu kỳ đều thuộc về chính xác một đỉnh như vậy. 

Vì vậy, bài toán toàn cục trở thành một tập hợp các bài toán cục bộ độc lập: tại mỗi đỉnh, ghép (n-1) cạnh sự cố của nó càng rẻ càng tốt. 

Giả sử các trọng số tới tại một đỉnh, được sắp xếp từ lớn nhất đến nhỏ nhất, là 

[ 
w_1\ge w_2\ge w_3\ge w_4\ge\cdots\ge w_{n-2}\ge w_{n-1}. 
] 

Có các cặp (\frac{n-1}{2}). Để giảm thiểu tổng cực đại theo cặp, hai giá trị lớn nhất phải được ghép nối, sau đó là hai giá trị lớn nhất tiếp theo, v.v.: 

[ 
(w_1,w_2),(w_3,w_4),\ldots 
] 

Do đó, sự đóng góp là 

[ 
w_1+w_3+w_5+\cdots+w_{n-2}. 
] 

Tại sao việc ghép đôi này lại tối ưu? Phần tử lớn nhất phải là phần tử lớn nhất của một số cặp, vì vậy mỗi cặp đều trả ít nhất (w_1). Sau khi ghép nó với giá trị lớn thứ hai, bài toán còn lại có dạng tương tự với các giá trị còn lại. Tương tự, việc ghép một giá trị lớn với một giá trị nhỏ sẽ lãng phí giá trị nhỏ đó vì giá trị lớn đã xác định chi phí của cặp đó. Việc ghép nối các giá trị liên tiếp từ lớn nhất đến nhỏ nhất sẽ giảm thiểu số lượng giá trị lớn trở thành cực đại của cặp. 

Câu hỏi còn lại là liệu việc chọn độc lập các cặp tối ưu này ở mọi đỉnh có thực sự tạo ra các mảng chu trình hợp lệ hay không. Nó có thể. Hãy coi mỗi cặp được chọn là một quá trình chuyển đổi cho chúng ta biết cạnh nào theo sau cạnh nào tại đỉnh đó. Bắt đầu từ bất kỳ cạnh nào chưa được sử dụng, hãy đi theo cạnh được ghép nối tại điểm cuối hiện tại của nó. Mỗi khi chúng ta đến một đỉnh, chính xác một cặp ở đó được sử dụng một phần, do đó có một cạnh duy nhất hoàn thành cặp đó. Tiếp tục quá trình này cuối cùng sẽ quay trở lại điểm bắt đầu vì có hữu hạn nhiều cạnh và mỗi cạnh có đúng hai điểm cuối được ghép nối. Đường dẫn khép kín thu được là một mảng chu kỳ hợp lệ. Sau khi hoàn thành, hãy bắt đầu lại từ một cạnh chưa sử dụng khác. 

Việc xây dựng kết quả hiện thực hóa mọi cặp tối ưu cục bộ, do đó tổng cực tiểu cục bộ không chỉ đơn thuần là giới hạn dưới. Nó có thể đạt được và chính xác là mức tối ưu toàn cầu. Đây là mức giảm trung tâm được sử dụng bởi bài xã luận chính thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ít nhất (O(m!)), trong đó (m=\frac{n(n-1)}2) | Trạng thái tìm kiếm theo cấp số nhân/giai thừa | Quá chậm | 
| Tối ưu | (O(n^2\log n)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các cạnh (m=\frac{n(n-1)}2) và lưu trữ mỗi cạnh dưới dạng hai điểm cuối và trọng số của nó. Đồ thị hoàn chỉnh nên mỗi đỉnh cuối cùng sẽ nhận được chính xác (n-1) cạnh liên quan. 
2. Với mỗi đỉnh, thu thập trọng số của tất cả các cạnh liên quan. Vì (n) là số lẻ, (n-1) là số chẵn nên các trọng số này có thể được chia thành từng cặp. 
3. Sắp xếp trọng số sự cố theo thứ tự giảm dần. Ghép các vị trí (0) và (1), vị trí (2) và (3), vị trí (4) và (5), v.v. Mức tối đa của mỗi cặp chỉ đơn giản là phần tử đầu tiên của cặp đó. 
4. Cộng trọng số tại các vị trí (0,2,4,\ldots) vào đáp án. Điều này tính toán mức đóng góp tối thiểu có thể có của đỉnh đó một cách độc lập với mọi đỉnh khác. 
5. Lặp lại điều này cho mọi đỉnh. Mỗi cạnh kề của hai cạnh liên tiếp trong mỗi mảng chu kỳ xảy ra ở đúng một đỉnh, do đó, việc tính tổng các đóng góp cục bộ này sẽ tính mỗi số hạng của giá toàn cầu đúng một lần. 

Bất biến chính là mỗi lần phân chia chu kỳ hợp lệ sẽ tạo ra một cặp các cạnh tới ở mỗi đỉnh, trong khi việc ghép cặp được sắp xếp của chúng tôi mang lại chi phí tối thiểu có thể có cho đỉnh đó. Các cặp được chọn độc lập có thể được theo dõi dưới dạng các đường dẫn khép kín, do đó chúng tương ứng với sự phân chia chu kỳ hợp lệ. Do đó, không có giải pháp nào có thể có chi phí thấp hơn tổng của chúng tôi và tồn tại một giải pháp đạt được chính xác số tiền đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    edges = n * (n - 1) // 2

    incident = [[] for _ in range(n)]

    for _ in range(edges):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        incident[u].append(w)
        incident[v].append(w)

    ans = 0

    for v in range(n):
        incident[v].sort(reverse=True)
        ans += sum(incident[v][i] for i in range(0, n - 1, 2))

    print(ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên đọc chính xác (\frac{n(n-1)}2) cạnh, là số cạnh trong một biểu đồ hoàn chỉnh. Mỗi trọng số được thêm vào danh sách của cả hai điểm cuối vì cạnh đó tham gia vào việc ghép nối cục bộ ở cả hai đỉnh. 

Với mỗi đỉnh, danh sách có chính xác (n-1) phần tử. Phạm vi`range(0, n - 1, 2)`chỉ số truy cập (0,2,4,\ldots,n-3). Mỗi phần tử được truy cập là phần tử lớn hơn của một cặp tối ưu, do đó việc cộng các giá trị này sẽ mang lại sự đóng góp tối thiểu cho đỉnh đó. 

Việc sắp xếp được thực hiện độc lập cho mọi đỉnh. Vì mỗi đỉnh có nhiều nhất (998) cạnh liên quan nên điều này dễ dàng đủ nhanh. Bản thân đầu vào đã chứa các cạnh (\Theta(n^2)), do đó việc xử lý theo tỷ lệ bậc hai là không thể tránh khỏi. 

Không cần xây dựng chu trình rõ ràng. Bằng chứng cho thấy rằng các cặp cục bộ tối ưu có thể được thực hiện đồng thời dưới dạng mảng chu kỳ, trong khi câu trả lời chỉ yêu cầu tổng giá của chúng. Tránh việc xây dựng tiết kiệm cả thời gian và sự phức tạp khi thực hiện. 

Kiểu số nguyên của Python cũng tránh tràn khi câu trả lời lớn hơn (2^{31}-1). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một hình tam giác và mọi cạnh đều có trọng số (1). 

| Đỉnh | Trọng lượng sự cố được sắp xếp | Cặp tối ưu | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | (1,1) | ((1,1)) | 1 | 
| 2 | (1,1) | ((1,1)) | 1 | 
| 3 | (1,1) | ((1,1)) | 1 | 

Tổng số là (1+1+1=3), khớp với mảng chu trình duy nhất có thể. 

Dấu vết thể hiện trường hợp ranh giới (n=3). Mỗi đỉnh có chính xác một cặp và tổng của ba đóng góp cục bộ chính xác là giá của tam giác. 

### Mẫu 2 

Đối với mười cạnh, trọng số tới trở thành: 

| Đỉnh | Trọng lượng sự cố được sắp xếp | Cặp maxima | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | (4,4,4,3) | (4,4) | 8 | 
| 2 | (4,4,3,2) | (4,3) | 7 | 
| 3 | (4,3,2,2) | (4,2) | 6 | 
| 4 | (4,3,2,2) | (4,2) | 6 | 
| 5 | (4,4,4,2) | (4,4) | 8 | 

Câu trả lời cuối cùng là 

[ 
8+7+6+6+8=35. 
] 

Sự phân chia tối ưu của mẫu có giá (12) và (23), cho tổng số như nhau (35). Tính toán cục bộ đạt đến giá trị đó mà không cần khám phá rõ ràng hai chu kỳ đó. 

Dấu vết này thể hiện tính bất biến trung tâm: mọi chuyển đổi giữa hai cạnh liên tiếp đều được tính vào đỉnh chung của chúng, do đó, việc cộng cực đại cặp tối ưu ở cả năm đỉnh sẽ tạo ra toàn bộ giá phân chia chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2\log n)) | Có (n) danh sách sự cố, mỗi danh sách có kích thước (n-1) và mỗi danh sách được sắp xếp | 
| Không gian | (O(n^2)) | Tất cả các trọng số cạnh (n(n-1)/2) được lưu trữ hai lần, một lần cho mỗi điểm cuối | 

Với (n\le999), có nhiều nhất (498501) cạnh đồ thị và mỗi danh sách sự cố chứa nhiều nhất (998) giá trị. Tổng công việc sắp xếp diễn ra thoải mái trong giới hạn, trong khi mức sử dụng bộ nhớ tỷ lệ thuận với chính lượng dữ liệu đầu vào dày đặc. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    edges = n * (n - 1) // 2

    incident = [[] for _ in range(n)]

    for _ in range(edges):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        incident[u].append(w)
        incident[v].append(w)

    ans = 0

    for v in range(n):
        incident[v].sort(reverse=True)
        ans += sum(incident[v][i] for i in range(0, n - 1, 2))

    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    try:
        old_input = globals()["input"]
        globals()["input"] = sys.stdin.readline

        # solve() uses its own local input binding, so changing stdin is enough.
        output = io.StringIO()
        old_stdout = sys.stdout
        sys.stdout = output
        try:
            solve()
        finally:
            sys.stdout = old_stdout

        return output.getvalue().strip()
    finally:
        globals()["input"] = old_input
        sys.stdin = old_stdin

# Sample 1
assert run(
    """3
1 2 1
2 3 1
1 3 1
"""
) == "3", "sample 1"

# Sample 2
assert run(
    """5
4 5 4
1 3 4
1 2 4
3 2 3
3 5 2
1 4 3
4 2 2
1 5 4
5 2 4
3 4 2
"""
) == "35", "sample 2"

# Minimum size, with different weights.
assert run(
    """3
1 2 1
2 3 2
1 3 3
"""
) == "8", "n=3 with non-equal weights"

# All weights equal.
assert run(
    """5
1 2 7
1 3 7
1 4 7
1 5 7
2 3 7
2 4 7
2 5 7
3 4 7
3 5 7
4 5 7
"""
) == "70", "all weights equal"

# Maximum edge weight boundary.
assert run(
    """3
1 2 1000000000
2 3 1000000000
1 3 1000000000
"""
) == "3000000000", "maximum weight"

# Maximum n, generated compactly.
n = 999
parts = [str(n)]
for u in range(1, n + 1):
    for v in range(u + 1, n + 1):
        parts.append(f"{u} {v} 1")

max_case = "\n".join(parts) + "\n"
assert run(max_case) == "498501", "maximum n, all weights equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=3), trọng số (1,2,3) | 8 | Kích thước biểu đồ tối thiểu và trọng lượng không bằng nhau | 
| (K_5), mỗi cân 7 | 70 | Giá trị bằng nhau và cặp cực đại lặp lại | 
| (K_3), mọi cân nặng (10^9) | 3000000000 | Ranh giới kích thước số nguyên | 
| (K_{999}), mỗi cân 1 | 498501 | Kích thước đầu vào tối đa và xử lý bậc hai | 

## Vỏ cạnh 

Với (n=3), mọi đỉnh đều có bậc (2). Coi như```
3
1 2 1
2 3 2
1 3 3
```Tại đỉnh (1), trọng số là (3,1), đóng góp (3). Tại đỉnh (2), chúng là (2,1), góp phần (2). Tại đỉnh (3), chúng là (3,2), góp phần (3). Câu trả lời là (8). Thuật toán xử lý việc này một cách tự nhiên vì chỉ số được xử lý duy nhất là (0) ở mọi đỉnh. 

Để có trọng số bằng nhau, hãy xem xét đồ thị đầy đủ trên năm đỉnh với mọi cạnh bằng (7). Mỗi đỉnh có bốn cạnh liên tiếp nên nó tạo thành hai cặp và mỗi cặp góp phần (7). Mỗi đỉnh đóng góp (14), cho ra (5\cdot14=70). Việc nhận dạng các cạnh bên trong mỗi cặp không quan trọng vì tất cả các trọng số đều bằng nhau. 

Đối với ranh giới trọng lượng tối đa,```
3
1 2 1000000000
2 3 1000000000
1 3 1000000000
```mỗi đỉnh trong số ba đỉnh đóng góp (10^9), vì vậy câu trả lời là (3\cdot10^9=3000000000). Không có tràn trong Python và phép tính không phụ thuộc vào độ lớn của trọng số ngoại trừ tổng số nguyên cuối cùng. 

Đối với đồ thị lớn nhất, (n=999), mỗi đỉnh có (998) cạnh liên quan. Nếu mỗi trọng số là (1), thì mỗi một trong số (499) cặp cục bộ đều đóng góp (1), do đó một đỉnh đóng góp (499). Trên (999) đỉnh, câu trả lời là (999\cdot499=498501), cũng chính xác là số cạnh của đồ thị. Việc triển khai xử lý tất cả các cạnh đầu vào (498501) và thực hiện các loại cục bộ cần thiết mà không cần cố gắng xây dựng hoặc liệt kê tập hợp khổng lồ các phân tách chu kỳ có thể có.
