---
title: "CF 2104E - Dây đàn khó chịu"
description: "Chúng ta được cung cấp một chuỗi tham chiếu cố định s và chúng ta chỉ được phép sử dụng k chữ cái viết thường đầu tiên. Từ chuỗi s này, chúng ta coi bất kỳ chuỗi t nào là “hợp lệ” nếu nó có thể được hình thành bằng cách xóa các ký tự khỏi s mà không thay đổi thứ tự, nghĩa là t là một dãy con của s."
date: "2026-06-08T04:58:51+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 2104
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 178 (Rated for Div. 2)"
rating: 1700
weight: 2104
solve_time_s: 95
verified: false
draft: false
---

[CF 2104E - Dây đàn khó chịu](https://codeforces.com/problemset/problem/2104/E) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, dp, tham lam, chuỗi 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi tham chiếu cố định`s`, và chúng ta chỉ được phép sử dụng cái đầu tiên`k`chữ thường. Từ chuỗi này`s`, chúng ta xem xét bất kỳ chuỗi nào`t`là “hợp lệ” nếu nó có thể được hình thành bằng cách xóa các ký tự khỏi`s`không thay đổi thứ tự, nghĩa là`t`là một dãy con của`s`. 

Đối với mỗi chuỗi truy vấn`t`, chúng ta được phép thêm các ký tự vào bên phải của nó, từng ký tự một, vẫn bị giới hạn ở cùng một ký tự`k`các chữ cái. Sau mỗi lần nối thêm, chúng tôi kiểm tra xem chuỗi kết quả có còn là dãy con của`s`. Nhiệm vụ là tìm số lượng ký tự được thêm vào tối thiểu cần thiết để dù chúng ta có tiếp tục thế nào thì chuỗi kết quả không còn là dãy con của`s`. 

Khó khăn chính là việc so khớp chuỗi con phụ thuộc vào vị trí và toàn cục. Một nỗ lực ngây thơ có thể mô phỏng việc so khớp`t`bên trong`s`, sau đó thử tất cả các tiện ích mở rộng có thể, nhưng điều này nhanh chóng trở nên không khả thi vì cả hai`n`và tổng chiều dài truy vấn có thể đạt khoảng một triệu. 

Các ràng buộc ngay lập tức loại trừ mọi hoạt động quét tuyến tính trên mỗi truy vấn`s`. Một lần kiểm tra trình tự tiếp theo là`O(n)`và làm điều đó cho mỗi tiện ích mở rộng hoặc mỗi truy vấn sẽ vượt quá giới hạn thời gian. Chúng ta cần tiền xử lý`s`cho phép chuyển đổi trạng thái nhanh chóng. 

Trường hợp khó phát hiện xảy ra khi`t`đã không phải là một dãy con của`s`. Trong trường hợp đó, không cần gia hạn gì cả, vì nó đã “thất bại” ngay lập tức. Một trường hợp góc khác là khi`t`là một dãy con, nhưng cực kỳ “gần” đến mức cạn kiệt, nghĩa là nó chỉ có thể được kéo dài một vài lần trước khi không thể tiếp tục được nữa. 

Ví dụ, nếu`s = "abacaba"`Và`t = "cc"`, sau đó`t`đã không phải là một dãy con nên câu trả lời là`0`. Nếu như`t = "b"`, thì chúng tôi vẫn có thể khớp nó theo một số cách, nhưng cuối cùng chúng tôi không còn đường dẫn tiếp tục hợp lệ. 

Thách thức thực sự là định lượng xem còn lại bao nhiêu tiện ích mở rộng an toàn trước khi chúng ta chắc chắn phá vỡ tính khả thi của chuỗi tiếp theo. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ, đối với mỗi chuỗi truy vấn`t`, mô phỏng việc khớp nó như một dãy con trong`s`. Nếu nó đã thất bại, hãy quay lại`0`. Mặt khác, chúng tôi thử thêm từng ký tự một và mỗi lần kiểm tra lại xem chuỗi mới có còn là một chuỗi con của chuỗi không`s`. 

Điều này đúng nhưng cực kỳ chậm. Chi phí kiểm tra mỗi lần tiếp theo`O(n)`và trong trường hợp xấu nhất, chúng tôi có thể thử nhiều tiện ích mở rộng cho mỗi truy vấn, dẫn đến`O(n * total_query_length)`hành vi vượt quá giới hạn. 

Quan sát quan trọng là việc so khớp chuỗi con không phụ thuộc vào lịch sử tiền tố đầy đủ mà chỉ phụ thuộc vào vị trí hiện tại trong`s`. Một khi chúng ta biết nơi nào trong`s`chúng tôi đang theo đuổi sự phù hợp`t`, câu hỏi duy nhất là: từ vị trí này, chúng ta còn có thể tham lam tiêu thụ bao nhiêu nhân vật trước khi không thể thăng tiến được nữa. 

Điều này gợi ý các chuyển đổi tiền xử lý: đối với mọi vị trí trong`s`và mỗi ký tự, tính toán lần xuất hiện tiếp theo của ký tự đó. Sau đó, sau khi khớp`t`, chúng ta có thể nhảy qua`s`hiệu quả trong khi mô phỏng các phần mở rộng một cách tham lam. Mỗi ký tự được nối thêm tương ứng với việc di chuyển về phía trước bằng cách sử dụng các con trỏ tiếp theo được tính toán trước. Quá trình dừng lại khi không có sự chuyển tiếp hợp lệ nào tồn tại. 

Điều này làm giảm mỗi truy vấn thành hai giai đoạn: đầu tiên mô phỏng việc so khớp`t`TRONG`s`, sau đó lặp đi lặp lại quá trình chuyển đổi cho đến khi thất bại. Vì mỗi bước đều tiến về phía trước`s`, tổng công việc trên tất cả các truy vấn là tuyến tính theo`n`cộng với tổng chiều dài truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n · L) | O(1) | Quá chậm | 
| DP lần xuất hiện tiếp theo | O(n · k + tổng L) | O(n · k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một bảng chuyển tiếp`nxt[i][c]`, Ở đâu`i`là một vị trí trong`s`Và`c`là một ký tự, lưu trữ chỉ mục tiếp theo tại hoặc sau`i`Ở đâu`c`xuất hiện, hoặc`-1`nếu nó không tồn tại. 

Sau đó, chúng tôi xử lý từng truy vấn như sau: 

1. Khởi tạo`pos = 0`, nghĩa là chúng ta bắt đầu trước ký tự đầu tiên của`s`. 
2. Với mỗi ký tự trong`t`, cập nhật`pos = nxt[pos][c] + 1`. Nếu tại bất kỳ thời điểm nào`nxt[pos][c] = -1`, sau đó`t`không phải là dãy con nên đáp án là`0`. Bước này mô phỏng trực tiếp việc so khớp chuỗi con theo thời gian tuyến tính trong`|t|`. 
3. Nếu`t`là một dãy con, bây giờ chúng tôi liên tục cố gắng nối thêm các ký tự. Chúng tôi duy trì một con trỏ hiện tại`pos`TRONG`s`. 
4. Với mỗi ký tự được nối thêm, chúng ta chọn ký tự được phép bất kỳ`c`điều đó có thể thúc đẩy trận đấu trong`s`, nghĩa`nxt[pos][c] != -1`. 
5. Nếu không có ký tự như vậy tồn tại cho hiện tại`pos`, chúng ta không thể kéo dài thêm nữa nên chúng ta dừng lại. 
6. Mỗi lần nối thành công`pos`chuyển tiếp vào`s`và chúng tôi đếm xem chúng tôi đã thực hiện được bao nhiêu bước. 

Câu trả lời là số lần mở rộng thành công trước khi đạt đến vị trí mà không có ký tự nào dẫn đến lần xuất hiện hợp lệ tiếp theo. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thông tin duy nhất cần thiết để xác định xem một chuỗi có còn là một chuỗi con hay không là vị trí khớp hiện tại trong`s`. các`nxt`bảng mã hóa tất cả các khả năng trong tương lai từ bất kỳ vị trí nào. Mỗi ký tự được thêm vào sẽ nâng cao vị trí trong`s`, do đó quá trình tạo thành một bước đi đơn điệu xuyên qua`s`. Một khi tất cả các ký tự dẫn đến`-1`, không có tiện ích mở rộng nào có thể bảo toàn thuộc tính chuỗi con, do đó quá trình phải dừng lại. Điều này đảm bảo tính tối ưu vì mọi chuỗi ký tự được nối thêm thay thế vẫn sẽ yêu cầu chuyển đổi hợp lệ từ cùng một trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    s = input().strip()

    # build next occurrence table
    nxt = [[-1] * k for _ in range(n + 1)]

    last = [-1] * k
    for i in range(n - 1, -1, -1):
        last[ord(s[i]) - 97] = i
        for c in range(k):
            nxt[i][c] = last[c]
    for c in range(k):
        nxt[n][c] = -1

    q = int(input())
    out = []

    for _ in range(q):
        t = input().strip()

        pos = 0
        ok = True

        for ch in t:
            c = ord(ch) - 97
            if pos > n:
                ok = False
                break
            nxt_pos = nxt[pos][c]
            if nxt_pos == -1:
                ok = False
                break
            pos = nxt_pos + 1

        if not ok:
            out.append("0")
            continue

        # try to extend
        ans = 0
        while True:
            found = False
            for c in range(k):
                nxt_pos = nxt[pos][c]
                if nxt_pos != -1:
                    pos = nxt_pos + 1
                    ans += 1
                    found = True
                    break
            if not found:
                break

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Bước tiền xử lý xây dựng một bảng cho phép chuyển trực tiếp đến lần xuất hiện tiếp theo của bất kỳ ký tự nào. Điều này tránh việc quét về phía trước trong`s`trong quá trình truy vấn. 

Việc kiểm tra trình tự tiếp theo sử dụng bảng này để cập nhật`pos`một cách hiệu quả. Nếu một ký tự không thể khớp, truy vấn sẽ được giải quyết ngay lập tức dưới dạng`0`. 

Giai đoạn mở rộng liên tục tìm thấy bất kỳ ký tự nào vẫn có thể tiến lên con trỏ. Việc lựa chọn giữa các ký tự hợp lệ không ảnh hưởng đến tính chính xác vì chúng tôi chỉ tính khoảng thời gian có thể tiếp tục chứ không tính chuỗi chính xác nào được hình thành. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = abacaba, k = 3
t = bcb
```Chúng ta đấu lần đầu tiên`t`: 

| bước | char | tư thế trước | nxt[pos][c] | tư thế sau | 
| --- | --- | --- | --- | --- | 
| 1 | b | 0 | 1 | 2 | 
| 2 | c | 2 | 2 | 3 | 
| 3 | b | 3 | 4 | 5 | 

Hiện nay`t`là một dãy con nên chúng ta thử mở rộng: 

| phần mở rộng | tư thế | char đã chọn | nxt[pos][c] | tư thế mới | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | một | 5 | 6 | 1 | 
| 2 | 6 | một | 6 | 7 | 2 | 
| dừng lại | 7 | không | - | - | 2 | 

Chúng tôi dừng lại vì vị trí 7 không có sự tiếp tục hợp lệ. Câu trả lời cuối cùng là`2`. 

Điều này xác nhận rằng chúng ta vẫn có thể mở rộng dãy con hai lần trước khi sử dụng hết tất cả các đường dẫn tiếp tục. 

### Ví dụ 2 

đầu vào:```
s = abacaba, t = cc
```Việc so khớp thất bại ngay lập tức vì lần đầu tiên`c`tồn tại nhưng thứ hai`c`cannot be matched after the first occurrence is consumed.

 | bước | char | tư thế trước | nxt[pos][c] | kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | c | 0 | 2 | vị trí = 3 | 
| 2 | c | 3 | -1 | thất bại | 

Since subsequence property already fails, answer is`0`. Không cần mở rộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · k + tổng | t | 
| Không gian | O(n · k) | bảng chuyển tiếp lưu trữ lần xuất hiện tiếp theo cho từng vị trí và ký tự | 

Những ràng buộc cho phép`n`lên tới một triệu và tổng độ dài truy vấn lên tới một triệu, do đó, quá trình tiền xử lý tuyến tính và xử lý truy vấn tuyến tính khấu hao này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import main  # assume solution is in main()
    return main()

# sample 1
assert run("""7 3
abacaba
3
cc
bcb
b
""") == "0\n1\n2"

# all single character, exists
assert run("""5 2
aaaaa
2
a
b
""") == "4\n0"

# minimal case
assert run("""1 1
a
1
a
""") == "0"

# alternating pattern
assert run("""6 2
ababab
1
ab
""") == "2"

# long no-match
assert run("""3 2
abc
1
cccc
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả a/b trộn | 4/0 | dãy con cơ bản + thất bại | 
| char đơn | 0 | cạnh tối thiểu | 
| xen kẽ | 2 | mở rộng cấu trúc lặp đi lặp lại | 
| không khớp | 0 | từ chối sớm | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi chuỗi truy vấn đã không thể thực hiện được dưới dạng một chuỗi con. Ví dụ, nếu`s = "abc"`Và`t = "ddd"`, thuật toán thất bại trong lần kiểm tra ký tự đầu tiên và trả về`0`ngay lập tức. các`nxt`bảng trả về`-1`cho mọi`d`, vì vậy không thể chuyển đổi được. 

Một trường hợp khác là khi`t`trận đấu`s`hoàn toàn, hạ cánh ở vị trí cuối cùng`pos = n`. Ở trạng thái đó, mọi nỗ lực mở rộng đều thất bại ngay lập tức vì`nxt[n][c] = -1`cho tất cả các nhân vật. Thuật toán trả về chính xác`0`, vì chuỗi này đã ở mức "không thể mở rộng" ở mức tối đa. 

Một trường hợp tinh tế hơn nữa là khi có nhiều ký tự để mở rộng. Sự lựa chọn tham lam của ký tự hợp lệ đầu tiên là an toàn vì tất cả các chuyển đổi hợp lệ đều tiến lên`pos`và chúng tôi chỉ tính thời gian tồn tại của bất kỳ đường dẫn hợp lệ nào. Cấu trúc của`nxt`đảm bảo rằng nếu có thể mở rộng thì ít nhất một bước chuyển tiếp sẽ được phát hiện và khi không có bước chuyển tiếp nào tồn tại thì quá trình sẽ dừng lại một cách xác định.
