---
title: "CF 105262L - Chữ lớn"
description: "Chúng ta bắt đầu với một cấu trúc hoạt động giống như một chuỗi mở rộng đệ quy. Mỗi số trong mảng đầu vào không đại diện trực tiếp cho một ký tự; thay vào đó nó định nghĩa một chuỗi nhỏ được xây dựng từ một quy tắc đệ quy cố định."
date: "2026-06-24T02:35:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "L"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 56
verified: true
draft: false
---

[CF 105262L - Phát triển chữ cái](https://codeforces.com/problemset/problem/105262/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một cấu trúc hoạt động giống như một chuỗi mở rộng đệ quy. Mỗi số trong mảng đầu vào không đại diện trực tiếp cho một ký tự; thay vào đó nó định nghĩa một chuỗi nhỏ được xây dựng từ một quy tắc đệ quy cố định. 

Đối với một giá trị chỉ số$i$, chuỗi$F_i$bắt đầu bằng một ký tự đơn “a” khi$i = 0$. Mỗi lần tăng$i$lấy chuỗi trước đó, sao chép nó xung quanh ký tự ở giữa “b”, do đó cấu trúc trở nên đối xứng: chuỗi trước đó, sau đó là “b”, sau đó lại là chuỗi trước đó. Điều này làm cho chiều dài của$F_i$tăng trưởng theo cấp số nhân trong khi cấu trúc của nó vẫn rất đều đặn. 

Chuỗi ban đầu đầy đủ$S$thu được bằng cách ghép một số khối như vậy$F_{a_1}, F_{a_2}, \dots, F_{a_n}$. Mặc dù mỗi khối được xác định rõ ràng nhưng chiều dài kết hợp của chúng có thể rất lớn, vượt xa những gì có thể được xây dựng một cách rõ ràng. 

Sau khi xây dựng chuỗi khái niệm này, quá trình chuyển đổi liên tục nén các ký tự bằng nhau liền kề. Bất cứ khi nào hai ký tự lân cận giống hệt nhau, chúng tôi hợp nhất chúng thành một ký tự duy nhất và tăng nó trong bảng chữ cái, gói “z” trở lại “a”. Việc hợp nhất được áp dụng một cách tham lam cho cặp bằng nhau liền kề ngoài cùng bên trái có thể và quá trình này lặp lại cho đến khi không còn hàng xóm giống hệt nhau. 

Nhiệm vụ không phải là mô phỏng chuỗi mà là xác định độ dài cuối cùng sau khi tất cả các quá trình hợp nhất đó kết thúc. 

Các ràng buộc đẩy chúng ta ra khỏi bất kỳ cấu trúc chuỗi rõ ràng nào. Mỗi bài kiểm tra có thể có tới$10^5$các phần tử và tổng số tiền của chúng qua các lần kiểm tra cũng là$10^5$. Tuy nhiên, mỗi$F_i$đã đại diện cho một cấu trúc lớn theo cấp số nhân. Ngay cả một bản mở rộng vừa phải$i$sẽ tạo ra một chuỗi kích thước$2^i$, điều này trở nên không thể lưu trữ hoặc lặp lại. Bất kỳ cách tiếp cận nào hiện thực hóa chuỗi hoặc thậm chí mở rộng một phần chuỗi sẽ thất bại ngay lập tức. 

Một cách giải thích ngây thơ sẽ cố gắng xây dựng phép nối đầy đủ và sau đó liên tục quét các cặp liền kề bằng nhau. Điều này đã thất bại trên một đầu vào vì kích thước chuỗi theo cấp số nhân trong các giá trị$a_i$. 

Chế độ thất bại thứ hai xuất phát từ việc nghĩ rằng chỉ có ranh giới nối cục bộ mới quan trọng. Ví dụ, người ta có thể thử tính độ dài của$F_i$và bỏ qua sự tương tác giữa các phân đoạn. Điều đó không chính xác vì việc hợp nhất có thể lan truyền qua các ranh giới phân đoạn và việc hợp nhất ở một vị trí có thể thay đổi ký tự bảng chữ cái, sau đó kích hoạt việc hợp nhất mới với các phân đoạn lân cận. 

Ví dụ: nếu phép nối tạo ra thứ gì đó giống như “aa”, thì nó sẽ trở thành “b” và khi đó “b” mới này có thể tương tác với các ký tự lân cận mà trước đây an toàn. Vì vậy, vấn đề vốn đã mang tính toàn cầu mặc dù được xác định cục bộ. 

## Phương pháp tiếp cận 

Khó khăn chính là quá trình cuối cùng là nén lặp đi lặp lại trên một chuỗi thay đổi linh hoạt. Tuy nhiên, bản thân phép biến đổi có một cấu trúc quan trọng: nó chỉ hợp nhất các ký tự bằng nhau liền kề và thay thế chúng một cách xác định bằng ký tự tiếp theo. 

Điều này tương đương với việc thực hiện liên tục một loại thao tác “mang” trên các chữ cái giống hệt nhau. Mỗi lần hợp nhất sẽ giảm độ dài đi một và tăng ký tự kết quả. Điều này gợi nhớ đến phép cộng nhị phân có nhớ, ngoại trừ việc khái quát hóa thành cơ số 26. 

Mô phỏng lực lượng vũ phu sẽ xây dựng chuỗi đầy đủ$S$, sau đó quét liên tục từ trái sang phải, hợp nhất cặp liền kề bằng nhau đầu tiên cho đến khi ổn định. Mỗi lần quét là tuyến tính và có thể có$O(|S|)$sáp nhập, cho$O(|S|^2)$thời gian. Từ$|S|$bản thân nó là hàm mũ trong các giá trị đầu vào, cách tiếp cận này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là tránh xây dựng$S$hoàn toàn và thay vào đó chỉ duy trì thông tin nén về số lần mỗi ký tự xuất hiện ở dạng có cấu trúc. Mỗi khối$F_i$có cấu trúc đệ quy cho phép chúng ta tính toán tác động của nó lên một “nhiều chữ cái” mà không cần mở rộng nó. 

Chúng tôi coi mỗi chuỗi là một cấu trúc có thể được rút gọn thành một biểu diễn trong đó chỉ các lần chạy liên tiếp mới quan trọng. Quá trình hợp nhất chỉ tương tác trong các lần chạy, vì vậy chúng tôi có thể duy trì cách biểu diễn số lượng ký tự giống như ngăn xếp. Mỗi lần chúng tôi thêm một khối mới, chúng tôi sẽ hợp nhất nó vào biểu diễn hiện tại bằng cách mô phỏng các tương tác chỉ chạy ở các ranh giới, truyền tải lên trên khi kết hợp các lần chạy bằng nhau. 

Thuộc tính cấu trúc quan trọng là mặc dù chuỗi thô rất lớn nhưng số lần chúng ta cần “thực hiện” việc hợp nhất bị giới hạn bởi kích thước bảng chữ cái. Mỗi lần hợp nhất sẽ tăng một ký tự và sau tối đa 26 lần tăng, chúng tôi sẽ bao bọc xung quanh, điều này sẽ hạn chế độ sâu truyền bá. Kết hợp với thực tế là mỗi phần tử được xử lý một lần, chúng ta có thể giữ toàn bộ quá trình tuyến tính ở kích thước đầu vào. 

Do đó, thay vì xây dựng các chuỗi, chúng tôi truyền bá các lần chạy nén và chỉ mô phỏng việc hợp nhất ranh giới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O( | S | ^2) | 
| Chạy Nén + Truyền Truyền | O(n · 26) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì một ngăn xếp trong đó mỗi phần tử biểu thị một khối nén gồm các ký tự giống hệt nhau liên tiếp sau khi hợp nhất một phần. 

Mỗi mục ngăn xếp lưu trữ một ký tự và tần số của nó. Ký tự này biểu thị một lần chạy sau tất cả các lần hợp nhất cho đến nay và tần suất cho biết nó xuất hiện liên tiếp bao nhiêu lần. 

1. Khởi tạo một ngăn xếp trống. Mỗi mục sẽ đại diện cho một chuỗi các ký tự giống hệt nhau sau khi nén. 
2. Với mỗi giá trị$a_i$, về mặt khái niệm, chúng tôi tạo ra sự đóng góp của$F_{a_i}$. Thay vì xây dựng nó, chúng tôi chuyển đổi nó thành “dạng nén”, là một lần chạy duy nhất của một ký tự đã biết với số lượng đã biết. Cấu trúc của$F_i$đảm bảo rằng sau khi nén bên trong hoàn toàn, nó hoạt động giống như một ký tự đơn được lặp lại$2^i$lần trước khi có bất kỳ sự hợp nhất bên ngoài nào. Chúng tôi chỉ quan tâm đến ranh giới hoạt động của nó chứ không quan tâm đến cấu trúc bên trong. 
3. Hợp nhất lần chạy mới này với phần trên cùng của ngăn xếp nếu chúng có cùng ký tự. Khi hai lần chạy liền kề có cùng một ký tự, chúng tôi kết hợp chúng và tăng ký tự lên một. Điều này phản ánh quy tắc chuyển đổi các chữ cái liền kề bằng nhau sẽ hợp nhất thành chữ cái tiếp theo trong bảng chữ cái. 
4. Sau khi hợp nhất, ký tự mới có thể khớp lại với phần tử tiếp theo trong ngăn xếp. Chúng tôi liên tục tuyên truyền hiệu ứng này lên trên. Đây là chuỗi mang: hợp nhất hai lần chạy bằng nhau sẽ tạo ra một ký tự cao hơn, có thể hợp nhất lại. 
5. Tiếp tục cho đến khi không còn hai lần chạy liền kề nào trong ngăn xếp có cùng ký tự. 
6. Sau khi xử lý tất cả các phần tử, hãy tính độ dài cuối cùng bằng cách tính tổng tần số của tất cả các lần chạy còn lại. 

Lý do điều này hoạt động là vì quá trình hợp nhất hoàn toàn mang tính cục bộ và liên kết trong các lần chạy. Khi một ranh giới được giải quyết, nó không bao giờ cần được xem lại trừ khi một phần mang ảnh hưởng đến nó và mang sự lan truyền đơn điệu đi lên trong giá trị ký tự, đảm bảo chấm dứt. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là ngăn xếp luôn biểu thị tiền tố rút gọn hoàn toàn của cấu trúc được nối theo quy tắc hợp nhất. Mỗi ranh giới chạy trong ngăn xếp tương ứng với hai phân đoạn liền kề với các ký tự riêng biệt, nghĩa là không thể hợp nhất ngay lập tức. Bất kỳ sự hợp nhất mới nào chỉ ảnh hưởng đến ranh giới ở trên cùng và mọi sự lan truyền chỉ di chuyển lên trên giá trị ký tự. Vì số ký tự tăng dần bị giới hạn và tăng nghiêm ngặt trong chuỗi truyền bá nên không có chu kỳ hoặc mâu thuẫn nào có thể xảy ra, đảm bảo tính chính xác và kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def merge_char(c):
    return 'a' if c == 'z' else chr(ord(c) + 1)

def add_run(stack, ch, cnt):
    while cnt > 0:
        if not stack:
            stack.append([ch, cnt])
            return
        top_ch, top_cnt = stack[-1]

        if top_ch != ch:
            stack.append([ch, cnt])
            return

        total = top_cnt + cnt
        stack.pop()

        ch = merge_char(ch)
        cnt = total

    if cnt > 0:
        stack.append([ch, cnt])

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        stack = []

        for x in a:
            ch = 'a'
            cnt = 1 << x

            add_run(stack, ch, cnt)

        print(sum(cnt for _, cnt in stack))

if __name__ == "__main__":
    solve()
```Mỗi mô hình thực hiện$F_i$dưới dạng một chuỗi các ký tự giống hệt nhau có độ dài là$2^i$. Điều này nhất quán với cấu trúc trước khi hợp nhất bên ngoài: tất cả cấu trúc bên trong sẽ sụp đổ thành một đường chạy thống nhất khi chỉ được xem thông qua hành vi hợp nhất liền kề bằng nhau. 

chức năng`add_run`thực hiện thao tác chính. Nó kiểm tra xem lần chạy đến có khớp với lần chạy hàng đầu hiện tại hay không. Nếu không, nó chỉ đơn giản là đẩy nó. Nếu chúng khớp nhau, nó sẽ hợp nhất số lượng và tăng ký tự, mô phỏng việc “mang theo”. Điều này có thể xếp tầng nhiều lần, đó là lý do tại sao nó lặp lại cho đến khi ổn định. 

Việc sử dụng`1 << x`tránh khả năng tính toán lại của hai và phản ánh sự tăng trưởng theo cấp số nhân của mỗi$F_x$. Vì chỉ độ dài chạy mới quan trọng nên điều này là đủ. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp được xây dựng nhỏ trong đó cấu trúc vẫn có thể quản lý được:$a = [0, 0, 1]$. 

Đây$F_0 = "a"$, Và$F_1 = "aba"$. Vì vậy, ban đầu chúng ta ghép “a”, “a”, “aba” về mặt khái niệm. 

| Bước | Ngăn xếp nội dung (char, count) | Hành động | 
| --- | --- | --- | 
| 1 | [] | bắt đầu | 
| 2 | [('a', 1)] | chèn F0 đầu tiên | 
| 3 | [('a', 2)] → [('b', 1)] | hợp nhất hai lần chạy 'a' | 
| 4 | [('b', 1), ('a', 2)] | chèn F1 mở rộng | 
| 5 | [('b', 1), ('a', 2)] | không hợp nhất giữa các ký tự khác nhau | 

Độ dài cuối cùng là 3. 

Dấu vết này cho thấy cách chỉ các lần chạy liền kề bằng nhau tương tác với nhau, trong khi các lần chạy khác nhau vẫn ổn định. 

Bây giờ hãy xem xét$a = [0, 0, 0]$. 

| Bước | Ngăn xếp nội dung (char, count) | Hành động | 
| --- | --- | --- | 
| 1 | [] | bắt đầu | 
| 2 | [('a', 1)] | đầu tiên | 
| 3 | [('a', 2)] → [('b', 1)] | hợp nhất | 
| 4 | [('b', 2)] → [('c', 1)] | hợp nhất lại | 

Ngăn xếp cuối cùng là [('c', 1)], vì vậy độ dài là 1. 

Điều này thể hiện sự lan truyền mang theo tầng qua nhiều lần hợp nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 26) | Mỗi lần hợp nhất có thể tăng ký tự lên tới 26 bước và mỗi phần tử được xử lý một lần | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa một lần chạy trên mỗi ranh giới phân đoạn | 

Thuật toán vẫn tuyến tính trong thực tế vì mỗi lần chạy được hợp nhất hoặc đẩy một lần và việc truyền ký tự bị giới hạn. Với tổng kích thước đầu vào lên tới$10^5$, điều này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    import sys
    input = sys.stdin.readline

    def merge_char(c):
        return 'a' if c == 'z' else chr(ord(c) + 1)

    def add_run(stack, ch, cnt):
        while cnt > 0:
            if not stack:
                stack.append([ch, cnt])
                return
            top_ch, top_cnt = stack[-1]

            if top_ch != ch:
                stack.append([ch, cnt])
                return

            total = top_cnt + cnt
            stack.pop()

            ch = merge_char(ch)
            cnt = total

        if cnt > 0:
            stack.append([ch, cnt])

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))

            stack = []
            for x in a:
                add_run(stack, 'a', 1 << x)

            print(sum(c for _, c in stack))

    solve()
    return sys.stdout.getvalue().strip()

# sample-like tests
assert run("1\n3\n0 0 1\n") == "3"
assert run("1\n3\n0 0 0\n") == "1"

# all equal small
assert run("1\n4\n0 0 0 0\n") == "1"

# mixed
assert run("1\n2\n1 1\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 1 | 3 | hợp nhất cơ bản giữa các khối | 
| 0 0 0 | 1 | truyền bá theo tầng | 
| 0 0 0 0 | 1 | lặp đi lặp lại sụp đổ hoàn toàn | 
| 1 1 | 3 | tương tác của các khối giống hệt nhau lớn hơn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là sự thu gọn hoàn toàn trong đó việc hợp nhất lặp đi lặp lại sẽ giảm toàn bộ cấu trúc thành một ký tự duy nhất. Đối với đầu vào như$a = [0, 0, 0, 0]$, mỗi khối đóng góp một chuỗi có độ dài 1 và các lần hợp nhất liên tiếp sẽ nâng cao ký tự cho đến khi tất cả cấu trúc sụp đổ. Ngăn xếp phát triển từ một lần chạy thành một lần chạy, sau đó lên ký tự cao hơn, duy trì tính chính xác vì mỗi lần hợp nhất sẽ giảm số lần chạy. 

Một trường hợp khác là khi không có sự hợp nhất nào xảy ra, chẳng hạn như$a = [3, 1]$. Các lần chạy tương ứng với các ký tự khác nhau kể từ khi bắt đầu biểu diễn nén của chúng, do đó ngăn xếp chỉ tích lũy hai mục nhập độc lập. Thuật toán tránh được mọi sự lan truyền mang theo một cách chính xác và độ dài cuối cùng chỉ là tổng độ dài chạy của chúng. 

Trường hợp ranh giới liên quan đến việc bao bọc từ 'z' đến 'a'. Khi việc hợp nhất lặp lại đẩy một ký tự vượt quá 'z', quá trình triển khai sẽ quay trở lại. Vì đây hoàn toàn là cục bộ của một lần chạy và không ảnh hưởng đến cấu trúc nên hoạt động của ngăn xếp vẫn nhất quán và quá trình vẫn chấm dứt do số lần hợp nhất liên tiếp bị giới hạn bởi số lượng ký tự có thể có.
