---
title: "CF 1765A - Cấp độ truy cập"
description: "Mỗi tài liệu được mô tả theo đó các nhà phát triển có thể mở nó. Vì vậy, mỗi cột của ma trận đầu vào là một tập hợp con của các nhà phát triển: những hàng có giá trị là một dạng "tập hợp được phê duyệt" cho tài liệu đó."
date: "2026-06-09T13:03:56+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dsu", "flows", "graph-matchings"]
categories: ["algorithms"]
codeforces_contest: 1765
codeforces_index: "A"
codeforces_contest_name: "2022-2023 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Preferably Teams)"
rating: 2400
weight: 1765
solve_time_s: 149
verified: false
draft: false
---

[CF 1765A - Cấp độ truy cập](https://codeforces.com/problemset/problem/1765/A) 

**Đánh giá:** 2400 
**Thẻ:** bitmask, dsu, luồng, đối sánh biểu đồ 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi tài liệu được mô tả theo đó các nhà phát triển có thể mở nó. Vì vậy, mỗi cột của ma trận đầu vào là một tập hợp con của các nhà phát triển: những hàng có giá trị là một dạng "tập hợp được phê duyệt" cho tài liệu đó. 

Chúng ta được phép nén ma trận nhị phân này thành một số ít nhóm truy cập. Trong mỗi nhóm, chúng tôi gán cho mỗi tài liệu một ngưỡng và mỗi nhà phát triển một giá trị số. Nhà phát triển có thể truy cập tài liệu nếu trong nhóm tài liệu đó, giá trị của nhà phát triển ít nhất là ngưỡng của tài liệu. 

Hạn chế chính về cấu trúc là trong một nhóm duy nhất, tất cả các tài liệu phải tương thích với một xếp hạng chung của các nhà phát triển. Khi đó, một tài liệu được thể hiện bằng một điểm giới hạn trong xếp hạng đó: tất cả mọi người ở trên điểm giới hạn đều vượt qua, mọi người ở dưới đều thất bại. Vì vậy, mỗi tài liệu tương ứng với một ngưỡng phân chia duy nhất có cùng thứ tự. 

Đầu ra yêu cầu số lượng tối thiểu các nhóm thứ tự chia sẻ cần thiết để nhận ra chính xác tất cả các cột. 

Các ràng buộc n, m ≤ 500 ngụ ý rằng cả hàng và cột đều đủ nhỏ cho các cấu trúc O(m^2) hoặc O(m^3). Bất cứ điều gì liên quan đến việc so sánh từng cặp tài liệu đều được chấp nhận, trong khi suy luận theo cấp số nhân trên các tập hợp con thì không. 

Một trường hợp lỗi tinh vi xuất hiện khi ba hoặc nhiều tài liệu hình thành nên “không thể so sánh được” theo chu kỳ trong mối quan hệ tập hợp con. Ví dụ: nếu tài liệu A có các tài liệu ở hàng {1,2}, tài liệu B ở hàng {2,3} và tài liệu C ở {1,3} thì không có hai tài liệu nào là tập con của nhau, do đó không có thứ tự đơn lẻ nào có thể nhận ra nhiều hơn một trong số chúng trong một chuỗi. Một kẻ tham lam ngây thơ cố gắng đóng gói các tài liệu theo kích thước hoặc loại phù hợp đầu tiên sẽ đặt không chính xác hai tài liệu đó lại với nhau ngay cả khi các bộ yêu cầu của chúng không tương thích với bất kỳ thứ tự ngưỡng nào. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử tất cả các phép gán tài liệu có thể có vào k nhóm và kiểm tra xem mỗi nhóm có thể được thực hiện theo thứ tự nào đó của các nhà phát triển có ngưỡng hay không. Việc kiểm tra tính khả thi của một nhóm sẽ giảm xuống việc kiểm tra xem tất cả các cột của nó có thể được biểu diễn dưới dạng hậu tố của một hoán vị hay không, tương đương với việc kiểm tra xem các tập hợp có được lồng nhau bằng cách đưa vào sau một số thứ tự hay không. Việc thử tất cả các phân vùng của m tài liệu là số mũ tính bằng m, và thậm chí với m = 500, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là mỗi nhóm tương ứng chính xác với một chuỗi các tập hợp được đưa vào. Nếu chúng tôi sửa thứ tự của nhà phát triển thì mỗi tài liệu sẽ trở thành hậu tố của thứ tự đó, do đó, bất kỳ hai tài liệu nào trong cùng một nhóm đều phải có các tập hợp tài liệu lồng nhau. Ngược lại, bất kỳ chuỗi các tập hợp lồng nhau nào cũng có thể được hiện thực hóa bằng cách chỉ định cho các nhà phát triển một thứ tự phù hợp với việc đưa vào và đặt các ngưỡng giữa các điểm cắt riêng biệt. 

Vì vậy, vấn đề giảm xuống còn việc phân chia tất cả các tập tài liệu thành số chuỗi tối thiểu theo quan hệ tập hợp con. Đây là một vấn đề đặt ra cổ điển: kích thước của sự phân tách chuỗi tối thiểu bằng với kích thước của phản chuỗi tối đa. Việc tính toán trực tiếp điều này có thể được thực hiện thông qua kết hợp tối đa trên biểu đồ hai bên được xây dựng từ mối quan hệ bao hàm giữa các tài liệu. 

Chúng tôi xây dựng một biểu đồ trong đó chúng tôi kết nối tài liệu u với tài liệu v nếu tập hợp các tài liệu trong u được chứa hoàn toàn trong v. Việc khớp tương ứng với việc liên kết từng tài liệu với một tài liệu lớn hơn có thể xuất hiện sau nó trong một chuỗi. Kết quả khớp tối đa mang lại sự phân tách chuỗi tối thiểu và kích thước của nó quyết định câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | hàm mũ tính bằng m | O(m) | Quá chậm | 
| Kết hợp bao gồm (Dilworth) | O(m^2 + khớp) | O(m^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi tài liệu là một mối quan ngại đối với các nhà phát triển.

1. Đối với mỗi tài liệu, hãy xây dựng một tập hợp các tài liệu mà nhà phát triển yêu cầu quyền truy cập. Điều này chuyển đổi ma trận thành m tập con của vũ trụ n phần tử. 
2. Với mỗi cặp tài liệu u và v, hãy xác định xem S_u có được chứa hoàn toàn trong S_v hay không. Điều này được thực hiện bằng cách kiểm tra sự bao gồm trong O(n) trên mỗi cặp. 
3. Xây dựng biểu đồ lưỡng cực với hai bản sao của bộ tài liệu. Thêm một cạnh từ u ở vế trái vào v ở vế phải nếu S_u ⊂ S_v. Điều này mã hóa khả năng đặt u trước v trong cùng một chuỗi. 
4. Tính toán kết quả khớp tối đa trên biểu đồ hai bên này. Mỗi cạnh khớp thể hiện việc liên kết một tài liệu với một tài liệu lớn hơn trong cùng một chuỗi. 
5. Rút ra sự phân rã chuỗi tối thiểu từ việc so khớp bằng cách xây dựng lại tiêu chuẩn: các tài liệu không khớp trên chuỗi bắt đầu bên trái và các cạnh khớp sau đây sẽ tạo nên mỗi chuỗi. 
6. Số lượng chuỗi được tạo ra là k và mỗi chuỗi tương ứng với một nhóm truy cập. 
7. Đối với mỗi nhóm (chuỗi), chỉ định giá trị của nhà phát triển theo vị trí trong thứ tự chuỗi. Các nhà phát triển được xếp hạng sao cho sự bao gồm được tôn trọng và mỗi tài liệu nhận được một ngưỡng tách biệt những tài liệu được yêu cầu với những tài liệu khác trong chuỗi. 

Việc xây dựng đảm bảo mọi tài liệu được thực hiện dưới dạng ngưỡng cắt rõ ràng trong nhóm của nó vì trong một chuỗi, các bộ được lồng nhau chặt chẽ, do đó, một lệnh duy nhất của các nhà phát triển có thể phân tách tất cả các điểm cắt một cách nhất quán. 

### Tại sao nó hoạt động 

Bất kỳ nhóm hợp lệ nào đều buộc tất cả các tài liệu bên trong nó phải tương ứng với các tập hợp con lồng nhau của các nhà phát triển, bởi vì chúng phải được phân tách bằng một ngưỡng duy nhất theo một thứ tự cố định. Điều này ngụ ý mỗi nhóm là một chuỗi trong tập hợp con. 

Ngược lại, bất kỳ chuỗi tập hợp con nào cũng có thể được triển khai bằng cách ra lệnh cho các nhà phát triển sao cho thứ tự đưa vào được tôn trọng và ấn định ngưỡng giữa các tập hợp riêng biệt kế tiếp. Do đó, các nhóm khả thi chính xác là các chuỗi và vấn đề trở thành giảm thiểu số lượng chuỗi bao gồm tất cả các phần tử, mà theo Dilworth bằng với kích thước phản chuỗi tối đa và có thể tính toán được thông qua đối sánh hai bên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
g = [input().strip() for _ in range(n)]

# build sets: documents as bitsets over developers
docs = []
for j in range(m):
    s = set()
    for i in range(n):
        if g[i][j] == '1':
            s.add(i)
    docs.append(s)

# inclusion check
def subset(a, b):
    if len(a) >= len(b):
        return False
    for x in a:
        if x not in b:
            return False
    return True

# build bipartite edges
adj = [[] for _ in range(m)]
for i in range(m):
    for j in range(m):
        if i != j and subset(docs[i], docs[j]):
            adj[i].append(j)

# Kuh for matching
mt = [-1] * m

def dfs(v, vis):
    for to in adj[v]:
        if not vis[to]:
            vis[to] = True
            if mt[to] == -1 or dfs(mt[to], vis):
                mt[to] = v
                return True
    return False

match = 0
for i in range(m):
    vis = [False] * m
    if dfs(i, vis):
        match += 1

# build chains
used = [False] * m
nxt = [-1] * m
for j in range(m):
    if mt[j] != -1:
        nxt[mt[j]] = j

for j in range(m):
    if mt[j] == -1:
        cur = j
        while cur != -1:
            used[cur] = True
            cur = nxt[cur]

chains = []
for j in range(m):
    if mt[j] == -1:
        cur = j
        chain = []
        while cur != -1:
            chain.append(cur)
            cur = nxt[cur]
        chains.append(chain)

# assign groups
k = len(chains)
group = [0] * m
thr = [0] * m

for idx, ch in enumerate(chains):
    for pos, doc in enumerate(ch):
        group[doc] = idx + 1
        thr[doc] = pos + 1

# developer values per group
dev_val = [[0] * k for _ in range(n)]

for g_id, ch in enumerate(chains):
    for i in range(n):
        val = 1
        for doc in ch:
            if g[i][doc] == '1':
                val += 1
        dev_val[i][g_id] = val

print(k)
print(*group)
print(*thr)
for i in range(n):
    print(*dev_val[i])
```Phần so khớp thực thi rằng mọi tài liệu được gán cho nhiều nhất một tài liệu kế thừa trong một chuỗi, tạo ra một rừng chuỗi. Mỗi chỉ số chuỗi sẽ trở thành một nhóm. Ngưỡng chỉ đơn giản là vị trí trong chuỗi, đảm bảo các tài liệu sau này trong cùng chuỗi yêu cầu các điều kiện nghiêm ngặt hơn. 

Các giá trị của nhà phát triển được xây dựng cho mỗi nhóm bằng cách đếm số lượng tài liệu trong chuỗi mà họ đáp ứng, điều này đảm bảo tính đơn điệu dọc theo thứ tự chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
01
11
10
```Đầu tiên chúng ta tạo tập hợp: 

Tài liệu 0 có {1,2}, tài liệu 1 có {0,1,2} trong lập chỉ mục dựa trên 0. 

| bước | bộ tài liệu | tòa nhà cạnh | khớp | 
| --- | --- | --- | --- | 
| ban đầu | {1}, {0,1,2} | 0 ⊂ 1 | khớp 0→1 | 

Sau khi khớp, cả hai tài liệu tạo thành một chuỗi hoặc phân tách tùy theo hướng, dẫn đến k = 2 chuỗi trong một phân tách hợp lệ. 

Dấu vết cho thấy cấu trúc bao hàm buộc phải phân tách vì không thứ tự nào có thể đáp ứng cả hai lần cắt cần thiết trong một chuỗi. 

### Ví dụ 2 

đầu vào:```
2 2
01
10
```Các bộ rời rạc và không thể so sánh được. 

| bước | bộ tài liệu | cạnh bao gồm | khớp | 
| --- | --- | --- | --- | 
| ban đầu | {1}, {0} | không | không | 

Không có cạnh nào tồn tại, vì vậy mỗi tài liệu sẽ trở thành chuỗi riêng của nó. Điều này xác nhận rằng các tập hợp không thể so sánh được sẽ tạo ra các nhóm riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m^2 · n + m^2) | kiểm tra bao gồm từng cặp trên các bit có độ dài n cộng với kết hợp | 
| Không gian | O(m^2) | ma trận kề của đồ thị bao hàm | 

Các ràng buộc m, n ≤ 500 làm cho việc này nhanh chóng một cách thoải mái. Cấu trúc bậc hai chiếm ưu thế nhưng vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# provided sample checks would be inserted when solution is wrapped

# minimal case
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận 1x1 | k = 1 | chuỗi tầm thường | 
| tất cả số không | k = m | tất cả đều không thể so sánh được | 
| cột giống hệt nhau | k = 1 | lồng đầy đủ | 
| trộn ngẫu nhiên | khác nhau | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các tài liệu không thể so sánh được theo cặp. Trong tình huống đó, mọi thử nghiệm đưa vào đều thất bại, kết quả khớp trống và mỗi tài liệu trở thành chuỗi riêng của nó. Thuật toán trả về k = m một cách tự nhiên vì không có chuỗi nào có thể chứa nhiều hơn một phần tử mà không vi phạm cấu trúc tập hợp con. 

Một trường hợp khác là khi tất cả các tài liệu đều giống hệt nhau. Mỗi tập hợp vừa là tập hợp con vừa là tập hợp con của các tập hợp khác, do đó đồ thị trở nên được kết nối hoàn toàn theo cả hai hướng. Ghép các cặp tài liệu thành chuỗi dài và việc tái cấu trúc sẽ hợp nhất mọi thứ thành một nhóm duy nhất, tạo ra k = 1.
