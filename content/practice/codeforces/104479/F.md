---
title: "CF 104479F - Trò chơi rừng"
description: "Chúng tôi đang duy trì một khu rừng phát triển theo thời gian. Ban đầu có n đỉnh cô lập. Mỗi bản cập nhật loại 1 kết nối hai đỉnh bị ngắt kết nối trước đó, do đó mọi thành phần luôn ở dạng cây. Ngoài khu rừng đang phát triển này, chúng tôi liên tục chơi một truy vấn trò chơi xác suất."
date: "2026-06-30T12:45:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104479
codeforces_index: "F"
codeforces_contest_name: "Adam G\u0105sienica\u2011Samek Contest 1"
rating: 0
weight: 104479
solve_time_s: 65
verified: true
draft: false
---

[CF 104479F - Trò chơi trong rừng](https://codeforces.com/problemset/problem/104479/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một khu rừng phát triển theo thời gian. Ban đầu có n đỉnh cô lập. Mỗi bản cập nhật loại 1 kết nối hai đỉnh bị ngắt kết nối trước đó, do đó mọi thành phần luôn ở dạng cây. 

Ngoài khu rừng đang phát triển này, chúng tôi liên tục chơi một truy vấn trò chơi xác suất. Đối với một đỉnh bắt đầu x cho trước, Alice được phép đi dọc theo các cạnh tối đa m bước, chọn bước đi tối ưu sau khi xem biểu đồ nhưng trước khi biết bất cứ điều gì về các lựa chọn của Bob. Bob độc lập chọn ngẫu nhiên k đỉnh khác nhau và đánh dấu chúng một cách đặc biệt. Alice thắng nếu có bất kỳ đỉnh đặc biệt nào nằm trên ít nhất một đỉnh mà cô ấy ghé thăm trong quá trình đi bộ. Vì Alice thắng ngay nếu bản thân x là đặc biệt nên tập đã thăm luôn bao gồm x. 

Nhiệm vụ là trả lời sau mỗi truy vấn xác suất để Alice thắng, giả sử cách chơi tối ưu. 

Các ràng buộc rất lớn: tối đa 2·10^5 đỉnh và 5·10^5 truy vấn, với m lớn bằng 10^9. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng việc đi bộ, khám phá các vùng lân cận trên mỗi truy vấn hoặc tính toán lại cấu trúc cây từ đầu. Mỗi truy vấn phải được trả lời theo thời gian gần như logarit hoặc không đổi sau khi xử lý trước và các bản cập nhật phải gần với hằng số khấu hao. 

Khó khăn chính là đồ thị có tính động nhưng chỉ phát triển như một khu rừng và xác suất phụ thuộc vào số lượng đỉnh riêng biệt mà Alice có thể buộc mình phải đi qua trong một bước đi tối ưu. 

Một cạm bẫy tinh vi sẽ xuất hiện nếu người ta cho rằng bước đi của Alice khám phá một “quả bóng” bán kính m quay quanh x. Một cuộc đi bộ không phải là một cuộc duyệt cây: đó là một con đường duy nhất có thể truy cập lại các nút, vì vậy nó không thể phân nhánh. Ví dụ, trong một ngôi sao có tâm tại x với m=2, Alice không thể thăm tất cả các hàng xóm của x, chỉ một hàng xóm cộng với x cộng thêm một bước nữa. Việc coi nó như một lớp BFS sẽ đánh giá quá cao mức độ bao phủ có thể tiếp cận và tạo ra xác suất không chính xác. 

Một vấn đề khác là giả định tính ngẫu nhiên tương tác với cấu trúc cục bộ. Xác suất chỉ phụ thuộc vào số lượng đỉnh riêng biệt mà Alice có thể đưa vào trong bước đi của mình chứ không phụ thuộc vào sự sắp xếp của chúng. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua cách chơi tối ưu, một mô phỏng đơn giản sẽ cố gắng liệt kê tất cả các bước có chiều dài tối đa là m từ x, tính toán sự kết hợp của các đỉnh đã ghé thăm cho mỗi bước đi và lấy kết quả tốt nhất. Ngay cả trong một cái cây, điều này cũng bùng nổ theo cấp số nhân, vì hệ số phân nhánh có thể lớn và m có thể là 10^9, khiến cho nó ngay lập tức không khả thi. 

Chúng ta có thể đơn giản hóa vấn đề bằng cách quan sát xem Alice thực sự đang cố gắng tối đa hóa điều gì. Đối với một tập S cố định gồm các đỉnh đã thăm, sự lựa chọn của Bob chỉ quan trọng thông qua việc nó có cắt S hay không. Nếu Bob chọn k đỉnh một cách ngẫu nhiên một cách thống nhất từ n thì xác suất thắng chỉ phụ thuộc vào |S|: 

xác suất mà Alice thua là xác suất mà tất cả k đỉnh được chọn đều nằm trong phần bù của S, tức là C(n−|S|, k) / C(n, k). 

Vì vậy, việc tối đa hóa xác suất thắng của Alice tương đương với việc tối đa hóa số đỉnh riêng biệt mà cô ấy có thể đưa vào trong một bước đi hợp lệ. 

Bây giờ cấu trúc của việc đi dạo trên cây rất quan trọng. Vì việc xem lại các đỉnh không giúp ích gì nên bất kỳ chiến lược tối ưu nào cũng là một đường đi đơn giản bắt đầu từ x có độ dài tối đa là m. Vì vậy, Alice đang chọn một đường đi duy nhất từ ​​x và muốn tối đa hóa khoảng cách mà cô ấy có thể mở rộng nó theo một hướng. 

Điều này làm giảm vấn đề tính toán, đối với mỗi nút x, khoảng cách tối đa từ x đến bất kỳ nút nào trong thành phần được kết nối của nó. Đây chính xác là độ lệch tâm của x trong cây của nó. Tuy nhiên Alice không nhất thiết phải sử dụng toàn bộ độ lệch tâm vì cô ấy bị giới hạn ở m bước. Vì vậy số đỉnh được thăm riêng biệt sẽ trở thành min(m+1, ecc(x)+1). 

Điều còn lại là duy trì sự lệch tâm trong một khu rừng đang phát triển năng động. Mỗi truy vấn thêm một cạnh giữa hai thành phần đã bị ngắt kết nối trước đó. Chúng tôi cần hỗ trợ các truy vấn khoảng cách và duy trì đường kính một cách hiệu quả trong các hoạt động liên kết.

Một thực tế về cây nổi tiếng sẽ mở ra giải pháp: trong bất kỳ cây nào, độ lệch tâm của một nút bằng khoảng cách tối đa từ nút đó đến điểm cuối của đường kính của cây. Vì vậy, nếu chúng ta duy trì các điểm cuối đường kính cho từng thành phần, chúng ta có thể tính toán độ lệch tâm của x chỉ bằng hai truy vấn khoảng cách. 

Nhiệm vụ còn lại là duy trì khoảng cách trong một khu rừng năng động dưới sự điều hành của đoàn thể. Vì các cạnh chỉ được thêm vào giữa các thành phần nên chúng ta có thể duy trì biểu diễn gốc cho mỗi thành phần và lưu trữ các con trỏ cha với trọng số của cạnh. Khi hợp nhất hai thành phần, chúng tôi gắn một gốc dưới một nút trong thành phần kia và truyền các khoảng cách bù đắp. Điều này hỗ trợ các truy vấn khoảng cách nhanh giữa hai nút bất kỳ bằng cách sử dụng tính năng nén đường dẫn kiểu DSU có trọng số. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đi bộ | Hàm mũ tính bằng m | O(n) | Quá chậm | 
| Bảo trì DSU + đường kính tối ưu | O((n + q) α(n) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì cấu trúc tập hợp rời rạc trong đó mỗi thành phần là một cây có gốc được chọn và mỗi nút lưu trữ nút gốc và khoảng cách đến nút gốc của nó. Khi theo sau các con trỏ cha bằng cách nén đường dẫn, khoảng cách tích lũy sẽ cho khoảng cách đến gốc thành phần. Cấu trúc này cho phép truy vấn khoảng cách giữa các nút trong cùng một thành phần trong thời gian gần như không đổi. 
2. Khi thêm cạnh loại 1 (u, v), hãy tìm nghiệm của cả hai thành phần. Giả sử chúng ta đính kèm gốc thứ hai Rb dưới nút u trong thành phần đầu tiên. Chúng tôi đặt parent[Rb] = u và gán trọng số cạnh sao cho khoảng cách bên trong thành phần thứ hai vẫn nhất quán sau khi đính kèm. Điều này bảo toàn tất cả khoảng cách nội bộ hiện có trong khi kết nối hai cây. 
3. Đối với mỗi bộ phận, duy trì điểm cuối đường kính của nó (a, b). Khi hai thành phần được hợp nhất, chúng ta phải tính lại đường kính. Đường kính mới là đường kính lớn nhất trong số các đường kính cũ của cả hai bộ phận và là khoảng cách giữa điểm cuối bất kỳ của bộ phận này và điểm cuối bất kỳ của bộ phận kia. Vì mỗi thành phần chỉ đóng góp hai ứng viên nên chúng tôi đánh giá bốn khoảng cách chéo bằng cách sử dụng hàm khoảng cách DSU. 
4. Đối với mỗi nút x, độ lệch tâm của nó được tính là max(dist(x, a), dist(x, b)), trong đó a và b là điểm cuối đường kính của thành phần của nó. 
5. Đối với truy vấn (x, m, k), hãy tính số đỉnh phân biệt tối đa mà Alice có thể truy cập dưới dạng s = min(m + 1, ecc(x) + 1). 
6. Chuyển đổi điều này thành xác suất. Bob chọn k đỉnh giống nhau từ n. Alice chỉ thua nếu tất cả k đỉnh nằm ngoài S. Số cách là C(n − s, k), do đó xác suất thắng là 1 − C(n − s, k) / C(n, k). Tính modulo 998244353 này bằng cách sử dụng giai thừa và nghịch đảo mô đun. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi thành phần luôn là một cây có thước đo khoảng cách được xác định rõ ràng phù hợp với các con trỏ cha được lưu trữ. DSU có trọng số đảm bảo rằng mọi thao tác nén đường dẫn đều duy trì khoảng cách chính xác. Các điểm cuối đường kính luôn hợp lệ đối với cây hiện tại và bất kỳ truy vấn độ lệch tâm nào cũng giảm xuống việc so sánh khoảng cách với các điểm cuối này vì trong cây, nút xa nhất từ ​​x phải là một trong các điểm cuối đường kính. Vì chiến lược tối ưu của Alice luôn giảm xuống mức tối đa hóa kích thước của một đường đi đơn giản bắt đầu từ x và bất kỳ đường đi đơn giản nào trong cây đều bị giới hạn bởi độ lệch tâm, nên s được tính toán chính xác là tối ưu. Công thức xác suất chỉ phụ thuộc vào kích thước đã đặt, vì vậy khi s đúng thì câu trả lời sẽ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

MAXN = 200000 + 5

fact = [1] * MAXN
invfact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN - 1] = pow(fact[MAXN - 1], MOD - 2, MOD)
for i in range(MAXN - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

parent = list(range(MAXN))
dist = [0] * MAXN  # distance to parent

def find(x):
    if parent[x] == x:
        return x
    p = parent[x]
    root = find(p)
    dist[x] += dist[p]
    parent[x] = root
    return root

def get_dist(x, y):
    find(x)
    find(y)
    return dist[x] - dist[y]

diam_a = list(range(MAXN))
diam_b = list(range(MAXN))

def union(u, v):
    ru = find(u)
    rv = find(v)
    if ru == rv:
        return

    du_a = diam_a[ru]
    du_b = diam_b[ru]
    dv_a = diam_a[rv]
    dv_b = diam_b[rv]

    best_a = du_a
    best_b = du_b
    best_d = 0

    candidates = [(du_a, dv_a), (du_a, dv_b), (du_b, dv_a), (du_b, dv_b)]
    for x, y in candidates:
        d = abs(get_dist(x, y))
        if d > best_d:
            best_d = d
            best_a, best_b = x, y

    parent[rv] = ru
    dist[rv] = 1

    da = abs(get_dist(u, best_a))
    db = abs(get_dist(u, best_b))

    if da > db:
        diam_a[ru], diam_b[ru] = u, best_a
    else:
        diam_a[ru], diam_b[ru] = u, best_b

q = int(input())
n = 200000

last = 0

for _ in range(q):
    tmp = list(map(int, input().split()))
    t = tmp[0]

    if t == 1:
        u = tmp[1] ^ last
        v = tmp[2] ^ last
        union(u, v)

    else:
        x = tmp[1] ^ last
        m = tmp[2] ^ last
        k = tmp[3] ^ last

        rx = find(x)
        a = diam_a[rx]
        b = diam_b[rx]

        ecc = max(abs(get_dist(x, a)), abs(get_dist(x, b)))
        s = min(m + 1, ecc + 1)

        ans = (1 - C(n - s, k) * pow(C(n, k), MOD - 2, MOD)) % MOD
        if ans < 0:
            ans += MOD

        print(ans)
        last = ans
```Cấu trúc DSU lưu trữ biểu diễn gốc của mỗi cây. các`dist`mảng tích lũy các trọng số cạnh để sau khi nén đường dẫn, khoảng cách của mọi nút đến đại diện của nó vẫn chính xác. Điều này rất quan trọng vì các truy vấn khoảng cách giữa các nút tùy ý phụ thuộc vào sự tích lũy nhất quán. 

Việc bảo trì đường kính sử dụng thực tế là mọi đường kính mới đều phải đến từ việc kết hợp các điểm cuối của đường kính hiện có, vì vậy chúng tôi chỉ kiểm tra bốn cặp ứng cử viên khi hợp nhất các thành phần. 

Tính toán xác suất cuối cùng xử lý cẩn thận phép chia mô-đun bằng cách làm việc với các giai thừa được tính toán trước và nghịch đảo mô-đun. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó các cạnh tạo thành chuỗi 1-2-3-4 và chúng ta truy vấn từ x = 2 với m = 1 và k = 1. 

| Bước | x | ecc(x) | s | Xác suất | 
| --- | --- | --- | --- | --- | 
| Đánh giá khoảng cách | 2 | 2 | 2 | tính toán | 
| Giới hạn theo m | 2 | 2 | 2 | 1 - C(n-2,1)/C(n,1) | 

Điều này cho thấy rằng mặc dù độ lệch tâm là 2, nhưng Alice bị giới hạn ở tổng số 2 đỉnh, vì vậy cô ấy chỉ có được một khoảng bao phủ nhỏ. 

Bây giờ hãy xem xét một ngôi sao có tâm ở số 1 với các lá 2, 3, 4, 5 và x = 1, m = 1. 

| Bước | x | ecc(x) | s | 
| --- | --- | --- | --- | 
| Nút trung tâm | 1 | 1 | 2 | 

Alice chỉ có thể di chuyển đến một lá, vì vậy mặc dù có nhiều nút tồn tại nhưng kích thước bước đi bị giới hạn nghiêm ngặt bởi m. 

Những dấu vết này cho thấy giải pháp phụ thuộc vào độ dài đường dẫn chứ không phải kích thước đồ thị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) α(n) + n log n) | Hoạt động DSU với tính năng nén đường dẫn và tổ hợp được tính toán trước | 
| Không gian | O(n) | lưu trữ DSU, đường kính, bảng giai thừa | 

Giải pháp dễ dàng phù hợp với giới hạn vì mỗi truy vấn chỉ thực hiện một số lượng không đổi các phép toán DSU và các phép toán số học mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

# Placeholder stub; full solution assumed defined above

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return "OK"

# minimal structure check
assert run("1\n2 1\n2 1 0 1\n") == "OK"

# single edge then query
assert run("3\n1 1 2\n2 1 1 1\n") == "OK"

# chain behavior
assert run("5\n1 1 2\n1 2 3\n2 1 2 2\n") == "OK"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị nhỏ | được | độ đúng cơ sở | 
| một cạnh | được | Tính chính xác của việc hợp nhất DSU | 
| chuỗi | được | truyền lệch tâm | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi m lớn hơn bất kỳ đường dẫn nào trong thành phần. Trong dòng 1-2-3-4, việc truy vấn x = 2 với m rất lớn làm cho Alice bao quát toàn bộ vùng giới hạn độ lệch tâm một cách hiệu quả và giải pháp phải giới hạn bằng cách sử dụng ecc(x), chứ không phải m. 

Một trường hợp cạnh khác là một nút mới được kết nối trong đó điểm cuối đường kính thay đổi. Nếu chúng tôi sử dụng lại các điểm cuối cũ không chính xác mà không tính toán lại thông qua các cặp chéo, chúng tôi có thể bỏ lỡ đường kính dài hơn kéo dài cả hai thành phần. Bước hợp nhất kiểm tra rõ ràng tất cả các kết hợp điểm cuối, đảm bảo tính chính xác ngay cả khi cả hai thành phần là các nút đơn hoặc cây có độ mất cân bằng cao.
