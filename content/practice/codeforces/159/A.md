---
title: "CF 159A - Bạn bè hay không"
description: "Chúng tôi được cung cấp nhật ký theo trình tự thời gian về các tin nhắn riêng tư giữa những người dùng trong mạng xã hội. Mỗi bản ghi chứa người gửi, người nhận và dấu thời gian của tin nhắn. Hai người dùng trở thành bạn bè nếu một trong số họ trả lời tin nhắn của người kia đủ nhanh."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 159
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2012 Qualification Round 2"
rating: 1400
weight: 159
solve_time_s: 107
verified: true
draft: false
---

[CF 159A - Bạn bè hay không](https://codeforces.com/problemset/problem/159/A) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, tham lam, thực hiện 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhật ký theo trình tự thời gian về các tin nhắn riêng tư giữa những người dùng trong mạng xã hội. Mỗi bản ghi chứa người gửi, người nhận và dấu thời gian của tin nhắn. 

Hai người dùng trở thành bạn bè nếu một trong số họ trả lời tin nhắn của người kia đủ nhanh. Chính xác hơn, giả sử người dùng`A`gửi tin nhắn cho người dùng`B`vào thời điểm đó`t1`, và người dùng sau này`B`gửi tin nhắn cho người dùng`A`vào thời điểm đó`t2`. Nếu như`0 < t2 - t1 <= d`, thì tin nhắn thứ hai được tính là thư trả lời cho tin nhắn đầu tiên. Ngay khi có ít nhất một câu trả lời như vậy tồn tại giữa hai người dùng, họ được coi là bạn bè. 

Nhiệm vụ là tìm ra mọi cặp tình bạn có thể hình thành từ lịch sử tin nhắn. 

Kích thước đầu vào nhỏ. Số lượng tin nhắn nhiều nhất`1000`, vì vậy ngay cả một`O(n^2)`giải pháp thực hiện tối đa một triệu kiểm tra cặp, hoàn toàn an toàn trong giới hạn thời gian 3 giây. Không cần cấu trúc dữ liệu nâng cao hoặc thủ thuật tối ưu hóa. 

Thách thức chính là xử lý chính xác điều kiện trả lời. Các tin nhắn đã được sắp xếp theo thời gian nhưng dấu thời gian có thể bằng nhau và chỉ những tin nhắn hoàn toàn muộn hơn mới có thể được tính là thư trả lời. Sự khác biệt bằng 0 là không hợp lệ. 

Một sai lầm dễ mắc phải là quên hướng trả lời. 

Hãy xem xét đầu vào này:```
2 5
alice bob 1
alice bob 3
```Đầu ra đúng:```
0
```Cả hai tin nhắn đều đi từ`alice`ĐẾN`bob`. Một câu trả lời yêu cầu hướng đảo ngược. 

Một trường hợp tế nhị khác là sự bất bình đẳng nghiêm ngặt về thời gian.```
2 5
alice bob 10
bob alice 10
```Đầu ra đúng:```
0
```Dấu thời gian bằng nhau, vì vậy`t2 - t1 = 0`, không thỏa mãn`0 < t2 - t1`. 

Cái bẫy thứ ba là in các cặp tình bạn trùng lặp.```
4 10
alice bob 1
bob alice 2
alice bob 3
bob alice 4
```Đầu ra đúng:```
1
alice bob
```Mặc dù tồn tại một số câu trả lời hợp lệ nhưng cặp này phải xuất hiện đúng một lần. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force kiểm tra mọi cặp tin nhắn được sắp xếp. 

Giả sử tin nhắn`i`là:```
A -> B at time t1
```và tin nhắn`j`là:```
B -> A at time t2
```Nếu như`i < j`Và`0 < t2 - t1 <= d`, thì tin nhắn thứ hai là một câu trả lời hợp lệ, vì vậy`A`Và`B`trở thành bạn bè. 

Phương pháp này đúng vì định nghĩa tình bạn chỉ phụ thuộc vào các cặp tin nhắn. Bằng cách kiểm tra mọi cặp có thể, chúng tôi không thể bỏ lỡ bất kỳ tương tác hợp lệ nào. 

Trường hợp xấu nhất thực hiện khoảng`1000 × 1000 = 1,000,000`so sánh, có thể dễ dàng đủ nhanh. 

Quan sát quan trọng là các ràng buộc đủ nhỏ để ý tưởng vũ phu đã tối ưu trong thực tế. Không có lý do gì để làm phức tạp giải pháp bằng cửa sổ trượt hoặc theo dõi dấu thời gian dựa trên hàm băm. 

Chi tiết bổ sung duy nhất là cách lưu trữ các cặp tình bạn mà không bị trùng lặp. Vì tình bạn là đối xứng,`(alice, bob)`Và`(bob, alice)`thể hiện mối quan hệ tương tự. Chúng ta có thể chuẩn hóa từng cặp bằng cách sắp xếp hai tên theo thứ tự bảng chữ cái trước khi chèn chúng vào một bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(k) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(k) | Đã chấp nhận | 

Đây,`k`là số cặp bạn bè. 

## Hướng dẫn thuật toán 

1. Đọc tất cả tin nhắn vào một mảng. 
2. Tạo một tập hợp trống có tên`friends`. 

Bộ này tự động loại bỏ các cặp tình bạn trùng lặp. 
3. Lặp lại từng cặp tin nhắn`(i, j)`như vậy`j > i`. 

Vì nhật ký được sắp xếp theo trình tự thời gian nên thông báo`j`không bao giờ sớm hơn tin nhắn`i`. 
4. Hãy nhắn tin`i`là`(a1, b1, t1)`và tin nhắn`j`là`(a2, b2, t2)`. 
5. Kiểm tra xem hướng có bị đảo ngược hay không. 

Chúng tôi cần:```
a1 == b2
b1 == a2
```Điều này có nghĩa là tin nhắn thứ hai sẽ được gửi lại cho người gửi ban đầu. 
6. Kiểm tra xem chênh lệch múi giờ có thỏa mãn không:```
0 < t2 - t1 <= d
```Sự khác biệt phải hoàn toàn tích cực vì các phản hồi không thể xảy ra cùng một lúc. 
7. Nếu cả hai điều kiện đều đúng, hãy lắp cặp đã chuẩn hóa vào bộ. 

Chúng tôi lưu trữ:```
tuple(sorted([a1, b1]))
```để cả hai hướng đều hướng tới cùng một tình bạn. 
8. Sau khi xử lý tất cả các cặp, hãy in kích thước của bộ và từng cặp tình bạn. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cặp thông báo sớm hơn và muộn hơn có thể. Một tình bạn tồn tại khi và chỉ khi có ít nhất một cặp thỏa mãn điều kiện trả lời. Vì không có cặp nào bị bỏ qua nên mọi tình bạn hợp lệ đều được phát hiện. 

Bước chuẩn hóa đảm bảo rằng tình bạn giống nhau được thể hiện duy nhất bất kể hướng thông điệp. Vì kết quả được lưu trữ trong một tập hợp nên các câu trả lời hợp lệ lặp đi lặp lại giữa những người dùng giống nhau không thể tạo ra các dòng đầu ra trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())

    messages = []

    for _ in range(n):
        a, b, t = input().split()
        messages.append((a, b, int(t)))

    friends = set()

    for i in range(n):
        a1, b1, t1 = messages[i]

        for j in range(i + 1, n):
            a2, b2, t2 = messages[j]

            if a1 == b2 and b1 == a2:
                diff = t2 - t1

                if 0 < diff <= d:
                    friends.add(tuple(sorted((a1, b1))))

    print(len(friends))

    for a, b in friends:
        print(a, b)

solve()
```Giải pháp bắt đầu bằng cách lưu trữ tất cả các tin nhắn trong một danh sách vì mỗi tin nhắn có thể cần được so sánh với mọi tin nhắn sau này. 

Các vòng lặp lồng nhau tự động thực thi thứ tự thời gian. Từ`j > i`, tin nhắn thứ hai không bao giờ sớm hơn tin nhắn đầu tiên. 

Việc kiểm tra chỉ đạo là phần quan trọng nhất của việc thực hiện. Chúng tôi chỉ chấp nhận những tin nhắn mà người gửi và người nhận hoán đổi vị trí cho nhau. Hai tin nhắn cùng hướng không thể tạo thành một cặp phản hồi. 

Điều kiện thời gian sử dụng tính tích cực nghiêm ngặt:```
0 < diff <= d
```sử dụng`diff >= 0`sẽ chấp nhận sai các tin nhắn đồng thời. 

Cặp tình bạn được chuẩn hóa trước khi đưa vào tập hợp. Nếu không sắp xếp, cả hai`(alice, bob)`Và`(bob, alice)`có thể xuất hiện riêng tùy thuộc vào câu trả lời nào được tìm thấy đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
vasya petya 1
petya vasya 2
anya ivan 2
ivan anya 4
```Dấu vết: 

| tôi | j | Nhắn tin cho tôi | Tin nhắn j | Đảo ngược hướng? | Chênh lệch thời gian | Đã thêm tình bạn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | vasya→petya @1 | petya→vasya @2 | Có | 1 | (petya, vasya) | 
| 0 | 2 | vasya→petya @1 | Anya→ivan @2 | Không | - | Không | 
| 0 | 3 | vasya→petya @1 | ivan→anyya @4 | Không | - | Không | 
| 1 | 2 | petya→vasya @2 | Anya→ivan @2 | Không | - | Không | 
| 1 | 3 | petya→vasya @2 | ivan→anyya @4 | Không | - | Không | 
| 2 | 3 | Anya→ivan @2 | ivan→anyya @4 | Có | 2 | Không | 

Cặp đầu tiên tạo thành một câu trả lời hợp lệ vì hướng đảo ngược và sự khác biệt chính xác là`d`. Ứng cử viên thứ hai thất bại vì độ trễ quá lớn. 

### Ví dụ 2 

đầu vào:```
4 10
alice bob 1
bob alice 2
alice bob 3
bob alice 4
```Dấu vết: 

| tôi | j | Nhắn tin cho tôi | Tin nhắn j | Đảo ngược hướng? | Chênh lệch thời gian | Đã thêm tình bạn | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | alice→bob @1 | bob→alice @2 | Có | 1 | (alice, bob) | 
| 0 | 2 | alice→bob @1 | alice→bob @3 | Không | - | Không | 
| 0 | 3 | alice→bob @1 | bob→alice @4 | Có | 3 | Đã tồn tại | 
| 1 | 2 | bob→alice @2 | alice→bob @3 | Có | 1 | Đã tồn tại | 
| 1 | 3 | bob→alice @2 | bob→alice @4 | Không | - | Không | 
| 2 | 3 | alice→bob @3 | bob→alice @4 | Có | 1 | Đã tồn tại | 

Dấu vết này cho thấy lý do tại sao một bộ là cần thiết. Có nhiều cặp phản hồi hợp lệ nhưng chỉ có một tình bạn xuất hiện ở đầu ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cặp tin nhắn được kiểm tra một lần | 
| Không gian | O(k) | Bộ tình bạn lưu trữ nhiều nhất`k`cặp độc đáo | 

Với`n ≤ 1000`, quá trình quét bậc hai thực hiện khoảng một triệu so sánh trong trường hợp xấu nhất, điều này không đáng kể đối với Python trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, d = map(int, input().split())

        messages = []

        for _ in range(n):
            a, b, t = input().split()
            messages.append((a, b, int(t)))

        friends = set()

        for i in range(n):
            a1, b1, t1 = messages[i]

            for j in range(i + 1, n):
                a2, b2, t2 = messages[j]

                if a1 == b2 and b1 == a2:
                    diff = t2 - t1

                    if 0 < diff <= d:
                        friends.add(tuple(sorted((a1, b1))))

        out = [str(len(friends))]

        for a, b in sorted(friends):
            out.append(f"{a} {b}")

        return "\n".join(out)

    return solve()

# provided sample
assert run(
    "4 1\n"
    "vasya petya 1\n"
    "petya vasya 2\n"
    "anya ivan 2\n"
    "ivan anya 4\n"
) == (
    "1\n"
    "petya vasya"
), "sample 1"

# minimum-size input
assert run(
    "1 5\n"
    "alice bob 1\n"
) == (
    "0"
), "single message cannot form friendship"

# equal timestamps should not count
assert run(
    "2 5\n"
    "alice bob 10\n"
    "bob alice 10\n"
) == (
    "0"
), "difference must be strictly positive"

# multiple replies but only one friendship
assert run(
    "4 10\n"
    "alice bob 1\n"
    "bob alice 2\n"
    "alice bob 3\n"
    "bob alice 4\n"
) == (
    "1\n"
    "alice bob"
), "duplicate friendships must be removed"

# boundary condition diff == d
assert run(
    "2 3\n"
    "a b 5\n"
    "b a 8\n"
) == (
    "1\n"
    "a b"
), "difference equal to d is valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tin nhắn đơn | 0 tình bạn | Hành vi kích thước tối thiểu | 
| Dấu thời gian bằng nhau | 0 tình bạn | Bất bình đẳng nghiêm ngặt`t2 > t1`| 
| Nhiều câu trả lời hợp lệ | Chỉ một tình bạn | Loại bỏ trùng lặp | 
| Sự khác biệt chính xác`d`| Tình bạn tồn tại | Bao gồm giới hạn trên | 

## Vỏ cạnh 

Xem xét các tin nhắn đồng thời:```
2 5
alice bob 10
bob alice 10
```Thuật toán so sánh hai tin nhắn và tính toán:```
diff = 10 - 10 = 0
```Vì điều kiện là`0 < diff <= d`, cặp này bị từ chối một cách chính xác. Việc thực hiện bất cẩn bằng cách sử dụng`diff >= 0`sẽ đánh dấu họ là bạn bè một cách không chính xác. 

Bây giờ hãy xem xét các cuộc trò chuyện lặp đi lặp lại:```
4 10
alice bob 1
bob alice 2
alice bob 3
bob alice 4
```Thuật toán phát hiện ra tình bạn nhiều lần, nhưng mỗi lần chèn đều sử dụng:```
tuple(sorted((a1, b1)))
```luôn tạo ra:```
(alice, bob)
```Vì kết quả được lưu trữ trong một tập hợp nên các kết quả trùng lặp sẽ tự động biến mất. 

Cuối cùng, hãy xem xét các thông điệp chỉ theo một hướng:```
3 10
alice bob 1
alice bob 2
alice bob 3
```Mọi so sánh đều thất bại trong việc kiểm tra hướng ngược lại:```
a1 == b2 and b1 == a2
```nên không có tình bạn nào được thêm vào. Đầu ra là chính xác:```
0
```
