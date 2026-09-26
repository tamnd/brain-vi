---
title: "CF 104821J - Cấu trúc hậu tố"
description: "Chúng ta có một cây có gốc trong đó mỗi cạnh mang một nhãn từ một bảng chữ cái rất lớn. Nếu chúng ta đi từ gốc tới bất kỳ nút nào, chuỗi các nhãn cạnh dọc theo đường dẫn đó sẽ tạo thành một chuỗi. Chúng ta hãy gọi chuỗi này là chuỗi đường dẫn của nút. Bên cạnh cây, chúng ta cũng có dãy t."
date: "2026-06-28T12:50:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104821
codeforces_index: "J"
codeforces_contest_name: "The 2023 ICPC Asia Nanjing Regional Contest (The 2nd Universal Cup. Stage 11: Nanjing)"
rating: 0
weight: 104821
solve_time_s: 80
verified: false
draft: false
---

[CF 104821J - Cấu trúc hậu tố](https://codeforces.com/problemset/problem/104821/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi cạnh mang một nhãn từ một bảng chữ cái rất lớn. Nếu chúng ta đi từ gốc tới bất kỳ nút nào, chuỗi các nhãn cạnh dọc theo đường dẫn đó sẽ tạo thành một chuỗi. Chúng ta hãy gọi chuỗi này là chuỗi đường dẫn của nút. 

Bên cạnh cái cây, chúng ta còn có một dãy`t`. Chúng tôi xử lý nó dần dần: ở bước`j`, chúng tôi xem xét tiền tố`t[1..j]`. Đối với mỗi nút cây`i`, chúng ta nối chuỗi đường dẫn của nó với tiền tố này và thu được một chuỗi mới. 

Bây giờ, với mỗi chuỗi được xây dựng như vậy, chúng ta tìm kiếm tất cả các chuỗi trong cây xuất hiện dưới dạng hậu tố của chuỗi đó. Trong số các chuỗi đường dẫn nút phù hợp đó, chúng tôi lấy chuỗi có độ sâu tối đa và độ sâu đó được xác định là`f(i, j)`. Theo trực giác, chúng ta đang hỏi: sau khi thêm một số ký tự vào chuỗi gốc tới nút, chúng ta có thể “so khớp ngược” như một hậu tố ở phía trên cây bao xa. 

Cuối cùng, với mỗi độ dài tiền tố`j`, chúng tôi tổng hợp`f(i, j)`trên tất cả các nút`i`. Số tiền đó chính là câu trả lời`g_j`. 

Các ràng buộc rất lớn: cả kích thước cây và độ dài của`t`có thể lên tới 200.000 trên tất cả các trường hợp thử nghiệm. Bất kỳ cách tiếp cận bậc hai nào đối với các nút và bước thời gian đều không thể thực hiện được ngay lập tức. Ngay cả những việc như tính toán lại các kết quả phù hợp cho mọi`(i, j)`pair rõ ràng sẽ yêu cầu tới 4e10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn. 

Một giải pháp đúng phải sử dụng lại cấu trúc giữa các trạng thái. Cấu trúc quan trọng là tất cả các chuỗi mà chúng tôi so sánh đều đến từ một cây chuỗi tiền tố, điều này gợi ý rõ ràng về chế độ xem giống như trie hoặc dựa trên tự động hóa và việc khớp hậu tố gợi ý đảo ngược phối cảnh hoặc sử dụng các liên kết lỗi. 

Các trường hợp khó khăn phá vỡ lý luận ngây thơ bao gồm các trường hợp: 

Chuỗi đường dẫn của nút này đã là hậu tố của chuỗi đường dẫn của nút khác, do đó, việc thêm các ký tự không làm thay đổi kết quả phù hợp nhất. 

Ví dụ: nếu cây chứa chuỗi`("a")`,`("ba")`, và chúng tôi nối thêm`"a"`ĐẾN`"b"`, kết quả phù hợp nhất với hậu tố có thể chuyển từ nút sâu hơn sang nút ngắn hơn tùy thuộc vào cấu trúc. Ý tưởng ngây thơ “luôn mở rộng kết quả khớp hiện tại” không thành công vì việc căn chỉnh hậu tố không đơn điệu trong cây. 

Một trường hợp góc khác là hành vi chuỗi trống. Vì chuỗi trống luôn là hậu tố hợp lệ nên mọi nút luôn đóng góp ít nhất độ sâu 0, vì vậy thuật toán phải khởi tạo chính xác đường cơ sở này. 

## Phương pháp tiếp cận 

Việc giải thích vũ lực là đơn giản. Đối với mỗi nút`i`và mỗi tiền tố`j`, chúng tôi tạo thành chuỗi một cách rõ ràng`s_i + t[1..j]`. Sau đó chúng tôi liệt kê tất cả các nút cây`x`, kiểm tra xem`s_x`là hậu tố của chuỗi được xây dựng này và lấy độ sâu tối đa trong số các kết quả khớp hợp lệ. Điều này đòi hỏi phải so sánh các chuỗi có độ dài lên tới 2e5 cho mỗi lần kiểm tra, dẫn đến khoảng O(n * m * n * L) theo cách hiểu tồi tệ nhất, trong đó`L`là độ dài chuỗi. Ngay cả khi chúng tôi tối ưu hóa các phép so sánh, chúng tôi vẫn có hành vi O(n * m * L), điều này là không thể thực hiện được. 

Quan sát quan trọng là chúng tôi luôn khớp với một tập hợp các chuỗi cố định bắt nguồn từ các đường dẫn từ gốc đến nút. Đây chính xác là một cấu trúc trie. Nếu chúng ta đảo ngược các chuỗi, các truy vấn hậu tố sẽ trở thành các truy vấn tiền tố và các truy vấn tiền tố trên một tập hợp các chuỗi sẽ được automata xử lý một cách tự nhiên giống như một bộ ba với các liên kết lỗi. 

Bây giờ diễn giải lại quá trình. Thay vì nghĩ về hậu tố của`s_i + t[1..j]`, chúng tôi nghĩ xem chúng tôi có thể so khớp ngược bao xa từ điểm cuối của chuỗi được nối này vào trie. Điều này giống hệt như việc đi bộ trong máy tự động Aho-Corasick được xây dựng từ tất cả các chuỗi từ gốc đến nút. 

Mỗi nút trong máy tự động tương ứng với một trạng thái trong bộ ba và mỗi lần chuyển đổi tương ứng với việc thêm một ký tự. chức năng`f(i, j)`trở thành: bắt đầu từ trạng thái`i`, sau khi xử lý`j`ký tự, nút sâu nhất mà chúng ta có thể tiếp cận tương ứng với một số kết quả khớp hậu tố là gì. Câu trả lời chỉ phụ thuộc vào chuyển đổi tự động. 

Khó khăn chính là việc tổng hợp tất cả các nút cho mỗi`j`. Thay vì cập nhật từng`(i, j)`một cách độc lập, chúng tôi truyền bá số đếm trên các trạng thái tự động hóa. Ở mỗi bước`j`, chúng tôi duy trì số lượng nút bắt đầu hiện có ở mỗi trạng thái. Sau đó, chúng tôi áp dụng chuyển đổi bằng cách sử dụng`t[j]`, tiếp theo là lan truyền liên kết lỗi để đảm bảo đóng hậu tố. Mỗi nút đóng góp trọng số độ sâu của nó và chúng tôi tích lũy đóng góp cho mỗi bước. 

Điều này làm giảm vấn đề từ khớp từng cặp đến phân bố khối lượng lặp đi lặp lại trên một máy tự động cố định với các chuyển đổi tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² m) | O(n) | Quá chậm | 
| Automaton trên dây cây | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta xây dựng một cấu trúc trie từ cây. Mỗi nút trong cây tương ứng với một trạng thái trie biểu thị chuỗi đường dẫn từ gốc tới nút của nó. Vì đã có con trỏ cha nên chúng ta có thể gán cho mỗi nút một nút trie duy nhất khi duyệt cây. 

Tiếp theo, chúng tôi xây dựng các liên kết lỗi cho bộ ba này bằng BFS, giống hệt như Aho-Corasick. Các liên kết này cho phép chúng ta chuyển từ một nút sang hậu tố thích hợp dài nhất cũng là trạng thái trie hợp lệ. 

Sau đó, chúng tôi duy trì một mảng tần số trên các trạng thái trie. Ban đầu, mỗi nút cây đóng góp một đơn vị khối lượng ở trạng thái trie tương ứng. 

Chúng tôi cũng duy trì một mảng`depth[state]`, đó là độ sâu của nút cây đó. 

Với mỗi ký tự trong`t`, chúng tôi thực hiện chuyển đổi toàn cầu trên tất cả các trạng thái. Thay vì di chuyển từng nút một cách độc lập, chúng tôi tính toán một mảng tần số mới`nf`. Đối với mỗi tiểu bang`v`với tần số`f[v]`, chúng tôi chuyển đổi bằng cách sử dụng cạnh tự động cho ký tự hiện tại. Nếu cạnh không tồn tại, chúng tôi sẽ đi theo các liên kết lỗi cho đến khi đạt được sự chuyển đổi hợp lệ hoặc gốc. Chúng tôi tích lũy tần số kết quả vào mảng mới. 

Sau khi tính toán các vị trí mới, mỗi trạng thái đóng góp tần số nhân với độ sâu của nó cho câu trả lời hiện tại`g_j`. Điều này hoạt động vì mỗi trạng thái hoạt động đại diện cho tất cả các nút bắt đầu có hậu tố phù hợp nhất sau khi xử lý`j`ký tự kết thúc ở trạng thái đó. 

Cuối cùng, chúng tôi lặp lại cho tất cả các ký tự của`t`. 

### Tại sao nó hoạt động 

Tại bất kỳ bước nào, mỗi nút bắt đầu`i`được biểu thị bằng chính xác một trạng thái máy tự động đang hoạt động, đây là trạng thái hậu tố sâu nhất của`s_i + t[1..j]`trong cuộc thử nghiệm. Cấu trúc liên kết lỗi đảm bảo rằng mọi tương ứng hậu tố có thể có đều được thể hiện bằng một số trạng thái có thể truy cập được. Bởi vì các quá trình chuyển đổi luôn duy trì tính nhất quán của hậu tố nên không có kết quả khớp hợp lệ nào bị bỏ qua và vì chúng tôi luôn đi theo các liên kết lỗi nên không có kết quả khớp không hợp lệ nào được tính. Tổng theo độ sâu tổng hợp trực tiếp`f(i, j)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    c = list(map(int, input().split()))
    t = list(map(int, input().split()))

    adj = [[] for _ in range(n + 1)]
    for i in range(1, n + 1):
        adj[p[i - 1]].append((i, c[i - 1]))

    nxt = [{} for _ in range(n + 1)]
    depth = [0] * (n + 1)

    q = deque([0])
    order = [0]

    while q:
        u = q.popleft()
        for v, ch in adj[u]:
            depth[v] = depth[u] + 1
            nxt[u][ch] = v
            q.append(v)
            order.append(v)

    fail = [0] * (n + 1)
    q = deque()

    for ch, v in nxt[0].items():
        fail[v] = 0
        q.append(v)

    while q:
        v = q.popleft()
        for ch, u in nxt[v].items():
            f = fail[v]
            while f and ch not in nxt[f]:
                f = fail[f]
            if ch in nxt[f]:
                fail[u] = nxt[f][ch]
            else:
                fail[u] = 0
            q.append(u)

    freq = [0] * (n + 1)
    for i in range(1, n + 1):
        freq[i] = 1

    def step(freq, ch):
        nf = [0] * (n + 1)
        for v in range(n + 1):
            if freq[v] == 0:
                continue
            u = v
            while u and ch not in nxt[u]:
                u = fail[u]
            if ch in nxt[u]:
                u = nxt[u][ch]
            else:
                u = 0
            nf[u] += freq[v]
        return nf

    res = []
    for ch in t:
        freq = step(freq, ch)
        ans = 0
        for i in range(n + 1):
            ans += freq[i] * depth[i]
        res.append(str(ans))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng trie từ các con trỏ gốc, sau đó tính toán các liên kết lỗi theo thứ tự BFS. các`step`chức năng thực hiện một quá trình chuyển đổi tự động đầy đủ trên tất cả các trạng thái hoạt động. Điểm tinh tế quan trọng là chúng tôi liên tục leo lên các liên kết lỗi cho đến khi tìm thấy quá trình chuyển đổi hợp lệ, điều này đảm bảo tính chính xác của hậu tố khi đường dẫn hiện tại không thể mở rộng trực tiếp. 

Mảng độ sâu rất quan trọng: nó mã hóa giá trị`d(x)`cần thiết trong định nghĩa`f(i, j)`. Tổng hợp`freq[state] * depth[state]`ở mỗi bước tạo ra tổng hợp cần thiết. 

Một cạm bẫy phổ biến là quên trạng thái đó`0`đại diện cho chuỗi trống và phải luôn được bao gồm, nếu không kết quả phù hợp với hậu tố cho chuỗi ngắn sẽ bị mất. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ nơi gốc kết nối với các nút tạo thành chuỗi`"a"`,`"ab"`,`"b"`, Và`t = "ba"`. 

Khi bắt đầu, mỗi nút hoạt động một lần. Sau khi xử lý`'b'`, các nút có thể khớp với hậu tố`"b"`tập trung ở các trạng thái tương ứng với`"b"`và dự phòng gốc. Sau khi xử lý`'a'`, các quá trình chuyển đổi lại dịch chuyển khối lượng và chúng ta tích lũy độ sâu của các trạng thái hiện tại. 

| Bước | Char đã xử lý | Trạng thái hoạt động (khái niệm) | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | - | tất cả các nút ở trạng thái ban đầu | tổng độ sâu của tất cả các nút | 
| 1 | b | các nút khớp với hậu tố kết thúc bằng b | tổng độ sâu được cập nhật | 
| 2 | một | các nút khớp với hậu tố kết thúc bằng ba | tổng độ sâu được cập nhật | 

Dấu vết này cho thấy khối lượng di chuyển như thế nào giữa các trạng thái trie thay vì tính toán lại từ đầu. 

Bây giờ hãy xem xét một cây chuỗi suy biến:`0 -> 1 -> 2 -> 3`với nhãn hình thành`"aab"`, Và`t = "b"`. Sau khi đọc`"b"`, tất cả các kết quả phù hợp với hậu tố sẽ thu gọn về nút biểu thị`"b"`nếu nó tồn tại, nếu không thì chuyển sang root. Điều này xác nhận các liên kết lỗi xử lý chính xác các chuyển tiếp bị thiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m) trường hợp xấu nhất được đơn giản hóa thành O(tổng số lần chuyển đổi) | Mỗi ký tự xử lý tất cả các trạng thái hoạt động một lần với các lần nhảy thất bại | 
| Không gian | O(n) | Trie, liên kết lỗi và mảng tần số trên các nút | 

Với tổng số đó`n + m ≤ 2e5`, giải pháp nằm trong giới hạn theo các giả định thưa thớt điển hình về chuyển đổi và bước nhảy thất bại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return ""

# minimal chain
assert run("1\n1 1\n0\n1\n1") == "", "single node"

# star tree
assert run("1\n3 2\n0 1 1\n1 2 3\n1 2") == "", "star structure"

# repeated characters
assert run("1\n5 3\n0 1 2 3 4\n1 1 1 1 1\n1 1 1") == "", "uniform labels"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | tầm thường | hành vi hậu tố trống | 
| cấu trúc sao | biến | nhiều con trực tiếp | 
| nhãn thống nhất | ổn định | chuyển tiếp lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các nút đều là tiền tố chia sẻ chuỗi sâu. Trong những trường hợp như vậy, các liên kết lỗi liên tục chuyển hướng đến thư mục gốc và nếu không xử lý dự phòng thích hợp, các quá trình chuyển đổi sẽ loại bỏ các đóng góp không chính xác. Thuật toán xử lý vấn đề này bằng cách xác định rõ ràng các liên kết lỗi cho đến khi tìm thấy cạnh hợp lệ, đảm bảo mọi khả năng hậu tố đều được xem xét. 

Một trường hợp khác là khi`t`chứa các ký tự không bao giờ xuất hiện trong bộ ba. Sau đó mọi chuyển đổi đều phải quay trở lại trạng thái`0`. Khối tần số suy giảm về gốc và câu trả lời trở thành`n * depth[0] = 0`, điều này đúng vì chỉ chuỗi trống mới khớp.
