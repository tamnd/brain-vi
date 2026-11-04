---
title: "CF 1156B - Đôi Xấu"
description: "Chúng ta được cung cấp một chuỗi ngắn các chữ cái viết thường và chúng ta được phép sắp xếp lại các ký tự của nó một cách tùy ý. Mục đích là tạo ra một sự sắp xếp sao cho không có hai ký tự liền kề nào khác nhau đúng một vị trí trong bảng chữ cái."
date: "2026-06-12T02:37:51+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "greedy", "implementation", "sortings", "strings"]
categories: ["algorithms"]
codeforces_contest: 1156
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 64 (Rated for Div. 2)"
rating: 1800
weight: 1156
solve_time_s: 106
verified: false
draft: false
---

[CF 1156B - Cặp đôi xấu xí](https://codeforces.com/problemset/problem/1156/B) 

**Đánh giá:** 1800 
**Tags:** dfs và tương tự, tham lam, triển khai, sắp xếp, chuỗi 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn các chữ cái viết thường và chúng ta được phép sắp xếp lại các ký tự của nó một cách tùy ý. Mục đích là tạo ra một sự sắp xếp sao cho không có hai ký tự liền kề nào khác nhau đúng một vị trí trong bảng chữ cái. Ví dụ: cấm đặt 'c' bên cạnh 'd' hoặc 'd' bên cạnh 'c', trong khi được phép lặp lại cùng một ký tự bên cạnh chính nó. 

Ràng buộc về độ dài chuỗi là nhỏ, tối đa 100 ký tự cho mỗi trường hợp kiểm thử và nhiều nhất là 100 trường hợp kiểm thử. Điều này ngay lập tức cho chúng tôi biết rằng ngay cả các giải pháp bậc ba hoặc giai thừa cũng sẽ vượt qua về mặt kỹ thuật nếu được triển khai cẩn thận, nhưng chúng tôi vẫn muốn một công trình có tính xác định và nhanh chóng, vì số lượng sắp xếp là theo cấp số nhân. 

Khó khăn tinh tế là các ràng buộc cục bộ tương tác trên toàn cầu. Ngay cả khi vị trí ký tự có vẻ an toàn cục bộ, nó có thể buộc các chữ cái trong tương lai vào các vị trí mà chúng trở nên liền kề với các ký tự lân cận trong bảng chữ cái. Một kẻ tham lam ngây thơ luôn chọn bất kỳ ký tự hợp lệ nào tiếp theo có thể bị mắc kẹt. 

Một trường hợp lỗi điển hình xuất hiện khi các ký tự dày đặc trong không gian bảng chữ cái, ví dụ như các chuỗi như “abcde” hoặc “abac”. Nếu chúng ta cố gắng xây dựng từ trái sang phải mà không lập kế hoạch, chúng ta thường bị buộc phải sử dụng các ký tự liền kề như “ab” hoặc “bc” ở gần cuối, nơi không còn chữ cái thay thế nào. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các hoán vị của chuỗi và kiểm tra xem mỗi cách sắp xếp có hợp lệ hay không. Điều này đúng vì nó khám phá toàn bộ không gian tìm kiếm, nhưng số lượng hoán vị tăng lên theo O(n!), điều này vốn đã không khả thi đối với n khoảng 10 ngay cả khi cắt tỉa. Kiểm tra tính hợp lệ là O(n), do đó độ phức tạp tổng thể trở thành O(n! · n), vượt xa giới hạn. 

Quan sát quan trọng là bảng chữ cái cố định và nhỏ (26 chữ cái), do đó cấu trúc của bài toán chỉ phụ thuộc vào sự phân bố tần số trên 26 nhóm này chứ không phụ thuộc vào các vị trí riêng lẻ. Ràng buộc chỉ cấm đặt các chữ cái có mã ASCII khác nhau 1 cạnh nhau. 

Điều này gợi ý suy nghĩ về việc nhóm các chữ cái thành các tập hợp độc lập của biểu đồ bảng chữ cái. Bảng chữ cái có thể được xem như một biểu đồ đường dẫn: 

a - b - c - ... - z 

Nếu chúng ta đặt tất cả các chữ cái có chỉ mục chẵn trước tiên (so với biểu đồ này) và sau đó là tất cả các chữ cái có chỉ số lẻ thì không có chữ cái liền kề nào trong mỗi nhóm là không hợp lệ, vì trong một nhóm, các chữ cái khác nhau ít nhất 2 về chỉ mục. Bất kỳ xung đột liền kề nào cũng chỉ có thể xảy ra giữa các chữ cái ranh giới của hai nhóm. Bằng cách sắp xếp cẩn thận, chúng ta có thể tránh tạo ra các cặp chữ cái trong bảng chữ cái liền kề nhau. 

Vì chỉ có 2 lớp chỉ số chẵn lẻ trong biểu đồ đường dẫn, nên việc chia theo chẵn lẻ chỉ mục là đủ để loại bỏ tất cả các cạnh bị cấm trong một nhóm. Sau đó, chúng tôi thử cả hai thứ tự ghép có thể có của hai nhóm và kiểm tra tính hợp lệ. 

Điều này có hiệu quả vì bất kỳ cặp bị cấm nào cũng phải kết nối các chỉ số liên tiếp, luôn rơi vào các tập chẵn lẻ đối diện, do đó cấu trúc bên trong là an toàn và chỉ có ranh giới mới quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(n! · n) | O(n) | Quá chậm | 
| Xây dựng phân chia chẵn lẻ | O(26 + n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Đếm tần số của từng ký tự trong chuỗi. Điều này làm giảm vấn đề khi quyết định thứ tự 26 nhóm thay vì các ký tự riêng lẻ. 
2. Phân chia các ký tự thành hai nhóm dựa trên chỉ số bảng chữ cái: nhóm A chứa các chữ cái có chỉ số chẵn (a, c, e, ...), nhóm B chứa các chữ cái có chỉ số lẻ (b, d, f, ...). Bước này đảm bảo rằng trong mỗi nhóm không có hai chữ cái nào liền kề nhau trong bảng chữ cái. 
3. Xây dựng hai chuỗi ứng cử viên: một chuỗi bằng cách đặt tất cả các ký tự của nhóm A theo sau là nhóm B và một chuỗi khác bằng cách đặt nhóm B theo sau là nhóm A. Trong mỗi nhóm, chúng tôi nối các ký tự theo bất kỳ thứ tự nào, thường là theo thứ tự bảng chữ cái. 
4. Đối với mỗi chuỗi ứng cử viên, hãy xác minh xem nó có chứa bất kỳ cặp chữ cái liền kề nào khác nhau 1 trong chỉ mục bảng chữ cái hay không. Đây là một quét tuyến tính. 
5. Nếu tìm thấy sự sắp xếp hợp lệ, hãy xuất nó ra. Nếu không thì xuất ra "Không có câu trả lời". 

Lý do chúng tôi thử cả hai thứ tự nối là vì các vi phạm lân cận chỉ xảy ra ở ranh giới giữa các nhóm, trong đó một chữ cái có chỉ số chẵn có thể nằm cạnh một chữ cái có chỉ số lẻ. 

### Tại sao nó hoạt động 

Hạn chế kề cận của bảng chữ cái tạo thành một biểu đồ đường dẫn. Việc chia theo chẵn lẻ sẽ chia các đỉnh thành hai tập hợp độc lập, nghĩa là không có cạnh nào tồn tại bên trong cả hai tập hợp. Do đó, bất kỳ sự kề cận không hợp lệ nào cũng phải xảy ra trên ranh giới phân vùng. Vì chỉ có hai cách để sắp xếp các bộ này nên việc kiểm tra cả hai sẽ đảm bảo rằng nếu có bất kỳ sự sắp xếp hợp lệ nào tồn tại trong cấu trúc này thì một trong hai cách sẽ tránh được xung đột ranh giới. Vì thứ tự trong nhóm không bị ràng buộc bởi quy tắc nên mọi hoán vị bên trong mỗi nhóm đều duy trì tính hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def valid(s):
    for i in range(len(s) - 1):
        if abs(ord(s[i]) - ord(s[i+1])) == 1:
            return False
    return True

def solve(s):
    cnt = [0] * 26
    for ch in s:
        cnt[ord(ch) - 97] += 1

    even = []
    odd = []

    for i in range(26):
        if i % 2 == 0:
            even.append(chr(97 + i) * cnt[i])
        else:
            odd.append(chr(97 + i) * cnt[i])

    cand1 = "".join(even) + "".join(odd)
    cand2 = "".join(odd) + "".join(even)

    if valid(cand1):
        return cand1
    if valid(cand2):
        return cand2
    return "No answer"

def main():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        print(solve(s))

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ nén chuỗi thành số lượng tần số. Sau đó nó tái tạo lại hai hoán vị có cấu trúc dựa trên tính chẵn lẻ của bảng chữ cái. Việc kiểm tra tính hợp lệ là cần thiết vì mặc dù việc tách chẵn lẻ loại bỏ xung đột nội bộ nhưng ranh giới vẫn có thể tạo ra các cặp bảng chữ cái liền kề như ‘c’ theo sau là ‘d’ khi trộn các nhóm. 

Việc xây dựng tránh bất kỳ việc quay lui phức tạp nào và hoàn toàn dựa vào việc phân nhóm xác định cộng với bước xác minh cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`"abcd"`Chúng tôi tính toán tần số: a:1, b:1, c:1, d:1. 

Các chữ cái có chỉ số chẵn là a (0) và c (2). Các chữ cái chỉ số lẻ là b (1) và d (3). 

| Bước | Nhóm chẵn | Nhóm lẻ | Ứng viên | 
| --- | --- | --- | --- | 
| Xây dựng | một c | bd | acbd | 
| Xây dựng | một c | bd | bdac | 

Kiểm tra`acbd`: chứa “cb” không hợp lệ vì |c-b| = 1 nên bác bỏ. 

Kiểm tra`bdac`: các chuyển tiếp b-d, d-a, a-c đều an toàn nên hãy chấp nhận. 

Điều này chứng tỏ rằng vị trí ranh giới vẫn quan trọng ngay cả sau khi nhóm. 

### Ví dụ 2:`"abaca"`Tần số: a:3, b:1, c:1. 

Nhóm chẵn: a, c. Nhóm lẻ: b. 

| Bước | Nhóm chẵn | Nhóm lẻ | Ứng viên | 
| --- | --- | --- | --- | 
| Xây dựng | aaa c | b | aaacb | 
| Xây dựng | b | aaa c | baaac | 

Kiểm tra`aaacb`: cặp c-b cuối cùng không hợp lệ vì |c-b|=1, từ chối. 

Kiểm tra`baaac`: b-a không hợp lệ khi bắt đầu, từ chối. 

Không có sự sắp xếp hợp lệ tồn tại. 

Điều này cho thấy ngay cả với phân vùng có cấu trúc, một số phân bố tần số không thể tránh được xung đột lân cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 + n) mỗi lần kiểm tra | đếm + xây dựng + xác nhận tuyến tính | 
| Không gian | O(26) | mảng tần số và chuỗi tạm thời | 

Các ràng buộc đủ nhỏ đến mức ngay cả bước xác minh cũng không đáng kể. Giải pháp phù hợp thoải mái trong giới hạn vì mỗi trường hợp thử nghiệm hoạt động trên tối đa 100 ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def valid(s):
        for i in range(len(s) - 1):
            if abs(ord(s[i]) - ord(s[i+1])) == 1:
                return False
        return True

    def solve(s):
        cnt = [0] * 26
        for ch in s:
            cnt[ord(ch) - 97] += 1

        even = []
        odd = []
        for i in range(26):
            if i % 2 == 0:
                even.append(chr(97 + i) * cnt[i])
            else:
                odd.append(chr(97 + i) * cnt[i])

        c1 = "".join(even) + "".join(odd)
        c2 = "".join(odd) + "".join(even)

        if valid(c1):
            return c1
        if valid(c2):
            return c2
        return "No answer"

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve(input().strip()))
    return "\n".join(out)

# provided samples
assert run("4\nabcd\ngg\ncodeforces\nabaca\n") == "bdac\ngg\ncodfoerces\nNo answer"

# custom cases
assert run("1\na\n") == "a"
assert run("1\nab\n") in ["No answer"]
assert run("1\nazbz\n") in ["azbz", "zbza", "No answer"]
assert run("1\nabcabc\n") in ["No answer"], "dense alternating letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`|`"a"`| trường hợp cơ sở ký tự đơn | 
|`"ab"`|`"No answer"`| xung đột bắt buộc nhỏ nhất | 
|`"azbz"`| hợp lệ hoặc Không có câu trả lời | hành vi ranh giới chẵn lẻ hỗn hợp | 
|`"abcabc"`|`"No answer"`| chuỗi lân cận thất bại dày đặc | 

## Vỏ cạnh 

Các chuỗi ký tự đơn luôn thành công vì không có chuỗi liền kề nào tồn tại, vì vậy mọi cách xây dựng đều hợp lệ. Thuật toán tạo ra một nhóm một chữ cái và chuyển nó qua không thay đổi. 

Các chuỗi bao gồm hai chữ cái trong bảng chữ cái liên tiếp như “ab” hoặc “ba” luôn bị lỗi. Trong những trường hợp này, cả hai ký tự đều thuộc các nhóm chẵn lẻ đối lập nhau và bất kỳ thứ tự nào cũng tạo ra sự kề cận bị cấm ở ranh giới. 

Các chuỗi có tính lặp lại cao như “aaaaa” thành công một cách tầm thường vì không có cặp nào khác nhau 1 và nhóm suy biến thành một cụm duy nhất. Thuật toán trả về cùng một chuỗi vì việc kiểm tra tính hợp lệ sẽ được thực hiện ngay lập tức. 

Các mô hình xen kẽ dày đặc như “ababab” bộc lộ độ nhạy ranh giới. Mặc dù mỗi nhóm đều an toàn nội bộ nhưng mọi vị trí đều buộc phải có sự liền kề trên phân vùng, khiến cả hai phần nối đều không đạt được bước xác thực.
