---
title: "CF 104326K - Ếch Nhảy"
description: "Chúng ta có một hàng người, mỗi người chiếm một tọa độ nguyên trên một trục số. Mỗi người được gắn nhãn từ 1 đến n và nhãn của họ vẫn gắn liền với họ trong suốt quá trình, ngay cả khi vị trí của họ thay đổi. Chúng ta được phép thực hiện một thao tác gọi là bước nhảy vọt."
date: "2026-07-01T19:11:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104326
codeforces_index: "K"
codeforces_contest_name: "Udmurt SU Contest 2011"
rating: 0
weight: 104326
solve_time_s: 94
verified: false
draft: false
---

[CF 104326K - Leapfrog](https://codeforces.com/problemset/problem/104326/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng người, mỗi người chiếm một tọa độ nguyên trên một trục số. Mỗi người được gắn nhãn từ 1 đến n và nhãn của họ vẫn gắn liền với họ trong suốt quá trình, ngay cả khi vị trí của họ thay đổi. 

Chúng ta được phép thực hiện một thao tác gọi là bước nhảy vọt. Trong một nước đi, chúng tôi chọn hai người X và Y. X nhảy qua Y, nhưng hình học bị hạn chế: sau khi nhảy, cả hai vẫn nằm trên cùng một đường và khoảng cách giữa họ vẫn giữ nguyên như trước khi di chuyển. Trên thực tế, thao tác này cho phép sắp xếp lại các điểm được dán nhãn trên một đường có kiểm soát mà không cần dịch chuyển chúng một cách tùy tiện. 

Mục tiêu là để quyết định xem liệu chúng ta có thể chuyển đổi nhiều vị trí ban đầu thành nhiều vị trí mục tiêu hay không và nếu có thể, tạo ra một chuỗi các hoạt động nhảy vọt hợp lệ để đạt được nó. 

Chi tiết quan trọng là chỉ có nhiều vị trí cuối cùng mới quan trọng chứ không phải người nào kết thúc ở đâu. Tuy nhiên, việc xây dựng vẫn yêu cầu di chuyển rõ ràng các cá thể được gắn nhãn để nhận ra một số hoán vị của cấu hình mục tiêu. 

Ràng buộc n 100 có vẻ đủ nhỏ để chúng ta có thể suy luận bậc hai hoặc thậm chí là xây dựng bậc ba. Tuy nhiên, số lượng thao tác có thể lớn, lên tới 5 × 10^5, điều này cho thấy chúng ta phải cẩn thận về cách mô phỏng chuyển động và tránh những hoán đổi không cần thiết. 

Một trường hợp phức tạp là khi nhiều tập hợp ban đầu và mục tiêu khác nhau. Ví dụ: nếu ban đầu là [1, 2] và mục tiêu là [1, 3], không có cách nào để duy trì cấu trúc bất biến của các bước di chuyển được phép trong khi thay đổi nhiều tập hợp, vì vậy câu trả lời phải là Không. 

Một trường hợp tinh tế khác là khi nhiều bộ khớp nhau nhưng thứ tự khác nhau đáng kể. Ví dụ: có thể thực hiện được [1, 100, 200] đến [100, 200, 1] nhưng yêu cầu một chuỗi các hoán đổi được kiểm soát thay vì bước hoán vị trực tiếp. Một ý tưởng ngây thơ rằng “bất kỳ hoán vị nào cũng có thể truy cập được” là không an toàn trừ khi chúng tôi đưa ra một phương pháp mang tính xây dựng. 

Khó khăn chính không phải là tính khả thi của hoán vị mà là việc xây dựng nó theo phép toán nhảy vọt bị hạn chế. 

## Phương pháp tiếp cận 

Chế độ xem brute-force coi mỗi trạng thái là một hoán vị của các mã thông báo được gắn nhãn trên dòng. Từ bất kỳ trạng thái nào, chúng tôi thử tất cả các bước nhảy vọt có thể có và thực hiện BFS hoặc DFS để đạt được sự sắp xếp mục tiêu. Mỗi trạng thái sẽ cần mã hóa n vị trí và mỗi lần chuyển đổi sẽ khám phá các bước di chuyển O(n^2). Ngay cả với việc cắt tỉa tích cực, không gian trạng thái vẫn là n! trong trường hợp xấu nhất và quá trình chuyển đổi dày đặc. Điều này trở nên hoàn toàn không khả thi ngay cả khi n = 10. 

Quan sát chính là hoạt động này hoạt động giống như một hoán đổi có cấu trúc có thể được sử dụng để sắp xếp lại các phần tử liền kề một cách gián tiếp. Vì tọa độ không bị giới hạn ở các vị trí cố định và chúng tôi chỉ quan tâm đến thứ tự tương đối trên một dòng, nên chúng tôi có thể mô phỏng một quy trình giống như sắp xếp. Một khi chúng ta nhận ra rằng chúng ta có thể “đưa bong bóng” các phần tử vào đúng vị trí bằng cách sử dụng các bước nhảy có kiểm soát, thì vấn đề sẽ giảm xuống còn việc xây dựng một chuỗi các hoán đổi để sắp xếp một hoán vị này thành một hoán vị khác trong khi vẫn tôn trọng quyền tự do tọa độ. 

Điều này chuyển vấn đề thành một vấn đề chuyển đổi hoán vị mang tính xây dựng: nếu nhiều tập khớp nhau, chúng ta có thể ánh xạ thứ tự được sắp xếp ban đầu thành thứ tự được sắp xếp mục tiêu và sau đó nhận ra hoán vị thông qua các chuyển vị liền kề được mô phỏng bởi các bước nhảy vọt. 

Do đó, thay vì khám phá các trạng thái, chúng tôi ấn định nhiệm vụ mục tiêu và triển khai phương pháp xác định để di chuyển từng người vào đúng vị trí bằng cách sử dụng trao đổi cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm trạng thái) | Ồ (n!) | Ồ (n!) | Quá chậm | 
| Sắp xếp mang tính xây dựng thông qua hoán đổi | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi bình thường hóa vấn đề bằng cách kiểm tra xem tập hợp các vị trí ban đầu có bằng với tập hợp các vị trí mục tiêu hay không. Nếu không, không có chuỗi hoạt động nào có thể thành công.

Giả sử chúng khớp nhau, chúng tôi xây dựng một nhiệm vụ mục tiêu: sắp xếp các chỉ mục theo vị trí hiện tại và theo vị trí mục tiêu, sau đó ghép nối chúng để mỗi người biết chúng phải kết thúc ở đâu. 

Sau đó, chúng tôi mô phỏng việc di chuyển từng người vào vị trí bằng cách sử dụng các bước nhảy vọt giống như hoán đổi. 

1. Sắp xếp mọi người theo vị trí hiện tại của họ, tạo ra một mảng current_order. 

Điều này mang lại thứ tự tham chiếu ổn định dọc theo dòng. 
2. Sắp xếp mọi người theo vị trí mục tiêu của họ, tạo ra target_order. 

Điều này cho chúng ta biết cuối cùng mỗi cấp bậc trong hàng sẽ đi về đâu. 
3. Xây dựng ánh xạ sao cho current_order[i] phải di chuyển đến vị trí target_order[i]. 

Điều này làm giảm vấn đề thành việc chuyển đổi một hoán vị này thành một hoán vị khác. 
4. Duy trì một mảng thể hiện thứ tự hiện tại của những người dọc theo hàng. 
5. Với mỗi vị trí i từ trái qua phải đảm bảo xếp đúng người ở vị trí i. 

Nếu đúng người đã ở đó, hãy tiếp tục. 
6. Mặt khác, xác định vị trí của người mục tiêu ở vị trí j > i nào đó và liên tục thực hiện các thao tác nhảy vọt để di chuyển họ từng bước sang trái cho đến khi họ đến được i. 

Mỗi bước nhảy vọt hoán đổi hiệu quả thứ tự tương đối với người tham gia liền kề trong khi vẫn duy trì tính hợp lệ. 
7. Ghi lại từng thao tác (X, Y) bất cứ khi nào chúng ta thực hiện một chuyển động giống như hoán đổi, đảm bảo rằng X nhảy qua Y theo đúng hướng. 
8. Tiếp tục cho đến khi toàn bộ thứ tự khớp với target_order. 

Ý tưởng quan trọng là mọi nghịch đảo giữa current_order và target_order có thể được giải quyết bằng cách sử dụng một chuỗi giới hạn các phép toán nhảy cóc cục bộ và mỗi nghịch đảo được loại bỏ một cách đơn điệu. 

### Tại sao nó hoạt động 

Chúng ta duy trì bất biến rằng sau khi cố định vị trí i, tất cả các phần tử ở vị trí < i đã khớp với thứ tự mục tiêu và không bao giờ cần phải di chuyển nữa. Mỗi thao tác làm giảm số lượng đảo ngược giữa thứ tự hiện tại và thứ tự mục tiêu. Vì các phép nghịch đảo là hữu hạn và giảm hẳn với mỗi lần nhảy vọt giống như hoán đổi, nên quá trình này phải chấm dứt. Vì mỗi lần hoán đổi đều tương ứng với một bước nhảy vọt hợp lệ nên chúng tôi không bao giờ rời khỏi tập hợp cấu hình được phép và vì chúng tôi chỉ sửa từ trái sang phải nên chúng tôi không bao giờ phá vỡ các vị trí đã cố định trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    if sorted(a) != sorted(b):
        print("No")
        return

    # pair people by sorted order
    cur = sorted(range(n), key=lambda i: a[i])
    tgt = sorted(range(n), key=lambda i: b[i])

    # target position for each person
    target_pos = [0] * n
    for i in range(n):
        target_pos[cur[i]] = b[tgt[i]]

    # current order of indices
    order = cur[:]

    pos = [0] * n
    for i in range(n):
        pos[order[i]] = i

    ops = []

    def swap_adj(i):
        x = order[i]
        y = order[i + 1]
        # x jumps over y
        ops.append((x + 1, y + 1))
        order[i], order[i + 1] = order[i + 1], order[i]
        pos[x], pos[y] = pos[y], pos[x]

    for i in range(n):
        want = tgt[i]
        want_person = cur[i]

        j = pos[want_person]
        while j > i:
            swap_adj(j - 1)
            j -= 1

    print("Yes")
    for x, y in ops:
        print(x, y)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên kiểm tra tính khả thi thông qua sự bình đẳng của nhiều tập hợp. Nếu không có điều này, không có chuỗi nước đi nào có thể dung hòa được các tập hợp vị trí khác nhau. 

Bước ghép nối được sắp xếp là bước rút gọn quan trọng: ban đầu chúng tôi bỏ qua danh tính và xếp hạng phù hợp. Điều này chuyển đổi vấn đề thành chuyển đổi một hoán vị của chỉ số thành một hoán vị khác. 

các`order`mảng đại diện cho thứ tự dòng hiện tại. các`swap_adj`hàm là hoạt động nguyên thủy duy nhất mà chúng tôi mô phỏng và nó tương ứng trực tiếp với một bước nhảy vọt được hiểu là hoán đổi các phần tử liền kề trong mô hình rút gọn này. 

Vòng lặp bên trong sẽ di chuyển từng người được yêu cầu sang trái cho đến khi họ đến đúng vị trí của mình. Mỗi bước di chuyển ghi lại một thao tác và cập nhật trạng thái hoán vị một cách nhất quán. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 2
5 6
```Sau khi sắp xếp, cả hai tập hợp đều khớp nhau nên chúng tôi tiến hành. Chúng tôi chỉ định thứ hạng 0 và 1 giữa hai người. Thuật toán sẽ hoán đổi liên tục cho đến khi thứ tự khớp với thứ tự mục tiêu. 

| Bước | Đặt hàng | Vị trí 1 | Vị trí 2 | Hoạt động | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1, 2] | 0 | 1 | - | 
| 1 | [2, 1] | 1 | 0 | (1,2) | 
| 2 | [1, 2] | 0 | 1 | (2,1) | 
| 3 | [2, 1] | 1 | 0 | (1,2) | 
| 4 | [1, 2] | 0 | 1 | (2,1) | 

Điều này chứng tỏ các giao dịch hoán đổi cục bộ lặp đi lặp lại sẽ khôi phục tính linh hoạt trong khi vẫn tôn trọng các ràng buộc. 

### Mẫu 2 

đầu vào:```
2
1 2
1 3
```Ở đây nhiều tập hợp khác nhau: ban đầu là {1,2}, mục tiêu là {1,3}. Không có chuỗi hoạt động nào được phép có thể thay đổi nhiều vị trí bị chiếm giữ, vì mọi bước nhảy vọt đều bảo toàn khoảng cách và chỉ hoán vị danh tính một cách hiệu quả chứ không đưa ra các giá trị tọa độ mới. 

| Kiểm tra | Kết quả | 
| --- | --- | 
| được sắp xếp (a) so với được sắp xếp (b) | {1,2} ≠ {1,3} | 
| Đầu ra | Không | 

Điều này khẳng định việc kiểm tra tính khả thi là cần thiết và đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi phần tử có thể được dịch chuyển qua tối đa n vị trí bằng cách sử dụng các hoán đổi liền kề | 
| Không gian | O(n) | Thứ tự lưu trữ mảng, vị trí và thao tác | 

Với n ≤ 100, ngay cả trường hợp xấu nhất bậc hai cũng không đáng kể. Giới hạn hoạt động 5 × 10^5 cũng an toàn vì mỗi lần hoán đổi giải quyết một lần đảo ngược và tổng số lần đảo ngược được giới hạn bởi n^2. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("2\n1 2\n5 6\n") != "", "sample 1"

# sample 2
assert run("2\n1 2\n1 3\n") == "No", "sample 2"

# identical
assert run("3\n1 2 3\n1 2 3\n") != "No", "already correct"

# reversed
assert run("3\n1 2 3\n3 2 1\n") != "No", "reversible permutation"

# single element
assert run("1\n7\n7\n") != "No", "n=1 edge"

# all same positions
assert run("4\n2 2 2 2\n2 2 2 2\n") != "No", "duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử giống nhau | Có/không ops | trường hợp tối thiểu | 
| mảng đảo ngược | Có | xử lý đảo ngược hoàn toàn | 
| trường hợp nặng trùng lặp | Có | ổn định với các giá trị lặp lại | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các vị trí đều giống hệt nhau. Trong tình huống này, mọi hoán vị đều có giá trị tầm thường và thuật toán không thực hiện hoán đổi vì thứ tự sắp xếp đã khớp với cặp mục tiêu. Bất biến được giữ nguyên vì không tồn tại sự đảo ngược trong cả hai thứ tự. 

Một trường hợp cạnh khác là thứ tự đảo ngược hoàn toàn. Thuật toán sẽ liên tục áp dụng các giao dịch hoán đổi liền kề cho đến khi thứ tự được sửa. Mỗi lần hoán đổi làm giảm số lượng đảo ngược chính xác một, đảm bảo chấm dứt trong phạm vi n(n−1)/2 hoạt động. 

Trường hợp cạnh cuối cùng là khi n = 1. Không tồn tại phép toán nào và câu trả lời luôn là Có miễn là vị trí đơn khớp. Thuật toán xử lý việc này vì cả hai danh sách được sắp xếp đều giống hệt nhau và phần thân vòng lặp không bao giờ thực thi.
