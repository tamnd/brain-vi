---
title: "CF 102443G - Quá nhiều dấu gạch nối"
description: "Chúng ta có một chuỗi chỉ gồm + và -. Chúng ta có thể chèn dấu ngoặc nhọn vào bất cứ đâu mà không thay đổi ký tự gốc."
date: "2026-08-09T01:43:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "G"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 110
verified: true
draft: false
---

[CF 102443G - Quá nhiều dấu gạch nối](https://codeforces.com/problemset/problem/102443/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi chỉ được làm bằng`+`Và`-`. Chúng ta có thể chèn dấu ngoặc nhọn vào bất cứ đâu mà không thay đổi ký tự gốc. Sau khi chèn vào, bản thân các dấu ngoặc phải tạo thành một chuỗi dấu ngoặc đơn hợp lệ: quét từ trái sang phải, số lượng`{`ký tự không bao giờ được nhỏ hơn số lượng ký tự`}`, và cuối cùng hai số đếm phải bằng nhau. 

Lý do chèn dấu ngoặc nhọn là để tách từng cặp dấu gạch nối liền kề. Trong chuỗi cuối cùng, giữa mỗi hai bản gốc liên tiếp`-`các ký tự phải có ít nhất một dấu ngoặc nhọn. liên tiếp`+`các nhân vật không cần bất kỳ sự đối xử đặc biệt nào. 

Trong số tất cả các chuỗi hợp lệ, chúng tôi chỉ giữ lại những chuỗi sử dụng số dấu ngoặc nhọn tối thiểu có thể. Đây là những chuỗi thoát tối ưu. Chúng được sắp xếp theo thứ tự từ điển`+ < - < { < }`, và nhiệm vụ là xuất ra`k`-thứ 1 Nếu ít hơn`k`chuỗi tối ưu tồn tại, chúng tôi in`Overflow`. Chuỗi đầu vào ban đầu có tối đa 60 ký tự, trong khi`k`có thể lớn như`10^18`. 

Đại lượng hữu ích đầu tiên là số`r`các vị trí trong đó cả hai ký tự gốc liên tiếp đều là`-`. Mỗi vị trí như vậy phải có ít nhất một dấu ngoặc nhọn được chèn vào. Như vậy ít nhất`r`niềng răng là cần thiết. Tuy nhiên, các dấu ngoặc nhọn phải tạo thành một dãy cân đối nên tổng số của chúng phải là số chẵn. Do đó, số dấu ngoặc tối thiểu ít nhất là số chẵn nhỏ nhất`r`. 

Ví dụ, với`s = "--"`, có một kề cận bị cấm, do đó cần có ít nhất một dấu ngoặc nhọn. Một dấu ngoặc không thể tạo thành một chuỗi cân bằng, do đó phương án tối ưu sử dụng hai dấu ngoặc. Ba chuỗi tối ưu là`-{-}`,`-{}-`, Và`{-}-`. Giải pháp bất cẩn chỉ sử dụng một thanh giằng ở vị trí cần thiết sẽ vi phạm điều kiện thanh giằng cân bằng. 

Một trường hợp cạnh khác là một chuỗi không có dấu gạch nối liên tiếp. Ví dụ, với```
-+-+
2
```không có gì để thoát nên chuỗi tối ưu duy nhất là`-+-+`. Từ`k = 2`, đầu ra đúng là`Overflow`. Giải pháp luôn chèn một cặp dấu ngoặc nhọn sẽ sai vì tác vụ yêu cầu số lượng dấu ngoặc nhọn tối thiểu được chèn vào. Đây cũng là một trong những mẫu chính thức. 

Các vị trí khoảng trống ở đầu và cuối chuỗi cũng là những vị trí hợp lệ cho dấu ngoặc nhọn. Vì`s = "--"`, chuỗi`{-}-`là tối ưu mặc dù dấu ngoặc nhọn của nó không nằm giữa hai dấu gạch nối. Dấu ngoặc nhọn mở nằm trước dấu gạch ngang đầu tiên và dấu ngoặc nhọn đóng nằm giữa các dấu gạch ngang nên khoảng cách cần thiết vẫn được bảo vệ và các dấu ngoặc nhọn vẫn cân đối. Việc triển khai chỉ xem xét việc chèn dấu ngoặc nhọn trực tiếp vào các khoảng trống xấu sẽ bỏ lỡ chuỗi này. 

Độ dài giới hạn 60 đủ nhỏ cho một chương trình động có trạng thái có nhiều chiều, nhưng quá lớn để liệt kê tất cả các chuỗi thoát có thể có. giá trị`k <= 10^18`cũng cho chúng ta biết rằng số lượng nên được giới hạn ở mức`10^18`: khi một trạng thái có ít nhất số lần hoàn thành đó, giá trị chính xác lớn hơn của nó không bao giờ có thể ảnh hưởng đến câu trả lời. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể thử mọi cách có thể để chèn dấu ngoặc nhọn, kiểm tra xem chuỗi dấu ngoặc thu được có cân bằng hay không, kiểm tra từng cặp dấu gạch nối liên tiếp và sau đó chỉ giữ lại các chuỗi có số dấu ngoặc tối thiểu. Điều này đúng vì mọi chuỗi thoát có thể đều được xem xét. Vấn đề là không gian tìm kiếm. Có tới 60 ký tự gốc và trong trường hợp xấu nhất, có tới 60 dấu ngoặc nhọn trong một giải pháp tối ưu. Cây tìm kiếm theo từng ký tự với bốn ký tự đầu ra có thể có độ sâu lên tới 120, tạo ra giới hạn trên thô 

[ 
1 + 4 + 4^2 + \dots + 4^{120} 
= \frac{4^{121}-1}{3}, 
] 

đại khái là vậy`2^240 / 3`nút. Ngay cả việc liệt kê các chuỗi giằng cân bằng hợp lệ cẩn thận hơn nhiều cũng theo cấp số nhân. Đối với 60 dấu ngoặc nhọn, đã có sẵn các khả năng ở thang số Catalan, vượt xa những gì có thể tạo ra trong một giây. 

Brute-force hoạt động vì mọi lựa chọn đều có thể được kiểm tra cục bộ và các điều kiện hợp lệ cuối cùng rất đơn giản. Nó thất bại vì nhiều tiền tố khác nhau có những khả năng giống hệt nhau trong tương lai. Ví dụ: khi chúng tôi biết có bao nhiêu ký tự gốc đã được sử dụng, bao nhiêu dấu ngoặc nhọn đã được chèn, số dư dấu ngoặc nhọn hiện tại và liệu khoảng trống hiện tại đã nhận được dấu ngoặc nhọn hay chưa thì lịch sử chính xác trước thời điểm đó không còn quan trọng nữa. 

Quan sát đó biến vấn đề thành một chương trình động trạng thái hữu hạn. Chúng tôi đếm số lần hoàn thành tối ưu từ mỗi trạng thái như vậy. Khi số lượng đó đã có sẵn, việc hủy xếp hạng từ điển trở nên đơn giản: tại mỗi vị trí, chúng tôi xem xét các ký tự tiếp theo có thể có theo thứ tự bắt buộc`+`,`-`,`{`,`}`, hỏi có bao nhiêu chuỗi tối ưu hoàn chỉnh bắt đầu với mỗi lựa chọn và chọn lựa chọn đó hoặc bỏ qua toàn bộ khối của nó. 

Điểm tinh tế duy nhất là cách thể hiện khoảng cách giữa các ký tự gốc. Giả định`i`các ký tự gốc đã được phát ra. Chúng tôi hiện đang lấp đầy khoảng trống ngay trước khi`s[i]`, hoặc khoảng cách cuối cùng khi`i == n`. Nếu như`s[i-1]`Và`s[i]`cả hai đều`-`, khoảng trống này phải nhận được ít nhất một dấu ngoặc trước khi chúng ta được phép phát ra`s[i]`. Một boolean duy nhất ghi lại xem điều đó đã xảy ra chưa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`| số mũ trong`n`| Quá chậm | 
| Tối ưu |`O(n B²)`|`O(n B²)`| Đã chấp nhận | 

Đây`B`là số dấu ngoặc nhọn tối thiểu và`B <= 60`. 

## Hướng dẫn thuật toán 

1. Đếm từng cặp liền kề`s[i - 1] = s[i] = '-'`. Gọi số này`r`. Mỗi khoảng trống như vậy cần ít nhất một dấu ngoặc nhọn, vì vậy không có giải pháp hợp lệ nào có thể sử dụng ít hơn`r`niềng răng. 
2. Tính toán`B`, số dấu ngoặc nhọn tối thiểu, là số chẵn nhỏ nhất lớn hơn hoặc bằng`r`. Các dấu ngoặc nhọn tạo thành một chuỗi dấu ngoặc cân bằng nên tổng số của chúng phải là số chẵn. giá trị`B`luôn có thể đạt được: phân phối ít nhất một dấu ngoặc cho mỗi khoảng trống cần thiết và sử dụng các dấu ngoặc còn lại để hoàn thành một chuỗi cân bằng. 
3. Xác định trạng thái lập trình động`(i, balance, used, has)`. Đây`i`là số ký tự gốc đã được phát ra,`balance`là số lượng hiện tại chưa từng có`{`nhân vật,`used`là tổng số niềng răng được chèn cho đến nay và`has`cho biết khoảng trống hiện tại đã chứa ít nhất một dấu ngoặc chưa. 
4. Từ một tiểu bang, hãy cân nhắc việc chèn`{`. Điều này là hợp pháp khi ít hơn`B`niềng răng đã được sử dụng. Nó tăng lên`balance`, tăng`used`và đánh dấu khoảng trống hiện tại là có một dấu ngoặc nhọn. 
5. Cân nhắc việc chèn`}`. Điều này chỉ hợp pháp khi`balance > 0`và ít hơn`B`niềng răng đã được sử dụng. Nó giảm`balance`, tăng`used`và cũng đánh dấu khoảng trống hiện tại là có một dấu ngoặc nhọn. Dấu ngoặc đóng không thể được phát ra ở số dư 0 vì điều đó sẽ làm cho chuỗi dấu ngoặc không hợp lệ. 
6. Cân nhắc việc xuất ký tự gốc tiếp theo`s[i]`. Điều này chỉ hợp pháp nếu khoảng cách hiện tại không phải là một trong những khoảng trống dấu gạch nối bắt buộc hoặc`has`đã đúng rồi. Sau khi phát ra ký tự gốc, hãy tiến lên`i`và thiết lập lại`has`thành sai vì chúng ta đã bước vào khoảng trống tiếp theo. 
7. Trạng thái kết thúc đạt được khi tất cả các ký tự gốc đã được phát ra. Tại thời điểm đó, chỉ có trạng thái chính xác`B`dấu ngoặc nhọn được chèn và số dư bằng 0 là hợp lệ. Khoảng trống cuối cùng được phép chứa các dấu ngoặc nhọn, do đó DP tiếp tục chèn các dấu ngoặc nhọn ngay cả khi`i == n`. 
8. Ghi nhớ số lần hoàn thành hợp lệ ở mọi trạng thái. Từ`k`nhiều nhất là`10^18`, giới hạn mỗi lần đếm tại`10^18`. Số lượng lớn hơn số này không thể phân biệt được để quyết định phía nào của`k`họ nằm trên. 
9. Trước khi xây dựng câu trả lời, hãy kiểm tra số lượng trạng thái ban đầu. Nếu nó nhỏ hơn`k`, in`Overflow`. 
10. Ngược lại, hãy xây dựng câu trả lời từ trái sang phải. Tại mỗi trạng thái, hãy kiểm tra các ký tự tiếp theo có thể có theo thứ tự từ điển. Vì`+`hoặc`-`, có nhiều nhất một lựa chọn ký tự gốc. Vì`{`Và`}`, DP đưa ra số lần hoàn thành sau khi lấy ký tự đó. Nếu một nhánh chứa ít hơn`k`chuỗi, trừ số đó từ`k`và thử ký tự tiếp theo. Nếu không, hãy nối thêm ký tự và chuyển sang trạng thái đó. 

Điều bất biến là mọi trạng thái DP thể hiện chính xác thông tin có thể ảnh hưởng đến tính hợp lệ trong tương lai: số lượng chuỗi gốc còn lại, số lượng dấu ngoặc nhọn có sẵn, số dư dấu ngoặc đơn hiện tại và liệu khoảng cách bắt buộc hiện tại đã được bảo vệ hay chưa. Do đó, mỗi lần tiếp tục hợp pháp được tính đúng một lần. Trong quá trình hủy xếp hạng, thứ tự từ điển sẽ phân chia tất cả các phần hoàn thành thành các khối liên tiếp theo ký tự tiếp theo của chúng, do đó bỏ qua các khối hoàn chỉnh và đi xuống khối chứa`k`luôn chọn đúng chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def solve():
    s = input().strip()
    k = int(input())
    n = len(s)

    required = 0
    for i in range(1, n):
        if s[i - 1] == '-' and s[i] == '-':
            required += 1

    B = required
    if B % 2:
        B += 1

    memo = {}

    def add_cap(a, b):
        x = a + b
        return LIMIT if x > LIMIT else x

    def count(i, balance, used, has):
        key = (i, balance, used, has)
        if key in memo:
            return memo[key]

        if used > B or balance > B:
            return 0

        if i == n:
            if used == B and balance == 0:
                return 1

            if used == B:
                return 0

        ans = 0

        # Try inserting '{'.
        if used < B:
            ans = add_cap(ans, count(i, balance + 1, used + 1, True))

        # Try inserting '}'.
        if used < B and balance > 0:
            ans = add_cap(ans, count(i, balance - 1, used + 1, True))

        # Try emitting the next original character.
        if i < n:
            required_gap = (
                i > 0 and
                s[i - 1] == '-' and
                s[i] == '-'
            )

            if not required_gap or has:
                ans = add_cap(ans, count(i + 1, balance, used, False))

        memo[key] = ans
        return ans

    total = count(0, 0, 0, False)

    if total < k:
        print("Overflow")
        return

    ans = []
    i = 0
    balance = 0
    used = 0
    has = False

    while not (i == n and used == B and balance == 0):
        choices = []

        # Original character, if legal.
        if i < n:
            required_gap = (
                i > 0 and
                s[i - 1] == '-' and
                s[i] == '-'
            )

            if not required_gap or has:
                choices.append((s[i], (i + 1, balance, used, False)))

        # Opening brace.
        if used < B:
            choices.append(('{', (i, balance + 1, used + 1, True)))

        # Closing brace.
        if used < B and balance > 0:
            choices.append(('}', (i, balance - 1, used + 1, True)))

        choices.sort(key=lambda x: x[0])

        for ch, state in choices:
            ni, nb, nu, nh = state
            ways = count(ni, nb, nu, nh)

            if ways < k:
                k -= ways
            else:
                ans.append(ch)
                i, balance, used, has = state
                break

    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên đếm chính xác số khoảng trống phải chứa dấu ngoặc nhọn. Từ giá trị đó,`B`thu được bằng cách làm tròn đến số chẵn. Trường hợp đặc biệt`B = 0`không cần xử lý riêng vì cùng một DP đương nhiên chỉ cho phép chuỗi gốc. 

các`count`chức năng là chương trình động trung tâm. các`i == n`trường hợp này được xử lý trước logic chuyển tiếp vì không còn ký tự gốc nào để phát ra, nhưng các dấu ngoặc nhọn vẫn có thể phải được đặt ở khoảng trống cuối cùng. Một trạng thái chỉ chấp nhận khi tất cả`B`niềng răng đã được sử dụng và số dư bằng không. 

các`required_gap`biểu thức kiểm tra chính xác khoảng cách giữa`s[i - 1]`Và`s[i]`. Đây là lý do tại sao điều kiện sử dụng`i > 0`Và`i < n`ngầm thông qua nhánh xung quanh. các`has`cờ chỉ được đặt lại sau khi ký tự gốc được phát ra, vì vậy các dấu ngoặc nhọn được chèn trước ký tự đó đều thuộc cùng một khoảng cách. 

Việc xây dựng từ điển có chủ ý xem xét đặc điểm gốc,`{`, Và`}`riêng biệt và sắp xếp chúng theo giá trị ký tự thực tế của chúng. Từ`+ < - < { < }`, thứ tự chuỗi thông thường của Python đã đưa ra thứ tự được yêu cầu. 

Không có vấn đề tràn số nguyên trong Python, nhưng giới hạn rõ ràng tại`10^18`giữ số lượng giá trị số nguyên riêng biệt trong giới hạn DP và phản ánh độ chính xác duy nhất mà quy trình không xếp hạng cần. Giới hạn được áp dụng sau mỗi lần thêm, không phải sau khi toàn bộ DP đã được đánh giá. 

## Ví dụ đã hoạt động 

Đối với mẫu chính thức đầu tiên,`s = "++--"`Và`k = 2`. Có một khoảng cách xấu nên số lượng dấu ngoặc nhọn tối thiểu là hai. Các chuỗi tối ưu được sắp xếp như sau`++-{-}`,`++-{}-`,`++{-}-`,`+{+-}-`, Và`{++-}-`. Do đó, câu thứ hai chính là câu trả lời. Mẫu và thứ tự này được đưa ra trong tuyên bố ban đầu. 

|`i`|`balance`|`used`|`has`| Tiền tố được chọn | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | sai |`+`| 
| 1 | 0 | 0 | sai |`++`| 
| 2 | 0 | 0 | sai |`++-`| 
| 3 | 0 | 0 | sai |`++-`| 
| 3 | 1 | 1 | đúng |`++-{`| 
| 4 | 0 | 2 | sai |`++-{}`| 
| 4 | 0 | 2 | sai |`++-{}-`| 

Ở hai vị trí đầu tiên, các chuỗi tối ưu duy nhất bắt đầu bằng các dấu cộng ban đầu đó tạo thành khối sớm nhất về mặt từ điển. Ở dấu gạch nối đầu tiên, ứng viên đầu tiên cũng bị ép buộc. Bên trong khoảng trống cần thiết, chọn`}`ngay lập tức là không thể vì số dư bằng 0, vì vậy`{`được chọn. Khi dấu ngoặc mở đã được chèn vào, dấu gạch nối ban đầu tiếp theo sẽ nhỏ hơn về mặt từ điển so với dấu ngoặc nhọn`}`, do đó thuật toán sẽ phát ra nó trước khi đóng cặp. Điều này tạo ra`++-{}-`. 

Đối với mẫu chính thức thứ hai,`s = "-+-+"`. Không có dấu gạch nối liên tiếp, vì vậy`B = 0`. Chuỗi thoát tối ưu duy nhất là chính chuỗi gốc. Từ`k = 2`, số DP ban đầu là một và thuật toán sẽ in ngay lập tức`Overflow`. 

|`i`|`balance`|`used`|`has`| Tiền tố được chọn | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | sai |`-`| 
| 1 | 0 | 0 | sai |`-+`| 
| 2 | 0 | 0 | sai |`-+-`| 
| 3 | 0 | 0 | sai |`-+-+`| 

Bởi vì không có dấu ngoặc nhọn nào được phép trong một chuỗi tối ưu khi dấu ngoặc nhọn bằng 0 đủ, nên có chính xác một lần hoàn thành. Việc hoàn thành thứ hai được yêu cầu không tồn tại. 

## Phân tích độ phức tạp 

hãy để`n <= 60`là độ dài chuỗi ban đầu và`B <= 60`là số lượng dấu ngoặc nhọn tối thiểu. 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n B²)`| có`O(n B²)`các trạng thái liên quan và mỗi trạng thái có các chuyển tiếp có kích thước không đổi. | 
| Không gian |`O(n B²)`| Bảng ghi nhớ lưu trữ một số lượng giới hạn cho mỗi trạng thái. | 

Boolean bổ sung`has`chỉ nhân số lượng trạng thái lên hai, do đó nó không thay đổi độ phức tạp tiệm cận. Với`n`Và`B`cả tối đa là 60, số lượng trạng thái thoải mái dưới một triệu, phù hợp với giới hạn 1 giây và 512 MB của vấn đề. 

## Trường hợp thử nghiệm```python
import sys
import io
from functools import lru_cache

LIMIT = 10**18

def solve_string(inp: str) -> str:
    data = inp.strip().split()
    s = data[0]
    k = int(data[1])
    n = len(s)

    required = sum(
        1 for i in range(1, n)
        if s[i - 1] == '-' and s[i] == '-'
    )

    B = required if required % 2 == 0 else required + 1

    @lru_cache(None)
    def count(i, balance, used, has):
        if used > B or balance > B:
            return 0

        if i == n:
            return int(used == B and balance == 0)

        ans = 0

        if used < B:
            ans = min(
                LIMIT,
                ans + count(i, balance + 1, used + 1, True)
            )

            if balance > 0:
                ans = min(
                    LIMIT,
                    ans + count(i, balance - 1, used + 1, True)
                )

        required_gap = (
            i > 0 and
            s[i - 1] == '-' and
            s[i] == '-'
        )

        if not required_gap or has:
            ans = min(
                LIMIT,
                ans + count(i + 1, balance, used, False)
            )

        return ans

    if count(0, 0, 0, False) < k:
        return "Overflow"

    ans = []
    i = balance = used = 0
    has = False

    while not (i == n and used == B and balance == 0):
        choices = []

        if i < n:
            required_gap = (
                i > 0 and
                s[i - 1] == '-' and
                s[i] == '-'
            )

            if not required_gap or has:
                choices.append(
                    (s[i], (i + 1, balance, used, False))
                )

        if used < B:
            choices.append(
                ('{', (i, balance + 1, used + 1, True))
            )

            if balance > 0:
                choices.append(
                    ('}', (i, balance - 1, used + 1, True))
                )

        choices.sort()

        for ch, state in choices:
            ways = count(*state)
            if ways < k:
                k -= ways
            else:
                ans.append(ch)
                i, balance, used, has = state
                break

    return ''.join(ans)

# Provided sample 1
assert solve_string("++--\n2\n") == "++-{}-", "sample 1"

# Provided sample 2
assert solve_string("-+-+\n2\n") == "Overflow", "sample 2"

# Minimum-size input
assert solve_string("+\n1\n") == "+", "minimum-size input"

# Boundary case: all optimal strings for "--" are
# -{-}, -{}-, {-}-
assert solve_string("--\n1\n") == "-{-}", "first lexicographic answer"
assert solve_string("--\n2\n") == "-{}-", "second lexicographic answer"
assert solve_string("--\n3\n") == "{-}-", "last lexicographic answer"
assert solve_string("--\n4\n") == "Overflow", "past last answer"

# Maximum-size input, all characters equal
assert solve_string("+" * 60 + "\n1\n") == "+" * 60, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`+\n1`|`+`| Độ dài tối thiểu và không cần niềng răng | 
|`--\n1`|`-{-}`| Khoảng cách bắt buộc và lựa chọn từ điển | 
|`--\n3`|`{-}-`| Niềng răng có thể xảy ra ngoài khoảng cách yêu cầu | 
|`--\n4`|`Overflow`| Ranh giới chính xác của số lượng giải pháp | 
| 60 điểm cộng với`k = 1`| 60 điểm cộng | Độ dài đầu vào tối đa và các ký tự hoàn toàn bằng nhau | 

## Vỏ cạnh 

cho`s = "--"`Và`k = 1`, có một khoảng cách bắt buộc, vì vậy số lượng dấu ngoặc nhọn tối thiểu là hai. Ký tự đầu tiên của lời giải nhỏ nhất về mặt từ điển là`-`, bởi vì`- < {`. Sau dấu gạch nối đó, khoảng trống bắt buộc phải chứa dấu ngoặc nhọn. Lựa chọn`{`là có thể và để lại dấu gạch nối thứ hai ban đầu làm ký tự tiếp theo, vì`- < }`. Phần còn lại`}`được đặt sau dấu gạch nối thứ hai, cho`-{-}`. Thuật toán đạt chính xác chuỗi trạng thái đó và trả về giải pháp đầu tiên. 

Vì`s = "--"`Và`k = 3`, hai ứng cử viên đầu tiên`-{-}`Và`-{}-`được bỏ qua trong quá trình hủy xếp hạng. Ứng viên còn lại bắt đầu bằng`{`, lớn hơn về mặt từ điển so với`-`, và là`{-}-`. Trường hợp này mắc sai lầm khi cho rằng mọi dấu ngoặc phải được đặt trực tiếp giữa hai dấu gạch nối. 

Vì`s = "-+-+"`, không có khoảng trống cần thiết. Số lượng dấu ngoặc nhọn tối thiểu bằng 0, vì vậy việc chèn bất kỳ dấu ngoặc nhọn nào sẽ làm cho kết quả không tối ưu. Do đó, DP có chính xác một lần hoàn thành thiết bị đầu cuối, cụ thể là`-+-+`. Với`k = 2`, số đếm ban đầu nhỏ hơn`k`, do đó thuật toán in chính xác`Overflow`. 

Đối với một chuỗi có độ dài tối đa bao gồm toàn bộ dấu cộng, chẳng hạn như 60 dấu cộng, không có khoảng trống bắt buộc và số lượng dấu ngoặc tối ưu bằng 0. DP về cơ bản chứa một đường dẫn xuyên qua các ký tự gốc và trả về đầu vào không thay đổi. Điều này thực hiện ranh giới độ dài trên mà không tạo ra sự phức tạp tổ hợp không cần thiết. 

Đối với một chuỗi bao gồm toàn bộ dấu gạch nối, mọi khoảng trống bên trong đều được yêu cầu. Với 60 dấu gạch nối thì có 59 khoảng trống bắt buộc nên số dấu ngoặc tối ưu là 60 chứ không phải 59. Dấu ngoặc phụ bị ép buộc bởi yêu cầu tổng số dấu ngoặc phải chẵn. Đây là ranh giới kiểm tra trực tiếp nhất việc tính toán`B`và DP xử lý nó mà không có trường hợp đặc biệt vì nó xử lý điều kiện khoảng cách bắt buộc và điều kiện cân bằng toàn cầu một cách độc lập.
