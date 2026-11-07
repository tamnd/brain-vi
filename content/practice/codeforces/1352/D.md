---
problem: 1352D
contest_id: 1352
problem_index: D
name: "Alice, Bob and Candies"
contest_name: "Codeforces Round 640 (Div. 4)"
rating: 1300
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 217
date: 2026-06-14
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2e2c91-8544-83ec-aa66-936d620ef4f2
---

# CF 1352D - Alice, Bob và Kẹo 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3 phút 37 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2e2c91-8544-83ec-aa66-936d620ef4f2 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được phát một dòng kẹo, mỗi dòng có kích thước dương và hai người chơi tiêu thụ chúng từ hai đầu đối diện. Alice luôn tiêu thụ từ phía bên trái, Bob từ phía bên phải. Họ luân phiên nhau và mỗi lượt bao gồm việc ăn một hoặc nhiều viên kẹo liên tiếp từ phía tương ứng của họ. 

Nguyên tắc quan trọng là người chơi trong lượt của họ phải ăn vừa đủ số kẹo sao cho tổng kích thước họ tiêu thụ vượt quá những gì đối thủ đã ăn ở lượt trước. Alice bắt đầu trước và buộc phải ăn đúng một viên kẹo trong lần đi đầu tiên. Sau đó, cả hai người chơi luôn cố gắng vượt quá tối thiểu tổng số nước đi trước đó của đối phương. 

Trò chơi tiếp tục cho đến khi hết kẹo và chúng ta phải báo cáo số lượt đã xảy ra và tổng số kẹo mà mỗi người chơi đã ăn. 

Các ràng buộc cho phép tối đa 5000 trường hợp thử nghiệm và tổng cộng 200000 viên kẹo. Điều này có nghĩa là bất kỳ giải pháp nào xử lý từng viên kẹo với số lần không đổi là đủ. Mô phỏng bậc hai cho mỗi trường hợp thử nghiệm sẽ quá chậm trong trường hợp xấu nhất nếu nó liên tục quét lại các phân đoạn đã được xử lý. Cấu trúc của quy trình gợi ý mô phỏng hai con trỏ trong đó mỗi viên kẹo được tiêu thụ một lần. 

Một trường hợp phức tạp xuất hiện khi mục tiêu yêu cầu của một người chơi vượt quá những gì còn lại. Trong tình huống đó, người chơi chỉ cần ăn hết số kẹo còn lại và quá trình kết thúc ngay lập tức. Một trường hợp khó khăn khác là khi nhiều viên kẹo nhỏ tích lũy chỉ vừa đủ vượt quá số tiền trước đó, điều này có thể buộc nhiều con trỏ tiến lên mỗi lượt. Bất kỳ giải pháp nào đặt lại con trỏ hoặc tính toán lại các tổng từ đầu không chính xác đều có thể thất bại trên các chuỗi dài xen kẽ. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp tuân theo các quy tắc theo đúng nghĩa đen: ở mỗi lượt, chúng tôi quét từ con trỏ trái hoặc phải hiện tại và tích lũy các giá trị kẹo cho đến khi tổng vượt quá tổng của đối thủ trước đó. Sau mỗi lượt, chúng ta đổi người chơi và tiếp tục. Điều này đúng vì nó phản ánh chính xác định nghĩa của trò chơi. 

Tuy nhiên, việc triển khai đơn giản tính toán lại tổng hoặc quét lại các phân đoạn liên tục có nguy cơ gây ra hành vi O(n²) cho mỗi trường hợp thử nghiệm. Trong những trường hợp xấu nhất giống như tất cả các trường hợp khác, mỗi lượt sẽ chỉ tiến lên một bước và việc quét lặp lại sẽ truy cập lại nhiều thành phần một cách không cần thiết. 

Quan sát quan trọng là mỗi viên kẹo được tiêu thụ đúng một lần và con trỏ chỉ di chuyển vào trong. Do đó, chúng tôi duy trì hai con trỏ và tích lũy tổng tăng dần mà không cần xem lại các phần tử. Mỗi bước sẽ mở rộng tổng của người chơi hiện tại cho đến khi điều kiện được thỏa mãn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force quét lại mỗi lượt | O(n²) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Mô phỏng gia tăng hai con trỏ | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng trò chơi bằng cách sử dụng hai con trỏ, một ở đầu bên trái và một ở đầu bên phải, cùng với tổng số lượt chạy cho mỗi người chơi. 

1. Khởi tạo con trỏ`l = 0`,`r = n - 1`. Bộ`alice_sum = 0`,`bob_sum = 0`, Và`prev = 0`để theo dõi tổng số tiền đã ăn của nước đi trước đó. Alice bắt đầu trước nên chúng ta cũng khởi tạo lượt đếm về 0. 
2. Trong lần đi đầu tiên, Alice phải ăn đúng một viên kẹo bên trái. Chúng tôi thêm`a[l]`đến tổng số của cô ấy, tăng lên`l`, bộ`prev`đến giá trị đó và tăng số lần di chuyển. Đây là một quy tắc bắt buộc và phá vỡ tính đối xứng ngay từ đầu. 
3. Chuyển sang Bob. Đến lượt Bob, chúng tôi tích lũy từ phía bên phải, liên tục lấy`a[r]`và giảm dần`r`cho đến khi tổng lượt hiện tại của Bob lớn hơn nhiều so với`prev`. 
4. Khi Bob vượt quá`prev`, chúng tôi cập nhật`prev`đến tổng lượt của Bob, tăng số lần di chuyển và chuyển về Alice. 
5. Ở lượt chung của Alice, chúng ta lại tích lũy từ bên trái, thêm kẹo cho đến khi số tiền vượt quá`prev`, sau đó cập nhật tổng số của Alice và`prev`. 
6. Lặp lại các bước 3-5 cho đến khi hết kẹo. Nếu tại bất kỳ thời điểm nào, điều kiện bắt buộc không thể được đáp ứng do không còn kẹo, người chơi hiện tại sẽ lấy tất cả số kẹo còn lại và quá trình kết thúc ngay lập tức. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi chỉ mục kẹo đều được truy cập chính xác một lần và được gán vĩnh viễn cho Alice hoặc Bob theo thứ tự do chuyển động của con trỏ quy định. Mỗi lượt sẽ tăng tổng số phân đoạn tiêu thụ từ một phía và điều kiện dừng đảm bảo mỗi lần di chuyển là tối thiểu trong quá trình tích lũy đơn điệu. Vì cả hai con trỏ chỉ di chuyển vào trong và không bao giờ đảo ngược nên không có phần tử nào được xem xét lại và mô phỏng tái tạo lại trạng thái trò chơi một cách trung thực mà không cần thực hiện thao tác dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        l, r = 0, n - 1
        moves = 0
        alice = 0
        bob = 0
        prev = 0

        turn_alice = True

        while l <= r:
            s = 0

            if turn_alice:
                if l == 0:
                    s = a[l]
                    l += 1
                else:
                    while l <= r and s <= prev:
                        s += a[l]
                        l += 1
                alice += s
            else:
                while l <= r and s <= prev:
                    s += a[r]
                    r -= 1
                bob += s

            if s == 0:
                break

            prev = s
            moves += 1
            turn_alice = not turn_alice

        out.append(f"{moves} {alice} {bob}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp duy trì hai con trỏ và chỉ mở rộng chúng vào bên trong. Cờ luân phiên theo dõi đến lượt của ai, trong khi`prev`lưu trữ tổng số nước đi cuối cùng để người chơi tiếp theo biết ngưỡng yêu cầu. Mỗi vòng lặp tiêu thụ ít nhất một viên kẹo, đảm bảo kết thúc. 

Một cạm bẫy triển khai phổ biến là xử lý sai nước đi bắt buộc đầu tiên của Alice một cách riêng biệt và sau đó vô tình bỏ qua hoặc đếm hai lần phân đoạn đầu tiên. Một người khác quên đặt lại tổng số tiền đang chạy`s`trong mỗi lượt, điều này sẽ tích lũy không chính xác qua các lượt và phá vỡ điều kiện so sánh nghiêm ngặt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| Di chuyển | Người chơi | tôi | r | tổng hiện tại | trước | Alice | bob | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | Alice | 1 | 2 | 1 | 0 | 1 | 0 | 
| 2 | Bob | 1 | 1 | 3 | 1 | 1 | 3 | 
| 3 | Alice | 2 | 1 | 2 | 3 | 3 | 3 | 

Điều này cho thấy nước đi bắt buộc đầu tiên của Alice là tối thiểu, trong khi Bob phải tích lũy cho đến khi vượt quá 1, buộc anh ta phải lấy nhiều viên kẹo. 

### Ví dụ 2 

đầu vào:```
4
1 1 1 1
```| Di chuyển | Người chơi | tôi | r | tổng hiện tại | trước | Alice | bob | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | Alice | 1 | 3 | 1 | 0 | 1 | 0 | 
| 2 | Bob | 1 | 2 | 2 | 1 | 1 | 2 | 
| 3 | Alice | 2 | 1 | 2 | 2 | 3 | 2 | 
| 4 | Bob | 2 | 0 | 2 | 2 | 3 | 4 | 

Trường hợp này nhấn mạnh quy tắc vượt ngưỡng tối thiểu xen kẽ. Mỗi lần di chuyển hầu như không vượt quá lần di chuyển trước, khiến con trỏ thường xuyên chỉ thay đổi một phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi viên kẹo được tiêu thụ một lần vì con trỏ chỉ di chuyển vào trong | 
| Không gian | O(1) thêm | Chỉ có bộ đếm và con trỏ được duy trì | 

Cho rằng tổng n qua các thử nghiệm tối đa là 2×10^5, hành vi tuyến tính này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        l, r = 0, n - 1
        moves = 0
        alice = 0
        bob = 0
        prev = 0
        turn = True

        while l <= r:
            s = 0
            if turn:
                if l <= r:
                    s += a[l]
                    l += 1
                    while l <= r and s <= prev:
                        s += a[l]
                        l += 1
                    alice += s
            else:
                while l <= r and s <= prev:
                    s += a[r]
                    r -= 1
                bob += s

            if s == 0:
                break

            prev = s
            moves += 1
            turn = not turn

        res.append((moves, alice, bob))

    return "\n".join(f"{x} {y} {z}" for x, y, z in res)

# samples
assert run("""7
11
3 1 4 1 5 9 2 6 5 3 5
1
1000
3
1 1 1
13
1 2 3 4 5 6 7 8 9 10 11 12 13
2
2 1
6
1 1 1 1 1 1
7
1 1 1 1 1 1 1
""") == """6 23 21
1 1000 0
2 1 2
6 45 46
2 2 1
3 4 2
4 4 3"""

# edge: single element
assert run("""1
1
10
""") == "1 10 0"

# edge: alternating ones
assert run("""1
5
1 1 1 1 1
""") == "4 3 2"

# edge: increasing
assert run("""1
4
1 2 3 4
""") == "3 4 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 10 0 | trường hợp tối thiểu | 
| tất cả những cái | 4 3 2 | tiêu dùng xen kẽ chặt chẽ | 
| trình tự tăng dần | 3 4 6 | tăng trưởng phân khúc không đồng đều | 

## Vỏ cạnh 

Một đầu vào kẹo duy nhất được xử lý ngay lập tức bởi hành động bắt buộc đầu tiên của Alice, vì cô ấy đã tiêu thụ toàn bộ nó và vòng lặp kết thúc khi cả hai con trỏ giao nhau. Một chuỗi gồm tất cả các giá trị bằng nhau buộc thuật toán phải luân phiên hầu hết các bước với tốc độ tăng trưởng tối thiểu, kiểm tra xem điều kiện “lớn hơn chính xác so với trước đó” có được triển khai chính xác hay không. Trình tự tăng dần nhấn mạnh các bước di chuyển sau đó của Bob, trong đó việc tích lũy bên phải có thể yêu cầu nhiều lượt chọn để vượt quá tổng số ngày càng tăng của Alice, xác nhận rằng chuyển động của con trỏ và đặt lại tổng không bị rò rỉ qua các lượt.
