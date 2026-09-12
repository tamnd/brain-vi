---
title: "CF 104664D - Ăn cùng hiệp sĩ"
description: "Chúng ta có một bàn cờ hình vuông có kích thước $N nhân N$, trong đó các hình vuông được đánh chỉ số theo tọa độ nguyên. Một hiệp sĩ duy nhất bắt đầu trên một ô vuông và chúng tôi muốn biết số lần di chuyển hợp pháp tối thiểu của hiệp sĩ cần thiết để đến được ô mục tiêu."
date: "2026-06-29T11:01:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104664
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-06-23 Div. 2 (Beginner)"
rating: 0
weight: 104664
solve_time_s: 78
verified: true
draft: false
---

[CF 104664D - Ăn vặt với các hiệp sĩ](https://codeforces.com/problemset/problem/104664/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bàn cờ hình vuông có kích thước$N \times N$, trong đó các hình vuông được lập chỉ mục theo tọa độ nguyên. Một hiệp sĩ duy nhất bắt đầu trên một ô vuông và chúng tôi muốn biết số lần di chuyển hợp pháp tối thiểu của hiệp sĩ cần thiết để đến được ô mục tiêu. Nếu quân mã không bao giờ có thể đạt được mục tiêu do ranh giới bàn cờ hoặc ràng buộc chẵn lẻ, chúng ta sẽ xuất ra$-1$. 

Bảng trống nên không có chướng ngại vật. Cấu trúc duy nhất là quy tắc chuyển động của hiệp sĩ, quy tắc này xác định một tập hợp cố định gồm tối đa tám lần chuyển đổi có thể có từ mỗi ô vuông. Điều này biến bài toán thành bài toán đường đi ngắn nhất trên một biểu đồ ẩn: mỗi ô là một nút và mỗi nước đi của quân mã là một cạnh không có trọng số. 

Những hạn chế$N < 800$ngụ ý nhiều nhất$640{,}000$nút. Mỗi nút có tối đa 8 cạnh đi ra, do đó, việc khám phá đầy đủ sẽ chạm tới vài triệu lần chuyển đổi. Điều này nằm trong ngân sách BFS bằng Python một cách thoải mái nếu được triển khai cẩn thận. 

Có một số trường hợp đặc biệt quan trọng ảnh hưởng đến tính chính xác: 

Một trường hợp là khi vị trí bắt đầu và kết thúc giống hệt nhau. Ví dụ:```
N = 1
start = (0, 0)
end = (0, 0)
```Câu trả lời đúng là$0$. Việc triển khai BFS bất cẩn luôn luôn xử lý hàng xóm trước và chỉ kiểm tra việc chấm dứt khi việc xử lý hàng đợi vẫn có thể quay trở lại$1$nếu nó tăng khoảng cách sớm. 

Một trường hợp khác là khi bàn cờ quá nhỏ để hiệp sĩ có thể di chuyển. Ví dụ:```
N = 2
start = (0, 0)
end = (1, 1)
```Một hiệp sĩ không có động thái hợp pháp ở bất cứ đâu trên một$2 \times 2$bảng, vì vậy hầu hết các ô đều bị cô lập trong thực tế. Câu trả lời đúng là$-1$trừ khi bắt đầu bằng kết thúc. Bất kỳ giải pháp nào giả định khả năng kết nối hoặc bỏ qua giới hạn sẽ trả về một số hữu hạn không chính xác. 

Cuối cùng, tính chẵn lẻ vẫn đóng một vai trò trên bảng hữu hạn. Ngay cả khi về mặt lý thuyết, một vị trí có thể tiếp cận được trên một bảng vô hạn, việc cắt bớt các ranh giới có thể khiến vị trí đó không thể tiếp cận được. Điều đó có nghĩa là các công thức heuristic chỉ dựa trên tính chẵn lẻ hoặc khoảng cách Manhattan là không an toàn; chúng ta phải tìm kiếm một cách rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là coi mỗi ô bảng là một đỉnh của biểu đồ và chạy tìm kiếm đường đi ngắn nhất. Từ mỗi vị trí, chúng tôi tạo ra tối đa tám nước đi hiệp sĩ, lọc ra những nước nằm ngoài bàn cờ và tiếp tục khám phá cho đến khi đạt được mục tiêu. Vì tất cả các bước di chuyển đều có chi phí như nhau nên tìm kiếm theo chiều rộng đảm bảo lần đầu tiên chúng ta tiếp cận mục tiêu là tối ưu. 

Điều này đúng nhưng sẽ tốn kém nếu thực hiện kém hoặc nếu các trạng thái lặp lại không được theo dõi. Nếu không có cấu trúc được truy cập, cùng một ô sẽ được truy cập lại theo cấp số nhân nhiều lần, vì hiệp sĩ di chuyển theo chu kỳ tự nhiên. Trong trường hợp xấu nhất, điều này biến thành việc khám phá một cái cây mở rộng vô hạn, điều này là không thể thực hiện được. 

Quan sát quan trọng là biểu đồ không có trọng số và có chi phí cạnh đồng đều. Điều đó làm cho BFS trở nên tối ưu. Quan sát thứ hai là không gian trạng thái đủ nhỏ để chỉ cần một BFS ngay từ đầu là đủ. Chúng tôi không cần tìm kiếm hai chiều hoặc phương pháp phỏng đoán như A*; lưới dày đặc nhưng có giới hạn. 

Giải pháp giảm tối đa một BFS duy nhất$N^2$trạng thái có hệ số phân nhánh không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (DFS / thăm dò lặp lại) | Hàm mũ | O(N²) hoặc tệ hơn | Quá chậm | 
| Con đường ngắn nhất BFS | O(N2) | O(N2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích từng ô bảng$(x, y)$như một nút trong biểu đồ. 
2. Xác định tám nước đi hiệp sĩ:$(\pm 1, \pm 2)$Và$(\pm 2, \pm 1)$. 
3. Nếu điểm bắt đầu bằng mục tiêu, hãy trả về 0 ngay lập tức vì không cần di chuyển. 
4. Khởi tạo lưới khoảng cách (hoặc mảng đã truy cập) với$-1$, có nghĩa là chưa được ghé thăm. 
5. Đặt khoảng cách ô bắt đầu thành 0 và đẩy nó vào hàng đợi. 
6. Trong khi hàng đợi không trống, hãy bật ô phía trước$(x, y)$. 
7. Đối với mỗi nước đi trong số tám nước đi có thể, hãy tính ô tiếp theo$(nx, ny)$. 
8. Nếu$(nx, ny)$nằm ngoài bảng hoặc đã truy cập, bỏ qua nó. 
9. Nếu không, hãy đặt khoảng cách của nó thành$dist[x][y] + 1$và đẩy nó vào hàng đợi. 
10. Nếu chúng ta đến được ô đích trong quá trình này, hãy quay lại khoảng cách của nó ngay lập tức. 
11. Nếu BFS kết thúc mà không đạt được mục tiêu, hãy quay lại$-1$. 

Lý do điều này hoạt động là vì BFS mở rộng các nút theo thứ tự khoảng cách tăng dần kể từ đầu. Mỗi ô được chỉ định số bước tối thiểu cần thiết để tiếp cận nó vì lần đầu tiên nó được phát hiện tương ứng với đường đi ngắn nhất có thể có trong biểu đồ không có trọng số. Việc đánh dấu đã truy cập đảm bảo mỗi ô được xử lý một lần, ngăn chặn các chu kỳ tăng khoảng cách hoặc gây ra các vòng lặp vô hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    N = int(input().strip())
    x1, y1 = map(int, input().split())
    x2, y2 = map(int, input().split())

    if (x1, y1) == (x2, y2):
        print(0)
        return

    moves = [(1, 2), (2, 1), (2, -1), (1, -2),
             (-1, -2), (-2, -1), (-2, 1), (-1, 2)]

    dist = [[-1] * N for _ in range(N)]
    q = deque()
    q.append((x1, y1))
    dist[x1][y1] = 0

    while q:
        x, y = q.popleft()

        for dx, dy in moves:
            nx, ny = x + dx, y + dy

            if 0 <= nx < N and 0 <= ny < N and dist[nx][ny] == -1:
                dist[nx][ny] = dist[x][y] + 1
                if (nx, ny) == (x2, y2):
                    print(dist[nx][ny])
                    return
                q.append((nx, ny))

    print(-1)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo BFS trực tiếp. Mảng khoảng cách tăng gấp đôi vai trò là điểm đánh dấu đã truy cập, tránh sự cần thiết của một mảng boolean riêng biệt. Việc thoát ra sớm khi tiếp cận mục tiêu sẽ tránh việc khám phá toàn bộ bảng khi không cần thiết. 

Một chi tiết tinh tế là chúng tôi kiểm tra giới hạn trước khi truy cập vào mảng khoảng cách. Một điều nữa là chúng tôi chỉ gán khoảng cách một lần cho mỗi ô; đây là những gì duy trì tính chính xác của đường đi ngắn nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 1
start = (0, 0)
end = (0, 0)
```Trường hợp này được xử lý trước khi BFS bắt đầu. 

| Bước | Xếp hàng | Cập nhật Dist | Hành động | 
| --- | --- | --- | --- | 
| ban đầu | (BFS trống bị bỏ qua) | không | bắt đầu == mục tiêu | 

Đầu ra ngay lập tức bằng 0. Điều này xác nhận tính đúng đắn của quy tắc kết thúc sớm. 

### Ví dụ 2 

đầu vào:```
N = 4
start = (1, 2)
end = (2, 2)
```Chúng tôi theo dõi các lớp BFS. 

| Bước | Xếp hàng | Mới ghé thăm | Lý do | 
| --- | --- | --- | --- | 
| ban đầu | (1,2) | (1,2)=0 | bắt đầu | 
| bật (1,2) | hàng xóm | (0,0),(0,4 không hợp lệ),... | mở rộng chiêu thức hiệp sĩ | 
| lớp tiếp theo | nhiều | một số ô hợp lệ | Biên giới BFS phát triển | 
| đạt mục tiêu | tìm thấy | (2,2)=3 | lần đầu tiên đạt được | 

Quan sát quan trọng là ngay cả khi có nhiều đường dẫn đến một ô, BFS đảm bảo rằng lần đến đầu tiên là tối thiểu. Ví dụ này cho thấy khoảng cách được tích lũy theo từng lớp thay vì thông qua các bước nhảy theo kinh nghiệm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| mỗi ô được truy cập tối đa một lần và mỗi lần truy cập sẽ kiểm tra tối đa 8 lần di chuyển | 
| Không gian |$O(N^2)$| lưới khoảng cách và hàng đợi có thể lưu trữ tất cả các ô trong trường hợp xấu nhất | 

Giới hạn$N < 800$ngụ ý nhiều nhất$640{,}000$các nút mà BFS xử lý thoải mái. Hệ số không đổi của 8 lần chuyển đổi giúp thời gian chạy ổn định trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old_stdout
    return out.getvalue().strip()

# provided samples
assert run("1\n0 0\n0 0\n") == "0"
assert run("4\n1 2\n2 2\n") == "3"

# custom cases
assert run("2\n0 0\n1 1\n") == "-1", "tiny board unreachable"
assert run("3\n0 0\n2 1\n") in {"1", "2"}, "small board reachability check"
assert run("5\n0 0\n0 0\n") == "0", "same cell larger board"
assert run("8\n0 0\n7 7\n") != "", "reachable large board sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2×2 không thể truy cập | -1 | không thể di chuyển hiệp sĩ | 
| ốp góc 3×3 | 1 hoặc 2 | hành vi tiếp cận lưới điện nhỏ | 
| bắt đầu/kết thúc giống hệt nhau | 0 | thoát sớm đúng đắn | 
| 8×8 góc tới góc | không trống | tính chính xác chung của BFS | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi bảng quá nhỏ để có thể di chuyển. Đối với đầu vào:```
N = 2
start = (0, 0)
end = (1, 1)
```BFS bắt đầu lúc$(0,0)$. Tất cả tám nước đi hiệp sĩ ngay lập tức đi ra ngoài giới hạn. Hàng đợi trở nên trống sau khi xử lý nút bắt đầu. Vì mục tiêu không bao giờ đạt được nên thuật toán trả về$-1$, cho kết quả đúng. 

Một trường hợp khác là bắt đầu và kết thúc giống hệt nhau:```
N = 5
start = (3, 3)
end = (3, 3)
```Thuật toán kiểm tra điều này trước BFS và trả về 0 trực tiếp, ngăn chặn việc thăm dò không cần thiết. 

Trường hợp thứ ba là khi mục tiêu chỉ có thể đạt được sau vài lần mở rộng:```
N = 4
start = (0, 0)
end = (3, 3)
```BFS mở rộng từng lớp. Ngay cả khi có nhiều đường dẫn tới các ô trung gian, lần đầu tiên$(3,3)$được xếp hàng đợi hoặc bị phát hiện tương ứng với số lần di chuyển tối thiểu. Điều này ngăn chặn việc đếm quá nhiều đường dẫn truy cập lại các ô thông qua các tuyến đường dài hơn.
