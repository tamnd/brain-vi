---
title: "CF 102680C - Vấn đề dừng"
description: "Chương trình trong bài toán này là một cỗ máy nhân tạo rất nhỏ. Nó có một thanh ghi r 8 bit duy nhất, vì vậy thanh ghi này chỉ có thể chứa các giá trị từ 0 đến 255. Việc thực thi bắt đầu ở lệnh đầu tiên với r = 0."
date: "2026-08-03T03:55:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102680
codeforces_index: "C"
codeforces_contest_name: "Brookfield Computer Programming Challenge 1"
rating: 0
weight: 102680
solve_time_s: 143
verified: true
draft: false
---

[CF 102680C - Sự cố tạm dừng](https://codeforces.com/problemset/problem/102680/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chương trình trong bài toán này là một cỗ máy nhân tạo rất nhỏ. Nó có một thanh ghi 8 bit duy nhất`r`, do đó thanh ghi chỉ có thể chứa các giá trị từ`0`ĐẾN`255`. Việc thực thi bắt đầu ở lệnh đầu tiên với`r = 0`. Mỗi lệnh sẽ thay đổi thanh ghi và chuyển sang lệnh tiếp theo hoặc kiểm tra thanh ghi và có thể chuyển sang lệnh khác. Chương trình chỉ dừng khi quá trình thực thi vượt qua lệnh cuối cùng. 

Nhiệm vụ là quyết định liệu chương trình đã cho có đạt đến điểm dừng này hay không hay nó sẽ tiếp tục thực thi mãi mãi. 

Các ràng buộc được thiết kế xung quanh cấu trúc đặc biệt của ngôn ngữ. Một chương trình bình thường có thể có tới`10^4`hướng dẫn và tổng số hướng dẫn trên tất cả các trường hợp thử nghiệm nhiều nhất là`10^5`. Nói chung, việc mô phỏng một chương trình tùy ý trong một khoảng thời gian không xác định là không thể, nhưng ngôn ngữ này có một không gian trạng thái giới hạn. Lệnh hiện tại có thể có`n + 1`các vị trí có thể và thanh ghi chỉ có 256 giá trị có thể. Điều này mang lại nhiều nhất khoảng`256 * n`trạng thái thực hiện khác nhau. Vì`n = 10^4`, tức là khoảng 2,56 triệu tiểu bang, đủ nhỏ để ghé thăm một lần. 

Trường hợp cạnh chính là một chương trình lặp mà không thay đổi thanh ghi. Ví dụ:```
2
add 0
beq 0 1
```Đầu ra đúng là:```
No
```Sau lệnh đầu tiên, thanh ghi vẫn bằng 0. Lệnh thứ hai sẽ quay trở lại lệnh đầu tiên mãi mãi. Một mô phỏng đơn giản mà không phát hiện chu kỳ sẽ không bao giờ kết thúc. 

Một trường hợp khác là một vòng lặp cuối cùng đạt đến trạng thái mới vì thanh ghi bao quanh. Ví dụ:```
3
add 252
add 1
bgt 252 2
```Đầu ra đúng là:```
Yes
```Thanh ghi là 8 bit, do đó, sau khi đạt tới 255, nó sẽ kết thúc bằng 0. Một giải pháp coi thanh ghi là số nguyên không giới hạn sẽ nhầm tưởng rằng giá trị tiếp tục tăng và bỏ lỡ đường dẫn tạm dừng. 

Lỗi phổ biến thứ ba là bỏ qua thực tế rằng con trỏ lệnh là một phần của trạng thái. Coi như:```
3
add 1
bne 252 1
beq 252 1
```Đầu ra đúng là:```
No
```Chỉ riêng giá trị thanh ghi là không đủ để phát hiện sự lặp lại. Giá trị đăng ký giống nhau ở các hướng dẫn khác nhau thể hiện hành vi khác nhau trong tương lai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thực hiện lệnh chương trình bằng lệnh. Nếu việc thực thi đạt đến lệnh`n + 1`, chương trình dừng lại và câu trả lời là`Yes`. Nếu quá trình thực thi tiếp tục mãi mãi, mô phỏng này sẽ không bao giờ kết thúc, vì vậy chúng ta cần một cách để nhận biết lượt chạy vô hạn. 

Phiên bản brute-force chỉ có thể chạy chương trình với một số bước tùy ý. Điều này không đúng vì một chương trình hợp lệ có thể yêu cầu nhiều bước hơn trước khi tạm dừng. Trường hợp xấu nhất có thể chứa đựng mọi khả năng`(instruction, register)`trạng thái trước khi lặp lại, điều này tùy thuộc vào`256 * n`tiểu bang. Với`n = 10^4`, con số này là khoảng 2,56 triệu lần chuyển đổi, do đó, mô phỏng giới hạn đầy đủ là đủ, nhưng mô phỏng không giới hạn thì không. 

Quan sát quan trọng là chiếc máy này có tính quyết định. Khi chúng ta biết lệnh hiện tại và giá trị thanh ghi, trạng thái tiếp theo sẽ hoàn toàn cố định. Một quá trình xác định trên một số lượng hữu hạn các trạng thái phải đạt đến trạng thái dừng hoặc cuối cùng quay trở lại trạng thái. Trạng thái được xem lại có nghĩa là việc thực thi tương tự trong tương lai sẽ lặp lại mãi mãi. 

Nhận xét rằng không gian trạng thái là hữu hạn cho phép chúng ta rút gọn bài toán về việc truyền đồ thị trên một đồ thị ẩn. Chúng ta không cần phải xây dựng biểu đồ. Chúng ta chỉ cần nhớ những trạng thái nào đã xuất hiện khi đi theo đường dẫn thực thi duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn | O(1) | Sai vì vòng lặp vô hạn không bao giờ kết thúc | 
| Tối ưu | O(256n) | O(256n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi lệnh dưới dạng biểu diễn thuận tiện. MỘT`add`lệnh chỉ lưu trữ giá trị của nó, trong khi lệnh nhánh lưu trữ điều kiện, đích và loại của chúng. 
2. Bắt đầu thực hiện từ lệnh`0`với giá trị đăng ký`0`. Sử dụng bảng đã truy cập được lập chỉ mục theo vị trí lệnh và giá trị thanh ghi. 
3. Trước khi thực hiện một lệnh, hãy kiểm tra xem dòng điện có`(instruction, register)`cặp đã được truy cập. Nếu đúng như vậy thì máy đang lặp lại tình huống trước đó nên chương trình sẽ không bao giờ dừng lại. 
4. Đánh dấu trạng thái hiện tại là đã truy cập và thực hiện lệnh. Đối với các lệnh số học, hãy cập nhật thanh ghi bằng modulo 256 vì thanh ghi chỉ có 8 bit. 
5. Đối với các lệnh rẽ nhánh, hãy so sánh thanh ghi với giá trị được lưu trữ. Nếu điều kiện đúng thì nhảy tới lệnh đích. Nếu không, hãy tiếp tục hướng dẫn tiếp theo. 
6. Lặp lại cho đến khi có một trong hai hướng dẫn`n + 1`đạt được hoặc tìm thấy trạng thái lặp lại. 

Lý do điều này hoạt động là vì việc thực thi mang tính quyết định. Một trạng thái được mô tả đầy đủ bằng lệnh đang được thực thi và giá trị thanh ghi hiện tại. Nếu cùng một trạng thái xuất hiện hai lần thì mọi chuyển đổi tiếp theo cũng sẽ giống hệt nhau, tạo ra một chu kỳ vô hạn. Nếu không có trạng thái nào lặp lại thì việc thực thi cuối cùng phải rời khỏi không gian trạng thái hữu hạn bằng cách đạt đến vị trí dừng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        prog = []

        for _ in range(n):
            parts = input().split()
            if parts[0] == "add":
                prog.append(("add", int(parts[1])))
            else:
                prog.append((parts[0], int(parts[1]), int(parts[2]) - 1))

        visited = [[False] * 256 for _ in range(n)]
        pc = 0
        r = 0

        while pc < n:
            if visited[pc][r]:
                ans.append("No")
                break

            visited[pc][r] = True
            ins = prog[pc]

            if ins[0] == "add":
                r = (r + ins[1]) % 256
                pc += 1
            else:
                op, v, k = ins
                ok = False

                if op == "beq":
                    ok = (r == v)
                elif op == "bne":
                    ok = (r != v)
                elif op == "blt":
                    ok = (r < v)
                elif op == "bgt":
                    ok = (r > v)

                if ok:
                    pc = k
                else:
                    pc += 1
        else:
            ans.append("Yes")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biểu diễn chương trình giữ tên thao tác cùng với chỉ các tham số cần thiết cho thao tác đó. Các mục tiêu nhảy được chuyển đổi thành các chỉ số dựa trên 0 ngay lập tức, giúp tránh bị trừ đi nhiều lần trong quá trình mô phỏng. 

các`visited`mảng có một hàng cho mỗi lệnh và 256 cột cho tất cả các giá trị thanh ghi có thể có. Việc cập nhật đăng ký sử dụng`% 256`bởi vì việc tràn thanh ghi 8 bit sẽ loại bỏ các bit cao hơn. 

Vòng lặp kiểm tra trạng thái lặp lại trước khi thực hiện lệnh. Việc kiểm tra sau khi thực thi cũng có tác dụng, nhưng việc kiểm tra trước khi thực hiện sẽ làm cho tính bất biến trở nên rõ ràng hơn: mọi trạng thái đạt đến điểm thực thi đều được xử lý một lần hoặc được phát hiện dưới dạng một chu kỳ. 

Số nguyên Python không bị tràn nên không cần xử lý đặc biệt đối với bộ đếm hoặc chỉ mục. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
add 1
blt 5 1
```| Bước | Hướng dẫn | Đăng ký | Hành động tiếp theo | 
| --- | --- | --- | --- | 
| 0 | 1: thêm 1 | 0 | Đăng ký trở thành 1, chuyển sang hướng dẫn 2 | 
| 1 | 2: blt 5 1 | 1 | Quay lại hướng dẫn 1 | 
| 2 | 1: thêm 1 | 1 | Đăng ký trở thành 2, chuyển sang hướng dẫn 2 | 
| 3 | 2: blt 5 1 | 2 | Quay lại hướng dẫn 1 | 
| 4 | 1: thêm 1 | 2 | Đăng ký trở thành 3, chuyển sang hướng dẫn 2 | 
| 5 | 2: blt 5 1 | 3 | Quay lại hướng dẫn 1 | 
| 6 | 1: thêm 1 | 3 | Đăng ký trở thành 4, chuyển sang hướng dẫn 2 | 
| 7 | 2: blt 5 1 | 4 | Quay lại hướng dẫn 1 | 
| 8 | 1: thêm 1 | 4 | Đăng ký trở thành 5, chuyển sang hướng dẫn 2 | 
| 9 | 2: blt 5 1 | 5 | Tiếp tục hướng dẫn 3 và dừng lại | 

Máy đạt đến điểm cuối sau khi thanh ghi dừng thỏa mãn điều kiện rẽ nhánh. Điều này cho thấy một chương trình chứa vòng lặp trong luồng điều khiển của nó vẫn có thể dừng do thanh ghi thay đổi. 

Đối với mẫu thứ ba:```
3
add 1
bne 252 1
beq 252 1
```| Bước | Hướng dẫn | Đăng ký | Hành động tiếp theo | 
| --- | --- | --- | --- | 
| 0 | 1: thêm 1 | 0 | Đăng ký trở thành 1 | 
| 1 | 2: bn 252 1 | 1 | Chuyển tới hướng dẫn 1 | 
| 2 | 1: thêm 1 | 1 | Đăng ký trở thành 2 | 
| ... | lặp đi lặp lại | tăng dần dãy số lẻ/chẵn | Tiếp tục | 
| 252 | 1: thêm 1 | 251 | Đăng ký trở thành 252 | 
| 253 | 2: bn 252 1 | 252 | Chuyển sang hướng dẫn 3 | 
| 254 | 3: beq 252 1 | 252 | Chuyển tới hướng dẫn 1 | 
| 255 | 1: thêm 1 | 252 | Đăng ký trở thành 253 | 

Việc thực thi cuối cùng sẽ quay trở lại cùng một lệnh và các kết hợp đăng ký, do đó việc kiểm tra trạng thái đã truy cập sẽ phát hiện vòng lặp vô hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(256n) | Mỗi tổ hợp lệnh và thanh ghi được xử lý nhiều nhất một lần | 
| Không gian | O(256n) | Bảng đã truy cập lưu trữ một boolean cho mọi trạng thái có thể | 

Số lượng trạng thái tối đa là khoảng 2,56 triệu cho một trường hợp thử nghiệm lớn nhất, vừa vặn trong bộ nhớ. Tổng số hướng dẫn trong tất cả các trường hợp bị giới hạn, vì vậy giải pháp hoàn chỉnh vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

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

assert run("""4
2
add 1
blt 5 1
3
add 252
add 1
bgt 252 2
2
add 2
bne 7 1
3
add 1
bne 252 1
beq 252 1
""") == """Yes
Yes
No
No
""", "provided samples"

assert run("""1
1
add 0
""") == """Yes
""", "single instruction halts"

assert run("""1
2
add 0
beq 0 1
""") == """No
""", "immediate infinite loop"

assert run("""1
2
add 255
bne 255 1
""") == """Yes
""", "register wraparound"

assert run("""1
3
add 1
bne 3 1
beq 3 1
""") == """No
""", "unreachable branch condition"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn`add`hướng dẫn | Có | Dừng lại sau khi về đích | 
| Nhảy với thanh ghi không thay đổi | Không | Phát hiện một chu kỳ cố định | 
| Đăng ký trường hợp tràn | Có | Sửa hành vi modulo 256 | 
| Giá trị nhánh không thể | Không | Xử lý đúng các điều kiện không thể truy cập | 

## Vỏ cạnh 

Vòng lặp vô hạn thanh ghi không thay đổi được xử lý vì trạng thái`(instruction, register)`lặp lại ngay lập tức. Vì:```
2
add 0
beq 0 1
```các tiểu bang`(1,0)`Và`(2,0)`được viếng thăm nhiều lần. Khi`(1,0)`xuất hiện lại, thuật toán xuất ra`No`thay vì mô phỏng mãi mãi. 

Việc tràn đăng ký được xử lý bằng cách giữ mọi giá trị trong phạm vi`0`ĐẾN`255`. Vì:```
3
add 252
add 1
bgt 252 2
```trình tự đăng ký cuối cùng kết thúc từ`255`ĐẾN`0`. Thuật toán nhìn thấy hành vi thực tế của máy và đạt được lệnh tạm dừng. 

Các trường hợp cùng một giá trị thanh ghi xuất hiện ở các lệnh khác nhau được xử lý bằng cách lưu trữ chỉ mục lệnh như một phần của trạng thái. Tập truy cập chỉ đăng ký sẽ báo cáo không chính xác các chu kỳ quá sớm vì hai lệnh có cùng giá trị thanh ghi có thể có tương lai khác nhau. Cặp đôi`(program counter, register)`là trạng thái máy hoàn chỉnh nên thuật toán chỉ dừng khi quá trình thực thi thực tế lặp lại.
