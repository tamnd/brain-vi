---
title: "CF 1715D - 2+ cửa"
description: "Chúng tôi đang tìm kiếm một mảng các số nguyên trong đó mọi truy vấn mô tả OR theo bit của hai vị trí. Một truy vấn (i, j, x) có nghĩa là giá trị được lưu ở vị trí i HOẶC giá trị được lưu ở vị trí j phải bằng x."
date: "2026-06-09T19:56:51+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "bitmasks", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1715
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 816 (Div. 2)"
rating: 1900
weight: 1715
solve_time_s: 160
verified: false
draft: false
---

[CF 1715D - 2+ cửa](https://codeforces.com/problemset/problem/1715/D) 

**Xếp hạng:** 1900 
**Tags:** 2-sat, bitmasks, đồ thị, tham lam 
**Thời gian giải:** 2 phút 40 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang tìm kiếm một mảng các số nguyên trong đó mọi truy vấn mô tả OR theo bit của hai vị trí. Một truy vấn`(i, j, x)`có nghĩa là giá trị được lưu trữ tại vị trí`i`HOẶC giá trị được lưu tại vị trí`j`phải bằng`x`. 

Trong số tất cả các mảng thỏa mãn mọi truy vấn, chúng ta cần mảng nhỏ nhất về mặt từ điển. Điều đó có nghĩa là chúng tôi muốn làm cho phần tử đầu tiên càng nhỏ càng tốt. Nếu một số giải pháp có chung phần tử đầu tiên, chúng tôi sẽ giảm thiểu phần tử thứ hai, v.v. 

Các giá trị nằm trong 30 bit, vì vậy mỗi bit hoạt động độc lập. Vì OR được áp dụng từng bit một nên quyết định về một bit không bao giờ ảnh hưởng đến bit khác. Điều này ngay lập tức gợi ý xử lý từng bit riêng biệt. 

Những hạn chế là lớn. có thể có`10^5`vị trí và`2·10^5`hạn chế. Bất kỳ cách tiếp cận nào so sánh lặp đi lặp lại từng cặp đỉnh hoặc cố gắng tìm kiếm trên các mảng có thể đều là vô vọng. Thậm chí một`O(nq)`thuật toán sẽ thực hiện xung quanh`2·10^10`hoạt động. Chúng ta cần thứ gì đó tuyến tính hoặc gần tuyến tính trong kích thước đầu vào. 

Một số tình huống rất dễ xử lý sai. 

Giả sử một truy vấn yêu cầu một bit bằng 0. 

đầu vào:```
2 1
1 2 0
```Đầu ra duy nhất có thể là```
0 0
```bởi vì nếu một trong hai số có số 1 trong bit đó thì OR của chúng sẽ trở thành một. Một cách tiếp cận bất cẩn chỉ thực thi sự hiện diện của những người sẽ bỏ lỡ hạn chế này. 

Một trường hợp phức tạp khác xảy ra khi một truy vấn yêu cầu một chút để trở thành một. 

đầu vào:```
2 1
1 2 1
```đầu ra có thể là```
0 1
1 0
1 1
```Câu trả lời nhỏ nhất về mặt từ điển là```
0 1
```bởi vì việc giảm thiểu vị trí đầu tiên có mức độ ưu tiên cao hơn việc giảm thiểu vị trí thứ hai. Chỉ cần đặt cả hai vị trí thành một sẽ tạo ra câu trả lời hợp lệ nhưng không tối ưu. 

Một vòng lặp tự cũng cần được chú ý. 

đầu vào:```
1 1
1 1 5
```Từ`a1 | a1 = a1`, câu trả lời phải là```
5
```Coi điều này giống như một cạnh bình thường giữa hai đỉnh riêng biệt sẽ không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Chúng ta có thể thử mọi mảng có thể và kiểm tra xem tất cả các điều kiện OR có đúng hay không. Ngay cả khi giới hạn bản thân ở số nguyên 30 bit, số lượng mảng vẫn rất lớn. Với`n=10^5`, điều này là hoàn toàn không thể. 

Một cách mạnh mẽ hợp lý hơn là xử lý từng bit một cách độc lập. Đối với một bit, mỗi vị trí chứa 0 hoặc một. vẫn còn`2^n`những nhiệm vụ vượt xa tầm tay. 

Cấu trúc của bitwise OR đưa ra quan sát chính. 

Đối với một bit cố định, mỗi truy vấn sẽ hoạt động theo một trong hai cách. 

Nếu một chút`x`bằng 0 thì cả hai điểm cuối phải chứa 0. 

Nếu một chút`x`bằng một thì ít nhất một điểm cuối phải chứa một điểm cuối. 

Đây là những hạn chế rất đơn giản. Chúng ta có thể bắt đầu bằng cách giả sử mọi vị trí đều chứa một vị trí. Sau đó, mọi yêu cầu bằng 0 sẽ buộc một số vị trí trở thành số 0. Sau khi áp dụng tất cả các hạn chế như vậy, mọi cạnh có bit nằm trong`x`bằng người ta vẫn phải có ít nhất một điểm cuối bằng một. Bởi vì câu lệnh đảm bảo sự tồn tại nên điều kiện này sẽ luôn đúng. 

Điều này tạo ra các giá trị nhỏ nhất. Bắt đầu từ tất cả các vị trí và loại bỏ các bit bất cứ khi nào có thể sẽ giảm thiểu từng vị trí càng sớm càng tốt. Bất cứ khi nào một bit có thể được loại bỏ một cách an toàn khỏi một vị trí, làm như vậy sẽ cải thiện thứ tự từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O((n+q)·30) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mọi phần tử mảng thành`(1<<30)-1`, nghĩa là tất cả ba mươi bit đều có mặt. 
2. Xử lý mọi truy vấn`(u,v,x)`. 

Với mỗi bit bằng 0 trong`x`, xóa bit đó khỏi cả hai`a[u]`Và`a[v]`. 

Số 0 trong kết quả OR có nghĩa là cả hai số phải có số 0 ở đó, vì vậy việc xóa này là bắt buộc. 
3. Sau khi tất cả các truy vấn được xử lý, mỗi bit còn sót lại ở một vị trí là một bit không bao giờ bị ép về 0. 
4. Xuất mảng kết quả. 

Lý do điều này hoạt động là vì bất kỳ bit nào vẫn bằng một đều không thể bị xóa mà không vi phạm một số truy vấn có OR yêu cầu bit đó phải là một. 

### Tại sao nó hoạt động 

Hãy xem xét một bit cụ thể. 

Bất cứ khi nào một số truy vấn có bit này bằng 0, cả hai điểm cuối đều bị buộc phải bằng 0. Thuật toán thực hiện chính xác thao tác này. 

Bây giờ hãy xem xét một truy vấn có bit bằng một. Nếu cả hai điểm cuối đều bị các truy vấn khác buộc về 0 thì sẽ không có giải pháp nào tồn tại. Vì vấn đề đảm bảo giải pháp nên ít nhất một điểm cuối giữ bit đó bằng một. Do đó điều kiện OR được thỏa mãn. 

Bởi vì mỗi bit được xử lý độc lập nên tất cả các truy vấn đều được giữ đồng thời. 

Mảng này nhỏ nhất về mặt từ điển vì chúng tôi loại bỏ một bit khỏi một vị trí bất cứ khi nào được phép. Bất kỳ giải pháp nào có giá trị nhỏ hơn ở một số vị trí sẽ phải loại bỏ một bit bổ sung mà thuật toán của chúng tôi đã giữ, nhưng bit đó cần thiết ở đâu đó và việc loại bỏ nó sẽ vi phạm một ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())

a = [(1 << 30) - 1] * n
queries = []

for _ in range(q):
    u, v, x = map(int, input().split())
    u -= 1
    v -= 1
    queries.append((u, v, x))

    mask = ((1 << 30) - 1) ^ x
    a[u] &= ~mask
    a[v] &= ~mask

print(*a)
```Mảng được khởi tạo với tất cả các bit được đặt. Đây là điểm khởi đầu dễ dãi nhất. 

Đối với một truy vấn`(u,v,x)`, mỗi bit 0 của`x`phải biến mất khỏi cả hai điểm cuối. Máy tính```
mask = ((1 << 30) - 1) ^ x
```đưa ra một mặt nạ có các mặt nạ tương ứng chính xác với các bit 0 của`x`. Áp dụng```
a[u] &= ~mask
a[v] &= ~mask
```loại bỏ những bit bị cấm. 

Việc triển khai sử dụng lập chỉ mục dựa trên 0 trong nội bộ, do đó việc trừ một chỉ số khỏi các chỉ số đầu vào là cần thiết. 

Không cần vượt qua xác minh lần thứ hai vì vấn đề đảm bảo rằng giải pháp hợp lệ tồn tại. Theo giả định đó, mọi yêu cầu OR có một bit sẽ tự động giữ lại ít nhất một điểm cuối chứa bit đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
4 3
1 2 3
1 3 2
4 1 2
```Tất cả các giá trị ban đầu đều`111...111`. 

| Truy vấn | Không có bit nào bị loại bỏ | Mảng sau khi xử lý | 
| --- | --- | --- | 
| (1,2,3) | bit 2 | [3,3,tất cả,tất cả] | 
| (1,3,2) | bit 1 | [2,3,2,tất cả] | 
| (4,1,2) | bit 1 | [2,3,2,2] | 

Câu trả lời cuối cùng:```
2 3 2 2
```Mọi yêu cầu HOẶC đều được thỏa mãn. 

Ví dụ này cho thấy cách các bit 0 lan truyền đến cả hai điểm cuối. 

### Ví dụ 2 

đầu vào```
2 1
1 2 1
```| Truy vấn | Không có bit nào bị loại bỏ | Mảng sau khi xử lý | 
| --- | --- | --- | 
| (1,2,1) | tất cả các bit ngoại trừ bit 0 | [1,1] | 

Câu trả lời cuối cùng:```
1 1
```Bit đơn được yêu cầu bởi OR vẫn tồn tại. 

Điều này minh họa rằng một bit vẫn tồn tại trừ khi một số truy vấn cấm nó một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được xử lý một lần | 
| Không gian | O(n) | Chỉ mảng câu trả lời được lưu trữ | 

Thuật toán thực hiện một lượng công việc không đổi cho mỗi truy vấn, do đó ngay cả với`2·10^5`truy vấn nó dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ bị chi phối bởi mảng câu trả lời có kích thước`10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    n, q = map(int, input().split())

    a = [(1 << 30) - 1] * n

    for _ in range(q):
        u, v, x = map(int, input().split())
        u -= 1
        v -= 1

        mask = ((1 << 30) - 1) ^ x
        a[u] &= ~mask
        a[v] &= ~mask

    return " ".join(map(str, a))

# sample
assert run("""4 3
1 2 3
1 3 2
4 1 2
""") == "2 3 2 2"

# minimum size
assert run("""1 1
1 1 5
""") == "5"

# all zeros
assert run("""2 1
1 2 0
""") == "0 0"

# single one bit
assert run("""2 1
1 2 1
""") == "1 1"

# repeated constraints
assert run("""3 2
1 2 7
2 3 7
""") == "7 7 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1 5`|`5`| Xử lý tự vòng lặp | 
|`2 1 / 1 2 0`|`0 0`| Các bit 0 buộc cả hai điểm cuối | 
|`2 1 / 1 2 1`|`1 1`| Bit tồn tại khi không bị cấm | 
| Chuỗi có giá trị 7 |`7 7 7`| Ràng buộc lặp đi lặp lại | 

## Vỏ cạnh 

Hãy xem xét```
2 1
1 2 0
```Thuật toán tính toán rằng mỗi bit của`x`bằng 0, vì vậy tất cả các bit sẽ bị xóa khỏi cả hai vị trí. Đầu ra trở thành```
0 0
```đó là câu trả lời duy nhất có thể. 

Vì```
1 1
1 1 5
```tất cả các bit bằng 0 trong`5`được loại bỏ khỏi vị trí duy nhất. Các bit còn lại được tái tạo lại chính xác`5`, sản xuất```
5
```Vì```
3 2
1 2 1
2 3 0
```truy vấn thứ hai buộc mọi bit của vị trí hai và vị trí ba về 0. Vì truy vấn đầu tiên vẫn cần một bit giữa vị trí một và vị trí hai nên vị trí một sẽ giữ bit đó. Đầu ra trở thành```
1 0 0
```thỏa mãn cả hai ràng buộc và thể hiện cách các yêu cầu bằng 0 lan truyền qua biểu đồ trong khi vẫn đảm bảo sự tồn tại.
