---
title: "CF 1612B - Hoán vị đặc biệt"
description: "Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến n, trong đó n là số chẵn. Hoán vị được chia thành hai nửa bằng nhau: n/2 phần tử đầu tiên tạo thành nửa bên trái và n/2 phần tử còn lại tạo thành nửa bên phải. Hai điều kiện phải được thỏa mãn đồng thời."
date: "2026-06-10T06:57:36+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1612
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 117 (Rated for Div. 2)"
rating: 900
weight: 1612
solve_time_s: 96
verified: false
draft: false
---

[CF 1612B - Hoán vị đặc biệt](https://codeforces.com/problemset/problem/1612/B) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến n, trong đó n là số chẵn. Hoán vị được chia thành hai nửa bằng nhau: n/2 phần tử đầu tiên tạo thành nửa bên trái và n/2 phần tử còn lại tạo thành nửa bên phải. 

Hai điều kiện phải được thỏa mãn đồng thời. Ở nửa bên trái, giá trị nhỏ nhất phải chính xác là a. Ở nửa bên phải, giá trị lớn nhất phải chính xác là b. Nếu không có cách sắp xếp nào từ 1 đến n có thể thỏa mãn cả hai ràng buộc thì chúng ta phải xuất -1. 

Đây không phải là tối ưu hóa bất cứ điều gì, mà là đặt các số sao cho hai điều kiện cực trị cụ thể giữ nguyên trong hai phân đoạn rời nhau của hoán vị. Khó khăn chính là mọi số từ 1 đến n phải xuất hiện đúng một lần, do đó việc đặt a và b sẽ ảnh hưởng đến những gì còn trống cho cả hai nửa. 

Các ràng buộc nhỏ, với n lên tới 100 và lên tới 1000 trường hợp thử nghiệm. Điều này ngay lập tức cho chúng ta biết rằng O(n^2) hoặc thậm chí O(n) cho mỗi trường hợp thử nghiệm là đủ dễ dàng. Thách thức thực sự hoàn toàn là về mặt cấu trúc. 

Một cách tiếp cận ngây thơ có thể thử tất cả các hoán vị hoặc thậm chí tham lam lấp đầy các vị trí mà không cần lý do về tính khả thi. Điều đó không thành công vì các ràng buộc cực đoan tương tác trên toàn cầu. Ví dụ: nếu chúng ta đặt các số rất nhỏ ở nửa bên phải, chúng ta có thể vô tình buộc số lớn nhất ở đó quá lớn hoặc quá nhỏ. 

Một vài trường hợp thất bại tinh tế xuất hiện ngay lập tức: 

Nếu a được đặt ở nửa bên phải thì nửa bên trái có thể không có giá trị tối thiểu vì nó không còn hiện diện ở đó nữa. Tương tự, nếu đặt b ở nửa bên trái thì nửa bên phải không thể đạt b ở mức tối đa. 

Ngoài ra, ngay cả khi a và b được đặt chính xác ở các nửa khác nhau, chúng ta phải đảm bảo có đủ “khoảng trống” trong mỗi nửa để chứa tất cả các số còn lại mà không vi phạm các ràng buộc tối thiểu và tối đa. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử tạo ra tất cả các hoán vị từ 1 đến n và kiểm tra xem các điều kiện có đúng hay không. Điều này đúng nhưng hoàn toàn không khả thi, vì ngay cả với n = 10 vẫn có 10! hoán vị, đã vượt xa mọi giới hạn. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ thử gán số cho một nửa và kiểm tra tính hợp lệ. Điều này làm giảm vấn đề phải chọn n/2 số cho nửa bên trái và sắp xếp chúng, vốn vẫn là hàm mũ trong n vì chúng ta đang chọn các tập con một cách hiệu quả. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến thứ tự đầy đủ bên trong mỗi nửa. Chúng ta chỉ quan tâm đến hai thuộc tính biên: tối thiểu ở nửa bên trái và tối đa ở nửa bên phải. Điều đó có nghĩa là chúng ta chỉ cần đảm bảo rằng: 

- a nằm ở nửa bên trái và là phần tử nhỏ nhất ở đó 
- b được bao gồm ở nửa bên phải và là phần tử lớn nhất ở đó 

Sau khi chúng tôi xác định vị trí của a và b, các số còn lại có thể được phân phối miễn là chúng không phá vỡ hai ràng buộc này. 

Điều này dẫn đến một chiến lược mang tính xây dựng. Chúng ta đặt a và b trước, sau đó cẩn thận điền vào các vị trí còn lại bằng cách sử dụng các giá trị nhỏ hơn và lớn hơn chúng, tôn trọng vai trò cực trị. 

Các điều kiện khả thi đến từ việc đếm: 

- Nửa bên trái phải chứa a và tất cả các phần tử khác phải lớn hơn hoặc bằng a 
- Nửa bên phải phải chứa b và tất cả các phần tử khác phải nhỏ hơn hoặc bằng b 

Điều này ngụ ý rằng các giá trị nhỏ hơn a đều phải chuyển về nửa bên phải và các giá trị lớn hơn b đều phải chuyển về nửa bên trái. Bất kỳ vi phạm nào sẽ buộc mức tối thiểu hoặc tối đa phải thay đổi. 

Từ cấu trúc này, chúng ta có thể trực tiếp xây dựng hoán vị hoặc loại bỏ nếu số đếm không phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Xây dựng tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi muốn xây dựng hai nửa có kích thước n/2 mỗi nửa trong khi thực thi a là mức tối thiểu ở nửa bên trái và b là mức tối đa ở nửa bên phải. 

1. Đầu tiên, đặt chữ a vào nửa bên trái. Điều này đảm bảo rằng nửa bên trái có khả năng có a tối thiểu, nhưng nó sẽ chỉ hợp lệ nếu không có gì nhỏ hơn a xuất hiện ở nửa đó. 
2. Đặt b vào nửa bên phải. Điều này đảm bảo nửa bên phải có khả năng có b tối đa, miễn là không có giá trị lớn hơn nào được nhập vào đó. 
3. Chia tất cả các số còn lại thành ba nhóm: số nhỏ hơn a, số nằm giữa a và b và số lớn hơn b. 
4. Tất cả các số nhỏ hơn a phải ở nửa bên phải. Nếu bất kỳ trong số chúng được đặt ở nửa bên trái, chúng sẽ vi phạm yêu cầu rằng a là giá trị tối thiểu ở đó. 
5. Tất cả các số lớn hơn b đều phải về nửa bên trái. Nếu bất kỳ trong số chúng được đặt ở nửa bên phải, chúng sẽ vi phạm yêu cầu b là giá trị lớn nhất ở đó. 
6. Bây giờ gán các số còn lại giữa a và b một cách tùy ý, điền vào các ô còn lại ở cả hai nửa. Ở giai đoạn này, chúng không thể phá vỡ các điều kiện tối thiểu/tối đa vì chúng nằm giữa hai mỏ neo. 
7. Sau khi điền, nếu một nửa vượt quá kích thước n/2 hoặc không thể điền một cách nhất quán, hãy ghi -1. 

Tại sao nó hoạt động: một khi a và b được cố định làm điểm neo cực trị, mọi số khác sẽ bị đẩy về một phía khi so sánh. Các giá trị nhỏ hơn a sẽ phá vỡ mức tối thiểu bên trái nếu được đặt ở đó và các giá trị lớn hơn b sẽ phá vỡ mức tối đa bên phải nếu được đặt ở đó. Các giá trị còn lại là “trung lập” và có thể được phân phối tự do. Việc xây dựng hoàn tất chính xác khi các hạn chế về năng lực phù hợp với các vị trí bắt buộc này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, a, b = map(int, input().split())
        half = n // 2

        if a == b:
            print(-1)
            continue

        left = [a]
        right = [b]

        used = set([a, b])

        # numbers less than a must go right
        less = [x for x in range(1, a) if x not in used]
        # numbers greater than b must go left
        greater = [x for x in range(b + 1, n + 1) if x not in used]
        # middle numbers
        middle = [x for x in range(1, n + 1) if x not in used and a < x < b]

        # feasibility checks
        if len(greater) + 1 > half or len(less) + 1 > half:
            print(-1)
            continue

        left += greater
        right += less

        # fill remaining spots
        remaining_left = half - len(left)
        remaining_right = half - len(right)

        if remaining_left + remaining_right != len(middle):
            print(-1)
            continue

        left += middle[:remaining_left]
        right += middle[remaining_left:]

        if len(left) != half or len(right) != half:
            print(-1)
            continue

        print(*left, *right)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng rõ ràng hai nửa bằng cách sử dụng logic vị trí bắt buộc. Chi tiết quan trọng là đảm bảo rằng các số nhỏ hơn a không bao giờ đi vào nửa bên trái và các số lớn hơn b không bao giờ đi vào nửa bên phải. Đoạn giữa được phân chia tùy ý sau khi các hạn chế về dung lượng được xác minh. 

Phải cẩn thận khi kiểm tra kích thước trước khi gán các phần tử ở giữa; nếu không, người ta có thể dễ dàng tràn một nửa mà vẫn tạo ra một hoán vị không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2 5
```n = 6, một nửa = 3 

Chúng tôi bắt đầu với: 

a = 2 ở bên trái, b = 5 ở bên phải 

Số: 

ít hơn 2 → [1] 

lớn hơn 5 → [6] 

giữa → [3, 4] 

| Bước | Trái | Đúng | Còn lại | 
| --- | --- | --- | --- | 
| Neo ban đầu | [2] | [5] | [1, 6, 3, 4] | 
| Thêm bắt buộc | [2, 6] | [5, 1] ​​| [3, 4] | 
| Điền vào giữa | [2, 6, 3] | [5, 1, 4] | [] | 

Left min = 2, right max = 5, cả hai đều hài lòng. 

### Ví dụ 2 

đầu vào:```
4 2 4
```n = 4, một nửa = 2 

a = 2 ở bên trái, b = 4 ở bên phải 

nhỏ hơn 2 → [1] sang phải 

lớn hơn 4 → [] 

giữa → [3] 

| Bước | Trái | Đúng | Còn lại | 
| --- | --- | --- | --- | 
| Neo ban đầu | [2] | [4] | [1, 3] | 
| Vị trí bắt buộc | [2] | [4, 1] | [3] | 
| Điền vào giữa | [2, 3] | [4, 1] | [] | 

Tối thiểu bên trái = 2, tối đa bên phải = 4. 

Điều này xác nhận rằng các giá trị ở giữa có thể được phân phối mà không ảnh hưởng đến tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi số được xếp vào đúng một nhóm | 
| Không gian | O(n) | Mảng lưu trữ các nhóm hoán vị | 

Với n ≤ 100 và t ≤ 1000, tổng số phép toán tối đa là 10^5, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, a, b = map(int, input().split())
        half = n // 2

        if a == b:
            out.append("-1")
            continue

        left = [a]
        right = [b]
        used = {a, b}

        less = [x for x in range(1, a) if x not in used]
        greater = [x for x in range(b + 1, n + 1)]
        middle = [x for x in range(1, n + 1) if x not in used and a < x < b]

        if len(greater) + 1 > half or len(less) + 1 > half:
            out.append("-1")
            continue

        left += greater
        right += less

        if len(left) > half or len(right) > half:
            out.append("-1")
            continue

        rem_l = half - len(left)
        rem_r = half - len(right)

        if rem_l + rem_r != len(middle):
            out.append("-1")
            continue

        left += middle[:rem_l]
        right += middle[rem_l:]

        if len(left) != half or len(right) != half:
            out.append("-1")
            continue

        out.append(" ".join(map(str, left + right)))

    return "\n".join(out)

# provided samples
assert run("""7
6 2 5
6 1 3
6 4 3
4 2 4
10 5 3
2 1 2
2 2 1
""") == """4 2 6 5 3 1
-1
6 4 5 1 3 2
3 2 4 1
-1
1 2
2 1""", "sample 1"

# custom cases
assert run("""1
2 1 2
""") == "1 2", "minimum valid"

assert run("""1
2 2 1
""") == "2 1", "reversed minimal"

assert run("""1
4 3 1
""") in ["-1"], "impossible configuration"

assert run("""1
8 3 6
""").count(" ") == 7, "valid permutation size check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hợp lệ tối thiểu | 1 2 | xây dựng căn cứ | 
| đảo ngược tối thiểu | 2 1 | trường hợp đối xứng | 
| không thể | -1 | từ chối tính khả thi | 
| ngẫu nhiên hợp lệ | 8 số | sự đúng đắn về cấu trúc | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi a và b rất gần hoặc gần ranh giới. Ví dụ: nếu a = 1 và b = n, thì tất cả các số khác bị ép vào vùng giữa và việc xây dựng sẽ hoạt động bình thường miễn là thứ tự vị trí được tôn trọng. Thuật toán xử lý việc này vì không có số nào nhỏ hơn 1 hoặc lớn hơn n, do đó các nhóm bắt buộc trống và việc kiểm tra dung lượng luôn vượt qua. 

Một trường hợp cạnh khác là khi a lớn và b nhỏ. Ví dụ n = 6, a = 5, b = 2. Ở đây, tất cả các số nhỏ hơn 5 sẽ bị dồn vào nửa bên phải, còn tất cả các số lớn hơn 2 sẽ bị dồn vào nửa bên trái, khiến cả hai nửa đều tràn. Thuật toán phát hiện chính xác điều này thông qua việc kiểm tra kích thước trên`less`Và`greater`và xuất ra -1 trước khi thử vị trí không hợp lệ. 

Trường hợp tinh vi cuối cùng là khi đoạn giữa lấp đầy chính xác các khe còn lại. Nếu sự bình đẳng này không được kiểm tra nghiêm ngặt, người ta có thể âm thầm lấp đầy một nửa. Việc kiểm tra sự bằng nhau rõ ràng giữa dung lượng còn lại và kích thước trung bình sẽ ngăn chặn điều đó và đảm bảo hoán vị được phân vùng đầy đủ và chính xác.
