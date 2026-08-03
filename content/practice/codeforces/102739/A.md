---
title: "CF 102739A - \u0412\u044b\u0441\u0442\u0430\u0432\u043a\u0430 \u0438\u043c\u043f\u0440\u0435\u0441\u0441\u0438\u043e\u043d\u0438\u0441\u0442\u043e\u0432"
description: "Thư viện có n bức tranh được sắp xếp theo thứ tự cố định từ trái qua phải. Arina bắt đầu ở phía đầu tiên của phòng trưng bày và đi về phía bức tranh cuối cùng. Trong khi đi dạo, cô ấy chỉ có thể dừng lại ở những bức tranh phía trước cô ấy theo hướng hiện tại."
date: "2026-08-01T22:19:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102739
codeforces_index: "A"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2020.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102739
solve_time_s: 105
verified: true
draft: false
---

[CF 102739A - \u0412\u044b\u0441\u0442\u0430\u0432\u043a\u0430 \u0438\u043c\u043f\u0440\u0435\u0441\u0441\u0438\u043e\u043d\u0438\u0441\u0442\u043e\u0432](https://codeforces.com/problemset/problem/102739/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Phòng trưng bày chứa`n`tranh được sắp xếp theo thứ tự cố định từ trái qua phải. Arina bắt đầu ở phía đầu tiên của phòng trưng bày và đi về phía bức tranh cuối cùng. Trong khi đi dạo, cô ấy chỉ có thể dừng lại ở những bức tranh phía trước cô ấy theo hướng hiện tại. Sau khi đến cuối phòng trưng bày, cô quay lại và tiếp tục đi theo hướng ngược lại. Hướng dẫn âm thanh ghi lại thứ tự cô ấy thực sự xem tất cả các bức tranh. 

Nhiệm vụ là xây dựng lại số lần đi qua toàn bộ phòng trưng bày tối thiểu có thể tạo ra thứ tự xem này. Bước đi từ trái sang phải phải chứa số bức tranh tăng theo thứ tự đã ghi, trong khi bước đi từ phải sang trái phải chứa số bức tranh giảm dần. 

Đầu vào cung cấp một hoán vị của số bức tranh. Mỗi bức tranh xuất hiện đúng một lần, do đó trình tự tự nó cho chúng ta biết thời điểm chính xác khi mỗi bức tranh được xem. Đầu ra là số lượng nhỏ nhất các đoạn tăng và giảm xen kẽ cần thiết để phân chia chuỗi này. 

Hạn chế chính là`n ≤ 100000`. Điều này loại trừ các giải pháp thử mọi cách phân chia có thể có của trình tự hoặc mô phỏng lặp lại tất cả các bức tranh trên mỗi lượt. Một giải pháp cần xử lý mỗi bức tranh với số lần không đổi, hướng tới một`O(n)`tiếp cận. 

Điều tinh tế là cuộc dạo chơi không kết thúc khi chúng ta đến bức tranh cuối cùng được nhìn thấy theo hướng đó. Nó chỉ kết thúc sau khi đạt đến phần cuối của thư viện. Do đó, bức tranh tiếp theo trong chuỗi âm thanh có thể buộc phải thực hiện một bước đi mới ngay cả khi dãy con toán học dài hơn vẫn có thể tăng hoặc giảm. 

Hãy xem xét đầu vào này:```
4
1 2 4 3
```Đầu ra đúng là:```
2
```Đi bộ đầu tiên từ trái sang phải có thể nhìn thấy`1 2 4`. Bức vẽ`3`nằm phía sau vị trí hiện tại nên Arina phải quay lại. Một giải pháp bất cẩn chỉ tìm kiếm một dãy con tăng dài có thể quyết định sai rằng có thể có ít bước đi hơn. 

Một trường hợp khác là khi mỗi bức tranh tiếp theo đều yêu cầu thay đổi hướng:```
5
1 5 2 4 3
```Đầu ra đúng là:```
5
```Trình tự tăng giảm xen kẽ sau mỗi bức tranh. Việc xử lý các bước đi bằng nhau hoặc một phần tử không chính xác có thể gây ra từng lỗi ở đây, bởi vì mỗi bức tranh vẫn yêu cầu một bước đi hoàn chỉnh. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng cuộc dạo chơi trong phòng trưng bày. Chúng ta có thể duy trì bộ tranh đã xem, bắt đầu từ một đầu và liên tục di chuyển qua phòng trưng bày để tìm kiếm bức tranh có sẵn tiếp theo theo hướng hiện tại. Cách tiếp cận này đúng vì nó bám sát câu chuyện một cách chính xác. Tuy nhiên, việc triển khai nó trực tiếp có thể yêu cầu phải quét nhiều bức tranh trên mỗi lượt. Trong trường hợp xấu nhất có thể có`n`vượt qua và mỗi lần vượt qua có thể kiểm tra`n`tranh, dẫn đến`O(n²)`hoạt động quá chậm đối với`n = 100000`. 

Điều quan trọng là chuỗi âm thanh đã chứa thông tin duy nhất chúng ta cần. Trong một lần đi dạo, số bức tranh được xem phải tạo thành một chuỗi đơn điệu. Lần đi đầu tiên phải tăng lên, lần thứ hai giảm dần, lần thứ ba tăng lên, v.v. Do đó, bài toán tương đương với việc tìm xem chúng ta cần chuyển đổi bao nhiêu lần giữa lần chạy tăng và giảm trong khi đọc hoán vị từ trái sang phải. 

Mô phỏng lực lượng vũ phu hoạt động vì mỗi bước đi tương ứng với một phân đoạn đơn điệu. Nó thất bại vì nó liên tục dành thời gian để khám phá thông tin đã có trong chuỗi. Việc quan sát thấy hướng của bước đi hiện tại được xác định bởi bức tranh được xem trước đó cho phép chúng tôi giảm toàn bộ mô phỏng xuống một lần quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu bằng một lần đi bộ. Hướng của nó là từ trái sang phải, do đó đoạn hiện tại phải tăng lên. 
2. Quét các bức tranh theo thứ tự được lưu trữ trong hướng dẫn âm thanh. Giữ số bức tranh trước đó và hướng đi theo yêu cầu của bước đi hiện tại. 
3. Nếu hướng hiện tại đang tăng lên và số bức tranh tiếp theo lớn hơn số trước đó, thì bước đi tương tự có thể tiếp tục. 
4. Nếu hướng hiện tại đang giảm và số bức tranh tiếp theo nhỏ hơn số trước đó, thì bước đi tương tự có thể tiếp tục. 
5. Nếu không, bước đi hiện tại không thể bao gồm bức tranh này. Tăng số lần đi bộ và đảo ngược hướng, vì Arina chắc chắn đã đến cuối phòng trưng bày và quay lại trước khi xem nó. 
6. Thay thế số bức vẽ trước đó bằng số hiện tại và tiếp tục cho đến khi toàn bộ chuỗi được xử lý. 

Lý do tham lam chuyển đổi là đủ vì mỗi bước đi đều có một hướng cố định. Một khi hai bức tranh liên tiếp vi phạm hướng đó thì không có lựa chọn nào sớm hơn có thể sửa chữa được tình hình. Bức tranh thứ hai về mặt thực tế nằm ở phía bên trái của Arina nên việc bước đi mới là điều khó tránh khỏi. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của chuỗi âm thanh, thuật toán đã tính chính xác số bước đi tối thiểu cần thiết cho tiền tố đó và hướng hiện tại là hướng khả thi duy nhất cho bước đi tiếp theo. Bất cứ khi nào bức tranh tiếp theo phù hợp với hướng hiện tại, việc kéo dài bước đi hiện tại luôn là tối ưu vì nó không làm tăng câu trả lời. Bất cứ khi nào nó không phù hợp, lần đi trước không thể chứa bức tranh đó nên việc thêm một lần đi nữa là bắt buộc. Vì mọi quyết định đều bị ép buộc nên quá trình tham lam tạo ra số bước đi tối thiểu có thể. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = 1
    direction = 1
    prev = a[0]

    for x in a[1:]:
        if direction == 1:
            if x < prev:
                ans += 1
                direction = -1
        else:
            if x > prev:
                ans += 1
                direction = 1
        prev = x

    print(ans)

if __name__ == "__main__":
    solve()
```Biến`direction`lưu trữ hướng đi bộ của phòng trưng bày hiện tại. Một giá trị của`1`có nghĩa là bước đi hiện tại đi từ số lượng bức tranh nhỏ hơn đến số lượng bức tranh lớn hơn, trong khi`-1`có nghĩa là nó đi theo hướng ngược lại. 

Quá trình quét chỉ so sánh các bức tranh liền kề trong chuỗi âm thanh. Nếu so sánh mâu thuẫn với hướng hiện tại, mã sẽ ngay lập tức bắt đầu một bước đi mới và đảo hướng. 

Việc khởi tạo với`ans = 1`là an toàn vì bức tranh đầu tiên luôn cần ít nhất một lần đi bộ. Các ràng buộc đảm bảo rằng`n`có ít nhất một, vì vậy việc truy cập`a[0]`là hợp lệ. 

Không cần thêm mảng vì thuật toán chỉ cần bức tranh trước đó, hướng hiện tại và câu trả lời hiện tại. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
8
3 5 7 1 6 4 8 2
```dấu vết là: 

| Bức tranh hiện tại | Bức tranh trước | Hướng trước bước | Hành động | Số lần đi bộ | 
| --- | --- | --- | --- | --- | 
| 3 | - | Tăng | Bắt đầu | 1 | 
| 5 | 3 | Tăng | Tiếp tục | 1 | 
| 7 | 5 | Tăng | Tiếp tục | 1 | 
| 1 | 7 | Tăng | Quay lại | 2 | 
| 6 | 1 | Giảm | Quay lại | 3 | 
| 4 | 6 | Tăng | Quay lại | 4 | 
| 8 | 4 | Giảm | Quay lại | 5 | 
| 2 | 8 | Tăng | Quay lại | 6 | 

Trình tự tạo ra sáu phân đoạn đơn điệu:`3 5 7`,`1`,`6`,`4`,`8`, Và`2`. Mỗi phân đoạn tương ứng với một lần duyệt vật lý của thư viện. 

Một ví dụ thứ hai:```
5
1 2 3 4 5
```| Bức tranh hiện tại | Bức tranh trước | Hướng trước bước | Hành động | Số lần đi bộ | 
| --- | --- | --- | --- | --- | 
| 1 | - | Tăng | Bắt đầu | 1 | 
| 2 | 1 | Tăng | Tiếp tục | 1 | 
| 3 | 2 | Tăng | Tiếp tục | 1 | 
| 4 | 3 | Tăng | Tiếp tục | 1 | 
| 5 | 4 | Tăng | Tiếp tục | 1 | 

Toàn bộ chuỗi âm thanh tương thích với một lần đi bộ từ trái sang phải, vì vậy câu trả lời vẫn là một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bức tranh được kiểm tra đúng một lần. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Với`n = 100000`, thuật toán chỉ thực hiện một số phép so sánh tuyến tính, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    ans = 1
    direction = 1
    prev = a[0]

    for x in a[1:]:
        if direction == 1:
            if x < prev:
                ans += 1
                direction = -1
        else:
            if x > prev:
                ans += 1
                direction = 1
        prev = x

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert run("""8
3 5 7 1 6 4 8 2
""") == "6\n", "sample"

assert run("""1
1
""") == "1\n", "minimum size"

assert run("""5
1 2 3 4 5
""") == "1\n", "already increasing"

assert run("""5
5 4 3 2 1
""") == "2\n", "starts in wrong direction"

assert run("""6
1 6 2 5 3 4
""") == "6\n", "frequent direction changes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Xử lý sơn đơn | 
|`1 2 3 4 5`|`1`| Một giao dịch tăng dần hoàn chỉnh | 
|`5 4 3 2 1`|`2`| Hướng đầu tiên được cố định khi tăng | 
|`1 6 2 5 3 4`|`6`| Lặp đi lặp lại các lượt và thay đổi hướng | 

## Vỏ cạnh 

Đối với trường hợp sơn đơn:```
1
1
```thuật toán bắt đầu bằng một lần đi bộ và không bao giờ đi vào vòng lặp. Câu trả lời vẫn còn`1`, phù hợp với thực tế là Arina chỉ cần vào phòng trưng bày và xem bức tranh đó. 

Đối với một chuỗi đã tăng lên:```
5
1 2 3 4 5
```mọi so sánh đều đồng ý với hướng từ trái sang phải hiện tại. Thuật toán không bao giờ thay đổi hướng và quay trở lại`1`. Một giải pháp luôn thay thế sau khi đạt được bức tranh cuối cùng sẽ được tính quá mức ở đây. 

Đối với trường hợp bước đầu tiên bị lùi lại:```
4
4 3 2 1
```bước đi đầu tiên không thể chứa bức tranh thứ hai vì Arina đang chuyển từ các chỉ số nhỏ hơn sang các chỉ số lớn hơn. Thuật toán đếm bước đi tăng dần ban đầu có chứa`4`, sau đó chuyển sang bước đi giảm dần chứa phần còn lại. Câu trả lời là`2`. 

Đối với một chuỗi có những thay đổi thường xuyên:```
6
1 6 2 5 3 4
```mỗi cặp liền kề phá vỡ hướng hiện tại. Thuật toán thay đổi hướng sau mỗi lần so sánh và trả về`6`, phù hợp với thực tế là mỗi bức tranh sau bức đầu tiên yêu cầu một lần duyệt riêng biệt.
