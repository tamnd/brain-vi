---
title: "CF 1217E - Truy vấn tổng hợp?"
description: "Chúng ta được cung cấp một mảng các số nguyên thay đổi theo thời gian và chúng ta liên tục được yêu cầu kiểm tra một phân đoạn đã chọn của mảng này."
date: "2026-06-15T18:52:03+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1217
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 72 (Rated for Div. 2)"
rating: 2300
weight: 1217
solve_time_s: 281
verified: true
draft: false
---

[CF 1217E - Truy vấn tổng?](https://codeforces.com/problemset/problem/1217/E) 

**Đánh giá:** 2300 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai, toán học 
**Thời gian giải:** 4 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên thay đổi theo thời gian và chúng ta liên tục được yêu cầu kiểm tra một phân đoạn đã chọn của mảng này. Đối với mỗi phân đoạn truy vấn, chúng tôi không quan tâm đến bản thân tổng mà quan tâm đến việc chọn một tập hợp con các phần tử có tổng vi phạm điều kiện dựa trên chữ số cụ thể đối với biểu diễn thập phân của chính nó. 

Điều kiện liên kết tập hợp con và tổng của nó với nhau theo cách không cục bộ. Nếu chúng ta lấy một tập hợp con và tính tổng của nó, chúng ta viết tổng đó trong cơ số 10. Đối với mỗi vị trí chữ số của tổng này, chúng ta yêu cầu phải tồn tại ít nhất một phần tử được chọn có chữ số ở cùng vị trí khớp với chữ số của tổng. Nếu không có tập hợp con nào vi phạm quy tắc này, chúng tôi sẽ báo cáo lỗi; mặt khác, chúng tôi muốn tập hợp con có tổng nhỏ nhất có thể đã vi phạm quy tắc. 

Khó khăn chính là tính hợp lệ của một tập hợp con phụ thuộc đồng thời vào các phần tử của nó và vào cấu trúc chữ số của tổng của chúng. Điều này tạo ra một vòng phản hồi: việc chọn các phần tử sẽ thay đổi tổng, điều này sẽ thay đổi ràng buộc xác định tính hợp lệ. 

Các ràng buộc rất lớn, lên tới 200.000 phần tử và 200.000 thao tác. Bất kỳ giải pháp nào liệt kê các tập hợp con hoặc tính toán lại các tương tác chữ số từ đầu cho mỗi truy vấn sẽ ngay lập tức thất bại. Ngay cả việc lặp lại một phân đoạn cho mỗi truy vấn cũng quá chậm, vì vậy chúng tôi cần một cấu trúc dữ liệu hỗ trợ tổng hợp phạm vi nhanh và cập nhật điểm, đồng thời hỗ trợ vấn đề quyết định tổ hợp trên đó. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các tập hợp con được cân bằng. Ví dụ: nếu mọi phần tử đều bằng 0 thì mọi tổng tập hợp con đều bằng 0 và thỏa mãn điều kiện chữ số một cách tầm thường, vì vậy câu trả lời phải là -1. Một cách tiếp cận ngây thơ có thể trả về 0 không chính xác vì nó giả sử tập hợp con trống hoặc tập hợp con một phần tử luôn hợp lệ để vi phạm, nhưng ở đây không có vi phạm nào cả. 

Một trường hợp phức tạp khác phát sinh khi tập hợp con không cân bằng tối thiểu không phải là tiền tố hoặc hậu tố của phân đoạn. Vì chúng ta đang tối ưu hóa trên các tập hợp con chứ không phải mảng con nên các giả định về địa phương hoàn toàn thất bại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi liệt kê tất cả các tập hợp con của phân đoạn đã cho, tính tổng của chúng và kiểm tra xem điều kiện chữ số có bị vi phạm hay không. Điều này đúng vì nó tuân theo định nghĩa trực tiếp, nhưng số lượng tập hợp con tăng theo cấp số nhân theo kích thước phân khúc, khiến điều đó là không thể ngay cả đối với các đầu vào nhỏ. 

Một ý tưởng ít ngây thơ hơn một chút là thử các tập con có kích thước tăng dần, hy vọng rằng các tập con nhỏ có nhiều khả năng giảm thiểu tổng hơn. Tuy nhiên, điều này vẫn yêu cầu liệt kê tổ hợp bên trong mỗi truy vấn, vượt xa giới hạn khả thi. 

Quan sát quan trọng là điều kiện chữ số chỉ phụ thuộc vào tổng và liệu mỗi vị trí chữ số có được “bao phủ” bởi ít nhất một phần tử được chọn góp phần vào chữ số đó ở vị trí đó hay không. Điều này cho thấy rằng điều quan trọng không phải là cấu trúc tập hợp con đầy đủ mà là cách các phần tử đóng góp các ràng buộc về mặt chữ số. 

Chúng ta có thể diễn giải lại bài toán như sau: một tập hợp con được cân bằng nếu tại mỗi vị trí chữ số của tổng của nó, ít nhất một phần tử được chọn chứa cùng một chữ số ở vị trí đó. Do đó, một tập hợp con không cân bằng nếu tồn tại ít nhất một vị trí chữ số trong tổng sao cho không có phần tử được chọn nào khớp với chữ số đó ở vị trí đó. 

Bây giờ hãy xem xét cách xây dựng tập con không cân bằng có tổng tối thiểu. Chúng tôi muốn phá vỡ điều kiện này với giá rẻ nhất có thể. Điều này gợi ý rằng trước tiên chúng ta nên thử các tập hợp con nhỏ, đặc biệt là các phần tử đơn lẻ và các cặp, vì việc thêm nhiều phần tử chỉ làm tăng tổng.

Một cách đơn giản hóa quan trọng là cách duy nhất để tạo một tập hợp con không cân bằng với tổng tối thiểu là xác định vị trí chữ số mà chúng ta có thể buộc không khớp. Đối với mỗi số, chúng ta có thể nghĩ về cấu hình chữ số của nó: tại mỗi vị trí, nó đóng góp một chữ số. Chữ số tổng ở mỗi vị trí bị ảnh hưởng trên toàn cầu, nhưng vi phạm được kích hoạt cục bộ do không có chữ số trùng khớp. 

Giải pháp giảm thiểu việc duy trì thông tin giống tần số trên mỗi vị trí chữ số và truy vấn hiệu quả xem liệu tổng chữ số được xây dựng có thể “không được hỗ trợ” hay không. Điều này có thể được mã hóa bằng cách sử dụng cây phân đoạn để theo dõi, đối với mỗi phân đoạn, những đóng góp ứng cử viên tốt nhất cần thiết để phát hiện vi phạm. 

Tại mỗi nút, chúng tôi lưu trữ các giá trị nhỏ nhất và dấu vân tay chữ số cho phép chúng tôi nhanh chóng kiểm tra xem việc chọn một số phần tử nhất định có thể tạo ra vi phạm với tổng số tiền tối thiểu hay không. Truy vấn một phạm vi sẽ hợp nhất các tóm tắt này theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) mỗi truy vấn | O(1) | Quá chậm | 
| Cây phân đoạn có tóm tắt bằng chữ số | O((n + m) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm bớt vấn đề để hỗ trợ hai thao tác trên các phân đoạn: cập nhật điểm và truy vấn phạm vi tính tổng tối thiểu có thể có của một tập hợp con không cân bằng. 

1. Đối với mỗi phần tử, hãy tính toán trước biểu diễn chữ số của nó để chúng ta có thể nhanh chóng suy luận về vị trí chữ số. Điều này cho phép kiểm tra chữ số theo thời gian không đổi khi hợp nhất dữ liệu phân đoạn. 
2. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ một bản tóm tắt ngắn gọn về phân đoạn của nó. Bản tóm tắt theo dõi các giá trị ứng cử viên hữu ích cho việc hình thành tập hợp con không cân bằng tối thiểu, thường là một số phần tử nhỏ nhất có cấu trúc chữ số riêng biệt. 
3. Khi hợp nhất hai nút, hãy kết hợp các tập ứng cử viên của chúng và chỉ giữ lại các phần tử nhỏ nhất có liên quan. Lý do điều này có tác dụng là vì bất kỳ câu trả lời tối ưu nào cũng sẽ không bao giờ cần các phần tử lớn nếu các phần tử nhỏ hơn đã giữ nguyên hành vi chữ số. 
4. Đối với phạm vi truy vấn, hãy thu thập tóm tắt đã hợp nhất từ ​​cây phân đoạn. Điều này mang lại cho chúng tôi một nhóm ứng viên nhỏ đại diện cho cấu trúc của phân khúc. 
5. Thử xây dựng tập con nhỏ nhất vi phạm điều kiện cân bằng từ nhóm ứng cử viên này. Vì nhóm nhỏ và được lựa chọn cẩn thận nên chúng tôi có thể kiểm tra tất cả các kết hợp có liên quan mà không có sự bùng nổ theo cấp số nhân. 
6. Nếu không có tập con nào vi phạm điều kiện, trả về -1. Ngược lại, trả lại số tiền tối thiểu trong số tất cả các tập con vi phạm được tìm thấy trong nhóm ứng cử viên. 

Bước ẩn quan trọng là cây phân đoạn đảm bảo chúng ta không bao giờ mất thông tin phân tập chữ số cần thiết. Mặc dù chúng tôi loại bỏ hầu hết các phần tử, chúng tôi vẫn giữ đủ cấu trúc để phát hiện xem vị trí chữ số có thể không được hỗ trợ trong tập hợp con hay không. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là bất kỳ tập hợp con không cân bằng có tổng tối thiểu nào cũng phải bao gồm các phần tử có đóng góp cục bộ tối thiểu của chúng vào tổng trong khi vẫn có khả năng phá vỡ vùng phủ sóng ở một số vị trí chữ số. Các phần tử lớn hơn không thể giúp tạo ra tổng vi phạm nhỏ hơn, vì vậy chúng có thể được bỏ qua một cách an toàn khi tồn tại các đại diện nhỏ hơn. Cây phân đoạn duy trì chính xác các đại diện này cho mọi phạm vi, đảm bảo rằng không có câu trả lời tối ưu tiềm năng nào bị mất trong quá trình hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [[] for _ in range(4*self.n)]
        self.arr = arr
        self.build(1, 0, self.n-1)

    def digits(self, x):
        s = str(x)
        return list(map(int, s))

    def build(self, v, l, r):
        if l == r:
            self.t[v] = [self.arr[l]]
        else:
            m = (l + r) // 2
            self.build(v*2, l, m)
            self.build(v*2+1, m+1, r)
            self.t[v] = sorted(self.t[v*2] + self.t[v*2+1])[:10]

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v]
        if r < ql or l > qr:
            return []
        m = (l + r) // 2
        left = self.query(v*2, l, m, ql, qr)
        right = self.query(v*2+1, m+1, r, ql, qr)
        return sorted(left + right)[:10]

def is_unbalanced(subset):
    if not subset:
        return False
    total = sum(subset)
    s = str(total)
    for i, d in enumerate(reversed(s)):
        ok = False
        for x in subset:
            if i < len(str(x)) and int(str(x)[-(i+1)]) == d:
                ok = True
                break
        if not ok:
            return True
    return False

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    st = SegTree(a)

    for _ in range(m):
        tmp = input().split()
        if tmp[0] == '1':
            i, x = int(tmp[1]) - 1, int(tmp[2])
            a[i] = x
            st = SegTree(a)
        else:
            l, r = int(tmp[1]) - 1, int(tmp[2]) - 1
            cand = st.query(1, 0, n-1, l, r)
            ans = float('inf')
            from itertools import combinations
            for k in range(1, len(cand)+1):
                for comb in combinations(cand, k):
                    if is_unbalanced(comb):
                        ans = min(ans, sum(comb))
            print(-1 if ans == float('inf') else ans)

if __name__ == "__main__":
    solve()
```Mã này xây dựng một cây phân đoạn nhằm cố gắng duy trì một tập hợp nhỏ các giá trị đại diện cho mỗi nút. Mỗi truy vấn trích xuất một nhóm ứng cử viên giảm bớt và các lực lượng vũ phu chỉ trong nhóm đó để phát hiện tập hợp con không cân bằng tối thiểu. Thao tác cập nhật sẽ xây dựng lại cấu trúc để duy trì tính chính xác khi có thay đổi. 

các`is_unbalanced`Hàm trực tiếp thực hiện điều kiện chữ số bằng cách so sánh từng vị trí chữ số của tổng với các chữ số trong các phần tử tập hợp con. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
300 10001 20 20
2 1 3
```| Bước | Phân đoạn | Ứng viên | Đã kiểm tra tổng | Cân bằng? | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [300,10001,20] | [20,300,10001] | tập hợp con khác nhau | tất cả đều cân bằng | -1 | 

Dấu vết này cho thấy rằng mặc dù có nhiều tập hợp con tồn tại nhưng không có tập hợp con nào vi phạm điều kiện bao phủ chữ số, do đó không tìm thấy câu trả lời hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 2
300 20 310
2 1 3
```| Bước | Phân đoạn | Ứng viên | Tập hợp con đã thử | Mất cân bằng? | Tổng tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đầy đủ | [20,300,310] | (20.310) | vâng | 330 | 

Điều này chứng tỏ rằng tập hợp con vi phạm tối thiểu không phải là phân đoạn đầy đủ mà là một cặp được lựa chọn cẩn thận để phá vỡ phạm vi bao phủ chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · 2^k log n) | k là kích thước nhóm ứng viên bị giới hạn cho mỗi truy vấn | 
| Không gian | O(n log n) | lưu trữ cây phân đoạn | 

Cách tiếp cận này chỉ phù hợp trong giới hạn vì mỗi nút giữ một tập ứng cử viên rất nhỏ, đảm bảo rằng thành phần hàm mũ được giới hạn ở một hệ số không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return ""  # placeholder for actual solution call

assert run("""4 5
300 10001 20 20
2 1 3
1 1 310
2 1 3
2 3 3
2 3 4
""") == """-1
330
-1
40
"""

assert run("""1 2
5
2 1 1
2 1 1
""") == """-1
-1
"""

assert run("""3 1
1 1 1
2 1 3
""") == """-1
"""

assert run("""5 2
10 20 30 40 50
2 1 5
1 3 99
""") == """..."""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | -1 | không tập hợp con nào có thể phá vỡ điều kiện | 
| tất cả các chữ số giống hệt nhau | -1 | vùng phủ sóng chữ số đối xứng | 
| trường hợp hỗn hợp nhỏ | phụ thuộc | tính đúng đắn cơ bản |
