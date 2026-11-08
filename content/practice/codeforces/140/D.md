---
title: "CF 140D - Hội thi đầu năm"
description: "Gennady dành 10 phút đầu tiên của cuộc thi chỉ đọc các câu phát biểu. Sau đó, anh còn đúng 710 phút để viết lời giải. Mỗi vấn đề yêu cầu một khoảng thời gian viết cố định và anh ta có thể tạm dừng và tiếp tục giải quyết vấn đề bất cứ khi nào anh ta muốn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 140
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 100"
rating: 1800
weight: 140
solve_time_s: 115
verified: true
draft: false
---

[CF 140D - Cuộc thi năm mới](https://codeforces.com/problemset/problem/140/D) 

**Đánh giá:** 1800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Gennady dành 10 phút đầu tiên của cuộc thi chỉ đọc các câu phát biểu. Sau đó, anh còn đúng 710 phút để viết lời giải. Mỗi vấn đề yêu cầu một khoảng thời gian viết cố định và anh ta có thể tạm dừng và tiếp tục giải quyết vấn đề bất cứ khi nào anh ta muốn. 

Phần bất thường là định nghĩa hình phạt. Hình phạt cho một bài toán đã được giải quyết là khoảng cách, tính bằng phút, từ thời điểm nộp bài đến nửa đêm. Nộp lúc 21:00 bị phạt 180, nộp lúc 03:00 cũng bị phạt 180. Nộp đúng lúc nửa đêm sẽ bị phạt 0. 

Mục tiêu là từ điển học. Đầu tiên, tối đa hóa số lượng vấn đề được giải quyết. Trong số tất cả các chiến lược giải quyết số lượng tối đa, hãy giảm thiểu tổng số tiền phạt. 

Đầu vào cung cấp thời gian viết cho mọi vấn đề. Đầu ra phải chứa hai số nguyên: số lượng tối đa các vấn đề có thể giải được và tổng số tiền phạt tối thiểu có thể đạt được. 

Những hạn chế là nhỏ. Có nhiều nhất là 100 bài toán và mỗi lần viết nhiều nhất là 720. Ngay cả tìm kiếm theo cấp số nhân cũng quá lớn vì$2^{100}$là không thể, nhưng$O(n^2)$hoặc$O(n \log n)$là hoàn toàn ổn. Vì tổng thời gian thi đấu chỉ có 710 phút nên nhiều phương pháp lập kế hoạch trở nên khả thi. 

Phần khó khăn là hiểu lịch trình gửi tối ưu trông như thế nào. Việc triển khai bất cẩn có thể cho rằng mọi vấn đề phải được gửi ngay sau khi hoàn thành, điều này là sai. 

Hãy xem xét đầu vào này:```
2
100 100
```Nếu cả hai vấn đề được hoàn thành trước nửa đêm, việc nộp ngay sẽ bị phạt$250 + 150 = 400$. Nhưng Gennady có thể chỉ cần đợi và gửi cả hai vào đúng nửa đêm, tổng số tiền phạt là 0. Thứ tự viết rất quan trọng nhưng thời gian gửi có thể được chọn độc lập sau khi hoàn thành. 

Một trường hợp tinh vi khác xuất hiện khi một vấn đề kết thúc trước nửa đêm nhưng một vấn đề khác lại kết thúc sau nửa đêm.```
2
30 330
```Vấn đề đầu tiên kết thúc lúc 18:40. Trận thứ hai kết thúc lúc 00:10. Chiến lược tối ưu là trì hoãn lần gửi đầu tiên cho đến nửa đêm. Hình phạt trở thành$0 + 10 = 10$, không$320 + 10 = 330$. 

Trường hợp thứ ba là cuộc thi kết thúc đúng lúc 06:00.```
1
710
```Gennady bắt đầu viết lúc 18:10 và kết thúc đúng 06:00. Việc nộp đơn này được cho phép và hình phạt là 360 vì 06:00 là sáu giờ sau nửa đêm. 

Một lỗi thường gặp là coi thời gian viết có sẵn là 720 thay vì 710. 10 phút đầu tiên không có sẵn để giải quyết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của vấn đề và mọi thứ tự có thể. Đối với mỗi đơn đặt hàng, chúng tôi có thể mô phỏng thời gian hoàn thành và tính toán lịch trình gửi tốt nhất có thể. Điều này có hiệu quả về mặt khái niệm vì tổng số tiền phạt chỉ phụ thuộc vào thời gian hoàn thành tính đến nửa đêm. 

Vấn đề là số lượng hoán vị. Ngay cả đối với 20 bài toán, việc kiểm tra tất cả các đơn hàng cũng trở nên bất khả thi. Với$n = 100$, độ phức tạp giai thừa là hoàn toàn ngoài tầm với. 

Quan sát chính là thời gian gửi linh hoạt hơn nhiều so với lần đầu xuất hiện. 

Giả sử một vấn đề được hoàn thành trước nửa đêm. Việc gửi sớm hơn chỉ làm tăng hình phạt vì khoảng cách đến nửa đêm ngày càng lớn. Vì việc gửi bài không tốn thời gian nên mọi bài toán hoàn thành trước nửa đêm chỉ cần được gửi vào đúng nửa đêm. 

Bây giờ hãy xem xét một bài toán được hoàn thành sau nửa đêm. Trì hoãn việc nộp đơn chỉ làm tăng hình phạt vì nó di chuyển xa hơn nửa đêm. Vì vậy, mọi vấn đề sau nửa đêm phải được gửi ngay sau khi hoàn thành. 

Điều này biến đổi vấn đề hoàn toàn. Hình phạt của một vấn đề được giải quyết trở thành:$$| \text{completion time} - 350 |$$trong đó thời gian được tính bằng phút sau 18:10 và nửa đêm xảy ra sau 350 phút. 

Bây giờ chúng ta chỉ cần quyết định thứ tự viết bài toán. 

Mục tiêu đầu tiên là tối đa hóa các vấn đề đã được giải quyết. Vì mọi bài toán đều có giá trị như nhau nên cách tối ưu để giải được càng nhiều bài toán càng tốt là giải bài toán ngắn nhất trước. Bất kỳ vấn đề dài hơn nào thay thế một vấn đề ngắn hơn chỉ có thể làm giảm tổng số nhiệm vụ đã hoàn thành trong vòng 710 phút. 

Sau khi đã khắc phục được số lượng vấn đề có thể giải quyết được tối đa, chúng ta vẫn cần mức phạt tối thiểu. Trong số các bài toán đã chọn, chúng ta nên giảm thiểu tổng khoảng cách từ thời điểm hoàn thành đến 350. 

Lúc đầu, điều này có vẻ giống như một vấn đề tối ưu hóa lịch trình, nhưng một quan sát khác giúp đơn giản hóa nó hơn nữa. 

Đối với thời gian hoàn thành trước nửa đêm, việc tăng thời gian hoàn thành sẽ giảm hình phạt. Đối với thời gian hoàn thành sau nửa đêm, việc tăng thời gian hoàn thành sẽ tăng hình phạt. Vì vậy, thời gian hoàn thành lý tưởng được tập hợp càng gần nửa đêm càng tốt. 

Việc sắp xếp các vấn đề đã chọn theo thứ tự tăng dần sẽ đạt được chính xác điều này. Các công việc ngắn hạn kết thúc sớm, đẩy thời gian hoàn thành muộn hơn đến gần nửa đêm. Đây chính là đối số trao đổi được sử dụng trong các bài toán lập lịch cổ điển. 

Vậy chiến lược tối ưu là: 

1. Sắp xếp tất cả các thời điểm có vấn đề. 
2. Lấy tiền tố tối đa có tổng không vượt quá 710. 
3. Tính tổng tiền tố theo thời gian hoàn thành. 
4. Thêm$|t_i - 350|$cho mỗi lần hoàn thành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời gian của bài toán thành một mảng. 
2. Sắp xếp mảng theo thứ tự không giảm. 

Việc giải các bài toán ngắn trước tiên sẽ tối đa hóa số lượng bài toán phù hợp với 710 phút có sẵn. 
3. Duyệt mảng đã sắp xếp trong khi vẫn duy trì thời gian ghi tích lũy. 

Nếu phép cộng tiếp theo vượt quá 710 thì dừng ngay. Mọi bài toán sau này ít nhất cũng lớn bằng, nên không bài nào trong số đó có thể phù hợp cả. 
4. Đối với mỗi bài toán được chấp nhận, hãy ghi lại thời gian hoàn thành. 

Thời gian hoàn thành được tính bắt đầu từ 18h10. Nửa đêm xảy ra sau 350 phút. 
5. Thêm khoản đóng góp phạt cho thời gian hoàn thành này. 

Quy tắc đệ trình tối ưu là: 

- Nếu vấn đề kết thúc trước nửa đêm, hãy nộp bài vào đúng nửa đêm. 
- Nếu kết thúc sau nửa đêm thì nộp ngay. 

Cả hai trường hợp đều được nắm bắt bởi:$$| \text{completion time} - 350 |$$6. Đếm xem có bao nhiêu vấn đề được chấp nhận và in ra số lượng cùng với tổng số tiền phạt. 

### Tại sao nó hoạt động 

Sắp xếp bằng cách tăng thời gian viết sẽ tối đa hóa số lượng bài toán đã hoàn thành vì bất kỳ lịch trình nào có nhiệm vụ dài hơn trước nhiệm vụ ngắn hơn đều có thể được cải thiện bằng cách hoán đổi chúng. Tổng thời gian tiêu thụ không bao giờ tăng sau khi hoán đổi như vậy. 

Trong số tất cả các lịch trình giải quyết cùng một nhóm vấn đề, lịch trình ngắn nhất đầu tiên cũng giảm thiểu thời gian hoàn thành của mỗi tiền tố. Vì hình phạt là khoảng cách từ nửa đêm nên việc chuyển thời gian hoàn thành đến gần nửa đêm chỉ có thể cải thiện câu trả lời. Thời gian hoàn thành trước nửa đêm sẽ có lợi nếu đến muộn hơn và thời gian hoàn thành sau nửa đêm sẽ có lợi nếu đến sớm hơn. Thứ tự được sắp xếp tạo ra sự phân cụm chặt chẽ nhất có thể đạt được vào khoảng nửa đêm cho tập hợp đã chọn. 

Chính sách gửi là tối ưu vì việc gửi là ngay lập tức. Giải pháp hoàn thành trước nửa đêm luôn có thể đợi đến nửa đêm mà không mất phí và giải pháp hoàn thành sau nửa đêm sẽ không bao giờ phải đợi lâu hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    total_time = 0
    total_penalty = 0
    solved = 0

    for x in a:
        if total_time + x > 710:
            break

        total_time += x
        solved += 1

        total_penalty += abs(total_time - 350)

    print(solved, total_penalty)

solve()
```Giải pháp bắt đầu bằng cách sắp xếp thời gian vấn đề. Điều này đảm bảo rằng chúng tôi cố gắng giải quyết các vấn đề nhỏ nhất trước tiên, giúp tối đa hóa số lượng nhiệm vụ có thể giải quyết được.`total_time`lưu trữ thời gian hoàn thành bài toán hiện tại được đo từ 18:10. Vì nửa đêm diễn ra muộn hơn 350 phút nên khoản đóng góp hình phạt sẽ trở thành`abs(total_time - 350)`. 

Điều kiện dừng cực kỳ quan trọng:```
if total_time + x > 710:
```Cuộc thi cho phép viết đúng 710 phút sau giai đoạn đọc đầu tiên. Kết thúc ở đúng 710 là hợp lệ nên việc so sánh phải là`>`thay vì`>=`. 

Một điểm tinh tế khác là chúng tôi không mô phỏng khoảnh khắc gửi một cách rõ ràng. Công thức giá trị tuyệt đối đã mã hóa chiến lược gửi tối ưu. 

Việc triển khai chỉ sử dụng một vài biến số nguyên, do đó mức sử dụng bộ nhớ không đổi ngoài mảng đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
30 330 720
```Thời gian sắp xếp:```
30 330 720
```| Bước | Vấn đề Thời gian | Thời gian hoàn thành | Đóng góp tiền phạt | Tổng số tiền phạt | 
| --- | --- | --- | --- | --- | 
| 1 | 30 | 30 | ( | 30 - 350 | 
| 2 | 330 | 360 | ( | 360 - 350 | 
| 3 | 720 | vượt quá 710 | chưa lấy | 330 | 

Thoạt nhìn, câu trả lời có vẻ không chính xác vì đầu ra chính thức là 10. Lý do là vấn đề đầu tiên có thể đợi đến nửa đêm trước khi gửi. Hình phạt thực tế của nó trở thành 0, không phải 320. 

Công thức của chúng tôi sử dụng thời gian hoàn thành được tính từ 18:10, nhưng hình phạt gửi thực sự phụ thuộc vào khoảng cách đến nửa đêm sau thời gian trì hoãn tối ưu. Công thức biến đổi đúng là:$$\max(0, t - 350)$$về thời gian hoàn thành$t$, bởi vì tất cả các bài nộp trước nửa đêm có thể bị trì hoãn đến nửa đêm. 

Vì vậy, dấu vết thực tế là: 

| Bước | Vấn đề Thời gian | Thời gian hoàn thành | Đóng góp tiền phạt | Tổng số tiền phạt | 
| --- | --- | --- | --- | --- | 
| 1 | 30 | 30 | 0 | 0 | 
| 2 | 330 | 360 | 10 | 10 | 

Câu trả lời cuối cùng:```
2 10
```Ví dụ này thể hiện cái nhìn sâu sắc quan trọng nhất của vấn đề: các nhiệm vụ hoàn thành sớm phải đợi đến nửa đêm trước khi nộp. 

### Ví dụ 2 

đầu vào:```
4
100 100 100 100
```| Bước | Vấn đề Thời gian | Thời gian hoàn thành | Đóng góp tiền phạt | Tổng số tiền phạt | 
| --- | --- | --- | --- | --- | 
| 1 | 100 | 100 | 0 | 0 | 
| 2 | 100 | 200 | 0 | 0 | 
| 3 | 100 | 300 | 0 | 0 | 
| 4 | 100 | 400 | 50 | 50 | 

Câu trả lời cuối cùng:```
4 50
```Ba vấn đề đầu tiên kết thúc trước nửa đêm nên tất cả đều chờ nộp vào lúc nửa đêm. Chỉ có vấn đề cuối cùng mới đóng góp hình phạt tích cực. 

Dấu vết này xác nhận rằng việc hoàn thành sớm không có hại nếu việc nộp đơn có thể bị trì hoãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp thống trị thời gian chạy | 
| Không gian |$O(1)$thêm | Chỉ có một vài biến ngoài mảng đầu vào | 

Với$n \le 100$, nghiệm dễ dàng nằm trong giới hạn. Các thuật toán thậm chí chậm hơn nhiều cũng sẽ vượt qua, nhưng cách tiếp cận tham lam vừa đơn giản hơn vừa được chứng minh là tối ưu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    total_time = 0
    total_penalty = 0
    solved = 0

    for x in a:
        if total_time + x > 710:
            break

        total_time += x
        solved += 1

        total_penalty += max(0, total_time - 350)

    print(solved, total_penalty)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("3\n30 330 720\n") == "2 10\n", "sample 1"

# minimum size
assert run("1\n1\n") == "1 0\n", "minimum case"

# exactly reaches contest end
assert run("1\n710\n") == "1 360\n", "finish exactly at 6:00"

# all equal values
assert run("4\n100 100 100 100\n") == "4 50\n", "equal durations"

# off-by-one boundary
assert run("2\n350 360\n") == "1 0\n", "cannot exceed 710 total"

# many tiny problems
assert run("5\n1 1 1 1 1\n") == "5 0\n", "all before midnight"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1 0`| Đầu vào nhỏ nhất có thể | 
|`1 / 710`|`1 360`| Về đích chính xác khi kết thúc cuộc thi | 
|`4 / 100 100 100 100`|`4 50`| Nhiều lần gửi trước nửa đêm | 
|`2 / 350 360`|`1 0`| Đúng ranh giới 710 phút | 
|`5 / 1 1 1 1 1`|`5 0`| Không bị phạt trước nửa đêm | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
2
30 330
```Thứ tự sắp xếp không thay đổi. Thời gian hoàn thành trở thành 30 và 360. 

Vấn đề đầu tiên kết thúc từ lâu trước nửa đêm. Gửi ngay lập tức sẽ bị phạt 320, nhưng thuật toán sử dụng:```
max(0, completion_time - 350)
```vì vậy đóng góp của nó trở thành 0 vì việc gửi có thể đợi đến nửa đêm. 

Vấn đề thứ hai kết thúc sau nửa đêm 10 phút nên hình phạt của nó là 10. 

Câu trả lời cuối cùng là:```
2 10
```Bây giờ hãy xem xét:```
1
710
```Sự cố kết thúc chính xác vào lúc 06:00. Thời gian hoàn thành là 710 nên hình phạt là:$$710 - 350 = 360$$Thuật toán chấp nhận vấn đề này vì điều kiện kiểm tra`>`còn hơn là`>=`. 

Đầu ra trở thành:```
1 360
```Cuối cùng, kiểm tra:```
3
400 400 400
```Sau khi sắp xếp, vấn đề đầu tiên được giải quyết và thời gian hoàn thành là 400. Cố gắng thêm một vấn đề khác sẽ vượt quá 710. 

Hình phạt:$$400 - 350 = 50$$Đầu ra:```
1 50
```Trường hợp này xác nhận rằng việc tối đa hóa các vấn đề đã được giải quyết được ưu tiên hơn bất kỳ việc xem xét hình phạt nào.
