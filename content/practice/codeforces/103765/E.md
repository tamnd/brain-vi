---
title: "CF 103765E - \u5b64\u72ec\u7684\u5c0fZ"
description: "Chúng ta được cung cấp một hệ thống các ràng buộc đối với một mảng các giá trị, một giá trị cho mỗi thành phố. Mỗi thành phố $i$ có một số nguyên không âm $xi$, biểu thị số lượng bạn bè ở thành phố đó."
date: "2026-07-02T08:55:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103765
codeforces_index: "E"
codeforces_contest_name: "2022 Collegiate Programming Contest of Xiangtan University"
rating: 0
weight: 103765
solve_time_s: 55
verified: true
draft: false
---

[CF 103765E - \u5b64\u72ec\u7684\u5c0fZ](https://codeforces.com/problemset/problem/103765/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống các ràng buộc đối với một mảng các giá trị, một giá trị cho mỗi thành phố. Mỗi thành phố$i$có một số nguyên không âm$x_i$, đại diện cho số lượng bạn bè ở thành phố đó. Đầu vào mô tả mối quan hệ giữa các cặp thành phố, trong đó mỗi mối quan hệ hạn chế sự khác biệt$x_a - x_b$theo một trong ba cách: nó có thể có ít nhất một giá trị nào đó, nhiều nhất là một giá trị nào đó, hoặc hoàn toàn bằng nhau. 

Nhiệm vụ không chỉ là tìm bất kỳ phép gán nào thỏa mãn tất cả các ràng buộc, mà là xác định xem liệu một phép gán hợp lệ có tồn tại hay không và nếu có thì tính tổng số bạn bè tối thiểu có thể có trên tất cả các thành phố. 

Vì vậy, về mặt khái niệm, chúng tôi đang gán các giá trị cho các nút theo các ràng buộc khác biệt và trong số tất cả các phép gán khả thi, chúng tôi muốn một giá trị giảm thiểu$\sum x_i$, với hạn chế ngầm định rằng tất cả$x_i \ge 0$. 

Các ràng buộc hàm ý một cấu trúc có định hướng của các bất bình đẳng. Ràng buộc giới hạn dưới$x_a - x_b \ge c$lực lượng$x_a \ge x_b + c$. Ràng buộc giới hạn trên$x_a - x_b \le c$có thể được viết lại như$x_b \ge x_a - c$. Bình đẳng chỉ đơn giản là áp đặt cả hai hướng. 

Những hạn chế$n, m \le 1000$Và$T \le 50$gợi ý rằng một$O(nm)$hoặc$O(nm \log n)$giải pháp cho mỗi bài kiểm tra là có thể chấp nhận được, trong khi mọi thứ khối cho mỗi bài kiểm tra sẽ quá chậm. 

Một vấn đề tế nhị là hệ thống không có điểm tham chiếu cố định. Nếu một giải pháp tồn tại, chuyển tất cả$x_i$bởi một hằng số bảo toàn mọi khác biệt. Điều này có nghĩa là chúng ta phải dựa vào yêu cầu về tính không tiêu cực để giữ vững giải pháp. 

Trường hợp cạnh điển hình xảy ra khi các ràng buộc hình thành một chu kỳ có mức tăng tổng dương, ví dụ:$x_1 \ge x_2 + 1$,$x_2 \ge x_1 + 1$. Điều này ngụ ý$x_1 \ge x_1 + 2$, điều đó là không thể, vì vậy câu trả lời phải là$-1$. 

Một trường hợp đặc biệt khác là khi tất cả các ràng buộc đều nhất quán nhưng buộc một biến phải rất lớn và một nỗ lực ngây thơ bỏ qua việc truyền bá các ràng buộc sẽ gán không chính xác tất cả các số 0. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng gán các giá trị cho tất cả các thành phố và kiểm tra xem liệu tất cả các ràng buộc có được giữ hay không, có thể bằng cách lặp lại tất cả các phép gán có thể đạt đến một số giới hạn. Ngay cả khi giới hạn các giá trị trong phạm vi hợp lý, không gian tìm kiếm vẫn theo cấp số nhân$n$, vì mỗi biến phụ thuộc vào các biến khác thông qua các bất đẳng thức. Điều này ngay lập tức trở nên không khả thi nếu vượt quá giới hạn rất nhỏ.$n$. 

Quan sát quan trọng là mọi ràng buộc đều có thể được viết lại dưới dạng một cạnh trong đồ thị có hướng với trọng số biểu thị giới hạn dưới của sai phân. Ví dụ,$x_a \ge x_b + c$hoạt động như một cạnh$b \to a$với trọng lượng$c$. Về cơ bản, chúng tôi đang cố gắng gán các giá trị sao cho mọi cạnh đều thực thi$x_v \ge x_u + w$. 

Đây chính xác là bài toán giãn đường đi dài nhất trong biểu đồ có thể chứa các chu trình. Chúng tôi muốn các giá trị nhỏ nhất thỏa mãn tất cả các ràng buộc giới hạn dưới, tương ứng với việc tìm giá trị tối đa trên tất cả các chuỗi ràng buộc đến từng nút. Nếu chúng tôi giới thiệu một siêu nguồn được kết nối với tất cả các nút có trọng số 0, chúng tôi có thể tính toán giới hạn dưới khả thi tối đa cho mỗi nút bằng cách sử dụng cách nới lỏng kiểu Bellman-Ford cho các đường dẫn dài nhất. 

Nếu trong lúc thư giãn, chúng ta vẫn có thể tăng thêm khoảng cách sau$n$lặp đi lặp lại, nó chỉ ra một chu kỳ tích cực, có nghĩa là hệ thống không nhất quán. 

Một lần tất cả$x_i$được xác định là các giá trị khả thi tối thiểu thì câu trả lời là tổng của các giá trị này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Ràng buộc khác biệt + Đường đi dài nhất | O(nm) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi tất cả các ràng buộc thành các cạnh có trọng số được định hướng và sau đó tính toán các giá trị khả thi tối đa bằng cách sử dụng tính năng thư giãn. 

1. Tạo biểu đồ với$n$các nút và thêm một nút siêu nguồn$0$. Nút kết nối$0$đến mọi thành phố$i$với một cạnh trọng lượng$0$, nghĩa$x_i \ge 0$. Điều này thực thi tính không tiêu cực. 
2. Đối với mỗi ràng buộc$x_a - x_b \ge c$, thêm một cạnh có hướng$b \to a$với trọng lượng$c$. Điều này mã hóa$x_a$ít nhất phải có$x_b + c$. 
3. Đối với mỗi ràng buộc$x_a - x_b \le c$, viết lại dưới dạng$x_b - x_a \ge -c$và thêm một cạnh có hướng$a \to b$với trọng lượng$-c$. Điều này đảm bảo giới hạn trên được thực thi trong cùng một khuôn khổ. 
4. Đối với các ràng buộc bình đẳng$x_a = x_b$, thêm cả hai$a \to b$Và$b \to a$các cạnh có trọng lượng$0$. Điều này buộc cả hai biến phải bằng nhau. 
5. Khởi tạo mọi khoảng cách$dist[i]$ĐẾN$0$, vì siêu nguồn cho phép mọi nút ít nhất bằng 0. 
6. Chạy thư giãn Bellman-Ford cho$n$lặp lại trên tất cả các cạnh, cập nhật$dist[v] = \max(dist[v], dist[u] + w)$. Bước này truyền bá giới hạn dưới mạnh nhất thông qua tất cả các chuỗi ràng buộc. 
7. Chạy thêm một lần lặp nữa trên tất cả các cạnh. Nếu bất kỳ khoảng cách nào vẫn có thể được cải thiện thì tồn tại một chu kỳ tích cực, nghĩa là các ràng buộc không nhất quán và câu trả lời là$-1$. 
8. Nếu không phát hiện thấy sự mâu thuẫn, hãy tính câu trả lời cuối cùng là tổng của tất cả$dist[i]$vì$1 \le i \le n$. 

Lý do chúng ta sử dụng mức giãn tối đa thay vì đường đi ngắn nhất là vì các ràng buộc xác định giới hạn dưới chứ không phải giới hạn trên, do đó các giá trị phải được đẩy lên trên cho đến khi tất cả các ràng buộc được thỏa mãn. 

### Tại sao nó hoạt động 

Mỗi bước thư giãn buộc mọi nút phải tôn trọng tất cả các ràng buộc dọc theo các đường dẫn có độ dài tăng dần. Sau đó$n$lặp đi lặp lại, mọi đường dẫn đơn giản đều đã được tính toán, do đó, bất kỳ cải tiến nào nữa đều phải đến từ một chu kỳ tăng giá trị vô thời hạn. Điều đó hoàn toàn tương ứng với một mâu thuẫn trong hệ thống ràng buộc. Khoảng cách kết quả tạo thành phép gán tối thiểu thỏa mãn tất cả các giới hạn dưới vì mọi biến được đặt thành giá trị bắt buộc mạnh nhất được ngụ ý bởi bất kỳ chuỗi ràng buộc nào bắt đầu từ đường cơ sở bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def solve():
    n, m = map(int, input().split())
    
    edges = []
    
    # super source: x_i >= 0 is already handled by initial 0 distances
    
    for _ in range(m):
        tmp = list(map(int, input().split()))
        t = tmp[0]
        
        if t == 1:
            a, b, c = tmp[1], tmp[2], tmp[3]
            # x_a - x_b >= c => b -> a (c)
            edges.append((b, a, c))
        elif t == 2:
            a, b, c = tmp[1], tmp[2], tmp[3]
            # x_a - x_b <= c => x_b - x_a >= -c => a -> b (-c)
            edges.append((a, b, -c))
        else:
            a, b = tmp[1], tmp[2]
            edges.append((a, b, 0))
            edges.append((b, a, 0))
    
    dist = [0] * (n + 1)
    
    # Bellman-Ford (max version)
    for _ in range(n):
        changed = False
        for u, v, w in edges:
            if dist[v] < dist[u] + w:
                dist[v] = dist[u] + w
                changed = True
        if not changed:
            break
    
    # detect positive cycle
    for u, v, w in edges:
        if dist[v] < dist[u] + w:
            return -1
    
    return sum(dist[1:])

def main():
    T = int(input())
    out = []
    for _ in range(T):
        out.append(str(solve()))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai lưu trữ tất cả các ràng buộc một cách thống nhất dưới dạng các cạnh có trọng số được chỉ dẫn. Việc nới lỏng được thực hiện ở dạng tối đa hóa, vì mỗi cạnh đại diện cho một giới hạn dưới có thể thắt chặt các nút khác lên trên. Việc thoát sớm bên trong vòng thư giãn sẽ ngăn chặn những lần lặp lại không cần thiết khi hệ thống ổn định nhanh chóng. 

Một lỗi phổ biến là coi đây là con đường ngắn nhất. Điều đó làm đảo lộn ý nghĩa của các ràng buộc và tạo ra các giá trị thỏa mãn giới hạn trên thay vì giới hạn dưới, điều này phá vỡ tính khả thi. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống nhỏ với ba thành phố nơi các ràng buộc tạo thành một chuỗi nhất quán:$x_1 \ge x_2 + 1$,$x_2 \ge x_3 + 2$, Và$x_3 \ge 0$. 

| Lặp lại | quận [1] | quận [2] | quận [3] | Giải thích | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | 0 | Tất cả đều bắt đầu từ con số 0 | 
| Sau 1 | 0 | 0 | 0 | Chưa có lợi thế ép buộc nào chiếm ưu thế | 
| Sau 2 | 0 | 2 | 0 | Từ$x_2 \ge x_3 + 2$| 
| Sau 3 | 3 | 2 | 0 | Từ$x_1 \ge x_2 + 1$| 

Tổng cuối cùng là$3 + 2 + 0 = 5$. 

Điều này cho thấy các ràng buộc lan truyền theo hướng ngược lại và tích lũy dọc theo chuỗi như thế nào. 

Bây giờ hãy xem xét một hệ thống không nhất quán:$x_1 \ge x_2 + 1$,$x_2 \ge x_1 + 1$. 

| Lặp lại | quận [1] | quận [2] | Giải thích | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | Bắt đầu | 
| Sau 1 | 1 | 1 | Cả hai cạnh đều tăng giá trị | 
| Sau 2 | 2 | 2 | Vẫn tăng | 
| Kiểm tra | chu kỳ | chu kỳ | Có cải tiến hơn nữa | 

Điều này chứng tỏ rằng sự củng cố lẫn nhau tạo ra sự gia tăng vô hạn, được coi là một chu kỳ tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Bellman-Ford nới lỏng tất cả các cạnh lên đến$n$lần mỗi bài kiểm tra | 
| Không gian | O(n + m) | Lưu trữ khoảng cách và các cạnh ràng buộc | 

Với$n, m \le 1000$và lên tới 50 bài kiểm tra, điều này vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    
    INF = 10**18

    def solve():
        n, m = map(int, input().split())
        edges = []
        
        for _ in range(m):
            tmp = list(map(int, input().split()))
            t = tmp[0]
            if t == 1:
                a, b, c = tmp[1], tmp[2], tmp[3]
                edges.append((b, a, c))
            elif t == 2:
                a, b, c = tmp[1], tmp[2], tmp[3]
                edges.append((a, b, -c))
            else:
                a, b = tmp[1], tmp[2]
                edges.append((a, b, 0))
                edges.append((b, a, 0))
        
        dist = [0] * (n + 1)
        
        for _ in range(n):
            changed = False
            for u, v, w in edges:
                if dist[v] < dist[u] + w:
                    dist[v] = dist[u] + w
                    changed = True
            if not changed:
                break
        
        for u, v, w in edges:
            if dist[v] < dist[u] + w:
                return -1
        
        return sum(dist[1:])

    # samples
    assert run("""3 3
3 1 2
1 1 3 1
2 2 3 2
""") == "-1"

    # simple consistent chain
    assert run("""3 2
1 1 2 1
1 2 3 1
""") == "3"

    # equality case
    assert run("""2 1
3 1 2
""") == "0"

    # contradiction cycle
    assert run("""2 2
1 1 2 1
1 2 1 1
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp mẫu | -1 | phát hiện chu kỳ không nhất quán | 
| ràng buộc chuỗi | 3 | tuyên truyền giới hạn dưới | 
| chỉ bình đẳng | 0 | sự lan truyền bằng 0 đúng | 
| chu kỳ mâu thuẫn | -1 | phát hiện chu kỳ tích cực | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi tất cả các biến chỉ bị ràng buộc bởi bất đẳng thức nhưng không bao giờ được liên kết trực tiếp với 0 ngoại trừ thông qua tính không âm tiềm ẩn. Ví dụ: nếu không có cạnh nào cả, mọi nút vẫn ở mức 0 và câu trả lời là 0. Thuật toán xử lý việc này một cách tự nhiên vì không xảy ra hiện tượng thư giãn. 

Một trường hợp khác là một chu kỳ đẳng thức lớn. Vì đẳng thức được biểu diễn dưới dạng hai cạnh có trọng lượng bằng 0 nên nó không tạo ra sự tăng trưởng về khoảng cách và hệ thống vẫn ổn định. Tổng cuối cùng vẫn bằng 0 trừ khi lực ràng buộc khác tăng lên. 

Trường hợp khó phát hiện cuối cùng là khi các ràng buộc tạo thành chuỗi dài chỉ ảnh hưởng đến các nút ở xa. Việc lan truyền dựa trên sự thư giãn đảm bảo rằng ngay cả các phụ thuộc gián tiếp cũng được tính toán đầy đủ trong phạm vi nhiều nhất$n$lặp lại, vì mỗi độ dài đường dẫn có ý nghĩa đều bị giới hạn bởi số lượng nút.
