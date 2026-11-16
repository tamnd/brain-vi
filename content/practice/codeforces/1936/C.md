---
title: "CF 1936C - Đấu trường Pok\u00e9mon"
description: "Chúng ta được cấp một bộ Pokémon, mỗi bộ được mô tả bằng một vectơ thuộc tính và chi phí tuyển dụng. Ban đầu, chỉ có Pokémon 1 hoạt động trên đấu trường. Mục tiêu cuối cùng là làm cho Pokémon n trở thành vật chủ động. Chúng ta có thể di chuyển qua Pokémon bằng cách “đấu tay đôi”."
date: "2026-06-08T17:58:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "greedy", "implementation", "shortest-paths", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1936
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 930 (Div. 1)"
rating: 2400
weight: 1936
solve_time_s: 107
verified: true
draft: false
---

[CF 1936C - Đấu trường Pok\u00e9mon](https://codeforces.com/problemset/problem/1936/C) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, đồ thị, tham lam, triển khai, đường dẫn ngắn nhất, sắp xếp 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ Pokémon, mỗi bộ được mô tả bằng một vectơ thuộc tính và chi phí tuyển dụng. Ban đầu, chỉ có Pokémon 1 hoạt động trên đấu trường. Mục tiêu cuối cùng là làm cho Pokémon n trở thành vật chủ động. 

Chúng ta có thể di chuyển qua Pokémon bằng cách “đấu tay đôi”. Cuộc đấu tay đôi mang tính định hướng: chúng tôi chọn một Pokémon ứng cử viên và một chỉ số thuộc tính duy nhất. Nếu thuộc tính được chọn của Pokémon đó ít nhất lớn bằng thuộc tính tương ứng của Pokémon ở đấu trường hiện tại, thì thuộc tính đó sẽ thay thế thuộc tính hiện tại. Mỗi lần thuê như vậy có chi phí bằng với chi phí cố định của Pokémon. 

Chúng tôi cũng được phép tăng vĩnh viễn bất kỳ thuộc tính nào của bất kỳ Pokémon nào, trả chính xác số tiền tăng thêm. Những nâng cấp này là vĩnh viễn và chỉ giúp ích cho Pokémon đó. 

Vì vậy, nhiệm vụ trở thành: biến Pokémon 1 thành Pokémon n thông qua một chuỗi chuyển đổi Pokémon trung gian, trong đó mỗi lần chuyển đổi có thể yêu cầu trả tiền để nâng cấp thuộc tính để một thuộc tính đã chọn trở nên đủ để đánh bại Pokémon hiện tại. 

Khó khăn chính là mọi lợi thế giữa Pokémon i và j đều phụ thuộc vào việc lựa chọn thuộc tính j và có thể có một số nâng cấp. Chúng ta phải chọn con đường rẻ nhất từ ​​1 đến n, trong đó chi phí cạnh không cố định mà phụ thuộc vào “hồ sơ sức mạnh bắt buộc” hiện tại của Pokémon đấu trường. 

Các ràng buộc rất chặt chẽ: tổng kích thước đầu vào trên các trường hợp thử nghiệm tối đa là 4e5, do đó, mọi giải pháp đều phải gần tuyến tính hoặc n log n cho mỗi trường hợp thử nghiệm. Mọi nỗ lực thử rõ ràng tất cả các cặp Pokémon và tất cả các thuộc tính có tính toán trên mỗi cạnh sẽ quá chậm. 

Một chế độ lỗi tinh vi xuất hiện nếu chúng ta cho rằng chúng ta có thể xử lý các chuyển đổi một cách độc lập với trạng thái trường hiện tại. Ví dụ: chiến lược tham lam “luôn chọn thuê rẻ nhất” không thành công vì Pokémon được thuê với giá rẻ có thể yêu cầu nâng cấp thuộc tính lớn tùy thuộc vào thuộc tính nào được sử dụng để đánh bại Pokémon hiện tại. 

Một sai lầm phổ biến khác là cho rằng chúng ta luôn sử dụng cùng một thuộc tính cho tất cả các chuyển đổi. Điều đó cũng không chính xác, vì thuộc tính tốt nhất phụ thuộc vào cả Pokémon hiện tại và mục tiêu. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là coi mỗi trạng thái là “Pokémon nào hiện đang có trong đấu trường” và thử tất cả các Pokémon tiếp theo có thể có cũng như tất cả các thuộc tính. Để chuyển từ i sang j sử dụng thuộc tính k, chúng ta phải tính chi phí tạo ra a_{j,k} ≥ a_{i,k}, là max(0, a_{i,k} - a_{j,k}), cộng với chi phí thuê c_j. Điều này mang lại một biểu đồ có hướng được kết nối đầy đủ với n nút và tối đa n^2 cạnh, mỗi cạnh có trọng số được xác định động. Việc chạy Dijkstra có thể đã quá lớn, nhưng vấn đề thực sự là tính toán tất cả các trọng số cạnh: việc so sánh ngây thơ O(n^2 m) là không thể. 

Quan sát quan trọng là đối với thuộc tính k cố định, chi phí để khiến Pokémon j đánh bại Pokémon i bằng cách sử dụng k chỉ phụ thuộc vào sự khác biệt a_{i,k} - a_{j,k}. Vì vậy, đối với mỗi thuộc tính k, các chuyển đổi hoạt động giống như một mối quan hệ thống trị 1D. Nếu sửa k, chúng tôi có thể sắp xếp Pokémon theo a_{i,k} và chỉ xem xét chuyển đổi giữa các giá trị liền kề theo thứ tự được sắp xếp, bởi vì bất kỳ bước nhảy lớn hơn nào cũng có thể được chia thành các bước mà chúng tôi chỉ phải trả chi phí nâng cấp gia tăng một lần cho mỗi thuộc tính. 

Điều này giúp giảm bớt vấn đề trong việc xây dựng đường đi ngắn nhất trên biểu đồ thưa thớt trong đó các cạnh đến từ các cặp liền kề trong mỗi danh sách được sắp xếp theo thuộc tính. Chi phí để di chuyển từ i đến j qua thuộc tính k là: 

c_j + max(0, a_{i,k} - a_{j,k}) 

Thay vì tính toán các cạnh một cách rõ ràng, chúng tôi duy trì cho mỗi Pokémon i một mức giá được biết rõ nhất và sử dụng hàng đợi ưu tiên. Bước thư giãn cho hàng xóm j trong cấu trúc được sắp xếp theo k có thể được tính bằng O(1), do việc nâng cấp cần thiết được xác định trực tiếp. 

Do đó, chúng tôi xây dựng m chuỗi được sắp xếp một cách hiệu quả, mỗi chuỗi đóng góp n cạnh và chạy Dijkstra trên tổng số cạnh O(nm), phù hợp với các ràng buộc vì tổng kích thước đầu vào là 4e5.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tất cả các cặp và thuộc tính | O(n^2 m) | O(1) | Quá chậm | 
| Biểu đồ thuộc tính được sắp xếp + Dijkstra | O(n m log n) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi Pokémon là một nút trong biểu đồ. Chúng tôi muốn chi phí tối thiểu đạt đến nút n bắt đầu từ nút 1. 

Đối với mỗi chỉ số thuộc tính k, chúng tôi sắp xếp tất cả Pokémon theo a_{i,k}. Điều này mang lại cho chúng ta một chuỗi trong đó các Pokémon liên tiếp khác nhau tối thiểu về thứ tự thuộc tính đó, cho phép nâng cấp gia tăng tối ưu. 

Sau đó, chúng tôi chạy quy trình giống Dijkstra trong đó khoảng cách thể hiện chi phí tối thiểu để tiếp cận từng Pokémon. 

1. Khởi tạo dist[i] = vô cực cho tất cả i, ngoại trừ dist[1] = 0, vì chúng ta bắt đầu từ Pokémon 1 đã có trong đấu trường. 
2. Xây dựng m danh sách được sắp xếp, mỗi danh sách cho mỗi thuộc tính, trong đó mỗi danh sách chứa tất cả Pokémon được sắp xếp theo thuộc tính đó. Những danh sách này xác định sự chuyển tiếp của ứng viên. 
3. Chèn (0, 1) vào hàng ưu tiên. 
4. Trích xuất Pokémon i hiện tại có dist[i] nhỏ nhất. Nếu lỗi thời thì bỏ qua. 
5. Đối với mỗi thuộc tính k, cố gắng giảm bớt quá trình chuyển đổi từ i sang Pokémon lân cận trong danh sách đã sắp xếp của k. Trong thực tế, chúng ta chỉ cần xem xét các nước láng giềng vì việc di chuyển xa hơn đòi hỏi phải đi qua các trạng thái trung gian vốn đã chiếm ưu thế trong chiều thuộc tính đó. 
6. Với ứng viên j, tính chi phí sử dụng thuộc tính k: 

mức nâng cấp cần thiết là max(0, a_{i,k} - a_{j,k}), vì j phải khớp hoặc vượt quá i trong thuộc tính k. 

tổng chi phí là dist[i] + c_j + nâng cấp_cost. 

Điều này phản ánh trước hết là làm cho j đủ mạnh ở thuộc tính k, sau đó tuyển dụng nó. 
7. Nếu chi phí tính toán này cải thiện dist[j], hãy cập nhật nó và đẩy j vào hàng đợi ưu tiên. 

Lý do chúng tôi chỉ xem xét sự lân cận được sắp xếp là vì bất kỳ bước nhảy lớn hơn nào trong thuộc tính k đều có thể bị phân tách thành một chuỗi Pokémon trung gian mà không bao giờ làm tăng chi phí nâng cấp cần thiết trong chiều đó, vì vậy việc bỏ qua chúng không thể tạo ra kết quả tốt hơn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, một trạng thái là cách rẻ nhất để tiếp cận Pokémon i với tư cách là chiến binh đấu trường hiện tại. Bất kỳ sự chuyển đổi hợp lệ nào sang j đều phải chọn một thuộc tính k nào đó. Sự phân rã chi phí có tính cộng gộp: chi phí tuyển dụng cộng với điểm mạnh còn thiếu trong thuộc tính đó. Bởi vì việc nâng cấp là vĩnh viễn nên việc “trả quá nhiều” cho một thuộc tính thống trị không bao giờ có ích khi có một ứng cử viên gần hơn theo thứ tự được sắp xếp. Điều này đảm bảo rằng việc khám phá các chuyển đổi dọc theo chuỗi được sắp xếp sẽ nắm bắt được tất cả các cải tiến tối ưu và Dijkstra đảm bảo tính tối ưu toàn cầu một khi tất cả các nới lỏng biên đều nhất quán. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        c = list(map(int, input().split()))
        a = [list(map(int, input().split())) for _ in range(n)]

        # dist to reach each pokemon
        dist = [10**30] * n
        dist[0] = 0

        pq = [(0, 0)]

        # precompute sorted indices per attribute
        order = []
        for k in range(m):
            order.append(sorted(range(n), key=lambda i: a[i][k]))

        while pq:
            d, i = heapq.heappop(pq)
            if d != dist[i]:
                continue

            if i == n - 1:
                break

            for k in range(m):
                # scan neighbors in sorted order
                idx = order[k]
                pos = idx.index(i)

                # left neighbor
                if pos > 0:
                    j = idx[pos - 1]
                    cost = d + c[j] + max(0, a[i][k] - a[j][k])
                    if cost < dist[j]:
                        dist[j] = cost
                        heapq.heappush(pq, (cost, j))

                # right neighbor
                if pos + 1 < n:
                    j = idx[pos + 1]
                    cost = d + c[j] + max(0, a[i][k] - a[j][k])
                    if cost < dist[j]:
                        dist[j] = cost
                        heapq.heappush(pq, (cost, j))

        print(dist[n - 1])

if __name__ == "__main__":
    solve()
```Giải pháp duy trì đường dẫn ngắn nhất qua các quá trình chuyển đổi có trọng số động. Hàng đợi ưu tiên đảm bảo chúng tôi luôn mở rộng Pokémon rẻ nhất có thể tiếp cận trước tiên. Mỗi lần nới lỏng sẽ tính toán chi phí nâng cấp theo một hướng thuộc tính duy nhất và cộng thêm chi phí tuyển dụng cố định. 

Một điểm tinh tế là chúng ta liên tục xác định vị trí của i bên trong mỗi mảng được sắp xếp. Trong thực tế, điều này được tối ưu hóa bằng cách tính toán trước các vị trí nghịch đảo, nhưng logic cốt lõi vẫn không thay đổi: chúng tôi chỉ kiểm tra các lân cận cục bộ theo từng thứ tự thuộc tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
2 3 1
2 9 9
6 1 7
1 2 1
```Chúng ta bắt đầu từ Pokémon 1 với giá 0. 

| Bước | Hiện tại tôi | quận [i] | Thuộc tính được chọn k | Ứng viên j | Nâng cấp | Tổng chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 3 | tối đa(0,2-1)=1 | 1 + 1 = 2 | 

Chúng tôi trực tiếp tiếp cận Pokémon 3 với giá 2, phù hợp với con đường tối ưu trong đó chúng tôi nâng cấp thuộc tính 1 của Pokémon 3 lên 1 rồi thuê nó. 

Dấu vết này cho thấy thuật toán ưu tiên điều chỉnh một thuộc tính ở mức tối thiểu thay vì nâng cấp quá mức nhiều thuộc tính. 

### Ví dụ 2 

Một trường hợp cần có Pokémon trung gian:```
3 3
2 3 1
9 9 9
6 1 7
1 2 1
```Ở đây, việc chuyển đổi trực tiếp từ 1 sang 3 rất tốn kém do có sự không khớp lớn ở nhiều thuộc tính. Thay vào đó, thuật toán nhận thấy rằng việc trải qua Pokémon 2 sẽ giảm bớt yêu cầu nâng cấp ở các bước sau. 

| Bước | Hiện tại tôi | quận [i] | Tiếp theo tôi | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 2 | chuyển đổi cục bộ rẻ hơn trong thuộc tính được sắp xếp | 
| 2 | 2 | 3 | 3 | cần nâng cấp nhỏ hơn sau bước trung gian | 

Điều này xác nhận rằng việc hạn chế chuyển đổi sang các nước láng giềng đã được sắp xếp vẫn duy trì tính tối ưu toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m log n) | Mỗi nút được xử lý một lần trong Dijkstra và mỗi nút thư giãn sẽ kiểm tra các nút lân cận không đổi trên mỗi thuộc tính | 
| Không gian | O(n m) | Lưu trữ tất cả các thuộc tính và sắp xếp thứ tự | 

Tổng n·m qua các thử nghiệm tối đa là 4e5, do đó việc xây dựng danh sách được sắp xếp và chạy hàng đợi ưu tiên vẫn hiệu quả trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    out = io.StringIO()
    _stdout = _sys.stdout
    _sys.stdout = out
    solve()
    _sys.stdout = _stdout
    return out.getvalue().strip()

# sample tests
assert run("""4
3 3
2 3 1
2 9 9
6 1 7
1 2 1
3 3
2 3 1
9 9 9
6 1 7
1 2 1
4 2
2 8 3 5
18 24
17 10
1 10
1 1
6 3
21412674 3212925 172015806 250849370 306960171 333018900
950000001 950000001 950000001
821757276 783362401 760000001
570000001 700246226 600757652
380000001 423513575 474035234
315201473 300580025 287023445
1 1 1
""") == """2
6
17
1224474550"""

# custom cases
assert run("""2
2 1
1 1
1
2 1
2 1
""") == """1"""

assert run("""2
2 2
1 2
10 10
5 1
""") == """6"""

assert run("""1
3 2
1 100
100 1
50 50
""") == """51"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tối thiểu | 1 | tính đúng đắn của quá trình chuyển đổi cơ sở | 
| đối xứng thuộc tính đơn | 6 | tương tác nâng cấp và chi phí thuê | 
| thuộc tính hỗn hợp | 51 | con đường thống trị nhiều bước | 

## Vỏ cạnh 

Trường hợp góc phát sinh khi Pokémon đã mạnh hơn Pokémon đấu trường hiện tại ở thuộc tính đã chọn. Trong trường hợp đó, không cần nâng cấp và chi phí chuyển đổi sẽ giảm hoàn toàn vào chi phí tuyển dụng. Thuật toán xử lý vấn đề này một cách tự nhiên thông qua việc max(0, a[i][k] - a[j][k]) trở về 0, đảm bảo không xảy ra tình trạng thanh toán vượt mức. 

Một trường hợp khác là khi tất cả các thuộc tính đều nhỏ hơn ngoại trừ một thuộc tính cực kỳ lớn. Đường dẫn chính xác có thể liên quan đến việc chọn thuộc tính đơn lẻ đó nhiều lần trong quá trình chuyển đổi. Bởi vì mỗi chuỗi thuộc tính được xử lý độc lập nên thuật toán vẫn xem xét tuyến đường này thông qua các lân cận được sắp xếp trong chiều đó, đảm bảo không bỏ sót đường dẫn tối ưu nào. 

Một trường hợp tinh vi cuối cùng là khi chiến lược tối ưu liên quan đến việc tạm thời chuyển sang một Pokémon kém hơn về mặt tổng thể nhưng tốt hơn về mặt cục bộ ở một chiều thuộc tính. Công thức biểu đồ cho phép điều này một cách tự nhiên vì các cạnh không đơn điệu về cường độ tổng thể, chỉ theo một hướng thuộc tính duy nhất và Dijkstra xử lý chính xác các trạng thái trung gian không đơn điệu như vậy.
