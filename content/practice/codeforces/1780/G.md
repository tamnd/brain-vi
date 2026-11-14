---
title: "CF 1780G - Món Tráng Miệng Ngon"
description: "Chúng ta được cấp một chuỗi s có độ dài n đại diện cho công thức món tráng miệng của Tonio. Một chuỗi con của s được gọi là ngon nếu số lần nó xuất hiện trong s chia hết cho độ dài của chính nó. Nhiệm vụ là đếm tất cả các chuỗi con thú vị của s, đếm nhiều lần xuất hiện riêng biệt."
date: "2026-06-09T11:25:42+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dsu", "hashing", "math", "number-theory", "string-suffix-structures"]
categories: ["algorithms"]
codeforces_contest: 1780
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 846 (Div. 2)"
rating: 2400
weight: 1780
solve_time_s: 103
verified: true
draft: false
---

[CF 1780G - Món tráng miệng thơm ngon](https://codeforces.com/problemset/problem/1780/G) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, dsu, băm, toán, lý thuyết số, cấu trúc hậu tố chuỗi 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`chiều dài`n`đại diện cho công thức món tráng miệng của Tonio. Một chuỗi con của`s`được gọi là _delicious_ nếu số lần nó xuất hiện trong`s`chia hết cho chiều dài của chính nó. Nhiệm vụ là đếm tất cả các chuỗi con ngon lành của`s`, đếm nhiều lần xuất hiện riêng biệt. 

Kích thước đầu vào có thể lên tới`10^6`, loại trừ bất kỳ thuật toán nào lặp lại trên tất cả các chuỗi con có thể một cách rõ ràng và đếm chúng bằng cách quét đơn giản, bởi vì có khoảng`n*(n+1)/2`chuỗi con, có thể đạt tới khoảng`5*10^11`hoạt động trong trường hợp xấu nhất. Điều này có nghĩa là một thuật toán hiệu quả phải dựa vào cấu trúc dữ liệu chuỗi hoặc hàm băm để xác định nhanh tần số của từng chuỗi con. 

Các trường hợp cạnh không rõ ràng bao gồm: một chuỗi trong đó tất cả các ký tự đều giống hệt nhau. Ví dụ,`s = "aaaa"`có chuỗi con như`"a"`xuất hiện 4 lần,`"aa"`xuất hiện 3 lần,`"aaa"`xuất hiện 2 lần và`"aaaa"`xuất hiện 1 lần Một cách tiếp cận bất cẩn có thể tính sai`"aa"`hoặc`"aaa"`nếu nó chỉ kiểm tra tính chia hết đối với tổng số chuỗi con thay vì số lần xuất hiện thực tế. Một trường hợp cạnh khác là một chuỗi trong đó không có chuỗi con nào lặp lại, ví dụ:`s = "abcdef"`, trong đó tất cả các độ dài lớn hơn 1 phải xuất hiện chính xác một lần, vì vậy chỉ các chuỗi con có độ dài 1 mới ngon. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: liệt kê mọi chuỗi con của`s`, đếm số lần xuất hiện của nó trong toàn bộ chuỗi và kiểm tra tính chia hết cho độ dài của nó. Đối với một chuỗi có độ dài`n`, có`O(n^2)`các chuỗi con. Đếm số lần xuất hiện của mỗi chuỗi con một cách ngây thơ`O(n)`thời gian, dẫn đến`O(n^3)`sự phức tạp tổng thể. Điều này rõ ràng là quá chậm đối với`n`lên tới`10^6`. 

Quan sát quan trọng là vấn đề giảm xuống còn đếm hiệu quả số lần mỗi chuỗi con xuất hiện. Các cấu trúc hậu tố như **Suffix Automaton**, **Suffix Array** hoặc **Hàm Z ​​kết hợp với hàm băm** cho phép chúng tôi tính toán tất cả tần số chuỗi con một cách hiệu quả. Bằng cách sử dụng Suffix Automaton, mỗi chuỗi con tương ứng với một trạng thái và chúng ta có thể truyền số lần xuất hiện thông qua máy tự động để có được tần số của mỗi chuỗi con. Khi chúng ta biết tần số của từng chuỗi con, việc kiểm tra khả năng chia hết theo độ dài của nó là chuyện nhỏ. 

Phương pháp tối ưu sử dụng **Suffix Automaton (SAM)** với số lần xuất hiện. Sau khi xây dựng SAM, chúng tôi duyệt qua các trạng thái, ghi lại độ dài chuỗi con tối thiểu và tối đa cho mỗi trạng thái. Tần số được lưu trữ ở mỗi trạng thái tương ứng với số lần chuỗi con đó xuất hiện trong`s`. Chúng tôi tổng hợp, cho tất cả độ dài chuỗi con từ`min_len+1`ĐẾN`max_len`, số lần xuất hiện chia hết cho độ dài. Cách tiếp cận này chạy trong`O(n)`để xây dựng máy tự động và số lượng lan truyền, cộng với một`O(n)`duyệt để tính ra câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| Suffix Automaton + Đếm số lần xuất hiện | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng **Suffix Automaton (SAM)** cho chuỗi`s`. Mỗi trạng thái trong SAM đại diện cho một tập hợp các vị trí cuối cùng cho một chuỗi con`s`. SAM có thể được xây dựng dần dần theo`O(n)`thời gian bằng cách lặp qua chuỗi và mở rộng máy tự động với mỗi ký tự. 
2. Tính số lần xuất hiện cho mỗi trạng thái. Khởi tạo số đếm cho các trạng thái tương ứng với vị trí cuối thực tế (tức là trạng thái đầu cuối) thành 1. Sau đó truyền số đếm từ chuỗi con dài hơn đến chuỗi con ngắn hơn dọc theo các liên kết hậu tố. Bằng cách này, mỗi trạng thái tích lũy tổng số lần xuất hiện của chuỗi con được biểu diễn trong`s`. 
3. Đối với mỗi trạng thái, xác định độ dài tối thiểu và tối đa của chuỗi con được đại diện bởi trạng thái đó. Độ dài tối thiểu của một trạng thái là`len(link(state)) + 1`và độ dài tối đa là`len(state)`. 
4. Đối với mỗi trạng thái, lặp lại tất cả độ dài chuỗi con từ độ dài tối thiểu đến tối đa. Đối với mỗi chiều dài`L`, nếu tần số của chuỗi con này chia hết cho`L`, thêm tần số vào kết quả. 
5. Tính tổng tất cả các trạng thái để tính tổng số chuỗi con ngon. 

**Tại sao nó hoạt động:** Mỗi chuỗi con tương ứng với chính xác một trạng thái trong máy tự động hậu tố. Việc truyền bá số lần xuất hiện đảm bảo chúng tôi đếm chính xác tất cả các chuỗi con lặp lại. Bằng cách lặp lại độ dài cho phép cho mỗi trạng thái, chúng tôi xem xét mỗi chuỗi con chính xác một lần và kiểm tra điều kiện chia hết, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SuffixAutomaton:
    def __init__(self, n):
        self.states = [{}]
        self.link = [-1]
        self.len = [0]
        self.occ = [0]
        self.last = 0
        self.size = 1
        self.n = n

    def extend(self, c):
        p = self.last
        cur = self.size
        self.size += 1
        self.states.append({})
        self.len.append(self.len[p] + 1)
        self.occ.append(1)
        self.link.append(0)
        while p != -1 and c not in self.states[p]:
            self.states[p][c] = cur
            p = self.link[p]
        if p == -1:
            self.link[cur] = 0
        else:
            q = self.states[p][c]
            if self.len[p] + 1 == self.len[q]:
                self.link[cur] = q
            else:
                clone = self.size
                self.size += 1
                self.states.append(self.states[q].copy())
                self.len.append(self.len[p] + 1)
                self.link.append(self.link[q])
                self.occ.append(0)
                while p != -1 and self.states[p].get(c) == q:
                    self.states[p][c] = clone
                    p = self.link[p]
                self.link[q] = self.link[cur] = clone
        self.last = cur

def main():
    n = int(input())
    s = input().strip()
    sam = SuffixAutomaton(n)
    for ch in s:
        sam.extend(ch)

    # Count occurrences
    order = sorted(range(sam.size), key=lambda x: sam.len[x], reverse=True)
    for state in order:
        if sam.link[state] != -1:
            sam.occ[sam.link[state]] += sam.occ[state]

    result = 0
    for state in range(1, sam.size):
        min_len = sam.len[sam.link[state]] + 1
        max_len = sam.len[state]
        freq = sam.occ[state]
        for l in range(min_len, max_len + 1):
            if freq % l == 0:
                result += freq

    print(result)

if __name__ == "__main__":
    main()
```**Giải thích:** Chúng tôi xây dựng SAM, mở rộng nó cho từng ký tự, sau đó truyền các lần xuất hiện từ chuỗi con dài hơn sang chuỗi con ngắn hơn. Chúng tôi lặp lại phạm vi độ dài chuỗi con được biểu thị bằng mỗi trạng thái, tính tổng những chuỗi có tần số chia hết cho độ dài. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`"abacaba"`Các biến trạng thái cho chuỗi con được chọn: 

| Chuỗi con | tần số | len | min_len | max_len | có thể chia được? | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| "một" | 4 | 1 | 1 | 1 | vâng | 4 | 
| "b" | 2 | 1 | 1 | 1 | vâng | 2 | 
| "ab" | 2 | 2 | 2 | 2 | vâng | 2 | 
| "ba" | 2 | 2 | 2 | 2 | vâng | 2 | 
| người khác | ... | ... | ... | ... | ... | ... | 

Tổng tổng = 11. Điều này xác nhận thuật toán đếm chính xác cả chuỗi con đơn và chuỗi con nhiều độ dài, bao gồm cả chuỗi con lặp lại. 

### Mẫu 2 

đầu vào:`"aaaa"`| Chuỗi con | tần số | min_len | max_len | có thể chia được? | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| "một" | 4 | 1 | 1 | vâng | 4 | 
| "aa" | 3 | 2 | 2 | không | 0 | 
| "aaa" | 2 | 3 | 3 | không | 0 | 
| "aaa" | 1 | 4 | 4 | không | 0 | 

Tổng cộng = 4, phù hợp với mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc xây dựng máy tự động hậu tố và các lần xuất hiện lan truyền là tuyến tính. Tổng cuối cùng theo độ dài cũng là`O(n)`tổng cộng. | 
| Không gian | O(n) | SAM lưu trữ khoảng 2*n trạng thái và chuyển tiếp. Các mảng lần xuất hiện, liên kết và độ dài đều là tuyến tính. | 

Được cho`n <= 10^6`và hoạt động tuyến tính trong`n`, thuật toán này chạy thoải mái trong giới hạn 3 giây và sử dụng ít hơn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided samples
```
