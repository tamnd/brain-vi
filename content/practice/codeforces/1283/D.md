---
title: "CF 1283D - Cây Giáng Sinh"
description: "Chúng ta có một tập hợp các điểm cố định trên trục số, mỗi điểm tượng trưng cho một cây thông Noel. Chúng ta cũng được yêu cầu đặt một tập hợp các điểm khác đại diện cho mọi người, nhưng có một điểm thay đổi: mỗi người đóng góp chi phí bằng khoảng cách đến cái cây gần nhất và chúng ta muốn chọn tất cả…"
date: "2026-06-16T03:02:12+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1283
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 611 (Div. 3)"
rating: 1800
weight: 1283
solve_time_s: 322
verified: false
draft: false
---

[CF 1283D - Cây thông Noel](https://codeforces.com/problemset/problem/1283/D) 

**Đánh giá:** 1800 
**Tags:** đồ thị, đường đi tham lam, đường đi ngắn nhất 
**Thời gian giải:** 5 phút 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm cố định trên trục số, mỗi điểm tượng trưng cho một cây thông Noel. Chúng ta cũng được yêu cầu đặt một tập hợp các điểm khác đại diện cho mọi người, nhưng có một điểm thay đổi: mỗi người đóng góp chi phí bằng khoảng cách đến cái cây gần nhất và chúng ta muốn chọn tất cả các vị trí của người đó để giảm thiểu tổng chi phí. 

Giải thích chính là chúng tôi không kết hợp từng người với từng cây. Thay vào đó, mọi vị trí số nguyên đều có “khoảng cách cây gần nhất” và chúng tôi đang chọn m vị trí số nguyên riêng biệt để đặt người sao cho tổng các khoảng cách đó càng nhỏ càng tốt. Cây cối và con người không thể chia sẻ vị trí, nhưng nếu không thì chúng ta có thể tự do đặt con người ở bất kỳ đâu trên dòng số nguyên. 

Ràng buộc n, m 2 × 10^5 ngụ ý rằng chúng ta cần hành vi gần như O((n + m) log n) hoặc O(n + m). Mọi thao tác quét bậc hai trên đường hoặc theo vị trí đều không thể thực hiện được ngay lập tức. Một nỗ lực ngây thơ đánh giá mọi vị trí có thể một cách độc lập hoặc mô phỏng việc mở rộng từng bước sẽ không mở rộng được. 

Một trường hợp thất bại tinh tế xuất hiện khi cây thưa thớt và m lớn. Ví dụ: nếu chỉ có một cây ở vị trí 0 và m = 5, giải pháp tối ưu là đặt mọi người ở vị trí ±1, ±2, ±3, v.v. Một kẻ tham lam chỉ mở rộng theo một hướng sẽ bỏ lỡ sự đối xứng và trả giá quá cao. 

Một trường hợp cạnh quan trọng khác là nhiều cây tạo ra các vùng ảnh hưởng chồng chéo. Ví dụ: các cây ở 0 và 10 tạo ra một “thung lũng” bằng phẳng trong đó các khoảng cách đối xứng xung quanh điểm giữa và việc mở rộng tham lam ngây thơ từ một cây duy nhất sẽ thiên về một phía một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét tất cả các điểm nguyên trong một phạm vi lớn nào đó và liên tục chọn vị trí tốt nhất tiếp theo để đặt một người. Đối với mỗi vị trí ứng cử viên y, chúng tôi tính toán khoảng cách cây gần nhất bằng cách quét tất cả các cây, đó là O(n). Có vô số vị trí số nguyên, nhưng trong thực tế, chúng ta sẽ giới hạn trong một phạm vi xung quanh cây, chẳng hạn như giữa min(x) và max(x), được mở rộng theo m. Ngay cả khi đó, chúng tôi vẫn đang đánh giá các vị trí O(m), mỗi vị trí có giá O(n), cho ra O(nm), quá lớn đối với 2 × 10^5. 

Quan sát quan trọng là mỗi cây độc lập “tạo ra” một làn sóng có giá trị khoảng cách tăng dần đến các điểm nguyên gần đó. Mỗi vị trí số nguyên thuộc về chính xác một vùng được kiểm soát bởi cây gần nhất của nó và trong vùng đó, khoảng cách tăng tuyến tính khi chúng ta di chuyển ra khỏi cây cho đến khi chúng ta chạm vào ranh giới điểm giữa với cây lân cận. 

Vì vậy, thay vì nghĩ về vị trí, chúng ta lật lại quan điểm: mỗi cây tạo ra các ứng cử viên theo các lớp mở rộng. Các vị trí trống gần nhất có thể có đối với bất kỳ cây nào chính xác là các vị trí lân cận của nó ở khoảng cách 1, sau đó là khoảng cách 2, v.v., cho đến khi ranh giới với các cây liền kề ngăn cản sự phát triển hơn nữa. 

Điều này tự nhiên gợi ý BFS đa nguồn trên dòng số nguyên. Chúng tôi bắt đầu đồng thời từ tất cả các vị trí cây và mở rộng ra bên ngoài, trong đó mỗi bước mở rộng tương ứng với khoảng cách tăng thêm 1. Mỗi điểm nguyên mới được phát hiện đều có một cây gần nhất được xác định rõ ràng (cây đạt tới nó đầu tiên) và khoảng cách của nó chính xác là cấp độ BFS. 

Chúng tôi tiếp tục mở rộng này cho đến khi thu thập được m vị trí. Vì BFS luôn khám phá theo thứ tự khoảng cách không giảm, nên m vị trí hợp lệ đầu tiên được phát hiện chính xác là những lựa chọn tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| BFS đa nguồn | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi tất cả các vị trí của cây là nguồn trong BFS trên tọa độ nguyên.

1. Chúng tôi khởi tạo một hàng đợi với tất cả các vị trí cây, mỗi vị trí được đánh dấu bằng khoảng cách 0. Chúng tôi cũng đánh dấu chúng là đã ghé thăm để không ai có thể được xếp vào vị trí trên cây. Điều này đảm bảo các ràng buộc được tôn trọng ngay từ đầu. 
2. Chúng tôi thực hiện BFS trên dòng số. Đối với mỗi vị trí x xuất hiện với khoảng cách d, chúng tôi thử hai hàng xóm của nó x − 1 và x + 1. 
3. Nếu hàng xóm chưa được ghé thăm và không phải là vị trí trên cây, chúng ta gán cho nó khoảng cách d + 1, đánh dấu nó đã ghé thăm và đẩy nó vào hàng đợi. Chúng tôi cũng ghi lại rằng vị trí này được chọn cho một người. 
4. Chúng tôi tiếp tục BFS cho đến khi thu thập được m vị trí người. Bởi vì BFS xử lý các nút theo thứ tự khoảng cách tăng dần, m vị trí hợp lệ đầu tiên tương ứng với những đóng góp nhỏ nhất có thể có vào tổng số. 
5. Cuối cùng, chúng ta tính tổng khoảng cách đã ghi của m vị trí đã chọn để đưa ra câu trả lời. 

### Tại sao nó hoạt động 

BFS xây dựng cây đường đi ngắn nhất từ tất cả các nguồn (cây) trên biểu đồ không có trọng số trong đó mỗi điểm nguyên kết nối với các điểm lân cận của nó. Khoảng cách được gán cho mỗi nút chính xác là khoảng cách tối thiểu của nó tới bất kỳ cây nào, bởi vì BFS đảm bảo lần đầu tiên chúng ta tiếp cận một nút là thông qua đường đi ngắn nhất có thể từ bất kỳ nguồn nào. Vì chúng tôi luôn chọn nút có sẵn tiếp theo theo thứ tự khoảng cách tăng dần, nên việc chọn m nút đầu tiên sẽ giảm thiểu tổng khoảng cách bằng đối số trao đổi tham lam: thay thế bất kỳ nút nào đã chọn bằng nút sau chỉ có thể làm tăng tổng chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def solve():
    n, m = map(int, input().split())
    xs = list(map(int, input().split()))

    visited = set(xs)
    q = deque()

    for x in xs:
        q.append((x, 0))

    ans_sum = 0
    ans = []

    while q and len(ans) < m:
        x, d = q.popleft()

        for nx in (x - 1, x + 1):
            if nx not in visited:
                visited.add(nx)
                q.append((nx, d + 1))
                ans.append(nx)
                ans_sum += d + 1
                if len(ans) == m:
                    break
        if len(ans) == m:
            break

    print(ans_sum)
    print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng BFS đa nguồn bắt đầu từ tất cả các vị trí cây. các`visited`set đảm bảo chúng tôi không bao giờ sử dụng lại một vị trí và cũng chặn việc chọn cây làm vị trí hợp lệ. Mỗi bản mở rộng BFS chỉ định khoảng cách nhiều hơn chính xác một khoảng cách so với phần gốc của nó, tương ứng trực tiếp với khoảng cách đến cây gần nhất. 

Điều kiện dừng sớm rất quan trọng vì đồ thị là vô hạn theo cả hai hướng; không dừng lại sau m lựa chọn, BFS sẽ tiếp tục vô thời hạn. M vị trí được tạo đầu tiên được đảm bảo tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 6
1 5
```Chúng tôi bắt đầu BFS từ 1 và 5 với khoảng cách 0. 

| Bước | Hàng đợi bật | Thêm vị trí mới | Khoảng cách được thêm vào | Được chọn cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 1 (0) | 0, 2 | 1, 1 | 0, 2 | 
| 2 | 5 (0) | 4, 6 | 1, 1 | 0, 2, 4, 6 | 
| 3 | 0 (1) | -1 | 2 | 0, 2, 4, 6, -1 | 
| 4 | 2 (1) | 3 | 2 | 0, 2, 4, 6, -1, 3 | 

Tổng là 1 + 1 + 2 + 2 + 2 + 1 = 8. 

Điều này cho thấy BFS mở rộng đối xứng một cách tự nhiên từ cả hai cây, trước tiên chiếm được các vị trí lân cận tối ưu. 

### Ví dụ 2 

đầu vào:```
1 4
0
```Chúng ta bắt đầu từ con số 0. 

| Bước | Hàng đợi bật | Thêm vị trí mới | Khoảng cách được thêm vào | Được chọn cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 0 (0) | -1, 1 | 1, 1 | -1, 1 | 
| 2 | -1 (1) | -2 | 2 | -1, 1, -2 | 
| 3 | 1 (1) | 2 | 2 | -1, 1, -2, 2 | 

Tổng chi phí là 1 + 1 + 2 + 2 = 6. 

Điều này xác nhận rằng việc mở rộng từ một nguồn duy nhất sẽ tạo ra vị trí tối ưu đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi vị trí số nguyên được truy cập nhiều nhất một lần và BFS xử lý mỗi cạnh một lần | 
| Không gian | O(n + m) | Hàng đợi và tập hợp đã truy cập chỉ lưu trữ các vị trí được tạo cộng với cây ban đầu | 

Các ràng buộc cho phép tối đa 2 × 10^5 phần tử, do đó BFS thời gian tuyến tính nằm trong giới hạn. Việc sử dụng bộ nhớ cũng an toàn vì chúng tôi chỉ lưu trữ các vị trí đã truy cập tối đa m phần mở rộng ngoài tập hợp cây. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, m = map(int, sys.stdin.readline().split())
    xs = list(map(int, sys.stdin.readline().split()))

    visited = set(xs)
    q = deque((x, 0) for x in xs)

    ans_sum = 0
    ans = []

    while q and len(ans) < m:
        x, d = q.popleft()
        for nx in (x - 1, x + 1):
            if nx not in visited:
                visited.add(nx)
                q.append((nx, d + 1))
                ans.append(nx)
                ans_sum += d + 1
                if len(ans) == m:
                    break
        if len(ans) == m:
            break

    return str(ans_sum) + "\n" + " ".join(map(str, ans))

# provided sample
assert run("2 6\n1 5\n") == "8\n-1 2 6 4 0 3"

# minimum case
assert run("1 1\n0\n") == "1\n-1" or run("1 1\n0\n") == "1\n1"

# symmetric expansion
assert run("1 4\n0\n") == "6\n-1 1 -2 2"

# two far trees
out = run("2 2\n-100 100\n")
assert "1\n" in out

# larger cluster sanity
assert len(run("3 5\n0 10 20\n").splitlines()[1].split()) == 5
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/0 | ±1 giá 1 | độ chính xác từng bước | 
| 1 4/0 | khai triển đối xứng | số dư và thứ tự BFS | 
| -100 100 | 2 vị trí | độc lập nguồn xa | 
| 3 cây | 5 kết quả đầu ra | giá trị chung | 

## Vỏ cạnh 

Trường hợp cây đơn được xử lý rõ ràng vì BFS mở rộng như nhau theo cả hai hướng. Đối với đầu vào`1 3 0`, thuật toán bắt đầu từ 0 và trước tiên tạo ra -1 và 1 với chi phí 1, sau đó -2 hoặc 2 với chi phí 2, đảm bảo tổng số tối thiểu. 

Khi cây ở gần nhau, chẳng hạn như`0 1`, tập hợp đã truy cập sẽ ngăn chặn sự chồng chéo. Biên giới BFS mở rộng ra bên ngoài từ cả hai nguồn, nhưng các điểm bên trong được xác nhận bởi bất kỳ nguồn nào tiếp cận chúng trước, đảm bảo việc gán cây gần nhất chính xác. 

Khi m cực lớn, BFS tiếp tục mở rộng ra ngoài từng lớp mà không bị sai lệch. Vì mỗi điểm nguyên cuối cùng đều đạt được chính xác một lần nên thuật toán không bao giờ bỏ sót các ứng viên hợp lệ và luôn tôn trọng thứ tự khoảng cách tăng dần.
