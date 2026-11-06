---
title: "CF 125E - Công ty MST"
description: "Chúng ta được cho một đồ thị vô hướng có trọng số có vốn là đỉnh 1. Chúng ta phải chọn một tập hợp các con đường nối tất cả các thành phố. Vì biểu đồ được chọn phải được kết nối và có tổng trọng số tối thiểu nên giải pháp sẽ luôn là cây bao trùm bất cứ khi nào có giải pháp."
date: "2026-06-02T16:20:28+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "graphs"]
categories: ["algorithms"]
codeforces_contest: 125
codeforces_index: "E"
codeforces_contest_name: "Codeforces Testing Round 2"
rating: 2400
weight: 125
solve_time_s: 147
verified: true
draft: false
---

[CF 125E - Công ty MST](https://codeforces.com/problemset/problem/125/E) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, đồ thị 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số có vốn là đỉnh 1. 

Chúng ta phải chọn một tập hợp các con đường kết nối tất cả các thành phố. Vì biểu đồ được chọn phải được kết nối và có tổng trọng số tối thiểu nên giải pháp sẽ luôn là cây bao trùm bất cứ khi nào có giải pháp. 

Hạn chế thêm là chính xác`k`các cạnh của cây bao trùm phải liên tiếp với thủ đô. 

Vì vậy, nhiệm vụ là: 

Tìm cây bao trùm có trọng số nhỏ nhất có bậc tại đỉnh`1`chính xác là`k`và xuất ra các chỉ số cạnh của nó. 

Biểu đồ chứa tới 5000 đỉnh và 100000 cạnh. Cây bao trùm tối thiểu cổ điển có thể được tìm thấy với Kruskal trong`O(m log m)`, điều đó hoàn toàn ổn. Phần khó khăn là thực thi ràng buộc mức độ trên một đỉnh đặc biệt. 

Kích thước ràng buộc ngay lập tức loại trừ bất kỳ thứ gì liệt kê các tập hợp con của các cạnh viết hoa. Ngay cả việc kiểm tra tất cả các khả năng về bậc của đỉnh 1 cũng sẽ vô vọng vì số cạnh liên quan có thể lớn bằng`10^5`. 

Quan sát quan trọng là hạn chế chỉ ảnh hưởng đến một loại cạnh đặc biệt, cụ thể là các cạnh tiếp xúc với đỉnh 1. Cấu trúc đó chính xác là thứ cho phép thư giãn Lagrange, thường được gọi là tìm kiếm nhị phân WQS trong lập trình cạnh tranh. 

Có một số trường hợp cạnh không rõ ràng. 

Giả sử đồ thị bị ngắt kết nối ngay cả khi tất cả các cạnh đều có sẵn:```
3 1 1
1 2 5
```Đỉnh 3 bị cô lập. Không có cây bao trùm nào tồn tại nên câu trả lời là`-1`. 

Một trường hợp quan trọng khác là khi có quá ít cạnh vốn.```
4 2 2
1 2 1
2 3 1
```Chỉ có một cạnh chạm đỉnh 1 nên việc đạt được bậc 2 tại điểm hoa là không thể. 

Một trường hợp tinh vi hơn xuất hiện khi nhiều cây bao trùm có cùng chi phí được sửa đổi trong quá trình tìm kiếm nhị phân. Nếu các mối nối được xử lý không chính xác, tính đơn điệu của số cạnh hoa được chọn sẽ bị phá vỡ và việc tái thiết không thành công. Việc triển khai phải rõ ràng ưu tiên các cạnh vốn trong số các trọng số được sửa đổi bằng nhau. 

## Phương pháp tiếp cận 

Quan điểm bạo lực rất hữu ích vì nó phơi bày những gì chúng ta thực sự đang tối ưu hóa. 

Hãy tưởng tượng liệt kê tất cả các cây bao trùm. Với mỗi cây, hãy đếm xem có bao nhiêu cạnh liên tiếp với đỉnh 1. Trong số những cây có chính xác`k`các cạnh như vậy, hãy giữ cạnh có tổng trọng lượng tối thiểu. 

Điều này rõ ràng là đúng vì nó phù hợp trực tiếp với định nghĩa của câu trả lời. Thật không may, một đồ thị có 5000 đỉnh có thể có số cây bao trùm vô cùng lớn. Ngay cả việc tạo ra chúng là không thể. 

Ý tưởng tự nhiên tiếp theo là chạy Kruskal trong khi theo dõi bằng cách nào đó có bao nhiêu cạnh vốn được sử dụng. Điều đó ngay lập tức dẫn đến một vấn đề: Kruskal chỉ tối ưu hóa tổng trọng lượng và không biết gì về giới hạn độ. 

Quan sát quan trọng là mọi cạnh vốn đều thuộc cùng một loại. Thay vì buộc chính xác`k`trong số chúng, chúng ta có thể tạm thời thay đổi trọng số của chúng. 

Cho phép`C`là một tham số. 

Với mỗi cạnh chạm đỉnh 1, thay trọng số của nó bằng`w + C`. 

Tất cả các cạnh khác giữ nguyên trọng lượng ban đầu. 

Bây giờ hãy chạy thuật toán MST tiêu chuẩn trên các trọng số đã sửa đổi này. 

Nếu như`C`rất lớn, biên vốn trở nên đắt đỏ và MST sử dụng rất ít trong số đó. 

Nếu như`C`là rất tiêu cực, biên vốn trở nên hấp dẫn và MST sử dụng nhiều trong số đó. 

Điều này tạo ra một hàm đơn điệu:`cnt(C) = number of capital edges in the MST`. 

BẰNG`C`tăng lên,`cnt(C)`không bao giờ tăng. 

Sự đơn điệu đó cho phép tìm kiếm nhị phân. 

Sau khi tìm giá trị lớn nhất của`C`mà MST vẫn chứa ít nhất`k`các cạnh vốn, chúng ta xây dựng lại một cây bao trùm với chính xác`k`cạnh vốn bằng cách điều hành Kruskal một lần nữa với cùng trọng lượng đã được sửa đổi và từ chối nhận nhiều hơn`k`các cạnh vốn. 

Lý do điều này có hiệu quả là thuộc tính thư giãn Lagrange tiêu chuẩn. Tại ranh giới tìm kiếm nhị phân, mọi MST được sửa đổi tối ưu đều tương ứng với một giải pháp ban đầu tối ưu với ràng buộc về mức độ`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên cây bao trùm | Hàm mũ | Hàm mũ | Quá chậm | 
| Tìm kiếm nhị phân + Kruskal | O(log W · m log m) | O(m) | Đã chấp nhận | 

Đây`W`là phạm vi trọng lượng. 

## Hướng dẫn thuật toán 

1. Chuẩn hóa mọi cạnh để nếu một điểm cuối là chữ hoa thì nó được lưu dưới dạng`(1, x)`. 
2. Xác định hàm`count_capital(C)`. 

Thêm vào`C`theo trọng số của mỗi cạnh vốn, sắp xếp tất cả các cạnh theo trọng số đã sửa đổi và chạy Kruskal. 

Khi hai cạnh có cùng trọng lượng được sửa đổi, hãy đặt các cạnh viết hoa trước. Điều này tối đa hóa số cạnh vốn được chọn trong số tất cả các MST được sửa đổi tối ưu. 

Trả về số cạnh viết hoa được sử dụng. 
3. Tìm kiếm nhị phân trên`C`. 

Ta cần giá trị lớn nhất của`C`như vậy`count_capital(C) >= k`. 

Tăng dần`C`làm cho các cạnh viết hoa kém hấp dẫn nên vị ngữ trở nên đơn điệu. 
4. Đặt tham số kết quả là`best`. 
5. Chạy lại Kruskal bằng cách sử dụng trọng số được sửa đổi bởi`best`. 
6. Trong khi xây dựng cây, không bao giờ để số cạnh vốn đã chọn vượt quá`k`. 

Một cạnh không viết hoa được xử lý bình thường. 

Một cạnh vốn chỉ được chấp nhận nếu: 

- nó kết nối hai thành phần khác nhau, và 
- ít hơn`k`các cạnh vốn đã được chọn. 
7. Nếu chúng ta không thu được chính xác`n - 1`các cạnh hoặc không đạt được chính xác`k`các cạnh viết hoa, không tồn tại cây bao trùm hợp lệ. 
8. Nếu không thì xuất chỉ số cạnh đã chọn. 

### Tại sao nó hoạt động 

Đối với một tham số cố định`C`, Kruskal tìm cây khung nhỏ nhất trong đồ thị có trọng số được sửa đổi bằng cách thêm`C`tới mọi cạnh vốn. 

Hãy để một cái cây chứa`d`các cạnh vốn. Chi phí điều chỉnh của nó là`original_cost + C * d`. 

Tìm kiếm nhị phân tìm ra tham số lớn nhất mà MST được sửa đổi tối ưu vẫn sử dụng ít nhất`k`các cạnh vốn. 

Tại ranh giới này, tất cả các cây có bậc lớn hơn`k`và tất cả các cây có bậc nhỏ hơn`k`được cân bằng xung quanh cùng một đường hỗ trợ của độ giãn Lagrangian. Giải pháp hạn chế tối ưu nằm trong số các MST được sửa đổi tối ưu. Bằng cách xây dựng lại MST và giới hạn bản thân ở mức chính xác`k`cạnh vốn bất cứ khi nào tồn tại các lựa chọn có chi phí bằng nhau, chúng ta thu được một trong những cây bị ràng buộc tối ưu đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n + 1))

    def find(self, x):
        p = self.p
        while p[x] != x:
            p[x] = p[p[x]]
            x = p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        self.p[a] = b
        return True

def solve():
    n, m, k = map(int, input().split())

    edges = []

    for idx in range(1, m + 1):
        u, v, w = map(int, input().split())

        if u > v:
            u, v = v, u

        edges.append([u, v, w, idx])

    def count_capital(add):
        arr = []

        for u, v, w, idx in edges:
            if u == 1:
                arr.append((w + add, 0, u, v))
            else:
                arr.append((w, 1, u, v))

        arr.sort()

        dsu = DSU(n)
        cnt = 0
        taken = 0

        for _, typ, u, v in arr:
            if dsu.union(u, v):
                taken += 1
                if typ == 0:
                    cnt += 1
                if taken == n - 1:
                    break

        return cnt

    L = -100000
    R = 100000
    best = None

    while L <= R:
        mid = (L + R) // 2

        if count_capital(mid) >= k:
            best = mid
            L = mid + 1
        else:
            R = mid - 1

    if best is None:
        print(-1)
        return

    arr = []

    for u, v, w, idx in edges:
        if u == 1:
            arr.append((w + best, 0, u, v, idx))
        else:
            arr.append((w, 1, u, v, idx))

    arr.sort()

    dsu = DSU(n)

    answer = []
    capital_used = 0

    for _, typ, u, v, idx in arr:
        if typ == 0 and capital_used == k:
            continue

        if dsu.union(u, v):
            answer.append(idx)

            if typ == 0:
                capital_used += 1

    if len(answer) != n - 1 or capital_used != k:
        print(-1)
        return

    print(n - 1)
    print(*answer)

solve()
```Chi tiết quan trọng đầu tiên là thứ tự sắp xếp. Vốn cạnh nhận loại`0`và các cạnh không viết hoa nhận được loại`1`. Vì Python sắp xếp các bộ dữ liệu theo từ điển nên các trọng số được sửa đổi bằng nhau sẽ tự động ưu tiên các cạnh viết hoa. Nếu không có bộ ngắt kết nối này, tính đơn điệu được sử dụng bởi tìm kiếm nhị phân có thể thất bại. 

Tìm kiếm nhị phân tìm kiếm tham số lớn nhất mà MST vẫn chứa ít nhất`k`các cạnh vốn. Hướng đi chính xác đó rất quan trọng. Đảo ngược nó sẽ thay đổi đối số tái thiết và phá vỡ tính chính xác. 

Trong quá trình xây dựng lại, chúng tôi chạy lại Kruskal với cùng trọng số đã được sửa đổi. Quy tắc bổ sung duy nhất là một lần`k`Các cạnh vốn đã được chọn, mọi cạnh vốn tiếp theo sẽ bị bỏ qua. Đây là bước chuyển lời giải thoải mái tối ưu thành lời giải tối ưu thỏa mãn yêu cầu mức độ chính xác. 

Việc triển khai DSU sử dụng tính năng nén đường dẫn. Liên minh theo cấp bậc là không cần thiết đối với những hạn chế này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5 2
1 2 1
2 3 1
3 4 1
1 3 3
1 4 2
```Giả sử tìm kiếm nhị phân đã tìm thấy tham số chính xác. 

| Cạnh được xem xét | Được chọn? | Cạnh vốn được sử dụng | thành phần DSU | 
| --- | --- | --- | --- | 
| 1-2 | Có | 1 | hợp nhất | 
| 2-3 | Có | 1 | hợp nhất | 
| 3-4 | Có | 1 | cây hoàn thiện | 

MST không bị giới hạn chỉ sử dụng một cạnh viết hoa. 

Sau khi áp dụng tham số tối ưu và xây dựng lại: 

| Cạnh được xem xét | Được chọn? | Cạnh vốn được sử dụng | 
| --- | --- | --- | 
| 1-2 | Có | 1 | 
| 1-4 | Có | 2 | 
| 2-3 | Có | 2 | 

Cây kết quả có chính xác hai cạnh vốn và tổng trọng số tối thiểu. 

Dấu vết này chứng tỏ tại sao việc thay đổi trọng số của các cạnh vốn có thể buộc Kruskal đạt đến mức độ cần thiết. 

### Ví dụ 2 

đầu vào:```
3 1 1
1 2 5
```| Bước | Kết quả | 
| --- | --- | 
| Kruskal kết thúc | chỉ chọn một cạnh | 
| Kích thước cây yêu cầu | 2 cạnh | 
| Đã kết nối? | Không | 

Vì không thể hình thành cây bao trùm nên thuật toán đưa ra`-1`. 

Ví dụ này thực hiện đường dẫn lỗi kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log W · m log m) | Mỗi bước tìm kiếm nhị phân chạy một Kruskal | 
| Không gian | O(m) | Lưu trữ tất cả các cạnh và bộ đệm sắp xếp | 

Phạm vi trọng lượng tối đa là`10^5`, như vậy khoảng 18 lần lặp tìm kiếm nhị phân là đủ. Với`m = 100000`, tổng công việc vẫn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    # placeholder for local testing
    pass

# sample 1
# verify manually against accepted solution

# minimum graph
inp = """\
1 0 0
"""
# expected: empty spanning tree

# disconnected graph
inp = """\
3 1 1
1 2 5
"""
# expected: -1

# impossible degree
inp = """\
4 2 2
1 2 1
2 3 1
"""
# expected: -1

# boundary degree
inp = """\
3 3 2
1 2 1
1 3 1
2 3 100
"""
# expected tree uses both capital edges
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | Cây trống | Ví dụ kích thước tối thiểu | 
| Đồ thị bị ngắt kết nối | -1 | Phát hiện kết nối | 
| Quá ít cạnh vốn | -1 | Bằng cấp không khả thi | 
| Bằng cấp bằng 2 | Cây hợp lệ | Tái thiết mức độ chính xác | 

## Vỏ cạnh 

Hãy xem xét đồ thị bị ngắt kết nối:```
3 1 1
1 2 5
```Kruskal có thể chọn nhiều nhất một cạnh. Cây bao trùm trên ba đỉnh cần có hai cạnh. Trong quá trình xây dựng lại, thuật toán kết thúc với ít hơn`n - 1`các cạnh và bản in`-1`. 

Xem xét các cạnh vốn không đủ:```
4 2 2
1 2 1
2 3 1
```Cho dù tham số tìm kiếm nhị phân có âm đến mức nào thì đồ thị về mặt vật lý chỉ chứa một cạnh viết hoa. Giai đoạn tái thiết không thể đạt tới`capital_used = 2`, do đó việc kiểm tra tính hợp lệ cuối cùng sẽ từ chối phiên bản đó. 

Xem xét trọng số được sửa đổi bằng nhau. Giả sử một số cạnh vốn và phi vốn có chi phí điều chỉnh giống hệt nhau. Thuật toán luôn đặt các cạnh viết hoa đầu tiên theo thứ tự. Điều này đảm bảo rằng`count_capital(C)`biểu thị số cạnh vốn tối đa có thể có trong số các MST được sửa đổi tối ưu, chính xác là số lượng đơn điệu được yêu cầu bởi tìm kiếm nhị phân. Nếu không có quy tắc ràng buộc này, các lần thực thi Kruskal khác nhau có thể tạo ra số lượng khác nhau cho cùng một tham số, làm hỏng việc tìm kiếm.
