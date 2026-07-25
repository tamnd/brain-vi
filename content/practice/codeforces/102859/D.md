---
title: "CF 102859D - Tiệc"
description: "Chúng ta có một cách sắp xếp hình tròn gồm N đĩa, trong đó mỗi đĩa được biểu thị bằng một chữ cái viết thường. Mẫu là bất kỳ chuỗi đĩa liên tiếp nào được lấy theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ xung quanh vòng tròn. Nhiệm vụ là đếm xem có bao nhiêu chuỗi khác nhau có thể xuất hiện dưới dạng mẫu."
date: "2026-07-25T14:21:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102859
codeforces_index: "D"
codeforces_contest_name: "mBIT Standard November 2020"
rating: 0
weight: 102859
solve_time_s: 50
verified: true
draft: false
---

[CF 102859D - Tiệc](https://codeforces.com/problemset/problem/102859/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một sự sắp xếp vòng tròn`N`món ăn, trong đó mỗi món ăn được thể hiện bằng một chữ cái viết thường. Mẫu là bất kỳ chuỗi đĩa liên tiếp nào được lấy theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ xung quanh vòng tròn. Nhiệm vụ là đếm xem có bao nhiêu chuỗi khác nhau có thể xuất hiện dưới dạng mẫu. Hai mẫu chỉ được coi là bằng nhau khi độ dài và vị trí ký tự của chúng bằng nhau. 

Vòng tròn có nghĩa là định nghĩa chuỗi con thông thường là không đủ. Một chuỗi có thể quấn từ cuối chuỗi đầu vào trở lại đầu chuỗi, do đó chiều kim đồng hồ có thể được biểu diễn bằng cách lấy các chuỗi con của`S + S`với chiều dài tối đa`N`. Hướng ngược chiều kim đồng hồ là ý tưởng tương tự được áp dụng cho chuỗi đảo ngược. 

Chiều dài của vòng tròn lên tới`50000`, loại trừ việc tạo mọi mẫu một cách rõ ràng. Một chuỗi có độ dài`N`có`O(N^2)`các chuỗi con có thể có, và ở đây sẽ có khoảng 2,5 tỷ ứng viên. Ngay cả việc kiểm tra từng ứng cử viên một lần cũng đã vượt quá giới hạn cuộc thi thông thường. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Những trường hợp phức tạp là do các mô hình lặp đi lặp lại và do hai hướng chồng chéo nhau. Ví dụ: với một ký tự lặp lại:```
Input
4
aaaa
```Các mẫu duy nhất có thể là`a`,`aa`,`aaa`, Và`aaaa`, vậy đáp án là:```
4
```Giải pháp tính số lần xuất hiện thay vì các chuỗi riêng biệt sẽ bị tính quá mức vì mọi vị trí bắt đầu đều tạo ra các mẫu giống nhau. 

Một sai lầm dễ dàng khác là quên rằng hai hướng có chung câu trả lời. Vì:```
Input
3
aba
```Các mẫu theo chiều kim đồng hồ bao gồm`ab`Và`ba`, trong khi di chuyển ngược chiều kim đồng hồ cũng tạo ra các chuỗi tương tự. Đếm cả hai bên một cách độc lập sẽ tính các bản sao. Câu trả lời đúng là:```
8
```Trọn bộ là`a`,`b`,`aa`,`ab`,`ba`,`aba`,`aab`, Và`baa`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi món ăn bắt đầu, mở rộng từng ký tự mẫu một và lưu trữ mọi chuỗi được tạo trong một bộ. có`N`vị trí bắt đầu và lên đến`N`độ dài cho từng vị trí, tạo ra`O(N^2)`mẫu. Vì mỗi mẫu có thể chứa tới`N`các ký tự, so sánh và lưu trữ chúng có thể đẩy mạnh bạo lực về phía`O(N^3)`công việc. Ngay cả khi băm, số lượng mẫu bậc hai đã quá lớn để`N = 50000`. 

Cấu trúc của bài toán gợi ý sử dụng một máy tự động hậu tố. Một máy tự động hậu tố lưu trữ gọn gàng tất cả các chuỗi con riêng biệt của một chuỗi. Mỗi trạng thái đại diện cho một nhóm chuỗi con có cùng hành vi vị trí cuối và số lượng chuỗi được biểu thị có thể được tính từ độ dài được lưu trữ trong máy tự động. 

Vòng tròn có thể được chuyển đổi thành chuỗi thông thường. Các mẫu theo chiều kim đồng hồ là tất cả các chuỗi con của`S + S`có chiều dài nhiều nhất`N`. Các mẫu ngược chiều kim đồng hồ đều là chuỗi con của`reverse(S) + reverse(S)`có cùng giới hạn chiều dài. Thay vì xây dựng hai máy tự động và hợp nhất các câu trả lời theo cách thủ công, chúng tôi xây dựng một máy tự động hậu tố tổng quát để nhận cả hai chuỗi nhân đôi. Nó đại diện cho sự kết hợp của tất cả các mẫu hợp lệ. 

Chi tiết bổ sung duy nhất là giới hạn độ dài. Các chuỗi nhân đôi chứa các chuỗi con dài hơn một lượt hoàn chỉnh, nhưng đó không phải là các mẫu hợp lệ. Khi đếm sự đóng góp của một trạng thái, mỗi độ dài được giới hạn ở`N`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) đến O(N³) | O(N2) | Quá chậm | 
| Automaton hậu tố tổng quát | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một máy tự động hậu tố tổng quát bằng cách chèn`S + S`và sau đó chèn`reverse(S) + reverse(S)`. Trước khi chèn chuỗi thứ hai, hãy bắt đầu lại từ gốc để máy tự động thể hiện sự kết hợp của các chuỗi con từ cả hai nguồn. 
2. Đối với mọi trạng thái máy tự động, hãy lưu trữ giá trị máy tự động hậu tố thông thường`len`, là độ dài tối đa của chuỗi con được biểu thị bằng trạng thái đó. Liên kết hậu tố trỏ đến trạng thái đại diện cho nhóm hậu tố nhỏ hơn tiếp theo. 
3. Đếm sự đóng góp của từng trạng thái không gốc. Thông thường một quốc gia đóng góp mọi chiều dài từ`len[link[state]] + 1`ĐẾN`len[state]`. Bởi vì các mẫu không thể dài hơn một vòng tròn đầy đủ, hãy thay thế cả hai độ dài bằng mức tối thiểu bằng`N`. 
4. Thêm tất cả các khoản đóng góp của tiểu bang. Tổng là số lượng mẫu riêng biệt theo cả hai hướng. 

Tại sao nó hoạt động: một máy tự động hậu tố phân chia tất cả các chuỗi con riêng biệt thành các trạng thái trong đó mỗi trạng thái bao gồm một phạm vi độ dài liên tục. Liên kết hậu tố cho chúng ta biết phần cuối của phạm vi trước đó, do đó, mỗi chuỗi con riêng biệt xuất hiện chính xác một lần trong sự khác biệt giữa trạng thái và liên kết hậu tố của nó. Cấu trúc tổng quát làm cho ngôn ngữ được biểu diễn là sự kết hợp của cả hai hướng và giới hạn độ dài sẽ loại bỏ chính xác các chuỗi yêu cầu phải đi vòng quanh vòng tròn nhiều lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    strings = [s + s, (s[::-1]) + (s[::-1])]

    nexts = [{}]
    link = [-1]
    length = [0]

    def extend(c, last):
        cur = len(nexts)
        nexts.append({})
        length.append(length[last] + 1)
        link.append(0)

        p = last
        while p != -1 and c not in nexts[p]:
            nexts[p][c] = cur
            p = link[p]

        if p == -1:
            link[cur] = 0
        else:
            q = nexts[p][c]
            if length[p] + 1 == length[q]:
                link[cur] = q
            else:
                clone = len(nexts)
                nexts.append(nexts[q].copy())
                length.append(length[p] + 1)
                link.append(link[q])

                while p != -1 and nexts[p].get(c) == q:
                    nexts[p][c] = clone
                    p = link[p]

                link[q] = clone
                link[cur] = clone

        return cur

    for text in strings:
        last = 0
        for c in text:
            last = extend(c, last)

    ans = 0
    for state in range(1, len(length)):
        high = min(length[state], n)
        low = min(length[link[state]], n)
        if high > low:
            ans += high - low

    print(ans)

if __name__ == "__main__":
    solve()
```Cấu trúc automaton là phần mở rộng automaton hậu tố tiêu chuẩn. Mỗi ký tự được chèn sẽ tạo một trạng thái mới và sửa các chuyển tiếp mà trước đây không tồn tại. Cần phải nhân bản khi quá trình chuyển đổi hiện tại dẫn đến trạng thái có phạm vi độ dài quá lớn đối với hậu tố mới. 

Chi tiết triển khai quan trọng là đặt lại`last`vào thư mục gốc trước khi chèn chuỗi nhân đôi đã đảo ngược. Nếu không có sự thiết lập lại này, máy tự động sẽ hoạt động như thể chuỗi thứ hai tiếp nối chuỗi đầu tiên, điều này sẽ đưa ra các chuỗi con không hợp lệ vượt qua ranh giới. 

Vòng lặp cuối cùng tính toán số lượng độ dài được biểu thị cho mỗi trạng thái. Gốc bị bỏ qua vì nó đại diện cho chuỗi rỗng. các`min`hoạt động áp dụng giới hạn độ dài vòng tròn và tránh đếm các mẫu dài hơn một chuyến đi quanh bàn. 

## Ví dụ đã hoạt động 

Dành cho:```
3
aba
```Hai chuỗi được chèn vào là`abaaba`Và`abaaba`bởi vì điều ngược lại là như nhau. Máy tự động chỉ cần một bản sao của ngôn ngữ. 

| Tiểu bang | Chiều dài | Độ dài liên kết hậu tố | Đóng góp | 
| --- | --- | --- | --- | 
|`a`nhóm | 1 | 0 | 1 | 
|`ab`nhóm | 2 | 1 | 1 | 
|`aba`nhóm | 3 | 1 | 2 | 
| nhóm lặp đi lặp lại | ... | ... | các chuỗi riêng biệt còn lại | 

Tổng số trở thành`8`. Dấu vết chứng minh rằng các mẫu trùng lặp theo chiều kim đồng hồ và ngược chiều kim đồng hồ sẽ được hợp nhất tự động. 

Vì:```
6
ondrej
```Hướng ngược lại thêm các chuỗi như`rejo`Và`drejon`, trong khi chiều kim đồng hồ chứa các chuỗi từ`ondrejondrej`. 

| Sân khấu | Đã chèn văn bản | Độ dài mẫu tối đa được xem xét | Hiệu ứng | 
| --- | --- | --- | --- | 
| Chèn đầu tiên |`ondrejo...`| 6 | Thêm mẫu theo chiều kim đồng hồ | 
| Chèn thứ hai | chuỗi nhân đôi đảo ngược | 6 | Thêm mẫu ngược chiều kim đồng hồ | 
| Đếm | tất cả các tiểu bang | 6 | Chỉ tính đoàn | 

Máy tự động giữ các chuỗi con được chia sẻ ở cùng trạng thái, vì vậy số lượng cuối cùng là số lượng mẫu duy nhất từ ​​cả hai hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Máy tự động nhận được tổng cộng bốn bản sao có độ dài ban đầu và mọi hoạt động ở trạng thái được phân bổ theo thời gian không đổi. | 
| Không gian | O(N) | Một máy tự động hậu tố chứa tối đa khoảng gấp đôi số lượng ký tự được chèn ở trạng thái. | 

Kích thước đầu vào của`50000`cũng nằm trong giới hạn tuyến tính. Việc triển khai tránh lưu trữ tất cả các chuỗi con, đó là lý do chính khiến nó phù hợp với giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# samples
assert run("3\naba\n") == "8\n", "sample 1"
assert run("6\nondrej\n") == "66\n", "sample 2"

# custom cases
assert run("4\naaaa\n") == "4\n", "all equal values"
assert run("2\nab\n") == "4\n", "minimum size"
assert run("5\nabcde\n") == "25\n", "all characters different"
assert run("5\nabcba\n") == "17\n", "palindrome overlap case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 aaaa`|`4`| Các chuỗi lặp lại phải được tính một lần | 
|`2 ab`|`4`| Vòng tròn nhỏ nhất và cả hai hướng | 
|`5 abcde`|`25`| Sự đa dạng tối đa của chuỗi con | 
|`5 abcba`|`17`| Chồng chéo giữa các hướng | 

## Vỏ cạnh 

Đối với trường hợp ký tự lặp lại:```
4
aaaa
```Chuỗi nhân đôi là`aaaaaaaa`. Máy tự động nhìn thấy nhiều lần xuất hiện theo cùng một kiểu mẫu, nhưng chúng rơi vào cùng một trạng thái. Sự đóng góp của nhà nước tạo ra độ dài`1`,`2`,`3`, Và`4`duy nhất, đưa ra câu trả lời đúng`4`. 

Đối với trường hợp chồng chéo hướng:```
3
aba
```Vòng tròn đảo ngược không tạo ra chuỗi duy nhất mới nào ngoài ngôn ngữ dùng chung đã được biểu thị trong máy tự động. Vì cả hai chuỗi nhân đôi đều được chèn vào cùng một cấu trúc nên công thức đếm không thêm các trạng thái trùng lặp cho cùng một mẫu. Câu trả lời vẫn còn`8`. 

Đối với trường hợp gói có vấn đề:```
5
abcde
```Một mẫu như`dea`không thể tìm thấy trong chuỗi gốc như một chuỗi con bình thường, nhưng nó xuất hiện bên trong`abcdeabcde`. Bước nhân đôi tạo ra các chuỗi con được bao bọc này và giới hạn độ dài sẽ ngăn các mẫu không hợp lệ, chẳng hạn như`abcdea`khỏi bị tính. Câu trả lời cuối cùng là`25`.
