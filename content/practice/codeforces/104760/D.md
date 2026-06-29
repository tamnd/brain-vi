---
title: "CF 104760D - \u0412\u0435\u0441\u0435\u043b\u044b\u0435 \u0444\u043e\u043d\u0430\u0440\u0438"
description: "Thành phố có thể được mô hình hóa như một đồ thị vô hướng trong đó mỗi cột đèn là một đỉnh và mỗi đường phố là một cạnh giữa hai đỉnh. Nhiệm vụ là quyết định xem chúng ta có thể gán một trong hai màu cho mỗi đỉnh để mỗi con phố kết nối các đỉnh có màu khác nhau hay không."
date: "2026-06-29T02:21:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104760
codeforces_index: "D"
codeforces_contest_name: "2023-2024 ICPC NERC (NEERC), Kyrgyzstan Qualification Contest"
rating: 0
weight: 104760
solve_time_s: 69
verified: false
draft: false
---

[CF 104760D - \u0412\u0435\u0441\u0435\u043b\u044b\u0435 \u0444\u043e\u043d\u0430\u0440\u0438](https://codeforces.com/problemset/problem/104760/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Thành phố có thể được mô hình hóa như một đồ thị vô hướng trong đó mỗi cột đèn là một đỉnh và mỗi đường phố là một cạnh giữa hai đỉnh. Nhiệm vụ là quyết định xem chúng ta có thể gán một trong hai màu cho mỗi đỉnh để mỗi con phố kết nối các đỉnh có màu khác nhau hay không. 

Đây chính xác là câu hỏi liệu biểu đồ có phải là lưỡng cực hay không, nhưng vấn đề được đặt ra trong bối cảnh vật lý hơn: chúng tôi đang sơn lại các bóng đèn bằng hai màu có sẵn và muốn các cột đèn liền kề (được kết nối bởi ít nhất một con phố) không bao giờ có cùng màu. 

Đầu vào có thể chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi có tối đa 400 đỉnh và tối đa N·(N−1)/2 cạnh, bao gồm các vòng tự lặp và các cạnh lặp lại. Một vòng tự lặp ngay lập tức kết nối một đỉnh với chính nó, điều này khiến cho việc tô màu hai màu hợp lệ là không thể vì đỉnh đó cần phải khác với chính nó. 

Các ràng buộc ngụ ý rằng ngay cả việc truyền tải đồ thị O(N2 + M) cho mỗi trường hợp thử nghiệm cũng có thể được chấp nhận. Kiểm tra lưỡng cực dựa trên BFS hoặc DFS là đủ. 

Một số trường hợp đặc biệt rất dễ bị bỏ sót nếu thực hiện bất cẩn. Đầu tiên, các vòng lặp tự phải được xử lý một cách rõ ràng. Ví dụ, đầu vào`1 1 1 1`có một đỉnh có cạnh liền kề với chính nó. Câu trả lời đúng là`NO`bởi vì đỉnh đó sẽ cần hai màu khác nhau cùng một lúc. 

Thứ hai, nhiều cạnh giữa cùng một cặp đỉnh không làm thay đổi độ chính xác nhưng có thể gây ra quá trình xử lý dư thừa nếu cạnh kề không được loại bỏ trùng lặp. Một cách tiếp cận đúng chỉ cần bỏ qua các bản sao. 

Thứ ba, các đồ thị bị ngắt kết nối phải được xử lý bằng cách chạy kiểm tra lưỡng cực từ mọi đỉnh chưa được thăm. Nếu không, một thành phần có thể được kiểm tra trong khi những thành phần khác vẫn chưa được xác minh. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ thử tất cả các phép gán 2 màu có thể có của N đỉnh. Mỗi đỉnh có hai lựa chọn, đưa ra 2ⁿ cấu hình. Đối với mỗi cấu hình, chúng tôi xác minh tất cả các cạnh trong O(M). Điều này dẫn đến O(2ⁿ · M), điều này trở nên không khả thi ngay cả khi N = 30. 

Quan sát quan trọng là các ràng buộc hoàn toàn mang tính cục bộ: mỗi cạnh chỉ thực thi sự bất bình đẳng giữa các điểm cuối của nó. Đây chính xác là hệ thống ràng buộc của việc kiểm tra hai bên. Thay vì tìm kiếm các bài tập trên toàn cầu, chúng ta có thể truyền bá các bài tập bắt buộc cục bộ. 

Chúng ta chọn một đỉnh không được tô màu, gán màu cho nó và truyền qua các cạnh: các đỉnh lân cận phải lấy màu đối diện. Nếu chúng ta gặp phải mâu thuẫn thì đồ thị đó không phải là đồ thị lưỡng cực. Điều này làm giảm vấn đề về việc truyền tải đồ thị. 

Sự hiện diện của các vòng lặp tự lập ngay lập tức phá vỡ tính lưỡng cực vì một đỉnh phải khác với chính nó dọc theo một cạnh. Vì vậy chúng ta có thể từ chối sớm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ · M) | O(N) | Quá chậm | 
| Kiểm tra lưỡng bên BFS/DFS | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho biểu đồ, bỏ qua các cạnh trùng lặp vì chúng không ảnh hưởng đến tính lưỡng cực. 
2. Trong khi đọc các cạnh, nếu bất kỳ cạnh nào nối một đỉnh với chính nó, ngay lập tức đánh dấu trường hợp kiểm thử là không hợp lệ. 
3. Duy trì một mảng màu được khởi tạo thành không màu cho tất cả các đỉnh. 
4. Đối với mọi đỉnh vẫn chưa được tô màu, hãy bắt đầu BFS. 
5. Gán màu đỉnh bắt đầu là 0 và đẩy nó vào hàng đợi. 
6. Trong BFS, với mỗi cạnh u → v, nếu v không được tô màu, gán cho nó màu đối lập với u và đẩy nó vào hàng đợi. 
7. Nếu v đã được tô màu và có cùng màu với u thì đồ thị không lưỡng cực nên ta dừng và xuất ra NO. 
8. Nếu BFS hoàn tất cho tất cả các thành phần mà không có xung đột, xuất ra CÓ. 

### Tại sao nó hoạt động 

Thuật toán thực thi rằng mọi cạnh đều áp đặt một ràng buộc bất bình đẳng nghiêm ngặt giữa các điểm cuối của nó. Việc truyền bá BFS đảm bảo rằng khi một đỉnh được gán một màu, tất cả các đỉnh có thể tiếp cận sẽ nhận được các màu được xác định duy nhất phù hợp với tính chẵn lẻ của độ dài đường dẫn. Nếu mâu thuẫn xảy ra, điều đó có nghĩa là tồn tại hai đường chẵn lẻ khác nhau giữa cùng một cặp đỉnh, điều này dẫn đến một chu trình lẻ. Một đồ thị là lưỡng cực khi và chỉ nếu nó không chứa chu trình lẻ, do đó việc phát hiện là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        parts = input().split()
        n = int(parts[0])
        m = int(parts[1])

        adj = [[] for _ in range(n + 1)]
        idx = 2
        ok = True

        for _ in range(m):
            u = int(parts[idx]); v = int(parts[idx + 1])
            idx += 2

            if u == v:
                ok = False
            else:
                adj[u].append(v)
                adj[v].append(u)

        if not ok:
            out.append("NO")
            continue

        color = [-1] * (n + 1)

        from collections import deque

        for i in range(1, n + 1):
            if color[i] != -1:
                continue

            color[i] = 0
            q = deque([i])

            while q and ok:
                u = q.popleft()
                for v in adj[u]:
                    if color[v] == -1:
```
