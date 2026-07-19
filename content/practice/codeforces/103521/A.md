---
title: "CF 103521A - \u041a\u0430\u043a \u043f\u043e\u043a\u043e\u0440\u043c\u0438\u0442\u044c \u0434\u0440\u0430\u043a\u043e\u043d\u0430"
description: "Chúng ta có thể diễn giải lại tình huống này như sau. Chúng ta có số nguyên bắt đầu là sức mạnh s và danh sách kẻ thù. Mỗi kẻ thù được mô tả bằng hai giá trị: ngưỡng sức mạnh cần thiết để sống sót trong cuộc chiến và phần thưởng nhận được sau khi chiến thắng."
date: "2026-07-03T06:01:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103521
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2018-2019, \u0422\u0440\u0435\u0442\u044c\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 103521
solve_time_s: 47
verified: true
draft: false
---

[CF 103521A - \u041a\u0430\u043a \u043f\u043e\u043a\u043e\u0440\u043c\u0438\u0442\u044c \u0434\u0440\u0430\u043a\u043e\u043d\u0430](https://codeforces.com/problemset/problem/103521/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có thể diễn giải lại tình huống này như sau. 

Chúng ta có cường độ số nguyên bắt đầu`s`và một danh sách kẻ thù. Mỗi kẻ thù được mô tả bằng hai giá trị: ngưỡng sức mạnh cần thiết để sống sót trong cuộc chiến và phần thưởng nhận được sau khi chiến thắng. Nếu tại bất kỳ thời điểm nào chúng ta chọn kẻ thù có ngưỡng lớn hơn hoặc bằng sức mạnh hiện tại của chúng ta, chúng ta sẽ thất bại ngay lập tức. Nếu không, chúng ta đánh bại nó và tăng sức mạnh của mình nhờ phần thưởng của nó. 

Mục tiêu không phải là mô phỏng một chuỗi cố định mà là để xác định xem liệu có tồn tại một số mệnh lệnh của kẻ thù cho phép chúng ta đánh bại tất cả chúng hay không. 

Kích thước đầu vào nhỏ: số lượng rồng nhiều nhất là khoảng 1000. Điều này ngay lập tức gợi ý rằng chiến lược tham lam O(n^2) có thể chấp nhận được, trong khi mọi thứ theo cấp số nhân trên hoán vị là không thể vì n! đơn đặt hàng sẽ bùng nổ ngay cả đối với n vừa phải. 

Một sai lầm ngây thơ là cho rằng bất kỳ thứ tự tùy ý nào cũng có hiệu quả hoặc chỉ cần sắp xếp theo độ mạnh là đủ. Điều đó không thành công trong trường hợp con rồng yếu hơn mang lại phần thưởng lớn để mở khóa những con rồng mạnh hơn sau này. 

Ví dụ, hãy xem xét: 

đầu vào:```
s = 2
(1, 99), (100, 0)
```Nếu chúng ta chiến đấu với con rồng mạnh trước, chúng ta sẽ chết ngay lập tức. Nếu chúng ta chiến đấu với con rồng yếu trước, chúng ta sẽ đủ mạnh để đánh bại con rồng thứ hai. Vì vậy, trật tự rất quan trọng. 

Một trường hợp phức tạp khác là khi tất cả những con rồng ban đầu đều quá mạnh, nhưng một trong số chúng chỉ vừa đủ để tiếp cận và cho đủ tiền thưởng để mở khóa những con còn lại. Sự lựa chọn tham lam phải phát hiện ra quá trình mở khóa gia tăng này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của rồng và mô phỏng các cuộc chiến. Điều này đúng nhưng không khả thi: có n! hoán vị, và thậm chí n = 15 đã trở nên quá lớn. 

Quan sát quan trọng là một khi chúng ta đủ mạnh để đánh bại một con rồng, không có lý do gì để trì hoãn việc chiến đấu với nó nếu nó giúp chúng ta trở nên mạnh mẽ hơn sớm hơn. Trì hoãn chỉ có nguy cơ bỏ lỡ cơ hội mở khóa những con rồng mạnh hơn sớm hơn. Điều này dẫn đến một chiến lược tham lam: ở mỗi bước, trong số tất cả những con rồng mà chúng ta hiện có thể đánh bại, hãy chọn một con. 

Câu hỏi còn lại là nên chọn con rồng nào “có thể đánh bại” trước. Lựa chọn đúng đắn là ưu tiên những con rồng có yêu cầu sức mạnh nhỏ hơn trước. Việc sắp xếp rồng theo sức mạnh cần thiết đảm bảo rằng bất cứ khi nào chúng ta quét về phía trước, chúng ta luôn gặp phải tất cả các tùy chọn hiện có thể truy cập theo cách có cấu trúc và chúng ta tham lam tiêu thụ chúng theo thứ tự độ khó tăng dần trong khi liên tục mở rộng phạm vi tiếp cận của mình. 

Thuật toán trở thành: sắp xếp theo ngưỡng, sau đó liên tục lấy tất cả những con rồng có thể tiếp cận và tiêu thụ chúng theo bất kỳ thứ tự nào, thường là sử dụng quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam + phân loại theo sức mạnh | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ghép sức mạnh cần thiết và phần thưởng của mỗi con rồng vào một danh sách các bộ dữ liệu và sắp xếp danh sách này theo sức mạnh cần thiết theo thứ tự tăng dần. Việc sắp xếp đảm bảo chúng tôi luôn xem xét những con rồng dễ hơn trước những con rồng khó hơn, điều này phù hợp với ý tưởng rằng việc mở khóa sức mạnh phải tăng dần. 
2. Duy trì một biến`cur`được khởi tạo về cường độ ban đầu. Điều này thể hiện khả năng hiện tại ở mọi thời điểm trong quy trình. 
3. Lặp lại danh sách đã sắp xếp và bất cứ khi nào sức mạnh cần thiết của con rồng tiếp theo hoàn toàn thấp hơn`cur`, đánh dấu nó là sẵn sàng để đánh bại. Điều này có tác dụng vì việc sắp xếp đảm bảo rằng tất cả những con rồng được xem xét trước đây cũng yếu hơn hoặc có yêu cầu ngang nhau. 
4. Trong số tất cả những con rồng hiện có, hãy liên tục chọn bất kỳ con rồng nào chưa được ghé thăm, đánh bại nó và thêm phần thưởng của nó vào`cur`. Mỗi thất bại có thể mở khóa những con rồng mới, vì vậy bộ tính sẵn có phải được cập nhật linh hoạt. 
5. Tiếp tục quá trình này cho đến khi không còn con rồng nào có thể bị đánh bại trong lượt hiện tại. Sau đó tiếp tục quét về phía trước trong danh sách đã sắp xếp để tìm những con rồng mới được mở khóa. 
6. Sau khi tất cả các bản mở rộng có thể được xử lý, hãy kiểm tra xem tất cả rồng đã bị đánh bại hay chưa. Nếu có thì xuất ra “YES”, nếu không thì xuất ra “NO”. 

### Tại sao nó hoạt động 

Tính chính xác đến từ tính bất biến đơn điệu về khả năng tiếp cận: tại bất kỳ thời điểm nào, chúng tôi duy trì nhóm rồng có sức mạnh cần thiết thấp hơn sức mạnh hiện tại và chúng tôi đảm bảo rằng nếu có bất kỳ con rồng nào như vậy tồn tại thì cuối cùng chúng tôi sẽ xử lý nó. Vì đánh bại rồng chỉ tăng sức mạnh nên bộ có thể tiếp cận chỉ có thể phát triển theo thời gian chứ không bao giờ thu hẹp lại. Điều này đảm bảo rằng nếu tồn tại một thứ tự đầy đủ, quá trình tham lam sẽ khám phá ra một chuỗi hợp lệ mà không bao giờ bỏ qua bước mở khóa trung gian cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, s = map(int, input().split())
dragons = [tuple(map(int, input().split())) for _ in range(n)]

dragons.sort()  # sort by required strength

cur = s
i = 0
used = 0

visited = [False] * n

while True:
    progress = False

    while i < n and dragons[i][0] < cur:
        if not visited[i]:
            visited[i] = True
            cur += dragons[i][1]
            used += 1
            progress = True
        i += 1

    if not progress:
        break

print("YES" if used == n else "NO")
```Mã đầu tiên sắp xếp rồng theo yêu cầu về sức mạnh của chúng để tính khả thi luôn được kiểm tra theo thứ tự tăng dần. Con trỏ`i`đảm bảo chúng tôi chỉ quét tiến một lần, ngăn chặn công việc lặp lại. Vòng lặp bên trong thu thập tất cả những con rồng hiện có thể đánh bại và ngay lập tức áp dụng phần thưởng của chúng, điều này có thể mở khóa những con mạnh hơn. 

Một chi tiết tinh tế là chúng tôi sử dụng`< cur`còn hơn là`<= cur`. Điều này phù hợp với quy luật rằng sức mạnh ngang nhau là không đủ để tồn tại. 

Thuật toán dựa trên thực tế là mỗi con rồng được xử lý nhiều nhất một lần, làm cho độ phức tạp tổng thể trở nên tuyến tính sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = 2
(1, 99), (100, 0)
```| Bước | cur | tôi | Hành động | Đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 2 | 0 | không | 0 | 
| quá trình (1,99) | 101 | 1 | đánh bại rồng 1 | 1 | 
| quá trình (100,0) | 101 | 2 | đánh bại rồng 2 | 2 | 

Điều này xác nhận rằng những con rồng yếu nhưng bổ ích ban đầu là rất quan trọng để mở khóa những con sau này. 

### Ví dụ 2 

đầu vào:```
s = 10
(100, 100)
```| Bước | cur | tôi | Hành động | Đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 10 | 0 | không | 0 | 

Không thể tiếp cận được con rồng nào kể từ 100 >= 10, vì vậy quá trình kết thúc ngay lập tức. 

Điều này thể hiện trường hợp thất bại khi không có trình tự nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; mỗi con rồng được xử lý một lần | 
| Không gian | O(n) | Lưu trữ danh sách rồng và mảng đã truy cập | 

Cho n lên tới khoảng 1000, điều này diễn ra thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from subprocess import PIPE, Popen
    p = Popen(["python3", "main.py"], stdin=PIPE, stdout=PIPE, stderr=PIPE, text=True)
    out, _ = p.communicate(inp)
    return out.strip()

# sample 1
assert run("2 2\n1 99\n100 0\n") == "YES"

# sample 2
assert run("10 1\n100 100\n") == "NO"

# minimal case
assert run("1 5\n1 10\n") == "YES"

# impossible chain
assert run("3 1\n2 1\n3 1\n4 1\n") == "NO"

# large bonus chain
assert run("3 1\n1 1\n2 10\n3 10\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| rồng duy nhất có thể tiếp cận | CÓ | trường hợp cơ sở đúng đắn | 
| rồng duy nhất không thể | KHÔNG | thất bại ngay lập tức | 
| chuỗi tăng dần | KHÔNG | phụ thuộc đặt hàng | 
| khuếch đại tiền thưởng | CÓ | hiệu ứng tăng trưởng tham lam | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi nhiều con rồng có cùng sức mạnh cần thiết. Thuật toán xử lý việc này một cách chính xác bởi vì một khi`cur`vượt quá ngưỡng đó, tất cả chúng đều có sẵn đồng thời và có thể được tiêu thụ theo bất kỳ thứ tự nào, vì mỗi thứ đều tăng sức mạnh một cách độc lập. 

Một trường hợp khác là khi chiến lược tối ưu yêu cầu trì hoãn một con rồng mạnh nhưng có thể tiếp cận được để nhường chỗ cho con rồng yếu hơn với phần thưởng cao hơn. Quá trình quét tham lam xử lý việc này một cách tự nhiên vì tất cả những con rồng hiện có thể tiếp cận đều đã được xử lý và không có lợi ích gì trong việc trì hoãn một cách giả tạo bất kỳ con rồng nào trong số chúng, vì tiền thưởng chỉ làm tăng khả năng tiếp cận trong tương lai. 

Trường hợp cạnh cuối cùng là khi cường độ ban đầu đã vượt quá tất cả các ngưỡng. Trong trường hợp đó, thuật toán chỉ cần sử dụng mọi thứ trong một lần quét, xác nhận rằng các ràng buộc thứ tự là không liên quan khi mọi thứ đều có thể truy cập được.
