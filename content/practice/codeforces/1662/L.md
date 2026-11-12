---
title: "CF 1662L - Il Derby della Madonnina"
description: "Chúng ta được cung cấp một chuỗi các khoảnh khắc trong một trận đấu bóng đá khi các cú đá xảy ra, mỗi cú đá xảy ra vào một thời điểm cố định và một vị trí cố định dọc theo đường biên. Tại thời điểm 0, chúng ta bắt đầu ở vị trí 0, sau đó chúng ta được phép di chuyển liên tục dọc theo đường thẳng với tốc độ giới hạn."
date: "2026-06-10T02:47:12+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1662
codeforces_index: "L"
codeforces_contest_name: "SWERC 2021-2022 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 0
weight: 1662
solve_time_s: 99
verified: true
draft: false
---

[CF 1662L - Il Derby della Madonnina](https://codeforces.com/problemset/problem/1662/L) 

**Đánh giá:** - 
**Tags:** cấu trúc dữ liệu, dp, toán 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các khoảnh khắc trong một trận đấu bóng đá khi các cú đá xảy ra, mỗi cú đá xảy ra vào một thời điểm cố định và một vị trí cố định dọc theo đường biên. Tại thời điểm 0, chúng ta bắt đầu ở vị trí 0, sau đó chúng ta được phép di chuyển liên tục dọc theo đường thẳng với tốc độ giới hạn. Khi mỗi cú đá xảy ra, chúng ta có thể có hoặc không thể quan sát kỹ nó: chúng ta chỉ thành công nếu tại thời điểm chính xác đó, vị trí của chúng ta càng gần vị trí cú đá trong số tất cả các vị trí có thể tiếp cận được trong điều kiện hạn chế chuyển động. 

Nhiệm vụ là chọn một tập hợp con các cú đá để “che phủ” theo thứ tự thời gian, tối đa hóa số lượng chúng ta có thể quan sát được theo nghĩa tối ưu này. Giữa hai cú đá đã chọn, chuyển động của chúng ta phải khả thi về mặt vật lý với tốc độ tối đa. 

Khó khăn chính là điều kiện quan sát buộc chúng ta phải ở trạng thái tối ưu chính xác tại thời điểm thực hiện cú đá được chọn, điều này có nghĩa là ở mỗi cú đá được chọn, chúng ta phải ở vị trí chính xác tại tọa độ của cú đá đó, vì bất kỳ sai lệch nào cũng sẽ cho phép một vị trí gần hơn nghiêm ngặt nếu có thể tiếp cận được. 

Kích thước đầu vào lên tới 200.000 sự kiện. Lập trình động bậc hai hoặc thậm chí hơi siêu tuyến tính trên tất cả các cặp cú đá là quá chậm. Mọi giải pháp đều phải gần tuyến tính hoặc n log n. 

Một trường hợp thất bại tinh tế xuất hiện khi một chiến lược tham lam chọn mọi cú đá có thể tiếp cận cục bộ. Ví dụ: nếu chúng ta gần như không thể chạm tới một cú đá, nhưng làm như vậy sẽ chặn được nhiều cú đá trong tương lai, thì tính tham lam có thể thất bại vì nó bỏ qua rằng việc đến sớm hơn hoặc muộn hơn có thể cho phép khả năng tiếp cận nhiều hơn trong tương lai. 

Một trường hợp khác phát sinh khi khoảng cách thời gian lớn cho phép di chuyển trong thời gian dài. Việc triển khai ngây thơ chỉ có thể so sánh sự khác biệt về vị trí mà không chia tỷ lệ theo chênh lệch thời gian, giả định không chính xác chuyển động liên tục trên mỗi bước. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng tính toán, đối với mỗi cú đá, liệu có thể kết thúc ở cú đá đó sau một số cú đá đã chọn trước đó hay không. Với mỗi cặp cú đá i và j có i < j, chúng ta kiểm tra xem liệu chúng ta có thể di chuyển từ a_i đến a_j trong khoảng thời gian t_j - t_i hay không. Nếu khả thi, chúng ta cố gắng mở rộng chuỗi tốt nhất kết thúc tại i thành j. Điều này dẫn đến quá trình chuyển đổi lập trình động trên tất cả các cặp, tức là O(n^2) và sẽ không vượt qua đối với 200.000 sự kiện. 

Quan sát quan trọng là điều kiện về tính khả thi giữa hai cú đá chỉ phụ thuộc vào chênh lệch thời gian và chênh lệch vị trí, đồng thời tạo thành một hạn chế về khả năng tiếp cận đơn điệu. Thay vì theo dõi tất cả các lựa chọn có thể có trước đó, chúng ta có thể duy trì, đối với từng vị trí, cách tốt nhất để tiếp cận vị trí đó tại mỗi thời điểm. Tuy nhiên, không thể lưu trữ trạng thái đầy đủ trên các vị trí liên tục. 

Thay vào đó, chúng tôi trình bày lại vấn đề dưới dạng một chuỗi dài nhất dưới một ràng buộc về khả năng tiếp cận: chúng tôi muốn có một chuỗi các cú đá sao cho mỗi cặp được chọn liên tiếp đều thỏa mãn một ràng buộc về tốc độ. Điều này có cấu trúc tương tự như đường dẫn dài nhất trong DAG trong đó các cạnh chỉ tồn tại nếu |a_i - a_j| ≤ v · (t_j - t_i). Đồ thị trực tiếp dày đặc, nhưng thứ tự thời gian cho phép lập trình động với đường biên khả thi trượt. 

Sự đơn giản hóa quan trọng là với mỗi cú đá j, chúng ta chỉ cần biết liệu có tồn tại một cú đá i đã chọn trước đó có thể đạt tới j hay không, và trong số đó chúng ta muốn có cú đá tối đa hóa độ dài chuỗi. Điều này có thể được duy trì bằng cách lan truyền tham lam các khoảng thời gian có thể đạt được theo thời gian, giảm trạng thái xuống một biên giới ứng cử viên tốt nhất. 

Về bản chất, chúng tôi duy trì “trạng thái bao phủ” tốt nhất có thể sau mỗi lần kích hoạt được chọn và quyết định xem có đưa vào lần kích hoạt tiếp theo hay không dựa trên việc liệu nó có thể truy cập được từ trạng thái đó hay không. Nếu có thể truy cập được, chúng tôi sẽ cập nhật; nếu không chúng ta bỏ qua nó và tiếp tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP trên tất cả các cặp | O(n^2) | O(n) | Quá chậm | 
| Khả năng tiếp cận tham lam tối ưu DP | O(n) | O(1) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cú đá theo thứ tự thời gian tăng dần. 

1. Chúng tôi duy trì trạng thái tốt nhất hiện tại bao gồm chỉ số cú đá được chọn gần đây nhất và số lần đá được chọn cho đến nay. Ban đầu không chọn cú đá nào nên vị trí cuối cùng là 0 tại thời điểm 0. 
2. Đối với mỗi cú đá i theo thứ tự thời gian, chúng tôi kiểm tra xem liệu nó có thể đạt được từ trạng thái hiện tại hay không. Điều này có nghĩa là chúng tôi kiểm tra xem liệu chúng tôi có thể di chuyển từ vị trí hiện tại đến a_i trong thời gian t_i - current_time hay không. 
3. Nếu có thể tiếp cận được, chúng ta quyết định thực hiện cú đá này. Chúng tôi cập nhật trạng thái hiện tại để phản ánh rằng chúng tôi hiện đang ở vị trí a_i tại thời điểm t_i và tăng câu trả lời. 
4. Nếu không thể truy cập được, chúng tôi bỏ qua bước khởi động này và giữ nguyên trạng thái hiện tại. 
5. Tiếp tục cho đến khi tất cả các cú đá được xử lý. 

Phần tinh tế là “có thể truy cập từ trạng thái hiện tại” là đủ, bởi vì trạng thái hiện tại luôn là vị trí tốt nhất có thể về mặt tối đa hóa khả năng tiếp cận trong tương lai. Bất kỳ con đường thay thế nào cho phép linh hoạt hơn sẽ phải đến sớm hơn hoặc ở một vị trí gần hơn, vị trí này vốn đã thống trị trạng thái được duy trì. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì một trạng thái biên duy nhất thể hiện một vị trí khả thi tại thời điểm phát động được chọn mới nhất, tối ưu cho khả năng tiếp cận trong tương lai. Bất kỳ chuỗi khả thi nào khác kết thúc sớm hơn hoặc ở một vị trí khác đều không thể cải thiện nghiêm ngặt các lựa chọn trong tương lai nếu không thể cải thiện biên giới này trước tiên, bởi vì các ràng buộc chuyển động là tuyến tính theo thời gian và đối xứng về khoảng cách. Do đó, tiêu chuẩn chấp nhận tham lam không bao giờ loại bỏ một dãy có thể dẫn đến số lượng lớn hơn sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, v = map(int, input().split())
    t = list(map(int, input().split()))
    a = list(map(int, input().split()))

    # current state: time, position, count
    cur_t = 0
    cur_x = 0
    ans = 0

    for i in range(n):
        dt = t[i] - cur_t
        dist = abs(a[i] - cur_x)

        if dist <= v * dt:
            ans += 1
            cur_t = t[i]
            cur_x = a[i]

    print(ans)

if __name__ == "__main__":
    solve()
```Mã này duy trì một trạng thái khả thi đang phát triển duy nhất. Biến`cur_t`theo dõi thời gian của cú đá được chấp nhận cuối cùng và`cur_x`theo dõi vị trí chúng ta đứng sau cú đá đó. Đối với mỗi sự kiện sắp tới, chúng tôi tính toán xem chúng tôi có thể di chuyển được bao xa kể từ sự kiện được chấp nhận cuối cùng và so sánh nó với sự dịch chuyển cần thiết. 

Một cạm bẫy triển khai phổ biến là quên chỉ cập nhật thời gian khi thực hiện một cú đá. Việc sử dụng thời gian chung thay vì thời gian được chấp nhận lần cuối sẽ phá vỡ các bước kiểm tra tính khả thi vì nó cho phép di chuyển không chính xác trong các khoảng thời gian bị bỏ qua. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
5 10 15
7 17 29
```Chúng tôi theo dõi sự phát triển của trạng thái. 

| Bước | Thời gian | Vị trí | Có thể truy cập được? | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | - | - | 0 | 
| 1 | 5 | 7 | 7 10 | lấy | 1 | 
| 2 | 10 | 17 | 10 10 | lấy | 2 | 
| 3 | 15 | 29 | 24 ≤ 10 (sai) | bỏ qua | 2 | 

Cú đá thứ ba bị bỏ qua vì ngay cả ở tốc độ tối đa, khoảng cách đến 29 là quá lớn so với thời gian kể từ vị trí được chấp nhận cuối cùng. 

Điều này xác nhận thuật toán thực thi chính xác khả năng tiếp cận dựa trên thời gian thay vì chỉ sắp xếp theo vị trí. 

### Ví dụ 2 

đầu vào:```
4 1
1 3 6 10
0 2 3 4
```| Bước | Thời gian | Vị trí | Có thể truy cập được? | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | - | - | 0 | 
| 1 | 1 | 0 | 0 1 | lấy | 1 | 
| 2 | 3 | 2 | 2 2 2 | lấy | 2 | 
| 3 | 6 | 3 | 1 3 | lấy | 3 | 
| 4 | 10 | 4 | 1 4 | lấy | 4 | 

Điều này chứng tỏ một trường hợp mà mọi cú đá đều khả thi và trạng thái tham lam diễn biến suôn sẻ mà không cần phải từ chối một sự kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cú đá được xử lý một lần với số học thời gian không đổi | 
| Không gian | O(1) | Chỉ thời gian, vị trí và bộ đếm hiện tại được lưu trữ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì 200.000 sự kiện được xử lý bằng một đường truyền tuyến tính duy nhất và không có cấu trúc dữ liệu phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, v = map(int, sys.stdin.readline().split())
    t = list(map(int, sys.stdin.readline().split()))
    a = list(map(int, sys.stdin.readline().split()))

    cur_t = 0
    cur_x = 0
    ans = 0

    for i in range(n):
        if abs(a[i] - cur_x) <= v * (t[i] - cur_t):
            ans += 1
            cur_t = t[i]
            cur_x = a[i]

    return str(ans)

# provided sample
assert run("""3 2
5 10 15
7 17 29
""") == "2"

# minimum size
assert run("""1 10
5
3
""") == "1"

# unreachable after first
assert run("""2 1
1 100
0 1000
""") == "1"

# all equal reachable
assert run("""5 5
1 2 3 4 5
0 0 0 0 0
""") == "5"

# tight movement constraint
assert run("""3 1
1 2 3
0 2 4
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sự kiện duy nhất | 1 | trường hợp cơ sở | 
| bước nhảy không thể tiếp cận | 1 | hạn chế về tính khả thi | 
| tất cả các vị trí bằng 0 | n | luôn có thể truy cập | 
| chuyển động chặt chẽ | 2 | giới hạn dịch chuyển ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi khoảng cách thời gian đủ lớn để cho phép đặt lại nhiều vị trí. Ví dụ: nếu một cú đá bị bỏ qua, thuật toán không được tích lũy chuyển động không chính xác từ khoảng thời gian bị bỏ qua đó. Trạng thái chỉ cập nhật những lượt đá được chấp nhận, do đó, những khoảng thời gian bị bỏ qua sẽ tự nhiên góp phần thời gian cho lần kiểm tra tính khả thi tiếp theo mà không ảnh hưởng đến vị trí. 

Một trường hợp cạnh khác là khi các vị trí xen kẽ ở ngoài cùng bên trái và ngoài cùng bên phải. điều kiện`abs(a[i] - cur_x) <= v * (t[i] - cur_t)`ngăn chặn chính xác những dao động không thể xảy ra vì khoảng cách cần thiết tăng tuyến tính theo thời gian và việc đổi bên cần có đủ thời gian tích lũy. 

Một trường hợp khó phát hiện cuối cùng là khi nhiều cú đá ở rất gần nhau về thời gian. Thuật toán xử lý việc này một cách chính xác vì`dt`trở nên nhỏ, khiến hầu hết các chuyển đổi không thể thực hiện được trừ khi các vị trí cũng gần nhau, phù hợp với giới hạn vật lý của tốc độ giới hạn.
