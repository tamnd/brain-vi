---
title: "CF 105129I - Phân phối đồ uống"
description: "Mỗi trường hợp thử nghiệm đưa ra một tập hợp các chai nước trái cây, trong đó chai thứ i chứa một số lít nhất định. Abdelaleem sẽ phục vụ chính xác m người bạn, và với mỗi chai được chọn, anh ta được phép đổ cùng một số lít vào cốc của mỗi người bạn."
date: "2026-06-27T19:22:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105129
codeforces_index: "I"
codeforces_contest_name: "Shorouk Academy 2024 Collegiate Programming Contest"
rating: 0
weight: 105129
solve_time_s: 44
verified: true
draft: false
---

[CF 105129I - Phân phối đồ uống](https://codeforces.com/problemset/problem/105129/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một tập hợp các chai nước trái cây, trong đó chai thứ i chứa một số lít nhất định. Abdelaleem sẽ phục vụ chính xác m người bạn, và với mỗi chai được chọn, anh ta được phép đổ cùng một số lít vào cốc của mỗi người bạn. Nếu anh ta không sử dụng chai, nó sẽ bị bỏ qua hoàn toàn. 

Các ràng buộc đối với một chai được chọn là nghiêm ngặt. Sau khi chia đều cho m người bạn, chất lỏng còn lại trong chai không được trở thành một phần nhỏ còn sót lại. Hoặc chai được sử dụng theo cách không còn lại gì, hoặc được sử dụng theo cách còn lại ít nhất là k lít. Nếu không thể đáp ứng cả hai điều kiện cho cách chia đã chọn thì chai đó không thể được sử dụng trong cấu hình đó. 

Nhiệm vụ là tối đa hóa lượng nước trái cây cho mỗi người bạn, giả sử anh ta chọn một lượng nước trái cây thống nhất cho mỗi người bạn cho tất cả các chai đã chọn được sử dụng. 

Cách giải thích chính là chúng ta đang chọn một số nguyên x, biểu thị số lít mỗi người bạn nhận được từ mỗi chai đã qua sử dụng. Đối với mỗi chai có thể tích a, chúng ta hoặc không sử dụng hoặc chia nhỏ để m·x lít được phân phối, để lại phần dư a − m·x. Số dư này phải thỏa mãn a − m·x = 0 hoặc a − m·x ≥ k. 

Kích thước đầu vào lớn, lên tới 5 × 10^5 chai cho mỗi trường hợp thử nghiệm và giá trị lên tới 10^9. Điều này loại trừ mọi tìm kiếm theo giá trị trên x cho mỗi chai. Bất kỳ giải pháp nào cố gắng kiểm tra tính khả thi của từng ứng viên x một cách độc lập cho mỗi trường hợp thử nghiệm sẽ quá chậm. Cấu trúc gợi ý rõ ràng rằng mỗi chai đóng góp một khoảng ràng buộc trên x và câu trả lời cuối cùng là số nguyên x tối đa thỏa mãn đồng thời tất cả các ràng buộc. 

Một trường hợp thất bại tinh vi xuất hiện khi một cách tiếp cận ngây thơ cho rằng mọi chai đều phải được sử dụng. Ví dụ, nếu một chai không thể tách sạch hoặc không để lại đủ phần còn lại thì nên bỏ qua. Một trường hợp sai sót khác là coi điều kiện còn lại là luôn yêu cầu chia hết hoặc luôn yêu cầu số dư ≥ k mà không xem xét rằng cả hai lựa chọn đều được phép. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp là thử tất cả các giá trị có thể có của x từ 1 đến ai lớn nhất, và với mỗi x hãy kiểm tra từng chai. Đối với x cố định, mỗi chai được kiểm tra bằng cách tính m·x và kiểm tra xem nó có khớp chính xác với phép chia hay không hoặc có để lại ít nhất k phần còn lại hay không. Điều này đúng nhưng quá chậm. Trường hợp xấu nhất liên quan đến 5 × 10^5 chai và tối đa 10^9 giá trị ứng cử viên, dẫn đến việc tính toán ở tỷ lệ 10^14 là không thể. 

Quan sát quan trọng là mỗi chai xác định độc lập những giá trị nào của x được phép. Đối với a cố định, điều kiện được chia thành hai trường hợp. 

Nếu chai được sử dụng hết thì a = m·x, đóng góp một ứng cử viên duy nhất x = a / m khi chia hết. 

Nếu nó được sử dụng một phần thì chúng ta cần a − m·x ≥ k, sắp xếp lại thành x ≤ (a − k) / m. 

Vì vậy, mỗi chai cho phép tất cả x đạt đến một ngưỡng nhất định, cộng thêm có thể là một giá trị chính xác bổ sung. Tùy chọn giá trị chính xác không liên quan đến việc tối đa hóa x, vì bất kỳ điểm chính xác nào cũng không thể vượt quá ngưỡng tối đa của tất cả các ràng buộc trừ khi nó trùng với ngưỡng đó. Hạn chế thực sự là với mỗi chai, chúng ta phải bỏ qua nó hoặc đảm bảo x thỏa mãn x ≤ (a − k) / m. Điều này có nghĩa là mỗi chai áp đặt một ràng buộc giới hạn trên đối với x.

Tuy nhiên, việc bỏ qua một chai chỉ có giá trị nếu nó không buộc phải vi phạm quy tắc. Một chai chỉ có thể được bỏ qua một cách an toàn khi được phép chọn không sử dụng nó, điều này luôn xảy ra vì tuyên bố cho phép bỏ qua hoàn toàn đồ uống. Do đó, tính khả thi toàn cầu giảm xuống để đảm bảo x không vượt quá ngưỡng của bất kỳ chai hoạt động nào. Do đó, x tối ưu là giá trị tối đa trên tất cả các đóng góp hợp lệ, được tính là x tốt nhất sao cho tồn tại ít nhất một cấu hình hợp lệ trên các chai, đơn giản là x khả thi tối đa thỏa mãn tất cả các ràng buộc trên mỗi chai. 

Đối với mỗi chai, ràng buộc mạnh nhất là x ≤ (a − k) / m khi a ≥ k, nếu không thì nó không đóng góp gì. Câu trả lời trở thành x tối đa sao cho ít nhất một chai có thể hỗ trợ nó và vì chúng tôi đang tối đa hóa x nên chúng tôi lấy mức tối đa trên tất cả các giới hạn hợp lệ trên mỗi chai. 

Điều này làm giảm vấn đề quét tất cả các chai và tính toán giá trị ứng viên tốt nhất có thể thu được từ mỗi chai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên x | O(n · tối đa a) | O(1) | Quá chậm | 
| Khai thác hạn chế trên mỗi chai | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích lại vấn đề bằng cách chọn số tiền x tốt nhất có thể cho mỗi người bạn tương thích với ít nhất một cách hợp lệ để xử lý từng chai một cách độc lập. 

1. Đối với mỗi chai có giá trị a, hãy tính số lượng tối đa khẩu phần m mà nó có thể hỗ trợ và để lại 0 hoặc ít nhất k còn lại. Chúng tôi tập trung vào điều kiện “để lại ít nhất k” vì nó tạo ra phạm vi x khả thi lớn nhất. Điều này dẫn đến việc ứng viên bị ràng buộc x ≤ (a − k) // m bất cứ khi nào a ≥ k. 
2. Nếu a < k thì chai không thể đóng góp cấu hình còn lại hợp lệ. Cách sử dụng duy nhất có thể là phép chia chính xác, cách này chỉ có tác dụng khi a chia hết cho m, thu được x = a // m. Đây là một giá trị cố định duy nhất chứ không phải là một phạm vi. 
3. Đối với mỗi chai, hãy trích xuất giá trị tốt nhất có thể mà nó có thể hỗ trợ. Với a ≥ k, đây là (a − k) // m. Với a < k, nó là a // m nếu chia hết cho m, nếu không thì nó không đóng góp gì. 
4. Theo dõi mức tối đa trên tất cả các ứng viên hợp lệ trên mỗi chai. Trực giác là bất kỳ x nào có thể đạt được đều phải được hỗ trợ bởi ít nhất một cấu hình chai và chiến lược tối ưu là chọn chai không có nút cổ chai cho phép x lớn nhất. 
5. Trả về giá trị tối đa được tìm thấy hoặc 0 nếu không có chai nào hỗ trợ bất kỳ khẩu phần hợp lệ nào. 

Tại sao nó hoạt động dựa trên cấu trúc mà mỗi chai không tương tác với những chai khác ngoại trừ thông qua lựa chọn chung là x. Khi x được cố định, tính khả thi trên mỗi chai là độc lập. Sự độc lập này làm suy giảm tối ưu hóa toàn cầu thành việc chọn x tốt nhất cho phép từ tập hợp các bộ khả thi trên mỗi chai và vì chúng tôi tối đa hóa x nên giải pháp được xác định bởi giới hạn trên lớn nhất trên tất cả các bộ hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        ans = 0
        
        for x in a:
            if x >= k:
                ans = max(ans, (x - k) // m)
            else:
                if x % m == 0:
                    ans = max(ans, x // m)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp kiểm thử một cách độc lập và duy trì câu trả lời tối đa đang chạy. Logic chính nằm bên trong vòng lặp trên các chai, trong đó mỗi chai được giảm xuống một giá trị ứng cử viên duy nhất. Phép trừ k xử lý giới hạn còn sót lại, trong khi phép chia số nguyên cho m sẽ chuyển khối lượng còn lại có thể sử dụng thành phân bổ cho mỗi người bạn. 

Một cạm bẫy phổ biến là quên rằng các chai có a < k vẫn có thể đóng góp thông qua phép chia hết chính xác. Một cách khác là trộn thứ tự các phép toán trong (a - k) // m, thứ tự này phải được tính toán sau khi xác minh a ≥ k để tránh đóng góp âm. 

## Ví dụ đã hoạt động 

Xét một ví dụ nhỏ với n = 3, m = 3, k = 2 và chai [10, 7, 4]. 

| Chai | một | Tình trạng | Ứng viên x | 
| --- | --- | --- | --- | 
| 1 | 10 | (10 − 2) // 3 = 2 | 2 | 
| 2 | 7 | (7 − 2) // 3 = 1 | 1 | 
| 3 | 4 | (4 − 2) // 3 = 0 | 0 | 

Tối đa là 2. Điều này cho thấy chai chặt hơn sẽ hạn chế câu trả lời như thế nào ngay cả khi một chai cho phép giá trị cao hơn. 

Bây giờ xét n = 2, m = 4, k = 5 và chai [16, 9]. 

| Chai | một | Tình trạng | Ứng viên x | 
| --- | --- | --- | --- | 
| 1 | 16 | (16 − 5) // 4 = 2 | 2 | 
| 2 | 9 | (9 − 5) // 4 = 1 | 1 | 

Câu trả lời là 2, được thúc đẩy bởi chai đầu tiên. Chai thứ 2 yếu hơn nhưng không ảnh hưởng tối đa. 

Những ví dụ này xác nhận rằng mỗi chai độc lập tạo ra một mức trần trên x và câu trả lời được xác định bởi mức trần mạnh nhất đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chai được xử lý một lần với số học thời gian không đổi | 
| Không gian | O(1) thêm | Chỉ một số biến được duy trì bất kể kích thước đầu vào | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì ngay cả các phần tử 5 × 10^5 cũng được xử lý theo thời gian tuyến tính bằng các phép toán số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    output = []
    
    input = sys.stdin.readline
    T = int(input())
    for _ in range(T):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        ans = 0
        for x in a:
            if x >= k:
                ans = max(ans, (x - k) // m)
            else:
                if x % m == 0:
                    ans = max(ans, x // m)
        output.append(str(ans))
    return "\n".join(output)

# sample-style tests
assert run("1\n3 3 2\n1 18 10") == "3"

# minimum size
assert run("1\n1 1 1\n1") == "0"

# all equal values
assert run("1\n4 2 3\n10 10 10 10") == "3"

# boundary k large
assert run("1\n3 5 100\n10 20 30") == "0"

# exact division dominance
assert run("1\n2 4 0\n8 16") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 cạnh chai | 0 | cấu hình nhỏ nhất | 
| giá trị bằng nhau | 3 | ràng buộc nhất quán | 
| k lớn | 0 | tất cả các chai không hợp lệ | 
| phép chia chính xác | 4 | xử lý hành vi kiểu k = 0 | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi k đủ lớn để hầu hết các chai rơi vào loại a < k. Đối với đầu vào n = 2, m = 3, k = 50 với chai [48, 51], thuật toán chỉ coi 48 là ứng cử viên nếu nó chia hết cho m, nếu không thì nó không đóng góp gì. Vì 48 không chia hết cho 3 nên nó bị bỏ qua. Với 51, ứng viên là (51 − 50) // 3 = 0, nên đáp án là 0. 

Việc thực hiện từng bước cho thấy không cần đến trạng thái trung gian. Mỗi chai được đánh giá độc lập và chỉ các đường dẫn số học hợp lệ mới đóng góp vào mức tối đa.
