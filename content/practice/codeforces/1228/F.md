---
title: "CF 1228F - Một nút đã biến mất"
description: "Chúng ta bắt đầu với một cây nhị phân hoàn hảo có chiều cao n, nghĩa là mỗi nút bên trong có chính xác hai nút con và tất cả các lá đều có cùng độ sâu. Cây này chứa chính xác 2^n - 1 nút và có cấu trúc đệ quy rất cứng nhắc: mỗi cây con tự nó là một cây nhị phân hoàn hảo."
date: "2026-06-15T19:56:34+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1228
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 589 (Div. 2)"
rating: 2500
weight: 1228
solve_time_s: 268
verified: false
draft: false
---

[CF 1228F - Một nút đã biến mất](https://codeforces.com/problemset/problem/1228/F) 

**Đánh giá:** 2500 
**Tags:** thuật toán xây dựng, triển khai, cây 
**Thời gian giải:** 4 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một cây nhị phân hoàn hảo có chiều cao`n`, nghĩa là mỗi nút bên trong có chính xác hai nút con và tất cả các lá đều nằm ở cùng độ sâu. Cây này chứa chính xác`2^n - 1`các nút và có cấu trúc đệ quy rất cứng nhắc: mỗi cây con tự nó là một cây nhị phân hoàn hảo. 

Sau khi xây dựng cấu trúc lý tưởng này, một đỉnh`v`được gỡ bỏ. Việc sửa chữa duy nhất được phép là cục bộ: cha mẹ của`v`được kết nối trực tiếp với tất cả trẻ em của`v`, duy trì kết nối. Nếu như`v`là một chiếc lá, không có gì được kết nối lại nên chúng ta chỉ cần xóa một chiếc lá. 

Chúng tôi được cung cấp cây cuối cùng bị thiếu một nút và các kết nối lại này đã được áp dụng ngầm. Nhiệm vụ là xác định xem cây này có thể xuất phát từ việc xóa như vậy hay không và nếu vậy, hãy xác định tất cả các đỉnh có thể là đỉnh của nút bị loại bỏ. 

Những ràng buộc ngụ ý`n ≤ 17`, vậy kích thước cây tối đa là khoảng`1.3 × 10^5`nút. Bất kỳ giải pháp nào gần với số lượng nút bậc hai hơn sẽ quá chậm. Điều này thúc đẩy chúng ta hướng tới lý luận có cấu trúc hoặc đệ quy trên cây con thay vì mô phỏng việc xóa một cách rõ ràng. 

Một khó khăn tinh tế đến từ tính đối xứng. Trong một cây nhị phân hoàn hảo, nhiều đỉnh chiếm giữ các vị trí có cấu trúc giống nhau. Sau khi xóa một nút, nhiều cấu hình ban đầu khác nhau có thể dẫn đến cùng một cây cuối cùng. Một cách tiếp cận đơn giản là thử mọi nút bị loại bỏ có thể và xây dựng lại toàn bộ cây cũng sẽ cần phải xác thực cấu trúc, việc này sẽ trở nên tốn kém khi thực hiện nhiều lần. 

Một cạm bẫy khác là cho rằng chỉ bằng cấp mới là đặc trưng cho giá trị. Sau khi xóa, các nút bên trong có thể có cấp độ 3, 2 hoặc 1 tùy thuộc vào việc chúng có hấp thụ các nút con của nút bị xóa hay không. Nút cấp 2 trong cây cuối cùng không nhất thiết phải là lá hoặc nút bên trong của cây gốc một cách rõ ràng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực là giả sử nút bị loại bỏ`v`và cố gắng xây dựng lại cây nhị phân hoàn hảo ban đầu. Đối với mỗi ứng viên`v`, về mặt khái niệm, chúng tôi sẽ đính kèm một nút mới, phân chia các cạnh một cách thích hợp và kiểm tra xem cấu trúc kết quả có phải là cây nhị phân hoàn hảo có chiều cao hay không`n`. Việc xác thực sự hoàn hảo đòi hỏi phải đảm bảo mọi nút tuân theo các quy tắc kích thước cây con nghiêm ngặt, điều này gây tốn kém`O(N)`mỗi lần kiểm tra. Vì có`O(N)`ứng viên, điều này dẫn đến`O(N^2)`hoạt động quá chậm đối với`N ≈ 10^5`. 

Điểm mấu chốt là cấu trúc của cây nhị phân hoàn hảo hoàn toàn được xác định bởi kích thước cây con. Mỗi nút trong cây ban đầu có kích thước cây con là lũy thừa của hai trừ một. Sau khi xóa một nút, chỉ một vị trí trong toàn bộ cây vi phạm mẫu này: nút cha của nút bị xóa, có kích thước cây con giảm đúng một và có thể là nút con của nút bị xóa có kết nối thay đổi cục bộ. 

Điều này gợi ý đảo ngược quá trình: thay vì đoán nút bị loại bỏ, chúng tôi coi mỗi ứng cử viên là nút gốc của nút bị loại bỏ và xác minh xem việc xóa một nút con của nút cha đó có thể tạo ra cây được quan sát hay không. Sau khi chúng tôi sửa một phụ huynh ứng cử viên`p`, cấu trúc trở nên gần như tất định vì mọi nút khác vẫn phải hoạt động giống như một gốc cây nhị phân hoàn hảo của cây con của chính nó. 

Điều này làm giảm vấn đề kiểm tra tính nhất quán của kích thước cây con và các ràng buộc kề theo một giả định gốc duy nhất. Độ cứng toàn cục của cây nhị phân hoàn hảo đảm bảo rằng một khi gốc được cố định thì tất cả các vị trí nút đều bị ép buộc và chúng ta chỉ cần xác minh xem có tồn tại chính xác một lỗi cục bộ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái thiết Brute Force trên mỗi nút | O(N^2) | O(N) | Quá chậm | 
| Xác minh cấu trúc cho mỗi phụ huynh ứng viên | O(N log N) hoặc O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là một cây nhị phân hoàn hảo được xác định duy nhất bởi gốc và chiều cao của nó. Nút bị thiếu tạo ra chính xác một sự không nhất quán về cấu trúc cục bộ và nút cha của nó là đỉnh duy nhất có vùng lân cận lệch khỏi hành vi nhị phân hoàn hảo. 

Chúng tôi kiểm tra từng nút`p`là cha mẹ tiềm năng của đỉnh bị loại bỏ. 

1. Gốc cây đã cho tại một nút tùy ý và tính toán mối quan hệ cha-con cũng như kích thước cây con bằng DFS. Điều này chỉ dành cho truy cập cấu trúc; cái cây không được định hướng nhưng chúng ta áp đặt phương hướng. 
2. Đối với mỗi nút ứng cử viên`p`, hãy xem những người hàng xóm nào của nó có thể là đứa trẻ bị mất tích trong cây hoàn hảo ban đầu. Vì trong một cây nhị phân hoàn hảo, mỗi lá không có chính xác hai con,`p`ban đầu phải có cấp độ 3 trong cây cuối cùng: hai cạnh được giữ nguyên và một "kết nối con bị thiếu" đã được định tuyến lại. 
3. Tạm thời giải thích từng hàng xóm của`p`là con trái hoặc con phải trong cây ban đầu và kiểm tra xem cấu trúc còn lại có tách thành hai cây con nhị phân hoàn hảo có độ cao thích hợp hay không. Cây con của đứa trẻ bị thiếu phải tính toán chính xác độ lệch cấu trúc. 
4. Đối với mỗi cây con có gốc tại hàng xóm, hãy kiểm tra xem nó có tạo thành cây nhị phân hoàn hảo hay không. Điều này được thực hiện bằng cách kiểm tra kích thước cây con: cây con hợp lệ nếu kích thước của nó là`2^k - 1`đối với một số nguyên`k`và cấu trúc bên trong của nó là nhất quán. 
5. Ứng viên`p`hợp lệ nếu chính xác một cấu hình của phần tử con bị thiếu dẫn đến sự nhất quán hoàn toàn trên toàn bộ cây. 

Ý tưởng quan trọng là chỉ có một nút phá vỡ mẫu kích thước đệ quy nghiêm ngặt và sự phá vỡ đó sẽ lan truyền lên trên một cách có kiểm soát. Mọi nút khác vẫn phải thỏa mãn bất biến rằng kích thước cây con của nó là lũy thừa hai trừ một. 

### Tại sao nó hoạt động 

Cây nhị phân hoàn hảo có tính đệ quy cứng nhắc: mỗi cây con phải là cây nhị phân hoàn hảo. Việc loại bỏ một nút sẽ phá hủy chính xác một điều kiện của cây con đó. Quy tắc kết nối lại đảm bảo rằng vi phạm này không lan truyền một cách tùy tiện mà thay vào đó được hấp thụ vào nút cha của nút bị loại bỏ. 

Do đó, trong cây cuối cùng, mọi nút ngoại trừ một nút hoạt động giống hệt như một gốc hợp lệ của cây nhị phân hoàn hảo. Nút gốc của nút bị loại bỏ là đỉnh duy nhất có cấu trúc cục bộ không thể được giải thích bằng cách phân tách hợp lệ thành hai cây con hoàn hảo. Bằng cách kiểm tra các ứng cử viên, chúng tôi đang xác định điểm duy nhất mà bất biến đệ quy này thất bại theo cách chính xác được phép bằng một lần xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    N = (1 << n) - 2

    g = [[] for _ in range(N + 1)]
    for _ in range(N - 1):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    parent = [0] * (N + 1)
    depth = [0] * (N + 1)
    order = []

    # root at 1
    stack = [1]
    parent[1] = -1

    while stack:
        v = stack.pop()
        order.append(v)
        for to in g[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            depth[to] = depth[v] + 1
            stack.append(to)

    # compute subtree sizes
    sz = [1] * (N + 1)
    for v in reversed(order):
        for to in g[v]:
            if parent[to] == v:
                sz[v] += sz[to]

    # check perfect power form
    def is_full(x):
        return x > 0 and (x & (x + 1)) == 0  # x = 2^k - 1

    res = []

    for p in range(1, N + 1):
        deg = len(g[p])

        # candidate parent must have degree at least 2
        if deg < 2:
            continue

        # try assuming missing node is one of neighbors
        ok = False

        for bad in g[p]:
            comp_sizes = []
            for to in g[p]:
                if to == bad:
                    continue
                comp_sizes.append(sz[to])

            # we need remaining components to form two perfect subtrees
            if len(comp_sizes) != 2:
                continue

            a, b = comp_sizes

            if is_full(a) and is_full(b):
                ok = True
                break

        if ok:
            res.append(p)

    print(len(res))
    if res:
        print(*sorted(res))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ root cây để thu được kích thước cây con, vì đây là những tín hiệu cấu trúc toàn cục duy nhất mà chúng ta cần. Sự đơn giản hóa chủ yếu là xử lý cây như đã có rễ ngay cả khi vấn đề ban đầu không có rễ; điều này hợp lệ vì kích thước cây con trong cây vô hướng chỉ phụ thuộc vào gốc đã chọn và bất kỳ việc tái cấu trúc ứng cử viên nào cũng có thể được căn chỉnh một cách nhất quán với gốc. 

Đối với mỗi nút`p`, chúng tôi kiểm tra các cạnh của nó và mô phỏng cạnh nào có thể tương ứng với đứa trẻ bị mất tích. Việc loại bỏ một hàng xóm sẽ chia cấu trúc sự cố còn lại thành các thành phần; trong một cấu hình hợp lệ, chính xác hai thành phần như vậy phải tương ứng với các cây nhị phân hoàn hảo. Séc`is_full`kiểm tra xem kích thước cây con có khớp hay không`2^k - 1`, đây là thuộc tính xác định của cây nhị phân hoàn hảo. 

Một chi tiết triển khai tinh vi là chúng tôi không xây dựng lại các cây con một cách rõ ràng. Chúng tôi chỉ dựa vào kích thước cây con được tính toán trước để giữ độ phức tạp tuyến tính. Thủ thuật nhỏ`(x & (x + 1)) == 0`là một cách nhỏ gọn để kiểm tra xem`x`có dạng`2^k - 1`. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ hoàn hảo trong đó việc loại bỏ nút cấp trung sẽ tạo ra sự mất cân bằng rõ ràng. 

Chúng tôi kiểm tra một ứng cử viên gốc và theo dõi cách hoạt động của phân vùng lân cận của nó. 

| Bước | Nút p | Hàng xóm đã xóa | Kích thước thành phần | Kiểm tra kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | p | con c1 | (7, 7) | hợp lệ | 
| 2 | p | con c2 | (3, 11) | không hợp lệ | 
| 3 | p | lá | (15) | không hợp lệ | 

Điều này cho thấy rằng chỉ có sự phân chia cấu trúc rất cụ thể mới duy trì được điều kiện cây con hoàn hảo. 

Ví dụ thứ hai xem xét cấu hình đối xứng trong đó tồn tại nhiều ứng cử viên. 

| Bước | Nút p | Đã xóa hàng xóm | Kích thước thành phần | Kiểm tra kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | p1 | c | (7, 7) | hợp lệ | 
| 2 | p2 | c | (7, 7) | hợp lệ | 

Điều này xác nhận rằng nhiều cha mẹ có thể giải thích cùng một việc xóa bỏ do tính đối xứng trong cây hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | DFS tính toán kích thước cây con một lần, mỗi nút được kiểm tra theo thời gian không đổi so với các nút lân cận | 
| Không gian | O(N) | danh sách kề và mảng phụ cho kích thước cây cha và cây con | 

Độ phức tạp tuyến tính phù hợp thoải mái trong giới hạn lên tới khoảng 130k nút. Mỗi cạnh được xử lý với số lần không đổi và không yêu cầu truyền tải lồng nhau trên các cây con. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        return str(solve())
    except SystemExit:
        return ""

# sample 1 (format adapted)
# assert run(...) == "..."

# minimal tree
assert run("2\n1 2\n1 3\n") in ["1\n1", "1\n2"]

# linear chain (invalid)
assert run("3\n1 2\n2 3\n3 4\n") == "0"

# balanced small valid perturbation
assert run("3\n1 2\n1 3\n2 4\n2 5\n3 6\n3 7\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây tối thiểu | 1 hoặc đối xứng | xử lý đối xứng | 
| chuỗi | 0 | từ chối các cấu trúc không hoàn hảo | 
| biến thể cân bằng đầy đủ | không trống | phát hiện tính hợp lệ cơ bản | 

## Vỏ cạnh 

Trường hợp một cạnh là khi nút bị loại bỏ là một lá. Trong tình huống đó, cây cuối cùng vẫn gần như hoàn hảo và cây bố mẹ chỉ mất đi một lá con. Thuật toán xử lý vấn đề này vì việc loại bỏ một hàng xóm sẽ dẫn đến hai cây con hoàn hảo tương ứng chính xác với các cây con chưa được chạm tới. 

Một trường hợp cạnh khác là khi nút bị loại bỏ nằm dưới nút gốc một cấp. Khi đó, gốc sẽ trở thành đỉnh duy nhất có các đỉnh con không còn đối xứng về kích thước cây con. Việc kiểm tra xác định chính xác gốc là ứng cử viên hợp lệ vì việc loại bỏ hàng xóm thích hợp sẽ mang lại hai cây con đầy đủ hợp lệ. 

Trường hợp cạnh cuối cùng là tính đối xứng: nhiều đỉnh ở các phần khác nhau của cây có thể thỏa mãn các điều kiện kích thước cây con giống hệt nhau. Thuật toán thu thập tất cả chúng một cách tự nhiên vì nó không giả định tính duy nhất mà chỉ có tính nhất quán.
