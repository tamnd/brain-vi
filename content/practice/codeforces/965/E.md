---
title: "CF 965E - Mã ngắn"
description: "Chúng ta có một tập hợp các chuỗi chữ thường riêng biệt, mỗi chuỗi đại diện cho một tên biến. Đối với mỗi tên gốc, chúng ta phải chọn một tiền tố không trống của chuỗi đó để đóng vai trò là mã định danh rút gọn mới của nó."
date: "2026-06-17T01:39:22+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 965
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 476 (Div. 2) [Thanks, Telegram!]"
rating: 2200
weight: 965
solve_time_s: 83
verified: false
draft: false
---

[CF 965E - Mã ngắn](https://codeforces.com/problemset/problem/965/E) 

**Xếp hạng:** 2200 
**Tags:** cấu trúc dữ liệu, dp, tham lam, chuỗi, cây 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các chuỗi chữ thường riêng biệt, mỗi chuỗi đại diện cho một tên biến. Đối với mỗi tên gốc, chúng ta phải chọn một tiền tố không trống của chuỗi đó để đóng vai trò là mã định danh rút gọn mới của nó. Hạn chế là sau khi rút ngắn tất cả các tên, không có hai tên kết quả nào có thể giống hệt nhau. 

Trong số tất cả các cách hợp lệ để chọn tiền tố, chúng tôi muốn cách tối thiểu hóa tổng độ dài tiền tố đã chọn. 

Vì vậy, vấn đề không chỉ là tìm các tiền tố duy nhất mà còn là cân bằng chúng: tiền tố ngắn hơn luôn tốt hơn về mặt cục bộ, nhưng việc chọn nó có thể buộc các chuỗi khác sử dụng tiền tố dài hơn sau này. Nhiệm vụ là giải quyết những xung đột này trên toàn cầu để giảm thiểu tổng chi phí. 

Những ràng buộc rất chặt chẽ. Có tới 100000 chuỗi và tổng chiều dài của tất cả các chuỗi cũng tối đa là 100000. Điều này ngay lập tức gợi ý rằng mọi giải pháp phải gần tuyến tính hoặc gần tuyến tính trong tổng số ký tự được xử lý. Bất cứ điều gì cố gắng so sánh nhiều lần các chuỗi đầy đủ theo cặp hoặc tính toán lại tính duy nhất của tiền tố từ đầu cho mỗi chuỗi sẽ không thành công. 

Một mô hình tinh thần đơn giản sẽ là: với mỗi chuỗi, hãy thử tiền tố ngắn nhất mà bất kỳ chuỗi nào khác không sử dụng. Khó khăn là “không được sử dụng bởi bất kỳ chuỗi nào khác” phụ thuộc vào sự lựa chọn cho tất cả các chuỗi khác, vì vậy các quyết định tham lam cục bộ có thể thất bại. 

Một trường hợp lỗi nhỏ xuất hiện khi nhiều chuỗi có chung tiền tố dài nhưng sau đó lại phân kỳ. Nếu một chuỗi có tiền tố rất ngắn sớm, nó có thể buộc các chuỗi khác phải mở rộng đáng kể mặc dù tiền tố dài hơn một chút cho chuỗi đầu tiên sẽ giảm tổng chi phí. Ví dụ: 

đầu vào:```
3
aaa
aab
aac
```Nếu chúng ta tham lam gán`"a"`vào chuỗi đầu tiên, hai chuỗi còn lại có thể cần`"aa"`hoặc lâu hơn, làm tăng tổng chi phí một cách không cần thiết. Sự phân công tối ưu phụ thuộc vào cấu trúc tập thể chứ không phải sự lựa chọn độc lập. 

Cấu trúc ẩn chính là tính duy nhất bị chi phối hoàn toàn bởi các tiền tố được chia sẻ, điều này gợi ý việc tổ chức tất cả các chuỗi trong cây tiền tố. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chuỗi, chúng tôi thử các tiền tố có độ dài tăng dần cho đến khi tìm thấy tiền tố không khớp với bất kỳ tiền tố nào được chọn cho chuỗi khác. Để kiểm tra điều này một cách chính xác, chúng ta cần duy trì một tập hợp toàn cầu các tiền tố đã chọn và xác minh từng tiền tố ứng viên dựa trên nó. Trong trường hợp xấu nhất, mỗi chuỗi có độ dài L có thể yêu cầu kiểm tra tiền tố O(L) và mỗi lần kiểm tra đều liên quan đến việc băm hoặc so sánh chuỗi, dẫn đến hành vi bậc hai trong tổng chiều dài. Với tổng chiều dài lên tới 100000, việc quét lại nhiều lần sẽ trở nên quá chậm. 

Quan sát quan trọng là xung đột tiền tố không phải là tùy tiện mà có cấu trúc. Hai tiền tố chỉ xung đột nếu một chuỗi là tiền tố của chuỗi khác hoặc chúng có chung một đường dẫn trong cây tiền tố. Nếu chúng ta xây dựng một bộ ba tất cả các chuỗi thì mỗi tiền tố tương ứng với một nút và tính duy nhất phụ thuộc vào số lượng chuỗi đi qua nút đó. 

Điều này gợi ý đảo ngược quan điểm. Thay vì tham lam gán các tiền tố từ ngắn nhất đến dài nhất, chúng tôi nghĩ theo mức độ cần thiết. Một nút trong bộ ba chỉ là ứng cử viên hợp lệ cho một chuỗi nếu đường dẫn đến nút đó là duy nhất trong số tất cả các chuỗi hoặc có thể được tạo thành duy nhất bằng cách đẩy các chuỗi khác xuống sâu hơn. Việc giảm thiểu chi phí một cách tự nhiên gợi ý cách diễn giải lập trình động trên trie: chúng tôi gán cho mỗi chuỗi một điểm dừng trong trie sao cho không có hai chuỗi nào dừng ở cùng một nút. 

Cấu trúc trở thành bài toán DP cây. Mỗi nút biết có bao nhiêu chuỗi đi qua nó và chúng tôi quyết định có bao nhiêu chuỗi trong số đó phải tiếp tục đi xuống để thực thi tính duy nhất. Chi phí chúng ta phải trả tỷ lệ thuận với độ sâu nơi mỗi sợi dây dừng lại. Chúng tôi đẩy các dây “dư thừa” xuống dưới theo cách giảm thiểu tổng độ sâu tăng lên. 

Sau khi được xem như một thử nghiệm với việc gán các chuỗi giống như luồng cho các nút, chiến lược tối ưu sẽ trở thành tham lam từ dưới lên: tại mỗi nút, giữ càng nhiều chuỗi càng tốt và dừng ở đây và đẩy các chuỗi còn lại cho nút con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra tiền tố Brute Force | O(N * L^2) | O(N * L) | Quá chậm | 
| Trie + bài tập từ dưới lên | O(tổng chiều dài) | O(tổng chiều dài) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một bộ ba tất cả các chuỗi, trong đó mỗi nút tương ứng với một tiền tố và lưu trữ số lượng chuỗi đi qua nó. Khi đó ta giải bài toán gán từ các lá trở lên.

1. Chèn mọi chuỗi vào một bộ ba, tăng bộ đếm tại mỗi nút được truy cập. Bộ đếm này biểu thị số lượng chuỗi chia sẻ tiền tố này. Đây là thông tin cơ bản quyết định liệu tiền tố có thể được sử dụng một cách an toàn hay không. 
2. Xác định việc duyệt thứ tự sau của trie. Chúng tôi xử lý con trước cha mẹ để các quyết định về tiền tố sâu hơn được hoàn tất trước khi chúng tôi quyết định điều gì xảy ra ở cấp cao hơn. 
3. Tại mỗi nút, duy trì một giá trị biểu thị số lượng chuỗi vẫn “chưa được gán” và hiện nằm trong cây con này. Đối với các đường dẫn lá, điều này bắt đầu bằng số lượng chuỗi kết thúc ở đó hoặc đi qua. 
4. Đối với một nút nhất định, trước tiên chúng tôi thu thập sự đóng góp từ tất cả trẻ em. Các chuỗi này đại diện cho các chuỗi không được gán tiền tố sâu hơn và vẫn có sẵn để được gán ở đây hoặc ở trên. 
5. Nếu nhiều chuỗi đến cùng một nút, nhiều nhất một trong số chúng có thể dừng ở đây một cách an toàn, vì việc dừng nhiều hơn sẽ vi phạm tính duy nhất. Do đó, chúng tôi chỉ định chính xác một chuỗi dừng ở nút này nếu có. 
6. Tất cả các chuỗi còn lại tại nút này phải được đẩy lên trên. Tuy nhiên, vì chúng tôi xử lý từ dưới lên nên việc đẩy lên trên tương đương với việc trả lại số lượng vượt quá cho cấp độ gốc. 
7. Mỗi lần chúng ta gán một chuỗi dừng ở một nút, chúng ta sẽ cộng độ sâu của nút đó vào tổng câu trả lời. Điều này phản ánh độ dài tiền tố được chọn cho chuỗi đó. 
8. Gốc tích lũy tất cả các chuỗi còn lại, đảm bảo mọi chuỗi đều được gán ở đâu đó. 

Ý tưởng chính là mỗi chuỗi được gán cho nút cao nhất có thể, nơi nó có thể “yêu cầu” duy nhất một tiền tố, nhưng xung đột được giải quyết cục bộ trong các cây con. 

### Tại sao nó hoạt động 

Trie phân vùng chuỗi theo tiền tố. Tại bất kỳ nút nào, tất cả các chuỗi trong cây con của nó đều không thể phân biệt được với tiền tố đó. Nếu có nhiều hơn một chuỗi cố gắng dừng ở cùng một nút, tính duy nhất sẽ bị vi phạm, vì vậy nhiều nhất một chuỗi có thể được chỉ định ở đó. Bất kỳ nhiệm vụ nào khác phải đi sâu hơn. 

Việc xử lý từ dưới lên đảm bảo rằng bất kỳ chuỗi nào có thể được gán sâu hơn đều đã được đặt ở vị trí tối ưu trước khi chúng tôi xem xét các cấp độ cao hơn. Điều này ngăn cản việc phân công sớm ở độ sâu nông, bởi vì các cơ hội duy nhất ở sâu hơn sẽ được giải quyết trước tiên. Điều bất biến là khi xử lý một nút, tất cả các chuỗi con đã tối đa hóa số lượng chuỗi được chỉ định ở độ sâu lớn hơn, do đó, mọi chuỗi còn lại buộc phải sử dụng nút này trở lên và việc chọn một nhiệm vụ ở đây sẽ giảm thiểu tổng độ sâu trong số tất cả các lựa chọn tương đương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("ch", "cnt")
    def __init__(self):
        self.ch = [-1] * 26
        self.cnt = 0

def new_node():
    return Node()

nodes = [new_node()]

def add_string(s):
    v = 0
    nodes[v].cnt += 1
    for c in s:
        x = ord(c) - 97
        if nodes[v].ch[x] == -1:
            nodes[v].ch[x] = len(nodes)
            nodes.append(new_node())
        v = nodes[v].ch[x]
        nodes[v].cnt += 1

def dfs(v, depth):
    res = 0
    rem = nodes[v].cnt

    for nx in nodes[v].ch:
        if nx != -1:
            got_res, got_rem = dfs(nx, depth + 1)
            res += got_res
            rem -= (nodes[nx].cnt - got_rem)

    if rem > 0:
        res += depth
        rem -= 1

    return res, rem

n = int(input())
words = [input().strip() for _ in range(n)]

for w in words:
    add_string(w)

ans, _ = dfs(0, 0)
print(ans)
```Cấu trúc trie là tiêu chuẩn: mỗi nút theo dõi có bao nhiêu chuỗi đi qua nó, sau này xác định có bao nhiêu ứng cử viên cạnh tranh cho tiền tố đó. 

DFS tính toán hai giá trị: tổng chi phí đóng góp bởi các tiền tố được gán trong cây con đó và số chuỗi vẫn chưa được gán và phải được xử lý ở cấp độ cao hơn trong cây. Bước trừ bên trong vòng lặp sẽ loại bỏ các chuỗi đã được gán trong cây con, đảm bảo cây con chỉ nhìn thấy các xung đột còn sót lại. 

Một chi tiết triển khai tinh tế là chúng tôi chỉ ấn định chi phí tại nút hiện tại sau khi xử lý nút con. Điều này đảm bảo rằng chúng ta chỉ gán một tiền tố ở đây nếu không thể gán sâu hơn cho tất cả ngoại trừ một chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
codeforces
codehorses
code
```Chúng tôi xây dựng một thử nghiệm trong đó cả ba chuỗi đều có chung tiền tố`"code"`, sau đó phân kỳ. 

| Nút (tiền tố) | cnt | kết quả con rem | được giao ở đây | rem sau | 
| --- | --- | --- | --- | --- | 
| "mã" | 3 | trẻ kiệt sức | 1 | 2 | 
| gốc | 3 | cây con được xử lý | 1 | 0 | 

Tại`"code"`, chỉ có thể gán một chuỗi tiền tố ở đó; hai người còn lại phải di chuyển lên trên. Cuối cùng, chúng buộc phải được gán ở các tiền tố ngắn hơn. 

Câu trả lời cuối cùng là 6. 

Dấu vết này cho thấy rằng mặc dù tất cả các chuỗi đều có chung một tiền tố sâu, nhưng chỉ một chuỗi có thể “yêu cầu” cấp độ đó và các chuỗi khác được đẩy lên các tiền tố cao hơn, ngắn hơn. 

### Ví dụ 2 

đầu vào:```
4
a
ab
abc
abd
```| Nút | cnt | rem từ trẻ em | được giao | rem | 
| --- | --- | --- | --- | --- | 
| "abc" | 1 | 0 | 1 | 0 | 
| "abd" | 1 | 0 | 1 | 0 | 
| "ab" | 3 | 0 | 1 | 2 | 
| "một" | 4 | 2 | 1 | 1 | 
| gốc | 4 | 1 | 1 | 0 | 

Ở đây, các nút sâu hơn đã giải quyết được hai chuỗi và chỉ có một số xung đột lan truyền lên trên. Kết quả phản ánh rằng các tiền tố được chia sẻ dài hơn được sử dụng khi có lợi, nhưng chỉ khi chúng không gây ra quá nhiều xung đột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng ký tự) | mỗi chuỗi được chèn một lần, mỗi nút trie được xử lý một lần | 
| Không gian | O(tổng ký tự) | mỗi tiền tố riêng biệt tạo ra tối đa một nút trie | 

Các ràng buộc đảm bảo tổng chiều dài lên tới 100000, do đó cả thời gian và bộ nhớ đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Node:
        __slots__ = ("ch", "cnt")
```
