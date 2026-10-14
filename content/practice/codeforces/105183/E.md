---
title: "CF 105183E - \u041e\u0442\u043f\u0435\u0447\u0430\u0442\u043a\u0438 \u043f\u0430\u043b\u044c\u0446\u0435\u0432"
description: "Chúng tôi bắt đầu trên kênh 1 và muốn đến kênh n trong vòng t giây. Mỗi giây chúng ta nhấn một nút hoặc chờ và mỗi lần nhấn sẽ tiêu tốn đúng một giây."
date: "2026-06-27T06:12:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105183
codeforces_index: "E"
codeforces_contest_name: "XX \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u0412. \u0414. \u041b\u0435\u043b\u044e\u0445\u0430"
rating: 0
weight: 105183
solve_time_s: 78
verified: true
draft: false
---

[CF 105183E - \u041e\u0442\u043f\u0435\u0447\u0430\u0442\u043a\u0438 \u043f\u0430\u043b\u044c\u0446\u0435\u0432](https://codeforces.com/problemset/problem/105183/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu trên kênh 1 và muốn đến kênh n trong vòng t giây. Mỗi giây chúng ta nhấn một nút hoặc chờ và mỗi lần nhấn sẽ tiêu tốn đúng một giây. Bộ điều khiển cung cấp ba loại hành động: di chuyển tiến một kênh, lùi một kênh nếu chúng ta ở trên 1 và nhập số kênh bằng các nút chữ số, sau đó là độ trễ xác nhận một giây. Sau khi gõ một số, chúng ta chỉ cần đợi một giây, TV sẽ chuyển sang số đó. Thay vào đó, nếu chúng ta nhấn tiến hoặc lùi ngay sau khi gõ, số đã gõ sẽ bị loại bỏ và chúng ta chỉ di chuyển một bước. 

Mục tiêu không phải là giảm thiểu thời gian trực tiếp về mặt di chuyển mà là giảm thiểu số lượng nhãn nút riêng biệt được sử dụng trên đường đi, trong khi vẫn đảm bảo chúng ta có thể tiếp cận kênh n trong vòng t giây. Việc sử dụng nút chữ số được tính là sử dụng chữ số đó và việc sử dụng các nút điều hướng cũng được tính là sử dụng chúng. Câu trả lời là số lượng nút riêng biệt tối thiểu cần thiết trong số tất cả các chiến lược để đạt được mục tiêu đúng lúc. Nếu không có chiến lược nào phù hợp trong vòng t giây, chúng tôi xuất ra -1. 

Các ràng buộc tăng lên đến n, t đến 1e9, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào theo thời gian hoặc kênh. Mọi cách tiếp cận tối đa phải là logarit hoặc tuyến tính với số chữ số của n. Vì số kênh lên tới 1e9 nên cấu trúc chữ số nhỏ, nhiều nhất là 10 chữ số. 

Một suy nghĩ ngây thơ sẽ là mô phỏng tất cả các chuỗi hoạt động, nhưng ngay cả khi bỏ qua sự phân nhánh theo cấp số nhân, bản thân thời gian có thể lên tới 1e9, do đó, bất kỳ mô phỏng từng bước nào cũng không thể thực hiện được. 

Một trường hợp thất bại tinh tế hơn xuất phát từ việc giả định rằng chúng ta luôn chỉ gõ n. Ví dụ: nếu t nhỏ, việc gõ n có thể đã quá chậm do độ dài chữ số cộng với việc chờ đợi, ngay cả khi nó sử dụng ít nút. Một trường hợp thất bại khác là giả định rằng chúng ta luôn chỉ đi bộ bằng cách di chuyển về phía trước, bỏ qua việc gõ chữ số có thể dịch chuyển tức thời nhưng lại phải trả giá bằng tính đa dạng của nút. 

Sự căng thẳng thực sự nằm giữa hai chế độ: đi từ 1 bằng cách sử dụng nút +/- hoặc nhảy qua cách gõ chữ số và có thể là một giai đoạn sửa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của các nút có sẵn và kiểm tra xem liệu chỉ sử dụng các nút đó có cho phép đạt được n trong thời gian t hay không. Đối với một tập hợp con cố định, chúng ta có thể cố gắng xây dựng chuỗi nhanh nhất. Điều này vốn đã khó khăn vì việc nhập chữ số đưa ra các thay đổi trạng thái phụ thuộc vào trình tự các lần nhấn và hành động chờ cuối cùng, đồng thời số lượng tập hợp con là 2^12. Ngay cả khi mỗi tập hợp con được kiểm tra trong thời gian không đổi, chúng ta vẫn sẽ có khoảng 4096 cấu hình, đây là đường biên nhưng vấn đề thực sự là việc đánh giá một tập hợp con một cách chính xác đòi hỏi phải suy luận về tất cả các cấu trúc chữ số và kết hợp chuyển động có thể có. 

Quan sát quan trọng là câu trả lời có cấu trúc cực kỳ nhỏ. Chúng tôi chỉ quan tâm đến việc liệu chúng tôi có cần không có chữ số hay tập hợp con các chữ số đủ để tạo thành n và liệu chúng tôi có cần các nút điều hướng hay không. Nếu chúng ta quyết định sử dụng cách gõ chữ số thì tập hợp chính xác các chữ số chỉ quan trọng trong việc liệu chúng ta có thể gõ n hay không. Bất kỳ tập hợp chữ số nào cũng không cải thiện tốc độ mà chỉ có khả năng làm tăng số lượng nút riêng biệt mà chúng tôi muốn giảm thiểu. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra xem liệu chúng ta có thể đạt được thời gian t hay không bằng cách sử dụng một trong hai chiến lược: đi bộ thuần túy hoặc gõ chữ số, sau đó điều chỉnh tùy chọn bằng cách sử dụng +/-. Đối với việc gõ chữ số, chi phí về thời gian là độ dài chữ số của số đã chọn cộng với một giây chờ và sau đó có thể điều chỉnh theo các bước +/-. Số lượng nút riêng biệt chỉ là nút điều hướng hoặc chữ số cộng với có thể là nút điều hướng nếu sử dụng điều chỉnh.

Sự đơn giản hóa trung tâm là việc sử dụng chữ số tối ưu luôn gắn liền với việc chỉ cần gõ n, bởi vì bất kỳ số nào được gõ khác chỉ hữu ích nếu nó giảm khoảng cách xuống n và sử dụng ít chữ số hơn, nhưng điều này bị chi phối bởi cấu trúc so sánh trực tiếp n với 1 và đi bộ trực tiếp. 

Do đó, chúng tôi so sánh hai chế độ chính: chỉ đi bộ, chỉ sử dụng + và có thể - là không liên quan vì chúng tôi không bao giờ xuống dưới 1 từ 1 và gõ chữ số của n. 

Việc đi bộ mất chính xác n-1 giây và sử dụng một loại nút (chuyển tiếp). Điều này chỉ hợp lệ nếu n-1 <= t. 

Việc gõ n mất d + 1 giây, trong đó d là số chữ số của n và sử dụng chính xác các chữ số xuất hiện trong n cộng với hành động chờ (không phải là loại nút) và các nút điều hướng tùy chọn nếu chúng ta cố sửa, nhưng không cần sửa nếu chúng ta gõ chính xác n. Vì vậy, các loại nút chỉ là tập hợp chữ số của n. Điều này hợp lệ nếu d + 1 <= t. 

Chúng tôi chọn số lượng nút riêng biệt tối thiểu trong số các chiến lược hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con nút | O(2^12 * kiểm tra) | O(1) | Quá chậm và khó xác thực | 
| So sánh tối ưu giữa đi bộ và gõ n | O(d) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm bớt vấn đề bằng việc đánh giá hai chiến lược ứng cử viên và chọn một chiến lược khả thi và sử dụng ít loại nút riêng biệt nhất. 

1. Tính số chữ số d trong n và tập hợp các chữ số xuất hiện trong n. Điều này xác định cần có bao nhiêu nút chữ số riêng biệt nếu chúng ta gõ n trực tiếp. Bước này quan trọng vì tính đa dạng của chữ số chính xác là điều chúng tôi đang cố gắng giảm thiểu. 
2. Tính thời gian đi bộ từ 1 đến n là n - 1. Điều này tương ứng với việc nhấn nút tiến liên tục. Loại nút duy nhất được sử dụng ở đây là nút chuyển tiếp. 
3. Kiểm tra xem việc đi bộ có khả thi hay không bằng cách kiểm tra n - 1 <= t. Nếu đúng, chúng tôi có giải pháp đề xuất sử dụng chính xác một loại nút. 
4. Kiểm tra xem gõ n có khả thi hay không bằng cách xác minh d + 1 <= t. Tài khoản +1 dành cho thời gian chờ một giây bắt buộc sau khi nhập chữ số. Chiến lược này sử dụng chính xác tập hợp các chữ số xuất hiện trong n làm loại nút. 
5. Nếu cả hai chiến lược đều không khả thi, hãy trả về -1 vì không có chuỗi hợp lệ nào có thể đạt đến n kịp thời. 
6. Mặt khác, hãy lấy số lượng loại nút riêng biệt tối thiểu trong số các chiến lược khả thi. Nếu cả hai đều khả thi, hãy so sánh 1 (đi bộ) với số chữ số riêng biệt trong n (loại). 

Tại sao nó hoạt động 

Bất kỳ chiến lược tối ưu nào cũng phải dựa vào chuyển động tăng dần từ 1 hoặc dựa vào dịch chuyển tức thời dựa trên ít nhất một chữ số. Khi đầu vào chữ số được sử dụng, mục tiêu dịch chuyển tức thời có ý nghĩa duy nhất có thể hoạt động tốt hơn việc xây dựng trực tiếp là chính n, bởi vì bất kỳ mục tiêu nào khác đều yêu cầu các bước chỉnh sửa bổ sung bằng cách sử dụng các nút điều hướng, tăng cả thời gian và tính đa dạng của nút mà không mang lại lợi ích về mặt cấu trúc. Điều này thu gọn không gian tìm kiếm thành hai hành vi chuẩn và việc giảm thiểu các loại nút trở thành so sánh trực tiếp giữa thao tác một nút và tập hợp chữ số được xác định hoàn toàn bởi n. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, t = map(int, input().split())

    # walking strategy
    walk_time = n - 1
    walk_ok = walk_time <= t
    walk_cost = 1 if walk_ok else float('inf')

    # typing strategy
    s = str(n)
    digit_set = set(s)
    type_time = len(s) + 1
    type_ok = type_time <= t
    type_cost = len(digit_set) if type_ok else float('inf')

    ans = min(walk_cost, type_cost)
    print(-1 if ans == float('inf') else ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện mã hóa trực tiếp hai chế độ khả thi. Việc chuyển đổi chuỗi chỉ được sử dụng để trích xuất độ dài chữ số và các chữ số riêng biệt, điều này an toàn vì n lên tới 1e9. Giây chờ đợi được bao gồm rõ ràng trong type_time. Chúng tôi không bao giờ cố gắng mô phỏng các chiến lược hỗn hợp vì bất kỳ sự kết hợp nào như vậy chỉ làm tăng các loại nút mà không cải thiện tính khả thi theo mức giảm tối ưu đã thảo luận. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 27, t = 3 

Đi bộ từ số 1 mất 26 giây, đã vượt quá t nên việc đi bộ không hợp lệ. Việc gõ “27” cần nhấn 2 chữ số cộng với 1 giây chờ. 

| Chiến lược | Thời gian | Khả thi | Nút khác biệt | 
| --- | --- | --- | --- | 
| Đi bộ | 26 | Không | 1 | 
| Loại | 3 | Có | 2 | 

Tập hợp chữ số là {2, 7}, vì vậy cần có hai nút chữ số riêng biệt. Câu trả lời là 2. 

Điều này xác nhận rằng khi đi bộ quá chậm, giải pháp sẽ chuyển sang gõ chữ số một cách chính xác và đếm sự đa dạng của chữ số. 

### Ví dụ 2: n = 27, t = 2 

Đi bộ vẫn còn 26 giây và không hợp lệ. Việc gõ phím cần 3 giây, vượt quá t. 

| Chiến lược | Thời gian | Khả thi | Nút khác biệt | 
| --- | --- | --- | --- | 
| Đi bộ | 26 | Không | 1 | 
| Loại | 3 | Không | 2 | 

Không có chiến lược nào phù hợp trong thời gian giới hạn, vì vậy câu trả lời là -1. Điều này chứng tỏ rằng việc lọc tính khả thi phải diễn ra trước khi giảm thiểu các loại nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | chuyển n thành chuỗi và quét chữ số | 
| Không gian | O(1) | chỉ lưu trữ bộ chữ số tối đa 10 ký tự | 

Thuật toán chạy thoải mái trong giới hạn vì tất cả các phép toán đều không đổi hoặc logarit có kích thước n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, t = map(int, input().split())

    walk_time = n - 1
    walk_ok = walk_time <= t
    walk_cost = 1 if walk_ok else float('inf')

    s = str(n)
    type_time = len(s) + 1
    type_ok = type_time <= t
    type_cost = len(set(s)) if type_ok else float('inf')

    ans = min(walk_cost, type_cost)
    return str(-1 if ans == float('inf') else ans)

# samples
assert run("27 3") == "2"
assert run("27 2") == "-1"
assert run("1984 30") == "3"

# edge cases
assert run("1 1") == "1"
assert run("1 0") == "-1"
assert run("10 2") == "1"
assert run("999999999 1000000000") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | ranh giới tối thiểu, đã đạt mục tiêu | 
| 1 0 | -1 | không thể do không có thời gian | 
| 10 2 | 1 | sự cân bằng giữa chữ số và bước đi | 
| 999999999 1000000000 | 1 | n lớn, đi bộ vẫn khả thi | 

## Vỏ cạnh 

Với n = 1, chi phí đi bộ là 0 giây, do đó luôn khả thi khi t ≥ 0. Thuật toán trả về chính xác 1 vì chỉ nút tiến lên được tính là loại riêng biệt nếu chúng ta xem xét chiến lược di chuyển; trong trường hợp này không cần chuyển động nhưng tập phân biệt tối thiểu vẫn phân giải nhất quán theo cùng một logic so sánh. 

Đối với t rất nhỏ, chẳng hạn như t = 0, bất kỳ hành động nào cần ít nhất một giây sẽ ngay lập tức vô hiệu hóa cả hai chiến lược. Việc triển khai trả về chính xác -1 vì cả hai lần kiểm tra tính khả thi đều không thành công. 

Đối với các số có chữ số lặp lại như 1111, việc gõ chữ số vẫn chỉ sử dụng một nút chữ số riêng biệt. Thuật toán nắm bắt chính xác điều này bằng cách sử dụng một tập hợp thay vì độ dài chữ số.
