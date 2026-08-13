---
title: "CF 102281A - \u041f\u0440\u043e\u0441\u0442\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta có một đống n cookie. Hai người chơi lần lượt loại bỏ cookie, Giáo sư X di chuyển trước. Một động thái hợp pháp sẽ loại bỏ p^k cookie, trong đó p là số nguyên tố và k là số nguyên không âm. Vì k = 0 được cho phép nên việc loại bỏ chính xác 1 cookie luôn là hợp pháp."
date: "2026-08-13T09:18:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "A"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 76
verified: true
draft: false
---

[CF 102281A - \u041f\u0440\u043e\u0441\u0442\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một đống`n`cookie. Hai người chơi lần lượt loại bỏ cookie, Giáo sư X di chuyển trước. Một động thái hợp pháp sẽ loại bỏ`p^k`bánh quy, ở đâu`p`là số nguyên tố và`k`là số nguyên không âm. Từ`k = 0`được cho phép, loại bỏ chính xác`1`cookie luôn hợp pháp. Người chơi loại bỏ cookie cuối cùng sẽ thắng. 

Nhiệm vụ là xác định giáo sư nào chiến thắng trong lối chơi tối ưu. Nếu X thắng, chúng ta cũng cần thực hiện một nước đi hợp pháp đầu tiên khiến Giáo sư Y rơi vào thế thua. 

Đầu vào chứa một số nguyên`n`, số lượng cookie ban đầu, với`1 <= n <= 10^9`. Một giải pháp lập trình động trên tất cả các vị trí sẽ yêu cầu phải làm việc với tới một tỷ trạng thái, vượt xa thời gian và bộ nhớ sẵn có. Ngay cả một thế hệ sàng hoặc thế hệ nguyên tố được tối ưu hóa cũng không giải quyết được vấn đề thực sự, vì bản thân trò chơi có cấu trúc đơn giản hơn nhiều. Chiến lược chiến thắng chỉ có thể được xác định bằng cách sử dụng`n mod 6`. 

Có một số trường hợp nhỏ trong đó việc triển khai dựa trên danh sách nước đi không đầy đủ có thể thất bại. Vì`n = 1`, X có thể loại bỏ`1 = 2^0`, vì vậy đầu ra đúng là`X WINS`theo sau là`1`. Một giải pháp coi số mũ là dương sẽ khẳng định sai rằng X không chuyển động. 

Vì`n = 4`, X thắng bằng cách loại bỏ`4 = 2^2`, không để lại cookie nào. Vì vậy, đầu ra đúng là`X WINS`theo sau là`4`. Điều này nắm bắt các triển khai chỉ tạo ra số nguyên tố và quên các số nguyên tố cao hơn. 

Vì`n = 6`, đầu ra đúng là`Y WINS`. Động thái hấp dẫn`2`lá`4`, Nhưng`4`chính nó đã mang lại chiến thắng cho người chơi tiếp theo. Trên thực tế, mọi động thái hợp pháp từ`6`để lại một số không bội số`6`và mọi bội số của`6`đang chiến thắng. 

Vì`n = 12`, đầu ra đúng lại là`Y WINS`. Lập luận tương tự được áp dụng vì`12`chia hết cho`6`. Việc thực hiện bất cẩn chỉ kiểm tra xem`n`bản thân nó là một động thái hợp pháp, thay vì phân tích vị trí dẫn đến, có thể hiểu sai trường hợp này. 

## Phương pháp tiếp cận 

Người giải trò chơi trực tiếp sẽ phân loại mọi vị trí từ`0`bởi vì`n`. Chức vụ`0`đang thua vì người chơi di chuyển không có cookie. Đối với mỗi vị trí tích cực, chúng tôi sẽ thử mọi lũy thừa không vượt quá vị trí đó. Vị trí sẽ thắng nếu có ít nhất một nước đi đến vị trí thua và thua nếu mọi nước đi hợp pháp đều đạt đến vị trí thắng. 

Sự tái diễn này đúng vì trò chơi là hữu hạn. Sau khi thực hiện một động thái, số lượng cookie sẽ giảm đi đáng kể, do đó, các vị trí có thể được phân loại từ giá trị nhỏ hơn đến giá trị lớn hơn. Vấn đề là kích thước của nó. Ngay cả khi chỉ xem xét những động thái cơ bản, vẫn có`50,847,534`số nguyên tố không vượt quá`10^9`. Một DP ngây thơ có tới một tỷ trạng thái và việc kiểm tra nhiều động thái hợp pháp cho mỗi trạng thái sẽ dẫn đến một số lượng lớn các hoạt động, vượt xa giới hạn 1,5 giây. Việc bổ sung thêm quyền lực không cải thiện được tình hình. 

Lực lượng vũ phu hoạt động vì biểu đồ trò chơi không theo chu kỳ, nhưng nó không thành công vì nó coi tất cả các vị trí là không liên quan. Quan sát quan trọng là các bước đi hợp pháp tương tác hoàn hảo với dư lượng modulo`6`. 

Mỗi động thái hợp pháp là một quyền lực hàng đầu. Không có số nào trong số này chia hết cho`6`, vì lũy thừa nguyên tố chỉ có một ước số nguyên tố. Bây giờ hãy xem xét một vị trí chia hết cho`6`. Trừ bất kỳ nước đi hợp pháp nào sẽ tạo ra một số không chia hết cho`6`. Ngược lại, mọi modulo dư lượng khác 0`6`có thể bị loại bỏ trực tiếp bằng một trong năm động thái hợp pháp nhỏ:`1`là`p^0`,`2`Và`3`là số nguyên tố,`4 = 2^2`, Và`5`là nguyên tố. 

Giả định`n`có dư lượng`1`modulo`6`. Đang xóa`1`đạt đến bội số của`6`. Đối với dư lượng`2`, di dời`2`; cho dư lượng`3`, di dời`3`; cho dư lượng`4`, di dời`4`; và cho dư lượng`5`, di dời`5`. Như vậy mọi bội số của`6`có sự di chuyển đến bội số của`6`. 

Điều này mang lại đặc tính hoàn chỉnh. Chính xác là bội số dương của`6`đang mất vị trí. Từ bội số của`6`, mọi nước đi hợp pháp sẽ không phải là bội số của`6`, đó là chiến thắng. Từ mọi không bội số của`6`, một trong`1, 2, 3, 4, 5`đạt đến bội số của`6`, mang lại cho người chơi hiện tại một nước đi chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | Ít nhất O(n * pi(n)) | O(n) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng cookie ban đầu`n`. Chỉ modulo còn lại của nó`6`vấn đề, bởi vì vị thế thua chính xác là bội số dương của`6`. 
2. Tính toán`r = n % 6`. Nếu như`r == 0`, đầu ra`Y WINS`. Không có nước đi đầu tiên thắng cho X vì mọi nước đi hợp pháp đều có modulo dư lượng khác 0`6`, do đó đống kết quả không chia hết cho`6`. 
3. Nếu`r != 0`, đầu ra`X WINS`và chọn`r`là bước đi đầu tiên của X. giá trị`r`luôn luôn hợp pháp:`1`là lũy thừa bậc 0 của bất kỳ số nguyên tố nào,`2`Và`3`là số nguyên tố,`4`là`2^2`, Và`5`là nguyên tố. 
4. Sau khi loại bỏ`r`, số còn lại là`n - r`, chia hết cho`6`. Giáo sư Y do đó rơi vào tình thế thua cuộc. Dù Y thực hiện nước đi hợp pháp nào thì số kết quả cũng không chia hết cho`6`, do đó X có thể quay trở lại bội số của`6`. Việc lặp lại chiến lược này cuối cùng sẽ không để lại cookie nào cho X lấy. 

### Tại sao nó hoạt động 

Điều bất biến là X luôn có thể trả trò chơi về bội số của`6`sau hành động của Y. bội số của`6`không thể di chuyển đến bội số khác của`6`, bởi vì không có nước đi hợp pháp nào có thể chia hết cho`6`. Mặt khác, từ mọi không bội số của`6`, X có thể loại bỏ phần dư của nó theo modulo`6`, và phần dư đó luôn là một trong những động thái hợp pháp`1, 2, 3, 4, 5`. Thus multiples of`6`chính xác là các vị trí thua và thuật toán vừa xác định người chiến thắng vừa đưa ra nước đi thắng đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    move = n % 6

    if move == 0:
        print("Y WINS")
    else:
        print("X WINS")
        print(move)

if __name__ == "__main__":
    solve()
```Chương trình đọc giá trị đầu vào duy nhất và tính modulo còn lại của nó`6`. Không cần phải tạo ra các số nguyên tố, bởi vì động thái duy nhất mà chúng ta từng chọn một cách rõ ràng là năm lũy thừa nguyên tố nhỏ nhất có liên quan. 

Khi phần dư bằng 0, vị trí bị mất nên chỉ in dòng đầu ra. Khi số dư khác 0 thì số dư đó là một nước đi hợp lệ và được in ở dòng thứ hai. 

ranh giới`n = 1`hoạt động tự động:`1 % 6 = 1`, vì vậy X sẽ xóa một cookie. Không có mối lo ngại về tràn trong Python và ngay cả trong ngôn ngữ có chiều rộng cố định, tất cả các giá trị liên quan đều có nhiều nhất`10^9`. 

Thứ tự của các hoạt động cũng quan trọng về mặt khái niệm. Chúng ta phải kiểm tra`n % 6`trước khi chọn hành động. Phần dư bằng 0 có nghĩa là không có động tác nào bảo toàn tính chia hết cho`6`, trong khi số dư khác 0 mang lại chính xác nước đi cần thiết để tiếp cận lớp thua cuộc. 

## Ví dụ đã hoạt động 

Mẫu chính thức đầu tiên có`n = 10`. 

|`n`|`n % 6`| Quyết định | Bước đi đầu tiên | 
| --- | --- | --- | --- | 
| 10 | 4 | X thắng | 4 | 
| 6 | 0 | Y đang thua | | 

Sau khi X loại bỏ`4`, vẫn còn sáu cái bánh quy. số`6`chia hết cho`6`, do đó, người chơi đến lượt sẽ bị thua. Điều này mang lại đầu ra cần thiết`X WINS`theo sau là`4`. Ví dụ này cũng cho thấy tại sao nước đi thắng không nhất thiết phải là nước nguyên tố, bởi vì`4 = 2^2`là quyền lực tối cao hợp pháp. 

Mẫu chính thức thứ hai có`n = 12`. 

|`n`|`n % 6`| Quyết định | Bước đi đầu tiên | 
| --- | --- | --- | --- | 
| 12 | 0 | Y thắng | không | 

Mười hai cookie đã là bội số của`6`. Mỗi nước đi hợp lệ sẽ loại bỏ một số không chia hết cho`6`, vì vậy mọi nước đi của X đều mang lại cho Y một vị thế chiến thắng. Do đó đầu ra chỉ đơn giản là`Y WINS`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một hoạt động modulo và đầu ra có kích thước không đổi | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Đầu vào có thể lớn như`10^9`, nhưng thuật toán không bao giờ lặp lại tối đa`n`và không bao giờ xây dựng được tập hợp lũy thừa nguyên tố. Thời gian chạy và mức sử dụng bộ nhớ của nó là không đổi, do đó, nó thoải mái trong giới hạn 1,5 giây và 128 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    n = int(input())
    move = n % 6

    if move == 0:
        print("Y WINS")
    else:
        print("X WINS")
        print(move)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        old_stdout = sys.stdout
        sys.stdout = io.StringIO()

        solve()
        result = sys.stdout.getvalue()

        sys.stdout = old_stdout
        return result
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run("10\n") == "X WINS\n4\n", "sample 1"
assert run("12\n") == "Y WINS\n", "sample 2"

# Minimum-size input
assert run("1\n") == "X WINS\n1\n", "minimum n"

# Small losing position
assert run("6\n") == "Y WINS\n", "first positive multiple of 6"

# Maximum-size input
assert run("1000000000\n") == "X WINS\n4\n", "maximum n"

# Boundary immediately before a multiple of 6
assert run("11\n") == "X WINS\n5\n", "residue 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`X WINS`/`1`| Đầu vào tối thiểu và di chuyển công suất thứ 0 | 
|`6`|`Y WINS`| Vị thế thua tích cực đầu tiên | 
|`1000000000`|`X WINS`/`4`| Đầu vào và dư lượng tối đa`4`| 
|`11`|`X WINS`/`5`| Ranh giới ngay trước bội số của`6`| 

## Vỏ cạnh 

cho`n = 1`, thuật toán tính toán`1 % 6 = 1`. Nó chọn di chuyển`1`, điều này là hợp pháp vì`1 = 2^0`, và đống còn lại bằng 0. Đầu ra là`X WINS`theo sau là`1`. 

Vì`n = 4`, phần còn lại là`4`. Thuật toán chọn bước di chuyển`4`, Và`4`là hợp pháp vì`4 = 2^2`. Cọc trở thành số 0 ngay lập tức, do đó X thắng. Trường hợp này xác minh rằng tập hợp di chuyển có chứa các lũy thừa nguyên tố nằm ngoài chính các số nguyên tố. 

Vì`n = 6`, phần còn lại bằng 0, do đó thuật toán báo cáo`Y WINS`. Nếu X loại bỏ`1`,`2`,`3`,`4`, hoặc`5`, đống còn lại lần lượt là`5`,`4`,`3`,`2`, hoặc`1`, tất cả không phải là bội số của`6`. Mỗi vị trí đó cho phép người chơi tiếp theo loại bỏ phần dư tương ứng và quay lại bội số của`6`. 

Vì`n = 12`, lập luận tương tự cũng được áp dụng. Mỗi hành động hợp pháp đều để lại một trong những dư lượng`1, 2, 3, 4, 5`modulo`6`, do đó X không thể chuyển sang vị trí thua khác. Y luôn có thể trả lời bằng cách loại bỏ phần dư đó và khôi phục khả năng chia hết cho`6`. 

Để có giá trị lớn nhất`n = 1,000,000,000`, phần còn lại là`4`, do đó chương trình xuất ra`X WINS`Và`4`. Đống kết quả chứa`999,999,996`cookie, có thể chia cho`6`. Độ lớn của`n`không ảnh hưởng đến số lượng hoạt động được thực hiện.
