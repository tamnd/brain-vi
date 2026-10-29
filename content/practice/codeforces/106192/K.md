---
title: "CF 106192K - \u0417\u0430\u043b\u0438\u0432\u043a\u0430"
description: "Chúng ta có một đồ thị đầy đủ với các đỉnh $n$. Mỗi đỉnh ban đầu có một màu và các màu được cho dưới dạng số nguyên. Bản thân cấu trúc đồ thị không thực sự là thứ chúng ta cần thao tác một cách rõ ràng vì mọi cặp đỉnh đều được kết nối với nhau."
date: "2026-06-22T19:10:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106192
codeforces_index: "K"
codeforces_contest_name: "\u041f\u0435\u0440\u043c\u0441\u043a\u0430\u044f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2025"
rating: 0
weight: 106192
solve_time_s: 50
verified: true
draft: false
---

[CF 106192K - \u0417\u0430\u043b\u0438\u0432\u043a\u0430](https://codeforces.com/problemset/problem/106192/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một biểu đồ hoàn chỉnh với$n$đỉnh. Mỗi đỉnh ban đầu có một màu và các màu được cho dưới dạng số nguyên. Bản thân cấu trúc đồ thị không thực sự là thứ chúng ta cần thao tác một cách rõ ràng vì mọi cặp đỉnh đều được kết nối với nhau. 

Thao tác duy nhất khả dụng là chọn một đồ thị con được kết nối có các đỉnh hiện có cùng màu và tô màu lại tất cả chúng thành bất kỳ màu nào khác. Mục tiêu là làm cho mỗi đỉnh có một màu đồng nhất duy nhất bằng cách sử dụng số lượng thao tác như vậy tối thiểu. 

Bởi vì đồ thị hoàn chỉnh nên bất kỳ tập con đỉnh nào cũng được kết nối. Điều này làm thay đổi đáng kể vấn đề: khả năng kết nối không hạn chế những đỉnh chúng ta có thể chọn, chỉ có điều kiện là tất cả các đỉnh được chọn hiện phải có cùng màu. 

Kích thước đầu vào tăng lên$10^6$, điều này ngay lập tức loại trừ mọi thứ bậc hai chẳng hạn như thử tất cả các cặp màu hoặc mô phỏng các thao tác lặp đi lặp lại trên các tập hợp con. Chúng ta cần một giải pháp về cơ bản xử lý mảng màu theo thời gian tuyến tính. 

Một vài tình huống tinh tế quan trọng: 

Nếu tất cả các đỉnh đã có cùng màu, ví dụ: 

đầu vào:```
5
3 3 3 3 3
```câu trả lời phải là 0, vì không cần thực hiện thao tác nào. Bất kỳ cách tiếp cận nào mù quáng giả định ít nhất một thao tác cho mỗi màu sẽ sai ở đây. 

Nếu tất cả các màu đều khác biệt, ví dụ:```
4
1 2 3 4
```chúng ta có thể nghi ngờ mình cần nhiều thao tác, nhưng vì chúng ta có thể tô lại nhiều đỉnh có cùng màu cùng một lúc nên câu trả lời đúng chỉ phụ thuộc vào số lượng màu riêng biệt tồn tại chứ không phụ thuộc vào$n$chính nó. 

Cạm bẫy chính là suy nghĩ quá mức về kết nối. Vì biểu đồ hoàn chỉnh nên khả năng kết nối không bao giờ hạn chế chúng tôi và cấu trúc thực sự duy nhất là cách phân bổ màu sắc. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng quá trình này. Ở mỗi bước, chúng tôi chọn một số lớp màu, chọn một số tập hợp con của nó, tô màu lại, cập nhật biểu đồ và lặp lại cho đến khi tất cả các đỉnh có cùng một màu. Về nguyên tắc, điều này đúng nhưng nó đòi hỏi phải duy trì các nhóm màu động và liên tục quét hoặc cập nhật các tập hợp đỉnh. Trong trường hợp xấu nhất, mỗi thao tác có thể chạm vào$O(n)$các đỉnh và chúng tôi có thể thực hiện tối đa$O(n)$hoạt động, dẫn đến$O(n^2)$hành vi này quá chậm đối với$n = 10^6$. 

Quan sát quan trọng là cấu trúc của biểu đồ làm cho mọi tập hợp đỉnh có cùng màu có thể hoán đổi cho nhau thành một đơn vị. Vì bất kỳ tập hợp con nào của các đỉnh cùng màu đều được kết nối nên chúng ta có thể coi mỗi màu riêng biệt là một khối. Quan trọng hơn, chúng ta có thể loại bỏ toàn bộ một màu trong một lần di chuyển bằng cách đổi màu tất cả các đỉnh của màu đó thành một màu mục tiêu nào đó. 

Một khi điều này rõ ràng, quá trình sẽ trở thành tổ hợp thuần túy. Giả sử có$k$màu sắc khác biệt ban đầu. Chúng tôi chọn một trong số chúng làm màu mục tiêu cuối cùng. Mọi lớp màu khác phải được hợp nhất vào đó ít nhất một lần và mỗi lớp có thể được hợp nhất trong một thao tác duy nhất. Điều này ngay lập tức mang lại$k - 1$như câu trả lời tối ưu. 

Không có lợi ích gì khi cố gắng tô màu lại các tập hợp con một phần hoặc thực hiện các phép biến đổi trung gian, bởi vì mỗi thao tác có thể sử dụng toàn bộ lớp màu mà không phải trả thêm phí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Đếm Màu Sắc Khác Biệt |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đơn giản hóa vấn đề để hiểu có bao nhiêu màu riêng biệt tồn tại trong mảng ban đầu. 

1. Đọc tất cả các màu của đỉnh và lưu chúng vào danh sách. Cấu trúc của biểu đồ không liên quan vì nó hoàn chỉnh nên khả năng kết nối không áp đặt hạn chế nào trong việc nhóm các đỉnh. 
2. Tính số màu riêng biệt trong mảng. Điều này có thể được thực hiện bằng cách sử dụng bộ băm trong khi quét mảng một lần. Lý do điều này có tác dụng là vì mỗi màu hoạt động như một nhóm độc lập có thể được hợp nhất trong một thao tác duy nhất. 
3. Gọi số màu phân biệt là$k$. Nếu như$k = 1$, trả về 0 ngay lập tức vì đồ thị đã đơn sắc. 
4. Nếu không, hãy quay lại$k - 1$. Điều này tương ứng với việc chọn một màu làm mục tiêu cuối cùng và hợp nhất từng lớp màu khác vào đó. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại bất kỳ thời điểm nào trong quy trình, mỗi lớp màu còn lại đều có thể thu gọn hoàn toàn thành bất kỳ màu nào khác chỉ trong một thao tác. Vì bất kỳ tập hợp con nào của màu đều được kết nối trong một biểu đồ hoàn chỉnh nên chúng tôi không bao giờ cần nhiều thao tác cho mỗi màu riêng biệt mà chúng tôi quyết định loại bỏ. Không có thao tác nào có thể giảm số lượng màu riêng biệt nhiều hơn một trừ khi nó hợp nhất toàn bộ lớp màu thành một lớp màu khác và việc hợp nhất như vậy luôn có thể thực hiện được. Điều này làm cho$k - 1$vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    colors = list(map(int, input().split()))
    distinct = len(set(colors))
    print(max(0, distinct - 1))

if __name__ == "__main__":
    main()
```Giải pháp đọc mảng màu, chèn tất cả các giá trị vào một tập hợp để tính xem có bao nhiêu màu duy nhất tồn tại và in$k - 1$. các`max(0, ...)`Guard xử lý trường hợp tầm thường trong đó tất cả các đỉnh đã có cùng màu. 

Điểm tinh tế chính là chúng tôi không bao giờ cố gắng mô phỏng các hoạt động. Bất kỳ mô phỏng nào cũng sẽ không cần thiết vì cấu trúc hoạt động cho phép thu gọn toàn bộ các lớp màu trong một bước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
```Chúng tôi theo dõi các màu sắc riêng biệt: 

| Bước | Màu đã qua xử lý | Số lượng riêng biệt | 
| --- | --- | --- | 
| Bắt đầu | {} | 0 | 
| Sau khi quét | {1,2,3,4} | 4 | 

Chúng tôi có$k = 4$, vậy câu trả lời là$3$. 

Điều này xác nhận rằng mặc dù tất cả các đỉnh đều khác nhau, nhưng chúng ta có thể hợp nhất chúng một cách hiệu quả bằng cách liên tục hấp thụ một lớp màu đầy đủ này vào một lớp màu khác. 

### Ví dụ 2 

đầu vào:```
5
2 2 2 1 1
```| Bước | Màu đã qua xử lý | Số lượng riêng biệt | 
| --- | --- | --- | 
| Bắt đầu | {} | 0 | 
| Sau khi quét | {1,2} | 2 | 

Đây$k = 2$, vậy câu trả lời là$1$. 

Điều này cho thấy trường hợp không tầm thường tối thiểu trong đó cần có chính xác một lần hợp nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần để xây dựng một bộ màu | 
| Không gian |$O(n)$| Đặt cửa hàng lên tới$n$giá trị riêng biệt trong trường hợp xấu nhất | 

Các ràng buộc cho phép lên đến$10^6$các đỉnh, do đó việc quét tuyến tính với hàm băm có thể thoải mái trong giới hạn cả về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input())
    colors = list(map(int, input().split()))
    return str(max(0, len(set(colors)) - 1))

# provided sample (if interpreted as single case)
assert run("1\n1\n") == "0"

# all equal
assert run("5\n3 3 3 3 3\n") == "0"

# all distinct
assert run("4\n1 2 3 4\n") == "3"

# mixed
assert run("5\n1 1 2 2 3\n") == "2"

# two colors only
assert run("6\n7 7 7 8 8 8\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không cần thao tác | 
| tất cả đều khác biệt | n-1 | sáp nhập trường hợp xấu nhất | 
| trùng lặp hỗn hợp | k-1 | sự đúng đắn khi lặp lại | 
| hai màu | 1 | trường hợp không tầm thường tối thiểu | 

## Vỏ cạnh 

Khi tất cả các đỉnh có cùng màu, kích thước được đặt là 1, do đó công thức mang lại$1 - 1 = 0$. Thuật toán trả về chính xác 0 mà không yêu cầu bất kỳ phân nhánh đặc biệt nào ngoài kết quả cuối cùng`max(0, ...)`. 

Khi mỗi đỉnh có một màu khác nhau, kích thước tập hợp sẽ trở thành$n$, và câu trả lời trở thành$n - 1$. Điều này phù hợp với thực tế là mỗi lớp màu phải được hấp thụ một lần và mỗi lần hấp thụ là một hoạt động hợp lệ. 

Khi màu sắc lặp lại theo các mẫu tùy ý, chẳng hạn như các giá trị xen kẽ hoặc nhóm, tính trừu tượng của tập hợp sẽ loại bỏ tất cả cấu trúc ngoài tính duy nhất. Vì các thao tác chỉ phụ thuộc vào nhận dạng màu sắc chứ không phải vị trí nên việc đơn giản hóa này vẫn hợp lệ cho tất cả các cấu hình.
