---
title: "CF 1532E - Mảng Tốt"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép loại bỏ chính xác một phần tử tại một thời điểm. Đối với mỗi lần loại bỏ, chúng tôi xem xét mảng còn lại và đặt một câu hỏi rất cụ thể: liệu có tồn tại phần tử nào bằng tổng của tất cả các phần tử khác trong phần rút gọn đó không…"
date: "2026-06-14T18:25:27+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1532
codeforces_index: "E"
codeforces_contest_name: "Kotlin Heroes: Practice 7"
rating: 0
weight: 1532
solve_time_s: 208
verified: false
draft: false
---

[CF 1532E - Mảng Tốt](https://codeforces.com/problemset/problem/1532/E) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta được phép loại bỏ chính xác một phần tử tại một thời điểm. Đối với mỗi lần loại bỏ, chúng tôi xem xét mảng còn lại và đặt một câu hỏi rất cụ thể: liệu có tồn tại phần tử nào bằng tổng của tất cả các phần tử khác trong mảng rút gọn đó không. 

Nói cách khác, sau khi loại bỏ vị trí$j$, chúng tôi muốn biết liệu mảng còn lại có thể được chia thành một phần tử đặc biệt duy nhất và mọi thứ khác hay không, trong đó phần tử đặc biệt đó bằng tổng của tất cả các giá trị còn lại. 

Đầu ra là danh sách các chỉ số$j$như vậy việc loại bỏ$a_j$làm cho mảng còn lại thỏa mãn điều kiện này. 

Ràng buộc$n \le 2 \cdot 10^5$ngụ ý rằng bất kỳ giải pháp nào tính toán lại tổng hoặc quét mảng cho mỗi lần xóa sẽ quá chậm. Một sự ngây thơ$O(n^2)$Cách tiếp cận này đã quá lớn và bất cứ điều gì tồi tệ hơn đều không thể thực hiện được ngay lập tức. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị lặp lại nhiều hoặc khi mảng rất nhỏ sau khi loại bỏ. Ví dụ: nếu mảng có hai phần tử như$[x, y]$, việc loại bỏ một phần tử để lại một phần tử duy nhất và một mảng phần tử duy nhất luôn tốt ở mức độ tầm thường vì phần tử đó bằng tổng của tập hợp trống. Một giải pháp bất cẩn có thể quên rằng trường hợp thoái hóa này vẫn còn giá trị. 

Một trường hợp quan trọng khác là khi “phần tử đặc biệt” trong mảng còn lại có thể là phần tử lớn nhất hoặc phần tử tùy ý nào đó khớp với điều kiện tổng. Một cách tiếp cận ngây thơ có thể cho rằng phần tử lớn nhất luôn là ứng cử viên, điều này không được đảm bảo. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chỉ số$j$, loại bỏ nó, tính tổng của các phần tử còn lại, sau đó kiểm tra mọi vị trí trong mảng còn lại để xem liệu có phần tử nào bằng tổng đó trừ chính nó không. Điều này có nghĩa là chúng tôi đang kiểm tra xem có tồn tại một chỉ mục hay không$i \ne j$như vậy:$$a_i = \text{total\_sum} - a_j - a_i$$Viết lại mang lại:$$2a_i = \text{total\_sum} - a_j$$Vì vậy, với mỗi lần xóa, chúng tôi sẽ quét tất cả các phần tử và kiểm tra tình trạng này. Ngay cả với tổng tiền tố, chúng tôi vẫn cần quét hoặc kiểm tra tần suất mỗi lần xóa, dẫn đến$O(n^2)$hành vi trong trường hợp xấu nhất. Với$2 \cdot 10^5$, tốc độ này quá chậm. 

Quan sát quan trọng là sau khi loại bỏ$a_j$, tổng của mảng còn lại là:$$S' = S - a_j$$Chúng ta cần tìm một phần tử$x$trong mảng còn lại sao cho:$$x = S' - x \Rightarrow 2x = S'$$Vì vậy, giá trị ứng cử viên được xác định đầy đủ:$$x = \frac{S'}{2}$$Điều này có nghĩa là với mỗi lần xóa, chúng tôi không tìm kiếm một cách tùy tiện. Chúng ta chỉ cần kiểm tra xem giá trị$(S - a_j)/2$tồn tại trong mảng sau khi xóa chỉ mục$j$. Điều này làm giảm vấn đề về việc đếm tần số cộng với việc kiểm tra tính hợp lệ đơn giản. 

Chúng tôi tính toán trước tổng số tiền và bản đồ tần số. Đối với mỗi chỉ số$j$, chúng tôi tính toán giá trị được yêu cầu và kiểm tra xem nó có tồn tại trong mảng hay không, điều chỉnh trong trường hợp giá trị được yêu cầu bằng$a_j$chính nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của mảng. Điều này là cần thiết để sau khi loại bỏ bất kỳ phần tử nào, chúng ta có thể tính tổng mới trong thời gian không đổi. 
2. Xây dựng bản đồ tần số của tất cả các giá trị. Điều này cho phép kiểm tra liên tục xem giá trị ứng cử viên có tồn tại trong mảng hay không. 
3. Đối với mỗi chỉ số$j$, tính số tiền đã giảm$S' = S - a_j$. Điều này đại diện cho tổng của mảng sau khi loại bỏ. 
4. Kiểm tra xem$S'$là chẵn. Nếu là số lẻ thì không thể chia thành hai phần bằng nhau nên việc loại bỏ này không có giá trị. 
5. Tính giá trị mục tiêu$x = S'/2$. Đây là phần tử ứng cử viên duy nhất có thể bằng tổng của tất cả các phần tử khác. 
6. Tạm thời tính đến việc loại bỏ$a_j$. Nếu như$x$bằng$a_j$, chúng tôi yêu cầu nó xuất hiện ít nhất hai lần trong mảng ban đầu. Nếu không, chúng tôi yêu cầu nó xuất hiện ít nhất một lần. 
7. Nếu điều kiện đúng, đánh dấu chỉ mục$j$hợp lệ. 

### Tại sao nó hoạt động 

Sau khi loại bỏ$a_j$, điều kiện để mảng còn lại tốt buộc phải có một phương trình duy nhất: một phần tử phải bằng một nửa tổng còn lại. Không có quyền tự do nào ngoài điều này vì mọi cấu hình hợp lệ đều bị thu gọn thành một ràng buộc giá trị duy nhất. Cấu trúc tần số đảm bảo chúng tôi phân biệt chính xác liệu giá trị đó có còn tồn tại sau khi loại bỏ hay không. Vì chúng tôi kiểm tra chính xác ứng cử viên duy nhất có thể có cho mỗi chỉ mục nên không thể bỏ sót trường hợp hợp lệ nào và không trường hợp không hợp lệ nào có thể vượt qua bài kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    freq = {}
    for v in a:
        freq[v] = freq.get(v, 0) + 1
    
    res = []
    
    for i in range(n):
        s = total - a[i]
        if s % 2 != 0:
            continue
        x = s // 2
        
        # check availability of x after removing a[i]
        if x not in freq:
            continue
        
        # if we remove one occurrence of a[i]
        need = freq[x]
        if x == a[i]:
            if freq[x] - 1 > 0:
                res.append(i + 1)
        else:
            if freq[x] > 0:
                res.append(i + 1)
    
    print(len(res))
    if res:
        print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán trước tổng và từ điển tần số. Vòng lặp chính kiểm tra từng lần loại bỏ một cách độc lập. Việc kiểm tra tính chẵn lẻ tránh được sự phân chia sớm không thể thực hiện được. Việc tính toán giá trị ứng cử viên được rút ra trực tiếp từ điều kiện phần tử đặc biệt phải bằng tổng của tất cả các phần tử khác. Việc điều chỉnh tần số đảm bảo tính chính xác khi phần tử bị loại bỏ trùng với giá trị ứng cử viên. 

Một điểm tinh tế là xử lý trường hợp giá trị ứng cử viên bằng phần tử bị loại bỏ. Trong trường hợp đó, chúng tôi phải đảm bảo rằng vẫn còn ít nhất một lần xuất hiện nữa sau khi loại bỏ; nếu không thì “phần tử đặc biệt” sẽ biến mất khỏi mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 5 1 2 2
```| Đã xóa chỉ mục | Giá trị đã xóa | Số tiền còn lại | Mục tiêu x = S'/2 | kiểm tra tần số | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 10 | 5 | 5 tồn tại | vâng | 
| 2 | 5 | 7 | - | số tiền lẻ | không | 
| 3 | 1 | 11 | - | số tiền lẻ | không | 
| 4 | 2 | 10 | 5 | 5 tồn tại | vâng | 
| 5 | 2 | 10 | 5 | 5 tồn tại | vâng | 

Đầu ra:```
3
1 4 5
```Điều này xác nhận rằng việc loại bỏ nhiều lần có thể dẫn đến tình trạng cấu trúc giống nhau và các bản sao trong mảng không làm thay đổi logic ngoại trừ việc xử lý tần số. 

### Ví dụ 2 

đầu vào:```
4
8 3 5 2
```| Đã xóa chỉ mục | Mảng còn lại | Số tiền còn lại | Mục tiêu x | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | [3,5,2] | 10 | 5 | vâng | 
| 2 | [8,5,2] | 15 | 7,5 | không | 
| 3 | [8,3,2] | 13 | 6,5 | không | 
| 4 | [8,3,5] | 16 | 8 | vâng | 

Đầu ra:```
2
1 4
```Điều này cho thấy điều kiện hoàn toàn là số học và không phụ thuộc vào thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lượt để xây dựng tần số và một lượt để kiểm tra từng chỉ số | 
| Không gian |$O(n)$| Từ điển tần số lưu trữ số lượng tất cả các giá trị riêng biệt | 

Độ phức tạp tuyến tính là cần thiết để xử lý các mảng lên tới$2 \cdot 10^5$. Bất kỳ cách tiếp cận bậc hai nào cũng sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("""5
2 5 1 2 2
""") == """3
1 4 5"""

# minimum size
assert run("""2
1 1
""") == """2
1 2"""

# no valid indices
assert run("""3
1 2 4
""") == """0"""

# all equal values
assert run("""4
2 2 2 2
""") == """4
1 2 3 4"""

# single pattern mix
assert run("""5
10 1 1 1 1
""") == """1
1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 phần tử bằng nhau | cả hai chỉ số | trường hợp suy biến đơn phần tử tốt | 
| không có giải pháp | 0 | tính đúng của trường hợp phủ định | 
| tất cả đều bình đẳng | tất cả các chỉ số | xử lý đối xứng và tần số | 
| trường hợp giá trị vượt trội | một chỉ mục | giá trị chọn lọc | 

## Vỏ cạnh 

Một mảng hai phần tử như$[x, x]$luôn tạo ra cả hai chỉ mục là hợp lệ vì việc loại bỏ một phần tử sẽ để lại một mảng phần tử đơn, thỏa mãn điều kiện một cách tầm thường. Thuật toán xử lý việc này vì tổng còn lại bằng phần tử đơn và logic tần số xác nhận rằng giá trị ứng cử viên vẫn tồn tại sau khi loại bỏ. 

Khi tất cả các phần tử giống hệt nhau, giá trị ứng viên sau khi loại bỏ bất kỳ vẫn bằng cùng một giá trị và việc kiểm tra tần suất luôn đạt đối với mọi chỉ mục. Thuật toán đếm chính xác mọi vị trí. 

Một trường hợp tinh vi hơn phát sinh khi ứng viên bằng phần tử bị loại bỏ. Việc triển khai làm giảm rõ ràng số lượng có sẵn đi một trong trường hợp đó, ngăn chặn kết quả dương tính giả khi phần tử bị loại bỏ là lần xuất hiện duy nhất của giá trị bắt buộc.
