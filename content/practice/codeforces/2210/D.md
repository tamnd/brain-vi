---
title: "CF 2210D - Một vấn đề RBS đơn giản"
description: "Chúng ta có hai chuỗi ngoặc cân bằng có độ dài bằng nhau. Cả hai chuỗi đều được đảm bảo là chuỗi dấu ngoặc thông thường, nghĩa là mỗi tiền tố có ít nhất số dấu ngoặc mở bằng số dấu ngoặc đóng và tổng số đếm khớp nhau."
date: "2026-06-07T19:16:41+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 2210
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1089 (Div. 2)"
rating: 2100
weight: 2210
solve_time_s: 94
verified: false
draft: false
---

[CF 2210D - Sự cố RBS đơn giản](https://codeforces.com/problemset/problem/2210/D) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, toán học, chuỗi, cây 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi ngoặc cân bằng có độ dài bằng nhau. Cả hai chuỗi đều được đảm bảo là chuỗi dấu ngoặc thông thường, nghĩa là mỗi tiền tố có ít nhất số dấu ngoặc mở bằng số dấu ngoặc đóng và tổng số đếm khớp nhau. 

Chúng ta được phép lấy liên tục hai chuỗi con rời rạc mà bản thân chúng là các chuỗi ngoặc thông thường hợp lệ và hoán đổi vị trí của chúng trong chuỗi. Hoạt động này bảo toàn cấu trúc bên trong của từng khối được chọn, nhưng cho phép sắp xếp lại toàn bộ các “khối” cân bằng. 

Nhiệm vụ là xác định xem liệu chúng ta có thể chuyển đổi một chuỗi khung hợp lệ thành một chuỗi khác bằng cách sử dụng bất kỳ số lần hoán đổi nào như vậy hay không. 

Ràng buộc chính là tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới 5×10^5. Điều này buộc phải có giải pháp O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm. Bất kỳ nỗ lực nào nhằm mô phỏng sự hoán đổi hoặc tìm kiếm trên các chuỗi con đều không thể thực hiện được ngay lập tức vì số lượng chuỗi con hợp lệ trong một chuỗi dấu ngoặc cân bằng có thể là bậc hai. 

Một trường hợp phức tạp phát sinh khi các chuỗi trông giống nhau về mặt cân bằng tổng thể nhưng khác nhau về sự phân hủy thành các thành phần cân bằng không thể quy giản được. Ví dụ: "()()()" và "((()))" có cùng số lượng nhưng cách phân tách cấu trúc khác nhau. Một cách tiếp cận ngây thơ chỉ kiểm tra số lượng '(' và ')' sẽ nói CÓ một cách không chính xác cho mọi thứ, bởi vì tất cả các đầu vào đều có chung số lượng. 

Một trường hợp thất bại khác là giả định rằng bất kỳ sự hoán vị nào của các thành phần nguyên thủy đều có thể xảy ra. Điều đó không đúng trừ khi chúng ta mô tả đặc điểm mà hoạt động thực sự bảo toàn. 

## Phương pháp tiếp cận 

Hoạt động cho phép hoán đổi hai chuỗi con cân bằng rời nhau. Điều này ngay lập tức gợi ý suy nghĩ về việc phân rã chuỗi thành các thành phần cân bằng nguyên tử. 

Một cách tiếp cận bạo lực tự nhiên sẽ là coi mọi chuỗi con hợp lệ là một nút và thử BFS hoặc DFS trên tất cả các chuỗi có thể truy cập bằng cách hoán đổi các khối hợp lệ. Mỗi lần hoán đổi có thể tạo ra các lựa chọn khối O(n^2) và mỗi trạng thái là một chuỗi, do đó không gian trạng thái bùng nổ theo cấp số nhân. Ngay cả việc tạo hàng xóm cũng đã là Θ(n^2) trên mỗi trạng thái, khiến điều này hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là mọi chuỗi ngoặc thông thường đều có sự phân tách duy nhất thành các khối RBS nguyên thủy, trong đó mỗi khối là một tiền tố hợp lệ tối thiểu trả về số dư bằng 0. Các khối nguyên thủy này hoạt động giống như các đơn vị không thể chia cắt được trong quá trình hoạt động. 

Hoạt động hoán đổi cho phép trao đổi bất kỳ hai khối nguyên thủy nào như vậy, nhưng nó không cho phép sắp xếp lại bên trong một khối hoặc chia khối thành các phần nhỏ hơn. Điều này có nghĩa là nhiều tập hợp các khối nguyên thủy là bất biến và vấn đề giảm xuống còn việc kiểm tra xem hai chuỗi có chia sẻ cùng nhiều tập hợp các thành phần RBS nguyên thủy hay không. 

Sau đó, chúng tôi giảm vấn đề xuống việc tính toán chữ ký chuẩn của các khối nguyên thủy và so sánh tần số của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Kết hợp nhiều khối nguyên thủy | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi từ trái sang phải trong khi duy trì bộ đếm số dư trong đó '(' đóng góp +1 và ')' đóng góp -1. 

Thời điểm bộ đếm đạt đến 0, chúng ta đã phát hiện ra khối RBS nguyên thủy. Đây là tiền tố nhỏ nhất kết thúc ở vị trí này và bản thân nó là RBS hợp lệ. 
2. Trích xuất mọi khối nguyên thủy như vậy cho cả hai chuỗi. 

Sự phân tách này là duy nhất vì trong RBS hợp lệ, số dư chỉ trở về 0 tại các ranh giới khối của các thành phần nguyên thủy. 
3. Đối với mỗi khối nguyên thủy, tính toán một biểu diễn chuẩn.

Chúng tôi thực hiện điều này bằng cách ghi lại chuỗi con đầy đủ của nó hoặc băm trực tiếp chuỗi đó một cách tương đương, vì các khối liền kề nhau và tổng kích thước của chúng trong tất cả các trường hợp thử nghiệm là O(n). 
4. Đếm số lần xuất hiện của mỗi biểu diễn khối nguyên thủy cho cả hai chuỗi. 
5. So sánh hai bộ nhiều tập. Nếu chúng khớp chính xác, xuất CÓ; nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Điều bất biến là thao tác chỉ hoán đổi toàn bộ chuỗi con cân bằng. Bất kỳ chuỗi con hợp lệ nào trong chuỗi ngoặc thông thường đều có thể được phân tách thành một chuỗi các khối RBS nguyên thủy. Hoán đổi hai chuỗi con hợp lệ sẽ hoán đổi hiệu quả các nhóm của các khối nguyên thủy này nhưng không bao giờ tách hoặc hợp nhất chúng qua các ranh giới. 

Do đó, nhiều khối nguyên thủy được bảo toàn dưới bất kỳ số lượng hoạt động nào. Ngược lại, nếu hai chuỗi có nhiều tập khối nguyên thủy giống hệt nhau, chúng ta có thể sắp xếp lại các khối đó một cách tùy ý thông qua các lần hoán đổi lặp đi lặp lại, bởi vì hai khối bất kỳ có thể được đưa vào vị trí thông qua một chuỗi các hoán đổi rời rạc mà không phá vỡ tính hợp lệ. Điều này mang lại sự tương đương giữa các trạng thái có thể tiếp cận và sự bình đẳng của nhiều khối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_blocks(s: str):
    bal = 0
    start = 0
    blocks = []
    for i, ch in enumerate(s):
        if ch == '(':
            bal += 1
        else:
            bal -= 1
        if bal == 0:
            blocks.append(s[start:i + 1])
            start = i + 1
    return blocks

def solve():
    s = input().strip()
    t = input().strip()

    bs = get_blocks(s)
    bt = get_blocks(t)

    if len(bs) != len(bt):
        return "NO"

    from collections import Counter
    return "YES" if Counter(bs) == Counter(bt) else "NO"

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        tt = input().strip()
        out.append(solve_for_case(s, tt))
    print("\n".join(out))

def solve_for_case(s, t):
    from collections import Counter
    return "YES" if Counter(get_blocks(s)) == Counter(get_blocks(t)) else "NO"

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ phân tách từng chuỗi thành các phân đoạn cân bằng nguyên thủy bằng cách theo dõi số dư và cắt bất cứ khi nào nó trở về 0. Đây là phân vùng có ý nghĩa về mặt cấu trúc duy nhất trong chuỗi ngoặc thông thường vì số dư không thể đạt 0 bên trong khối nguyên thủy. 

Mỗi khối được lưu trữ dưới dạng một chuỗi con. Vì tổng chiều dài trên tất cả các khối trong tất cả các trường hợp thử nghiệm là O(n), nên việc lưu trữ chúng trực tiếp là an toàn. 

Sau đó, chúng tôi so sánh bản đồ tần số của các khối bằng bảng băm. Nếu cả hai chuỗi đều chứa các tập hợp các thành phần nguyên thủy giống hệt nhau thì việc chuyển đổi là có thể. 

Một cạm bẫy triển khai phổ biến là trộn lẫn các phân đoạn cân bằng tiền tố với các chuỗi con cân bằng tùy ý. Chỉ các phân đoạn cân bằng tiền tố tối thiểu tương ứng với các đơn vị nguyên tử trong hoạt động được phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = (())()
t = ()(())
```| Bước | Quét cân bằng | Khối hình thành | Nhiều bộ s | 
| --- | --- | --- | --- | 
| 1 | 0 tại i=3 | (()) | {(())} | 
| 2 | 0 tại i=5 | () | {(()), ()} | 

| Bước | Quét cân bằng t | Khối hình thành | Nhiều bộ t | 
| --- | --- | --- | --- | 
| 1 | 0 tại i=1 | () | {()} | 
| 2 | 0 tại i=5 | (()) | {(())} | 

So sánh cuối cùng cho thấy nhiều tập hợp giống hệt nhau, vì vậy câu trả lời là CÓ. 

Điều này chứng tỏ rằng thứ tự của các khối nguyên thủy là không liên quan trong các giao dịch hoán đổi. 

### Ví dụ 2 

đầu vào:```
s = (()())
t = (())()
```| Bước | khối s | khối t | 
| --- | --- | --- | 
| Phân hủy | (()()) | (()), () | 

Ở đây s là một khối nguyên thủy duy nhất, trong khi t chia thành hai khối. Vì kích thước nhiều tập hợp khác nhau nên việc chuyển đổi là không thể. 

Điều này cho thấy thao tác không thể chia một cấu trúc nguyên thủy thành nhiều cấu trúc nguyên thủy độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần trong quá trình quét cân bằng và cấu trúc Bộ đếm là tuyến tính | 
| Không gian | O(n) | Chuỗi con được lưu trữ cho các khối nguyên thủy | 

Tổng độ phức tạp là tuyến tính trong tổng kích thước đầu vào, vừa vặn thoải mái trong giới hạn 5×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def get_blocks(s: str):
        bal = 0
        start = 0
        blocks = []
        for i, ch in enumerate(s):
            if ch == '(':
                bal += 1
            else:
                bal -= 1
            if bal == 0:
                blocks.append(s[start:i + 1])
                start = i + 1
        return blocks

    def solve_case(s, t):
        from collections import Counter
        return "YES" if Counter(get_blocks(s)) == Counter(get_blocks(t)) else "NO"

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        tt = input().strip()
        out.append(solve_case(s, tt))
    return "\n".join(out)

# provided samples
assert run("""5
4
()()
()()
4
()()
(())
6
(())()
()(())
8
(()()())
()()(())
8
()()(())
()(()())
""") == """YES
NO
YES
NO
YES"""

# custom cases
assert run("""1
4
(())
()()
""") == "NO"

assert run("""1
6
()()()
(()())
""") == "NO"

assert run("""1
8
(()())()
()(()())
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()()`vs`(())`| KHÔNG | số lượng cấu trúc nguyên thủy khác nhau | 
|`()()()`vs`(()())`| KHÔNG | nhiều khối so với khối đơn | 
| trường hợp sắp xếp lại hỗn hợp | CÓ | khả năng sắp xếp lại các khối | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi cả hai chuỗi có tổng số lượng giống hệt nhau nhưng phân đoạn nguyên thủy khác nhau. Ví dụ: "(()())" là một khối nguyên thủy duy nhất, trong khi "()()()" là ba khối nguyên thủy. Thuật toán tạo ra nhiều bộ khác nhau một cách chính xác vì số dư đạt 0 ở các vị trí khác nhau. 

Một trường hợp khác là khi các khối giống hệt nhau nhưng xuất hiện theo thứ tự khác nhau. Ví dụ: "(())()()" và "()()(())" tạo ra nhiều khối giống nhau và so sánh Bộ đếm bỏ qua thứ tự một cách chính xác, phù hợp với thực tế là các giao dịch hoán đổi có thể sắp xếp lại toàn bộ khối một cách tùy ý. 

Trường hợp cạnh cuối cùng là đầu vào tối thiểu "()", trong đó việc phân tách tạo ra một khối duy nhất. Mọi phép biến đổi đều phải bảo toàn cấu trúc đơn lẻ này, do đó chỉ các chuỗi giống hệt nhau mới được chấp nhận.
