---
title: "CF 102956J - Bản cập nhật bảo mật đã được đánh bóng"
description: "Chúng ta được cung cấp một mạng lưới các máy tính được kết nối bằng cáp vô hướng và chúng ta cần chọn một tập hợp con các máy tính để “kích hoạt” theo hai quy tắc đồng thời. Đầu tiên, không có hai máy tính được chọn nào được kết nối trực tiếp bằng cáp, vì vậy tập hợp được chọn phải độc lập về mặt đồ thị."
date: "2026-07-04T07:09:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102956
codeforces_index: "J"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, Belarusian SU Contest (XXI Open Cup, Grand Prix of Belarus)"
rating: 0
weight: 102956
solve_time_s: 53
verified: true
draft: false
---

[CF 102956J - Bản cập nhật bảo mật đã được đánh giá cao](https://codeforces.com/problemset/problem/102956/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mạng lưới các máy tính được kết nối bằng cáp vô hướng và chúng ta cần chọn một tập hợp con các máy tính để “kích hoạt” theo hai quy tắc đồng thời. Đầu tiên, không có hai máy tính được chọn nào được kết nối trực tiếp bằng cáp, vì vậy tập hợp được chọn phải độc lập về mặt đồ thị. Thứ hai, mỗi sợi cáp phải chạm vào ít nhất một điểm cuối đã chọn, do đó mọi cạnh phải được bao phủ bởi các đỉnh đã chọn. Trong số tất cả các bộ thỏa mãn cả hai ràng buộc, chúng ta muốn bộ có số lượng máy tính được chọn nhỏ nhất có thể và chúng ta phải xuất ra kích thước của nó. Nếu không có tập hợp như vậy tồn tại thì câu trả lời là không thể. 

Các ràng buộc lên tới ba trăm nghìn đỉnh và cạnh, điều này ngay lập tức loại trừ mọi thứ bậc hai hoặc thậm chí gần nhau. Bất kỳ giải pháp nào về cơ bản phải tuyến tính theo kích thước của biểu đồ, vì ngay cả một hệ số nhỏ như n log n cũng có thể chấp nhận được nhưng bất kỳ giải pháp nào liên tục khám phá các cạnh theo cách lồng nhau sẽ hết thời gian. 

Một vấn đề tế nhị ẩn giấu trong chính định nghĩa: không rõ ràng rằng một tập hợp như vậy luôn tồn tại hay khi nó tồn tại thì nó có cấu trúc đơn giản. Một cạm bẫy tiềm ẩn khác là giả định tập hợp có thể được chọn một cách tham lam mà không xem xét tính nhất quán toàn cục giữa các cạnh. 

Một vài trường hợp đặc biệt làm rõ cấu trúc: 

Nếu đồ thị có một hình tam giác, chẳng hạn như 1-2-3-1, thì bất kỳ tập độc lập nào cũng có thể chứa nhiều nhất một đỉnh, nhưng bất kỳ đỉnh nào cũng không bao phủ được tất cả các cạnh. Ví dụ: chọn đỉnh 1 để lộ cạnh 2-3. Đầu vào này phải trả về -1. 

Nếu đồ thị hoàn toàn không có cạnh nào thì mọi tập hợp đều độc lập và che phủ đỉnh một cách trống rỗng. Tuy nhiên, bài toán yêu cầu tập được chọn không trống nên câu trả lời đúng sẽ là 1 chứ không phải 0. 

Nếu đồ thị là một chuỗi đơn giản như 1-2-3-4 thì việc chọn các đỉnh {1, 3} hoặc {2, 4} đều thỏa mãn điều kiện và kích thước tối thiểu phụ thuộc vào cách chúng ta phân vùng đồ thị. 

Những ví dụ này đã gợi ý rằng cấu trúc này gắn chặt với tính chất lưỡng đảng. 

## Phương pháp tiếp cận 

Một nỗ lực brute-force sẽ thử tất cả các tập hợp con của đỉnh và kiểm tra cả hai điều kiện. Đối với mỗi tập hợp con, chúng tôi sẽ xác minh tính độc lập bằng cách kiểm tra tất cả các cạnh và cũng xác minh phạm vi bao phủ bằng cách quét lại tất cả các cạnh. Điều này dẫn đến khoảng O(2^n · m), điều này hoàn toàn không khả thi đối với n lên tới 300.000. 

Một quan sát có cấu trúc hơn đến từ việc viết lại hai điều kiện. Nếu một tập S độc lập thì không có cạnh nào có cả hai điểm cuối trong S. Nếu S cũng là một phủ đỉnh thì mọi cạnh đều có ít nhất một điểm cuối trong S. Việc kết hợp cả hai câu lệnh buộc mỗi cạnh phải có chính xác một điểm cuối trong S. Điều này có nghĩa là mọi cạnh phải nằm giữa S và phần bù V \ S của nó. 

Điều này ngay lập tức ngụ ý rằng đồ thị là lưỡng cực, với S tạo thành một lớp màu và V \ S tạo thành lớp màu kia. Ngược lại, trong bất kỳ đồ thị hai bên nào, mỗi cạnh của lưỡng phân là độc lập và cũng bao phủ tất cả các cạnh. Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem đồ thị có phải là lưỡng cực hay không và nếu có thì chọn cạnh nhỏ hơn trong mỗi thành phần liên thông. 

Mỗi thành phần được kết nối có thể được tô màu bằng hai màu. Trong một thành phần, chúng ta có thể chọn tất cả các đỉnh có màu 0 hoặc tất cả các đỉnh có màu 1. Để giảm thiểu tổng kích thước, chúng ta chọn lớp màu nhỏ hơn một cách độc lập cho mỗi thành phần. Điều phức tạp duy nhất còn lại là bài toán cấm một tập hợp trống, vì vậy nếu đồ thị không có cạnh nào, chúng ta vẫn phải trả về 1 thay vì 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · m) | O(n) | Quá chậm | 
| Màu lưỡng cực | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý riêng từng thành phần được kết nối bằng BFS hoặc DFS trong khi chỉ định hai màu.

1. Chúng tôi lặp lại tất cả các đỉnh. Nếu một đỉnh chưa được thăm, chúng ta sẽ bắt đầu BFS từ đỉnh đó. 
2. Trong BFS, chúng ta gán màu của đỉnh bắt đầu là 0 và truyền các màu xen kẽ dọc theo các cạnh. Nếu chúng ta nhìn thấy một hàng xóm đã được tô cùng màu, đồ thị không phải là lưỡng cực và chúng ta ngay lập tức trả về -1. Điều này là cần thiết vì xung đột như vậy có nghĩa là một số cạnh buộc cả hai điểm cuối vào cùng một phía, điều này phá vỡ cấu trúc cần thiết. 
3. Đối với mỗi thành phần được kết nối, chúng tôi duy trì số lượng đỉnh được tô màu 0 và bao nhiêu đỉnh được tô màu 1. 
4. Sau khi hoàn thành một thành phần, chúng ta thêm min(count0, count1) vào câu trả lời, vì chúng ta có thể tự do chọn mặt rẻ hơn làm tập hợp đã chọn cho thành phần đó. 
5. Sau khi xử lý tất cả các thành phần, nếu tổng đáp án bằng 0 thì thay bằng 1 vì bài toán yêu cầu tập hợp được chọn không trống. 

### Tại sao nó hoạt động 

Việc tô màu buộc các đỉnh liền kề luôn nằm trong các tập hợp đối diện. Điều này đảm bảo tính độc lập cho từng lớp màu. Thời điểm cả hai điểm cuối của một cạnh yêu cầu cùng một màu, đồ thị không thể được phân chia sao cho mọi cạnh đều có điểm cuối ở các cạnh đối diện, nghĩa là không tồn tại tập hợp hợp lệ. Sau khi tìm thấy một phân vùng hợp lệ, mỗi cạnh sẽ tự động có một điểm cuối trong mỗi lớp màu, do đó, việc chọn toàn bộ lớp màu sẽ đảm bảo thuộc tính che phủ đỉnh trong khi vẫn duy trì tính độc lập. Tối thiểu hóa từng thành phần là đủ vì các thành phần bị ngắt kết nối nên các quyết định không tương tác với nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    color = [-1] * n
    ans = 0

    for i in range(n):
        if color[i] != -1:
            continue

        q = deque([i])
        color[i] = 0
        cnt = [1, 0]

        while q:
            u = q.popleft()
            for v in g[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    cnt[color[v]] += 1
                    q.append(v)
                elif color[v] == color[u]:
                    print(-1)
                    return

        ans += min(cnt[0], cnt[1])

    if ans == 0:
        ans = 1

    print(ans)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ biểu đồ một cách hiệu quả để hỗ trợ truyền tải tuyến tính. BFS gán màu và đồng thời đếm kích thước của từng phân vùng trong một thành phần. Kiểm tra xung đột bên trong BFS là cổng chính xác vì nó phát hiện cấu trúc không lưỡng cực ngay lập tức. 

Việc điều chỉnh cuối cùng cho câu trả lời trống sẽ xử lý trường hợp suy biến trong đó không có cạnh nào, vì trong tình huống đó mọi đỉnh đều bị cô lập và tất cả các thành phần đóng góp bằng 0 ở mức tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 2
3 4
```Chúng ta có hai cạnh độc lập tạo thành hai thành phần. 

| Thành phần | Nút | Màu 0 | Màu 1 | Được chọn | 
| --- | --- | --- | --- | --- | 
| 1 | {1,2} | 1 | 1 | 1 | 
| 2 | {3,4} | 1 | 1 | 1 | 

Tổng số câu trả lời trở thành 2. 

Điều này chứng tỏ rằng mỗi thành phần được tối ưu hóa độc lập và cả hai phân vùng đều được cân bằng. 

### Ví dụ 2 

đầu vào:```
4 3
1 2
2 3
1 3
```Đây là một hình tam giác. 

Trong quá trình tô màu BFS, đỉnh 1 là 0, đỉnh 2 trở thành 1, đỉnh 3 trở thành 0, nhưng cạnh 1-3 buộc cả hai điểm cuối phải có màu 0 đồng thời yêu cầu các màu đối lập nhau, tạo ra xung đột. 

Thuật toán phát hiện điều này và trả về -1 ngay lập tức, phù hợp với thực tế là không có phân vùng nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được xử lý một lần trong quá trình truyền tải BFS | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng màu | 

Các ràng buộc cho phép lên tới 300.000 đỉnh và cạnh, đồng thời một đường truyền tuyến tính duy nhất vừa khít trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    color = [-1] * n
    ans = 0

    for i in range(n):
        if color[i] != -1:
            continue
        q = deque([i])
        color[i] = 0
        cnt = [1, 0]

        ok = True
        while q:
            u = q.popleft()
            for v in g[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    cnt[color[v]] += 1
                    q.append(v)
                elif color[v] == color[u]:
                    ok = False
        if not ok:
            return "-1"
        ans += min(cnt)

    if ans == 0:
        ans = 1

    return str(ans)

# provided sample-like cases
assert run("4 2\n1 2\n3 4\n") == "2"
assert run("4 3\n1 2\n2 3\n1 3\n") == "-1"

# custom cases
assert run("2 0\n") == "1"
assert run("3 2\n1 2\n2 3\n") == "1"
assert run("5 4\n1 2\n2 3\n3 4\n4 5\n") == "2"
assert run("6 3\n1 2\n3 4\n5 6\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nút, không có cạnh | 1 | Biểu đồ trống yêu cầu tập hợp không trống | 
| đồ thị đường dẫn | 1 | Xử lý lưỡng cực một thành phần | 
| chuỗi 5 | 2 | Phân chia tối ưu trên đường dẫn lưỡng cực | 
| ba cạnh rời nhau | 3 | Tập hợp thành phần độc lập | 

## Vỏ cạnh 

Đối với đồ thị cạnh trống, thuật toán tạo ra tất cả các thành phần dưới dạng các đỉnh cô lập, mỗi thành phần đóng góp 0 vào câu trả lời. Vì giá trị tích lũy trở thành 0, nên điều chỉnh cuối cùng buộc đầu ra về 1, phù hợp với yêu cầu rằng tập đã chọn không thể trống ngay cả khi không tồn tại cạnh nào. 

Đối với một chu trình lẻ đơn lẻ chẳng hạn như một hình tam giác, việc tô màu BFS chắc chắn sẽ gán hai đỉnh liền kề cùng một màu tại một số điểm trong đường truyền. Xung đột được phát hiện ngay lập tức khi truy cập vào cạnh cuối của chu trình và thuật toán trả về -1 mà không cần xử lý phần còn lại của biểu đồ. 

Đối với các biểu đồ lưỡng cực bị ngắt kết nối, mỗi thành phần được xử lý độc lập. Ngay cả khi một thành phần lớn và thành phần khác là một cạnh đơn, quyết định trong một thành phần không ảnh hưởng đến thành phần kia vì các cạnh không giao nhau giữa các thành phần, do đó việc tối thiểu hóa cục bộ là tối ưu toàn cục.
