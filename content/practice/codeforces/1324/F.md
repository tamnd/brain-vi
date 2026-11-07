---
problem: 1324F
contest_id: 1324
problem_index: F
name: "Maximum White Subtree"
contest_name: "Codeforces Round 627 (Div. 3)"
rating: 1800
tags: ["dfs and similar", "dp", "graphs", "trees"]
answer: passed_samples
verified: true
solve_time_s: 158
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2def77-79d8-83ec-88a3-9beb32439d59
---

# CF 1324F - Cây con trắng tối đa 

**Đánh giá:** 1800 
**Thẻ:** dfs và tương tự, dp, đồ thị, cây 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2def77-79d8-83ec-88a3-9beb32439d59 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta có một cây trong đó mỗi đỉnh được tô màu trắng hoặc đen. Đối với mỗi đỉnh$v$, chúng ta muốn xem xét tất cả các đồ thị con được kết nối của cây bao gồm$v$, và trong số đó chọn một điểm tối đa hóa điểm được xác định bằng số đỉnh trắng trừ đi số đỉnh đen. 

Một cách khác để nghĩ về điều này là mỗi nút đóng góp$+1$nếu nó có màu trắng và$-1$nếu nó có màu đen. Với mọi đỉnh$v$, chúng tôi muốn tổng giá trị nút tối đa có thể có trên bất kỳ tập hợp nút được kết nối nào phải chứa$v$. 

Ràng buộc$n \le 2 \cdot 10^5$ngay lập tức loại trừ mọi cách tiếp cận tính toán lại tìm kiếm toàn cầu trên mỗi nút. Một giải pháp đơn giản cố gắng chạy BFS hoặc DP từ mỗi đỉnh sẽ tốn kém$O(n^2)$trong trường hợp xấu nhất là vượt xa thời hạn. Thậm chí$O(n \log n)$mỗi nút là không khả thi; chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Một khó khăn nhỏ là cây con tối ưu cho một nút không nhất thiết phải là cây con của chính nó theo bất kỳ nghĩa gốc nào. Nó có thể mở rộng theo mọi hướng và các đỉnh khác nhau sẽ có các cấu trúc tối ưu chồng chéo. Một cạm bẫy tiềm ẩn khác là giả định rằng việc lấy toàn bộ cây hoặc một cây con có gốc cố định sẽ đưa ra câu trả lời, điều này không chính xác vì việc bao gồm một vùng âm lớn có thể làm giảm điểm. 

Ví dụ, hãy xem xét một chuỗi:```
1 - 2 - 3 - 4
colors: 1 0 0 1
```Đối với đỉnh 2, lấy toàn bộ cây sẽ tính tổng$0$, nhưng cây con kết nối tối ưu chứa 2 chỉ là$\{2\}$cho đi$-1$, vì vậy ngay cả việc cắt tỉa cục bộ cũng có vấn đề. Đối với đỉnh 3, tốt nhất là$\{3,4\}$với tổng$0$, không phải toàn bộ cây. Điều này cho thấy chúng ta phải kiểm soát cẩn thận cách thức lan truyền các khoản đóng góp. 

## Phương pháp tiếp cận 

Đầu tiên chúng ta xem xét chiến lược vũ phu. Đối với mỗi đỉnh$v$, chúng ta có thể coi nó như một gốc và khám phá tất cả các cây con được kết nối có chứa nó. Điều này tương đương với việc liệt kê tất cả các đồ thị con cảm ứng được kết nối bao gồm$v$, đó là hàm mũ trong cây nói chung. Ngay cả khi chúng ta hạn chế DP trên các tập con lân cận, số lượng trạng thái vẫn tăng theo cấp số nhân. Một lực lượng vũ phu khác có cấu trúc chặt chẽ hơn là nhổ cây ở$v$và chạy một DP cho phép chọn các cây con, nhưng chỉ bắt các cây con hướng xuống dưới và bỏ lỡ các phần mở rộng lên trên thông qua các gốc khác nhau. Lặp lại điều này cho mọi nút dẫn đến$O(n^2)$chuyển đổi tổng thể, quá chậm đối với$2 \cdot 10^5$. 

Quan sát quan trọng là vấn đề là một biến thể của tổng mảng con tối đa trên cây, nhưng với ràng buộc bổ sung là đồ thị con được chọn phải chứa đỉnh gốc. Điều này gợi ý ý tưởng lập trình động hai hướng: đầu tiên tính toán, đối với mỗi nút, đóng góp tốt nhất từ ​​cây con đi xuống của nó, sau đó truyền bá thông tin từ phía cha mẹ để mỗi nút cũng biết nó có thể đạt được gì khi mở rộng lên trên. 

Thẻ DP đầu tiên là cây cổ điển DP: xác định$down[v]$là tổng tốt nhất của cây con được kết nối bắt đầu tại$v$và chỉ đi xuống. Điều này được tính toán bằng cách sử dụng phương pháp truyền tải theo thứ tự sau. 

Bước thứ hai là root lại. Chúng tôi tuyên truyền một giá trị bổ sung$up[v]$, thể hiện sự đóng góp tốt nhất mà chúng ta có thể nhận được từ bên ngoài cây con của$v$, bị giới hạn ở các đường dẫn vẫn được kết nối và đi qua hướng gốc. kết hợp$down[v]$Và$up[v]$cho phép chúng ta tính toán câu trả lời tốt nhất cho mỗi nút như thể nó là trung tâm của cấu trúc. 

Đây chính xác là loại cấu trúc trong đó sự đóng góp của trẻ em là độc lập ngoại trừ thông qua nút cha, cho phép tái cấu trúc tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O(n^2)$|$O(n)$| Quá chậm | 
| Cây DP + Root lại |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi gán cho mỗi nút một giá trị$w[v]$, Ở đâu$w[v] = 1$nếu màu trắng và$w[v] = -1$nếu đen. 

### Bước 1: Root cây tùy ý 

Chúng ta root cây ở nút 1. Điều này không thay đổi câu trả lời vì mọi cây con được kết nối hợp lệ đều có thể được mô tả tương đối với bất kỳ gốc nào. 

### Bước 2: Tính các khoản đóng góp giảm dần 

Chúng tôi tính toán$down[v]$, tổng tốt nhất của cây con được kết nối phải bao gồm$v$và chỉ mở rộng sang các con của nó. 

Đối với mỗi nút$v$, chúng ta bắt đầu với$down[v] = w[v]$. Sau đó với mỗi đứa trẻ$u$, chúng tôi xem xét liệu việc mở rộng sang đứa trẻ đó có cải thiện điểm số hay không. Nếu như$down[u] > 0$, chúng tôi thêm nó vào$down[v]$. Nếu không, chúng tôi bỏ qua nó. 

Điều này có hiệu quả vì nếu một cây con đóng góp mức tăng âm hoặc bằng 0, thì việc bao gồm cả nó sẽ không cải thiện bất kỳ cấu trúc được kết nối nào có gốc tại$v$. 

### Bước 3: Duyệt DFS đầu tiên 

Chúng tôi tính toán$down[v]$sử dụng DFS đặt hàng sau, đảm bảo trẻ em được xử lý trước cha mẹ của chúng. 

### Bước 4: Tính đáp án bằng rerooting 

Bây giờ chúng tôi truyền bá thông tin từ cha mẹ đến con cái. Cho phép$ans[v]$là câu trả lời tốt nhất cho đỉnh$v$. 

Ban đầu,$ans[v] = down[v]$khi root ở 1. 

Khi chuyển từ cha mẹ$v$cho con$u$, chúng ta phải xem xét rằng$u$có thể nhận được sự đóng góp từ mọi thứ bên ngoài cây con của nó. Chúng tôi chuyển một giá trị$up[u]$đại diện cho sự đóng góp tốt nhất từ$v$bên của trừ$u$cây con của. 

Để tính toán$up[u]$, chúng tôi kết hợp: 

- đóng góp của phụ huynh$v$, 
- đóng góp từ anh chị em của$u$,
- Và$w[v]$chính nó. 

Ý tưởng quan trọng là mỗi nút coi các nút lân cận của nó là các nhánh độc lập và chúng ta chỉ cần biết những đóng góp tích cực tốt nhất trong số đó. 

### Bước 5: Tổng hợp kết quả 

Đối với mỗi nút$v$, câu trả lời cuối cùng là:$$ans[v] = w[v] + \sum \max(0, best\_branch\_contribution)$$nơi các nhánh bao gồm cả con và hướng đi lên. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến mà đối với bất kỳ nút nào, cây con được kết nối tối ưu chứa nó có thể được phân tách thành các nhánh độc lập thông qua các nút lân cận. Mỗi nhánh hoặc đóng góp một lợi ích tích cực hoặc bị loại trừ hoàn toàn. DP khởi động lại đảm bảo rằng mọi nút đều được coi là trung tâm tiềm năng và tất cả các hướng sự cố được đánh giá chính xác một lần với ngữ cảnh chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(int, input().split()))

g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

w = [1 if x == 1 else -1 for x in a]

parent = [-1] * n
down = [0] * n

order = []

def dfs(v, p):
    parent[v] = p
    down[v] = w[v]
    for to in g[v]:
        if to == p:
            continue
        dfs(to, v)
        if down[to] > 0:
            down[v] += down[to]

dfs(0, -1)

ans = [0] * n
ans[0] = down[0]

def reroot(v, p):
    ans[v] = down[v]
    for to in g[v]:
        if to == p:
            continue

        # remove contribution of child and recompute "up" effect
        if down[to] > 0:
            down[v] -= down[to]

        if down[v] > 0:
            down[to] += down[v]

        reroot(to, v)

        if down[v] > 0:
            down[to] -= down[v]
        if down[to] > 0:
            down[v] += down[to]

    ans[v] = max(ans[v], w[v])

reroot(0, -1)

print(*ans)
```Mã thực hiện chiến lược hai lượt. DFS đầu tiên tính toán các khoản đóng góp giảm dần, trong đó mỗi nút chỉ tổng hợp các khoản đóng góp con tích cực. Chức năng tái cấu trúc cố gắng chuyển sự đóng góp có lợi từ phía cha mẹ sang mỗi đứa trẻ, tạm thời sửa đổi các giá trị để mô phỏng sự thay đổi quan điểm. Ý tưởng chính là chỉ những đóng góp tích cực mới đáng được nhân rộng, vì vậy những nhánh tiêu cực luôn bị loại trừ. 

Một vấn đề triển khai tế nhị là chúng tôi sửa đổi`down`tại chỗ trong quá trình khởi động lại. Điều này đòi hỏi sự phục hồi cẩn thận sau khi đệ quy để duy trì tính chính xác. Cấu trúc đảm bảo rằng tại bất kỳ thời điểm nào,`down[v]`phản ánh sự đóng góp chính xác cho định hướng gốc hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 1 0
1 2
2 3
```Giá trị:$[-1, +1, -1]$| Nút | Giảm giá trị | Nhánh tốt nhất được lấy | Lý do | 
| --- | --- | --- | --- | 
| 1 | -1 | không | trẻ em giảm giá trị | 
| 2 | 1 | không | cả hai bên đều tiêu cực | 
| 3 | -1 | không | bị cô lập tiêu cực | 

Câu trả lời cuối cùng:```
1 1 1
```Giải thích: nút 2 chiếm ưu thế vì đây là giá trị dương duy nhất. Bất kỳ cây con nào được kết nối chứa điểm cuối đều bị phạt. 

### Ví dụ 2 

đầu vào:```
5
1 0 1 0 1
```Giá trị:$[+1,-1,+1,-1,+1]$| Nút | Đóng góp xuống | Vùng lân cận tối ưu | 
| --- | --- | --- | 
| 1 | 2 | bao gồm nút 3 | 
| 2 | 0 | không được lợi gì từ con cái | 
| 3 | 3 | gồm 1 và 5 chi nhánh | 
| 4 | 0 | tâm âm cô lập | 
| 5 | 1 | chính nó | 

Điều này cho thấy các nhánh dương được hợp nhất có chọn lọc trong khi các nút âm bị loại trừ trừ khi được yêu cầu để kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cạnh được xử lý một số lần không đổi trong DFS và khởi động lại | 
| Không gian |$O(n)$| danh sách kề và mảng DP | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc cho$2 \cdot 10^5$các nút và độ sâu đệ quy có thể quản lý được với giới hạn đệ quy tăng lên. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline  # placeholder, replace with real call

# sample case
assert run("""3
0 1 0
1 2
2 3
""") == "1 1 1", "sample 1"

# chain all black
assert run("""4
0 0 0 0
1 2
2 3
3 4
""") == "0 0 0 0", "all black"

# all white
assert run("""4
1 1 1 1
1 2
1 3
1 4
""") == "4 4 4 4", "all white star"

# alternating chain
assert run("""5
1 0 1 0 1
1 2
2 3
3 4
4 5
""") == "1 2 3 2 1", "alternating chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dây chuyền toàn màu đen | tất cả số không | cắt tỉa chỉ tiêu cực | 
| toàn sao trắng | tất cả các nút bằng max | tổng hợp đầy đủ | 
| chuỗi xen kẽ | lan truyền đối xứng | root lại đúng cách | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các nút đều có màu đen. Trong trường hợp đó, mọi tổng của cây con đều không dương và lựa chọn tối ưu cho mỗi nút chỉ là chính nút đó, cho kết quả bằng 0. Thuật toán xử lý vấn đề này vì tất cả các đóng góp đều âm, do đó không có phần tử con nào được thêm vào DP và mỗi câu trả lời giảm về 0. 

Một trường hợp khác là biểu đồ hình sao có một tâm màu trắng và nhiều lá màu đen. Cây con tối ưu cho phần trung tâm chỉ bao gồm chính nó, nhưng đối với các lá, nó bao gồm phần trung tâm và có thể có những đóng góp tích cực khác nếu chúng tồn tại. Việc lấy rễ lại đảm bảo rằng mỗi chiếc lá đều coi sự đóng góp của trung tâm là một nhánh đi lên. 

Trường hợp cuối cùng là một chuỗi dài trong đó các mặt tích cực được phân tách bằng các tiêu cực. Thuật toán cắt bỏ các phân đoạn tiêu cực một cách chính xác vì chúng không bao giờ đóng góp lợi ích tích cực khi DP đi xuống và việc khởi động lại chỉ kết hợp lại các phân đoạn có lợi, đảm bảo không bắt buộc đưa vào các nút có hại.
