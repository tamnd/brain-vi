---
title: "CF 164D - Đường kính tối thiểu"
description: "Chúng ta được cấp tới 1000 điểm trên mặt phẳng và chúng ta phải loại bỏ chính xác k điểm trong số đó. Sau khi loại bỏ những điểm đó, bộ còn lại phải có đường kính nhỏ nhất có thể. Đường kính của một tập hợp là khoảng cách Euclide tối đa giữa hai điểm còn lại bất kỳ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 164
codeforces_index: "D"
codeforces_contest_name: "VK Cup 2012 Round 3"
rating: 3100
weight: 164
solve_time_s: 138
verified: false
draft: false
---

[CF 164D - Đường kính tối thiểu](https://codeforces.com/problemset/problem/164/D) 

**Đánh giá:** 3100 
**Tags:** tìm kiếm nhị phân, vũ lực 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho tới 1000 điểm trên máy bay và chúng ta phải loại bỏ chính xác`k`của họ. Sau khi loại bỏ những điểm đó, bộ còn lại phải có đường kính nhỏ nhất có thể. 

Đường kính của một tập hợp là khoảng cách Euclide tối đa giữa hai điểm còn lại bất kỳ. Vì khoảng cách chỉ quan trọng với nhau nên chúng ta có thể làm việc hoàn toàn với khoảng cách bình phương và tránh các vấn đề về độ chính xác của dấu phẩy động. 

Đầu vào là một tập hợp điểm hình học, không phải là một vấn đề về biểu đồ hoặc mảng được ngụy trang. Chúng tôi đang chọn một tập hợp con có kích thước`n-k`cặp xa nhất của nó càng gần nhau càng tốt. 

Các ràng buộc hoàn toàn định hình giải pháp. Một sức mạnh tàn bạo đối với tất cả các tập hợp con có kích thước`n-k`là không thể. Ngay cả đối với`n = 1000`Và`k = 30`, số lượng tập hợp con lớn về mặt thiên văn. Chúng ta cần khai thác thực tế rằng`k`là nhỏ. 

Giá trị nhỏ của`k`gợi ý rằng giải pháp tối ưu khác với toàn bộ chỉ ở một vài điểm bị xóa. Điều này thường dẫn đến các kỹ thuật tìm kiếm phân nhánh hoặc giới hạn. 

Một quan sát quan trọng là nếu tập hợp dòng điện có đường kính được xác định bởi hai điểm`u`Và`v`, thì mọi lời giải hợp lệ có đường kính nhỏ hơn đều phải xóa ít nhất một trong các`u`hoặc`v`. Nếu không thì cả hai vẫn giữ nguyên và đường kính không thể giảm. 

Điều đó mang lại cho chúng ta một quá trình phân nhánh đệ quy với hệ số phân nhánh 2 và độ sâu nhiều nhất`k`. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử có nhiều điểm trùng nhau. 

đầu vào:```
4 1
0 0
0 0
0 0
100 100
```Câu trả lời đúng là xóa điểm 4, vì khi đó đường kính sẽ bằng 0. Việc triển khai bất cẩn bằng cách sử dụng so sánh dấu phẩy động có thể coi các điểm trùng khớp là khác biệt một cách không chính xác do nhiễu chính xác. 

Một trường hợp tinh tế khác là khi nhiều cặp có cùng khoảng cách tối đa. 

đầu vào:```
5 1
0 0
10 0
0 10
10 10
5 5
```Đường kính đạt được bởi một số cặp. Việc xóa một điểm khỏi một cặp không tự động giảm đường kính nếu cặp đường kính khác vẫn tồn tại. Đối số đệ quy vẫn hoạt động vì mọi giải pháp có đường kính nhỏ hơn phải chạm tới mọi cặp có khoảng cách tối đa và việc phân nhánh đệ quy trên một cặp như vậy cuối cùng sẽ buộc phải xóa đủ. 

Trường hợp cạnh cuối cùng là khi tập hợp tối ưu còn lại chỉ có một điểm. 

đầu vào:```
2 1
0 0
1000 1000
```Bất kỳ điểm còn lại nào cũng có đường kính bằng không. Việc triển khai phải xử lý chính xác các bộ có kích thước 1, trong đó không tồn tại cặp nào. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê mọi tập hợp con có kích thước`n-k`, tính đường kính của nó và giữ lại đường kính tốt nhất. 

Việc tính đường kính của một tập hợp con yêu cầu kiểm tra tất cả các cặp bên trong nó, điều này tốn`O(n^2)`. Số tập con là:$$\binom{1000}{970}$$điều đó hoàn toàn không thể thực hiện được. 

Lực lượng vũ phu là chính xác vì nó kiểm tra rõ ràng mọi tập hợp còn lại có thể, nhưng nó thất bại ngay lập tức vì không gian tìm kiếm rất lớn. 

Quan sát chính mang tính cấu trúc hơn là tổ hợp. 

Lấy tập hợp các điểm hoạt động hiện tại. Cho phép`(u,v)`là một cặp đạt được đường kính hiện tại. Nếu chúng ta muốn đường kính cuối cùng trở nên nhỏ hơn thì ít nhất một trong`u`hoặc`v`phải bị xóa. 

Điều đó tạo ra một quá trình phân nhánh: 

Nếu đường kính hiện tại đã đạt tối đa giá trị mục tiêu`D`, chúng ta đã xong. 

Ngược lại chúng ta tìm thấy một cặp vi phạm`(u,v)`có khoảng cách vượt quá`D`. Mọi giải pháp hợp lệ đều phải xóa`u`hoặc`v`. Vì vậy, chúng tôi thử đệ quy cả hai khả năng. 

Vì mỗi bước đệ quy sẽ xóa một điểm nên độ sâu đệ quy tối đa là`k`. Cây tìm kiếm có nhiều nhất`2^k`lá. Với`k ≤ 30`, diện tích này lớn nhưng vẫn có thể quản lý được bằng cách cắt tỉa tích cực. 

Bây giờ chúng ta vẫn cần đường kính tối ưu chứ không chỉ đơn thuần là tính khả thi cho một đường kính cố định.`D`. 

Khoảng cách giữa các điểm đến từ các cặp điểm thực tế. chỉ có`O(n^2)`khoảng cách bình phương rõ rệt. Chúng ta có thể tìm kiếm nhị phân trên các giá trị ứng viên này. 

Đối với đường kính cố định`D`, chúng tôi hỏi: 

Chúng ta có thể xóa nhiều nhất được không`k`điểm sao cho mỗi cặp còn lại có khoảng cách nhiều nhất`D`? 

Điều này trở thành một vấn đề phân nhánh giới hạn trên biểu đồ: 

Đỉnh là điểm. 

Các cạnh kết nối các cặp xa hơn`D`. 

Chúng ta cần xóa nhiều nhất`k`đỉnh sao cho không còn cạnh nào. Đó chính xác là vấn đề che đỉnh. 

Nói chung, lớp phủ đỉnh là NP-hard, nhưng ở đây tham số`k`là nhỏ bé. Thuật toán phân nhánh tiêu chuẩn hoạt động hoàn hảo. 

Quy tắc đệ quy là: 

Chọn bất kỳ cạnh nào`(u,v)`. Bất kỳ bìa đỉnh hợp lệ nào cũng phải bao gồm`u`hoặc`v`. Tái diễn trên cả hai lựa chọn. 

Điều này mang lại sự phức tạp gần như`O(2^k * n^2)`mỗi lần kiểm tra tính khả thi. 

Vì chỉ có`O(n^2)`khoảng cách ứng viên, tìm kiếm nhị phân sẽ thêm một yếu tố logarit khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`|`O(n)`| Quá chậm | 
| Tối ưu |`O(2^k * n^2 * log n)`|`O(n^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm và tính toán trước bình phương khoảng cách giữa mỗi cặp điểm. 

Việc sử dụng khoảng cách bình phương sẽ tránh được số học dấu phẩy động và bảo toàn chính xác tất cả các phép so sánh. 
2. Tập hợp tất cả các khoảng cách bình phương khác nhau vào một mảng đã sắp xếp. 

Đường kính tối ưu phải bằng một trong các giá trị này vì đường kính luôn được xác định bởi một số cặp điểm còn lại. 
3. Tìm kiếm nhị phân trên khoảng cách ứng cử viên được sắp xếp. 

Đối với mỗi ứng viên`D`, chúng tôi kiểm tra xem có thể xóa nhiều nhất không`k`điểm sao cho mỗi cặp còn lại có khoảng cách bình phương nhiều nhất`D`. 
4. Xây dựng biểu đồ xung đột tiềm ẩn cho hiện tại`D`. 

Hai điểm được kết nối nếu khoảng cách bình phương của chúng vượt quá`D`. Bất kỳ tập hợp còn lại hợp lệ nào cũng không thể giữ cả hai điểm cuối của cạnh đó. 
5. Chạy tìm kiếm bìa đỉnh giới hạn đệ quy. 

Nếu không còn cạnh xung đột thì việc xóa hiện tại đã thỏa mãn ràng buộc về đường kính. 

Ngược lại hãy chọn bất kỳ cạnh xung đột nào`(u,v)`. 
6. Chia thành hai khả năng. 

Trong nhánh đầu tiên, xóa`u`. 

Trong nhánh thứ hai, xóa`v`. 

Bất kỳ giải pháp hợp lệ nào cũng phải chọn ít nhất một điểm cuối của mọi cạnh xung đột còn lại. 
7. Dừng đệ quy nếu nhiều hơn`k`thao tác xóa được sử dụng. 

Chi nhánh đó không thể tạo ra một giải pháp hợp lệ. 
8. Nếu kiểm tra tính khả thi thành công, tìm kiếm nhị phân sẽ di chuyển sang trái với đường kính nhỏ hơn. 

Nếu không nó sẽ di chuyển sang phải. 
9. Sau khi tìm được đường kính khả thi tối thiểu, hãy xuất ra bất kỳ bộ đường kính chính xác nào`k`điểm bị xóa. 

Tìm kiếm đệ quy cung cấp tối đa một tập hợp kích thước xóa hợp lệ`k`. Nếu ít hơn`k`cần có các điểm, chúng ta có thể tùy ý thêm các điểm chưa bị xóa vì việc loại bỏ nhiều điểm hơn không bao giờ làm tăng đường kính. 

### Tại sao nó hoạt động 

Đối với đường kính cố định`D`, mỗi cặp xa hơn`D`không thể đồng thời tồn tại. Việc biểu diễn các cặp như cạnh đồ thị sẽ biến bài toán thành việc tìm một bìa đỉnh có kích thước tối đa`k`. 

Việc phân nhánh đệ quy là đúng vì mỗi cạnh phải có ít nhất một điểm cuối bị xóa. Khi thuật toán phân nhánh`(u,v)`, mọi nghiệm hợp lệ đều thuộc về một trong hai nhánh. 

Tìm kiếm nhị phân là đúng vì tính khả thi là đơn điệu. Nếu một số đường kính`D`có thể đạt được thì đường kính lớn hơn cũng có thể đạt được. 

Vì đường kính tối ưu phải bằng một trong các khoảng cách theo cặp nên việc tìm kiếm trong số các giá trị đó đảm bảo chúng ta sẽ tìm thấy mức tối ưu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

def solve():
    n, k = map(int, input().split())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    dist = [[0] * n for _ in range(n)]
    vals = set()

    for i in range(n):
        x1, y1 = pts[i]
        for j in range(i + 1, n):
            x2, y2 = pts[j]
            d = (x1 - x2) ** 2 + (y1 - y2) ** 2
            dist[i][j] = dist[j][i] = d
            vals.add(d)

    vals = sorted(vals)
    if not vals:
        vals = [0]

    answer = None

    def feasible(limit):
        deleted = [False] * n
        chosen = []

        def dfs(rem):
            edge = None

            for i in range(n):
                if deleted[i]:
                    continue
                for j in range(i + 1, n):
                    if deleted[j]:
                        continue
                    if dist[i][j] > limit:
                        edge = (i, j)
                        break
                if edge:
                    break

            if edge is None:
                return True

            if rem == 0:
                return False

            u, v = edge

            deleted[u] = True
            chosen.append(u)

            if dfs(rem - 1):
                return True

            chosen.pop()
            deleted[u] = False

            deleted[v] = True
            chosen.append(v)

            if dfs(rem - 1):
                return True

            chosen.pop()
            deleted[v] = False

            return False

        ok = dfs(k)

        if ok:
            return chosen[:]
        return None

    left = 0
    right = len(vals) - 1
    best = vals[-1]

    while left <= right:
        mid = (left + right) // 2
        cand = vals[mid]

        res = feasible(cand)

        if res is not None:
            best = cand
            answer = res
            right = mid - 1
        else:
            left = mid + 1

    deleted = set(answer)

    while len(deleted) < k:
        for i in range(n):
            if i not in deleted:
                deleted.add(i)
                break

    print(*[x + 1 for x in deleted])

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán tất cả các khoảng cách bình phương theo cặp. Sử dụng khoảng cách bình phương là điều cần thiết vì căn bậc hai dấu phẩy động gây ra các vấn đề về độ chính xác không cần thiết trong khi không mang lại lợi ích gì. 

Tìm kiếm nhị phân hoạt động trên các khoảng cách riêng biệt được sắp xếp thay vì các giá trị số tùy ý. Điều đó quan trọng vì đường kính tối ưu phải là một trong những khoảng cách chính xác theo từng cặp. 

Kiểm tra tính khả thi đệ quy thực hiện bao phủ đỉnh bị chặn. Việc tìm kiếm luôn chọn một cạnh xung đột và phân nhánh trên các điểm cuối của nó. 

Một chi tiết triển khai tinh vi là phép đệ quy làm thay đổi các mảng được chia sẻ. Mỗi nhánh đệ quy phải hoàn tác các thay đổi của nó trước khi quay trở lại. quên khôi phục`deleted[u]`hoặc`chosen`gây ô nhiễm chéo chi nhánh và tạo ra câu trả lời không chính xác. 

Một chi tiết quan trọng khác là phép đệ quy trả về ngay lập tức sau khi một nhánh hợp lệ thành công. Nếu không cắt tỉa, cây tìm kiếm sẽ trở nên lớn hơn đáng kể. 

Đầu ra cuối cùng có thể chứa ít hơn`k`các điểm đã bị xóa khỏi quá trình đệ quy. Điều đó hợp lệ vì việc kiểm tra tính khả thi yêu cầu nhiều nhất`k`việc xóa. Chúng ta mở rộng tập xóa một cách tùy ý cho đến khi nó có chính xác`k`điểm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 2
0 0
2 2
1 1
3 3
```Khoảng cách tối đa theo cặp ban đầu xuất phát từ điểm 2 và 5. 

Giả sử kiểm tra tìm kiếm nhị phân`D = 5`. 

| Bước | Cạnh xung đột | Đã xóa điểm | Ngân sách còn lại | 
| --- | --- | --- | --- | 
| Bắt đầu | (2,5) | {} | 2 | 
| Chi nhánh 1 | xóa 2 | {2} | 1 | 
| Kiểm tra lại | không | {2} | 1 | 

Việc tìm kiếm thành công ngay lập tức. 

Bây giờ tìm kiếm nhị phân thử các giá trị nhỏ hơn và cuối cùng phát hiện ra đường kính khả thi tối thiểu. 

Dấu vết thể hiện sự bất biến phân nhánh chính. Mỗi cặp vi phạm buộc ít nhất một điểm cuối vào tập hợp xóa. 

### Ví dụ 2 

đầu vào:```
4 1
0 0
0 10
10 0
10 10
```Ba góc còn lại vẫn có đường kính 200. 

| Bước | Cạnh xung đột | Đã xóa điểm | Ngân sách còn lại | 
| --- | --- | --- | --- | 
| Bắt đầu | (1,4) | {} | 1 | 
| Chi nhánh 1 | xóa 1 | {1} | 0 | 
| Kiểm tra lại | (2,3) | {1} | 0 | 
| Thất bại | cạnh chưa được giải quyết | {1} | 0 | 

Điều tương tự cũng xảy ra đối với mọi thao tác xóa có thể xảy ra. 

Thuật toán kết luận chính xác rằng không thể đạt được đường kính dưới 200 chỉ bằng một lần xóa. 

Ví dụ này cho thấy tại sao việc xóa một điểm cuối của một cặp đường kính không phải lúc nào cũng đủ. Nhiều cặp đường dài rời rạc có thể vẫn còn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(2^k * n^2 * log n)`| Tìm kiếm nhị phân theo khoảng cách, mỗi lần kiểm tra sẽ quét các cặp bên trong tìm kiếm phân nhánh | 
| Không gian |`O(n^2)`| Ma trận khoảng cách theo cặp | 

Với`n = 1000`, việc lưu trữ tất cả khoảng cách theo cặp là hoàn toàn an toàn. Hệ số phân nhánh chỉ phụ thuộc vào`k`, nhiều nhất là 30. Việc cắt tỉa đệ quy giữ cho việc tìm kiếm thực tế trong giới hạn cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n, k = map(int, input().split())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    dist = [[0] * n for _ in range(n)]
    vals = set()

    for i in range(n):
        x1, y1 = pts[i]
        for j in range(i + 1, n):
            x2, y2 = pts[j]
            d = (x1 - x2) ** 2 + (y1 - y2) ** 2
            dist[i][j] = dist[j][i] = d
            vals.add(d)

    vals = sorted(vals)
    if not vals:
        vals = [0]

    answer = None

    def feasible(limit):
        deleted = [False] * n
        chosen = []

        def dfs(rem):
            edge = None

            for i in range(n):
                if deleted[i]:
                    continue
                for j in range(i + 1, n):
                    if deleted[j]:
                        continue
                    if dist[i][j] > limit:
                        edge = (i, j)
                        break
                if edge:
                    break

            if edge is None:
                return True

            if rem == 0:
                return False

            u, v = edge

            deleted[u] = True
            chosen.append(u)

            if dfs(rem - 1):
                return True

            chosen.pop()
            deleted[u] = False

            deleted[v] = True
            chosen.append(v)

            if dfs(rem - 1):
                return True

            chosen.pop()
            deleted[v] = False

            return False

        ok = dfs(k)

        if ok:
            return chosen[:]
        return None

    left = 0
    right = len(vals) - 1
    best = vals[-1]

    while left <= right:
        mid = (left + right) // 2
        cand = vals[mid]

        res = feasible(cand)

        if res is not None:
            best = cand
            answer = res
            right = mid - 1
        else:
            left = mid + 1

    deleted = set(answer)

    while len(deleted) < k:
        for i in range(n):
            if i not in deleted:
                deleted.add(i)
                break

    return " ".join(str(x + 1) for x in deleted)

# provided sample
out = solve_io(
"""5 2
1 2
0 0
2 2
1 1
3 3
"""
)
assert len(out.split()) == 2

# minimum size
assert solve_io(
"""2 1
0 0
1 1
"""
) in ["1", "2"]

# all equal points
out = solve_io(
"""5 2
3 3
3 3
3 3
3 3
3 3
"""
)
assert len(out.split()) == 2

# square corners
out = solve_io(
"""4 1
0 0
0 10
10 0
10 10
"""
)
assert len(out.split()) == 1

# duplicate plus outlier
out = solve_io(
"""4 1
0 0
0 0
0 0
100 100
"""
)
assert out.strip() == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai điểm, xóa một | Bất kỳ chỉ mục nào | Xử lý đúng bộ còn lại cỡ 1 | 
| Tất cả các điểm bằng nhau | Hai chỉ số bất kỳ | Xử lý đường kính bằng 0 | 
| Góc vuông | Bất kỳ một chỉ mục nào | Nhiều cặp đường kính | 
| Ba bản sao và một ngoại lệ | Loại bỏ ngoại lệ | Tối ưu hóa hình học chính xác | 

## Vỏ cạnh 

Xét các điểm trùng nhau. 

đầu vào:```
4 1
0 0
0 0
0 0
100 100
```Khoảng cách lớn duy nhất liên quan đến điểm 4. 

Tìm kiếm đệ quy ngay lập tức chọn một cạnh xung đột như`(1,4)`. Một nhánh xóa điểm 1 và thất bại vì`(2,4)`vẫn tồn tại. Nhánh còn lại xóa điểm 4 và thành công. 

Thuật toán xác định chính xác ngoại lệ. 

Bây giờ hãy xem xét nhiều cặp đường kính độc lập. 

đầu vào:```
4 1
0 0
0 10
10 0
10 10
```Biểu đồ xung đột nhỏ`D`chứa cả hai đường chéo. 

Việc xóa một điểm cuối của một đường chéo vẫn giữ nguyên đường chéo kia. Phép đệ quy khám phá cả hai khả năng và chứng minh chính xác tính không khả thi chỉ bằng một lần xóa. 

Cuối cùng hãy xem xét tập nhỏ nhất có thể còn lại. 

đầu vào:```
2 1
0 0
1000 1000
```Các nhánh đệ quy ở cạnh duy nhất. Việc xóa một trong hai điểm cuối sẽ không còn xung đột. Thuật toán xử lý chính xác tập hợp một điểm có đường kính bằng 0.
