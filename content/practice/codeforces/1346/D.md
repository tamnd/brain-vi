---
title: "CF 1346D - Xây dựng ngục tối"
description: "Chúng ta có một đồ thị liên thông vô hướng trong đó các đỉnh biểu thị các phòng và các cạnh biểu thị các đường hầm. Mỗi đường hầm đã có một giá trị cố định và chúng ta phải gán một giá trị cho mỗi phòng."
date: "2026-06-16T10:00:57+07:00"
tags: ["codeforces", "competitive-programming", "*special", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1346
codeforces_index: "D"
codeforces_contest_name: "Kotlin Heroes: Episode 4"
rating: 1600
weight: 1346
solve_time_s: 176
verified: true
draft: false
---

[CF 1346D - Xây dựng ngục tối](https://codeforces.com/problemset/problem/1346/D) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, đồ thị, tham lam 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị liên thông vô hướng trong đó các đỉnh biểu thị các phòng và các cạnh biểu thị các đường hầm. Mỗi đường hầm đã có một giá trị cố định và chúng ta phải gán một giá trị cho mỗi phòng. 

Quy tắc liên kết chặt chẽ các phòng và đường hầm với nhau: đối với mọi cạnh giữa hai phòng, giá trị cạnh phải bằng giá trị nhỏ hơn trong hai giá trị phòng. Khi các giá trị phòng được chọn, mọi cạnh sẽ tự động bị ép buộc, do đó quyền tự do duy nhất mà chúng ta có là gán các giá trị phòng. 

Mục tiêu không chỉ là thỏa mãn mọi ràng buộc mà còn là giảm thiểu tổng giá trị phòng. Nếu không có nhiệm vụ nào tồn tại, chúng tôi phải báo cáo là không thể thực hiện được. 

Các ràng buộc đẩy chúng ta tới một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số đỉnh và cạnh trong tất cả các thử nghiệm nhiều nhất là 200.000, do đó, bất kỳ phương pháp nào thậm chí là bậc hai cho mỗi trường hợp thử nghiệm sẽ thất bại. Ngay cả những việc như liên tục tính toán lại tính nhất quán bằng cách quét toàn bộ biểu đồ cũng quá chậm. Chúng tôi cần một cấu trúc về cơ bản bao gồm một hoặc hai biểu đồ cho mỗi bài kiểm tra. 

Một vấn đề tế nhị phát sinh từ các chu kỳ. Trên cây, các ràng buộc lan truyền rõ ràng từ lá vào trong. Trong biểu đồ tuần hoàn, các yêu cầu xung đột có thể xuất hiện: một phòng có thể bị buộc phải lớn hơn hoặc bằng hoàn toàn các phòng lân cận khác nhau theo những cách không nhất quán. Một phép gán tham lam ngây thơ không có sự lan truyền toàn cầu có thể dễ dàng chấp nhận các cấu hình không hợp lệ. 

Một trường hợp lỗi đơn giản là một tam giác trong đó các cạnh thực thi các giá trị cực tiểu không tương thích. Ví dụ: nếu các cạnh yêu cầu (1,2)=5, (2,3)=7, (1,3)=6 thì phòng 2 phải có ít nhất 7, phòng 1 ít nhất là 6, nhưng khi đó cạnh (1,2) sẽ yêu cầu min(a1,a2)=5, không thể giữ được. Chiến lược phân công cục bộ sẽ bỏ qua mâu thuẫn này trừ khi nó phổ biến các ràng buộc trên toàn cầu. 

Khó khăn cốt lõi là mỗi cạnh ràng buộc điểm cuối thấp hơn giữa hai phòng của nó và những ràng buộc này phải nhất quán trên toàn cầu trên tất cả các cách có thể mà một phòng có thể hoạt động như điểm cuối tối thiểu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi giá trị phòng là một số nguyên không xác định và cố gắng thực thi các ràng buộc lặp đi lặp lại. Chúng ta có thể bắt đầu với tất cả các phòng chưa được chỉ định, liên tục chọn một cạnh và cố gắng đặt cả hai điểm cuối sao cho mức tối thiểu khớp với trọng lượng cạnh được yêu cầu. Điều này nhanh chóng trở nên mơ hồ: việc chỉ định một điểm cuối sẽ ảnh hưởng đến tất cả các điểm cuối lân cận và việc xem lại các cạnh có thể buộc phải sửa đổi. 

Trong trường hợp xấu nhất, mỗi bản cập nhật có thể xếp tầng trên toàn bộ biểu đồ và chúng ta có thể phải xem lại các cạnh nhiều lần. Điều này hoạt động giống như sự lan truyền ràng buộc với khả năng thư giãn lặp đi lặp lại, tạo ra trường hợp xấu nhất theo cấp số nhân hoặc ít nhất là bậc hai do các giá trị thắt chặt liên tục theo chu kỳ. 

Quan sát quan trọng là mỗi phòng ít nhất phải lớn bằng mọi trọng số của cạnh tới, bởi vì nếu một phòng tham gia vào một cạnh có trọng số w và đó là điểm cuối nhỏ hơn thì phòng đó phải bằng w; nếu đó là điểm cuối lớn hơn thì điểm cuối còn lại bằng w, buộc điểm cuối lớn hơn ít nhất phải bằng w. Vì vậy, mỗi phòng đều có giới hạn dưới tự nhiên: trọng lượng cạnh tối đa liên quan đến nó. 

Đặt giới hạn dưới này là L[i] = trọng lượng tối đa của các cạnh chạm vào i. Giải pháp tối ưu sẽ không bao giờ chọn giá trị lớn hơn mức cần thiết, vì vậy chúng tôi bắt đầu bằng cách đặt a[i] = L[i]. 

Bây giờ chúng ta kiểm tra tính khả thi: với mọi cạnh (u, v, w), điều kiện min(a[u], a[v]) = w phải đúng. Vì cả hai điểm cuối ít nhất đều đã đạt cực đại sự cố nên điều này giảm xuống còn việc xác minh rằng ít nhất một điểm cuối chính xác là w và điểm cuối kia ít nhất là w. Nếu cả hai điểm cuối đều lớn hơn w thì cạnh đó không thể đạt được mức tối thiểu cần thiết, do đó cấu hình không hợp lệ.

Do đó, việc xây dựng sẽ trở thành: gán cho mỗi nút trọng số cạnh tới tối đa của nó, sau đó xác minh tất cả các cạnh. Nếu tất cả các cạnh đều thỏa mãn điều kiện tối thiểu thì phép gán này là tối ưu; nếu không thì không có giải pháp nào tồn tại. 

Điều này có tác dụng vì việc giảm bất kỳ nút nào xuống dưới trọng số cạnh tới tối đa của nó sẽ ngay lập tức phá vỡ cạnh đó và việc tăng bất kỳ nút nào vượt quá mức tối đa của nó chỉ làm tăng tổng tổng một cách không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tuyên truyền hạn chế vũ phu | O(nm) trường hợp xấu nhất | O(n + m) | Quá chậm / Không ổn định | 
| Phân bổ tối đa tối ưu + Xác minh | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính trọng lượng tối đa của tất cả các đường hầm đi tới mỗi phòng. Giá trị này thể hiện phép gán khả thi nhỏ nhất mà vẫn có thể đáp ứng bất kỳ cạnh nào mà phòng đóng vai trò là điểm cuối tối thiểu. Bất kỳ giá trị nhỏ hơn nào cũng sẽ ngay lập tức vi phạm ít nhất một ràng buộc. 
2. Chỉ định trọng số cạnh sự cố tối đa được tính toán của mỗi phòng. Tại thời điểm này, mỗi phòng được đặt ở giá trị nhỏ nhất không mâu thuẫn ngay lập tức với các yêu cầu về biên cục bộ. 
3. Kiểm tra mọi đường hầm (u, v, w) và xác minh rằng điều kiện min(a[u], a[v]) bằng w đúng. Bước này đảm bảo rằng đối với mỗi cạnh, ít nhất một điểm cuối chính xác là w và điểm cuối kia không nằm dưới w. 
4. Nếu bất kỳ cạnh nào không đạt điều kiện này, xuất ra NO vì không có sự điều chỉnh nào có thể khắc phục được nó mà không phá vỡ ràng buộc đã được thỏa mãn trước đó. 
5. Nếu không thì xuất CÓ theo sau là tất cả các giá trị phòng được chỉ định. 

### Tại sao nó hoạt động 

Mỗi giá trị phòng buộc phải có ít nhất trọng số cạnh lớn nhất gắn với nó, nếu không thì cạnh đó không thể đạt được mức tối thiểu cần thiết. Đặt a[i] chính xác vào giới hạn này sẽ giảm thiểu tổng. 

Nếu một cạnh (u, v, w) không thể thỏa mãn min(a[u], a[v]) = w theo phép gán này thì cả hai điểm cuối đều vượt quá w, nghĩa là không điểm cuối nào được phép giảm xuống w mà không vi phạm ràng buộc sự cố khác. Điều đó ngụ ý một mâu thuẫn toàn cầu: mọi nhiệm vụ ứng cử viên sẽ yêu cầu hạ thấp ít nhất một điểm cuối xuống dưới mức tối đa sự cố của chính nó, điều này là không thể theo định nghĩa của công trình. Điều này làm cho việc kiểm tra vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        adj_max = [0] * n
        edges = []

        for _ in range(m):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1
            edges.append((u, v, w))
            if w > adj_max[u]:
                adj_max[u] = w
            if w > adj_max[v]:
                adj_max[v] = w

        a = adj_max[:]
        ok = True

        for u, v, w in edges:
            if min(a[u], a[v]) != w:
                ok = False
                break

        if not ok:
            print("NO")
        else:
            print("YES")
            print(*a)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng giá trị yêu cầu tối đa cho mỗi nút trong khi đọc các cạnh, do đó không cần chuyển thêm danh sách kề. Bước chuyển nhượng chỉ là sao chép các cực đại này. 

Vòng xác nhận là cần thiết vì việc xây dựng không được tự động đảm bảo đáp ứng điều kiện tối thiểu trên mọi cạnh; nó chỉ đảm bảo rằng không có nút nào vi phạm ràng buộc cục bộ mạnh nhất của nó. Kiểm tra cuối cùng đảm bảo tính nhất quán giữa các cạnh có thể có cực đại không đối xứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 1
2 3 1
```Chúng tôi tính toán trọng số sự cố tối đa: 

phòng 1 → 1, phòng 2 → 1, phòng 3 → 1 

Bài tập: 

a = [1, 1, 1] 

| Bước | Cạnh | một [u] | một [v] | phút(a[u], a[v]) | w | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | (1,2) | 1 | 1 | 1 | 1 | vâng | 
| 2 | (2,3) | 1 | 1 | 1 | 1 | vâng | 

Tất cả các cạnh đều khớp nhau, vì vậy đầu ra là CÓ. 

Điều này cho thấy trường hợp tất cả các ràng buộc đều căn chỉnh và phép gán tối đa cho mỗi nút đã đáp ứng chính xác mọi đẳng thức. 

### Ví dụ 2 

đầu vào:```
4 4
1 2 5
3 2 2
4 1 3
3 4 4
```Tính cực đại: 

phòng 1 → 5, phòng 2 → 5, phòng 3 → 4, phòng 4 → 4 

Bài tập: 

a = [5, 5, 4, 4] 

| Bước | Cạnh | một [u] | một [v] | phút(a[u], a[v]) | w | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | (1,2) | 5 | 5 | 5 | 5 | vâng | 
| 2 | (3,2) | 4 | 5 | 4 | 2 | không | 

Edge (3,2) không thành công vì min là 4 nhưng yêu cầu là 2. 

Điều này chứng tỏ rằng ngay cả khi cực đại cục bộ nhất quán, cấu trúc toàn cục vẫn có thể buộc một cấu hình không thể thực hiện được khi trọng số cạnh nhỏ hơn hoàn toàn so với giới hạn dưới được thực thi của cả hai điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Một lượt để tính cực đại và một lượt để xác minh các cạnh | 
| Không gian | O(n + m) | Lưu trữ danh sách cạnh và giá trị nút | 

Tổng kích thước của tất cả các trường hợp thử nghiệm được giới hạn ở mức 200.000, do đó chiến lược truyền tải tuyến tính dễ dàng phù hợp với giới hạn thời gian. Thuật toán chỉ thực hiện công việc không đổi trên mỗi cạnh và trên mỗi nút. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        adj_max = [0] * n
        edges = []
        for _ in range(m):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1
            edges.append((u, v, w))
            adj_max[u] = max(adj_max[u], w)
            adj_max[v] = max(adj_max[v], w)

        a = adj_max[:]
        ok = True
        for u, v, w in edges:
            if min(a[u], a[v]) != w:
                ok = False
                break

        if not ok:
            out.append("NO")
        else:
            out.append("YES")
            out.append(" ".join(map(str, a)))

    return "\n".join(out)

# provided sample 1
assert run("""3
3 2
1 2 1
2 3 1
5 7
3 2 7
3 4 9
1 5 5
1 2 5
4 1 5
4 2 7
3 1 5
4 4
1 2 5
3 2 2
4 1 3
3 4 4
""") == """YES
1 1 1
YES
5 7 9 9 5
NO"""

# custom 1: single edge
assert run("""1
2 1
1 2 7
""").startswith("YES")

# custom 2: impossible triangle
assert run("""1
3 3
1 2 5
2 3 6
1 3 4
""").endswith("NO")

# custom 3: star graph
assert run("""1
4 3
1 2 2
1 3 3
1 4 4
""").startswith("YES")

# custom 4: all equal
assert run("""1
3 3
1 2 1
2 3 1
1 3 1
""").startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | CÓ ... | tính khả thi cơ bản | 
| xung đột tam giác | KHÔNG | phát hiện sự không nhất quán toàn cầu | 
| đồ thị sao | CÓ | ràng buộc độc lập trên mỗi nút | 
| tất cả đều bình đẳng | CÓ | tính nhất quán của cạnh đồng nhất | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một nút có nhiều cạnh tới với các trọng số khác nhau. Việc xây dựng đặt nút ở mức tối đa, điều này có thể vô hiệu hóa ngay lập tức các cạnh nhỏ hơn. Ví dụ: nếu một nút kết nối với trọng số 2 và 10, nó sẽ trở thành 10 và cạnh có trọng số 2 chỉ có thể giữ nguyên hiệu lực nếu điểm cuối khác của nó chính xác là 2. 

Một trường hợp cạnh khác là khi đồ thị là một cây. Trong trường hợp đó, việc xây dựng luôn thành công vì mỗi cạnh luôn có thể gán điểm cuối nhỏ hơn một cách chính xác mà không gây ra xung đột theo chu kỳ. Thuật toán xử lý việc này một cách tự nhiên vì bước xác minh sẽ không bao giờ thất bại khi tồn tại một phép gán cây nhất quán. 

Trường hợp cuối cùng là khi tất cả các cạnh có trọng số giống nhau. Mọi nút đều nhận được cùng một giá trị và mọi cạnh đều thỏa mãn điều kiện tối thiểu một cách tầm thường.
