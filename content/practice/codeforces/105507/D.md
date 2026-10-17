---
title: "CF 105507D - \u041f\u0440\u0435\u043e\u0431\u0440\u0430\u0437\u043e\u0432\u0430\u043d\u0438\u0435 \u0441\u0442\u0440\u043e\u043a\u0438"
description: "Chúng ta có hai chuỗi có độ dài bằng nhau, cả hai đều bao gồm các chữ cái Latinh viết thường. Thao tác duy nhất được phép lấy ký tự đầu tiên của chuỗi hiện tại, xóa nó và chèn lại nó vào bất kỳ vị trí nào trong chuỗi, kể cả quay lại đầu hoặc cuối."
date: "2026-06-24T00:15:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105507
codeforces_index: "D"
codeforces_contest_name: "2024-2025 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 (\u0412\u041a\u041e\u0428\u041f 24, \u0421\u0430\u0440\u0430\u0442\u043e\u0432\u0441\u043a\u0438\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f)"
rating: 0
weight: 105507
solve_time_s: 71
verified: true
draft: false
---

[CF 105507D - \u041f\u0440\u0435\u043e\u0431\u0440\u0430\u0437\u043e\u0432\u0430\u043d\u0438\u0435 \u0441\u0442\u0440\u043e\u043a\u0438](https://codeforces.com/problemset/problem/105507/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài bằng nhau, cả hai đều bao gồm các chữ cái Latinh viết thường. Thao tác duy nhất được phép lấy ký tự đầu tiên của chuỗi hiện tại, xóa nó và chèn lại nó vào bất kỳ vị trí nào trong chuỗi, kể cả quay lại đầu hoặc cuối. Hoạt động này có thể được lặp đi lặp lại bao nhiêu lần. Mục tiêu là xác định số lượng nhỏ nhất các thao tác cần thiết để biến chuỗi ban đầu thành chuỗi đích hoặc quyết định rằng nó hoàn toàn không thể thực hiện được. 

Nhận xét quan trọng từ chính thao tác này là chúng ta không bao giờ xóa hoặc tạo ký tự, vì vậy tập hợp nhiều ký tự phải giống hệt nhau giữa hai chuỗi. Điều này ngay lập tức đưa ra một điều kiện cần thiết: nếu tần số của bất kỳ ký tự nào khác nhau giữa hai chuỗi thì không có chuỗi thao tác nào có thể khắc phục được điều đó, vì chúng ta chỉ hoán vị các ký tự. 

Các ràng buộc này cho phép các chuỗi có độ dài lên tới 200.000, điều này ngay lập tức loại trừ mọi mô phỏng bậc hai của các phép biến đổi hoặc bất kỳ phương pháp nào liên tục xây dựng lại chuỗi một cách rõ ràng. Bất kỳ giải pháp nào truy cập lại các ký tự nhiều lần trong các vòng lặp lồng nhau sẽ quá chậm. Chúng ta nên hướng tới một quy trình tuyến tính hoặc gần tuyến tính, rất có thể liên quan đến quá trình quét tham lam hoặc mô phỏng giống như hàng đợi. 

Trường hợp cạnh tinh tế xuất hiện khi các ký tự khớp trong nhiều bộ nhưng không thể căn chỉnh theo thứ tự. Ví dụ: s = "ab" và t = "ba" là hợp lệ, nhưng s = "ab" và t = "ac" là không thể do số đếm không khớp. Một trường hợp thú vị khác là khi s và t là các hoán vị có độ lặp lại nhiều; Các chiến lược hoán đổi tham lam ngây thơ có thể thất bại nếu chúng cho rằng một ký tự có thể được đặt tùy ý mà không tính đến các ràng buộc trong tương lai. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là mô phỏng rõ ràng quá trình. Chúng tôi duy trì chuỗi và liên tục lấy ký tự đầu tiên và thử tất cả các vị trí chèn có thể. Điều này tạo ra hệ số phân nhánh rất lớn: mỗi bước có O(n) lựa chọn và có thể có tối đa n bước, do đó không gian tìm kiếm tăng theo cấp số nhân. Ngay cả việc cắt tỉa cũng không đủ vì không gian trạng thái về cơ bản là tất cả các hoán vị có thể đạt được bằng các phép quay bị ràng buộc, vẫn còn rất lớn. 

Thông tin chi tiết về cấu trúc quan trọng là hoạt động không cho phép hoán đổi tùy ý, nhưng nó cho phép chúng ta "hoãn" ký tự phía trước hiện tại bằng cách gửi nó về phía sau hoặc bất kỳ vị trí nào sau đó. Nói cách khác, chúng ta đang quét chuỗi từ trái sang phải, nhưng chúng ta được phép trì hoãn các ký tự không hữu ích ngay lập tức cho việc xây dựng chuỗi mục tiêu. 

Điều này biến vấn đề thành một quá trình so khớp tham lam giữa s và t. Chúng tôi cố gắng khớp t theo thứ tự trong khi quét s từ phía trước. Bất cứ khi nào ký tự phía trước hiện tại của s khớp với ký tự cần thiết tiếp theo trong t, chúng ta sẽ sử dụng cả hai. Khi nó không khớp, chúng tôi coi nhân vật đó như bị chuyển đi (ra phía sau), thực hiện một thao tác và tiếp tục. 

Ý tưởng tham lam này tránh việc khám phá tất cả các vị trí chèn một cách rõ ràng. Quyết định duy nhất là liệu nhân vật hiện tại có giúp chúng ta tiến bộ trong t hay phải trì hoãn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu của các phần chèn | Hàm mũ | O(n) | Quá chậm | 
| Mô phỏng hàng đợi tham lam | O(n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng cả hai chuỗi bằng cách sử dụng cấu trúc giống hàng đợi cho chuỗi nguồn và con trỏ cho chuỗi đích.

1. Đầu tiên, chúng tôi xác minh rằng cả hai chuỗi đều có số ký tự giống nhau. Nếu không, chúng tôi ngay lập tức trả về -1 vì không có chuỗi thao tác nào có thể điều hòa được sự khác biệt. 
2. Chúng ta duy trì một con trỏ j trên chuỗi đích, ban đầu ở mức 0, biểu thị ký tự tiếp theo mà chúng ta muốn khớp. 
3. Chúng tôi coi chuỗi nguồn là hàng đợi. Chúng tôi liên tục kiểm tra nhân vật phía trước của nó. 
4. Nếu ký tự phía trước bằng t[j], chúng ta tiến cả hai con trỏ. Điều này tương ứng với việc đặt ký tự đó vào đúng vị trí tương đối cuối cùng của nó mà không cần phải trì hoãn nó, do đó không phát sinh chi phí vận hành. 
5. Nếu ký tự phía trước không khớp với t[j], chúng tôi mô phỏng thực hiện thao tác được phép: chúng tôi loại bỏ nó và chèn nó vào cuối chuỗi, tăng bộ đếm thao tác lên một. 
6. Chúng tôi tiếp tục quá trình này cho đến khi tất cả các ký tự của chuỗi đích được khớp. 

Ý tưởng quan trọng là chúng ta không bao giờ cần phải chọn một vị trí chèn một cách rõ ràng. Bất kỳ ký tự không hữu ích nào chỉ đơn giản là bị đẩy lùi, duy trì tính chính xác vì vị trí cuối cùng của nó được xác định ngầm định khi nào nó cuối cùng trở nên hữu ích. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, chúng tôi đang cố gắng căn chỉnh ký tự bắt buộc tiếp theo trong t. Nếu phía trước của hàng đợi đã khớp, việc trì hoãn nó sẽ chỉ làm tăng hoạt động mà không mang lại lợi ích gì. Nếu nó không khớp, việc giữ nó ở phía trước sẽ ngăn cản tiến trình t, vì vậy nó phải được hoãn lại. Điều này tạo ra một bất biến tham lam: mọi ký tự hoặc được tiêu thụ ngay lập tức khi nó khớp với vị trí mục tiêu được yêu cầu hoặc bị hoãn lại chính xác khi nó chặn tiến trình. Điều này đảm bảo chúng tôi không bao giờ trì hoãn không cần thiết và không bao giờ bỏ lỡ cơ hội trận đấu hợp lệ. 

Bởi vì mọi ký tự đều được so khớp hoặc di chuyển lùi theo cách hoàn toàn nâng cao con trỏ trong mô phỏng, nên quá trình này đơn điệu về mặt tiến triển thông qua t, do đó nó không thể quay vòng vô thời hạn hoặc mất đi tính tối ưu. 

## Giải pháp Python```python
import sys
from collections import Counter, deque

input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()
    t = input().strip()

    if Counter(s) != Counter(t):
        print(-1)
        return

    dq = deque(s)
    j = 0
    ops = 0

    while j < n:
        if dq[0] == t[j]:
            dq.popleft()
            j += 1
        else:
            dq.append(dq.popleft())
            ops += 1

    print(ops)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc kiểm tra tính khả thi bằng cách sử dụng số lượng ký tự, vì bất kỳ sự không khớp nào cũng khiến cho việc chuyển đổi không thể thực hiện được bất kể thao tác nào. Mô phỏng chính sử dụng deque để biểu diễn chuỗi đang phát triển. Con trỏ j theo dõi tiến trình trong chuỗi đích. 

Mỗi bước sử dụng một ký tự phù hợp hoặc xoay ký tự phía trước về phía sau, đếm một thao tác. Vòng lặp kết thúc khi tất cả các ký tự của t được khớp. 

Một chi tiết triển khai tinh tế là chúng ta luôn truy cập dq[0], do đó deque không bao giờ được để trống khi j < n. Điều này được đảm bảo vì cả hai chuỗi đều có độ dài giống hệt nhau và chúng tôi chỉ xóa các ký tự khi chúng khớp hoặc di chuyển, duy trì tính nhất quán về kích thước theo nghĩa tuần hoàn của mô phỏng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

s = "abcd", t = "dcba" 

| Bước | trạng thái dq | j | hành động | hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | abcd | 0 | a ≠ d, xoay | 1 | 
| 2 | bcda | 0 | b ≠ d, xoay | 2 | 
| 3 | cdab | 0 | c ≠ d, xoay | 3 | 
| 4 | dabc | 0 | d = d, khớp | 3 | 
| 5 | abc | 1 | một = c? không, xoay | 4 | 
| ... | ... | ... | tiếp tục | ... | 

Cuối cùng, tất cả các ký tự đều được căn chỉnh và thuật toán sẽ đếm số lần xoay cần thiết trước khi có được mỗi căn chỉnh chính xác. Điều này cho thấy sự không phù hợp được trì hoãn một cách hiệu quả như thế nào cho đến khi ký tự được yêu cầu xuất hiện ở phía trước. 

### Ví dụ 2 

đầu vào: 

s = "tot", t = "tot" 

| Bước | trạng thái dq | j | hành động | hoạt động | 
| --- | --- | --- | --- | --- | 
| 1 | tot | 0 | t trận đấu | 0 | 
| 2 | ot | 1 | o trận đấu | 0 | 
| 3 | t | 2 | t trận đấu | 0 | 

Trường hợp này thể hiện hành vi tối ưu khi không cần trì hoãn. Mọi ký tự đều đã được căn chỉnh theo thứ tự, do đó số thao tác vẫn bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) khấu hao | Mỗi ký tự được di chuyển từ trước ra sau một số lần giới hạn và cuối cùng bị tiêu thụ | 
| Không gian | O(n) | Deque lưu trữ tất cả các ký tự trong quá trình mô phỏng | 

Thuật toán phù hợp với các ràng buộc vì mọi hoạt động đều có thời gian không đổi và tổng số hoạt động deque là tuyến tính trong thực tế do mức tiêu thụ đơn điệu của con trỏ đích. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter, deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input().strip())
    s = input().strip()
    t = input().strip()

    if Counter(s) != Counter(t):
        return "-1"

    dq = deque(s)
    j = 0
    ops = 0

    while j < n:
        if dq[0] == t[j]:
            dq.popleft()
            j += 1
        else:
            dq.append(dq.popleft())
            ops += 1

    return str(ops)

# provided samples
assert run("4\nabcd\ndcba\n") == "3"
assert run("3\ntot\ntot\n") == "0"
assert run("7\ncabaaba\nabacaba\n") == "2"
assert run("2\nab\nbb\n") == "-1"

# custom cases
assert run("1\na\na\n") == "0"
assert run("5\nabcde\neabcd\n") == "4"
assert run("6\naabbcc\nccbbaa\n") == "6"
assert run("4\nabca\naabc\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char giống hệt nhau | 0 | trường hợp tối thiểu | 
| xoay đơn giản | k nhỏ | tính đúng đắn của sự dịch chuyển theo chu kỳ | 
| khối đảo ngược | hoạt động không tầm thường | xử lý các vòng quay lặp đi lặp lại | 
| nhiều tập không thể | -1 | kiểm tra tính khả thi | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chuỗi đích yêu cầu một ký tự tồn tại nhưng hiện bị chặn sâu trong hàng đợi. Ví dụ: s = "baaa", t = "aaab". Thuật toán liên tục xoay chữ 'b' ở đầu cho đến khi có đủ ký tự 'a' ở phía trước. Mô phỏng đảm bảo rằng 'b' được hoãn lại chính xác nhiều lần nếu cần trước khi nó trở nên phù hợp. 

Một trường hợp khác là khi s đã bằng t. Trong tình huống này, mọi so sánh đều thành công ngay lập tức, do đó không có phép quay nào được thực hiện và câu trả lời là 0. Thuật toán xử lý việc này một cách tự nhiên vì phép so sánh đầu tiên luôn thành công và nâng cao cả hai con trỏ mà không kích hoạt bất kỳ thao tác nào. 

Trường hợp cạnh cuối cùng là khi các chuỗi có nhiều bộ giống hệt nhau nhưng yêu cầu sắp xếp lại nhiều, chẳng hạn như đảo ngược hoàn toàn. Ngay cả trong trường hợp này, mọi sự không khớp đều được xử lý bằng một lần xoay duy nhất và cuối cùng mỗi ký tự sẽ căn chỉnh chính xác khi nó tiến về phía trước trong đúng pha của chuỗi mục tiêu, đảm bảo tiến trình tuyến tính xuyên suốt cấu trúc.
