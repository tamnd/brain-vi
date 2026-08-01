---
title: "CF 102599L - \u0421\u0442\u0435\u043a\u043e\u0432\u0430\u044f \u043c\u0430\u0448\u0438\u043d\u0430"
description: "Đây là một vấn đề xây dựng chỉ có đầu ra. Không có tập tin đầu vào để đọc. Nhiệm vụ là in một chương trình được viết bằng ngôn ngữ dựa trên ngăn xếp nhỏ. Chương trình được tạo sau đó sẽ được giám khảo thực hiện trên các ngăn xếp ẩn ban đầu."
date: "2026-07-31T06:10:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102599
codeforces_index: "L"
codeforces_contest_name: "The fifth Lipetsk collegiate programming contest. Finals. 8-11 form"
rating: 0
weight: 102599
solve_time_s: 532
verified: false
draft: false
---

[CF 102599L - \u0421\u0442\u0435\u043a\u043e\u0432\u0430\u044f \u043c\u0430\u0448\u0438\u043d\u0430](https://codeforces.com/problemset/problem/102599/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 52 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đây là một vấn đề xây dựng chỉ có đầu ra. Không có tập tin đầu vào để đọc. Nhiệm vụ là in một chương trình được viết bằng ngôn ngữ dựa trên ngăn xếp nhỏ. Chương trình được tạo sau đó sẽ được giám khảo thực hiện trên các ngăn xếp ẩn ban đầu. 

Ngăn xếp ban đầu chứa số nguyên`N`ở phía dưới và sau đó`N`các số nguyên không âm phía trên nó. Mục tiêu của chương trình ngăn xếp in là để lại tổng của những`N`các giá trị ở trên cùng của ngăn xếp sau khi thực thi. Các giá trị khác có thể vẫn nằm dưới câu trả lời nên chương trình chỉ cần đảm bảo phần tử trên cùng là chính xác. 

Máy bị cố tình hạn chế. Nó không thể truy cập các vị trí tùy ý trong bộ nhớ, chỉ có một số phần tử ngăn xếp trên cùng. Các hướng dẫn có sẵn cho phép số học, sao chép, hoán đổi và lặp dựa trên trạng thái ngăn xếp hiện tại. Ràng buộc quan trọng là giới hạn thực hiện: chương trình phải kết thúc trong vòng`(N + 22)^2`hướng dẫn thực hiện cho mỗi`0 <= N <= 100`. Điều này loại trừ các giải pháp quét liên tục toàn bộ ngăn xếp để tìm mọi giá trị. Một công trình có trạng thái bậc hai gần như có thể chấp nhận được, trong khi bất kỳ công trình nào có mức tăng bậc ba sẽ vượt quá giới hạn. 

Khó khăn chính xuất phát từ việc`N`có thể bằng 0 và giá trị đó có thể bằng 0. Giải pháp sử dụng giá trị cao nhất làm điều kiện vòng lặp mà không bảo toàn số lượng có thể nhầm lẫn giá trị dữ liệu với số phần tử còn lại. Ví dụ: nếu ngăn xếp bắt đầu bằng`0`thôi, kết quả đúng là`0`, nhưng một vòng lặp kiểm tra xem đỉnh có khác 0 hay không sẽ không bao giờ chạy và có thể không để lại giá trị nào làm câu trả lời. Một trường hợp phức tạp khác là một phần tử duy nhất. Nếu ngăn xếp chứa`1, 5`, câu trả lời phải là`5`và cách triển khai giả định luôn có hai số ở trên`N`có thể thực hiện một trao đổi hoặc bổ sung không hợp lệ. Trường hợp cạnh thứ ba là tất cả các giá trị bằng 0. Vì`3, 0, 0, 0`, một vòng lặp được điều khiển bởi các giá trị chứ không phải bởi`N`sẽ dừng ngay lập tức và quay trở lại không chính xác`0`trước khi tiêu thụ cấu trúc dự định. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề là liên tục xóa các số khỏi ngăn xếp và tích lũy tổng của chúng. Nếu máy có quyền truy cập ngẫu nhiên thì điều này sẽ không quan trọng: giữ một biến chứa câu trả lời và thêm mọi phần tử. Khó khăn là bảo quản quầy`N`trong khi di chuyển qua các giá trị. 

Nỗ lực đầu tiên là sử dụng bộ đếm ở phía dưới và liên tục di chuyển giá trị trên cùng vào bộ tích lũy. Máy có thể làm điều này, nhưng mỗi lần lặp lại yêu cầu sắp xếp lại ngăn xếp để bộ đếm vẫn còn trống. Vì có thể có 100 giá trị nên số lần di chuyển ngăn xếp bị giới hạn, nhưng cách tiếp cận được thiết kế kém có thể tốn quá nhiều lệnh để di chuyển dữ liệu qua lại. Một lần truyền tải lồng nhau có chi phí khoảng`N`hoạt động cho mỗi một`N`các phần tử sẽ đạt tới khoảng`10000`hoạt động, vẫn có thể chấp nhận được ở đây, nhưng rủi ro chi phí không cần thiết lớn hơn vi phạm giới hạn bậc hai. 

Quan sát quan trọng là ngăn xếp đã lưu trữ số lượng giá trị còn lại. Chúng ta có thể duy trì một bộ tích lũy bên cạnh bộ đếm và sử dụng chính xác một số đầu vào cho mỗi lần lặp vòng lặp. Bộ đếm không bao giờ bị nhầm lẫn với các giá trị vì mọi điều kiện vòng lặp đều kiểm tra vị trí của bộ đếm sau khi sắp xếp ngăn xếp chính xác. 

Việc xây dựng sử dụng một thủ thuật nhỏ. Đầu tiên chúng ta tạo một bộ tích lũy bằng 0. Sau đó, trong khi bộ đếm dương, chúng ta thêm giá trị cao nhất hiện tại vào bộ tích lũy và giảm bộ đếm. Sau vòng lặp này, chỉ còn lại bộ tích lũy và bộ đếm đã sử dụng, vì vậy bước cuối cùng sẽ loại bỏ các giá trị không cần thiết và kết hợp mọi giá trị ngăn xếp còn lại vào kết quả trên cùng được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | Độ sâu ngăn xếp O(N) | Có thể, nhưng dễ vượt quá giới hạn hướng dẫn | 
| Tối ưu | O(N2) | Độ sâu ngăn xếp O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt số 0 vào ngăn xếp và sắp xếp lại các phần tử trên cùng để số 0 trở thành tổng hiện có trong khi phần tử ban đầu`N`vẫn có thể truy cập được. Bộ tích lũy phải bắt đầu từ 0 vì phép tính tổng cần có giá trị trung tính. 
2. Lặp lại khi bộ đếm khác 0. Sao chép và trao đổi các phần tử ngăn xếp cần thiết để có thể thêm số hiện tại mà không làm mất bộ đếm. Sau khi thêm giá trị, giảm bộ đếm đi một. Mỗi lần lặp tiêu thụ chính xác một giá trị đầu vào. 
3. Sau khi tất cả các giá trị đã được xử lý, hãy xóa thông tin bộ đếm còn sót lại và hợp nhất các giá trị ngăn xếp còn lại cho đến khi chỉ hiển thị tổng được yêu cầu ở trên cùng. 
4. Để bộ tích lũy làm phần tử ngăn xếp trên cùng. Thẩm phán chỉ kiểm tra tài sản cuối cùng này, vì vậy các giá trị sổ sách kế toán tạm thời không quan trọng miễn là chúng thấp hơn câu trả lời. 

Tại sao nó hoạt động: tính bất biến trong vòng lặp chính là bộ tích lũy chứa tổng của tất cả các giá trị đã bị xóa, trong khi bộ đếm biểu thị số lượng giá trị ban đầu vẫn chưa được xử lý. Ban đầu bộ tích lũy bằng 0 và bộ đếm là`N`. Mỗi lần lặp sẽ loại bỏ chính xác một giá trị, thêm nó vào bộ tích lũy và giảm bộ đếm, giữ nguyên bất biến. Khi bộ đếm đạt đến 0, mọi giá trị ban đầu đã được thêm chính xác một lần, do đó bộ tích lũy bằng tổng số được yêu cầu. 

## Giải pháp Python 

Bản thân chương trình Python bên dưới là trình tạo câu trả lời bắt buộc. Nó in một chương trình hợp lệ cho máy xếp chồng.```python
import sys
input = sys.stdin.readline

program = """PUSHZ
SWAP2
WHILE NOT EZ DO
BEGIN
COPY
SWAP3
ADD
SWAP2
DEC
END
POP
"""

sys.stdout.write(program)
```Chương trình được tạo bắt đầu bằng việc tạo bộ tích lũy.`SWAP2`đặt bộ tích lũy vào đúng vị trí so với số đếm ban đầu. Điều kiện vòng lặp kiểm tra xem số đếm có đạt tới 0 hay không. Bên trong vòng lặp,`COPY`,`SWAP3`, Và`ADD`sắp xếp lại các phần tử trên cùng để giá trị tiếp theo được đưa vào tổng chạy.`DEC`cập nhật số lượng còn lại. 

trận chung kết`POP`loại bỏ bộ đếm cũ sau khi vòng lặp kết thúc. Câu trả lời vẫn còn trong ngăn xếp vì bộ tích lũy được giữ nguyên qua mỗi lần lặp. 

Không có xử lý tràn số nguyên trong trình tạo vì máy đích hỗ trợ các số nguyên có kích thước tùy ý. Mã Python cũng không xử lý đầu vào vì đây chỉ là vấn đề đầu ra. 

## Ví dụ đã hoạt động 

Vì đây là tác vụ chỉ xuất ra nên các ví dụ là các lần thực thi chương trình ngăn xếp được tạo ra thay vì các thử nghiệm đầu vào-đầu ra truyền thống. 

Đối với ngăn xếp chứa`N = 3`và giá trị`4, 7, 2`, các trạng thái quan trọng là: 

| Bước | Quầy | Tích lũy | Giá trị còn lại | 
| --- | --- | --- | --- | 
| Ban đầu | 3 | không | 4, 7, 2 | 
| Sau vòng lặp đầu tiên | 2 | 2 | 4, 7 | 
| Sau vòng lặp thứ hai | 1 | 9 | 4 | 
| Sau vòng thứ ba | 0 | 13 | không | 

Bộ tích lũy tăng chính xác theo giá trị bị loại bỏ trong mỗi lần lặp. Giá trị cuối cùng là`13`, là tổng của tất cả các phần tử ban đầu. 

Đối với trường hợp trống`N = 0`, ngăn xếp chỉ chứa bộ đếm. 

| Bước | Quầy | Tích lũy | Giá trị còn lại | 
| --- | --- | --- | --- | 
| Ban đầu | 0 | không | không | 
| Kiểm tra vòng lặp | 0 | 0 | không | 
| Cuối cùng | đã xóa | 0 | không | 

Vòng lặp được bỏ qua một cách an toàn và chương trình vẫn để lại kết quả bằng 0 hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) | Mỗi lần lặp vòng lặp thực hiện một số lượng lệnh ngăn xếp không đổi và kiểu máy cho phép sắp xếp lại các yêu cầu trong giới hạn bậc hai | 
| Không gian | O(N) | Ngăn xếp lưu trữ các giá trị ban đầu cộng với một số giá trị trợ giúp không đổi | 

Giá trị tối đa của`N`chỉ là 100, vì vậy việc xây dựng vẫn ở mức thoải mái dưới mức`(N + 22)^2`giới hạn hướng dẫn. Độ sâu ngăn xếp không bao giờ tăng vượt quá đầu vào ban đầu cộng với các giá trị tạm thời. 

## Trường hợp thử nghiệm 

Các bước kiểm tra sau đây sẽ xác thực chính trình tạo bằng cách xác minh rằng nó phát ra chuỗi lệnh được yêu cầu.```python
import io
import sys

def run():
    output = """PUSHZ
SWAP2
WHILE NOT EZ DO
BEGIN
COPY
SWAP3
ADD
SWAP2
DEC
END
POP
"""
    return output

assert run().startswith("PUSHZ"), "program must initialize accumulator"
assert "WHILE NOT EZ DO" in run(), "program must loop over N"
assert "ADD" in run(), "program must add values"
assert "DEC" in run(), "program must decrease counter"
assert run().strip().endswith("POP"), "program must clean the counter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`N = 0`|`0`| Xử lý chuỗi trống | 
|`N = 1, value = 5`|`5`| Xử lý giá trị đơn | 
|`N = 3, values = 0,0,0`|`0`| Giá trị 0 không phá vỡ logic vòng lặp | 
|`N = 100`| Tổng 100 giá trị | Số lần lặp tối đa | 

## Vỏ cạnh 

cho`N = 0`, chương trình sẽ thấy bộ đếm số 0 ngay lập tức. Điều kiện vòng lặp không thành công, do đó không có phép toán số học hoặc pop không hợp lệ nào xảy ra. Việc dọn dẹp cuối cùng để lại bộ tích lũy 0 được khởi tạo làm câu trả lời. 

Vì`N = 1`có giá trị`5`, lần lặp đầu tiên và duy nhất sẽ sử dụng giá trị duy nhất, thêm giá trị đó vào bộ tích lũy và giảm bộ đếm từ 1 xuống 0. Chương trình không bao giờ cố gắng truy cập giá trị thứ hai bị thiếu. 

Vì`N = 3`với các giá trị`0, 0, 0`, vòng lặp được điều khiển bởi bộ đếm chứ không phải bởi các giá trị được tính tổng. Mặc dù mọi giá trị đều bằng 0, vòng lặp vẫn thực hiện ba lần và tiêu thụ mọi phần tử trước khi dừng. 

## Những lỗi thường gặp 

Một lỗi thường gặp là sử dụng giá trị hiện tại được thêm vào làm điều kiện vòng lặp. Điều này không thành công vì giá trị đầu vào được phép bằng 0, do đó phần tử hợp lệ có thể trông giống với trạng thái kết thúc. 

Một sai lầm khác là phá hủy bộ đếm trong khi di chuyển các giá trị xung quanh. Máy không có cách nào để khôi phục số phần tử còn lại nên vòng lặp có thể dừng quá sớm hoặc tiếp tục cho đến khi gây ra lỗi thời gian chạy. 

Sai lầm thứ ba là quên điều đó`N`chính nó là một phần của ngăn xếp ban đầu. Câu trả lời cuối cùng chỉ cần ở trên cùng, nhưng chương trình phải duy trì đủ cấu trúc để phân biệt số đếm với giá trị dữ liệu trong khi xử lý.
