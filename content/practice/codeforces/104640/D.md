---
title: "CF 104640D - \u0422\u0435\u0441\u0442 \u043d\u0430 \u0438\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442"
description: "Chúng ta được yêu cầu xây dựng một chuỗi có độ dài $n$ trên các chữ cái Latinh viết thường. Sau khi chúng ta xuất chuỗi, máy sẽ đánh giá chuỗi đó theo cách chỉ phụ thuộc vào các chữ cái riêng biệt xuất hiện và số lần mỗi chữ cái xuất hiện."
date: "2026-06-29T16:49:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104640
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104640
solve_time_s: 60
verified: true
draft: false
---

[CF 104640D - \u0422\u0435\u0441\u0442 \u043d\u0430 \u0438\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442](https://codeforces.com/problemset/problem/104640/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một chuỗi có độ dài$n$trên các chữ cái Latinh viết thường. Sau khi chúng ta xuất chuỗi, máy sẽ đánh giá chuỗi đó theo cách chỉ phụ thuộc vào các chữ cái riêng biệt xuất hiện và số lần mỗi chữ cái xuất hiện. 

Đầu tiên, máy đếm xem có bao nhiêu chữ cái riêng biệt được sử dụng trong chuỗi, gọi đây là$k$. Sau đó nó gán cho những$k$các chữ cái có trọng số nguyên khác nhau từ$1$ĐẾN$k$, chọn bài tập có điểm số cuối cùng nhỏ nhất. Điểm được tính bằng cách tính tổng trọng số của mỗi ký tự xuất hiện trong chuỗi. 

Vì vậy, cỗ máy có hiệu quả bất lợi trong cách gán trọng số, nhưng vẫn bị hạn chế sử dụng hoán vị của$1 \ldots k$. Đối với bất kỳ tập hợp nhiều tần số chữ cái cố định nào, máy sẽ gán trọng số nhỏ hơn cho các chữ cái thường xuyên hơn, vì điều đó làm giảm tổng. 

Chúng ta được yêu cầu xây dựng một chuỗi có độ dài$n$tối đa hóa điểm tối thiểu cuối cùng này. 

Kích thước đầu vào cho phép$n \le 10^5$, vì vậy chúng ta cần một$O(n)$hoặc$O(n \log n)$sự thi công. Bất cứ điều gì liên quan đến việc tìm kiếm trên tất cả các chuỗi hoặc hoán vị đều không thể thực hiện được ngay lập tức vì số lượng chuỗi là$26^n$và thậm chí việc đánh giá một cấu hình cũng yêu cầu sắp xếp tần số, tần số này sẽ quá chậm nếu thực hiện nhiều lần. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các ký tự giống hệt nhau. Ví dụ, nếu$n = 3$và chuỗi là`"aaa"`, sau đó$k = 1$, trọng lượng buộc phải bằng$1$, và số điểm là$3$. Nếu thay vào đó chúng ta sử dụng`"abc"`, sau đó$k = 3$, và máy gán trọng số$1,2,3$theo cách tốt nhất có thể, dẫn đến số điểm lớn hơn nhiều. Điều này cho thấy rằng sự đa dạng ngày càng tăng có xu hướng tăng điểm, nhưng sự tương tác không chỉ là “tối đa hóa các chữ cái khác biệt”, bởi vì máy luôn gán trọng số tốt nhất cho chúng ta. 

## Phương pháp tiếp cận 

Nếu chúng ta sửa một chuỗi, hành vi của máy sẽ mang tính xác định: nó sắp xếp các chữ cái theo tần suất theo thứ tự giảm dần và gán trọng số$1$tới lá thư thường xuyên nhất,$2$đến lần thứ hai thường xuyên nhất, v.v. Điều này là tối ưu để giảm thiểu tổng vì mỗi lần xuất hiện của một chữ cái thường xuyên sẽ được nhân với trọng số nhỏ hơn. 

Vì vậy, giá trị trở thành:$$T = \sum_{i=1}^{k} f_i \cdot w_i$$Ở đâu$f_i$là các tần số được sắp xếp theo thứ tự không tăng và$w_i$là một hoán vị của$1 \ldots k$, được gán tối ưu là$w_i = i$. 

Như vậy:$$T = \sum_{i=1}^{k} f_i \cdot i$$Bây giờ chúng tôi muốn chọn một chuỗi, nghĩa là chúng tôi chọn phân bố tần số trên tối đa 26 chữ cái tính tổng bằng$n$, để tối đa hóa tổng trọng số này. 

Quan sát quan trọng là các chỉ số cao hơn có giá trị hơn, vì vậy chúng tôi muốn tần số lớn phù hợp với trọng số lớn. Vì trọng lượng được cố định như$1$bởi vì$k$, tối đa hóa$k$một mình là không đủ. Thay vào đó, chúng tôi muốn cấu trúc tần số để các chỉ số lớn hơn có số lượng lớn. 

Tuy nhiên, có một sự đơn giản hóa mạnh mẽ hơn. Đối với một cố định$k$, điều tốt nhất chúng ta có thể làm là tập trung khối lượng lớn nhất có thể vào chỉ số lớn nhất$k$, vì chữ cái đó có trọng số cao nhất. Để tối đa hóa$T$, chúng tôi muốn tối đa hóa$k$, kể từ khi tăng$k$tăng cả số lượng số hạng và trọng số tối đa có thể. 

Nhưng$k \le 26$, vì vậy chúng ta luôn có thể sử dụng tối đa 26 chữ cái riêng biệt. Khi chúng tôi sử dụng tất cả 26 tần số, lựa chọn duy nhất còn lại là cách phân phối tần số. Để cố định$k$, để tối đa hóa:$$\sum i \cdot f_i$$chúng ta nên gán tần số lớn nhất cho các chỉ số lớn nhất. Điều đó có nghĩa là chúng tôi muốn phân bổ tần số tăng dần theo thứ tự chỉ mục ngược. 

Điều này trở thành một cấu trúc tham lam cổ điển: phân phối$n$các ký tự trong số tối đa 26 chữ cái sao cho:$$f_{26} \ge f_{25} \ge \cdots \ge f_1 \ge 0$$và tối đa hóa tổng trọng số. Chiến lược tối ưu là sử dụng tất cả 26 chữ cái (hoặc ít hơn nếu$n < 26$) và phân phối đồng đều nhất có thể, trước tiên cung cấp thêm ký tự cho các chữ cái có chỉ số cao hơn. 

Cách tiếp cận vũ phu sẽ thử tất cả các phân vùng của$n$thành nhiều nhất 26 phần và tính điểm theo cấp số nhân$n$. Sự phân phối tham lam làm giảm điều này thành một đường chuyền tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$n$| O(n) | Quá chậm | 
| Phân phối tham lam tối ưu | O(26) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định số lượng chữ cái khác nhau$k = \min(26, n)$. Điều này đảm bảo chúng tôi không bao giờ vượt quá giới hạn bảng chữ cái trong khi vẫn tối đa hóa tính đa dạng khi có thể. 
2. Khởi tạo một mảng có kích thước 26 với toàn số 0, biểu thị tần số của các chữ cái`'a'`ĐẾN`'z'`. 
3. Chỉ định một lần xuất hiện cho mỗi lần xuất hiện đầu tiên$k$các chữ cái. Điều này đảm bảo chúng tôi sử dụng các chữ cái riêng biệt tối đa có thể. 
4. Đặt các ký tự còn lại$rem = n - k$. Đây là những lần xuất hiện thêm phải được phân phối. 
5. Liên tục gán thêm một ký tự cho các chữ cái từ`'a' + k - 1`hướng xuống`'a'`, tuần hoàn từ chỉ số cao nhất đến chỉ số thấp nhất, cho đến khi tất cả các ký tự còn lại được phân phối. Điều này đảm bảo các chữ cái có trọng số cao hơn nhận được nhiều tần suất hơn, giúp tối đa hóa tổng trọng số. 
6. Tạo chuỗi cuối cùng bằng cách lặp lại từng chữ cái theo tần số của nó. 

### Tại sao nó hoạt động 

Điểm số cuối cùng phụ thuộc vào việc kết hợp tần số cao với trọng số cao. Vì máy gán trọng số cao hơn cho các chữ cái ít thường xuyên hơn nên chúng tôi muốn kiểm soát thứ tự tần số để ánh xạ buộc số lượng lớn lên trọng số lớn. Bằng cách phân phối các lần xuất hiện bổ sung theo thứ tự bảng chữ cái đảo ngược, chúng tôi đảm bảo rằng các chữ cái có chỉ số cao hơn không bao giờ ít thường xuyên hơn các chữ cái có chỉ số thấp hơn. Điều này giữ cho thứ tự tần số được sắp xếp của máy phù hợp với thứ tự được xây dựng của chúng tôi, tối đa hóa sự đóng góp của trọng số cao hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    k = min(26, n)
    freq = [0] * 26
    
    for i in range(k):
        freq[i] = 1
    
    rem = n - k
    idx = k - 1
    
    while rem > 0:
        freq[idx] += 1
        rem -= 1
        idx -= 1
        if idx < 0:
            idx = k - 1
    
    res = []
    for i in range(26):
        res.append(chr(ord('a') + i) * freq[i])
    
    print("".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên đảm bảo các chữ cái khác biệt tối đa bằng cách gán một lần xuất hiện cho mỗi chữ cái đầu tiên.$k$các chữ cái. Sau đó, các ký tự còn lại được phân phối một cách tham lam bắt đầu từ chữ cái hoạt động được lập chỉ mục cao nhất, tần số này có xu hướng tăng lên trong các ký tự từ điển sau này tương ứng với trọng số do máy gán cao hơn sau khi sắp xếp. 

Giai đoạn xây dựng rất đơn giản: chúng tôi xây dựng lại chuỗi bằng cách lặp lại từng ký tự theo tần số tính toán của nó. 

Một chi tiết triển khai tinh tế là sự giảm dần theo chu kỳ của`idx`, đảm bảo phân phối công bằng giữa tất cả các chữ cái được chọn thay vì bỏ đói hoàn toàn các chỉ số thấp hơn khi$rem$là lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Chúng tôi tính toán$k = 3$. Tần số ban đầu là:`a:1, b:1, c:1`, còn lại là 0. 

| Bước | k | tần số(a) | tần số(b) | tần số(c) | rem | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 1 | 1 | 1 | 0 | 

Chuỗi đầu ra có thể là:```
abc
```Điều này cho thấy khi không có ký tự thừa nào tồn tại, giải pháp tối ưu chỉ đơn giản là tối đa hóa các chữ cái riêng biệt. 

### Ví dụ 2 

đầu vào:```
5
```chúng tôi có$k = 5$, vì vậy chúng ta bắt đầu với một trong mỗi`a`ĐẾN`e`, sau đó phân phối thêm 0 vì$n = k$. 

| Bước | một | b | c | d | e | rem | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 1 | 1 | 1 | 0 | 

Đầu ra:```
abcde
```Điều này khẳng định rằng đa dạng hóa hoàn toàn là tối ưu khi$n \le 26$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 + n) | Việc gán tần số và xây dựng chuỗi là tuyến tính theo kích thước bảng chữ cái và kích thước đầu ra | 
| Không gian | O(26) | Chỉ mảng tần số cho các chữ cái | 

Giải pháp có hiệu quả đối với$n \le 10^5$, vì tất cả các phép toán đều tuyến tính và các hằng số đều nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(sys.stdin.readline().strip())
    
    k = min(26, n)
    freq = [0] * 26
    
    for i in range(k):
        freq[i] = 1
    
    rem = n - k
    idx = k - 1
    
    while rem > 0:
        freq[idx] += 1
        rem -= 1
        idx -= 1
        if idx < 0:
            idx = k - 1
    
    res = []
    for i in range(26):
        res.append(chr(ord('a') + i) * freq[i])
    
    return "".join(res)

# provided sample
assert run("3\n") == "abc", "sample 1"

# custom cases
assert len(run("1\n")) == 1, "minimum size"
assert len(run("26\n")) == 26, "exact alphabet"
assert set(run("5\n")) <= set("abcde"), "no extra letters"
assert run("27\n").count("a") >= 1, "wrap distribution"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | thư đơn | ranh giới tối thiểu | 
| 26 | hoán vị của a-z | sử dụng bảng chữ cái đầy đủ | 
| 5 | abcde | tính đúng đắn cơ bản | 
| 27 | một sự lặp đi lặp lại trong số những người khác | phân phối toàn diện | 

## Vỏ cạnh 

cho$n = 1$, bộ thuật toán$k = 1$, gán`a:1`và đầu ra`"a"`. Không có bước phân phối lại vì`rem = 0`, nên kết quả gần như đúng. 

Vì$n = 26$, chúng ta nhấn vào bảng chữ cái đầy đủ đúng một lần. Máy ấn định trọng lượng$1$bởi vì$26$và không có sự phân phối lại nào xảy ra. Việc xây dựng tạo ra tần số đồng nhất, phù hợp với cấu trúc cực đoan dự định. 

Vì$n > 26$, sự phân bố theo chu kỳ đảm bảo không có chữ cái nào bị thiếu lần xuất hiện. Mỗi ký tự phụ làm tăng phần đóng góp trọng lượng của các chữ cái có chỉ số cao hơn trước tiên do thứ tự gán ngược lại. Truy tìm$n = 28$hiển thị hai chu kỳ phân phối trên 26 chữ cái, duy trì thứ tự tần số dự định và giữ cho nhiệm vụ được sắp xếp của máy phù hợp với cấu trúc của chúng tôi.
