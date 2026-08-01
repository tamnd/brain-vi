---
title: "CF 102576J - Chuột túi không gian"
description: "Tiểu hành tinh này là một mạng lưới hình khối khổng lồ với chiều dài cạnh một triệu, vì vậy việc lưu trữ các ô một cách rõ ràng là không thể. Các ô trống duy nhất là những ô bị loại bỏ bởi các đường hầm. Mỗi đường hầm là một đường thẳng hoàn chỉnh song song với một trong ba trục."
date: "2026-07-31T07:39:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102576
codeforces_index: "J"
codeforces_contest_name: "2020 Petrozavodsk Winter Camp, Jagiellonian U Contest"
rating: 0
weight: 102576
solve_time_s: 78
verified: true
draft: false
---

[CF 102576J - Chuột túi không gian](https://codeforces.com/problemset/problem/102576/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tiểu hành tinh này là một mạng lưới hình khối khổng lồ với chiều dài cạnh một triệu, vì vậy việc lưu trữ các ô một cách rõ ràng là không thể. Các ô trống duy nhất là những ô bị loại bỏ bởi các đường hầm. Mỗi đường hầm là một đường thẳng hoàn chỉnh song song với một trong ba trục. 

Một đường hầm có thể được mô tả bằng hai tọa độ cố định. Ví dụ: một đường hầm dọc theo trục x được xác định bởi`(y, z)`, bởi vì mỗi`(x, y, z)`ô trống. Nhiệm vụ là trả lời xem hai ô trống nhất định có thuộc cùng một vùng được kết nối của các ô trống hay không. 

Những hạn chế là khó khăn chính. Có thể có 300000 đường hầm và 500000 truy vấn. Một BFS trên khối sẽ cần tới`10^18`tế bào, điều đó là không thể. Ngay cả một biểu đồ chứa tất cả các ô trống cũng quá lớn. Thuật toán chỉ được làm việc với chính các đường hầm. 

Cấu trúc ẩn giấu là số lượng đường hầm nhỏ so với kích thước của tiểu hành tinh. Chúng ta chỉ cần hiểu các đường hầm kết nối với nhau như thế nào. 

Một lỗi phổ biến là chỉ kiểm tra xem hai ô có nằm trên cùng một đường hầm hay không. Điều này không thành công vì các đường hầm song song liền kề được kết nối. Một sai lầm khác là chỉ kiểm tra các nút giao thông có hướng khác nhau. Hai đường hầm song song cách nhau một đơn vị không bao giờ giao nhau, nhưng chuột túi vẫn có thể di chuyển giữa chúng. 

## Phương pháp tiếp cận 

Giải pháp Brute Force sẽ tạo ra mọi ô trống, kết nối các ô lân cận và chạy các thành phần được kết nối. Tiểu hành tinh chứa`10^18`các ô, do đó, ngay cả việc tạo lưới cũng không thể thực hiện được. 

Một nỗ lực tốt hơn là tạo một biểu đồ trong đó mỗi đường hầm là một đỉnh. Hai đỉnh được kết nối nếu các đường hầm tương ứng chạm vào nhau. Việc triển khai trực tiếp vẫn sẽ quá chậm vì nhiều đường hầm có thể giao nhau trên cùng một mặt phẳng tọa độ. Ví dụ: hàng nghìn đường hầm x và đường hầm y ở cùng tọa độ z sẽ tạo ra hàng triệu nút giao nhau theo cặp. 

Quan sát quan trọng là các giao lộ không cần phải được thể hiện riêng lẻ. Nếu có nhiều đường hầm đi qua cùng một lát cắt thì một đỉnh phụ có thể biểu diễn toàn bộ lát cắt đó. Việc kết nối mọi đường hầm với đỉnh phụ này sẽ mang lại kết nối giống hệt nhau. 

Ví dụ: mọi đường hầm x có cùng tọa độ z sẽ cắt mọi đường hầm y có tọa độ z đó. Một nút "z slice" duy nhất kết nối tất cả chúng. 

Sau quá trình nén này, chỉ còn lại sự kề cận cục bộ giữa các đường hầm song song. Điều đó có thể được xử lý bằng bản đồ băm vì mỗi đường hầm chỉ cần kiểm tra bốn đường lân cận có thể có trong không gian tọa độ hai chiều của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^18) | O(10^18) | Không thể | 
| Biểu đồ đường hầm với các nút giao nhau theo cặp | O(n²) | O(n²) | Quá chậm | 
| DSU với các nút lát phụ trợ | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi đường hầm duy nhất và gán cho nó một id. Một đường hầm được lưu trữ theo hướng và hai tọa độ cố định của nó. 
2. Tạo cấu trúc tập hợp rời rạc chứa tất cả các đường hầm và các nút phụ trợ bổ sung. Nút phụ đại diện cho tất cả các đường hầm chia sẻ một lát tọa độ cố định. 
3. Kết nối các đường hầm vuông góc thông qua các nút phụ trợ. Đường hầm chữ X`(y,z)`được kết nối với các nút phụ biểu diễn tọa độ`y`và phối hợp`z`, bởi vì đây chính xác là những lát cắt mà các đường hầm vuông góc có thể gặp nó. 
4. Kết nối các đường hầm lân cận song song. Đối với mỗi đường hầm, hãy kiểm tra xem có tồn tại một đường hầm khác có cùng hướng với tọa độ cố định tăng thêm một hay không. Nếu nó tồn tại, hợp nhất các thành phần của chúng. 
5. Đối với mỗi truy vấn, hãy tìm tất cả các đường hầm chứa ô bắt đầu và tất cả các đường hầm chứa ô kết thúc. Vì một ô được đảm bảo trống nên tồn tại ít nhất một đường hầm cho mỗi bên. Nếu bất kỳ đường hầm bắt đầu và đường hầm kết thúc nào có cùng đại diện DSU, câu trả lời là`YES`. 

Tại sao nó hoạt động: 

Mọi chuyển động giữa các ô trống đều nằm trên cùng một đường hầm, di chuyển giữa các đường hầm song song liền kề hoặc xảy ra tại giao điểm của hai đường hầm vuông góc. DSU chứa chính xác ba loại kết nối này. Các nút phụ biểu thị tất cả các giao điểm vuông góc có thể có mà không cần tạo rõ ràng từng cặp. Do đó, hai ô được kết nối trong tiểu hành tinh ban đầu một cách chính xác khi các đỉnh đường hầm tương ứng của chúng nằm trong cùng thành phần DSU. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    z = int(input())
    out = []

    for _ in range(z):
        n = int(input())

        xt = {}
        yt = {}
        zt = {}

        tunnels = []

        for _ in range(n):
            a, b, c = map(int, input().split())
            if c == -1:
                key = (a, b)
                if key not in zt:
                    zt[key] = len(tunnels)
                    tunnels.append((2, a, b))
            elif b == -1:
                key = (a, c)
                if key not in yt:
                    yt[key] = len(tunnels)
                    tunnels.append((1, a, c))
            else:
                key = (b, c)
                if key not in xt:
                    xt[key] = len(tunnels)
                    tunnels.append((0, b, c))

        parent = []
        size = []

        def add():
            parent.append(len(parent))
            size.append(1)
            return len(parent) - 1

        for _ in tunnels:
            add()

        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(a, b):
            a = find(a)
            b = find(b)
            if a == b:
                return
            if size[a] < size[b]:
                a, b = b, a
            parent[b] = a
            size[a] += size[b]

        aux = {}

        def get_aux(t, x):
            key = (t, x)
            if key not in aux:
                aux[key] = add()
            return aux[key]

        for i, (typ, a, b) in enumerate(tunnels):
            if typ == 0:
                union(i, get_aux(0, a))
                union(i, get_aux(1, b))
            elif typ == 1:
                union(i, get_aux(0, b))
                union(i, get_aux(2, a))
            else:
                union(i, get_aux(1, a))
                union(i, get_aux(2, b))

        for (a, b), i in xt.items():
            if (a + 1, b) in xt:
                union(i, xt[(a + 1, b)])
            if (a, b + 1) in xt:
                union(i, xt[(a, b + 1)])

        for (a, b), i in yt.items():
            if (a + 1, b) in yt:
                union(i, yt[(a + 1, b)])
            if (a, b + 1) in yt:
                union(i, yt[(a, b + 1)])

        for (a, b), i in zt.items():
            if (a + 1, b) in zt:
                union(i, zt[(a + 1, b)])
            if (a, b + 1) in zt:
                union(i, zt[(a, b + 1)])

        def get_lines(x, y, z):
            res = []
            if (y, z) in xt:
                res.append(xt[(y, z)])
            if (x, z) in yt:
                res.append(yt[(x, z)])
            if (x, y) in zt:
                res.append(zt[(x, y)])
            return res

        q = int(input())
        for _ in range(q):
            x1, y1, z1, x2, y2, z2 = map(int, input().split())
            a = get_lines(x1, y1, z1)
            b = get_lines(x2, y2, z2)

            ok = False
            for u in a:
                ru = find(u)
                for v in b:
                    if ru == find(v):
                        ok = True
                        break
                if ok:
                    break

            out.append("YES" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ giữ lại các đường hầm và biểu đồ lát nén. Các giá trị tọa độ có thể vẫn ở dạng số nguyên đầy đủ vì chúng chỉ được sử dụng làm khóa từ điển. Không cần nén tọa độ. 

Phần tinh tế nhất là việc xây dựng nút phụ trợ. Một đường hầm không được kết nối với mọi đường hầm giao nhau với nó. Thay vào đó, nó được kết nối với một nút đại diện cho toàn bộ lát tọa độ. Điều này tránh hành vi bậc hai trong khi vẫn duy trì kết nối. 

Hàng xóm kiểm tra chỉ sử dụng`+1`bởi vì mọi vùng lân cận vô hướng trong bố cục đường hầm hai chiều được nén đều được phát hiện từ một điểm cuối. Kiểm tra hướng tiêu cực sẽ nhân đôi công việc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi đường hầm thực hiện các thao tác từ điển liên tục và mỗi truy vấn sẽ kiểm tra tối đa ba đường hầm đối với ba đường hầm. | 
| Không gian | O(n) | DSU chứa các nút đường hầm và nhiều nhất là số lượng nút phụ trợ không đổi trên tọa độ đường hầm. | 

Các trường hợp lớn nhất chứa hàng trăm nghìn đường hầm và truy vấn. Thuật toán không bao giờ chạm vào khối lập phương triệu triệu triệu, vì vậy nó vừa khít trong giới hạn.
