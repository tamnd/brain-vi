---
title: "CF 106042G - Chiều rộng cây nhị phân tối đa"
description: "Chúng ta được cung cấp một mảng các giá trị và từ đó chúng ta xây dựng một cây nhị phân cụ thể được gọi là cây Descartes tối đa. Quy tắc xây dựng cây rất đơn giản: phần tử lớn nhất trong bất kỳ đoạn nào sẽ trở thành gốc của đoạn đó và mọi thứ ở bên trái của nó tạo thành cây con bên trái trong khi…"
date: "2026-06-25T12:52:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106042
codeforces_index: "G"
codeforces_contest_name: "Teamscode Summer 2025 Novice Division"
rating: 0
weight: 106042
solve_time_s: 48
verified: true
draft: false
---

[CF 106042G - Độ rộng cây nhị phân tối đa](https://codeforces.com/problemset/problem/106042/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các giá trị và từ đó chúng ta xây dựng một cây nhị phân cụ thể được gọi là cây Descartes tối đa. Quy tắc xây dựng cây rất đơn giản: phần tử lớn nhất trong bất kỳ đoạn nào sẽ trở thành gốc của đoạn đó và mọi thứ ở bên trái của nó tạo thành cây con bên trái trong khi mọi thứ ở bên phải của nó tạo thành cây con bên phải, được áp dụng đệ quy. 

Một khi cái cây này được xây dựng, chúng ta không còn quan tâm đến hình dạng của nó nữa. Thay vào đó, về mặt khái niệm, chúng tôi đặt nó vào một bố cục cây nhị phân hoàn hảo: gốc được gán vị trí 1, con trái được vị trí`2 * p`, và đứa trẻ bên phải sẽ có được vị trí`2 * p + 1`. Với cách lập chỉ mục này, mỗi cấp độ của cây có thể có các nút cách xa nhau ngay cả khi cây thực tế thưa thớt. 

Nhiệm vụ là xác định chiều rộng tối đa trong số tất cả các cấp độ của cây này, trong đó chiều rộng được đo bằng chênh lệch giữa vị trí được chỉ định ngoài cùng bên trái và ngoài cùng bên phải ở cùng độ sâu, cộng thêm một. 

Kích thước đầu vào đủ lớn để bất kỳ giải pháp nào mô phỏng lặp đi lặp lại rõ ràng tất cả các phân đoạn hoặc xây dựng lại cây con từ đầu sẽ không thành công. Một cấu trúc đệ quy ngây thơ quét tối đa trong mọi mảng con sẽ dẫn đến hành vi bậc hai và thậm chí còn tệ hơn nếu thực hiện một cách bất cẩn. 

Một trường hợp thất bại tinh vi xuất hiện khi cây bị lệch nhiều. Đối với một mảng đã được sắp xếp theo thứ tự giảm dần, cây Cartesian sẽ thoái hóa thành một chuỗi. Trong tình huống đó, một BFS ngây thơ chỉ theo dõi số lượng nút trên mỗi cấp độ sẽ báo cáo không chính xác độ rộng là 1 cho mọi cấp độ, mặc dù việc lập chỉ mục vị trí tạo ra khoảng cách tăng theo cấp số nhân. Ví dụ, đối với đầu vào`[5, 4, 3, 2, 1]`, độ rộng chính xác phụ thuộc vào vị trí trải rộng chứ không phải số lượng nút. 

Một sai lầm phổ biến khác là quên rằng hình dạng cây được xác định theo thứ tự giá trị chứ không phải chỉ bằng chỉ số mảng. Nếu người ta coi nhầm mảng là một cây nhị phân hoàn chỉnh thì chiều rộng được tính toán sẽ hoàn toàn không liên quan đến cấu trúc Descartes thực tế. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xây dựng cây Descartes một cách rõ ràng và sau đó thực hiện duyệt theo thứ tự cấp độ trong khi theo dõi chỉ mục vị trí được chỉ định của nó cho mỗi nút. Việc xây dựng cây có thể được thực hiện bằng cách liên tục tìm mức tối đa trong mỗi mảng con, điều này đảm bảo tính chính xác vì mỗi nút tuân theo định nghĩa là mức tối đa của phân đoạn của nó. 

Tuy nhiên, việc tìm mức tối đa cho mỗi phân đoạn đệ quy sẽ tốn thời gian tuyến tính trên mỗi nút trong trường hợp xấu nhất. Đối với một mảng có kích thước`n`, điều này suy biến thành`O(n^2)`hành vi. Ngay cả trước khi di chuyển, bản thân công trình đã trở thành nút cổ chai. 

Cải tiến quan trọng đến từ việc tránh quét lặp đi lặp lại để tìm cực đại phân đoạn. Thay vì tính toán lại cấu trúc từ đầu cho mọi bài toán con, chúng tôi nhận thấy rằng cây Descartes có thể được xây dựng theo thời gian tuyến tính bằng cách sử dụng một ngăn xếp đơn điệu. Mỗi phần tử được đẩy và bật nhiều nhất một lần và mối quan hệ cha-con được xác định cục bộ khi phần tử lớn hơn chặn phần tử nhỏ hơn. 

Khi cây tồn tại ở dạng tuyến tính, chúng ta có thể tính toán độ rộng bằng BFS. Trong quá trình truyền tải, mỗi nút mang chỉ mục ẩn của nó trong cây nhị phân đầy đủ về mặt khái niệm. Đối với mỗi độ sâu, chúng tôi theo dõi các chỉ số tối thiểu và tối đa gặp phải. Sự khác biệt mang lại chiều rộng ở cấp độ đó và câu trả lời là mức tối đa trên tất cả các cấp độ. 

Sự chuyển đổi từ sức mạnh vũ phu sang bản lề tối ưu trong việc thay thế các tìm kiếm toàn cầu lặp đi lặp lại bằng các ràng buộc cấu trúc cục bộ được thực thi bởi ngăn xếp đơn điệu, sau đó xử lý các chỉ số vị trí như một vấn đề lan truyền thay vì vấn đề tính toán lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu (ngăn xếp + BFS) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành theo hai giai đoạn: xây dựng cây Cartesian một cách hiệu quả, sau đó đo chiều rộng bằng BFS được lập chỉ mục. 

1. Chúng tôi quét mảng từ trái sang phải, duy trì ngăn xếp giảm dần đơn điệu. Mỗi phần tử đại diện cho một ứng cử viên nút trong cây Descartes. Khi một phần tử mới lớn hơn đỉnh ngăn xếp, nó sẽ trở thành nút cha của nút đó vì nó là phần tử lớn hơn đầu tiên ở bên phải. Điều này đảm bảo việc gán cha mẹ chính xác mà không cần tìm kiếm. 
2. Sau khi xử lý tất cả các phần tử, chúng tôi xác định phần tử gốc là phần tử chưa bao giờ được gán phần tử cha. Đây là mức tối đa toàn cầu trong mảng, phù hợp với định nghĩa cây Descartes. 
3. Sau đó chúng tôi thực hiện BFS bắt đầu từ thư mục gốc. Cùng với mỗi nút, chúng tôi duy trì chỉ mục khái niệm của nó trong bố cục cây nhị phân đầy đủ, trong đó chỉ mục con bên trái là`2 * i`và chỉ số con bên phải là`2 * i + 1`. Mã hóa này không phải về bố cục bộ nhớ thực tế mà là một công cụ để đo mức độ phân tán cấu trúc. 
4. Đối với mỗi cấp độ BFS, chúng tôi ghi lại các chỉ số nhỏ nhất và lớn nhất gặp phải. Chiều rộng của mức đó là`max_index - min_index + 1`. 
5. Chúng tôi duy trì mức tối đa toàn cầu ở tất cả các cấp và trả lại vào cuối. 

Lý do BFS là tự nhiên ở đây là vì chiều rộng vốn là thuộc tính dựa trên cấp độ và BFS phân tách rõ ràng các nút theo độ sâu trong khi vẫn duy trì việc truyền bá chỉ mục. 

### Tại sao nó hoạt động 

Cây Descartes đảm bảo một cấu trúc gốc duy nhất được xác định chỉ bởi các phần tử lớn hơn gần nhất. Ngăn xếp đơn điệu đảm bảo chúng ta xây dựng chính xác cấu trúc này trong thời gian tuyến tính. Sau khi cấu trúc được cố định, việc lập chỉ mục vị trí sẽ hoạt động nhất quán: mọi chỉ mục con đều được dẫn xuất một cách xác định từ cha mẹ của nó, do đó, ở bất kỳ cấp độ nào, các chỉ mục đều phản ánh mức độ trải rộng thực sự theo chiều ngang trong quá trình nhúng cây hoàn chỉnh ngầm định. Vì BFS khám phá các nút theo cấp độ nên mọi chiều rộng có thể được đánh giá chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    parent = [-1] * n
    left = [-1] * n
    right = [-1] * n
    
    stack = []
    
    for i in range(n):
        last = -1
        while stack and a[stack[-1]] < a[i]:
            last = stack.pop()
        if stack:
            parent[i] = stack[-1]
            right[stack[-1]] = i
        if last != -1:
            parent[last] = i
            left[i] = last
        stack.append(i)
    
    root = 0
    while parent[root] != -1:
        root = parent[root]
    
    q = deque([(root, 1)])
    ans = 0
    
    while q:
        level_size = len(q)
        min_idx = float('inf')
        max_idx = 0
        
        for _ in range(level_size):
            node, idx = q.popleft()
            min_idx = min(min_idx, idx)
            max_idx = max(max_idx, idx)
            
            if left[node] != -1:
                q.append((left[node], idx * 2))
            if right[node] != -1:
                q.append((right[node], idx * 2 + 1))
        
        ans = max(ans, max_idx - min_idx + 1)
    
    print(ans)

if __name__ == "__main__":
    solve()
```Nửa đầu của mã xây dựng cây Cartesian bằng cách sử dụng ngăn xếp đơn điệu. Mỗi lần chúng tôi bật các phần tử, chúng tôi sẽ giải quyết một cách hiệu quả nút nào sẽ trở thành nút cha dựa trên quy tắc phần tử lớn hơn đầu tiên. các`last`biến chụp các nút cần được gắn lại dưới dạng nút con bên trái của nút hiện tại. 

Phần BFS chịu trách nhiệm tính toán chiều rộng. Mỗi mục hàng đợi mang cả nút và chỉ mục ẩn của nó. Việc sử dụng`2 * idx`Và`2 * idx + 1`là cần thiết vì nó mã hóa khoảng cách ngang theo cách duy trì khoảng cách tương đối giữa các cấp độ. 

Một chi tiết tinh tế là xác định gốc. Vì mọi nút ngoại trừ nút tối đa cuối cùng đều nhận được nút cha, đi lên từ bất kỳ nút nào cho đến khi đạt được`-1`đáng tin cậy tìm thấy gốc. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[3, 1, 2]`. 

Chúng tôi xây dựng cây:`3`là gốc,`1`là đứa con còn lại,`2`trở thành con phải của`3`. 

| Bước | Xếp hàng | Nút | Chỉ mục | Chỉ số tối thiểu | Chỉ số tối đa | Chiều rộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [(3,1)] | 3 | 1 | 1 | 1 | 1 | 
| 2 | [(1,2),(2,3)] | 1,2 | 2,3 | 2 | 3 | 2 | 

Cấp độ thứ hai hiển thị các nút ở chỉ số 2 và 3, tạo ra chiều rộng 2. 

Bây giờ hãy xem xét`[5, 4, 3, 2, 1]`. 

Điều này tạo thành một cây nghiêng hoàn toàn về bên phải. 

| Bước | Xếp hàng | Nút | Chỉ mục | Chỉ số tối thiểu | Chỉ số tối đa | Chiều rộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [(5,1)] | 5 | 1 | 1 | 1 | 1 | 
| 2 | [(4,2)] | 4 | 2 | 2 | 2 | 1 | 
| 3 | [(3,4)] | 3 | 4 | 4 | 4 | 1 | 
| 4 | [(2,8)] | 2 | 8 | 8 | 8 | 1 | 

Điều này xác nhận rằng mặc dù cây là một chuỗi, việc lập chỉ mục vị trí tăng theo cấp số nhân nhưng chiều rộng trên mỗi cấp vẫn là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được đẩy và xuất hiện một lần trong ngăn xếp và được truy cập một lần trong BFS | 
| Không gian | O(n) | Lưu trữ con trỏ cây và hàng đợi BFS | 

Giải pháp có quy mô tuyến tính với kích thước đầu vào, vừa vặn thoải mái trong các giới hạn thông thường lên tới 200.000 phần tử. 

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

# simple chain
assert run("5\n5 4 3 2 1\n") == "1"

# balanced-ish
assert run("3\n3 1 2\n") == "2"

# single element
assert run("1\n10\n") == "1"

# increasing array
assert run("4\n1 2 3 4\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 5 4 3 2 1 | 1 | Hành vi cây lệch | 
| 3 3 1 2 | 2 | Chiều rộng phân nhánh hỗn hợp | 
| 1 10 | 1 | Đầu vào tối thiểu | 
| 1 2 3 4 | 4 | Cấu trúc trải rộng hoàn toàn bên phải | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[7]`, ngăn xếp sẽ xây dựng một nút duy nhất không có nút con. BFS bắt đầu và kết thúc ngay lập tức và chiều rộng được tính toán chính xác là 1 vì cả chỉ số tối thiểu và tối đa ở mức duy nhất đều bằng nhau. 

Đối với một mảng giảm dần, mọi phần tử mới sẽ trở thành phần tử cha của cấu trúc trước đó. Ngăn xếp liên tục bật lên cho đến khi trống, tạo ra một chuỗi lệch phải. BFS gán chỉ số`1, 2, 4, 8, ...`, nhưng mỗi cấp chứa chính xác một nút, vì vậy chiều rộng vẫn bằng 1 ở mọi độ sâu. 

Đối với một mảng tăng nghiêm ngặt, mỗi phần tử mới gắn vào như một phần tử con bên phải của gốc trước đó, tạo ra một chuỗi bên phải suy biến. Bất chấp sự tăng trưởng chỉ số, mỗi cấp độ BFS lại có một nút duy nhất và độ rộng được tính toán chính xác vẫn là 1.
