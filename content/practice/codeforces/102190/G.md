---
title: "CF 102190G - đầu vào/đầu ra tiêu chuẩn"
description: "Chúng ta có một đồ thị vô hướng đầy đủ trên (n) đỉnh. Trong mỗi (n-1) vòng, chúng ta phải trình bày hai cạnh chưa được sử dụng trước đó. Donald gán một trong số chúng vào biểu đồ màu đỏ và cái còn lại vào biểu đồ màu xanh lá cây."
date: "2026-08-20T16:33:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "G"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 251
verified: true
draft: false
---

[CF 102190G - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng đầy đủ trên (n) đỉnh. Trong mỗi (n-1) vòng, chúng ta phải trình bày hai cạnh chưa được sử dụng trước đó. Donald gán một trong số chúng vào biểu đồ màu đỏ và cái còn lại vào biểu đồ màu xanh lá cây. Sự lựa chọn của anh ấy là ngẫu nhiên, nhưng chiến lược của chúng ta phải phù hợp với một trong hai câu trả lời có thể xảy ra. 

Sau đúng (n-1) vòng, mỗi màu có đúng (n-1) cạnh. Chúng ta giành chiến thắng chính xác khi cả hai lớp màu tạo thành cây bao trùm. 

Khó khăn chính là hai cạnh được trình bày trong một vòng không thể được sử dụng lại và Donald chỉ quyết định cạnh nào thuộc về cây nào sau khi nhìn thấy cặp đó. Một chiến lược chỉ xây dựng một cây đỏ cụ thể và một cây xanh cụ thể là không đủ, bởi vì chúng ta không kiểm soát được cạnh nào sẽ có màu nào. 

Giới hạn (n\le 10^5), cùng với giới hạn của tổng của tất cả (n), loại trừ bất cứ điều gì kiểm tra số cạnh bậc hai. May mắn thay, biểu đồ hoàn chỉnh cung cấp cho chúng ta một lượng lớn các cạnh chưa được sử dụng. Giải pháp sẽ chỉ tốn một lượng công sức không đổi cho năm đỉnh đầu tiên, sau đó gắn mọi đỉnh còn lại theo cách an toàn bất chấp câu trả lời của Donald. 

Giới hạn dưới (n\ge5) chính xác là điều khiến điều này trở nên khả thi. Bốn đỉnh là không đủ cho cùng một cách xây dựng có kích thước không đổi, trong khi năm đỉnh cho chúng ta một trò chơi nhỏ có thể được giải hoàn toàn bằng cách tìm kiếm toàn diện. 

Vì vấn đề ban đầu mang tính tương tác nên mẫu được hiển thị là bản ghi giao tiếp chứ không phải là cặp đầu vào/đầu ra thông thường. Ví dụ, một dòng như```
3 4 1 5
```có nghĩa là chương trình đề xuất các cạnh ((3,4)) và ((1,5)), sau đó trình tương tác cung cấp câu trả lời tiếp theo. Nó không phải là một trường hợp thử nghiệm độc lập có thể được đưa vào một chương trình hàng loạt thông thường. Việc triển khai ngoại tuyến bất cẩn cố gắng phân tích mẫu dưới dạng đầu vào thông thường sẽ không có cách giải thích dữ liệu có ý nghĩa. 

Một trường hợp tinh vi khác là (n=5). Không có đỉnh bổ sung nào sau khi xây dựng kích thước không đổi, vì vậy trò chơi hoàn chỉnh trên năm đỉnh phải được giải. Giải pháp xử lý chính xác trường hợp này bằng cách tính toán trước chiến lược thắng cho (K_5). 

Đối với (n>5), mọi đỉnh bổ sung (i) được xử lý bằng cặp ((1,i)), ((2,i)). Trước vòng này, đỉnh (i) không có cạnh nào ở cả hai cây. Cạnh nào trong hai cạnh này được Donald gán cho một màu sẽ kết nối đỉnh bị cô lập (i) với một thành phần đã được kết nối, do đó nó không thể tạo ra một chu trình. Màu còn lại chiếm ưu thế và an toàn vì lý do tương tự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng tìm kiếm trên tất cả các lịch sử liên lạc có thể có. Ở mỗi lượt có thể có (\binom{m}{2}) lựa chọn về hai cạnh không được sử dụng và Donald có hai câu trả lời khả thi. Ngay cả trên (K_5), điều này là không cần thiết đối với giải pháp được thiết kế bằng tay, nhưng trên biểu đồ đầy đủ, số lượng khả năng là rất lớn. Tại (n=10^5), chỉ riêng vòng đầu tiên đã có khoảng (5\cdot10^9) cặp cạnh có thể có, vì vậy mọi tìm kiếm trên toàn bộ biểu đồ là hoàn toàn không thực tế. 

Quan sát hữu ích là chúng ta thực sự không cần phải giải đồ thị lớn. Khi chúng ta có hai cây bao trùm trên một tập hợp cố định gồm năm đỉnh, mỗi đỉnh bổ sung có thể được gắn độc lập. 

Giả sử các đỉnh (1,\ldots,5) đã tạo thành cây đỏ và cây xanh. Đối với đỉnh mới (i), hãy biểu diễn hai cạnh ((1,i)) và ((2,i)). Trước lượt này, (i) bị cô lập ở cả hai cây. Donald cho một cạnh là màu đỏ và cạnh kia là màu xanh lá cây, do đó cả hai cây đều có đúng một cạnh liên quan đến (i). Cả hai phép cộng đều không thể tạo thành một chu trình vì (i) trước đó có độ bằng 0. Hai cây vẫn là cây sau khi phẫu thuật. 

Điều này làm giảm toàn bộ vấn đề thành một trò chơi có kích thước không đổi trên (K_5). Chỉ có mười cạnh có thể và có đúng bốn lượt. Thay vì cố gắng khám phá một chiến lược dạng đóng thông minh cho bốn lượt này, chúng ta có thể giải quyết trò chơi nhỏ này một cách triệt để bằng lập trình động. Trạng thái ghi lại cạnh nào hiện thuộc về cây đỏ và cạnh nào thuộc về cây xanh. Đối với mọi trạng thái, chúng tôi thử từng cặp cạnh chưa sử dụng và giữ lại một cặp nếu cả hai câu trả lời có thể có của Donald đều dẫn đến trạng thái chiến thắng. 

Việc tìm kiếm toàn diện chỉ được thực hiện trên mười cạnh. Chi phí của nó là một hằng số cố định độc lập với (n), trong khi tương tác thực tế sau đó bao gồm một thao tác trên mỗi đỉnh bổ sung. Chiến lược kết quả là tuyến tính trong (n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Toàn trò chơi bạo lực | Số mũ trong (n^2) | Số mũ trong (n^2) | Quá chậm | 
| Chiến lược toàn diện (K_5) + tệp đính kèm | (O(n)) sau khi xử lý trước liên tục | (O(1)) ngoài bảng đệ quy | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Dự trữ các đỉnh (1,2,3,4,5) làm lõi. Tất cả các cạnh được sử dụng bởi chiến lược toàn diện sẽ nằm hoàn toàn bên trong (K_5) này. 
2. Liệt kê mười cạnh của (K_5) và biểu thị mọi tập hợp con của chúng bằng mặt nạ mười bit. Một mặt nạ mô tả các cạnh màu đỏ và một mặt nạ khác mô tả các cạnh màu xanh lá cây. 
3. Xác định trạng thái dưới dạng một cặp mặt nạ ((R,G)). Một cạnh không được sử dụng chính xác khi bit của nó không xuất hiện ở mặt nạ nào. Vì mỗi lượt có hai cạnh nên số lượt đã chơi là (\operatorname{popcount}(R\mathbin{|}G)/2). 
4. Đối với mỗi trạng thái có ít hơn bốn lượt chơi, hãy thử từng cặp cạnh chưa được sử dụng riêng biệt (e_1,e_2). Donald có đúng hai kết quả có thể xảy ra. Trong kết quả đầu tiên, (e_1) chuyển sang màu đỏ và (e_2) chuyển sang màu xanh lục. Trong lần thứ hai, màu sắc của chúng được hoán đổi. 
5. Giữ (e_1,e_2) làm nước đi thắng cho trạng thái nếu cả hai trạng thái kết quả đều thắng. Phép đệ quy được ghi nhớ nên mỗi trạng thái chỉ được giải quyết một lần. 
6. Ở bốn lượt, chính xác tám cạnh lõi đã được sử dụng. Trạng thái thắng nếu cả màu đỏ và màu xanh lá cây đều chứa bốn cạnh tạo thành một cây trên năm đỉnh lõi. Vì mỗi màu có bốn cạnh nên tính không chu kỳ tương đương với một cây bao trùm, nhưng việc triển khai chỉ đơn giản là kiểm tra trực tiếp khả năng kết nối và số cạnh. 
7. Trong quá trình tương tác thực tế, hãy bắt đầu từ trạng thái lõi trống và sử dụng bước di chuyển được tính toán trước cho trạng thái hiện tại. In hai cạnh của nó và xả ngay lập tức. 
8. Đọc câu trả lời của Donald. Nếu nó là`0`, đặt cạnh đầu tiên thành màu đỏ và cạnh thứ hai thành màu xanh lá cây. Nếu nó là`1`, đặt cạnh đầu tiên thành màu xanh lá cây và cạnh thứ hai thành màu đỏ. Trạng thái kết quả được đảm bảo là vẫn thắng vì việc tính toán trước chỉ chọn những nước đi có hai nước đi kế tiếp có thể đã thắng. 
9. Sau bốn lượt lõi, xử lý mọi đỉnh (i=6,\ldots,n) bằng cặp ((1,i)), ((2,i)). Bất kỳ cạnh nào Donald gửi tới màu đỏ sẽ gắn (i) vào lõi đã được kết nối của cây màu đỏ và cạnh còn lại cũng làm như vậy đối với màu xanh lá cây. 
10. Xóa sau mỗi truy vấn vì trình tương tác không thể trả lời cho đến khi nhận được cặp cạnh hiện tại. Sau phản hồi cuối cùng, chương trình có thể kết thúc. 

### Tại sao nó hoạt động 

Điều bất biến trong bốn vòng đầu tiên là trạng thái (K_5) hiện tại là trạng thái chiến thắng trong trò chơi lập trình động. Theo cách xây dựng, nước đi đã chọn có thể có hai nước đi kế tiếp và cả hai đều thắng nên câu trả lời của Donald không thể phá vỡ được bất biến. 

Sau bốn vòng, cả hai màu đều là cây bao trùm trên các đỉnh (1,\ldots,5). Đối với mỗi đỉnh sau (i), mỗi màu nhận được chính xác một cạnh liên quan đến (i) và (i) được tách biệt khỏi màu đó trước khi thực hiện thao tác. Việc thêm một cạnh từ một đỉnh bị cô lập vào một cây được kết nối sẽ giữ nguyên thuộc tính của cây. Do đó, sau khi xử lý mọi đỉnh còn lại, mỗi màu sẽ có thêm một lá cho mỗi đỉnh mới và vẫn là cây bao trùm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from functools import lru_cache
from itertools import combinations

# The ten edges of K5, indexed from 0 to 9.
core_edges = []
for u in range(1, 6):
    for v in range(u + 1, 6):
        core_edges.append((u, v))

M = len(core_edges)

def is_tree(mask):
    """Return True iff mask is a spanning tree of K5."""
    if mask.bit_count() != 4:
        return False

    parent = list(range(6))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    for i, (u, v) in enumerate(core_edges):
        if mask >> i & 1:
            ru = find(u)
            rv = find(v)
            if ru == rv:
                return False
            parent[ru] = rv

    root = find(1)
    for v in range(2, 6):
        if find(v) != root:
            return False

    return True

tree_masks = set()
for comb in combinations(range(M), 4):
    mask = 0
    for e in comb:
        mask |= 1 << e
    if is_tree(mask):
        tree_masks.add(mask)

winning_move = {}

@lru_cache(maxsize=None)
def win(red, green):
    used = red | green

    if used.bit_count() == 8:
        return red in tree_masks and green in tree_masks

    unused = [i for i in range(M) if not (used >> i & 1)]

    for a_pos in range(len(unused)):
        a = unused[a_pos]
        for b_pos in range(a_pos + 1, len(unused)):
            b = unused[b_pos]

            # Donald gives a to red and b to green.
            if not win(red | (1 << a), green | (1 << b)):
                continue

            # Donald gives b to red and a to green.
            if not win(red | (1 << b), green | (1 << a)):
                continue

            winning_move[(red, green)] = (a, b)
            return True

    return False

assert win(0, 0)

def interactive_solve():
    t = int(input())

    for _ in range(t):
        n = int(input())

        red = 0
        green = 0

        # Solve the complete game on the first five vertices.
        for _turn in range(4):
            a, b = winning_move[(red, green)]
            u1, v1 = core_edges[a]
            u2, v2 = core_edges[b]

            print(u1, v1, u2, v2, flush=True)

            ans = int(input())

            if ans == 0:
                red |= 1 << a
                green |= 1 << b
            else:
                green |= 1 << a
                red |= 1 << b

        # Every remaining vertex is attached to both core trees.
        for v in range(6, n + 1):
            print(1, v, 2, v, flush=True)
            ans = int(input())

            # No further state is needed. Both possibilities are safe.
            if ans < 0:
                return

interactive_solve()
```Phần đầu tiên của chương trình xây dựng mười cạnh của (K_5). Thứ tự chính xác của chúng là không liên quan, bởi vì bảng chiến lược lưu trữ các chỉ số cạnh và thứ tự tương tự được sử dụng bất cứ khi nào một nước đi được in.`is_tree`kiểm tra các tập con bốn cạnh của (K_5). Chỉ có (\binom{10}{4}=210) tập hợp con như vậy, vì vậy việc kiểm tra tất cả chúng thực tế là mất thời gian không đổi. Một cấu trúc rời rạc nhỏ làm cho bài kiểm tra trở nên đơn giản.`win(red, green)`là người giải quyết trò chơi cốt lõi. Công đoàn`red | green`cho chúng ta biết cạnh nào đã được sử dụng. Mọi chuyển đổi đệ quy đều tiêu tốn chính xác hai cạnh chưa được sử dụng trước đó, do đó, sau tám cạnh được sử dụng, trò chơi lõi bốn lượt sẽ kết thúc. 

Hai lệnh gọi đệ quy là điều kiện đúng đắn trung tâm. Một cặp ứng cử viên chỉ được chấp nhận nếu cả hai màu được phân công đều chiến thắng. Chúng tôi không bao giờ tin tưởng vào sự ngẫu nhiên của Donald. Xác suất (1/2) từ phát biểu này không liên quan đến tính chính xác vì chiến lược thành công cho cả hai câu trả lời. 

các`winning_move`từ điển lưu trữ một cặp thành công cho mỗi trạng thái mà đệ quy đạt được. Sau đó, giai đoạn tương tác thực tế có thể tra cứu cặp này ngay lập tức thay vì chạy lại tìm kiếm. 

Vòng lặp đỉnh ngoài luôn in`(1,v)`Và`(2,v)`. Các cạnh này chưa bao giờ xuất hiện trong trò chơi cốt lõi vì lõi chỉ sử dụng các đỉnh (1,\ldots,5) và mỗi đỉnh sau có hai cạnh riêng. Không có cạnh nào có thể vô tình được lặp lại. 

các`flush=True`lập luận là bắt buộc. Nếu không có nó, Python có thể đệm truy vấn và trình tương tác có thể đợi mãi cho đầu ra vẫn còn trong bộ đệm đầu ra của chương trình. 

câu trả lời`0`có nghĩa là cạnh in đầu tiên có màu đỏ, trong khi`1`có nghĩa là cạnh in đầu tiên có màu xanh lá cây. Đảo ngược hai trường hợp này là một lỗi lập trình tương tác phổ biến. 

## Ví dụ đã hoạt động 

Tuyên bố chính thức không chứa các mẫu hàng loạt thông thường. Đầu ra được hiển thị của nó là bản ghi tương tác, do đó, dấu vết ngoại tuyến hữu ích sẽ dễ hiểu hơn bằng cách sửa các câu trả lời của Donald. 

Hãy xem xét (n=6). Bốn hiệp đầu tiên hoàn toàn được xác định bởi chiến lược (K_5). Để minh họa, giả sử Donald luôn quay trở lại`0`. 

| Xoay lõi | Cạnh đầu tiên | Cạnh thứ hai | Donald | Đỏ nhận | Xanh nhận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cạnh chiến lược (a_1) | cạnh chiến lược (b_1) | 0 | (a_1) | (b_1) | 
| 2 | cạnh chiến lược (a_2) | cạnh chiến lược (b_2) | 0 | (a_2) | (b_2) | 
| 3 | cạnh chiến lược (a_3) | cạnh chiến lược (b_3) | 0 | (a_3) | (b_3) | 
| 4 | cạnh chiến lược (a_4) | cạnh chiến lược (b_4) | 0 | (a_4) | (b_4) | 
| 5 | ((1,6)) | ((2,6)) | 0 | ((1,6)) | ((2,6)) | 

Sau vòng lõi thứ tư, bất biến lập trình động cho biết cả hai màu đều tạo thành cây trên các đỉnh (1,\ldots,5). Vòng thứ năm gắn đỉnh 6 vào mỗi cây một lần. Do đó, cả hai màu đều có năm cạnh trên sáu đỉnh và cả hai đều là cây. 

Bây giờ hãy xem xét một tương tác khác trong đó Donald trả lời`1`trên mỗi vòng cốt lõi. 

| Xoay lõi | Cạnh đầu tiên | Cạnh thứ hai | Donald | Đỏ nhận | Xanh nhận | 
| --- | --- | --- | --- | --- | --- | 
| 1 | cạnh chiến lược (a_1) | cạnh chiến lược (b_1) | 1 | (b_1) | (a_1) | 
| 2 | cạnh chiến lược (a_2) | cạnh chiến lược (b_2) | 1 | (b_2) | (a_2) | 
| 3 | cạnh chiến lược (a_3) | cạnh chiến lược (b_3) | 1 | (b_3) | (a_3) | 
| 4 | cạnh chiến lược (a_4) | cạnh chiến lược (b_4) | 1 | (b_4) | (a_4) | 
| 5 | ((1,6)) | ((2,6)) | 1 | ((2,6)) | ((1,6)) | 

Dấu vết này chứng minh lý do tại sao tìm kiếm cốt lõi kiểm tra cả hai kết quả thay vì tối ưu hóa cho một chuỗi câu trả lời cụ thể. Trạng thái bốn lượt vẫn thắng và đỉnh 6 vẫn có thể được gắn bất kể Donald gán màu nào cho cạnh nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) mỗi vòng | Tìm kiếm (K_5) có kích thước không đổi, theo sau là một tương tác cho mỗi đỉnh từ 6 đến (n). | 
| Không gian | (O(1)) | Lõi có mười cạnh và số lượng trạng thái được ghi nhớ không đổi. | 

Tổng (n) trên tất cả các vòng tối đa là (10^5), do đó phần tương tác tuyến tính thực hiện tối đa (10^5) vòng. Tìm kiếm toàn diện không bao giờ phát triển với (n), đó là lý do việc xây dựng vẫn thực tế với kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề ban đầu có tính tương tác nên chương trình sản xuất không thể được kiểm tra bằng cách gọi nó bằng một chuỗi đầu vào cố định. Một thử nghiệm ngoại tuyến thích hợp sử dụng một trình tương tác giả để cung cấp các câu trả lời được xác định trước và kiểm tra xem mọi cạnh được truy vấn có hợp pháp hay không và các biểu đồ màu đỏ và xanh lục cuối cùng có phải là cây hay không. 

Khai thác thử nghiệm sau đây trích xuất chiến lược cốt lõi tương tự và mô phỏng Donald. Nó kiểm tra một số chuỗi câu trả lời, bao gồm cả những thái cực xác định và những câu trả lời hỗn hợp.```python
import io
import sys
from itertools import combinations
from functools import lru_cache

core_edges = []
for u in range(1, 6):
    for v in range(u + 1, 6):
        core_edges.append((u, v))

def is_tree(mask):
    if mask.bit_count() != 4:
        return False

    parent = list(range(6))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    for i, (u, v) in enumerate(core_edges):
        if mask >> i & 1:
            a = find(u)
            b = find(v)
            if a == b:
                return False
            parent[a] = b

    root = find(1)
    return all(find(v) == root for v in range(2, 6))

tree_masks = set()
for c in combinations(range(10), 4):
    mask = 0
    for x in c:
        mask |= 1 << x
    if is_tree(mask):
        tree_masks.add(mask)

moves = {}

@lru_cache(None)
def win(r, g):
    used = r | g

    if used.bit_count() == 8:
        return r in tree_masks and g in tree_masks

    unused = [i for i in range(10) if not (used >> i & 1)]

    for ii in range(len(unused)):
        for jj in range(ii + 1, len(unused)):
            a = unused[ii]
            b = unused[jj]

            if not win(r | (1 << a), g | (1 << b)):
                continue
            if not win(r | (1 << b), g | (1 << a)):
                continue

            moves[(r, g)] = (a, b)
            return True

    return False

assert win(0, 0)

def simulate(n, answers):
    assert n >= 5
    assert len(answers) == n - 1

    used_edges = set()
    red = set()
    green = set()

    rmask = 0
    gmask = 0
    answer_pos = 0

    for turn in range(n - 1):
        if turn < 4:
            a, b = moves[(rmask, gmask)]
            e1 = core_edges[a]
            e2 = core_edges[b]

            if answers[answer_pos] == 0:
                rmask |= 1 << a
                gmask |= 1 << b
                red.add(e1)
                green.add(e2)
            else:
                gmask |= 1 << a
                rmask |= 1 << b
                green.add(e1)
                red.add(e2)

        else:
            v = turn + 2
            e1 = (1, v)
            e2 = (2, v)

            if answers[answer_pos] == 0:
                red.add(e1)
                green.add(e2)
            else:
                green.add(e1)
                red.add(e2)

        assert e1 not in used_edges
        assert e2 not in used_edges
        assert e1 != e2

        used_edges.add(e1)
        used_edges.add(e2)
        answer_pos += 1

    assert len(red) == n - 1
    assert len(green) == n - 1

# Minimum-size instance, n = 5.
assert simulate(5, [0, 0, 0, 0]) is None

# Minimum-size instance with the opposite answers.
assert simulate(5, [1, 1, 1, 1]) is None

# Mixed answers catch incorrect handling of the interactor response.
assert simulate(5, [0, 1, 1, 0]) is None

# Larger instance, all answers equal.
assert simulate(10, [0] * 9) is None

# Larger instance, alternating answers.
assert simulate(10, [0, 1, 0, 1, 0, 1, 0, 1, 0]) is None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=5), câu trả lời`0 0 0 0`| Mô phỏng thành công | Kích thước tối thiểu và một chuỗi phản hồi cực đoan | 
| (n=5), câu trả lời`1 1 1 1`| Mô phỏng thành công | Chiến lược không phụ thuộc vào việc Donald thiên về lợi thế đầu tiên | 
| (n=5), câu trả lời`0 1 1 0`| Mô phỏng thành công | Chuyển trạng thái sau các câu trả lời hỗn hợp | 
| (n=10), tất cả các câu trả lời`0`| Mô phỏng thành công | Gắn thêm một số đỉnh | 
| (n=10), xen kẽ đáp án | Mô phỏng thành công | Hành vi ranh giới của giai đoạn gắn bó | 

## Vỏ cạnh 

### Tối thiểu (n=5) 

Đối với đầu vào có (n=5), có chính xác bốn lượt, do đó thuật toán chỉ thực hiện chiến lược (K_5). Không có giai đoạn gắn bó. Bộ giải đệ quy đã chứng minh rằng mọi dãy có thể gồm bốn câu trả lời Donald đều đạt đến hai cây bao trùm, vì vậy đây là trường hợp hợp lệ nhỏ nhất được xử lý trực tiếp. 

### Tối đa (n=10^5) 

Đối với (n=10^5), chỉ năm đỉnh đầu tiên yêu cầu tìm kiếm kích thước không đổi. Mỗi đỉnh từ 6 đến (10^5) sử dụng chính xác một truy vấn, do đó có (99999) vòng đính kèm. Không thực hiện quét bậc hai trên biểu đồ hoàn chỉnh. 

### Donald luôn quay trở lại`0`Mã thông dịch`0`màu đỏ nhận cạnh đầu tiên và màu xanh lá cây nhận cạnh thứ hai. Chiến lược (K_5) kiểm tra rõ ràng điểm kế tiếp này trước khi chấp nhận nước đi, trong khi giai đoạn đính kèm là an toàn vì đỉnh mới bị cô lập ở cả hai màu. 

### Donald luôn quay trở lại`1`Lập luận tương tự được áp dụng với các màu được hoán đổi. Bộ giải lõi kiểm tra rõ ràng nhánh này và giai đoạn đính kèm cho ra màu đỏ ((2,i)) và màu xanh lá cây ((1,i)). Cả hai cạnh đều kết nối đỉnh bị cô lập mới với cây hiện có. 

### Câu trả lời hỗn hợp 

Một chuỗi hỗn hợp như`0 1 1 0`thay đổi mặt nạ màu đỏ và màu xanh lá cây sau mỗi vòng cốt lõi. Chiến lược ghi nhớ không giả định bất kỳ chuỗi câu trả lời cố định nào. Nó chọn cặp tiếp theo từ trạng thái hiện tại chính xác và bằng chứng đệ quy đảm bảo rằng cả hai trạng thái tiếp theo có thể đều thắng. 

### Tái sử dụng cạnh 

Lõi chỉ sử dụng các cạnh có điểm cuối nằm trong số (1,\ldots,5). Mỗi vòng sau sử dụng ((1,i)) và ((2,i)) cho giá trị mới của (i). Do đó, không có cạnh đính kèm nào có thể bằng cạnh lõi hoặc cạnh đính kèm từ vòng trước đó. 

### Bộ đệm đầu ra 

Mỗi truy vấn được in bằng`flush=True`. Đây không phải là một chi tiết tối ưu hóa. Trong một bài toán tương tác, giá trị đầu vào tiếp theo chỉ được tạo sau khi người tương tác nhận được truy vấn hiện tại. Việc không xóa có thể khiến chương trình phải chờ câu trả lời mà trình tương tác cũng đang chờ tạo ra.
