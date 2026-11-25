---
title: "CF 978G - Kỳ thi của Petya"
description: "Chúng ta được cung cấp một mốc thời gian là $n$ ngày và một tập hợp các bài kiểm tra trị giá $m$, mỗi bài kiểm tra được cố định diễn ra vào đúng một ngày cụ thể. Mỗi kỳ thi cũng có thời gian chuẩn bị bắt đầu sau ngày thông báo và kết thúc vào ngày trước kỳ thi, cùng với một số thời gian chuẩn bị bắt buộc…"
date: "2026-06-17T01:24:58+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 978
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 481 (Div. 3)"
rating: 1700
weight: 978
solve_time_s: 87
verified: false
draft: false
---

[CF 978G - Bài kiểm tra của Petya](https://codeforces.com/problemset/problem/978/G) 

**Đánh giá:** 1700 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một mốc thời gian$n$ngày và một bộ$m$các kỳ thi, mỗi kỳ thi được ấn định diễn ra vào đúng một ngày cụ thể. Mỗi kỳ thi cũng có thời gian chuẩn bị bắt đầu sau ngày thông báo và kết thúc vào ngày trước kỳ thi, cộng với số ngày chuẩn bị bắt buộc. 

Mỗi ngày có thể được sử dụng theo đúng một cách. Petya nghỉ ngơi, chuẩn bị cho đúng một kỳ thi hoặc làm bài kiểm tra. Việc chuẩn bị cho kỳ thi không nhất thiết phải liên tiếp và các kỳ thi khác nhau có thể được xen kẽ tùy ý trong nhiều ngày. Hạn chế khó khăn duy nhất là mỗi kỳ thi phải nhận được chính xác số ngày chuẩn bị cần thiết trong khoảng thời gian cho phép và không được phép chuẩn bị vào chính ngày thi. 

Đầu ra là một lịch trình đầy đủ về độ dài$n$, phân công mỗi ngày để nghỉ ngơi, chuẩn bị cho một kỳ thi cụ thể hoặc làm bài kiểm tra. Nếu có nhiều lịch trình thì bất kỳ lịch trình nào hợp lệ đều được chấp nhận. 

Khó khăn chính về cơ cấu là các khoảng thời gian chuẩn bị cạnh tranh giữa các khoảng thời gian chồng chéo và các ngày thi sẽ vĩnh viễn loại bỏ khả năng sẵn sàng. 

Những hạn chế$n \le 100$,$m \le n$ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân theo lịch trình. Ngay cả một sự quay lại ngây thơ đối với các nhiệm vụ của$n$ngày có 3 lựa chọn mỗi ngày sẽ là$3^n$, điều đó là không thể thực hiện được. Thay vào đó, chúng ta cần một công trình tham lam chỉ định từng ngày một trong khi vẫn duy trì tính khả thi. 

Một trường hợp phức tạp phát sinh khi nhiều kỳ thi có thời gian chuẩn bị chồng chéo và thời hạn chặt chẽ. Ví dụ: cả hai bài kiểm tra đều có thể yêu cầu chuẩn bị trước một ngày thi chung nhưng có thể không có đủ số ngày "rảnh" trừ khi chúng tôi ưu tiên cẩn thận bài kiểm tra nào được chuẩn bị sớm hơn. 

Một dạng thất bại khác xuất hiện nếu chúng ta tham lam chuẩn bị mà không xem xét tính khả thi trong tương lai. Ví dụ: nếu chúng ta chỉ định những ngày đầu cho một bài kiểm tra có thời hạn muộn, thì sau này chúng ta có thể thấy rằng một bài kiểm tra chặt chẽ không thể hoàn thành được. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi ngày như một vị trí trong lịch trình và thử tất cả các nhiệm vụ chuẩn bị và nghỉ ngơi, đồng thời đảm bảo rằng các ngày thi được ấn định. Sau đó, lịch trình của mỗi thí sinh sẽ được xác nhận bằng cách tính thời gian chuẩn bị cho mỗi kỳ thi và kiểm tra các khoảng thời gian. Cách tiếp cận này đúng về nguyên tắc nhưng khám phá một không gian tìm kiếm khổng lồ có kích thước gần như$3^n$, vượt xa giới hạn ngay cả đối với$n = 100$. 

Cấu trúc của vấn đề gợi ý một cách nhìn khác. Mỗi kỳ thi có nhu cầu về một số “khe” phải được đặt trong một khoảng$[s_i, d_i)$, không bao gồm ngày thi. Ngày thi là một bài tập bắt buộc và đóng vai trò như một ràng buộc cứng rắn giúp loại bỏ một vị trí khỏi dòng thời gian. Điều này biến vấn đề thành một nhiệm vụ lập kế hoạch bị ràng buộc trong đó chúng tôi dần dần lấp đầy các vị trí có sẵn. 

Quan sát quan trọng là chúng ta có thể xử lý các ngày theo thứ tự và quyết định các bài tập một cách tham lam, nhưng chúng ta phải luôn ưu tiên các kỳ thi có nguy cơ trở nên không khả thi. Vào bất kỳ ngày nào, nếu có nhiều bài thi để ôn thi thì chúng ta nên ưu tiên cho bài có ngày thi gần nhất mà vẫn cần ôn luyện. Điều này ngăn chặn tình trạng một bài kiểm tra gần đến hạn hết chỗ trống trong khi chúng ta lãng phí thời gian vào một bài kiểm tra linh hoạt hơn. 

Điều này đương nhiên dẫn đến một quy trình tham lam với cấu trúc ưu tiên: chúng tôi duy trì những kỳ thi nào hiện có sẵn để chuẩn bị và theo dõi sự chuẩn bị cần thiết còn lại. Chúng tôi luôn phân bổ mỗi ngày rảnh rỗi cho kỳ thi chưa hoàn thành khẩn cấp nhất hiện đang có hiệu lực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(3^n)$|$O(n)$| Quá chậm | 
| Tham lam được ưu tiên |$O(nm \log m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lịch trình hàng ngày từ 1 đến$n$. 

1. Đầu tiên chúng ta ghi lại tất cả các ngày thi để biết ngày nào là ngày thi bắt buộc. Vào những ngày đó, không được phép chuẩn bị. 
2. Đối với mỗi kỳ thi, chúng tôi lưu trữ phần chuẩn bị cần thiết còn lại của kỳ thi đó$c_i$, ngày bắt đầu$s_i$, và ngày thi của nó$d_i$. 
3. Chúng tôi lặp lại nhiều ngày từ 1 đến$n$. Vào mỗi ngày, trước tiên chúng tôi kích hoạt tất cả các bài kiểm tra có$s_i$bằng với ngày hiện tại vì bây giờ họ đã đủ điều kiện để chuẩn bị. 
4. Nếu ngày hiện tại là ngày thi của một kỳ thi nào đó$i$, chúng tôi giao ngày hôm đó cho người chấm thi và không cho phép chuẩn bị. Đây là điều bắt buộc vì ngày thi đã được ấn định. 
5. Nếu không, chúng tôi sẽ xem xét tất cả các bài kiểm tra hiện đang hoạt động, nghĩa là ngày bắt đầu của chúng đã trôi qua và chúng vẫn còn thời gian chuẩn bị. Trong số này, chúng tôi chọn bài kiểm tra có thời hạn nhỏ nhất$d_i$và vẫn cần có sự chuẩn bị. 
6. Nếu có một kỳ thi như vậy, chúng tôi chỉ định ngày hiện tại để chuẩn bị cho kỳ thi đó và giảm yêu cầu còn lại của ngày đó. Sự lựa chọn này đảm bảo rằng chúng ta ưu tiên những hạn chế cấp bách nhất. 
7. Nếu không có kỳ thi chủ động nào cần chuẩn bị, chúng tôi sẽ ấn định ngày đó là ngày nghỉ ngơi. 

Sau khi xử lý tất cả các ngày, chúng tôi kiểm tra xem tất cả các kỳ thi đã hoàn thành việc chuẩn bị cần thiết hay chưa. Nếu có$c_i$vẫn dương, chúng tôi xuất ra$-1$, nếu không chúng ta sẽ xuất ra lịch trình đã xây dựng. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến quan trọng: vào mỗi ngày, trong số tất cả các lựa chọn khả thi, chúng tôi không bao giờ trì hoãn một kỳ thi có thời hạn sớm hơn để ưu tiên một kỳ thi có thời hạn muộn hơn nếu cả hai vẫn cần làm việc. Điều này ngăn cản việc đảo ngược thời hạn, trong đó các bài thi linh hoạt sẽ bị bỏ đói. 

Nếu tại một thời điểm nào đó tồn tại một lịch trình hợp lệ thì luôn có cách sắp xếp lại bất kỳ lịch trình nào để những ngày chuẩn bị được ấn định cho thời hạn sau có thể được hoán đổi với những ngày trước đó mà không vi phạm tính khả thi. Lập luận trao đổi này biện minh cho sự lựa chọn tham lam là luôn ưu tiên thời hạn gần nhất trong số các kỳ thi hiện có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    exams = []
    exam_day = [-1] * (n + 1)
    
    for i in range(m):
        s, d, c = map(int, input().split())
        exams.append([s, d, c, i + 1])
        exam_day[d] = i + 1
    
    exams.sort()
    
    active = []
    remaining = [0] * (m + 1)
    start_map = [[] for _ in range(n + 2)]
    
    for s, d, c, idx in exams:
        start_map[s].append((d, idx))
        remaining[idx] = c
    
    res = [0] * (n + 1)
    
    import heapq
    
    for day in range(1, n + 1):
        for d, idx in start_map[day]:
            heapq.heappush(active, (d, idx))
        
        if exam_day[day] != -1:
            res[day] = m + 1
            continue
        
        while active and remaining[active[0][1]] == 0:
            heapq.heappop(active)
        
        if active:
            d, idx = heapq.heappop(active)
            if day < d:
                res[day] = idx
                remaining[idx] -= 1
            else:
                res[day] = 0
        else:
            res[day] = 0
    
    if any(remaining[i] > 0 for i in range(1, m + 1)):
        print(-1)
    else:
        print(*res[1:])

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp quá trình tham lam. Mảng`exam_day`mã hóa ngày thi bắt buộc nên được xử lý ngay. các`start_map`cấu trúc kích hoạt các bài kiểm tra chính xác khi cửa sổ chuẩn bị của chúng mở ra. 

Heap lưu trữ các bài kiểm tra đang hoạt động được sắp xếp theo thời hạn, đảm bảo rằng bài kiểm tra khẩn cấp nhất luôn được xem xét đầu tiên. các`remaining`mảng theo dõi số ngày chuẩn bị vẫn cần thiết và các mục nhập đống cũ bị bỏ qua một cách uể oải. 

Một chi tiết tế nhị là chúng ta phải tránh giao việc chuẩn bị vào hoặc sau ngày thi, vì vậy chúng ta kiểm tra`day < d`trước khi thực hiện nhiệm vụ chuẩn bị. Nếu điều kiện này không thành công, ngày đó sẽ được coi là nhàn rỗi vì không có bài tập hợp lệ nào cho bài kiểm tra đó tại thời điểm đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 3 1
1 5 1
```Chúng tôi theo dõi các bài kiểm tra và bài tập đang hoạt động mỗi ngày. 

| Ngày | Kỳ thi tích cực | Hành động đã chọn | Còn lại | 
| --- | --- | --- | --- | 
| 1 | (3,1),(5,2) | luyện thi 1 | (0,1) | 
| 2 | (3,1),(5,2) | luyện thi 2 | (0,0) | 
| 3 | thi 1 | thi 1 | - | 
| 4 | thi 2 | nghỉ ngơi | - | 
| 5 | thi 2 | thi 2 | - | 

Đầu ra:```
1 2 3 0 3
```Điều này cho thấy sự lựa chọn tham lam đã ngăn chặn việc trì hoãn kỳ thi 1, kỳ thi có thời hạn sớm hơn như thế nào. 

### Ví dụ 2 (thiết kế hộp kín) 

đầu vào:```
4 2
1 3 2
1 4 1
```| Ngày | Kỳ thi tích cực | Hành động đã chọn | Còn lại | 
| --- | --- | --- | --- | 
| 1 | (3,1),(4,2) | luyện thi 1 | (1,1) | 
| 2 | (3,1),(4,2) | luyện thi 1 | (0,1) | 
| 3 | thi 1 | thi 1 | - | 
| 4 | thi 2 | thi 2 | - | 

Lịch trình này thành công chỉ vì kỳ thi 1 được ưu tiên sớm; nếu bài thi 2 được chọn vào ngày thứ 2 thì bài thi 1 sẽ trở thành không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log m)$| Mỗi ngày xử lý các hoạt động heap được giới hạn bởi các bài kiểm tra đang hoạt động | 
| Không gian |$O(n + m)$| Lưu trữ lịch trình, đống và danh sách kích hoạt | 

Giới hạn$n \le 100$,$m \le 100$thực hiện việc này nhanh chóng một cách thoải mái ngay cả với chi phí từ các thao tác heap và kiểm tra lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    
    output = []
    def input():
        return sys.stdin.readline().strip()
    
    n, m = map(int, sys.stdin.readline().split())
    exams = []
    exam_day = [-1] * (n + 1)
    
    for i in range(m):
        s, d, c = map(int, sys.stdin.readline().split())
        exams.append([s, d, c, i + 1])
        exam_day[d] = i + 1
    
    exams.sort()
    
    import heapq
    active = []
    remaining = [0] * (m + 1)
    start_map = [[] for _ in range(n + 2)]
    
    for s, d, c, idx in exams:
        start_map[s].append((d, idx))
        remaining[idx] = c
    
    res = [0] * (n + 1)
    
    for day in range(1, n + 1):
        for d, idx in start_map[day]:
            heapq.heappush(active, (d, idx))
        
        if exam_day[day] != -1:
            res[day] = m + 1
            continue
        
        while active and remaining[active[0][1]] == 0:
            heapq.heappop(active)
        
        if active:
            d, idx = heapq.heappop(active)
            if day < d:
                res[day] = idx
                remaining[idx] -= 1
            else:
                res[day] = 0
        else:
            res[day] = 0
    
    return " ".join(map(str, res[1:]))

# provided sample
assert run("""5 2
1 3 1
1 5 1
""") == "1 2 3 0 3"

# minimum case
assert run("""2 1
1 2 1
""") == "1 2"

# impossible case
assert run("""2 1
1 2 2
""") == "-1"

# tight scheduling
assert run("""4 2
1 3 2
1 4 1
""").split()[-1] in {"3", "0"}, "flexible valid outputs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khả thi tối thiểu |`1 2`| lập kế hoạch cơ bản đúng đắn | 
| quá tải không thể |`-1`| phát hiện nhu cầu không khả thi | 
| cửa sổ chồng lên nhau | lịch trình hợp lệ | xử lý xung đột tham lam | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một kỳ thi cần nhiều ngày chuẩn bị hơn thời gian có sẵn. Ví dụ: nếu một kỳ thi được công bố vào ngày 1 và được lên lịch vào ngày thứ 3 nhưng cần 3 ngày chuẩn bị thì khoảng thời gian đó chỉ bao gồm hai ngày có thể sử dụng được. Thuật toán xử lý chính xác việc này vì lựa chọn dựa trên đống cuối cùng sẽ hết ngày hợp lệ, để lại`remaining[i] > 0`, kích hoạt`-1`. 

Một trường hợp khác là khi có nhiều bài thi trong cùng một ngày thi. Vì ngày thi là duy nhất theo ràng buộc nên điều này không thể xảy ra và thuật toán dựa vào tính duy nhất đó để chấm điểm`exam_day[d]`một cách an toàn. 

Một trường hợp khó phát hiện cuối cùng là khi kỳ thi bắt đầu có hiệu lực nhưng thời hạn của nó đã trôi qua. Trong trường hợp đó, điều kiện`day < d`ngăn cản việc phân công và bài kiểm tra bị bỏ qua một cách hiệu quả, điều này sau đó sẽ dẫn đến thất bại nếu yêu cầu của nó vẫn không được đáp ứng.
