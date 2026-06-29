---
title: "CF 104741H - \u73bb\u7483\u7403"
description: "Chúng ta có một cây có gốc có kích thước $N$, bắt nguồn từ nút 1. Mỗi nút đại diện cho một nền tảng và mỗi nền tảng ban đầu chứa chính xác một quả cầu thủy tinh."
date: "2026-06-28T23:21:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104741
codeforces_index: "H"
codeforces_contest_name: "The 10th Jimei University Programming Contest"
rating: 0
weight: 104741
solve_time_s: 86
verified: true
draft: false
---

[CF 104741H - \u73bb\u7483\u7403](https://codeforces.com/problemset/problem/104741/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc có kích thước$N$, bắt nguồn từ nút 1. Mỗi nút đại diện cho một nền tảng và ban đầu mỗi nền tảng chứa chính xác một quả cầu thủy tinh. Mỗi cạnh được hướng từ một nút đến nút cha của nó, vì vậy mỗi quả bóng sẽ luôn di chuyển lên trên về phía gốc, đi qua một cạnh trong một đơn vị thời gian. 

Khi một quả bóng đến một nút$v$, nó có thể được thu thập ở đó nếu một thiết bị ở$v$hoạt động. Mỗi nút$v$có xác suất độc lập$p_v$thiết bị của nó hoạt động thành công. Nếu thiết bị hoạt động, quả bóng sẽ dừng vĩnh viễn tại$v$. Nếu thất bại, quả bóng tiếp tục di chuyển lên trên về phía bố mẹ. 

Một quả bóng tiếp tục chuyển động cho đến khi nó được nút đầu tiên trên đường đi có thiết bị hoạt động thu lại. Bởi vì$p_1 = 1$, mọi quả bóng được đảm bảo sẽ được thu thập tận gốc nếu không có gì ngăn chặn nó sớm hơn. 

Giá của một quả bóng được định nghĩa là số cạnh mà nó di chuyển trước khi được thu thập. Chúng ta được yêu cầu tính toán tổng chi phí dự kiến ​​trên tất cả$N$quả bóng, một quả bóng bắt đầu từ mỗi nút. 

Điểm tinh tế quan trọng là nhiều quả bóng chỉ tương tác thông qua việc chia sẻ cùng một cấu trúc cây chứ không phải trực tiếp. Mỗi quả bóng độc lập di chuyển lên trên, nhưng sự đóng góp dự kiến ​​của chúng phụ thuộc vào xác suất chung dọc theo đường đi. 

Các ràng buộc đi lên đến$N = 5 \cdot 10^5$, điều này ngay lập tức loại trừ mọi phương trình bậc hai hoặc thậm chí$O(N \log^2 N)$tính toán lại trên mỗi nút. Giải pháp phải tuyến tính hoặc gần tuyến tính, vì chỉ có khoảng vài chục triệu thao tác là an toàn về mặt thời gian. 

Một mô phỏng đơn giản sẽ di chuyển mọi quả bóng lên trên cho đến khi nó được thu thập, nhưng điều đó có thể giảm xuống$O(N^2)$trong một cây hình chuỗi. Ví dụ, trong một dòng$1 \leftarrow 2 \leftarrow 3 \leftarrow \dots \leftarrow N$, quả bóng ở nút$N$đi ngang qua$N-1$các cạnh, bóng ở$N-1$đi ngang qua$N-2$, v.v., tạo ra tổng bậc hai. 

Một khó khăn tiềm ẩn khác là việc liệt kê xác suất đơn giản trên tất cả các tập hợp con của thiết bị hoạt động là theo cấp số nhân. Ngay cả những chuỗi nhỏ cũng đã sản xuất$2^N$cấu hình, do đó, bất kỳ cách tiếp cận nào phân nhánh rõ ràng trên các thiết bị đang hoạt động/bị lỗi đều không thể thực hiện được. 

Khó khăn chính là tách tính ngẫu nhiên dọc theo một đường đi khỏi tập hợp khoảng cách của cây xác định. 

## Phương pháp tiếp cận 

Một cách trực tiếp để hình dung về quá trình này là mô phỏng từng quả bóng một cách độc lập. Đối với một quả bóng bắt đầu từ nút$u$, chúng ta đi lên trên cho đến khi gặp nút đầu tiên$v$thiết bị của họ hoạt động và chúng tôi thêm khoảng cách từ$u$ĐẾN$v$. Nếu không có thiết bị nào hoạt động thì nó sẽ kết thúc ở phần gốc, nhưng vì$p_1 = 1$, trường hợp này được hấp thụ tại nút 1. 

Cách nhìn này đúng nhưng quá chậm vì mỗi quả bóng có thể di chuyển ngang$O(N)$các cạnh, cho$O(N^2)$trường hợp xấu nhất. 

Quan sát quan trọng là thay vì theo dõi vị trí kết thúc của mỗi quả bóng, chúng ta đảo ngược góc nhìn. Sửa một nút$v$. Chúng tôi hỏi: những quả bóng nào được thu thập tại$v$, và tổng quãng đường họ đi được là bao nhiêu? 

Một quả bóng từ nút$u$được thu thập tại$v$chính xác khi có hai điều kiện. Đầu tiên, tất cả các thiết bị trên đường dẫn từ$u$lên đến nhưng không bao gồm$v$thất bại. Thứ hai, thiết bị ở$v$thành công. Xác suất này chỉ phụ thuộc vào tổ tiên của$v$, không phải trên cấu trúc cây con bên dưới. 

Điều này cho phép chúng ta tính xác suất thành một giá trị gắn liền với$v$, độc lập với cá nhân$u$, nhân với tổng cấu trúc thuần túy trên cây con của$v$. 

Vì vậy, kỳ vọng trở thành tổng của các nút$v$, trong đó mỗi nút đóng góp xác suất có trọng số thành công của nó nhân với tổng khoảng cách từ tất cả các nút trong cây con của nó đến$v$. Điều này biến đổi tính ngẫu nhiên trên các đường dẫn thành một hệ số nhân duy nhất cho mỗi nút. 

Để tính tổng khoảng cách của cây con một cách hiệu quả, chúng ta viết lại khoảng cách bằng cách sử dụng độ sâu. Đối với bất kỳ hậu duệ nào$u$của$v$, khoảng cách là$\text{depth}[u] - \text{depth}[v]$. Do đó, sự đóng góp của cây con giảm xuống để duy trì tổng chiều sâu và kích thước cây con. 

Cấu trúc cuối cùng trở thành một cây DP với một DFS để tích lũy xác suất và một để tổng hợp cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute mỗi quả bóng |$O(N^2)$|$O(N)$| Quá chậm | 
| Bảng liệt kê xác suất |$O(2^N)$|$O(2^N)$| Không thể | 
| Hệ số DP cây |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 1 và xử lý nó theo thứ tự DFS. 

1. Tính xác suất nút đó$v$là thiết bị làm việc đầu tiên trên đường đi từ bên dưới. Chúng tôi xác định một giá trị$g[v]$như xác suất mà tất cả tổ tiên của$v$thất bại và$v$thành công. Trong khi truyền từ cha mẹ sang con cái, chúng ta duy trì một tích số có xác suất thất bại đang chạy, vì vậy$g[v] = p_v \cdot \prod_{x \in \text{ancestors of } v} (1 - p_x)$. Điều này đảm bảo mỗi nút biết khả năng nó là điểm dừng cho bất kỳ quả bóng nào tiếp cận cây con của nó. 
2. Chạy DFS từ gốc trong khi vẫn duy trì thông tin cây con. Đối với mỗi nút$v$, tính hai giá trị trên cây con của nó: số lượng nút$sz[v]$và tổng độ sâu của các nút trong cây con của nó, ký hiệu là$sumDepth[v]$. 
3. Đối với mỗi nút$v$, tính tổng khoảng cách từ tất cả các nút trong cây con của nó tới$v$. Sử dụng danh tính$\text{dist}(u,v) = \text{depth}[u] - \text{depth}[v]$, chúng tôi suy ra:$$\sum_{u \in subtree(v)} \text{dist}(u,v) = sumDepth[v] - depth[v] \cdot sz[v].$$4. Nhân tổng cấu trúc này với trọng số xác suất$g[v]$. Điều này mang lại sự đóng góp dự kiến ​​của nút$v$đến câu trả lời cuối cùng. 
5. Tổng các khoản đóng góp trên tất cả các nút và xuất kết quả theo modulo$998244353$. 

### Tại sao nó hoạt động 

Chi phí dự kiến của mỗi quả bóng có thể được phân tách bằng cách điều chỉnh nút nơi nó được thu thập lần đầu tiên. Sự kiện “bóng từ$u$được thu thập tại$v$” chỉ phụ thuộc vào trạng thái của các nút trên đường đi từ$u$ĐẾN$v$, và sự phụ thuộc này tách biệt rõ ràng thành một yếu tố chỉ phụ thuộc vào tổ tiên của$v$và một con số xác định có bao nhiêu$u$nằm ở$v$cây con của . 

Sự phân tách này đảm bảo không tính hai lần: mỗi quả bóng đóng góp chính xác một lần, tại nút dừng của nó và tập hợp DFS liệt kê tất cả những đóng góp đó chính xác một lần cho mỗi nút. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    parent = [0] * n
    children = [[] for _ in range(n)]

    for i in range(1, n):
        fa = int(input().split()[0]) if False else None  # placeholder safe
```Chúng tôi không thể sử dụng dòng đầu vào không chính xác đó; sửa chữa đúng cách:
