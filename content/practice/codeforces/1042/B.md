---
problem: 1042B
contest_id: 1042
problem_index: B
name: "Vitamins"
contest_name: "Codeforces Round 510 (Div. 2)"
rating: 1200
tags: ["bitmasks", "brute force", "dp", "implementation"]
answer: passed_samples
verified: false
solve_time_s: 306
date: 2026-06-18
model: gpt-5-5
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33bcc3-88bc-83ec-9157-fb55c8978ed7
---

#CF 1042B - Vitamin 

**Đánh giá:** 1200 
**Tags:** bitmasks, vũ phu, dp, triển khai 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 5 phút 6s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33bcc3-88bc-83ec-9157-fb55c8978ed7 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các lựa chọn về nước trái cây, trong đó mỗi lựa chọn đều có giá thành và một bộ vitamin mà nó cung cấp. Tổng cộng chỉ có ba loại vitamin, được dán nhãn A, B và C. Khi Petya mua một bộ nước trái cây, anh ấy sẽ sử dụng hiệu quả sự kết hợp của tất cả các loại vitamin có trong nước trái cây đó. Mục tiêu là chọn một tập hợp con các loại nước ép sao cho chứa được cả ba loại vitamin trong khi giảm thiểu tổng chi phí. 

Đây không phải là vấn đề về trình tự hay thứ tự, đây là vấn đề lựa chọn tập hợp con thuần túy trên tối đa 1000 mục. Mỗi mục chỉ đóng góp một trong những$2^3 - 1 = 7$có thể kết hợp các loại vitamin nên cấu trúc của bài toán bị nén rất nhiều. Nhiệm vụ là tìm ra tổ hợp rẻ nhất có hợp bằng tập đầy đủ {A, B, C}. 

Ràng buộc$n \le 1000$ngụ ý rằng ngay cả các giải pháp bậc hai đối với các mục cũng là đường biên nhưng vẫn khả thi ở dạng tối ưu. Tuy nhiên, bất kỳ cách tiếp cận nào cố gắng liệt kê tập hợp con đầy đủ trên tất cả các loại nước ép, đó là$2^{1000}$, là không thể. Ngay cả một DP ngây thơ trên tất cả các tập hợp con cũng bị loại trừ. Hạn chế chính là không gian trạng thái không được xác định bởi số lượng nước trái cây mà bởi số lượng kết hợp vitamin. 

Một vấn đề tế nhị nảy sinh khi nhiều loại nước trái cây dùng chung một mặt nạ vitamin nhưng có giá thành khác nhau. Ví dụ: nếu hai loại nước trái cây đều chỉ cung cấp vitamin A, chúng ta chỉ quan tâm đến loại rẻ nhất, bởi vì bất kỳ giải pháp tối ưu nào cũng sẽ không bao giờ sử dụng loại đắt tiền hơn. Một trường hợp khác là khi không có nước trái cây nào chứa vitamin cần thiết, chẳng hạn như không có nước trái cây chứa C. Trong trường hợp đó, ngay cả khi A và B được bao phủ hoàn toàn, câu trả lời phải là -1. 

Một trường hợp góc xa hơn là khi một loại nước ép duy nhất chứa tất cả các vitamin A, B và C. Chỉ riêng loại nước ép đó có thể hoặc không thể tối ưu so với việc kết hợp các loại nước ép từng phần rẻ tiền hơn. Bất kỳ cách tiếp cận tham lam nào luôn ưu tiên loại nước trái cây “hoàn chỉnh” nhất sẽ thất bại trong các trường hợp như A+B+C có giá 100, trong khi A, B và C riêng lẻ có giá 1 mỗi cái. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là xem xét từng tập hợp con nước trái cây và tính toán sự kết hợp của vitamin và tổng chi phí. Điều này đúng vì nó khám phá mọi khả năng, nhưng nó đòi hỏi phải lặp đi lặp lại$2^n$tập hợp con. Với$n = 1000$, đây là một con số lớn về mặt thiên văn, vượt xa mọi giới hạn tính toán. 

Quan sát quan trọng là không gian chứa vitamin rất nhỏ. Mỗi juice tương ứng với một mặt nạ trong phạm vi từ 1 đến 7, trong đó mỗi bit biểu thị sự hiện diện của A, B hoặc C. Thay vì suy nghĩ về việc chọn các nhóm nước trái cây, chúng ta có thể nghĩ đến việc chọn kết hợp các loại mặt nạ. 

Khi chúng tôi nén nước trái cây bằng mặt nạ, vấn đề sẽ trở thành: chọn nhiều bộ mặt nạ tối đa ba bit có bitwise OR bằng 7, giảm thiểu chi phí. Điều này gợi ý lập trình động trên mặt nạ bit hoặc đơn giản là duy trì chi phí tối thiểu cho mỗi mặt nạ và sau đó thử tất cả các kết hợp của 1, 2 hoặc 3 mặt nạ. 

Vì chỉ có 7 mặt nạ nên chúng ta có thể liệt kê tất cả các cặp và bộ ba mặt nạ một cách an toàn để tính toán sự kết hợp tốt nhất. Điều này làm giảm vấn đề từ hàm mũ trong$n$đến thời gian liên tục trên mặt nạ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Nén bitmask + liệt kê |$O(n + 7^3)$|$O(7)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi vitamin của mỗi loại nước trái cây thành mặt nạ 3 bit, trong đó A = 1, B = 2, C = 4. Điều này làm giảm mỗi mục thành một số từ 1 đến 7. Bước này biến bài toán thành bài toán tối ưu hóa trạng thái cố định. 
2. Duy trì một mảng`best[8]`, được khởi tạo với vô cùng, trong đó`best[m]`lưu trữ chi phí tối thiểu của bất kỳ loại nước trái cây nào phù hợp chính xác với mặt nạ`m`. Chúng tôi chỉ quan tâm đến đại diện rẻ nhất cho từng loại. 
3. Với mỗi loại nước trái cây, hãy tính mặt nạ của nó và cập nhật`best[mask] = min(best[mask], cost)`. Điều này đảm bảo chúng ta không bao giờ có những lựa chọn thống trị. 
4. Nếu`best`không có cách nào để bao phủ tất cả các bit cần thiết ngay cả khi kết hợp, chúng tôi sẽ phát hiện nó sau bằng cách kiểm tra tính khả thi. 
5. Liệt kê tất cả các cặp mặt nạ`(i, j)`và tính toán`best[i] + best[j]`, theo dõi những người có OR bằng 7. Phần này bao gồm các giải pháp sử dụng một hoặc hai loại nước ép. 
6. Liệt kê tất cả các bộ ba`(i, j, k)`tương tự, kiểm tra`i | j | k == 7`, cập nhật câu trả lời. Điều này đảm bảo chúng tôi cũng bao gồm các trường hợp cần ba phần nước trái cây. 
7. Trả về số tiền hợp lệ tối thiểu hoặc -1 nếu không có sự kết hợp nào đạt được phạm vi bao phủ đầy đủ. 

### Tại sao nó hoạt động 

Mỗi loại nước trái cây đều thuộc một trong bảy loại tương đương được xác định bởi mặt nạ vitamin của nó. Bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành một giải pháp sử dụng tối đa một đại diện cho mỗi mặt nạ đã chọn và việc thay thế nước trái cây đã chọn bằng một loại mặt nạ tương tự rẻ hơn không bao giờ làm kết quả xấu đi. Vì không gian mục tiêu chỉ có 7 trạng thái nên việc liệt kê tất cả các kết hợp của tối đa ba mặt nạ là đủ để thể hiện bất kỳ sự kết hợp khả thi nào của vitamin. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

n = int(input())
best = [INF] * 8

for _ in range(n):
    parts = input().split()
    c = int(parts[0])
    s = parts[1]

    mask = 0
    for ch in s:
        if ch == 'A':
            mask |= 1
        elif ch == 'B':
            mask |= 2
        else:
            mask |= 4

    best[mask] = min(best[mask], c)

ans = INF

for i in range(1, 8):
    if best[i] == INF:
        continue
    if i == 7:
        ans = min(ans, best[i])

for i in range(1, 8):
    for j in range(1, 8):
        if best[i] == INF or best[j] == INF:
            continue
        if (i | j) == 7:
            ans = min(ans, best[i] + best[j])

for i in range(1, 8):
    for j in range(1, 8):
        for k in range(1, 8):
            if best[i] == INF or best[j] == INF or best[k] == INF:
                continue
            if (i | j | k) == 7:
                ans = min(ans, best[i] + best[j] + best[k])

print(-1 if ans == INF else ans)
```Giải pháp trước tiên nén từng loại nước thành một mặt nạ bit, sau đó giảm trùng lặp bằng cách chỉ giữ lại chi phí rẻ nhất cho mỗi mặt nạ. Câu trả lời cuối cùng được tính toán bằng cách kết hợp tối đa ba mặt nạ. Vòng lặp ba là an toàn vì không gian trạng thái được cố định ở mức 7, do đó tổng số phép toán không đổi. 

Một chi tiết tinh tế là chúng tôi cho phép kết hợp các loại mặt nạ lặp đi lặp lại, điều này đúng vì đôi khi cần mua hai loại nước trái cây khác nhau với cùng một mặt nạ nếu một loại rẻ nhất không đủ để đạt được độ che phủ toàn diện cùng với các mặt nạ khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
5 C
6 B
16 BAC
4 A
```Chúng tôi ánh xạ nước trái cây vào mặt nạ: 

| Nước ép | Mặt nạ | Chi phí | 
| --- | --- | --- | 
| C | 4 | 5 | 
| B | 2 | 6 | 
| BẮC | 7 | 16 | 
| A | 1 | 4 | 

Sau khi nén,`best`là: 

A=4, B=6, C=5, ABC=16. 

Bây giờ chúng tôi đánh giá sự kết hợp. 

| Bước | Mặt nạ được chọn | HOẶC | Chi phí | 
| --- | --- | --- | --- | 
| độc thân | (7) | 7 | 16 | 
| cặp | (1,2,4) | 7 | 4+6+5=15 | 

Tối thiểu là 15. 

Điều này khẳng định rằng mặc dù một loại nước ép có chứa tất cả các loại vitamin nhưng kết hợp những loại rẻ tiền hơn một phần sẽ tốt hơn. 

### Ví dụ 2 (không có trường hợp giải pháp) 

đầu vào:```
3
5 A
6 B
10 AB
```Mặt nạ là A=1, B=2, AB=3. Không có mặt nạ nào chứa C. 

| Bước | Kết hợp | HOẶC | hợp lệ | 
| --- | --- | --- | --- | 
| bất kỳ | tập hợp con | không bao giờ 7 | không | 

Không có sự kết hợp nào có thể tạo ra bit 4, vì vậy câu trả lời là -1. Thuật toán trả về INF một cách tự nhiên và xuất ra -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + 7^3)$| quét tuyến tính để tính toán mặt nạ cộng với phép liệt kê liên tục trên 7 trạng thái | 
| Không gian |$O(1)$| mảng cố định có kích thước 8 | 

Thuật toán dễ dàng phù hợp trong giới hạn vì phần nặng không phụ thuộc vào$n$sau khi tiền xử lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**18
    n = int(input())
    best = [INF] * 8

    for _ in range(n):
        parts = input().split()
        c = int(parts[0])
        s = parts[1]

        mask = 0
        for ch in s:
            if ch == 'A':
                mask |= 1
            elif ch == 'B':
                mask |= 2
            else:
                mask |= 4

        best[mask] = min(best[mask], c)

    ans = INF

    for i in range(1, 8):
        if best[i] < INF and i == 7:
            ans = min(ans, best[i])

    for i in range(1, 8):
        for j in range(1, 8):
            if best[i] < INF and best[j] < INF and (i | j) == 7:
                ans = min(ans, best[i] + best[j])

    for i in range(1, 8):
        for j in range(1, 8):
            for k in range(1, 8):
                if best[i] < INF and best[j] < INF and best[k] < INF and (i | j | k) == 7:
                    ans = min(ans, best[i] + best[j] + best[k])

    return "-1" if ans == INF else str(ans)

# provided sample
assert run("""4
5 C
6 B
16 BAC
4 A
""") == "15"

# no C case
assert run("""3
5 A
6 B
10 AB
""") == "-1"

# single optimal juice
assert run("""3
10 ABC
100 A
100 B
""") == "10"

# duplicates cheaper combination
assert run("""5
10 A
1 A
10 B
1 B
100 C
""") == "3"

# all separate
assert run("""3
1 A
1 B
1 C
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hỗn hợp tối ưu ba | 15 | sự kết hợp tối ưu so với nước ép nguyên chất | 
| thiếu vitamin | -1 | phát hiện không thể | 
| ABC đơn | 10 | trường hợp tối ưu một mục | 
| trùng lặp | 3 | xử lý mặt nạ lặp đi lặp lại | 
| tất cả riêng biệt | 3 | tính đúng đắn cơ bản của sự kết hợp | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều loại nước trái cây có chung một mặt nạ. Thuật toán thu gọn chúng một cách chính xác thông qua`best[mask]`, đảm bảo chỉ xem xét giá rẻ nhất. Ví dụ: nếu A có giá 5 và A khác có giá 2 thì chỉ giữ lại 2 và mọi giải pháp tối ưu liên quan đến A sẽ sử dụng giá trị đó. 

Một trường hợp khác là khi giải pháp tối ưu sử dụng ít hơn ba loại nước ép. Việc liệt kê bao gồm các kết hợp đơn và cặp một cách rõ ràng, vì vậy các giải pháp như (A+B+C trong một nước ép) hoặc (A+B, C) đều được đề cập. 

Cuối cùng, trường hợp không thể xảy ra được xử lý rõ ràng vì không có sự kết hợp mặt nạ nào có thể tạo ra các bit bị thiếu. Nếu vitamin C không bao giờ xuất hiện trong bất kỳ mặt nạ nào, mọi OR vẫn giữ nguyên 3 và thuật toán không bao giờ cập nhật`ans`, dẫn đến -1.
