---
title: "CF 413B - Spyke Chat"
description: "Chúng tôi được giao cho một công ty nơi nhân viên tham gia vào một số nhóm trò chuyện độc lập. Mỗi cuộc trò chuyện có tư cách thành viên cố định được xác định trước. Theo thời gian, nhật ký sẽ ghi lại các tin nhắn: mỗi sự kiện cho biết một nhân viên cụ thể đăng bài trong một cuộc trò chuyện cụ thể."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 413
codeforces_index: "B"
codeforces_contest_name: "Coder-Strike 2014 - Round 2"
rating: 1300
weight: 413
solve_time_s: 108
verified: false
draft: false
---

[CF 413B - Trò chuyện gián điệp](https://codeforces.com/problemset/problem/413/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một công ty nơi nhân viên tham gia vào một số nhóm trò chuyện độc lập. Mỗi cuộc trò chuyện có tư cách thành viên cố định được xác định trước. Theo thời gian, nhật ký sẽ ghi lại các tin nhắn: mỗi sự kiện cho biết một nhân viên cụ thể đăng bài trong một cuộc trò chuyện cụ thể. 

Quy tắc chính rất đơn giản nhưng không đối xứng: khi ai đó gửi tin nhắn trong cuộc trò chuyện, mọi người tham gia khác trong cùng cuộc trò chuyện đó sẽ nhận được chính xác một thông báo. Người gửi không nhận được thông báo về tin nhắn của chính họ trong cuộc trò chuyện đó. 

Nhiệm vụ là tính toán xem mỗi nhân viên có bao nhiêu thông báo họ nhận được trên tất cả các cuộc trò chuyện và tất cả tin nhắn trong nhật ký. 

Cấu trúc đầu vào mô tả mối quan hệ hai bên giữa nhân viên và các cuộc trò chuyện bằng cách sử dụng ma trận nhị phân n x m, trong đó m rất nhỏ (tối đa là 10). Sau đó là một chuỗi lên tới 200.000 sự kiện tin nhắn. Mỗi sự kiện đều độc lập và đóng góp thông báo cho tất cả các thành viên khác trong cuộc trò chuyện có liên quan. 

Những ràng buộc ngay lập tức cho thấy sự mất cân bằng quan trọng. Số lượng nhân viên có thể lên tới 20.000 nhưng số lượng chat lại rất ít. Sự bất đối xứng này là điều cho phép chúng tôi tránh việc mô phỏng từng thông điệp một cách ngây thơ đối với tất cả những người tham gia. Mô phỏng trực tiếp cho mỗi sự kiện sẽ vẫn chỉ hoạt động nếu mỗi cuộc trò chuyện nhỏ hoặc ít sự kiện, nhưng ở đây cả hai đều có thể lớn. 

Đối với mỗi sự kiện, một cách triển khai đơn giản sẽ quét tất cả n nhân viên và kiểm tra xem họ có thuộc nhóm trò chuyện hay không, tăng dần bộ đếm. Điều này dẫn đến 200.000 sự kiện nhân với 20.000 nhân viên, tức là 4 tỷ séc trong trường hợp xấu nhất, rõ ràng là quá chậm. 

Một chế độ lỗi tinh vi hơn sẽ xuất hiện nếu một người cố gắng tính toán trước danh sách lân cận nhưng vẫn lặp lại mỗi sự kiện trên toàn bộ tư cách thành viên trò chuyện mà không tối ưu hóa công việc lặp đi lặp lại. Điều đó vẫn thoái hóa thành hành vi O(k · n). 

Các trường hợp Edge phá vỡ các giải pháp ngây thơ bao gồm: 

1. Một cuộc trò chuyện duy nhất có chứa hầu hết tất cả nhân viên, trong đó có nhiều tin nhắn. Mọi sự kiện đều yêu cầu gần như tất cả nhân viên phải chạm vào. 
2. Nhiều tin nhắn lặp đi lặp lại trong cùng một cuộc trò chuyện. Mọi lần quét toàn bộ mỗi sự kiện đều lặp lại cùng một công việc một cách không cần thiết. 
3. Nhân viên thuộc nhiều chat (tối đa m=10). Việc tổng hợp mỗi nhân viên qua các cuộc trò chuyện trên mỗi sự kiện vẫn trở nên nặng nề nếu thực hiện không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi sự kiện tin nhắn (x, y), chúng ta xem xét tất cả nhân viên i từ 1 đến n. Nếu nhân viên tôi đang trò chuyện y và tôi không phải là x, chúng tôi sẽ tăng bộ đếm thông báo của họ. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa về việc gửi thông báo. 

Tuy nhiên, mỗi sự kiện đều tiêu tốn O(n) thời gian theo cách tiếp cận này. Với k lên tới 2·10^5 và n lên đến 2·10^4, điều này mang lại khoảng 4·10^9 phép toán, vượt xa giới hạn. 

Quan sát quan trọng là chúng tôi không thực sự quan tâm đến danh tính của người gửi trong quá trình truyền bá, ngoại trừ việc loại họ khỏi nhận thông báo. Thay vì lặp lại tất cả những người tham gia trong mỗi sự kiện, chúng tôi có thể tổng hợp số lượt đóng góp cho mỗi cuộc trò chuyện. 

Đối với mỗi cuộc trò chuyện y, chúng tôi duy trì tổng số tin nhắn được gửi trong đó. Sau đó, đối với bất kỳ nhân viên i nào thuộc nhóm trò chuyện y, họ sẽ nhận được một thông báo cho mỗi tin nhắn trong y, ngoại trừ những tin nhắn mà tôi là người gửi. Điều đó có nghĩa là chúng tôi có thể tách tổng số tin nhắn trong một cuộc trò chuyện khỏi số lần mỗi nhân viên là người gửi trong cuộc trò chuyện đó. 

Vì vậy, chúng tôi lưu giữ hai thông tin: tổng số tin nhắn trên mỗi cuộc trò chuyện và số lượng tin nhắn họ đã gửi riêng cho mỗi nhân viên trong mỗi cuộc trò chuyện. Vì m nhiều nhất là 10 nên việc lưu trữ một bảng n x m là đủ rẻ. 

Sau đó, câu trả lời cho mỗi nhân viên là tổng của tất cả các cuộc trò chuyện mà họ tham gia (total_messages_in_chat − messages_sent_by_employee_in_that_chat). 

Điều này tránh việc lặp lại tất cả các thành viên trong mỗi sự kiện và thay thế nó bằng các cập nhật O(1) cho mỗi sự kiện.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force mỗi sự kiện quét tất cả nhân viên | O(k · n) | O(1) | Quá chậm | 
| Tổng hợp mỗi cuộc trò chuyện với bộ đếm | O(k + n·m) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế rằng các cuộc trò chuyện là nơi chứa tin nhắn độc lập và thông báo chỉ phụ thuộc vào số lượng thành viên và tin nhắn. 

1. Đọc ma trận thành viên. Đối với mỗi nhân viên i và chat y, chúng tôi lưu trữ liệu tôi có thuộc về y hay không. Vì m ≤ 10 nên đây là một bảng boolean nhỏ gọn. 
2. Khởi tạo một mảng Total_chat có kích thước m để lưu trữ bao nhiêu tin nhắn đã được gửi trong mỗi cuộc trò chuyện. 
3. Khởi tạo một mảng đã gửi[i] [y] để lưu trữ bao nhiêu tin nhắn mà cá nhân tôi đã gửi trong cuộc trò chuyện y. 
4. Xử lý từng sự kiện (x, y). Chúng tôi tăng Total_chat[y]. Chúng tôi cũng tăng số lần gửi [x] [y]. Điều này ghi lại cả hoạt động toàn cầu trong cuộc trò chuyện và sự đóng góp của người gửi. 
5. Sau khi xử lý tất cả các sự kiện, tính toán câu trả lời cho từng nhân viên i bằng cách lặp lại tất cả các cuộc trò chuyện y. Nếu nhân viên i thuộc nhóm chat y thì hãy thêm Total_chat[y] − send[i][y] vào câu trả lời của họ. 

Phép trừ rất quan trọng vì Total_chat[y] tính tất cả tin nhắn nhưng nhân viên sẽ không được thông báo về tin nhắn của chính họ. 

### Tại sao nó hoạt động 

Sửa bất kỳ nhân viên i và bất kỳ trò chuyện y. Mỗi tin nhắn trong cuộc trò chuyện y đóng góp chính xác một thông báo cho tôi trừ khi tôi là người gửi tin nhắn đó. Do đó, trên tất cả các tin nhắn trong y, tổng số thông báo mà tôi nhận được là tổng số tin nhắn trong y trừ đi số lượng tin nhắn tôi đã gửi trong y. Tính tổng này một cách độc lập giữa các cuộc trò chuyện sẽ có tác dụng vì thông báo từ các cuộc trò chuyện khác nhau là các sự kiện rời rạc và không tương tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    # membership[i][j] = 1 if employee i is in chat j
    membership = []
    for _ in range(n):
        membership.append(list(map(int, input().split())))

    total = [0] * m
    sent = [[0] * m for _ in range(n)]

    for _ in range(k):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        total[y] += 1
        sent[x][y] += 1

    ans = [0] * n

    for i in range(n):
        res = 0
        for j in range(m):
            if membership[i][j]:
                res += total[j] - sent[i][j]
        ans[i] = res

    print(*ans)

if __name__ == "__main__":
    solve()
```Ma trận thành viên được đọc trực tiếp vào danh sách danh sách để chúng ta có thể truy vấn sự tham gia trò chuyện trong O(1). các`total`mảng theo dõi lượng tin nhắn trên mỗi cuộc trò chuyện, trong khi`sent`tách biệt có bao nhiêu tin nhắn trong số đó thuộc về mỗi nhân viên. 

Bước tích lũy cuối cùng là lúc tính chính xác xuất hiện: đối với mỗi cuộc trò chuyện mà nhân viên tham gia, chúng tôi trừ đi những đóng góp của chính họ khỏi tổng số lượng đóng góp. 

Một điểm tinh tế là lập chỉ mục. Dữ liệu đầu vào dựa trên 1, do đó, cả chỉ số nhân viên và chỉ số trò chuyện đều giảm ngay lập tức để tránh lặp lại lỗi sai sót sau này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 5
1 1 1 1
1 0 1 1
1 1 0 0
1 1
3 1
1 3
2 4
3 2
```Chúng tôi theo dõi tổng số tin nhắn trên mỗi cuộc trò chuyện và mỗi đóng góp của nhân viên. 

| Sự kiện | (x, y) | tổng cộng[trò chuyện] | đã gửi[x][trò chuyện] | 
| --- | --- | --- | --- | 
| 1 | (1,1) | [1,0,0,0] | đã gửi[1][1]=1 | 
| 2 | (3,1) | [2,0,0,0] | đã gửi[3][1]=1 | 
| 3 | (1,3) | [2,0,1,0] | đã gửi[1][3]=1 | 
| 4 | (2,4) | [2,0,1,1] | đã gửi[2][4]=1 | 
| 5 | (3,2) | [2,1,1,1] | đã gửi[3][2]=1 | 

Bây giờ tính toán cho mỗi nhân viên: 

Nhân viên 1: chat 1,2,3,4 

Đóng góp = (2−1) + (1−0) + (1−1) + (1−0) = 1 + 1 + 0 + 1 = 3 

Nhân viên 2: chat 1,3,4 

Đóng góp = (2−0) + (1−0) + (1−1) = 2 + 1 + 0 = 3 

Nhân viên 3: chat 1,2 

Đóng góp = (2−1) + (1−1) = 1 + 0 = 1 

Kết quả đầu ra phù hợp:```
3 3 1
```Dấu vết này xác nhận rằng việc phân tách theo mỗi cuộc trò chuyện sẽ tránh được việc mở rộng theo từng sự kiện một cách chính xác. 

### Ví dụ 2 

Hãy xem xét một trường hợp tối thiểu: 

đầu vào:```
2 1 3
1
1
1 1
2 1
1 1
```Ở đây cả hai nhân viên đều đang trò chuyện. 

| Sự kiện | tổng cộng[1] | đã gửi[1][1] | đã gửi[2][1] | 
| --- | --- | --- | --- | 
| (1,1) | 1 | 1 | 0 | 
| (2,1) | 2 | 1 | 1 | 
| (1,1) | 3 | 2 | 1 | 

Nhân viên 1 nhận được (3−2)=1 thông báo, nhân viên 2 nhận được (3−1)=2 thông báo. 

Điều này nhấn mạnh rằng các tin nhắn lặp lại của cùng một người gửi chỉ làm giảm số lượng tin nhắn nhận được của chính họ chứ không phải những tin nhắn khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m + k + n·m) | Đọc thành viên, xử lý sự kiện, tổng hợp cuối cùng | 
| Không gian | O(n·m) | Lưu trữ cho thành viên và quầy gửi | 

Giải pháp này hiệu quả vì m nhiều nhất là 10, vì vậy n·m là khoảng 200.000 phép tính trong trường hợp xấu nhất, có thể so sánh với k. Điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    n, m, k = map(int, inp.splitlines()[0].split())
    membership = []
    idx = 1
    for _ in range(n):
        membership.append(list(map(int, inp.splitlines()[idx].split())))
        idx += 1

    total = [0] * m
    sent = [[0] * m for _ in range(n)]

    for line in inp.splitlines()[1+n:]:
        if not line.strip():
            continue
        x, y = map(int, line.split())
        total[y-1] += 1
        sent[x-1][y-1] += 1

    ans = [0] * n
    for i in range(n):
        res = 0
        for j in range(m):
            if membership[i][j]:
                res += total[j] - sent[i][j]
        ans[i] = res

    return " ".join(map(str, ans))

# provided sample
assert run("""3 4 5
1 1 1 1
1 0 1 1
1 1 0 0
1 1
3 1
1 3
2 4
3 2
""") == "3 3 1"

# single chat small
assert run("""2 1 3
1
1
1 1
2 1
1 1
""") == "1 2"

# minimum size
assert run("""2 1 1
1
1
1 1
""") == "0 1"

# all separate chats
assert run("""3 3 3
1 0 0
0 1 0
0 0 1
1 1
2 2
3 3
""") == "0 0 0"

# dense activity
assert run("""4 2 4
1 1
1 1
1 1
1 1
1 1
2 1
3 1
4 2
""") == "3 3 3 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người gửi lặp lại trò chuyện đơn lẻ | 1 2 | xử lý loại trừ tự tin nhắn | 
| kích thước tối thiểu | 0 1 | độ đúng cơ sở | 
| cuộc trò chuyện rời rạc | 0 0 0 | sự độc lập giữa các cuộc trò chuyện | 
| hoạt động dày đặc | số lượng bằng nhau | hành vi trò chuyện chia sẻ lớn | 

## Vỏ cạnh 

Một cuộc trò chuyện dày đặc mà hầu hết mọi nhân viên đều tham gia nhấn mạnh sự cần thiết phải tránh việc quét theo từng sự kiện. Giả sử 20.000 nhân viên đều thuộc về một cuộc trò chuyện và có 200.000 tin nhắn. Một mô phỏng đơn giản sẽ yêu cầu 4 tỷ bản cập nhật. Thay vào đó, phương pháp được tối ưu hóa thực hiện hai lần tăng cho mỗi sự kiện và một lần trừ cuối cùng cho mỗi nhân viên. 

Việc tự đăng bài nhiều lần là một trường hợp tinh vi khác. Nếu một nhân viên gửi tất cả tin nhắn trong cuộc trò chuyện thì mọi người tham gia khác vẫn nhận được tất cả thông báo nhưng người gửi đó sẽ không nhận được thông báo nào. Công thức Total_chat − đã gửi đảm bảo điều này một cách tự động: tổng số lần gửi của họ khớp với tổng số, không mang lại đóng góp nào từ cuộc trò chuyện đó. 

Tư cách thành viên thưa thớt được xử lý một cách tự nhiên vì vòng lặp cuối cùng chỉ xem xét các cuộc trò chuyện có tư cách thành viên là 1, do đó tránh được phép trừ không cần thiết.
