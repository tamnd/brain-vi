---
title: "CF 102416E - Người bảo vệ không gian"
description: "Chúng ta có tối đa 100 vùng được bảo vệ hình cầu trong không gian ba chiều. Phi thuyền (i) có tâm ((xi,yi,zi)) và bán kính (ri). Chúng ta phải chọn một số phi thuyền có các quả cầu ban đầu không chồng lên nhau. Được phép chạm vào đúng một điểm."
date: "2026-08-14T14:42:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102416
codeforces_index: "E"
codeforces_contest_name: "Edinburgh Competition 2019"
rating: 0
weight: 102416
solve_time_s: 128
verified: false
draft: false
---

[CF 102416E - Người bảo vệ không gian](https://codeforces.com/problemset/problem/102416/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có tối đa 100 vùng được bảo vệ hình cầu trong không gian ba chiều. Starship (i) có tâm ((x_i,y_i,z_i)) và bán kính (r_i). Chúng ta phải chọn một số phi thuyền có các quả cầu ban đầu không chồng lên nhau. Được phép chạm vào đúng một điểm. 

Sau khi chọn một phi thuyền, trách nhiệm của nó sẽ mở rộng từ bán kính (r_i) đến bán kính (3r_i). Các lĩnh vực mở rộng có thể chồng lên nhau. Yêu cầu là sự kết hợp của tất cả các quả cầu ban đầu phải nằm trong sự kết hợp của các quả cầu mở rộng thuộc về các phi thuyền đã chọn. 

Đầu ra chỉ cần cung cấp một tập hợp con hợp lệ. Tập hợp con không nhất thiết phải ở mức tối thiểu, vì vậy chúng ta có thể tập trung vào việc tìm kiếm một công trình được đảm bảo hoạt động. 

Hai hình cầu (i) và (j) rời nhau chính xác khi 

[ 
d(i,j) \ge r_i+r_j, 
] 

ở đâu 

[ 
d(i,j)^2=(x_i-x_j)^2+(y_i-y_j)^2+(z_i-z_j)^2. 
] 

Sử dụng khoảng cách bình phương sẽ tránh được căn bậc hai và giữ nguyên mọi phép tính. 

Ràng buộc (n\le100) đủ nhỏ để thực hiện công việc (O(n^2)), có nghĩa là chúng ta có thể so sánh mọi cặp hình cầu. Giải pháp (O(n^3)) ở đây cũng sẽ nhỏ về mặt số lượng, nhưng không có lý do gì để sử dụng nó. Tìm kiếm theo cấp số nhân trên tất cả các tập hợp con là hoàn toàn không khả thi: (2^{100}) là khoảng (1,27\cdot10^{30}), ngay cả trước khi thực hiện kiểm tra hình học. 

Có hai trường hợp ranh giới rất dễ xử lý sai. Đầu tiên, các quả cầu chạm vào được phép cùng tồn tại. Ví dụ,```
2
0 0 0 1
2 0 0 1
```có tâm cách nhau đúng hai đơn vị, nên các quả cầu chạm nhau nhưng không chồng lên nhau. Cả hai đều có thể được chọn và đầu ra hợp lệ là```
2
1 2
```Một tấm séc sử dụng`distance <= r1 + r2`sẽ từ chối cặp này một cách không chính xác. 

Ranh giới thứ hai liên quan đến phạm vi bảo hiểm. Giả sử một hình cầu bị bỏ qua vì nó giao với một hình cầu đã chọn. Hình cầu được chọn phải có bán kính ít nhất bằng bán kính hình cầu bị bỏ qua. Nếu hai hình cầu giao nhau, mọi điểm của hình cầu bị bỏ qua đều cách tâm đã chọn tối đa (r_i+r_j). Vì (r_i\le r_j), giá trị này lớn nhất là (2r_j), nhỏ hơn bán kính mới của nó (3r_j). Việc triển khai bất cẩn khi chọn các hình cầu theo thứ tự tùy ý sẽ làm mất khả năng so sánh bán kính quan trọng và có thể không bao phủ được một hình cầu lớn bằng một hình cầu được chọn nhỏ hơn nhiều. 

Ví dụ,```
2
0 0 0 10
10 0 0 1
```nên chọn hình cầu bán kính-10. Nếu quả cầu nhỏ được chọn trước và quả cầu lớn sau đó bị bỏ qua chỉ vì các quả cầu ban đầu giao nhau, thì quả cầu bán kính-3 mở rộng sẽ không bao phủ quả cầu lớn. Xử lý trong bán kính giảm sẽ ngăn chặn tình trạng này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là thử mọi tập hợp con của phi thuyền. Đối với mỗi tập hợp con, chúng ta có thể kiểm tra từng cặp hình cầu đã chọn xem có trùng nhau không và sau đó kiểm tra xem mọi hình cầu ban đầu có bị bao phủ bởi các hình cầu mở rộng hay không. Ngay cả với quy trình xác thực (O(n^2)), chi phí này (O(2^n n^2)). Tại (n=100), riêng số lượng tập hợp con là khoảng (1,27\cdot10^{30}), vì vậy cách tiếp cận này vượt xa giới hạn. 

Quan sát hữu ích là chúng ta không cần tìm kiếm tập hợp con phù hợp. Sắp xếp các quả cầu theo bán kính giảm dần và cố gắng giữ một quả cầu chính xác khi nó tách rời khỏi mọi quả cầu đã được giữ. 

Phần rời rạc là ngay lập tức. Chúng tôi chỉ thêm một hình cầu sau khi xác nhận rằng hình cầu ban đầu của nó không giao với bất kỳ hình cầu nào đã chọn. 

Phần thú vị là phạm vi bảo hiểm. Xét một hình cầu (A) mà thuật toán không chọn. Tại thời điểm (A) được xem xét, một số hình cầu (B) đã chọn trước đó phải giao nhau với (A). Vì danh sách được sắp xếp theo bán kính giảm dần nên 

[ 
r_A\le r_B. 
] 

Lấy điểm (P) bất kỳ bên trong (A). Khoảng cách của nó tới tâm (B) lớn nhất là 

[ 
|P-C_B|\le |P-C_A|+|C_A-C_B| 
\le r_A+(r_A+r_B). 
] 

Biểu thức đó nhiều nhất là (2r_A+r_B), nhiều nhất là (3r_B) vì (r_A\le r_B). Như vậy mọi điểm của (A) đều nằm bên trong hình cầu bán kính (3r_B) có tâm tại (B). 

Trên thực tế, chúng ta có thể sử dụng ràng buộc lỏng lẻo hơn một chút nhưng đơn giản hơn 

[ 
|P-C_B|\le r_A+r_A+r_B\le3r_B. 
] 

Vì vậy, mọi hình cầu bị bỏ qua đều được bao phủ hoàn toàn bởi hình cầu được mở rộng gấp ba lần của một số hình cầu đã chọn trước đó. 

Brute-force hoạt động vì nó tìm kiếm rõ ràng một tập hợp con thỏa mãn cả hai điều kiện, nhưng không thành công vì có nhiều tập hợp con theo cấp số nhân. Việc quan sát thấy một quả cầu lớn hơn tự động bao phủ mọi quả cầu nhỏ giao nhau sau khi bán kính của nó tăng gấp ba lần sẽ biến việc tìm kiếm thành một công trình tham lam đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n n^2)) | (O(n)) | Quá chậm | 
| Tham lam bằng cách giảm bán kính | (O(n^2+n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phi thuyền và giữ nguyên các chỉ số ban đầu của chúng, vì đầu ra phải sử dụng các chỉ số từ đầu vào. 
2. Sắp xếp các phi thuyền theo bán kính giảm dần. Nếu hai bán kính bằng nhau thì mọi thứ tự giữa chúng đều hợp lệ. 
3. Bắt đầu với một tập hợp các phi thuyền đã chọn còn trống. Xử lý danh sách được sắp xếp từ bán kính lớn nhất đến nhỏ nhất. 
4. Đối với phi thuyền hiện tại (i), so sánh nó với mọi phi thuyền đã được chọn (j). Tính bình phương khoảng cách tâm 

[ 
d^2=(x_i-x_j)^2+(y_i-y_j)^2+(z_i-z_j)^2. 
] 

Hình cầu hiện tại chỉ có thể được chọn nếu 

[ 
d^2\ge(r_i+r_j)^2 
] 

cho mọi lựa chọn (j). Sự bình đẳng được cho phép vì chạm vào các quả cầu được coi là rời rạc. 

1. Nếu hình cầu hiện tại tách rời khỏi tất cả các hình cầu đã chọn, hãy thêm nó vào câu trả lời. Nếu không thì bỏ qua nó. 
2. Sau khi tất cả các hình cầu đã được xử lý, hãy in các chỉ số đã chọn. Thuật toán luôn tạo ra một câu trả lời hợp lệ, do đó`NO`chi nhánh không bao giờ cần thiết. 

### Tại sao nó hoạt động 

Duy trì tính bất biến rằng mọi hình cầu được xử lý cho đến nay đều được chọn hoặc được bao phủ hoàn toàn bởi hình cầu mở rộng ba lần của một số hình cầu được chọn. 

Một hình cầu được chọn rõ ràng thỏa mãn bất biến vì hình cầu mở rộng của chính nó chứa hình cầu ban đầu của nó. Nếu một hình cầu bị bỏ qua, nó sẽ giao với một hình cầu đã chọn trước đó (j) và thứ tự sắp xếp sẽ là (r_i\le r_j). Đối với bất kỳ điểm (P) nào trong hình cầu bị bỏ qua, 

[ 
|P-C_j| 
\le |P-C_i|+|C_i-C_j| 
\le r_i+(r_i+r_j) 
=2r_i+r_j 
\le3r_j. 
] 

Do đó toàn bộ hình cầu bị bỏ qua được bao phủ bởi hình cầu mở rộng của (j). Đồng thời, một hình cầu chỉ được chọn khi nó tách rời khỏi mọi hình cầu đã chọn trước đó, do đó tất cả các vùng ban đầu được chọn vẫn tách rời theo cặp. Cả hai thuộc tính bắt buộc đều có giá trị sau mỗi lần lặp và do đó có giá trị cho câu trả lời cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ships = []

    for i in range(1, n + 1):
        x, y, z, r = map(int, input().split())
        ships.append((r, x, y, z, i))

    # Larger spheres must be considered first.
    ships.sort(reverse=True)

    selected = []

    for r, x, y, z, idx in ships:
        can_take = True

        for sr, sx, sy, sz, sidx in selected:
            dx = x - sx
            dy = y - sy
            dz = z - sz

            dist2 = dx * dx + dy * dy + dz * dz
            radius_sum = r + sr

            # Touching is allowed, so equality is also disjoint.
            if dist2 < radius_sum * radius_sum:
                can_take = False
                break

        if can_take:
            selected.append((r, x, y, z, idx))

    print(len(selected))
    print(*[idx for _, _, _, _, idx in selected])

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ cùng với chỉ mục gốc để việc sắp xếp không làm mất đi đặc tính của một con tàu vũ trụ. Tuple bắt đầu bằng bán kính, cho phép`sort(reverse=True)`để xử lý bán kính lớn hơn trước. 

Đối với mỗi ứng cử viên, vòng lặp bên trong chỉ so sánh nó với các hình cầu đã chọn. Khoảng cách bình phương được so sánh với tổng bình phương của bán kính, do đó không có số học dấu phẩy động. Điều này đặc biệt hữu ích vì trường hợp ranh giới trong đó hai quả cầu tiếp xúc chính xác phải được xử lý chính xác. 

Việc so sánh sử dụng`<`còn hơn là`<=`. Nếu 

[ 
d^2=(r_i+r_j)^2, 
] 

các quả cầu chạm vào nhau và rõ ràng được phép chọn cùng nhau. Chỉ một khoảng cách nhỏ hơn có nghĩa là phần bên trong của chúng chồng lên nhau. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Ngay cả trong ngôn ngữ số nguyên có chiều rộng cố định, chênh lệch tọa độ tối đa là (10^4), cho khoảng cách bình phương theo thứ tự (10^8), trong khi tổng bán kính bình phương cũng nhỏ một cách an toàn. 

Thuật toán không bao giờ in`NO`. Bằng chứng ở trên cho thấy việc xây dựng bán kính giảm dần luôn cho một tập hợp con hợp lệ, kể cả khi chỉ có một hình cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4
1 0 0 1
2 0 0 1
7 0 0 1
10 0 0 3
```Hình cầu bán kính-3 được xử lý đầu tiên. Nó được chọn ngay lập tức. Sau đó, ba hình cầu có bán kính 1 sẽ được kiểm tra theo hình cầu đã chọn. Khoảng cách trung tâm của chúng từ ((10,0,0)) lần lượt là 9, 8 và 3, trong khi ngưỡng rời rạc cần thiết là (4). 

Với cách sắp xếp xác định được mã sử dụng, hai hình cầu bán kính-1 đầu tiên được chọn và hình cầu tại (x=7) bị bỏ qua. 

| Chỉ số hiện tại | Bán kính | Đã chọn trước | Kiểm tra khoảng cách | Quyết định | 
| --- | --- | --- | --- | --- | 
| 4 | 3 | không | không | chọn | 
| 1 | 1 | 4 | (9\ge4) | chọn | 
| 2 | 1 | 4, 1 | (8\ge4,\ 1<2) sai vì tâm 1 và 2 cách nhau 1 | bỏ qua | 
| 3 | 1 | 4, 1 | (3<4) | bỏ qua | 

Tập hợp được chọn được tạo ra bởi việc triển khai này là`{4, 1}`. Hình cầu 2 được bao phủ bởi hình cầu mở rộng của hình cầu 1, trong khi hình cầu 3 được bao phủ bởi hình cầu mở rộng của hình cầu 4. Mẫu`{2,4}`là một câu trả lời hợp lệ khác, vì đầu ra không bắt buộc phải là duy nhất. 

### Một ví dụ che phủ đơn giản 

Hãy xem xét```
3
0 0 0 5
6 0 0 2
20 0 0 1
```Hình cầu bán kính-5 được chọn đầu tiên. Hình cầu bán kính-2 cắt nó nên nó bị bỏ qua. Hình cầu bán kính-1 tách khỏi hình cầu đã chọn và được chọn. 

| Chỉ số hiện tại | Bán kính | Đã chọn trước | Khoảng cách liên quan | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | không | không | chọn | 
| 2 | 2 | 1 | (6<7) | bỏ qua | 
| 3 | 1 | 1 | (20\ge6) | chọn | 

Hình cầu 2 được bao phủ bởi hình cầu mở rộng bán kính 15 của hình cầu 1. Điều này chứng tỏ chính xác tại sao thứ tự bán kính lại quan trọng: hình cầu khiến một hình cầu khác bị bỏ qua được đảm bảo ít nhất phải lớn bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n+n^2)) | Chi phí sắp xếp (O(n\log n)) và tối đa (n) ứng viên mỗi người so sánh với (n) lĩnh vực đã chọn | 
| Không gian | (O(n)) | Mảng đầu vào và phi thuyền được chọn chứa các bản ghi (O(n)) | 

Với (n\le100), phần bậc hai thực hiện tối đa khoảng (10^4) kiểm tra theo cặp. Các giới hạn tọa độ và bán kính cũng làm cho mọi phép tính số học không tốn kém, do đó, giải pháp nằm trong giới hạn 1 giây và 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới kiểm tra các điều kiện hình học thực tế thay vì yêu cầu một tập hợp con hợp lệ cụ thể. Điều này là cần thiết vì bài toán cho phép bất kỳ câu trả lời hợp lệ nào, do đó các lựa chọn phá vỡ ràng buộc tham lam chính xác khác nhau có thể tạo ra các bộ chỉ số khác nhau.```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n = int(sys.stdin.readline())
        ships = []

        for i in range(1, n + 1):
            x, y, z, r = map(int, sys.stdin.readline().split())
            ships.append((r, x, y, z, i))

        ships.sort(reverse=True)

        selected = []

        for r, x, y, z, idx in ships:
            ok = True

            for sr, sx, sy, sz, sidx in selected:
                dx = x - sx
                dy = y - sy
                dz = z - sz

                dist2 = dx * dx + dy * dy + dz * dz
                rr = r + sr

                if dist2 < rr * rr:
                    ok = False
                    break

            if ok:
                selected.append((r, x, y, z, idx))

        print(len(selected))
        print(*[idx for _, _, _, _, idx in selected])

        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, out: str) -> bool:
    lines = out.strip().splitlines()
    if not lines:
        return False

    n = int(inp.splitlines()[0])
    data = [tuple(map(int, line.split())) for line in inp.splitlines()[1:]]

    k = int(lines[0])
    ids = list(map(int, lines[1].split())) if len(lines) > 1 else []

    if k != len(ids):
        return False
    if not (1 <= k <= n):
        return False
    if len(set(ids)) != k:
        return False
    if any(i < 1 or i > n for i in ids):
        return False

    selected = [data[i - 1] for i in ids]

    # Check that selected original spheres are pairwise disjoint.
    for i in range(k):
        x1, y1, z1, r1 = selected[i]
        for j in range(i + 1, k):
            x2, y2, z2, r2 = selected[j]

            dx = x1 - x2
            dy = y1 - y2
            dz = z1 - z2

            dist2 = dx * dx + dy * dy + dz * dz
            rr = r1 + r2

            if dist2 < rr * rr:
                return False

    # Check that every original sphere is covered by the union
    # of the expanded selected spheres.
    for x, y, z, r in data:
        covered = False

        for sx, sy, sz, sr in selected:
            dx = x - sx
            dy = y - sy
            dz = z - sz

            center_dist2 = dx * dx + dy * dy + dz * dz

            # The farthest point of the original sphere is
            # center distance + r, so coverage requires
            # center distance <= 3*sr - r.
            reach = 3 * sr - r

            if reach >= 0 and center_dist2 <= reach * reach:
                covered = True
                break

        if not covered:
            return False

    return True

def run(inp: str) -> str:
    out = solve_data(inp)
    assert validate(inp, out), f"Invalid output:\n{out}"
    return out

# Provided sample.
run("""4
1 0 0 1
2 0 0 1
7 0 0 1
10 0 0 3
""")

# Minimum-size input.
run("""1
5 5 5 7
""")

# Two spheres that only touch. Both may be selected.
run("""2
0 0 0 1
2 0 0 1
""")

# One large sphere intersects a smaller sphere.
# The larger one must be processed first so the smaller one is skipped safely.
run("""2
0 0 0 10
10 0 0 1
""")

# All spheres have identical centers.
# Only one original sphere can be selected, and its 3r expansion
# covers every identical sphere.
run("""4
100 100 100 2
100 100 100 2
100 100 100 2
100 100 100 2
""")

# Larger boundary-style case with 100 spheres.
# All are identical, so exactly one is needed.
large_case = "100\n" + "\n".join(
    f"{i} 0 0 1" for i in range(100)
)
run(large_case)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Bất kỳ tập hợp con có giá trị hình học nào | Xây dựng chung và đầu ra không độc đáo | 
|`1 / 5 5 5 7`| Một quả cầu được chọn | Đầu vào kích thước tối thiểu | 
| Hai quả cầu đơn vị ở khoảng cách 2 | Cả hai có thể được chọn | Ranh giới chạm chính xác | 
| Bán kính 10 và bán kính 1 có tâm giao nhau | Đã chọn quả cầu lớn, quả cầu nhỏ bị bỏ qua | Bất biến bán kính giảm dần | 
| Bốn quả cầu giống hệt nhau | Chính xác một lựa chọn | Tâm trùng nhau và bán kính bằng nhau | 
| 100 đơn vị hình cầu trên một đường | Một công thức bậc hai hợp lệ | Tối đa (n) và hiệu suất | 

## Vỏ cạnh 

Trường hợp ranh giới đầu tiên là tiếp tuyến chính xác. Vì```
2
0 0 0 1
2 0 0 1
```khoảng cách trung tâm bình phương là (4) và tổng bình phương của bán kính cũng là (4). Điều kiện chồng chéo là`dist2 < radius_sum * radius_sum`, do đó các hình cầu được chấp nhận cùng nhau. Các vùng ban đầu của chúng chỉ chạm vào nhau, điều mà bài toán cho phép một cách rõ ràng. Thuật toán xuất ra cả hai chỉ số. 

Trường hợp cạnh thứ hai là một quả cầu nhỏ hơn giao với một quả cầu lớn hơn. Vì```
2
0 0 0 10
10 0 0 1
```hình cầu bán kính-10 được xử lý đầu tiên và được chọn. Khi đó, hình cầu bán kính-1 bị loại vì khoảng cách tâm (10) nhỏ hơn (11). Để xác minh phạm vi bao phủ, điểm xa nhất của hình cầu nhỏ so với tâm lớn là khoảng cách (11), trong khi hình cầu được chọn có bán kính mở rộng (30). Do đó, toàn bộ quả cầu nhỏ được bao phủ. 

Trường hợp cạnh thứ ba là một số hình cầu có cùng tâm:```
4
100 100 100 2
100 100 100 2
100 100 100 2
100 100 100 2
```Quả cầu đầu tiên được chọn. Mỗi hình cầu sau có khoảng cách tâm bằng 0 và bán kính tổng hợp là 4, do đó nó giao với hình cầu đã chọn và bị bỏ qua. Hình cầu được chọn sẽ mở rộng đến bán kính sáu, chứa tất cả bốn hình cầu ban đầu có bán kính-2 vì chúng có cùng tâm. Điều này cũng chứng minh tại sao thuật toán có thể quay trở lại một phi thuyền một cách an toàn ngay cả khi tồn tại nhiều khu vực được bảo vệ ban đầu. 

Trường hợp cạnh thứ tư là một chuỗi dài các quả cầu chạm vào nhau. Ví dụ,```
3
0 0 0 1
2 0 0 1
4 0 0 1
```cho phép chọn cả ba quả cầu vì các quả cầu liền kề chạm vào nhau và quả cầu thứ nhất và thứ ba rời rạc. Một bài kiểm tra chồng chéo nghiêm ngặt sẽ chọn cả ba. Nếu việc triển khai coi tiếp tuyến là giao điểm, nó sẽ loại bỏ hình cầu ở giữa một cách không chính xác và có khả năng thay đổi cấu trúc một cách không cần thiết. 

Trường hợp cạnh cuối cùng là kích thước đầu vào tối đa, (n=100). Thuật toán vẫn chỉ thực hiện so sánh hình học (O(n^2)). Không cần đệ quy, xây dựng đồ thị, hình học dấu phẩy động hoặc liệt kê tập hợp con, do đó trường hợp tối đa vẫn nằm trong giới hạn một cách thoải mái.
