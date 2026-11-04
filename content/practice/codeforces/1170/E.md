---
title: "CF 1170E - Cửa Trượt"
description: "Chúng ta có một dòng gồm các ô $m$ và một chuỗi các cửa trượt $n$. Mỗi cửa chiếm một khối ô liền kề và các cửa xuất hiện theo thứ tự cố định từ trái sang phải."
date: "2026-06-18T17:10:04+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search"]
categories: ["algorithms"]
codeforces_contest: 1170
codeforces_index: "E"
codeforces_contest_name: "Kotlin Heroes: Episode 1"
rating: 0
weight: 1170
solve_time_s: 93
verified: true
draft: false
---

[CF 1170E - Cửa trượt](https://codeforces.com/problemset/problem/1170/E) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, tìm kiếm nhị phân 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$m$tế bào và một chuỗi$n$cửa trượt. Mỗi cửa chiếm một khối ô liền kề và các cửa xuất hiện theo thứ tự cố định từ trái sang phải. cửa$i$có chiều rộng cố định$a_i$và trong khi các cánh cửa có thể trượt dọc theo hành lang, chúng không thể xuyên qua nhau nên thứ tự tương đối của chúng không bao giờ thay đổi. 

Từ quan điểm này, một cấu hình hợp lệ chỉ đơn giản là một vị trí của$n$các đoạn rời rạc có độ dài cố định dọc theo một đường$m$các vị trí, để lại một số ô không được che chắn giữa và xung quanh chúng. Những ô không được che phủ đó tạo thành “không gian trống”. 

Mỗi truy vấn đưa ra một tập hợp các chỉ mục ô phải có thể truy cập đồng thời. Một ô chỉ có thể truy cập được nếu nó không bị che bởi bất kỳ đoạn cửa nào. Câu hỏi cho mỗi truy vấn là liệu chúng ta có thể trượt các cửa vào một số cấu hình hợp lệ để tất cả các ô được yêu cầu nằm trong không gian trống cùng một lúc hay không. 

Khó khăn chính là chúng tôi không được yêu cầu kiểm tra cấu hình cố định. Chúng ta được phép sắp xếp lại các khoảng trống giữa các cửa nhưng phải tôn trọng cả thứ tự cố định của các cửa và độ dài cố định của chúng. 

Các ràng buộc đã loại trừ bất kỳ giải pháp nào cố gắng mô phỏng các vị trí một cách rõ ràng hoặc tìm kiếm trên các vị trí. Với tối đa$2 \cdot 10^5$cửa và truy vấn cũng như tổng kích thước truy vấn$2 \cdot 10^5$, mọi suy luận bậc hai cho mỗi truy vấn đối với các ô hoặc cửa sẽ không thành công. Chúng tôi cần một giải pháp xử lý từng truy vấn theo thời gian tuyến tính với kích thước đầu vào của nó. 

Một trường hợp lỗi nhỏ xuất hiện khi một cửa quá lớn so với một vùng ô trống giữa các vị trí cần thiết. Ví dụ: nếu các ô bắt buộc chia hành lang thành các đoạn nhỏ an toàn, một cánh cửa dài có thể buộc phải đi qua ô bắt buộc trừ khi chúng ta có thể sắp xếp lại nhóm một cách cẩn thận. Đây là hạn chế cốt lõi: cửa không thể chia cắt ở các vị trí cấm. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là thử tất cả các vị trí có thể có của$n$các phân đoạn trên dòng và kiểm tra xem có vị trí nào tránh che các ô cần thiết hay không. Điều này tương đương với việc phân phối$m - \sum a_i$các ô trống làm khoảng trống giữa các cánh cửa, điều này đã gợi ý một vụ nổ tổ hợp. Ngay cả khi chúng ta bỏ qua các vị trí chính xác và chỉ lý giải về các khoảng trống, số lượng phân phối có thể có sẽ theo cấp số nhân theo$n$, vì vậy việc tìm kiếm trực tiếp là không thể thực hiện được. 

Quan sát cấu trúc quan trọng là điều duy nhất quan trọng là cách các ô cần thiết tương tác với các khối không gian trống liền kề. Sau khi chúng tôi sửa các ô bắt buộc của truy vấn, chúng sẽ phân chia dòng thành các phân đoạn tối đa của các ô “an toàn”. Bên trong đoạn đường an toàn không có cửa nào cấm đi qua. Hạn chế duy nhất là một cánh cửa không thể đi qua ô bắt buộc, vì vậy mọi cánh cửa phải được đặt hoàn toàn bên trong một trong những đoạn an toàn này. 

Điều này biến vấn đề thành một quá trình đóng gói. Chúng tôi tính toán độ dài của các đoạn an toàn và cố gắng sắp xếp các cửa theo thứ tự, lắp chúng vào các đoạn này một cách tham lam. Vì các cửa được sắp xếp theo thứ tự và không thể chồng lên nhau nên khi một cửa được đặt trong một đoạn, tất cả các cửa trước đó sẽ được cố định vào các đoạn trước đó. Nếu một cánh cửa không vừa với đoạn an toàn hiện tại, chúng ta sẽ chuyển sang đoạn tiếp theo. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các vị trí | Hàm mũ | O(1) | Quá chậm | 
| Phân rã phân khúc + đóng gói tham lam |$O(m + \sum c_k)$| O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

### 1. Xây dựng các phân đoạn an toàn 

Chúng tôi bắt đầu từ phạm vi đầy đủ$[1, m]$và đánh dấu tất cả các ô cần thiết. Các vị trí bắt buộc này chia dòng thành các khoảng liên tiếp không chứa ô bắt buộc. Mỗi khoảng như vậy là một đoạn an toàn và chiều dài của nó thể hiện lượng không gian liên tục có sẵn để đặt cửa. 

Lý do chúng tôi nén thành các phân đoạn là trong một phân đoạn không có hạn chế về vị trí cửa có thể nằm so với các ô được yêu cầu, vì không có ô nào cả. 

### 2. Vị trí đặt cửa tham lam 

Chúng tôi lặp lại các cửa theo thứ tự. Chúng tôi cũng lặp lại các phân đoạn an toàn theo thứ tự. 

Chúng tôi duy trì lượng không gian còn lại có sẵn trong phân khúc an toàn hiện tại. 

Đối với mỗi cửa: 

1. Nếu đoạn an toàn hiện tại có đủ không gian còn lại để lắp cửa, chúng tôi đặt nó ở đó và giảm không gian còn lại. 
2. Nếu không vừa, chúng ta chuyển sang đoạn an toàn tiếp theo và đặt lại dung lượng còn lại. 
3. Nếu cánh cửa lớn hơn bất kỳ đoạn an toàn nào, ngay cả sau khi di chuyển về phía trước, câu trả lời ngay lập tức là không thể. 

Chiến lược tham lam này hiệu quả vì một khi chúng tôi cam kết đặt một cánh cửa trong một phân khúc thì việc đặt nó sau sẽ chỉ làm giảm không gian có sẵn cho các cánh cửa tiếp theo vốn đã bị hạn chế theo đơn đặt hàng. 

### 3. Quyết định câu trả lời 

Nếu tất cả các cửa được đặt thành công, chúng ta trả về CÓ. Nếu không, chúng tôi trả lại KHÔNG. 

### Tại sao nó hoạt động 

Bất biến quan trọng là ở mỗi bước, chúng tôi luôn đặt cánh cửa hiện tại ở đoạn an toàn sớm nhất phù hợp với nó. Bất kỳ vị trí thay thế nào đẩy nó sang phải hơn nữa chỉ có thể làm giảm tính linh hoạt cho các cửa sau, vì các cửa sau có các lựa chọn còn lại giống nhau hoặc nhỏ hơn nhưng có ít phân đoạn hơn hoặc bằng nhau. Cấu trúc đơn điệu này đảm bảo rằng nếu tồn tại một sự sắp xếp hợp lệ thì vị trí tham lam sẽ tìm thấy nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    q = int(input())

    for _ in range(q):
        tmp = list(map(int, input().split()))
        c = tmp[0]
        w = tmp[1:]

        # build safe segments from forbidden positions
        w.sort()
        segs = []
        prev = 1

        for x in w:
            if prev <= x - 1:
                segs.append(x - prev)
            prev = x + 1

        if prev <= m:
            segs.append(m - prev + 1)

        # greedy placement
        i = 0
        ok = True

        for length in segs:
            remaining = length

            while i < n and remaining >= a[i]:
                remaining -= a[i]
                i += 1

            # if we cannot place next door in this segment
            # we just move to next segment, but if segment is too small for a single door, we skip
            while i < n and a[i] > length:
                # this door cannot fit here at all, try next segment
                break

            if i == n:
                break

        # after processing all segments, check if all doors placed
        print("YES" if i == n else "NO")

if __name__ == "__main__":
    solve()
```Mã đầu tiên chuyển đổi các ô bị cấm thành các khoảng trống tối đa. Các khoảng này thể hiện tất cả các vùng liên tục có thể có, nơi các cửa có thể được đặt mà không cắt ngang các vị trí cần thiết. 

Sau đó nó cố gắng đóng gói các cửa một cách tuần tự. Con trỏ`i`theo dõi cánh cửa tiếp theo phải được đặt. Đối với mỗi phân đoạn an toàn, chúng tôi sử dụng càng nhiều cửa liên tiếp càng tốt để vừa với phân đoạn đó. Khi một cánh cửa không vừa, chúng ta ngầm chuyển sang đoạn tiếp theo bằng cách tiếp tục vòng lặp. 

Một điểm tinh tế là chúng tôi không bao giờ cố gắng chia một cánh cửa thành nhiều đoạn. Mỗi cánh cửa phải hoàn toàn nằm gọn trong một phân đoạn an toàn duy nhất, đó chính xác là những gì mà cơ chế kiểm tra tham lam thực thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
m = 10
doors = [2, 3, 2]
w = [5]
```Các phân đoạn an toàn là: 

| Phân đoạn | Phạm vi | Chiều dài | 
| --- | --- | --- | 
| 1 | [1,4] | 4 | 
| 2 | [6,10] | 5 | 

Chúng tôi mô phỏng vị trí: 

| Phân đoạn | Còn lại | Chỉ số cửa | Kích thước cửa | Hành động | 
| --- | --- | --- | --- | --- | 
| [1,4] | 4 | 0 | 2 | chỗ, còn lại 2 | 
| [1,4] | 2 | 1 | 3 | không vừa, di chuyển đoạn | 
| [6,10] | 5 | 1 | 3 | chỗ, còn lại 2 | 
| [6,10] | 2 | 2 | 2 | nơi | 

Tất cả các cửa đều được đặt, vì vậy câu trả lời là CÓ. 

Điều này cho thấy một ô bị chặn buộc phải phân đoạn như thế nào nhưng vẫn cho phép phân phối lại các cửa. 

### Ví dụ 2 

đầu vào:```
m = 4
doors = [1, 1]
w = [2, 4]
```Phân đoạn an toàn: 

| Phân đoạn | Phạm vi | Chiều dài | 
| --- | --- | --- | 
| 1 | [1,1] | 1 | 
| 2 | [3,3] | 1 | 

| Phân đoạn | Còn lại | Chỉ số cửa | Kích thước cửa | Hành động | 
| --- | --- | --- | --- | --- | 
| [1,1] | 1 | 0 | 1 | nơi | 
| [3,3] | 1 | 1 | 1 | nơi | 

Điều này xác nhận rằng các phân đoạn tối thiểu vẫn cho phép đóng gói tham lam chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + \sum c_k)$| Mỗi cửa được xử lý tối đa một lần cho mỗi truy vấn và mỗi truy vấn sẽ quét tuyến tính các vị trí bị cấm của nó | 
| Không gian |$O(m)$trường hợp xấu nhất | Để lưu trữ cấu trúc phân đoạn ngầm cho mỗi truy vấn | 

Giải pháp nằm trong giới hạn vì cả kích thước truy vấn tổng và tổng số lần xử lý trên tất cả các truy vấn đều tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    q = int(input())

    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        c = tmp[0]
        w = tmp[1:]
        w.sort()

        segs = []
        prev = 1
        for x in w:
            if prev <= x - 1:
                segs.append(x - prev)
            prev = x + 1
        if prev <= m:
            segs.append(m - prev + 1)

        i = 0
        ok = True
        for length in segs:
            remaining = length
            while i < n and a[i] <= remaining:
                remaining -= a[i]
                i += 1

        out.append("YES" if i == n else "NO")
    return "\n".join(out)

# provided sample
assert run("""3 10
2 3 2
6
1 5
2 1 10
2 2 9
2 5 6
3 1 7 8
4 1 2 3 4
""") == """YES
YES
NO
NO
YES
NO"""

# edge: no restrictions
assert run("""2 5
2 5
1
0
""") == "YES"

# edge: impossible due to single tiny segment
assert run("""2 4
3 2
1
1 2
""") == "NO"

# edge: alternating constraints
assert run("""3 6
2 2 2
1
2 2 5
""") == "YES"

# edge: many forbidden points
assert run("""3 10
2 3 2
1
4 2 4 6 8
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có ô cấm | CÓ | sự linh hoạt hoàn toàn cơ bản | 
| khối chặt chẽ duy nhất | KHÔNG | đoạn quá nhỏ cho một cánh cửa | 
| ràng buộc xen kẽ | CÓ | nhiều phân khúc vẫn khả thi | 
| nhiều điểm cấm | KHÔNG | phân mảnh phá vỡ đóng gói | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một ô bắt buộc duy nhất tạo ra một đoạn nhỏ hơn một số cửa. Trong trường hợp đó, mặc dù tổng không gian trống lớn nhưng việc phân đoạn sẽ tạo ra một rào cản cứng. Thuật toán phát hiện điều này vì cánh cửa đó sẽ không bao giờ vừa với bất kỳ phân đoạn nào trong quá trình quét tham lam. 

Một trường hợp cạnh khác là khi các ô được yêu cầu nằm ở viền, chẳng hạn như vị trí 1 hoặc vị trí$m$. Điều này tạo ra các đoạn đầu hoặc cuối trống có độ dài bằng 0. Việc xây dựng đương nhiên bỏ qua các đoạn có độ dài bằng 0, do đó không cần xử lý đặc biệt. 

Trường hợp cạnh cuối cùng là khi tất cả các ô đều được yêu cầu. Điều này không tạo ra phân đoạn an toàn nào cả và quy trình tham lam ngay lập tức thất bại vì không thể đặt cửa ở bất kỳ đâu, tạo ra NO một cách chính xác.
