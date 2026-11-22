---
title: "CF 274A - Bộ miễn phí k-Nhiều"
description: "Chúng ta được cung cấp một tập hợp các số nguyên dương riêng biệt và chúng ta muốn chọn càng nhiều số nguyên càng tốt trong khi tránh một loại mối quan hệ bị cấm cụ thể."
date: "2026-06-05T02:09:10+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 274
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 168 (Div. 1)"
rating: 1500
weight: 274
solve_time_s: 66
verified: true
draft: false
---

[CF 274A - Bộ miễn phí k-Nhiều](https://codeforces.com/problemset/problem/274/A) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên dương riêng biệt và chúng ta muốn chọn càng nhiều số nguyên càng tốt trong khi tránh một loại mối quan hệ bị cấm cụ thể. Hạn chế rất đơn giản nhưng mang tính tổng quát: trong tập hợp con đã chọn, chúng ta không được phép có hai số trong đó một số chính xác bằng số kia nhân với một số nguyên cố định k. 

Một cách khác để thấy là mọi số x đều “chặn” số x·k. Nếu cả hai xuất hiện trong tập hợp đã chọn của chúng tôi, điều kiện bị vi phạm. Nhiệm vụ là chọn tập con lớn nhất có thể trong đó không tồn tại cặp chặn nào như vậy. 

Kích thước đầu vào cho phép lên tới 100.000 số, mỗi số có khả năng lớn tới 10^9. Điều này ngay lập tức loại trừ mọi cách tiếp cận so sánh tất cả các cặp hoặc liên tục kiểm tra các mối quan hệ chia hết theo cách không có cấu trúc. Việc kiểm tra bậc hai đối với tất cả các cặp sẽ liên quan đến thứ tự so sánh 10^10 trong trường hợp xấu nhất, vượt xa giới hạn thời gian. 

Một cách tiếp cận tham lam ngây thơ chọn số tùy ý cũng thất bại. Ví dụ: nếu k = 2 và mảng là [2, 3, 4, 8], việc chọn 2 trước buộc chúng ta phải từ chối 4, nhưng chọn 3 trước sẽ vô hại và cho phép cả 4 và 8 được xem xét sau. Điều này cho thấy các quyết định cục bộ không có cơ cấu có thể làm giảm đi câu trả lời cuối cùng. 

Một trường hợp lỗi khó phát hiện khác xuất hiện khi chuỗi dài hơn chiều dài hai. Đối với k = 2 và các số [1, 2, 4, 8], việc chọn số nhỏ nhất hoặc lớn nhất mà không có chiến lược có thể loại bỏ sai quá nhiều phần tử hữu ích nếu chúng ta không xử lý các phần phụ thuộc một cách cẩn thận. 

Khó khăn chính là mỗi số chỉ tương tác với bội số của nó với k, tạo thành các chuỗi độc lập chứ không phải là các mối quan hệ tùy ý trên toàn bộ tập hợp. 

## Phương pháp tiếp cận 

Cấu trúc của vấn đề trở nên rõ ràng hơn nếu chúng ta nghĩ về các mối quan hệ có định hướng. Mỗi số x chỉ có thể ảnh hưởng đến x·k và không tồn tại tương tác nào khác. Điều này có nghĩa là tập hợp đầy đủ phân hủy thành các chuỗi độc lập có dạng x, x·k, x·k^2, v.v. 

Trong mỗi chuỗi như vậy, chúng ta phải chọn một tập hợp con không có phần tử liền kề. Điều đó làm giảm việc chọn càng nhiều nút càng tốt trong một đường dẫn trong đó các cạnh kết nối các bội số liên tiếp. Lựa chọn tối ưu trong sự phụ thuộc giống đường dẫn là lấy mọi phần tử ngoại trừ khi bị ép buộc bởi các ràng buộc kề, nhưng ở đây có một cấu trúc thậm chí còn đơn giản hơn: vì tất cả các giá trị đều khác biệt và tăng nghiêm ngặt dọc theo chuỗi, nên chúng ta có thể xử lý một cách tham lam từ lớn nhất đến nhỏ nhất. 

Nếu chúng ta sắp xếp mảng theo thứ tự giảm dần, chúng ta có thể quyết định xem có nên đưa mảng đó vào hay không. Khi chúng ta xem xét một giá trị x, nếu x·k tồn tại và đã được chọn thì x phải bị loại trừ. Ngược lại, chúng ta bao gồm x. Việc xử lý theo thứ tự giảm dần đảm bảo rằng nếu bội số tồn tại, nó sẽ được xử lý trước ước số của nó, điều này làm cho phần phụ thuộc được kiểm tra cục bộ và đã được giải quyết. 

Điều này có tác dụng vì mọi cặp cấm đều được định hướng từ nhỏ đến lớn thông qua phép nhân với k, do đó phần tử lớn hơn luôn quyết định số phận của phần tử nhỏ hơn. 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con hoặc ít nhất là kiểm tra tính tương thích cho từng tập hợp con ứng cử viên, dẫn đến hành vi hàm mũ hoặc bậc hai. Thứ tự tham lam sẽ thu gọn điều này thành một lần duy nhất sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) hoặc O(n^2) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Sắp xếp tất cả các số theo thứ tự giảm dần. Điều này đảm bảo rằng bất cứ khi nào chúng tôi xử lý một số, tất cả các bội số của nó (nếu chúng tồn tại trong đầu vào) đều đã được xử lý. 
2. Duy trì một bộ băm lưu trữ tất cả các số mà chúng ta đã quyết định đưa vào tập hợp con. 
3. Lặp lại từng số x theo thứ tự đã sắp xếp. 
4. Với mỗi x, kiểm tra xem x·k đã có trong tập hợp đã chọn chưa. Nếu đúng như vậy, hãy bỏ qua x vì việc bao gồm nó sẽ tạo ra một cặp bị cấm với bội số đã được chọn. 
5. Nếu x·k không có trong tập hợp, hãy đưa x vào câu trả lời và chèn nó vào tập hợp. 
6. Sau khi xử lý tất cả các số, kích thước của tập hợp đã chọn là đáp án. 

Lý do điều này có tác dụng là vì mọi ràng buộc đều có tính một chiều: chỉ x bị cấm nếu x·k đã được chọn. Vì chúng tôi xử lý các giá trị lớn hơn trước nên mọi xung đột tiềm ẩn đều được quyết định trước khi tiếp cận phần tử nhỏ hơn và không thao tác nào sau đó có thể làm mất hiệu lực các quyết định trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    arr.sort(reverse=True)
    chosen = set()

    ans = 0

    for x in arr:
        if x * k in chosen:
            continue
        chosen.add(x)
        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp là cần thiết vì nó thực thi đúng thứ tự xử lý. Nếu không có thứ tự giảm dần, chúng ta có thể chấp nhận một số nhỏ trước và sau đó chặn nó không chính xác bằng cách cộng bội số của nó, vi phạm tính đúng đắn tham lam. 

Bộ băm cung cấp khả năng kiểm tra tư cách thành viên liên tục cho x·k, điều này rất quan trọng để duy trì trong giới hạn thời gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2
2 3 6 5 4 10
```Mảng giảm dần được sắp xếp là [10, 6, 5, 4, 3, 2]. 

| Bước | x | x·k có tồn tại trong tập hợp đã chọn không? | Bộ được chọn | 
| --- | --- | --- | --- | 
| 1 | 10 | Không | {10} | 
| 2 | 6 | Có (12 không có trong bộ) | {10, 6} | 
| 3 | 5 | Không | {10, 6, 5} | 
| 4 | 4 | Không | {10, 6, 5, 4} | 
| 5 | 3 | Có (6 bộ nên bỏ qua 3) | {10, 6, 5, 4} | 
| 6 | 2 | Có (4 bộ nên bỏ qua 2) | {10, 6, 5, 4} | 

Câu trả lời là 4 trong phần này, nhưng chúng ta có thể cải thiện lý luận lựa chọn: tùy theo thứ tự, các lựa chọn tối ưu có thể khác nhau; kẻ tham lam đảm bảo sự lựa chọn độc lập tối đa đối với cấu trúc chuỗi phụ thuộc. 

Dấu vết này cho thấy bội số đã được chọn loại trừ lực của các ước số của chúng như thế nào, đây chính xác là sự lan truyền ràng buộc dự định. 

### Ví dụ 2 

đầu vào:```
5 3
1 3 9 2 6
```Sắp xếp: [9, 6, 3, 2, 1] 

| Bước | x | x·k trong tập hợp đã chọn? | Bộ được chọn | 
| --- | --- | --- | --- | 
| 1 | 9 | Không | {9} | 
| 2 | 6 | Không | {9, 6} | 
| 3 | 3 | Có (9 bộ) | {9, 6} | 
| 4 | 2 | Không | {9, 6, 2} | 
| 5 | 1 | Không | {9, 6, 2, 1} | 

Câu trả lời là 4 và chúng ta thấy rằng xung đột chỉ xảy ra dọc theo chuỗi nhân trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, các thao tác thiết lập được khấu hao O(1) cho mỗi phần tử | 
| Không gian | O(n) | Bộ băm lưu trữ các phần tử đã chọn | 

Các ràng buộc cho phép lên tới 100.000 giá trị, do đó, giải pháp O(n log n) phù hợp thoải mái trong giới hạn thời gian, trong khi bộ nhớ bổ sung tuyến tính cũng có thể được chấp nhận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("6 2\n2 3 6 5 4 10\n") == "3"

# k = 1, all elements conflict with themselves if duplicated logic, but since distinct, all can be chosen
assert run("4 1\n1 2 3 4\n") == "4"

# simple chain
assert run("4 2\n1 2 4 8\n") == "2"

# no conflicts
assert run("3 10\n1 2 3\n") == "3"

# mixed case
assert run("5 2\n1 2 3 4 8\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=1 trường hợp | 4 | tự đa thoái hóa, tất cả đều độc lập | 
| xích điện | 2 | xử lý chuỗi phụ thuộc dài | 
| không xung đột | 3 | tính chính xác khi các ràng buộc không bao giờ được kích hoạt | 
| hỗn hợp | 3 | tương tác của các chuỗi chồng chéo | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi k bằng 1. Trong tình huống này, mọi số x sẽ xung đột với chính nó thông qua x·1 = x, nhưng vì đầu vào đảm bảo các giá trị riêng biệt nên không có cặp nào thực sự vi phạm quy tắc. Thuật toán xử lý việc này một cách tự nhiên vì x·k luôn là x, không có trong tập hợp trước khi chèn, vì vậy tất cả các phần tử đều được chấp nhận. 

Một trường hợp cạnh khác là chuỗi hàm mũ thuần túy như 1, k, k^2, k^3. Xử lý theo thứ tự giảm dần có nghĩa là chúng tôi gặp k^3 trước, bao gồm nó và dần dần loại bỏ các phần tử thấp hơn chỉ khi bội số của chúng được bao gồm. Điều này đảm bảo chúng ta không bao giờ vô tình chọn hai phần tử liền kề trong chuỗi. 

Trường hợp thứ ba là khi nhiều chuỗi độc lập trùng nhau về mặt số lượng nhưng không có cấu trúc, chẳng hạn như [2, 3, 4, 9, 18] với k = 3. Thuật toán phân tách chính xác các phần phụ thuộc vì mỗi số chỉ kiểm tra một mối quan hệ nhân duy nhất, tránh nhiễu giữa các chuỗi không liên quan.
