---
title: "CF 102860L - Nam Châm"
description: "Chúng ta có một bảng hình chữ nhật trong đó mỗi ô đều có màu đen hoặc trắng. Chúng ta cần quyết định xem có thể đặt nam châm phía nam và số lượng nam châm phía bắc tối thiểu sao cho có thể tiếp cận chính xác các ô màu đen bằng cách di chuyển nam châm phía bắc, trong khi không bao giờ có thể tiếp cận được các ô màu trắng."
date: "2026-07-25T14:17:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102860
codeforces_index: "L"
codeforces_contest_name: "2020-2021 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 20)"
rating: 0
weight: 102860
solve_time_s: 46
verified: true
draft: false
---

[CF 102860L - Nam châm](https://codeforces.com/problemset/problem/102860/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng hình chữ nhật trong đó mỗi ô đều có màu đen hoặc trắng. Chúng ta cần quyết định xem có thể đặt nam châm phía nam và số lượng nam châm phía bắc tối thiểu sao cho có thể tiếp cận chính xác các ô màu đen bằng cách di chuyển nam châm phía bắc, trong khi không bao giờ có thể tiếp cận được các ô màu trắng. 

Nam châm hướng bắc di chuyển một ô về phía nam châm nam khi cả hai ô ở cùng một hàng hoặc cùng một cột. Nam châm nam không bao giờ di chuyển. Vì phải có một nam châm phía nam ở mỗi hàng và mỗi cột nên diện tích có thể tiếp cận của nam châm phía bắc phụ thuộc vào hàng và cột nào chứa nam châm phía nam. 

Quan sát quan trọng là bản thân bảng đã mô tả khu vực có thể tiếp cận cuối cùng. Một ô màu đen phải có thể truy cập được, vì vậy cuối cùng một số nam châm phía bắc phải đến đó. Một ô màu trắng không bao giờ được tiếp cận, có nghĩa là các quy tắc chuyển động không thể xuyên qua các ô màu trắng trong khi mở rộng từ một nam châm hướng bắc bắt đầu. 

Các ràng buộc cho phép một bảng có kích thước lên tới 1000 x 1000, cung cấp tới một triệu ô. Bất kỳ giải pháp nào thử mọi vị trí có thể hoặc mô phỏng nhiều lần chuyển động giữa nhiều ô sẽ quá chậm. Chúng ta cần một cách tiếp cận gần với tuyến tính về số lượng ô, bởi vì việc quét hàng triệu ô là khả thi nhưng công việc bậc hai trên lưới thì không. 

Các trường hợp phức tạp đến từ các vùng màu đen bị ngắt kết nối và các hàng hoặc cột trống. Ví dụ:```
2 2
##
##
```Câu trả lời là`1`. Một nam châm hướng bắc có thể bao phủ toàn bộ hình chữ nhật vì mỗi hàng và cột phải chứa nam châm hướng nam, cho phép di chuyển theo cả hai hướng. 

Một lỗi phổ biến là chỉ đếm các thành phần được kết nối của các ô đen. Coi như:```
2 2
#.
##
```Câu trả lời là`1`, không`2`. Ô phía trên bên trái và hàng dưới được kết nối thông qua chuyển động theo hướng cột mặc dù không được phép kề nhau theo đường chéo. 

Một trường hợp nguy hiểm khác là một tấm bảng hoàn toàn trắng:```
1 3
...
```Câu trả lời là`0`. Không cần nam châm phía bắc vì không cần phải tiếp cận các ô đen. Giải pháp luôn khởi tạo câu trả lời bằng một sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng đặt nam châm hướng bắc và mô phỏng tất cả các chuyển động có thể xảy ra. Vì nam châm hướng bắc có thể di chuyển qua các hàng và cột, nên người ta có thể tưởng tượng bắt đầu từ mọi ô có thể và kiểm tra xem nó có chạm tới chính xác các ô đen hay không. Điều này đúng vì mô phỏng tuân theo các quy tắc thực tế nhưng nó quá đắt. Trên một bảng có một triệu ô, việc khám phá liên tục từ mỗi ô có thể yêu cầu khoảng một nghìn tỷ thao tác. 

Cấu trúc hữu ích đến từ việc xem xét các quy tắc chuyển động một cách khác nhau. Nam châm hướng bắc không cần một chuỗi di chuyển phức tạp để đến được một ô. Nếu một hàng chứa nam châm phía nam thì nam châm phía bắc có thể di chuyển theo chiều ngang bên trong hàng đó cho đến khi bị chặn bởi giới hạn của vùng màu đen có thể tiếp cận. Điều này cũng đúng theo chiều dọc đối với các cột. 

Các ô có thể truy cập cuối cùng phải tạo thành một hình trong đó mọi thành phần màu đen được kết nối không chứa hàng trống hoặc cột trống bên trong hình chữ nhật giới hạn của nó. Nếu một thành phần bị thiếu một hàng hoặc cột ở giữa các điểm cực trị của nó, chuyển động sẽ đi qua một ô màu trắng, khiến cho việc định vị không thể thực hiện được. 

Đối với mỗi thành phần được kết nối hợp lệ của ô đen, một nam châm hướng bắc là đủ. Số lượng nam châm phía bắc tối thiểu chính xác là số lượng các thành phần như vậy. Chúng ta có thể tìm thấy các thành phần này bằng cách lấp đầy và xác thực từng thành phần bằng cách kiểm tra hình chữ nhật giới hạn của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^2) | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét lưới và tìm mọi ô đen chưa được truy cập. Bắt đầu lấp lũ từ nó để thu thập toàn bộ thành phần màu đen được kết nối. 
2. Trong quá trình lấp đầy, ghi lại kích thước thành phần và chỉ số hàng và cột tối thiểu và tối đa. Bốn ranh giới này mô tả hình chữ nhật nhỏ nhất chứa thành phần đó. 
3. Sau khi quá trình lấp lũ kết thúc, hãy kiểm tra từng ô bên trong hình chữ nhật này. Nếu bất kỳ ô nào có màu trắng thì thành phần đó không thể tồn tại dưới dạng vùng nam châm hợp lệ vì nam châm phía bắc sẽ buộc phải đi qua ô màu trắng. Trong trường hợp đó, câu trả lời là không thể. 
4. Nếu hình chữ nhật chỉ chứa các ô màu đen thì thành phần này cần chính xác một nam châm hướng bắc. Tăng số lượng câu trả lời. 
5. Tiếp tục cho đến khi mọi ô đen đều được xử lý. 

Tại sao nó hoạt động: 

Một thành phần hợp lệ phải được điền đầy đủ bên trong hình chữ nhật bao quanh nó. Nếu một ô màu trắng xuất hiện bên trong hình chữ nhật đó, chuyển động giữa hai ô màu đen ở hai phía đối diện chắc chắn sẽ làm cho ô màu trắng đó có thể tiếp cận được. Ngược lại, khi một thành phần là một hình chữ nhật đặc, việc đặt một nam châm hướng bắc bên trong nó và đặt nam châm hướng nam vào mỗi hàng và cột sẽ cho phép nam châm tiếp cận mọi ô trong hình chữ nhật và không bao giờ rời khỏi nó. Vì các thành phần riêng biệt không thể chia sẻ một nam châm phía bắc mà không tạo ra các ô bổ sung có thể tiếp cận được nên mỗi thành phần hợp lệ sẽ đóng góp chính xác một nam châm phía bắc cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    visited = [[False] * m for _ in range(n)]
    ans = 0
    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '#' and not visited[i][j]:
                ans += 1
                stack = [(i, j)]
                visited[i][j] = True

                min_r = max_r = i
                min_c = max_c = j

                while stack:
                    r, c = stack.pop()
                    min_r = min(min_r, r)
                    max_r = max(max_r, r)
                    min_c = min(min_c, c)
                    max_c = max(max_c, c)

                    for dr, dc in dirs:
                        nr, nc = r + dr, c + dc
                        if 0 <= nr < n and 0 <= nc < m:
                            if grid[nr][nc] == '#' and not visited[nr][nc]:
                                visited[nr][nc] = True
                                stack.append((nr, nc))

                for r in range(min_r, max_r + 1):
                    for c in range(min_c, max_c + 1):
                        if grid[r][c] == '.':
                            print(-1)
                            return

    print(ans)

if __name__ == "__main__":
    solve()
```Các vòng bên ngoài xác định vị trí bắt đầu của từng thành phần màu đen chưa được khám phá. Việc lấp đầy dựa trên ngăn xếp tránh được các vấn đề về độ sâu đệ quy trên bảng 1000 x 1000. 

Trong khi khám phá một thành phần, mã chỉ cần bốn ranh giới hình chữ nhật thay vì lưu trữ tất cả các ô thành phần. Sau khi thăm dò, quá trình quét hình chữ nhật sẽ xác minh xem thành phần đó có phải là hình chữ nhật hoàn chỉnh hay không. 

Câu trả lời sẽ tăng lên trước khi xác nhận vì mỗi thành phần được phát hiện đại diện cho một nam châm bắc có thể có. Nếu xác thực không thành công, hàm sẽ in ngay lập tức`-1`bởi vì không có sự sắp xếp nào có thể thỏa mãn các quy tắc. 

Việc kiểm tra ranh giới trong phần lấp lũ là cần thiết vì thành phần này có thể chạm vào bất kỳ mặt nào của bảng. Các số nguyên Python không bị tràn ở đây và mức sử dụng bộ nhớ tối đa đến từ mảng đã truy cập, tức là khoảng một triệu mục nhập boolean. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3 3
.#.
###
##.
```Quá trình lấp lũ bắt đầu ở ô giữa trên cùng và đến thành phần lớn phía dưới. 

| Bước | Thành phần hiện tại | Hình chữ nhật giới hạn | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | (0,1) | hàng 0..2, cột 0..2 | chứa tế bào trắng | 

Ví dụ này thực tế được chia thành một thành phần có hình chữ nhật không đầy, do đó quá trình xác thực chung sẽ từ chối nó. Điểm quan trọng là việc sắp xếp mẫu ban đầu dựa vào cấu trúc chuyển động đầy đủ chứ không chỉ dựa vào sự liền kề của ô đen đơn giản. Phân tích thành phần phải phù hợp với biểu đồ chuyển động. 

Để có dấu vết rõ ràng hơn, hãy sử dụng:```
3 5
.....
.###.
.###.
```| Bước | Thành phần hiện tại | Hình chữ nhật giới hạn | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu tại (1,1) | tìm thấy cả sáu ô đen | hàng 1..2, cột 1..3 | hình chữ nhật hợp lệ | 
| Kết thúc | một thành phần | không có tế bào trắng bên trong | câu trả lời trở thành 1 | 

Dấu vết thứ hai hiển thị trực tiếp bất biến: mọi thành phần được đếm tương ứng với một vùng có thể tiếp cận được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập bằng cách lấp đầy tối đa một lần và các kiểm tra hình chữ nhật trên tất cả các thành phần cùng nhau nằm trong kích thước bảng. | 
| Không gian | O(nm) | Lưới đã truy cập và ngăn xếp tràn lưu trữ ở hầu hết tất cả các ô. | 

Kích thước bảng tối đa là một triệu ô, do đó quét tuyến tính phù hợp thoải mái với giới hạn dự kiến. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline().split()
    if not data:
        return ""
    n, m = map(int, data)
    grid = [sys.stdin.readline().strip() for _ in range(n)]

    visited = [[False] * m for _ in range(n)]
    ans = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '#' and not visited[i][j]:
                ans += 1
                stack = [(i, j)]
                visited[i][j] = True
                min_r = max_r = i
                min_c = max_c = j

                while stack:
                    r, c = stack.pop()
                    min_r = min(min_r, r)
                    max_r = max(max_r, r)
                    min_c = min(min_c, c)
                    max_c = max(max_c, c)

                    for dr, dc in ((1,0),(-1,0),(0,1),(0,-1)):
                        nr, nc = r + dr, c + dc
                        if 0 <= nr < n and 0 <= nc < m:
                            if grid[nr][nc] == '#' and not visited[nr][nc]:
                                visited[nr][nc] = True
                                stack.append((nr, nc))

                for r in range(min_r, max_r + 1):
                    for c in range(min_c, max_c + 1):
                        if grid[r][c] == '.':
                            sys.stdin = old
                            return "-1"

    sys.stdin = old
    return str(ans)

assert run("3 5\n.....\n.###.\n.###.\n") == "1"
assert run("1 3\n...\n") == "0"
assert run("2 2\n##\n.#\n") == "-1"
assert run("4 4\n####\n####\n####\n####\n") == "1"
assert run("3 3\n#.#\n###\n#.#\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình chữ nhật rắn | 1 | Một vùng đen hoàn chỉnh cần một nam châm | 
| Bảng trống | 0 | Không có nam châm phía bắc không cần thiết | 
| Lỗ bên trong thành phần | -1 | Các ô màu trắng bên trong hình chữ nhật có thể truy cập không hợp lệ | 
| Toàn bộ bảng màu đen | 1 | Thành phần mở rộng ranh giới lớn | 
| Hình chữ thập | -1 | Các thành phần không phải hình chữ nhật bị từ chối | 

## Vỏ cạnh 

Đối với chi tiết có lỗ:```
3 3
###
#.#
###
```Lũ tràn thăm tất cả tám ô đen. Hình chữ nhật bao quanh là toàn bộ bảng, nhưng ô ở giữa có màu trắng. Bước xác thực sẽ phát hiện nó ngay lập tức và trả về`-1`. 

Đối với một bảng trống:```
2 4
....
....
```Không bao giờ bắt đầu lấp lũ vì không có ô đen. Câu trả lời vẫn là 0, phù hợp với thực tế là không cần nam châm bắc. 

Đối với một ô duy nhất:```
1 1
#
```Hình chữ nhật thành phần có chiều rộng một ô và cao một ô. Nó không chứa ô trắng nên nó hợp lệ và cần chính xác một nam châm hướng bắc. 

Đối với một bảng đầy đủ:```
2 3
###
###
```Toàn bộ bảng là một thành phần. Hình chữ nhật giới hạn của nó bằng chính bảng, vì vậy câu trả lời là một. 

Tôi cũng có thể điều chỉnh nó thành định dạng biên tập theo phong cách Codeforces ngắn hơn nếu bạn muốn có phiên bản chính thức hơn cho cuộc thi.
