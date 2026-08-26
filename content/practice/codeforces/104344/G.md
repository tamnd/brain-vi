---
title: "CF 104344G - Quà tặng của P\u00e1scoa"
description: "Fred có một danh sách các quả trứng sô cô la, mỗi quả đều có giá xác định bằng xu và một số tiền cố định. Nhiệm vụ là xác định xem anh ta có thể mua tối đa bao nhiêu quả trứng nếu chọn chúng một cách tối ưu."
date: "2026-07-01T18:29:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "G"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 64
verified: true
draft: false
---

[CF 104344G - Quà tặng của P\u00e1scoa](https://codeforces.com/problemset/problem/104344/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Fred có một danh sách các quả trứng sô cô la, mỗi quả đều có giá xác định bằng xu và một số tiền cố định. Nhiệm vụ là xác định xem anh ta có thể mua tối đa bao nhiêu quả trứng nếu chọn chúng một cách tối ưu. Mỗi quả trứng có thể được mua nhiều nhất một lần và anh ta có thể tự do chọn bất kỳ tập hợp con nào trong số những quả trứng có sẵn. 

Đầu vào cung cấp số lượng trứng có sẵn, số tiền và danh sách giá. Đầu ra là một số nguyên duy nhất biểu thị số lượng mặt hàng tối đa có tổng chi phí không vượt quá ngân sách. 

Hạn chế chính là số lượng trứng có thể lên tới 100000. Bất kỳ giải pháp nào cố gắng xem xét tất cả các tập hợp con hoặc thậm chí tất cả các kết hợp đều không khả thi ngay lập tức. Cách tiếp cận bậc hai sẽ yêu cầu khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn thời gian. 

Trường hợp đặc biệt không tầm thường duy nhất phát sinh khi giá cả rất khác nhau. Ví dụ: nếu tất cả các mặt hàng đều đắt tiền ngoại trừ một mặt hàng rẻ tiền, một chiến lược ngây thơ không ưu tiên các giá trị nhỏ có thể sớm lãng phí ngân sách. Một dạng lỗi khác xuất hiện khi danh sách không được sắp xếp và cách tiếp cận tham lam cho rằng thứ tự có vấn đề mà không được sắp xếp rõ ràng. 

Một ví dụ đơn giản về vấn đề đặt hàng: 

đầu vào:```
3
5
5 4 1
```Câu trả lời đúng là`1`, vì mua món đồ có giá 1 là tối ưu. Một cách tiếp cận bất cẩn chọn tiền tố hợp lý đầu tiên mà không sắp xếp có thể hoạt động không chính xác, không thể đoán trước tùy thuộc vào thứ tự đầu vào. 

Một trường hợp cạnh khác: 

đầu vào:```
4
10
8 7 6 5
```Đầu ra đúng là`1`. Bất kỳ chiến lược nào thử kết hợp mà không sắp xếp thứ tự sẽ không thể tối đa hóa số lượng một cách hiệu quả. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ là thử tất cả các tập hợp con trứng, tính tổng chi phí của chúng và theo dõi tập hợp con lớn nhất có tổng không vượt quá ngân sách. Điều này đúng vì nó đánh giá rõ ràng mọi lựa chọn có thể, nhưng nó tăng theo cấp số nhân khi có 2^N tập hợp con và thậm chí việc kiểm tra từng tập hợp con có chi phí O(N), dẫn đến thời gian là O(N·2^N). Với N lên tới 100000 thì điều này là không thể. 

Một quan sát tốt hơn đến từ việc điều chỉnh lại mục tiêu. Chúng tôi không cố gắng tối đa hóa giá trị hoặc giảm thiểu chi phí; chúng tôi đang tối đa hóa số lượng mục, điều này cho thấy rằng mỗi mục đều đóng góp như nhau cho mục tiêu. Vì mỗi mặt hàng đều mang lại +1 cho câu trả lời bất kể giá của nó như thế nào nên chiến lược tối ưu là mua những mặt hàng rẻ nhất trước tiên. 

Khi sắp xếp giá theo thứ tự tăng dần, chúng ta có thể tham lam lấy từng món một cho đến khi hết ngân sách. Điều này hiệu quả vì bất kỳ giải pháp nào bao gồm một mặt hàng đắt tiền hơn trong khi loại trừ một mặt hàng rẻ hơn đều có thể được cải thiện bằng cách hoán đổi chúng, giảm hoặc duy trì tổng chi phí trong khi không giảm số lượng. 

Điều này làm giảm vấn đề sắp xếp cộng với một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N·2^N) | O(N) | Quá chậm | 
| Sắp xếp + Tham lam | O(N log N) | O(1) hoặc O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc N, số lượng trứng và C, ngân sách hiện có. 
2. Đọc bảng giá. 
3. Sắp xếp danh sách theo thứ tự không giảm dần để những mặt hàng rẻ hơn xếp trước. 

Việc sắp xếp là cần thiết vì các quyết định cục bộ phụ thuộc vào thứ tự chi phí toàn cầu. 
4. Khởi tạo bộ đếm các mặt hàng đã mua và tổng số tiền đã chi tiêu. 
5. Lặp lại các mức giá đã được sắp xếp. 
6. Đối với mỗi mức giá, hãy kiểm tra xem việc thêm nó vào số tiền chi tiêu hiện tại có nằm trong ngân sách hay không. 

Nếu có, hãy bao gồm nó và cập nhật số tiền chi tiêu cũng như bộ đếm. 

Nếu không, hãy dừng ngay lập tức vì tất cả các mặt hàng còn lại ít nhất cũng đắt như vậy. 
7. Xuất bộ đếm. 

Tại sao nó hoạt động: 

Tại bất kỳ thời điểm nào trong quá trình, chúng tôi đã chọn tập hợp con rẻ nhất có thể có kích thước nhất định. Nếu tồn tại một giải pháp tốt hơn với nhiều mặt hàng hơn, nó nhất thiết sẽ thay thế một số mặt hàng đắt tiền đã chọn bằng một mặt hàng rẻ hơn chưa sử dụng, điều này mâu thuẫn với thực tế là chúng ta luôn xử lý các mặt hàng theo thứ tự tăng dần. Lựa chọn tham lam duy trì tính bất biến rằng sau k bước, chúng ta có chi phí tối thiểu có thể có trong số tất cả các tập con có kích thước k. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    c = int(input())
    p = list(map(int, input().split()))
    
    p.sort()
    
    used = 0
    total = 0
    
    for price in p:
        if total + price <= c:
            total += price
            used += 1
        else:
            break
    
    print(used)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc sắp xếp giá sao cho chúng ta luôn xem xét những quả trứng rẻ hơn trước tiên. Biến`total`theo dõi số tiền đã chi tiêu cho đến nay và`used`đếm xem có bao nhiêu quả trứng đã được chọn. Vòng lặp duy trì tính bất biến là chúng ta luôn lấy mục khả thi rẻ nhất tiếp theo. Điều kiện dừng sớm là an toàn vì một khi giá vượt quá ngân sách còn lại thì tất cả các mức giá tiếp theo ít nhất cũng lớn bằng. 

Một chi tiết tinh tế là việc sử dụng kiểm tra ngân sách nghiêm ngặt`total + price <= c`. Điều này đảm bảo chúng tôi không bao giờ vượt quá giới hạn ngay cả trong các trường hợp giới hạn khi ngân sách còn lại khớp chính xác với một mức giá. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
15
1 2 3 5
```Giá đã sắp xếp vẫn còn`[1, 2, 3, 5]`. 

| Bước | Giá | Tổng cộng trước | Lấy? | Tổng Sau | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | vâng | 1 | 1 | 
| 2 | 2 | 1 | vâng | 3 | 2 | 
| 3 | 3 | 3 | vâng | 6 | 3 | 
| 4 | 5 | 6 | vâng | 11 | 4 | 

Tất cả các mục đều nằm trong ngân sách 15, vì vậy câu trả lời là 4. 

Điều này xác nhận rằng khi tổng số tiền của tất cả các mục nằm trong ngân sách, thuật toán sẽ tiêu thụ mọi thứ một cách chính xác. 

### Mẫu 2 

đầu vào:```
5
10
1 9 4 6 3
```Giá sắp xếp:`[1, 3, 4, 6, 9]`| Bước | Giá | Tổng cộng trước | Lấy? | Tổng Sau | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | vâng | 1 | 1 | 
| 2 | 3 | 1 | vâng | 4 | 2 | 
| 3 | 4 | 4 | vâng | 8 | 3 | 
| 4 | 6 | 8 | không | 8 | 3 | 

Chúng tôi dừng lại khi không thể đưa mục tiếp theo vào. Điều này cho thấy việc tham lam lấy những món đồ nhỏ nhất sẽ tối đa hóa số lượng trước khi cạn kiệt ngân sách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế; tuyến tính đơn sau đó | 
| Không gian | O(1) bổ sung (hoặc O(N) tùy thuộc vào việc triển khai sắp xếp) | Chỉ các bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 100000 mục và việc sắp xếp O(N log N) nằm trong giới hạn. Quét tuyến tính là không đáng kể khi so sánh, do đó giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(input())
    c = int(input())
    p = list(map(int, input().split()))
    
    p.sort()
    
    used = 0
    total = 0
    
    for price in p:
        if total + price <= c:
            total += price
            used += 1
        else:
            break
    
    return str(used).strip()

# provided samples
assert run("4\n15\n1 2 3 5\n") == "4", "sample 1"
assert run("5\n10\n1 9 4 6 3\n") == "3", "sample 2"

# custom cases
assert run("1\n5\n10\n") == "0", "cannot buy anything"
assert run("3\n10\n5 5 5\n") == "2", "boundary exact fit"
assert run("6\n21\n4 4 4 4 4 4\n") == "5", "repeated small values"
assert run("5\n100\n1 2 3 4 5\n") == "5", "all fit easily"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 món quá đắt | 0 | xử lý việc mua hàng bằng không | 
| giá trị bằng nhau lặp đi lặp lại | 2 | chính xác về ranh giới ngân sách | 
| nhiều giá trị nhỏ bằng nhau | 5 | tham lam tích lũy ổn định | 
| tất cả các mục phù hợp | 5 | trường hợp tiêu thụ đầy đủ | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các mặt hàng đều đắt hơn ngân sách. Sau khi sắp xếp, mục đầu tiên đã vi phạm ràng buộc, do đó vòng lặp kết thúc ngay lập tức và đầu ra vẫn bằng 0. Ví dụ: 

đầu vào:```
3
2
5 6 7
```Sau khi sắp xếp`[5, 6, 7]`, lần so sánh đầu tiên thất bại và không có cập nhật nào xảy ra, tạo ra kết quả`0`. 

Một trường hợp khác là khi nhiều mặt hàng có giá giống nhau bằng ngân sách còn lại ở các giai đoạn khác nhau. điều kiện`total + price <= c`đảm bảo rằng sự bình đẳng được chấp nhận một cách chính xác. Ví dụ: 

đầu vào:```
4
10
2 2 2 4
```Thực thi: 

Ba số 2 đầu tiên được lấy, đạt tổng số 6, sau đó lấy 4 để đạt chính xác 10. Thuật toán cho phép sự bằng nhau ở mỗi bước một cách chính xác, đảm bảo đạt được số lượng tối đa.
