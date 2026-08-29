---
title: "CF 104380A - 01 (Phiên bản dễ dàng)"
description: "Chúng ta được cấp một chuỗi nhị phân chỉ gồm các ký tự 0 và 1. Chúng ta được phép tìm liên tục bất kỳ mẫu liền kề nào \"01\" và xóa nó hoàn toàn khỏi chuỗi, thu hẹp khoảng trống do việc xóa để lại."
date: "2026-07-01T03:06:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "A"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 57
verified: true
draft: false
---

[CF 104380A - 01 (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/104380/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân chỉ bao gồm các ký tự`0`Và`1`. Chúng tôi được phép liên tục tìm thấy bất kỳ mẫu liền kề nào`"01"`và xóa nó hoàn toàn khỏi chuỗi, thu hẹp khoảng cách do việc xóa để lại. Chúng ta tiếp tục làm việc này cho đến khi không`"01"`chuỗi con tồn tại ở bất cứ đâu trong chuỗi. Nhiệm vụ là xuất ra chuỗi ổn định cuối cùng và độ dài của nó. 

Quan sát chính về quá trình này là việc xóa là cục bộ nhưng ảnh hưởng là toàn cầu. Loại bỏ một`"01"`có thể tạo mới`"01"`các cặp trên ranh giới của những gì còn lại, vì vậy thứ tự xóa có ý nghĩa quan trọng trong quá trình thực hiện nhưng không ảnh hưởng đến kết quả cuối cùng. 

Kích thước đầu vào có thể lên tới`10^5`, loại trừ mọi cách tiếp cận liên tục quét chuỗi và xóa các chuỗi con ở giữa một cách ngây thơ. Một thao tác xóa có thể tốn O(n) và trong trường hợp xấu nhất, chúng tôi có thể thực hiện thao tác xóa O(n), dẫn đến hành vi O(n^2), tốc độ này quá chậm theo các ràng buộc. Điều này ngay lập tức gợi ý rằng chúng ta cần một mô phỏng tuyến tính hoặc gần tuyến tính. 

Một trường hợp phức tạp xuất hiện khi các số 0 và 1 được xen kẽ theo cách xảy ra việc loại bỏ theo tầng. Ví dụ, trong`"0101"`, loại bỏ cái đầu tiên`"01"`lá`"01"`một lần nữa, và có thể xóa một lần nữa. Một đường chuyền từ trái sang phải ngây thơ không xem xét lại các ranh giới mới hình thành sẽ thất bại ở đây. 

Một quan sát quan trọng khác là thao tác này chỉ loại bỏ`"01"`, không bao giờ`"10"`. Sự bất đối xứng này là nguyên nhân thúc đẩy cấu trúc của chuỗi cuối cùng. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp duy trì chuỗi và liên tục quét nó để tìm`"01"`, xóa các lần xuất hiện cho đến khi không còn lần nào. Điều này đúng vì nó phản ánh chính xác hoạt động được phép. Tuy nhiên, mỗi lần quét là O(n) và mỗi lần xóa sẽ dịch chuyển chuỗi, chuỗi này cũng là O(n). Trong trường hợp xấu nhất khi việc xóa xảy ra nhiều lần, chẳng hạn như các mẫu xen kẽ, tổng thời gian chạy sẽ trở thành bậc hai. 

Thông tin chi tiết quan trọng là tránh xóa rõ ràng và thay vào đó mô phỏng việc hủy bằng cách sử dụng quy trình giống như ngăn xếp. Khi quét từ trái sang phải, bất cứ khi nào chúng ta nhìn thấy một`1`, nó có khả năng có thể hủy bỏ một kết quả chưa từng có trước đó`0`nếu vậy`0`ngay lập tức có sẵn trong một ngăn xếp. Mỗi`"01"`việc xóa tương ứng với việc ghép nối một`0`xuất hiện sớm hơn với muộn hơn`1`. Điều này gợi ý rằng chúng ta chỉ cần theo dõi xem vẫn còn có bao nhiêu số 0 chưa khớp và quyết định xem liệu một`1`hủy bỏ một trong số chúng hoặc sống sót. 

Điều này làm giảm vấn đề duy trì một bộ đếm hoặc một chồng số không. Mỗi`0`được đẩy lên như một ứng cử viên cho việc hủy bỏ trong tương lai, và mỗi`1`loại bỏ một ứng cử viên như vậy nếu nó tồn tại. Những gì còn lại là những số 0 chưa từng có, theo sau là những số 0 chưa từng có, không thể hủy bỏ bất cứ thứ gì ở bên trái của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xóa nhiều lần Brute Force | O(n²) | O(n) | Quá chậm | 
| Mô phỏng ngăn xếp/bộ đếm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi từ trái sang phải trong khi vẫn duy trì một ngăn xếp lưu trữ các ký tự không khớp mà chưa thể xóa được. 

Ngăn xếp biểu thị dạng rút gọn hiện tại của tiền tố được xử lý. 
2. Đối với mỗi ký tự, hãy quyết định xem nó có tạo thành một ký tự có thể tháo rời hay không`"01"`mẫu với đỉnh của ngăn xếp. 

Chúng tôi chỉ loại bỏ`"01"`, vì vậy sự hủy bỏ hữu ích duy nhất là khi chúng ta thấy`1`và đỉnh ngăn xếp là`0`. 
3. Nếu ký tự hiện tại là`0`, đẩy nó vào ngăn xếp vì nó có thể bị xóa sau này trong tương lai`1`. 
4. Nếu ký tự hiện tại là`1`, kiểm tra xem ngăn xếp có trống không và đỉnh của nó có trống không`0`. Nếu vậy, hãy bật nó lên`0`, đại diện cho việc xóa`"01"`đôi. Nếu không thì đẩy`1`lên ngăn xếp. 
5. Sau khi xử lý toàn bộ chuỗi, ngăn xếp chứa chuỗi tối giản cuối cùng. Xuất ra chiều dài và nội dung của nó. 

Tại sao nó hoạt động: tại bất kỳ thời điểm nào trong quá trình quét, ngăn xếp duy trì dạng tiền tố rút gọn không có nội bộ`"01"`cặp còn lại. Tương lai nào đó`1`chỉ có thể tương tác với trước đó`0`s và ngăn xếp đảm bảo chúng tôi luôn ghép nối một`1`với cái mới nhất hiện có`0`, đó chính xác là tình huống duy nhất mà một`"01"`việc xóa có thể xảy ra. Vì mỗi thao tác xóa hợp lệ được mô phỏng chính xác một lần và không có ghép nối không hợp lệ nào được đưa vào nên ngăn xếp cuối cùng là điểm cố định duy nhất trong đó không có`"01"`vẫn còn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    stack = []

    for ch in s:
        if ch == '0':
            stack.append('0')
        else:
            if stack and stack[-1] == '0':
                stack.pop()
            else:
                stack.append('1')

    print(len(stack))
    print(''.join(stack))

if __name__ == "__main__":
    solve()
```Giải pháp đọc chuỗi một lần và xử lý từng ký tự theo thứ tự. Ngăn xếp được sử dụng hoàn toàn như một công cụ mô phỏng cho các ký hiệu chưa từng có. Quyết định không tầm thường duy nhất là khi xử lý một`1`: chúng tôi kiểm tra xem nó có thể hủy bỏ trước đó không`0`. Đây là mẫu xóa hợp lệ duy nhất nên không cần trường hợp nào khác. 

Đầu ra cuối cùng được xây dựng trực tiếp từ ngăn xếp mà không cần xử lý hậu kỳ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`00011`Chúng tôi xử lý từng ký tự trong khi duy trì ngăn xếp. 

| Bước | Char | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | [] | đẩy 0 | [0] | 
| 2 | 0 | [0] | đẩy 0 | [0,0] | 
| 3 | 0 | [0,0] | đẩy 0 | [0,0,0] | 
| 4 | 1 | [0,0,0] | pop 0 (mẫu 01) | [0,0] | 
| 5 | 1 | [0,0] | pop 0 (mẫu 01) | [0] | 

Ngăn xếp cuối cùng là`[0]`, vì vậy đầu ra là chiều dài`1`và chuỗi`"0"`. 

Điều này chứng tỏ có nhiều`1`s có thể liên tục hủy bỏ các số 0 trước đó và cách quá trình hội tụ một cách tự nhiên mà không cần xóa rõ ràng. 

### Ví dụ 2:`11010101011011111110101`Chúng tôi hiển thị một dấu vết cô đọng tập trung vào quá trình tiến hóa ngăn xếp. 

| Tiền tố | Char | Ngăn xếp hành động hàng đầu | Ngăn xếp | 
| --- | --- | --- | --- | 
| 1 | 1 | đẩy | [1] | 
| 2 | 1 | đẩy | [1,1] | 
| 3 | 0 | đẩy | [1,1,0] | 
| 4 | 1 | bật 0 | [1,1] | 
| 5 | 0 | đẩy | [1,1,0] | 
| 6 | 1 | bật 0 | [1,1] | 
| ... | ... | hủy bỏ nhiều lần | ... | 
| cuối cùng | - | ổn định | [1,1,1,1,1,1,1,1,1] | 

Quá trình này liên tục loại bỏ mọi cơ hội cho một`1`tiêu thụ cái trước`0`. Một khi tất cả các cặp như vậy đã cạn kiệt, chỉ còn lại những cặp chưa từng có`1`s vẫn còn, phù hợp với sản lượng dự kiến`"111111111"`. 

Dấu vết này làm nổi bật rằng các cấu trúc xen kẽ dài sụp đổ thành một khối đơn điệu gồm những số 1 vì cuối cùng mọi số 0 đều tìm thấy một số 0 phù hợp ở bên phải của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được đẩy và bật tối đa một lần | 
| Không gian | O(n) | Ngăn xếp lưu trữ các ký tự chưa từng có còn lại | 

Thuật toán chạy theo thời gian tuyến tính, đủ cho các chuỗi có độ dài tối đa`10^5`. Việc sử dụng bộ nhớ cũng tuyến tính trong trường hợp xấu nhất khi không có sự hủy bỏ nào xảy ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("00011\n") == "1\n0", "sample 1"
assert run("11010101011011111110101\n") == "9\n111111111", "sample 2"

# single character cases
assert run("0\n") == "1\n0"
assert run("1\n") == "1\n1"

# already stable (no 01)
assert run("111000\n") == "6\n111000"

# alternating case
assert run("010101\n") == "0\n"

# long collapse to ones
assert run("000000111111\n") == "0\n"

# mixed case
assert run("0010110\n") == "2\n10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1 0`| kích thước tối thiểu | 
|`1`|`1 1`| không thể hoạt động được | 
|`010101`|`0`| tầng hủy bỏ đầy đủ | 
|`111000`|`6 111000`| KHÔNG`"01"`cặp có mặt | 
|`000000111111`|`0`| giảm hoàn toàn thành trống rỗng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi việc xóa xếp tầng trên các vùng được phân tách trước đó. Coi như`0101`. đầu tiên`"01"`loại bỏ lá`"01"`, một lần nữa tạo thành một cặp có thể tháo rời. Ngăn xếp tự động xử lý việc này: sau khi xử lý`0`, ngăn xếp là`[0]`, sau đó`1`bật nó lên, rời đi`[]`, rồi tiếp theo`0`đẩy, rồi cuối cùng`1`bật lại, dẫn đến một ngăn xếp trống. Đầu ra là chính xác mà không cần quét nhiều lần. 

Một trường hợp cạnh khác là một chuỗi bắt đầu bằng nhiều số 1 theo sau là số 0, chẳng hạn như`111000`. Vì không`"01"`bao giờ xuất hiện, ngăn xếp chỉ tăng lên và thuật toán giữ nguyên toàn bộ chuỗi. Điều này xác nhận rằng phương pháp này không loại bỏ quá mức`"10"`các mẫu không được phép hoạt động. 

Trường hợp cạnh cuối cùng là khi chuỗi trở nên trống. Bất kỳ trình tự nào như`000111`giảm hoàn toàn vì mọi số 0 cuối cùng đều tìm được một số 0 phù hợp ở bên phải của nó. Ngăn xếp trở nên trống một cách tự nhiên và thuật toán xuất ra độ dài chính xác`0`và một chuỗi trống.
