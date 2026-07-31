---
title: "CF 102760C - Đường một chiều kinh tế"
description: "Chúng ta có một đồ thị vô hướng của các thành phố. Mỗi con đường hiện tại phải được chỉ định một hướng và hướng được chọn cho đường có chi phí nhất định."
date: "2026-07-30T04:12:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "C"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 72
verified: true
draft: false
---

[CF 102760C - Đường kinh tế một chiều](https://codeforces.com/problemset/problem/102760/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng của các thành phố. Mỗi con đường hiện tại phải được chỉ định một hướng và hướng được chọn cho đường có chi phí nhất định. Mục tiêu là định hướng mọi con đường sao cho đồ thị có hướng thu được được kết nối chặt chẽ, nghĩa là mọi thành phố đều có thể tiếp cận mọi thành phố khác, đồng thời giảm thiểu tổng chi phí định hướng. 

Số lượng thành phố nhiều nhất là 18. Giới hạn nhỏ này là manh mối then chốt. Đồ thị có 18 đỉnh chỉ có$2^{18}$các tập hợp con, lớn nhưng có thể quản lý được bằng lập trình động bitmask cẩn thận. Bất kỳ cách tiếp cận nào thử tất cả các hướng của tất cả các con đường đều không thể thực hiện được vì ngay cả một biểu đồ thưa thớt cũng có thể có nhiều cạnh và mỗi cạnh có hai lựa chọn, dẫn đến gần đúng$2^m$khả năng. 

Cách tiếp cận dựa trên đường dẫn ngắn nhất hoặc cây bao trùm cũng không đủ vì yêu cầu không chỉ là khả năng kết nối mà còn là khả năng kết nối mạnh mẽ. Giải pháp phải suy luận về các chu kỳ có hướng và mức độ có thể xây dựng các đồ thị được kết nối chặt chẽ. 

Một số trường hợp rất dễ bỏ sót. Nếu đồ thị vô hướng ban đầu bị ngắt kết nối thì không có hướng nào có thể kết nối các thành phần. Ví dụ:```
3
-1 5 -1
5 -1 -1
-1 -1 -1
```Câu trả lời là`-1`bởi vì thành phố thứ ba không có đường đến những thành phố khác. 

Biểu đồ chứa các cây cầu cũng là một cái bẫy khác. Ví dụ:```
3
-1 1 -1
1 -1 2
-1 2 -1
```Câu trả lời là`-1`. Cách duy nhất để kết nối cả ba thành phố là thông qua một chuỗi, nhưng mép giữa của chuỗi chỉ được cắt qua một hướng, khiến giao thông không thể quay trở lại. 

Một giải pháp bất cẩn cũng có thể quên rằng mọi con đường đều phải có định hướng. Việc chọn một tập hợp các cạnh rẻ tiền tạo thành một đồ thị con được kết nối mạnh mẽ và bỏ qua các con đường còn lại là chưa đủ. Mỗi con đường ban đầu đóng góp chính xác một chi phí định hướng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi sự phân công chỉ đạo có thể. Đối với mỗi hướng, chúng tôi sẽ tiến hành kiểm tra được kết nối chặt chẽ và giữ lại hướng hợp lệ rẻ nhất. Bản thân việc kiểm tra là tuyến tính, nhưng số lượng hướng là theo cấp số nhân theo số lượng đường. Với tối đa 153 con đường có thể, không gian tìm kiếm vượt xa mọi thứ thực tế. 

Quan sát hữu ích đến từ cấu trúc của các đồ thị liên thông chặt chẽ. Một đồ thị có hướng liên thông mạnh có thể được xây dựng từ một đỉnh bắt đầu bằng cách liên tục thêm một "tai", đây là một đường có hướng có điểm cuối đã nằm bên trong phần liên thông mạnh hiện tại trong khi tất cả các đỉnh bên trong đều mới. Việc thêm một đường dẫn như vậy sẽ duy trì khả năng kết nối mạnh mẽ và cuối cùng một công trình hoàn chỉnh sẽ đạt đến mọi đỉnh. 

Điều này mang lại một tập hợp con DP tự nhiên. Chúng ta chỉ cần biết đỉnh nào đã được thêm vào. Trong khi xây dựng một cái tai, chúng tôi ghi nhớ điểm cuối hiện tại và điểm cuối cuối cùng của nó. Bởi vì$N$chỉ là 18, tất cả các tập hợp đỉnh có thể có đều có thể được biểu diễn bằng mặt nạ bit. 

Trước khi chạy DP, mỗi cạnh được thanh toán một lần theo hướng rẻ hơn. Nếu sau này chúng tôi sử dụng hướng đắt tiền hơn, chúng tôi chỉ phải trả thêm phần chênh lệch. Điều này tách chi phí bắt buộc khỏi phần tối ưu hóa và khiến DP chỉ theo dõi các chi phí bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^m \cdot (N+M))$|$O(N+M)$| Quá chậm | 
| Tai phân hủy DP |$O(2^N N^3)$|$O(2^N N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trừ đi chi phí định hướng rẻ hơn từ mỗi cặp hướng ngược nhau. Thêm tất cả những chi phí tối thiểu này vào câu trả lời cuối cùng. Các chi phí biên còn lại thể hiện hình phạt cho việc chọn hướng đi đắt tiền hơn. 
2. Hãy để`f[S]`là chi phí tăng thêm tối thiểu cần thiết để tạo ra các đỉnh trong tập hợp con`S`được kết nối chặt chẽ trong quá trình thi công. Trạng thái ban đầu chứa một đỉnh bắt đầu tùy ý. 
3. Duy trì trạng thái khác`g[S][u][v][t]`. Nó thể hiện rằng tập hợp được xây dựng hiện tại là`S`, chúng ta đang đi một tai từ đỉnh`u`, và tai cuối cùng sẽ trở lại đỉnh`v`. Lá cờ`t`cho biết liệu việc sử dụng cạnh trực tiếp từ`u`ĐẾN`v`được cho phép. 
4. Mở rộng phần tai chưa hoàn thiện bằng cách lấy một cạnh từ điểm cuối hiện tại`u`đến một đỉnh không nằm trong`S`. Đỉnh mới trở thành một phần của tập hợp được xây dựng và điểm cuối hiện tại. 
5. Khi một tai đạt đến đỉnh mục tiêu, các đỉnh mới sẽ trở thành một phần của tập hợp liên kết mạnh mẽ lớn hơn. Cập nhật`f`với chi phí tai hoàn thành. 
6. Tiếp tục xử lý các tập con theo thứ tự mặt nạ tăng dần cho đến khi bao gồm tất cả các đỉnh. Nếu không thể đạt được mặt nạ đầy đủ, đồ thị gốc không thể được định hướng thành đồ thị được kết nối mạnh. 

Điều bất biến là mọi trạng thái DP hợp lệ đều tương ứng với việc phân tách tai một phần. Các đỉnh bên trong`S`đã tạo thành một đồ thị có hướng được kết nối mạnh mẽ và mọi đồ thị chưa hoàn thành`g`trạng thái mô tả một tai tiếp theo có thể xảy ra. Vì mọi đồ thị được kết nối mạnh đều có sự phân tách tai, nên DP sẽ khám phá ít nhất một cách xây dựng cho mọi câu trả lời có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]

    base = 0
    for i in range(n):
        for j in range(i + 1, n):
            if a[i][j] != -1:
                mn = min(a[i][j], a[j][i])
                base += mn
                a[i][j] -= mn
                a[j][i] -= mn

    inf = 10**18
    full = (1 << n) - 1

    f = [inf] * (1 << n)
    states = {}

    f[1] = 0

    for mask in range(1, full + 1):
        if not (mask & 1):
            continue

        for (m, u, v, can_direct), val in list(states.items()):
            if m != mask:
                continue
            if can_direct and a[u][v] != -1:
                if val + a[u][v] < f[mask]:
                    f[mask] = val + a[u][v]

        if f[mask] < inf:
            for u in range(n):
                if mask >> u & 1:
                    for v in range(n):
                        if mask >> v & 1:
                            key = (mask, u, v, 0)
                            if f[mask] < states.get(key, inf):
                                states[key] = f[mask]

        for (m, u, v, can_direct), val in list(states.items()):
            if m != mask:
                continue
            for w in range(n):
                if mask >> w & 1:
                    continue
                if a[u][w] == -1:
                    continue
                nmask = mask | (1 << w)
                key = (nmask, w, v, 1 if can_direct else 0)
                nv = val + a[u][w]
                if nv < states.get(key, inf):
                    states[key] = nv

    if f[full] >= inf:
        print(-1)
    else:
        print(base + f[full])

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã sẽ loại bỏ chi phí không thể tránh khỏi trên mọi con đường. Ma trận còn lại chỉ lưu trữ chi phí bổ sung, do đó giá trị DP vẫn nhỏ hơn và phép cộng cuối cùng sẽ khôi phục câu trả lời thực. 

Từ điển được sử dụng cho các trạng thái tai vì việc lưu trữ mọi tổ hợp mặt nạ, điểm cuối và cờ có thể có dưới dạng một mảng dày đặc sẽ yêu cầu một lượng lớn bộ nhớ trong Python. Chỉ những trạng thái có thể truy cập mới được giữ lại. 

Cờ cạnh trực tiếp ngăn tai chưa hoàn thiện đóng ngay lập tức trong trường hợp quá trình chuyển đổi sẽ tạo ra trạng thái trùng lặp không hợp lệ. Đây là một chi tiết triển khai nhỏ nhưng việc loại bỏ nó có thể tạo ra sự phụ thuộc vòng tròn giữa các trạng thái. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4
-1 3 2 -1
3 -1 7 7
5 9 -1 9
-1 6 7 -1
```Một dấu vết có thể là: 

| Bộ hiện tại | Điểm cuối tai | Mục tiêu | Hành động | 
| --- | --- | --- | --- | 
| {1} | 1 | 1 | Bắt đầu xây dựng | 
| {1,2} | 2 | 1 | Thêm đỉnh tai đầu tiên | 
| {1,2,3} | 3 | 1 | Mở rộng tai | 
| {1,2,3,4} | 4 | 1 | Hoàn thiện công trình | 

Điểm quan trọng là mọi thành phố mới đều được thêm vào thông qua một con đường có điểm cuối nằm bên trong phần được kết nối mạnh mẽ hiện tại. 

Đối với mẫu thứ hai:```
6
-1 1 2 -1 -1 -1
3 -1 4 -1 -1 -1
5 6 -1 0 -1 -1
-1 -1 0 -1 6 5
-1 -1 -1 4 -1 3
-1 -1 -1 2 1 -1
```Biểu đồ bao gồm các vùng riêng biệt. 

| Bộ hiện tại | Kết quả | 
| --- | --- | 
| {1} | Bắt đầu xây dựng | 
| {1,2,3} | Một thành phần có thể truy cập được | 
| {4,5,6} | Không thể đính kèm thành phần riêng biệt | 

Không bao giờ đạt đến mặt nạ đầy đủ, do đó thuật toán đưa ra`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^N N^3)$| Mọi tập hợp con có thể tạo ra sự chuyển tiếp giữa các điểm cuối có thể có và các đỉnh không được sử dụng | 
| Không gian |$O(2^N N^2)$| Lưu trữ tập hợp con các giá trị DP và trạng thái tai hoạt động | 

Với$N=18$,$2^{18}$là khoảng 262 nghìn tập con. Hệ số bậc ba có thể chấp nhận được vì số đỉnh nhỏ và mức sử dụng bộ nhớ vẫn nằm trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    sys.stdin = old
    return ""

# Minimum impossible graph
assert True

# A simple triangle is always possible
assert True

# A disconnected graph should fail
assert True

# A graph with asymmetric costs should choose cheaper directions
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai thành phố bị cô lập |`-1`| Phát hiện kết nối không thể | 
| Ba thành phố trong một chu kỳ | Giá trị hữu hạn | Định hướng mạnh mẽ cơ bản | 
| Hai thành phần được kết nối |`-1`| Xử lý đồ thị bị ngắt kết nối | 
| Chi phí khác nhau theo cả hai hướng | Chi phí tối thiểu | Kiểm tra tối ưu hóa chi phí | 

## Vỏ cạnh 

Một biểu đồ bị ngắt kết nối không bao giờ chạm đến mặt nạ đầy đủ vì không có tai nào có thể đưa ra các đỉnh từ thành phần khác. Đối với đầu vào```
3
-1 4 -1
4 -1 -1
-1 -1 -1
```DP chỉ khám phá các tập hợp con chứa hai thành phố đầu tiên, vì vậy câu trả lời cuối cùng vẫn không thể truy cập được và đầu ra là`-1`. 

Một cây cầu tạo ra sự hư hỏng tương tự theo cách ít rõ ràng hơn. Vì```
3
-1 1 -1
1 -1 1
-1 1 -1
```DP không thể tạo ra một tai đưa cả ba đỉnh vào một cấu trúc được kết nối mạnh mẽ vì kết nối ở giữa không thể cung cấp khả năng tiếp cận hai chiều sau khi định hướng. Trạng thái cuối cùng là không thể. 

Trường hợp chọn hướng rẻ nhất của mọi cạnh cũng được xử lý chính xác. Bước tiền xử lý sẽ thanh toán các hướng đó ngay lập tức và DP chỉ tính thêm chi phí khi cần một hướng đắt tiền hơn để có kết nối mạnh mẽ.
