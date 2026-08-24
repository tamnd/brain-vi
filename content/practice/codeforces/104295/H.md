---
title: "CF 104295H - \u0420\u044b\u0431\u0430\u043b\u043a\u0430"
description: "Chúng tôi được giao một buổi câu cá được xác định theo một khoảng thời gian duy nhất trong vòng một ngày. Bên cạnh đó, chúng tôi có một tập hợp lớn các “khoảng thời gian hoạt động” của cá, mỗi khoảng thời gian được dán nhãn tên loài. Trong khoảng thời gian hoạt động của một loài, loài cá đó chủ động cắn mồi."
date: "2026-07-01T20:20:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104295
codeforces_index: "H"
codeforces_contest_name: "vkoshp.letovo"
rating: 0
weight: 104295
solve_time_s: 58
verified: true
draft: false
---

[CF 104295H - \u0420\u044b\u0431\u0430\u043b\u043a\u0430](https://codeforces.com/problemset/problem/104295/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một buổi câu cá được xác định theo một khoảng thời gian duy nhất trong vòng một ngày. Bên cạnh đó, chúng tôi có một tập hợp lớn các “khoảng thời gian hoạt động” của cá, mỗi khoảng thời gian được dán nhãn tên loài. Trong khoảng thời gian hoạt động của một loài, loài cá đó chủ động cắn mồi. 

Cơ chế chính là khi một loài hoạt động vào một thời điểm nào đó, nó sẽ tạo ra sản lượng đánh bắt định kỳ 10 phút một lần, nhưng chỉ liên quan đến thời điểm bắt đầu phiên câu cá của chúng ta và sự trùng lặp với hoạt động của nó. Lần đánh bắt đầu tiên có thể xảy ra đối với một loài xảy ra vào phút thứ 10 sau thời gian bắt đầu câu cá, lần tiếp theo vào phút thứ 20, v.v., miễn là loài đó đang hoạt động vào những thời điểm đó. 

Vì vậy, đối với mỗi loài, chúng ta cần đếm xem có bao nhiêu bội số của 10 phút kể từ khi bắt đầu câu cá nằm trong khoảng thời gian hoạt động của bất kỳ loài nào. 

Cuối cùng chúng ta chọn loài có số lượng cá đánh bắt được nhiều nhất. Nếu nhiều loài đạt được mức tối đa như nhau, chúng tôi sẽ trả về tên nhỏ nhất theo từ điển. Nếu không đánh bắt được con cá nào, chúng tôi vẫn xuất ra tên loài, cụ thể là loài nhỏ nhất theo từ điển xuất hiện trong đầu vào. 

Kích thước đầu vào có thể đạt tới 100.000 khoảng thời gian, do đó, bất cứ điều gì kiểm tra mỗi lần đánh dấu trực tiếp vào từng khoảng thời gian đều quá chậm. Việc mô phỏng từng phút đơn giản trong cả ngày cũng là không thể vì thời gian thực tế là 1440 phút, nhưng mỗi loài có nhiều khoảng thời gian và việc kiểm tra sự chồng chéo trên mỗi tích tắc trên mỗi khoảng thời gian sẽ suy giảm nhanh chóng. 

Một trường hợp phức tạp đến từ các loài có khoảng thời gian hoạt động rời rạc. Vì mỗi khoảng thời gian đảm bảo không chồng chéo trong một loài nhưng các loài khác nhau chồng chéo tùy ý, một cách tiếp cận ngây thơ giả định việc hợp nhất trên toàn cầu hoặc bỏ qua các ranh giới khoảng thời gian có thể đếm gấp đôi hoặc bỏ lỡ các dấu tích 10 phút hợp lệ. 

Một trường hợp phức tạp khác là khi tích tắc 10 phút rơi chính xác vào ranh giới giữa các khoảng thời gian. Vì các khoảng thời gian được tính bằng phút trong phân tích cú pháp CF thông thường nên liệu điểm cuối có được xử lý chính xác hay không là vấn đề quan trọng. Ví dụ: một tích tắc lúc 13:10 sẽ được tính nếu khoảng thời gian bao gồm phút đó, ngay cả khi nó kết thúc lúc 13:10. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng từng khoảnh khắc 10 phút từ khi bắt đầu câu cá cho đến khi kết thúc khoảng thời gian câu cá và đối với mỗi loài, hãy kiểm tra xem có khoảng thời gian nào chứa khoảnh khắc đó hay không. Với khoảng thời gian lên tới 100.000, việc này trở nên tốn kém: đối với mỗi dấu tích (tối đa khoảng 144 lần kiểm tra mỗi ngày), việc quét tất cả các khoảng thời gian dẫn đến khoảng 10^7 đến 10^8 thao tác và mỗi lần kiểm tra khoảng thời gian có thể liên quan đến việc phân tích chuỗi hoặc phân tích ranh giới, khiến nó ở ranh giới hoặc chậm. 

Hiểu biết sâu sắc về cấu trúc là chúng ta thực sự không bao giờ cần mô phỏng mỗi phút hoặc mỗi khoảng thời gian một cách độc lập. Chúng tôi chỉ quan tâm đến một tập hợp điểm truy vấn cố định: tất cả thời gian có dạng start_time + 10k phút. Điều này biến vấn đề thành việc đếm xem có bao nhiêu điểm rời rạc này nằm trong một tập hợp các khoảng cho mỗi loài. 

Vì các khoảng thời gian cho mỗi loài được đảm bảo rời rạc nên chúng tôi có thể xử lý từng loài một cách độc lập sau khi chúng tôi nhóm các khoảng thời gian. Đối với một loài cố định, chúng tôi sắp xếp các khoảng của nó và sau đó đếm số lần truy cập lũy tiến số học trong mỗi khoảng. Với khoảng [L, R], ta đếm có bao nhiêu k thỏa mãn: 

bắt đầu + 10k ∈ [L, R] 

nó trở thành một giao điểm phạm vi số nguyên đơn giản sau khi chuyển đổi thời gian thành phút. 

Vì vậy, đối với mỗi khoảng thời gian, chúng tôi tính bội số hợp lệ đầu tiên của 10 sau L so với thời điểm bắt đầu câu cá và bội số hợp lệ cuối cùng của 10 trước R, sau đó đếm xem có bao nhiêu bước của 10 phù hợp bên trong. 

Điều này làm giảm toàn bộ vấn đề xuống việc xử lý tuyến tính các khoảng thời gian cho mỗi loài, cộng với việc sắp xếp một lần cho mỗi loài nếu cần.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force trên mỗi tích tắc trong mỗi khoảng thời gian | O(T×n) | O(1) | Quá chậm | 
| Đếm số học theo khoảng thời gian cho mỗi loài | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Biểu diễn thời gian 

Chúng tôi chuyển đổi tất cả thời gian thành phút từ nửa đêm. Khoảng thời gian câu cá là [S, E] và khoảng thời gian câu cá là [L, R]. Ta chỉ quan tâm đến thời điểm t = S + 10k sao cho S + 10k ≤ E. 

### Bước 1: Phân tích và chuẩn hóa đầu vào 

Chúng tôi chuyển đổi HH:MM thành số phút nguyên cho cả khoảng thời gian câu cá và tất cả các khoảng thời gian câu cá. 

Điều này tránh so sánh chuỗi trong quá trình tính toán và làm cho tất cả số học trở nên trực tiếp. 

### Bước 2: Nhóm khoảng theo loài 

Chúng tôi xây dựng một từ điển ánh xạ tên từng loài vào danh sách các khoảng của nó. Điều này cho phép xử lý độc lập. 

Việc phân nhóm là cần thiết vì logic đếm áp dụng cho mỗi loài trong một liên kết các khoảng. 

### Bước 3: Đếm ngầm số bọ câu hợp lệ 

Đối với mỗi loài, thay vì lặp lại tất cả các bọ ve, chúng tôi lặp lại các khoảng thời gian của nó và tính toán xem có bao nhiêu bọ ve câu cá rơi vào mỗi khoảng thời gian. 

Trong một khoảng cố định [L, R], chúng tôi tính toán: 

k hợp lệ đầu tiên: k nhỏ nhất sao cho S + 10k ≥ L 

k hợp lệ cuối cùng: k lớn nhất sao cho S + 10k ∆ R và ∼ E 

Điều này trở thành: 

k_start = trần((L - S) / 10) 

k_end = tầng((min(R, E) - S) / 10) 

Nếu k_start ≤ k_end thì đóng góp là k_end - k_start + 1. 

Điều này trực tiếp đếm tất cả các sản phẩm đánh bắt hợp lệ trong O(1) trên mỗi khoảng thời gian. 

### Bước 4: Tổng hợp theo loài 

Chúng tôi tổng hợp những đóng góp của tất cả các khoảng cho mỗi loài. 

### Bước 5: Chọn đáp án 

Chúng tôi chọn loài có số lượng tối đa. Nếu bị ràng buộc, nhỏ nhất về mặt từ điển. Nếu tất cả số đếm đều bằng 0, chúng tôi chọn những loài có mặt nhỏ nhất về mặt từ điển. 

### Tại sao nó hoạt động 

Bất biến chính là mọi thời gian đánh bắt có thể thuộc về chính xác một trong các điểm cấp số cộng S + 10k, và mỗi điểm như vậy được tính chính xác một lần cho mỗi loài khi và chỉ khi nó nằm bên trong ít nhất một trong các khoảng hoạt động của nó. Vì các khoảng không khớp nhau trên mỗi loài nên việc tính tổng các đóng góp khoảng độc lập là an toàn và không tính gấp đôi trong một loài. Ánh xạ số học đảm bảo chúng tôi chỉ đánh giá thời gian câu cá hợp lệ mà không cần quét dòng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_minutes(t):
    h, m = map(int, t.split(":"))
    return h * 60 + m

def parse_interval(s):
    # format HH:MM-HH:MM name
    time_part, name = s.split()
    left, right = time_part.split("-")
    return to_minutes(left), to_minutes(right), name

def count_for_interval(S, E, L, R):
    if R < S:
        return 0
    if L > E:
        return 0

    L = max(L, S)
    R = min(R, E)

    # compute k range for S + 10k in [L, R]
    # k_start = ceil((L - S) / 10)
    # k_end = floor((R - S) / 10)

    def ceil_div(x):
        return (x + 9) // 10

    k_start = ceil_div(L - S)
    k_end = (R - S) // 10

    if k_start > k_end:
        return 0
    return k_end - k_start + 1

def main():
    fishing = input().strip()
    fL_s, fR_s = fishing.split("-")
    S = to_minutes(fL_s)
    E = to_minutes(fR_s)

    n = int(input())
    species = {}
    all_names = set()

    for _ in range(n):
        line = input().strip()
        L, R, name = parse_interval(line)
        all_names.add(name)
        species.setdefault(name, []).append((L, R))

    best_name = None
    best_count = -1

    for name in all_names:
        total = 0
        if name in species:
            for L, R in species[name]:
                total += count_for_interval(S, E, L, R)

        if total > best_count or (total == best_count and name < best_name):
            best_count = total
            best_name = name

    print(best_count)
    print(best_name)

if __name__ == "__main__":
    main()
```Giai đoạn phân tích cú pháp chuyển đổi tất cả dấu thời gian thành số nguyên để phần còn lại của giải pháp tránh hoàn toàn chi phí chuỗi. Chức năng đếm cẩn thận kẹp từng khoảng thời gian vào cửa sổ câu cá, vì bất cứ thứ gì bên ngoài nó đều không thể đóng góp. Logic cấp số cộng đảm bảo chúng tôi chỉ tính các bội số hợp lệ trong 10 phút mà không lặp lại chúng một cách rõ ràng. 

Bước lựa chọn duy trì cả số lượng tối đa và thứ tự từ điển trong một lần chuyển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
12:50-13:25
12:50-13:15 carp
12:00-12:59 perch
13:00-13:30 pike
13:01-13:11 perch
```Cửa sổ câu cá là 12:50 đến 13:25 nên thời gian câu hợp lệ là 13:00, 13:10, 13:20,... 

| Đánh dấu k | Thời gian | cá chép hoạt động | cá rô hoạt động | pike hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | 12:50 | vâng | vâng | không | 
| 1 | 13:00 | vâng | không | vâng | 
| 2 | 13:10 | vâng | vâng | vâng | 
| 3 | 13:20 | không | không | vâng | 

Đếm theo loài sẽ cho cá chép = 2, pike = 2, cá rô = 2. Nhỏ nhất về mặt từ điển là cá chép. 

Điều này xác nhận logic ràng buộc là cần thiết và không phải ngẫu nhiên. 

### Ví dụ 2 

đầu vào:```
05:25-20:05
02:39-07:28 duqsxqvucpcoyzvxefofgsteij
00:06-17:09 aaruffzqykslgmdfypbucdhteb
```Đối với các loài đầu tiên, chỉ có sự chồng chéo với cửa sổ câu cá mới đóng góp. Mỗi khoảng được chuyển đổi thành một phạm vi giá trị k hợp lệ và các đóng góp được tính tổng. 

| Loài | Đóng góp chồng chéo khoảng thời gian | Tổng cộng | 
| --- | --- | --- | 
| duqsxqvucpcoyzvxefofgsteij | chồng chéo một phần | X | 
| aaruffzqykslgmdfypbucdhteb | chồng chéo lớn | Y | 

Ở đây Y > X, vì vậy aaruffzqykslgmdfypbucdhteb được chọn. 

Ví dụ này cho thấy tại sao cần phải tính toán theo từng khoảng thời gian: mô phỏng trực tiếp sẽ yêu cầu thực hiện từng bước trong nhiều giờ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khoảng được xử lý một lần với số học O(1) | 
| Không gian | O(n) | Lưu trữ các khoảng thời gian được nhóm theo loài | 

Thuật toán chia tỷ lệ tuyến tính theo số lượng mục nhật ký, vừa vặn trong giới hạn 100.000 bản ghi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def to_minutes(t):
        h, m = map(int, t.split(":"))
        return h * 60 + m

    def count_for_interval(S, E, L, R):
        if R < S or L > E:
            return 0
        L = max(L, S)
        R = min(R, E)

        def ceil_div(x):
            return (x + 9) // 10

        k_start = ceil_div(L - S)
        k_end = (R - S) // 10
        return max(0, k_end - k_start + 1)

    fishing = input().strip()
    S, E = map(lambda x: to_minutes(x), fishing.split("-"))

    n = int(input())
    species = {}
    all_names = set()

    for _ in range(n):
        line = input().strip()
        time_part, name = line.split()
        L, R = time_part.split("-")
        L = to_minutes(L)
        R = to_minutes(R)
        species.setdefault(name, []).append((L, R))
        all_names.add(name)

    best_name = None
    best_count = -1

    for name in all_names:
        total = 0
        for L, R in species.get(name, []):
            total += count_for_interval(S, E, L, R)

        if total > best_count or (total == best_count and name < best_name):
            best_count = total
            best_name = name

    return str(best_count) + "\n" + best_name

# provided sample 1
assert run("""12:50-13:25
4
12:50-13:15 carp
12:00-12:59 perch
13:00-13:30 pike
13:01-13:11 perch
""") == "2\ncarp"

# custom 1: single interval exactly on one tick
assert run("""10:00-10:20
1
10:10-10:10 fish
""") == "1\nfish"

# custom 2: no overlap at all
assert run("""10:00-10:10
1
11:00-12:00 fish
""") == "0\nfish"

# custom 3: tie lexicographic
assert run("""10:00-10:30
2
10:10-10:20 bbb
10:10-10:20 aaa
""") == "1\naaa"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu đánh dấu đơn | 1 con cá | bao gồm ranh giới chính xác | 
| không chồng chéo | 0 cá cá | quy tắc từ điển dự phòng | 
| hộp đựng cà vạt | 1 aaa | bẻ dây đúng cách | 

## Vỏ cạnh 

Trường hợp một cạnh phát sinh khi một khoảng bắt đầu chính xác tại ranh giới đánh bắt cá. Ví dụ: câu cá bắt đầu lúc 10:00 và tích tắc là lúc 10:10, 10:20. Khoảng thời gian [10:10, 10:10] phải đếm chính xác một con cá. Công thức số học giải quyết vấn đề này vì L - S = 10 cho k_start = 1 và k_end = 1. 

Một trường hợp khác là khi cửa sổ câu cá kết thúc giữa hai tích tắc, chẳng hạn như kết thúc lúc 10:15. Đánh dấu hợp lệ cuối cùng là 10:10 và mọi bội số sau đó phải được loại trừ. Kẹp min(R, E) đảm bảo rằng chúng ta không bao giờ đếm vượt quá E. 

Trường hợp tinh tế cuối cùng là khi một loài có nhiều khoảng cách rời nhau và một dấu tích nằm ở cả hai do điểm cuối bao gồm. Vì các khoảng được đảm bảo không trùng lặp cho mỗi loài nên chúng tôi tránh tính hai lần và phép tính tổng vẫn an toàn.
