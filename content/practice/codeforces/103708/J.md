---
title: "CF 103708J - Tham vọng của Jeffrey"
description: "Bài toán đưa ra một tập hợp các cá nhân giàu có và một tập hợp các công ty. Mỗi người có một danh sách các công ty mà họ sẵn sàng mua và mỗi công ty có thể được giao cho nhiều nhất một người."
date: "2026-07-02T09:32:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103708
codeforces_index: "J"
codeforces_contest_name: "2022 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 103708
solve_time_s: 50
verified: true
draft: false
---

[CF 103708J - Tham vọng của Jeffrey](https://codeforces.com/problemset/problem/103708/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một tập hợp các cá nhân giàu có và một tập hợp các công ty. Mỗi người có một danh sách các công ty mà họ sẵn sàng mua và mỗi công ty có thể được giao cho nhiều nhất một người. Một số người có thể không có công ty nào được phép và một số công ty có thể xuất hiện trong nhiều danh sách. 

Hội đồng phải giao công ty cho mọi người để mọi công việc đều tôn trọng danh sách, nghĩa là một người chỉ có thể nhận công ty mà họ đã niêm yết rõ ràng. Sau khi tất cả các nhiệm vụ được thực hiện, một số công ty có thể vẫn chưa được phân công. Mục tiêu là lựa chọn nhiệm vụ theo cách giảm thiểu số lượng công ty chưa được phân công. 

Theo thuật ngữ biểu đồ, đây là vấn đề đối sánh hai bên giữa mọi người và công ty, trong đó chúng tôi muốn tối đa hóa số lượng công ty phù hợp, vì mỗi công ty phù hợp sẽ loại bỏ một công ty khỏi số lượng "chưa được chỉ định". 

Các ràng buộc đủ lớn để cả số lượng người và công ty có thể lên tới 10.000 và tổng số ưu tiên của tất cả mọi người lên tới 100.000. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê tất cả các kết quả phù hợp hoặc liên tục mô phỏng việc phân công lại tham lam một cách ngây thơ. Bất kỳ giải pháp nào cũng cần phải hành xử gần tuyến tính hoặc gần tuyến tính về số cạnh. 

Trường hợp khó nhận biết xuất hiện khi một số người có danh sách trống. Ví dụ: nếu một người không có công ty được phép, họ không đóng góp gì cho việc so khớp nhưng vẫn tồn tại trong đầu vào. Một trường hợp đặc biệt khác xảy ra khi nhiều người muốn có cùng một công ty, chẳng hạn như cả hai người đều chỉ liệt kê công ty 1. Một nhiệm vụ tham lam ngây thơ xử lý mọi người theo thứ tự đầu vào có thể dễ dàng gán công ty cho một người dưới mức tối ưu và chặn sự kết hợp tổng thể tốt hơn. 

Ví dụ: hãy xem xét hai người đều quan tâm đến hai công ty giống nhau nhưng có cấu trúc không đối xứng: 

đầu vào:```
2 2
2 1 2
1 1
```Nếu chúng ta giao ngay người 1 cho công ty 1 thì không thể đáp ứng được người 2, nhưng tốt hơn là giao người 1 cho công ty 2, người 2 cho công ty 1, đạt được 0 công ty chưa được giao. Điều này cho thấy những quyết định tham lam của địa phương có thể thất bại. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng khám phá tất cả các nhiệm vụ có thể có của công ty đối với mọi người trong khi vẫn tôn trọng sở thích. Mỗi người có thể chọn trong số nhiều công ty và chúng tôi sẽ chỉ định hoặc bỏ qua các công ty một cách đệ quy, quay lại khi xảy ra xung đột. Điều này nhanh chóng trở thành cấp số nhân vì mỗi công ty được chia sẻ bởi nhiều người sẽ tạo ra các quyết định phân nhánh và trong trường hợp xấu nhất khi mỗi người muốn có nhiều công ty, số lượng trạng thái sẽ tăng lên khi tìm kiếm theo kiểu giai thừa trên các kết quả khớp. 

Quan sát quan trọng là chúng tôi thực sự không cố gắng phân công mọi người một cách tối ưu vì lợi ích của họ mà là để tối đa hóa số lượng công ty phù hợp. Điều này biến vấn đề thành việc tìm kiếm sự phù hợp lưỡng cực tối đa giữa mọi người và công ty. Mỗi trận đấu thành công sẽ giảm đi một công ty chưa được chỉ định, vì vậy tối đa hóa các trận đấu tương đương với việc giảm thiểu số lượng còn sót lại. 

Cấu trúc này cho phép chúng ta sử dụng các kỹ thuật so khớp cổ điển. Vì các cạnh không có trọng số và đồ thị là lưỡng cực nên chúng ta có thể áp dụng cách tiếp cận đường dẫn tăng cường dựa trên DFS tiêu chuẩn (thuật toán Kuhn). Ý tưởng là lặp lại mọi người và cố gắng phân công từng người cho một công ty, có thể sắp xếp lại các nhiệm vụ trước đó nếu điều đó cho phép kết hợp tổng thể tốt hơn. Khi một công ty đã bị chiếm đoạt, chúng tôi cố gắng đệ quy “đẩy” chủ sở hữu hiện tại của nó sang một công ty khác được chấp nhận, giải phóng nó cho người hiện tại. Cơ chế phân công lại cục bộ này chính xác là cơ chế khắc phục các trường hợp thất bại của việc phân công tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Tìm kiếm qua bài tập | Hàm mũ | O(N + M) | Quá chậm | 
| Kết hợp đường dẫn tăng cường DFS của Kuhn | O(N·E) trường hợp xấu nhất, ~O(E√V) điển hình | O(N + M + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình những người ở phía bên trái và các công ty ở phía bên phải của biểu đồ hai bên. Một cạnh kết nối một người với từng công ty trong danh sách ưu tiên của họ. 

1. Xây dựng danh sách lân cận nơi mỗi người lưu trữ tất cả các công ty mà họ sẵn sàng mua. Cách biểu diễn này cho phép duyệt một cách hiệu quả tất cả các kết quả phù hợp có thể có đối với một người. 
2. Duy trì một mảng`match_to_company`ghi lại người nào hiện đang sở hữu mỗi công ty. Ban đầu, tất cả các công ty đều chưa được chỉ định nên mảng này trống. 
3. Xác định chức năng tìm kiếm theo chiều sâu để cố gắng gán một công ty cho một người nhất định. Hàm này khám phá tất cả các công ty trong danh sách của người đó. 
4. Đối với mỗi công ty, hãy kiểm tra xem hiện tại công ty đó có chưa được chỉ định hay không. Nếu nó miễn phí, hãy gán ngay cho người hiện tại và trả lại thành công, vì chúng tôi đã tăng kích thước phù hợp mà không có xung đột. 
5. Nếu công ty đã được chỉ định cho một người khác, hãy cố gắng chỉ định lại người hiện tại đó cho một công ty khác bằng cách gọi đệ quy DFS cho người đó. Nếu lệnh gọi đệ quy đó thành công, điều đó có nghĩa là chúng tôi đã tìm được vị trí thay thế, vì vậy chúng tôi có thể đảm nhận công ty cho người hiện tại một cách an toàn. 
6. Lặp lại quy trình này cho tất cả mọi người. Mỗi khi một nhiệm vụ mới được thực hiện, nó có khả năng kích hoạt một chuỗi các nhiệm vụ được phân công lại nhằm duy trì tính khả thi trong khi tăng tổng số kết quả phù hợp. 
7. Sau khi xử lý tất cả mọi người, hãy đếm xem còn bao nhiêu công ty chưa được chỉ định. Câu trả lời là tổng số công ty trừ đi số công ty phù hợp. 

Ý tưởng chính đằng sau tính đúng đắn là mỗi khi chúng tôi chỉ định lại một công ty, chúng tôi sẽ duy trì một kết quả khớp hợp lệ trong khi có thể giải phóng không gian cho một nhiệm vụ khác. DFS đảm bảo rằng nếu có một cấu hình tốt hơn có thể truy cập được thông qua các giao dịch hoán đổi cục bộ dọc theo các đường dẫn xen kẽ thì cấu hình đó sẽ được tìm thấy. 

## Tại sao nó hoạt động 

Tại bất kỳ điểm nào trong thuật toán, tập hợp các công ty được chỉ định sẽ tạo thành một kết quả khớp hợp lệ. Tìm kiếm DFS khám phá các đường dẫn xen kẽ giữa các công ty tự do và bị chiếm đóng. Mỗi lần gán lại đệ quy thành công tương ứng với việc đi theo một con đường xen kẽ kết thúc tại một công ty tự do, điều này cho phép đảo ngược toàn bộ con đường. Điều này duy trì tính bất biến rằng không có công ty nào được giao cho nhiều hơn một người trong khi tăng số lượng công ty được giao một cách đơn điệu bất cứ khi nào có thể. Vì mỗi DFS thành công sẽ chỉ định một công ty chưa được chỉ định trước đó hoặc định cấu hình lại một chuỗi để giải phóng một công ty, nên tổng số kết quả phù hợp là tối đa khi không tồn tại đường dẫn tăng thêm nào nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    
    for i in range(n):
        data = list(map(int, input().split()))
        k = data[0]
        if k > 0:
            g[i] = data[1:]

    match = [-1] * (m + 1)
    vis = None

    def dfs(u):
        for v in g[u]:
            if vis[v]:
                continue
            vis[v] = True
            if match[v] == -1 or dfs(match[v]):
                match[v] = u
                return True
        return False

    for i in range(n):
        vis = [False] * (m + 1)
        dfs(i)

    matched = sum(1 for x in match if x != -1)
    print(m - matched)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ trực tiếp các công ty được phép của mỗi người, do đó DFS chỉ có thể liệt kê các chuyển đổi hợp lệ. các`match`mảng được công ty lập chỉ mục và lưu trữ người được chỉ định hiện tại. 

Mảng đã truy cập được tạo lại cho mỗi lần thử DFS nhằm ngăn chặn việc quay vòng trong một tìm kiếm tăng cường duy nhất, điều này là cần thiết vì biểu đồ có tính năng động về mặt khám phá nhưng có cấu trúc tĩnh. Đặt lại`vis`mỗi lần lặp bên ngoài đảm bảo rằng mỗi lần thử sẽ khám phá một cây tìm kiếm mới. 

Phép trừ cuối cùng`m - matched`chuyển đổi kích thước phù hợp tối đa thành số lượng công ty chưa được chỉ định, đây là đầu ra bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 6
2 1 2
0
1 3
1 4
2 1 5
```Chúng tôi chỉ theo dõi những thay đổi quan trọng về trận đấu. 

| Người | Sở thích | Kết quả DFS | trạng thái phù hợp (công ty → người) | 
| --- | --- | --- | --- | 
| 0 | 1,2 | giao 1 | 1→0 | 
| 1 | không | không thay đổi | 1→0 | 
| 2 | 3 | giao 3 | 1→0, 3→2 | 
| 3 | 4 | giao 4 | 1→0, 3→2, 4→3 | 
| 4 | 1,5 | 1 bị chiếm, 5 miễn phí | 1→0, 3→2, 4→3, 5→4 | 

Các công ty phù hợp = 4, do đó câu trả lời = 6 − 4 = 2. 

Dấu vết này cho thấy cách DFS tránh chặn các nhiệm vụ trong tương lai bằng cách chỉ cam kết với một công ty khi công ty đó rảnh hoặc có thể được giải phóng thông qua việc phân công lại. 

### Ví dụ 2 

đầu vào:```
5 5
1 1
1 2
1 3
1 4
1 5
```| Người | Sở thích | Kết quả DFS | trạng thái khớp | 
| --- | --- | --- | --- | 
| 0 | 1 | chỉ định 1 | 1→0 | 
| 1 | 2 | phân công 2 | 1→0, 2→1 | 
| 2 | 3 | phân công 3 | 1→0, 2→1, 3→2 | 
| 3 | 4 | phân công 4 | 1→0, 2→1, 3→2, 4→3 | 
| 4 | 5 | phân công 5 | 1→0, 2→1, 3→2, 4→3, 5→4 | 

Các công ty phù hợp = 5, câu trả lời = 0. 

Trường hợp này xác nhận thuật toán đạt được kết quả khớp hoàn toàn khi biểu đồ đã có cấu trúc hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N·E) trường hợp xấu nhất | Mỗi DFS có thể đi qua các cạnh nhiều lần trong chuỗi tăng cường | 
| Không gian | O(N + M + E) | danh sách kề cộng với mảng khớp và trạng thái đệ quy | 

Tổng số cạnh được giới hạn bởi 100.000 và cả N và M tối đa là 10.000, điều này làm cho phương pháp này đủ nhanh trong thực tế dưới các ràng buộc điển hình của Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, m = map(int, inp.split()[0:2])  # dummy to silence lint
    # re-run solution
    input_data = inp
    sys.stdin = io.StringIO(input_data)

    def solve():
        n, m = map(int, input().split())
        g = [[] for _ in range(n)]
        for i in range(n):
            data = list(map(int, input().split()))
            k = data[0]
            if k > 0:
                g[i] = data[1:]

        match = [-1] * (m + 1)

        def dfs(u, vis):
            for v in g[u]:
                if vis[v]:
                    continue
                vis[v] = True
                if match[v] == -1 or dfs(match[v], vis):
                    match[v] = u
                    return True
            return False

        for i in range(n):
            dfs(i, [False] * (m + 1))

        return str(m - sum(1 for x in match if x != -1))

    return solve()

# provided samples
assert run("""5 6
2 1 2
0
1 3
1 4
2 1 5
""") == "2"

assert run("""5 5
1 1
1 2
1 3
1 4
1 5
""") == "0"

# custom tests
assert run("""1 3
2 1 2
""") == "2", "single person"

assert run("""3 3
1 1
1 1
1 1
""") == "2", "conflict single target"

assert run("""2 2
1 1
1 2
""") == "0", "perfect matching"

assert run("""2 3
0
0
""") == "3", "no assignments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người độc thân | 2 | bài tập cơ bản | 
| tất cả đều muốn cùng một công ty | 2 | xử lý xung đột | 
| kết hợp hoàn hảo | 0 | phân công đầy đủ tối ưu | 
| không có bài tập | 3 | xử lý tùy chọn trống | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều người liệt kê cùng một công ty. Trong tình huống đó, một cách tiếp cận tham lam ngây thơ sẽ chỉ định người đầu tiên và chặn những người khác, nhưng việc so khớp dựa trên DFS đảm bảo chỉ còn lại một nhiệm vụ cuối cùng và tính chính xác phần còn lại là chưa được chỉ định. 

Một trường hợp khó khăn khác là những người có danh sách ưu tiên trống. Các nút này không tham gia vào bất kỳ nỗ lực so khớp nào và DFS tự nhiên bỏ qua chúng mà không ảnh hưởng đến tính chính xác. Thuật toán vẫn tính đúng các công ty chưa được chỉ định vì quy mô phù hợp chỉ phụ thuộc vào việc phân công thành công. 

Trường hợp cạnh cuối cùng phát sinh khi danh sách ưu tiên lớn và có mức độ chồng chéo cao, tạo ra các đường dẫn tăng cường dài. DFS đệ quy xử lý các chuỗi này một cách chính xác bằng cách liên tục gán lại dọc theo các đường dẫn xen kẽ cho đến khi đạt được một công ty miễn phí, đảm bảo không còn cấu hình tối ưu cục bộ nhưng vẫn tối ưu toàn cầu.
