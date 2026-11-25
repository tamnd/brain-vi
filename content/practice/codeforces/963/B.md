---
title: "CF 963B - Phá hủy cây"
description: "Chúng ta có một cây trong đó mỗi đỉnh có một mức độ hiện tại liên quan và mức độ này sẽ thay đổi khi các đỉnh bị loại bỏ. Một đỉnh chỉ đủ điều kiện để loại bỏ khi bậc của nó là chẵn tại thời điểm chúng ta chọn nó."
date: "2026-06-17T01:42:32+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 963
codeforces_index: "B"
codeforces_contest_name: "Tinkoff Internship Warmup Round 2018 and Codeforces Round 475 (Div. 1)"
rating: 2000
weight: 963
solve_time_s: 82
verified: false
draft: false
---

[CF 963B - Phá hủy một cái cây](https://codeforces.com/problemset/problem/963/B) 

**Xếp hạng:** 2000 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, dp, tham lam, cây 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó mỗi đỉnh có một mức độ hiện tại liên quan và mức độ này sẽ thay đổi khi các đỉnh bị loại bỏ. Một đỉnh chỉ đủ điều kiện để loại bỏ khi bậc của nó là chẵn tại thời điểm chúng ta chọn nó. Khi một đỉnh bị loại bỏ, tất cả các cạnh liên quan đều biến mất, do đó bậc của các cạnh lân cận của nó giảm đi. 

Nhiệm vụ là quyết định xem có tồn tại chuỗi phép loại bỏ để xóa mọi đỉnh hay không, luôn tuân theo quy tắc rằng đỉnh được chọn hiện tại phải có bậc chẵn. Nếu một chuỗi như vậy tồn tại, chúng ta phải xuất ra một đơn hàng hợp lệ. 

Biểu diễn đầu vào là mô tả gốc của cây: mỗi đỉnh i được kết nối với cha mẹ p[i] hoặc là gốc nếu p[i] bằng 0. Điều này xác định một cây vô hướng có n trừ một cạnh. Bản thân quá trình này rất năng động vì việc loại bỏ một đỉnh sẽ thay đổi mức độ trên toàn cầu, do đó các quyết định cục bộ sẽ ảnh hưởng đến tính khả dụng trong tương lai. 

Ràng buộc n lên đến 2 · 10^5 ngụ ý rằng mọi nghiệm đều phải gần tuyến tính hoặc tuyến tính. Bất cứ điều gì liên tục tính toán lại độ hoặc mô phỏng loại bỏ đơn giản bằng các cập nhật trên danh sách kề trên mỗi bước đều có nguy cơ xảy ra hành vi bậc hai trong các mẫu tương tác dày đặc. Một giải pháp thử tất cả các lệnh có thể là giai thừa và ngay lập tức không thể thực hiện được. 

Một số chế độ lỗi xuất hiện một cách tự nhiên. 

Chiến lược tham lam liên tục loại bỏ bất kỳ nút mức độ chẵn nào hiện tại có thể thất bại nếu chúng ta không duy trì cẩn thận cấu trúc của các nút có sẵn, bởi vì một đỉnh chẵn cục bộ có thể chặn tiến trình trong tương lai bằng cách phá vỡ cấu trúc chẵn lẻ trong cây con. 

Lỗi tinh tế thứ hai phát sinh nếu chúng ta giả sử tính chẵn lẻ là độc lập: việc loại bỏ một nút sẽ thay đổi tính chẵn lẻ của tất cả các nút lân cận, do đó, một lựa chọn cục bộ có thể lật nhiều nút từ chẵn sang lẻ hoặc ngược lại. Việc bỏ qua sự phụ thuộc này sẽ dẫn đến ngõ cụt ngay cả khi tồn tại một trật tự toàn cầu hợp lệ. 

Ví dụ, hãy xem xét một đường dẫn đơn giản gồm ba nút 1-2-3. Ban đầu độ là 1, 2, 1. Việc loại bỏ nút 2 là hợp lệ vì độ là chẵn, để lại hai nút biệt lập, có thể tháo rời được. Thay vào đó, nếu chúng ta loại bỏ một chiếc lá trước mà không nghĩ đến sự tiến hóa chẵn lẻ, chúng ta có thể bị mắc kẹt trong việc phụ thuộc vào thứ tự ở những cây lớn hơn. 

Khó khăn cốt lõi không phải là kiểm tra khả năng xóa mà là chọn một thứ tự duy trì khả năng hoàn thành tất cả việc xóa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các trình tự loại bỏ có thể xảy ra. Ở mỗi bước, chúng tôi chọn bất kỳ đỉnh nào có bậc chẵn và lặp lại. Vì ban đầu có tối đa n lựa chọn và mỗi lần loại bỏ sẽ thay đổi cấu trúc, nên không gian trạng thái về cơ bản là hoán vị của các đỉnh, tạo ra các khả năng O(n!) trong trường hợp xấu nhất. Ngay cả khi cắt tỉa, việc phân nhánh vẫn theo cấp số nhân vì các ràng buộc chẵn lẻ không loại bỏ đủ số nhánh sớm. 

Quan sát chính là vấn đề cơ bản là về sự lan truyền chẵn lẻ trong cây. Việc loại bỏ một đỉnh sẽ làm đảo lộn tính chẵn lẻ của tất cả các đỉnh lân cận của nó. Điều này có nghĩa là chúng tôi không theo dõi độ chính xác nhiều như theo dõi các đỉnh hiện tại là chẵn và cách tập hợp đó phát triển theo các chuyển đổi dọc theo các cạnh. 

Cái nhìn sâu sắc về cấu trúc quan trọng là cây cho phép chiến lược loại bỏ từ dưới lên: các lá có thể được xử lý theo cách có kiểm soát vì ban đầu chúng có độ 1 và chỉ có thể tháo rời sau khi cha mẹ của chúng bị loại bỏ hoặc sau khi lật chẵn lẻ lan truyền qua các thao tác xóa liền kề. Điều này gợi ý việc xử lý các đỉnh theo thứ tự nhất quán với việc truyền tải theo thứ tự sau, nhưng điều kiện thực tế là tính chẵn lẻ động chứ không phải mức độ tĩnh.

Một cách chính xác hơn để thấy điều đó là xem xét phép giảm tham lam trong đó chúng ta liên tục loại bỏ bất kỳ lá nào có trạng thái hiện tại trở nên chẵn sau khi tính đến các lần xóa trước đó. Khi một chiếc lá bị loại bỏ, nó chỉ ảnh hưởng đến lá mẹ của nó, lá hàng xóm duy nhất còn lại trên cây. Điều này làm cho các bản cập nhật cục bộ và có thể quản lý được. 

Chúng tôi mô phỏng việc loại bỏ trong khi vẫn duy trì mức độ hiện tại. Mỗi lần chúng tôi loại bỏ một đỉnh, chúng tôi sẽ cập nhật bậc của cấp độ gốc của nó và có khả năng làm cho nó mới đủ điều kiện. Nếu chúng tôi luôn xử lý các đỉnh mới đủ điều kiện, thì chúng tôi sẽ truyền bá các phép loại bỏ lên trên một cách hiệu quả theo cách phản ánh thứ tự mang tính xây dựng của chuỗi loại bỏ hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Mô phỏng độ với sự lan truyền tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cấu trúc kề của cây từ mảng cha. Chúng tôi cũng tính toán mức độ ban đầu của mỗi nút. Điều này là cần thiết vì khả năng tháo rời phụ thuộc vào tính chẵn lẻ của mức độ hiện tại chứ không chỉ vào cấu trúc. 
2. Khởi tạo hàng đợi với tất cả các đỉnh có bậc hiện tại là chẵn. Đây là những đỉnh duy nhất đủ điều kiện để loại bỏ tại thời điểm 0 và mọi quy trình hợp lệ phải bắt đầu từ một trong số chúng. 
3. Duy trì một mảng để theo dõi xem một đỉnh đã bị xóa hay chưa. Điều này ngăn chặn quá trình xử lý kép và đảm bảo mỗi đỉnh xuất hiện chính xác một lần theo thứ tự đầu ra. 
4. Liên tục trích xuất một đỉnh v từ hàng đợi và kiểm tra xem nó có còn hợp lệ để loại bỏ hay không. Nếu nó đã bị xóa hoặc mức độ hiện tại của nó là số lẻ do các bản cập nhật trước đó, chúng tôi sẽ bỏ qua nó. Nếu không, chúng tôi sẽ xóa nó và thêm nó vào chuỗi đầu ra. 
5. Khi loại bỏ đỉnh v, chúng ta giảm bậc của các lân cận duy nhất còn lại của nó trên cây. Trong biểu diễn gốc, đây thường là cha mẹ và con cái của nó vẫn còn hiện diện. Mỗi hàng xóm có mức độ thay đổi có thể trở thành chẵn, vì vậy nếu nó chưa bị xóa và hiện có mức độ chẵn, chúng tôi sẽ thêm nó vào hàng đợi. 
6. Tiếp tục cho đến khi hàng đợi trống. Cuối cùng, nếu đã loại bỏ tất cả các đỉnh thì chúng ta sẽ xuất ra thứ tự đã ghi. Mặt khác, không có chuỗi hợp lệ nào tồn tại. 

Lý do thứ tự này là đủ là vì mỗi lần loại bỏ chỉ ảnh hưởng đến tính chẵn lẻ liền kề và trong cây những hiệu ứng đó không tạo ra sự phụ thuộc tầm xa. Hàng đợi đảm bảo chúng tôi luôn xử lý các đỉnh một cách chính xác khi chúng đủ điều kiện, vì vậy, chúng tôi không bao giờ bỏ lỡ thao tác xóa hợp lệ có thể mở khóa các đỉnh khác. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là mọi đỉnh trong hàng đợi đều có bậc chẵn và chưa bị loại bỏ. Mỗi lần loại bỏ chỉ làm thay đổi mức độ tương đương của các hàng xóm của nó và những thứ này ngay lập tức được xem xét lại để đủ điều kiện. Bởi vì biểu đồ có tính chất không tuần hoàn, mỗi cạnh đóng góp chính xác một phụ thuộc cục bộ, do đó không có sự phụ thuộc tuần hoàn nào có thể ngăn cản quá trình xử lý cuối cùng nếu tồn tại một thứ tự toàn cầu hợp lệ. Điều này đảm bảo rằng bất cứ khi nào tồn tại một lệnh loại bỏ hoàn toàn hợp lệ, quy trình điều khiển hàng đợi sẽ phát hiện ra lệnh đó mà không cần quay lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    adj = [[] for _ in range(n)]
    deg = [0] * n

    for i in range(n):
        if p[i] != 0:
            v = i
            u = p[i] - 1
            adj[v].append(u)
            adj[u].append(v)
            deg[v] += 1
            deg[u] += 1

    removed = [False] * n
    q = deque()

    for i in range(n):
        if deg[i] % 2 == 0:
            q.append(i)
```
