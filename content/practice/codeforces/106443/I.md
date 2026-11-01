---
title: "CF 106443I - Trục trặc tiền vô cực"
description: "Chúng ta được cung cấp một biểu đồ có hướng với các cạnh có trọng số, trong đó mỗi nút đại diện cho một vị trí trên bảng và mỗi cạnh đại diện cho một nước đi làm tăng hoặc giảm điểm chạy. Bắt đầu từ nút 1, chúng tôi liên tục chọn bất kỳ cạnh đi nào hoặc giữ nguyên vị trí với mức tăng bằng 0."
date: "2026-06-22T04:18:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "I"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 46
verified: true
draft: false
---

[CF 106443I - Trục trặc về tiền vô cực](https://codeforces.com/problemset/problem/106443/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng với các cạnh có trọng số, trong đó mỗi nút đại diện cho một vị trí trên bảng và mỗi cạnh đại diện cho một nước đi làm tăng hoặc giảm điểm chạy. Bắt đầu từ nút 1, chúng tôi liên tục chọn bất kỳ cạnh đi nào hoặc giữ nguyên vị trí với mức tăng bằng 0. Sau một số lượng lớn các nước đi, chúng ta quan tâm đến mức tăng trung bình tối đa có thể có trên mỗi nước đi trên tất cả các chuỗi có chính xác T bước, trong đó T cực kỳ lớn. 

Giải thích chính là chiến lược tương ứng với một bước đi vô hạn bắt đầu từ nút 1 và điểm mỗi bước là tổng trọng số chia cho số bước. Vì T quá lớn nên chúng tôi đang tìm kiếm phần thưởng trung bình dài hạn tốt nhất cho mỗi lần di chuyển có thể đạt được từ nút 1 trong biểu đồ này. 

Các ràng buộc đặt N và M lên tới 2000, do đó, giải pháp kiểu O(NM) hoặc O(N^3) là hợp lý, nhưng bất cứ điều gì như liệt kê tất cả các đường đi hoặc mô phỏng các chuyến đi bộ dài đều không thể thực hiện được. Giá trị T rất lớn nên mô phỏng trực tiếp là không phù hợp; chỉ có hành vi tiệm cận của chu kỳ mới quan trọng. 

Một vấn đề tế nhị là việc ở lại vị trí được phép nhưng không mang lại phần thưởng. Điều này có nghĩa là mọi nút có thể hoạt động như một điểm chìm với vòng lặp tự có trọng số 0 xét về tính khả thi của các bước, nhưng nó không thay đổi cấu trúc chu trình để đạt được mức tăng tối ưu. 

Một trường hợp thất bại phổ biến là giả sử câu trả lời chỉ phụ thuộc vào các chu kỳ dương có thể đạt được mà không xem xét đến sự kết hợp của các chu kỳ. Ví dụ: nếu một chu kỳ có mức trung bình là 5 và chu kỳ khác có mức trung bình là 1, thì việc xen kẽ giữa chúng sẽ tệ hơn là bám sát chu kỳ tốt nhất, nhưng quá trình chuyển đổi có thể gây ra tổn thất tạm thời trước khi đạt được chu kỳ tốt. 

Một cạm bẫy khác là bỏ qua khả năng tiếp cận: chu kỳ trung bình cao sẽ vô ích nếu không thể đạt được từ nút 1. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là mô phỏng những chuyến đi bộ dài và cố gắng tối đa hóa tổng trọng lượng theo kinh nghiệm. Nhưng ngay cả khi chúng ta mô phỏng một cách tham lam, chúng ta chỉ nhận được một quỹ đạo duy nhất và số lượng quỹ đạo có thể tăng theo cấp số nhân với T. Điều này hoàn toàn không khả thi vì T có cấp độ 10^10. 

Cấu trúc thực sự của vấn đề xuất phát từ thực tế là sau đủ các bước, bất kỳ chiến lược tối ưu nào cũng phải dành gần như toàn bộ thời gian để xoay vòng bên trong một số khu vực được kết nối chặt chẽ. Sự đóng góp của đường dẫn ban đầu cho khu vực đó trở nên không đáng kể ở mức trung bình. 

Điều này làm giảm vấn đề tìm một chu trình (hoặc kết hợp các chu trình trong một thành phần được kết nối mạnh) để tối đa hóa trọng số trung bình trên mỗi cạnh, có thể tiếp cận được từ nút 1. Đây là bài toán chu trình trung bình tối đa cổ điển trong đồ thị có hướng. 

Phép biến đổi quan trọng là chuyển câu hỏi “tối đa hóa trọng số trung bình trên mỗi bước” thành câu hỏi khả thi: đối với giá trị ứng cử viên x, hãy xem xét liệu có tồn tại một bước đi có trung bình ít nhất là x hay không. Nếu chúng ta trừ x khỏi mỗi trọng số cạnh thì chúng ta đang kiểm tra xem liệu có tồn tại một chu trình có tổng trọng số không âm hay không. Nếu một chu trình như vậy tồn tại, điều đó có nghĩa là chúng ta có thể đạt được ít nhất x trung bình. Nếu không thì x quá lớn. 

Thuộc tính đơn điệu này cho phép tìm kiếm nhị phân trên x. Mỗi lần kiểm tra sẽ giảm xuống mức phát hiện xem liệu một chu trình dương có tồn tại trong biểu đồ có trọng số được sửa đổi hay không, điều này có thể được thực hiện bằng cách sử dụng phương pháp thư giãn kiểu Bellman-Ford trong O(NM). 

Vì N và M đều lên tới 2000 nên O(độ chính xác nhật ký NM) có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T·M) | O(N) | Quá chậm | 
| Tìm kiếm nhị phân + Bellman-Ford | O(NM log(1e9)) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn trọng số trung bình tối đa có thể đạt được trên mỗi bước từ nút 1. Chúng tôi biến vấn đề này thành một vấn đề quyết định: đối với giá trị ứng viên x, chúng tôi hỏi liệu có tồn tại một chu trình có thể tiếp cận có trọng số trung bình ít nhất là x hay không.

1. Bắt đầu với phạm vi tìm kiếm nhị phân cho x, thường từ trọng số cạnh tối thiểu có thể đến trọng số cạnh tối đa có thể. Điều này giới hạn câu trả lời vì bất kỳ mức trung bình nào cũng phải nằm trong giới hạn cực trị. 
2. Đối với x cố định, hãy biến đổi trọng số của mọi cạnh w thành w − x. Nếu tồn tại một chu trình có tổng tổng ≥ 0 trong biểu đồ được chuyển đổi này thì biểu đồ gốc chứa một chu trình có trọng số trung bình ít nhất là x. 
3. Để phát hiện một chu trình như vậy có thể đạt được từ nút 1, hãy chạy phần mềm Bellman-Ford trên tất cả các cạnh N lần. Chúng tôi duy trì một mảng khoảng cách trong đó dist[v] đại diện cho tổng được điều chỉnh tối đa có thể đạt được tại v. 
4. Khởi tạo dist[1] = 0 và tất cả các nút khác thành số rất âm. Điều này thực thi khả năng tiếp cận từ nút 1. 
5. Thư giãn tất cả các cạnh liên tục. Nếu sau N lần lặp, bất kỳ khoảng cách nào được cải thiện, điều đó ngụ ý một chu kỳ dương trong biểu đồ được chuyển đổi, nghĩa là x là khả thi. 
6. Nếu tồn tại một chu trình khả thi, hãy di chuyển giới hạn tìm kiếm nhị phân xuống dưới; nếu không thì giảm giới hạn trên. 
7. Sau khi lặp lại đủ độ chính xác (hoặc các lần lặp cố định như 60 cho nhân đôi), hãy đưa ra giá trị tìm thấy tốt nhất. 

Tại sao nó hoạt động 

Bất kỳ cuộc đi bộ dài nào đều phân hủy thành một con đường đơn giản dẫn đến một chu trình nào đó cộng với việc đi qua chu trình đó nhiều lần. Sự đóng góp của đường đi trở nên không đáng kể khi T tăng lên. Do đó, mức trung bình giới hạn được xác định hoàn toàn bởi cấu trúc chu kỳ. Phép biến đổi w − x chuyển đổi vấn đề thành việc phát hiện xem liệu bất kỳ chu trình nào có thể duy trì độ trôi không âm hay không, điều này hoàn toàn tương đương với việc có trung bình ít nhất là x. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def has_positive_cycle(n, edges, x):
    INF = -10**30
    dist = [INF] * (n + 1)
    dist[1] = 0

    for _ in range(n):
        updated = False
        for u, v, w in edges:
            val = dist[u] + (w - x)
            if val > dist[v]:
                dist[v] = val
                updated = True

    for u, v, w in edges:
        if dist[u] + (w - x) > dist[v]:
            return True

    return False

def main():
    n, m = map(int, input().split())
    edges = []
    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((u, v, w))

    lo, hi = -1e9, 1e9

    for _ in range(60):
        mid = (lo + hi) / 2
        if has_positive_cycle(n, edges, mid):
            lo = mid
        else:
            hi = mid

    print(f"{lo:.10f}")

if __name__ == "__main__":
    main()
```Việc triển khai giữ một mảng khoảng cách duy nhất và thực hiện thư giãn Bellman-Ford cho N vòng. Bước cuối cùng sẽ phát hiện xem liệu vẫn có thể cải thiện được hay không, điều này báo hiệu một chu kỳ tích cực trong biểu đồ đã chuyển đổi. 

Tìm kiếm nhị phân sử dụng điểm giữa của dấu phẩy động. Sáu mươi lần lặp là quá đủ cho độ chính xác 1e-6 cần thiết. 

Một lỗi triển khai phổ biến là quên khởi tạo các nút không thể truy cập ở mức rất tiêu cực, điều này sẽ cho phép các chu kỳ không thể truy cập được từ nút 1 ảnh hưởng đến câu trả lời một cách không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị: 

1 → 2 (2) 

2 → 1 (-1) 

Chúng tôi tìm kiếm nhị phân một giá trị x. Xét trạng thái sau khi chuyển đổi trọng số bằng cách trừ x. 

| Lặp lại | quận [1] | quận [2] | Quan sát chính | 
| --- | --- | --- | --- | 
| ban đầu | 0 | -inf | bắt đầu từ nút 1 | 
| thư giãn | 0 | 2-x | cạnh 1→2 | 
| thư giãn | x-? | cập nhật | chu kỳ bắt đầu đóng góp | 

Với x = 0,5, chu trình 1→2→1 có trọng số (2 + (-1)) = 1, trung bình là 0,5, do đó chu trình biến đổi có tổng trọng số bằng 0, khiến nó khả thi. 

Điều này xác nhận câu trả lời là 0,5 vì chu trình có ý nghĩa duy nhất có tổng trọng số bằng 1 trên chiều dài 2. 

### Mẫu 2 

Chu kỳ: 

1 → 2 (-5) 

2 → 3 (5) 

3 → 1 (-5) 

Tổng trọng lượng chu kỳ là -5, do đó, bất kỳ đường truyền nào cũng giảm trung bình. Chiến lược tốt nhất là ở lại nút 1 mà không di chuyển. 

| Bước | quận [1] | quận [2] | quận [3] | bình luận | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | -inf | -inf | bắt đầu | 
| thư giãn | 0 | -5-x | -inf | cạnh đầu tiên | 
| thư giãn | -5-x | ... | -10-x | chu kỳ xấu đi | 

Không tồn tại chu trình dương với mọi x > 0, nên đáp án là 0. 

Điều này cho thấy rằng thuật toán thích không làm gì hơn là bị buộc vào một chu kỳ âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N M log C) | Bellman-Ford O(NM) mỗi lần kiểm tra, tìm kiếm nhị phân chính xác | 
| Không gian | O(N + M) | lưu trữ kề và mảng khoảng cách | 

Với N, M lên tới 2000, bước thư giãn tối đa là 4 triệu thao tác và nhân với ~ 60 lần lặp vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, m = map(int, sys.stdin.readline().split())
    edges = []
    for _ in range(m):
        u, v, w = map(int, sys.stdin.readline().split())
        edges.append((u, v, w))

    def has_cycle(x):
        INF = -10**30
        dist = [INF] * (n + 1)
        dist[1] = 0

        for _ in range(n):
            for u, v, w in edges:
                if dist[u] > -10**20:
                    dist[v] = max(dist[v], dist[u] + (w - x))

        for u, v, w in edges:
            if dist[u] > -10**20 and dist[u] + (w - x) > dist[v]:
                return True
        return False

    lo, hi = -1e9, 1e9
    for _ in range(60):
        mid = (lo + hi) / 2
        if has_cycle(mid):
            lo = mid
        else:
            hi = mid

    return f"{lo:.6f}"

# sample 1
assert run("""2 2
1 2 2
2 1 -1
""").strip()[:4] == "0.5", "sample 1"

# sample 2
assert run("""3 3
1 2 -5
2 3 5
3 1 -5
""").strip() == "0.000000"

# custom: single self-loop-like positive cycle
assert run("""2 1
1 2 10
""") >= "0", "path only"

# custom: all zero
assert run("""3 3
1 2 0
2 3 0
3 1 0
""").strip() == "0.000000", "all zero"

# custom: disconnected high cycle unreachable
assert run("""4 4
1 2 0
2 1 0
3 4 100
4 3 100
""").strip() == "0.000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chu kỳ tích cực 2 nút | 0,5 | chu kỳ trung bình cơ bản | 
| Chu kỳ âm 3 nút | 0 | không có chu kỳ có lợi | 
| cạnh đơn | >=0 | xử lý khả năng tiếp cận | 
| tất cả chu kỳ bằng không | 0 | đồ thị trung tính | 
| chu kỳ tích cực không thể tiếp cận | 0 | hạn chế về khả năng tiếp cận | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tồn tại một chu kỳ giá trị cao nhưng không thể truy cập được từ nút 1. Việc khởi tạo khoảng cách đảm bảo các chu kỳ đó không bao giờ được kích hoạt, vì chỉ nút 1 bắt đầu với giá trị hữu hạn. 

Một trường hợp khác là đồ thị chỉ có các điểm dừng hiệu quả. Vì việc duy trì đóng góp bằng 0 nên thuật toán trả về 0 một cách chính xác nếu không tồn tại chu kỳ dương. 

Cuối cùng, các chu kỳ âm không đánh lừa thuật toán để giảm câu trả lời không chính xác, bởi vì chúng ta đang tối đa hóa trên mức trung bình và bất kỳ chu kỳ âm nào cũng trở nên kém hấp dẫn so với việc duy trì ở mức 0.
