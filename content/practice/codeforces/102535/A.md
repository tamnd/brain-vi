---
title: "CF 102535A - Làm việc với ổ khóa"
description: "Chúng tôi có một bộ sưu tập nhỏ gồm năm chiếc chìa khóa và năm ổ khóa. Đầu vào cung cấp số chìa khóa mà Perry sở hữu và số ổ khóa trên cửa. Nhiệm vụ là quyết định xem chiếc chìa khóa cụ thể đó có khả năng mở được ổ khóa cụ thể đó hay không."
date: "2026-08-05T15:11:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "A"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 640
verified: true
draft: false
---

[CF 102535A - Thao tác với ổ khóa](https://codeforces.com/problemset/problem/102535/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 40 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập nhỏ gồm năm chiếc chìa khóa và năm ổ khóa. Đầu vào cung cấp số chìa khóa mà Perry sở hữu và số ổ khóa trên cửa. Nhiệm vụ là quyết định xem chiếc chìa khóa cụ thể đó có khả năng mở được ổ khóa cụ thể đó hay không. Nếu phím hoạt động, chúng tôi in thông báo thành công, nếu không chúng tôi sẽ in thông báo lỗi. 

Mối quan hệ giữa chìa khóa và ổ khóa được cố định. Phím 1 chỉ hoạt động với khóa 2, khóa 2 hoạt động với khóa 1 và 3, khóa 3 hoạt động với khóa 2 và 4, khóa 4 hoạt động với khóa 3 và 5, và khóa 5 chỉ hoạt động với khóa 4. Vì chỉ có năm phím có thể và năm ổ khóa có thể, nên toàn bộ không gian quyết định chỉ chứa 25 tổ hợp. Các ràng buộc không yêu cầu bất kỳ cấu trúc dữ liệu hoặc tối ưu hóa nâng cao nào. Bất kỳ giải pháp nào thực hiện khối lượng công việc không đổi đều đủ nhanh. 

Các trường hợp cạnh chính xuất phát từ phần cuối của chuỗi khóa và từ việc giả định rằng các số gần đó luôn hoạt động theo cùng một hướng. Ví dụ, đầu vào```
1
4
```nên sản xuất:```
CURSE YOU
```Chìa khóa 1 chỉ mở được khóa 2, vì vậy việc coi các chìa khóa như thể chúng có thể mở được tất cả các ổ khóa trong một khoảng cách nào đó sẽ cho kết quả không chính xác. 

Một trường hợp ranh giới khác là khóa cuối cùng:```
5
4
```Đầu ra đúng là:```
GOOD LUCK AGENT P
```Khóa 5 chỉ có một khóa hợp lệ và khóa đó là 4. Việc triển khai cố gắng truy cập khóa 6 không tồn tại trong khi kiểm tra các khóa lân cận có thể không thành công. 

Trường hợp thứ ba là chìa và ổ khóa có cùng số:```
3
3
```Đầu ra đúng là:```
CURSE YOU
```Các con số xác định các đối tượng khác nhau. Số khóa khớp với số khóa không hàm ý tính tương thích. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là lưu trữ các khóa có thể có cho mỗi khóa và kiểm tra xem khóa đã cho có xuất hiện trong danh sách đó hay không. Đây thực sự là một mô phỏng trực tiếp của hệ thống khóa. Điều này đúng vì mọi cặp khóa hợp lệ đều được kiểm tra theo các quy tắc chính xác. Trong vấn đề này, trường hợp xấu nhất vẫn yêu cầu chỉ kiểm tra hai ổ khóa có thể có cho một phím, do đó nó thực hiện nhiều nhất một vài thao tác. 

Một cách giải thích tổng quát hơn sẽ là thử mọi mối quan hệ khóa và khóa có thể có cho đến khi tìm thấy cặp đã cho. Với năm chìa khóa và năm ổ khóa, điều đó có nghĩa là kiểm tra tối đa 25 cặp. Điều này cũng dễ dàng được chấp nhận ở đây, nhưng nó bỏ qua thực tế là đầu vào đã xác định được cặp duy nhất mà chúng ta quan tâm. 

Quan sát quan trọng là hệ thống khóa là tĩnh. Câu trả lời không phụ thuộc vào bất kỳ tính toán hay tìm kiếm nào. Nó chỉ phụ thuộc vào việc cặp đó có thuộc về một tập hợp nhỏ các cặp hợp lệ được xác định trước hay không. Việc biểu diễn trực tiếp các cặp đó sẽ biến vấn đề thành một cuộc kiểm tra tư cách thành viên liên tục. 

Cách tiếp cận bạo lực hoạt động vì kích thước đầu vào rất nhỏ nhưng nó không thể hiện được cấu trúc thực sự của vấn đề. Việc quan sát thấy các mối quan hệ hợp lệ được cố định cho phép chúng tôi rút gọn giải pháp thành một tra cứu duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(25) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số phím và số khóa từ đầu vào. Hai giá trị này mô tả đầy đủ tình huống chúng ta cần đánh giá. 
2. Tạo biểu diễn các cặp khóa phím hợp lệ. Việc biểu diễn ánh xạ từng chìa khóa tới các ổ khóa mà nó có thể mở, khớp với các quy tắc của hệ thống khóa. 
3. Kiểm tra xem khóa đã cho có tồn tại trong số các khóa có sẵn cho khóa đã cho hay không. Tra cứu thành công nghĩa là chìa khóa có thể mở được cửa. 
4. In`"GOOD LUCK AGENT P"`cho một cặp hợp lệ và`"CURSE YOU"`nếu không thì. 

Tại sao nó hoạt động: 

Ánh xạ được lưu trữ chứa chính xác mọi mối quan hệ khóa phím được phép. Đối với bất kỳ đầu vào nào, thuật toán sẽ kiểm tra điều kiện tương tự như định nghĩa bài toán: liệu khóa được cung cấp có thuộc về bộ khóa được mở bằng khóa được cung cấp hay không. Vì không có cặp hợp lệ nào bị thiếu và không có cặp không hợp lệ nào được đưa vào nên câu trả lời do tra cứu tạo ra luôn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    L = int(input())

    can_open = {
        1: {2},
        2: {1, 3},
        3: {2, 4},
        4: {3, 5},
        5: {4}
    }

    if L in can_open[k]:
        print("GOOD LUCK AGENT P")
    else:
        print("CURSE YOU")

if __name__ == "__main__":
    solve()
```Từ điển lưu trữ hệ thống khóa hoàn chỉnh. Mỗi số chìa khóa trỏ đến một bộ chứa chính xác các số khóa mà nó có thể mở được. 

Kiểm tra tư cách thành viên sử dụng khóa đã cho để lấy các khóa có thể có của nó và sau đó kiểm tra xem khóa đầu vào có nằm trong bộ đó hay không. Vì các giá trị đầu vào được đảm bảo nằm trong khoảng từ 1 đến 5 nên không cần xác thực bổ sung hoặc xử lý ranh giới. 

Không có tính toán lập chỉ mục, do đó không có rủi ro riêng lẻ. Giải pháp này cũng tránh được các giả định số học về mối quan hệ giữa phím và số khóa, điều này rất quan trọng vì phím đầu tiên và phím cuối hoạt động khác với phím giữa. 

## Ví dụ đã hoạt động 

Đối với mẫu 1:```
1
4
```| Bước | Chìa khóa | Khóa | Ổ khóa có sẵn cho chìa khóa | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 1 | 4 | {2} | Tiếp tục | 
| Kiểm tra tư cách thành viên | 1 | 4 | {2} | 4 không có mặt | 
| Đầu ra | 1 | 4 | {2} | NGUYỆN BẠN | 

Dấu vết cho thấy các số bằng nhau hoặc gần nhau không xác định được khả năng tương thích. Câu trả lời chỉ đến từ những mối quan hệ được xác định trước. 

Đối với mẫu 2:```
2
3
```| Bước | Chìa khóa | Khóa | Ổ khóa có sẵn cho chìa khóa | Kết quả | 
| --- | --- | --- | --- | --- | 
| Đọc đầu vào | 2 | 3 | {1, 3} | Tiếp tục | 
| Kiểm tra tư cách thành viên | 2 | 3 | {1, 3} | 3 có mặt | 
| Đầu ra | 2 | 3 | {1, 3} | CHÀO MAY MẮN ĐẠI LÝ P | 

Ví dụ này xác nhận rằng một khóa có thể mở nhiều khóa và việc tra cứu xử lý chính xác nhiều lựa chọn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán thực hiện một lần tra cứu từ điển và một lần kiểm tra tư cách thành viên. | 
| Không gian | O(1) | Ánh xạ chứa một số mục cố định bất kể kích thước đầu vào. | 

Các ràng buộc là cực kỳ nhỏ nên giải pháp thời gian không đổi dễ dàng phù hợp với giới hạn thời gian và giới hạn bộ nhớ. Cách tiếp cận tương tự sẽ vẫn hiệu quả ngay cả khi số lượng ca kiểm thử tăng lên đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    k = int(sys.stdin.readline())
    L = int(sys.stdin.readline())

    can_open = {
        1: {2},
        2: {1, 3},
        3: {2, 4},
        4: {3, 5},
        5: {4}
    }

    if L in can_open[k]:
        print("GOOD LUCK AGENT P")
    else:
        print("CURSE YOU")

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert solution("1\n4\n") == "CURSE YOU\n", "sample 1"
assert solution("2\n3\n") == "GOOD LUCK AGENT P\n", "sample 2"
assert solution("3\n1\n") == "CURSE YOU\n", "sample 3"

assert solution("1\n2\n") == "GOOD LUCK AGENT P\n", "minimum key boundary"
assert solution("5\n4\n") == "GOOD LUCK AGENT P\n", "maximum key boundary"
assert solution("5\n5\n") == "CURSE YOU\n", "same value but invalid pair"
assert solution("3\n4\n") == "GOOD LUCK AGENT P\n", "middle key upper boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n2\n`|`GOOD LUCK AGENT P`| Kiểm tra khóa nhỏ nhất có khóa hợp lệ duy nhất. | 
|`5\n4\n`|`GOOD LUCK AGENT P`| Kiểm tra khóa lớn nhất và mối quan hệ biên của nó. | 
|`5\n5\n`|`CURSE YOU`| Ngăn chặn giả định các số bằng nhau luôn khớp. | 
|`3\n4\n`|`GOOD LUCK AGENT P`| Kiểm tra khóa giữa có khóa cao hơn hợp lệ. | 

## Vỏ cạnh 

Đối với trường hợp biên thứ nhất:```
1
4
```Thuật toán lấy tập hợp`{2}`đối với khóa 1. Kiểm tra thành viên hỏi xem 4 có tồn tại trong bộ đó hay không, điều này sai nên in ra`CURSE YOU`. Điều này xử lý trường hợp một khóa chỉ có một khóa có thể và tránh việc chấp nhận các khóa không liên quan. 

Đối với khóa cuối cùng:```
5
4
```Thuật toán truy xuất`{4}`cho khóa 5. Giá trị 4 được tìm thấy ngay lập tức, do đó đầu ra là`GOOD LUCK AGENT P`. Ánh xạ cố định ngăn chặn mọi nỗ lực nhìn xa hơn phạm vi khóa có sẵn. 

Đối với các số phù hợp:```
3
3
```Thuật toán truy xuất`{2, 4}`cho khóa 3. Vì không có 3 nên nó sẽ in`CURSE YOU`. Điều này xác nhận rằng giải pháp kiểm tra tính tương thích thực tế thay vì so sánh trực tiếp hai số đầu vào. 

Tôi cũng có thể điều chỉnh bài xã luận này thành một phiên bản ngắn hơn theo phong cách Codeforces nếu bạn muốn một cái gì đó gần với lời giải thích thực tế hơn về việc gửi bài dự thi.
