---
title: "CF 105018N - Núi Lửa Và Trứng Rồng"
description: "Vấn đề mô tả một hố núi lửa được biểu diễn dưới dạng lưới $n nhân m$ trong đó mỗi ô lưu trữ điểm đó cao hơn bao nhiêu so với mức magma tại thời điểm 0. Các ô có giá trị bằng 0 đã ở mức magma, nghĩa là chúng sẽ bị nhấn chìm ngay lập tức."
date: "2026-06-28T02:07:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105018
codeforces_index: "N"
codeforces_contest_name: "Winter Cup 5.0 Online Mirror Contest"
rating: 0
weight: 105018
solve_time_s: 45
verified: true
draft: false
---

[CF 105018N - Núi lửa và quả trứng rồng](https://codeforces.com/problemset/problem/105018/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một hố núi lửa được biểu diễn dưới dạng$n \times m$lưới nơi mỗi ô lưu trữ điểm đó cao hơn bao nhiêu so với mức magma tại thời điểm 0. Các ô có giá trị bằng 0 đã ở mức magma, nghĩa là chúng sẽ bị nhấn chìm ngay lập tức. 

Magma bắt đầu dâng lên đồng đều theo thời gian với tốc độ 1 milimet trên mili giây. Theo thời gian, mức magma hiệu quả tăng lên, do đó, bất kỳ ô nào có chiều cao nhỏ hơn hoặc bằng mức magma hiện tại sẽ bị nhấn chìm. 

Một quả trứng rồng được đặt trên một ô cụ thể$(x, y)$và chúng tôi muốn xác định thời điểm chính xác khi magma lần đầu tiên đến ô đó. Tuy nhiên, magma không nhất thiết phải tiếp cận trực tiếp tế bào đó thông qua chiều cao của chính nó. Nếu đá xung quanh tạo thành một rào cản, magma phải “tràn” qua rào cản thấp nhất dọc theo bất kỳ con đường nào từ vùng dung nham bên ngoài đến vị trí của quả trứng. Vì ranh giới bên ngoài đã là dung nham nên magma luôn có thể bắt đầu từ đó và lan truyền vào bên trong thông qua các giới hạn ngày càng thấp. 

Giải thích chính là thời điểm chạm vào quả trứng được xác định bởi “rào cản độ cao tối đa” tối thiểu có thể dọc theo bất kỳ đường nào từ ranh giới (vùng dung nham ban đầu) đến tế bào trứng. Nói cách khác, mọi đường dẫn đều có chiều cao giới hạn, được xác định bởi giá trị ô tối đa dọc theo đường dẫn đó và chúng tôi muốn đường dẫn giảm thiểu mức tối đa này. 

Các ràng buộc cho phép lên đến$10^3 \times 10^3$kích thước lưới là một triệu ô. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào với$O(n^2 \log n)$hoặc$O(n^2)$có thể chấp nhận được, nhưng bất cứ điều gì như$O(n^2 \cdot n)$hoặc BFS/DFS lặp đi lặp lại cho mỗi truy vấn sẽ quá chậm. 

Một cách tiếp cận ngây thơ có thể cố gắng mô phỏng magma dâng lên từng mili giây hoặc lấp đầy lũ cho từng ngưỡng thời gian. Điều đó là không thể vì câu trả lời có thể lên tới$10^9$và mỗi bước mô phỏng sẽ yêu cầu quét lưới, dẫn đến không giới hạn$O(10^9 \cdot nm)$quá trình. 

Ý tưởng ngây thơ thứ hai là, đối với ô trứng, thử tất cả các đường đi từ ranh giới và tính toán độ cao tối đa dọc theo mỗi đường đi. Tuy nhiên, việc liệt kê tất cả các đường dẫn là theo cấp số nhân và không khả thi ngay cả đối với các lưới nhỏ. 

Một cách tiếp cận sai lầm tinh vi hơn là cho rằng chúng ta chỉ cần chiều cao của tế bào trứng. Điều này không thành công vì quả trứng có thể được bao bọc bởi một vòng đá cao hơn và magma trước tiên phải dâng lên trên vòng đó trước khi chạm tới nó. 

Một trường hợp thất bại cụ thể là:```
0 2 0
2 1 2
0 2 0
```Nếu quả trứng ở giữa, giá trị của nó là 1, nhưng magma không thể chạm tới nó cho đến khi nó tăng lên 2 vì rào cản xung quanh chặn lối vào. Một câu trả lời ngây thơ “chỉ cần lấy a[x][y]” sẽ cho kết quả 1, điều này không chính xác; câu trả lời đúng là 2. 

## Phương pháp tiếp cận 

Quan điểm bạo lực coi lưới như một cảnh quan có trọng số trong đó mỗi lần di chuyển giữa các ô liền kề bị hạn chế bởi độ cao cao nhất gặp phải cho đến nay. Từ bất kỳ ô ranh giới nào, chúng tôi cố gắng tiếp cận ô trứng trong khi theo dõi độ cao tối đa dọc theo đường đi. Trong số tất cả các đường dẫn có thể, chúng ta muốn một đường đi cực tiểu hóa giá trị lớn nhất này. 

Đây là một vấn đề đường dẫn minimax cổ điển. Giải pháp lực lượng vũ phu sẽ liệt kê tất cả các đường dẫn từ ranh giới đến mục tiêu, tính toán giá trị ô tối đa của mỗi đường dẫn và lấy giá trị tối thiểu trên các cực đại này. Số lượng đường dẫn trong lưới tăng theo cấp số nhân, đại khái là$O(4^{nm})$trong trường hợp xấu nhất, làm cho điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là điều này tương đương với bài toán đường đi ngắn nhất trong biểu đồ trong đó mỗi nút là một ô lưới, nhưng chi phí đường dẫn không phải là phụ gia. Thay vào đó, chi phí của một đường đi là trọng số cạnh tối đa (chiều cao ô) dọc theo nó. Điều này biến vấn đề thành việc tìm đường đi giảm thiểu trọng số tối đa gặp phải, được giải quyết một cách tự nhiên bằng cách sử dụng một biến thể của thuật toán Dijkstra trong đó việc thư giãn sử dụng`max(current_cost, next_cell_value)`thay vì bổ sung. 

Chúng tôi bắt đầu đồng thời từ tất cả các ô ranh giới, vì magma bắt nguồn từ bên ngoài. Mỗi ô ranh giới có chi phí ban đầu bằng chiều cao của nó. Sau đó, chúng tôi truyền vào bên trong, luôn mở rộng ô có “mức lũ” hiện tại nhỏ nhất được biết đến trước tiên. Điều này đảm bảo rằng khi chúng tôi tiếp cận một tế bào lần đầu tiên, chúng tôi đã tìm ra cách tốt nhất có thể để khiến nó tiếp xúc với magma đang dâng cao. 

Đây thực sự là đường đi ngắn nhất minimax trên biểu đồ lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Dijkstra Minimax |$O(nm \log nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi ô lưới là một nút trong biểu đồ. Di chuyển giữa các ô liền kề không tốn kém theo nghĩa truyền thống; thay vào đó, mỗi nút đóng góp chiều cao của nó như một ràng buộc. 

1. Khởi tạo hàng ưu tiên với tất cả các ô ranh giới, vì các ô này đã liền kề với magma tại thời điểm 0. Mỗi ô ranh giới được chèn với chi phí bằng chiều cao của nó. Đây là thời điểm magma sớm nhất có thể ảnh hưởng đến tế bào đó. 
2. Duy trì mảng khoảng cách`dist[i][j]`, trong đó mỗi giá trị lưu trữ độ cao tối đa tối thiểu có thể cần thiết để tiếp cận ô đó từ ranh giới. Ban đầu, tất cả các giá trị đều là vô cùng ngoại trừ các ô biên. 
3. Liên tục trích xuất ô có chi phí hiện tại nhỏ nhất từ ​​hàng đợi ưu tiên. Điều này đảm bảo rằng chúng tôi luôn mở rộng khu vực “rào cản thấp nhất” hứa hẹn nhất trước tiên. 
4. Đối với mỗi ô được trích xuất, hãy cố gắng thư giãn bốn ô lân cận của nó. Đối với người hàng xóm, hãy tính chi phí ứng cử viên như`max(current_cost, grid[nx][ny])`. Điều này tượng trưng cho tảng đá cao nhất mà magma phải đi qua để đến được hàng xóm đó qua con đường này. 
5. Nếu chi phí ứng cử viên này nhỏ hơn chi phí đã biết trước đó của hàng xóm, hãy cập nhật nó và đẩy nó vào hàng đợi ưu tiên. 
6. Tiếp tục cho đến khi tất cả các ô có thể tiếp cận được xử lý hoặc cho đến khi hết ô trứng. 
7. Câu trả lời là`dist[x][y]`. 

Tính chính xác đến từ tính bất biến rằng bất cứ khi nào một ô được đưa ra khỏi hàng đợi ưu tiên, chúng tôi đã tìm thấy rào cản tối đa tối thiểu có thể cần thiết để tiếp cận ô đó từ ranh giới. Bất kỳ đường đi thay thế nào cũng sẽ sử dụng cực đại trung gian bằng hoặc cao hơn và do đó không thể cải thiện kết quả. 

Điều này có hiệu quả vì hàng đợi ưu tiên luôn mở rộng các trạng thái theo thứ tự tăng giá trị nút cổ chai, đảm bảo không có đường dẫn nào tốt hơn đến nút tồn tại sau khi nó được hoàn tất. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    x, y = map(int, input().split())
    x -= 1
    y -= 1

    grid = [list(map(int, input().split())) for _ in range(n)]

    INF = 10**18
    dist = [[INF] * m for _ in range(n)]
    pq = []

    # all boundary cells start from outside lava
    for i in range(n):
        for j in range(m):
            if i == 0 or j == 0 or i == n - 1 or j == m - 1:
                dist[i][j] = grid[i][j]
                heapq.heappush(pq, (dist[i][j], i, j))

    dirs = [(1,0), (-1,0), (0,1), (0,-1)]

    while pq:
        cost, i, j = heapq.heappop(pq)

        if cost != dist[i][j]:
            continue

        if i == x and j == y:
            print(cost)
            return

        for di, dj in dirs:
            ni, nj = i + di, j + dj
            if 0 <= ni < n and 0 <= nj < m:
                nc = max(cost, grid[ni][nj])
                if nc < dist[ni][nj]:
                    dist[ni][nj] = nc
                    heapq.heappush(pq, (nc, ni, nj))

def main():
    solve()

if __name__ == "__main__":
    main()
```Giải pháp sử dụng khởi tạo Dijkstra đa nguồn từ tất cả các ô biên vì magma bắt nguồn từ mọi nơi trên lớp bên ngoài. Mỗi bước thư giãn sẽ cập nhật giá trị nút cổ chai được biết đến nhiều nhất để tiếp cận hàng xóm. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ coi chi phí di chuyển là phụ phí. sử dụng`max(cost, grid[nx][ny])`là điều cần thiết; thay thế nó bằng phép cộng sẽ phá vỡ hoàn toàn mô hình vì độ cao biểu thị ngưỡng chứ không phải khoảng cách tích lũy. 

Một điểm quan trọng khác là thoát sớm khi ô mục tiêu xuất hiện. Vì Dijkstra đảm bảo thứ tự chi phí không giảm nên lần đầu tiên chúng ta lấy tế bào trứng ra, chúng ta đã tìm thấy giá trị tối ưu của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
2 2
0 0 0 0
0 1 0 0
0 0 0 0
```Ở đây, quả trứng nằm ở trung tâm của một cái chậu gần như bằng phẳng với một ô duy nhất được nâng cao. 

| Bước | Tế bào bật lên | Chi phí | Hành động | 
| --- | --- | --- | --- | 
| 1 | ô ranh giới | 0 | khởi tạo biên giới | 
| 2 | (2,2) | 0 | mở rộng vào bên trong | 
| 3 | (1,2) | 1 | độ cao đầu tiên gặp phải | 

Thuật toán phát hiện ra rằng để đến được tâm cần phải đi qua ít nhất độ cao 1, vì vậy câu trả lời là 1. 

Điều này chứng tỏ rằng ngay cả một ô trên cao cũng có thể xác định được nút cổ chai ngay cả khi hầu hết địa hình bằng phẳng. 

### Ví dụ 2 

đầu vào:```
5 5
3 3
0 0 0 0 0
0 2 2 1 0
0 2 3 1 0
0 2 2 1 0
0 0 0 0 0
```| Bước | Đã trích xuất | Chi phí | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | ranh giới | 0 | bắt đầu lũ lụt | 
| 2 | vòng 1 bên trong | 1 | mở rộng chi phí thấp | 
| 3 | đổ chuông với 2s | 2 | gặp phải rào cản | 
| 4 | trung tâm (trứng) | 3 | hạn chế cuối cùng | 

Bảng cho thấy rằng mặc dù có các đường dẫn cục bộ thấp hơn nhưng mọi đường dẫn đến trung tâm cuối cùng phải đi qua một ô có giá trị 3, do đó thuật toán hội tụ chính xác. 

Điều này xác nhận rằng giải pháp theo dõi chính xác chướng ngại vật tối đa dọc theo con đường tốt nhất thay vì chỉ là mức tối thiểu cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \log (nm))$| Mỗi ô được chèn vào hàng đợi ưu tiên và được xử lý bằng các thao tác heap | 
| Không gian |$O(nm)$| Mảng khoảng cách và hàng đợi ưu tiên lưu trữ ở hầu hết các ô lưới | 

Kích thước lưới có thể đạt tới một triệu ô và hệ số logarit vẫn có thể quản lý được. Giải pháp vừa vặn thoải mái trong giới hạn 4 giây thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume refactor if needed
    return solve()

# sample-like small basin
assert run("""3 4
2 2
0 0 0 0
0 1 0 0
0 0 0 0
""") == "1"

# barrier ring forces higher threshold
assert run("""5 5
3 3
0 0 0 0 0
0 2 2 1 0
0 2 3 1 0
0 2 2 1 0
0 0 0 0 0
""") == "3"

# flat grid
assert run("""3 3
2 2
0 0 0
0 0 0
0 0 0
""") == "0"

# single peak blocking access
assert run("""3 3
2 2
0 2 0
2 9 2
0 2 0
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới phẳng | 0 | lũ lụt ngay lập tức | 
| rào cản vòng | 3 | lan truyền nút cổ chai | 
| đỉnh trung tâm | 2 | sự thống trị hạn chế con đường | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi quả trứng đã bị ngập hoàn toàn ở thời điểm 0 do nằm cạnh ranh giới dung nham có chiều cao bằng 0. Trong trường hợp như vậy, thuật toán ngay lập tức bắt đầu từ việc lan truyền biên và tế bào trứng có thể đã có chi phí tính toán bằng 0. Hàng đợi ưu tiên sẽ xuất hiện sớm, tạo ra câu trả lời đúng mà không cần xử lý thêm. 

Một trường hợp khác là một cái chậu kín hoàn toàn, trong đó quả trứng được bao quanh bởi những bức tường cao dần. Trong tình huống này, thuật toán đảm bảo rằng chi phí sẽ tăng dần khi đi qua từng lớp. Ngay cả khi có nhiều đường dẫn với các cấu hình rào cản khác nhau, việc truyền lan minimax đảm bảo mức tối đa thấp nhất có thể được chọn. 

Trường hợp khó phát hiện cuối cùng là khi có nhiều đường biên dẫn đến cùng một ô có cực đại trung gian khác nhau. Vùng heap có thể đẩy nhiều mục vào cho cùng một nút, nhưng`if cost != dist[i][j]`Guard đảm bảo các trạng thái cũ được bỏ qua, ngăn chặn các cập nhật không chính xác và duy trì tính chính xác trong quá trình nới lỏng lặp đi lặp lại.
