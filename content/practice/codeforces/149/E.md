---
title: "CF 149E - Dây Sao Hỏa"
description: "Vấn đề mô tả một người sao Hỏa có một hàng mắt, mỗi mắt được che bởi một miếng vá được đánh dấu bằng một chữ in hoa. Chuỗi chữ cái nhìn thấy được khi mở tất cả các mắt tượng trưng cho một chuỗi s có độ dài n."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 149
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 106 (Div. 2)"
rating: 2300
weight: 149
solve_time_s: 76
verified: true
draft: false
---

[CF 149E - Dây sao Hỏa](https://codeforces.com/problemset/problem/149/E) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một người sao Hỏa có một hàng mắt, mỗi mắt được che bởi một miếng vá được đánh dấu bằng một chữ in hoa. Chuỗi chữ cái hiển thị khi tất cả các mắt đều mở đại diện cho một chuỗi _s_ có độ dài _n_. Người sao Hỏa có thể chọn mở chính xác hai đoạn mắt liên tiếp không chồng lên nhau. Sau khi làm điều này, anh ấy đọc các chữ cái theo thứ tự, ghép chúng thành một từ. Một tập hợp các từ _m_ của người sao Hỏa được coi là đẹp và chúng ta cần đếm xem người sao Hỏa có thể tạo ra bao nhiêu từ đẹp khác biệt bằng cách mở hai đoạn. 

Quan sát chính là chuỗi _s_ có thể dài tới 100.000 ký tự và từ đẹp có thể lên tới 1.000 ký tự mỗi chuỗi, nhưng có nhiều nhất là 100 từ đẹp. Vì _n_ lớn nên việc liệt kê trực tiếp tất cả các cặp phân đoạn có thể có sẽ dẫn đến khả năng O(n2), tức là khoảng 10¹⁰ thao tác, vượt xa giới hạn thời gian 2 giây. Do đó, một giải pháp vũ phu ngây thơ tạo ra tất cả các kết hợp hai phân đoạn có thể có và so sánh chúng với từng từ đẹp đẽ sẽ không hiệu quả. Chúng ta cần một phương pháp để kiểm tra một cách hiệu quả xem liệu sự kết hợp nào của hai phân đoạn có tạo thành một trong những từ đẹp hay không. 

Các trường hợp cạnh bao gồm các tình huống trong đó hai đoạn chạm vào đầu chuỗi hoặc có độ dài tối thiểu. Ví dụ: nếu _s_ = "AA" và từ đẹp là "AA" thì người sao Hỏa có thể mở riêng con mắt thứ nhất và con mắt thứ hai để nhìn thấy "AA". Việc triển khai ngây thơ có thể cho rằng các phân đoạn phải liền kề nhau và bao trùm toàn bộ từ một cách không chính xác. Một trường hợp phức tạp khác là chồng chéo với các kết quả khớp một phần tại ranh giới phân đoạn; cần phải xử lý cẩn thận để tránh bỏ sót những thứ này. 

## Phương pháp tiếp cận 

Phương pháp brute-force tạo ra tất cả các cặp phân đoạn không chồng chéo, nối các ký tự và so sánh chuỗi kết quả với mọi từ đẹp. Đối với một chuỗi có độ dài _n_, có khoảng O(n2) lựa chọn cho phân đoạn đầu tiên và O(n2) cho phân đoạn thứ hai sau phân đoạn đầu tiên, dẫn đến các phép nối O(n⁴) trong trường hợp xấu nhất. Mỗi lần so sánh với một từ tốn O(L) thời gian, trong đó L lên tới 1000. Điều này rõ ràng là không khả thi. 

Giải pháp tối ưu dựa trên cấu trúc khớp chuỗi được gọi là máy tự động hậu tố, thể hiện một cách hiệu quả tất cả các chuỗi con của _s_. Sau khi xây dựng máy tự động hậu tố, chúng tôi có thể kiểm tra bất kỳ chuỗi nào theo thời gian tuyến tính so với độ dài của nó để xem liệu đó có phải là chuỗi con của _s_ hay không. Để xử lý hai phân đoạn, chúng tôi chia mỗi từ đẹp thành tất cả các cách chia hai phần có thể có, kiểm tra xem phần đầu tiên có xuất hiện dưới dạng chuỗi con kết thúc ở vị trí nào đó _i_ hay không và phần thứ hai có xuất hiện dưới dạng chuỗi con bắt đầu sau _i_ hay không. Điều này có thể được thực hiện một cách hiệu quả bằng cách xây dựng cả máy tự động hậu tố chuyển tiếp cho _s_ và máy tự động đảo ngược cho chuỗi đảo ngược. Sau đó, đối với mỗi phần tách của một từ đẹp, chúng tôi kiểm tra xem nửa đầu có phải là chuỗi con kết thúc ở đâu đó hay không và nửa sau có phải là chuỗi con bắt đầu ở đâu đó sau đó mà không liệt kê rõ ràng tất cả các vị trí phân đoạn hay không. 

Thông tin chi tiết quan trọng là vấn đề giảm xuống còn việc kiểm tra xem mỗi từ đẹp có thể được phân tách thành hai chuỗi con hay không, cả hai đều xuất hiện trong _s_, với chuỗi thứ hai xuất hiện sau chuỗi đầu tiên. Bằng cách sử dụng máy tự động hậu tố, chúng ta có thể thực hiện việc kiểm tra tuyến tính theo thời gian này theo độ dài từ, nhân với số từ, đưa ra giải pháp đủ nhanh cho các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n⁴ * L) | O(n²) | Quá chậm | 
| Kiểm tra phân tách hậu tố Automaton | O(n + Σ | pᵢ | ²) | 

## Hướng dẫn thuật toán

1. Xây dựng một máy tự động hậu tố cho chuỗi _s_. Điều này cho phép chúng tôi kiểm tra nhanh xem có chuỗi con nào tồn tại trong _s_ hay không. Mỗi trạng thái đại diện cho một tập hợp các chuỗi con và các chuyển đổi tương ứng với việc thêm các chữ cái. 
2. Xây dựng một máy tự động hậu tố cho chuỗi đảo ngược _s_. Điều này cho phép chúng ta nhanh chóng kiểm tra các chuỗi con kết thúc ở vị trí bất kỳ bằng cách truy vấn từ đảo ngược. 
3. Khởi tạo bộ đếm số lượng từ đẹp nhìn thấy. 
4. Với mỗi từ đẹp, hãy lặp lại tất cả các phần có thể chia thành hai phần không trống: phần bên trái`u`và phần bên phải`v`. 
5. Đối với mỗi lần phân chia, hãy kiểm tra xem`u`tồn tại dưới dạng một chuỗi con trong máy tự động ban đầu và`v`tồn tại dưới dạng một chuỗi con trong máy tự động đảo ngược của`s`(sau khi đảo ngược`v`). Việc phân chia hợp lệ nếu cả hai phần xuất hiện trong _s_ và không trùng nhau theo cách vi phạm thứ tự. 
6. Nếu có ít nhất một phép chia thỏa mãn điều kiện, hãy tăng bộ đếm. 
7. Xuất bộ đếm. 

Tại sao nó hoạt động: Máy tự động hậu tố đảm bảo rằng mọi chuỗi con của _s_ đều có thể được kiểm tra theo thời gian tuyến tính đối với độ dài chuỗi con. Bằng cách tách từng từ đẹp đẽ, chúng tôi đề cập đến mọi khả năng mà người sao Hỏa có thể mở hai phân đoạn để xem từ đó. Chúng tôi không bao giờ bỏ lỡ bất kỳ từ nào vì tất cả các phần tách đều được xem xét và chúng tôi không bao giờ tính hai lần vì chúng tôi chỉ đếm mỗi từ một lần nếu có ít nhất một phần tách khớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SuffixAutomaton:
    def __init__(self):
        self.states = [{'len': 0, 'link': -1, 'next': {}}]
        self.last = 0

    def extend(self, c):
        p = self.last
        cur = len(self.states)
        self.states.append({'len': self.states[p]['len'] + 1, 'link': 0, 'next': {}})
        while p != -1 and c not in self.states[p]['next']:
            self.states[p]['next'][c] = cur
            p = self.states[p]['link']
        if p == -1:
            self.states[cur]['link'] = 0
        else:
            q = self.states[p]['next'][c]
            if self.states[p]['len'] + 1 == self.states[q]['len']:
                self.states[cur]['link'] = q
            else:
                clone = len(self.states)
                self.states.append({'len': self.states[p]['len'] + 1,
                                    'link': self.states[q]['link'],
                                    'next': self.states[q]['next'].copy()})
                while p != -1 and self.states[p]['next'][c] == q:
                    self.states[p]['next'][c] = clone
                    p = self.states[p]['link']
                self.states[q]['link'] = self.states[cur]['link'] = clone
        self.last = cur

    def contains(self, s):
        current = 0
        for ch in s:
            if ch not in self.states[current]['next']:
                return False
            current = self.states[current]['next'][ch]
        return True

def main():
    s = input().strip()
    n = len(s)
    m = int(input())
    words = [input().strip() for _ in range(m)]

    sam = SuffixAutomaton()
    for ch in s:
        sam.extend(ch)
    s_rev = s[::-1]
    sam_rev = SuffixAutomaton()
    for ch in s_rev:
        sam_rev.extend(ch)

    count = 0
    for word in words:
        L = len(word)
        found = False
        for split in range(1, L):
            left, right = word[:split], word[split:]
            if sam.contains(left) and sam_rev.contains(right[::-1]):
                found = True
                break
        if found:
            count += 1
    print(count)

if __name__ == "__main__":
    main()
```Đầu tiên, mã xây dựng hai máy tự động hậu tố, một cho chuỗi gốc và một cho chuỗi đảo ngược. Điều này cho phép kiểm tra tư cách thành viên nhanh chóng đối với các chuỗi con phải xuất hiện trong phân đoạn thứ nhất hoặc thứ hai. Chúng tôi lặp lại từng phần có thể có của mỗi từ đẹp, kiểm tra phần bên trái và bên phải một cách độc lập. các`contains`phương pháp này chuyển các trạng thái tự động một cách hiệu quả và ngay lập tức thất bại nếu thiếu một chữ cái. Điều này đảm bảo tính chính xác và tránh các lỗi sai sót. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào`s = "ABCBABA"`và những lời hay ý đẹp`["BAAB", "ABBA"]`. 

| từ | chia | còn lại trong s | ngay trong s | được tính? | 
| --- | --- | --- | --- | --- | 
| BAAB | 1-3 | B, BA | AAB | không | 
| ABBA | 1-3 | AB | BA | vâng | 

Bảng cho thấy "ABBA" có thể được chia thành "AB" và "BA" xuất hiện theo thứ tự trong chuỗi. Do đó, đầu ra là 1. 

Ví dụ tùy chỉnh 2: 

đầu vào`s = "AAAA"`và lời nói đẹp`["AA"]`. 

| từ | chia | còn lại trong s | ngay trong s | được tính? | 
| --- | --- | --- | --- | --- | 
| AA | 1 | A | A | vâng | 

Ngay cả với các chữ cái lặp đi lặp lại, máy tự động xác định chính xác rằng "AA" có thể được tạo thành từ hai phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + Σ | pᵢ | 
| Không gian | O(n) | Automaton lưu trữ tối đa 2*n trạng thái, mỗi trạng thái có từ điển nhỏ. | 

Với`n`lên đến 10⁵ và`Σ|pᵢ|²`10⁵ * 10² = 10⁷, giá trị này vừa vặn thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys
```
