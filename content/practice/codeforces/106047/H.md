---
title: "CF 106047H - Không phải vấn đề truy vấn đường dẫn khác"
description: "Chúng ta được cho một đồ thị vô hướng trong đó mỗi cạnh mang trọng số 60 bit. Đường dẫn giữa hai đỉnh được đánh giá không phải bằng cách tính tổng hoặc giảm thiểu trọng số mà bằng cách lấy AND theo từng bit của tất cả các trọng số cạnh dọc theo đường dẫn đó."
date: "2026-06-20T21:39:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106047
codeforces_index: "H"
codeforces_contest_name: "The 1st Universal Cup. Stage 21: Shandong"
rating: 0
weight: 106047
solve_time_s: 56
verified: true
draft: false
---

[CF 106047H - Không phải vấn đề truy vấn đường dẫn khác](https://codeforces.com/problemset/problem/106047/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng trong đó mỗi cạnh mang trọng số 60 bit. Đường dẫn giữa hai đỉnh được đánh giá không phải bằng cách tính tổng hoặc giảm thiểu trọng số mà bằng cách lấy AND theo từng bit của tất cả các trọng số cạnh dọc theo đường dẫn đó. Điều này có nghĩa là mọi bit trong giá trị đường dẫn chỉ tồn tại nếu mọi cạnh trên đường dẫn đều được đặt bit đó. 

Đối với mỗi cặp đỉnh truy vấn, chúng ta phải quyết định xem có tồn tại bất kỳ đường dẫn nào giữa chúng có giá trị AND theo bit ít nhất là một giá trị ngưỡng nhất định hay không$V$. “Ít nhất” ở đây quan trọng vì chúng ta so sánh kết quả AND cuối cùng dưới dạng số nguyên chứ không phải riêng lẻ từng bit một. 

Hạn chế cơ cấu chính là$n$Và$q$cả hai có thể đạt được$10^5$, và số cạnh có thể đạt tới$5 \times 10^5$. Bất kỳ cách tiếp cận nào cố gắng khám phá các đường dẫn cho mỗi truy vấn hoặc thậm chí chạy tìm kiếm giống như đường dẫn ngắn nhất cho mỗi truy vấn sẽ quá chậm. Ngay cả một BFS duy nhất cho mỗi truy vấn cũng dẫn đến$O(q(m+n))$, điều đó hoàn toàn không thể thực hiện được. Giải pháp phải xử lý trước biểu đồ để mỗi truy vấn được trả lời trong thời gian gần như không đổi. 

Một trường hợp lỗi tinh tế xuất hiện khi các cạnh tồn tại giữa hai đỉnh nhưng không có đường dẫn hợp lệ nào tồn tại dưới ràng buộc AND. Ví dụ, nếu$V = 6$và chúng ta có các cạnh$1-2$với trọng lượng 7 và$2-3$với trọng số 1 thì mặc dù$1$được kết nối với$3$trong biểu đồ cơ bản, đường dẫn$1 \to 2 \to 3$có giá trị AND$7 \& 1 = 1$, bên dưới$V$, vậy câu trả lời đúng là “Không”. 

Một trường hợp cạnh khác là khi$V = 0$. Trong trường hợp này, mọi đường dẫn đều thỏa mãn điều kiện một cách tầm thường vì mọi kết quả AND luôn ít nhất bằng 0. Vì vậy, câu trả lời sẽ giảm xuống kết nối tiêu chuẩn trong biểu đồ đầy đủ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xử lý từng truy vấn một cách độc lập. Đối với một truy vấn$(u, v)$, chúng tôi cố gắng tìm bất kỳ con đường nào từ$u$ĐẾN$v$và tính toán AND theo từng bit của các cạnh dọc theo đường dẫn đó. Một DFS hoặc BFS ngây thơ theo dõi giá trị AND hiện tại sẽ phân nhánh trên tất cả các đường dẫn có thể. Trong trường hợp xấu nhất, số lượng đường dẫn đơn giản trong biểu đồ là theo cấp số nhân và thậm chí giới hạn ở trạng thái BFS, chúng ta sẽ cần theo dõi các cặp có dạng$(node, current\_and)$, dẫn đến sự bùng nổ trong không gian trạng thái. Điều này nhanh chóng trở nên không thể được$q = 10^5$. 

Thông tin chi tiết quan trọng đến từ việc hiểu cách hành xử theo bit AND dọc theo một đường dẫn. Khi một bit trở thành 0 ở bất kỳ cạnh nào, nó không bao giờ có thể quay trở lại ở các cạnh sau. Điều này có nghĩa là để giá trị AND cuối cùng ít nhất$V$, mọi bit được đặt trong$V$phải tồn tại ở mọi cạnh trên đường đi. Nếu thậm chí một cạnh dọc theo đường dẫn bị thiếu một bit bắt buộc thì bit đó sẽ bị mất vĩnh viễn. 

Điều này biến vấn đề từ vấn đề tối ưu hóa đường dẫn thành vấn đề lọc đơn giản trên các cạnh. Chúng tôi chỉ quan tâm đến các cạnh tương thích với$V$, nghĩa là các cạnh có trọng số chứa tất cả các bit được đặt trong$V$. Khi chúng tôi loại bỏ tất cả các cạnh không tương thích, mọi đường dẫn trong biểu đồ còn lại sẽ tự động có ít nhất giá trị AND$V$, bởi vì mọi cạnh đều bảo toàn tất cả các bit cần thiết. 

Vì vậy, mỗi truy vấn sẽ giảm xuống thành kiểm tra kết nối thuần túy trong biểu đồ được lọc. Điều này có thể được xử lý một cách hiệu quả bằng cách sử dụng cấu trúc tập hợp rời rạc được xây dựng một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ cho mỗi truy vấn | O(n + m) | Quá chậm | 
| Tối ưu (lọc DSU) | O(m α(n) + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và xác định xem mỗi cạnh có thể sử dụng được hay không bằng cách kiểm tra xem$(w_i \& V) = V$. 

Điều kiện này đảm bảo mọi bit được yêu cầu trong$V$hiện diện trong trọng lượng cạnh. 
2. Khởi tạo cấu trúc hợp tập hợp rời rạc trong đó mỗi đỉnh bắt đầu trong thành phần riêng của nó. 
3. Đối với mọi cạnh thỏa mãn điều kiện, hãy hợp nhất các điểm cuối của nó trong DSU. 

Điều này xây dựng các thành phần được kết nối chỉ sử dụng các cạnh hợp lệ. 
4. Đối với mỗi truy vấn$(u, v)$, kiểm tra xem$u$Và$v$thuộc cùng một thành phần DSU. 

Nếu có thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

### Tại sao nó hoạt động 

Xét mọi đường đi được hình thành hoàn toàn từ các cạnh thỏa mãn$(w \& V) = V$. Đối với mỗi bit được đặt trong$V$, mọi cạnh dọc theo đường dẫn đều có bit đó được đặt, do đó bit tồn tại trong toàn bộ thao tác AND. Do đó giá trị đường dẫn luôn chứa tất cả các bit của$V$, ngụ ý rằng ít nhất nó là$V$. 

Ngược lại, nếu một đường dẫn sử dụng ngay cả một cạnh vi phạm điều kiện thì ít nhất một bit bắt buộc sẽ bị xóa vĩnh viễn, khiến kết quả AND không thể đạt được$V$. Vì vậy, mọi đường dẫn hợp lệ phải nằm hoàn toàn bên trong biểu đồ được lọc và khả năng kết nối trong biểu đồ đó mô tả chính xác câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

def main():
    n, m, q, V = map(int, input().split())
    dsu = DSU(n)

    for _ in range(m):
        u, v, w = map(int, input().split())
        if (w & V) == V:
            dsu.union(u, v)

    out = []
    for _ in range(q):
        u, v = map(int, input().split())
        out.append("Yes" if dsu.find(u) == dsu.find(v) else "No")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Cấu trúc DSU được sử dụng hoàn toàn để nén thông tin kết nối sau khi lọc các cạnh. Nén đường dẫn đảm bảo thời gian khấu hao gần như không đổi cho mỗi hoạt động, điều này cần thiết cho đến$10^5$truy vấn. 

Chi tiết triển khai quan trọng là điều kiện lọc cạnh`(w & V) == V`. Điều này chặt chẽ hơn việc kiểm tra`w >= V`, điều này sẽ không chính xác vì thứ tự số không bảo toàn sự bao gồm bit. Chỉ bao gồm bitwise đảm bảo sự ổn định trong AND. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3 2 4
1 2 5
2 3 7
1 3 6
1 3
1 4
```Chúng tôi xử lý các cạnh liên quan đến$V = 4$(nhị phân`100`). 

| Cạnh | Trọng lượng (thùng) | hợp lệ | hành động DSU | 
| --- | --- | --- | --- | 
| 1-2 | 101 | Có | công đoàn(1,2) | 
| 2-3 | 111 | Có | công đoàn(2,3) | 
| 1-3 | 110 | Có | công đoàn(1,3) | 

Sau khi xử lý, tất cả các nút {1,2,3} được kết nối. 

Truy vấn: 

| Truy vấn | Cùng thành phần | Trả lời | 
| --- | --- | --- | 
| 1-3 | Có | Có | 
| 1-4 | Không | Không | 

Điều này cho thấy rằng một khi các cạnh thỏa mãn ràng buộc bit, vấn đề sẽ chuyển thành kết nối. 

### Ví dụ 2 

đầu vào:```
3 2 1 6
1 2 7
2 3 1
1 3
```Đây$V = 6$(`110`). 

| Cạnh | Cân nặng | hợp lệ | Lý do | 
| --- | --- | --- | --- | 
| 1-2 | 7 | Có | chứa 110 | 
| 2-3 | 1 | Không | thiếu bit cần thiết | 

Các thành phần DSU trở thành {1,2} và {3}. Truy vấn 1-3 nằm giữa các thành phần khác nhau nên câu trả lời là Không. 

Điều này chứng tỏ rằng mặc dù đường dẫn đồ thị tồn tại nhưng nó không hợp lệ theo ràng buộc AND. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \alpha(n) + q \alpha(n))$| Mỗi cạnh được xử lý một lần để lọc và kết hợp, mỗi truy vấn là một DSU find | 
| Không gian |$O(n)$| Mảng kích thước và cha mẹ DSU | 

Giải pháp dễ dàng phù hợp trong giới hạn vì cả hai$m$Và$q$là$5 \times 10^5$và các hoạt động DSU thực tế là thời gian không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    try:
        main()
    except Exception:
        pass
    return sys.stdout.getvalue().strip()

# sample-like small case
assert run("""4 3 2 4
1 2 5
2 3 7
1 3 6
1 3
1 4
""") == "Yes\nNo"

# disconnected graph
assert run("""3 2 1 6
1 2 7
2 3 1
1 3
""") == "No"

# V = 0, all edges valid, connectivity works
assert run("""3 2 2 0
1 2 1
2 3 2
1 3
1 2
""") == "Yes\nYes"

# single edge only
assert run("""2 1 1 5
1 2 7
1 2
""") == "Yes"

# no edges
assert run("""3 0 2 0
1 2
2 3
""") == "No\nNo"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị lọc nhỏ | Có/Không | tính đúng đắn của việc lọc DSU | 
| con đường bị hỏng | Không | đường dẫn khối cạnh trung gian không hợp lệ | 
| V = 0 trường hợp | Có/Có | tất cả các cạnh được chấp nhận | 
| cạnh đơn | Có | kết nối tối thiểu | 
| đồ thị trống | Không/Không | không có kết nối nào cả | 

## Vỏ cạnh 

Khi nào$V = 0$, điều kiện$(w \& V) = V$luôn đúng với mọi cạnh vì cả hai cạnh đều bằng 0. Do đó, DSU hợp nhất theo biểu đồ đầy đủ và mọi truy vấn giảm xuống kết nối tiêu chuẩn, khớp chính xác với thực tế là mọi kết quả AND luôn ít nhất bằng 0. 

Khi đồ thị được kết nối theo nghĩa ban đầu nhưng không tồn tại cạnh hợp lệ nào dưới bộ lọc, mọi nút sẽ bị cô lập. Đối với một đầu vào như$V = 63$và tất cả các cạnh có bit thấp ngẫu nhiên, DSU không bao giờ hợp nhất bất kỳ đỉnh nào, do đó, ngay cả các truy vấn giữa các nút được kết nối trực tiếp trong biểu đồ gốc cũng trả về “Không”, khớp với yêu cầu rằng mọi cạnh trong một đường dẫn hợp lệ phải đáp ứng ràng buộc bit. 

Khi nhiều cạnh kết nối cùng một cặp nút, chúng sẽ được xử lý độc lập. Nếu có ít nhất một thỏa mãn điều kiện, DSU sẽ hợp nhất chúng, nhưng việc hợp nhất trùng lặp không ảnh hưởng đến tính chính xác do hoạt động hợp nhất không có hiệu lực.
