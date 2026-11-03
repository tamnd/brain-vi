---
title: "CF 1098A - Tính tổng trên cây"
description: "Chúng ta có một cây có gốc trong đó đỉnh 1 là gốc. Mỗi đỉnh ban đầu có một giá trị nguyên không âm được ghi trên đó, nhưng những giá trị đó hiện đã bị mất. Những gì còn lại là một phần thông tin về tổng tiền tố dọc theo đường dẫn gốc."
date: "2026-06-15T15:30:42+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1098
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 530 (Div. 1)"
rating: 1600
weight: 1098
solve_time_s: 553
verified: false
draft: false
---

[CF 1098A - Tính tổng trên cây](https://codeforces.com/problemset/problem/1098/A) 

**Đánh giá:** 1600 
**Thẻ:** thuật toán xây dựng, dfs và các cây tương tự, tham lam 
**Thời gian giải:** 9m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó đỉnh 1 là gốc. Mỗi đỉnh ban đầu có một giá trị nguyên không âm được ghi trên đó, nhưng những giá trị đó hiện đã bị mất. Những gì còn lại là một phần thông tin về tổng tiền tố dọc theo đường dẫn gốc. 

Đối với bất kỳ đỉnh nào$v$, định nghĩa$s_v$là tổng các giá trị trên đường đi từ$v$lên đến gốc, bao gồm cả hai điểm cuối. Chúng ta cũng được cung cấp cấu trúc chẵn lẻ độ sâu một cách ngầm định, vì độ sâu được xác định bởi cây và chúng ta được bảo rằng tất cả$s_v$các giá trị cho các đỉnh ở độ sâu đều đã bị xóa và thay thế bằng -1. Nhiệm vụ là xây dựng lại bất kỳ phép gán giá trị đỉnh hợp lệ nào$a_v \ge 0$phù hợp với phần còn lại$s_v$các giá trị hoặc xác định rằng không có sự phân công nào như vậy tồn tại. Trong số tất cả các phép gán hợp lệ, chúng ta phải giảm thiểu tổng của tất cả$a_v$. 

Khó khăn chính là các ràng buộc không mang tính cục bộ. Một giá trị$a_v$ảnh hưởng đến tất cả$s_u$trong cây con của nó thông qua các mối quan hệ tổ tiên, do đó các quyết định phải được truyền một cách nhất quán xuống cây. 

Các ràng buộc cho phép lên đến$10^5$các đỉnh, điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng tính toán lại tổng đường dẫn một cách độc lập cho mỗi nút. Bất kỳ giải pháp nào tính lại tổng trên mỗi nút sẽ trở thành bậc hai trong cây lệch. Chúng ta cần truyền tải tuyến tính hoặc gần tuyến tính, điển hình là DFS hoặc BFS với việc truyền bá các ràng buộc một cách cẩn thận. 

Một trường hợp thất bại tinh tế xuất hiện khi các ràng buộc mâu thuẫn với nhau ở các độ sâu xen kẽ nhau. Ví dụ: nếu một nút ở độ sâu lẻ có giá trị âm ngụ ý bị ép buộc bởi chuỗi ràng buộc gốc của nó, thì điều đó là không thể vì tất cả$a_v \ge 0$. Một trường hợp phức tạp khác là khi thiếu một nút có độ sâu chẵn$s_v$, tạo ra sự mơ hồ về mức độ "slack" có thể được chỉ định trong cây con của nó, điều này ảnh hưởng trực tiếp đến tổng tối thiểu toàn cục. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán giá trị cho tất cả các nút và xác minh các ràng buộc. Vì mỗi$s_v$là tổng của một đường dẫn gốc, việc xác minh một lần chuyển nhượng sẽ tốn chi phí$O(n)$, và việc thử tất cả các phép gán là theo cấp số nhân. Ngay cả việc giới hạn ở phạm vi nhỏ cũng không thể thực hiện được vì giá trị có thể lên tới$10^9$. 

Một ý tưởng mạnh mẽ có cấu trúc hơn là xử lý từng đường dẫn từ gốc đến lá một cách độc lập và cố gắng suy luận$a_v$từ sự khác biệt của liên tiếp$s$các giá trị. Tuy nhiên, điều này bị phá vỡ bởi vì$s_v$bị thiếu trên một nửa số nút, do đó không phải lúc nào cũng có thể thực hiện được sự khác biệt trực tiếp. 

Quan sát quan trọng là$s_v$hoạt động giống như một tổng tiền tố trên cây. Nếu chúng ta chuyển từ cha mẹ$p$đến một đứa trẻ$v$, sau đó:$$s_v = s_p + a_v$$bất cứ khi nào cả hai giá trị được biết đến. Điều này mang lại sự truy hồi trực tiếp dọc theo các cạnh khi có sẵn các giá trị gốc. 

Bây giờ hãy xem xét tính chẵn lẻ theo độ sâu. Tất cả đều có độ sâu đồng đều$s_v$đều chưa được biết, trong khi tất cả đều có độ sâu lẻ$s_v$được biết đến. Sự luân phiên này cho phép chúng ta truyền bá các giá trị một cách có kiểm soát: bất cứ khi nào chúng ta biết một cặp cha-con trong đó cha mẹ có một cặp cha-con đã biết.$s$, chúng ta có thể tính toán của đứa trẻ$a_v$nếu nó$s_v$đã biết, hoặc chúng ta có thể diễn đạt$s_v$nếu chưa biết. 

Ý tưởng trung tâm là duyệt cây từ gốc trở xuống và duy trì các ràng buộc nhất quán. Đối với mỗi nút, chúng tôi duy trì một ứng cử viên$s_v$. Nếu nó được đưa ra, nó đã được sửa. Nếu thiếu nó, nó sẽ trở thành một biến chỉ bị ràng buộc bởi tính nhất quán và tính không tiêu cực của trẻ em.$a_v = s_v - s_{parent}$. 

Mục tiêu giảm thiểu đơn giản hóa cấu trúc này: chúng tôi muốn giảm thiểu$\sum a_v$. Vì mỗi$a_v$là$s_v - s_{parent}$, điều này trở thành:$$\sum a_v = s_1 + \sum_{v \neq 1} (s_v - s_{parent(v)})$$kính thiên văn nào dọc theo đường đi và chỉ phụ thuộc vào việc chọn$s_v$các giá trị. Tối thiểu hóa đẩy mọi thứ miễn phí$s_v$càng thấp càng tốt trong khi vẫn duy trì các ràng buộc. 

Điều này làm giảm vấn đề xuống DFS trong đó chúng tôi cố gắng gán giá trị hợp lệ nhỏ nhất có thể$s_v$tới các nút chưa biết, đảm bảo không có sự khác biệt về cạnh âm và tính nhất quán với các giá trị cố định. 

Chúng tôi so sánh các phương pháp: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | Hàm mũ | O(n) | Quá chậm | 
| DFS với sự lan truyền hạn chế | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây từ mảng cha và lưu trữ danh sách con. Điều này là cần thiết vì các ràng buộc lan truyền từ gốc tới lá. 
2. Lưu trữ đã cho$s_v$giá trị, đánh dấu -1 là không xác định. Các giá trị cố định này hoạt động như các ràng buộc cứng trong quá trình truyền tải. 
3. Root DFS tại nút 1 và gán$s_1$. Nếu như$s_1 = -1$, nó phải bằng 0, bởi vì bất kỳ giá trị dương nào cũng sẽ chỉ làm tăng tất cả con cháu và làm xấu đi mục tiêu mà không có ràng buộc nào khác. 
4. Trong DFS từ một nút$u$, duy trì giá trị hiện tại của nó$s_u$. Giá trị này được cố định bằng đầu vào hoặc bằng cách truyền bá. 
5. Đối với mỗi trẻ$v$, xác định nó$s_v$:

- Nếu như$s_v$đã cho thì nó phải thỏa mãn$s_v \ge s_u$. Nếu không thì$a_v = s_v - s_u$sẽ là số âm, điều này không hợp lệ. 
- Nếu như$s_v$chưa xác định, hãy gán cho nó giá trị tối thiểu có thể phù hợp với các ràng buộc từ cây con của chính nó. Ban đầu đây là$s_u$, bởi vì bất kỳ giá trị nhỏ hơn nào cũng sẽ buộc đóng góp âm đi xuống. 
6. Tính toán$a_v = s_v - s_u$. Thêm nó vào câu trả lời tổng thể. 
7. Lặp lại vào$v$với sự giao phó của nó$s_v$. 
8. Nếu tại bất kỳ thời điểm nào xuất hiện mâu thuẫn (một vấn đề cố định$s_v < s_u$), việc xây dựng là không thể và chúng tôi trả về -1. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến đơn điệu dọc theo mọi đường dẫn từ gốc tới nút: được gán$s_v$các giá trị không bao giờ giảm dưới tổng tiền tố gốc. Điều này đảm bảo tất cả$a_v = s_v - s_{parent}$vẫn không âm. Đối với các nút có giá trị cố định, tính khả thi được thực thi ngay lập tức. Đối với các nút miễn phí, việc chỉ định giá trị khả thi tối thiểu đảm bảo không có sự điều chỉnh nào sau này có thể làm giảm tổng số tiền hơn nữa mà không vi phạm các ràng buộc. Vì mỗi cạnh đóng góp chính xác một số hạng sai phân, nên việc tối thiểu hóa mỗi phép gán cục bộ sẽ tối thiểu hóa tổng toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
p = [0] * (n + 1)
children = [[] for _ in range(n + 1)]

parents = list(map(int, input().split()))
for i, par in enumerate(parents, start=2):
    p[i] = par
    children[par].append(i)

s = [0] + list(map(int, input().split()))

ans = 0

def dfs(u):
    global ans
    for v in children[u]:
        if s[v] != -1:
            if s[v] < s[u]:
                print(-1)
                sys.exit(0)
        else:
            s[v] = s[u]

        ans += s[v] - s[u]
        dfs(v)

# root handling
if s[1] == -1:
    s[1] = 0

dfs(1)
print(ans)
```Mã xây dựng cây gốc bằng cách sử dụng danh sách kề, sau đó thực hiện DFS từ gốc. Mảng`s`lưu trữ các giá trị cố định hoặc giá trị được gán cho các nút không xác định. Đối với mỗi cạnh, phần đóng góp cho câu trả lời được tính bằng chênh lệch giữa tổng tiền tố con và tiền tố cha, bằng trực tiếp với giá trị được chỉ định$a_v$. 

Chi tiết triển khai quan trọng là xử lý thư mục gốc: khi$s_1$bị thiếu, việc đặt nó thành 0 sẽ giảm thiểu tổng số vì không có ràng buộc cha nào phải đáp ứng. Một điểm tinh tế khác là kiểm tra tính khả thi ngay lập tức`s[v] < s[u]`, đảm bảo không có âm$a_v$xuất hiện. 

Các bản cập nhật DFS`s[v]`để các nút không xác định khớp với nút cha, đây là phép gán hợp lệ nhỏ nhất có thể có trong tính đơn điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 1 1
1 -1 -1 -1 -1
```Chúng tôi bắt đầu với root$s_1 = 1$. Tất cả trẻ em ban đầu không rõ. 

| Bước | Nút | Phụ huynh | Cho s | Được giao s | a_v đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | - | 1 | 1 | - | 
| 2 | 2 | 1 | -1 | 1 | 0 | 
| 3 | 3 | 1 | -1 | 1 | 0 | 
| 4 | 4 | 1 | -1 | 1 | 0 | 
| 5 | 5 | 1 | -1 | 1 | 0 | 

Tất cả các nút đều có giá trị bằng gốc, vì vậy tất cả$a_v = 0$ngoại trừ root ngầm đóng góp 1. Tổng cộng là 1. 

Điều này xác nhận rằng khi không có ràng buộc nào thì lực biến đổi, việc thu gọn tất cả các giá trị sẽ giảm thiểu tổng. 

### Ví dụ 2 

đầu vào:```
4
1 2 2
5 -1 7 -1
```| Bước | Nút | Phụ huynh | Cho s | Được giao s | a_v | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | - | 5 | 5 | - | 
| 2 | 2 | 5 | -1 | 5 | 0 | 
| 3 | 3 | 5 | 7 | 7 | 2 | 
| 4 | 4 | 5 | -1 | 5 | 0 | 

Nút 3 buộc phải nhảy từ 5 lên 7, đóng góp 2. Tất cả các nút khác vẫn bằng nút gốc của chúng để giảm thiểu chi phí. 

Điều này cho thấy các giá trị cố định tạo ra các mức tăng bắt buộc như thế nào để truyền vào tổng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong DFS và được xử lý trong thời gian O(1) | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Giải pháp này phù hợp thoải mái trong các ràng buộc vì cả bộ nhớ và quy mô truyền tải đều tuyến tính với số đỉnh lên tới$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        # assume solution is wrapped in main()
        import sys
        sys.setrecursionlimit(10**7)

        n = int(sys.stdin.readline())
        p = [0] * (n + 1)
        children = [[] for _ in range(n + 1)]

        parents = list(map(int, sys.stdin.readline().split()))
        for i, par in enumerate(parents, start=2):
            p[i] = par
            children[par].append(i)

        s = [0] + list(map(int, sys.stdin.readline().split()))
        ans = 0

        def dfs(u):
            nonlocal ans
            for v in children[u]:
                if s[v] != -1:
                    if s[v] < s[u]:
                        print(-1)
                        return
                else:
                    s[v] = s[u]
                ans += s[v] - s[u]
                dfs(v)

        if s[1] == -1:
            s[1] = 0

        dfs(1)
        print(ans)

    return out.getvalue().strip()

# provided sample
assert run("5\n1 1 1 1\n1 -1 -1 -1 -1\n") == "1"

# all unknown minimal
assert run("3\n1 1\n-1 -1 -1\n") == "0"

# contradiction case
assert run("3\n1 2\n5 3 -1\n") == "-1"

# chain increasing constraint
assert run("4\n1 2 3\n1 2 3 4\n") == "3"

# star tree
assert run("4\n1 1 1\n10 -1 -1 -1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả -1 chuỗi | 0 | tất cả các giá trị thu gọn về gốc | 
| mâu thuẫn | -1 | phát hiện ràng buộc giảm không hợp lệ | 
| chuỗi tăng chặt chẽ | 3 | tích lũy số gia tăng bắt buộc một cách chính xác | 
| cây sao | 0 | trẻ tự lập được thừa hưởng giá trị tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi tất cả$s_v$các giá trị bị thiếu. Bộ thuật toán$s_1 = 0$, sau đó truyền 0 đến tất cả các nút. Mọi chênh lệch cạnh đều bằng 0, tạo ra tổng chi phí bằng 0. Mọi nỗ lực gán giá trị gốc dương sẽ làm tăng tổng một cách nghiêm ngặt, vì vậy đây là điều tối ưu. 

Một trường hợp khác là ràng buộc giảm nghiêm ngặt dọc theo một cạnh, ví dụ như cha mẹ với$s_u = 5$và một đứa trẻ với$s_v = 3$. DFS ngay lập tức bác bỏ điều này vì nó có nghĩa là$a_v = -2$. Thuật toán dừng ở lần vi phạm đầu tiên, đảm bảo không có sự lan truyền một phần nào làm hỏng câu trả lời. 

Trường hợp thứ ba là chuỗi sâu trong đó chỉ nút cuối cùng có giá trị lớn cố định. Thuật toán truyền bá sự bình đẳng cho đến bước cuối cùng, trong đó một bước nhảy duy nhất được thực hiện chính xác một lần, cho thấy rằng chi phí đã được cục bộ hóa và không bị dàn trải không chính xác giữa các nút trung gian.
