---
title: "CF 1025A - Đổi màu Doggo"
description: "Chúng ta được cấp một chuỗi biểu thị màu sắc của một dòng chó con, trong đó mỗi ký tự có màu từ 'a' đến 'z'. Mục tiêu là xác định xem liệu chúng ta có thể chuyển đổi chuỗi này để tất cả các ký tự trở nên giống nhau hay không bằng cách sử dụng một thao tác rất cụ thể."
date: "2026-06-16T21:41:58+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1025
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 505 (rated, Div. 1 + Div. 2, based on VK Cup 2018 Final)"
rating: 900
weight: 1025
solve_time_s: 133
verified: true
draft: false
---

[CF 1025A - Đổi màu Doggo](https://codeforces.com/problemset/problem/1025/A) 

**Xếp hạng:** 900 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi biểu thị màu sắc của một dòng chó con, trong đó mỗi ký tự có màu từ 'a' đến 'z'. Mục tiêu là xác định xem liệu chúng ta có thể chuyển đổi chuỗi này để tất cả các ký tự trở nên giống nhau hay không bằng cách sử dụng một thao tác rất cụ thể. 

Thao tác này cho phép chúng ta chọn một màu x chỉ khi nó hiện xuất hiện ít nhất hai lần, sau đó đổi màu mỗi lần xuất hiện của x thành một số màu y khác mà chúng ta chọn. Chúng ta có thể lặp lại thao tác này bao nhiêu lần cũng được. 

Điều quan trọng không phải là trình tự đổi màu chính xác mà là liệu cuối cùng chúng ta có thể loại bỏ tất cả trừ một lớp màu hay không. 

Hạn chế chính là một màu chỉ có thể sử dụng được để đổi màu khi nó có tần số ít nhất là 2. Một màu xuất hiện một lần không bao giờ có thể được chọn trực tiếp làm nguồn đổi màu, vì vậy các đơn sắc hoạt động giống như các phần tử “bị khóa” trừ khi chúng được hấp thụ gián tiếp bởi các hoạt động khác. 

Với n lên tới 100.000, mọi giải pháp đều phải chạy theo thời gian tuyến tính hoặc thời gian gần tuyến tính. Điều này ngay lập tức loại trừ việc mô phỏng các hoạt động hoặc quét lặp lại chuỗi sau mỗi lần đổi màu, vì chúng có thể giảm xuống O(n²) trong các trường hợp bất lợi. 

Trường hợp góc tinh tế phát sinh khi tất cả các ký tự đều khác biệt. Ví dụ,`abcdef`. Mỗi màu xuất hiện một lần nên không thể thực hiện thao tác nào. Câu trả lời rõ ràng là "Không". 

Một trường hợp góc khác là khi tất cả các ký tự đều giống hệt nhau, như`aaaaa`. Ở đây chúng ta không cần thao tác nào cả nên câu trả lời là "Có". 

Một trường hợp thú vị hơn là khi có chính xác một màu có tần số lớn hơn một và tất cả các màu khác đều là màu đơn, chẳng hạn như`aab`. Mặc dù một màu có thể sử dụng được nhưng có những màu không bao giờ có thể bị loại bỏ vì không có màu nào khác có đủ tần số để được chọn làm nguồn trong chuỗi các lần đổi màu. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ cố gắng mô phỏng các hoạt động. Ở mỗi bước, chúng tôi sẽ quét chuỗi, chọn bất kỳ màu nào có tần số ít nhất là hai và tô màu lại tất cả các lần xuất hiện của nó thành một màu khác. Sau mỗi thao tác, chúng tôi sẽ tính toán lại tần số và lặp lại cho đến khi tất cả các ký tự đều bằng nhau hoặc không thể thực hiện được thao tác nào. 

Về nguyên tắc, điều này hoạt động vì nó tuân thủ trực tiếp các quy tắc, nhưng mỗi bước yêu cầu tính toán lại tần số trên toàn bộ chuỗi và chúng tôi có thể thực hiện tối đa các thao tác O(n). Mỗi lần tính toán lại tốn O(n), do đó độ phức tạp trong trường hợp xấu nhất trở thành O(n²), quá chậm đối với n = 100.000. 

Quan sát quan trọng là hoạt động không phụ thuộc vào vị trí, chỉ phụ thuộc vào số lượng tần số. Quá trình này có thể được coi là liên tục hợp nhất các lớp màu, nhưng chỉ các lớp có kích thước ít nhất là hai mới có thể bắt đầu hợp nhất. 

Điều này dẫn đến một bất biến đơn giản hơn nhiều: nếu tồn tại ít nhất một màu xuất hiện nhiều lần thì chúng ta có thể sử dụng nó làm màu “bộ sưu tập” và cuối cùng hấp thụ tất cả các màu khác vào đó, bởi vì khi chúng ta chọn màu mục tiêu y, nó không cần phải đáp ứng bất kỳ ràng buộc nào khi nhận các phần tử. Yêu cầu duy nhất là có ít nhất một màu nguồn có tần số ≥ 2 ở mỗi bước cho đến khi tất cả việc hợp nhất hoàn tất. 

Vì vậy, tình huống duy nhất khiến chúng ta bị mắc kẹt là khi không có màu nào có tần số ≥ 2 khi bắt đầu. Trong trường hợp đó, không có thao tác nào có thể được thực hiện, vì vậy chúng ta không bao giờ có thể hợp nhất chuỗi trừ khi nó đã đồng nhất. 

Vì vậy, vấn đề được rút gọn thành một kiểm tra đơn giản: nếu chuỗi đã có một ký tự riêng biệt, hãy trả lời "Có". Nếu không, hãy kiểm tra xem tần số ký tự có ít nhất là 2 hay không. Nếu có, hãy trả lời "Có", nếu không thì "Không". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Kiểm tra tần số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách phân tích cấu trúc tần số của chuỗi. 

1. Đếm tần số của từng ký tự từ 'a' đến 'z'. Điều này nắm bắt tất cả thông tin liên quan đến hoạt động vì vị trí không quan trọng. 
2. Kiểm tra xem có bao nhiêu ký tự riêng biệt tồn tại. Nếu có chính xác một thì chuỗi đã đồng nhất nên không cần thao tác nào và chúng ta có thể trả về ngay "Có". Điều này hoạt động vì điều kiện mục tiêu đã được thỏa mãn. 
3. Nếu có nhiều ký tự riêng biệt, hãy quét xem có ký tự nào xuất hiện ít nhất hai lần hay không. Nếu ký tự như vậy tồn tại, chúng tôi trả về "Có" vì ký tự này có thể đóng vai trò là nguồn hợp lệ cho ít nhất một thao tác. 
4. Nếu không có ký tự nào có tần số ≥ 2 thì trả về "No". Trong tình huống này, mỗi ký tự là duy nhất, do đó không thể áp dụng thao tác nào và không thể bắt đầu quá trình hợp nhất. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là cách duy nhất để thay đổi cấu hình là chọn màu nguồn có tần số ít nhất là hai. Nếu không có màu như vậy tồn tại, hệ thống sẽ bị đóng băng hoàn toàn. Nếu có ít nhất một màu tồn tại, chúng ta luôn có thể sử dụng nó để hấp thụ các màu khác vì màu đích không có hạn chế. Điều này có nghĩa là một lớp màu “hoạt động” duy nhất là đủ để thúc đẩy hệ thống hướng tới sự thống nhất hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    freq = [0] * 26
    for ch in s:
        freq[ord(ch) - 97] += 1

    distinct = sum(1 for x in freq if x > 0)
    if distinct == 1:
        print("Yes")
        return

    for x in freq:
        if x >= 2:
            print("Yes")
            return

    print("No")

if __name__ == "__main__":
    solve()
```Mảng tần số nén toàn bộ đầu vào vào trạng thái có kích thước không đổi vì chỉ tồn tại 26 ký tự. Việc kiểm tra cho`distinct == 1`xử lý trực tiếp trường hợp đã thống nhất. Vòng lặp thứ hai phát hiện xem có bất kỳ hoạt động hợp lệ nào có thể bắt đầu hay không; không có sự khởi đầu như vậy thì không thể chuyển đổi được. 

Không có vấn đề gì về thứ tự phát sinh vì chúng tôi không bao giờ mô phỏng các hoạt động mà chỉ kiểm tra các điều kiện tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`aabddc`Tần số phát triển như sau: 

| Bước | Tần số (khác 0) | Màu sắc riêng biệt | Tần số bất kỳ ≥ 2 | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | a:2, b:1, d:2, c:1 | 4 | Có | Có | 

Chúng tôi ngay lập tức quan sát thấy nhiều bản sao, vì vậy tồn tại ít nhất một nguồn hợp lệ. Hệ thống có thể bắt đầu hợp nhất, cuối cùng thu gọn tất cả các màu thành một. 

### Ví dụ 2 

đầu vào:`abcd`| Bước | Tần số (khác 0) | Màu sắc riêng biệt | Tần số bất kỳ ≥ 2 | Quyết định | 
| --- | --- | --- | --- | --- | 
| Ban đầu | a:1, b:1, c:1, d:1 | 4 | Không | Không | 

Không có ký tự nào xuất hiện hai lần nên không thể thực hiện thao tác nào. Vì sợi dây chưa đồng nhất nên chúng ta bị mắc kẹt. 

Dấu vết cho thấy chế độ lỗi chính: việc không có bất kỳ nguồn nào có thể sử dụng được sẽ ngăn cản ngay cả lần chuyển đổi đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tính toán tần số cộng với quét liên tục trên 26 chữ cái | 
| Không gian | O(1) | Mảng tần số có kích thước cố định có chiều dài 26 | 

Lời giải dễ dàng nằm trong giới hạn vì n lớn nhất là 100.000 và mọi phép toán đều tuyến tính với các hằng số rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        solve()
    return output.getvalue().strip()

# provided samples
assert run("6\naabddc\n") == "Yes"
assert run("4\nabcd\n") == "No"

# custom cases
assert run("1\na\n") == "Yes"          # single character
assert run("5\naaaaa\n") == "Yes"      # already uniform
assert run("3\nabc\n") == "No"         # all distinct
assert run("4\naabc\n") == "Yes"       # one duplicate enables process
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 a`| Có | Kích thước tối thiểu | 
|`aaaaa`| Có | Đã đồng phục | 
|`abc`| Không | Không có bản sao tồn tại | 
|`aabc`| Có | Nguồn hợp lệ duy nhất tồn tại | 

## Vỏ cạnh 

Đối với đầu vào`a`, mảng tần số chứa một mục nhập khác không. Thuật toán phát hiện`distinct == 1`và ngay lập tức trả về "Có", xử lý chính xác trường hợp tầm thường. 

Vì`abcd`, tất cả các tần số đều bằng 1. Thuật toán bỏ qua điều kiện đầu tiên và không tìm thấy`freq >= 2`, trả về "Không", phù hợp với thực tế là không thể thực hiện thao tác nào. 

Vì`aab`, tần số là`a:2, b:1`. Thuật toán phát hiện nguồn hợp lệ và trả về "Có", phản ánh rằng sự hiện diện của một màu có thể trùng lặp là đủ để thúc đẩy quá trình chuyển đổi.
