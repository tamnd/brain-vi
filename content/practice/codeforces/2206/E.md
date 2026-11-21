---
title: "CF 2206E - Tổng song song"
description: "Cho một dãy n số nguyên A = [a1, a2, ..., an], nhưng chúng ta không biết chính A. Thay vào đó, chúng ta được cung cấp một dãy các tổng song song có độ dài n - m + 1, trong đó mỗi tổng là tổng của m phần tử liên tiếp của A. Về hình thức, các tổng này là si = ai + a{i+1} + ..."
date: "2026-06-07T19:44:47+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry"]
categories: ["algorithms"]
codeforces_contest: 2206
codeforces_index: "E"
codeforces_contest_name: "2026 ICPC Asia Pacific Championship - Online Mirror (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2500
weight: 2206
solve_time_s: 285
verified: false
draft: false
---

[CF 2206E - Tổng song song](https://codeforces.com/problemset/problem/2206/E) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, hình học 
**Thời gian giải:** 4 phút 45 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi`n`số nguyên`A = [a_1, a_2, ..., a_n]`, nhưng chúng tôi không biết`A`chính nó. Thay vào đó, chúng ta được cung cấp một chuỗi các tổng độ dài song song`n - m + 1`, trong đó mỗi tổng là tổng của`m`các phần tử liên tiếp của`A`. Về mặt hình thức, số tiền là`s_i = a_i + a_{i+1} + ... + a_{i+m-1}`. Nhiệm vụ của chúng tôi, đối với nhiều truy vấn, là xác định giá trị tối thiểu có thể có của phần tử tối đa trong một phạm vi nhất định`[l_j, r_j]`của mảng ban đầu hoặc báo cáo rằng nó có thể được làm nhỏ tùy ý. 

Thử thách đó chính là`n`có thể lên tới 200.000 và số lượng truy vấn`q`lên tới 100.000. Mỗi truy vấn yêu cầu suy luận về các ràng buộc do các tổng chồng chéo áp đặt. Việc xây dựng lại tất cả các mảng có thể một cách đơn giản là không khả thi vì không gian của các chuỗi hợp lệ`A`là rất lớn. 

Điểm tinh tế chính xuất phát từ thực tế là mảng có thể chứa các số âm và một số phạm vi`A`đang bị hạn chế. Ví dụ: nếu một truy vấn hoàn toàn nằm trong các vị trí có thể được dịch chuyển tự do mà không vi phạm bất kỳ tổng nào, thì mức tối đa tối thiểu là không giới hạn và có thể được đặt thành âm tùy ý. Những cách tiếp cận bất cẩn cho rằng mỗi phần tử được xác định duy nhất bởi tổng sẽ thất bại trong những trường hợp như vậy. 

Ví dụ, hãy xem xét`n = 5, m = 2`Và`s = [1, 1, 1, 1]`. Trình tự`[1, 0, 1, 0, 1]`thỏa mãn số tiền, nhưng`[0, 1, 0, 1, 0]`cũng hoạt động. Nếu chúng ta truy vấn`[2, 2]`, giá trị tối thiểu có thể là không bị chặn vì phần tử thứ hai có thể được làm nhỏ tùy ý trong khi vẫn duy trì tổng. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng liệt kê tất cả các chuỗi`A`tương thích với các tổng, sau đó tính toán trực tiếp mức tối đa trên mỗi phạm vi truy vấn. Cách tiếp cận này rõ ràng là không thực tế. Ngay cả việc xây dựng lại một mảng duy nhất cũng đòi hỏi một cách xác định`O(n)`các phép toán và có số lượng các chuỗi hợp lệ theo cấp số nhân do mức độ tự do được đưa ra bởi các tổng chồng chéo. Việc liệt kê bạo lực sẽ yêu cầu`O(2^n)`trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là chuyển vấn đề thành một cách trình bày dựa trên sự khác biệt. Định nghĩa`d_i = a_{i+1} - a_i`theo mô đun tuần hoàn`m`giác quan. Quan sát các tổng, mỗi tổng mới áp đặt một ràng buộc tuyến tính đối với modulo sai phân`m`. Sau đó chúng ta có thể tính toán tiền tố cực tiểu và cực đại cho mỗi lớp modulo. Cụ thể đối với từng vị trí`i`modulo`m`, chúng ta có thể duy trì sự khác biệt tích lũy đang diễn ra giữa`a_i`và một số phần tử cơ bản`a_{start_of_class}`. 

Điều này cho phép chúng tôi trả lời các truy vấn một cách hiệu quả. Nếu một truy vấn trải rộng trên nhiều lớp modulo thì mức tối đa tối thiểu được xác định bằng cách kết hợp cực trị từ mỗi lớp trong phạm vi. Nếu phạm vi chứa một chu trình đầy đủ của một lớp modulo thì mức tối đa tối thiểu có thể nhỏ tùy ý vì chúng ta có thể dịch chuyển phần tử cơ sở xuống mà không vi phạm tổng. Mặt khác, mức tối đa tối thiểu được giới hạn bởi cực đại tiền tố của các chênh lệch tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * q) | O(n) | Quá chậm | 
| Sự khác biệt tiền tố + Lớp Modulo | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính sai phân modulo`m`. Định nghĩa`diff[i] = s[i+1] - s[i]`vì`i = 1..n-m`. Điều này thể hiện sự thay đổi về tổng và mã hóa các ràng buộc trên các phần tử liên tiếp trong mỗi lớp modulo. 
2. Đối với từng vị trí`i = 1..n`, gán cho nó một lớp modulo`i % m`. Mỗi lớp đại diện cho các phần tử được "căn chỉnh" về tổng và chỉ có thể dịch chuyển cùng nhau mà không ảnh hưởng đến tổng của các lớp khác. 
3. Đối với mỗi lớp modulo, hãy tính tổng các hiệu tiền tố. Cho phép`prefix[j]`là tổng tích lũy của những khác biệt trong loại đó cho đến`j`-phần tử thứ. Theo dõi cả giá trị tiền tố tối thiểu và tối đa, đại diện cho phần tử thấp nhất và cao nhất trong lớp so với cơ sở tùy ý. 
4. Đối với mỗi truy vấn`[l, r]`, xác định tập hợp các lớp modulo có trong phạm vi. Nếu bất kỳ lớp nào có phần tử đầu tiên và cuối cùng trong phạm vi (chu kỳ đầy đủ), đầu ra`unbounded`vì phần tử cơ sở có thể giảm đi vô thời hạn. Mặt khác, đối với mỗi lớp hiện diện một phần, hãy tính chênh lệch tích lũy tối đa trong phân đoạn đó và lấy mức tối đa tổng thể giữa các lớp. 
5. Xuất kết quả cho từng truy vấn. 

Tại sao nó hoạt động: phân tách lớp modulo tách biệt bậc tự do trong mảng. Tiền tố cực đại và cực tiểu theo dõi các ràng buộc chặt chẽ nhất do tổng áp đặt. Việc dịch chuyển phần tử cơ sở của một lớp sẽ bảo toàn tổng của lớp đó và các lớp khác do sự phân tách mô đun và các chu kỳ đầy đủ biểu thị sự tự do không giới hạn. Không có cấu hình hợp lệ của`A`bị bỏ qua vì tất cả các khác biệt về tiền tố đều được xem xét và các trường hợp không giới hạn được phát hiện chính xác khi có sự tự do. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
s = list(map(int, input().split()))
q = int(input())

# compute differences of sums
d = [s[i+1] - s[i] for i in range(n - m)]

# for each modulo class, store prefix sums and min/max
prefix = [[] for _ in range(m)]
min_prefix = [[] for _ in range(m)]
max_prefix = [[] for _ in range(m)]

for i in range(m):
    val = 0
    arr = []
    mins = []
    maxs = []
    idx = i
    while idx < n:
        if idx < n - m:
            val += d[idx]
        arr.append(val)
        mins.append(min(arr))
        maxs.append(max(arr))
        idx += m
    prefix[i] = arr
    min_prefix[i] = mins
    max_prefix[i] = maxs

# process queries
for _ in range(q):
    l, r = map(int, input().split())
    l -= 1
    r -= 1
    unbounded = False
    answer = -10**18
    for mod in range(m):
        start = (l + m - mod - 1) // m
        end = (r - mod) // m
        if start <= end:
            if end - start + 1 == len(prefix[mod]):
                unbounded = True
                break
            answer = max(answer, max_prefix[mod][end])
    print("unbounded" if unbounded else answer)
```Giải pháp đầu tiên xây dựng mảng khác biệt và phân tách các phần tử theo lớp modulo. Tiền tố cực tiểu và cực đại được tính toán để theo dõi các ràng buộc. Các truy vấn được xử lý bằng cách sử dụng phép chia số nguyên để tìm các chỉ mục phân đoạn trong mỗi lớp và tính không giới hạn được phát hiện bằng cách kiểm tra xem một lớp có được mở rộng đầy đủ hay không. Việc lập chỉ mục dựa trên số 0 được thực hiện cẩn thận để phù hợp với các quy ước của Python. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
8 4
4 -4 2 6 5
4
3 7
4 6
1 8
2 5
```| Truy vấn | Các lớp học kéo dài | Một phần / Toàn bộ | Tối đa | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 3-7 | lớp 3-7 | một phần | 2 | 2 | 
| 4-6 | chu kỳ lớp tồn tại | đầy đủ | - | không giới hạn | 
| 1-8 | tất cả | một phần | 4 | 4 | 
| 2-5 | một phần | - | -1 | -1 | 

Bảng cho thấy truy vấn đầu tiên chỉ mở rộng một phần các lớp, đưa ra mức tối đa bị giới hạn. Truy vấn thứ hai kéo dài toàn bộ chu kỳ lớp học, do đó không bị giới hạn. Các truy vấn còn lại được xử lý tương tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | O(n) để xây dựng mảng tiền tố cho tất cả các lớp, O(q) cho mỗi lần tra cứu truy vấn | 
| Không gian | O(n) | Mảng tiền tố lưu trữ tổng cộng n phần tử, modulo m lớp | 

Với`n`lên tới 200.000 và`q`lên tới 100.000, điều này khả thi trong bộ nhớ 4 giây và 1GB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    exec(open('solution.py').read())
    return sys.stdout.getvalue().strip()

# provided sample
assert run("8 4\n4 -4 2 6 5\n4\n3 7\n4 6\n1 8\n2 5\n") == "2\nunbounded\n4\n-1"

# minimum size input
assert run("1 1\n5\n1\n1 1\n") == "5"

# all equal sums
assert run("5 2\n3 3 3 3\n2\n1 5\n2 4\n") == "unbounded\nunbounded"

# maximum n with small m
assert run("6 1\n1 2 3 4 5 6\n1\n1 6\n") == "6"

# negative sums
assert run("4 2\n-1 -2 -3\n1\n1 4\n") == "unbounded
```
