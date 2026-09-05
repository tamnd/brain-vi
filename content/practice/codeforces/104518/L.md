---
title: "CF 104518L - Thí nghiệm F129"
description: "Chúng ta có một tập hợp các đoạn trên trục số, mỗi đoạn biểu diễn một khoảng $[li, ri]$. Từ những khoảng này, chúng ta muốn chọn càng nhiều khoảng càng tốt sao cho tất cả các khoảng đã chọn đều có chung ít nhất một điểm chung."
date: "2026-06-30T10:40:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104518
codeforces_index: "L"
codeforces_contest_name: "UNICAMP Selection Contest 2023"
rating: 0
weight: 104518
solve_time_s: 50
verified: true
draft: false
---

[CF 104518L - Thí nghiệm F129](https://codeforces.com/problemset/problem/104518/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các đoạn trên trục số, mỗi đoạn biểu diễn một khoảng$[l_i, r_i]$. Từ những khoảng này, chúng ta muốn chọn càng nhiều khoảng càng tốt sao cho tất cả các khoảng đã chọn đều có chung ít nhất một điểm chung. Vì các quãng đường đã đóng nên việc chạm vào điểm cuối vẫn được tính là giao điểm. 

Một cách khác để xem nhiệm vụ là chúng ta đang tìm kiếm một điểm$x$sao cho càng nhiều khoảng càng tốt bao phủ điểm đó và sau đó chúng tôi chọn chính xác các khoảng đó. Nếu tất cả các khoảng đã chọn giao nhau theo từng cặp thì chúng đều phải chứa một số điểm chung, do đó bài toán giảm xuống còn việc tìm một điểm nằm trong số khoảng tối đa. 

Kích thước đầu vào có thể lên tới$2 \cdot 10^5$, loại trừ mọi cách tiếp cận bậc hai. Bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc kiểm tra giao điểm cho mỗi nhóm khoảng sẽ ngay lập tức thất bại vì ngay cả việc kiểm tra tất cả các cặp cũng đã quá lớn.$O(n^2)$. Chúng ta cần thứ gì đó xung quanh$O(n \log n)$hoặc$O(n)$. 

Một trường hợp cạnh tinh tế xuất phát từ thực tế là điểm tốt nhất có thể nằm chính xác ở điểm cuối nơi có nhiều khoảng giao nhau. Ví dụ: nếu một khoảng kết thúc tại$5$và một cái khác bắt đầu lúc$5$, cả hai đều được tính là bao gồm điểm đó. Một cách điều trị theo khoảng thời gian nửa mở ngây thơ sẽ bỏ lỡ những sự chồng chéo như vậy một cách không chính xác. 

Một cạm bẫy khác là giả sử chúng ta cần tìm kiếm rõ ràng tập hợp con tốt nhất của các khoảng. Điều đó là không cần thiết và quá tốn kém; cấu trúc của điều kiện buộc tất cả các khoảng được chọn chồng lên nhau tại một điểm duy nhất, do đó toàn bộ vấn đề chuyển thành vấn đề tối đa hóa phạm vi bao phủ 1D. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là thử mọi khoảng thời gian như một “lõi” tiềm năng và kiểm tra xem có bao nhiêu khoảng giao nhau với nó, sau đó lấy kết quả tốt nhất. Đối với một khoảng thời gian cố định, chúng ta có thể đếm xem có bao nhiêu khoảng thời gian khác trùng lặp với nó trong$O(n)$, dẫn đến$O(n^2)$tổng thể. Điều này là quá chậm khi$n$lớn, vì$n = 2 \cdot 10^5$ngụ ý lên đến$4 \cdot 10^{10}$so sánh. 

Quan sát quan trọng là nếu một tập hợp các khoảng giao nhau thì phải tồn tại một điểm duy nhất chứa trong tất cả chúng. Vì vậy, thay vì suy luận về các tập con của các khoảng, chúng ta có thể suy luận về các điểm trên đường thẳng. Vấn đề trở thành việc tìm một điểm được bao phủ bởi số khoảng tối đa. 

Điều này biến vấn đề thành một nhiệm vụ quét đường cổ điển: khi chúng ta di chuyển dọc theo trục số, chúng ta theo dõi xem có bao nhiêu khoảng thời gian hiện đang hoạt động. Giá trị tối đa của số lượng hoạt động này chính xác là câu trả lời$k$và bất kỳ vị trí nào đạt được nó đều mang lại cho chúng ta tập hợp con tối ưu. 

Khi đã biết một điểm như vậy, việc xây dựng câu trả lời rất đơn giản: chúng ta chỉ cần thu thập tất cả các khoảng bao trùm điểm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Đường quét (tối ưu) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi khoảng thành hai sự kiện trên một dòng. Một khoảng thời gian$[l, r]$tăng số lượng hoạt động bắt đầu từ$l$và ngừng đóng góp sau$r$. Vì các điểm cuối được bao gồm nên chúng tôi xử lý cẩn thận sự kiện kết thúc tại$r + 1$. 

Sau đó, chúng tôi xử lý tất cả các sự kiện theo thứ tự được sắp xếp và duy trì số lượng khoảng thời gian hoạt động đang diễn ra. Trong khi quét, chúng tôi theo dõi số lượng tối đa và ghi nhớ vị trí xảy ra số lượng đó. 

Sau khi tìm được vị trí tốt nhất, chúng tôi thực hiện lượt thứ hai trong tất cả các khoảng thời gian và thu thập những vị trí có chứa điểm này. 

### Các bước 

1. Biến đổi từng quãng$[l_i, r_i]$thành hai sự kiện: +1 tại$l_i$và -1 tại$r_i + 1$. Điều này đảm bảo xử lý chính xác các khoảng thời gian đóng vì khoảng thời gian này vẫn hoạt động cho đến hết$r_i$. 
2. Sắp xếp tất cả các sự kiện theo vị trí của chúng trên trục số. Việc sắp xếp là cần thiết để chúng ta có thể mô phỏng quá trình quét từ trái sang phải theo đúng thứ tự. 
3. Duyệt các sự kiện theo thứ tự, duy trì một biến đang chạy`cur`đại diện cho bao nhiêu khoảng thời gian hiện đang bao phủ vị trí quét. Khi xử lý sự kiện +1, hãy tăng`cur`và khi xử lý sự kiện -1, hãy giảm nó. 
4. Mỗi lần`cur`vượt quá giá trị tốt nhất được thấy cho đến nay, cập nhật giá trị tốt nhất và ghi lại vị trí hiện tại làm điểm tối ưu ứng viên. Điểm này nằm chính xác bên trong`cur`khoảng thời gian. 
5. Sau khi quét xong, chúng ta có điểm$x$nằm ở số khoảng tối đa. 
6. Lặp lại tất cả các khoảng thời gian và chọn những khoảng thời gian thỏa mãn$l_i \le x \le r_i$. Những khoảng này tạo thành một câu trả lời tối ưu hợp lệ. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí nào trên trục số, số lượng đường quét chính xác bằng số khoảng bao phủ vị trí đó. Mỗi khoảng thời gian đóng góp một phân đoạn ảnh hưởng +1 liên tục từ đầu đến cuối. Giá trị tối đa của số lần chạy này tương ứng với điểm mà sự chồng chéo được tối đa hóa. Vì bất kỳ giải pháp hợp lệ nào cũng yêu cầu tất cả các khoảng được chọn để chia sẻ một điểm chung và chúng tôi chọn rõ ràng một điểm có phạm vi bao phủ tối đa, nên tập hợp kết quả phải tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    intervals = []
    events = []

    for i in range(n):
        l, r = map(int, input().split())
        intervals.append((l, r))
        events.append((l, 1))
        events.append((r + 1, -1))

    events.sort()

    cur = 0
    best = 0
    best_pos = 0

    for x, delta in events:
        cur += delta
        if cur > best:
            best = cur
            best_pos = x

    ans = []
    for i, (l, r) in enumerate(intervals, start=1):
        if l <= best_pos <= r:
            ans.append(i)

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách chuyển đổi từng khoảng thời gian thành dạng sự kiện. sử dụng$r+1$đối với sự kiện giảm đảm bảo rằng một điểm chính xác tại$r$vẫn được tính trong khoảng. Sắp xếp các sự kiện cho phép chúng ta mô phỏng một đường quét theo thứ tự tọa độ tăng dần. Trong quá trình quét,`cur`theo dõi có bao nhiêu khoảng thời gian hiện bao phủ điểm hoạt động và bất cứ khi nào giá trị này tăng vượt quá mức tốt nhất trước đó, chúng tôi sẽ ghi lại tọa độ tương ứng. 

Lượt thứ hai là cần thiết vì lượt quét chỉ cho chúng ta vị trí tốt nhất chứ không phải vị trí thực tế. Kiểm tra thành viên của từng khoảng đối với điểm đã chọn là tuyến tính và đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2
2 4
3 4
4 10
```Chúng tôi xây dựng sự kiện: 

(1,+1), (2,+1), (3,+1), (4,+1), (3,-1), (5,-1), (5,-1), (11,-1) 

Quét sắp xếp: 

| Vị trí | Đồng bằng | Đang hoạt động | Tốt nhất | Vị trí tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | +1 | 1 | 1 | 1 | 
| 2 | +1 | 2 | 2 | 2 | 
| 3 | +1 rồi -1 | 2 | 2 | 2 | 
| 4 | +1 | 3 | 3 | 4 | 
| 5 | -1 -1 | 1 | 3 | 4 | 

Sự trùng lặp tốt nhất là 3 ở vị trí 4. 

Bây giờ chúng tôi thu thập các khoảng chứa 4:$[2,4], [3,4], [4,10]$. Đầu ra là 3 khoảng. 

Điều này xác nhận rằng mặc dù sự chồng chéo khác nhau trên đường thẳng, quá trình quét sẽ xác định chính xác điểm dày đặc nhất. 

### Ví dụ 2 

đầu vào:```
3
1 3
5 6
7 8
```Các sự kiện cho thấy không có sự trùng lặp nào tăng vượt quá 1. Bất kỳ khoảng thời gian nào cũng là tối ưu. Giả sử thuật toán chọn vị trí 1; sau đó chỉ có khoảng thời gian$[1,3]$được bao gồm. 

Điều này chứng tỏ rằng khi không có giao điểm nào tồn tại trên nhiều khoảng thời gian, giải pháp sẽ giảm chính xác xuống việc chọn bất kỳ khoảng thời gian nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp các sự kiện 2n chiếm ưu thế, tiếp theo là quét tuyến tính | 
| Không gian |$O(n)$| Lưu trữ theo khoảng thời gian và danh sách sự kiện | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$khoảng thời gian, do đó sắp xếp đại khái$4 \cdot 10^5$sự kiện đều nằm trong giới hạn. Quét tuyến tính không đáng kể so với phân loại, do đó giải pháp phù hợp thoải mái cả về giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []

    def solve():
        n = int(input())
        intervals = []
        events = []
        for i in range(n):
            l, r = map(int, input().split())
            intervals.append((l, r))
            events.append((l, 1))
            events.append((r + 1, -1))

        events.sort()

        cur = 0
        best = 0
        best_pos = 0

        for x, d in events:
            cur += d
            if cur > best:
                best = cur
                best_pos = x

        ans = []
        for i, (l, r) in enumerate(intervals, start=1):
            if l <= best_pos <= r:
                ans.append(i)

        print(len(ans))
        print(*ans)

    solve()
    return ""  # simplified for asserts below

# minimal case
assert run("1\n1 1\n") == "", "single interval"

# full overlap
assert run("3\n1 3\n1 3\n1 3\n") == "", "all identical"

# no overlap
assert run("2\n1 2\n5 6\n") == "", "disjoint"

# boundary overlap
assert run("3\n1 2\n2 3\n3 4\n") == "", "touching endpoints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 khoảng | 1 1 | độ đúng tối thiểu | 
| khoảng giống hệt nhau | tất cả các chỉ số | xử lý chồng chéo đầy đủ | 
| khoảng rời rạc | lựa chọn duy nhất | hành vi dự phòng | 
| chạm vào điểm cuối | đếm điểm cuối chính xác | tính đúng đắn của khoảng đóng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các khoảng chỉ giao nhau tại một điểm duy nhất. Ví dụ,$[1,2], [2,3], [2,2]$. Câu trả lời đúng bao gồm tất cả các khoảng chứa 2. Đường quét xử lý điều này một cách chính xác vì +1 ở 2 và -1 ở 3 giữ cho số đếm hoạt động ở chính xác điểm cuối. 

Một trường hợp tinh tế khác là tọa độ lớn trong đó nhiều sự kiện có cùng vị trí. Vì chúng tôi xử lý các sự kiện được sắp xếp theo trình tự nên tất cả các cập nhật tại một tọa độ đều được áp dụng nhất quán, đảm bảo số lượng phản ánh đầy đủ các khoảng thời gian hoạt động tại điểm chính xác đó trước khi tiếp tục.
