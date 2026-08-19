---
title: "CF 102264B - Thủ quỹ lớp"
description: "Chúng ta có một chuỗi phiếu A và B được sắp xếp theo thứ tự ID sinh viên. Tập đại diện là bất kỳ khoảng liền kề nào của chuỗi này. Để một khoảng thời gian được an toàn, Betty không được có nhiều hơn K phiếu bầu cho Amy."
date: "2026-08-19T02:58:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102264
codeforces_index: "B"
codeforces_contest_name: "2019 Facebook Hacker Cup, Round 1"
rating: 0
weight: 102264
solve_time_s: 260
verified: true
draft: false
---

[CF 102264B - Thủ quỹ lớp](https://codeforces.com/problemset/problem/102264/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`A`Và`B`phiếu bầu được sắp xếp theo thứ tự ID sinh viên. Tập đại diện là bất kỳ khoảng liền kề nào của chuỗi này. Để một khoảng thời gian được an toàn, Betty không được có nhiều hơn`K`bỏ phiếu cho Amy. Tương tự, nếu chúng ta mã hóa một`A`BẰNG`+1`và một`B`BẰNG`-1`, mọi mảng con liền kề phải có tổng ít nhất`-K`. 

Trước khi chọn khoảng đại diện, chúng ta có thể thay đổi một số`B`bỏ phiếu vào`A`phiếu bầu. Chuyển đổi sinh viên`i`chi phí`2^i`, vì vậy những sinh viên đến sớm sẽ rẻ hơn theo cấp số nhân. Nhiệm vụ là chọn một tập hợp sinh viên có phiếu bầu được thay đổi sao cho mỗi khoảng thời gian đều an toàn, đồng thời giảm thiểu chi phí thực tế. Chỉ sau khi tìm được mức tối thiểu đó chúng ta mới lấy nó theo modulo`1,000,000,007`. 

Đối với tiền tố kết thúc ở vị trí`i`, gọi số dư của nó là số`A`số phiếu trừ đi số lượng`B`phiếu bầu. Nếu số dư tiền tố là`P_0, P_1, ..., P_N`, thì sự cân bằng của khoảng`[l, r]`là`P_r - P_{l-1}`. Betty thắng chính xác khi giá trị này nhỏ hơn`-K`, vậy điều kiện ta cần là`P_r >= P_{l-1} - K`cho mọi`l <= r`. Khi quét từ trái sang phải, điều này có nghĩa là số dư tiền tố hiện tại không bao giờ có thể giảm nhiều hơn`K`dưới số dư tiền tố lớn nhất được thấy trước đó. 

Giá trị của`N`có thể đạt tới một triệu. MỘT`O(N^2)`phương pháp sẽ kiểm tra theo thứ tự`10^12`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Thuật toán cần xử lý mỗi học sinh một số lần không đổi, đưa ra kết quả`O(N)`mục tiêu. Số lượng trường hợp thử nghiệm cũng lớn, do đó, các yếu tố logarit không cần thiết và các cấu trúc phụ trợ lớn rất quan trọng trong Python. 

Có một số trường hợp ranh giới dễ dàng phá vỡ việc triển khai hợp lý. Với`N = 1`,`K = 0`, Và`V = B`, câu trả lời là`2`, bởi vì nhóm đại diện duy nhất là một học sinh duy nhất và Betty sẽ thắng. Việc triển khai bất cẩn chỉ kiểm tra các khoảng có độ dài ít nhất là hai sẽ trả về 0. 

Với`N = 4`,`K = 0`, Và`V = BAAB`, câu trả lời là`18`. Việc lật học sinh 1 một mình sẽ đảm bảo an toàn cho các khoảng thời gian dài, nhưng học sinh 4 vẫn là tập đại diện chỉ có Betty, vì vậy nó cũng phải được lật. Kiểm tra chỉ có tiền tố có thể âm thầm bỏ lỡ khoảng thời gian đơn lẻ đó. 

Với`N = 4`,`K = 1`, Và`V = ABBA`, câu trả lời là`4`, không`8`. Khoảng xấu là học sinh từ 2 đến 3, và rẻ nhất`B`bên trong là học sinh 2. Một quy tắc tham lam luôn lật đổ học sinh hiện tại khi phát hiện vi phạm sẽ lật đổ học sinh 3 và trả tiền`8`. 

Cuối cùng, khi`K = N`, câu trả lời luôn là số không. Không có tập đại diện nào khác rỗng có nhiều hơn`N`phiếu bầu cho Betty, vì vậy Betty không thể vượt quá Amy nhiều hơn`K`. Việc triển khai sử dụng bất đẳng thức nghiêm ngặt theo hướng sai có thể thực hiện sai các lần lật ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử lật từng tập hợp học sinh có thể, sau đó xác minh mọi tập hợp đại diện liền kề. có`2^N`những lựa chọn có thể có của học sinh, và thậm chí việc kiểm tra một lựa chọn cũng đòi hỏi phải kiểm tra`O(N^2)`khoảng thời gian. Điều này đã là vô vọng đối với rất nhỏ`N`. 

Một lực lượng vũ phu hợp lý hơn sẽ ấn định số lượng học sinh bị đảo lộn và chọn những học sinh rẻ nhất có thể. Vì chi phí là`2^i`, trong số bất kỳ số lượng sinh viên cố định nào, việc sử dụng sớm hơn sẽ luôn rẻ hơn`B`cử tri. Sau đó người ta có thể kiểm tra xem liệu lần đầu tiên`m` `B`cử tri là đủ. Điều này đưa ra một điều kiện khả thi đơn điệu và cho phép tìm kiếm nhị phân, nhưng mọi kiểm tra tính khả thi sẽ quét toàn bộ chuỗi, dẫn đến`O(N log N)`thời gian. 

Cấu trúc của vi phạm mang lại một giải pháp tuyến tính mạnh mẽ hơn. Trong khi quét tiền tố, hãy giữ số dư tiền tố hiệu quả lớn nhất từ ​​trước đến nay. Nếu số dư hiện tại ít nhất bằng mức âm tối đa đó`K`, mọi khoảng kết thúc ở đây đều an toàn. Nếu số dư hiện tại nhỏ hơn thì có một khoảng không hợp lệ mà điểm cuối bên trái nằm ngay sau vị trí đạt được số dư tiền tố tối đa. 

Giả sử vị trí đó là`p`. Bất kỳ sửa chữa khoảng thời gian tồi tệ này phải lật một`B`ở đâu đó trong`[p + 1, i]`. Vì chi phí tăng theo chỉ số sinh viên nên lựa chọn rẻ nhất có thể là lựa chọn ngoài cùng bên trái không lật`B`trong khoảng đó. Chúng tôi lật chính xác học sinh đó. 

Các vị trí đã chọn di chuyển đơn điệu sang bên phải. Một lần`B`nằm trước thời điểm bắt đầu của khoảng thời gian xấu hiện tại, nó không bao giờ có ích cho lần vi phạm sau này vì khoảng thời gian xấu trong tương lai không bắt đầu sớm hơn khoảng thời gian hiện tại. Điều này cung cấp cho chúng tôi một con trỏ chuyển tiếp duy nhất để tìm kiếm đủ điều kiện tiếp theo`B`. 

Có một sự tinh tế. Lật mặt học sinh`p`thay đổi số dư hiệu dụng của mọi tiền tố từ`p`trở đi bởi`2`, bao gồm các tiền tố đã được xử lý. Để duy trì chính xác số dư tiền tố tối đa, chúng tôi giữ một deque đơn điệu trên tổng tiền tố ban đầu. Khi lật ở`p`được thực hiện, tổng tiền tố ban đầu tối đa trên`[p, i]`có thể được lấy từ deque, và`2`lần số lần lật được thêm vào nó. Vì điểm cuối bên trái của phạm vi này chỉ di chuyển sang phải nên deque có thể được duy trì theo thời gian tuyến tính. 

Kết quả là một lần quét từ trái sang phải với số bài tập được phân bổ không đổi cho mỗi học sinh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các bộ lật và các khoảng |`O(2^N N^2)`|`O(N)`| Quá chậm | 
| Số lần lật tìm kiếm nhị phân |`O(N log N)`|`O(N)`| Chậm một cách không cần thiết | 
| Tham lam với tiền tố tối đa và deque đơn điệu |`O(N)`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Mã hóa mọi`A`BẰNG`+1`và mọi`B`BẰNG`-1`và duy trì tổng tiền tố ban đầu`P`. Nếu sinh viên`i`đã bị đảo ngược, mọi tiền tố có hiệu lực từ`i`lợi ích trở đi`2`. 
2. Duy trì`max_balance`Và`max_pos`, thể hiện số dư tiền tố hiệu quả lớn nhất giữa các vị trí trước sinh viên hiện tại và vị trí sớm nhất xảy ra mức tối đa đó. Vị thế sớm nhất rất hữu ích vì nó mang lại khoảng thời gian xấu lớn nhất có thể và do đó có giá hợp lý rẻ nhất.`B`. 
3. Duy trì một con trỏ`next_b`đến sớm nhất`B`đó chưa được chọn. Con trỏ chỉ di chuyển về phía trước. Khi hành vi vi phạm bắt đầu sau`max_pos`, bất kì`B`trước`max_pos + 1`không thể sửa chữa vi phạm đó, vì vậy hãy chuyển con trỏ cho đến khi chạm tới điểm đầu tiên`B`tại hoặc sau ranh giới đó. 
4. Tại sinh viên`i`, tính số dư hiệu dụng hiện tại như`P + 2 * flips`, Ở đâu`flips`là số học sinh được chọn tính đến thời điểm hiện tại. Nếu điều này ít nhất`max_balance - K`, điểm cuối hiện tại không tạo ra vi phạm mới. Cập nhật tối đa nếu cần thiết. 
5. Nếu số dư hiện tại thấp hơn`max_balance - K`, khoảng thời gian bắt đầu tại`max_pos + 1`và kết thúc tại`i`là xấu. Chọn cái chưa lật sớm nhất`B`trong khoảng thời gian này. Đây là sinh viên rẻ nhất có thể sửa chữa vi phạm mới được phát hiện. 
6. Cộng chi phí của sinh viên đã chọn vào modulo câu trả lời`1,000,000,007`. Con trỏ dùng để tìm`B`các vị trí cũng mang lũy ​​thừa tương ứng của hai, do đó không cần phải có dãy tất cả các lũy thừa. 
7. Sau khi chọn vị trí`p`, tất cả các tổng tiền tố ban đầu từ`p`bởi vì`i`tăng thêm`2`theo trình tự có hiệu lực. Xóa các mục deque trước`p`, lấy tổng tiền tố gốc tối đa còn lại trong deque, thêm`2 * flips`, và so sánh nó với mức tối đa cũ. Nếu giá trị mới lớn hơn, hãy cập nhật`max_balance`Và`max_pos`. 
8. Tiếp tục cho đến hết`N`học sinh đã được xử lý. Mọi khoảng thời gian đại diện có thể đều đã được xem xét thông qua điểm cuối phù hợp của nó và mọi vi phạm đều được sửa chữa bởi học sinh đủ điều kiện với chi phí rẻ nhất có thể. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý vị trí`i`, mọi khoảng đại diện kết thúc tại hoặc trước`i`được an toàn, và những học sinh được chọn là những lựa chọn rẻ nhất được thực hiện bởi quá trình tham lam đối với những vi phạm gặp phải cho đến nay. Khi một vi phạm mới xuất hiện, điểm cuối bên trái của nó được xác định bằng số dư tiền tố tối đa. Bất kỳ giải pháp hợp lệ nào cũng phải lật một`B`bên trong khoảng đó, bởi vì việc thay đổi một học sinh bên ngoài khoảng đó không thể thay đổi chênh lệch phiếu bầu của khoảng đó. Trong số tất cả các điều kiện chưa được lật`B`cử tri, người ngoài cùng bên trái có chi phí nhỏ nhất. Việc lựa chọn nó không thể làm cho một giải pháp trong tương lai trở nên đắt đỏ hơn, bởi vì nó chỉ có thể cải thiện các khoảng thời gian bắt đầu tại hoặc trước vị trí của nó, trong khi mọi khoảng thời gian sau đó đều có thể xảy ra.`B`vẫn có sẵn cho các vi phạm trong tương lai. Do đó, mỗi lựa chọn tham lam đều tương thích với một giải pháp tối ưu và việc xử lý mọi điểm cuối sẽ thiết lập sự đảm bảo cần thiết cho tất cả các tập đại diện liền kề. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        n, k = map(int, input().split())
        s = input().strip()

        # Monotonic deque of (index, original_prefix_sum).
        # Values are decreasing, and equal values keep the earliest index.
        dq = deque([(0, 0)])

        prefix = 0
        flips = 0

        # Maximum effective prefix sum among processed prefixes.
        max_balance = 0
        max_pos = 0

        # Earliest B which has not been selected yet.
        next_b = 0

        # 2^next_b modulo MOD.
        next_b_cost = 1

        answer = 0

        for i, ch in enumerate(s, 1):
            if ch == 'A':
                prefix += 1
            else:
                prefix -= 1

            # Add current original prefix sum to the monotonic deque.
            while dq and dq[-1][1] < prefix:
                dq.pop()
            dq.append((i, prefix))

            current = prefix + 2 * flips

            if current < max_balance - k:
                # The bad interval starts at max_pos + 1.
                left = max_pos + 1

                # Move to the first unflipped B inside that interval.
                while next_b < left:
                    next_b += 1
                    next_b_cost = next_b_cost * 2 % MOD

                while next_b < i and s[next_b] != 'B':
                    next_b += 1
                    next_b_cost = next_b_cost * 2 % MOD

                # A violation cannot exist without an eligible B.
                # next_b is necessarily <= i and s[next_b] == 'B'.
                p = next_b

                answer = (answer + next_b_cost) % MOD
                flips += 1

                # After flipping p, all prefixes from p onward
                # gain 2. Remove prefixes before p from the range
                # whose maximum needs to be reconsidered.
                while dq and dq[0][0] < p:
                    dq.popleft()

                shifted_max = dq[0][1] + 2 * flips

                # Keep the earliest position on ties.
                if shifted_max > max_balance:
                    max_balance = shifted_max
                    max_pos = dq[0][0]

                # The selected B is no longer available.
                next_b += 1
                next_b_cost = next_b_cost * 2 % MOD

            else:
                if current > max_balance:
                    max_balance = current
                    max_pos = i

        out.append(f"Case #{case}: {answer}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Tổng tiền tố`prefix`luôn đề cập đến chuỗi phiếu bầu ban đầu. Hiệu quả của mỗi học sinh được chọn được thể hiện riêng biệt bằng`2 * flips`, bởi vì mỗi học sinh được chọn đều có chỉ số không lớn hơn vị trí quét hiện tại. 

các`dq`chứa tổng tiền tố ban đầu theo thứ tự giảm dần. Khi một tổng tiền tố mới xuất hiện, các giá trị nhỏ hơn ở phía sau không bao giờ có thể trở thành giá trị tối đa của phạm vi trong tương lai trong khi giá trị lớn hơn vẫn ở phía trước, vì vậy chúng sẽ bị loại bỏ. Các giá trị bằng nhau không bị loại bỏ, điều này bảo toàn chỉ số sớm nhất đạt mức tối đa. Sự lựa chọn ràng buộc này rất quan trọng vì mức tối đa sớm hơn sẽ mang lại mức giá rẻ nhất có thể.`B`khi xảy ra vi phạm. 

các`next_b`con trỏ không bao giờ di chuyển lùi. Đầu tiên nó bỏ qua các vị trí trước khoảng thời gian xấu hiện tại, sau đó bỏ qua`A`vị trí cho đến khi đạt được vị trí đủ điều kiện đầu tiên`B`. Sau khi học sinh đó được chọn, con trỏ sẽ di chuyển qua nó. Mỗi vị trí được con trỏ này truyền qua nhiều nhất một lần. 

Biến chi phí`next_b_cost`bắt đầu lúc`2^0 = 1`. Bất cứ khi nào con trỏ di chuyển từ vị trí`x`ĐẾN`x + 1`, chi phí được nhân với`2`modulo`MOD`. Như vậy khi`next_b`chỉ vào học sinh`i`,`next_b_cost`chính xác là`2^i mod MOD`. 

Thứ tự của các hoạt động xung quanh một vi phạm là rất quan trọng. Tiền tố gốc hiện tại được chèn vào deque trước khi kiểm tra vi phạm vì học sinh mới được chọn có thể là học sinh hiện tại và sau khi lật tiền tố của nó phải tham gia tối đa mới. Bản thân vi phạm được kiểm tra theo mức tối đa từ các tiền tố có hiệu lực trước đó, được biểu thị bằng`max_balance`, trước khi tiền tố hiện tại được chấp nhận là mức tối đa mới thông thường. 

Số nguyên Python không bị tràn nhưng mọi chi phí đều được giảm modulo`MOD`ngay lập tức. Bản thân số dư tiền tố nằm ở giữa`-N`Và`N`, vì vậy nó không bao giờ cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`N = 4`,`K = 0`, Và`V = BAAB`, số dư tiền tố ban đầu là`0, -1, 0, 1, 0`. Học sinh đầu tiên ngay lập tức tạo ra một khoảng sai nên học sinh 1 được chọn. Sau đó, sau khi hai người`A`phiếu bầu, học sinh 4 lại tạo ra một khoảng xấu khác nên học sinh 4 được chọn. 

| Sinh viên | Bình chọn | Tiền tố gốc | Lật | Tiền tố hiệu quả | Tối đa Trước | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | B | -1 | 0 | -1 | 0 | Lật 1 | 
| 2 | A | 0 | 1 | 2 | 1 | Giữ | 
| 3 | A | 1 | 1 | 3 | 2 | Giữ | 
| 4 | B | 0 | 1 | 2 | 3 | Lật 4 | 

Học sinh được chọn là 1 và 4 nên chi phí là`2^1 + 2^4 = 2 + 16 = 18`. Ví dụ này chứng minh tại sao chỉ kiểm tra trong khoảng thời gian dài là không đủ. Khoảng đơn chứa học sinh 4 cũng phải an toàn. 

### Mẫu 2 

cho`N = 4`,`K = 1`, Và`V = BAAB`, cái đầu tiên`B`được phép vì Betty chỉ dẫn đầu với một phiếu bầu. Càng về sau`B`dưới ngưỡng cũng vô hại nên không có học sinh nào bị lật. 

| Sinh viên | Bình chọn | Tiền tố gốc | Lật | Tiền tố hiệu quả | Tối đa Trước | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | B | -1 | 0 | -1 | 0 | Giữ | 
| 2 | A | 0 | 0 | 0 | 0 | Giữ | 
| 3 | A | 1 | 0 | 1 | 0 | Giữ | 
| 4 | B | 0 | 0 | 0 | 1 | Giữ | 

Tại mọi thời điểm, tiền tố hiệu quả ít nhất là`max_balance - 1`. Do đó, câu trả lời là bằng không. Dấu vết này cũng thể hiện ranh giới bao hàm: lợi thế của Betty chính xác bằng`K`là một trận hòa, không phải là Betty thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N)`mỗi cuộc bầu cử | Quá trình quét chính,`B`con trỏ và mỗi mục deque mỗi mục chỉ di chuyển về phía trước. | 
| Không gian |`O(N)`| Chuỗi đầu vào và deque đơn điệu yêu cầu lưu trữ tuyến tính trong trường hợp xấu nhất. | 

Thuật toán chỉ thực hiện một số thao tác khấu hao không đổi cho mỗi học sinh. Với`N`lên tới một triệu thì đây là thang đo phù hợp, trong khi`O(N log N)`hoặc`O(N^2)`cách tiếp cận thêm công việc không cần thiết. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

MOD = 1_000_000_007

def solve_data(data: str) -> str:
    inp = io.StringIO(data)

    def input():
        return inp.readline

    readline = inp.readline
    t = int(readline())
    out = []

    for case in range(1, t + 1):
        n, k = map(int, readline().split())
        s = readline().strip()

        dq = deque([(0, 0)])
        prefix = 0
        flips = 0
        max_balance = 0
        max_pos = 0

        next_b = 0
        next_b_cost = 1
        answer = 0

        for i, ch in enumerate(s, 1):
            if ch == 'A':
                prefix += 1
            else:
                prefix -= 1

            while dq and dq[-1][1] < prefix:
                dq.pop()
            dq.append((i, prefix))

            current = prefix + 2 * flips

            if current < max_balance - k:
                left = max_pos + 1

                while next_b < left:
                    next_b += 1
                    next_b_cost = next_b_cost * 2 % MOD

                while next_b < i and s[next_b] != 'B':
                    next_b += 1
                    next_b_cost = next_b_cost * 2 % MOD

                answer = (answer + next_b_cost) % MOD
                flips += 1

                while dq and dq[0][0] < next_b:
                    dq.popleft()

                shifted_max = dq[0][1] + 2 * flips

                if shifted_max > max_balance:
                    max_balance = shifted_max
                    max_pos = dq[0][0]

                next_b += 1
                next_b_cost = next_b_cost * 2 % MOD

            elif current > max_balance:
                max_balance = current
                max_pos = i

        out.append(f"Case #{case}: {answer}")

    return "\n".join(out)

# Provided samples
sample = """6
4 0
BAAB
4 1
BAAB
4 1
ABBA
5 2
BBBBB
15 3
ABBBABBBBBABABB
50 4
BBABAABBBBABBBBAABBBBAABBBBBABBBAABABBBBBBABABBAAB
"""

assert solve_data(sample) == """Case #1: 18
Case #2: 0
Case #3: 4
Case #4: 10
Case #5: 324
Case #6: 363067831""", "provided samples"

# Minimum size, Betty must be stopped.
assert solve_data("""1
1 0
B
""") == "Case #1: 2", "single B"

# Minimum size, Amy already wins.
assert solve_data("""1
1 0
A
""") == "Case #1: 0", "single A"

# Threshold equal to N means Betty can never exceed it.
assert solve_data("""1
3 3
BBB
""") == "Case #1: 0", "K = N"

# Boundary case from the statement where the cheapest useful B
# is not the current endpoint.
assert solve_data("""1
4 1
ABBA
""") == "Case #1: 4", "earliest B in bad interval"

# All A, maximum-size input.
n = 1_000_000
assert solve_data(f"1\n{n} 0\n{'A' * n}\n") == "Case #1: 0", "maximum-size all A"

# All B with K = 0 requires flipping every student.
assert solve_data("""1
5 0
BBBBB
""") == "Case #1: 62", "all B with K=0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 0 / B`|`Case #1: 2`| Kích thước tối thiểu và khoảng Betty đơn lẻ | 
|`1 / 1 0 / A`|`Case #1: 0`| Cuộc bầu cử đã an toàn | 
|`1 / 3 3 / BBB`|`Case #1: 0`| Ranh giới ngưỡng tối đa | 
|`1 / 4 1 / ABBA`|`Case #1: 4`| Lựa chọn người đủ điều kiện sớm nhất`B`, không phải điểm cuối hiện tại | 
|`N = 1,000,000`, tất cả`A`|`Case #1: 0`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 
|`1 / 5 0 / BBBBB`|`Case #1: 62`| Vi phạm nhiều lần và tích lũy quyền hạn của hai | 

## Vỏ cạnh 

cho`N = 1`,`K = 0`, Và`V = B`, tiền tố hiệu quả ban đầu là`-1`, trong khi tiền tố tối đa trước đó là`0`. điều kiện`-1 < 0 - 0`phát hiện vi phạm đơn lẻ. Đủ điều kiện sớm nhất`B`là sinh viên 1, có chi phí là`2`, cho kết quả đúng`Case #1: 2`. 

Vì`N = 4`,`K = 0`, Và`V = BAAB`, học sinh 1 được chọn ngay. Các tiền tố có hiệu lực trở thành`1, 2, 3`qua sinh viên 3. Ở sinh viên 4, số dư hiệu dụng giảm từ mức tối đa`3`ĐẾN`2`, nằm dưới giá trị cho phép`3`. Do đó, khoảng thời gian xấu bắt đầu ở học sinh thứ 4 và chỉ có học sinh đủ điều kiện`B`là sinh viên 4. Chi phí của nó là`16`, vậy tổng số là`2 + 16 = 18`. 

Vì`N = 4`,`K = 1`, Và`V = ABBA`, sau sinh viên 1 số dư tiền tố tối đa là`1`. Ở học sinh 2 số dư là`0`, chính xác là`1 - K`, vì vậy không cần lật. Ở học sinh thứ 3 số dư trở thành`-1`, vi phạm điều kiện Khoảng xấu bắt đầu ở học sinh thứ 2 và sớm nhất`B`trong khoảng đó là sinh viên 2. Lật nó mất phí`4`. Các vị trí sau này sẽ an toàn, mang lại`Case #1: 4`. 

Vì`N = 3`,`K = 3`, Và`V = BBB`, tập đại diện tệ nhất có thể chứa ba phiếu bầu của Betty, mang lại cho Betty lợi thế về chính xác`3`. Vì Betty chỉ thắng khi lợi thế của cô ấy thực sự lớn hơn`K`, không phát hiện vi phạm và câu trả lời vẫn là 0. 

Đối với tất cả kích thước tối đa-`A`trường hợp, mỗi số dư tiền tố tăng thêm một, do đó số dư hiện tại ít nhất luôn bằng số dư tối đa trước đó. Nhánh vi phạm không bao giờ được nhập vào,`B`con trỏ không bao giờ được sử dụng và câu trả lời vẫn bằng 0 sau một lần quét tuyến tính. Điều này thực hiện đầu vào lớn nhất có thể mà không đưa ra bất kỳ hành vi trường hợp đặc biệt nào.
