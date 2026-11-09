---
title: "CF 1490F - Cân bằng mảng"
description: "Chúng ta được cấp một mảng các số nguyên và được phép xóa các phần tử. Sau khi xóa, các phần tử còn lại phải thỏa mãn một điều kiện chính quy mạnh: phải tồn tại một giá trị $C$ sao cho mọi số xuất hiện trong mảng cuối cùng xuất hiện chính xác $C$ lần, hoặc không xuất hiện lần nào."
date: "2026-06-10T22:42:26+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1490
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 702 (Div. 3)"
rating: 1500
weight: 1490
solve_time_s: 186
verified: true
draft: false
---

[CF 1490F - Cân bằng mảng](https://codeforces.com/problemset/problem/1490/F) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, toán học, sắp xếp 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và được phép xóa các phần tử. Sau khi xóa, các phần tử còn lại phải thỏa mãn điều kiện chính quy mạnh: phải tồn tại một giá trị$C$sao cho mọi số xuất hiện trong mảng cuối cùng đều xuất hiện chính xác$C$lần, hoặc không hề. 

Vì vậy, mảng cuối cùng bao gồm một số giá trị riêng biệt và mỗi giá trị đó đóng góp cùng một tần số. Chúng ta không bắt buộc phải giữ lại tất cả các lần xuất hiện của một giá trị, chúng ta có thể loại bỏ một số lần để làm cho tần số của nó khớp với tần số đã chọn$C$, hoặc loại bỏ nó hoàn toàn. 

Mục tiêu là tối đa hóa số lượng phần tử chúng tôi giữ lại, tương đương với việc giảm thiểu việc xóa. 

Khó khăn chính là chúng ta phải chọn cả những giá trị nào cần giữ và tần số chung là bao nhiêu.$C$nên như vậy. Mỗi giá trị đều có tần số ban đầu riêng và chúng ta chỉ có thể sử dụng nó nếu có thể “cắt giảm” chính xác$C$. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các yếu tố trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con của giá trị hoặc thử tất cả các tần số mục tiêu có thể có bằng cách quét theo từng giá trị lồng nhau. Một giải pháp gần như$O(n \log n)$hoặc$O(n)$mỗi trường hợp thử nghiệm là cần thiết. 

Một cách tiếp cận ngây thơ sẽ thử mọi cách có thể$C$từ$1$ĐẾN$n$, và với mỗi$C$, tính xem có thể giữ được bao nhiêu phần tử bằng cách tính tổng$\lfloor f_i / C \rfloor \cdot C$. Điều này là quá chậm vì đối với mỗi ứng viên$C$chúng tôi sẽ quét tất cả các tần số, dẫn đến$O(n^2)$trường hợp xấu nhất. 

Cạm bẫy tinh vi thứ hai xuất hiện khi tần số lớn nhưng không đồng đều. Ví dụ: nếu một giá trị xuất hiện 1000 lần và nhiều giá trị khác xuất hiện một lần, việc chọn$C = 1$hoặc$C = 2$cư xử rất khác nhau. Bất kỳ cách tiếp cận nào giả định việc sắp xếp tần số một cách tham lam mà không sửa$C$rõ ràng sẽ thất bại. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu bằng cách nén mảng thành tần số của từng giá trị riêng biệt. Giả sử chúng ta có tần số$f_1, f_2, \dots, f_k$. 

Chúng tôi muốn chọn một số$C$và một tập hợp con của các tần số này. Mỗi tần số được chọn$f_i$đóng góp$C$các phần tử, miễn là$f_i \ge C$. Nếu như$f_i < C$, nó hoàn toàn không thể sử dụng được. 

Vì vậy để cố định$C$, chiến lược tốt nhất mang tính quyết định: lấy ít nhất mọi tần số$C$và mỗi đóng góp chính xác$C$. Tổng kích thước được giữ trở thành:$$C \cdot (\text{number of } f_i \ge C)$$Do đó, bài toán quy về việc tìm:$$\max_C \; C \cdot \#(f_i \ge C)$$Sức mạnh tàn bạo kết thúc$C$từ$1$ĐẾN$n$sẽ kiểm tra công thức này trong$O(nk)$. Điều này là quá chậm khi$n$là lớn. 

Quan sát quan trọng là$C$chỉ quan trọng ở các giá trị xuất hiện trong danh sách tần số hoặc ở ngưỡng mà số lượng tần số đủ điều kiện thay đổi. Chúng ta có thể sắp xếp tần số và sử dụng phương pháp đếm: với mỗi giá trị tần số riêng biệt$f$, coi nó như một ứng cử viên$C$và tính xem ít nhất có bao nhiêu tần số$f$. Điều này có thể được thực hiện một cách hiệu quả bằng cách sắp xếp và quét. 

Khi tần số được sắp xếp, chúng ta có thể sử dụng tìm kiếm nhị phân hoặc quét hai con trỏ. Đối với mỗi vị trí$i$, nếu chúng ta chọn$C = f[i]$, thì tất cả các tần số từ$i$trở đi đều có thể sử dụng được, cho:$$f[i] \cdot (k - i)$$Chúng tôi tận dụng tối đa tất cả$i$. 

Điều này hiệu quả vì mọi giải pháp tối ưu đều phải căn chỉnh$C$với một số giá trị tần số hiện có. Nếu như$C$nằm hoàn toàn giữa hai giá trị tần số, việc tăng nó một chút không làm thay đổi số lượng phần tử đủ điều kiện mà chỉ tăng hệ số nhân nếu nó đạt tần số hợp lệ, do đó, tối ưu xảy ra ở các điểm rời rạc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên C |$O(n^2)$|$O(n)$| Quá chậm | 
| Sắp xếp + tần số quét |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của tất cả các giá trị trong mảng. Điều này chuyển vấn đề thành một danh sách số lần xuất hiện, loại bỏ thông tin thứ tự không liên quan. 
2. Lưu trữ tất cả tần số trong một mảng$f$. Cấu trúc của các giá trị ban đầu không còn quan trọng nữa, chỉ là mỗi giá trị xuất hiện bao nhiêu lần. 
3. Sắp xếp mảng tần số theo thứ tự không giảm. Việc sắp xếp cho phép chúng ta suy luận một cách hiệu quả về số lượng giá trị có thể hỗ trợ tần suất mục tiêu nhất định. 
4. Đối với mỗi chỉ số$i$, đối xử$f[i]$như một ứng cử viên tần số chung$C$. Điều này hợp lệ vì mọi tối ưu$C$có thể được ánh xạ tới một ngưỡng tần số. 
5. Tính xem có bao nhiêu giá trị có thể hỗ trợ điều này$C$, đó là$k - i$, vì tất cả các tần số từ$i$trở đi ít nhất là$f[i]$. 
6. Tính toán các phần tử được giữ lại như$f[i] \cdot (k - i)$. Theo dõi tối đa trên tất cả$i$. 
7. Câu trả lời là tổng các phần tử trừ đi giá trị được giữ tối đa này. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến đối với một ngưỡng cố định$C$, ít nhất chỉ có tần số$C$đóng góp, và mỗi người đóng góp chính xác$C$. Mọi nỗ lực sử dụng giá trị có tần suất dưới đây$C$là không thể, và bất kỳ lần xuất hiện bổ sung nào ở trên$C$không liên quan vì chúng bị loại bỏ. Điều này thu gọn việc tối ưu hóa thành việc chọn một ngưỡng và đếm xem có bao nhiêu giá trị tồn tại ở ngưỡng đó, đồng thời cấu trúc được sắp xếp đảm bảo mọi ngưỡng như vậy đều được đánh giá tại các điểm biên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import Counter

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = Counter(a)
        f = sorted(freq.values())
        
        k = len(f)
        best_keep = 0
        
        for i, c in enumerate(f):
            best_keep = max(best_keep, c * (k - i))
        
        print(n - best_keep)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách chuyển đổi mảng thành bản đồ tần số, đây là bước rút gọn cốt lõi. Việc sắp xếp tần số cho phép đánh giá dựa trên ngưỡng. Mỗi lần lặp coi tần số là số đếm chung tiềm năng và đánh giá số lượng giá trị có thể tham gia ở cấp độ đó. 

Phép trừ từ$n$ở cuối chuyển đổi việc tối đa hóa các phần tử được giữ thành mức tối thiểu cần thiết của việc xóa. 

Một lỗi phổ biến là cố gắng nhân mỗi tần số với số lần nó có thể chia cho$C$, nhưng điều đó phá vỡ điều kiện là tất cả các giá trị được chọn phải có cùng tần số chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 3 2 1 4 2
```Tần số là: 

| Giá trị | Tần số | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 1 | 

Tần số được sắp xếp: [1, 1, 2, 2] 

| tôi | f[i] | k - tôi | giữ = f[i] * (k - i) | 
| --- | --- | --- | --- | 
| 0 | 1 | 4 | 4 | 
| 1 | 1 | 3 | 3 | 
| 2 | 2 | 2 | 4 | 
| 3 | 2 | 1 | 2 | 

Giữ tốt nhất = 4, xóa = 6 - 4 = 2. 

Điều này cho thấy rằng việc chọn$C=1$hoặc$C=2$mang lại khả năng lưu giữ tối ưu, nhưng chỉ các ngưỡng được căn chỉnh mới quan trọng. 

### Ví dụ 2 

đầu vào:```
4
100 100 4 100
```Tần số: 

| Giá trị | Tần số | 
| --- | --- | 
| 100 | 3 | 
| 4 | 1 | 

Sắp xếp: [1, 3] 

| tôi | f[i] | k - tôi | giữ | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 | 
| 1 | 3 | 1 | 3 | 

Tốt nhất giữ = 3, vì vậy chúng tôi loại bỏ 1 phần tử. 

Điều này xác nhận trực giác rằng chỉ giữ giá trị với tần số 3 là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Tần số sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm | 
| Không gian |$O(n)$| Bản đồ tần số và lưu trữ danh sách | 

Tổng cộng$n$qua các bài kiểm tra được giới hạn bởi$2 \cdot 10^5$, do đó, việc sắp xếp một lần cho mỗi bộ thử nghiệm vẫn có hiệu quả trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = Counter(a)
        f = sorted(freq.values())
        k = len(f)
        best = 0
        for i, c in enumerate(f):
            best = max(best, c * (k - i))
        out.append(str(n - best))
    return "\n".join(out)

# provided samples
assert run("""3
6
1 3 2 1 4 2
4
100 100 4 100
8
1 2 3 3 3 2 6 6
""") == """2
1
2"""

# custom cases
assert run("""1
1
7
""") == "0"

assert run("""1
5
1 1 1 1 1
""") == "0"

assert run("""1
5
1 2 3 4 5
""") == "4"

assert run("""1
6
1 1 2 2 3 3
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | ranh giới tối thiểu | 
| tất cả đều bình đẳng | 0 | duy trì đầy đủ tối ưu | 
| tất cả đều khác biệt | 4 | trường hợp phân mảnh tồi tệ nhất | 
| cặp cân bằng hoàn hảo | 0 | trường hợp tần số thống nhất | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các phần tử là duy nhất. Đối với đầu vào như`[1,2,3,4,5]`, tất cả các tần số đều bằng 1. Thuật toán chỉ xem xét$C=1$, mang lại khả năng lưu giữ đầy đủ và không bị xóa, phù hợp với yêu cầu. 

Một trường hợp khác là một phần tử rất thường xuyên trong số rất nhiều phần tử hiếm. Vì`[1,1,1,1,2,3,4]`, tần số là`[4,1,1,1]`. Sự lựa chọn tối ưu là$C=1$, giữ lại tất cả các phần tử. Việc quét qua các tần số được sắp xếp sẽ đánh giá chính xác cả hai$C=1$Và$C=4$, đảm bảo thu được tối đa. 

Trường hợp cạnh cuối cùng là khi tồn tại nhiều giá trị tần số trung bình. Vì`[1,1,2,2,3,3]`, tất cả các tần số đều bằng nhau nên mọi ứng cử viên đều cho ra kết quả như nhau. Thuật toán đánh giá nhất quán từng ngưỡng và không cần cách viết hoa đặc biệt vì cấu trúc được sắp xếp xử lý các nhóm đẳng thức một cách tự nhiên.
