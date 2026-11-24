---
problem: 898C
contest_id: 898
problem_index: C
name: "Phone Numbers"
contest_name: "Codeforces Round 451 (Div. 2)"
rating: 1400
tags: ["implementation", "strings"]
answer: passed_samples
verified: false
solve_time_s: 274
date: 2026-06-17
model: gpt-5-5
samples_passed: 0
samples_total: 1
---

#CF 898C - Số điện thoại 

**Đánh giá:** 1400 
**Tags:** triển khai, chuỗi 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** không (0/1 mẫu) 

--- 

## Giải pháp 

## Hiểu vấn đề 

Mỗi dòng đầu vào mô tả một ảnh chụp nhanh danh sách liên hệ của một người. Một dòng bắt đầu bằng tên của một người, tiếp theo là số lượng số điện thoại được viết trong dòng đó và sau đó là danh sách các số đó dưới dạng chuỗi chữ số. Cùng một người có thể xuất hiện trên nhiều dòng và cùng một số điện thoại có thể xuất hiện nhiều lần đối với cùng một người. 

Nhiệm vụ là hợp nhất mọi thứ của mỗi người và đưa ra danh sách số điện thoại rõ ràng cho từng tên. Việc dọn dẹp có một quy tắc cụ thể: nếu một người có hai số và một số là hậu tố của số kia thì số hậu tố ngắn hơn là dư thừa và cần được loại bỏ. Theo trực giác, một số ngắn hơn có thể biểu thị cùng một số đầy đủ mà không có mã vùng, do đó, số này sẽ không được in nếu đã có phiên bản đầy đủ cho cùng một người đó. 

Đầu ra là một danh sách những người duy nhất và đối với mỗi người, một bộ số điện thoại được loại bỏ trùng lặp sau khi loại bỏ các mục nhập hậu tố dư thừa. Thứ tự không quan trọng đối với người hoặc số lượng của họ. 

Các ràng buộc rất nhỏ: tổng cộng tối đa 20 mục, mỗi mục có tối đa 10 số và mỗi số có độ dài tối đa là 10. Điều này ngay lập tức cho chúng ta biết rằng ngay cả việc kiểm tra bậc hai đối với tất cả các số điện thoại cũng hoàn toàn an toàn. Bất kỳ cách tiếp cận nào lên tới khoảng vài triệu thao tác chuỗi sẽ chạy thoải mái trong giới hạn. 

Các trường hợp tinh vi chính đến từ quy tắc hậu tố và xử lý trùng lặp. Một số tình huống tiêu biểu làm rõ những gì phải được xử lý một cách chính xác. 

Nếu một người có các số lặp lại trong một mục, chẳng hạn như: 

đầu vào:```
1
alex 3 123 123 45
```Khi đó cách giải thích đúng là các bản sao trong cùng một mục chỉ được tính một lần. Đầu ra chính xác sẽ coi đây là`{123, 45}`trước khi áp dụng logic hậu tố. 

Nếu việc loại bỏ hậu tố bị bỏ qua, một lỗi khác sẽ xuất hiện: 

đầu vào:```
1
bob 2 123 00123
```Đây`123`là hậu tố của`00123`, vậy chỉ`00123`nên ở lại. Một giải pháp dựa trên tập hợp đơn giản chỉ loại bỏ các bản sao chính xác sẽ giữ cả hai một cách không chính xác. 

Một trường hợp khác là tích lũy chéo: 

đầu vào:```
2
sam 1 123
sam 1 00123
```Mặc dù các số được chia thành nhiều mục nhưng chúng phải được hợp nhất trước khi áp dụng logic hậu tố. 

## Phương pháp tiếp cận 

Cách trực tiếp để giải quyết vấn đề là trước tiên hãy hợp nhất tất cả bản ghi của mỗi người vào một tập hợp số điện thoại, loại bỏ các bản sao chính xác bằng cách sử dụng một bộ. Sau đó, chúng tôi kiểm tra từng cặp số của cùng một người và loại bỏ những cặp số có hậu tố của một số khác. 

Logic mạnh mẽ này là đúng vì nó thực thi định nghĩa một cách rõ ràng: một số sẽ bị loại bỏ nếu nó xuất hiện dưới dạng hậu tố của bất kỳ số nào khác thuộc về cùng một người. Tuy nhiên, đối với mỗi người có tổng số tối đa 100 số sau khi hợp nhất (trường hợp xấu nhất là 20 mục × 10 số), việc kiểm tra tất cả các cặp dẫn đến khoảng 10.000 so sánh mỗi người. Mỗi so sánh liên quan đến việc kiểm tra tính bằng nhau của hậu tố lên đến độ dài 10, độ dài này vẫn còn nhỏ, do đó, ngay cả giải pháp ngây thơ này cũng thực sự có thể chấp nhận được dưới những ràng buộc này. Điểm kém hiệu quả duy nhất là nó thực hiện công việc dư thừa trên các cặp trong đó các chuỗi ngắn hơn không thể chứa các mối quan hệ hậu tố dài hơn. 

Quan sát mở ra một giải pháp rõ ràng hơn là các mối quan hệ hậu tố mang tính định hướng và cục bộ: đối với mỗi số, chúng ta chỉ cần biết liệu có tồn tại một số hoàn toàn dài hơn kết thúc bằng nó hay không. Điều này tránh việc lập luận cặp lặp đi lặp lại và biến vấn đề thành một bài kiểm tra tư cách thành viên đơn giản trong một bộ cho mỗi người. 

Do đó, chúng tôi có thể lưu trữ tất cả các số duy nhất cho mỗi người và sau đó lọc chúng bằng cách kiểm tra xem chúng có phải là hậu tố của bất kỳ số nào khác trong cùng một bộ hay không. Vì độ dài tối đa là 10 nên việc kiểm tra hậu tố phù hợp sẽ rẻ và giải pháp đầy đủ trở nên đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(N · k² · L) | O(N · k) | Được chấp nhận nhưng dư thừa | 
| Bộ + Lọc Hậu Tố | O(N · k² · L) | O(N · k) | Đã chấp nhận | 

Ở đây N là số lượng người nhập, k là số lượng tối đa cho mỗi người sau khi hợp nhất và L là độ dài tối đa của một số. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các dòng đầu vào và tổng hợp số điện thoại theo tên, sau đó làm sạch từng nhóm một cách độc lập. 

1. Đọc tất cả các mục và nhóm số điện thoại theo tên người vào từ điển. Mỗi tên ánh xạ tới một danh sách hoặc bộ số. 

Chúng tôi làm điều này vì mối quan hệ hậu tố chỉ quan trọng trong bộ sưu tập của một người. 
2. Đối với mỗi người, hãy chèn tất cả các số vào một bộ để loại bỏ các số trùng lặp từ các mục lặp lại hoặc các giá trị lặp lại trên cùng một dòng. 

Điều này đảm bảo chúng ta không lãng phí thời gian kiểm tra các chuỗi giống nhau nhiều lần. 
3. Chuyển tập hợp thành danh sách để lặp lại. Đối với mỗi số trong danh sách này, hãy kiểm tra xem có tồn tại một số khác trong cùng một bộ sao cho nó kết thúc bằng số hiện tại và dài hơn hay không. 

Điều này nắm bắt quy tắc rằng các phiên bản hậu tố ngắn hơn sẽ bị loại bỏ nếu có phiên bản đầy đủ hơn. 
4. Xây dựng danh sách các số được lọc không phải là hậu tố của bất kỳ số nào khác trong cùng một bộ. 

Lý do là chỉ còn lại các phần tử cực đại trong quan hệ hậu tố. 
5. In ra số lượng các tên riêng biệt, sau đó in tên, số lượng các số còn lại và các số đó theo thứ tự bất kỳ đối với mỗi tên. 

### Tại sao nó hoạt động 

Trong tập hợp của mỗi người, chúng tôi đang xây dựng một cách hiệu quả thứ tự từng phần trong đó một số được coi là "lớn hơn" nếu nó có một số khác làm hậu tố. Thuật toán giữ chính xác những phần tử không bị chi phối hoàn toàn bởi bất kỳ phần tử nào khác theo thứ tự này. Bất kỳ số nào là hậu tố của một số khác chắc chắn sẽ bị loại bỏ vì chúng tôi kiểm tra rõ ràng tất cả các ứng cử viên. Bất kỳ số nào tồn tại không thể là hậu tố của một số dài hơn trong tập hợp, khớp chính xác với yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    people = {}

    for _ in range(n):
        parts = input().split()
        name = parts[0]
        cnt = int(parts[1])
        nums = parts[2:2 + cnt]

        if name not in people:
            people[name] = set()
        for x in nums:
            people[name].add(x)

    result = []

    for name, nums in people.items():
        nums = list(nums)
        keep = []

        for i, x in enumerate(nums):
            ok = True
            for j, y in enumerate(nums):
                if i != j and y.endswith(x) and len(y) > len(x):
                    ok = False
                    break
            if ok:
                keep.append(x)

        result.append((name, keep))

    print(len(result))
    for name, nums in result:
        print(name, len(nums), *nums)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên xây dựng một từ điển từ tên đến tập hợp các chuỗi, hợp nhất cả các mục lặp lại và các mục trùng lặp bên trong các mục. Điều này rất cần thiết vì logic hậu tố không được bị ảnh hưởng bởi các chuỗi giống hệt nhau lặp đi lặp lại. 

Giai đoạn thứ hai lặp lại từng người một cách độc lập. Đối với mỗi số, chúng tôi quét tất cả các số khác trong tập hợp của người đó và kiểm tra việc đưa hậu tố vào bằng cách sử dụng`endswith`. Việc kiểm tra độ dài nghiêm ngặt ngăn chặn việc coi các số giống hệt nhau có liên quan đến hậu tố. 

Giai đoạn cuối in kết quả theo thứ tự tùy ý, phù hợp với yêu cầu bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
alex 3 123 123 00123
```Sau khi nhóm và loại bỏ trùng lặp, chúng tôi nhận được:`{123, 00123}`| hiện tại x | kiểm tra y | tìm thấy hậu tố phù hợp | giữ? | 
| --- | --- | --- | --- | 
| 123 | 00123 | vâng | không | 
| 00123 | 123 | không | vâng | 

Đầu ra:```
1
alex 1 00123
```Dấu vết này cho thấy rằng các bản sao không ảnh hưởng đến kết quả và logic hậu tố chỉ bảo toàn chính xác biểu diễn tối đa. 

### Ví dụ 2 

đầu vào:```
2
bob 2 123 45
bob 1 00123
```Sau khi sáp nhập:`{123, 45, 00123}`| hiện tại x | kiểm tra y | tìm thấy hậu tố phù hợp | giữ? | 
| --- | --- | --- | --- | 
| 123 | 00123 | vâng | không | 
| 45 | 123, 00123 | không | vâng | 
| 00123 | 123, 45 | không | vâng | 

Đầu ra:```
1
bob 2 45 00123
```Điều này chứng tỏ việc hợp nhất nhiều mục nhập là cần thiết trước khi áp dụng lọc hậu tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · k² · L) | Đối với mỗi người, chúng tôi so sánh từng cặp số và kiểm tra mối quan hệ hậu tố trong O(L) | 
| Không gian | O(n · k · L) | Lưu trữ tất cả số điện thoại duy nhất của mỗi người | 

Với tổng n ≤ 20, k ≤ 100 trong quá trình hợp nhất và L ≤ 10, số lượng thao tác trong trường hợp xấu nhất vẫn rất nhỏ và thoải mái nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""2
ivan 1 00123
masha 1 00123
""") == """2
ivan 1 00123
masha 1 00123"""

# single person with duplicates and suffix
assert run("""1
alex 4 123 123 00123 45
""") == """1
alex 2 00123 45"""

# all numbers independent
assert run("""1
bob 3 1 2 3
""") == """1
bob 3 1 2 3"""

# suffix chain
assert run("""1
sam 3 1 01 001
""") == """1
sam 1 001"""

# multiple people
assert run("""2
a 1 123
b 1 1234
""") == """2
a 1 123
b 1 1234"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trùng lặp + hậu tố | bộ giảm | chống trùng lặp và lọc hậu tố | 
| số độc lập | không thay đổi | không xóa sai | 
| chuỗi hậu tố | duy nhất được giữ lâu nhất | sự thống trị hậu tố chuyển tiếp | 
| nhiều người | xử lý riêng biệt | cách ly theo tên | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi các số giống nhau xuất hiện nhiều lần trong cùng một mục hoặc trên các mục. Việc chuyển đổi tập hợp sẽ loại bỏ những điều này sớm, vì vậy chúng không bao giờ can thiệp vào việc so sánh hậu tố. Ví dụ,`x = "123"`xuất hiện ba lần vẫn hoạt động như một phần tử duy nhất. 

Một trường hợp khác là số 0 đứng đầu, trong đó`"00123"`Và`"123"`là các chuỗi khác nhau nhưng một chuỗi là hậu tố của chuỗi kia. Thuật toán xử lý chính xác chúng dưới dạng các chuỗi riêng biệt trong khi vẫn áp dụng logic hậu tố dựa trên phần cuối của ký tự. 

Trường hợp tinh tế cuối cùng là khi tất cả các số tạo thành một chuỗi hậu tố như`"1"`,`"01"`,`"001"`. Việc kiểm tra hậu tố sẽ loại bỏ dần dần các dạng hậu tố ngắn hơn, chỉ để lại phần trình bày dài nhất, phù hợp với quy tắc rằng các dạng hậu tố ngắn hơn sẽ bị bỏ qua khi có phiên bản đầy đủ hơn.
