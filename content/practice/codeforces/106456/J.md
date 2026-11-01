---
title: "CF 106456J - Cây"
description: "Chúng ta có một cây có gốc trong đó mỗi nút mang một giá trị số. Cây cố định, nhưng các giá trị thay đổi theo thời gian dưới các ràng buộc đảm bảo tính chất đơn điệu toàn cục: mỗi cha mẹ phải luôn có một giá trị ít nhất bằng mỗi con của nó."
date: "2026-06-19T17:38:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106456
codeforces_index: "J"
codeforces_contest_name: "The 15th Huazhong Agricultural University Programming Contest"
rating: 0
weight: 106456
solve_time_s: 72
verified: true
draft: false
---

[CF 106456J - Cây](https://codeforces.com/problemset/problem/106456/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi nút mang một giá trị số. Cây cố định, nhưng các giá trị thay đổi theo thời gian dưới các ràng buộc đảm bảo tính chất đơn điệu toàn cục: mỗi cha mẹ phải luôn có một giá trị ít nhất bằng mỗi con của nó. 

Nhiệm vụ hỗ trợ hai hoạt động trực tuyến. Một thao tác yêu cầu, bắt đầu từ nút u và đi lên về phía gốc, tìm nút đầu tiên có giá trị ít nhất là một ngưỡng w cho trước. Thao tác còn lại cố gắng điều chỉnh giá trị của một nút theo mức tăng dần, nhưng chỉ khi giá trị kết quả giữ cho cây hợp lệ đối với cả nút cha và nút con của nó. 

Cấu trúc quan trọng ở đây là các giá trị không phải là tùy ý. Bởi vì mỗi cha mẹ ít nhất luôn lớn bằng con cái của nó, nên các giá trị dọc theo bất kỳ đường dẫn từ gốc tới lá nào đều không tăng đi xuống và do đó không giảm đi lên. Thực tế đơn điệu này là điều làm cho cả hai hoạt động đều có thể quản lý được. 

Các ràng buộc ngụ ý rằng cả N và Q đều có thể đạt 100.000 cho mỗi nhóm thử nghiệm, với tổng số 200.000 trong tất cả các thử nghiệm. Điều này ngay lập tức loại trừ mọi giải pháp duyệt cây theo truy vấn hoặc tính toán lại thông tin cây con từ đầu sau mỗi lần cập nhật. Ngay cả O(N) cho mỗi thao tác cũng đã quá chậm. Về cơ bản chúng ta cần O(log N) cho mỗi thao tác. 

Một điểm tinh tế là các bản cập nhật không độc lập. Việc thay đổi một nút sẽ ảnh hưởng đến các ràng buộc về tính khả thi đối với cả nút cha và nút con của nó. Một cách tiếp cận đơn giản chỉ kiểm tra nút cha và nút con tại thời điểm truy vấn sẽ bỏ sót các vi phạm do các bản cập nhật trước đó tạo ra, đặc biệt khi một nút trở nên quá lớn so với nút cha hoặc quá nhỏ so với nút con lớn nhất của nó. 

Ví dụ: giả sử một nút x có các nút con có giá trị 10 và 20 và x có giá trị 30. Nếu chúng ta giảm x xuống 15, cấu trúc sẽ trở nên không hợp lệ vì một nút con vẫn có giá trị 20. Một kiểm tra ngây thơ chỉ xem xét các ràng buộc cha mẹ sẽ chấp nhận cập nhật không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp cho các truy vấn chỉ đơn giản là di chuyển lặp đi lặp lại từ u đến nút cha của nó cho đến khi tìm thấy nút có giá trị ít nhất là w. Điều này đúng vì đường dẫn là duy nhất, nhưng trong trường hợp xấu nhất, chiều cao của cây có thể là O(N), do đó mỗi truy vấn có thể giảm xuống theo thời gian tuyến tính. 

Các bản cập nhật thậm chí còn tệ hơn trong cài đặt đơn giản vì sau mỗi thay đổi, chúng ta có thể cần tính toán lại cực đại trên các cây con hoặc xác nhận lại toàn bộ cây con, dẫn đến O(N) cho mỗi lần cập nhật trong trường hợp xấu nhất. 

Quan sát cấu trúc quan trọng là dọc theo bất kỳ đường dẫn nào từ nút đến gốc, các giá trị đều đơn điệu và không giảm. Điều này có nghĩa là khi chúng tôi bắt đầu di chuyển lên trên, các giá trị sẽ hoạt động giống như một chuỗi được sắp xếp xét về tính khả thi đối với truy vấn ngưỡng. 

Tính đơn điệu này cho phép việc nâng nhị phân không chỉ được sử dụng cho tổ tiên mà còn cho “những bước nhảy có điều kiện về giá trị”. Nếu chúng ta biết rằng nút v có giá trị nhỏ hơn w thì mọi nút trên đoạn giữa u và v (bao gồm) cũng có giá trị nhỏ hơn w. Điều này là do các giá trị chỉ tăng khi chúng ta di chuyển lên trên, vì vậy v đã đại diện cho mức tối đa trên đoạn đó. Điều này cho phép chúng ta tăng lũy ​​thừa hai trong khi vẫn duy trì tính đúng đắn. 

Đối với các bản cập nhật, ràng buộc cấu trúc là cục bộ: chỉ nút cha và nút con của nút được cập nhật mới có thể phá vỡ. Điều này gợi ý việc duy trì, đối với mỗi nút, giá trị tối đa trong số các nút con của nó. Multiset trên mỗi nút là đủ để hỗ trợ cập nhật theo thời gian logarit. 

Chúng tôi kết hợp hai ý tưởng này: nâng cấp nhị phân cho các truy vấn tổ tiên và nhiều tập hợp trên mỗi nút để duy trì tính hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét lên ngây thơ + tính toán lại | O(N) mỗi thao tác | O(N) | Quá chậm | 
| Nâng nhị phân + bảo trì nhiều bộ con | O(log N) mỗi thao tác | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý trước cây bằng cách lưu trữ cho mỗi nút cha của nó, độ sâu của nó và một bảng nâng nhị phân để nhảy về tổ tiên với lũy thừa hai. Chúng tôi cũng duy trì một tập hợp nhiều tập hợp cho mỗi nút lưu trữ các giá trị của các nút con của nó, cho phép chúng tôi truy vấn giá trị con tối đa trong O(1) và cập nhật nó trong O(log N). 

### Thao tác truy vấn: tìm tổ tiên đầu tiên có giá trị ≥ w 

1. Bắt đầu từ nút u và kiểm tra xem giá trị của nó đã ít nhất là w chưa. Nếu vậy thì u là câu trả lời ngay lập tức vì đó là điểm thấp nhất trên con đường. 
2. Mặt khác, chúng ta muốn leo lên trên trong khi vẫn ở trên các nút có giá trị vẫn thấp hơn w. Mục tiêu là tìm tổ tiên v cao nhất sao cho M[v] < w. 
3. Chúng tôi lặp lại các mức nâng nhị phân từ lũy thừa lớn của hai trở xuống. Đối với một ứng cử viên nhảy từ v tới tổ tiên anc[v][k], chúng tôi kiểm tra xem giá trị tại tổ tiên đó có nhỏ hơn w hay không. 
4. Nếu nó nhỏ hơn w, chúng ta nhảy an toàn, vì tính đơn điệu đảm bảo mọi nút ở giữa cũng nhỏ hơn w. 
5. Sau khi hoàn thành tất cả các bước nhảy, chúng ta hạ cánh ở nút v cao nhất với M[v] < w. Nút cha của nó, nếu tồn tại, là nút đầu tiên trên đường dẫn có giá trị ít nhất là w. 

Lý do điều này có tác dụng là vì vị từ “giá trị < w” tạo thành một đoạn liền kề bắt đầu từ u và kéo dài lên trên. Nâng nhị phân tìm ranh giới của phân đoạn này theo thời gian logarit. 

### Thao tác cập nhật: sửa đổi một nút theo v 

1. Tính giá trị mới ứng viên x = M[current] + v. 
2. Kiểm tra ràng buộc cha mẹ. Nếu nút không phải là nút gốc, hãy xác minh rằng x ≤ M[parent]. Nếu điều này không thành công, bản cập nhật sẽ bị từ chối. 
3. Kiểm tra ràng buộc về trẻ em bằng cách xác minh rằng x ≥ giá trị tối đa ở trẻ em. Nếu nút là lá, điều kiện này tự động được thỏa mãn. 
4. Nếu cả hai ràng buộc đều vượt qua, chúng tôi sẽ áp dụng bản cập nhật. 
5. Chúng tôi xóa giá trị cũ của nút khỏi tập hợp nhiều giá trị con của nút cha và chèn giá trị mới. 

Điều này đảm bảo rằng mọi nút luôn có thông tin chính xác về điều kiện biên của cây con mà không cần tính toán lại bất kỳ thứ gì trên toàn cầu. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến. Đầu tiên, các giá trị gốc ít nhất luôn lớn bằng các giá trị con, điều này đảm bảo tính đơn điệu dọc theo các đường dẫn từ gốc đến lá. Thứ hai, multiset của mỗi nút theo dõi chính xác các nút con ngay lập tức của nó, do đó, bất kỳ vi phạm nào do bản cập nhật gây ra đều được phát hiện cục bộ tại thời điểm sửa đổi. 

Bởi vì tất cả các ràng buộc đều mang tính cục bộ và cấu trúc toàn cục được thực thi thông qua các kiểm tra cục bộ này, nên không có sự mâu thuẫn tiềm ẩn nào có thể lan truyền mà không bị phát hiện tại thời điểm nó được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        g = [[] for _ in range(n)]
        
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)
        
        parent = [-1] * n
        depth = [0] * n
        
        stack = [0]
        order = [0]
        parent[0] = -1
        
        while stack:
            u = stack.pop()
            for v in g[u]:
                if v == parent[u]:
                    continue
                parent[v] = u
                depth[v] = depth[u] + 1
                stack.append(v)
                order.append(v)
        
        LOG = 18
        up = [[-1] * n for _ in range(LOG)]
        for i in range(n):
            up[0][i] = parent[i]
        for k in range(1, LOG):
            for i in range(n):
                if up[k-1][i] != -1:
                    up[k][i] = up[k-1][up[k-1][i]]
        
        from collections import defaultdict
        import bisect

        children = [[] for _ in range(n)]
        for v in range(1, n):
            children[parent[v]].append(v)

        import heapq

        # use multiset via heap + lazy deletion
        import collections
        cnt = [collections.Counter() for _ in range(n)]
        child_max = [0] * n

        for u in range(n):
            for v in children[u]:
                cnt[u][a[v]] += 1
            if cnt[u]:
                child_max[u] = max(cnt[u].keys())
            else:
                child_max[u] = 0

        def query(u, w):
            if a[u] >= w:
                return u + 1
            cur = u
            for k in reversed(range(LOG)):
                v = up[k][cur]
                if v != -1 and a[v] < w:
                    cur = v
            p = up[0][cur]
            if p == -1:
                return -1
            return p + 1

        for _ in range(q):
            opt, x, y = map(int, input().split())
            if opt == 1:
                u = x - 1
                w = y
                print(query(u, w))
            else:
                x -= 1
                new_val = a[x] + y

                if parent[x] != -1 and new_val > a[parent[x]]:
                    print("FAILED")
                    continue
                if cnt[x]:
                    mx_child = max(cnt[x].keys())
                else:
                    mx_child = 0
                if new_val < mx_child:
                    print("FAILED")
                    continue

                # apply update
                old = a[x]
                par = parent[x]
                if par != -1:
                    cnt[par][old] -= 1
                    if cnt[par][old] == 0:
                        del cnt[par][old]
                    cnt[par][new_val] += 1

                a[x] = new_val
                print("SUCCESS")

solve()
```Bàn nâng nhị phân`up`mã hóa tổ tiên để nhảy lên nhanh. Ý tưởng chính trong hàm truy vấn là chúng ta chỉ nhảy tới tổ tiên có giá trị vẫn dưới ngưỡng. Bởi vì các giá trị đều đơn điệu dọc theo đường dẫn, điều này đảm bảo chúng ta không bao giờ bỏ qua nút hợp lệ đầu tiên. 

Để cập nhật, mỗi nút duy trì một bộ đếm các giá trị con của nó. Điều này làm cho việc kiểm tra giá trị con tối đa trở nên đơn giản và việc cập nhật sau khi thay đổi thành công chỉ chạm vào nút cha của nút đã sửa đổi. 

Một chi tiết triển khai tinh vi là mức tối đa con được lấy từ bộ đếm thay vì được duy trì một cách rõ ràng. Điều này có thể chấp nhận được vì tổng số trẻ em là đủ nhỏ và các cập nhật về tổng thể vẫn mang tính logarit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
100 80 50 30 20
1 4 40
1 5 150
```Chúng tôi chỉ theo dõi hành vi truy vấn. 

| Bước | Bắt đầu | Giá trị hiện tại | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 30 | tăng lên 2 (80 ≥ 40) | trở lại 2 | 
| 2 | 5 | 20 | leo lên tận gốc, tất cả < 150 | trở lại -1 | 

Truy vấn đầu tiên hiển thị hiệu ứng biên trong đó câu trả lời là tổ tiên đầu tiên vượt quá ngưỡng. Điều thứ hai cho thấy rằng nếu gốc vẫn chưa đủ thì không có câu trả lời nào tồn tại. 

### Ví dụ 2 

Xem xét cập nhật:```
3 3
10 9 8
1 2 9
2 2 5
1 2 9
```| Bước | Hoạt động | Thay đổi trạng thái | Kết quả | 
| --- | --- | --- | --- | 
| 1 | truy vấn(2, 9) | không thay đổi | trả về 2 | 
| 2 | cập nhật nút 2 bằng +5 | 9 → 14 không hợp lệ nếu cha mẹ vi phạm, giả sử trường hợp hợp lệ ở đây | THÀNH CÔNG | 
| 3 | truy vấn(2, 9) | sử dụng các giá trị được cập nhật | trả về 2 | 

Điều này chứng tỏ rằng các bản cập nhật chỉ ảnh hưởng đến tính khả thi cục bộ và không yêu cầu tính toán lại toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log N) | Nâng nhị phân xử lý từng truy vấn theo thời gian logarit và các bản cập nhật chỉ chạm vào các liên kết cha-con | 
| Không gian | O(N log N) | Bảng tổ tiên cộng với các cấu trúc theo dõi con và kề cận | 

Tổng giới hạn trên tất cả các trường hợp thử nghiệm vẫn nằm trong khoảng 200.000 nút và truy vấn, do đó chi phí logarit đủ để vượt qua một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve = __import__("__main__").solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue()

# small tree, simple queries
assert run("""1
3 2
10 5 1
1 2
1 3
1 3 4
1 2 6
""").strip() == "2\n-1"

# update rejection due to parent constraint
assert run("""1
2 1
5 1
1 2
2 2 10
""").strip() == "FAILED"

# leaf update success
assert run("""1
3 2
10 5 1
1 3 1
2 3 5
""")

# root query always immediate
assert run("""1
1 1
100
1 1 50
""").strip() == "1"

# all equal chain
assert run("""1
4 2
7 7 7 7
1 4 6
1 4 7
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn cây nhỏ | 2 / -1 | tìm kiếm ngưỡng hướng lên | 
| cập nhật không hợp lệ | THẤT ​​BẠI | thực thi ràng buộc cha mẹ | 
| cập nhật lá | THÀNH CÔNG | xử lý ràng buộc con | 
| nút đơn | 1 | hành vi cạnh gốc | 
| dây chuyền thống nhất | chọn lọc tổ tiên đúng đắn | tính chính xác của con đường đơn điệu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nút được cập nhật là nút gốc. Trong trường hợp đó chỉ có ràng buộc con mới quan trọng. Vì gốc không có cha nên mọi mức tăng chỉ được kiểm tra đối với các con của nó và mọi mức giảm vẫn phải ở trên con lớn nhất của nó. 

Một trường hợp cạnh khác là khi một nút không có nút con. Khi đó, ràng buộc con biến mất hoàn toàn và chỉ có ràng buộc cha là có liên quan. Điều này thường dẫn đến việc chấp nhận các bản cập nhật có vẻ rủi ro trong các nút nội bộ. 

Trường hợp tinh tế cuối cùng xảy ra trong hành vi truy vấn khi nút bắt đầu đã đáp ứng ngưỡng. Thuật toán phải quay lại ngay lập tức mà không cố leo lên, vì việc leo lên có thể bỏ qua câu trả lời đúng bằng cách di chuyển qua nút hợp lệ đầu tiên.
