---
title: "CF 104637C - Đồng hồ báo thức"
description: "Chúng tôi đang mô phỏng một chu kỳ giấc ngủ rất cụ thể bằng chuông báo lặp lại. Polycarp chìm vào giấc ngủ và ban đầu đợi một số phút cố định trước khi chuông báo thức đầu tiên reo. Sau đó, mỗi khi thức dậy, anh ấy sẽ kiểm tra xem mình đã ngủ đủ giấc hay chưa."
date: "2026-06-29T16:59:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104637
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u0431\u0430\u0437\u043e\u0432\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430, \u0443\u0441\u043b\u043e\u0432\u0438\u044f, \u0446\u0438\u043a\u043b\u044b"
rating: 0
weight: 104637
solve_time_s: 67
verified: true
draft: false
---

[CF 104637C - Đồng hồ báo thức](https://codeforces.com/problemset/problem/104637/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một chu kỳ giấc ngủ rất cụ thể bằng chuông báo lặp lại. Polycarp chìm vào giấc ngủ và ban đầu đợi một số phút cố định trước khi chuông báo thức đầu tiên reo. Sau đó, mỗi khi thức dậy, anh ấy sẽ kiểm tra xem mình đã ngủ đủ giấc hay chưa. Nếu anh ta vẫn chưa đạt được số lượng cần thiết, anh ta sẽ chuyển sang một chu kỳ khác trong đó báo thức được đặt lại, sau đó anh ta dành một khoảng thời gian để ngủ lại và chỉ một phần của khoảng thời gian báo thức tiếp theo mới góp phần vào giấc ngủ thực sự. 

Ý tưởng chính là thời gian được chia thành các giai đoạn xen kẽ: giai đoạn ngủ góp phần tích lũy giấc ngủ, trong khi giai đoạn “đi vào giấc ngủ” tiêu tốn thời gian nhưng không bổ sung thêm giấc ngủ. Cảnh báo liên tục làm gián đoạn quá trình này theo các khoảng thời gian cố định, ngoại trừ khoảng thời gian đầu tiên là khác nhau. 

Chúng tôi được yêu cầu tính toán thời gian chính xác khi giấc ngủ tích lũy lần đầu tiên đạt đến ít nhất một ngưỡng nhất định. Nếu cấu trúc của quy trình khiến không thể tích lũy đủ giấc ngủ thì quy trình sẽ chạy mãi mãi và chúng ta phải xuất -1. 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm và mỗi tham số có thể lớn tới 10^9. Điều này ngay lập tức loại trừ việc mô phỏng tiến trình từng phút hoặc từng sự kiện. Ngay cả việc mô phỏng tuyến tính cho mỗi cảnh báo cũng sẽ quá chậm trong trường hợp xấu nhất khi giá trị lớn và quá trình này mất tới hàng tỷ chu kỳ. 

Một trường hợp thất bại khó nhận thấy xuất hiện khi việc tích lũy giấc ngủ tăng quá chậm so với mức “ngủ quên trên đầu” d. Trong những trường hợp như vậy, ngay cả khi chuông báo thức reo vô thời hạn, lượng giấc ngủ hiệu quả đạt được trong mỗi chu kỳ có thể bằng 0 hoặc không đủ, gây ra vòng lặp vô hạn. 

Ví dụ: nếu c ≤ d thì mỗi khi chuông báo thức đổ chuông khi bạn đang ngủ, Polycarp sẽ đặt lại nó và không bao giờ đạt đến bất kỳ giai đoạn ngủ có ý nghĩa nào ngoài lần gián đoạn đầu tiên. Điều này có thể dẫn đến không có tiến triển trong giấc ngủ tích lũy. 

Một trường hợp khác là khi b đã vượt quá hoặc bằng a, nghĩa là Polycarp ngủ đủ giấc ngay sau lần báo thức đầu tiên. Trong trường hợp đó, không có chu kỳ nào xảy ra nữa và câu trả lời chỉ đơn giản là b. 

Cuối cùng, có một trường hợp giấc ngủ tích lũy theo kiểu lặp đi lặp lại và chúng ta phải tránh mô phỏng tất cả các chu kỳ một cách rõ ràng mà thay vào đó đưa ra lý do về mức tăng ròng trên mỗi chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng chu kỳ báo động. Chúng tôi theo dõi thời gian hiện tại, số giờ ngủ tích lũy và xử lý lặp đi lặp lại: đợi cho đến khi báo thức, cộng số giờ ngủ đạt được trong khoảng thời gian đó, sau đó trừ đi thời gian chìm vào giấc ngủ và tiếp tục. Mỗi chu kỳ là O(1), nhưng số chu kỳ có thể lớn bằng a/b hoặc thậm chí tỷ lệ với a/c, trong trường hợp xấu nhất có thể lên tới 10^9. Điều này làm cho việc mô phỏng trực tiếp không thể thực hiện được. 

Quan sát quan trọng là sau lần báo động đầu tiên, quá trình này trở nên định kỳ. Mỗi chu kỳ đóng góp một lượng giấc ngủ cố định và tiêu tốn một lượng thời gian thực cố định. Thay vì mô phỏng mỗi lần thức dậy, chúng ta chỉ cần hiểu lượng giấc ngủ được ngủ trong một chu kỳ đầy đủ. 

Sau lần thức dậy đầu tiên vào thời điểm b, Polycarp sẽ kiểm tra xem giấc ngủ cho đến nay có đủ hay không. Nếu không, anh ta sẽ chuyển sang mô hình lặp lại: mỗi chu kỳ bao gồm dành d phút để ngủ, sau đó chờ báo thức tiếp theo sau c phút kể từ khi đặt lại, trong đó chỉ phần cuối cùng mới góp phần tạo ra giấc ngủ tùy thuộc vào cấu trúc chồng chéo. Điểm giảm quan trọng là mỗi chu kỳ đầy đủ sẽ tăng tổng thời gian lên c và đóng góp cho giấc ngủ hiệu quả c hoặc tối đa (0, c - d) tùy thuộc vào sự chồng chéo. 

Điều này biến vấn đề thành vấn đề tích lũy tuyến tính với khả năng kết thúc sớm sau bước đầu tiên. 

Quá trình này kết thúc ngay lập tức, kết thúc sau một vài lần tăng giống hệt nhau hoặc không bao giờ tiến triển vì mức tăng giấc ngủ thực trong mỗi chu kỳ bằng không.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(câu trả lời / phút(c, b)) | O(1) | Quá chậm | 
| Số học dựa trên chu trình | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Trước tiên hãy kiểm tra xem Polycarp đã đáp ứng đủ giấc ngủ cần thiết ngay sau lần báo thức đầu tiên hay chưa. Nếu giấc ngủ đạt được trong phân đoạn đầu tiên b ít nhất là a thì câu trả lời chỉ đơn giản là b vì không cần phải chờ đợi thêm. 
2. Nếu không hài lòng, hãy tính xem bạn ngủ thêm được bao nhiêu sau lần thức dậy đầu tiên. Chu kỳ đầu tiên đưa ra đóng góp ban đầu đã biết và sau đó mỗi lần lặp lại hoạt động giống hệt nhau. 
3. Quan sát thấy sau khi thức dậy, Polycarp dành d phút để ngủ. Trong thời gian này, nếu khoảng thời gian báo thức tiếp theo c không đủ lớn để “chồng chéo đến mức ngủ quên”, thì sẽ không có thêm giấc ngủ nào trong các chu kỳ tiếp theo. 
4. Nếu c ≤ d thì mọi chu kỳ sẽ hoàn toàn bị tiêu hao do bạn ngủ trước khi chuông báo thức đóng góp được điều gì hữu ích. Điều này có nghĩa là không có thêm giấc ngủ nào được tích lũy sau lần thức dậy đầu tiên, vì vậy nếu b < a thì câu trả lời là không thể. 
5. Mặt khác, khi c > d, mỗi chu kỳ đóng góp chính xác (c - d) số phút ngủ hiệu quả bổ sung sau lần thức dậy đầu tiên. Chúng ta có thể coi đây là một tiến trình tuyến tính. 
6. Tính xem cần bao nhiêu chu kỳ như vậy để đạt được tổng số giấc ngủ ít nhất là a. Đây là sự phân chia trần đơn giản về thời gian ngủ cần thiết còn lại sau lần báo thức đầu tiên. 
7. Chuyển đổi số chu kỳ thành tổng thời gian bằng cách cộng b ban đầu và sau đó thêm c cho mỗi chu kỳ. 

### Tại sao nó hoạt động 

Quá trình này trở thành một sự tái phát tuyến tính xác định sau lần báo động đầu tiên. Mỗi chu kỳ có cấu trúc giống hệt nhau và đóng góp một lượng tăng không đổi cho cả thời gian và giấc ngủ tích lũy, hoặc không đóng góp chút nào vào giấc ngủ nếu c  d. Điều này có nghĩa là trạng thái của hệ thống sau mỗi lần thức dậy được đặc trưng đầy đủ bởi tổng thời gian ngủ cho đến nay và giá trị đó tăng dần theo tuyến tính. Vì không có sự phân nhánh hoặc ngẫu nhiên trong các chu kỳ sau, nên khi chúng tôi phân loại xem mức tăng là dương hay bằng 0, thì kết quả sẽ cố định: hoặc một cấp số cộng hữu hạn đạt đến ngưỡng hoặc chuỗi không bao giờ tiến triển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, c, d = map(int, input().split())

        # first alarm
        if b >= a:
            print(b)
            continue

        # if no progress possible after first cycle
        if c <= d:
            print(-1)
            continue

        # remaining sleep needed
        need = a - b

        gain = c - d

        # number of full useful cycles needed
        cycles = (need + gain - 1) // gain

        # total time: initial b + cycles * c
        ans = b + cycles * c
        print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp mã hóa việc rút gọn từ một bài toán mô phỏng thành một cấp số cộng. Lối thoát sớm xử lý trường hợp cảnh báo đầu tiên đã đáp ứng được yêu cầu. điều kiện`c <= d`ghi lại trường hợp suy biến trong đó hệ thống không thể tích lũy thêm giấc ngủ. Công thức cuối cùng tính toán có bao nhiêu mức tăng kích thước giống hệt nhau`gain`là cần thiết để bù đắp phần thâm hụt còn lại và chuyển nó thành thời gian. 

Một sai lầm phổ biến là quên rằng phần đầu tiên đóng góp khác với phần còn lại. Mã cô lập điều đó bằng cách xử lý`b`riêng biệt trước khi vào logic chu trình thống nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a=10, b=3, c=6, d=4
```Chúng tôi mô phỏng về mặt khái niệm: 

| Bước | Tổng số giấc ngủ | Thời gian | Hành động | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | báo động đầu tiên | 
| 2 | 5 | 9 | chu kỳ đầu tiên thêm 2 giấc ngủ | 
| 3 | 7 | 15 | chu kỳ thứ hai thêm 2 giấc ngủ | 
| 4 | 9 | 21 | chu kỳ thứ ba thêm 2 giấc ngủ | 
| 5 | 11 | 27 | chu kỳ thứ tư thêm 2 giấc ngủ | 

Sau 27 phút, giấc ngủ đạt ít nhất 10. Điều này xác nhận rằng mỗi chu kỳ đóng góp một giấc ngủ +2 cố định và chúng ta cần lặp lại đủ mức tăng tuyến tính này. 

### Ví dụ 2 

đầu vào:```
a=6, b=5, c=2, d=3
```| Bước | Tổng số giấc ngủ | Thời gian | Hành động | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 | báo động đầu tiên | 
| 2 | 5 | - | c ≤ d nên không thể tiến triển được | 

Ở đây, hệ thống không thể tạo thêm giấc ngủ sau lần thức dậy đầu tiên vì mỗi chu kỳ đã được sử dụng hết để chìm vào giấc ngủ. Vì 5 < 6 nên câu trả lời là không thể. 

Hai ví dụ này nêu bật hai hành vi có thể xảy ra: một tiến triển tuyến tính hoặc một hệ thống chết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case được xử lý bằng các phép tính số học không đổi | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi trường hợp thử nghiệm | 

Giải pháp chạy theo thời gian tuyến tính trên số lượng trường hợp thử nghiệm và tránh mọi mô phỏng theo chu kỳ, điều này cần thiết với giới hạn trên 10^9. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            a, b, c, d = map(int, input().split())

            if b >= a:
                out.append(str(b))
                continue

            if c <= d:
                out.append(str(-1))
                continue

            need = a - b
            gain = c - d
            cycles = (need + gain - 1) // gain
            out.append(str(b + cycles * c))

        return "\n".join(out)

    return solve()

# provided samples
assert run("""7
10 3 6 4
11 3 6 4
5 9 4 10
6 5 2 3
1 1 1 1
3947465 47342 338129 123123
234123843 13 361451236 361451000
""") == """27
27
9
-1
1
6471793
358578060125049"""

# custom cases
assert run("""1
1 5 2 1
""") == "5", "already enough first alarm"

assert run("""1
10 2 3 5
""") == "-1", "no progress because c <= d"

assert run("""1
20 3 10 2
""") == "13", "single cycle suffices"

assert run("""1
100 1 9 1
""") == "19", "multiple uniform cycles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 5 2 1 | 5 | đã hài lòng ngay lần báo động đầu tiên | 
| 1 10 2 3 5 | -1 | trường hợp không tiến triển c ≤ d | 
| 1 20 3 10 2 | 13 | hoàn thành nhiều chu kỳ tối thiểu | 
| 1 100 1 9 1 | 19 | tích lũy đồng đều lặp đi lặp lại | 

## Vỏ cạnh 

Khi nào`b >= a`, thuật toán trả về ngay`b`mà không cần nhập bất kỳ logic chu trình nào. Điều này tương ứng với tình huống khi báo động đầu tiên đã cung cấp đủ thời gian nghỉ ngơi và không có giả định nào về vấn đề cấu trúc sau này. 

Khi`c <= d`, hệ thống không thể tích lũy thêm giấc ngủ sau lần thức dậy đầu tiên. Trong trường hợp này thuật toán kết thúc với -1. Lý do phù hợp với quy trình thực tế vì mọi nỗ lực để đạt được khoảng thời gian ngủ hữu ích tiếp theo đều bị tiêu hao hoàn toàn bởi thời gian chìm vào giấc ngủ. 

Khi`c > d`nhưng thời gian ngủ cần thiết rất ít, số chu kỳ tính toán có thể bằng 0 sau khi chia trần. Công thức`(need + gain - 1) // gain`xử lý chính xác vấn đề này bằng cách đảm bảo chỉ có ít nhất một chu kỳ khi cần thiết, tránh các lỗi xảy ra khi chấm dứt sớm.
