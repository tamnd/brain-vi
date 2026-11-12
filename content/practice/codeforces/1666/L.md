---
title: "CF 1666L - Mê cung"
description: "Chúng ta được cho một đồ thị có hướng biểu diễn một mê cung gồm các hành lang và lối đi một chiều. Một du khách xuất phát từ một điểm xuất phát cố định $s$. Chúng ta được phép chọn bất kỳ hội trường $t$ nào khác làm điểm tập trung."
date: "2026-06-10T02:21:17+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1666
codeforces_index: "L"
codeforces_contest_name: "2021-2022 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1800
weight: 1666
solve_time_s: 139
verified: false
draft: false
---

[CF 1666L - Mê cung](https://codeforces.com/problemset/problem/1666/L) 

**Đánh giá:** 1800 
**Thẻ:** dfs và các biểu đồ tương tự 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng biểu diễn một mê cung gồm các hành lang và lối đi một chiều. Một lữ khách xuất phát từ một sảnh xuất phát cố định$s$. Chúng ta được phép chọn bất kỳ hội trường nào khác$t$như một điểm gặp gỡ. Mục tiêu là xây dựng hai đường đi đơn giản khác nhau từ$s$đến cùng một lựa chọn$t$, với điều kiện tách biệt chặt chẽ: hai tuyến không được phép dùng chung bất kỳ sảnh trung gian nào. Các đỉnh duy nhất mà chúng có thể có chung là điểm bắt đầu$s$và điểm gặp gỡ cuối cùng$t$. 

Nói cách khác, chúng ta đang tìm kiếm một đỉnh$t$sao cho tồn tại hai đường đi có hướng rời nhau theo đỉnh bên trong từ$s$ĐẾN$t$. Chúng ta cũng cần xuất ra rõ ràng cả hai đường dẫn. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các đỉnh và các cạnh, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê tất cả các đường đi hoặc tính toán lại nhiều lần khả năng tiếp cận giữa các cặp đỉnh. Bất cứ điều gì vượt quá độ phức tạp tuyến tính hoặc gần tuyến tính trong$n + m$sẽ quá chậm, vì vậy giải pháp về cơ bản phải là duyệt một đồ thị duy nhất và ghi chép cẩn thận. 

Một nỗ lực ngây thơ sẽ là chọn một ứng cử viên$t$và chạy hai đường dẫn ngắn nhất hoặc các cấu trúc DFS trong khi cấm các nút đã được sử dụng. Điều này không thành công vì việc lựa chọn đường dẫn đầu tiên có thể chặn đường dẫn thứ hai ngay cả khi tồn tại một cặp hợp lệ. Ví dụ: nếu cả hai đường dẫn hợp lệ đều phân kỳ sớm và nối lại muộn, thì việc tham lam đi theo một đường sẽ phá hủy đường kia. 

Một trường hợp lỗi nhỏ khác xảy ra khi có nhiều đường dẫn từng phần chồng chéo nhau. Một DFS chỉ ghi lại cha mẹ có thể tạo ra một đường dẫn hợp lệ nhưng không đảm bảo rằng một đường dẫn khác tồn tại đến cùng một điểm cuối. 

Khó khăn chính là chúng tôi không chỉ tìm kiếm khả năng tiếp cận mà còn tìm kiếm _hai cách độc lập_ để đến cùng một đỉnh mà không chia sẻ cấu trúc bên trong. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét mọi mục tiêu có thể$t$, và với mỗi người hãy cố gắng tìm hai đường đi khác nhau từ$s$ĐẾN$t$. Điều này có thể được mô hình hóa bằng cách sử dụng flow: chia mọi đỉnh (ngoại trừ$s$Và$t$) vào các nút vào ra và chạy kiểm tra luồng tối đa với dung lượng đơn vị. Nếu luồng tối đa ít nhất là 2 thì đỉnh sẽ hoạt động. 

Cách tiếp cận này đúng, nhưng cực kỳ tốn kém. Chạy thuật toán luồng cho từng khả năng$t$sẽ dẫn đến đại khái$n$tính toán dòng chảy, mỗi khả năng$O(m \sqrt n)$hoặc tệ hơn là vượt xa giới hạn. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần phải kiểm tra mọi$t$. Thay vào đó, chúng ta có thể xây dựng một bản khám phá _single root từ$s$_ và phát hiện điểm đầu tiên nơi hai “nhánh” khả năng tiếp cận độc lập va chạm nhau. 

Hãy nghĩ đến việc khám phá biểu đồ từ$s$. Mỗi hàng xóm đi của$s$bắt đầu một cuộc khám phá "màu sắc" riêng biệt. Nếu một đỉnh được đạt tới bởi hai màu khác nhau thì có hai cách khác nhau để đạt tới đỉnh đó: một cách thông qua mỗi cây con được tô màu. Vì mỗi bản mở rộng màu là một DFS không bao giờ hợp nhất các đường dẫn bên trong nên hai tuyến vẫn tách rời nhau cho đến điểm xung đột đầu tiên. 

Điều này chuyển vấn đề thành việc phát hiện nút đầu tiên có thể truy cập được từ ít nhất hai cây DFS riêng biệt có gốc tại các nút lân cận của$s$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra đường dẫn rời rạc đa luồng / mỗi mục tiêu |$O(n \cdot \text{flow})$|$O(n)$| Quá chậm | 
| Màu DFS đa nguồn từ các nước láng giềng của$s$|$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đối xử với mỗi người hàng xóm của$s$như một nguồn truyền bá riêng biệt và theo dõi “nhánh của$s$”đầu tiên đến mọi nút. 

1. Bắt đầu bằng cách gán một mã định danh riêng biệt cho mỗi hàng xóm đi của$s$. Những mã định danh này đại diện cho các cây thăm dò độc lập. 
2. Đối với mọi người hàng xóm$v$của$s$, chạy DFS bắt đầu từ$v$, đánh dấu tất cả các nút có thể truy cập bằng mã định danh của$v$và ghi lại các con trỏ cha bên trong cây DFS đó. 

DFS không bao giờ truy cập lại các nút đã được chỉ định, vì khi một nút được nhánh xác nhận quyền sở hữu, chúng tôi sẽ không ghi đè lên nút đó. 
3. Trong DFS, bất cứ khi nào chúng tôi cố gắng truy cập một nút đã được gán cho nhánh _other_, chúng tôi sẽ ngay lập tức dừng và sửa nút này làm điểm gặp gỡ của chúng tôi$t$. 

Thời điểm này rất quan trọng: nút$t$hiện đã được truy cập từ hai cây con riêng biệt có nguồn gốc từ các hàng xóm khác nhau của$s$. 
4. Khi một nút như vậy$t$được tìm thấy, xây dựng lại hai đường dẫn: 

- Đường dẫn đầu tiên: đi theo con trỏ cha mẹ từ$t$trở lại gốc nhánh của nó, sau đó thêm vào trước$s$. 
- Đường dẫn thứ 2: làm tương tự bằng nhánh thứ 2 cũng đạt$t$. 
5. Xuất ra hai đường dẫn được xây dựng lại. 

Lý do chúng tôi theo dõi cha mẹ một cách rõ ràng trên mỗi nhánh là vì mỗi nhánh DFS tạo ra một cấu trúc cây, vì vậy mỗi nút có một nút tiền thân duy nhất bên trong nhánh đó, giúp việc tái thiết trở nên đơn giản. 

### Tại sao nó hoạt động 

Mỗi nút (trừ$s$) được gán vĩnh viễn cho chính xác một nhánh, nghĩa là mọi đỉnh trong đều thuộc về một cây DFS duy nhất có gốc tại một cây lân cận của$s$. Nếu một nút$t$được bắt gặp lần đầu tiên từ nhánh thứ hai, điều đó có nghĩa là tồn tại hai đường dẫn có hướng từ$s$ĐẾN$t$phân kỳ ngay lập tức tại$s$và không bao giờ giao nhau nữa trước đây$t$. Điều này đảm bảo tính tách rời đỉnh bên trong, vì không có nút bên trong nào có thể thuộc về cả hai nhánh cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, m, s = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v = map(int, input().split())
    g[u].append(v)

# branch_id[v] = which child-o_
```
