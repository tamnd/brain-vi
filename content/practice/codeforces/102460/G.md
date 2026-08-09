---
title: "CF 102460G - Lựa chọn tối ưu"
description: "Chúng ta có một mảng gồm (n) giá trị riêng biệt, nhưng bản thân chúng ta không được cung cấp các giá trị đó. Thay vào đó, trước khi thuật toán lựa chọn bắt đầu, chúng ta đã biết kết quả của một số phép so sánh. Với mỗi cặp ((x,y)), chúng ta biết rằng (a[x] < a[y])."
date: "2026-08-09T18:27:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 304
verified: true
draft: false
---

[CF 102460G - Lựa chọn tối ưu](https://codeforces.com/problemset/problem/102460/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 4 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng gồm (n) giá trị riêng biệt, nhưng bản thân chúng ta không được cung cấp các giá trị đó. Thay vào đó, trước khi thuật toán lựa chọn bắt đầu, chúng ta đã biết kết quả của một số phép so sánh. Với mỗi cặp ((x,y)), chúng ta biết rằng (a[x] < a[y]). 

Nhiệm vụ không phải là tìm phần tử thứ (k) cho một mảng cụ thể. Chúng tôi được yêu cầu số lượng so sánh bổ sung trong trường hợp xấu nhất có thể nhỏ nhất cần thiết cho bất kỳ thuật toán dựa trên so sánh nào để xác định phần tử mảng nhỏ nhất thứ (k), giả sử thuật toán có thể sử dụng miễn phí tất cả các so sánh đã biết trước đó. 

Một cách hữu ích để suy nghĩ về đầu vào là một tập hợp được sắp xếp một phần. Mọi so sánh đã biết đều đưa ra một mối quan hệ có hướng (x<y) và tính bắc cầu mang lại nhiều mối quan hệ miễn phí hơn. Ví dụ: nếu chúng ta biết (0<1) và (1<2), thì chúng ta cũng biết (0<2), mặc dù cặp đó chưa bao giờ được đưa ra một cách rõ ràng. 

Câu trả lời là chiều cao của cây quyết định so sánh tốt nhất có thể. Tại mỗi nút bên trong, thuật toán chọn hai phần tử hiện không thể so sánh được và so sánh chúng. Hai kết quả có thể xảy ra sẽ dẫn đến hai vấn đề con nhỏ hơn. Chi phí của một nút bằng một cộng với chi phí lớn hơn của hai nút con của nó vì đầu vào có thể dẫn đến kết quả tồi tệ hơn. 

Giới hạn nhỏ (n\le 8) thay đổi hoàn toàn cách tiếp cận dự định. Có nhiều nhất (8!=40320) tổng số thứ tự có thể có của mảng, vì vậy chúng ta có thể biểu diễn rõ ràng mọi thứ tự đầu vào có thể có. Điều này sẽ là vô vọng đối với (n) lớn hơn, nhưng ở đây nó cho phép chúng ta biến bài toán so sánh thành một bài toán cực tiểu ở trạng thái hữu hạn chính xác. Giới hạn (\ell\le n) có nghĩa là thông tin được biết ban đầu cũng nhỏ, mặc dù tính bắc cầu có thể làm cho thông tin đó mạnh hơn đáng kể so với số lượng cặp đã cho. 

Một giải pháp bất cẩn có thể thất bại khi nó chỉ xử lý các quan hệ đã cho rõ ràng như đã biết. Ví dụ,```
3 1 2
0 1
1 2
```có câu trả lời`0`. Vì (0<1<2) nên phần tử đầu tiên được coi là nhỏ nhất. Việc triển khai bỏ qua tính bắc cầu có thể cho rằng một sự so sánh khác là cần thiết một cách không chính xác. 

Một trường hợp khó phát hiện khác là khi phần tử mong muốn không được xác định duy nhất mặc dù đã biết một số quan hệ. Vì```
3 2 1
0 1
```câu trả lời là`2`. Nếu (2<0), thứ tự là (2<0<1), vậy đáp án là (0). Nếu (0<2) thì ta vẫn cần so sánh (1) và (2). Việc triển khai bất cẩn chỉ đơn giản là đếm số lượng phần tử được biết bên dưới mỗi ứng cử viên có thể dừng quá sớm mà không xem xét cả hai kết quả có thể xảy ra của một so sánh trong tương lai. 

Đầu vào không chứa giá trị số thực tế, do đó trường hợp thử nghiệm có giá trị bằng nhau là không có ý nghĩa. Vấn đề giả định rõ ràng rằng tất cả các phần tử mảng là khác biệt và thuật toán dựa vào tổng thứ tự là một hoán vị không có ràng buộc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là liệt kê các chiến lược so sánh có thể có. Có thể có (n(n-1)/2) cặp không có thứ tự, nhiều nhất là 28 khi (n=8). Chiến lược là một cây quyết định nhị phân thích ứng có các nút bên trong chọn một cặp để so sánh và các lá của nó xác định phần tử thứ (k). Việc tìm kiếm tất cả những cây như vậy là quá tốn kém. Ngay cả khi chúng ta giới hạn bản thân ở các chuỗi trong đó mỗi cặp được sử dụng nhiều nhất một lần thì chỉ riêng lớp sâu nhất đã chứa 

[ 
28! = 
304888344611713860501504000000 
] 

trình tự so sánh khác nhau. Cây quyết định thích ứng chứa nhiều khả năng hơn theo cấp số nhân vì mọi so sánh có thể tạo ra hai trạng thái khác nhau. 

Tuy nhiên, ý tưởng vũ phu có cấu trúc khái niệm phù hợp. Sự so sánh không tiết lộ một thông tin tùy ý. Nó chỉ đơn giản là thêm một mối quan hệ giữa hai phần tử. Sau nhiều lần so sánh, tất cả thông tin thu thập được cho đến nay chỉ là một phần trật tự. Tương lai chỉ phụ thuộc vào tổng số đơn đặt hàng nào vẫn nhất quán với đơn đặt hàng một phần đó. 

Quan sát đó cho phép chúng ta thay thế không gian khổng lồ của cây quyết định bằng lập trình động trên các trạng thái thông tin. Thay vì lưu trữ trực tiếp một phần thứ tự, chúng ta lưu trữ tập hợp tất cả các hoán vị tổng phù hợp với nó. Chỉ có 40320 hoán vị, vì vậy bộ này tự nhiên phù hợp với một bitset. 

Giả sử một trạng thái chứa một số tập hợp (S) các hoán vị có thể có. Đối với một cặp (x,y), tính toán trước tập hợp bit (M_{x,y}) chứa mọi hoán vị trong đó (x<y). So sánh (x) và (y) chia trạng thái thành 

[ 
S_1=S\cap M_{x,y} 
] 

và 

[ 
S_2=S\setminus M_{x,y}. 
] 

Nếu một bên trống thì việc so sánh không đưa ra thông tin mới và vô ích. Nếu không thì đó là một so sánh tiếp theo hợp pháp. 

Trạng thái kết thúc khi mọi hoán vị còn lại đặt cùng một phần tử vào vị trí (k). Tại thời điểm đó, câu trả lời đã được xác định và chi phí bổ sung là bằng không. 

Điều này mang lại sự tái diễn chính xác 

[ 
dp(S)= 
1+\min_{x,y} 
\max\left(dp(S_1),dp(S_2)\right), 
] 

trong đó mức tối thiểu vượt quá các so sánh thực sự phân chia (S). 

Phép truy hồi là chính xác vì mọi thuật toán dựa trên so sánh có thể phải chọn một số cặp ở trạng thái hiện tại và sau so sánh đó, đối thủ có thể chọn bất kỳ kết quả nào có chi phí còn lại lớn hơn. Ngược lại, việc chọn cặp tốt nhất theo phép truy toán sẽ xây dựng cây quyết định có chiều cao chính xác trong trường hợp xấu nhất đó. 

Sự khác biệt so với cách tiếp cận bạo lực là nhiều lịch sử so sánh khác nhau dẫn đến cùng một trạng thái thông tin. Ghi nhớ chỉ đánh giá trạng thái như vậy một lần. Các số nguyên Python đặc biệt hữu ích ở đây vì các thử nghiệm giao, bù và trống trên trạng thái 40320 bit được triển khai dưới dạng các phép toán số nguyên lớn được tối ưu hóa cao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ của cây quyết định, với (28!) chuỗi so sánh đã ở cấp độ chuỗi sâu nhất | Hàm mũ | Quá chậm | 
| Tối ưu | (O(Sn^2B)), trong đó (S) là số trạng thái được ghi nhớ và (B=40320) bit cấp máy | (O(SB)) | Đã chấp nhận | 

Giới hạn của (S) không phải là đa thức, nhưng đó là sự cố ý. Bài toán yêu cầu một mức tối ưu chính xác và giới hạn (n) đến 8 một cách chính xác để có thể thực hiện được việc tìm kiếm trạng thái hữu hạn này. 

## Hướng dẫn thuật toán

1. Tạo mọi hoán vị của phần tử (n). Một hoán vị thể hiện một thứ tự hoàn chỉnh có thể có của mảng từ nhỏ nhất đến lớn nhất. Vì (n\le8) nên có nhiều nhất 40320 hoán vị như vậy. 
2. Gán một bit cho mỗi hoán vị. Trạng thái DP khi đó là một số nguyên Python có các bit được đặt chính xác là các hoán vị vẫn nhất quán với tất cả thông tin đã biết cho đến nay. 
3. Đối với mỗi cặp phần tử không có thứ tự, hãy tính toán trước một tập bit chứa tất cả các hoán vị trong đó phần tử đầu tiên nhỏ hơn phần tử thứ hai. Điều này làm cho phép so sánh trở thành một cặp phép toán bit số nguyên thay vì lặp lại tất cả các hoán vị. 
4. Xây dựng trạng thái ban đầu bằng cách bắt đầu với mọi hoán vị có thể và giao nó với các mặt nạ tương ứng với các quan hệ đã cho. Nếu đầu vào ghi (x<y), chỉ những hoán vị thỏa mãn (x<y) mới tồn tại. 
5. Tính toán trước tám tập hợp bit khác, một tập hợp cho mỗi phần tử câu trả lời có thể có. Mặt nạ cho phần tử (x) chứa chính xác các hoán vị trong đó (x) chiếm vị trí (k). 
6. Đối với trạng thái DP, hãy kiểm tra xem tất cả các hoán vị còn sót lại có đồng ý ở vị trí (k)-th hay không. Nếu trạng thái được chứa trong một dấu hiệu trả lời, hãy trả về 0 vì không cần so sánh thêm. 
7. Ngược lại, hãy xem xét từng cặp phần tử. Giao trạng thái hiện tại với mặt nạ cặp để thu được các hoán vị trong đó kết quả so sánh là (x<y). Phần bổ sung đưa ra các hoán vị trong đó (y<x). 
8. Bỏ qua một cặp nếu một trong hai trạng thái kết quả của nó trống. Sự so sánh như vậy đã được ngụ ý bởi thông tin ở trạng thái hiện tại và không thể làm giảm sự không chắc chắn. 
9. Giải đệ quy cả hai trạng thái kết quả. Bản thân việc so sánh tốn một chi phí và kết quả tồi tệ nhất sẽ xác định chi phí còn lại, do đó giá trị ứng cử viên là một cộng với giá trị tối đa của hai chi phí con. 
10. Chọn ứng cử viên tối thiểu trong tất cả các so sánh hữu ích. Ghi nhớ kết quả bằng cách sử dụng tập bit hoán vị hiện tại làm khóa, vì việc tiếp tục tối ưu chỉ phụ thuộc vào thông tin được biểu thị bởi trạng thái đó. 
11. Sử dụng giới hạn dưới dựa trên số phần tử trả lời có thể có. Nếu có (c) các phần tử khác nhau vẫn có thể chiếm vị trí (k), thì ít nhất (\lceil\log_2 c\rceil) cần có thêm các phép so sánh nhị phân. Nếu câu trả lời tốt nhất hiện tại đã bằng giới hạn dưới này thì không có sự so sánh nào khác có thể cải thiện nó, do đó việc tìm kiếm trạng thái đó có thể dừng sớm. 
12. Trong quá trình đánh giá minimax, trước tiên hãy đánh giá đứa trẻ có triển vọng hơn. Nếu chi phí của nó đã đạt đến giá trị tốt nhất hiện tại thì đứa trẻ kia chỉ cần được giới hạn đủ tốt để chứng minh rằng sự so sánh này không thể cải thiện được giá trị tốt nhất hiện tại. Điều này tránh được một lượng đáng kể đệ quy không cần thiết. 

Tại sao nó hoạt động: điều bất biến là trạng thái DP chứa chính xác tổng số đơn hàng phù hợp với mọi so sánh đã biết dọc theo đường dẫn cây quyết định hiện tại. Phép so sánh sẽ phân chia các thứ tự đó thành hai kết quả có thể xảy ra một cách chính xác, do đó, hai trạng thái đệ quy mô tả chính xác các tình huống có thể xảy ra sau phép so sánh đó. Trạng thái kết thúc chính xác khi tất cả các đơn hàng còn sót lại đồng ý về phần tử thứ (k). Do đó, mọi chuyển đổi đệ quy đều tương ứng với một so sánh pháp lý, mỗi trạng thái cuối cùng có một câu trả lời được xác định duy nhất và phép lặp minimax xem xét mọi so sánh tiếp theo có thể xảy ra. Do đó, việc lấy mức tối thiểu trong các lựa chọn đó sẽ mang lại số lượng so sánh trong trường hợp xấu nhất có thể nhỏ nhất. 

## Giải pháp Python```python
import sys
import itertools
from functools import lru_cache

input = sys.stdin.readline

def solve_case(n, k, relations):
    permutations = list(itertools.permutations(range(n)))
    pcount = len(permutations)
    byte_count = (pcount + 7) >> 3

    pairs = []
    pair_id = [[-1] * n for _ in range(n)]

    for i in range(n):
        for j in range(i + 1, n):
            pair_id[i][j] = len(pairs)
            pairs.append((i, j))

    pair_bytes = [bytearray(byte_count) for _ in pairs]
    answer_bytes = [bytearray(byte_count) for _ in range(n)]

    for idx, perm in enumerate(permutations):
        byte_index = idx >> 3
        bit = 1 << (idx & 7)

        answer_bytes[perm[k - 1]][byte_index] |= bit

        pos = [0] * n
        for rank, x in enumerate(perm):
            pos[x] = rank

        for q, (x, y) in enumerate(pairs):
            if pos[x] < pos[y]:
                pair_bytes[q][byte_index] |= bit

    pair_masks = [
        int.from_bytes(bytes(data), "little")
        for data in pair_bytes
    ]
    answer_masks = [
        int.from_bytes(bytes(data), "little")
        for data in answer_bytes
    ]

    full = (1 << pcount) - 1
    initial = full

    for x, y in relations:
        if x < y:
            q = pair_id[x][y]
            initial &= pair_masks[q]
        else:
            q = pair_id[y][x]
            initial &= full ^ pair_masks[q]

    @lru_cache(maxsize=None)
    def possible_answers(state):
        result = 0
        for x, mask in enumerate(answer_masks):
            if state & ~mask == 0:
                result |= 1 << x
        return result

    @lru_cache(maxsize=None)
    def lower_bound(state):
        cnt = possible_answers(state).bit_count()
        if cnt <= 1:
            return 0
        return (cnt - 1).bit_length()

    @lru_cache(maxsize=None)
    def dp(state):
        candidates = possible_answers(state)

        if candidates.bit_count() <= 1:
            return 0

        best = 29
        lb = (candidates.bit_count() - 1).bit_length()

        if lb == best:
            return best

        for pair_mask in pair_masks:
            left = state & pair_mask
            if not left or left == state:
                continue

            right = state ^ left

            first, second = left, right

            # Explore the state with more possible answers first.
            if possible_answers(first).bit_count() < \
                    possible_answers(second).bit_count():
                first, second = second, first

            first_cost = dp(first)

            # The comparison cannot beat best if its first branch
            # is already too expensive.
            if 1 + first_cost >= best:
                continue

            second_lb = lower_bound(second)
            if 1 + max(first_cost, second_lb) >= best:
                continue

            second_cost = dp(second)
            value = 1 + max(first_cost, second_cost)

            if value < best:
                best = value
                if best == lb:
                    break

        return best

    return dp(initial)

def solve_input(data):
    it = iter(data.strip().split())
    n = int(next(it))
    k = int(next(it))
    l = int(next(it))

    relations = []
    for _ in range(l):
        x = int(next(it))
        y = int(next(it))
        relations.append((x, y))

    return str(solve_case(n, k, relations))

def main():
    data = sys.stdin.buffer.read().decode()
    sys.stdout.write(solve_input(data) + "\n")

if __name__ == "__main__":
    main()
```Việc tạo hoán vị tạo ra vô số đầu vào có thể có. Bản thân hoán vị được lưu trữ theo thứ tự tăng dần, vì vậy`perm[k - 1]`chính xác là phần tử sẽ được trả về bằng cách chọn trên đầu vào đó. 

Các bytearrays được sử dụng trong khi xây dựng các mặt nạ vì việc liên tục sửa đổi một số nguyên Python khổng lồ từng bit một sẽ tốn kém một cách không cần thiết. Sau khi xây dựng,`int.from_bytes`chuyển đổi từng bytearray thành một số nguyên Python nhỏ gọn, sau đó tất cả các chuyển đổi trạng thái trở thành các phép toán số nguyên nhanh. 

Mặt nạ cặp mã hóa cả hai kết quả có thể xảy ra của mọi so sánh. Nếu như`pair_mask`đại diện cho (x<y), thì`state & pair_mask`là kết quả đầu tiên và`state ^ left`là kết quả thứ hai bởi vì`state`chỉ chứa các hoán vị hợp lệ. 

Thử nghiệm cuối cùng không yêu cầu xây dựng lại trật tự từng phần. Nếu mọi hoán vị còn sót lại có cùng phần tử ở vị trí (k) thì kết quả lựa chọn đã được cố định. Đây chính xác là điểm mà cây quyết định so sánh được phép dừng lại. 

các`possible_answers`bộ đệm tránh quét liên tục tất cả tám mặt nạ trả lời cho cùng một trạng thái. Bit (x) của nó được đặt chính xác khi phần tử (x) vẫn có thể là thống kê thứ tự được yêu cầu. 

Không có mối lo ngại về tràn số nguyên trong Python. Trạng thái lớn nhất chỉ là 40320 bit và số nguyên Python xử lý trực tiếp độ chính xác tùy ý. Độ sâu đệ quy nhiều nhất là số lượng so sánh riêng biệt, là 28 cho (n=8). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 2 0
```Ban đầu mọi hoán vị của`0, 1, 2`là có thể. Cả ba phần tử vẫn có thể là phần tử nhỏ thứ hai. 

| Tiểu bang | Yếu tố thứ 2 có thể có | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| Tất cả 6 hoán vị |`{0,1,2}`| So sánh`0`Và`1`| Hai trạng thái | 
|`0 < 1`|`{0,1,2}`| So sánh`0`Và`2`| Hai trạng thái | 
|`2 < 0 < 1`|`{0}`| Dừng lại | Chi phí 0 | 
|`0 < 2`Và`0 < 1`|`{1,2}`| So sánh`1`Và`2`| Hai trạng thái đầu cuối | 

So sánh đầu tiên có giá một, so sánh thứ hai có giá một và ở nhánh nơi`0<2`sự so sánh thứ ba là cần thiết. Do đó chi phí trong trường hợp xấu nhất là`3`. 

DP kiểm tra tất cả các so sánh đầu tiên có thể có và tìm thấy cùng một giá trị, do đó đầu ra là```
3
```Ví dụ này chứng tỏ tại sao việc xác định câu trả lời lại yếu hơn việc xác định thứ tự hoàn chỉnh. Sau khi học`0<1`, phần tử nhỏ thứ hai vẫn chưa được biết đến, mặc dù mối quan hệ đã khá hạn chế. 

### Mẫu 2 

Đầu vào là```
7 2 5
0 6
3 6
4 6
2 0
0 5
```Các mối quan hệ ban đầu ngụ ý 

[ 
2<0<5,\qquad 0<6,\qquad 3<6,\qquad 4<6. 
] 

các yếu tố`5`Và`6`không thể nhỏ thứ hai vì ít nhất đã có hai phần tử đứng trước chúng. Do đó, các phần tử nhỏ thứ hai có thể có là`0`,`1`,`2`,`3`, Và`4`. 

| Tiểu bang | Quan hệ đã biết | Yếu tố thứ 2 có thể có | Giới hạn dưới | 
| --- | --- | --- | --- | 
| Ban đầu |`0<6, 3<6, 4<6, 2<0, 0<5`|`{0,1,2,3,4}`| 3 | 
| Sau bất kỳ sự so sánh hữu ích nào | Quan hệ ban đầu cộng với một quan hệ mới | Tập hợp con của`{0,1,2,3,4}`| Tính toán lại | 
| Trạng thái đầu cuối | Tất cả các đơn hàng còn sót lại đều đồng ý ở vị trí 2 | Một phần tử | 0 | 

Giới hạn dưới của 3 chỉ là giới hạn thông tin. Mối quan hệ giữa các ứng viên và các phần tử bên ngoài tập ứng viên làm cho một số câu hỏi nhị phân trở nên kém cân bằng hơn nhiều so với các giả định ràng buộc. DP minimax kiểm tra mọi so sánh hữu ích và tính đến kết quả tồi tệ nhất theo cách đệ quy. 

Tối ưu chính xác là`5`, vì vậy đầu ra là```
5
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(Sn^2B)) | (S) trạng thái thông tin được ghi nhớ, (O(n^2)) so sánh có thể có trên mỗi trạng thái và (B=40320) bit được xử lý bởi các phép toán số nguyên lớn của Python | 
| Không gian | (O(SB)) | Mỗi trạng thái được ghi nhớ là một số nguyên 40320 bit, có thêm bộ nhớ cho cặp và mặt nạ trả lời | 

Phần giai thừa là lý do giải pháp bị giới hạn ở (n\le8). Tại (n=8), chỉ có tổng số 40320 đơn đặt hàng, do đó, một trạng thái phù hợp với khoảng 5 KB dưới dạng bitset thô. Việc tìm kiếm không bao giờ khám phá các mảng số tùy ý, chỉ khám phá tập hợp hữu hạn các thứ tự tương đối có thể có. Việc ghi nhớ và phát hiện thiết bị đầu cuối sẽ dừng tìm kiếm ngay khi buộc phải thống kê đơn hàng được yêu cầu. 

Các ràng buộc của bài toán được công bố là (n\le8), (1\le k\le n) và (\ell\le n), đây chính xác là các giới hạn giúp cho việc tìm kiếm chính xác trong không gian trạng thái này trở nên khả thi. 

## Trường hợp thử nghiệm 

Vấn đề không có giá trị mảng trong đầu vào của nó, do đó không thể xây dựng phép thử "tất cả các giá trị bằng nhau". Các giá trị bằng nhau cũng bị cấm bởi giả định về tính khác biệt của câu lệnh. Thay vào đó, các thử nghiệm sau đây bao gồm các trường hợp nhỏ nhất, trạng thái đã được giải quyết, thống kê thứ tự biên và giá trị tối đa của (n).```
# These tests assume solve_input(data) is the function
# from the solution above.

def run(inp: str) -> str:
    return solve_input(inp).strip()

# Provided sample 1
assert run(
    """3 2 0
"""
) == "3", "sample 1"

# Provided sample 2
assert run(
    """7 2 5
0 6
3 6
4 6
2 0
0 5
"""
) == "5", "sample 2"

# Minimum-size input: one element is already the answer.
assert run(
    """1 1 0
"""
) == "0", "minimum n"

# Two elements, smallest element unknown.
assert run(
    """2 1 0
"""
) == "1", "two elements without information"

# Two elements, complete information already known.
assert run(
    """2 2 1
0 1
"""
) == "0", "already determined maximum"

# Boundary order statistic with transitive information.
# 0 < 1 < 2, so the minimum is known without extra comparisons.
assert run(
    """3 1 2
0 1
1 2
"""
) == "0", "transitive minimum"

# Maximum-size input with a complete chain.
# The 4th smallest element is already determined.
assert run(
    """8 4 7
0 1
1 2
2 3
3 4
4 5
5 6
6 7
"""
) == "0", "maximum n and complete order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 0`|`3`| Cung cấp ví dụ trung bình không có thông tin trước | 
|`7 2 5`với năm quan hệ đã cho |`5`| Đã cung cấp trạng thái đặt hàng một phần | 
|`1 1 0`|`0`| Tối thiểu có thể (n) | 
|`2 1 0`|`1`| Trường hợp ranh giới có chính xác một so sánh chưa biết | 
|`2 2 1`với`0 1`|`0`| Thống kê đơn hàng đã được xác định | 
|`3 1 2`với`0 1`,`1 2`|`0`| Tính chuyển tiếp | 
|`8 4 7`tạo thành một chuỗi hoàn chỉnh |`0`| Câu trả lời tối đa (n) và đã biết | 

## Vỏ cạnh 

Khi (n=1), chỉ có một hoán vị có thể xảy ra và phần tử duy nhất tự động là phần tử nhỏ nhất thứ (k). Trạng thái ban đầu đã được chứa trong một mặt nạ trả lời, do đó DP trả về`0`. 

Vì```
2 1 0
```cả hai hoán vị đều có thể. Yếu tố`0`là mức tối thiểu trong một hoán vị và phần tử`1`là mức tối thiểu trong cái kia. Một so sánh phân biệt hai trường hợp, do đó DP trả về`1`. 

Vì```
2 2 1
0 1
```thứ tự nhất quán duy nhất là`0<1`. Mọi vị trí hoán vị còn sót lại`1`thứ hai, vậy là thử nghiệm cuối cùng thành công ngay lập tức và câu trả lời là`0`. Điều này mắc phải lỗi phổ biến khi tính phí cho các so sánh đã được cung cấp trong đầu vào. 

Vì```
3 1 2
0 1
1 2
```các mối quan hệ được cung cấp rõ ràng ngụ ý`0<2`bởi tính quá độ. Do đó, mọi hoán vị nhất quán có`0`Đầu tiên. Trạng thái chỉ chứa các hoán vị có phần tử đầu tiên là`0`, do đó DP dừng ngay lập tức với`0`. 

Vì```
3 2 1
0 1
```mối quan hệ`0<1`không đủ để xác định số trung vị. Nếu như`2<0`, thứ tự là`2<0<1`và câu trả lời là`0`. Nếu như`0<2`, trung vị là giá trị nào nhỏ hơn giữa`1`Và`2`, vì vậy cần phải có thêm một so sánh nữa. DP giữ cả hai trạng thái thay vì cho rằng yếu tố bị ràng buộc nhất hiện nay phải là câu trả lời. 

Đối với trường hợp kích thước tối đa,```
8 4 7
0 1
1 2
2 3
3 4
4 5
5 6
6 7
```tính bắc cầu xác định thứ tự hoàn chỉnh`0<1<2<3<4<5<6<7`. Do đó, phần tử nhỏ thứ tư là`3`. Tất cả các hoán vị nhất quán đã đồng ý ở vị trí thứ tư, do đó không thực hiện so sánh đệ quy và kết quả là`0`.
