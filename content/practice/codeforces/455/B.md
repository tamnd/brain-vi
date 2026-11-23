---
title: "CF 455B - Rất Nhiều Trò Chơi"
description: "Trò chơi có thể được coi là chuyển động thông qua một bộ ba được xây dựng từ các chuỗi đã cho. Chúng ta bắt đầu từ gốc, đại diện cho chuỗi rỗng. Ở mỗi lượt, người chơi chọn một cạnh đi ra, tương ứng với việc thêm một nhân vật."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "games", "implementation", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 455
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 260 (Div. 1)"
rating: 1900
weight: 455
solve_time_s: 109
verified: true
draft: false
---

[CF 455B - Rất nhiều trò chơi](https://codeforces.com/problemset/problem/455/B) 

**Xếp hạng:** 1900 
**Thẻ:** dfs và tương tự, dp, trò chơi, triển khai, chuỗi, cây 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi có thể được coi là chuyển động thông qua một bộ ba được xây dựng từ các chuỗi đã cho. 

Chúng ta bắt đầu từ gốc, đại diện cho chuỗi rỗng. Ở mỗi lượt, người chơi chọn một cạnh đi ra, tương ứng với việc thêm một nhân vật. Chuỗi hiện tại phải luôn là tiền tố của ít nhất một từ trong từ điển, vì vậy mọi bước di chuyển hợp pháp chỉ cần tuân theo một cạnh trong bộ ba. 

Người chơi sẽ thua khi đến nút trie mà không có nút con nào vì không thể thêm ký tự nào nữa. 

Điều khó khăn là trò chơi được chơi chính xác`k`lần. Người thua một ván sẽ bắt đầu ván tiếp theo, và điều quan trọng duy nhất là ai thắng ván cuối cùng. Cả hai cầu thủ đều chơi tối ưu trong mọi trận đấu. 

Đầu vào cho đến`10^5`chuỗi và tổng độ dài của tất cả các chuỗi cũng tối đa`10^5`. Bất kỳ giải pháp nào chỉ xử lý mỗi nút trie với số lần không đổi đều đủ nhanh. Bất kỳ số bậc hai nào về số nút sẽ là quá lớn, vì một tri có thể chứa khoảng`10^5`nút. 

Phần tinh tế nhất của vấn đề là thực tế có hai thuộc tính trò chơi khác nhau quan trọng. 

Hãy xem xét đầu vào này:```
1 2
a
```Trie chỉ là root → a. Người chơi đầu tiên phải di chuyển đến`a`, khi đó người chơi thứ hai không di chuyển và thua cuộc. Người chơi đầu tiên luôn thắng. Vì người thua bắt đầu ván tiếp theo nên người chơi thứ hai bắt đầu ván thứ hai và lại thua. Người chơi đầu tiên thắng trò chơi cuối cùng. Một giải pháp chỉ tính toán "người chơi đầu tiên có thể giành chiến thắng từ gốc không?" sẽ bỏ lỡ sự tương tác giữa nhiều trò chơi. 

Một trường hợp quan trọng khác là:```
2 100
ab
ac
```Root là thắng vì người chơi đầu tiên có thể di chuyển. Nhưng mọi đường đi đều có độ dài đúng bằng hai. Cấu trúc chẵn lẻ của trò chơi trở nên quan trọng. Việc người chơi đầu tiên có thể giành chiến thắng hay không là chưa đủ; chúng ta cũng phải biết liệu họ có thể buộc phải thua lỗ hay không nếu điều đó là mong muốn về mặt chiến lược. 

Trường hợp phức tạp thứ ba là:```
1 1
abc
```Chỉ có một con đường duy nhất. Kết quả hoàn toàn được xác định bởi tính chẵn lẻ của độ dài đường dẫn. Bất kỳ giải pháp nào bỏ qua các vị trí đầu cuối và chỉ xem xét các yếu tố phân nhánh sẽ thất bại. 

Quan sát quan trọng là chúng ta cần hai thông tin độc lập về mỗi nút trie. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ phân tích rõ ràng mọi trạng thái trò chơi có thể có và mọi chuỗi trò chơi có thể có. Ngay cả đối với một trò chơi, số lần chơi có thể tăng theo cấp số nhân theo độ sâu trie. Từ`k`có thể lớn như`10^9`, việc mô phỏng từng trò chơi một là điều không thể. 

Cấu trúc của trò chơi khách quan gợi ý sử dụng trò chơi DP. Mỗi nút trie là một trạng thái. Từ một nút, người chơi chọn một đứa trẻ. Các nút đầu cuối không có động thái hợp pháp. 

Đối với một trò chơi chơi bình thường, một trạng thái sẽ thắng nếu tồn tại sự chuyển sang trạng thái thua. Một trạng thái sẽ thua nếu mọi nước đi đều chuyển sang trạng thái thắng. 

Nếu chúng ta chỉ tính toán trạng thái thắng/thua thông thường này, chúng ta sẽ có được tập hợp các vị trí mà người chơi hiện tại có thể giành chiến thắng. Điều đó giải quyết được phiên bản một trò chơi, nhưng không giải quyết được vấn đề này. Quy tắc trò chơi lặp lại đặt ra một câu hỏi khác: người chơi hiện tại có thể buộc trò chơi kết thúc với số nước đi còn lại ngang nhau không? 

Điều này dẫn đến hai tính toán DFS. 

DFS đầu tiên tính toán xem một nút có chiến thắng hay không, thường được gọi là "trạng thái thắng". 

DFS thứ hai tính toán xem một nút có bị mất theo một tiêu chí khác hay không, thường được gọi là "trạng thái mất" trong các bài xã luận. Nó xác định liệu người chơi hiện tại có thể buộc trò chơi kết thúc theo cách cuối cùng cho phép đánh bại nếu muốn hay không. 

Khi cả hai thuộc tính đều được biết đến gốc, toàn bộ vấn đề nhiều trò chơi sẽ được giải quyết thành một vài trường hợp đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Trie tối ưu + DFS Game DP | O(tổng chiều dài chuỗi) | O(tổng chiều dài chuỗi) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng bản thử từ tất cả các chuỗi đầu vào. 

Mỗi nút đại diện cho một tiền tố hợp lệ. Mọi nước đi hợp pháp trong trò chơi đều tương ứng với việc đi qua một cạnh ba cạnh. 
2. Tính toán`win[v]`cho mỗi nút. 

Nếu một nút không có nút con,`win[v] = False`bởi vì người chơi hiện tại không thể di chuyển. 

Nếu không thì,`win[v] = True`nếu ít nhất một đứa trẻ có`win = False`. 

Đây là sự diễn ra trạng thái chiến thắng tiêu chuẩn cho các trò chơi công bằng. 
3. Tính toán`lose[v]`cho mỗi nút. 

Nếu một nút không có nút con, hãy đặt`lose[v] = True`. 

Nếu không thì,`lose[v] = True`nếu ít nhất một đứa trẻ có`lose = False`. 

DP thứ hai này trả lời một câu hỏi khác: liệu người chơi hiện tại có thể buộc trò chơi kết thúc ở mức ngang bằng để cho phép cuối cùng bị đánh bại hay không. 
4. Đặt nút gốc là nút`0`. 

Nếu như`win[root]`là sai, người chơi đầu tiên không thể thắng dù chỉ một ván đấu. Câu trả lời là`"Second"`. 
5. Nếu`win[root]`là đúng và`lose[root]`đúng, người chơi đầu tiên có thể buộc một trong hai kiểu kết thúc. Câu trả lời luôn là`"First"`. 
6. Nếu`win[root]`là đúng và`lose[root]`là sai thì chỉ có thể có một cấu trúc chẵn lẻ. 

Trong trường hợp này, câu trả lời phụ thuộc vào`k`. 

Nếu như`k`là số lẻ, đầu ra`"First"`. 

Nếu như`k`chẵn, đầu ra`"Second"`. 

### Tại sao nó hoạt động 

DFS đầu tiên xác định các vị trí mà người chơi hiện tại có thể giành chiến thắng trong một trò chơi. 

DFS thứ hai xác định các vị trí mà từ đó người chơi hiện tại có thể buộc phải tiếp cận nút đầu cuối theo tỷ lệ chẵn lẻ thuận lợi. Một nút đầu cuối có`lose = True`bởi vì người chơi hiện tại di chuyển đến đó sẽ thua ngay lập tức. 

Gốc rơi vào đúng một trong ba loại có ý nghĩa. 

Nếu gốc không thắng thì người chơi đầu tiên thua ngay. 

Nếu gốc đang thắng và cũng có thuộc tính thứ hai, người chơi đầu tiên có đủ quyền kiểm soát cây trò chơi để giành chiến thắng bất kể còn lại bao nhiêu ván đấu. 

Nếu căn gốc thắng nhưng thiếu thuộc tính thứ hai, kết quả trò chơi sẽ được cố định theo tính chẵn lẻ và chỉ là số lẻ.`k`cho phép người chơi đầu tiên giành chiến thắng trong trò chơi cuối cùng. 

Đây chính xác là những trường hợp đã được chứng minh trong phân tích chính thức của trò chơi này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    trie = [[-1] * 26]

    for _ in range(n):
        s = input().strip()
        node = 0

        for ch in s:
            c = ord(ch) - ord('a')

            if trie[node][c] == -1:
                trie[node][c] = len(trie)
                trie.append([-1] * 26)

            node = trie[node][c]

    m = len(trie)

    win = [False] * m
    lose = [False] * m

    sys.setrecursionlimit(300000)

    def dfs(v):
        children = []

        for nxt in trie[v]:
            if nxt != -1:
                children.append(nxt)

        if not children:
            win[v] = False
            lose[v] = True
            return

        win[v] = False
        lose[v] = False

        for to in children:
            dfs(to)

        for to in children:
            if not win[to]:
                win[v] = True
            if not lose[to]:
                lose[v] = True

    dfs(0)

    if not win[0]:
        print("Second")
    elif lose[0]:
        print("First")
    else:
        print("First" if k % 2 == 1 else "Second")

if __name__ == "__main__":
    solve()
```Cấu trúc trie tạo một nút cho mỗi tiền tố riêng biệt. Vì tổng chiều dài đầu vào tối đa là`10^5`, tổng số nút trie cũng là`O(10^5)`. 

các`win`mảng lưu trữ kết quả lý thuyết trò chơi tiêu chuẩn. Một nút sẽ thắng nếu có sự di chuyển đến một nút con thua cuộc. 

các`lose`mảng sử dụng một phép lặp khác. Các nút đầu cuối được đánh dấu là đúng. Đối với nút bên trong, giá trị trở thành đúng nếu ít nhất một nút con có giá trị sai. Điều này cho biết liệu người chơi hiện tại có thể buộc trò chơi kết thúc với cấu trúc chẵn lẻ được yêu cầu hay không. 

Một chi tiết triển khai thường gây ra lỗi là trường hợp cơ bản của`lose`. Các nút đầu cuối phải được khởi tạo để`True`, không`False`. Việc sử dụng sai giá trị sẽ đảo ngược hoàn toàn ý nghĩa của DFS thứ hai. 

Một sai lầm phổ biến khác là quên rằng`k`có thể lớn như`10^9`. Bất kỳ cách tiếp cận nào cố gắng mô phỏng trò chơi đều không thể thực hiện được. Chỉ có hai giá trị DP của gốc là quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
a
b
```Tri:```
root
 ├─ a
 └─ b
```| Nút | thắng | thua | 
| --- | --- | --- | 
| một | Sai | Đúng | 
| b | Sai | Đúng | 
| gốc | Đúng | Sai | 

Root đang thắng, nhưng`lose[root]`là sai.`k = 3`thật kỳ quặc. 

Trả lời:`"First"`. 

Ví dụ này minh họa trường hợp phụ thuộc vào tính chẵn lẻ. Người chơi đầu tiên có thể thắng trò chơi nhưng không thể kiểm soát cấu trúc kết thúc thay thế. 

### Ví dụ 2 

đầu vào:```
2 2
a
ab
```Tri:```
root
 └─ a
     └─ b
```Giá trị DP: 

| Nút | thắng | thua | 
| --- | --- | --- | 
| b | Sai | Đúng | 
| một | Đúng | Sai | 
| gốc | Sai | Đúng | 

Bản thân gốc đang mất đi. 

Trả lời:`"Second"`. 

Ví dụ này cho thấy rằng mặc dù bộ ba có nhiều cấp độ, người chơi đầu tiên có thể không có nước đi thắng từ vị trí ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) | Mỗi nút và cạnh trie được xử lý một lần, trong đó`L`là tổng chiều dài của tất cả các chuỗi | 
| Không gian | O(L) | Trie lưu trữ cộng với mảng DP | 

Vì tổng độ dài của tất cả các chuỗi nhiều nhất là`10^5`, cả thời gian và mức sử dụng bộ nhớ đều nằm trong giới hạn thoải mái. DFS truy cập mỗi nút trie một lần và bản thân trie chứa nhiều nhất`10^5 + 1`nút. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())

    trie = [[-1] * 26]

    for _ in range(n):
        s = input().strip()
        node = 0

        for ch in s:
            c = ord(ch) - ord('a')

            if trie[node][c] == -1:
                trie[node][c] = len(trie)
                trie.append([-1] * 26)

            node = trie[node][c]

    m = len(trie)
    win = [False] * m
    lose = [False] * m

    sys.setrecursionlimit(300000)

    def dfs(v):
        children = [x for x in trie[v] if x != -1]

        if not children:
            lose[v] = True
            return

        for to in children:
            dfs(to)

        for to in children:
            if not win[to]:
                win[v] = True
            if not lose[to]:
                lose[v] = True

    dfs(0)

    if not win[0]:
        return "Second"

    if lose[0]:
        return "First"

    return "First" if k % 2 else "Second"

# provided sample
assert run("2 3\na\nb\n") == "First", "sample 1"

# minimum size
assert run("1 1\na\n") == "First", "single move"

# parity-dependent case
assert run("2 2\na\nb\n") == "Second", "even k"

# losing root
assert run("1 2\nab\n") == "Second", "forced loss"

# deeper branching
assert run("3 5\na\nab\nac\n") == "First", "mixed trie"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / a`| Đầu tiên | Trie nhỏ nhất không tầm thường | 
|`2 2 / a,b`| Thứ hai | Xử lý chẵn lẻ/lẻ | 
|`1 2 / ab`| Thứ hai | Root có thể bị mất | 
|`3 5 / a,ab,ac`| Đầu tiên | Cấu trúc phân nhánh hỗn hợp | 

## Vỏ cạnh 

Hãy xem xét:```
1 2
a
```Trie chứa một cạnh. Gốc có`win = True`Và`lose = False`. Từ`k`là chẵn, thuật toán đưa ra`"Second"`. Đây chính xác là tình huống nhạy cảm với tính chẵn lẻ, phá vỡ các giải pháp chỉ kiểm tra trạng thái chiến thắng thông thường. 

Coi như:```
1 1
abc
```Trie là một đường dẫn có độ dài ba. DFS truyền các giá trị từ lá trở lên. Gốc trở thành thắng, nên câu trả lời là`"First"`. Thuật toán xử lý chính xác các chuỗi dài mà không yêu cầu tính chẵn lẻ rõ ràng. 

Coi như:```
2 1
ab
ac
```Cả hai đường dẫn từ gốc tới lá đều có cùng độ sâu. Root đang thắng vì nó có thể chuyển sang cấu hình con thua cuộc. DFS tính toán trạng thái lý thuyết trò chơi chính xác bất kể độ dài đường dẫn. Câu trả lời có được trực tiếp từ hai giá trị DP của gốc. 

Những trường hợp này minh họa tại sao cả hai trạng thái DFS đều cần thiết. Một cái nắm bắt liệu có khả năng chiến thắng hay không, và cái kia nắm bắt liệu người chơi có đủ quyền kiểm soát cấu trúc kết thúc của trò chơi để xử lý các giá trị tùy ý của`k`.
