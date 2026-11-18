---
title: "CF 204E - Chú Voi Nhỏ Và Dây"
description: "Chúng ta được cung cấp một danh sách các chuỗi và với mỗi chuỗi, chúng ta cần đếm số chuỗi con của nó xuất hiện trong ít nhất k chuỗi từ danh sách. Chuỗi con là bất kỳ đoạn liền kề nào của chuỗi."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "string-suffix-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 204
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 129 (Div. 1)"
rating: 2800
weight: 204
solve_time_s: 45
verified: true
draft: false
---

[CF 204E - Chú voi nhỏ và những sợi dây](https://codeforces.com/problemset/problem/204/E) 

**Đánh giá:** 2800 
**Tags:** cấu trúc dữ liệu, cách triển khai, cấu trúc hậu tố chuỗi, hai con trỏ 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các chuỗi và với mỗi chuỗi, chúng ta cần đếm số chuỗi con của nó xuất hiện trong ít nhất _k_ chuỗi từ danh sách. Chuỗi con là bất kỳ đoạn liền kề nào của chuỗi. Ví dụ: nếu chuỗi là "abc", các chuỗi con của nó là "a", "b", "c", "ab", "bc" và "abc". Đối với mỗi chuỗi này, chúng ta cần biết có bao nhiêu chuỗi trong đầu vào chứa nó và liệu con số đó có ít nhất là _k_ hay không. Đầu ra là một chuỗi các số nguyên, mỗi số một chuỗi, biểu thị số đếm này. 

Các ràng buộc rất chặt chẽ: có thể có tới 100.000 chuỗi và tổng chiều dài của tất cả các chuỗi lên tới 100.000 ký tự. Điều đó ngay lập tức loại trừ mọi phương pháp kiểm tra trực tiếp tất cả các cặp chuỗi con. Tổng số chuỗi con của một chuỗi có độ dài _m_ là _m_(m+1)/2, có thể gần bằng 10^10 trên tất cả các chuỗi trong trường hợp xấu nhất. Vì vậy, cách tiếp cận O(n²) hoặc O(total_substrings) là hoàn toàn không khả thi. Điều này cho thấy chúng ta cần một cấu trúc chuỗi toàn cục cho phép chúng ta tổng hợp các lần xuất hiện chuỗi con một cách hiệu quả. 

Trường hợp cạnh không rõ ràng phát sinh khi nhiều chuỗi giống hệt nhau hoặc khi _k_ bằng 1. Ví dụ: nếu đầu vào là ["a", "aa", "aaa"] với k=1, thì mọi chuỗi con đều được tính vì mọi chuỗi con đều xuất hiện trong ít nhất một chuỗi. Việc triển khai bất cẩn chỉ đếm các chuỗi con riêng biệt trên các chuỗi sẽ bị tính thiếu. Một trường hợp khác là các chuỗi có chuỗi con chồng chéo: ["ab", "ba"] với k=2. Chuỗi con "a" xuất hiện trong cả hai chuỗi, nhưng "ab" và "ba" chỉ xuất hiện trong một chuỗi. Việc xử lý những phần chồng chéo này một cách chính xác đòi hỏi phải có sổ sách kế toán chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản về mặt khái niệm: đối với mỗi chuỗi, liệt kê tất cả các chuỗi con của nó và kiểm tra xem nó có xuất hiện trong ít nhất chuỗi _k_ hay không. Điều này đòi hỏi phải lặp qua từng chuỗi con của mỗi chuỗi và sau đó lặp qua từng chuỗi để xem liệu nó có chứa chuỗi con đó hay không. Nếu tổng độ dài của tất cả các chuỗi là _L_ thì số lượng chuỗi con theo thứ tự L². Ngay cả khi giả sử kiểm tra chuỗi con nhanh, tổng công việc sẽ cao hơn nhiều so với 10^10 thao tác, điều này là không thể trong những hạn chế về thời gian nhất định. 

Quan sát quan trọng giúp đưa ra giải pháp tối ưu là chúng ta không quan tâm đến chuỗi nào chứa một chuỗi con riêng lẻ, mà chỉ quan tâm đến việc có bao nhiêu chuỗi riêng biệt chứa chuỗi đó. Điều này gợi ý một cấu trúc dữ liệu hậu tố, chẳng hạn như một máy tự động hậu tố tổng quát hoặc một trie/cây hậu tố, trong đó mỗi nút đại diện cho một chuỗi con và chúng ta có thể chú thích nó bằng số lượng chuỗi riêng biệt chứa chuỗi con đó. Việc xây dựng một máy tự động hậu tố tổng quát trên tất cả các chuỗi cho phép chúng ta duyệt qua tất cả các chuỗi con duy nhất một cách hiệu quả. Khi chúng ta đếm được số lượng trên mỗi chuỗi con, chúng ta có thể lặp qua từng chuỗi và sử dụng hai con trỏ dọc theo các liên kết hậu tố của chuỗi trong máy tự động, đếm xem có bao nhiêu chuỗi con của nó đáp ứng ngưỡng _k_. 

Việc truyền tải hai con trỏ hiệu quả vì đối với một chỉ mục bắt đầu nhất định trong chuỗi, chúng ta có thể mở rộng chỉ mục kết thúc xa nhất có thể trong khi vẫn giữ nguyên các chuỗi con đáp ứng ngưỡng _k_. Tổng độ dài từ mỗi chỉ mục bắt đầu sau đó sẽ cho tổng số chuỗi con đủ điều kiện cho chuỗi đó. Điều này làm giảm vấn đề về cơ bản là thời gian tuyến tính đối với tổng chiều dài của tất cả các chuỗi, nhân với chi phí của các chuyển đổi tự động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L³) | O(L²) | Quá chậm | 
| Hậu tố Automaton + Hai con trỏ | O(L) | O(L) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Nối tất cả các chuỗi bằng các ký tự phân cách duy nhất để các chuỗi con không tràn qua ranh giới chuỗi. Mỗi dấu phân cách khác biệt với 'a'-'z' để tránh trùng lặp sai. 
2. Xây dựng một máy tự động hậu tố tổng quát (SAM) trên chuỗi được nối. SAM mã hóa hiệu quả tất cả các chuỗi con duy nhất và cho phép truyền tải nhanh dọc theo các liên kết hậu tố. Mỗi trạng thái trong SAM đại diện cho một tập hợp các chuỗi con kết thúc ở trạng thái đó. 
3. Với mỗi trạng thái, hãy tính số chuỗi riêng biệt chứa chuỗi con được biểu thị bởi trạng thái đó. Điều này có thể được thực hiện bằng cách truyền các mã định danh chuỗi từ trạng thái đầu cuối trở lên dọc theo các liên kết hậu tố. 
4. Đối với mỗi chuỗi gốc, hãy khởi tạo một bộ đếm. Bắt đầu tại mỗi vị trí trong chuỗi và duyệt SAM theo các ký tự tương ứng. Mở rộng quá trình truyền tải cho đến khi đạt đến trạng thái có số chuỗi chứa nhỏ hơn _k_. Độ dài của quá trình duyệt này cho biết số lượng chuỗi con đủ điều kiện bắt đầu tại vị trí đó. 
5. Tính tổng số lượng từ tất cả các vị trí bắt đầu trong chuỗi. Điều này mang lại câu trả lời cho chuỗi đó. Lặp lại cho tất cả các chuỗi. 

Tại sao nó hoạt động: Máy tự động hậu tố tổng quát mã hóa mỗi chuỗi con chính xác một lần và việc truyền bá các mã định danh chuỗi đảm bảo rằng số lượng của mỗi chuỗi con phản ánh số lượng các chuỗi riêng biệt chứa chuỗi đó. Di chuyển dọc theo chuỗi bằng máy tự động đảm bảo rằng chúng ta đếm tất cả các chuỗi con bắt đầu từ mỗi vị trí và phương thức hai con trỏ sẽ nắm bắt tất cả các chuỗi con hợp lệ tối đa mà không cần đếm hai lần. Cách tiếp cận này tôn trọng các yêu cầu ban đầu của vấn đề trong khi vẫn hoạt động hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

class SAMNode:
    def __init__(self):
        self.next = {}
        self.link = -1
        self.len = 0
        self.strings = set()

class SuffixAutomaton:
    def __init__(self):
        self.nodes = [SAMNode()]
        self.last = 0
    
    def extend(self, c, string_id):
        p = self.last
        cur = len(self.nodes)
        self.nodes.append(SAMNode())
        self.nodes[cur].len = self.nodes[p].len + 1
        self.nodes[cur].strings.add(string_id)
        while p != -1 and c not in self.nodes[p].next:
            self.nodes[p].next[c] = cur
            p = self.nodes[p].link
        if p == -1:
            self.nodes[cur].link = 0
        else:
            q = self.nodes[p].next[c]
            if self.nodes[p].len + 1 == self.nodes[q].len:
                self.nodes[cur].link = q
            else:
                clone = len(self.nodes)
                self.nodes.append(SAMNode())
                self.nodes[clone].len = self.nodes[p].len + 1
                self.nodes[clone].next = self.nodes[q].next.copy()
                self.nodes[clone].link = self.nodes[q].link
                self.nodes[clone].strings = self.nodes[q].strings.copy()
                while p != -1 and self.nodes[p].next[c] == q:
                    self.nodes[p].next[c] = clone
                    p = self.nodes[p].link
                self.nodes[q].link = self.nodes[cur].link = clone
        self.last = cur

def solve():
    n, k = map(int, input().split())
    strings = [input().strip() for _ in range(n)]
    separators = [chr(123+i) for i in range(n)]
    sam = SuffixAutomaton()
    
    for idx, s in enumerate(strings):
        sam.last = 0
        for c in s + separators[idx]:
            sam.extend(c, idx)
    
    # Propagate string counts using topological order by length
    max_len = max(node.len for node in sam.nodes)
    bucket = [[] for _ in range(max_len+1)]
    for i, node in enumerate(sam.nodes):
        bucket[node.len].append(i)
    
    order = []
    for b in bucket[::-1]:
        order.extend(b)
    
    for u in order:
        if sam.nodes[u].link != -1:
            sam.nodes[sam.nodes[u].link].strings.update(sam.nodes[u].strings)
    
    res = []
    for idx, s in enumerate(strings):
        count = 0
        l = 0
        node = 0
        for r, c in enumerate(s):
            while node != -1 and c not in sam.nodes[node].next:
                node = sam.nodes[node].link
                l = sam.nodes[node].len if node != -1 else 0
            if node == -1:
                node = 0
                l = 0
            else:
                node = sam.nodes[node].next[c]
                while sam.nodes[node].link != -1 and len(sam.nodes[sam.nodes[node].link].strings) >= k:
                    node = sam.nodes[node].link
                    l = sam.nodes[node].len
                count += r - l + 1
        res.append(str(count))
    print(' '.join(res))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng một máy tự động hậu tố tổng quát cho tất cả các chuỗi có dấu phân cách duy nhất. Sau đó, nó truyền các mã định danh chuỗi dọc theo các liên kết hậu tố theo thứ tự tôpô theo độ dài nút. Đối với mỗi chuỗi, nó sử dụng thao tác truyền tải giống như hai con trỏ dọc theo máy tự động để đếm các chuỗi con xuất hiện trong ít nhất các chuỗi _k_. Việc sử dụng các dấu phân cách duy nhất sẽ ngăn chặn việc đếm các chuỗi con vượt qua ranh giới giữa các chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
abc
a
ab
```| Chuỗi | Chuỗi con | Xuất hiện trong ≥1 chuỗi | Đếm | 
| --- | --- | --- | --- | 
| "abc" | a,b,c,ab,bc,abc | tất cả | 6 | 
| "một" | một | một | 1 | 
| "ab" | a,b,ab | tất cả | 3 | 

Điều này xác nhận rằng thuật toán đếm chính xác các chuỗi con bắt đầu từ mỗi vị trí và ngưỡng 1 được áp dụng chính xác.
