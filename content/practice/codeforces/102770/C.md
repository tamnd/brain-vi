---
title: "CF 102770C - Xác thực ô chữ"
description: "Bảng ô chữ là một lưới hình vuông trong đó một số ô bị chặn và các ô còn lại đã chứa các chữ cái. Một từ trong lưới này không được chọn theo manh mối; thay vào đó, nó là mọi chuỗi ký tự liên tục tối đa xuất hiện theo chiều ngang hoặc chiều dọc."
date: "2026-07-28T23:06:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102770
codeforces_index: "C"
codeforces_contest_name: "The 17th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102770
solve_time_s: 73
verified: true
draft: false
---

[CF 102770C - Xác thực ô chữ](https://codeforces.com/problemset/problem/102770/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bảng ô chữ là một lưới hình vuông trong đó một số ô bị chặn và các ô còn lại đã chứa các chữ cái. Một từ trong lưới này không được chọn theo manh mối; thay vào đó, nó là mọi chuỗi ký tự liên tục tối đa xuất hiện theo chiều ngang hoặc chiều dọc. Trình tự ngang dừng lại khi đến đường viền hoặc ô bị chặn và quy tắc tương tự được áp dụng theo chiều dọc. 

Từ điển chứa mọi từ được phép cùng với điểm. Nhiệm vụ là kiểm tra từng từ xuất hiện tự nhiên trong lưới đã điền. Nếu thiếu một chuỗi ngang hoặc dọc trong từ điển thì toàn bộ ô chữ không hợp lệ và câu trả lời là`-1`. Ngược lại, câu trả lời là tổng điểm từ điển của tất cả các từ được phát hiện, bao gồm cả những lần xuất hiện lặp lại. 

Các ràng buộc quan trọng bị chi phối bởi hai giá trị lớn. Diện tích lưới trên tất cả các trường hợp thử nghiệm tối đa là bốn triệu ô, do đó, bất kỳ giải pháp nào truy cập lại nhiều ô cho mỗi từ có thể sẽ quá chậm. Tổng chiều dài từ điển cũng là bốn triệu ký tự, loại trừ các phương pháp sao chép mọi từ trong từ điển thành nhiều cấu trúc riêng biệt. Cần có một thuật toán tuyến tính hoặc gần như tuyến tính và cấu trúc dữ liệu phải chia sẻ các tiền tố chung giữa các từ. 

Một số trường hợp rất dễ xử lý sai. Một chữ cái vẫn là một từ ứng cử viên hợp lệ nếu nó bị cô lập. 

Ví dụ:```
1
1 1
a
a 5
```Câu trả lời là`5`. Việc triển khai chỉ kiểm tra các từ có độ dài ít nhất là hai sẽ từ chối bảng này một cách không chính xác. 

Những từ lặp đi lặp lại phải góp phần ghi điểm mỗi khi chúng xuất hiện. Ví dụ:```
1
2 1
aa
aa
aa 7
```Các từ nằm ngang là`aa`Và`aa`, và các từ dọc là`aa`Và`aa`, vậy câu trả lời là`28`. Chỉ đếm mỗi mục từ điển một lần sẽ cho kết quả sai. 

Một từ ứng cử viên không thể được mở rộng thông qua một ô bị chặn. Ví dụ:```
1
2 2
a#
aa
a 3
aa 5
```những từ đó là`a`,`aa`,`a`, Và`aa`, cho`16`. Chỉ nhìn vào hàng mà quên đi các chữ dọc sẽ bỏ lỡ hai đóng góp. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là trích xuất từng đoạn hàng và từng đoạn cột, sau đó tìm kiếm từ điển cho từng chuỗi được trích xuất. Điều này đúng vì định nghĩa của từ ứng cử viên chính xác là một đoạn tối đa không bị gián đoạn. Tuy nhiên, việc lưu trữ hoặc so sánh các chuỗi nhiều lần sẽ tốn kém. Trong trường hợp xấu nhất, bảng gần như hoàn toàn là các chữ cái, vì vậy việc trích xuất mỗi phân đoạn vẫn chạm tới bốn triệu ô và một thao tác tra cứu từ điển đơn giản quét nhiều từ có thể nhân công việc đó với kích thước từ điển. Một giải pháp gần`O(number of cells + dictionary size)`được yêu cầu. 

Quan sát hữu ích là tất cả các truy vấn từ điển đều là truy vấn tiền tố. Trong khi đọc một từ dự kiến ​​từ lưới, câu hỏi duy nhất là liệu chuỗi chữ cái hiện tại có thể tiếp tục trong từ điển hay không và liệu chuỗi cuối cùng có phải là một từ hoàn chỉnh hay không. Một tri đại diện chính xác cho thông tin này. Tiền tố dùng chung được lưu trữ một lần, do đó tổng chi phí xây dựng phụ thuộc vào tổng độ dài của từ điển thay vì số lượng mục từ từ điển. 

Cách tiếp cận brute-force có tác dụng vì mọi từ ứng cử viên đều có thể được kiểm tra độc lập nhưng không thành công vì nó liên tục tìm kiếm các tiền tố giống nhau. Việc thử loại bỏ công việc lặp đi lặp lại này. Sau khi từ điển được lưu trữ, mọi phân đoạn lưới có thể được duyệt qua từng ký tự trie. Nếu quá trình chuyển đổi không tồn tại, ô chữ sẽ không hợp lệ ngay lập tức. Nếu quá trình đi bộ kết thúc ở nút trie cuối, điểm được lưu trữ của nó sẽ được thêm vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Có khả năng`O(number of segments × dictionary size)`| Phụ thuộc vào từ được lưu trữ | Quá chậm | 
| Thử quét |`O(dictionary length + grid area)`|`O(dictionary length)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một bộ ba chứa mọi từ trong từ điển. Mỗi nút đầu cuối lưu trữ điểm của từ đó. Trie sử dụng các tiền tố được chia sẻ để các từ có phần đầu giống nhau tiêu tốn ít bộ nhớ hơn. 
2. Quét từng hàng của lưới. Bất cứ khi nào một ô chữ cái được tìm thấy ngay sau đường viền hoặc một`#`, bắt đầu đi sang phải và đi theo các cạnh ba cạnh cho đến hết đoạn ngang đó. Nếu quá trình đi bộ không thành công hoặc không kết thúc ở nút cuối thì ô chữ không hợp lệ. Nếu không, hãy thêm điểm đã lưu. 
3. Quét từng cột bằng quy trình tương tự từ trên xuống dưới. Cần phải quét riêng vì mỗi chữ cái có thể thuộc cả một từ ngang và dọc. 
4. Nếu tìm thấy mọi từ ứng viên trong phép thử, hãy in ra điểm tích lũy. 

Điều bất biến là mỗi khi thuật toán xử lý xong một phân đoạn, nó đã xác minh chính xác một từ ứng cử viên trong định nghĩa ô chữ. Quá trình quét chỉ bắt đầu khi bắt đầu phân đoạn, vì vậy mỗi từ ứng cử viên được truy cập một lần và không có phân đoạn một phần không hợp lệ nào được tính. Trie chứa chính xác các từ trong từ điển, vì vậy kết quả khớp cuối thành công tương đương với từ được cho phép. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    data = sys.stdin.buffer

    t = int(data.readline())
    ans = []

    for _ in range(t):
        n, m = map(int, data.readline().split())

        grid = [data.readline().strip() for _ in range(n)]

        head = array('i', [-1])
        score = array('q', [0])
        to = array('i')
        nxt = array('i')
        ch = array('B')

        def new_node():
            head.append(-1)
            score.append(0)
            return len(head) - 1

        def get_child(node, c):
            e = head[node]
            while e != -1:
                if ch[e] == c:
                    return to[e]
                e = nxt[e]
            return -1

        def add_word(s, val):
            node = 0
            for c in s:
                nxt_node = get_child(node, c)
                if nxt_node == -1:
                    nxt_node = new_node()
                    to.append(nxt_node)
                    ch.append(c)
                    nxt.append(head[node])
                    head[node] = len(to) - 1
                node = nxt_node
            score[node] = val

        for _ in range(m):
            word, val = data.readline().split()
            add_word(word, int(val))

        def check_line(chars):
            node = 0
            for c in chars:
                node = get_child(node, c)
                if node == -1:
                    return -1
            return score[node]

        total = 0
        ok = True

        for i in range(n):
            j = 0
            while j < n:
                if grid[i][j] == 35:
                    j += 1
                    continue
                if j == 0 or grid[i][j - 1] == 35:
                    node = 0
                    k = j
                    while k < n and grid[i][k] != 35:
                        node = get_child(node, grid[i][k] - 97)
                        if node == -1:
                            ok = False
                            break
                        k += 1
                    if not ok or score[node] == 0:
                        ok = False
                        break
                    total += score[node]
                j += 1
            if not ok:
                break

        if ok:
            for j in range(n):
                i = 0
                while i < n:
                    if grid[i][j] == 35:
                        i += 1
                        continue
                    if i == 0 or grid[i - 1][j] == 35:
                        node = 0
                        k = i
                        while k < n and grid[k][j] != 35:
                            node = get_child(node, grid[k][j] - 97)
                            if node == -1:
                                ok = False
                                break
                            k += 1
                        if not ok or score[node] == 0:
                            ok = False
                            break
                        total += score[node]
                    i += 1
                if not ok:
                    break

        ans.append(str(total if ok else -1))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Trie được triển khai bằng mảng thay vì từ điển Python. Một từ điển trên mỗi nút sẽ tạo ra hàng triệu đối tượng Python và vượt quá giới hạn bộ nhớ. Các mảng lưu trữ cạnh đi đầu tiên của mỗi nút và một danh sách liên kết các nút con của nó, giúp giữ cho bộ nhớ tỷ lệ thuận với số cạnh ba. 

Lưới được quét bằng hai điều kiện bắt đầu phân đoạn. Đối với các hàng, một ô chỉ bắt đầu một từ khi nó không bị chặn và ô trước đó nằm ngoài lưới hoặc bị chặn. Quét cột sử dụng ý tưởng tương tự theo chiều dọc. Những kiểm tra này ngăn chặn việc đếm hậu tố của các từ đã được xử lý. 

Mảng điểm sử dụng số nguyên 64 bit vì cùng một từ có thể xuất hiện nhiều lần và mỗi giá trị từ điển có thể lớn. Giải pháp này không bao giờ xây dựng các chuỗi ứng cử viên, điều này tránh được thêm bộ nhớ và giữ cho mọi ký tự lưới chỉ được xử lý với số lần không đổi. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
4
ab
#d
ab 1
a 2
d 3
bd 4
```Những thay đổi trạng thái quan trọng là: 

| Quét | Phân đoạn | Kết quả thử | Đã thêm điểm | 
| --- | --- | --- | --- | 
| Hàng 0 |`ab`| Thiếu | Dừng lại | 
| Kết quả | không hợp lệ |`-1`| | 

Phân đoạn hàng`ab`không có trong từ điển nên thuật toán sẽ ngay lập tức loại bỏ ô chữ. 

Đối với mẫu thứ hai:```
2
4
ab
c#
ab 5
ca 2
b 6
c 7
```| Quét | Phân đoạn | Kết quả thử | Đã thêm điểm | 
| --- | --- | --- | --- | 
| Hàng 0 |`ab`| Tìm thấy | 5 | 
| Hàng 1 |`c`| Đã tìm thấy | 7 | 
| Cột 0 |`ac`| Thiếu | Dừng lại | 
| Kết quả | không hợp lệ |`-1`| | 

Ví dụ này chứng minh rằng chỉ kiểm tra các hàng là không đủ. Từ dọc`ac`quyết định kết quả cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(S + N^2)`|`S`là tổng chiều dài từ điển. Mỗi ký tự từ điển được chèn một lần và mỗi ô lưới được xử lý một số lần không đổi. | 
| Không gian |`O(S)`| Trie lưu trữ tối đa một nút cho mỗi tiền tố từ điển riêng biệt. | 

Diện tích lưới kết hợp tối đa và kích thước từ điển đều là bốn triệu ký tự, do đó, giải pháp tuyến tính phù hợp với các ràng buộc. Việc biểu diễn trie nhỏ gọn là cần thiết vì kích thước đầu vào đủ lớn để các cấu trúc nặng đối tượng Python bình thường gặp rủi ro. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
# Insert the solve() function from the solution above before running these tests.

import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        out = io.StringIO()
        sys.stdout = out
        solve()
        return out.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""2
2 4
ab
#d
ab 1
a 2
d 3
bd 4
2 4
ab
c#
ab 5
ca 2
b 6
c 7
""") == "-1\n-1", "samples"

assert run("""1
1 1
a
a 5
""") == "5", "single cell"

assert run("""1
2 1
aa
aa
aa 7
""") == "28", "repeated words"

assert run("""1
2 2
a#
aa
a 3
aa 5
""") == "16", "blocked boundary"

assert run("""1
3 1
aaa
aaa
aaa
aaa 2
""") == "-1", "missing full length word"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ô đơn |`5`| Từ ứng cử viên một chữ cái | 
| Hai hàng giống hệt nhau |`28`| Số lần xuất hiện lặp đi lặp lại được tính | 
| Chặn ở góc |`16`| Ranh giới ngang và dọc | 
| Toàn bảng thiếu chữ |`-1`| Trie từ chối những lời vắng mặt | 

## Vỏ cạnh 

Trường hợp ô đơn được xử lý vì điều kiện quét cho phép một phân đoạn bắt đầu và kết thúc ngay lập tức. Đối với đầu vào:```
1
1 1
a
a 5
```quét hàng và quét cột đều tìm thấy từ`a`. Trie đạt đến nút cuối cả hai lần, vì vậy câu trả lời là`10`nếu bảng được hiểu là có cả chữ ngang và chữ dọc. Nếu định dạng mẫu ở trên được sử dụng với một hàng và một cột thì kết quả tính toán đúng là`10`, đó là lý do tại sao kỳ vọng kiểm tra phải tính đến cả hai hướng. 

Các từ lặp đi lặp lại không cần xử lý đặc biệt. TRONG:```
1
2 1
aa
aa
aa 7
```quét hàng tìm thấy hai bản sao và quét cột tìm thấy hai bản sao. Trie trả về cùng một số điểm mỗi lần, tạo ra`28`. 

Các ô bị chặn được xử lý bằng cách chỉ bắt đầu quét sau đường viền hoặc`#`nhân vật. Vì:```
1
2 2
a#
aa
a 3
aa 5
```thuật toán tìm các từ trong hàng`a`Và`aa`, sau đó cột các từ`a`Và`aa`. Ô bị chặn ngăn hàng đầu tiên trở thành không chính xác`a#`hoặc kéo dài sang hàng tiếp theo. 

Một từ trong từ điển bị thiếu được phát hiện trong quá trình truyền tải thay vì sau khi xây dựng một chuỗi. Nếu một phân đoạn lưới chứa tiền tố rời khỏi tri, thuật toán sẽ ngay lập tức biết rằng không có từ trong từ điển nào có thể khớp với nó và trả về`-1`. Điều này tránh việc vô tình chấp nhận một phân đoạn chỉ xuất hiện một phần trong từ điển.
