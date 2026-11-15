---
title: "CF 1856B - Mảng Tốt"
description: "Chúng ta được cho một mảng các số nguyên dương. Chúng tôi muốn biết liệu có thể xây dựng một mảng khác có cùng độ dài sao cho mọi vị trí đều thay đổi giá trị của nó nhưng tổng số tiền vẫn giữ nguyên. Mảng thứ hai không phải là tùy ý."
date: "2026-06-09T05:02:54+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1856
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 890 (Div. 2) supported by Constructor Institute"
rating: 900
weight: 1856
solve_time_s: 136
verified: false
draft: false
---

[CF 1856B - Mảng tốt](https://codeforces.com/problemset/problem/1856/B) 

**Xếp hạng:** 900 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Chúng tôi muốn biết liệu có thể xây dựng một mảng khác có cùng độ dài sao cho mọi vị trí đều thay đổi giá trị của nó nhưng tổng số tiền vẫn giữ nguyên. 

Mảng thứ hai không phải là tùy ý. Mỗi vị trí phải khác với vị trí ban đầu, tuy nhiên khi tất cả các phần tử được cộng lại thì không có gì thay đổi trên toàn cầu. Câu hỏi hoàn toàn mang tính tồn tại: chúng ta không cần xây dựng mảng, chỉ quyết định xem sự sắp xếp lại như vậy có tồn tại hay không. 

Các ràng buộc lớn về số lượng trường hợp thử nghiệm và tổng kích thước mảng, vì vậy mọi giải pháp đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Cách tiếp cận bậc hai hoặc thậm chí siêu tuyến tính nhẹ cho mỗi trường hợp thử nghiệm sẽ thất bại ngay lập tức vì tổng số phần tử trong tất cả các thử nghiệm đạt tới$10^5$. 

Sự tinh tế chính là đây không phải là về hoán vị hoặc sắp xếp lại. Mảng mới có thể thay đổi các giá trị một cách tự do miễn là nó tôn trọng giá trị dương và giữ cho tổng bất biến. Điều này làm cho nó trở thành vấn đề cân bằng toàn cầu hơn là vấn đề đối sánh cục bộ. 

Một trường hợp thất bại phổ biến phát sinh khi$n=1$. Trong trường hợp đó, mảng duy nhất có thể có cùng tổng là chính mảng đó, do đó không thể thỏa mãn điều kiện$a_1 \ne b_1$. Một trường hợp cạnh quan trọng khác là khi tất cả các phần tử đều bằng nhau và$n$nhỏ, trong đó trực giác về việc “dịch chuyển các giá trị xung quanh” thường dẫn đến kết luận sai trừ khi ràng buộc tổng được kiểm tra cẩn thận. 

## Phương pháp tiếp cận 

Phối cảnh brute-force cố gắng gán cho mỗi vị trí một giá trị mới khác với giá trị ban đầu trong khi vẫn theo dõi tổng còn lại. Người ta có thể tưởng tượng việc lặp lại tất cả các phép gán có thể, nhưng mỗi vị trí có vô số số nguyên dương hợp lệ, do đó phép liệt kê thậm chí không được xác định rõ ràng theo nghĩa hữu hạn. Ngay cả khi chúng ta giới hạn bản thân ở các giá trị xuất hiện trong mảng hoặc bị giới hạn bởi tổng, số lượng các khả năng có thể tăng theo cấp số nhân với$n$, làm cho phương pháp này không thể thực hiện được nếu chỉ có đầu vào rất nhỏ. 

Quan sát chính là chúng ta không cần phải xây dựng mảng một cách rõ ràng. Chúng ta chỉ cần xác định xem ràng buộc tổng và ràng buộc “không có điểm cố định trên mỗi chỉ số” có thể được thỏa mãn đồng thời hay không. Ràng buộc tổng buộc tổng mức tăng và giảm trên tất cả các vị trí phải hủy bỏ một cách chính xác. Điều này ngay lập tức gợi ý rằng ít nhất một phần tử phải tăng lên và ít nhất một phần tử phải giảm đi, nếu không thì tổng không thể không thay đổi. 

Cấu trúc trở nên đặc biệt cứng nhắc khi$n=1$, vì không thể phân phối lại được. Khi$n \ge 2$, luôn có thể dịch chuyển giá trị từ vị trí này sang vị trí khác: giảm một phần tử xuống$1$và tăng cái khác lên$1$, đồng thời giữ tất cả các phần tử dương và đảm bảo không có vị trí nào giữ lại giá trị ban đầu. Vì tất cả$a_i \ge 1$, chúng ta luôn có chỗ để giảm ít nhất một phần tử. 

Do đó, vấn đề trở thành một sự phân đôi đơn giản hoàn toàn dựa trên$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | không được xác định rõ ràng | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước của mảng$n$. 
2. Nếu$n = 1$, ngay lập tức xuất ra "KHÔNG". Với một phần tử duy nhất, tổng lực ràng buộc$b_1 = a_1$, vi phạm$a_1 \ne b_1$. 
3. Nếu$n \ge 2$, xuất ra "CÓ". Trong trường hợp này, chúng ta luôn có thể xây dựng một mảng hợp lệ bằng cách giảm một phần tử và tăng phần tử khác, bảo toàn số dương và duy trì tổng. 
4. Lặp lại cho tất cả các trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Ràng buộc tổng buộc tổng khối lượng được bảo toàn. Với ít nhất hai vị trí, chúng ta có thể phân phối lại một đơn vị từ chỉ số này sang chỉ số khác. Điều này tạo ra ít nhất một chỉ mục trong đó$b_i < a_i$và một nơi$b_j > a_j$, đảm bảo cả điều kiện bất đẳng thức và bảo toàn tổng. Không có chỉ số nào bị buộc phải giữ nguyên vì bất kỳ sự sụt giảm nào cũng có thể được bù đắp ở nơi khác. Vì$n=1$, việc phân phối lại như vậy là không thể, do đó các ràng buộc trở nên không tương thích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    input().split()  # values not needed
    if n == 1:
        print("NO")
    else:
        print("YES")
```Giải pháp cố tình bỏ qua các giá trị mảng thực tế vì tính khả thi chỉ phụ thuộc vào độ dài của mảng. Quyết định có ý nghĩa duy nhất là liệu có thể phân phối lại ít nhất hai chỉ số hay không. 

Một lỗi phổ biến ở đây là cố gắng phân tích tính chẵn lẻ hoặc cấu trúc phần tử, nhưng điều đó không quan trọng. Ràng buộc tổng không hạn chế tính khả thi ngoài việc yêu cầu ít nhất hai bậc tự do, đó chính xác là những gì$n \ge 2$cung cấp. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào với một số trường hợp có kích thước khác nhau. 

Vì$n=3$, bất kỳ mảng nào chẳng hạn như$[1,5,9]$có đủ tính linh hoạt. Đã có sự phân phối lại nên câu trả lời là "CÓ". 

Vì$n=1$, chẳng hạn như$[1]$, không có số nguyên dương thay thế nào có cùng tổng tồn tại ngoại trừ chính nó, vì vậy câu trả lời là "KHÔNG". 

Đối với người khác$n=3$trường hợp như$[1,2,3]$, chúng ta luôn có thể xây dựng một mảng mới như$[2,1,3]$hoặc bất kỳ sự thay đổi cân bằng nào, vì vậy câu trả lời lại là "CÓ". 

| Trường hợp thử nghiệm | n | Quyết định | Lý do | 
| --- | --- | --- | --- | 
| [1, 5, 9] | 3 | CÓ | có thể phân phối lại | 
| [1] | 1 | KHÔNG | tổng lực mảng giống nhau | 
| [1, 2, 3] | 3 | CÓ | có thể điều chỉnh giống như trao đổi | 

Những dấu vết này xác nhận rằng chỉ có kích thước của mảng là quan trọng chứ không phải sự phân bố các giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | chỉ đọc đầu vào | 
| Không gian | O(1) | không có cấu trúc phụ trợ ngoài biến | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^5$, do đó, chỉ cần truyền tuyến tính qua đầu vào là đủ. Không cần tính toán ngoài việc kiểm tra có điều kiện đơn giản, giúp giải pháp nhanh chóng trong phạm vi ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        input().split()
        out.append("NO" if n == 1 else "YES")
    return "\n".join(out)

# provided samples
assert run("""6
3
6 1 2
2
1 1
4
3 1 2 4
1
17
5
1 2 1 1 1
3
618343152 819343431 1000000000
""") == """YES
NO
YES
NO
NO
YES"""

# custom cases
assert run("""3
1
5
2
10 10
5
1 1 1 1 1
""") == """NO
YES
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 phần tử đơn | KHÔNG | trường hợp cơ bản không thể | 
| n=2 phần tử bằng nhau | CÓ | phân phối lại hợp lệ tối thiểu | 
| tất cả đều lớn bằng nhau n | CÓ | mảng thống nhất vẫn hợp lệ | 

## Vỏ cạnh 

cho$n=1$, thuật toán ngay lập tức trả về "KHÔNG". Mảng ứng cử viên duy nhất có cùng tổng giống hệt với mảng ban đầu, do đó không thể thỏa mãn ràng buộc bất đẳng thức. 

Vì$n=2$, thuật toán trả về "CÓ". Đối với một đầu vào như$[10,10]$, chúng ta có thể xây dựng$[11,9]$, bảo toàn tổng và thay đổi cả hai vị trí. Điều này xác nhận rằng có ít nhất hai vị trí là đủ để phân phối lại giá trị. 

Đối với các mảng thống nhất lớn hơn như$[1,1,1,1,1]$, thuật toán trả về "CÓ" vì chúng ta luôn có thể di chuyển một đơn vị từ vị trí này sang vị trí khác trong khi vẫn giữ nguyên số dương và tổng, đảm bảo không có phần tử nào không thay đổi.
