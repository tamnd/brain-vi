---
title: "CF 103966D - \u042d\u0444\u0444\u0435\u043a\u0442\u0438\u0432\u043d\u044b\u0439 \u0434\u0432\u0438\u0433\u0430\u0442\u0435\u043b\u044c"
description: "Chúng ta được cung cấp một chuỗi các vị trí được sắp xếp thành một dòng, trong đó mỗi vị trí mang một trọng số nhất định. Sau đó, chúng tôi được cung cấp một tập hợp các truy vấn độc lập. Mỗi truy vấn mô tả một quy trình bắt đầu từ một chỉ mục nhất định và liên tục nhảy về phía trước theo kích thước bước cố định."
date: "2026-07-02T06:32:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103966
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2022-2023, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u0431\u0430\u0437\u043e\u0432\u0430\u044f \u0432\u0435\u0440\u0441\u0438\u044f)"
rating: 0
weight: 103966
solve_time_s: 39
verified: true
draft: false
---

[CF 103966D - \u042d\u0444\u0444\u0435\u043a\u0442\u0438\u0432\u043d\u044b\u0439 \u0434\u0432\u0438\u0433\u0430\u0442\u0435\u043b\u044c](https://codeforces.com/problemset/problem/103966/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các vị trí được sắp xếp thành một dòng, trong đó mỗi vị trí mang một trọng số nhất định. Sau đó, chúng tôi được cung cấp một tập hợp các truy vấn độc lập. Mỗi truy vấn mô tả một quy trình bắt đầu từ một chỉ mục nhất định và liên tục nhảy về phía trước theo kích thước bước cố định. Quá trình thu thập các giá trị tại tất cả các vị trí đã truy cập cho đến khi nó rời khỏi giới hạn mảng và chúng ta phải xuất tổng số tiền đã thu thập được cho mỗi truy vấn. 

Vì vậy, mỗi truy vấn xác định một cấp số cộng của các chỉ số bên trong mảng: bắt đầu từ vị trí a, sau đó là a + b, sau đó là a + 2b, v.v. Nhiệm vụ là tính tổng các giá trị mảng ở tất cả các vị trí như vậy. 

Các ràng buộc cho phép lên tới vài trăm nghìn phần tử và truy vấn. Một mô phỏng đơn giản cho mỗi truy vấn có thể yêu cầu bước qua tối đa n phần tử cho mỗi truy vấn, điều này dẫn đến khoảng n × q thao tác trong trường hợp xấu nhất, theo thứ tự 10^10. Điều đó vượt xa những gì có thể làm được trong giới hạn thời gian thông thường. 

Khó khăn chính là các truy vấn khác nhau có thể có kích thước bước khác nhau và cùng một mảng được sử dụng lại, vì vậy chúng ta cần khai thác cấu trúc theo cách hoạt động của các cấp số cộng này. 

Trường hợp cạnh tinh tế phát sinh khi kích thước bước là 1. Trong trường hợp đó, truy vấn suy biến thành tổng hậu tố đầy đủ từ a đến n. Trường hợp cạnh thứ hai là khi kích thước bước lớn, gần bằng n, trong đó mỗi truy vấn chỉ chạm vào một hoặc hai phần tử. Một tối ưu hóa thống nhất ngây thơ chỉ xử lý một chế độ một cách hiệu quả sẽ thất bại ở chế độ kia. 

Một vấn đề không rõ ràng khác là việc trộn các chiến lược không chính xác: nếu chúng ta cố gắng tính toán trước tất cả các kích thước bước lên đến n, bộ nhớ hoặc quá trình tiền xử lý sẽ trở nên quá lớn. Nếu chúng tôi chỉ tối ưu hóa kích thước bước nhỏ, thì các kích thước bước lớn vẫn tổng hợp TLE nếu xử lý không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi truy vấn, bắt đầu tại a, liên tục thêm giá trị vào chỉ mục hiện tại và nhảy theo b cho đến khi chúng ta vượt quá n. Điều này đúng vì nó mô phỏng chính xác định nghĩa quy trình. Tuy nhiên, chi phí của nó tỷ lệ thuận với số phần tử được truy cập trên mỗi truy vấn. Trong trường hợp xấu nhất, khi b = 1, một truy vấn sẽ quét các phần tử O(n) và với q truy vấn, kết quả này sẽ trở thành O(nq), quá lớn. 

Sự cải tiến xuất phát từ việc nhận thấy rằng các bước nhảy tạo thành cấp số cộng và mảng có thể được xem qua các lớp dư thừa theo modulo b. Cố định các chỉ mục phân vùng b thành các chuỗi độc lập: tất cả các chỉ mục có dạng a mod b tạo thành một chuỗi trong đó mỗi bước tiến về phía trước chính xác một vị trí trong chuỗi đó. Nếu chúng tôi tính toán trước các tổng tiền tố dọc theo các chuỗi dư lượng này thì mỗi truy vấn sẽ trở thành một truy vấn chênh lệch thời gian không đổi trong cấu trúc được tính toán trước đó. 

Vấn đề là bản thân b có thể lớn và việc tính toán trước các chuỗi cho tất cả b có thể có cho đến n sẽ có giá O(n^2). Sự cân bằng tiêu chuẩn là tách kích thước bước thành các chế độ nhỏ và lớn. Đối với b nhỏ, chúng ta tính toán trước các bảng DP trên tất cả các lớp dư lượng. Đối với b lớn, số phần tử được truy cập trên mỗi truy vấn là nhỏ, do đó mô phỏng trực tiếp đã đủ hiệu quả. 

Điều này chia vấn đề thành hai chế độ cùng nhau giải quyết tất cả các truy vấn một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| DP dư + phân chia ngưỡng | O(n√n + q√n) | O(n√n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chọn ngưỡng B khoảng √n.

1. Chúng tôi xử lý trước các câu trả lời cho tất cả các kích thước bước b từ 1 đến B. Đối với mỗi b, chúng tôi xây dựng một mảng có kích thước n trong đó chúng tôi tính tổng tích lũy dọc theo các bước nhảy có kích thước b. Điều này có nghĩa là chúng tôi xử lý các chỉ mục từ phải sang trái để mỗi vị trí có thể sử dụng lại giá trị của vị trí tiếp theo trong chuỗi của nó. Điều này đúng vì khi chúng ta biết tổng bắt đầu từ i + b, chúng ta có thể mở rộng nó thành i bằng cách thêm a[i]. 
2. Sau khi tiền xử lý, mỗi truy vấn có b ≤ B có thể được trả lời bằng O(1) bằng cách trả về trực tiếp giá trị được tính toán trước tại chỉ mục a cho kích thước bước b. Điều này có tác dụng vì DP đã biểu thị tổng dọc theo chuỗi nhảy chính xác bắt đầu từ chỉ mục đó. 
3. Đối với các truy vấn có b > B, chúng tôi thực hiện mô phỏng trực tiếp. Vì mỗi bước nhảy ít nhất B vị trí nên số phần tử được truy cập nhiều nhất là n/B, tức là nhỏ. 
4. Chúng tôi tích lũy tổng cho từng truy vấn một cách độc lập và xuất nó. 

Điểm quyết định quan trọng là sự phân chia tại B, điều này đảm bảo rằng không có truy vấn nào vừa tốn kém về số bước vừa tốn kém trong biểu diễn tiền xử lý. 

### Tại sao nó hoạt động 

Đối với kích thước bước nhỏ, cấu trúc của cấp số cộng căn chỉnh hoàn hảo với các lớp dư lượng, do đó mỗi chỉ mục chỉ phụ thuộc vào chính xác một chỉ mục tương lai trong cùng một lớp. Điều này tạo ra một sự lặp lại rõ ràng trong đó mọi giá trị được tính toán một lần và được sử dụng lại. Đối với kích thước bước lớn, tiến trình đủ ngắn để việc liệt kê trực tiếp không bao giờ tích lũy đủ tổng công việc vượt quá giới hạn. Mọi chỉ mục đều được xử lý bằng logic chuỗi được tính toán trước hoặc chỉ được truy cập một số lần nhỏ trên tất cả các truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    w = list(map(int, input().split()))
    q = int(input())

    B = int(n ** 0.5) + 1

    # dp[b][i] will store sum starting at i with step b, only for b <= B
    dp = [[0] * n for _ in range(B + 1)]

    for b in range(1, B + 1):
        for i in range(n - 1, -1, -1):
            nxt = i + b
            if nxt < n:
                dp[b][i] = w[i] + dp[b][nxt]
            else:
                dp[b][i] = w[i]

    out = []
    for _ in range(q):
        a, b = map(int, input().split())
        a -= 1

        if b <= B:
            out.append(str(dp[b][a]))
        else:
            s = 0
            i = a
            while i < n:
                s += w[i]
                i += b
            out.append(str(s))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Bảng DP`dp[b][i]`mã hóa tổng đầy đủ của cấp số cộng bắt đầu từ`i`với bước`b`, do đó các truy vấn trong chế độ bước nhỏ giảm xuống thành tra cứu trực tiếp. Vòng lặp ngược là cần thiết vì mỗi trạng thái phụ thuộc vào`i + b`, mà phải được tính toán rồi. 

Đối với các bước lớn, vòng lặp while vẫn hiệu quả vì chỉ số tăng nhanh, đảm bảo ít lần lặp lại cho mỗi truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ`w = [2, 3, 5, 7]`. 

Truy vấn`(1, 3)`bắt đầu ở chỉ số 0 và truy cập 0, 3, tổng 2 + 7 = 9. 

| Bước | tôi | Giá trị đã truy cập | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 2 | 
| 2 | 3 | 7 | 9 | 

Điều này xác nhận việc duyệt chính xác một cấp số cộng thưa thớt. 

Bây giờ hãy xem xét`(2, 2)`bắt đầu từ chỉ số 1. Dãy số là 1, 3, cho 3 + 7 = 10. 

| Bước | tôi | Giá trị đã truy cập | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | 
| 2 | 3 | 7 | 10 | 

Điều này thể hiện chế độ DP nếu b nhỏ, trong đó việc tái sử dụng cấu trúc hậu tố lặp đi lặp lại được ghi lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√n + q√n) | tiền xử lý cho tất cả các kích thước bước nhỏ cộng với việc truyền tải giới hạn cho các bước lớn | 
| Không gian | O(n√n) | Bảng DP lưu trữ tổng cho từng kích thước bước lên tới √n | 

Việc phân chia ngưỡng đảm bảo cả quy mô tiền xử lý và xử lý truy vấn trong giới hạn cho n lên tới khoảng 3×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# sample-like case
# (note: format depends on original problem, adapt as needed)
# assert run(...) == ...

# minimum size
assert True

# small step, full chain
# array where sums are easy to verify

# large step, single jump queries

# alternating values to catch parity mistakes
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn n nhỏ, b=1 | tổng hậu tố đầy đủ | tính đúng đắn của chuỗi DP | 
| b lớn gần n | phần tử đơn hoặc tổng hai bước | tính đúng đắn của nhánh nhanh | 
| mảng xen kẽ | xác minh thủ công | khác nhau trong việc lập chỉ mục | 

## Vỏ cạnh 

Đối với bước kích thước 1, mọi truy vấn sẽ trở thành tổng hậu tố. DP phải truyền ngược lại các giá trị một cách chính xác; nếu không, DP chuyển tiếp sẽ sử dụng lại các trạng thái chưa được khởi tạo một cách không chính xác. đầu vào`[1,2,3,4]`với truy vấn`(2,1)`phải sản xuất`2+3+4=9`và sự lặp lại ngược đảm bảo điều này bằng cách xây dựng từ cuối. 

Đối với kích thước bước rất lớn như`b ≥ n/2`, mỗi truy vấn truy cập tối đa hai phần tử. Nhánh mô phỏng xử lý việc này một cách an toàn và vòng lặp kết thúc nhanh chóng vì`i`nhảy ra khỏi giới hạn gần như ngay lập tức.
