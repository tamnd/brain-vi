---
title: "CF 102361I - Invoker"
description: "Invoker duy trì một chuỗi tối đa ba phần tử. Nhấn Q, W hoặc E sẽ thêm phần tử đó vào chuỗi. Nếu đã có ba phần tử thì phần tử cũ nhất sẽ biến mất trước tiên."
date: "2026-08-13T00:13:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "I"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 75
verified: true
draft: false
---

[CF 102361I - Invoker](https://codeforces.com/problemset/problem/102361/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Invoker duy trì một chuỗi tối đa ba phần tử. Nhấn`Q`,`W`, hoặc`E`nối phần tử đó vào chuỗi. Nếu đã có ba phần tử thì phần tử cũ nhất sẽ biến mất trước tiên. Một kỹ năng đặc biệt chỉ được xác định bởi tập hợp nhiều phần tử hiện tại, do đó thứ tự quan trọng đối với các bản cập nhật trong tương lai nhưng không quan trọng khi quyết định kỹ năng đặc biệt nào hiện có sẵn. 

Chuỗi đầu vào mô tả các kỹ năng đặc biệt phải được gọi theo thứ tự. Trước mỗi ký tự của chuỗi, Invoker phải có multiset 3 phần tử tương ứng, sau đó nhấn`R`. Nhấn`R`tốn một kỹ năng và không thay đổi thứ tự các phần tử hiện tại. Mục tiêu là giảm thiểu tổng số lần nhấn kỹ năng cơ bản cộng với`R`máy ép. 

Chỉ có ba yếu tố có thể có và kho chứa tối đa ba yếu tố trong số đó. Điều này làm cho không gian trạng thái hoàn chỉnh trở nên nhỏ bé. Khi kho chứa ba phần tử, chỉ có thể có (3^3=27) trạng thái được đặt hàng. Trạng thái trống ban đầu thêm một trạng thái nữa. Độ dài đầu vào có thể đạt tới (10^5), do đó, thuật toán có hệ số tùy thuộc vào số trạng thái có thể đủ nhanh, trong khi tìm kiếm trên tất cả các lịch sử có thể có là theo cấp số nhân và không thể xử lý đầu vào tối đa. 

Nguyên nhân chính của sai lầm là kỹ năng đặc biệt bỏ qua thứ tự, trong khi những kỹ năng thay thế trong tương lai thì không. Ví dụ, sau khi xây dựng`X`, các phần tử có thể là`QWW`, và sau khi xây dựng`V`họ chỉ cần chứa`QQW`. Bắt đầu từ`QWW`, nối thêm`Q`cho`WWQ`, vẫn là`X`; nối thêm cái khác`Q`cho`WQQ`, đó là`V`. Như vậy`XV`cần (4+2+1=7) kỹ năng. Giải pháp sắp xếp hàng tồn kho sau mỗi hoạt động sẽ làm mất thông tin theo trình tự thời gian và có thể tuyên bố không chính xác rằng cần ít máy ép cơ bản hơn. 

Một trường hợp khác là lặp lại một kỹ năng đặc biệt. Đối với đầu vào`YY`, đầu tiên`Y`nhu cầu`QQQ`theo sau là`R`, trong khi thứ hai`Y`chỉ cần cái khác`R`, bởi vì các phần tử vẫn còn sau khi gọi. Câu trả lời là`5`. Việc triển khai bất cẩn để xây dựng lại mọi kỹ năng được yêu cầu từ đầu sẽ quay trở lại`8`. 

Trạng thái ban đầu cũng đặc biệt. Đối với đầu vào`B`, không có phần tử nào ở đầu, vì vậy cả ba phần tử phải được tạo trước lần gọi đầu tiên. Câu trả lời là`4`. Việc coi trạng thái ban đầu như một kho hàng đầy đủ tùy ý sẽ đánh giá thấp chi phí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể thử mọi chuỗi kỹ năng cơ bản có thể có giữa hai lần gọi. Vì mọi kỹ năng đặc biệt đều cần chính xác ba yếu tố nên không bao giờ có lý do để nhấn nhiều hơn ba kỹ năng cơ bản trước kỹ năng tiếp theo.`R`: sau ba lần nhấn mới, toàn bộ kho cũ đã bị loại bỏ và có thể xây dựng bất kỳ bộ đa thành phần ba phần tử mong muốn nào. Tuy nhiên, một tìm kiếm hoàn toàn không bị cắt xén trong toàn bộ lịch sử sẽ xem xét mọi chuỗi kỹ năng cơ bản có thể có. Đối với (n) kỹ năng được yêu cầu, việc tìm kiếm cho phép tối đa ba lần nhấn cơ bản trước mỗi lần gọi 

[ 
\sum_{k=0}^{3n}3^k=\frac{3^{3n+1}-1}{2} 
] 

tiền tố kỹ năng cơ bản có thể có trong trường hợp xấu nhất. Với (n=100000), đây là con số lớn về mặt thiên văn. 

Lý luận thô bạo là đúng bởi vì mọi trình tự hợp pháp đều có thể được biểu diễn bằng các lệnh nhấn và lệnh gọi cơ bản của nó, và việc liệt kê các trình tự đó cuối cùng sẽ tìm ra mức tối thiểu. Vấn đề của nó là nó liên tục khám phá những lịch sử có cùng kho đồ hiện tại và những khả năng giống nhau trong tương lai. 

Quan sát quan trọng là quá khứ có thể bị loại bỏ khi chúng ta biết lượng hàng tồn kho đã đặt hàng hiện tại. Chỉ có 27 hàng tồn kho được đặt hàng đầy đủ. Đối với mỗi cặp trạng thái như vậy, chúng ta có thể tính số lần nhấn cơ bản tối thiểu cần thiết để chuyển đổi trạng thái này thành trạng thái kia. Mỗi thao tác nhấn cơ bản chỉ đơn giản là một cạnh trong biểu đồ có hướng nhỏ: nối thêm`Q`,`W`, hoặc`E`và nếu cần, hãy xóa phần tử cũ nhất. 

Điều này biến vấn đề thành lập trình động chỉ trên 27 trạng thái. Đối với mỗi kỹ năng đặc biệt được yêu cầu, chúng tôi liệt kê tất cả các hoán vị có thứ tự của ba thành phần của nó. Có nhiều nhất là sáu trạng thái như vậy. Nếu hàng tồn kho được đặt hàng trước đó là`state`và một trong những hoán vị này là`target`, chi phí chuyển đổi là số lần nhấn cơ bản tối thiểu được tính toán trước từ`state`ĐẾN`target`, cộng một cho`R`. 

Bảng khoảng cách có thể được tính toán bằng BFS từ mọi trạng thái. Biểu đồ chỉ có 28 trạng thái nếu bao gồm trạng thái trống và mọi trạng thái đều có ba cạnh hướng ra ngoài. Quá trình tiền xử lý có hiệu quả là thời gian không đổi. Vòng lặp chính thực hiện tối đa (27\times6) chuyển đổi cho mỗi ký tự, tạo ra độ phức tạp tuyến tính trong độ dài đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^{3n})) | (O(n)) cho đường dẫn tìm kiếm | Quá chậm | 
| Tối ưu | (O(n)) với hằng số nhỏ | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trình bày mọi khoảng không quảng cáo được đặt hàng dưới dạng một bộ từ 0 đến 3 ký tự. Bộ dữ liệu trống biểu thị trạng thái ban đầu. Để có hàng tồn kho đầy đủ,`(Q, W, E)`Và`(E, W, Q)`là các trạng thái khác nhau vì chúng phản ứng khác nhau với kỹ năng cơ bản tiếp theo. 
2. Tạo tất cả các trạng thái có thể truy cập bằng cách bắt đầu từ trạng thái trống và liên tục nối thêm từng trạng thái`Q`,`W`, Và`E`. Khi phần tử thứ tư được thêm vào, chỉ giữ lại ba phần tử mới nhất. Tổng cộng chỉ có 28 trạng thái, vì vậy điều này có thể được thực hiện với một BFS nhỏ. 
3. Xây dựng bảng khoảng cách ngắn nhất giữa tất cả các trạng thái. Từ mỗi tiểu bang, một BFS khám phá ba kỹ năng cơ bản có thể có. Khoảng cách kết quả cho chúng ta biết số lần nhấn cơ bản tối thiểu cần thiết để đạt đến bất kỳ trạng thái nào khác. 
4. Ánh xạ từng nhân vật có kỹ năng đặc biệt vào nhiều bộ yêu cầu của nó. Ví dụ,`X`tương ứng với`QWW`,`B`tương ứng với`QWE`, Và`T`tương ứng với`EEE`. 
5. Đối với mỗi kỹ năng đặc biệt, hãy tạo ra tất cả các hoán vị riêng biệt của ba yếu tố bắt buộc. Đơn hàng không phải là một phần của kỹ năng đặc biệt, vì vậy mọi hoán vị đều là kho cuối cùng hợp lệ. Nhiều nhất là sáu trạng thái được sản xuất. 
6. Duy trì`dp[state]`, số lượng kỹ năng tối thiểu đã được sử dụng sau khi gọi tất cả các kỹ năng đặc biệt đã xử lý và kết thúc bằng kho được đặt hàng chính xác`state`. Ban đầu chỉ có thể truy cập trạng thái trống với chi phí bằng 0. 
7. Đối với kỹ năng đặc biệt được yêu cầu tiếp theo, hãy xem xét mọi trạng thái hiện có thể tiếp cận và mọi trạng thái mục tiêu được yêu cầu hợp lệ cho kỹ năng đặc biệt đó. Di chuyển giữa chúng bằng khoảng cách ngắn nhất được tính toán trước, sau đó thêm một khoảng cách để nhấn`R`. Giữ giá trị tối thiểu cho từng trạng thái mục tiêu. 
8. Thay thế mảng DP cũ bằng mảng DP mới và xử lý ký tự tiếp theo. Sau ký tự cuối cùng, câu trả lời là giá trị DP tối thiểu trên tất cả các trạng thái đầy đủ vì lệnh gọi cuối cùng có thể kết thúc với bất kỳ thứ tự nào của nhiều tập hợp được yêu cầu. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[state]`chính xác là chi phí tối thiểu để hoàn thành tiền tố được xử lý của đầu vào và có`state`như hàng tồn kho được đặt hàng hiện tại ngay sau lần gọi cuối cùng. Đối với kỹ năng đặc biệt tiếp theo, mọi chiến lược pháp lý trước tiên phải chuyển từ trạng thái hiện tại sang một số khoảng không quảng cáo được đặt hàng có nhiều bộ đại diện cho kỹ năng được yêu cầu. Khoảng cách BFS cung cấp số lần nhấn cơ bản tối thiểu có thể có cho chuyển động đó và lệnh gọi được yêu cầu sẽ thêm chính xác một thao tác nữa. Vì mọi thứ tự mục tiêu hợp lệ đều được xem xét nên quá trình chuyển đổi bao gồm mọi cách hợp pháp có thể có để gọi kỹ năng tiếp theo. Do đó, việc lấy mức tối thiểu trên tất cả các trạng thái trước đó sẽ bảo toàn tính bất biến. Bằng cách quy nạp trên chuỗi đầu vào, giá trị tối thiểu cuối cùng là câu trả lời tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from collections import deque
from itertools import permutations

input = sys.stdin.readline

ELEMENTS = "QWE"

SPECIAL = {
    "Y": "QQQ",
    "V": "QQW",
    "G": "QQE",
    "C": "WWW",
    "X": "QWW",
    "Z": "WWE",
    "T": "EEE",
    "F": "QEE",
    "D": "WEE",
    "B": "QWE",
}

def build_states():
    states = [()]
    index = {(): 0}
    q = deque([()])

    while q:
        state = q.popleft()

        for ch in ELEMENTS:
            nxt = state + (ch,)
            if len(nxt) > 3:
                nxt = nxt[-3:]

            if nxt not in index:
                index[nxt] = len(states)
                states.append(nxt)
                q.append(nxt)

    return states, index

def build_dist(states, index):
    m = len(states)
    dist = [[10**9] * m for _ in range(m)]

    for start in range(m):
        dist[start][start] = 0
        q = deque([start])

        while q:
            u = q.popleft()
            state = states[u]

            for ch in ELEMENTS:
                nxt = state + (ch,)
                if len(nxt) > 3:
                    nxt = nxt[-3:]

                v = index[nxt]
                if dist[start][v] == 10**9:
                    dist[start][v] = dist[start][u] + 1
                    q.append(v)

    return dist

def solve_string(s):
    states, index = build_states()
    dist = build_dist(states, index)

    m = len(states)
    inf = 10**9

    dp = [inf] * m
    dp[index[()]] = 0

    targets = {}

    for skill, elements in SPECIAL.items():
        target_states = set()

        for p in permutations(elements):
            target_states.add(index[p])

        targets[skill] = tuple(target_states)

    for skill in s:
        ndp = [inf] * m

        for u in range(m):
            if dp[u] == inf:
                continue

            for v in targets[skill]:
                cost = dp[u] + dist[u][v] + 1
                if cost < ndp[v]:
                    ndp[v] = cost

        dp = ndp

    return str(min(dp))

def main():
    s = input().strip()
    print(solve_string(s))

if __name__ == "__main__":
    main()
```các`SPECIAL`Từ điển lưu trữ một thứ tự đại diện của ba yếu tố được yêu cầu bởi mỗi kỹ năng đặc biệt. Giá trị từ điển sau này được coi là nhiều tập hợp, do đó thứ tự cụ thể của nó không có ý nghĩa ngữ nghĩa.`build_states`xây dựng mọi trạng thái có thể tiếp cận được thông qua các kỹ năng cơ bản. Việc biểu diễn bộ dữ liệu rất hữu ích vì các bộ dữ liệu có thể băm được và giữ nguyên thứ tự thời gian. Khi bộ dữ liệu phát triển vượt quá ba phần tử,`nxt[-3:]`thực hiện chính xác quy tắc thay thế FIFO.`build_dist`chạy BFS từ mọi tiểu bang. Mỗi cạnh đều có giá một vì một lần nhấn kỹ năng cơ bản sẽ thay đổi kho đồ chỉ bằng một thao tác. Do đó, BFS là thuật toán đường đi ngắn nhất chính xác. Trạng thái trống được đưa vào tự động, xử lý kỹ năng đặc biệt được yêu cầu đầu tiên mà không có trường hợp đặc biệt riêng. 

Với mỗi kỹ năng đặc biệt,`permutations`tạo ra tất cả các đơn đặt hàng có thể. MỘT`set`loại bỏ các bản sao cho các kỹ năng như`Y`, trong đó cả ba hoán vị đều giống hệt nhau. Đây là một chi tiết triển khai tinh tế nhưng hữu ích vì`QQQ`chỉ nên tạo ra một trạng thái mục tiêu thay vì xử lý nhiều lần cùng một quá trình chuyển đổi. 

Bản cập nhật DP bổ sung`dist[u][v]`cho các kỹ năng cơ bản cần thiết và sau đó thêm chính xác một kỹ năng cho`R`. Lệnh gọi không sửa đổi khoảng không quảng cáo, vì vậy`v`vẫn là trạng thái sau khi chuyển đổi. Mảng DP cũ chỉ được thay thế sau khi tất cả các chuyển đổi cho ký tự hiện tại đã được đánh giá, ngăn không cho một kỹ năng được yêu cầu được gọi nhiều lần trong một lần lặp. 

Tất cả chi phí tối đa chỉ vài trăm nghìn cho đầu vào tối đa, vì vậy số nguyên Python không có vấn đề tràn. Bảng khoảng cách chỉ sử dụng một giá trị trọng điểm lớn để khởi tạo, mặc dù mọi trạng thái đều có thể truy cập được từ mọi trạng thái khác trong tối đa ba lần nhấn cơ bản khi kho đã đầy. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là`XDTBVV`. Bảng bên dưới hiển thị trạng thái DP hiện tại tốt nhất sau mỗi kỹ năng được yêu cầu. Một số trạng thái có thể có cùng chi phí, nhưng chỉ trạng thái có chi phí tối thiểu liên quan đến việc tiếp tục cuối cùng mới được hiển thị ở đây dưới dạng một đường dẫn tối ưu. 

| Kỹ năng yêu cầu | Đã thêm máy ép cơ bản | Hàng tồn kho được đặt hàng sau`R`| Tổng chi phí | 
| --- | --- | --- | --- | 
|`X`|`QWW`|`QWW`| 4 | 
|`D`|`EE`|`WEE`| 7 | 
|`T`|`E`|`EEE`| 9 | 
|`B`|`WQ`|`EWQ`| 12 | 
|`V`|`Q`|`WQQ`| 14 | 
|`V`| không |`WQQ`| 15 | 

Trình tự các thao tác là`QWWREERERWQRQRR`. Điểm mấu chốt trong quá trình chuyển đổi thứ tư là mục tiêu`B`chỉ cần multiset`QWE`. Bắt đầu từ`EEE`, nối thêm`W`cho`EEW`, sau đó nối thêm`Q`cho`EWQ`, có nhiều tập hợp chính xác`QWE`. trận chung kết`V`chỉ tốn một`R`bởi vì hàng tồn kho sau lần gọi trước đó đã có sẵn`WQQ`, đại diện cho`QQW`. 

Đối với ví dụ thứ hai, hãy xem xét đầu vào được xây dựng`XV`. 

| Kỹ năng yêu cầu | Đã thêm máy ép cơ bản | Hàng tồn kho được đặt hàng sau`R`| Tổng chi phí | 
| --- | --- | --- | --- | 
|`X`|`QWW`|`QWW`| 4 | 
|`V`|`QQ`|`WQQ`| 7 | 

Sau khi tạo`QWW`, thêm một`Q`sản xuất`WWQ`, vẫn đại diện cho`X`. Một giây`Q`sản xuất`WQQ`, đại diện cho`V`. Do đó, cần có hai lần nhấn cơ bản giữa hai lần gọi, cho ra (4+2+1=7). Ví dụ này giải thích tại sao trạng thái có thứ tự không thể được thay thế chỉ bằng nhiều tập hợp không có thứ tự hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Biểu đồ trạng thái chỉ có 28 trạng thái và mỗi ký tự đầu vào kiểm tra tối đa 28 trạng thái trước đó và 6 hoán vị đích. | 
| Không gian | (O(1)) | Bảng khoảng cách và mảng DP có kích thước không đổi, không phụ thuộc vào (n). | 

Đầu vào có thể chứa (10^5) kỹ năng đặc biệt, do đó sự phụ thuộc tuyến tính vào (n) là phần có liên quan của độ phức tạp. Không gian trạng thái không đổi rất nhỏ và việc triển khai chỉ thực hiện vài triệu chuyển đổi đơn giản ở mức tồi tệ nhất, nằm trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque
from itertools import permutations

ELEMENTS = "QWE"

SPECIAL = {
    "Y": "QQQ",
    "V": "QQW",
    "G": "QQE",
    "C": "WWW",
    "X": "QWW",
    "Z": "WWE",
    "T": "EEE",
    "F": "QEE",
    "D": "WEE",
    "B": "QWE",
}

def make_solver():
    states = [()]
    index = {(): 0}
    q = deque([()])

    while q:
        state = q.popleft()

        for ch in ELEMENTS:
            nxt = state + (ch,)
            if len(nxt) > 3:
                nxt = nxt[-3:]

            if nxt not in index:
                index[nxt] = len(states)
                states.append(nxt)
                q.append(nxt)

    m = len(states)
    dist = [[10**9] * m for _ in range(m)]

    for start in range(m):
        dist[start][start] = 0
        q = deque([start])

        while q:
            u = q.popleft()

            for ch in ELEMENTS:
                nxt = states[u] + (ch,)
                if len(nxt) > 3:
                    nxt = nxt[-3:]

                v = index[nxt]

                if dist[start][v] == 10**9:
                    dist[start][v] = dist[start][u] + 1
                    q.append(v)

    targets = {}

    for skill, elements in SPECIAL.items():
        targets[skill] = tuple(
            index[p] for p in set(permutations(elements))
        )

    def run(inp: str) -> str:
        s = inp.strip()

        inf = 10**9
        dp = [inf] * m
        dp[index[()]] = 0

        for skill in s:
            ndp = [inf] * m

            for u in range(m):
                if dp[u] == inf:
                    continue

                for v in targets[skill]:
                    ndp[v] = min(
                        ndp[v],
                        dp[u] + dist[u][v] + 1
                    )

            dp = ndp

        return str(min(dp))

    return run

run = make_solver()

# Provided sample
assert run("XDTBVV") == "15", "sample 1"

# Constructed second sample
assert run("XV") == "7", "two different skills requiring ordered-state tracking"

# Minimum-size input
assert run("B") == "4", "one special skill starts from an empty inventory"

# Repeated skill
assert run("YY") == "5", "the inventory survives R"

# All-equal values
assert run("TTTTT") == "8", "EEE is already present after the first invocation"

# Maximum-size input
assert run("Y" * 100000) == str(100003), "maximum input length"

# Boundary transition where one additional element is not enough
assert run("XY") == "8", "changing QWW into QQQ requires three Q presses"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`B`|`4`| Hàng tồn kho trống ban đầu | 
|`YY`|`5`| Sử dụng lại cùng một khoảng không quảng cáo sau`R`| 
|`TTTTT`|`8`| Lặp đi lặp lại các kỹ năng đặc biệt giống hệt nhau | 
|`XV`|`7`| Thứ tự thời gian của các yếu tố | 
|`XY`|`8`| thay thế FIFO và chuyển đổi ba lần nhấn | 
|`Y`lặp đi lặp lại 100000 lần |`100003`| Kích thước đầu vào tối đa và xử lý tuyến tính | 

## Vỏ cạnh 

Đối với đầu vào`B`, thuật toán bắt đầu với trạng thái trống. Mỗi trạng thái mục tiêu đầy đủ đại diện cho`QWE`cách trạng thái trống ba cạnh BFS, vì cần có ba kỹ năng cơ bản để tạo kho đầu tiên. DP sau đó thêm một cho`R`, sản xuất`4`. Không có trạng thái đầy đủ nhân tạo nào được giả định ngay từ đầu. 

Đối với đầu vào`YY`, sau lần chuyển đổi đầu tiên, hàng tồn kho duy nhất có liên quan là các hoán vị của`QQQ`, tất cả đều thực sự có cùng trạng thái được sắp xếp`QQQ`. thứ hai`Y`có thể chuyển từ`QQQ`với chính nó với khoảng cách bằng 0, nên chỉ có giây`R`được thêm vào. Câu trả lời là`4+1=5`. 

Đối với đầu vào`XV`, đầu tiên`X`có thể được xây dựng như`QWW`cho bốn hoạt động bao gồm`R`. Từ`QWW`, một`Q`sản xuất`WWQ`, multiset của nó vẫn là`QWW`, vì vậy nó không thể gọi`V`chưa. Một giây`Q`sản xuất`WQQ`, có nhiều tập hợp`QQW`. DP tìm khoảng cách hai và thêm một cho`R`, cho`7`. 

Đối với đầu vào`XY`, hàng tồn kho sau`X`có thể`QWW`. Đang bổ sung`Q`hai lần cho`WWQ`và sau đó`WQQ`, cả hai đều không chứa ba`Q`là. Phần thứ ba được thêm vào`Q`loại bỏ cái cũ nhất`W`và lá`QQQ`. Do đó, khoảng cách chuyển tiếp là ba, nên tổng cộng là`4+3+1=8`. Điều này nắm bắt các triển khai chỉ so sánh số lượng loại phần tử phù hợp mà không mô phỏng thứ tự FIFO. 

Đối với đầu vào có kích thước tối đa bao gồm (100000) bản sao của`Y`, lần gọi đầu tiên tốn bốn thao tác. Mỗi lần gọi sau chỉ tốn một`R`, bởi vì`QQQ`vẫn không thay đổi. Kết quả là (4+99999=100003). DP xử lý từng ký tự một cách độc lập và không bao giờ tăng theo độ dài của lịch sử, do đó đầu vào lớn vẫn tuyến tính.
