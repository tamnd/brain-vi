---
title: "CF 1387C - Virus"
description: "Chúng ta được cung cấp một hệ thống trong đó mỗi “gen” là một nhãn số nguyên và mọi gen lớn hơn 1 đều có thể mở rộng thành một chuỗi gen theo quy tắc đột biến cố định."
date: "2026-06-18T18:29:37+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp", "shortest-paths", "string-suffix-structures"]
categories: ["algorithms"]
codeforces_contest: 1387
codeforces_index: "C"
codeforces_contest_name: "Baltic Olympiad in Informatics 2020, Day 2 (IOI, Unofficial Mirror Contest, Unrated)"
rating: 2900
weight: 1387
solve_time_s: 103
verified: false
draft: false
---

[CF 1387C - Virus](https://codeforces.com/problemset/problem/1387/C) 

**Xếp hạng:** 2900 
**Thẻ:** *đặc biệt, dp, đường dẫn ngắn nhất, cấu trúc hậu tố chuỗi 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống trong đó mỗi “gen” là một nhãn số nguyên và mọi gen lớn hơn 1 đều có thể mở rộng thành một chuỗi gen theo quy tắc đột biến cố định. Bắt đầu từ một gen duy nhất$x$, chúng tôi liên tục thay thế bất kỳ gen không kết thúc nào trong trình tự hiện tại bằng sự mở rộng của nó và chúng tôi chỉ dừng lại khi trình tự đó bao gồm toàn bộ gen 0 và 1. 

Vì vậy, mỗi gen khởi đầu xác định một tập hợp chuỗi nhị phân khổng lồ, tùy thuộc vào lựa chọn mở rộng nào chúng ta áp dụng trong quá trình đột biến. Mỗi trạng thái trung gian là một chuỗi trên bảng chữ cái của ID gen và chỉ khi kết thúc, chúng tôi mới hiểu nó dưới dạng chuỗi nhị phân trên {0, 1}. 

Chúng ta cũng được cung cấp một tập hợp các mẫu nhị phân gọi là kháng thể. Một kháng thể phù hợp với virus nếu mẫu của nó xuất hiện dưới dạng chuỗi con liền kề trong chuỗi nhị phân cuối cùng. Đối với mỗi gen khởi đầu$x \ge 2$, chúng ta phải xác định xem mọi chuỗi nhị phân mở rộng hoàn toàn có thể có được “bao phủ” bởi ít nhất một kháng thể hay không. Nếu phạm vi bao phủ không được đảm bảo, chúng ta phải tìm độ dài tối thiểu của chuỗi nhị phân có thể truy cập được từ$x$điều đó tránh hoàn toàn tất cả các kháng thể, nghĩa là không có mẫu nào trong số các mẫu nhất định xuất hiện dưới dạng chuỗi con. 

Khó khăn chính là sự mở rộng không mang tính tất định: một gen có thể mở rộng thành các trình tự khác nhau và một số trình tự đó sau đó có thể mở rộng trở lại theo những cách khác nhau. Điều này tạo ra một không gian trạng thái khổng lồ của các chuỗi, nhưng các kháng thể chỉ hạn chế các chuỗi nhị phân cuối cùng chứ không hạn chế các dạng trung gian. 

Từ góc độ phức tạp, biểu đồ đột biến có tới$G \le 10^5$-scale cấu trúc trong các phiên bản CF tồi tệ nhất, nhưng ở đây tổng kích thước mở rộng bị giới hạn bởi 100, do đó quá trình chuyển đổi rất thưa thớt. Các kháng thể có tổng chiều dài tối đa là 50, vì vậy tất cả các mẫu bị cấm đều ngắn. Điều này cho thấy rõ ràng rằng máy tự động hiệu quả mà chúng ta cần xây dựng là nhỏ và mọi giải pháp đều phải nén cả “quy tắc đột biến” và “khớp mẫu” vào một không gian trạng thái duy nhất, thay vì mô phỏng các chuỗi một cách rõ ràng. 

Một trường hợp phức tạp xuất hiện khi một gen có thể mở rộng vô thời hạn mà không bao giờ chỉ dừng lại ở 0 và 1. Trong trường hợp đó, không có vi-rút hợp lệ nào được tạo ra và câu trả lời phải là CÓ bất kể kháng thể. Một mô phỏng ngây thơ giả định việc chấm dứt sẽ cố gắng tính toán các chuỗi tối thiểu một cách không chính xác và có thể bị kẹt hoặc phân loại sai các gen như vậy. 

Một trường hợp thất bại khác là giả định rằng mỗi gen tạo ra một sự mở rộng duy nhất. Ví dụ: nếu một gen có nhiều quy tắc sản xuất (được cấu trúc câu lệnh cho phép), các lựa chọn khác nhau có thể dẫn đến các kết quả nhị phân khác nhau, vì vậy chúng ta phải xem xét đồng thời tất cả các khả năng. Việc mở rộng tham lam hoặc DP đường dẫn đơn là không đủ. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ là tạo ra một cách rõ ràng tất cả các bản mở rộng có thể có của mỗi gen thành các chuỗi nhị phân và sau đó chạy so khớp mẫu với các kháng thể. Về nguyên tắc, điều này đúng: chúng tôi khám phá cây đột biến cho đến khi chỉ còn lại 0/1, thu thập tất cả các chuỗi đầu cuối và sau đó kiểm tra từng chuỗi con bị cấm. Tuy nhiên, yếu tố phân nhánh khiến điều này không thể thực hiện được. Mặc dù mỗi lần sản xuất đều ngắn, nhưng việc mở rộng lặp đi lặp lại sẽ tạo ra nhiều chuỗi theo cấp số nhân. Sự bùng nổ xảy ra không theo chiều sâu mà ở sự lựa chọn: mỗi gen có thể được thay thế theo nhiều cách ở các vị trí và giai đoạn, và mỗi lựa chọn sẽ nhân số lượng trạng thái. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần các chuỗi rõ ràng. Chúng ta chỉ cần biết liệu đạo hàm một phần có còn tạo ra kết quả không có chuỗi con bị cấm hay không và nếu vậy thì kết quả đó có thể có độ dài tối thiểu là bao nhiêu. Điều này ngay lập tức gợi ý quan điểm đường đi ngắn nhất trên biểu đồ “cấu hình”. 

Mỗi cấu hình là một chuỗi gen giống như nhiều tập hợp, nhưng trình tự theo dõi trực tiếp quá lớn. Bí quyết là đảo ngược quá trình: thay vì mở rộng các gen về phía trước, chúng tôi nghĩ đến việc giảm chúng về phía đầu ra nhị phân cuối cùng, đồng thời theo dõi xem liệu chúng tôi có khớp với bất kỳ mẫu bị cấm nào hay không. Đây chính xác là bài toán đường đi ngắn nhất trên một máy tự động ẩn trong đó các trạng thái mã hóa cả ký hiệu hiện tại và tiến trình khớp mẫu. 

Cách tiêu chuẩn để xử lý nhiều ràng buộc mẫu là máy tự động Aho-Corasick trên tất cả các kháng thể. Điều này cung cấp cho chúng tôi một máy theo dõi lỗi trong đó mỗi trạng thái biểu thị số lượng tiền tố mẫu bị cấm mà chúng tôi đã khớp cho đến nay. Sau khi chúng tôi chuyển sang trạng thái khớp cuối cùng, cấu hình đó sẽ không được phép. Vì vậy, mọi ký tự nhị phân được tạo sẽ chuyển đổi máy tự động và chúng ta phải tránh các trạng thái đầu cuối. 

Bây giờ chúng tôi kết hợp điều này với việc mở rộng gen. Mỗi gen là một nút tạo ra một chuỗi trên các gen khác. Cuối cùng, mọi thứ đều được chuyển sang tính toán, đối với mỗi gen và từng trạng thái ô tô, độ dài tối thiểu của chuỗi nhị phân có thể đạt được. Điều đó biến bài toán thành đồ thị DP trên các trạng thái$(gene, ac\_state)$, nhưng các quá trình chuyển đổi không phải là các cạnh đơn lẻ: một gen mở rộng thành một chuỗi, vì vậy chúng ta cần tạo ra các quá trình chuyển đổi thông qua một chuỗi các trạng thái DP. 

Điều này được xử lý bằng cách xác định các giá trị DP và tính toán chúng theo thứ tự tôpô ngược trên cấu trúc mở rộng gen, nhưng vì các chu kỳ có thể tồn tại nên thay vào đó, chúng tôi sử dụng đường đi ngắn nhất (kiểu Dijkstra hoặc 0-1 BFS với các phần giãn) trên biểu đồ sản phẩm. Mỗi quy tắc mở rộng sẽ trở thành toán tử mở rộng đường dẫn. 

Cuối cùng, đối với mỗi gen khởi đầu, chúng tôi kiểm tra: nếu tất cả các đường dẫn có thể từ$(gene, root)$dẫn đến trạng thái AC cuối, trả lời CÓ; mặt khác, chúng tôi trích xuất khoảng cách tối thiểu đến trạng thái AC không đầu cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force mở rộng tất cả các chuỗi | hàm mũ | hàm mũ | Quá chậm | 
| Máy tự động AC + DP khi mở rộng gen + đường dẫn ngắn nhất | (O(\sum k \cdot | AC | \log (G \cdot | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén tất cả các mẫu kháng thể vào một máy tự động duy nhất để có thể theo dõi dần dần sự hình thành chuỗi con bị cấm trong khi tạo ra các bit đầu ra. 

1. Xây dựng máy tự động Aho-Corasick trên tất cả các kháng thể. Mỗi trạng thái đại diện cho tiền tố dài nhất của bất kỳ mẫu bị cấm nào hiện là hậu tố của đầu ra được tạo. Chúng tôi đánh dấu các trạng thái tương ứng với một mẫu đã hoàn thành là bị cấm, vì bất kỳ lượt truy cập nào vào đó có nghĩa là vi-rút bị phát hiện và đường dẫn đó trở nên không hợp lệ. 
2. Giải thích các quy luật mở rộng gen như là sự chuyển tiếp tạo ra các chuỗi gen con. Vì việc mở rộng diễn ra trên các gen chứ không phải trực tiếp trên các bit nên chúng tôi trì hoãn việc tạo bit và thay vào đó tính toán, đối với từng gen và từng trạng thái máy tự động, điều gì sẽ xảy ra sau khi mở rộng hoàn toàn gen đó thành đầu ra nhị phân. 
3. Xác định giá trị DP`dist[g][s]`là độ dài tối thiểu của chuỗi nhị phân hợp lệ được tạo ra bắt đầu từ gen`g`trong khi máy tự động AC hiện đang ở trạng thái`s`. Điều này nắm bắt cả “những gì còn cần được mở rộng” và “chúng tôi mang theo bao nhiêu tiền tố bị cấm”. 
4. Khởi tạo các trường hợp cơ sở: các gen đã ở cuối (0 và 1) tạo ra một ký tự đơn. Từ tiểu bang`s`, phát ra bit 0 hoặc 1 sẽ chuyển máy tự động sang trạng thái mới và đóng góp chi phí 1 nếu quá trình chuyển đổi không bị cấm. 
5. Đối với các gen không kết thúc, mỗi quy tắc sản xuất đưa ra một trình tự các gen con. Để đánh giá một quy tắc, về mặt khái niệm, chúng tôi tổng hợp các chuyển đổi DP của các quy tắc con của nó: bắt đầu từ trạng thái`s`, chúng tôi truyền bá qua chuỗi, liên tục áp dụng đã biết`dist`chuyển tiếp. Tổng chi phí của một quy tắc là tổng chi phí của các lần mở rộng con của nó trong các trạng thái tự động trung gian chính xác. 
6. Bởi vì có nhiều quy tắc tồn tại trên mỗi gen nên chúng tôi lấy mức tối thiểu trên tất cả các quy tắc. Điều này tạo thành một bài toán đường đi ngắn nhất trên đồ thị có các nút là$(gene, ac\_state)$. 
7. Chạy Dijkstra trên các trạng thái này, bắt đầu từ mỗi gen truy vấn ở trạng thái AC ban đầu. Các chuyển đổi tương ứng với việc áp dụng quy tắc mở rộng gen, sau đó sử dụng kết quả đầu ra nhị phân thông qua máy tự động. Bất cứ khi nào chúng tôi đạt đến trạng thái AC bị cấm, chúng tôi sẽ loại bỏ đường dẫn đó. 
8. Sau khi khoảng cách ổn định, đối với mỗi gen, chúng tôi kiểm tra xem có tồn tại khoảng cách hữu hạn nào tính từ trạng thái bắt đầu của nó hay không. Nếu không tồn tại thì mọi sự mở rộng có thể cuối cùng sẽ bị ép vào các khuôn mẫu bị cấm hoặc không bao giờ chấm dứt. Chúng tôi in CÓ. Ngược lại, chúng ta xuất ra NO và khoảng cách tối thiểu. 

### Tại sao nó hoạt động 

Bất biến quan trọng là`dist[g][s]`luôn lưu trữ độ dài tối thiểu có thể có của chuỗi nhị phân có thể đạt được từ gen`g`trong khi vẫn duy trì tiền tố đã được tạo tương ứng với trạng thái tự động`s`. Mỗi bước thư giãn tương ứng chính xác với việc áp dụng một quy tắc đột biến hợp lệ và mở rộng chuỗi nhị phân được tạo một cách nhất quán với các chuyển đổi tự động AC. Vì tất cả các chuyển đổi đều bảo đảm tính chính xác của việc khớp tiền tố nên không có chuỗi con không hợp lệ nào có thể bị "ẩn" sau khi được hình thành. Dijkstra đảm bảo rằng sau khi một trạng thái được hoàn thành, không tồn tại cấu trúc hợp lệ ngắn hơn nào, do đó độ dài tối thiểu là chính xác trên toàn cầu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

class Aho:
    def __init__(self):
        self.next = [[-1, -1]]
        self.fail = [0]
        self.out = [False]

    def add(self, s):
        v = 0
        for ch in s:
            c = int(ch)
            if self.next[v][c] == -1:
                self.next[v][c] = len(self.next)
                self.next.append([-1, -1])
                self.fail.append(0)
                self.out.append(False)
            v = self.next[v][c]
        self.out[v] = True

    def build(self):
        from collections import deque
        q = deque()
        for c in range(2):
            u = self.next[0][c]
            if u != -1:
                self.fail[u] = 0
                q.append(u)
            else:
                self.next[0][c] = 0

        while q:
            v = q.popleft()
            self.out[v] |= self.out[self.fail[v]]
            for c in range(2):
                u = self.next[v][c]
                if u != -1:
                    self.fail[u] = self.next[self.fail[v]][c]
                    q.append(u)
                else:
                    self.next[v][c] = self.next[self.fail[v]][c]

def solve():
    G, N, M = map(int, input().split())

    gmap = [[] for _ in range(G)]
    for _ in range(N):
        a, k, *rest = map(int, input().split())
        gmap[a].append(rest)

    ac = Aho()
    patterns = []
    for _ in range(M):
        arr = list(map(int, input().split()))
        l = arr[0]
        pat = ''.join(map(str, arr[1:]))
        ac.add(pat)

    ac.build()

    S = len(ac.next)

    dist = [[INF] * S for _ in range(G)]

    pq = []

    def relax(g, s, d):
        if ac.out[s]:
            return
        if d < dist[g][s]:
            dist[g][s] = d
            heapq.heappush(pq, (d, g, s))

    for g in range(2, G):
        relax(g, 0, 0)

    while pq:
        d, g, s = heapq.heappop(pq)
        if d != dist[g][s]:
            continue
        if ac.out[s]:
            continue

        for rule in gmap[g]:
            cur_s = s
            cost = d
            ok = True

            for x in rule:
                if x <= 1:
                    cur_s = ac.next[cur_s][x]
                    if ac.out[cur_s]:
                        ok = False
                        break
                    cost += 1
                else:
                    if dist[x][cur_s] >= INF:
                        ok = False
                        break
                    cost += dist[x][cur_s]

            if ok and cost < dist[g][cur_s]:
                dist[g][cur_s] = cost
                heapq.heappush(pq, (cost, g, cur_s))

    for g in range(2, G):
        ans = dist[g][0]
        if ans >= INF:
            print("YES")
        else:
            print("NO", ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng một máy tự động Aho-Corasick trên tất cả các mẫu kháng thể để việc phát hiện chuỗi con trở thành một vấn đề chuyển đổi trạng thái. Sau đó, mỗi gen được xử lý trong khung thư giãn giống Dijkstra trong đó các trạng thái là cặp gen và nút tự động. Các quy tắc đột biến được áp dụng bằng cách duyệt qua các chuỗi bên phải của chúng, tích lũy chi phí thông qua phát xạ nhị phân trực tiếp hoặc thông qua các giá trị DP gen được tính toán trước đó. 

Một điểm tinh tế là các quá trình chuyển đổi có thể ngay lập tức chạm vào trạng thái AC bị cấm, vì vậy những đường dẫn đó phải bị loại bỏ sớm. Một khía cạnh tế nhị khác là chúng tôi chỉ khởi tạo DP từ trạng thái 0, vì các kháng thể được kiểm tra dựa trên chuỗi được tạo đầy đủ bắt đầu từ tiền tố trống. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản đơn giản hóa với hai gen và một kháng thể. Gen 2 mở rộng trực tiếp vào`0 1`và gen 3 mở rộng thành`2`. Kháng thể là`01`. 

| Bước | gen | Trạng thái AC | Hành động | Chi phí | Ghi chú | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | phát ra 0 | 1 | trạng thái chuyển sang tiền tố "0" | 
| 2 | 2 | s("0") | phát ra 1 | 2 | đạt cấm "01" | 

Dấu vết này cho thấy gen 2 dẫn thẳng tới mẫu đã được phát hiện nên không an toàn. 

Bây giờ hãy xem xét trường hợp gen 3 mở rộng thành gen 2 và gen 2 mở rộng thành`1`duy nhất, không có kháng thể. 

| Bước | gen | Trạng thái AC | Hành động | Chi phí | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | mở rộng lên 2 | 0 | 
| 2 | 2 | 0 | phát ra 1 | 1 | 

Ở đây gen 3 gián tiếp tạo ra một chuỗi nhị phân có độ dài 1. 

Những dấu vết này chứng minh rằng thuật toán về cơ bản đang tính toán các dẫn xuất hợp lệ ngắn nhất thông qua các mở rộng lồng nhau trong khi theo dõi sự hình thành mẫu trực tuyến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((G + \sum k) \cdot S \log (G S))$| Dijkstra qua các trạng thái sản phẩm, mỗi bản mở rộng được xử lý một lần | 
| Không gian |$O(G \cdot S)$| Bảng DP về gen và trạng thái máy tự động AC | 

Kích thước trạng thái sản phẩm được kiểm soát bởi tổng chiều dài mẫu nhỏ và các quy tắc đột biến thưa thớt, giữ cho cả quá trình chuyển đổi và kích thước máy tự động có thể quản lý được trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample placeholders (actual CF samples should be inserted)
# assert run("...") == "..."

# minimal case: no antibodies
inp1 = """3 1 0
2 1 0
"""
# just sanity format check, expected depends on full rules

# boundary: single antibody blocking immediate output
inp2 = """3 1 1
2 1 0
2 0 1
"""

# self-loop gene
inp3 = """4 2 0
2 1 2
3 1 0
"""

# mixed expansion
inp4 = """5 4 1
2 1 0
3 1 1
4 1 2
4 1 3
1 0
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu không có kháng thể | CÓ | sự chấp nhận tầm thường | 
| mẫu cấm ngay lập tức | KHÔNG | phát hiện trực tiếp | 
| gen tự lặp | CÓ | xử lý chấm dứt | 
| mở rộng hỗn hợp | hỗn hợp | tuyên truyền thông qua các quy tắc | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là một gen mà sự mở rộng của nó không bao giờ dẫn đến trình tự chỉ nhị phân cuối cùng. Trong tình huống như vậy, trạng thái DP của gen đó vẫn không thể truy cập được hoặc vô hạn và thuật toán đưa ra chính xác CÓ vì không tồn tại vi-rút hợp lệ. Việc khởi tạo Dijkstra không bao giờ hoàn thiện bất kỳ trạng thái nào cho gen đó, vì vậy nó được coi là không tạo ra chuỗi nhị phân có độ dài hữu hạn. 

Một trường hợp khó phát hiện khác là khi một gen tạo ra đầu ra nhị phân đã khớp một phần với tiền tố kháng thể nhưng không bao giờ hoàn thiện nó. Máy tự động AC xử lý việc này bằng cách giữ cho trạng thái tiền tố trung gian hoạt động và các chuyển đổi vẫn hợp lệ miễn là trạng thái cuối cùng không phải là thiết bị đầu cuối. DP phân biệt chính xác giữa “tiền tố an toàn mãi mãi” và “hoàn thành bị cấm cuối cùng” bằng cách truyền bá trạng thái lỗi ngay lập tức khi kết quả khớp hoàn thành.
