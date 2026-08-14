---
title: "CF 102307K - Hạt Nhân Tình Yêu"
description: "Chúng ta có dãy Fibonacci được lập chỉ mục từ 1, với (F1=F2=1) và (F{k+2}=F{k+1}+Fk). Đối với một (n) nhất định, số người có sẵn tương ứng với các giá trị Fibonacci được lập chỉ mục (n) đầu tiên."
date: "2026-08-13T07:26:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "K"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 79
verified: true
draft: false
---

[CF 102307K - Hạt nhân tình yêu](https://codeforces.com/problemset/problem/102307/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có dãy Fibonacci được lập chỉ mục từ 1, với (F_1=F_2=1) và (F_{k+2}=F_{k+1}+F_k). Đối với một (n) nhất định, số người có sẵn tương ứng với các giá trị Fibonacci được lập chỉ mục (n) đầu tiên. Chúng ta cần đếm những cặp người có giá trị Fibonacci thỏa mãn ba điều kiện hiệu quả: gcd của họ là 1, tổng của họ là số lẻ và tổng của họ chính là số Fibonacci. 

Cụm từ "(n) số Fibonacci đầu tiên" có nghĩa là các chỉ số quan trọng. Cụ thể, (F_1) và (F_2) là hai vị trí mặc dù cả hai giá trị đều bằng 1. Sự trùng lặp nhỏ này tạo ra một cặp ngoại lệ và rất dễ bị bỏ qua. Mẫu chính thức có sáu truy vấn, với kết quả đầu ra (0,3,5,11,13,17). 

(n) lớn nhất là (10^5) và có thể có nhiều trường hợp thử nghiệm. Một thuật toán bậc hai sẽ kiểm tra khoảng 

[ 
\frac{100000\cdot99999}{2}=4.999.950.000 
] 

cặp trong trường hợp xấu nhất. Điều đó vượt xa những gì một giải pháp cuộc thi nên cố gắng, ngay cả với giới hạn 10 giây hào phóng. Chúng ta cần khai thác cấu trúc dãy số Fibonacci để mỗi vị trí bổ sung có thể được xử lý trong thời gian không đổi. 

Có một số trường hợp nhỏ mà việc triển khai bất cẩn có thể thất bại. Đối với đầu vào`1`, không có cặp nào cả nên đáp án là`0`. Đối với đầu vào`2`, cặp duy nhất bao gồm (F_1=1) và (F_2=1), nhưng tổng của chúng là 2, không phải là Fibonacci, nên đáp án cũng là`0`. Một giải pháp chỉ đơn giản là đếm các chỉ số liên tiếp sẽ vô tình chấp nhận cặp này. 

Trường hợp tinh tế hơn là đầu vào`3`. Các cặp ((F_2,F_3)=(1,2)) và ((F_1,F_3)=(1,2)) đều hoạt động, vì tổng của chúng là 3, gcd của chúng là 1 và 3 là Fibonacci. Như vậy câu trả lời là`2`. Cặp ((F_1,F_2)) vẫn không hoạt động vì (1+1=2). Một giải pháp giả định rằng mọi cặp hợp lệ phải có các chỉ số liên tiếp sẽ chỉ tìm thấy một cặp và đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi cặp chỉ số (i<j) với (1\leq i,j\leq n). Đối với mỗi cặp, chúng ta có thể kiểm tra điều kiện gcd, kiểm tra xem (F_i+F_j) có phải là số lẻ hay không và xác định xem tổng đó có phải là số Fibonacci hay không. Với cấu trúc thành viên Fibonacci được tính toán trước, mỗi cặp có thể được kiểm tra theo thời gian không đổi hoặc logarit. Khó khăn không phải là kiểm tra cá nhân mà là số lượng cặp. Tại (n=100000), có 4.999.950.000 trong số đó, vì vậy ngay cả một tấm séc cực kỳ rẻ cũng sẽ quá chậm. 

Quan sát quan trọng đến từ chính sự tái diễn Fibonacci. Giả sử (i<j) và (j\geq4). Vì số Fibonacci tăng từ (F_2) trở đi nên 

[ 
F_j < F_i+F_j < F_j+F_{j-1}=F_{j+1} 
] 

bất cứ khi nào (i<j-1). Không có số Fibonacci nào nằm giữa (F_j) và (F_{j+1}), vì vậy tổng không thể là Fibonacci. Do đó, đối với các chỉ số thông thường, một cặp hợp lệ phải có (i=j-1). 

Có một ngoại lệ do (F_1=F_2) gây ra. Cặp ((1,3)) cho (1+2=3=F_4), mặc dù các chỉ số của nó không liên tiếp. Cặp đặc biệt này phải được tính riêng. 

Bây giờ chúng ta còn lại các cặp liên tiếp ((i,i+1)) cho (i\geq2). gcd của chúng tự động là 1 vì các số Fibonacci liên tiếp là nguyên tố cùng nhau. Tổng của chúng chính xác là (F_{i+2}), do đó điều kiện tổng Fibonacci cũng tự động được thỏa mãn. Câu hỏi duy nhất còn lại là liệu (F_{i+2}) có lẻ hay không. 

Tính chẵn lẻ Fibonacci lặp lại như 

[ 
1,1,0,1,1,0,\ldots 
] 

vì vậy (F_k) chẵn khi (k) chia hết cho 3. Do đó, cặp liên tiếp bắt đầu tại (i) hợp lệ chính xác khi (i+2) không chia hết cho 3, hoặc tương đương khi (i\not\equiv1\pmod3). 

Do đó, toàn bộ vấn đề trở thành một số tiền tố đơn giản. Bắt đầu từ (n=3), chúng ta có cặp đặc biệt ((1,3)) và mỗi vị trí Fibonacci mới sẽ thêm một ứng cử viên liên tiếp mới, điều này được chấp nhận trừ khi chỉ số bắt đầu của nó bằng 1 modulo 3. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(\max n + T)) | (O(\max n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các trường hợp thử nghiệm và tìm (n) truy vấn lớn nhất. Chúng tôi chỉ cần câu trả lời tối đa giá trị này, vì vậy một mảng tiền tố là đủ cho mọi truy vấn. 
2. Khởi tạo`ans[1] = 0`Và`ans[2] = 0`. Với ít hơn ba vị trí, không có cặp hoàn hảo. 
3. Đặt`ans[3] = 2`. Hai cặp hợp lệ là ((F_1,F_3)) và ((F_2,F_3)), cả hai đều đại diện cho các giá trị (1) và (2). Cặp ((F_1,F_2)) không hợp lệ vì tổng của nó là 2. 
4. Với mọi (n\geq4), hãy bắt đầu bằng`ans[n] = ans[n-1]`. Cặp mới duy nhất có thể là ((F_{n-1},F_n)), bởi vì mọi cặp cũ hơn đều đã được tính. 
5. Chấp nhận cặp mới đó khi ((n-1)\bmod3\neq1). Tổng của nó là (F_{n+1}) và nó lẻ chính xác khi (n+1) không chia hết cho 3. Vì (n+1\equiv n-2\pmod3), tổng này tương đương với (n-1\not\equiv1\pmod3). 
6. Thêm một bất cứ khi nào điều kiện được thỏa mãn. Điều này tạo ra số lượng tiền tố, do đó mọi truy vấn có thể được trả lời trong (O(1)). 

### Tại sao nó hoạt động 

Đối với bất kỳ cặp nào có chỉ số (i<j), ngoại trừ đẳng thức đặc biệt (F_1=F_2), phép lặp Fibonacci đặt (F_i+F_j) nằm giữa (F_j) và (F_{j+1}) trừ khi (i=j-1). Do đó mọi cặp không ngoại lệ hợp lệ đều liên tiếp. Các số Fibonacci liên tiếp là số nguyên tố cùng nhau và tổng của chúng là số Fibonacci tiếp theo, vì vậy chỉ còn lại tính chẵn lẻ. Vì các số Fibonacci chẵn chính xác tại các chỉ số chia hết cho 3, nên các cặp liên tiếp có chỉ số bắt đầu không phải là (1\bmod3) là hợp lệ. Cặp bổ sung duy nhất là ((F_1,F_3)), được bao gồm trong quá trình khởi tạo tại (n=3). Do đó, mảng tiền tố đếm mỗi cặp hợp lệ chính xác một lần và không có cặp không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    queries = [int(input()) for _ in range(t)]

    mx = max(queries)

    ans = [0] * (mx + 1)

    if mx >= 3:
        ans[3] = 2

    for n in range(4, mx + 1):
        ans[n] = ans[n - 1]

        # The new pair is (F[n-1], F[n]).
        # It is valid iff F[n+1] is odd.
        if (n - 1) % 3 != 1:
            ans[n] += 1

    sys.stdout.write("\n".join(str(ans[n]) for n in queries))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc mọi truy vấn trước khi xử lý trước vì chỉ mục được yêu cầu tối đa xác định mảng tiền tố phải được xây dựng bao xa. Điều này tránh thực hiện cùng một công việc riêng biệt cho nhiều trường hợp thử nghiệm. 

Việc khởi tạo tại`ans[3] = 2`xử lý cả giá trị trùng lặp (F_1=F_2=1) và cặp đặc biệt liên quan đến (F_1). Bắt đầu từ`n=4`giữ cho lần lặp lại chính sạch sẽ và ngăn chặn logic trong trường hợp đặc biệt bị rò rỉ vào mỗi lần lặp. 

Đối với mỗi cái mới`n`, cặp duy nhất không tồn tại đối với`n-1`là ((F_{n-1},F_n)). Tổng của nó là (F_{n+1}), vì vậy việc kiểm tra xem tổng có lẻ hay không cũng tương đương với việc kiểm tra xem (n+1) có chia hết cho 3 hay không. Mã sử ​​dụng điều kiện tương đương`(n - 1) % 3 != 1`. 

Không cần thiết phải tự xây dựng các giá trị Fibonacci. Kích thước của chúng trở nên khổng lồ khi (n) tăng lên, nhưng bằng chứng làm giảm hoàn toàn vấn đề về chỉ số số học và tính chẵn lẻ Fibonacci. Điều này cũng tránh làm việc với số nguyên có độ chính xác tùy ý trong Python. 

Mảng có độ dài tối đa là 100001, do đó mức sử dụng bộ nhớ nằm trong giới hạn 256 MB. Đầu ra được tạo ra với một cuối cùng`join`, tránh việc ghi chậm lặp đi lặp lại. 

## Ví dụ đã hoạt động 

Mẫu chính thức là```
6
1
4
8
17
20
25
```với đầu ra```
0
3
5
11
13
17
```Đối với truy vấn đầu tiên, không có đủ vị trí Fibonacci để tạo thành một cặp hợp lệ. 

| (n) | Chỉ số bắt đầu mới (i=n-1) | (i\bmod3) | Cặp mới hợp lệ? |`ans[n]`| 
| --- | --- | --- | --- | --- | 
| 1 | không | không | không | 0 | 
| 2 | không | không | không | 0 | 
| 3 | 2 cộng với cặp đặc biệt | 2 | vâng | 2 | 
| 4 | 3 | 0 | vâng | 3 | 
| 5 | 4 | 1 | không | 3 | 
| 6 | 5 | 2 | vâng | 4 | 
| 7 | 6 | 0 | vâng | 5 | 
| 8 | 7 | 1 | không | 5 | 

Đối với (n=4), ba cặp hợp lệ là ((F_1,F_3)), ((F_2,F_3)) và ((F_3,F_4)). Cặp mới tại (n=4) bắt đầu ở chỉ số 3, vì vậy nó hợp lệ. Tại (n=5), cặp mới bắt đầu ở chỉ số 4 và (F_6=8) là số chẵn nên bị từ chối. Điều này đưa ra câu trả lời mẫu`3`với (n=4) và`5`cho (n=8). 

Đối với các giá trị mẫu lớn hơn, số tiền tố tương tự vẫn tiếp tục mà không có bất kỳ giá trị Fibonacci nào được xây dựng. 

| (n) | Câu trả lời trước | Chỉ số khởi đầu mới | Cặp mới hợp lệ? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 17 | 10 | 16 | không | 11 | 
| 18 | 11 | 17 | vâng | 12 | 
| 19 | 12 | 18 | vâng | 13 | 
| 20 | 13 | 19 | không | 13 | 
| 24 | 15 | 23 | vâng | 16 | 
| 25 | 16 | 24 | vâng | 17 | 

Sự chuyển đổi tại (n=17) chứng tỏ rằng mô hình dựa trên chỉ số modulo 3, chứ không dựa trên độ lớn của các giá trị Fibonacci. các câu trả lời`11`,`13`, Và`17`tại (n=17,20,25) khớp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\max n + T)) | Mảng tiền tố được xây dựng một lần cho đến truy vấn lớn nhất, sau đó mỗi trường hợp kiểm thử sẽ được trả lời theo thời gian không đổi. | 
| Không gian | (O(\max n)) | Một câu trả lời số nguyên được lưu trữ cho mọi chỉ mục cho đến truy vấn lớn nhất. | 

Với (\max n=10^5), quá trình tiền xử lý chỉ thực hiện khoảng một trăm nghìn lần lặp, sau đó là một lần tra cứu liên tục cho mỗi trường hợp thử nghiệm. Điều này dễ dàng đủ nhỏ cho giới hạn thời gian 10 giây và chỉ sử dụng một phần nhỏ trong giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Câu lệnh ban đầu được sao chép trong lời nhắc bỏ qua đầu vào và đầu ra mẫu, nhưng kho lưu trữ Codeforces cung cấp mẫu chính thức dưới dạng sáu truy vấn có đầu ra`0, 3, 5, 11, 13, 17`.```python
import io
import sys

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    t = int(input())
    queries = [int(input()) for _ in range(t)]

    mx = max(queries)
    ans = [0] * (mx + 1)

    if mx >= 3:
        ans[3] = 2

    for n in range(4, mx + 1):
        ans[n] = ans[n - 1]
        if (n - 1) % 3 != 1:
            ans[n] += 1

    result = "\n".join(str(ans[n]) for n in queries)

    sys.stdin = old_stdin
    return result

# Official sample
assert solve_data(
    """6
1
4
8
17
20
25
"""
) == """0
3
5
11
13
17""", "official sample"

# Minimum boundary: there is no pair.
assert solve_data(
    """2
1
2
"""
) == """0
0""", "minimum sizes"

# Duplicate Fibonacci value F1 = F2 and the exceptional pair.
assert solve_data(
    """3
3
4
5
"""
) == """2
3
3""", "duplicate 1s and first valid pairs"

# A modulo-3 boundary where n=7 adds a pair but n=8 does not.
assert solve_data(
    """4
6
7
8
9
"""
) == """4
5
5
6""", "parity cycle"

# Maximum allowed n.
assert solve_data(
    """1
100000
"""
) == """66667""", "maximum n"

# Repeated queries must return the same prefix answer.
assert solve_data(
    """5
25
25
20
20
17
"""
) == """17
17
13
13
11""", "repeated queries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1, 2`|`0, 0`| Ranh giới kích thước tối thiểu và cặp không hợp lệ (1+1=2). | 
|`3, 4, 5`|`2, 3, 3`| Giá trị Fibonacci trùng lặp và cặp đặc biệt ((F_1,F_3)). | 
|`6, 7, 8, 9`|`4, 5, 5, 6`| Mẫu chẵn lẻ lặp lại modulo 3 và xử lý từng cái một. | 
|`100000`|`66667`| Kích thước đầu vào tối đa và ranh giới tiền tố cuối cùng. | 
| lặp đi lặp lại`25,20,17`|`17,13,11`| Tái sử dụng chính xác các câu trả lời được tính toán trước. | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là (n=1). đầu vào```
1
1
```chỉ chứa (F_1=1), vì vậy không có hai người riêng biệt nào có thể tạo thành một cặp. Thuật toán không bao giờ bước vào quá trình khởi tạo cho (n=3), để lại`ans[1]`bằng 0, đó là kết quả đầu ra đúng. 

Trường hợp cạnh thứ hai là (n=2). Cặp duy nhất có thể là hai vị trí chứa 1. gcd của chúng là 1, nhưng tổng của chúng là 2, đây không phải là số Fibonacci. Thuật toán rời đi`ans[2]=0`, do đó nó không vô tình coi các giá trị trùng lặp là tổng Fibonacci hợp lệ. 

Trường hợp cạnh lập chỉ mục quan trọng nhất là (n=3). Các giá trị Fibonacci là (1,1,2). Cặp được tạo bởi vị trí 2 và 3 có tổng (1+2=3) và cặp được tạo bởi vị trí 1 và 3 có các tính chất hoàn toàn giống nhau. Như vậy đáp án là 2. Việc khởi tạo`ans[3] = 2`bắt cả hai cặp. Một công thức chỉ dựa trên các chỉ số liên tiếp sẽ bỏ sót chỉ số đầu tiên. 

Đối với ranh giới modulo-3, hãy xem xét (n=5). Cặp liên tiếp mới là ((F_4,F_5)=(3,5)), có tổng là 8. Vì 8 là số chẵn nên cặp này không đạt được điều kiện tổng lẻ. Ở đây (n-1=4\equiv1\pmod3), chính xác là phần dư bị thuật toán loại bỏ. Câu trả lời vẫn là 3. 

Tại (n=6), cặp mới là ((F_5,F_6)=(5,8)), có tổng bằng 13, một số Fibonacci lẻ. Vì (n-1=5\not\equiv1\pmod3), thuật toán tăng câu trả lời từ 3 lên 4. Hai trường hợp liên tiếp này chứng minh tại sao điều kiện phải dựa trên chỉ số modulo 3. 

Cuối cùng, tại (n=100000), có 99998 chỉ số bắt đầu liên tiếp thông thường có thể có từ 2 đến 99999. Chính xác 33332 trong số chúng đồng dư với 1 modulo 3 và không đáp ứng điều kiện chẵn lẻ, để lại 66666 cặp liên tiếp hợp lệ. Việc thêm cặp đặc biệt ((F_1,F_3)) sẽ cho`66667`. Thuật toán đạt đến giá trị này chỉ bằng cách sử dụng phép lặp tiền tố mà không bao giờ xây dựng số Fibonacci khổng lồ (100000).
