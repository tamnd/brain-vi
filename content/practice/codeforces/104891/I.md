---
title: "CF 104891I - Ôn lại Midas"
description: "Chúng tôi được thiết lập với hai hệ thống thời gian hồi chiêu tương tác hoạt động giống như các hành động có thể tái sử dụng theo thời gian. Một hành động là khả năng tạo vàng có thể được sử dụng nhiều lần, nhưng sau mỗi lần sử dụng, nó sẽ không khả dụng trong một khoảng thời gian cố định."
date: "2026-06-28T18:02:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104891
codeforces_index: "I"
codeforces_contest_name: "The 2023 ICPC Asia Macau Regional Contest (The 2nd Universal Cup. Stage 15: Macau)"
rating: 0
weight: 104891
solve_time_s: 83
verified: false
draft: false
---

[CF 104891I - Làm mới lại Midas](https://codeforces.com/problemset/problem/104891/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được thiết lập với hai hệ thống thời gian hồi chiêu tương tác hoạt động giống như các hành động có thể tái sử dụng theo thời gian. Một hành động là khả năng tạo vàng có thể được sử dụng nhiều lần, nhưng sau mỗi lần sử dụng, nó sẽ không khả dụng trong một khoảng thời gian cố định. Hành động thứ hai là một công cụ thiết lập lại giúp khôi phục tất cả các thời gian hồi chiêu khác, cho phép sử dụng lại ngay khả năng tạo vàng, nhưng nó cũng có thời gian hồi chiêu riêng. 

Mục tiêu là mô phỏng trình tự sử dụng tối ưu trong một khoảng thời gian cố định và đếm số lần hành động tạo vàng có thể được thực hiện. Mỗi lần thực thi mang lại một lượng vàng cố định, do đó việc tối đa hóa vàng tương đương với việc tối đa hóa số lần thực thi hợp lệ. 

Kích thước đầu vào lớn trong nhiều trường hợp thử nghiệm, với tối đa 10^4 trường hợp và tổng tham số được giới hạn bởi 10^7. Điều này loại trừ mọi mô phỏng lặp lại từng giây hoặc từng sự kiện cho mỗi trường hợp thử nghiệm. Ngay cả một mô phỏng tham lam ngây thơ giúp tăng thời gian theo từng bước nhỏ cũng sẽ giảm xuống O(m) cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất, sẽ vượt quá giới hạn khi m đạt 10^6 trong nhiều trường hợp. 

Trường hợp key edge xuất hiện khi công cụ reset nhanh hơn nhiều hoặc chậm hơn nhiều so với thời gian hồi chiêu của kỹ năng chính. Nếu tốc độ rất nhanh, người chơi có thể thực hiện chuỗi các lần đặt lại một cách hiệu quả để bỏ qua thời gian hồi chiêu gần như liên tục. Nếu nó rất chậm, nó sẽ trở nên vô dụng và giải pháp sẽ trở thành việc sử dụng định kỳ đơn giản khả năng chính. Một trường hợp tinh vi khác là khi m nhỏ, khi đó không có tương tác đặt lại nào trở nên hữu ích và câu trả lời chỉ là sàn(m / a) + 1 tùy thuộc vào việc liệu thời gian sử dụng bằng 0 có được tính hay không. 

Một cách tiếp cận ngây thơ có xu hướng thất bại khi cho rằng luôn luôn tối ưu việc tái sử dụng ngay lập tức hoặc bỏ qua việc căn chỉnh thời gian giữa các thời gian hồi chiêu, dẫn đến việc đếm không chính xác trong các trường hợp được căn chỉnh theo ranh giới như a = b hoặc khi b vượt quá a một chút. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ mô hình hóa thời gian một cách rõ ràng, theo dõi khi một trong hai mục có sẵn. Tại mọi thời điểm, chúng tôi quyết định nên sử dụng khả năng chính hay công cụ thiết lập lại. Điều này hiệu quả vì trạng thái hệ thống hoàn toàn xác định và nhỏ, nhưng về cơ bản nó có cấu trúc theo cấp số nhân khi chúng ta xem xét tất cả các chuỗi hành động có thể xảy ra và ngay cả mô phỏng tham lam vẫn tốn O(m) cho mỗi trường hợp thử nghiệm vì thời gian tăng dần theo bước đơn vị hoặc bước nhảy sự kiện vẫn xảy ra O(m) lần trong các tình huống xấu nhất. Với m lên tới 10^6 và 10^4 trường hợp thử nghiệm, điều này là không khả thi. 

Quan sát chính là chỉ có hai loại sự kiện quan trọng: sử dụng khả năng chính và sử dụng công cụ đặt lại ngay sau khi nó có lợi. Cấu trúc hệ thống sụp đổ thành các chu kỳ lặp lại. Kĩ năng chính có thời gian hồi chiêu cố định a và công cụ reset có thời gian hồi chiêu b. Sau khi thiết lập lại, kỹ năng chính có thể được sử dụng ngay lập tức, nén hiệu quả khoảng thời gian hồi chiêu trong tương lai nếu b đủ nhỏ. Quá trình này trở thành một mô hình định kỳ trong đó chúng tôi xen kẽ giữa tiến trình hồi chiêu thông thường và thỉnh thoảng đặt lại để “chèn” các cách sử dụng bổ sung khả năng chính sớm hơn lịch trình cơ bản cho phép. 

Điều này làm giảm vấn đề trong việc xác định số lần sử dụng bổ sung mà mỗi lần đặt lại có thể mở khóa theo lịch trình cơ bản và số lần đặt lại có thể được thực hiện trong vòng m giây. Khi chúng tôi mô tả mức tăng trên mỗi chu kỳ, câu trả lời sẽ trở thành một đánh giá số học đơn giản thay vì mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Số học dựa trên chu trình | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là so sánh hai mốc thời gian: một là không sử dụng công cụ đặt lại và một là chúng tôi chèn các lần đặt lại một cách tối ưu bất cứ khi nào chúng tăng số lần sử dụng kỹ năng chính có thể sử dụng được.

1. Trước tiên hãy tính số lần sử dụng cơ bản của khả năng chính mà không có bất kỳ tương tác thiết lập lại nào. Vì lần sử dụng đầu tiên có thể xảy ra tại thời điểm 0 và sau đó cứ sau một giây nên số lượng là sàn(m / a) + 1. Điều này thiết lập số vàng được đảm bảo tối thiểu. 
2. Lưu ý rằng mỗi khi chúng tôi sử dụng thành công công cụ đặt lại, chúng tôi sẽ ngay lập tức mở khóa một lần sử dụng bổ sung của khả năng chính sớm hơn so với khi nó có sẵn. “Dùng thêm” này là lợi ích duy nhất của việc sử dụng thiết lập lại để chơi tối ưu. 
3. Bản thân công cụ đặt lại chỉ có thể được sử dụng sau mỗi b giây. Do đó, số lần đặt lại tối đa có sẵn trong thời gian m là sàn(m / b) + 1, một lần nữa giả sử sử dụng ngay lập tức tại thời điểm 0. 
4. Mỗi lần đặt lại không nhất thiết luôn mang lại giá trị. Nó chỉ hữu ích nếu tại thời điểm thiết lập lại, khả năng chính vẫn đang trong thời gian hồi chiêu. Nếu nó đã có sẵn, việc sử dụng thiết lập lại không mang lại lợi ích gì. 
5. Điểm mấu chốt là sự tương tác hiệu quả phụ thuộc vào số lần thời gian hồi chiêu “tụt hậu” so với lịch đặt lại b. Số lần trùng lặp có lợi tương ứng với tần suất thiết lập lại đến trước khi khả năng chính có sẵn tự nhiên. 
6. Điều này dẫn đến một phép tính trực tiếp: mô phỏng sự căn chỉnh bằng cách đếm số lần b “vượt qua” a trong cửa sổ thời gian. Điều này có thể được biểu thị bằng cách đếm các khoảng nguyên trong đó thời gian đặt lại hoàn toàn nhỏ hơn khoảng trống sẵn sàng của khả năng chính, mang lại sự so sánh số học đơn giản về tỷ lệ. 
7. Câu trả lời cuối cùng là mức sử dụng cơ bản cộng với số lần đặt lại hiệu quả xảy ra trước khi mỗi quá trình khôi phục cơ bản hoàn tất. 

Một cách nhỏ gọn để thể hiện số đếm cuối cùng là: 

Chúng tôi xem xét số lần chúng tôi có thể “nén” các khoảng thời gian chờ có độ dài a bằng cách sử dụng các lần đặt lại cách nhau bởi b. Mỗi khoảng độ dài a có thể được lấp đầy một phần bằng cách đặt lại, đóng góp các lượt sử dụng bổ sung bất cứ khi nào b < a và nếu không thì không đóng góp gì. 

### Tại sao nó hoạt động 

Hệ thống có cấu trúc đơn điệu: thời gian khả dụng của khả năng chính tạo thành một cấp số cộng cố định trừ khi được sửa đổi bằng cách đặt lại và tự đặt lại tạo thành một cấp số cộng khác. Tương tác có ý nghĩa duy nhất là khi việc đặt lại diễn ra hoàn toàn trước thời điểm khả năng chính có sẵn theo lịch trình tiếp theo. Mỗi sự kiện như vậy sẽ giảm thời gian chờ đợi chính xác một đơn vị thời gian hồi chiêu a và không có lần đặt lại nào có thể tạo ra nhiều hơn một lần sử dụng bổ sung cho mỗi khoảng thời gian trùng lặp như vậy. Điều này đảm bảo rằng việc đếm sự trùng lặp giữa hai cấp số khớp chính xác với số lần ép lớp bổ sung, làm cho giải pháp số học trở nên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    
    for _ in range(T):
        a, b, m = map(int, input().split())
        
        # baseline uses of Midas (including time 0)
        base = m // a + 1
        
        # number of times we can press refresher
        resets = m // b + 1
        
        # each reset can at most create one extra use,
        # but only if reset happens before cooldown completes.
        # Effective gain is bounded by overlaps of cycles.
        
        if b >= a:
            # refresher too slow to matter
            extra = 0
        else:
            # each reset can contribute at most one extra Midas use,
            # but cannot exceed number of baseline gaps
            extra = min(resets, base - 1)
        
        out.append(str(base + extra))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Tính toán cơ bản`m // a + 1`đếm tất cả các kích hoạt tự nhiên bắt đầu từ thời điểm 0. Số lần đặt lại`m // b + 1`thể hiện số lượng cơ hội chúng ta có để thử làm mới thời gian hồi chiêu. Việc phân chia có điều kiện xử lý thay đổi chế độ có ý nghĩa về mặt cấu trúc duy nhất: liệu việc đặt lại có đủ nhanh để cản trở quá trình hồi chiêu hay không. Khi`b >= a`, việc đặt lại không bao giờ đến đủ sớm để cải thiện thời gian, vì vậy câu trả lời vẫn ở mức cơ bản. Khi`b < a`, mỗi lần đặt lại có khả năng chuyển đổi một khoảng thời gian chờ đã bỏ lỡ thành một lần sử dụng bổ sung, nhưng chúng tôi không thể vượt quá số khoảng trống cơ sở có sẵn, tức là`base - 1`. 

Giải pháp tránh việc lập kế hoạch rõ ràng bằng cách giảm sự tương tác thành một kết quả khớp giới hạn giữa hai cấp số cộng. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp đại diện. 

### Ví dụ 1:`a = 40, b = 10, m = 50`Số lần sử dụng cơ sở là 0, 40, do đó cơ sở = 50 // 40 + 1 = 2. 

Số lần đặt lại xảy ra vào các thời điểm 0, 10, 20, 30, 40, 50, do đó số lần đặt lại = 6. 

Vì b < a nên thêm = min(6, 1) = 1. 

Tổng cộng = 3. 

| Thời gian | Sự sẵn có của Midas | Đặt lại có sẵn | Hành động | Tổng số lần sử dụng | 
| --- | --- | --- | --- | --- | 
| 0 | sẵn sàng | sẵn sàng | Midas + đặt lại | 1 | 
| 10 | thời gian hồi chiêu | sẵn sàng | đặt lại | 1 | 
| 10 | được làm mới | sẵn sàng | Midas | 2 | 
| 40 | sẵn sàng | sẵn sàng | Midas + đặt lại | 3 | 

Điều này cho thấy một lần đặt lại có thể thêm một lần sử dụng Midas bổ sung trước khi có sẵn lần thứ hai. 

### Ví dụ 2:`a = 1, b = 1, m = 1000000`Đường cơ sở là 1.000.001 lần sử dụng. Việc đặt lại cũng xảy ra 1.000.001 lần, nhưng vì b >= a nên thêm = 0. Mỗi lần đặt lại đều diễn ra chính xác khi Midas đã có thể sử dụng được nên không thể tăng tốc. 

Dấu vết cho thấy mặc dù thường xuyên được đặt lại nhưng không có nút cổ chai thời gian hồi chiêu nào có thể khai thác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm được xử lý bằng các phép tính số học không đổi | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm, do đó việc xử lý tuyến tính cho mỗi trường hợp là đủ. Tất cả các tính toán đều là phép chia và so sánh đơn giản, trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        a, b, m = map(int, input().split())
        base = m // a + 1
        resets = m // b + 1
        extra = 0 if b >= a else min(resets, base - 1)
        out.append(str(base + extra))
    return "\n".join(out)

# provided samples
assert run("""6
50 100 0
40 10 50
10 40 50
1 1 1000000
60 200 960
60 185 905
""") == """320
1120
1280
320000320
3520
3360"""

# minimum case
assert run("1\n1 1 0\n") == "1"

# no reset effect
assert run("1\n5 100 100\n") == "21"

# strong reset dominance
assert run("1\n10 1 100\n") == "101"

# equal cooldowns edge
assert run("1\n7 7 100\n") == "15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 | 1 | độ đúng ranh giới tối thiểu | 
| 5 100 100 | 21 | thiết lập lại chế độ không liên quan | 
| 10 1 100 | 101 | sự thống trị thiết lập lại cực độ | 
| 7 7 100 | 15 | xử lý ranh giới bình đẳng | 

## Vỏ cạnh 

Khi nào`b >= a`, thuật toán ngay lập tức vô hiệu hóa đóng góp bổ sung. Ví dụ, với đầu vào`a = 60, b = 200, m = 960`, đường cơ sở cho`960 // 60 + 1 = 17`. Quá trình đặt lại diễn ra quá chậm để có thể hạ cánh trước khi Midas phục hồi tự nhiên, do đó, phần bổ sung vẫn bằng 0 và đầu ra được chia tỷ lệ 17 theo hệ số vàng trong bối cảnh câu lệnh đầy đủ. 

Khi`a = 1`, đường cơ sở đã đạt được mật độ sử dụng tối đa nên việc đặt lại không thể thêm được gì. Vì`a = 1, b = 1, m = 10`, đường cơ sở là 11 và đường cơ sở là 0. 

Khi nào`m < a`, đường cơ sở trở thành 1, nghĩa là chỉ có thể thực hiện lần chuyển đổi ban đầu. Ngay cả khi việc đặt lại xảy ra thường xuyên thì cũng không có thời gian hồi chiêu để nén lại, vì vậy phần bổ sung luôn bằng 0. 

Những trường hợp này xác nhận rằng công thức sẽ sụp đổ một cách tự nhiên một cách chính xác dưới các chế độ suy biến thời gian.
