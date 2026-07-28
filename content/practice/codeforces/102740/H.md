---
title: "CF 102740H - Máy dò ma của E. Gadd"
description: "Chúng tôi có một hành lang dài một chiều. Một số vị trí chứa thiết bị Zapper và một số vị trí chứa ma. Máy bắn điện có thể loại bỏ bất kỳ bóng ma nào, nhưng chi phí năng lượng phụ thuộc vào bình phương khoảng cách giữa chúng."
date: "2026-07-29T01:01:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102740
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 2"
rating: 0
weight: 102740
solve_time_s: 100
verified: true
draft: false
---

[CF 102740H - Máy dò ma của E. Gadd](https://codeforces.com/problemset/problem/102740/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hành lang dài một chiều. Một số vị trí chứa thiết bị Zapper và một số vị trí chứa ma. Máy bắn điện có thể loại bỏ bất kỳ bóng ma nào, nhưng chi phí năng lượng phụ thuộc vào bình phương khoảng cách giữa chúng. Vì tất cả các zapper có thể được sử dụng độc lập nên mọi bóng ma phải được xử lý bởi bất kỳ zapper nào gần nó nhất. Đầu ra bắt buộc là tổng của các khoảng cách bình phương tối thiểu này cho mỗi bóng ma, lấy theo modulo (10^9+7). 

Đầu vào cung cấp số lượng kẻ tấn công và bóng ma, tiếp theo là vị trí của chúng dọc theo hành lang. Vị trí là số nguyên và có thể lớn bằng (10^7), trong khi cả số lượng máy bắn và bóng ma đều có thể đạt tới (10^5). Với (10^5) bóng ma và (10^5) máy bắn, việc kiểm tra mọi cặp có thể sẽ yêu cầu tính toán khoảng cách (10^{10}), vượt xa giới hạn hai giây cho phép. Chúng tôi cần một giải pháp gần (O((n+m)\log n)) hoặc tốt hơn. 

Các trường hợp cạnh chính xuất phát từ thực tế là máy phát điện gần nhất có thể không phải là máy phát điện đầu tiên gặp phải và khoảng cách có thể bằng không. Ví dụ: nếu đầu vào là:```
1 1
5
5
```câu trả lời là`0`bởi vì hồn ma đã sẵn sàng tấn công. Một giải pháp chỉ kiểm tra những khoảng cách nhỏ hơn có thể vô tình bỏ sót trường hợp này. 

Một trường hợp khác là khi hồn ma nằm trước cú đánh đầu tiên hoặc sau cú đánh cuối cùng:```
2 2
10
20
0
30
```Câu trả lời đúng là`200`, bởi vì con ma ở vị trí`0`là khoảng cách`10`từ cú bắn đầu tiên và con ma ở vị trí`30`là khoảng cách`10`từ zapper thứ hai. Giải pháp chỉ tìm kiếm giữa hai thiết bị kích hoạt sẽ không thành công vì những bóng ma này không có thiết bị kích hoạt ở cả hai bên. 

Trường hợp thứ ba là khi hai máy phát điện gần nhau như nhau:```
2 1
4
10
7
```Câu trả lời là`9`, bởi vì cả hai máy phát điện đều có khoảng cách`3`đi và chi phí là (3^2). Bất kỳ lựa chọn hòa giải nào cũng được miễn là sử dụng khoảng cách tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là so sánh mọi con ma với mọi con ma. Đối với mỗi vị trí ma (x), chúng tôi tính toán ((x-z_i)^2) cho tất cả các vị trí zapper và giữ giá trị nhỏ nhất. Điều này đúng vì các lựa chọn của Zapper là độc lập nên mỗi bản ghost có thể được tối ưu hóa riêng biệt. Tuy nhiên, với (10^5) bóng ma và (10^5) máy dò, trường hợp xấu nhất sẽ thực hiện (10^{10}) so sánh và tính toán khoảng cách, quá chậm. 

Cấu trúc giúp giải bài toán này dễ dàng hơn là mọi vật đều nằm trên một đường thẳng. Sau khi sắp xếp các vị trí của máy bắn, máy phát điện gần con ma nhất phải là một trong hai máy phát điện xung quanh con ma đó. Bất kỳ Zapper nào ở xa bên trái hơn hàng xóm bên trái gần nhất thậm chí còn ở xa hơn và lập luận tương tự cũng áp dụng cho phía bên phải. 

Điều này làm giảm vấn đề tìm kiếm nhị phân. Đối với mỗi con ma, chúng ta tìm người bắn đầu tiên có vị trí ít nhất là vị trí con ma. Zapper đó là ứng cử viên gần nhất ở bên phải. Zapper trước đó trong mảng được sắp xếp là ứng cử viên gần nhất ở bên trái. Chúng tôi tính toán khoảng cách đến cả hai ứng cử viên khi chúng tồn tại và cộng khoảng cách bình phương nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O((n+m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các vị trí của Zapper. Thứ tự sắp xếp cho phép chúng tôi xác định vị trí hàng xóm gần nhất có thể của bất kỳ con ma nào bằng cách sử dụng tìm kiếm nhị phân. 
2. Với mỗi vị trí bóng ma, tìm vị trí chèn của bóng ma đó trong mảng Zapper đã sắp xếp. Chỉ số này trỏ đến zapper đầu tiên có vị trí không nhỏ hơn bóng ma. 
3. Kiểm tra Zapper tại chỉ mục đó nếu nó tồn tại. Đây là zapper gần nhất ở phía bên phải. 
4. Kiểm tra Zapper ngay trước chỉ mục đó nếu nó tồn tại. Đây là zapper gần nhất ở phía bên trái. 
5. Cộng khoảng cách bình phương nhỏ hơn vào đáp án. Lặp lại điều này cho mọi con ma và lấy kết quả theo modulo (10^9+7). 

Lý do chỉ cần kiểm tra hai zapper là thứ tự của dòng. Giả sử có một con ma ở giữa hai máy bắn tia. Di chuyển xa hơn về bên trái so với thiết bị phát hiện bên trái gần nhất chỉ làm tăng khoảng cách và di chuyển xa hơn về bên phải so với thiết bị phát hiện bên phải gần nhất cũng làm được điều tương tự. Lý do tương tự bao gồm cả những bóng ma nằm ngoài phạm vi của tất cả các máy phát điện. 

Tại sao nó hoạt động: sau khi sắp xếp, vị trí tìm kiếm nhị phân sẽ chia tất cả các Zapper thành phần bên trái của bóng ma và phần bên phải của nó. Phần tử gần nhất trong mỗi nhóm là lựa chọn tối ưu duy nhất có thể có. Mọi máy phát điện khác trong cùng một nhóm đều ở xa hơn nên bình phương khoảng cách của nó không thể nhỏ hơn. Vì thuật toán đánh giá cả hai hàng xóm gần nhất có thể nên nó luôn chọn chi phí năng lượng tối thiểu cho mỗi bóng ma. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())

    zappers = []
    for _ in range(n):
        zappers.append(int(input()))

    ghosts = []
    for _ in range(m):
        ghosts.append(int(input()))

    zappers.sort()

    ans = 0

    for x in ghosts:
        idx = bisect_left(zappers, x)

        best = None

        if idx < n:
            d = x - zappers[idx]
            best = d * d

        if idx > 0:
            d = x - zappers[idx - 1]
            cost = d * d
            if best is None or cost < best:
                best = cost

        ans = (ans + best) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Danh sách Zapper được sắp xếp là cấu trúc duy nhất cần được lưu trữ vì các bản ghost có thể được xử lý độc lập. các`bisect_left`call trả về vị trí đầu tiên mà bóng ma có thể được chèn mà không phá vỡ quá trình sắp xếp, điều này cung cấp chính xác vị trí đầu tiên ở bên phải. 

Mã kiểm tra ứng viên bên phải trước ứng viên bên trái, nhưng thứ tự không ảnh hưởng đến tính chính xác. Việc kiểm tra ranh giới là cần thiết vì vị trí chèn có thể bằng 0, nghĩa là không có zapper bên trái hoặc nó có thể bằng`n`, có nghĩa là không có Zapper đúng. 

Khoảng cách bình phương được tính bằng số nguyên. Python tự động xử lý các số nguyên lớn, nhưng lấy modulo sau mỗi phép cộng sẽ giữ cho câu trả lời tích lũy bị giới hạn và tuân theo định dạng đầu ra được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
4 4
4
1
11
7
2
9
6
15
```Sau khi phân loại, Zapper sẽ trở thành`[1, 4, 7, 11]`. 

| Ma | Chỉ mục tìm kiếm nhị phân | Ứng cử viên trái | Ứng viên phù hợp | Giá đã chọn | Tổng số chạy | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 4 | 1 | 1 | 
| 9 | 3 | 7 | 11 | 4 | 5 | 
| 6 | 2 | 4 | 7 | 1 | 6 | 
| 15 | 4 | 11 | không | 16 | 22 | 

Câu trả lời là`22`. Dấu vết cho thấy mỗi hồn ma chỉ cần hai thiết bị điện lân cận sau khi phân loại. 

Đối với ví dụ thứ hai:```
2 4
10
5
7
0
5
100
```Các Zapper được sắp xếp là`[5, 10]`. 

| Ma | Chỉ mục tìm kiếm nhị phân | Ứng cử viên trái | Ứng viên phù hợp | Giá đã chọn | Tổng số chạy | 
| --- | --- | --- | --- | --- | --- | 
| 7 | 1 | 5 | 10 | 4 | 4 | 
| 0 | 0 | không | 5 | 25 | 29 | 
| 5 | 0 | không | 5 | 0 | 29 | 
| 100 | 2 | 10 | không | 8100 | 8129 | 

Câu trả lời là`8129`. Ví dụ này thực hiện cả trường hợp ngoài phạm vi và trường hợp khoảng cách bằng không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log n) | Việc sắp xếp mất O(n log n) và mỗi bản ma thực hiện một tìm kiếm nhị phân | 
| Không gian | O(n) | Chỉ các vị trí zapper được sắp xếp mới được lưu trữ | 

Các ràng buộc cho phép khoảng vài triệu hoạt động, nhưng không phải hàng tỷ. Việc sắp xếp các công cụ tìm kiếm và thực hiện tìm kiếm nhị phân giúp công việc nằm trong phạm vi yêu cầu cho (10^5) vị trí. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_left

MOD = 10**9 + 7

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    zappers = [int(input()) for _ in range(n)]
    ghosts = [int(input()) for _ in range(m)]

    zappers.sort()
    ans = 0

    for x in ghosts:
        idx = bisect_left(zappers, x)
        best = None

        if idx < n:
            d = x - zappers[idx]
            best = d * d

        if idx > 0:
            d = x - zappers[idx - 1]
            cost = d * d
            if best is None or cost < best:
                best = cost

        ans = (ans + best) % MOD

    return str(ans)

assert solve("""4 4
4
1
11
7
2
9
6
15
""") == "22"

assert solve("""2 4
10
5
7
0
5
100
""") == "8129"

assert solve("""1 1
5
5
""") == "0"

assert solve("""2 2
10
20
0
30
""") == "200"

assert solve("""3 3
0
10000000
5000000
5000000
5000001
4999999
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Zapper đơn và ma ở cùng một vị trí | 0 | Xử lý khoảng cách bằng không | 
| Những bóng ma bên ngoài cả hai đầu của phạm vi Zapper | 200 | Các trường hợp tìm kiếm nhị phân ranh giới | 
| Phạm vi tọa độ lớn | 2 | Tính khoảng cách số nguyên và giá trị lớn | 
| Hai zappers gần nhau | Đúng khoảng cách bình phương tối thiểu | Xử lý cà vạt | 

## Vỏ cạnh 

Khi một bóng ma nằm chính xác trên một thiết bị phát hiện, tìm kiếm nhị phân sẽ tìm thấy thiết bị phát hiện đó là ứng cử viên phù hợp. Khoảng cách được tính toán bằng 0 nên thuật toán không thêm năng lượng. Vì:```
1 1
5
5
```danh sách Zapper được sắp xếp là`[5]`, chỉ mục tìm kiếm là`0`, và ứng viên phù hợp đưa ra chi phí là`0`. 

Khi một bóng ma xuất hiện trước tất cả các công cụ tìm kiếm, chỉ mục tìm kiếm sẽ là`0`, nên không còn ứng cử viên nào. Thuật toán sử dụng Zapper đầu tiên. Vì:```
2 2
10
20
0
30
```con ma ở`0`chỉ kiểm tra zapper tại`10`, cho (10^2=100). 

Khi một con ma nằm trong tất cả các công cụ tìm kiếm, chỉ mục tìm kiếm sẽ trở thành độ dài của mảng. Thuật toán bỏ qua ứng cử viên phù hợp không tồn tại và sử dụng công cụ tìm kiếm cuối cùng. Con ma ở`30`trong cùng một ví dụ sử dụng vị trí`20`, cũng có giá (10^2=100). 

Khi một con ma ở chính xác giữa hai máy bắn, cả hai ứng cử viên đều được kiểm tra. Vì:```
2 1
4
10
7
```chi phí là (3^2) và (3^2), do đó cả hai lựa chọn đều đưa ra giá trị tối thiểu chính xác là`9`.
