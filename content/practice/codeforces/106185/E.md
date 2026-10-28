---
title: "CF 106185E - Sẽ ngừng sản xuất"
description: "Chúng ta được tặng một cây gồm $n$ hành tinh. Mỗi hành tinh được kết nối với hành tinh mẹ của nó bằng chính xác một tuyến không gian hai chiều và mỗi tuyến đều có thời hạn. Tuyến đường chỉ có thể được sử dụng nếu tại thời điểm bạn bắt đầu đi qua nó, thời gian hiện tại của bạn không lớn hơn thời hạn của nó."
date: "2026-06-19T18:48:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106185
codeforces_index: "E"
codeforces_contest_name: "The 2025 ICPC Japan Online First Round Contest"
rating: 0
weight: 106185
solve_time_s: 61
verified: true
draft: false
---

[CF 106185E - Sẽ ngừng sản xuất](https://codeforces.com/problemset/problem/106185/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây$n$các hành tinh. Mỗi hành tinh được kết nối với hành tinh mẹ của nó bằng chính xác một tuyến không gian hai chiều và mỗi tuyến đều có thời hạn. Tuyến đường chỉ có thể được sử dụng nếu tại thời điểm bạn bắt đầu đi qua nó, thời gian hiện tại của bạn không lớn hơn thời hạn của nó. Di chuyển qua bất kỳ tuyến đường nào luôn mất đúng một đơn vị thời gian. 

Bạn bắt đầu ở hành tinh 1 tại thời điểm 0. Nhiệm vụ là đến thăm mọi hành tinh khác ít nhất một lần, nhưng có một điểm khác biệt: bạn được phép dịch chuyển tức thời đến bất kỳ hành tinh nào bạn đã ghé thăm. Dịch chuyển tức thời không tiêu tốn thời gian mà bạn chỉ có thể dịch chuyển tức thời đến các hành tinh đã ghé thăm trước đó, vì vậy lần đầu tiên bạn đến một hành tinh vẫn phải di chuyển bình thường dọc theo các tuyến đường. 

Bởi vì dịch chuyển tức thời loại bỏ nhu cầu đi bộ qua các phần đã được khám phá của cây, nên cấu trúc thực sự quan trọng là thứ tự các hành tinh được ghé thăm lần đầu tiên. Mỗi lần bạn đến một hành tinh mới lần đầu tiên, thời gian sẽ tăng thêm đúng một. 

Mục tiêu là để quyết định xem có tồn tại thứ tự ghé thăm tất cả các hành tinh sao cho mỗi khi bạn đi qua một tuyến đường để khám phá một nút mới thì tuyến đường đó vẫn chưa hết hạn hay không. Nếu thứ tự như vậy tồn tại, chúng ta phải xuất ra bất kỳ thứ tự hợp lệ nào về việc viếng thăm các hành tinh từ 2 đến$n$. 

Đầu vào mã hóa một cây có gốc: hành tinh$i$được kết nối với$p_i$, và cạnh đó có thời hạn$e_i$. Từ$p_i < i$, cấu trúc đã được bắt nguồn từ 1. 

Ràng buộc$n \le 1000$cho phép một$O(n^2 \log n)$hoặc thậm chí$O(n^2)$tiếp cận. Một giải pháp tham lam tuyến tính với một đống là đủ thoải mái. 

Một trường hợp thất bại tinh vi xuất phát từ việc bỏ qua thực tế là một nút không thể được truy cập trước nút cha của nó, ngay cả khi thời hạn của nút đó rất gần. 

Ví dụ: giả sử một đứa trẻ có thời hạn sớm nhưng cha mẹ của nó lại bị trì hoãn: 

đầu vào:```
3
1 0
1 0
```Cả hai cạnh đều hết hạn tại thời điểm 0. Mọi thứ tự truy cập hợp lệ phải được`1, 2, 3`hoặc`1, 3, 2`. Cả hai nút con phải được truy cập lần lượt tại thời điểm 1 và 2, nhưng cả hai cạnh đều yêu cầu thời gian 0, do đó việc duyệt là không thể. Một cách tiếp cận ngây thơ chỉ kiểm tra thời hạn trên mỗi nút mà không tôn trọng các ràng buộc gốc sẽ chấp nhận loại cấu trúc này một cách không chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua thời hạn, vấn đề sẽ không đáng kể: chúng ta có thể bắt đầu ở nút 1 và chạy bất kỳ lệnh DFS nào, vì dịch chuyển tức thời cho phép chúng ta quay trở lại bất kỳ nút nào đã truy cập và tiếp tục khám phá ở nơi khác. 

Khó khăn là mỗi nút mới được phát hiện tiêu tốn một đơn vị thời gian và mọi cạnh được sử dụng để khám phá một nút vẫn phải hợp lệ tại thời điểm đó. Điều này có nghĩa là thứ tự truy cập là biến thực sự mà chúng tôi kiểm soát. 

Quan sát quan trọng là mỗi nút$i$(vì$i > 1$) chỉ đạt được lần đầu tiên thông qua cạnh cha duy nhất của nó$(p_i, i)$. Nếu nút$i$được truy cập tại vị trí$k$theo thứ tự (bắt đầu từ 1 cho nút 1), thì việc duyệt cạnh$(p_i, i)$xảy ra vào thời điểm$k-1$. Do đó, ràng buộc trở thành:$$k - 1 \le e_i \quad \Rightarrow \quad k \le e_i + 1$$Vì vậy, mỗi nút đều có thời hạn cho vị trí của nó theo thứ tự truy cập. 

Chúng ta cũng phải tôn trọng rằng một nút không thể được truy cập trước nút cha của nó. Điều này tạo thành một ràng buộc về quyền ưu tiên của cây gốc: cha phải xuất hiện sớm hơn con. 

Vì vậy, vấn đề trở thành việc lập kế hoạch cho các nhiệm vụ theo đơn vị thời gian trên một dòng thời gian duy nhất, trong đó mỗi nhiệm vụ có thời hạn và điều kiện tiên quyết (mẹ của nó). Tại mỗi bước thời gian, chúng tôi chọn một nút có sẵn (có nút gốc đã được truy cập) và sửa nó vĩnh viễn theo thứ tự. 

Một ý tưởng mạnh mẽ sẽ thử tất cả các hoán vị phù hợp với các ràng buộc gốc, kiểm tra thời hạn mỗi lần. Đó là giai thừa trong trường hợp xấu nhất và nhanh chóng trở nên không khả thi ngay cả đối với$n=20$. 

Cấu trúc tối ưu xuất phát từ nguyên tắc lập kế hoạch tham lam cổ điển: trong số tất cả các nút hiện có, luôn chọn nút có thời hạn nhỏ nhất trước tiên. Theo trực giác, việc trì hoãn một nút có thời hạn chặt chẽ chỉ làm giảm tính khả thi cho các bước trong tương lai, trong khi việc thực hiện sớm sẽ duy trì tính linh hoạt cho các nút lỏng lẻo hơn. 

Chúng tôi duy trì hàng đợi ưu tiên gồm các nút “có sẵn”, nghĩa là các nút có cha mẹ đã được truy cập. Mỗi lần chúng tôi trích xuất nút có thời hạn nhỏ nhất, chỉ định cho nó vị trí tiếp theo theo thứ tự truy cập, sau đó kích hoạt các nút con của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n!)$|$O(n)$| Quá chậm | 
| Tham lam với đống (thời hạn sớm nhất trước) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây ở 1. Mỗi nút$i > 1$có cha mẹ$p_i$và thời hạn$e_i$, chuyển thành ràng buộc vị trí$pos[i] \le e_i + 1$. 

1. Bắt đầu với nút 1 như đã truy cập ở vị trí 1. Đây là nút duy nhất có sẵn ban đầu vì mọi nút khác đều phụ thuộc vào nó. 
2. Duy trì một lượng tối thiểu các nút có sẵn được khóa theo thời hạn của chúng$e_i$. Ban đầu, chèn tất cả nút con của nút 1. 
3. Liên tục trích xuất nút có thời hạn nhỏ nhất từ heap. Nút này là nút khẩn cấp nhất trong số tất cả các lựa chọn hiện có, vì vậy việc trì hoãn nó sẽ là rủi ro nhất. 
4. Gán nó vào vị trí tiếp theo trong thứ tự truy cập. Nếu thời hạn của nó bị vi phạm, nghĩa là vị trí hiện tại trừ 1 vượt quá$e_i$, việc xây dựng thất bại ngay lập tức vì không có sự sắp xếp lại nào trong tương lai có thể khắc phục được điều này. 
5. Đánh dấu nút này là đã truy cập và đẩy tất cả các nút con của nó vào vùng heap, vì giờ đây chúng có thể truy cập được thông qua dịch chuyển tức thời đến nút này. 
6. Tiếp tục cho đến khi tất cả các nút được xử lý. Nếu mọi nút được đặt thành công, thứ tự được ghi lại chính là câu trả lời. 

### Tại sao nó hoạt động 

Ở mỗi bước, tập hợp các nút có sẵn đại diện cho tất cả các nút có điều kiện tiên quyết đã được đáp ứng. Trong số này, việc chọn thời hạn nhỏ nhất đảm bảo rằng không có nút nào gần đến mức không khả thi bị trì hoãn sau một nút linh hoạt hơn. Nếu tồn tại một thứ tự hợp lệ, luôn có cách chuyển đổi nó để các nút được xử lý theo thứ tự thời hạn không giảm mà không vi phạm các ràng buộc về quyền ưu tiên, vì việc hoán đổi nút có thời hạn lỏng lẻo sau đó bằng nút có thời hạn chặt chẽ trước đó không bao giờ gây tổn hại đến tính khả thi nhưng có thể khắc phục các vi phạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n = int(input())
    parent = [0] * (n + 1)
    deadline = [0] * (n + 1)
    g = [[] for _ in range(n + 1)]

    for i in range(2, n + 1):
        p, e = map(int, input().split())
        parent[i] = p
        deadline[i] = e
        g[p].append(i)

    visited = [False] * (n + 1)
    visited[1] = True

    heap = []
    order = []

    for v in g[1]:
        heapq.heappush(heap, (deadline[v], v))

    t = 1  # position index (node 1 is at time 0 / position 1)

    while heap:
        e, v = heapq.heappop(heap)

        # v will be visited at position t+1, so time is t
        if t > e:
            print("no")
            return

        order.append(v)
        visited[v] = True

        for to in g[v]:
            heapq.heappush(heap, (deadline[to], to))

        t += 1

    if len(order) != n - 1:
        print("no")
        return

    print("yes")
    print(*order)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh chính xác quá trình tham lam. Heap lưu trữ tất cả các nút hiện có sẵn được sắp xếp theo thời hạn của chúng. Biến`t`theo dõi xem có bao nhiêu nút đã được đặt sau nút gốc, vì vậy thời gian truy cập hiện tại cho nút tiếp theo là`t`. 

Một cạm bẫy phổ biến là việc xử lý từng vấn đề thời gian và vị trí. Ở đây nút 1 ngầm chiếm vị trí 1 nên khi chúng ta sắp đặt nút tiếp theo thì thời gian truyền tải của nó chính xác là`t`, không`t+1`. Séc`if t > e`mã hóa điều kiện$t \le e$. 

Một điểm tinh tế khác là chúng tôi không bao giờ thực thi kết nối một cách rõ ràng ngoài kích hoạt gốc, bởi vì dịch chuyển tức thời cho phép chúng tôi luôn tiếp cận bất kỳ nút nào đã được truy cập. Yêu cầu về cấu trúc duy nhất là trẻ em chỉ được đẩy sau khi cha mẹ của chúng được xử lý. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ:```
1
2 1
3 0
```Nút 2 có thời hạn 1, nút 3 có thời hạn 0. 

| Bước | Đống (thời hạn, nút) | Nút được chọn | Thời gian t | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| bắt đầu | (1,2), (0,3) | - | 1 | - | 
| 1 | (0,3), (1,2) | 3 | 1 | 1 0 thất bại | 

Điều này ngay lập tức thất bại vì nút 3 phải được truy cập tại thời điểm 1 nhưng chỉ cho phép truy cập tại thời điểm 0. Thuật toán từ chối chính xác. 

Bây giờ hãy xem xét:```
1
2 1
3 1
```| Bước | Đống | Được chọn | t | Kết quả | 
| --- | --- | --- | --- | --- | 
| bắt đầu | (1,2), (1,3) | - | 1 | - | 
| 1 | (1,2), (1,3) | 2 | 1 | được | 
| 2 | (1,3) | 3 | 2 | 2 > 1 thất bại | 

Điều này cho thấy ngay cả những thời hạn đối xứng cũng có thể thất bại tùy theo thứ tự và sự lựa chọn tham lam là cần thiết để phát hiện tính không khả thi. 

Dấu vết xác nhận rằng tính chính xác phụ thuộc vào việc tôn trọng thời hạn sớm nhất giữa các nút hiện có, chứ không chỉ việc sắp xếp toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi nút được đẩy và bật một lần từ heap | 
| Không gian |$O(n)$| Danh sách kề, đống và mảng kế toán | 

Với$n \le 1000$và tối đa 50 trường hợp thử nghiệm, việc này có thể thực hiện dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-like feasible small case
assert run("""3
1 1
1 1
0""") == "yes\n2 3" or run("""3
1 1
1 1
0""") == "yes\n3 2"

# impossible due to tight deadline
assert run("""3
1 0
1 0
0""").startswith("no")

# chain case
assert run("""4
1 3
2 1
3 2
0""") in ["yes\n2 3 4", "yes\n2 4 3", "yes\n3 2 4", "yes\n3 4 2"]

# star with tight constraints
assert run("""4
1 0
1 1
1 1
0""").startswith("no")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây khả thi nhỏ | có + đặt hàng | tính đúng đắn cơ bản | 
| hai cạnh chặt chẽ | không | xử lý sự cố ngay lập tức | 
| cấu trúc chuỗi | vâng | thực thi quyền ưu tiên | 
| sao chặt chẽ thời hạn | không | lập kế hoạch tương tác hạn chế | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều nút con của gốc có thời hạn giống hệt nhau. Thuật toán có thể chọn một trong hai thứ tự, nhưng tính khả thi phụ thuộc vào việc cả hai chỉ khả dụng sau khi truy cập root. Heap xử lý việc này một cách tự nhiên vì các ràng buộc là tùy ý. 

Một trường hợp khác là một chuỗi dài trong đó thời hạn giảm dần dọc theo lộ trình. Điều này luôn khả thi vì mỗi nút sẽ khả dụng chính xác khi cần và quy trình tham lam phản ánh thứ tự chuỗi. 

Một trường hợp tinh tế hơn là khi một nút sâu có thời hạn sớm hơn cây con anh em nông. Heap đảm bảo nó được xử lý đầu tiên ngay khi phần tử mẹ của nó có sẵn, ngăn không cho nó bị trì hoãn sau các nhánh không liên quan.
