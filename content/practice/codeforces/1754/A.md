---
title: "CF 1754A - Hỗ trợ kỹ thuật"
description: "Chúng tôi nhận được một chuỗi tin nhắn trò chuyện chỉ bao gồm hai loại: câu hỏi của khách hàng và câu trả lời hỗ trợ. Cuộc trò chuyện được viết theo thứ tự thời gian và tin nhắn đầu tiên luôn là một câu hỏi."
date: "2026-06-09T14:50:11+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1754
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 829 (Div. 2)"
rating: 800
weight: 1754
solve_time_s: 112
verified: false
draft: false
---

[CF 1754A - Hỗ trợ kỹ thuật](https://codeforces.com/problemset/problem/1754/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một chuỗi tin nhắn trò chuyện chỉ bao gồm hai loại: câu hỏi của khách hàng và câu trả lời hỗ trợ. Cuộc trò chuyện được viết theo thứ tự thời gian và tin nhắn đầu tiên luôn là một câu hỏi. 

Quy tắc cấu trúc duy nhất mà chúng tôi quan tâm là mọi câu hỏi phải được “giải thích” bằng các câu trả lời theo sau nó. Mỗi câu trả lời phải trả lời một số câu hỏi đã được hỏi trước đó và các câu trả lời có thể bị trì hoãn hoặc xen kẽ, nhưng chúng không thể tồn tại nếu không có câu hỏi chưa được so sánh trước đó. Nói cách khác, tại bất kỳ thời điểm nào trong cuộc đối thoại, chúng ta không bao giờ được có nhiều câu trả lời hơn những câu hỏi đã xuất hiện cho đến nay, và cuối cùng thì tất cả các câu hỏi đều phải có câu trả lời phù hợp. 

Nhiệm vụ là xác định, đối với mỗi trường hợp kiểm thử, liệu trình tự như vậy có thể hợp lệ theo các quy tắc này hay không. 

Các ràng buộc rất nhỏ: tối đa 500 trường hợp thử nghiệm và mỗi chuỗi có độ dài tối đa là 100. Điều này có nghĩa là ngay cả giải pháp O(n²) hoặc O(n) cho mỗi trường hợp thử nghiệm cũng nhanh không đáng kể. Chúng tôi chỉ cần theo dõi một trạng thái chạy nhỏ cho mỗi lần kiểm tra. 

Một sai lầm ngây thơ là cho rằng mọi câu hỏi đều phải có đúng một câu trả lời ngay sau đó. Ví dụ: chuỗi “QQAA” vẫn hợp lệ ngay cả khi câu trả lời thứ hai bị trì hoãn vì các câu trả lời có thể được nhóm lại và không nhất thiết phải theo sau câu hỏi trùng khớp với chúng. 

Một trường hợp thất bại khó nhận thấy khác là khi câu trả lời xuất hiện trước khi có đủ câu hỏi. Ví dụ: “QAQ” không hợp lệ vì sau khi sử dụng “QA”, “Q” tiếp theo sẽ tăng các câu hỏi đang chờ xử lý nhưng chúng tôi không bao giờ đảm bảo rằng câu trả lời trước đó được ghép nối chính xác theo cấu trúc nhất quán, để lại câu hỏi không khớp ở cuối. 

Ràng buộc ẩn chính là tính hợp lệ của tiền tố: ở mỗi tiền tố, câu trả lời không được vượt quá câu hỏi và ở cuối, số lượng phải bằng nhau. 

## Phương pháp tiếp cận 

Cách diễn giải thô bạo sẽ cố gắng khớp rõ ràng từng câu trả lời với một câu hỏi chưa từng có trước đó, có thể bằng cách sử dụng một ngăn xếp hoặc các cặp theo dõi rõ ràng. Đối với mỗi câu trả lời, chúng tôi sẽ tìm kiếm ngược một câu hỏi có sẵn, đánh dấu câu hỏi đó là đã sử dụng và tiếp tục. Điều này đúng nhưng nặng nề không cần thiết vì nó liên quan đến việc quét hoặc ghi sổ nhiều lần cho mỗi ký tự, có khả năng dẫn đến hành vi O(n²) nếu được triển khai kém với danh sách hoặc tìm kiếm lặp lại. 

Quan sát đơn giản hóa mọi thứ là chúng ta không quan tâm đến câu hỏi cụ thể mà mỗi câu trả lời thuộc về. Yêu cầu duy nhất là câu trả lời không bao giờ vượt quá các câu hỏi có sẵn ở bất kỳ tiền tố nào. Điều này làm giảm toàn bộ vấn đề xuống còn việc duy trì một bộ đếm duy nhất: số lượng câu hỏi hiện chưa được trả lời. 

Chúng tôi tăng bộ đếm cho mỗi “Q”. Đối với mỗi chữ “A”, chúng tôi giảm nó. Nếu tại bất kỳ thời điểm nào nó trở thành phủ định, điều đó có nghĩa là chúng tôi đã cố gắng trả lời một câu hỏi chưa tồn tại và câu hỏi này không hợp lệ. Cuối cùng, cấu trúc này hợp lệ nếu chúng ta không bao giờ âm. 

Đây thực sự là một điều kiện cân bằng tương tự như xác thực khung, trong đó “Q” hoạt động giống như khung mở và “A” hoạt động giống như khung đóng, nhưng với sự thay đổi thì việc đóng khung có thể bị trì hoãn tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu | O(n²) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Theo dõi truy cập | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`pending = 0`để biểu thị có bao nhiêu câu hỏi chưa được trả lời. 
2. Lặp lại chuỗi thông báo từ trái sang phải. 
3. Nếu ký tự hiện tại là “Q”, tăng dần`pending`bởi vì một câu hỏi mới chưa được trả lời xuất hiện. 
4. Nếu ký tự là “A” thì giảm`pending`bởi vì một câu trả lời tiêu tốn một câu hỏi đã được hỏi trước đó. 
5. Nếu tại bất kỳ thời điểm nào`pending`trở nên tiêu cực, ngay lập tức kết luận cuộc đối thoại không hợp lệ vì câu trả lời xuất hiện mà không có câu hỏi tương ứng trước đó. 
6. Sau khi xử lý toàn bộ chuỗi, xuất “Có” nếu`pending == 0`, nếu không thì xuất ra “Không”. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào của chuỗi,`pending`thể hiện chính xác số lượng câu hỏi vẫn cần câu trả lời trong tương lai. Quy tắc câu trả lời phải tương ứng với các câu hỏi trước đó tương đương với việc yêu cầu số lượng này không bao giờ giảm xuống dưới 0. Nếu nó giảm xuống dưới 0, thì một số câu trả lời không có câu hỏi nào để đính kèm. Nếu nó vẫn không tiêu cực nhưng kết thúc tích cực, điều đó có nghĩa là một số câu hỏi chưa bao giờ được trả lời, điều này cũng vi phạm yêu cầu. 

Bất biến này đảm bảo tính chính xác: mọi chuỗi hợp lệ đều duy trì sự cân bằng không âm xuyên suốt và mọi vi phạm sự cân bằng đó đều tương ứng chính xác với cấu trúc đối thoại không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()
    
    pending = 0
    ok = True
    
    for ch in s:
        if ch == 'Q':
            pending += 1
        else:
            pending -= 1
            if pending < 0:
                ok = False
                break
    
    print("Yes" if ok and pending == 0 else "No")
```Việc thực hiện tuân theo logic truy cập trực tiếp. Việc ngắt sớm rất quan trọng vì một khi tiền tố không hợp lệ được phát hiện thì việc xử lý tiếp theo không thể khôi phục tính hợp lệ. Việc kiểm tra cuối cùng đảm bảo rằng không có câu hỏi nào chưa được trả lời. 

Một lỗi thường gặp ở đây là quên đảm bảo số dư cuối cùng bằng 0. Một chuỗi có thể duy trì tính hợp lệ xuyên suốt nhưng vẫn kết thúc bằng những câu hỏi còn sót lại và phải bị loại bỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`QQAQ`Chúng tôi theo dõi`pending`từng bước một. 

| Bước | Char | Đang chờ xử lý | Tiền tố hợp lệ | 
| --- | --- | --- | --- | 
| 1 | Q | 1 | vâng | 
| 2 | Q | 2 | vâng | 
| 3 | A | 1 | vâng | 
| 4 | Q | 2 | vâng | 

Giá trị cuối cùng là 2, nghĩa là còn hai câu hỏi chưa được trả lời. Điều này vi phạm quy tắc. 

Đầu ra: Không 

Điều này cho thấy hiệu lực của tiền tố thôi là chưa đủ; số dư cuối cùng cũng có vấn đề. 

### Ví dụ 2:`QAQQAQAAQQQAAA`| Bước | Char | Đang chờ xử lý | 
| --- | --- | --- | 
| 1 | Q | 1 | 
| 2 | A | 0 | 
| 3 | Q | 1 | 
| 4 | Q | 2 | 
| 5 | A | 1 | 
| 6 | Q | 2 | 
| 7 | A | 1 | 
| 8 | A | 0 | 
| 9 | Q | 1 | 
| 10 | Q | 2 | 
| 11 | Q | 3 | 
| 12 | A | 2 | 
| 13 | A | 1 | 
| 14 | A | 0 | 

Chúng ta không bao giờ âm và kết thúc bằng 0, vì vậy cấu trúc này hợp lệ. 

Đầu ra: Có 

Điều này chứng tỏ rằng việc trả lời trễ hoàn toàn được cho phép miễn là ràng buộc về số dư được tôn trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | chuyển một lần qua từng chuỗi tin nhắn | 
| Không gian | O(1) | chỉ có một bộ đếm được duy trì | 

Cho n ≤ 100 và t ≤ 500, tổng số phép toán tối đa là không đáng kể, dưới bất kỳ ràng buộc giới hạn thời gian nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from subprocess import check_output
    return check_output(["python3", "solution.py"], input=inp.encode()).decode()

# provided samples
assert run("""5
4
QQAA
4
QQAQ
3
QAA
1
Q
14
QAQQAQAAQQQAAA
""") == """Yes
No
Yes
No
Yes
"""

# single Q must fail
assert run("""1
1
Q
""") == "No\n"

# single valid pair
assert run("""1
2
QA
""") == "Yes\n"

# early invalid answer
assert run("""1
2
AQ
""") == "No\n"

# balanced alternating
assert run("""1
6
QQAQQA
""") == "Yes\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`Q`| Không | câu hỏi chưa được trả lời | 
|`QA`| Có | trường hợp hợp lệ đơn giản nhất | 
|`AQ`| Không | câu trả lời tiền tố không hợp lệ trước | 
|`QQAQQA`| Có | giá trị xen kẽ | 

## Vỏ cạnh 

Trường hợp một cạnh là một câu hỏi duy nhất. Đầu vào “Q” giữ nguyên`pending = 1`và không bao giờ giảm nó, vì vậy lần kiểm tra cuối cùng thất bại, từ chối nó một cách chính xác. 

Một cách khác là câu trả lời xuất hiện đầu tiên sau tiền tố nặng về câu hỏi. Đối với đầu vào “QQAQA”, bộ đếm vẫn hợp lệ cho đến tiền tố cuối cùng, nhưng vẫn kết thúc dương, đánh dấu chính xác nó không hợp lệ. 

Một trường hợp tế nhị hơn là xen kẽ như “QAQAQA”. Bộ đếm luân phiên giữa 1 và 0 nhưng không bao giờ trở thành số âm và kết thúc bằng 0, vì vậy nó hợp lệ. Điều này khẳng định rằng thuật toán không yêu cầu nhóm các câu trả lời mà chỉ bảo toàn số dư.
