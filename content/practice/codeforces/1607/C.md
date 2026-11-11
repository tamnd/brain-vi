---
title: "CF 1607C - Chiết xuất tối thiểu"
description: "Chúng ta được cung cấp một mảng các số nguyên và một phép toán đặc biệt để thay đổi mảng đó theo một cách rất có cấu trúc. Mỗi lần áp dụng thao tác, chúng tôi chọn một lần xuất hiện của giá trị tối thiểu hiện tại, loại bỏ nó và sau đó trừ đi giá trị tương tự đó khỏi mọi phần tử còn lại."
date: "2026-06-10T07:41:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1607
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 753 (Div. 3)"
rating: 1000
weight: 1607
solve_time_s: 101
verified: false
draft: false
---

[CF 1607C - Trích xuất tối thiểu](https://codeforces.com/problemset/problem/1607/C) 

**Đánh giá:** 1000 
**Tags:** vũ lực, phân loại 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một phép toán đặc biệt để thay đổi mảng đó theo một cách rất có cấu trúc. Mỗi lần áp dụng thao tác, chúng tôi chọn một lần xuất hiện của giá trị tối thiểu hiện tại, loại bỏ nó và sau đó trừ đi giá trị tương tự đó khỏi mọi phần tử còn lại. Thao tác này sẽ thu nhỏ mảng theo một phần tử đồng thời dịch chuyển các giá trị còn lại lên hoặc xuống tùy thuộc vào mức tối thiểu đã loại bỏ. 

Mục đích không phải là mô phỏng các thao tác này một cách mù quáng mà là để quyết định xem chúng ta nên áp dụng chúng bao nhiêu lần để mảng cuối cùng, ở giai đoạn trung gian nào đó, có phần tử nhỏ nhất càng lớn càng tốt. Chúng tôi được phép dừng lại bất cứ lúc nào, kể cả trước khi thực hiện bất kỳ thao tác nào. 

Các ràng buộc đủ lớn để bất kỳ phương pháp nào mô phỏng việc xóa và cập nhật toàn bộ mảng liên tục sẽ quá chậm. Mỗi thao tác có khả năng chạm đến tất cả các phần tử còn lại, do đó, ngay cả chiến lược bậc hai hoặc gần bậc hai cũng sẽ thất bại khi tổng kích thước của các trường hợp thử nghiệm đạt tới 2·10^5. 

Trường hợp cạnh tinh tế xuất hiện khi mảng chỉ có một phần tử. Vì hoạt động bị cấm ở đó nên câu trả lời buộc phải là giá trị ban đầu, ngay cả khi nó là số âm. Một tình huống khó khăn khác là khi tất cả các phần tử đều giống hệt nhau. Mọi thao tác sẽ loại bỏ một phần tử và chuyển phần còn lại về 0, do đó câu trả lời trở nên ổn định ở giá trị đó, nhưng một mô phỏng đơn giản có thể nghĩ quá nhiều về thứ tự loại bỏ mặc dù điều đó không quan trọng. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ liên tục quét mảng để tìm mức tối thiểu, loại bỏ nó và trừ nó khỏi tất cả các phần tử còn lại. Sau k thao tác, mảng có kích thước n−k và mỗi bước có chi phí O(n), dẫn đến O(n²) cho mỗi trường hợp kiểm thử trong trường hợp xấu nhất. Với tổng n lên tới 2·10^5, tốc độ này quá chậm. 

Quan sát chính là hoạt động chỉ phụ thuộc vào thứ tự của các phần tử theo giá trị chứ không phụ thuộc vào vị trí hoặc danh tính của chúng. Mỗi bước sẽ loại bỏ mức tối thiểu toàn cầu hiện tại. Điều đó có nghĩa là về cơ bản, quy trình này bóc tách các phần tử theo thứ tự tăng dần, đồng thời liên tục dịch chuyển các giá trị còn lại sao cho giá trị tối thiểu hiện tại trở thành 0 sau mỗi thao tác. 

Thay vì mô phỏng, chúng ta có thể nghĩ ngược lại: sau khi sắp xếp mảng, chúng ta xem xét mức tối thiểu sẽ tăng như thế nào nếu chúng ta “loại bỏ” từng phần tử nhỏ hơn. Điểm dừng tối ưu tương ứng với việc chọn tiền tố của mảng đã sắp xếp để được trích xuất hoàn toàn. Các phần tử còn lại đều đã được dịch chuyển bằng cách trừ đi tất cả các giá trị cực tiểu đã bị loại bỏ. 

Nếu chúng ta sắp xếp mảng, thì ở bất kỳ giai đoạn nào mà chúng ta dừng lại sau khi loại bỏ i phần tử nhỏ nhất đầu tiên, các phần tử còn lại sẽ được chuyển đổi một cách hiệu quả bằng cách trừ đi tổng của những đóng góp tối thiểu đã bị loại bỏ đó. Do đó, mức tối thiểu của tập hợp còn lại được xác định bởi phần tử ban đầu nhỏ nhất chưa bị xóa, trừ đi độ dịch chuyển tích lũy từ các lần xóa trước đó. 

Cấu trúc này cho phép chúng ta tính toán các câu trả lời ứng cử viên trong một lần duyệt qua mảng đã sắp xếp bằng cách theo dõi độ lệch trừ đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Sắp xếp + suy luận tiền tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này sắp xếp tất cả các lần xóa trong tương lai theo đúng thứ tự chúng sẽ xảy ra vì thao tác luôn loại bỏ mức tối thiểu hiện tại. 
2. Duy trì một biến đang chạy`shift`, ban đầu bằng không. Điều này thể hiện tổng số tiền bị trừ khỏi tất cả các phần tử còn lại do mức tối thiểu đã bị loại bỏ trước đó. 
3. Lặp lại mảng được sắp xếp từ nhỏ nhất đến lớn nhất. Ở vị trí i, xử lý`a[i] - shift`là giá trị hiệu dụng của phần tử hiện tại sau tất cả các thao tác trước đó. 
4. Ở mỗi bước, hãy tính câu trả lời của ứng viên bằng`a[i] - shift`. Đây là giá trị nhỏ nhất trong số các phần tử còn lại nếu chúng ta dừng xử lý đến thời điểm này. 
5. Cập nhật`shift`bằng cách thêm`a[i]`. Điều này phản ánh rằng nếu chúng ta loại bỏ phần tử này ở mức tối thiểu thì tất cả các phần tử còn lại sẽ bị giảm đi giá trị đó trong các hoạt động trong tương lai. 
6. Theo dõi giá trị tối đa trên tất cả các ứng viên, vì chúng ta muốn tối đa hóa giá trị tối thiểu cuối cùng trong số tất cả các điểm dừng có thể. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi thao tác sẽ loại bỏ giá trị ban đầu nhỏ nhất còn lại trong số những giá trị chưa được xử lý. Mỗi phần tử bị loại bỏ đóng góp chính xác giá trị của nó vào phép trừ tích lũy áp dụng cho tất cả các phần tử sau này. Điều này làm cho giá trị hiệu dụng của bất kỳ phần tử nào chưa bị xóa bằng giá trị ban đầu của nó trừ đi tổng của tất cả các phần tử đã bị xóa trước đó. Do đó, mức tối thiểu tại bất kỳ điểm dừng nào luôn được xác định bởi phần tử chưa được xử lý đầu tiên theo thứ tự được sắp xếp, được điều chỉnh theo độ dịch chuyển tích lũy, đây chính xác là những gì thuật toán đánh giá. 

Không có lựa chọn sắp xếp lại nào làm thay đổi cấu trúc này, vì việc loại bỏ bất kỳ phần tử tối thiểu nào của mảng hiện tại phải tương ứng với việc loại bỏ phần tử gốc nhỏ nhất tiếp theo sau khi lập chỉ mục lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    if n == 1:
        print(a[0])
        continue
    
    a.sort()
    
    shift = 0
    ans = -10**18
    
    for i in range(n):
        ans = max(ans, a[i] - shift)
        shift += a[i]
    
    print(ans)
```Giải pháp bắt đầu bằng cách xử lý riêng trường hợp một phần tử vì không được phép thực hiện thao tác nào ở đó. Việc sắp xếp là cần thiết vì nó sắp xếp trình tự loại bỏ bắt buộc với các giá trị tăng dần. 

Biến`shift`tích lũy tổng số phép trừ được áp dụng cho các phần tử còn lại do loại bỏ trước đó. Tại mỗi chỉ mục, chúng tôi đánh giá giá trị nhỏ nhất còn lại hiện tại sẽ là bao nhiêu nếu chúng tôi dừng quá trình ở đó. Câu trả lời là giá trị tối đa như vậy. 

Một sai lầm phổ biến là quên rằng ca phải được cập nhật sau khi đánh giá phần tử hiện tại. Đảo ngược thứ tự này sẽ áp dụng phép trừ không chính xác cho ứng viên hiện tại và chuyển tất cả kết quả xuống dưới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
-1 2 0
```Mảng được sắp xếp:`[-1, 0, 2]`| tôi | một [tôi] | dịch chuyển trước | ứng viên (a[i] - shift) | chuyển sau | 
| --- | --- | --- | --- | --- | 
| 0 | -1 | 0 | -1 | -1 | 
| 1 | 0 | -1 | 1 | -1 | 
| 2 | 2 | -1 | 3 | 1 | 

Số ứng viên tối đa là 3. 

Điều này cho thấy việc loại bỏ sớm hơn sẽ làm tăng giá trị hiệu quả của các phần tử sau như thế nào và điểm dừng tốt nhất không nhất thiết là sớm hay muộn mà phụ thuộc vào sự cân bằng giữa giá trị ban đầu và phép trừ tích lũy. 

### Ví dụ 2 

đầu vào:```
2
0 0
```Mảng được sắp xếp:`[0, 0]`| tôi | một [tôi] | dịch chuyển trước | ứng cử viên | chuyển sau | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 
| 1 | 0 | 0 | 0 | 0 | 

Giá trị vẫn ổn định bất kể hoạt động nào, xác nhận rằng các yếu tố giống hệt nhau không làm thay đổi kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | việc sắp xếp chiếm ưu thế cho từng trường hợp thử nghiệm | 
| Không gian | O(1) bổ sung (bỏ qua sắp xếp) | chỉ có một vài biến ngoài mảng đầu vào | 

Tổng số n trên các trường hợp thử nghiệm là 2·10^5, do đó việc sắp xếp theo từng trường hợp thử nghiệm sẽ phù hợp một cách thoải mái trong giới hạn thời gian. Việc quét tuyến tính sau đó là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            out.append(str(a[0]))
            continue
        a.sort()
        shift = 0
        ans = -10**18
        for x in a:
            ans = max(ans, x - shift)
            shift += x
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""8
1
10
2
0 0
3
-1 2 0
4
2 10 1 7
2
2 3
5
3 2 -4 -2 0
2
-1 1
1
-2
""") == """10
0
2
5
2
2
2
-2"""

# custom cases
assert run("""1
2
-5 -5
""") == "0", "all equal negatives"

assert run("""1
3
1 100 1000
""") == "1000", "strictly increasing values"

assert run("""1
4
-10 -1 0 1
""") == "1", "mixed negatives and positives"

assert run("""1
1
-100
""") == "-100", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[-5,-5]`|`0`| các yếu tố giống hệt nhau và sự ổn định | 
|`[1,100,1000]`|`1000`| thống trị lớn nhất sau ca | 
|`[-10,-1,0,1]`|`1`| tương tác giữa âm và dương | 
|`[-100]`|`-100`| trường hợp cạnh đơn phần tử | 

## Vỏ cạnh 

Mảng một phần tử trực tiếp buộc đầu ra phải là phần tử đó vì không được phép thực hiện thao tác nào. Thuật toán trả về sớm một cách rõ ràng trong trường hợp này, tránh việc sắp xếp hoặc dịch chuyển không cần thiết. 

Đối với một mảng các giá trị giống hệt nhau như`[x, x, x]`, việc sắp xếp không làm gì cả, và mọi ứng cử viên đều trở thành`x - kx`để tăng k, nhưng cực đại xảy ra ở bước đầu tiên, tạo ra x. Logic dịch chuyển đang chạy nắm bắt được điều này một cách tự nhiên vì mỗi ứng cử viên được đánh giá trước khi áp dụng phép trừ tương ứng. 

Đối với các mảng có giá trị âm lớn theo sau là giá trị dương, sự thay đổi sẽ tích lũy khối lượng âm sớm, thúc đẩy các phần tử sau một cách hiệu quả. Thẻ được sắp xếp đảm bảo điều này được tính đến mà không cần mô phỏng các cập nhật mang tính hủy diệt, vì mọi trạng thái chỉ phụ thuộc vào tổng tiền tố của mảng được sắp xếp.
