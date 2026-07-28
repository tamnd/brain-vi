---
title: "CF 102798H - Bom tin nhắn"
description: "Chúng tôi có một bộ sưu tập các nhóm trò chuyện và sinh viên. Một nhóm thay đổi theo thời gian vì học sinh có thể tham gia và rời đi. Khi một học sinh gửi tin nhắn trong một nhóm, mọi học sinh khác hiện đang ở trong nhóm đó sẽ nhận được một tin nhắn."
date: "2026-07-27T17:51:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102798
codeforces_index: "H"
codeforces_contest_name: "2020 China Collegiate Programming Contest, Weihai Site"
rating: 0
weight: 102798
solve_time_s: 55
verified: true
draft: false
---

[CF 102798H - Bom tin nhắn](https://codeforces.com/problemset/problem/102798/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các nhóm trò chuyện và sinh viên. Một nhóm thay đổi theo thời gian vì học sinh có thể tham gia và rời đi. Khi một học sinh gửi tin nhắn trong một nhóm, mọi học sinh khác hiện đang ở trong nhóm đó sẽ nhận được một tin nhắn. Nhiệm vụ là tính toán, đối với mỗi học sinh, tổng số tin nhắn họ nhận được sau khi xử lý toàn bộ nhật ký sự kiện. 

Đầu vào mô tả số lượng nhóm, số lượng sinh viên và danh sách các sự kiện theo trình tự thời gian. Sự kiện tham gia sẽ thêm một học sinh vào một nhóm, sự kiện rời khỏi sẽ xóa một học sinh khỏi nhóm và sự kiện tin nhắn có nghĩa là một học sinh sẽ gửi một tin nhắn đến các thành viên hiện tại khác của nhóm đó. Kết quả đầu ra là một giá trị cho mỗi học sinh, biểu thị số lượng tin nhắn đã gửi đến học sinh đó. 

Giới hạn rất lớn: có thể có tới 100.000 nhóm, 200.000 sinh viên và 1.000.000 sự kiện. Mô phỏng trực tiếp truy cập từng thành viên trong nhóm để xem từng tin nhắn sẽ quá chậm vì một nhóm có thể chứa hàng trăm nghìn sinh viên và có thể có hàng triệu thao tác. Với một triệu sự kiện, giải pháp cần duy trì thời gian gần như không đổi cho mỗi sự kiện, cho phép độ phức tạp gần như tuyến tính. 

Phần khó khăn là người gửi không nhận được tin nhắn của chính họ. Một giải pháp chỉ đơn giản là đếm từng tin nhắn được gửi trong một nhóm và gửi cho mọi thành viên sẽ bị tính quá mức. Ví dụ:```
1 1 1
3 1 1
```Học sinh tham gia nhóm 1 và gửi tin nhắn. Đầu ra đúng là:```
0
```Một cách tiếp cận bất cẩn khi thêm một thông báo cho mỗi thành viên sẽ tạo ra 1. 

Một trường hợp tế nhị khác là rời đi và sau đó gia nhập cùng một nhóm. Coi như:```
1 1 1
3 1 1
2 1 1
1 1 1
3 1 1
```Đầu ra đúng là:```
0
```Tin nhắn đầu tiên không ai nhận được vì chỉ có thành viên là người gửi. Học sinh rời đi, tham gia lại và tin nhắn thứ hai cũng có kết quả tương tự. Giải pháp chỉ ghi nhớ rằng học viên đã từng ở trong nhóm có thể vô tình đếm được tin nhắn từ thời gian thành viên trước đó. 

# Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Duy trì các thành viên của mỗi nhóm. Khi một sự kiện tin nhắn xảy ra, hãy duyệt qua tất cả các thành viên của nhóm đó và tăng dần câu trả lời của họ ngoại trừ người gửi. Điều này đúng vì nó tuân theo định nghĩa về việc gửi tin nhắn. 

Sự cố xuất hiện khi một nhóm lớn nhận được nhiều tin nhắn. Nếu một nhóm chứa 200.000 sinh viên và nhận được 1.000.000 tin nhắn thì mô phỏng sẽ thực hiện khoảng 200 tỷ lượt cập nhật thành viên. Bản thân các hoạt động của cấu trúc dữ liệu vẫn ổn, nhưng số lượt truy cập vượt xa giới hạn thời gian cho phép. 

Quan sát quan trọng là chúng tôi không cần cập nhật mọi thành viên khi có thông báo xảy ra. Thay vào đó, mỗi học sinh có thể chịu trách nhiệm tính toán các tin nhắn nhận được sau này của mình. Đối với một học sinh và nhóm cụ thể, số lượng tin nhắn họ nhận được là số lượng tin nhắn được gửi trong nhóm đó khi họ còn là thành viên, trừ đi số lượng tin nhắn mà cá nhân họ đã gửi trong cùng khoảng thời gian thành viên đó. 

Điều này chuyển đổi vấn đề thành khoảng thời gian theo dõi thành viên. Mỗi thành viên đang hoạt động chỉ cần hai thông tin: số lượng tin nhắn mà nhóm đã nhận được khi học sinh tham gia và số lượng tin nhắn mà học sinh đó đã gửi khi ở lại nhóm. 

Đối với mỗi nhóm, chúng tôi lưu trữ một bộ đếm tin nhắn toàn cầu. Tham gia ghi lại bộ đếm hiện tại. Rời đi hoàn tất sự đóng góp của học sinh từ nhóm đó. Việc gửi tin nhắn chỉ làm tăng bộ đếm nhóm và bộ đếm cá nhân của người gửi cho nhóm đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số tin nhắn × kích thước nhóm) trong trường hợp xấu nhất | O(số lượng thành viên) | Quá chậm | 
| Tối ưu | O (các) | O (các) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Giữ một mảng`group_messages`Ở đâu`group_messages[g]`là tổng số tin nhắn từng được gửi trong nhóm`g`. Mỗi sự kiện tin nhắn chỉ cần tăng một giá trị này. 
2. Lưu giữ thông tin thành viên hiện tại của từng cặp nhóm học sinh. Khi một học sinh tham gia một nhóm, lưu trữ giá trị hiện tại của`group_messages[g]`và đặt lại số lượng tin nhắn được gửi bởi sinh viên này trong thời gian thành viên này. Điều này tạo ra một điểm khởi đầu rõ ràng để tính toán các tin nhắn nhận được trong tương lai. 
3. Khi học sinh gửi tin nhắn, hãy tăng bộ đếm tin nhắn của nhóm. Đồng thời tăng bộ đếm tin nhắn đã gửi của chính người gửi cho nhóm đó vì tin nhắn này sau đó sẽ bị xóa khỏi số lượng tin nhắn đã nhận của họ. 
4. Khi một học sinh rời khỏi nhóm, hãy tính xem có bao nhiêu tin nhắn đã xảy ra trong nhóm trong thời gian là thành viên. Trừ các tin nhắn do học sinh này gửi, sau đó cộng kết quả vào câu trả lời cuối cùng của học sinh đó. Xóa bản ghi thành viên đang hoạt động. 
5. Sau khi tất cả các sự kiện được xử lý, một số học sinh vẫn có thể là thành viên của nhóm. Hoàn tất các tư cách thành viên đang hoạt động đó giống như sự kiện nghỉ phép vì tin nhắn nhận được của họ cũng phải được đưa vào câu trả lời. 

Tại sao nó hoạt động: Đối với mỗi khoảng thời gian thành viên, chênh lệch bộ đếm nhóm sẽ đếm chính xác tất cả tin nhắn đã xảy ra khi học sinh thuộc nhóm. Những tin nhắn duy nhất trong nhóm đó mà học sinh không nên nhận là những tin nhắn của chính họ, những tin nhắn này sẽ được tính và xóa riêng. Mỗi tin nhắn thuộc về chính xác một khoảng thời gian thành viên cho mỗi học sinh có thể nhận được nó, vì vậy không có tin nhắn nào bị bỏ sót hoặc bị tính hai lần. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, s = map(int, input().split())

    group_messages = [0] * (n + 1)
    answer = [0] * (m + 1)

    active = {}

    for _ in range(s):
        t, x, y = map(int, input().split())

        if t == 1:
            active[(x, y)] = [group_messages[y], 0]

        elif t == 2:
            start, sent = active.pop((x, y))
            answer[x] += group_messages[y] - start - sent

        else:
            group_messages[y] += 1
            active[(x, y)][1] += 1

    for (x, y), (start, sent) in active.items():
        answer[x] += group_messages[y] - start - sent

    print("\n".join(map(str, answer[1:])))

if __name__ == "__main__":
    solve()
```Mảng`group_messages`đại diện cho lịch sử toàn cầu của mỗi nhóm. Nó không bao giờ cần phải được khôi phục vì khoảng thời gian thành viên của học sinh được xử lý bằng cách sử dụng giá trị được ghi lại khi họ tham gia. 

Từ điển`active`chỉ lưu trữ các thành viên hiện đang tồn tại. Chìa khóa là cặp`(student, group)`và các giá trị được lưu trữ là số lượng tin nhắn của nhóm tại thời điểm tham gia và số lượng tin nhắn được học sinh đó gửi khi ở trong nhóm. 

Hoạt động của tin nhắn là thời gian không đổi. Ở đây rất dễ mắc sai lầm khi quên tăng bộ đếm cá nhân của người gửi, điều này sẽ khiến tin nhắn tự gửi được tính là tin nhắn đã nhận. 

Vòng cuối cùng xử lý những sinh viên không bao giờ rời khỏi nhóm của mình. Họ vẫn còn những khoảng thời gian thành viên chưa hoàn thành nên những khoảng thời gian đó phải được đóng lại sau khi tất cả các sự kiện đã được xử lý. 

Số nguyên Python là đủ vì số lượng tin nhắn nhận được tối đa có thể vượt quá giới hạn số nguyên 32 bit. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 3 10
1 3 2
1 3 1
1 1 2
1 2 1
3 1 2
2 3 1
3 3 2
3 2 1
3 3 2
3 2 1
```| Sự kiện | Hành động | Tin nhắn nhóm | Sinh viên 1 trả lời | Học sinh trả lời 2 | Học sinh trả lời 3 | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Sinh viên 3 tham gia nhóm 2 | 0 | 0 | 0 | 0 | 
| 2 | Sinh viên 3 tham gia nhóm 1 | 0 | 0 | 0 | 0 | 
| 3 | Sinh viên 1 tham gia nhóm 2 | 0 | 0 | 0 | 0 | 
| 4 | Sinh viên 2 tham gia nhóm 1 | 0 | 0 | 0 | 0 | 
| 5 | Sinh viên 1 gửi vào nhóm 2 | g2=1 | 0 | 0 | 1 | 
| 6 | Sinh viên 3 rời nhóm 1 | g2=1 | 0 | 0 | 1 | 
| 7 | Sinh viên 3 gửi vào nhóm 2 | g2=2 | 0 | 0 | 1 | 
| 8 | Sinh viên 2 gửi vào nhóm 1 | g1=1 | 0 | 0 | 1 | 
| 9 | Sinh viên 3 gửi vào nhóm 2 | g2=3 | 0 | 0 | 1 | 
| 10 | Sinh viên 2 gửi vào nhóm 1 | g1=2 | 0 | 0 | 1 | 

Sau khi hoàn tất tư cách thành viên tích cực, sinh viên 1 nhận được 2 tin nhắn và sinh viên 2 nhận được 0 tin nhắn, cho biết:```
2
0
1
```Dấu vết này cho thấy tin nhắn chỉ được tính trong thời gian thành viên đang hoạt động và người gửi sẽ bị loại trừ. 

Đối với mẫu thứ hai:```
2 5 10
1 1 2
3 1 2
2 1 2
1 3 2
1 1 2
3 1 2
3 3 2
1 4 2
3 3 2
1 5 1
```| Sự kiện | Hành động | Tin nhóm 2 | Sinh viên 1 trả lời | Học sinh trả lời 3 | 
| --- | --- | --- | --- | --- | 
| 1 | Sinh viên 1 tham gia nhóm 2 | 0 | 0 | 0 | 
| 2 | Sinh viên 1 gửi | 1 | 0 | 0 | 
| 3 | Sinh viên 1 ra đi | 1 | 0 | 0 | 
| 4 | Sinh viên 3 tham gia | 1 | 0 | 0 | 
| 5 | Sinh viên 1 tham gia lại | 1 | 0 | 0 | 
| 6 | Sinh viên 1 gửi | 2 | 0 | 1 | 
| 7 | Sinh viên 3 gửi | 3 | 1 | 1 | 
| 8 | Sinh viên 4 tham gia | 3 | 1 | 1 | 
| 9 | Sinh viên 3 gửi | 4 | 2 | 1 | 
| 10 | Sinh viên 5 tham gia nhóm 1 | 4 | 2 | 1 | 

Chi tiết quan trọng ở đây là thời gian thành viên đầu tiên và thời gian thành viên thứ hai của sinh viên 1 là riêng biệt. Tin nhắn từ trước khi rời đi không bị trộn lẫn với tin nhắn được tham gia sau này. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O (các) | Mỗi sự kiện chỉ thực hiện các thao tác từ điển hoặc mảng. | 
| Không gian | O (các) | Từ điển thành viên đang hoạt động có thể chứa tối đa tất cả các hoạt động tham gia. | 

Kích thước đầu vào bị chi phối bởi một triệu sự kiện, do đó cần có giải pháp tuyến tính. Thuật toán tránh lặp lại các thành viên trong nhóm và nằm trong giới hạn dự kiến. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""3 3 10
1 3 2
1 3 1
1 1 2
1 2 1
3 1 2
2 3 1
3 3 2
3 2 1
3 3 2
3 2 1
""") == "2\n0\n1\n", "sample 1"

assert run("""2 5 10
1 1 2
3 1 2
2 1 2
1 3 2
1 1 2
3 1 2
3 3 2
1 4 2
3 3 2
1 5 1
""") == "2\n0\n1\n1\n0\n", "sample 2"

assert run("""1 1 2
1 1 1
3 1 1
""") == "0\n", "single member self message"

assert run("""1 2 4
1 1 1
1 2 1
3 1 1
3 2 1
""") == "1\n1\n", "two members"

assert run("""1 2 5
1 1 1
1 2 1
3 1 1
2 2 1
3 1 1
""") == "0\n1\n", "leave before later messages"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Sinh viên gửi đơn |`0`| Người gửi không được nhận tin nhắn của chính họ. | 
| Hai học sinh trong một nhóm |`1 1`| Phát tin nhắn bình thường. | 
| Thành viên rời đi trước tin nhắn sau |`0 1`| Khoảng thời gian thành viên phải được tách ra. | 

# Vỏ cạnh 

Đối với trường hợp nhóm một thành viên:```
1 1 2
1 1 1
3 1 1
```Học sinh tham gia nhóm duy nhất và gửi một tin nhắn. Bộ đếm nhóm tăng lên nhưng bộ đếm cá nhân của học sinh cũng tăng lên. Khi tư cách thành viên tích cực được hoàn tất, phép tính sẽ được thực hiện`1 - 0 - 1 = 0`, vậy đáp án đúng. 

Đối với trường hợp nối lặp lại:```
1 1 5
1 1 1
3 1 1
2 1 1
1 1 1
3 1 1
```Tin nhắn đầu tiên thuộc về khoảng thời gian thành viên đầu tiên và không đóng góp gì. Lần tham gia thứ hai ghi lại giá trị bộ đếm nhóm mới, do đó thông báo trước đó không thể ảnh hưởng đến khoảng thời gian thứ hai. Phép tính cuối cùng lại xóa tin nhắn của chính học sinh và trả về số 0. 

Đối với nhóm có nhiều thành viên:```
1 2 3
1 1 1
1 2 1
3 1 1
```Bộ đếm nhóm đi từ 0 đến 1. Học sinh 1 ghi lại một tin nhắn đã gửi, trong khi học sinh 2 không có tin nhắn nào đã gửi trong nhóm. Việc hoàn thiện mang lại cho sinh viên 1`1 - 0 - 1 = 0`và sinh viên 2`1 - 0 - 0 = 1`. 

Dành cho những học sinh ở lại nhóm cho đến hết, chẳng hạn như:```
1 2 2
1 1 1
1 2 1
```không có sự kiện nghỉ phép. Bước dọn dẹp cuối cùng xử lý cả tư cách thành viên đang hoạt động một cách chính xác như thể họ đã để lại ở cuối nhật ký.
