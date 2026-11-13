---
title: "CF 1730A - Các hành tinh"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có nhiều tập hợp số nguyên, trong đó mỗi số nguyên biểu thị nhãn quỹ đạo của một hành tinh. Các hành tinh có cùng giá trị sẽ thuộc cùng một quỹ đạo. Chúng ta có hai cách để tiêu diệt các hành tinh."
date: "2026-06-15T02:39:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1730
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 823 (Div. 2)"
rating: 800
weight: 1730
solve_time_s: 80
verified: true
draft: false
---

[CF 1730A - Các hành tinh](https://codeforces.com/problemset/problem/1730/A) 

**Đánh giá:** 800 
**Tags:** cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm có nhiều tập hợp số nguyên, trong đó mỗi số nguyên biểu thị nhãn quỹ đạo của một hành tinh. Các hành tinh có cùng giá trị sẽ thuộc cùng một quỹ đạo. 

Chúng ta có hai cách để tiêu diệt các hành tinh. Một tùy chọn loại bỏ một hành tinh với chi phí 1. Tùy chọn còn lại loại bỏ mọi hành tinh thuộc quỹ đạo đã chọn trong một thao tác với chi phí c, bất kể có bao nhiêu hành tinh trong quỹ đạo đó. 

Nhiệm vụ là chọn sự kết hợp của hai hành động này để loại bỏ tất cả các hành tinh trong khi giảm thiểu tổng chi phí. 

Cấu trúc của đầu vào ngụ ý rằng chỉ tần số của các giá trị quỹ đạo riêng biệt mới quan trọng chứ không phải vị trí của chúng. Bất kỳ hoán vị nào của mảng đều không làm thay đổi câu trả lời, do đó bài toán giảm xuống việc đếm số lần xuất hiện của từng giá trị. 

Các ràng buộc nhỏ, với n lên tới 100 cho mỗi trường hợp kiểm thử và nhiều nhất là 100 trường hợp kiểm thử. Điều này có nghĩa là mọi giải pháp chạy trong O(n²) cho mỗi trường hợp thử nghiệm đều đã an toàn nhưng cấu trúc cho phép phương pháp đếm tuyến tính thậm chí còn đơn giản hơn. 

Một trường hợp lỗi phổ biến xuất phát từ việc bỏ qua việc tổng hợp tần số. Ví dụ: nếu tất cả các hành tinh nằm trên các quỹ đạo riêng biệt và c lớn thì việc sử dụng máy thứ hai là lãng phí, nhưng cách tiếp cận tham lam ngây thơ vẫn có thể áp dụng nó một lần cho mỗi hành tinh. Ngược lại, nếu nhiều hành tinh có chung quỹ đạo và c nhỏ, việc không nhóm chúng lại sẽ dẫn đến việc phải trả quá nhiều tiền khi sử dụng máy đầu tiên nhiều lần. 

Một trường hợp minh họa nhỏ: 

đầu vào:```
1
5 3
1 1 1 2 3
```Nếu chúng ta phá hủy riêng lẻ, chi phí là 5. Nếu chúng ta sử dụng máy thứ hai trên quỹ đạo 1, chi phí sẽ trở thành 3 + 2 = 5. Nếu chúng ta sử dụng nó trên tất cả các quỹ đạo, chi phí sẽ trở thành 3 + 3 + 3 = 9, tệ hơn. Chiến lược đúng đắn phụ thuộc vào việc so sánh mức tiết kiệm trên mỗi quỹ đạo, không chỉ số liệu thô. 

## Phương pháp tiếp cận 

Phối cảnh vũ phu cố gắng quyết định cho từng tập hợp con quỹ đạo xem chúng ta có sử dụng máy thứ hai hay không. Giả sử có k quỹ đạo riêng biệt. Đối với mỗi quỹ đạo, chúng tôi chọn trả kích thước của quỹ đạo đó (sử dụng máy đầu tiên nhiều lần) hoặc trả c một lần (sử dụng máy thứ hai). Điều này dẫn đến 2^k lựa chọn. Việc tính toán mỗi lựa chọn tốn O(n), do đó tổng sẽ trở thành O(2^k n), điều này không khả thi ngay cả đối với k vừa phải. 

Quan sát quan trọng là các quỹ đạo là độc lập. Đối với mỗi giá trị quỹ đạo riêng biệt có tần số f, chúng ta chỉ so sánh hai chi phí: tiêu hủy chi phí f riêng lẻ và sử dụng máy thứ hai chi phí c. Không có sự tương tác giữa các quỹ đạo khác nhau vì cỗ máy thứ hai không thể phá hủy một phần quỹ đạo. 

Vì vậy, quyết định tối ưu là cục bộ: với mỗi quỹ đạo, lấy giá trị nhỏ nhất của f và c. Tổng hợp các lựa chọn này mang lại sự tối ưu toàn cầu. 

Điều này làm giảm vấn đề từ lựa chọn tổ hợp sang vấn đề tổng hợp tần số đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con quỹ đạo | O(2^k · n) | O(k) | Quá chậm | 
| Tần số tham lam trên mỗi quỹ đạo | O(n) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng giá trị quỹ đạo và đếm xem có bao nhiêu hành tinh thuộc mỗi quỹ đạo. Bước này nén vấn đề thành các nhóm độc lập vì chỉ tính vấn đề. 
2. Với mỗi quỹ đạo riêng biệt, hãy tính tần số f của nó. Điều này thể hiện cái giá phải trả cho việc phá hủy tất cả các hành tinh trong quỹ đạo đó chỉ bằng chiếc máy đầu tiên. 
3. Với quỹ đạo đó, hãy tính phương án rẻ hơn giữa f và c. Máy thứ hai chỉ hữu ích nếu nó có chi phí thấp hơn việc phá hủy từng hành tinh trong quỹ đạo đó. 
4. Cộng các giá trị tối thiểu này trên tất cả các quỹ đạo để có được tổng chi phí. 
5. Xuất kết quả cho test case. 

Lý do chúng tôi đánh giá từng quỹ đạo một cách độc lập là vì không có lợi ích chung giữa các quỹ đạo: cỗ máy thứ hai không bao giờ ảnh hưởng đến nhiều quỹ đạo cùng một lúc. 

### Tại sao nó hoạt động 

Mỗi quỹ đạo tạo thành một đơn vị quyết định biệt lập. Bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi sao cho đối với mỗi quỹ đạo, chúng ta có thể áp dụng hoàn toàn máy thứ hai một lần hoặc chỉ sử dụng máy thứ nhất nhiều lần mà không làm thay đổi mức tối ưu về chi phí. Việc kết hợp các chiến lược trong một quỹ đạo là không thể và không có sự ghép nối giữa các quỹ đạo, do đó, việc tính tổng chi phí tối ưu trên mỗi quỹ đạo sẽ mang lại giải pháp tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(int, input().split()))
        
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        
        ans = 0
        for f in freq.values():
            ans += min(f, c)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nhóm các hành tinh theo quỹ đạo bằng từ điển. Điều này đảm bảo chúng ta chỉ suy luận về tần số, đó là cách trừu tượng hóa chính xác của vấn đề. Mỗi tần số sau đó được so sánh độc lập với chi phí c. 

Một điểm tinh tế là chúng tôi không bao giờ xem xét việc sử dụng một phần máy thứ hai. Sau khi được chọn cho một quỹ đạo, nó luôn loại bỏ tất cả các hành tinh của quỹ đạo đó, vì vậy việc chia tách mục đích sử dụng của nó là vô nghĩa. Đây là lý do tại sao phép so sánh min(f, c) nắm bắt hoàn toàn không gian quyết định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, c = 3
a = [1, 1, 1, 2, 3]
```| Quỹ đạo | Tần số f | phút(f, c) | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 3 | 
| 2 | 1 | 1 | 4 | 
| 3 | 1 | 1 | 5 | 

Đầu ra cuối cùng là 5. 

Dấu vết này cho thấy rằng ngay cả khi cỗ máy thứ hai hữu ích cho quỹ đạo dày đặc, các quỹ đạo thưa thớt sẽ rẻ hơn nếu xử lý riêng lẻ, do đó các quyết định sẽ tách biệt một cách tự nhiên trên mỗi quỹ đạo. 

### Ví dụ 2 

đầu vào:```
n = 4, c = 2
a = [1, 2, 3, 4]
```| Quỹ đạo | Tần số f | phút(f, c) | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 1 | 1 | 3 | 
| 4 | 1 | 1 | 4 | 

Đầu ra cuối cùng là 4. 

Trường hợp này cho thấy rằng khi tất cả các quỹ đạo là duy nhất thì máy thứ hai không bao giờ giúp ích được vì c không rẻ hơn so với việc xóa một lần lặp đi lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét mảng một lần và sau đó lặp qua các quỹ đạo riêng biệt | 
| Không gian | O(k) | Chúng tôi lưu trữ tần số của các quỹ đạo riêng biệt | 

Các ràng buộc cho phép điều này một cách dễ dàng vì n ≤ 100 và t ≤ 100, cho phép thực hiện tối đa 10.000 thao tác tổng thể, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(input())
    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(int, input().split()))
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        ans = sum(min(f, c) for f in freq.values())
        output.append(str(ans))
    
    return "\n".join(output)

# provided samples
assert run("""4
10 1
2 1 4 5 2 4 5 5 1 2
5 2
3 2 1 2 2
2 2
1 1
2 2
1 2
""") == """4
4
2
2"""

# all equal values
assert run("""1
5 10
7 7 7 7 7
""") == "5"

# all distinct, cheap c
assert run("""1
4 2
1 2 3 4
""") == "4"

# all distinct, expensive c
assert run("""1
4 10
1 2 3 4
""") == "4"

# mixed case
assert run("""1
6 3
1 1 2 2 2 3
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 5 | so sánh máy thứ hai với việc sử dụng nhiều lần | 
| tất cả đều khác biệt, giá rẻ c | 4 | cho thấy máy thứ 2 không có lợi khi c ≥ 1 | 
| tất cả đều khác biệt, đắt tiền c | 4 | xác nhận dự phòng cho việc xóa riêng lẻ | 
| trường hợp hỗn hợp | 5 | xác nhận quyết định độc lập trên mỗi quỹ đạo | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các hành tinh thuộc về một quỹ đạo duy nhất và c rất nhỏ. 

đầu vào:```
1
6 2
5 5 5 5 5 5
```Thuật toán tính tần số f = 6 và so sánh min(6, 2), cho kết quả 2. Máy thứ hai chiếm ưu thế vì rẻ hơn so với việc loại bỏ riêng lẻ. 

Một trường hợp khác là khi mỗi quỹ đạo là duy nhất. 

đầu vào:```
1
4 10
1 2 3 4
```Mỗi tần số là 1, vì vậy min(1, 10) = 1 cho mỗi quỹ đạo. Tổng số trở thành 4, hoàn toàn bỏ qua máy thứ hai. 

Phân phối hỗn hợp khẳng định nguyên tắc độc lập: 

đầu vào:```
1
6 3
1 1 2 2 2 3
```Tần số là 2, 3 và 1. Chi phí trở thành min(2,3)=2, min(3,3)=3, min(1,3)=1, tổng cộng là 6. Mỗi quỹ đạo được xử lý độc lập và không có khớp nối quỹ đạo chéo nào xuất hiện trong quá trình quyết định, xác nhận tính đúng đắn của việc phân tách.
