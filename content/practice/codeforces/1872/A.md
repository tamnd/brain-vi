---
title: "CF 1872A - Hai Tàu"
description: "Chúng tôi được cấp hai thùng chứa lượng nước có giá trị thực. Một cái bắt đầu bằng a, cái kia bắt đầu bằng b. Chúng ta cũng có một chiếc cốc có thể chuyển tối đa c đơn vị nước trong một thao tác."
date: "2026-06-08T23:17:39+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1872
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 895 (Div. 3)"
rating: 800
weight: 1872
solve_time_s: 73
verified: true
draft: false
---

[CF 1872A - Hai tàu](https://codeforces.com/problemset/problem/1872/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp hai thùng chứa lượng nước có giá trị thực. Một bắt đầu với`a`, cái còn lại với`b`. Chúng tôi cũng có một chiếc cốc có thể chuyển nhiều nhất`c`đơn vị nước trong một lần vận hành. Trong một hoạt động, chúng tôi chọn bình nguồn, lấy một lượng nước lên đến`c`, rồi đổ vào bình kia. 

Quá trình thay đổi hai giá trị một cách không đối xứng, nhưng tổng lượng nước không đổi. Sau mỗi lần di chuyển, một bình giảm đi một lượng và bình kia tăng cùng một lượng. Mục đích là làm cho cả hai bình chứa cùng một lượng. 

Vì tổng lượng nước là cố định nên trạng thái cuối cùng duy nhất có thể xảy ra là mỗi bình chứa`(a + b) / 2`. Nếu như`a == b`, câu trả lời là không có nước đi nào cả. Nếu không, chúng tôi đang cố gắng chuyển chính xác`|a - b| / 2`đơn vị nước từ bình lớn hơn sang bình nhỏ hơn, tối đa bằng cách chuyển đổi kích thước`c`. 

Những hạn chế rất nhỏ:`a, b, c ≤ 100`, và lên đến`1000`trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi mô phỏng trên các trạng thái phân số chi tiết hoặc tìm kiếm trên các lựa chọn liên tục. Bất kỳ giải pháp đúng nào cũng phải giảm quy trình xuống tính toán số học trực tiếp cho mỗi trường hợp thử nghiệm. 

Một trường hợp khó nhận thấy là khi`c`đủ lớn để có thể chuyển toàn bộ số tiền chênh lệch chỉ trong một nước đi. Một điều nữa là khi`c`rất nhỏ nên cần phải chuyển nhiều phần. Một kẻ tham lam ngây thơ luôn chuyển tiền chính xác`c`vẫn có thể làm việc ở đây, nhưng chỉ khi xử lý cẩn thận phần còn lại của việc chuyển giao được yêu cầu. 

## Phương pháp tiếp cận 

Cách diễn giải thô bạo sẽ mô phỏng quy trình theo từng bước. Ở mỗi bước, chúng tôi chọn tàu lớn hơn, chuyển một số tiền lên tới`c`, và cố gắng giảm bớt sự khác biệt một cách tham lam. Nếu chúng ta luôn chuyển`c`(hoặc số tiền cần thiết còn lại nếu nhỏ hơn), chúng tôi giảm chênh lệch tuyệt đối giữa`a`Và`b`. 

Mô phỏng này là chính xác vì mọi thao tác đều làm giảm nghiêm ngặt`|a - b|`trừ khi chúng đã bằng nhau. Tuy nhiên, độ phức tạp của nó tỷ lệ thuận với số lần di chuyển, trong trường hợp xấu nhất là khoảng`|a - b| / c`. Vì các giá trị ở đây nhỏ nên điều này khả thi về mặt kỹ thuật, nhưng nó che giấu cấu trúc thực tế: mỗi bước di chuyển sẽ làm giảm sự khác biệt nhiều nhất`c`, và chúng ta chỉ quan tâm đến việc đạt được hiệu số bằng 0. 

Quan sát quan trọng là chúng ta không bao giờ cần theo dõi từng trạng thái riêng lẻ. Chúng tôi chỉ theo dõi sự khác biệt tuyệt đối`d = |a - b|`. Mỗi nước đi có thể làm giảm sự khác biệt này nhiều nhất`c`và vì chúng ta luôn có thể chọn hướng một cách tối ưu (từ lớn hơn đến nhỏ hơn), chiến lược tối ưu là luôn giảm sự khác biệt càng nhiều càng tốt. Điều đó biến vấn đề thành một phép chia trần đơn giản: có bao nhiêu khối kích thước`c`cần thiết để che đậy`d / 2`đơn vị chuyển nhượng, nhưng chúng tôi lý luận rõ ràng hơn về mặt giảm chênh lệch trên mỗi lần di chuyển. 

Mỗi nước đi làm giảm sự khác biệt nhiều nhất`2c`xét về góc độ giải quyết sự mất cân bằng, nhưng trực tiếp hơn: chuyển giao`x`từ lớn hơn đến nhỏ hơn làm giảm sự khác biệt bằng cách`2x`, do đó nước đi tối ưu luôn luôn cần`x = min(c, remaining_needed/2)`có hiệu lực. Điều này sụp đổ thành một phép đếm tham lam đơn giản: liên tục trừ đi tối đa`c`từ sự mất cân bằng cho đến khi nó biến mất. 

Như vậy câu trả lời là`ceil(|a - b| / (2c))`nếu chúng ta lập mô hình rút gọn một cách đối xứng, nhưng có nguồn gốc rõ ràng hơn trong giải pháp CF tiêu chuẩn, thì việc giảm liên tục chênh lệch cho đến khi bằng 0 sẽ đơn giản hóa việc liên tục giảm chênh lệch cho đến 0, bằng`ceil(|a - b| / (2c))`. Trong thực tế, công thức được chấp nhận là`ceil(|a - b| / (2c))`phù hợp với thực tế mỗi bước di chuyển`c`từ bên này sang bên kia. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bước mô phỏng | O( | a-b | /c) | 
| Công thức trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chênh lệch tuyệt đối`d = |a - b|`. Điều này thể hiện sự mất cân bằng tổng thể cần phải được loại bỏ thông qua chuyển giao. 
2. Nếu`d == 0`, đầu ra`0`ngay lập tức vì cả hai bình đều đã bằng nhau và không cần thực hiện thao tác nào. 
3. Mỗi thao tác có thể di chuyển tối đa`c`đơn vị từ tàu này sang tàu khác, giúp giảm sự mất cân bằng theo một hướng một cách tối ưu. 
4. Vì mỗi nước đi sẽ làm giảm sự mất cân bằng một cách chính xác`2 * x`Ở đâu`x ≤ c`, mức giảm tốt nhất có thể cho mỗi lần di chuyển là`2c`. 
5. Tính số nước đi là số nguyên nhỏ nhất`k`như vậy`k * 2c ≥ d`. 
6. Đầu ra`ceil(d / (2c))`. 

### Tại sao nó hoạt động 

Trạng thái của hệ thống được mô tả đầy đủ bằng sự khác biệt`d`. Mỗi chuyển động đều bảo toàn khối lượng và sự thay đổi`d`bằng cách trừ đi hai lần số tiền đã chuyển. Bởi vì chúng tôi luôn chọn tàu lớn hơn làm nguồn nên chúng tôi luôn giảm thiểu`d`thay vì tăng nó. Không có chuỗi nước đi nào có thể làm tốt hơn việc luôn sử dụng số lần chuyển tối đa được phép`c`, vì bất kỳ khoản chuyển tiền nhỏ hơn nào cũng sẽ làm giảm`d`chậm hơn và không bao giờ tạo ra lợi ích bù đắp trong tương lai. Điều này làm cho quá trình tương đương với việc trừ liên tục`2c`từ`d`cho đến khi nó đạt tới 0, mang lại số bước tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())
    d = abs(a - b)
    if d == 0:
        print(0)
    else:
        # each move can reduce difference by at most 2c
        print((d + 2 * c - 1) // (2 * c))
```Việc triển khai bắt đầu bằng cách đọc từng trường hợp thử nghiệm và tính toán sự khác biệt tuyệt đối giữa hai bình. Bước không tầm thường duy nhất là nhận ra rằng chúng ta không bao giờ cần mô phỏng chuyển khoản một cách rõ ràng; toàn bộ quá trình giảm xuống để thu hẹp sự khác biệt này. 

Phần trần số nguyên`(d + 2*c - 1) // (2*c)`mã hóa thực tế là chúng tôi đang nhóm sự mất cân bằng thành các phần có kích thước`2c`. Trường hợp cạnh`d == 0`được xử lý riêng để tránh số học không cần thiết, mặc dù công thức cũng sẽ trả về 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`a = 3, b = 7, c = 2`Chúng tôi tính toán`d = 4`. 

| Bước | Sự khác biệt d | Hành động | Giảm | Mới d | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | bắt đầu | - | 4 | 
| 1 | 4 | chuyển 2 từ b sang a | 4 → 0 | 0 | 

Chỉ cần một lần di chuyển vì một lần chuyển kích thước`c`chính xác loại bỏ sự mất cân bằng. Điều này khẳng định rằng khi`d ≤ 2c`, một động tác là đủ. 

### Ví dụ 2:`a = 17, b = 4, c = 3`Chúng tôi tính toán`d = 13`. 

| Bước | Sự khác biệt d | Hành động | Giảm | Mới d | 
| --- | --- | --- | --- | --- | 
| 0 | 13 | bắt đầu | - | 13 | 
| 1 | 13 | chuyển 3 | 13 → 7 | 7 | 
| 2 | 7 | chuyển 3 | 7 → 1 | 1 | 
| 3 | 1 | chuyển 1 | 1 → 0 | 0 | 

Điều này cho thấy rằng việc chuyển tiền tối đa lặp đi lặp lại sẽ làm giảm sự khác biệt một cách tham lam cho đến khi nó biến mất. Động thái cuối cùng là chuyển giao một phần, đó là lý do tại sao hành vi trần là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí 1000 trường hợp thử nghiệm chỉ yêu cầu các đánh giá số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())
        d = abs(a - b)
        if d == 0:
            out.append("0")
        else:
            out.append(str((d + 2*c - 1) // (2*c)))
    return "\n".join(out)

# provided samples
assert run("""6
3 7 2
17 4 3
17 17 1
17 21 100
1 100 1
97 4 3
""") == """1
3
0
1
50
16"""

# custom cases
assert run("1\n1 1 1\n") == "0", "already equal"
assert run("1\n1 3 10\n") == "1", "large c single move"
assert run("1\n1 100 1\n") == "50", "maximum chain transfers"
assert run("1\n10 1 2\n") == "3", "direction reversed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`0`| trường hợp đã bằng nhau | 
|`1 3 10`|`1`| di chuyển một lần là đủ | 
|`1 100 1`|`50`| chuỗi dài các lần chuyển tối thiểu | 
|`10 1 2`|`3`| tính đúng đắn dưới vai trò đảo ngược | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chênh lệch nhỏ hơn dung tích cốc. Ví dụ`a = 1, b = 4, c = 10`cho`d = 3`. Thuật toán tính toán`(3 + 19) // 20 = 1`, điều này phù hợp với thực tế là chúng ta có thể chuyển tất cả lượng nước cần thiết trong một lần di chuyển mà không cần phải chia nhỏ. 

Một trường hợp khác là khi`a == b`. Ở đây, sự khác biệt được tính toán là bằng 0 và việc thoát sớm sẽ trả về 0 lượt di chuyển ngay lập tức. Nếu không có kiểm tra này, công thức vẫn tạo ra số 0, nhưng nhánh rõ ràng làm cho logic nhất quán với cách giải thích rằng không cần thực hiện thao tác nào. 

Trường hợp thứ ba là khi`c = 1`, buộc phải chuyển đơn vị. Vì`a = 1, b = 100`, chênh lệch là 99 và mỗi lần di chuyển sẽ giảm tối đa 2. Công thức mang lại`(99 + 1) // 2 = 50`, phù hợp với sự cần thiết của nhiều điều chỉnh nhỏ, luân chuyển hướng tối ưu để hội tụ chính xác về đẳng thức.
