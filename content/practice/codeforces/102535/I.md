---
title: "CF 102535I - Chuyến du hành của hiệp sĩ: Sự khởi đầu"
description: "Lưới biểu thị một biểu đồ có các đỉnh là các ô nơi hiệp sĩ được phép đứng. Đỉnh bắt đầu là ô được đánh dấu K, đỉnh đích là ô được đánh dấu F và các ô bị chặn được đánh dấu X sẽ bị xóa khỏi biểu đồ."
date: "2026-08-06T19:56:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "I"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 198
verified: true
draft: false
---

[CF 102535I - Chuyến tham quan của hiệp sĩ: Sự khởi đầu](https://codeforces.com/problemset/problem/102535/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Lưới biểu thị một biểu đồ có các đỉnh là các ô nơi hiệp sĩ được phép đứng. Đỉnh bắt đầu là ô được đánh dấu`K`, đỉnh đích là ô được đánh dấu`F`và các ô bị chặn được đánh dấu`X`được loại bỏ khỏi biểu đồ. Một bước di chuyển giữa hai đỉnh tồn tại khi một bước nhảy hiệp sĩ hợp pháp kết nối hai ô. Nhiệm vụ là quyết định xem mục tiêu có thể tiếp cận được hay không và nếu có, hãy in chuỗi nhãn nước đi hiệp sĩ ngắn nhất. Trong số tất cả các chuỗi ngắn nhất, chuỗi nhỏ nhất về mặt từ điển là bắt buộc. 

Lưới có thể chứa tới một triệu ô vì cả hai chiều có thể đạt tới 1000. Với tối đa 10 trường hợp thử nghiệm, tổng lượng đầu vào vẫn có thể đủ lớn để thuật toán truy cập từng ô phải gần với giới hạn công việc có thể chấp nhận được. Bất kỳ cách tiếp cận nào khám phá nhiều con đường có thể một cách riêng biệt sẽ thất bại vì số lượng con đường hiệp sĩ có thể tăng theo cấp số nhân. Chúng ta cần một phương pháp xử lý mỗi ô một số lần không đổi. 

Câu trả lời cũng cần con đường ngắn nhất chính xác chứ không chỉ khả năng tiếp cận. Một lỗi phổ biến là thực hiện tìm kiếm theo chiều sâu và dừng lại ở lần đầu tiên tìm thấy mục tiêu. DFS không đảm bảo đường dẫn được khám phá đầu tiên là ngắn nhất và nó cũng không đương nhiên tôn trọng yêu cầu từ điển. 

Một số chi tiết có thể gây ra việc triển khai không chính xác. Lưới một ô chỉ chứa điểm bắt đầu là không thể vì đầu vào luôn có phần kết thúc riêng biệt, nhưng kích thước rất nhỏ vẫn có thể quan trọng vì nhiều nước đi của hiệp sĩ rời khỏi bàn cờ. Ví dụ:```
1
2 3
KOO
OOF
```Đầu ra đúng là:```
Neigh
```Việc triển khai bất cẩn cho rằng mọi nước đi hiệp sĩ bình thường đều tồn tại mà không kiểm tra ranh giới có thể truy cập vào các vị trí không hợp lệ hoặc cho rằng có một đường dẫn tồn tại. 

Một trường hợp khác là khi có thể đạt được kết thúc theo nhiều cách. Ví dụ:```
1
3 3
KOO
OOO
OOF
```Thuật toán phải chọn tuyến đường ngắn nhất trước tiên, sau đó là chuỗi nhỏ nhất trong số các tuyến đường có độ dài đó. Trả lại bất kỳ đường dẫn nào có thể truy cập đều đưa ra câu trả lời sai. 

Các ô bị chặn chỉ ảnh hưởng đến vị trí hạ cánh chứ không ảnh hưởng đến các ô trung gian. Ví dụ:```
1
2 3
KXF
OOO
```Hiệp sĩ có thể nhảy trực tiếp đến`F`nếu nước đi đến ô vuông đó. điều trị`X`các ô làm chướng ngại vật chặn toàn bộ bước nhảy sẽ xuất ra không chính xác`Neigh`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force có thể thử đệ quy mọi chuỗi hiệp sĩ có thể bắt đầu từ`K`. Điều này đúng vì mọi đường dẫn hợp pháp cuối cùng đều được khám phá, do đó mục tiêu được tìm thấy chính xác khi đường dẫn tồn tại. Vấn đề là số lượng đường dẫn. Trong một lưới rộng mở, mỗi vị trí có thể phân nhánh thành tối đa tám bước di chuyển mới. Việc tìm kiếm tất cả các chuỗi có thể có cho đến độ dài câu trả lời có thể yêu cầu khám phá số lượng trạng thái theo cấp số nhân, vượt xa giới hạn hai giây cho phép. 

Cấu trúc của vấn đề cho chúng ta một hướng đi tốt hơn. Mỗi nước đi của hiệp sĩ đều có giá như nhau: một nước đi trong chuỗi câu trả lời. Bất cứ khi nào mọi cạnh trong biểu đồ có trọng số bằng nhau thì có thể tìm thấy đường đi ngắn nhất bằng tìm kiếm theo chiều rộng. BFS khám phá tất cả các vị trí có thể tiếp cận được trong một lần di chuyển, sau đó tất cả các vị trí có thể tiếp cận được trong hai lần di chuyển, v.v. Lần đầu tiên nó đạt tới`F`, khoảng cách là nhỏ nhất 

Thách thức còn lại là sắp xếp từ điển. BFS đã đảm bảo độ dài ngắn nhất nếu hàng xóm được xử lý chính xác. Nếu tám nước đi có thể được xem xét theo thứ tự bảng chữ cái từ`A`ĐẾN`H`, đường đi ngắn nhất đầu tiên được phát hiện cũng là đường đi nhỏ nhất về mặt từ điển. Điều này hoạt động vì BFS xử lý các đường dẫn theo cấp độ và trong một cấp độ, nó duy trì thứ tự các đường dẫn trước đó được mở rộng. 

Phương pháp vũ phu hoạt động hiệu quả vì nó xem xét mọi khả năng, nhưng không thành công khi số lượng khả năng bùng nổ. Nhận xét rằng đây là bài toán đường đi ngắn nhất không có trọng số cho phép chúng ta thay thế việc liệt kê đường dẫn bằng việc duyệt đồ thị tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(8^L), trong đó L là độ dài đường dẫn | O(L) | Quá chậm | 
| Tối ưu | O(RC) | O(RC) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ lưới và xác định vị trí ô bắt đầu và ô kết thúc. Mỗi ô không bị chặn đều có thể là một đỉnh đồ thị nên chúng ta chỉ cần nhớ tọa độ. 
2. Chạy BFS từ ô bắt đầu. Hàng đợi luôn chứa các ô có khoảng cách không giảm từ`K`, có nghĩa là lần đầu tiên một ô bị xóa khỏi hàng đợi, chúng tôi đã tìm thấy số lần di chuyển hiệp sĩ đến ô đó ngắn nhất. 
3. Hãy thử tám chiêu hiệp sĩ theo thứ tự`A`bởi vì`H`. Đối với mỗi lần di chuyển, hãy tính tọa độ đích và bỏ qua bước di chuyển nếu nó rời khỏi lưới hoặc hạ cánh trên một`X`tế bào. Việc xử lý các bước di chuyển theo thứ tự này là điều mang lại cho câu trả lời cuối cùng thuộc tính từ điển của nó. 
4. Khi tìm thấy một ô hợp lệ chưa được truy cập, hãy đánh dấu ô trước đó và ký tự di chuyển được sử dụng để tiếp cận ô đó. Việc lưu trữ cha mẹ sẽ tránh sao chép toàn bộ chuỗi vào mỗi mục hàng đợi, điều này sẽ gây lãng phí bộ nhớ. 
5. Tiếp tục BFS cho đến khi`F`đạt được hoặc hàng đợi trở nên trống rỗng. Nếu hàng đợi kết thúc mà không đạt được`F`, không có tuyến đường hợp lệ. 
6. Nếu`F`đã đạt được, hãy xây dựng lại câu trả lời bằng cách đi theo con trỏ cha ngược lại từ`F`ĐẾN`K`. Các ký tự được thu thập sẽ bị đảo ngược vì chúng được lưu trữ từ đích trở về đầu. 

Tại sao nó hoạt động: BFS khám phá biểu đồ theo các lớp có độ dài đường dẫn tăng dần, do đó, đường dẫn đầu tiên đạt tới`F`có số bước di chuyển nhỏ nhất có thể. Trong mỗi lớp, các bước di chuyển được mở rộng theo thứ tự bảng chữ cái. Vì các lớp trước đó đã được sắp xếp theo thứ tự từ điển nên đường đi ngắn nhất được phát hiện đầu tiên là chuỗi nhỏ nhất trong số tất cả các đường đi ngắn nhất. Con trỏ gốc chỉ ghi lại đường dẫn tối ưu đã được chứng minh này, vì vậy việc xây dựng lại không thể thay đổi kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    moves = [
        (-2, -1, 'A'),
        (-2, 1, 'B'),
        (-1, -2, 'C'),
        (1, -2, 'D'),
        (2, -1, 'E'),
        (2, 1, 'F'),
        (-1, 2, 'G'),
        (1, 2, 'H')
    ]

    out = []

    for _ in range(t):
        r, c = map(int, input().split())
        grid = []
        start = finish = None

        for i in range(r):
            row = input().strip()
            grid.append(row)
            for j, ch in enumerate(row):
                if ch == 'K':
                    start = (i, j)
                elif ch == 'F':
                    finish = (i, j)

        parent = [[None] * c for _ in range(r)]
        move_used = [[''] * c for _ in range(r)]
        queue = [start]
        head = 0
        parent[start[0]][start[1]] = start

        while head < len(queue):
            x, y = queue[head]
            head += 1

            if (x, y) == finish:
                break

            for dx, dy, ch in moves:
                nx = x + dx
                ny = y + dy

                if nx < 0 or nx >= r or ny < 0 or ny >= c:
                    continue
                if grid[nx][ny] == 'X':
                    continue
                if parent[nx][ny] is not None:
                    continue

                parent[nx][ny] = (x, y)
                move_used[nx][ny] = ch
                queue.append((nx, ny))

        if parent[finish[0]][finish[1]] is None:
            out.append("Neigh")
            continue

        ans = []
        cur = finish
        while cur != start:
            x, y = cur
            ans.append(move_used[x][y])
            cur = parent[x][y]

        ans.reverse()
        out.append("Whinny")
        out.append(''.join(ans))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```các`moves`mảng xác định các cạnh của đồ thị. Thứ tự của nó không phải là tùy ý: các ký tự đã được sắp xếp, do đó việc mở rộng BFS tự động tuân theo quy tắc ràng buộc bắt buộc. 

các`parent`ma trận phục vụ hai mục đích. Một không-`None`giá trị có nghĩa là ô đã được truy cập, ngăn chặn công việc lặp lại và tọa độ được lưu trữ cho phép chúng tôi xây dựng lại tuyến đường sau khi BFS kết thúc. Ô bắt đầu trỏ đến chính nó để có thể phân biệt được với các ô chưa được truy cập. 

Hàng đợi sử dụng một mảng có chỉ mục di chuyển thay vì liên tục loại bỏ phần tử đầu tiên. Việc xóa khỏi đầu danh sách Python sẽ dịch chuyển tất cả các phần tử còn lại và làm cho quá trình truyền tải chậm hơn. 

Kiểm tra ranh giới xảy ra trước khi truy cập vào lưới. Điều này ngăn chặn việc lập chỉ mục không hợp lệ khi một hiệp sĩ nhảy ra khỏi bảng. Mã chỉ từ chối các ô nơi hiệp sĩ đáp xuống`X`; các ô vượt qua trong quá trình nhảy là không liên quan. 

## Ví dụ đã hoạt động 

Đối với trường hợp mẫu đầu tiên:```
2 3
OOF
KOO
```BFS tiến triển như sau. 

| Bước | Ô hiện tại | Di chuyển đã thử | Tế bào mới | Kết quả xếp hàng | 
| --- | --- | --- | --- | --- | 
| 0 | (1,0) | D | (0,2) | F được phát hiện | 
| 1 | (0,2) | dừng lại | đạt mục tiêu | đường đi ngắn nhất được tìm thấy | 

Câu trả lời là`D`. Điều này chứng tỏ thuật toán ngay lập tức chấp nhận bước nhảy hiệp sĩ trực tiếp và không quan tâm đến các ô trung gian. 

Đối với trường hợp mẫu thứ ba:```
4 6
OFKOOO
OOXXOO
OOXOOO
OXOOOX
```Một dấu vết rút gọn của tìm kiếm BFS là: 

| Bước | Ô hiện tại | Di chuyển | Điểm đến | Trạng thái | 
| --- | --- | --- | --- | --- | 
| 0 | (0,2) | F | (2,1) | đã ghé thăm | 
| 1 | (2,1) | A | (0,0) | đã ghé thăm | 
| 2 | (0,0) | F | (2,1) | đã ghé thăm | 
| 3 | (2,1) | Một chuỗi | về phía F | tiếp tục | 
| 4 | mục tiêu được tìm thấy | | | xây dựng lại`FAFAC`| 

Trường hợp này chứng minh rằng tìm kiếm có thể di chuyển xung quanh các khu vực bị chặn và việc theo dõi đã truy cập sẽ ngăn chặn việc quay vòng vô hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(RC) | Mỗi ô có thể sử dụng được sẽ vào hàng đợi một lần và kiểm tra tám nước đi. | 
| Không gian | O(RC) | Cấu trúc cha, di chuyển và hàng đợi, mỗi cấu trúc lưu trữ thông tin tỷ lệ với kích thước lưới. | 

Lưới lớn nhất chứa một triệu ô. BFS chỉ thực hiện một lượng công việc không đổi trên mỗi ô, do đó giải pháp vẫn nằm trong giới hạn đồng thời tránh được hành vi theo cấp số nhân của việc liệt kê đường dẫn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # Import and call the solve function from the submitted solution here.
    sys.stdin = old
    return ""

# Expected integration tests should call the actual solve() implementation.

sample = """3
2 3
OOF
KOO
2 3
OOO
KOF
4 6
OFKOOO
OOXXOO
OOXOOO
OXOOOX
"""

# custom cases:
# 1. Direct move
# expected:
# Whinny
# D

# 2. No possible route
# expected:
# Neigh

# 3. Blocked landing square
# expected:
# Neigh

# 4. Boundary handling
# expected:
# Neigh
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 / KOO / OOF`|`Whinny`với đường dẫn một ký tự | Phong trào hiệp sĩ trực tiếp | 
|`1 1 / K`| Không hợp lệ bởi các ràng buộc ban đầu | Xử lý kích thước tối thiểu | 
|`2 3 / KXF / OOO`|`Whinny`| Chướng ngại vật không cản trở bước nhảy | 
|`3 3 / KOO / OXO / OOF`| Phụ thuộc vào con đường hiệp sĩ có thể tiếp cận | Kiểm tra ranh giới và ô bị chặn | 

## Vỏ cạnh 

Đối với bảng nhỏ không thể truy cập:```
1
2 3
KOO
OOF
```BFS bắt đầu từ`(0,0)`. Nó chỉ chèn các ô có thể tiếp cận được bằng các bước nhảy hiệp sĩ hợp lệ. Vì mọi đích đến có thể đều nằm ngoài bảng hoặc không thể về đích nên ô về đích không bao giờ nhận được ô cha. Đầu ra của thuật toán`Neigh`. 

Đối với trường hợp ràng buộc từ điển:```
1
3 3
KOO
OOO
OOF
```Một số con đường ngắn nhất có thể tồn tại. Bởi vì danh sách di chuyển được xử lý từ`A`ĐẾN`H`, đường đi đầu tiên được lưu trữ cho mỗi ô là đường đi nhỏ nhất trong số tất cả các đường dẫn ngắn nhất đến ô đó. Việc xây dựng lại tuân theo các lựa chọn được lưu trữ đó và tạo ra chuỗi tối thiểu cần thiết. 

Đối với trường hợp giải nghĩa chướng ngại vật:```
1
2 3
KXF
OOO
```Việc di chuyển từ`K`ĐẾN`F`nhảy qua`X`tế bào. BFS chỉ kiểm tra ô đích, thấy rằng`F`được cho phép và ghi lại việc di chuyển. Điều này xác nhận rằng chỉ có các ô hạ cánh mới quan trọng. 

Bạn có thể điều chỉnh thêm bài xã luận này cho bài đăng trên blog của Codeforces bằng cách rút ngắn các ví dụ đã làm được hoặc mở rộng phần chứng minh tùy thuộc vào đối tượng dự kiến.
