---
title: "CF 1185B - Email từ Polycarp"
description: "Nhiệm vụ là xác minh xem một từ được gõ t có thể là kết quả hợp lý từ việc nhấn các phím của từ s trên bàn phím thỉnh thoảng lặp lại các chữ cái hay không. Mỗi chữ cái trong s phải xuất hiện theo thứ tự trong t, nhưng nó có thể xuất hiện một hoặc nhiều lần liên tiếp."
date: "2026-06-12T00:53:28+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1185
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 568 (Div. 2)"
rating: 1200
weight: 1185
solve_time_s: 86
verified: true
draft: false
---

[CF 1185B - Email từ Polycarp](https://codeforces.com/problemset/problem/1185/B) 

**Đánh giá:** 1200 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xác minh xem một từ được gõ có`t`có thể là kết quả hợp lý từ việc nhấn các phím của một từ`s`trên bàn phím thỉnh thoảng lặp lại các chữ cái. Mỗi chữ cái trong`s`phải xuất hiện theo thứ tự trong`t`, nhưng nó có thể xuất hiện một hoặc nhiều lần liên tiếp. Ví dụ, nếu`s`là "hello", thì "helloo" hoặc "hheelloo" là kết quả đầu ra hợp lệ, nhưng "helo" hoặc "hlllloo" không phải do thiếu chữ cái hoặc thứ tự bị vi phạm. 

Đầu vào bao gồm nhiều cặp từ, tối đa 100.000, với mỗi từ có tối đa 1.000.000 ký tự. Tuy nhiên, tổng chiều dài của tất cả`s`tổng số từ không vượt quá 1.000.000 và tương tự cho tất cả`t`từ. Điều này có nghĩa là giải pháp của chúng tôi không thể có các vòng lặp lồng nhau lặp theo độ dài`s`Và`t`trong trường hợp xấu nhất, vì điều đó có thể dẫn đến tối đa 10^12 thao tác. Quét tuyến tính từng cặp từ được chấp nhận vì tổng số ký tự được xử lý sẽ tối đa là 2 * 10^6, khả thi trong thời gian giới hạn. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó`t`chứa các chữ cái bổ sung không tương ứng với các lần lặp lại liên tiếp của`s`. Ví dụ,`s = "abc"`Và`t = "aabbccx"`sẽ trả về KHÔNG, vì không thể tạo thêm 'x' từ`s`. Một trường hợp tế nhị khác là khi`t`ví dụ như thiếu toàn bộ một chữ cái`s = "aaa"`Và`t = "aa"`. Ở đây đầu ra cũng phải là KHÔNG vì mỗi chữ cái trong`s`phải xuất hiện ít nhất một lần theo thứ tự. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ cố gắng kết hợp mọi nhân vật trong`s`đến từng nhân vật trong`t`sử dụng các vòng lặp lồng nhau. Với mỗi ký tự trong`s`, người ta sẽ quét về phía trước trong`t`cho đến khi tìm thấy kết quả khớp, sau đó lặp lại cho ký tự tiếp theo. Điều này đúng về mặt logic, nhưng nếu`s`có chiều dài m và`t`có độ dài n, độ phức tạp trong trường hợp xấu nhất là O(m * n). Với độ dài từ tối đa, điều này có thể ở mức 10^12 thao tác, quá chậm. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu là nhận ra rằng cả hai`s`Và`t`có thể được quét đồng thời với hai con trỏ. Chúng tôi giữ một con trỏ`i`TRONG`s`và một con trỏ`j`TRONG`t`. Khi`s[i] == t[j]`, chúng tôi di chuyển`i`Và`j`phía trước. Nếu như`t[j]`bằng`t[j-1]`, chúng tôi chỉ di chuyển`j`về phía trước vì đó có thể là thao tác nhấn phím lặp đi lặp lại. Bất kỳ sai lệch nào so với mô hình này có nghĩa là`t`không thể là kết quả của`s`. Điều này làm giảm vấn đề xuống còn quét tuyến tính thông qua`t`Và`s`, với độ phức tạp tỷ lệ thuận với tổng độ dài của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m*n) | O(1) | Quá chậm | 
| Quét hai con trỏ | O(m+n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ`i`Và`j`về không.`i`sẽ đi qua`s`Và`j`sẽ đi qua`t`. 
2. Trong khi`j`chưa đến hồi kết`t`:

Một. Nếu như`i`đã đến cuối`s`, kiểm tra xem các ký tự còn lại trong`t`tất cả đều bằng nhau`t[j-1]`. Nếu không thì trả về NO. 

b. Nếu như`s[i] == t[j]`, di chuyển cả hai`i`Và`j`phía trước. 

c. Khác nếu`j > 0`Và`t[j] == t[j-1]`, di chuyển`j`phía trước. Điều này giải thích cho các ký tự lặp lại do bàn phím bị hỏng. 

d. Ngược lại, trả về KHÔNG, vì`t[j]`không khớp với ký tự hiện tại trong`s`hoặc bất kỳ sự lặp lại hợp lệ nào. 
3. Sau vòng lặp, kiểm tra xem`i`đã đến cuối`s`. Nếu không, trả về NO vì một số ký tự của`s`đã không được sử dụng. 
4. Nếu tất cả các bước kiểm tra đều đạt, hãy trả về CÓ. 

Tại sao nó hoạt động: Ở mỗi bước,`i`Và`j`duy trì sự bất biến mà tất cả các ký tự trước đó`i`TRONG`s`đã được khớp theo thứ tự với các ký tự trước đó`j`TRONG`t`, cho phép lặp lại. Nếu bất kỳ ký tự nào trong`t`vi phạm mẫu này, thuật toán ngay lập tức trả về KHÔNG. Quá trình quét đảm bảo rằng mỗi ký tự trong`s`được khớp ít nhất một lần và theo thứ tự, và`t`có thể có thêm các ký tự lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_be_typed(s: str, t: str) -> str:
    i, j = 0, 0
    while j < len(t):
        if i < len(s) and s[i] == t[j]:
            i += 1
            j += 1
        elif j > 0 and t[j] == t[j-1]:
            j += 1
        else:
            return "NO"
    return "YES" if i == len(s) else "NO"

n = int(input())
results = []
for _ in range(n):
    s = input().strip()
    t = input().strip()
    results.append(can_be_typed(s, t))

print("\n".join(results))
```chức năng`can_be_typed`thực hiện quét hai con trỏ được mô tả ở trên. Nó cẩn thận xử lý các ranh giới:`i`có thể đạt đến cuối`s`trong khi`j`vẫn còn ở`t`, và các chữ cái lặp lại ở cuối`t`được phép. Vòng lặp chính đảm bảo các con trỏ không bao giờ vượt quá giới hạn và lần kiểm tra cuối cùng sẽ xác nhận tất cả`s`đã được tiêu thụ. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

| tôi | j | s[i] | t[j] | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | h | h | trận đấu, i++, j++ | 
| 1 | 1 | e | e | trận đấu, i++, j++ | 
| 2 | 2 | tôi | tôi | trận đấu, i++, j++ | 
| 3 | 3 | tôi | tôi | trận đấu, i++, j++ | 
| 4 | 4 | o | o | trận đấu, i++, j++ | 
| 5 | 5 | - | - | kết thúc, CÓ | 

Theo dõi cặp thứ hai "hello" -> "helloo": 

| tôi | j | s[i] | t[j] | Hành động | 
| --- | --- | --- | --- | --- | 
| 0-4 | 0-4 | h,e,l,l,o | h,e,l,l,o | trận đấu | 
| 5 | 5 | - | o | t[j] == t[j-1], j++ | 
| 5 | 6 | - | - | kết thúc, CÓ | 

Những dấu vết này cho thấy thuật toán xử lý chính xác các ký tự lặp lại và xác nhận tính bất biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài của tất cả s + tổng chiều dài của tất cả t) | Mỗi nhân vật trong`s`Và`t`được con trỏ truy cập nhiều nhất một lần | 
| Không gian | O(1) thêm | Chỉ sử dụng con trỏ và biến cục bộ; danh sách đầu ra không đáng kể | 

Với các ràng buộc về tổng chiều dài tối đa là 10^6, thuật toán sẽ chạy hiệu quả trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    results = []
    for _ in range(n):
        s = input().strip()
        t = input().strip()
        results.append(can_be_typed(s, t))
    return "\n".join(results)

# Provided samples
assert run("4\nhello\nhello\nhello\nhelloo\nhello\nhlllloo\nhello\nhelo\n") == "YES\nYES\nNO\nNO", "sample 1"

# Custom cases
assert run("1\na\naaa\n") == "YES", "repeated single character"
assert run("1\nabc\naabbcc\n") == "YES", "all repeated in order"
assert run("1\nabc\nacb\n") == "NO", "wrong order"
assert run("1\nabc\nab\n") == "NO", "missing last char"
assert run("1\nx\ny\n") == "NO", "different characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a -> aaa | CÓ | lặp lại ký tự đơn | 
| abc -> aabbcc | CÓ | nhiều chữ cái lặp đi lặp lại | 
| abc -> acb | KHÔNG | thứ tự các chữ cái quan trọng | 
| abc -> ab | KHÔNG | chữ cái còn thiếu | 
| x -> y | KHÔNG | nhân vật hoàn toàn khác nhau | 

## Vỏ cạnh 

Đối với đầu vào`s = "aaa"`,`t = "aa"`, thuật toán bắt đầu bằng`i = 0, j = 0`. Nó khớp với 'a' đầu tiên, tăng cả hai con trỏ, sau đó khớp với 'a' thứ hai, tăng cả hai và tìm`j`vào cuối`t`trong khi`i`vẫn ở mức 2. Vì`i != len(s)`, nó trả về KHÔNG, xác định chính xác rằng`t`không bao gồm tất cả`s`. 

Vì`s = "abc"`Và`t = "aabbccx"`, quá trình quét khớp thành công với 'a','b','c' và
