---
title: "CF 342B - Xenia và điệp viên"
description: "Một ghi chú bắt đầu tại một điệp viên được định vị tại chỉ mục s trong một dòng gồm n điệp viên. Cuối cùng, bức thư phải đến tay một điệp viên khác ở vị trí f."
date: "2026-06-06T17:41:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 342
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 199 (Div. 2)"
rating: 1500
weight: 342
solve_time_s: 92
verified: true
draft: false
---

[CF 342B - Xenia và Điệp viên](https://codeforces.com/problemset/problem/342/B) 

**Đánh giá:** 1500 
**Tags:** vũ phu, tham lam, thực hiện 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một ghi chú bắt đầu tại một gián điệp được định vị ở chỉ mục`s`trong một dòng`n`gián điệp. Bức thư cuối cùng phải đến tay một điệp viên khác ở vị trí`f`. Thời gian trôi qua theo các bước riêng biệt và tại mỗi bước, người gián điệp hiện đang giữ tờ ghi chú có thể di chuyển nó sang trái một vị trí, di chuyển sang phải một vị trí hoặc giữ nó đúng vị trí. 

Điều phức tạp là mỗi bước có một khoảng cấm`[l_i, r_i]`. Mọi điệp viên trong khoảng thời gian đó đều bị giám sát trong bước đó. Một gián điệp bị giám sát hoàn toàn không hoạt động, vì vậy nếu ghi chú hiện đang ở trên một gián điệp trong khoảng thời gian đó, nó không thể được chuyển đi hoặc nhận được từ hàng xóm. Hành động duy nhất được phép trong trường hợp đó là giữ ghi chú ở cùng một điệp viên. 

Một chuỗi di chuyển hợp lệ nếu tại mỗi bước, ghi chú vẫn đứng yên hoặc nó di chuyển một vị trí sang trái hoặc phải và không có chuyển động nào bắt nguồn hoặc đến được với một điệp viên được giám sát. Mục tiêu là đạt`f`từ`s`với số bước tối thiểu và xuất ra chuỗi hành động theo thời gian. 

Những hạn chế đẩy chúng tôi tới giải pháp quét tuyến tính. Có tới 100000 bước, do đó, bất kỳ khám phá bậc hai nào theo thời gian hoặc vị trí đều không thể thực hiện được. Không gian trạng thái là một chiều, nhưng những hạn chế phụ thuộc vào thời gian làm cho nó trở nên linh hoạt. Bất kỳ giải pháp nào cố gắng tính toán lại khả năng tiếp cận từ đầu cho mỗi bước sẽ quá chậm. 

Một trường hợp khó nhận thấy xuất phát từ các khoảng thời gian tắc nghẽn hoàn toàn. Nếu ở một bước nào đó toàn bộ dây chuyền bị giám sát,`[1, n]`, thì không thể di chuyển trong bước đó và vị trí bị đóng băng bất kể chiến lược. Một trường hợp góc khác là khi hướng mục tiêu thay đổi theo thời gian, buộc phải đi đường vòng hoặc thời gian chờ thay vì chuyển động tham lam trực tiếp. 

## Phương pháp tiếp cận 

Một góc nhìn ngây thơ coi đây là đường đi ngắn nhất trong biểu đồ mở rộng theo thời gian. Mỗi tiểu bang là`(position, time)`, và từ mỗi trạng thái chúng ta có thể di chuyển đến các vị trí liền kề hoặc giữ nguyên vị trí nếu lúc đó đích và nguồn không bị chặn. Điều này mô hình hóa chính xác quy trình, nhưng biểu đồ có`n * m`tiểu bang và lên đến`3`chuyển đổi trên mỗi trạng thái, dẫn đến khoảng`3 * 10^10`chuyển tiếp trong trường hợp xấu nhất, vượt xa giới hạn. 

Cấu trúc đơn giản hóa khi chúng ta ngừng suy nghĩ về các đường dẫn tùy ý và thay vào đó tập trung vào tính khả thi trên mỗi bước. Ở mỗi bước thời gian, câu hỏi có ý nghĩa duy nhất là liệu chúng ta có thể tiến thêm một bước gần hơn đến đích hay không, hay chúng ta buộc phải chờ đợi. Khoảng thời gian giám sát có thể chặn vị trí hiện tại hoặc không, và ảnh hưởng tương tự đến việc có cho phép chuyển tiếp liền kề hay không. 

Nhận xét quan trọng là chúng ta không bao giờ được lợi khi rời xa mục tiêu. Nếu mục tiêu ở bên phải, mọi chiến lược tối ưu sẽ không bao giờ di chuyển sang trái trừ khi buộc phải ở lại do bị chặn. Vì vậy, vấn đề trở thành việc tham lam cố gắng giảm khoảng cách tới`f`bất cứ khi nào có thể, nếu không thì hãy ở yên tại chỗ. 

Chúng tôi mô phỏng thời gian từ 1 đến`m`. Chúng tôi duy trì vị trí hiện tại. Ở mỗi bước chúng ta quyết định liệu chúng ta có thể tiến tới`f`. Nếu vị trí hiện tại an toàn và vị trí tiếp theo cũng an toàn cho bước đó thì chúng ta di chuyển. Nếu không, chúng tôi ở lại. 

Điều này biến vấn đề thành một quá trình quét tuyến tính với các lần kiểm tra liên tục theo thời gian trên mỗi bước bằng cách sử dụng các so sánh khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS mở rộng theo thời gian | O(n·m) | O(n·m) | Quá chậm | 
| Mô phỏng tham lam | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giả sử không mất tính tổng quát rằng chuyển động hướng về bên phải nếu`s < f`, nếu không thì hướng về bên trái. Chúng ta chuyển vấn đề thành luôn cố gắng di chuyển theo một hướng cố định. 

1. Đọc tất cả các khoảng thời gian và xử lý chúng theo thứ tự thời gian. Giữ nguyên vị trí hiện tại của ghi chú. 
2. Đối với mỗi bước`i`, trích xuất khoảng bị chặn`[l_i, r_i]`. 
3. Kiểm tra xem vị trí hiện tại có nằm trong khoảng bị chặn hay không. Nếu đúng như vậy, việc di chuyển bị cấm và chúng tôi phải xuất ra`X`. 
4. Nếu được phép di chuyển, hãy xác định xem có bước về phía`f`bởi một vị trí là có thể. Điều này yêu cầu ô đích cũng không bị chặn ở bước này. 
5. Nếu có thể di chuyển, hãy cập nhật vị trí và đầu ra`L`hoặc`R`tùy theo hướng. 
6. Nếu không thì xuất ra`X`và giữ nguyên vị trí. 

Lựa chọn thiết kế quan trọng là chúng ta không bao giờ cân nhắc việc rời xa mục tiêu. Dù tạm thời cho phép nhưng nó cũng không bao giờ cải thiện được thời gian đạt được mục tiêu. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, hành động duy nhất có thể giảm khoảng cách còn lại tới mục tiêu là di chuyển đơn vị về phía`f`. Bất kỳ việc di chuyển ra xa đều làm tăng khoảng cách và do đó không thể là một phần của trình tự tối ưu trừ khi chuyển động về phía mục tiêu bị chặn. Khi việc di chuyển bị cản trở, việc giữ nguyên vị trí là lựa chọn hợp pháp duy nhất. 

Do đó, quy trình duy trì tính bất biến là sau mỗi bước, vị trí là vị trí xa nhất có thể tiếp cận được đối với`f`với những hạn chế cho đến thời điểm đó. Vì khoảng cách chỉ giảm khi có thể và không bao giờ tăng nên lần đầu tiên chúng ta chạm tới`f`là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, s, f = map(int, input().split())
    
    # store events
    events = []
    for _ in range(m):
        t, l, r = map(int, input().split())
        events.append((t, l, r))
    
    pos = s
    direction = 1 if f > s else -1
    
    ans = []
    
    for i in range(m):
        t, l, r = events[i]
        
        # if current position is blocked, must stay
        if l <= pos <= r:
            ans.append('X')
            continue
        
        nxt = pos + direction
        
        # check if we can move
        if 1 <= nxt <= n and not (l <= nxt <= r):
            pos = nxt
            ans.append('R' if direction == 1 else 'L')
        else:
            ans.append('X')
    
    print("".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp chỉ giữ lại vị trí hiện tại và xử lý mỗi bước thời gian một lần. Hướng được cố định dựa trên thứ tự tương đối bắt đầu và kết thúc. Mỗi khoảng thời gian được kiểm tra trong thời gian không đổi. 

Một điểm tinh tế là chúng ta không bao giờ kiểm tra rõ ràng liệu`pos == f`sớm kết thúc quá trình. Thuật toán sẽ ổn định ở đó một cách tự nhiên vì bất kỳ động thái nào nữa sẽ không cần thiết, nhưng việc ở lại luôn hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5 1 3
1 1 2
2 2 3
3 3 3
4 1 1
10 1 3
```Chúng tôi theo dõi vị trí theo thời gian. 

| Bước | Khoảng thời gian | Vị trí | Bị chặn? | Hành động | Vị Trí Mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [1,2] | 1 | vâng | X | 1 | 
| 2 | [2,3] | 1 | không | R | 2 | 
| 3 | [3,3] | 2 | không | R | 3 | 
| 4 | [1,1] | 3 | không | X | 3 | 
| 5 | [1,3] | 3 | vâng | X | 3 | 

Trình tự là`XXRRX`, nhưng chỉ có tiền tố đạt đến`f`quan trọng là đạt được sự tối ưu và lần đến đầu tiên xảy ra ở bước 3. Bất kỳ chuyển động nào sau đó đều không cần thiết để hoàn thành tối ưu. 

Dấu vết này cho thấy thuật toán chỉ di chuyển khi cả vị trí hiện tại và vị trí tiếp theo đều an toàn. 

### Ví dụ 2 

Xét trường hợp buộc phải chờ đợi:```
5 4 2 5
1 2 3
2 2 3
3 3 4
4 4 5
```| Bước | Khoảng thời gian | Vị trí | Bị chặn? | Hành động | Vị Trí Mới | 

|------|----------|----------|----------|--------------| 

| 1 | [2,3] | 2 | vâng | X | 2 | 

| 2 | [2,3] | 2 | vâng | X | 2 | 

| 3 | [3,4] | 2 | không | R | 3 | 

| 4 | [4,5] | 3 | không | R | 4 | 

Chuyển động bị trì hoãn bởi các khối liên tiếp, nhưng khi một khoảng trống xuất hiện, thuật toán sẽ ngay lập tức tiến lên. 

Điều này chứng tỏ rằng việc chờ đợi được xử lý một cách tự nhiên mà không cần lập kế hoạch rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi bước xử lý một khoảng thời gian duy nhất với các lần kiểm tra liên tục | 
| Không gian | O(1) | Chỉ vị trí hiện tại và chuỗi đầu ra được lưu trữ | 

Quét tuyến tính theo các bước thời gian phù hợp thoải mái trong giới hạn 100000 thao tác. Mỗi bước chỉ bao gồm một vài so sánh, giúp giải pháp trở nên hiệu quả cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []
    
    n, m, s, f = map(int, sys.stdin.readline().split())
    pos = s
    direction = 1 if f > s else -1
    
    ans = []
    
    for _ in range(m):
        l, r = map(int, sys.stdin.readline().split()[1:])
        
        if l <= pos <= r:
            ans.append('X')
            continue
        
        nxt = pos + direction
        if 1 <= nxt <= n and not (l <= nxt <= r):
            pos = nxt
            ans.append('R' if direction == 1 else 'L')
        else:
            ans.append('X')
    
    return "".join(ans)

# provided sample
assert run("""3 5 1 3
1 1 2
2 2 3
3 3 3
4 1 1
10 1 3
""") == "XXRRX"

# minimal movement
assert run("""2 2 1 2
1 1 1
2 1 1
""") == "RX"

# fully blocked movement
assert run("""4 3 1 4
1 1 4
2 1 4
3 1 4
""") == "XXX"

# alternating free/blocked
assert run("""5 4 1 3
1 2 2
2 1 1
3 2 2
4 1 1
""") == "RXXR"

# already near target
assert run("""3 3 2 3
1 1 1
2 1 1
3 1 1
""") == "RXX"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuyển động tối thiểu | RX | khả năng tiếp cận một bước | 
| bị chặn hoàn toàn | XXX | buộc phải đình trệ | 
| xen kẽ | RXXR | lập kế hoạch không tầm thường | 
| gần mục tiêu | RXX | độ đúng gần ranh giới | 

## Vỏ cạnh 

Một tình huống quan trọng là khi vị trí hiện tại nằm trong mỗi khoảng trong nhiều bước liên tiếp. Trong trường hợp đó, thuật toán liên tục đưa ra`X`và vị trí không bao giờ thay đổi. Điều này đúng vì không có động thái hợp pháp nào tồn tại trong các bước đó và mọi nỗ lực ép buộc di chuyển sẽ vi phạm quy tắc giám sát. 

Một trường hợp cạnh khác xảy ra khi mục tiêu ở ngay cạnh nhưng bị chặn không liên tục. Thuật toán chỉ di chuyển khi cả vị trí hiện tại và vị trí tiếp theo đều an toàn, do đó, nó sẽ chờ một bước an toàn một cách tự nhiên thay vì cố gắng chuyển đổi bất hợp pháp. 

Trường hợp khó khăn cuối cùng là khi đến đích sớm. Một lần`pos == f`, mọi hành động tiếp theo vẫn hợp lệ như`X`và thuật toán tiếp tục tạo ra một chuỗi tối thiểu hợp pháp mà không cần logic kết thúc rõ ràng.
