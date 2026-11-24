---
problem: 900A
contest_id: 900
problem_index: A
name: "Find Extra One"
contest_name: "Codeforces Round 450 (Div. 2)"
rating: 800
tags: ["geometry", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 59
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
---

#CF 900A - Tìm Thêm Một 

**Đánh giá:** 800 
**Tags:** hình học, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 59s 
**Đã xác minh:** có (1/1 mẫu) 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên mặt phẳng và không có điểm nào nằm trên trục tung. Điều này có nghĩa là mọi điểm đều nằm hoàn toàn ở bên trái của trục y hoặc hoàn toàn ở bên phải trục y. Nhiệm vụ là kiểm tra xem liệu chúng ta có thể xóa chính xác một điểm để tất cả các điểm còn lại hoàn toàn nằm về một phía của trục y hay không, nghĩa là tất cả tọa độ x còn lại hoàn toàn dương hoặc hoàn toàn âm. 

Vì vậy, câu hỏi cốt lõi không phải là về hình học theo nghĩa sâu sắc mà là về việc liệu tập hợp có trở thành “một phía” sau khi loại bỏ một điểm ngoại lệ hay không. 

Kích thước đầu vào cho phép lên tới 100000 điểm, loại trừ mọi chiến lược bậc hai như thử mọi loại bỏ và kiểm tra phần còn lại trực tiếp trong O(n) mỗi lần. Điều đó sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, quá chậm so với giới hạn một giây. Chúng ta cần quét tuyến tính hoặc gần tuyến tính. 

Điều tinh tế quan trọng là câu trả lời chỉ phụ thuộc vào số lượng điểm ở phía bên trái (x < 0) và bao nhiêu điểm ở phía bên phải (x > 0). Tọa độ y không liên quan đến điều kiện. 

Một sai lầm phổ biến là nghĩ rằng cấu trúc không gian quan trọng hơn dấu của x. Ví dụ: ai đó có thể thử sắp xếp hoặc kiểm tra độ lồi, nhưng điều đó không ảnh hưởng đến việc liệu tất cả các điểm còn lại có nằm hoàn toàn trên một phía của trục y hay không. 

Các trường hợp cạnh xuất hiện khi cả hai bên được điền một cách cân bằng. Ví dụ: nếu có hai điểm ở mỗi bên, việc loại bỏ một điểm vẫn giữ nguyên cả hai bên, vì vậy điều đó là không thể. 

Một trường hợp tinh vi khác là khi tất cả các điểm đều đã ở một bên. Ví dụ: 

đầu vào:```
3
1 2
3 4
5 6
```Điều này đã hợp lệ rồi, vì việc loại bỏ bất kỳ một điểm nào sẽ giữ tất cả các điểm còn lại ở mặt tích cực. 

Một sai lầm ngây thơ là cho rằng chúng ta phải loại bỏ một điểm làm thay đổi phân bố cạnh, nhưng điều kiện đã được thỏa mãn mà không có bất kỳ cấu trúc đặc biệt nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử lần lượt loại bỏ từng điểm, sau đó quét tất cả các điểm còn lại để kiểm tra xem tất cả các giá trị x có cùng dấu hay không. Đối với mỗi trong số n ứng cử viên, chúng tôi sẽ thực hiện quét O(n) khác, dẫn đến độ phức tạp về thời gian O(n^2). Với n lên tới 100000, điều này trở thành khoảng 10^10 kiểm tra, điều này không khả thi. 

Quan sát quan trọng là điều kiện sau khi loại bỏ một điểm chỉ phụ thuộc vào số tọa độ x âm và dương. Sau khi loại bỏ, chúng tôi muốn có số âm bằng 0 hoặc số dương bằng 0. Điều đó có nghĩa là chúng ta được phép “sửa chữa” tối đa một vi phạm bằng cách loại bỏ một điểm. 

Vì vậy, chúng tôi tính toán có bao nhiêu điểm nằm ở mỗi bên. Nếu cả hai bên cùng có ít nhất hai điểm, việc loại bỏ một điểm không thể loại bỏ cả hai bên nên câu trả lời ngay là “Không”. Nếu ít nhất một bên không có điểm thì câu trả lời đã là “Có”. Nếu một bên có đúng một điểm, việc loại bỏ điểm đó sẽ giải quyết được vấn đề, vì vậy câu trả lời là “Có”. 

Điều này làm giảm vấn đề thành một nhiệm vụ đếm đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Đếm Tối Ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm và phân loại chúng theo dấu tọa độ x của chúng. Chúng tôi chỉ quan tâm xem x là dương hay âm, không quan tâm đến giá trị chính xác hoặc tọa độ y. 
2. Đếm xem có bao nhiêu điểm có x < 0 và bao nhiêu điểm có x > 0. 
3. Nếu tất cả các điểm đều ở một bên, hãy trả về “Có” vì việc loại bỏ bất kỳ điểm nào sẽ giữ nguyên thuộc tính. 
4. Nếu cả hai số lớn hơn 1, hãy trả về “Không” vì việc loại bỏ một điểm vẫn để lại ít nhất một điểm cho mỗi bên. 
5. Ngược lại, trả về “Có” vì việc loại bỏ điểm còn lại ở một bên sẽ loại bỏ hoàn toàn bên đó. 

### Tại sao nó hoạt động 

Cách duy nhất để thỏa mãn điều kiện sau khi loại bỏ là thu được tất cả các điểm còn lại có cùng dấu x. Việc loại bỏ một điểm có thể làm giảm số lượng của một bên nhiều nhất là một. Vì vậy, nếu ban đầu cả hai bên đều có ít nhất hai điểm thì không bên nào có thể bị loại hoàn toàn chỉ bằng một lần loại bỏ. Ngược lại, nếu ít nhất một cạnh có kích thước bằng 0 hoặc bằng 1, chúng ta luôn có thể loại bỏ một điểm để làm cho tập hợp còn lại có kích thước một phía. 

Điều này cung cấp một đặc tính đầy đủ của tất cả các cấu hình hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
neg = 0
pos = 0

for _ in range(n):
    x, y = map(int, input().split())
    if x < 0:
        neg += 1
    else:
        pos += 1

# already one-sided
if neg == 0 or pos == 0:
    print("Yes")
elif neg <= 1 or pos <= 1:
    print("Yes")
else:
    print("No")
```Mã trực tiếp thực hiện logic đếm. Điều kiện đầu tiên xử lý trường hợp tất cả các điểm đều nằm về một phía. Điều kiện thứ hai thể hiện tình huống một bên có thể bị loại bỏ hoàn toàn bằng cách loại bỏ một điểm duy nhất. 

Không cần sắp xếp hoặc tính toán hình học vì chỉ có dấu của x mới quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1
-1 -1
2 -1
```Chúng tôi theo dõi số lượng: 

| Bước | x | phủ định | tư thế | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | -1 | 1 | 1 | 
| 3 | 2 | 1 | 2 | 

Sau khi xử lý, neg = 1, pos = 2. 

Vì một bên có kích thước 1 nên việc loại bỏ điểm đó sẽ tạo ra tất cả các điểm còn lại ở một bên. 

Đầu ra là “Có”. 

Điều này xác nhận rằng một ngoại lệ duy nhất ở mặt tiêu cực có thể được loại bỏ. 

### Ví dụ 2 

đầu vào:```
4
-1 0
-2 1
1 3
2 5
```| Bước | x | phủ định | tư thế | 
| --- | --- | --- | --- | 
| 1 | -1 | 1 | 0 | 
| 2 | -2 | 2 | 0 | 
| 3 | 1 | 2 | 1 | 
| 4 | 2 | 2 | 2 | 

Bây giờ âm = 2 và pos = 2. 

Bỏ đi một điểm vẫn để lại ít nhất một điểm cho cả hai bên nên không thể biến mọi điểm thành một phía. 

Đầu ra là “Không”. 

Điều này thể hiện ngưỡng quan trọng trong đó cả hai bên đều quá lớn để có thể khắc phục bằng một lần xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Dấu hiệu x đếm một lượt | 
| Không gian | O(1) | Chỉ có hai quầy được lưu trữ | 

Giải pháp thực hiện một lần quét tối đa 100000 điểm, thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    data = inp.strip().split()
    it = iter(data)
    n = int(next(it))
    neg = 0
    pos = 0
    for _ in range(n):
        x = int(next(it))
        y = int(next(it))
        if x < 0:
            neg += 1
        else:
            pos += 1
    return "Yes\n" if (neg == 0 or pos == 0 or neg <= 1 or pos <= 1) else "No\n"

# provided sample 1
assert run("""3
1 1
-1 -1
2 -1
""") == "Yes\n"

# sample 2 (constructed)
assert run("""4
-1 0
-2 1
1 3
2 5
""") == "No\n"

# all positive
assert run("""3
1 1
2 2
3 3
""") == "Yes\n"

# all negative
assert run("""3
-1 1
-2 2
-3 3
""") == "Yes\n"

# borderline case: one negative
assert run("""3
-1 1
1 2
2 3
""") == "Yes\n"

# borderline case: two and two
assert run("""4
-1 0
-2 0
1 0
2 0
""") == "No\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tích cực | Có | một bên đã hợp lệ | 
| tất cả đều tiêu cực | Có | trường hợp đối xứng | 
| một tiêu cực, còn lại tích cực | Có | sửa lỗi loại bỏ duy nhất | 
| chia hai và hai | Không | không thể khắc phục bằng một lần xóa | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các điểm đã nằm về một phía. Ví dụ: nếu mọi x đều dương thì thuật toán đếm neg = 0 và pos = n. điều kiện`neg == 0 or pos == 0`kích hoạt ngay lập tức và đầu ra là “Có”. Điều này phù hợp với thực tế là việc loại bỏ bất kỳ điểm nào sẽ bảo toàn tính chất một phía. 

Một trường hợp cạnh khác là khi có chính xác một điểm nằm về một phía. Ví dụ: 

đầu vào:```
3
-1 2
1 3
2 4
```Số đếm trở thành neg = 1 và pos = 2. Điều kiện`neg <= 1`là đúng, vì vậy chúng ta có thể loại bỏ điểm âm duy nhất và chỉ để lại điểm dương. Thuật toán xác định chính xác điều này mà không cần mô phỏng việc loại bỏ. 

Trường hợp cạnh cuối cùng là khi cả hai bên có ít nhất hai điểm. Ví dụ:```
4
-1 0
-2 0
1 0
2 0
```Ở đây neg = 2 và pos = 2. Không có điều kiện chấp nhận nào được đáp ứng, vì vậy đầu ra là “Không”. Mọi thao tác xóa vẫn để lại ít nhất một điểm cho mỗi bên nên không thể đáp ứng yêu cầu chỉ bằng một lần xóa.
