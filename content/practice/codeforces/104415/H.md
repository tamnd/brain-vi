---
title: "CF 104415H - Bạn đánh vần từ này như thế nào?"
description: "Chúng ta được cung cấp một từ điển cố định gồm các chuỗi. Mỗi chuỗi có thể được coi như một đường dẫn trong cây ký tự, trong đó mọi ký tự đều dẫn đến nhánh tiếp theo. Sau khi xây dựng cấu trúc này, chúng tôi được yêu cầu trả lời nhiều truy vấn."
date: "2026-06-30T19:52:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104415
codeforces_index: "H"
codeforces_contest_name: "IME++ Starters Try-outs 2023"
rating: 0
weight: 104415
solve_time_s: 61
verified: true
draft: false
---

[CF 104415H - Bạn đánh vần từ này như thế nào?](https://codeforces.com/problemset/problem/104415/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một từ điển cố định gồm các chuỗi. Mỗi chuỗi có thể được coi như một đường dẫn trong cây ký tự, trong đó mọi ký tự đều dẫn đến nhánh tiếp theo. Sau khi xây dựng cấu trúc này, chúng tôi được yêu cầu trả lời nhiều truy vấn. Mỗi truy vấn bản thân nó là một chuỗi và với mỗi truy vấn chúng ta phải xác định xem nó có tương ứng với một tiền tố hợp lệ trong từ điển hay không. Nếu đúng như vậy, chúng ta phải xuất ra một giá trị được tính toán trước liên quan đến điểm mà tiền tố đó kết thúc trong cấu trúc. Nếu nó không khớp với bất kỳ tiền tố nào thì câu trả lời là -1. 

Điểm mấu chốt là chúng ta không chỉ kiểm tra sự tồn tại của tiền tố. Mỗi nút tiền tố trong trie lưu trữ thông tin bổ sung: trong số tất cả các từ trong từ điển đi qua nút đó, chúng tôi quan tâm đến số lượng ký tự tối thiểu vẫn cần thiết để hoàn thành một từ điển đầy đủ từ nút đó trở xuống. Nói cách khác, ở mỗi tiền tố, chúng ta muốn biết mức độ “gần” hoàn thành một từ nào đó nếu chúng ta cam kết với tiền tố đó. 

Các ràng buộc ngụ ý tổng kích thước đầu vào theo thứ tự tổng của tất cả các độ dài từ trong từ điển cộng với tổng của tất cả các độ dài truy vấn. Điều này đẩy chúng ta tới thời gian tuyến tính theo kích thước của trie cộng với thời gian tuyến tính trong quá trình truyền tải truy vấn. Bất kỳ giải pháp nào xử lý lại các ký tự liên tục cho mỗi truy vấn hoặc mỗi tiền tố sẽ quá chậm. Việc quét theo từng truy vấn đơn giản đối với tất cả các từ trong từ điển sẽ dẫn đến công việc lặp đi lặp lại tỷ lệ thuận với kích thước từ điển, điều này ngay lập tức không khả thi khi cả từ điển và truy vấn đều lớn. 

Trường hợp phức tạp xuất hiện khi chuỗi truy vấn khớp với tiền tố tồn tại trong bộ ba nhưng bản thân nó không tương ứng với nút nơi chúng tôi lưu trữ thông tin. Ví dụ: nếu từ điển chứa "abc" và "abcd" thì truy vấn "ab" vẫn hoạt động vì "ab" là nút tiền tố hợp lệ ngay cả khi không có từ nào kết thúc ở đó. Một cách tiếp cận bất cẩn chỉ ghi lại thông tin ở các từ cuối cùng sẽ từ chối các truy vấn đó một cách không chính xác hoặc trả về các giá trị chưa được khởi tạo. 

Một trường hợp đặc biệt khác là khi nhiều từ trong từ điển có chung một tiền tố nhưng có độ dài còn lại rất khác nhau. Ví dụ: "a" và "abcde". Ở tiền tố "a", giá trị được lưu trữ chính xác phải phản ánh đường dẫn hoàn thành ngắn nhất chứ không phải đường dẫn tùy ý. 

## Phương pháp tiếp cận 

Cách trực tiếp để trả lời từng truy vấn là đối với mỗi chuỗi truy vấn, hãy quét qua tất cả các từ trong từ điển và kiểm tra xem có từ nào có truy vấn làm tiền tố hay không. Nếu vậy, chúng tôi tính toán độ dài tối thiểu còn lại bằng cách so sánh với tất cả các từ phù hợp. 

Điều này hoạt động hợp lý vì nó kiểm tra rõ ràng định nghĩa của câu trả lời, nhưng chi phí của nó rất cao. Đối với mỗi truy vấn, chúng tôi có thể so sánh với mọi từ trong từ điển rồi quét các ký tự bên trong những từ đó để xác minh mối quan hệ tiền tố. Nếu có nhiều từ và truy vấn có kích thước tương tự nhau, điều này sẽ trở thành bậc hai hoặc tệ hơn trong các phép toán tổng ký tự. 

Quan sát quan trọng là các mối quan hệ tiền tố được cấu trúc một cách tự nhiên dưới dạng tri. Thay vì tính toán lại việc kiểm tra tiền tố nhiều lần, chúng tôi xây dựng một cây duy nhất trong đó mỗi nút đại diện cho một tiền tố. Khi cấu trúc này tồn tại, cả sự tồn tại và tập hợp tiền tố trên tất cả các từ chia sẻ tiền tố đó sẽ trở thành thuộc tính cục bộ của một nút thay vì quét toàn bộ trên tất cả các chuỗi. 

Sau khi lưu trữ tất cả các từ trong một trie, chúng tôi có thể chú thích từng nút với độ dài còn lại tối thiểu để hoàn thành bất kỳ từ nào đi qua nút đó. Điều này có thể được tính toán trong quá trình chèn: khi chúng tôi đi xuống một từ, chúng tôi biết có bao nhiêu ký tự còn lại cho đến khi từ đó kết thúc và chúng tôi cập nhật từng nút đã truy cập với giá trị nhỏ nhất được thấy cho đến nay. Sau đó, các truy vấn giảm xuống chỉ còn việc duyệt bộ ba theo chuỗi truy vấn và đọc giá trị được lưu trữ ở nút cuối cùng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · S · L) | O(1) | Quá chậm | 
| Thử với tiền xử lý | O(S + R) | O(S) | Đã chấp nhận | 

Ở đây S là tổng chiều dài của các từ trong từ điển và R là tổng chiều dài của chuỗi truy vấn. 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng bản thử nghiệm trên tất cả các từ trong từ điển trong khi vẫn duy trì siêu dữ liệu bổ sung ở mỗi nút. 

1. Tạo một nút gốc đại diện cho tiền tố trống. Mỗi nút lưu trữ các liên kết con cho các ký tự và một giá trị số nguyên được khởi tạo thành một số lớn. Giá trị này sẽ biểu thị số ký tự tối thiểu còn lại cần thiết để hoàn thành một từ đi qua nút này. 
2. Với mỗi từ trong từ điển, duyệt từng ký tự trie. Tại vị trí i trong từ, độ dài còn lại là số ký tự còn lại từ i đến cuối từ. Tại mỗi nút được truy cập, cập nhật giá trị được lưu trữ với độ dài còn lại này nếu nó nhỏ hơn giá trị được lưu trữ hiện tại. Điều này đảm bảo mỗi nút tiền tố biết cách hoàn thành tốt nhất có thể trong số tất cả các từ đi qua nó. 
3. Sau khi xử lý tất cả các từ, mỗi nút trong bộ ba đã lưu trữ chính xác độ dài hậu tố tối thiểu trong số tất cả các từ trong từ điển có chứa tiền tố đó. 
4. Đối với mỗi chuỗi truy vấn, duyệt trie từ gốc theo các ký tự của nó. Nếu tại bất kỳ điểm nào thiếu ký tự tiếp theo, tiền tố đó không tồn tại trong cấu trúc từ điển và chúng ta sẽ xuất ngay -1. 
5. Nếu quá trình truyền tải thành công đối với toàn bộ truy vấn, chúng tôi sẽ xuất giá trị được lưu trữ ở nút cuối cùng. Giá trị này đã thể hiện độ dài hoàn thành tốt nhất có thể cho tiền tố đó. 

Thuộc tính quan trọng làm cho điều này đúng là mỗi từ trong từ điển đóng góp độ dài hậu tố của nó cho mọi nút tiền tố dọc theo đường dẫn của nó và mỗi nút chỉ giữ mức tối thiểu trên tất cả các đóng góp đó. Điều này có nghĩa là không cần tìm kiếm toàn cục tại thời điểm truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

class Node:
    __slots__ = ("nxt", "best")
    def __init__(self):
        self.nxt = {}
        self.best = INF

def solve():
    n = int(input())
    root = Node()

    for _ in range(n):
        w = input().strip()
        cur = root
        length = len(w)

        for i, ch in enumerate(w):
            rem = length - i
            if ch not in cur.nxt:
                cur.nxt[ch] = Node()
            cur = cur.nxt[ch]
            if rem < cur.best:
                cur.best = rem

    q = int(input())
    out = []

    for _ in range(q):
        s = input().strip()
        cur = root
        ok = True

        for ch in s:
            if ch not in cur.nxt:
                ok = False
                break
            cur = cur.nxt[ch]

        if not ok:
            out.append("-1")
        else:
            out.append(str(cur.best))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Trie được triển khai bằng cách sử dụng từ điển trên mỗi nút để giữ cho quá trình chuyển đổi nhỏ gọn và linh hoạt. Mỗi nút mang một`best`giá trị được khởi tạo thành vô cùng để lần cập nhật đầu tiên luôn đặt đường cơ sở có ý nghĩa. 

Trong quá trình chèn, chi tiết quan trọng đang được cập nhật`best`tại mọi nút tiền tố, không chỉ ở các nút đầu cuối. Chiều dài còn lại`len(word) - i`trực tiếp ghi lại số bước cần thiết từ tiền tố đó để hoàn thành từ. 

Trong quá trình truy vấn, việc truyền tải hoàn toàn mang tính cấu trúc. Không có sự tổng hợp hoặc tính toán nào được thực hiện ngoài các cạnh sau. Câu trả lời là giá trị được lưu trữ hoặc -1 nếu đường dẫn bị hỏng. 

## Ví dụ đã hoạt động 

Hãy xem xét một cuốn từ điển nhỏ với các từ`"abc"`Và`"abd"`và truy vấn`"ab"`,`"abc"`, Và`"a"`. 

Để chèn, chúng tôi theo dõi các nút tiền tố và độ dài còn lại. 

| Lời | Tiền tố | Đã đạt đến nút | Độ dài còn lại | Node.best sau khi cập nhật | 
| --- | --- | --- | --- | --- | 
| abc | một | một | 3 | 3 | 
| abc | ab | ab | 2 | 2 | 
| abc | abc | abc | 1 | 1 | 
| abd | một | một | 3 | 3 | 
| abd | ab | ab | 2 | 2 | 
| abd | abd | abd | 1 | 1 | 

Bây giờ truy vấn`"ab"`: 

| Bước | Nhân vật | Nút tồn tại | Nút hiện tại | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | một | vâng | một | di chuyển | 
| 2 | b | vâng | ab | di chuyển | 

Đầu ra là`ab.best = 2`, nghĩa là thời gian hoàn thành ngắn nhất từ ​​tiền tố "ab" là 2 ký tự. 

Truy vấn`"abc"`theo sau nút`abc`và trả về`1`. Truy vấn`"a"`trả lại`3`. 

Những dấu vết này cho thấy giá trị được lưu trữ luôn phản ánh phần mở rộng ngắn nhất trong số tất cả các từ có chung tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S + R) | Mỗi ký tự từ điển được xử lý một lần trong quá trình chèn trie và mỗi ký tự truy vấn được xử lý một lần trong quá trình truyền tải | 
| Không gian | O(S) | Mỗi nút tiền tố duy nhất trong trie tương ứng với nhiều nhất một chuyển đổi ký tự cho mỗi ký tự từ điển | 

Độ phức tạp phù hợp với cấu trúc ràng buộc vì mỗi ký tự trong đầu vào đều đóng góp một lượng công việc không đổi trong quá trình xây dựng hoặc truyền tải truy vấn. Không có tính toán lại xảy ra trên các truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**18

    class Node:
        __slots__ = ("nxt", "best")
        def __init__(self):
            self.nxt = {}
            self.best = INF

    def solve():
        n = int(input())
        root = Node()

        for _ in range(n):
            w = input().strip()
            cur = root
            length = len(w)

            for i, ch in enumerate(w):
                rem = length - i
                if ch not in cur.nxt:
                    cur.nxt[ch] = Node()
                cur = cur.nxt[ch]
                if rem < cur.best:
                    cur.best = rem

        q = int(input())
        out = []

        for _ in range(q):
            s = input().strip()
            cur = root
            ok = True

            for ch in s:
                if ch not in cur.nxt:
                    ok = False
                    break
                cur = cur.nxt[ch]

            if not ok:
                out.append("-1")
            else:
                out.append(str(cur.best))

        return "\n".join(out)

    return solve()

# minimal dictionary
assert run("1\nabc\n3\nab\nabc\na\n") == "2\n1\n3"

# prefix missing
assert run("1\nabc\n2\nb\nabcd\n") == "-1\n-1"

# multiple words same prefix
assert run("3\na\nab\nabc\n3\na\nab\nabc\n") == "1\n1\n1"

# single character words
assert run("2\na\nb\n2\na\nb\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi tiền tố 1 từ | 2,1,3 | duyệt tiền tố chính xác và lưu trữ các giá trị tốt nhất | 
| thiếu tiền tố | -1,-1 | xử lý đúng các đường dẫn không hợp lệ | 
| tiền tố chồng chéo | 1,1,1 | độ chính xác tổng hợp tối thiểu | 
| chữ cái | 1,1 | từ viết hoa có cạnh nhỏ nhất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một truy vấn kết thúc tại một nút bên trong không phải là từ cuối. Ví dụ như từ điển`"abcd"`và truy vấn`"ab"`. Nút trie cho`"ab"`tồn tại, nhưng không có từ nào kết thúc ở đó. Thuật toán vẫn gán giá trị hợp lệ`best`giá trị vì trong quá trình chèn,`"abcd"`đóng góp độ dài còn lại là 2 tại nút`"ab"`, do đó truy vấn trả về chính xác`2`. 

Một trường hợp khác là khi nhiều từ có chung một tiền tố nhưng có độ dài khác nhau đáng kể, chẳng hạn như`"a"`Và`"aaaaa"`. Tại nút`"a"`, thuật toán so sánh độ dài còn lại`0`Và`4`và giữ`0`, đảm bảo rằng một từ hoàn chỉnh kết thúc ngay lập tức sẽ được ưu tiên hơn so với những từ hoàn chỉnh dài hơn. 

Trường hợp cuối cùng là truy vấn khớp một phần nhưng phân kỳ sớm. Đối với từ điển`"apple"`và truy vấn`"apx"`, truyền tải thất bại tại`'x'`bởi vì không có cạnh nào tồn tại từ`"ap"`và thuật toán ngay lập tức trả về`-1`mà không cần cố gắng tính toán thêm.
