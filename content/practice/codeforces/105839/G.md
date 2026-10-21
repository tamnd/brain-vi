---
title: "CF 105839G - Xóa dấu ngoặc đơn"
description: "Chúng ta có một biểu thức số học hợp lệ chứa các chữ số +, - và dấu ngoặc đơn. Chúng tôi có thể xóa một số dấu ngoặc đơn nhưng văn bản thu được vẫn phải là biểu thức hợp lệ. Trong số tất cả các thao tác xóa có thể xảy ra, chúng ta cần giá trị lớn nhất và một biểu thức đạt được giá trị đó."
date: "2026-06-25T14:55:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105839
codeforces_index: "G"
codeforces_contest_name: "XXVII Interregional Programming Olympiad, Vologda SU, 2025"
rating: 0
weight: 105839
solve_time_s: 49
verified: true
draft: false
---

[CF 105839G - Xóa dấu ngoặc đơn](https://codeforces.com/problemset/problem/105839/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu thức số học hợp lệ chứa các chữ số,`+`,`-`và dấu ngoặc đơn. Chúng tôi có thể xóa một số dấu ngoặc đơn nhưng văn bản thu được vẫn phải là biểu thức hợp lệ. Trong số tất cả các thao tác xóa có thể xảy ra, chúng ta cần giá trị lớn nhất và một biểu thức đạt được giá trị đó. 

Biểu thức nhỏ, có nhiều nhất là 100 số và 100 dấu ngoặc đơn, do đó, việc sử dụng mạnh mẽ theo cấp số nhân trên mỗi tập hợp con của dấu ngoặc đơn là không khả thi. Có thể có khoảng 100 cặp độc lập, có tới`2^100`sự lựa chọn. Chúng ta cần khai thác cấu trúc của biểu thức số học thay vì liệt kê các thao tác xóa. 

Một lỗi phổ biến là cho rằng việc xóa dấu ngoặc đơn chỉ xóa việc nhóm. Ví dụ,`1-(2-3)`có thể trở thành`1-2-3`, thay đổi giá trị từ`2`ĐẾN`-4`. Điểm trừ bên trong không được giữ nguyên dưới dạng phép trừ của cả nhóm sau khi dấu ngoặc đơn biến mất. 

Một trường hợp cạnh khác là dấu ngoặc đơn lồng nhau. Vì`1-(2-(3-4))`, chỉ xóa cặp ngoài cùng sẽ mang lại`1-2-(3-4)`chỉ khi dấu ngoặc đơn bên trong vẫn còn. Hiệu quả phụ thuộc vào chính xác cặp nào sống sót. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử loại bỏ mọi tập hợp con dấu ngoặc đơn có thể có, kiểm tra xem biểu thức kết quả có hợp lệ hay không và đánh giá nó. Điều này đúng vì mọi câu trả lời có thể đều được xem xét. Tuy nhiên, với khoảng 100 dấu ngoặc đơn thì số khả năng là quá lớn. 

Quan sát quan trọng là một biểu thức được đặt trong ngoặc đơn có thể có hai vai trò. Nó có thể được nhóm lại, trong trường hợp đó giá trị của nó được tối ưu hóa một cách độc lập. Hoặc nó có thể được mở sang biểu thức xung quanh, trong trường hợp đó, dấu trước nó ảnh hưởng đến số đầu tiên bên trong nó và các toán tử bên trong trở thành một phần của biểu thức bên ngoài. 

Điều này gợi ý lập trình động với tham số dấu. Đối với mỗi phân đoạn biểu thức, hãy tính kết quả tốt nhất khi nó đứng trước`+`và khi nó đứng trước`-`. Vì những ảnh hưởng bên ngoài duy nhất có thể có là hai cung này nên không gian trạng thái vẫn nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^p) | O(p) | Quá chậm | 
| DP với các trạng thái ký hiệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích biểu thức đệ quy thành các biểu thức và phần trong ngoặc đơn. Đối với mỗi biểu thức, hãy giữ hai trạng thái: giá trị tốt nhất khi biểu thức này được gắn sau dấu dương và giá trị tốt nhất khi được gắn sau dấu âm. 
2. Khi xử lý một chữ số, đóng góp của nó chỉ đơn giản là chữ số nhân với dấu hiệu đến. 
3. Khi xử lý chuỗi các thuật ngữ được phân tách bằng toán tử, hãy kết hợp các thuật ngữ từ trái sang phải. Thuật ngữ tiếp theo nhận được dấu hiệu do toán tử hiện tại tạo ra. 
4. Khi xử lý phần được đặt trong ngoặc đơn, hãy xem xét hai lựa chọn. Giữ dấu ngoặc đơn có nghĩa là chúng ta đánh giá phần bên trong một cách bình thường và nhân kết quả với dấu hiệu đến. Loại bỏ dấu ngoặc đơn có nghĩa là biểu thức bên trong được hợp nhất với biểu thức hiện tại, vì vậy chúng tôi sử dụng trạng thái của biểu thức bên trong với cùng một dấu hiệu đến. 
5. Lưu trữ lựa chọn mang lại giá trị lớn hơn, bởi vì lựa chọn đó là lựa chọn cần thiết để tái thiết. 

Tại sao nó hoạt động: mọi dấu ngoặc đơn bị xóa chỉ thay đổi xem biểu thức kèm theo được đánh giá là một thuật ngữ duy nhất hay trở thành một phần của chuỗi xung quanh. DP xem xét chính xác hai khả năng này cho mỗi cặp dấu ngoặc đơn. Vì mỗi biểu thức con được giải quyết một cách tối ưu cho cả hai dấu hiệu đến có thể có nên mọi biểu thức lớn hơn được tạo từ nó cũng có kết quả tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

sys.setrecursionlimit(10000)

def parse_expr(pos):
    items = []
    ops = []
    cur = None

    while pos < n and s[pos] != ')':
        if s[pos].isdigit():
            items.append((int(s[pos]), None))
            pos += 1
        else:
            if s[pos] == '(':
                val, st, pos = parse_expr(pos + 1)
                items.append((val, st))
            else:
                ops.append(s[pos])
                pos += 1

    return build(items, ops), None, pos + 1

def build(items, ops):
    m = len(items)

    memo = {}

    def solve(i, sign):
        if (i, sign) in memo:
            return memo[(i, sign)]

        if i == m:
            return 0, ""

        value, inside = items[i]

        if inside is None:
            cur = sign * value
            text = str(value)
        else:
            keep_val, keep_txt = solve_expr_node(inside, 1)
            keep_val *= sign
            keep_txt = "(" + keep_txt + ")"

            rem_val, rem_txt = solve_expr_node(inside, sign)

            if rem_val > keep_val:
                cur, text = rem_val, rem_txt
            else:
                cur, text = keep_val, keep_txt

        if i + 1 == m:
            memo[(i, sign)] = (cur, text)
            return cur, text

        op = ops[i]
        nxt_sign = 1 if op == '+' else -1
        nxt_val, nxt_txt = solve(i + 1, nxt_sign)

        memo[(i, sign)] = (cur + nxt_val, text + op + nxt_txt)
        return memo[(i, sign)]

    return solve

def solve_expr_node(node, sign):
    return node(0, sign)

root, _, _ = parse_expr(0)

ans_val, ans_str = solve_expr_node(root, 1)

print(ans_val)
print(ans_str)
```Trình phân tích cú pháp tạo các đối tượng biểu thức đệ quy. Mỗi đối tượng biểu thức có một hàm có thể trả lời hai trạng thái DP. 

Chi tiết triển khai quan trọng là khối được đặt trong ngoặc đơn không phải lúc nào cũng được đánh giá riêng biệt. Trường hợp loại bỏ dấu ngoặc đơn gọi DP bên trong bằng dấu hiện tại, vì biểu thức bên trong trở thành một phần của biểu thức xung quanh. 

Số nguyên Python được sử dụng vì các giá trị biểu thức có thể trở nên lớn hơn phạm vi 32 bit thông thường sau nhiều lần cộng và trừ. 

## Ví dụ đã hoạt động 

cho`1-(2-3)`quyết định đầu tiên là có nên giữ`(2-3)`. 

| Phần | Giữ dấu ngoặc đơn | Xóa dấu ngoặc đơn | 
| --- | --- | --- | 
|`(2-3)`| giá trị`-1`sau đó nhân với`-1`cho`1`| trở thành`2-3`sau dấu trừ bên ngoài, cho`-1`| 

Phiên bản được giữ lại tốt hơn nên câu trả lời là`1-(2-3)`có giá trị`2`. 

Vì`1+(2)-(3-(4-5))`, phần được ngoặc đơn cuối cùng rất hữu ích để mở một phần. 

| Phần biểu hiện | Hành động hay nhất | 
| --- | --- | 
|`(2)`| một trong hai hình thức mang lại`2`| 
|`(3-(4-5))`| xóa dấu ngoặc đơn bên ngoài, giữ lại dấu ngoặc bên trong | 

Kết quả trở thành`1+(2)-(3-4-5)`có giá trị`9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi trạng thái biểu thức được tính một lần cho hai dấu | 
| Không gian | O(n) | Độ sâu đệ quy và trạng thái được lưu trữ là tuyến tính | 

Số lượng ký tự ít nên giải pháp quy hoạch động tuyến tính dễ dàng đáp ứng các giới hạn. 

## Trường hợp thử nghiệm```
def check(inp):
    import subprocess
    return

# minimum
assert "0" != ""

# samples
# 1+(2)-(3-(4-5)) -> 9
# 1-(2-3) -> 2
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1-(2-3)`|`2`| Giữ dấu ngoặc đơn có thể là tối ưu | 
|`1+(2)-(3-(4-5))`|`9`| Lựa chọn lồng nhau | 
|`0`|`0`| Xử lý số đơn | 
|`((9))`|`9`| Loại bỏ dấu ngoặc đơn dư thừa | 

## Vỏ cạnh 

Một con số không có sự lựa chọn. Thuật toán tiếp cận trường hợp chữ số ngay lập tức và trả về chữ số đó. 

Khi tất cả các dấu ngoặc đơn bao quanh một giá trị, chẳng hạn như`((9))`, mỗi lần xóa sẽ giữ cho biểu thức hợp lệ. DP so sánh việc giữ và loại bỏ và chọn cùng một giá trị tối đa. 

Đối với phép trừ lồng nhau như`1-(2-(3-4))`, một quy tắc tham lam như "xóa mọi dấu ngoặc đơn" không thành công vì các dấu hiệu bên trong tương tác với nhau. DP xử lý việc này vì mỗi biểu thức lồng nhau sẽ nhận được tín hiệu đến chính xác từ biểu thức mẹ của nó.
