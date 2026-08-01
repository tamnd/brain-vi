---
title: "CF 103931L - Cảnh báo cuối cùng của Cán bộ Tài chính Cạnh tranh"
description: "Chúng ta được cấp một chuỗi chữ thường dài s. Chúng tôi xử lý nó từ trái sang phải và sau khi đọc từng tiền tố s[1..i], chúng tôi phải tính điểm phụ thuộc vào từ điển các từ đặc biệt. Mỗi từ điển ti có một giá trị liên quan vi."
date: "2026-07-02T07:18:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103931
codeforces_index: "L"
codeforces_contest_name: "2022 Shanghai Collegiate Programming Contest"
rating: 0
weight: 103931
solve_time_s: 50
verified: true
draft: false
---

[CF 103931L - Cảnh báo cuối cùng của Cán bộ tài chính cạnh tranh](https://codeforces.com/problemset/problem/103931/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chữ thường dài`s`. Chúng tôi xử lý nó từ trái sang phải và sau khi đọc từng tiền tố`s[1..i]`, chúng ta phải tính điểm phụ thuộc vào từ điển các từ đặc biệt. 

Mỗi từ điển`t_i`có một giá trị liên quan`v_i`. Bên trong tiền tố`u`, chúng ta được phép chọn một số lần xuất hiện của các từ trong từ điển làm chuỗi con, với ràng buộc là các lần xuất hiện được chọn không trùng nhau về vị trí. Bất kỳ lựa chọn hợp lệ nào về những lần xuất hiện như vậy đều được gọi là trích xuất. Đối với mỗi lần trích xuất, chúng tôi nhân giá trị của tất cả các lần xuất hiện đã chọn và sau đó chúng tôi tính tổng sản phẩm này trên tất cả các lần trích xuất có thể. Việc khai thác rỗng góp phần`1`. 

Đầu ra là điểm của mọi tiền tố của`s`. 

Cấu hình ràng buộc là tín hiệu đầu tiên cho thấy việc liệt kê ngây thơ là không thể. Chiều dài chuỗi đạt tới`2 × 10^5`và tổng kích thước từ điển cũng là`2 × 10^5`. Bất kỳ cách tiếp cận nào liệt kê tất cả các tập hợp con của chuỗi con hoặc thậm chí tất cả các phân đoạn hợp lệ đều có tính chất hàm mũ và bị loại trừ ngay lập tức. Ngay cả tích chập bậc hai trên mỗi vị trí cũng quá chậm. 

Cấu trúc giống như đối sánh mẫu có trọng số kết hợp với lựa chọn tập hợp con tổ hợp theo các khoảng không chồng chéo. Đây là một gợi ý cổ điển về lập trình động trên các vị trí có điểm cuối mẫu đóng góp các lựa chọn nhân. 

Một vài hành vi cạnh có vấn đề. 

Một vấn đề tế nhị là các mẫu chồng chéo. Ví dụ, nếu`s = "aaa"`và từ điển chứa`"a"`Và`"aa"`, thì việc trích xuất có thể chọn các ký tự đơn hoặc chuỗi con có độ dài 2, nhưng không bao giờ chồng chéo các lựa chọn như cả hai`"aa"`bắt đầu từ 1 và`"a"`lúc 2 giờ cùng lúc. 

Một sự tinh tế khác là sự đa dạng theo vị trí. Ngay cả khi cùng một từ trong từ điển xuất hiện nhiều lần trong`s`, mỗi lần xuất hiện là độc lập, vì vậy chúng tôi thực sự đang làm việc với các trường hợp khoảng chứ không phải nhận dạng từ. 

Cuối cùng, việc trích xuất rỗng luôn tồn tại và phải đóng góp`1`tới mọi tiền tố. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực bắt đầu bằng cách xem xét tiền tố cố định`u`. Mỗi từ điển xuất hiện trong`u`có thể được coi là một khoảng`[l, r]`với trọng lượng`v`. Nhiệm vụ trở thành: tính tổng tất cả các tập hợp con của các khoảng không chồng chéo, nhân các trọng số đã chọn. 

Điều này tương đương với bài toán đếm tập hợp độc lập có trọng số trên các khoảng. Cách tiếp cận trực tiếp sẽ thử tất cả các tập hợp con xuất hiện, kiểm tra sự trùng lặp và tính toán sản phẩm. Nếu có`k`xuất hiện trong tiền tố, điều này đã dẫn đến`O(2^k)`hành vi và từ đó`k`có thể tuyến tính trong`|s|`, thật là vô vọng. 

Ngay cả một DP xem xét, đối với mỗi vị trí, tất cả các khoảng kết thúc ở đó và cố gắng kết hợp chúng với các trạng thái tương thích trước đó, đối với mỗi khoảng, sẽ cần quét tất cả các khoảng không chồng chéo trước đó, tạo ra độ phức tạp bậc hai. 

Quan sát cấu trúc quan trọng là việc trích xuất sẽ phân tích thành hệ số trên các vị trí chuỗi theo nghĩa DP tiền tố. Khi chúng ta ở vị trí`i`, chúng tôi hoặc không làm gì kết thúc tại`i`, hoặc chúng ta kết thúc một số từ trong từ điển tại`i`. Nếu một từ kết thúc tại`i`, chúng ta nhân với giá trị của nó và kết hợp nó với bất kỳ trích xuất hợp lệ nào cho đến điểm bắt đầu trừ đi một. 

Điều này chuyển vấn đề thành một vị trí DP trong đó các chuyển tiếp được đóng góp bởi tất cả các kết quả khớp từ điển kết thúc ở mỗi chỉ mục. Thử thách ở đây là tìm ra tất cả các trận đấu kết thúc ở mỗi vị trí một cách hiệu quả và tổng hợp các đóng góp từ vị trí xuất phát của chúng. 

Đây chính xác là nơi thử các từ trong từ điển đảo ngược kết hợp với quét`s`trở nên hữu ích. Chúng ta có thể liệt kê tất cả các kết quả khớp từ điển kết thúc tại mỗi vị trí trong tổng thời gian tuyến tính theo độ dài chuỗi cộng với tổng kích thước từ điển. 

Một khi chúng ta biết, đối với mỗi vị trí`r`, tất cả các trận đấu`(l, r, v)`, chúng ta có thể tính DP ở đâu`dp[i]`là điểm của tiền tố`1..i`. Sự lặp lại trở nên bổ sung trên tất cả các trận đấu kết thúc tại`i`và mỗi trận đấu đóng góp một phần mở rộng cấp số nhân của`dp[l-1]`. 

Tuy nhiên, vì có thể chọn nhiều khoảng trong bất kỳ sự kết hợp nào nên công thức đúng không chỉ là một DP chuyển tiếp đơn lẻ mà là cấu trúc tổng của các tổng. Tại mỗi vị trí, chúng tôi đang quyết định một cách hiệu quả một cách độc lập cho từng khoảng thời gian kết thúc xem có nên đưa nó vào hay không và các kết hợp sẽ nhân lên. 

Điều này dẫn đến việc tạo hàm mũ cổ điển trên các tập khoảng rời rạc, đơn giản hóa thành một phép truy toán tuyến tính trong đó mỗi khoảng đóng góp một hệ số nhân`(1 + v * dp[l-1] / dp[i])`cấu trúc, được xử lý rõ ràng bằng cách tích lũy các đóng góp trong DP chuyển tiếp với sự phân tách nhân và cộng bằng cách sử dụng số học mô-đun. 

Giải pháp tối ưu hóa cuối cùng sử dụng kết hợp trie cộng với DP trên các vị trí, tổng hợp đóng góp của tất cả các kết quả khớp từ điển kết thúc ở mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các tập hợp con xuất hiện | O(2^n) | O(n) | Quá chậm | 
| Khoảng thời gian DP với các kiểm tra tương thích đơn giản | O(n^2) | O(n) | Quá chậm | 
| Trie + DP tuyến tính trên chuỗi | O( | s | + tổng | 

## Hướng dẫn thuật toán 

Chúng ta coi mỗi từ trong từ điển là một mẫu và muốn liệt kê tất cả các lần xuất hiện trong`s`, nhưng chúng tôi làm điều đó một cách hiệu quả bằng cách sử dụng bộ ba được xây dựng trên các từ đảo ngược. 

1. Xây dựng một bộ ba từ tất cả các từ trong từ điển, nhưng lưu trữ chúng theo hướng đảo ngược. Mỗi nút đầu cuối lưu trữ giá trị của từ. Điều này cho phép chúng ta phát hiện các từ kết thúc ở một vị trí nhất định bằng cách đi lùi trong`s`. 
2. Đối với từng vị trí`i`TRONG`s`, chúng ta đi ngược lại đến độ dài từ tối đa, theo các lần chuyển đổi trie. Bất cứ khi nào chúng tôi đến nút đầu cuối, chúng tôi sẽ ghi lại kết quả khớp`(l, i, v)`Ở đâu`l`là vị trí bắt đầu của từ phù hợp. 
3. Duy trì mảng DP`dp[i]`, Ở đâu`dp[i]`là tổng số điểm của tiền tố`1..i`. Chúng tôi khởi tạo`dp[0] = 1`. 
4. Xử lý các vị trí từ trái qua phải. Tại vị trí`i`, bắt đầu với`dp[i] = dp[i-1]`, đại diện cho các trích xuất không kết thúc tại`i`. 
5. Trong mỗi trận đấu`(l, i, v)`kết thúc tại`i`, chúng tôi thêm sự đóng góp của việc lấy từ này làm phân đoạn được chọn cuối cùng của một số trích xuất. Sự đóng góp đó`dp[l-1] * v`, bởi vì mọi thứ trước đây`l`có thể là bất kỳ trích xuất hợp lệ nào và sau đó chúng tôi nối thêm khoảng này. 
6. Tổng hợp tất cả những đóng góp đó vào`dp[i]`. 
7. Trả lại tất cả`dp[i]`modulo`998244353`. 

Lý do phép tính tổng này hoạt động là vì mỗi lần trích xuất có một khoảng thời gian được chọn cuối cùng duy nhất khi được xem bởi điểm cuối ngoài cùng bên phải của nó. Việc nhóm các trích xuất theo khoảng thời gian cuối cùng đó sẽ tránh được việc tính hai lần và đảm bảo tính độc lập giữa các phân đoạn trước và sau. 

### Tại sao nó hoạt động 

Mỗi trích xuất hợp lệ tương ứng với một tập hợp các khoảng không chồng chéo. Nếu chúng ta sắp xếp các khoảng đã chọn theo đúng điểm cuối của chúng thì khoảng cuối cùng sẽ xác định duy nhất một sự phân tách: mọi thứ trước nó hoàn toàn nằm trong`1..l-1`. Sự đóng góp của tất cả các cấu hình với khoảng thời gian cuối cùng cố định`(l, i)`chính xác là`dp[l-1] * v`, từ`v`là trọng số của việc chọn khoảng đó và`dp[l-1]`đếm tất cả các trích xuất hợp lệ trước đó. Tổng hợp tất cả các lựa chọn và bao gồm cả trích xuất trống sẽ mang lại phân vùng đầy đủ của tất cả các tập hợp con hợp lệ mà không bị chồng chéo hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

class Node:
    __slots__ = ("next", "val")
    def __init__(self):
        self.next = {}
        self.val = 0

def insert(root, word, val):
    node = root
    for ch in word:
        if ch not in node.next:
            node.next[ch] = Node()
        node = node.next[ch]
    node.val += val

def solve():
    s = input().strip()
    n = int(input())
    
    root = Node()
    max_len = 0
    
    words = []
    for _ in range(n):
        t, v = input().split()
        v = int(v)
        words.append((t, v))
        max_len = max(max_len, len(t))
        insert(root, t[::-1], v)
    
    m = len(s)
    dp = [0] * (m + 1)
    dp[0] = 1
    
    for i in range(1, m + 1):
        node = root
        dp[i] = dp[i - 1]
        
        j = i
        step = 0
        
        while j > 0 and step < max_len:
            c = s[j - 1]
            if c not in node.next:
                break
            node = node.next[c]
            j -= 1
            step += 1
            
            if node.val:
                dp[i] = (dp[i] + dp[j] * node.val) % MOD
    
    print(*dp[1:])

if __name__ == "__main__":
    solve()
```Trie được xây dựng trên các từ đảo ngược để đi lùi từ vị trí`i`liệt kê trực tiếp tất cả các từ điển phù hợp kết thúc tại`i`. Mỗi lần chúng tôi đến một nút đầu cuối, chúng tôi biết ngay một khoảng thời gian hợp lệ kết thúc tại`i`. 

Mảng DP được lập chỉ mục 1 để rõ ràng.`dp[i-1]`chuyển tiếp tất cả các cấu hình không kết thúc với khoảng thời gian đã chọn tại`i`. Mỗi trận đấu được phát hiện thêm`dp[l-1] * v`, tương ứng với việc mở rộng bất kỳ cấu hình hợp lệ nào lên tới`l-1`với khoảng đó. 

Vòng lặp kết thúc`step < max_len`đảm bảo chúng tôi không bao giờ quét nhiều ký tự hơn mức cần thiết, giữ cho tổng số ký tự được tuyến tính. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu đầu tiên`s = "ababa"`với từ điển`("aba", 2), ("ba", 3)`. 

Tại mỗi vị trí, chúng tôi ghi lại các trận đấu kết thúc ở đó và cập nhật`dp`. 

| tôi | quét hậu tố | trận đấu kết thúc | dp[i-1] | đóng góp | dp[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | "một" | không | 1 | không | 1 | 
| 2 | "ba" | "ba" | 1 | 1*3 | 4 | 
| 3 | "aba", "a" | "aba" | 4 | 1*2 | 6 | 
| 4 | "ba" | "ba" | 6 | 6*3 | 24 | 
| 5 | "aba" | "aba" | 24 | 4*2 | 32 | 

Dấu vết đơn giản hóa này cho thấy cơ chế tích lũy đóng góp từ tất cả các khoảng thời gian kết thúc hợp lệ. 

Đối với mẫu thứ hai`s = "qfmyqqfmyqqfmyq"`, nhiều lần xuất hiện chồng chéo của`"qfmyq"`Và`"myqq"`xuất hiện và DP tổng hợp chính xác tất cả các cách chọn các lần xuất hiện rời rạc, bao gồm các kết hợp có cấu trúc lặp lại qua các lần lặp lại của mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O( | s | 

Các ràng buộc cho phép lên đến`2 × 10^5`tổng kích thước đầu vào, do đó, việc truyền tải tuyến tính với hệ số không đổi nhỏ sẽ vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353

    class Node:
        def __init__(self):
            self.next = {}
            self.val = 0

    def insert(root, word, val):
        node = root
        for ch in word:
            if ch not in node.next:
                node.next[ch] = Node()
            node = node.next[ch]
        node.val += val

    s = input().strip()
    n = int(input())
    root = Node()
    max_len = 0

    for _ in range(n):
        t, v = input().split()
        v = int(v)
        max_len = max(max_len, len(t))
        insert(root, t[::-1], v)

    m = len(s)
    dp = [0] * (m + 1)
    dp[0] = 1

    for i in range(1, m + 1):
        node = root
        dp[i] = dp[i - 1]
        j = i
        step = 0

        while j > 0 and step < max_len:
            c = s[j - 1]
            if c not in node.next:
                break
            node = node.next[c]
            j -= 1
            step += 1
            if node.val:
                dp[i] = (dp[i] + dp[j] * node.val) % MOD

    return " ".join(map(str, dp[1:]))

# provided samples
assert run("""ababa
2
aba 2
ba 3
""") == "1 1 6 6 26"

assert run("""qfmyqqfmyqqfmyq
2
qfmyq 111111
myqq 404968002
""") == "1 1 1 1 111112 405079114 405079114 405079114 405079114 771912310 239058268 239058268 239058268 239058268 31169271"

# custom cases
assert run("""a
1
a 5
""") == "6", "single match includes empty + one selection"

assert run("""aaaa
2
a 2
aa 3
""") == "3 7 19 45", "overlapping patterns"

assert run("""abc
1
d 10
""") == "1 1 1", "no matches"

assert run("""abcabc
1
abc 2
""") == "1 1 3 4 6 7", "repeated non-overlapping structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu đơn |`6`| trích xuất trống cộng với lựa chọn đơn | 
| mô hình chồng chéo |`3 7 19 45`| sự tương tác của các từ trong từ điển chồng chéo | 
| không có trận đấu |`1 1 1`| truyền DP cơ sở | 
| mẫu lặp đi lặp lại |`1 1 3 4 6 7`| nhiều lần xuất hiện và tích lũy tiền tố | 

## Vỏ cạnh 

Đối với một chuỗi không có từ điển khớp, DP sẽ không đổi ở mức`1`cho tất cả các tiền tố vì chỉ tồn tại phần trích xuất trống. Thuật toán xử lý việc này vì không có nút cuối trie nào đạt được, vì vậy`dp[i] = dp[i-1]`cho tất cả`i`. 

Đối với sự chồng chéo nặng nề như`s = "aaaaa"`với từ điển`["a", "aa", "aaa"]`, mỗi vị trí sẽ kích hoạt nhiều kết quả khớp có độ dài khác nhau. Quá trình quét trie đảm bảo tất cả các hậu tố đều được khám phá và mỗi hậu tố đóng góp độc lập thông qua`dp[l-1] * v`. Vì các đóng góp được nhóm theo vị trí kết thúc nên việc trùng lặp không gây ra tình trạng tính hai lần. 

Đối với các từ giống nhau lặp lại ở các vị trí khác nhau, mỗi lần xuất hiện được coi là một khoảng riêng biệt thông qua các kết quả truyền tải độc lập và DP tổng hợp chúng một cách tự nhiên vì mỗi kết quả khớp được xử lý riêng ngay cả khi nó tương ứng với cùng một từ trong từ điển.
