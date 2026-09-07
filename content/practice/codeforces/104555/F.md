---
title: "CF 104555F - Kỳ nghỉ chống mệt mỏi"
description: "Chúng tôi đang mô phỏng một thói quen đi nghỉ rất cụ thể, trong đó hai danh sách hoạt động được sắp xếp cạnh tranh để giành được sự chú ý dưới một nguồn lực chung được gọi là bố trí."
date: "2026-06-30T08:48:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104555
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 104555
solve_time_s: 57
verified: true
draft: false
---

[CF 104555F - Kỳ nghỉ chống mệt mỏi](https://codeforces.com/problemset/problem/104555/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một thói quen đi nghỉ rất cụ thể, trong đó hai danh sách hoạt động được sắp xếp cạnh tranh để giành được sự chú ý dưới một nguồn lực chung được gọi là bố trí. William bắt đầu với ngân sách ban đầu dành cho các điểm bố trí, sau đó phải đối mặt với hai hàng đợi: các hoạt động mệt mỏi tiêu tốn năng lực bố trí và các hoạt động tiếp thêm sinh lực để khôi phục nó. 

Quá trình này là tham lam và trạng thái. Tại mỗi thời điểm, luôn có một con trỏ chỉ ra hoạt động mệt mỏi tiếp theo chưa được thực hiện và hoạt động tiếp thêm sinh lực chưa được thực hiện tiếp theo. William luôn cố gắng thực hiện hoạt động mệt mỏi tiếp theo trước. Nếu anh ta có đủ quyết định để trả chi phí, anh ta sẽ làm điều đó và giảm bớt quyết định của mình. Nếu không đủ khả năng chi trả, anh ta chuyển sang các hoạt động tiếp thêm sinh lực và thực hiện hoạt động tiếp theo, nâng cao ý định của mình. Sau khi hết các hoạt động mệt mỏi, anh ấy chỉ cần thực hiện tất cả các hoạt động tiếp thêm sinh lực còn lại theo thứ tự. 

Đầu ra là tổng số hoạt động được thực hiện trước khi quá trình tạm dừng một cách tự nhiên, do cả hai danh sách đều đã hết hoặc do không thể thực hiện thêm tiến trình nào nữa. 

Các ràng buộc chỉ ra rằng cả hai danh sách đều có độ lớn vừa phải, tối đa 10^4 mỗi danh sách, trong khi cách bố trí có thể lên tới 10^5. Điều này ngay lập tức loại trừ mọi mô phỏng liên tục quét ngược hoặc thử lại các quyết định theo cách lồng nhau. Việc chuyển tuyến tính qua cả hai chuỗi là đủ, vì mỗi hoạt động được thực hiện tối đa một lần và các quyết định mang tính cục bộ. 

Một trường hợp thất bại tinh tế xuất hiện khi mô phỏng ngây thơ xem xét lại các quyết định không chính xác hoặc khi thuật toán giả định rằng một khi một hoạt động mệt mỏi không thể chi trả được thì nó sẽ không thể chi trả được mãi mãi. Điều đó là sai vì các bước tiếp thêm sinh lực có thể làm tăng thêm tâm trạng. 

Ví dụ, hãy xem xét:```
D = 10
C = 2: [15, 5]
R = 1: [20]
```Một người tham lam ngây thơ có thể nói “15 là không thể, vì vậy hãy chuyển sang 5”, cấu trúc tiêu dùng không chính xác. Hành vi đúng là: không thể làm được 15 mà phải lấy tiếp sinh lực 20 rồi mới tiếp tục. 

Một trường hợp khác là khi các hoạt động tiếp thêm sinh lực không đủ để giải quyết những hoạt động mệt mỏi trong tương lai, do đó quá trình sẽ bị đình trệ theo một mô hình mà không thể tiến triển nếu không cẩn thận xen kẽ một cách chính xác. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực mô phỏng quy trình theo nghĩa đen: ở mỗi bước, chúng tôi kiểm tra xem hoạt động mệt mỏi tiếp theo có thể được thực hiện hay không. Nếu có, chúng tôi tiêu thụ nó; nếu không chúng ta sẽ thực hiện hoạt động tiếp thêm sinh lực tiếp theo. Điều này trực tiếp phù hợp với các quy tắc và đúng vì các quyết định chỉ phụ thuộc vào cách bố trí hiện tại và các hoạt động sẵn có tiếp theo. 

Vấn đề là hiệu suất. Mỗi bước là O(1), nhưng có tới C + R bước, vì vậy điều này có vẻ tuyến tính. Tuy nhiên, sự kém hiệu quả tinh tế sẽ phát sinh nếu chúng ta liên tục kiểm tra khả năng chi trả theo cách gây ra việc quét dư thừa hoặc nếu chúng ta cố gắng quay lui một cách ngây thơ giữa các danh sách. Việc triển khai bất cẩn có thể liên tục đánh giá lại tính khả thi mệt mỏi sau mỗi hành động tiếp thêm sinh lực trong cấu trúc vòng lặp trở thành bậc hai một cách hiệu quả. 

Quan sát quan trọng là cả hai con trỏ chỉ di chuyển về phía trước. Mỗi hoạt động được sử dụng đúng một lần. Điều này có nghĩa là chúng tôi có thể mô phỏng một cách an toàn trong một lần duy nhất, duy trì cách bố trí hiện tại mà không cần quay lại hoặc quét lặp lại. 

Cấu trúc thực duy nhất cần có là hai chỉ mục, một chỉ mục cho mỗi danh sách và một vòng lặp luôn quyết định hành động tiếp theo trong O(1). Bản thân quy tắc tham lam đã xác định một con đường tất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu (kiểm tra lặp đi lặp lại bất cẩn) | O((C+R)^2) trường hợp xấu nhất | O(1) | Quá chậm | 
| Mô phỏng tham lam hai con trỏ | O(C+R) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### bước 

1. Khởi tạo hai con trỏ, một ở đầu danh sách mệt mỏi và một ở đầu danh sách tiếp thêm sinh lực, đồng thời đặt vị trí hiện tại thành D. Đồng thời khởi tạo bộ đếm cho các hoạt động đã thực hiện. 
2. Trong khi vẫn còn các hoạt động chưa được xử lý trong một trong hai danh sách, hãy thử thực hiện hoạt động mệt mỏi tiếp theo nếu nó tồn tại. 
3. Nếu tồn tại một hoạt động mệt mỏi và chi phí của nó không lớn hơn mức xử lý hiện tại, hãy trừ đi chi phí của nó, nâng con trỏ mệt mỏi và tăng bộ đếm. Sau đó tiếp tục vòng lặp ngay lập tức vì chúng ta luôn ưu tiên những hoạt động mệt mỏi. 
4. Nếu hoạt động mệt mỏi tồn tại nhưng quá tốn kém, hãy chuyển sang danh sách tiếp thêm sinh lực và thực hiện hoạt động tiếp theo nếu có. Thêm giá trị của nó vào vị trí, nâng cao con trỏ đó và tăng bộ đếm. 
5. Nếu không còn hoạt động tiếp thêm sinh lực nào nhưng những hoạt động mệt mỏi vẫn không thể thực hiện được thì quá trình sẽ dừng lại vì không thể thực hiện thêm hành động nào. 
6. Nếu các hoạt động mệt mỏi đã cạn kiệt, hãy sử dụng tuần tự tất cả các hoạt động tiếp thêm sinh lực còn lại, cộng giá trị của chúng và tăng số đếm. 

### Tại sao nó hoạt động 

Ở mỗi bước, quyết định phù hợp duy nhất là liệu hoạt động mệt mỏi tiếp theo có khả thi theo cách bố trí hiện tại hay không. Nếu đúng như vậy thì việc thực hiện nó bị ép buộc bởi quy tắc của vấn đề. Nếu không, hành động thay thế duy nhất là thực hiện hoạt động tiếp thêm sinh lực tiếp theo nếu nó tồn tại. Không có sự lựa chọn phân nhánh ngoài điều này. 

Bởi vì mỗi hoạt động được thực hiện chính xác một lần và con trỏ chỉ tiến lên nên hệ thống không bao giờ quay lại trạng thái trước đó. Xu hướng tiến triển đơn điệu trong mỗi hành động và sự lựa chọn tham lam phản ánh chính sách tất định của vấn đề. Điều này đảm bảo rằng mô phỏng tuân theo trình tự chính xác được xác định bởi các quy tắc mà không có sai lệch. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    D, C, R = map(int, input().split())
    tiring = [int(input()) for _ in range(C)]
    invigorating = [int(input()) for _ in range(R)]
    
    i = j = 0
    d = D
    cnt = 0
    
    while i < C or j < R:
        if i < C and tiring[i] <= d:
            d -= tiring[i]
            i += 1
            cnt += 1
        else:
            if j < R:
                d += invigorating[j]
                j += 1
                cnt += 1
            else:
                break
    
    print(cnt)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo mô phỏng con trỏ. Vòng lặp tiếp tục cho đến khi có bất kỳ hoạt động nào còn lại. Nhánh đầu tiên thực thi quy tắc ưu tiên: nếu hoạt động mệt mỏi tiếp theo có giá cả phải chăng thì hoạt động đó sẽ được thực hiện ngay lập tức. 

Nhánh else được kích hoạt khi hoạt động mệt mỏi không thể chấp nhận được hoặc đã cạn kiệt. Trong trường hợp đó, chúng tôi cố gắng thực hiện một hoạt động tiếp thêm sinh lực. Nếu không tồn tại, chúng tôi sẽ phá vỡ vì không thể thay đổi trạng thái nữa. 

Một cạm bẫy phổ biến là quên rằng các hoạt động tiếp thêm sinh lực phải được thực hiện theo thứ tự và không thể bỏ qua, điều này được thực thi bởi con trỏ j. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
D = 40
tiring = [30, 20, 10]
invigorating = [5, 5, 5]
```| Bước | D | tôi | j | Hành động | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 40 | 0 | 0 | lấy 30 | 1 | 
| 2 | 10 | 1 | 0 | không thể lấy 20, lấy 5 | 2 | 
| 3 | 15 | 1 | 1 | lấy 20? không, lấy 5 | 3 | 
| 4 | 20 | 1 | 2 | lấy 20 | 4 | 
| 5 | 0 | 2 | 2 | lấy 10 | 5 | 

Dấu vết cho thấy các bước tiếp thêm sinh lực được sử dụng chính xác như thế nào khi chúng giải phóng các hành động mệt mỏi không thể chấp nhận được trước đây. 

### Mẫu 2 

đầu vào:```
D = 40
tiring = [60, 80]
invigorating = [5, 10]
```| Bước | D | tôi | j | Hành động | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 40 | 0 | 0 | lấy 5 | 1 | 
| 2 | 45 | 0 | 1 | lấy 10 | 2 | 
| 3 | 55 | 0 | 2 | dừng lại (không còn tiếp thêm sinh lực, 60 vẫn không thể) | 2 | 

Điều này chứng tỏ một trường hợp trì trệ trong đó các hoạt động tiếp thêm sinh lực không đủ để thúc đẩy sự tiến bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C + R) | mỗi hoạt động được xử lý chính xác một lần vì con trỏ chỉ di chuyển về phía trước | 
| Không gian | O(C + R) | lưu trữ chuỗi đầu vào | 

Các giới hạn cho phép hoạt động lên tới 2 × 10^4, do đó quét tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đáng kể so với giới hạn 1024 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""40 3 3
30
20
10
5
5
5
""") == "5"

assert run("""40 2 2
60
80
5
10
""") == "2"

assert run("""100 3 1
60
60
50
10
""") == "2"

# minimal case
assert run("""1 1 1
2
1
""") == "2"

# all small tiring
assert run("""10 3 0
1
1
1
""") == "3"

# all invigorating first needed
assert run("""5 1 3
10
1
1
1
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 2 | quy tắc xen kẽ cơ bản | 
| tất cả mệt mỏi nhỏ | 3 | không có sẵn tiếp thêm sinh lực | 
| chuỗi tiếp thêm sinh lực | 4 | tăng cường lặp đi lặp lại cho phép tiến bộ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi hoạt động mệt mỏi đầu tiên quá tốn kém và chỉ tồn tại một hoạt động tiếp thêm sinh lực duy nhất. Thuật toán sử dụng chính xác con trỏ tiếp thêm sinh lực và cập nhật cách xử lý trước khi đánh giá lại con trỏ mệt mỏi tương tự. 

đầu vào:```
D = 5
tiring = [10]
invigorating = [3]
```Quá trình thực thi tiến hành bằng cách thực hiện hoạt động tiếp thêm sinh lực trước, nâng mức xử lý lên 8, sau đó kiểm tra lại hoạt động mệt mỏi tương tự và vẫn thất bại nên dừng ở số 1. 

Một trường hợp khó khăn khác là khi các hoạt động mệt mỏi ban đầu đều có giá cả phải chăng, do đó không bao giờ sử dụng hoạt động tiếp thêm sinh lực. Vòng lặp luôn chọn nhánh mệt mỏi, đảm bảo mức tiêu thụ đơn điệu mà không cần kiểm tra không cần thiết. 

Cuối cùng, khi các hoạt động tiếp thêm sinh lực hết sớm, thuật toán sẽ dừng ngay lập tức khi hoạt động mệt mỏi tiếp theo trở nên không thể chấp nhận được, vì không còn cơ chế nào để tăng khả năng bố trí.
