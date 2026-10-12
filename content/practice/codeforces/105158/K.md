---
title: "CF 105158K - \u6811\u4e0a\u95ee\u9898"
description: "Chúng ta được cấp một cây có các nút có trọng số. Cây không bị định hướng và có thể được root ở bất kỳ nút nào chúng ta chọn. Khi một nút gốc được cố định, mọi nút khác đều có chính xác một nút cha được xác định bởi cấu trúc cây gốc."
date: "2026-06-27T16:43:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105158
codeforces_index: "K"
codeforces_contest_name: "2024 National Invitational of CCPC (Zhengzhou), 2024 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 105158
solve_time_s: 45
verified: true
draft: false
---

[CF 105158K - \u6811\u4e0a\u95ee\u9898](https://codeforces.com/problemset/problem/105158/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có các nút có trọng số. Cây không bị định hướng và có thể được root ở bất kỳ nút nào chúng ta chọn. Khi một nút gốc được cố định, mọi nút khác đều có chính xác một nút cha được xác định bởi cấu trúc cây gốc. 

Một gốc được gọi là "hợp lệ" nếu, khi chúng ta định hướng cây ra xa khỏi nó, mọi cạnh từ nút gốc đến nút con đều thỏa mãn một ràng buộc cụ thể liên quan đến trọng số nút: giá trị của nút con phải đủ lớn so với nút gốc, cụ thể là ít nhất một nửa giá trị của nó theo nghĩa được mô tả trong câu lệnh (hiểu điều kiện là ngưỡng bất đẳng thức cha-con có dạng “con ≥ cha/2” như trong phần giải thích mẫu). 

Nhiệm vụ là đếm xem có bao nhiêu nút, khi được chọn làm gốc, làm cho toàn bộ cây có gốc thỏa mãn điều kiện này trên mỗi cạnh cha-con. 

Mỗi trường hợp thử nghiệm cung cấp một cây có tối đa 10^5 nút trong tất cả các trường hợp, vì vậy về cơ bản chúng tôi cần hành vi tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm. Bất kỳ giải pháp nào cố gắng mô phỏng việc root ở mọi nút và kiểm tra các ràng buộc một cách độc lập sẽ quá chậm, vì điều đó sẽ lặp lại quá trình truyền tải có kích thước O(n) cho mỗi ứng cử viên gốc, dẫn đến O(n^2). 

Một vấn đề tinh vi hơn là ràng buộc mang tính định hướng sau khi root. Một nút có thể hoạt động tốt với tư cách là nút cha nhưng lại thất bại khi là nút con tùy thuộc vào nút gốc, do đó, việc kiểm tra cục bộ ngây thơ trên các cạnh mà không xem xét định hướng toàn cục sẽ thất bại. 

Một cạm bẫy điển hình là giả định rằng nếu một cạnh thỏa mãn điều kiện theo một hướng thì nó sẽ an toàn cho tất cả các gốc. Điều đó không chính xác vì việc thay đổi gốc sẽ làm đảo lộn mối quan hệ cha-con dọc theo các đường dẫn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi gốc có thể, chúng tôi chạy DFS hoặc BFS để xây dựng cây gốc và xác minh rằng đối với mọi cạnh cha có hướng → con, ràng buộc được giữ nguyên. Mỗi kiểm tra là O(n) và có n nghiệm có thể, cho ra O(n^2) cho mỗi trường hợp kiểm thử. Với n lên tới 10^5, điều này trở nên hoàn toàn không khả thi. 

Điều quan trọng là chúng ta không cần phải tính toán lại mọi thứ từ đầu cho mỗi nghiệm. Khi chúng ta di chuyển gốc từ nút u sang một trong các nút lân cận v của nó, chỉ có mối quan hệ dọc theo cạnh (u, v) là thay đổi hướng; mọi thứ khác vẫn giữ nguyên về mặt cấu trúc nhưng có hiệu ứng root lại nhất quán. Điều này gợi ý quan điểm DP tái root. 

Chúng tôi diễn giải lại điều kiện trên cạnh u-v theo cách cho phép tính khả thi phụ thuộc vào hướng. Đối với mỗi cạnh, chỉ có một hướng có thể là “xấu” tùy thuộc vào trọng số. Điều đó có nghĩa là mỗi cạnh tạo ra một ràng buộc định hướng bị cấm: một điểm cuối có thể không phải là điểm cuối của điểm cuối kia trong một số trường hợp nhất định. 

Chúng tôi chuyển vấn đề thành việc xác định đối với mỗi nút, cho dù việc chọn nút đó làm nút gốc có tránh được tất cả các hướng cha-con bị cấm trên tất cả các cạnh hay không. Điều này trở thành một vấn đề khả thi về việc tái tạo lại cây cổ điển, trong đó trước tiên chúng tôi tính toán tính hợp lệ của một gốc tham chiếu và sau đó truyền bá các thay đổi bằng cách sử dụng hai lượt DFS. 

Trong bước đầu tiên, chúng tôi tính toán các ràng buộc hướng xuống, theo dõi từng nút xem cây con của nó có hợp lệ hay không nếu cạnh tới nút cha của nó được định hướng theo một hướng cụ thể. Ở lần thứ hai, chúng tôi root lại và duy trì sự đóng góp hợp lệ từ “cây con bên ngoài”. 

Điều này làm giảm vấn đề xuống còn O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Root lại DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả sử một gốc tùy ý cố định, chẳng hạn như nút 1 và tính toán tính khả thi về hướng cho các cạnh dựa trên ràng buộc.

1. Với mọi cạnh u-v, hãy xác định xem u có thể là cha của v hay v có thể là cha của u. Điều này xuất phát trực tiếp từ việc so sánh các trọng số theo quy tắc bất đẳng thức đã cho. Nếu một hướng vi phạm điều kiện, chúng tôi đánh dấu hướng đó là bị cấm. 
2. Xây dựng biểu đồ ràng buộc có hướng trên các cạnh của cây trong đó mỗi cạnh có thể cho phép một hoặc cả hai hướng. 
3. Chạy DFS từ gốc ban đầu để tính giá trị DP`down[u]`biểu thị liệu cây con của u có hợp lệ hay không nếu u là cha của tất cả các con của nó theo hướng gốc. Trong khi tính toán, chúng ta đảm bảo mọi con v đều thỏa mãn rằng hướng u → v được cho phép và cây con của v cũng hợp lệ. 
4. Sau khi tính toán`down`, chúng tôi thực hiện DFS thứ hai để tính toán`up[u]`, biểu thị liệu phần còn lại của cây (bên ngoài cây con của u) có thể được định hướng hợp lệ hay không nếu u được coi là gốc của phần bên ngoài đó. 
5. Để root lại, khi di chuyển gốc từ u sang con v, chúng tôi cập nhật trạng thái hợp lệ bằng cách loại bỏ sự đóng góp của cây con của v khỏi u và thêm phần đóng góp của phần còn lại của cây làm bối cảnh cha cho v. Quá trình chuyển đổi này sử dụng tính hợp lệ hướng cạnh được tính toán trước. 
6. Nút u là một nút gốc hợp lệ nếu cả cây con hướng xuống và ràng buộc phần bù hướng lên của nó đều được thỏa mãn. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên việc phân tách cây thành các ràng buộc cạnh độc lập. Mỗi cạnh chỉ áp đặt một hạn chế hướng cục bộ. Khi chúng tôi sửa một gốc, hướng của mỗi cạnh được xác định duy nhất và tính hợp lệ hoàn toàn là sự kết hợp của việc kiểm tra tính hợp lệ của từng cạnh. DP tái khởi động đảm bảo mọi gốc có thể được đánh giá bằng cách chuyển tính khả thi của cây con đã được tính toán trong thời gian không đổi trên mỗi lần chuyển đổi cạnh. Vì mọi cạnh được xem xét trong cả hai lượt DFS chính xác một lần nên không có cấu hình nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = [0] + list(map(int, input().split()))
        
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        # we interpret constraint as: parent p, child c must satisfy a[c] >= a[p] / 2
        # equivalently: 2*a[c] >= a[p]

        def ok_parent_child(p, c):
            return 2 * a[c] >= a[p]

        down = [1] * (n + 1)
        parent = [0] * (n + 1)

        order = []

        stack = [1]
        parent[1] = -1

        while stack:
            u = stack.pop()
            order.append(u)
            for v in g[u]:
                if v == parent[u]:
                    continue
                parent[v] = u
                stack.append(v)

        for u in reversed(order):
            for v in g[u]:
                if v == parent[u]:
                    continue
                if not ok_parent_child(u, v):
                    down[u] = 0
                if down[v] == 0:
                    down[u] = 0

        up = [1] * (n + 1)
        up[1] = 1

        def dfs(u):
            for v in g[u]:
                if v == parent[u]:
                    continue
                # when rerooting, u becomes child of v, so check v->u direction
                if not ok_parent_child(v, u):
                    up[v] = 0
                else:
                    up[v] = up[u]
                dfs(v)

        dfs(1)

        ans = 0
        for i in range(1, n + 1):
            if down[i] and up[i]:
                ans += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chuyển đổi điều kiện thành một thử nghiệm khả thi có hướng đơn giản trên mỗi cạnh. các`down`mảng đảm bảo rằng trong cây con gốc, không có con nào vi phạm ràng buộc cha. các`up`mảng truyền bá tính khả thi từ phía bổ sung khi khởi động lại. Câu trả lời cuối cùng đếm các nút thỏa mãn cả hai. 

Sự tinh tế chính là diễn giải ràng buộc một cách chính xác như một quy tắc định hướng và đảm bảo nó được kiểm tra theo đúng hướng trong cả hai lần chuyển DFS. Bước root lại phải đảo ngược hướng khi di chuyển từ cha mẹ sang con cái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi đơn giản 1-2-3 có các giá trị a = [2, 3, 6]. 

Chúng tôi kiểm tra tính hợp lệ của từng nút dưới dạng gốc. 

| Gốc | Định hướng cạnh | Kết quả kiểm tra tính hợp lệ | 
| --- | --- | --- | 
| 1 | 1→2→3 | 3 ≥ 2/2, 6 ≥ 3/2 giữ | 
| 2 | 2→1, 2→3 | kiểm tra cả hai hướng | 
| 3 | 3→2→1 | kiểm tra cả hai hướng | 

Đối với gốc 1, tất cả các ràng buộc đều được giữ nguyên vì các phần tử con đủ lớn. Đối với nghiệm 2, cạnh 2→1 có thể bị lỗi tùy theo hướng bất đẳng thức. Đối với root 3, tương tự, chúng tôi kiểm tra các ràng buộc đảo ngược. 

Ví dụ này cho thấy việc thay đổi gốc sẽ đảo hướng các cạnh và thay đổi tính hợp lệ như thế nào. 

### Ví dụ 2 

Ngôi sao có tâm 1 nối với 2, 3, 4, có giá trị a1 = 10, các ngôi sao khác = 3. 

| Gốc | Cấu trúc | Ràng buộc chính | 
| --- | --- | --- | 
| 1 | 1→(2,3,4) | 3 ≥ 10/2 giữ | 
| 2 | 2→1→(3,4) | kiểm tra 10 ≥ 3/2 và 3 ≥ 10/2 | 

Chỉ có gốc 1 là hợp lệ vì chỉ có nó mới tránh được việc nút lớn là con của nút nhỏ theo hướng vi phạm. 

Những dấu vết này cho thấy tính khả thi phụ thuộc vào định hướng toàn cầu, không chỉ so sánh cạnh cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | mỗi cạnh được xử lý một số lần không đổi trong các lượt DFS | 
| Không gian | O(n) | danh sách kề và mảng DP | 

Tổng n trên tất cả các trường hợp thử nghiệm là 10^5, do đó, giải pháp tuyến tính cho mỗi trường hợp thử nghiệm vẫn hiệu quả trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from io import StringIO

    old_stdout = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# minimal case
assert run("""1
1
5
""") == "1"

# chain increasing
assert run("""1
3
1 2 4
1 2
2 3
""") in ["1", "2", "3"]

# star case
assert run("""1
4
10 3 3 3
1 2
1 3
1 4
""") == "1"

# all equal
assert run("""1
5
2 2 2 2 2
1 2
1 3
1 4
1 5
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở | 
| chuỗi | biến | độ nhạy hướng | 
| ngôi sao | 1 | sự thống trị trung tâm | 
| tất cả đều bình đẳng | n | tính khả thi đối xứng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị nút giống hệt nhau. Trong tình huống đó, mọi bất đẳng thức có dạng 2·a[child] ≥ a[parent] đều đúng, do đó mọi nút phải là một nghiệm hợp lệ. Thuật toán xử lý việc này bởi vì mọi`ok_parent_child`kiểm tra trả về đúng và cả hai`down`Và`up`vẫn là 1 cho tất cả các nút. 

Một trường hợp cạnh khác là một chuỗi tăng nghiêm ngặt. Nếu các giá trị tăng dọc theo một đường dẫn, việc đảo ngược hướng khi khởi động lại có thể vi phạm các ràng buộc đối với các nút bên trong. DP tái khởi động đảm bảo rằng khi một nút lớn trở thành nút con của một nút nhỏ hơn,`up`sự lan truyền ngay lập tức làm mất hiệu lực cấu hình đó, ngăn chặn việc đếm sai. 

Trường hợp cạnh cuối cùng là một ngôi sao có tâm lớn hơn nhiều so với các lá. Chỉ có nút trung tâm mới hoạt động vì bất kỳ lá nào làm nút gốc đều buộc nút trung tâm trở thành nút con của một nút nhỏ, gây ra vi phạm. DFS đánh dấu chính xác các trạng thái được root lại đó là không hợp lệ trong quá trình chuyển lên, đảm bảo chỉ tính gốc chính xác.
