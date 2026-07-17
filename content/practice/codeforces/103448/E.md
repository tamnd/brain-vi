---
title: "CF 103448E - \u5723\u83ab\u5361\u9020\u9898\u7684\u4e03\u5929"
description: "Chúng ta được cung cấp một đồ thị vô hướng trong đó mỗi nút mang một giá trị nguyên không âm. Cấu trúc biểu đồ cho chúng ta biết các nút nào có thể tương tác và các giá trị phát triển thông qua một thao tác được áp dụng dọc theo bất kỳ đường dẫn đã chọn nào."
date: "2026-07-03T07:27:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103448
codeforces_index: "E"
codeforces_contest_name: "The 16-th Beihang University Collegiate Programming Contest (BCPC 2021) - Preliminary"
rating: 0
weight: 103448
solve_time_s: 42
verified: true
draft: false
---

[CF 103448E - \u5723\u83ab\u5361\u9020\u9898\u7684\u4e03\u5929](https://codeforces.com/problemset/problem/103448/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng trong đó mỗi nút mang một giá trị nguyên không âm. Cấu trúc biểu đồ cho chúng ta biết các nút nào có thể tương tác và các giá trị phát triển thông qua một thao tác được áp dụng dọc theo bất kỳ đường dẫn đã chọn nào. 

Một thao tác đơn lẻ sẽ chọn một đường dẫn trong biểu đồ, chẳng hạn như một chuỗi các nút trong đó các nút liên tiếp được kết nối bằng các cạnh. Nếu đường dẫn có độ dài t thì các nút được ghép đối xứng dọc theo đường dẫn: nút đầu tiên với nút cuối cùng, nút thứ hai với nút cuối cùng thứ hai, v.v. Mỗi nút trên đường dẫn thay thế giá trị của nó bằng bit AND của giá trị hiện tại và giá trị của nút ghép nối của nó. Tất cả các cập nhật diễn ra đồng thời, do đó thao tác dựa trên các giá trị ban đầu dọc theo đường dẫn chứ không phải các giá trị được cập nhật một phần. 

Chúng tôi có thể thực hiện thao tác này nhiều lần trên bất kỳ đường dẫn nào. Nhiệm vụ là giảm thiểu giá trị tối đa trên tất cả các nút sau nhiều thao tác tùy ý. 

Các ràng buộc cho phép lên tới năm trăm nghìn nút và cạnh, điều này ngay lập tức loại trừ mọi thứ bậc hai hoặc thậm chí gần bậc hai. Bất kỳ giải pháp nào cũng phải xử lý đồ thị theo thời gian tuyến tính hoặc gần tuyến tính một cách hiệu quả, điển hình là O(n + m). Điều này gợi ý rõ ràng rằng câu trả lời phụ thuộc vào các thành phần được kết nối và một số thuộc tính tổng hợp được tính toán trên mỗi thành phần. 

Một điểm tinh tế là các hoạt động không phải là cập nhật cạnh cục bộ mà là các tương tác cặp đối xứng trên toàn đường dẫn. Thật dễ dàng để giả định không chính xác rằng điều này giới hạn chúng ta chỉ trong các lần truyền lân cận, nhưng trên thực tế, một đường dẫn có thể kết nối các nút ở xa và các đường dẫn được chọn lặp lại có thể mô phỏng sự trộn rộng hơn. 

Một sự hiểu lầm ngây thơ là chỉ nghĩ rằng điểm cuối của những con đường đã chọn là quan trọng hoặc các giá trị đó lan truyền chậm. Ví dụ: trên chuỗi 1-2-3-4, người ta có thể nghĩ nút 1 chỉ có thể ảnh hưởng đến 2, nhưng đường dẫn 1-2-3-4 đồng thời kết hợp (1,4) và (2,3), cho phép thông tin chuyển qua toàn bộ chuỗi trong một thao tác. 

Trường hợp cạnh khóa là một cây hoặc chuỗi trong đó các thao tác lặp lại sẽ dần dần kết hợp các giá trị. Nếu chúng ta giả định không chính xác các giá trị chỉ hợp nhất cục bộ trên mỗi cạnh, chúng ta sẽ đánh giá thấp tốc độ lan truyền thông tin. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng các hoạt động. Chúng tôi sẽ liên tục chọn các đường dẫn tùy ý và áp dụng các cập nhật AND theo cặp đối xứng cho đến khi không thể cải thiện thêm. Mỗi thao tác quét một đường dẫn và có thể có nhiều đường dẫn và trình tự có thể có theo cấp số nhân. Ngay cả việc hạn chế bản thân trong các chiến lược đơn giản như áp dụng lặp đi lặp lại các thao tác trên các cạnh hoặc đường dẫn ngẫu nhiên cũng có thể dẫn đến hành vi có khả năng xảy ra O(nm) hoặc tệ hơn, vượt xa giới hạn. 

Cái nhìn sâu sắc về cấu trúc xuất phát từ việc hiểu thông tin nào thực sự được truyền bá. Hoạt động chỉ sử dụng bitwise AND, tức là giảm đơn điệu trên mỗi bit. Khi một bit trở thành 0 trong một nút, nó sẽ không bao giờ trả về. Điều này cho thấy trạng thái cuối cùng của một nút được xác định hoàn toàn bằng cách các giá trị ban đầu nào có thể bị buộc phải tương tác với nó thông qua các chuỗi thao tác. 

Bởi vì chúng ta có thể chọn các đường dẫn tùy ý bên trong một thành phần được kết nối, nên cuối cùng, bất kỳ nút nào cũng có thể được ghép nối, trực tiếp hoặc gián tiếp, với bất kỳ nút nào khác trong cùng một thành phần. Việc áp dụng nhiều lần các thao tác đường dẫn cho phép chúng ta hợp nhất thông tin trên toàn bộ thành phần. Điều này có nghĩa là mỗi thành phần được kết nối hoạt động giống như một hệ thống khép kín nơi tất cả các giá trị có thể được kết hợp. 

Bên trong một thành phần, giá trị mạnh nhất mà chúng ta có thể buộc mọi nút hướng tới là AND theo bit của tất cả các giá trị ban đầu trong thành phần đó. Bất kỳ bit nào bị thiếu trong một nút đều không thể tồn tại trong mục tiêu tối đa hóa toàn cục, bởi vì chúng ta luôn có thể thiết kế các hoạt động mà cuối cùng buộc bit đó được AND vào các nút khác và ngược lại, bất kỳ bit nào còn sót lại đều phải tồn tại trong tất cả các nút đóng góp.

Do đó, mỗi thành phần thu gọn về một giá trị đồng nhất bằng AND trên các nút của nó. Câu trả lời cuối cùng được xác định bởi mức tối đa của các giá trị AND khôn ngoan của thành phần này, vì các thành phần khác nhau phát triển độc lập và mục tiêu là giảm thiểu mức tối đa toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n + m) | Quá chậm | 
| Các thành phần được kết nối + Bitwise AND | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển lý luận thành một thủ tục cụ thể. 

1. Xác định tất cả các thành phần được kết nối của biểu đồ bằng DFS hoặc BFS. Mỗi thành phần đại diện cho một tập hợp các nút có thể được tạo để tương tác thông qua các chuỗi hoạt động hợp lệ. 
2. Đối với mỗi thành phần, tính toán AND theo bit của tất cả các giá trị nút bên trong nó. Điều này được thực hiện bằng cách khởi tạo một giá trị đang chạy cho tất cả các tập hợp bit (hoặc giá trị của nút đầu tiên) và AND-ing mọi giá trị của nút khác trong thành phần. 
3. Theo dõi mức tối đa của các kết quả VÀ theo thành phần này trên tất cả các thành phần. 
4. Xuất ra giá trị lớn nhất này. 

Lý do chúng tôi lấy mức tối đa cho các thành phần thay vì tổng hoặc thứ gì đó mang tính toàn cầu hơn là vì các thành phần phát triển độc lập. Không có hoạt động nào có thể kết nối hai thành phần bị ngắt kết nối, vì vậy trạng thái cuối cùng của chúng không ảnh hưởng lẫn nhau. 

Tại sao nó hoạt động 

Bên trong một thành phần được kết nối, các hoạt động đường dẫn lặp đi lặp lại cho phép chúng ta truyền bá thông tin giữa bất kỳ cặp nút nào. Mỗi thao tác có thể ghép hai nút thông qua tính đối xứng trên một đường dẫn đã chọn và bằng cách xâu chuỗi các thao tác đó, mọi nút cuối cùng có thể bị ảnh hưởng bởi mọi nút khác. Bởi vì thao tác duy nhất là AND, giá trị tại bất kỳ nút nào chỉ có thể giảm theo bit và bất kỳ bit nào còn tồn tại trong tất cả các nút đều có thể được bảo toàn thông qua việc ghép nối cẩn thận. Giá trị ổn định duy nhất mà mọi nút có thể buộc phải đồng ý là AND của tất cả các giá trị ban đầu trong thành phần, vì bất kỳ bit nào vắng mặt trong bất kỳ nút nào đều có thể bị loại bỏ bằng cách ghép nối các đường dẫn bao gồm nút đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    w = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    visited = [False] * n
    ans = 0

    for i in range(n):
        if visited[i]:
            continue

        stack = [i]
        visited[i] = True
        comp_and = (1 << 30) - 1  # enough for w <= 1e9

        while stack:
            u = stack.pop()
            comp_and &= w[u]
            for v in g[u]:
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)

        ans = max(ans, comp_and)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng biểu diễn danh sách kề của biểu đồ. Sau đó, nó chạy một DFS lặp để tránh giới hạn đệ quy trên các biểu đồ lớn. Mỗi khi một thành phần mới được phát hiện, nó sẽ khởi tạo một mặt nạ bit đầy đủ và giao nó với tất cả các giá trị nút gặp trong thành phần đó. 

Đáp án cuối cùng tích lũy kết quả thành phần lớn nhất. Mặt nạ bit`(1 << 30) - 1`bao phủ một cách an toàn tất cả các giá trị có thể có vì đầu vào được giới hạn bởi 10^9. 

## Ví dụ đã hoạt động 

Hãy xem xét một đồ thị nhỏ có hai thành phần. Giả sử thành phần một có giá trị 5 (101) và 7 (111) và thành phần hai có một nút duy nhất có giá trị 2 (010). Thuật toán xử lý từng thành phần một cách độc lập. 

Đối với thành phần đầu tiên, AND trở thành 101 & 111 = 101, bằng 5. Đối với thành phần thứ hai, nó chỉ đơn giản là 2. Câu trả lời cuối cùng là max(5, 2) = 5. 

| Thành phần | Các nút đã truy cập | Chạy VÀ | Kết quả thành phần | 
| --- | --- | --- | --- | 
| 1 | 5, 7 | 111 → 101 | 5 | 
| 2 | 2 | 010 | 2 | 

Dấu vết này cho thấy khả năng kết nối chỉ quan trọng trong việc nhóm; sau khi được nhóm lại, các giá trị sẽ thu gọn hoàn toàn thành một ràng buộc theo bit. 

Hiện nay
