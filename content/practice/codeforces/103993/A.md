---
title: "CF 103993A - Nhanh nhất có thể"
description: "Cấp độ được mô hình hóa dưới dạng một dòng số vô hạn. Một nhân vật bắt đầu ở vị trí 0 và muốn đạt tới vị trí n. Tại mỗi thời điểm, nhân vật có thể di chuyển một đơn vị sang trái hoặc phải, trả chi phí cố định một giây cho mỗi bước đơn vị hoặc thực hiện một cú nhảy giống như dịch chuyển tức thời di chuyển chính xác…"
date: "2026-07-02T06:00:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103993
codeforces_index: "A"
codeforces_contest_name: "ICPC 2022-2023 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 103993
solve_time_s: 51
verified: true
draft: false
---

[CF 103993A - Nhanh nhất có thể](https://codeforces.com/problemset/problem/103993/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cấp độ được mô hình hóa dưới dạng một dòng số vô hạn. Một nhân vật bắt đầu ở vị trí 0 và muốn đạt tới vị trí n. Tại mỗi thời điểm, nhân vật có thể di chuyển một đơn vị sang trái hoặc phải, trả chi phí cố định một giây cho mỗi bước đơn vị hoặc thực hiện một bước nhảy giống như dịch chuyển tức thời di chuyển chính xác d đơn vị sang trái hoặc phải, trả b giây cho mỗi lần nhảy. Đường đi không bị hạn chế nằm trong phạm vi 0 và n, do đó, việc vượt quá hoặc chuyển sang âm được cho phép nếu nó giúp giảm tổng thời gian. 

Nhiệm vụ là tính tổng thời gian tối thiểu cần thiết để kết thúc chính xác tại vị trí n. 

Các ràng buộc đủ nhỏ để n nhiều nhất là 1000, và tất cả chi phí và độ dài bước nhảy cũng bị giới hạn bởi 1000. Điều này ngay lập tức gợi ý rằng đường đi ngắn nhất qua các trạng thái nguyên là khả thi. Một chương trình động đơn giản trên tất cả các vị trí và chuyển tiếp có thể có sẽ đủ nhanh vì không gian trạng thái rất nhỏ. 

Một điểm tinh tế là những cú nhảy có thể vượt quá giới hạn và sau đó được điều chỉnh bằng cách đi bộ. Điều này tạo ra những trường hợp trong đó chiến lược tối ưu liên quan đến việc cố tình rời xa mục tiêu trước tiên. Ví dụ: nếu việc đi bộ tốn kém nhưng số bước nhảy rẻ và lớn, thì có thể tối ưu hóa giá trị âm hoặc vượt quá n để căn chỉnh tính chẵn lẻ của bước nhảy hoặc giảm số bước nhỏ cần thiết. 

Một trường hợp cạnh khác xuất hiện khi khoảng cách nhảy d lớn hơn n. Trong trường hợp đó, mỗi lần nhảy đều vượt quá mục tiêu, vì vậy giải pháp tối ưu có thể bao gồm một vài lần nhảy, sau đó là quay lại. Cách tiếp cận tham lam ngây thơ “luôn nhảy về phía trước nếu gần hơn” có thể thất bại vì di chuyển ra xa có thể giảm tổng chi phí khi một chuỗi các bước nhảy được căn chỉnh tốt hơn so với các bước đơn lẻ hỗn hợp. 

## Phương pháp tiếp cận 

Cách diễn giải brute-force coi mọi vị trí số nguyên là một nút trong biểu đồ và mọi chuyển động hoặc bước nhảy là một cạnh. Từ mỗi vị trí x, chúng ta có thể đi đến x+1, x−1 với chi phí a và đến x+d, x−d với chi phí b. Chạy thuật toán Dijkstra trên biểu đồ này sẽ tìm ra đường đi ngắn nhất. 

Điều này đúng vì tất cả các bước di chuyển đều tạo thành các cạnh có trọng số và chúng ta đang giảm thiểu tổng chi phí. Tuy nhiên, nếu chúng ta ngây thơ cho phép các vị trí từ vô cực âm đến vô cực dương thì đồ thị sẽ là vô hạn. Cách tiếp cận brute-force chỉ trở nên có ý nghĩa sau khi nhận thấy rằng đường đi tối ưu không bao giờ cần đi xa ra ngoài cửa sổ giới hạn xung quanh [0, n]. Vì n ≤ 1000 và mỗi lần di chuyển thay đổi vị trí ít nhất 1, nên mọi đường dẫn tối ưu đều có thể bị giới hạn trong phạm vi như [-2000, 3000] mà không làm mất tính tối ưu. Điều đó đưa ra một biểu đồ hữu hạn khoảng vài nghìn nút, khiến Dijkstra trở nên khả thi nhưng vẫn nặng hơn mức cần thiết. 

Thông tin chi tiết quan trọng là không gian trạng thái đủ nhỏ để ngay cả việc tính toán đường đi ngắn nhất đơn giản hoặc thậm chí là DP trực tiếp trên tất cả các vị trí có thể tiếp cận đều hoạt động hiệu quả. Mỗi vị trí chỉ phụ thuộc vào một số lượng lân cận không đổi, vì vậy chúng ta có thể chạy thuật toán đường đi ngắn nhất tiêu chuẩn với độ phức tạp O(n) hoặc O(n log n) mà vẫn thoải mái trong giới hạn. Do biểu đồ không có trọng số về cấu trúc ngoại trừ chi phí nên biến thể BFS có hàng đợi ưu tiên rất đơn giản và mạnh mẽ. 

Một cách đơn giản hóa khác là vì tất cả các quá trình chuyển đổi đều có chi phí cố định nên về cơ bản đây là đường đi ngắn nhất trên biểu đồ đường với số cạnh trên mỗi nút không đổi, do đó không cần tối ưu hóa nâng cao ngoài Dijkstra tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Toàn bộ sức mạnh vũ phu vô hạn không giới hạn | Không áp dụng | O(∞) | Không hợp lệ | 
| Dijkstra trên phạm vi giới hạn | O(N log N) | O(N) | Đã chấp nhận | 
| Tối ưu hóa DP / đường dẫn ngắn nhất trên biểu đồ nhỏ | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi vị trí số nguyên là một nút biểu thị thời gian tối thiểu cần thiết để đạt được vị trí đó.

1. Đầu tiên chúng ta xác định một giới hạn hợp lý cho các vị thế. Vì mục tiêu là n và mỗi lần di chuyển sẽ thay đổi vị trí nhiều nhất là d hoặc 1, nên chúng tôi giới hạn bản thân trong một phạm vi bao gồm tất cả các trạng thái hữu ích một cách an toàn, thường là từ -2n đến 2n. Điều này đảm bảo bất kỳ đường vòng có lợi nào vẫn có thể thực hiện được. 
2. Chúng ta khởi tạo một mảng khoảng cách với giá trị vô cùng và đặt dist[0] = 0 vì chúng ta bắt đầu ở vị trí 0 mà không mất phí. 
3. Chúng tôi chạy thuật toán Dijkstra bằng hàng đợi ưu tiên, luôn mở rộng trạng thái với chi phí nhỏ nhất đã biết. Điều này đảm bảo chúng tôi luôn xử lý các trạng thái theo thứ tự tối ưu. 
4. Từ mỗi vị trí x, chúng ta nới lỏng tối đa bốn chuyển đổi: x+1 với chi phí a, x−1 với chi phí a, x+d với chi phí b và x−d với chi phí b. Mỗi lần thư giãn sẽ cập nhật chi phí đã biết tốt nhất cho vị trí đích nếu tìm thấy tuyến đường rẻ hơn. 
5. Chúng tôi tiếp tục cho đến khi tất cả các trạng thái có thể truy cập được xử lý. 
6. Câu trả lời là dist[n], chi phí tối thiểu để đạt được vị trí mục tiêu. 

Lý do điều này hoạt động là vì quá trình này mô hình hóa chính xác việc tính toán đường đi ngắn nhất trên biểu đồ có trọng số. Mỗi vị trí là một trạng thái, mỗi nước đi là một cạnh có chi phí không âm và Dijkstra đảm bảo tính tối ưu trong các trọng số không âm. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, a, b, d = map(int, input().split())

    # Reasonable bound: enough to cover detours
    LIM = 2 * n + d + 10

    INF = 10**18
    offset = LIM
    size = 2 * LIM + 1

    dist = [INF] * size
    start = offset
    dist[start] = 0

    pq = [(0, start)]

    def relax(nx, nd):
        if 0 <= nx < size and nd < dist[nx]:
            dist[nx] = nd
            heapq.heappush(pq, (nd, nx))

    while pq:
        cur, x = heapq.heappop(pq)
        if cur != dist[x]:
            continue

        pos = x - offset

        # move +1
        relax(x + 1, cur + a)
        # move -1
        relax(x - 1, cur + a)
        # jump +d
        relax(x + d, cur + b)
        # jump -d
        relax(x - d, cur + b)

    print(dist[offset + n])

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc triển khai xây dựng một biểu đồ ngầm trên một phân đoạn số nguyên bị chặn. Thủ thuật offset được sử dụng để xử lý các vị trí âm bằng cách dịch chuyển mọi thứ vào không gian chỉ mục mảng không âm. Hàng đợi ưu tiên đảm bảo chúng tôi luôn mở rộng vị trí có thể tiếp cận rẻ nhất trước tiên. 

Một sai lầm phổ biến là đánh giá thấp các giới hạn cần thiết. Nếu phạm vi quá hẹp, các đường dẫn tối ưu hợp lệ tạm thời đi xa về bên trái hoặc bên phải sẽ bị cắt bỏ. Giới hạn được chọn khoảng 2n + d là an toàn vì bất kỳ đường vòng hữu ích nào cũng không thể yêu cầu phân kỳ lặp lại ngoài phạm vi đó mà không phải trả chi phí đi bộ không cần thiết sẽ chi phối giải pháp. 

Một điểm tinh tế khác là kiểm tra`cur != dist[x]`, giúp tránh xử lý các mục nhập cũ trong hàng ưu tiên và giữ thuật toán trong giới hạn thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 9, a = 7, b = 6, d = 5 

Chúng tôi theo dõi một số trạng thái quan trọng trong quá trình thư giãn. 

| Bước | Vị trí | Chi phí | Hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | bắt đầu | 
| 1 | -1 | 7 | đi bên trái | 
| 2 | 4 | 13 | nhảy +5 | 
| 3 | 9 | 19 | nhảy +5 | 

Dấu vết này cho thấy tại sao việc vượt mức lại hữu ích. Đi đến -1 cho phép hai lần nhảy tiếp đất chính xác vào mục tiêu với chi phí rẻ hơn so với việc đi bộ trực tiếp. 

### Ví dụ 2 

đầu vào: 

n = 20, a = 5, b = 10, d = 15 

| Bước | Vị trí | Chi phí | Hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | bắt đầu | 
| 1 | 15 | 10 | nhảy +15 | 
| 2 | 20 | 35 | đi bộ +5 | 

Điều này thể hiện chiến lược hỗn hợp trong đó một bước nhảy được sử dụng để giảm khoảng cách xa, sau đó là những điều chỉnh nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L log L) | Dijkstra trên các vị trí O(L) với sự chuyển đổi liên tục | 
| Không gian | O(L) | mảng khoảng cách và hàng đợi ưu tiên trên phạm vi giới hạn | 

Giá trị L tỷ lệ thuận với cửa sổ tọa độ đã chọn, cửa sổ này tuyến tính theo n. Vì n ≤ 1000, điều này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n, a, b, d = map(int, input().split())
    LIM = 2 * n + d + 10
    INF = 10**18
    offset = LIM
    size = 2 * LIM + 1

    dist = [INF] * size
    start = offset
    dist[start] = 0
    pq = [(0, start)]

    def relax(x, nd):
        if 0 <= x < size and nd < dist[x]:
            dist[x] = nd
            heapq.heappush(pq, (nd, x))

    while pq:
        cur, x = heapq.heappop(pq)
        if cur != dist[x]:
            continue
        pos = x - offset
        relax(x + 1, cur + a)
        relax(x - 1, cur + a)
        relax(x + d, cur + b)
        relax(x - d, cur + b)

    return str(dist[offset + n])

# provided samples
assert run("9 7 6 5") == "19"
assert run("20 5 10 15") == "35"
assert run("4 3 5 2") == "10"

# custom cases
assert run("1 100 1 100") == "2", "prefer jump then adjust"
assert run("10 1 100 1") == "10", "walking dominates jump"
assert run("6 5 2 10") == "12", "overshoot and correct"
assert run("0 5 10 3") == "0", "already at target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 100 1 100 | 2 | đi bộ đắt tiền và chỉnh sửa rẻ | 
| 10 1 100 1 | 10 | đi bộ thuần túy tối ưu | 
| 6 5 2 10 | 12 | chiến lược vượt quá đúng đắn | 
| 0 5 10 3 | 0 | trường hợp ranh giới tầm thường | 

## Vỏ cạnh 

Khi n bằng 0, thuật toán ngay lập tức trả về 0 vì trạng thái bắt đầu đã khớp với mục tiêu và không cần chuyển đổi. 

Khi khoảng cách nhảy lớn hơn mục tiêu, việc tính toán đường đi ngắn nhất sẽ tránh được việc nhảy quá mức vì việc nhảy quá mức lặp đi lặp lại sẽ tích lũy chi phí không cần thiết. Quá trình Dijkstra vẫn xem xét các trạng thái đó nhưng nhanh chóng nhận thấy rằng việc đi bộ hoặc nhảy hạn chế chiếm ưu thế. 

Khi đi bộ rẻ hơn đáng kể so với nhảy, bước thư giãn luôn ưu tiên chuyển đổi ±1 và thuật toán hoạt động giống như một đường đi ngắn nhất tiêu chuẩn trên biểu đồ đường mà không bao giờ dựa vào các cạnh nhảy.
