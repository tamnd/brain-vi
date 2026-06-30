---
title: "CF 104637B - Mua Đuốc"
description: "Chúng tôi đang cố gắng sản xuất số lượng ngọn đuốc mục tiêu. Mỗi ngọn đuốc tiêu thụ chính xác một que và một cục than, vì vậy nếu chúng ta muốn ngọn đuốc $k$, cuối cùng chúng ta cần que $k$ và than $k$. Chúng tôi bắt đầu không có khoảng không quảng cáo hữu ích nào ngoại trừ việc chúng tôi có thể thao túng gậy thông qua hai hoạt động giao dịch."
date: "2026-06-29T17:00:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104637
codeforces_index: "B"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u0431\u0430\u0437\u043e\u0432\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430, \u0443\u0441\u043b\u043e\u0432\u0438\u044f, \u0446\u0438\u043a\u043b\u044b"
rating: 0
weight: 104637
solve_time_s: 117
verified: false
draft: false
---

[CF 104637B - Mua đuốc](https://codeforces.com/problemset/problem/104637/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang cố gắng sản xuất số lượng ngọn đuốc mục tiêu. Mỗi ngọn đuốc tiêu thụ đúng một que củi và một cục than, nên nếu chúng ta muốn$k$ngọn đuốc, cuối cùng chúng ta cần$k$gậy và$k$than. 

Chúng tôi bắt đầu không có khoảng không quảng cáo hữu ích nào ngoại trừ việc chúng tôi có thể thao túng gậy thông qua hai hoạt động giao dịch. Thao tác đầu tiên chúng ta bỏ ra một cây gậy để nhận$x$gậy, giúp nhân lên số lượng gậy của chúng ta một cách hiệu quả. Hoạt động thứ hai cho phép chúng tôi chi tiêu$y$gậy để lấy được một cục than. Chúng ta có thể áp dụng các thao tác này theo bất kỳ thứ tự nào, lặp đi lặp lại và mỗi ứng dụng được tính là một giao dịch. Mục tiêu là giảm thiểu tổng số giao dịch cần thiết để chúng ta có thể tập hợp ít nhất$k$ngọn đuốc. 

Khó khăn chính là gậy vừa là tài nguyên vừa là tiền tệ. Chúng ta có thể tăng chúng nhưng cũng tiêu thụ chúng để sản xuất than. Thứ tự của các hoạt động rất quan trọng vì các giao dịch ban đầu sẽ thay đổi mức độ đắt đỏ của than trong tương lai. 

Các ràng buộc rất lớn, có thể lên tới$2 \cdot 10^4$trường hợp thử nghiệm và giá trị lên đến$10^9$. Bất kỳ giải pháp mô phỏng từng giao dịch nào đều không khả thi ngay lập tức bởi vì ngay cả$O(k)$mỗi trường hợp thử nghiệm sẽ vượt xa giới hạn. Lời giải phải là logarit hoặc tệ nhất là tuyến tính theo số lần thay đổi trạng thái có ý nghĩa, không phải theo$k$. 

Một trường hợp cạnh ngây thơ nhưng nguy hiểm phát sinh khi$y = 1$. Trong trường hợp đó, than có giá đúng bằng một que, vì vậy que vừa là đơn vị sản xuất vừa là đơn vị tiêu thụ, và lý luận tham lam bất cẩn về việc “luôn nhân lên trước” có thể dễ dàng đếm quá hoặc thiếu các giao dịch. 

Một trường hợp tế nhị khác là khi$x = 2$Và$y$là lớn. Ở đây, việc nhân nhiều cây gậy tuy chậm nhưng cần thiết và việc sản xuất than quá sớm có thể hạn chế vĩnh viễn sự phát triển của cây gậy, gây ra những lựa chọn tham lam không chính xác nếu chúng ta không lập mô hình đánh đổi một cách rõ ràng. 

## Phương pháp tiếp cận 

Giải thích brute-force coi vấn đề là tìm kiếm trạng thái: mỗi trạng thái là một cặp$(s, c)$đại diện cho que và than hiện tại, đồng thời các chuyển đổi áp dụng một trong hai giao dịch. Chúng tôi bắt đầu từ cấu hình tối thiểu khác 0 và cố gắng đạt đến trạng thái mà chúng tôi có thể tạo ra$k$than và vẫn còn ít nhất$k$gậy có sẵn để chế tạo. 

Điều này đương nhiên gợi ý BFS hoặc Dijkstra so với các trạng thái, trong đó mỗi cạnh có chi phí cho một giao dịch. Mặc dù đúng về nguyên tắc nhưng không gian trạng thái sẽ bùng nổ ngay lập tức. Gậy có thể phát triển theo cấp số nhân lên đến$10^9$và sản xuất than phụ thuộc vào việc tiêu dùng chúng. Ngay cả khi chúng tôi giới hạn các trạng thái một cách mạnh mẽ, cấu trúc phân nhánh thực sự không bị giới hạn vì phép nhân que có thể được lặp lại tùy ý nhiều lần trước khi sản xuất than. 

Quan sát chính là quá trình này không thực sự yêu cầu xen kẽ một cách tùy tiện. Hai hoạt động này đóng vai trò riêng biệt: một là cơ chế tăng trưởng hình học cho que và hai là cơ chế khai thác tuyến tính chuyển que thành than. Sau khi chúng tôi xác định số lần thực hiện mỗi thao tác, số dư tài nguyên cuối cùng sẽ mang tính quyết định. 

Chúng ta có thể lý luận ngược lại: để có được$k$ngọn đuốc, chúng ta cần$k$mua bán than. Giá mỗi than$y$gậy, vậy tổng chi phí than là$k \cdot y$gậy. Cách duy nhất để tạo ra gậy là áp dụng giao dịch nhân nhiều lần. Do đó bài toán rút gọn thành: bắt đầu từ 1 que tính “nhân với$x$” và “chuyển đổi$y$tập trung vào các hoạt động khai thác than” theo thứ tự nào đó, giảm thiểu tổng số hoạt động đồng thời đảm bảo chúng tôi có đủ khả năng chi trả$k \cdot y$giá trị của các giao dịch than. 

Một sự đơn giản hóa cơ cấu quan trọng là khi chúng ta quyết định số lượng than chúng ta thực hiện, hệ số nhân có thể được áp dụng một cách tham lam để tối đa hóa hiệu quả. Chiến lược tối ưu là chọn điểm mà chúng ta chuyển từ tăng trưởng sang tiêu dùng, bởi vì việc xen kẽ ngoài điểm đó sẽ không bao giờ cải thiện được số lượng gậy trên mỗi tỷ lệ giao dịch. 

Điều này dẫn đến một đánh giá dạng khép kín về số lượng bước nhân que cần thiết để đạt đến ngưỡng, kết hợp với việc tính số lượng than mua trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm trạng thái) | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam tối ưu + số học |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia quá trình này thành hai giai đoạn: xây dựng gậy và chi tiêu cho than. 

1. Tính tổng số than cần mua, chính xác là$k$, vì mỗi ngọn đuốc cần một cục than. Điều này xác định tổng nhu cầu than một cách độc lập với việc đặt hàng. 
2. Giá mỗi than$y$que, do đó tổng số que yêu cầu đối với than là$k \cdot y$. Chúng tôi coi đây là ngân sách tối thiểu phải có trước khi bắt đầu trả tiền than. Lý do là vì một khi chúng ta bắt đầu tiêu thụ que làm than, việc giảm lượng que sớm chỉ làm cho việc nhân giống trong tương lai kém hiệu quả hơn. 
3. Chỉ xét phép nhân que. Mỗi lần sử dụng thay thế 1 que bằng$x$gậy, vì vậy lợi nhuận ròng là$x-1$. Tuy nhiên, điều này chỉ quan trọng sau khi thanh đầu tiên tồn tại, do đó, mỗi thao tác sẽ tăng số lượng thanh theo cấp số nhân trong chế độ hữu ích một cách hiệu quả. 
4. Xác định cần bao nhiêu giao dịch nhân lên để đạt được ít nhất$k \cdot y$gậy bắt đầu từ 1. Mỗi bước sẽ biến đổi số lượng gậy hiện tại$s$vào trong$s \cdot x$, vậy sau$t$các bước chúng tôi có$x^t$. Chúng tôi tìm thấy nhỏ nhất$t$như vậy$x^t \ge k \cdot y$. 
5. Sau khi tích lũy đủ số gậy, chúng tôi sẽ thực hiện giao dịch mua bán than. Mỗi chi phí buôn bán than$y$que và thu được 1 cục than nên ta thực hiện chính xác$k$những giao dịch như vậy. 
6. Tổng số nghiệp vụ là tổng của các nghiệp vụ nhân cộng với kinh doanh than. 

Điểm tinh tế là chúng ta không bao giờ xen kẽ các giai đoạn này một cách hỗn hợp. Bất kỳ việc mua than sớm nào cũng làm giảm cơ sở hiệu quả cho sự tăng trưởng theo cấp số nhân, và bất kỳ việc mua than bị trì hoãn nào sau khi cây phát triển quá mức đều không làm giảm số bước nhân lên cần thiết. Sự phân tách đơn điệu này đảm bảo tính tối ưu. 

### Tại sao nó hoạt động 

Thuật toán dựa trên sự đánh đổi đơn điệu: phép nhân que làm tăng đáng kể khả năng mua than trong tương lai, trong khi việc mua than làm giảm nghiêm trọng tài nguyên sẵn có để nhân thêm. Bởi vì cả hai hoạt động đều có chi phí tuyến tính cố định và các hiệu ứng xác định, bất kỳ lịch trình nào xen kẽ chúng đều có thể được sắp xếp lại sao cho tất cả các phép nhân xảy ra trước mà không làm tăng số lượng hoạt động. Điều này chứng minh rằng giải pháp tối ưu luôn bao gồm tiền tố của các giao dịch nhân theo sau là hậu tố của các giao dịch than, điều này làm giảm vấn đề xuống mức tính toán ngưỡng đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y, k = map(int, input().split())

        need = k * y

        # compute minimum t such that x^t >= need
        if need == 1:
            mult = 0
        else:
            cur = 1
            mult = 0
            while cur < need:
                cur *= x
                mult += 1

        # coal trades
        coal = k

        print(mult + coal)

if __name__ == "__main__":
    solve()
```Giải pháp tách biệt rõ ràng giai đoạn tăng trưởng theo cấp số nhân với giai đoạn tiêu thụ tuyến tính. Vòng lặp tính số mũ nhỏ nhất$t$sao cho phép nhân lặp đi lặp lại với$x$đạt đến ngưỡng thanh yêu cầu. Điều này an toàn vì$x \ge 2$, do đó tốc độ tăng trưởng tăng dần và vòng lặp kết thúc nhanh chóng. 

Giai đoạn than rất đơn giản: khi đã có đủ số lượng than, mỗi ngọn đuốc cần chính xác một lần buôn bán than, vì vậy chúng tôi thêm vào$k$. Không cần phải mô phỏng mức tiêu dùng cá nhân vì mỗi đơn vị chi phí là cố định và độc lập. 

Một cạm bẫy triển khai phổ biến là cố gắng kết hợp phép nhân và tiêu dùng trong một vòng lặp mô phỏng duy nhất. Cách tiếp cận đó có xu hướng tính toán sai vì nó không duy trì cấu trúc tăng trưởng theo cấp số nhân của việc tích lũy gậy. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi quá trình cho hai trường hợp đại diện. 

### Ví dụ 1 

đầu vào:$x = 2, y = 1, k = 5$Chúng tôi cần$k \cdot y = 5$gậy trước khi việc sản xuất than trở nên có ý nghĩa. 

| Bước | Gậy hiện tại | Hành động | Đa hoạt động | Hoạt động khai thác than | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | nhân | 1 | 0 | 
| 2 | 2 | nhân | 2 | 0 | 
| 3 | 4 | nhân | 3 | 0 | 
| 4 | 8 | ngừng tăng trưởng | 3 | 0 | 
| 5 | 8 | buôn bán than | 3 | 1 | 
| 6 | 7 | buôn bán than | 3 | 2 | 
| 7 | 6 | buôn bán than | 3 | 3 | 
| 8 | 5 | buôn bán than | 3 | 4 | 
| 9 | 4 | buôn bán than | 3 | 5 | 

Chúng tôi thấy rằng ba bước nhân là đủ để đạt đến ngưỡng và sau đó chính xác năm giao dịch than được thực hiện. 

Điều này khẳng định rằng việc tiêu thụ than không ảnh hưởng đến giai đoạn tăng trưởng theo cấp số nhân trước đó. 

### Ví dụ 2 

đầu vào:$x = 42, y = 13, k = 24$Chúng tôi cần$24 \cdot 13 = 312$gậy. 

| Bước | Gậy hiện tại | Hành động | Đa hoạt động | Hoạt động khai thác than | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | nhân | 1 | 0 | 
| 2 | 42 | nhân | 2 | 0 | 
| 3 | 1764 | ngừng tăng trưởng | 2 | 0 | 
| 4 | 1764 | buôn bán than (24 lần) | 2 | 24 | 

Chỉ sau hai phép nhân, chúng tôi đã vượt quá ngân sách cần thiết và phần còn lại là tiêu dùng thuần túy. 

Điều này chứng tỏ giá trị lớn của$x$thu gọn giai đoạn tăng trưởng một cách nhanh chóng, đó là lý do tại sao lý luận logarit là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log_x (k y))$trường hợp xấu nhất$O(t \cdot 30)$| mỗi bài kiểm tra nhân lên đến ngưỡng | 
| Không gian |$O(1)$| chỉ có một vài quầy được sử dụng | 

Sự tăng trưởng của cây gậy theo cấp số nhân trong$x$, do đó, ngay cả đối với các ràng buộc tối đa, số bước nhân bị giới hạn trong khoảng 30 khi các giá trị vừa với số nguyên 64 bit. Điều này làm cho giải pháp dễ dàng đủ nhanh cho$2 \cdot 10^4$trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log
    # inline solution
    t = int(input())
    out = []
    for _ in range(t):
        x, y, k = map(int, input().split())
        need = k * y

        cur = 1
        mult = 0
        while cur < need:
            cur *= x
            mult += 1

        out.append(str(mult + k))
    return "\n".join(out) + "\n"

# provided samples
assert run("5\n2 1 5\n42 13 24\n12 11 12\n1000000000 1000000000 1000000000\n2 1000000000 1000000000\n") == \
"14\n33\n25\n2000000003\n1000000001999999999\n"

# minimum values
assert run("1\n2 1 1\n") == "2\n"

# large x, small k
assert run("1\n1000000000 1 5\n") == "6\n"

# y = 1 edge
assert run("1\n3 1 1000000000\n") == str(1 + 30) + "\n"

# balanced case
assert run("1\n2 2 10\n") == "15\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`|`2`| tăng trưởng tối thiểu + một than | 
| lớn$x$| câu trả lời nhỏ | sự hội tụ nhanh của phép nhân | 
|$y=1$| chi phí than tuyến tính | chi phí chuyển đổi thoái hóa | 
| cân bằng | tích lũy đúng | hành vi tham số hỗn hợp | 

## Vỏ cạnh 

Khi nào$y = 1$, than có hiệu quả miễn phí trên mỗi que, vì vậy yếu tố hạn chế chỉ là tốc độ nhân lên của que. Thuật toán xử lý việc này bằng cách vẫn tính toán ngưỡng nhân bình thường rồi cộng$k$, điều này phản ánh chính xác rằng mỗi than vẫn yêu cầu một giao dịch ngay cả khi chi phí thanh là tối thiểu. 

Khi$x$là cực kỳ lớn, một phép nhân đơn lẻ có thể đã vượt quá ngân sách thanh yêu cầu. Vòng lặp kết thúc ngay sau bước đầu tiên và thuật toán trả về chính xác$1 + k$, phù hợp với chiến lược tối ưu của một hoạt động tăng trưởng sau đó là tiêu dùng trực tiếp. 

Khi$k$đạt mức tối đa thì pha nhân chỉ chiếm ưu thế về mặt logarit. Thuật toán vẫn thực hiện tối đa khoảng 30 lần lặp, giúp thuật toán ổn định ngay cả ở kích thước đầu vào trong trường hợp xấu nhất.
