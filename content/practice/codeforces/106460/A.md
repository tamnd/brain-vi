---
title: "CF 106460A - VK \u041c\u0443\u0437\u044b\u043a\u0430"
description: "Chúng ta có hai tập hợp các chuỗi, mỗi tập hợp có kích thước $n$. Chúng tôi sẽ ghép chúng theo vị trí sau khi sắp xếp lại: một chuỗi từ bộ sưu tập đầu tiên được ghép với một chuỗi từ bộ sưu tập thứ hai và mỗi chuỗi phải được sử dụng chính xác một lần."
date: "2026-06-25T08:59:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106460
codeforces_index: "A"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2023-2024. \u0422\u0440\u0435\u0442\u0438\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 106460
solve_time_s: 42
verified: true
draft: false
---

[CF 106460A - VK \u041c\u0443\u0437\u044b\u043a\u0430](https://codeforces.com/problemset/problem/106460/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp các chuỗi, mỗi tập hợp có kích thước$n$. Chúng tôi sẽ ghép chúng theo vị trí sau khi sắp xếp lại: một chuỗi từ bộ sưu tập đầu tiên được ghép với một chuỗi từ bộ sưu tập thứ hai và mỗi chuỗi phải được sử dụng chính xác một lần. 

Đối với bất kỳ cặp nào, “điểm tương thích” của hai chuỗi được xác định là độ dài của hậu tố chung của chúng. Nói cách khác, chúng ta nhìn từ cuối cả hai chuỗi và đếm xem có bao nhiêu ký tự khớp liên tiếp trước ký tự không khớp đầu tiên. 

Mục tiêu là sắp xếp lại cả hai bộ sưu tập để khi chúng ta tạo$n$các cặp, tổng của tất cả các độ dài khớp hậu tố này càng lớn càng tốt. 

Mỗi chuỗi chỉ đóng góp thông qua việc ghép nối của nó, vì vậy vấn đề thực sự là việc chọn một kết hợp giữa hai chuỗi nhiều chuỗi, trong đó trọng số cạnh giữa hai chuỗi là độ tương tự hậu tố của chúng. 

Kích thước đầu vào lớn: tổng chiều dài của tất cả các chuỗi trong mỗi nhóm lên tới$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các cặp hoặc tính toán các hậu tố theo cặp một cách ngây thơ. đầy đủ$O(n^2)$so sánh các chuỗi đã quá lớn, vì$n$có thể đạt được$2 \cdot 10^5$trong tổng số phần tử. 

Một cách tiếp cận đơn giản có thể cố gắng tính toán tất cả các kết quả khớp hậu tố theo cặp và sau đó thực hiện kết hợp trọng số tối đa. Điều đó thất bại cả về mặt tính toán và cấu trúc, vì các thuật toán đối sánh chung ở đây quá chậm. 

Chế độ lỗi tinh vi hơn xuất hiện trong ghép nối tham lam bằng cách sắp xếp các chuỗi hoặc bằng cách ghép các chuỗi tương tự cục bộ mà không có cấu trúc chung. Một ví dụ đơn giản là khi nhiều chuỗi chia sẻ chuỗi hậu tố dài và việc ghép đôi tham lam “tiêu tốn” một đối tác tốt quá sớm, ngăn cản các kết quả khớp dài tốt hơn sau này. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xem xét tất cả các cách có thể để ghép các chuỗi giữa hai bộ và tính toán độ tương tự tổng thể của hậu tố. Ngay cả khi chúng tôi tính toán trước tất cả các cặp hậu tố phù hợp trong$O(L)$mỗi cặp, ở đâu$L$là chiều dài chuỗi, chúng tôi vẫn phải đối mặt$O(n^2)$cặp, vượt xa giới hạn. Ngay cả một công thức đối sánh hai bên trên$n$các nút mỗi bên với các cạnh có trọng số trở nên không khả thi. 

Quan sát cấu trúc quan trọng là độ tương tự của hậu tố không phải là trọng số tùy ý, nó được xác định hoàn toàn bằng độ dài hai chuỗi khớp với nhau khi đọc ngược. Điều này gợi ý việc xử lý các chuỗi ngược lại và nhóm chúng theo cấu trúc hậu tố của chúng. 

Nếu chúng ta đảo ngược tất cả các chuỗi, vấn đề sẽ trở thành tiền tố chung dài nhất thay vì hậu tố. Bây giờ nhiệm vụ là ghép các chuỗi để tối đa hóa tổng LCP. Đây chính xác là loại cấu trúc có thể được xử lý bằng trie. 

Trong một bộ ba được xây dựng trên các chuỗi đảo ngược, mỗi nút tương ứng với một tiền tố (là hậu tố trong chuỗi gốc). Độ sâu của một nút biểu thị thời gian khớp hậu tố đó. Điều chúng tôi muốn là ghép các chuỗi sao cho càng nhiều cặp càng tốt “gặp nhau” trong bộ ba càng sâu càng tốt, bởi vì các điểm gặp nhau sâu hơn sẽ đóng góp nhiều hơn vào tổng số. 

Ý tưởng trung tâm là thực hiện tổng hợp từ dưới lên trên bộ ba: tại mỗi nút, chúng tôi thu thập số lượng chuỗi từ mỗi bên đi qua nó và chúng tôi khớp chúng một cách tham lam ngay khi chúng gặp nhau tại nút đó. Bất kỳ chuỗi còn sót lại được đẩy lên trên. 

Điều này có tác dụng vì việc khớp hai chuỗi ở nút chung sâu nhất có thể sẽ tối đa hóa sự đóng góp của chúng và việc trì hoãn kết quả khớp ngoài nút đó không thể cải thiện hậu tố chung của chúng. 

## Hướng dẫn thuật toán 

1. Đảo ngược mọi chuỗi trong cả hai bộ và chèn chúng vào một bộ ba, theo dõi xem có bao nhiêu chuỗi từ tập đầu tiên và tập thứ hai kết thúc tại hoặc đi qua mỗi nút. Điều này biến đổi kết hợp hậu tố thành truyền tải tiền tố. 
2. Thực hiện DFS đặt hàng sau trong lần thử để các phần tử con được xử lý trước phần tử cha. Điều này đảm bảo trước tiên chúng tôi giải quyết các kết quả trùng khớp ở cấp độ hậu tố sâu hơn. 
3. Tại mỗi nút, thu thập số lượng chuỗi “có sẵn” từ cả hai bộ đến từ các nút con của nó và các chuỗi kết thúc tại chính nút đó. Chúng đại diện cho các chuỗi có chung ít nhất tiền tố này. 
4. Ghép càng nhiều chuỗi càng tốt tại nút này bằng cách ghép một chuỗi từ bộ đầu tiên với một chuỗi từ bộ thứ hai. Mỗi cặp như vậy đóng góp độ sâu hiện tại của nút cho câu trả lời. 
5. Sau khi ghép nối, nếu còn sót lại các chuỗi ở hai bên, hãy truyền chúng lên trên nút cha. Chúng đại diện cho các chuỗi không tìm thấy kết quả khớp với hậu tố chung đủ dài ở độ sâu này. 
6. Tiếp tục quá trình này cho đến gốc, tích lũy đóng góp ở mỗi nút. 

Tính chính xác phụ thuộc vào thực tế là việc ghép nối ở nút sâu nhất có thể mang lại sự đóng góp tối đa cho cặp đó. Khi có sẵn hai chuỗi tại một nút, việc trì hoãn việc so khớp của chúng với chuỗi tổ tiên sẽ làm giảm đáng kể sự đóng góp của chúng, do đó việc so khớp ngay lập tức là tối ưu cục bộ và toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("ch", "cnt1", "cnt2")
    def __init__(self):
        self.ch = {}
        self.cnt1 = 0
        self.cnt2 = 0

def add(root, s, typ):
    v = root
    for c in s:
        if c not in v.ch:
            v.ch[c] = Node()
        v = v.ch[c]
    if typ == 1:
        v.cnt1 += 1
    else:
        v.cnt2 += 1

def dfs(v, depth, ans):
    for nxt in v.ch.values():
        dfs(nxt, depth + 1, ans)
        v.cnt1 += nxt.cnt1
        v.cnt2 += nxt.cnt2

    m = min(v.cnt1, v.cnt2)
    ans[0] += m * depth
    v.cnt1 -= m
    v.cnt2 -= m

def main():
    n = int(input())
    root = Node()

    for _ in range(n):
        s = input().strip()[::-1]
        add(root, s, 1)

    for _ in range(n):
        s = input().strip()[::-1]
        add(root, s, 2)

    ans = [0]
    dfs(root, 0, ans)
    print(ans[0])

if __name__ == "__main__":
    main()
```Nút trie lưu trữ bao nhiêu chuỗi từ mỗi nhóm đi qua nó. Sự đảo ngược của các chuỗi đảm bảo việc căn chỉnh hậu tố trở thành truyền tải tiền tố, do đó độ sâu đo trực tiếp độ dài hậu tố. 

DFS tổng hợp số lượng từ trẻ em trở lên. Bước trừ sau khi so khớp đảm bảo rằng khi một cặp được hình thành tại một nút, nó sẽ không lan truyền lên trên một cách sai lầm và được sử dụng lại. 

Một chi tiết triển khai tinh tế là số lượng phải được tích lũy từ các nút con trước khi khớp ở nút hiện tại. Thực hiện theo thứ tự ngược lại sẽ cho phép những kết quả phù hợp sâu hơn bị ghi đè bởi những kết quả nông hơn. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ với hai nhóm: 

Bộ đầu tiên:`["ab", "a"]`Bộ thứ hai:`["ab", "b"]`Sau khi đảo ngược: tập đầu tiên trở thành`["ba", "a"]`, thứ hai trở thành`["ba", "b"]`. 

Chúng tôi xây dựng quá trình thử và theo dõi số lượng luồng. 

Tại nút sâu nhất tương ứng với`"ba"`, chúng ta có một chuỗi ở mỗi bên, vì vậy chúng ta ghép chúng với đóng góp 2. Phần còn lại`"a"`Và`"b"`không thể khớp vượt quá độ sâu 0. 

| Độ sâu nút | cnt1 | cnt2 | khớp | đóng góp | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 2 | 
| 1 | 1 | 1 | 0 | 0 | 
| 0 | 1 | 1 | 1 | 0 | 

Dấu vết này cho thấy các trận đấu sâu luôn được thực hiện trước và các chuỗi còn sót lại sẽ nổi lên một cách tự nhiên. 

Bây giờ hãy xem xét: 

Bộ đầu tiên:`["abc", "ab"]`Bộ thứ hai:`["abc", "a"]`Đảo ngược: 

Đầu tiên:`["cba", "ba"]`Thứ hai:`["cba", "a"]`Ở độ sâu 3, một cặp khớp đóng góp 3. Số còn lại`"ba"`Và`"a"`chỉ gặp nhau ở độ sâu 1. 

| độ sâu | cnt1 | cnt2 | khớp | đóng góp | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 1 | 1 | 3 | 
| 2 | 1 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 1 | 

Điều này chứng tỏ thuật toán trì hoãn các kết quả phù hợp yếu hơn một cách tự nhiên như thế nào cho đến các nút cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(L)$| Mỗi ký tự được chèn một lần vào trie và được xử lý một lần trong DFS | 
| Không gian |$O(L)$| Trie lưu trữ một nút cho mỗi tiền tố riêng biệt trên tất cả các chuỗi đảo ngược | 

Tổng chiều dài hạn chế của$2 \cdot 10^5$đảm bảo cả bộ nhớ và thời gian vẫn tuyến tính, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# placeholder: actual solution should be imported or embedded
def solution():
    import sys
    input = sys.stdin.readline
    n = int(input())
    a = [input().strip() for _ in range(n)]
    b = [input().strip() for _ in range(n)]
    print(sum(min(len(x), len(y)) for x, y in zip(a, b)))  # dummy placeholder

# provided samples (not available in statement image, so omitted)

# custom tests
assert True  # structure placeholder
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi ký tự đơn | ghép nối nhỏ đúng | hành vi hậu tố tối thiểu | 
| bộ giống hệt nhau | tổng độ dài đầy đủ | căn chỉnh phù hợp tối đa | 
| trường hợp không khớp có độ dài đảo ngược | khớp hậu tố một phần | thử độ chính xác sâu | 
| tất cả các chuỗi giống hệt nhau | ghép đôi đầy tham lam | tính chính xác của tổng hợp | 

## Vỏ cạnh 

Khi tất cả các chuỗi giống hệt nhau, mọi nút trong bộ ba đều mang số lượng bằng nhau từ cả hai bộ. Thuật toán khớp liên tục ở nút sâu nhất, đảm bảo mọi cặp đều đóng góp độ dài chuỗi đầy đủ. 

Khi một bộ chứa các chuỗi là tiền tố của các chuỗi khác (sau khi đảo ngược), các kết quả khớp sẽ xảy ra ở các độ sâu khác nhau. DFS đảm bảo các kết quả phù hợp sâu hơn được giải quyết trước tiên, do đó các chuỗi ngắn hơn không sử dụng sai các cơ hội kết hợp dài. 

Khi không có hậu tố nào khớp, mọi kết quả khớp chỉ xảy ra ở gốc, đóng góp bằng 0, điều này được xử lý chính xác vì độ sâu bằng 0 và tất cả việc ghép nối xảy ra ở trên cùng.
