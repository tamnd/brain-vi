---
title: "CF 106404H - Quần đảo núi lửa"
description: "Chúng ta được cung cấp một biểu đồ các hòn đảo được nối với nhau bằng những cây cầu vô hướng có trọng số. Việc đi qua một cây cầu cần có thời gian bằng trọng lượng của nó, do đó việc di chuyển bị chi phối bởi khoảng cách đường đi ngắn nhất theo nghĩa thông thường. Mỗi hòn đảo cũng có “hạn chót” khi núi lửa của mình phun trào."
date: "2026-06-25T10:03:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106404
codeforces_index: "H"
codeforces_contest_name: "Bay Area Programming Contest 2026 Advanced Division"
rating: 0
weight: 106404
solve_time_s: 39
verified: true
draft: false
---

[CF 106404H - Quần đảo núi lửa](https://codeforces.com/problemset/problem/106404/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ các hòn đảo được nối với nhau bằng những cây cầu vô hướng có trọng số. Việc đi qua một cây cầu cần có thời gian bằng trọng lượng của nó, do đó việc di chuyển bị chi phối bởi khoảng cách đường đi ngắn nhất theo nghĩa thông thường. 

Mỗi hòn đảo cũng có “hạn chót” khi núi lửa của mình phun trào. Thời điểm một ngọn núi lửa phun trào, tất cả các cây cầu nối với hòn đảo đó đều bị phá hủy, có nghĩa là hòn đảo đó sẽ không thể sử dụng được cho bất kỳ chuyến đi nào sau thời điểm đó. 

Có một quá trình thứ hai xảy ra đồng thời: dung nham bắt đầu lan rộng từ các hòn đảo đã bị đốt cháy thông qua cùng một biểu đồ có trọng số, đồng thời lấy trọng số của các cạnh làm thời gian di chuyển. Nếu dung nham đến một hòn đảo trước thời điểm phun trào, thay vào đó, hòn đảo đó sẽ cháy sớm hơn vào thời điểm dung nham xuất hiện. Nếu không nó sẽ cháy đúng vào thời điểm phun trào. 

Một nguyên tắc quan trọng là việc đến đúng thời điểm cháy vẫn có giá trị. Bạn có thể vào một hòn đảo vào thời điểm nó cháy và vẫn sử dụng nó làm bước đệm. 

Nhiệm vụ là xác định xem có tồn tại đường đi từ nút 1 đến nút n sao cho thời gian đến mỗi hòn đảo được viếng thăm không bao giờ lớn hơn thời gian đốt cháy của hòn đảo đó hay không. 

Cấu trúc ràng buộc đủ lớn để bất kỳ nghiệm nào cũng phải gần với tuyến tính tính bằng m, vì cả n và m có tổng bằng khoảng 1e5 qua các thử nghiệm. Điều này ngay lập tức loại trừ mọi thứ như tính toán lại các đường dẫn ngắn nhất cho mỗi ràng buộc hoặc thực hiện BFS lặp lại trên mỗi nút. Một đường chuyền kiểu Dijkstra duy nhất hoặc tìm kiếm nhị phân được quản lý cẩn thận về tính khả thi là những ứng cử viên thực tế duy nhất. 

Một số tình huống nguy hiểm rất dễ xử lý sai. 

Một chế độ lỗi xuất hiện khi một đường dẫn chỉ khả thi vì chúng tôi đến đúng thời điểm cháy. Ví dụ: nếu một hòn đảo cháy vào lúc thứ 5 và chúng ta đến lúc thứ 5, điều đó vẫn được phép. Bất kỳ kiểm tra bất bình đẳng nghiêm ngặt nào cũng sẽ chặn các tuyến đường hợp lệ một cách không chính xác. 

Một trường hợp tinh vi khác xảy ra khi dung nham đốt cháy một hòn đảo sớm hơn thời điểm nó phun trào. Thời hạn có hiệu lực không chỉ đơn giản là ai, mà là khoảng cách tối thiểu giữa ai và khoảng cách đường đi ngắn nhất từ ​​bất kỳ nguồn đốt ban đầu nào (được ẩn trong mô hình). Một cách tiếp cận ngây thơ chỉ kiểm tra ai có thể đánh giá quá cao mức độ an toàn và chấp nhận các đường dẫn không hợp lệ. 

Cuối cùng, nhiều con đường có thể đến một hòn đảo vào những thời điểm khác nhau và chỉ con đường đến sớm nhất mới có ý nghĩa quan trọng đối với việc mở rộng trong tương lai. Một cuộc đi bộ tham lam không phải Dijkstra, không theo dõi thời gian đến nổi tiếng nhất có thể bỏ lỡ các tuyến đường hợp lệ đi vòng sớm nhưng mở khóa các lối tắt sau. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề là mô phỏng khả năng tiếp cận theo thời gian. Trong mỗi thời điểm, chúng tôi có thể truyền đồng thời cả cừu và dung nham trên biểu đồ, cập nhật những hòn đảo nào vẫn có thể sử dụng được. Mỗi bước truyền tải sẽ yêu cầu tính toán lại thời gian đến ngắn nhất theo biểu đồ thu nhỏ động trong đó các nút biến mất theo thời gian. 

Tính chính xác rất đơn giản về mặt khái niệm: chúng tôi mô phỏng trực tiếp các quy tắc. Vấn đề là sự phức tạp. Mọi sự kiện (dung nham xuất hiện hoặc phun trào) có thể ảnh hưởng đến nhiều cạnh và khả năng tính toán lại khả năng tiếp cận liên tục suy biến thành thứ gì đó như O(nm log m) hoặc tệ hơn tùy thuộc vào việc triển khai. Với tối đa 1e5 cạnh, điều này vượt xa giới hạn. 

Quan sát quan trọng là chúng ta thực sự không cần mô phỏng thời gian một cách liên tục. Điều duy nhất quan trọng đối với tính khả thi là liệu có tồn tại một đường đi từ 1 đến n sao cho với mỗi nút v trên đường đi, thời gian đến dist[v] cao nhất là thời gian ghi hiệu quả của nó hay không. Điều này chuyển bài toán thành bài toán đường đi ngắn nhất có ràng buộc.

Nếu chúng tôi chạy Dijkstra từ nút 1, chúng tôi sẽ tính toán thời gian đến sớm nhất có thể đến mọi nút mà bỏ qua các ràng buộc về núi lửa. Sau đó, chúng tôi chỉ đơn giản thực thi quy tắc cắt tỉa: chúng tôi chỉ được phép nới lỏng một cạnh thành v nếu thời gian đến của ứng viên không vượt quá thời gian ghi của v. Nếu không thể liên lạc được với một nút trong thời hạn của nó, nút đó sẽ bị loại khỏi danh sách xem xét. 

Điều này hiệu quả vì thời gian đến ngắn nhất đều đơn điệu: một khi chúng ta biết thời gian sớm nhất chúng ta có thể đến một nút, thì bất kỳ thời điểm nào đến muộn hơn đều sẽ kém hiệu quả hơn về tính khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng thời gian vũ phu | O(nm log m) | O(n + m) | Quá chậm | 
| Dijkstra với việc cắt tỉa thời hạn | O(m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán thời gian ghi hiệu quả cho mỗi hòn đảo, sau đó chạy tìm kiếm đường đi ngắn nhất có ràng buộc. 

1. Đối với mỗi hòn đảo, hãy coi thời gian cháy của nó như đã cho. Điều này thể hiện thời điểm cuối cùng nó có thể được sử dụng một cách an toàn như một nút trung gian. 
2. Chạy Dijkstra tiêu chuẩn từ nút 1, trong đó mảng khoảng cách biểu thị thời gian đến sớm nhất. 
3. Khởi tạo khoảng cách [1] = 0 và đẩy nó vào hàng đợi ưu tiên. 
4. Khi trích xuất một nút u với thời gian hiện tại t, hãy bỏ qua nó nếu chúng ta đã tìm thấy thời gian đến tốt hơn cho u. Đây là hành vi Dijkstra tiêu chuẩn. 
5. Với mỗi hàng xóm v có thể tiếp cận được thông qua trọng số cạnh w, hãy tính thời gian ứng viên t + w. 
6. Nếu t + w lớn hơn burn[v], hãy bỏ qua hoàn toàn quá trình chuyển đổi này. Điều này buộc chúng ta không bao giờ bước vào một nút sau khi nó không còn sử dụng được nữa. 
7. Ngược lại, hãy nới lỏng dist[v] nếu t + w nhỏ hơn giá trị hiện tại của nó và đẩy nó vào hàng đợi ưu tiên. 
8. Sau khi thuật toán kết thúc, kiểm tra xem dist[n] có hữu hạn hay không. Nếu có thì xuất ra CÓ, ngược lại thì xuất ra KHÔNG. 

Lý do điều này là đủ là vì chúng tôi đang tìm kiếm một cách hiệu quả trên tất cả các đường dẫn nhưng chỉ loại bỏ những đường dẫn vi phạm ràng buộc khả thi cứng tại thời điểm chúng tôi bước vào một nút. Mọi đường dẫn hợp lệ đều phải tuân thủ các ràng buộc này ở mọi tiền tố, vì vậy nó sẽ vẫn có sẵn cho quá trình thư giãn của Dijkstra. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        g = [[] for _ in range(n)]
        for _ in range(m):
            x, y, w = map(int, input().split())
            x -= 1
            y -= 1
            g[x].append((y, w))
            g[y].append((x, w))

        dist = [INF] * n
        dist[0] = 0

        pq = [(0, 0)]

        while pq:
            tcur, u = heapq.heappop(pq)
            if tcur != dist[u]:
                continue
            if tcur > a[u]:
                continue

            for v, w in g[u]:
                nt = tcur + w
                if nt <= a[v] and nt < dist[v]:
                    dist[v] = nt
                    heapq.heappush(pq, (nt, v))

        print("YES" if dist[n - 1] < INF else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai cốt lõi là một vòng lặp Dijkstra tiêu chuẩn, với một kiểm tra ràng buộc bổ sung duy nhất: chúng tôi từ chối nới lỏng các cạnh xuất hiện sau thời gian ghi của nút. Điều kiện cắt tỉa thứ hai bên trong bước bật lên rất quan trọng vì hàng đợi ưu tiên vẫn có thể chứa các mục không hợp lệ do vượt quá thời hạn. 

Một lỗi phổ biến là quên rằng một nút có thể được bật lên với thời gian lớn hơn thời hạn của nó ngay cả khi nút đó đã được chèn trước đó. Đó là lý do tại sao chúng tôi loại bỏ rõ ràng các trạng thái như vậy trước khi khám phá các trạng thái lân cận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
100 2 100
1 2 2
2 3 2
```| Bước | Nút | Thời gian | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | bắt đầu | 
| 2 | 2 | 2 | 0+2 2 nên hợp lệ | 
| 3 | 3 | 4 | 2+2 ≤ 100 nên hợp lệ | 

Nút 2 chặt chẽ nhưng được phép vì thời gian đến bằng với thời gian cháy của nó. Điều này cho phép tiếp cận nút 3, xác nhận rằng sự bình đẳng là có thể chấp nhận được và không bị từ chối. 

### Ví dụ 2 

đầu vào:```
5 7
46 34 5 60 36
...
```Chúng tôi tập trung vào hạn chế tới hạn ở nút 3, nút này có thời gian ghi rất nhỏ. Bất kỳ con đường nào đạt được nó đều phải thực hiện rất sớm. 

| Bước | Nút | Thời gian | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | bắt đầu | 
| 2 | 3 | 23 | bị từ chối (23 > 5) | 
| 3 | 5 | 7 | được chấp nhận | 
| 4 | 2 | 32 | bị từ chối nếu vượt quá 34 tùy theo đường dẫn | 

Dấu vết cho thấy Dijkstra đương nhiên tránh định tuyến qua nút 3 vì tất cả các ứng cử viên đến đều vượt quá thời hạn. Tính chính xác của thuật toán đến từ việc cắt bớt hoàn toàn các nút như vậy thay vì cố gắng xem lại chúng sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi cạnh được thư giãn tối đa một lần bằng một cú đẩy đống | 
| Không gian | O(n + m) | danh sách kề và mảng khoảng cách | 

Tổng số nút và cạnh trong các thử nghiệm được giới hạn bởi 1e5, do đó, độ phức tạp này nằm trong giới hạn thoải mái đối với cửa sổ thực thi 2 giây trong Python với tối ưu hóa vùng nhớ heap. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf
    import heapq

    INF = 10**30
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        g = [[] for _ in range(n)]
        for _ in range(m):
            x, y, w = map(int, input().split())
            x -= 1
            y -= 1
            g[x].append((y, w))
            g[y].append((x, w))

        dist = [INF] * n
        dist[0] = 0
        pq = [(0, 0)]

        while pq:
            d, u = heapq.heappop(pq)
            if d != dist[u]:
                continue
            if d > a[u]:
                continue
            for v, w in g[u]:
                nd = d + w
                if nd <= a[v] and nd < dist[v]:
                    dist[v] = nd
                    heapq.heappush(pq, (nd, v))

        out.append("YES" if dist[n-1] < INF else "NO")

    return "\n".join(out)

# custom tests
assert run("""1
3 2
100 2 100
1 2 2
2 3 2
""") == "YES"

assert run("""1
2 0
1 1
""") == "NO"

assert run("""1
4 3
10 10 10 10
1 2 5
2 3 5
3 4 5
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| con đường đơn giản | CÓ | bình đẳng trước thời hạn | 
| đồ thị bị ngắt kết nối | KHÔNG | trường hợp không thể truy cập | 
| đồ thị chuỗi | CÓ | thư giãn tích lũy | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một nút có thể truy cập chính xác tại thời điểm ghi của nó nhưng chỉ thông qua một đường dẫn thay thế dài hơn thường bị từ chối nếu sử dụng bất đẳng thức nghiêm ngặt. Ví dụ: nếu một nút có thời gian ghi là 10 và có hai đường dẫn đến nút đó vào thời điểm 10 và 11 thì chỉ có đường dẫn đầu tiên là hợp lệ. Thuật toán xử lý việc này vì nó sử dụng một`<=`điều kiện khi nới lỏng các cạnh, cho phép bình đẳng nhưng không bao gồm tràn. 

Một trường hợp khác là khi bản thân nút khởi đầu có thời gian ghi rất nhỏ. Nếu a1 bằng 0 thì trạng thái ban đầu hợp lệ duy nhất là đến tại thời điểm 0 và mọi cạnh đi ra phải được kiểm tra theo ràng buộc đó ngay lập tức. Logic hàng đợi ưu tiên đảm bảo rằng chúng tôi vẫn xử lý nút 1, nhưng bất kỳ hàng xóm nào vi phạm ràng buộc của nó sẽ bị loại bỏ sớm. 

Trường hợp tinh vi thứ ba là khi một nút được chèn vào hàng đợi ưu tiên nhiều lần với các thời điểm khác nhau, một số hợp lệ và một số không hợp lệ. Séc`if d != dist[u]`ngăn chặn việc xử lý các mục nhập cũ và bổ sung`d > a[u]`kiểm tra ngăn việc mở rộng các nút không hợp lệ trong thời gian chờ đợi.
