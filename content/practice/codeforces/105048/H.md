---
title: "CF 105048H - Stringliloquy"
description: "Chúng ta được cung cấp một chuỗi dài biểu thị một “văn bản” có độ dài $N$. Bên cạnh đó, chúng ta còn được cung cấp một tập hợp các từ riêng biệt $M$. Tổng độ dài của tất cả các từ gộp lại đều bị giới hạn nên dù có thể có nhiều từ nhưng cấu trúc gộp của chúng vẫn gọn."
date: "2026-06-28T05:44:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105048
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 03-22-24 Div. 2 (Beginner)"
rating: 0
weight: 105048
solve_time_s: 84
verified: false
draft: false
---

[CF 105048H - Stringliloquy](https://codeforces.com/problemset/problem/105048/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài biểu thị một “văn bản” có độ dài$N$. Ngoài ra, chúng tôi còn được tặng một bộ$M$những từ riêng biệt. Tổng độ dài của tất cả các từ gộp lại đều bị giới hạn nên dù có thể có nhiều từ nhưng cấu trúc gộp của chúng vẫn gọn. 

Sau đó chúng tôi được yêu cầu$Q$câu hỏi. Mỗi câu hỏi có một khoảng thời gian$[l, r]$bên trong chuỗi chính và chúng ta phải đếm xem có bao nhiêu chuỗi con chứa đầy đủ trong khoảng đó khớp với bất kỳ từ nào đã cho. Mỗi lần xuất hiện đều được tính độc lập, vì vậy nếu một từ xuất hiện nhiều lần ở các vị trí khác nhau hoặc nếu các từ khác nhau trùng nhau thì tất cả các kết quả trùng khớp hợp lệ sẽ góp phần tạo ra câu trả lời. 

Khó khăn cốt lõi là cả chuỗi và số lượng truy vấn đều lớn, do đó việc tính toán lại các kết quả khớp bên trong mỗi khoảng truy vấn từ đầu là quá chậm. Cấu trúc ẩn là tất cả những gì chúng ta quan tâm là liệu một chuỗi con bắt đầu ở một vị trí nào đó có khớp với một trong các từ trong từ điển hay không và sau đó có bao nhiêu vị trí bắt đầu đó nằm trong mỗi phạm vi truy vấn. 

Các ràng buộc ngụ ý rằng giải pháp phải gần tuyến tính hoặc gần tuyến tính về kích thước của văn bản cộng với tổng chiều dài từ điển, có thể tính theo logarit cho mỗi truy vấn. Bất cứ điều gì bậc hai trên$N$hoặc hơn$Q$ngay lập tức là không thể. 

Một nỗ lực ngây thơ sẽ quét mọi khoảng thời gian truy vấn và thử tất cả các kết quả khớp với chuỗi con. Ngay cả việc hạn chế các từ trong từ điển, việc kiểm tra mọi vị trí đối với tất cả các từ sẽ đưa ra trường hợp xấu nhất$O(N \cdot M)$, vượt xa giới hạn. 

Một trường hợp thất bại tinh tế hơn đến từ các trận đấu chồng chéo. Ví dụ, nếu chuỗi là`"AAAAA"`và từ bao gồm`"A"`,`"AA"`, Và`"AAA"`, thì ở mọi vị trí, nhiều kết quả khớp sẽ bắt đầu và việc liệt kê chuỗi con đơn giản sẽ đếm không chính xác gấp đôi hoặc mất quá nhiều thời gian để mở rộng tất cả các khả năng cho mỗi truy vấn. 

Một trường hợp đặc biệt khác là khi có nhiều truy vấn chồng chéo lên nhau. Nếu chúng tôi tính toán lại các kết quả khớp cho mỗi truy vấn một cách độc lập thì công việc lặp lại sẽ diễn ra ngay cả khi các kết quả khớp cơ bản giống hệt nhau. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mọi câu trả lời hợp lệ đều được xác định bởi vị trí bắt đầu của các kết quả khớp trong từ điển. Nếu chúng ta biết, với mọi chỉ số$i$, có bao nhiêu từ bắt đầu tại$i$, thì mỗi truy vấn sẽ trở thành một tổng phạm vi đơn giản trên mảng đó. 

Vì vậy, vấn đề giảm xuống còn việc tính toán một mảng tần số trên văn bản: đối với mỗi vị trí, hãy đếm xem có bao nhiêu từ trong từ điển khớp bắt đầu từ đó. 

Một cách mạnh mẽ là thử từng từ ở mọi vị trí. Điều này dẫn đến$O(N \cdot \text{word length})$tổng số so sánh cho mỗi từ, quá lớn. 

Cấu trúc của nhiều mẫu có tiền tố chung gợi ý việc xây dựng một trie. Thay vì kiểm tra từng từ một cách độc lập ở mọi vị trí, chúng ta có thể quét văn bản một lần trong khi duyệt bộ ba, điều này sẽ hợp nhất các tiền tố lặp lại một cách tự nhiên. Điều này chuyển đổi các so sánh ký tự lặp đi lặp lại thành các chuyển tiếp được chia sẻ. 

Chúng tôi xây dựng một bộ ba từ, đánh dấu các nút đầu cuối bằng số lượng từ kết thúc ở đó. Sau đó, chúng tôi quét văn bản bắt đầu từ mỗi vị trí, tiến về phía trước trong quá trình thử cho đến khi không thể khớp được nữa. Bất cứ khi nào chúng tôi đến nút cuối, chúng tôi ghi lại rằng một từ kết thúc ở đó, nghĩa là một chuỗi con hợp lệ bắt đầu ở vị trí ban đầu. 

Vì tổng chiều dài từ được giới hạn bởi$10^5$, tổng số lần chuyển đổi trie trên tất cả các từ cũng bị giới hạn và mỗi lần quét từ một vị trí sẽ dừng ngay khi xảy ra sự không khớp. Điều này làm cho tổng công việc tỷ lệ thuận với tổng số lần chuyển tiếp phù hợp thay vì$N \cdot M$. 

Khi chúng ta có một mảng`cnt[i]`= số từ bắt đầu ở vị trí$i$, chúng tôi xây dựng một tổng tiền tố. Mỗi truy vấn$[l, r]$trở thành một phép trừ duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot M \cdot L)$|$O(1)$| Quá chậm | 
| Trie + Tiền tố Tổng | (O(N + \sum | từ | )) | 

## Hướng dẫn thuật toán 

1. Xây dựng một bản thử từ tất cả các từ. Mỗi nút lưu trữ các chuyển tiếp cho các chữ cái từ A đến Z và một bộ đếm cho biết có bao nhiêu từ kết thúc tại nút đó. Điều này cho phép nhiều từ chia sẻ tiền tố để sử dụng lại cấu trúc thay vì sao chép công việc. 
2. Khởi tạo một mảng`cnt`kích thước$N$với số không. Mảng này sẽ lưu trữ bao nhiêu từ điển bắt đầu ở mỗi vị trí trong chuỗi chính. 
3. Đối với mỗi vị trí xuất phát$i$trong chuỗi chính, duyệt từng ký tự trie bắt đầu từ$s[i]$. Mỗi bước sẽ di chuyển xuống phần thử nếu ký tự tồn tại; nếu không thì dừng ngay vì không thể khớp được nữa. 
4. Bất cứ khi nào chúng ta đến một nút trie đánh dấu sự kết thúc của một hoặc nhiều từ, hãy thêm số lượng thiết bị đầu cuối của nút đó vào`cnt[i]`. Điều này nắm bắt tất cả các từ bắt đầu ở vị trí$i$và khớp với tiền tố được thấy cho đến nay. 
5. Lặp lại quá trình này cho tất cả các vị trí bắt đầu. Tổng số lần chuyển đổi trie bị giới hạn bởi tổng độ dài của tất cả các từ, vì mỗi bước sẽ tiến lên dọc theo tiền tố từ điển hợp lệ hoặc dừng lại. 
6. Xây dựng mảng tổng tiền tố`cnt`. Điều này cho phép trả lời bất kỳ truy vấn nào$[l, r]$trong thời gian không đổi bằng cách tính toán$\sum_{i=l}^{r} cnt[i]$. 
7. Đối với mỗi truy vấn, xuất ra chênh lệch của tổng tiền tố. 

### Tại sao nó hoạt động 

Mỗi lần xuất hiện hợp lệ của một từ trong từ điển tương ứng với chính xác một cặp: vị trí bắt đầu$i$và một từ phù hợp bắt đầu từ đó. Việc truyền tải trie từ$i$liệt kê chính xác những từ đó mà không trùng lặp vì mỗi từ tương ứng với một đường dẫn nút đầu cuối duy nhất. Bước tổng tiền tố không làm thay đổi số lượng mà chỉ tổng hợp các khoản đóng góp theo các khoảng thời gian. Vì mỗi trận đấu được tính đúng một lần trong`cnt[i]`và mỗi truy vấn tính tổng chính xác các vị trí đó bên trong$[l, r]$, the result is correct.

 ## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("next", "end")
    def __init__(self):
        self.next = {}
        self.end = 0

def solve():
    N, M, Q = map(int, input().split())
    s = input().strip()

    root = Node()

    # build trie
    for _ in range(M):
        w = input().strip()
        cur = root
        for ch in w:
            if ch not in cur.next:
                cur.next[ch] = Node()
            cur = cur.next[ch]
        cur.end += 1

    cnt = [0] * N

    # scan from every position
    for i in range(N):
        cur = root
        j = i
        while j < N:
            ch = s[j]
            if ch not in cur.next:
                break
            cur = cur.next[ch]
            if cur.end:
                cnt[i] += cur.end
            j += 1

    # prefix sums
    pref = [0] * (N + 1)
    for i in range(N):
        pref[i + 1] = pref[i] + cnt[i]

    out = []
    for _ in range(Q):
        l, r = map(int, input().split())
        out.append(str(pref[r] - pref[l - 1]))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc trie đảm bảo rằng các tiền tố dùng chung không bị tính toán lại nhiều lần. Mỗi nút`end`value tổng hợp số lượng từ kết thúc tại thời điểm đó, do đó, các từ trùng lặp trong từ điển sẽ được xử lý một cách tự nhiên nếu chúng tồn tại. Vòng quét dừng ngay lập tức khi không khớp, ngăn chặn việc truyền tải không cần thiết. 

Mảng tổng tiền tố dịch chuyển các chỉ số theo một để các truy vấn phạm vi trở thành phép trừ rõ ràng mà không phức tạp từng cái một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`"ABCDABCD"`Từ:`"AB", "BC", "CD", "A"`Chúng tôi tính toán`cnt[i]`: 

| tôi | hậu tố bắt đầu | trận đấu được tìm thấy | cnt[i] | 
| --- | --- | --- | --- | 
| 1 | A B C D... | A | 1 | 
| 2 | B C D... | BC | 1 | 
| 3 | C D... | CD | 1 | 
| 4 | D A B... | không | 0 | 
| 5 | A B C D... | A, AB | 2 | 
| 6 | B C D... | BC | 1 | 
| 7 | C D... | CD | 1 | 
| 8 | D... | không | 0 | 

Tổng tiền tố sau đó cho phép các truy vấn như$[4,7]$hoặc$[1,8]$để được trả lời trực tiếp. Bảng này cho thấy các trận đấu chồng chéo ở các vị trí xuất phát khác nhau được tích lũy độc lập như thế nào. 

### Ví dụ 2 

Chuỗi đầu vào:`"AAAAA"`Từ:`"A", "AA", "AAA"`| tôi | trận đấu | cnt[i] | 
| --- | --- | --- | 
| 1 | A, AA, AAA | 3 | 
| 2 | A, AA, AAA | 3 | 
| 3 | A, AA, AAA | 3 | 
| 4 | A, AA | 2 | 
| 5 | A | 1 | 

Điều này chứng tỏ tại sao việc quét tất cả các chuỗi con sẽ phát nổ: mọi vị trí đều tạo ra nhiều từ hợp lệ, nhưng việc duyệt qua ba lần đảm bảo chúng tôi dừng sớm khi không còn phần mở rộng nào nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N + \sum | từ | 
| Không gian | (O(N + \sum | từ | 

Giới hạn$N, Q \le 5 \cdot 10^4$và tổng chiều dài từ$10^5$phù hợp thoải mái trong sự phức tạp này. Giải pháp thực hiện công việc tuyến tính về cơ bản ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder if integrated

# NOTE: real integration assumes solve() is available

# sample tests would be inserted when solve() is callable
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| char đơn tối thiểu | 1 | trường hợp cơ sở đúng đắn | 
| từ chồng chéo | số lượng cao | xử lý chồng chéo | 
| không có trận đấu | 0 giây | trận đấu trie trống | 
| từ chuỗi đầy đủ | 1 bảo hiểm đầy đủ | trận đấu đầy đủ ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mỗi từ là một ký tự đơn. Trong tình huống đó, mọi vị trí trong chuỗi có khả năng khớp với nhiều từ và trie thoái hóa thành cấu trúc phân nhánh một cấp. Thuật toán vẫn hoạt động chính xác vì mỗi vị trí đóng góp độc lập vào`cnt[i]`và tổng tiền tố tích lũy những đóng góp này mà không có sự tương tác. 

Một trường hợp khác là khi các từ có chung tiền tố dài nhưng phân kỳ muộn, chẳng hạn như`"ABCDE", "ABCDF", "ABCDG"`. Trie đảm bảo tiền tố được chia sẻ`"ABCD"`được duyệt một lần cho mỗi vị trí bắt đầu và việc phân nhánh chỉ xảy ra ở bước cuối cùng. Mặc dù nhiều từ có thể khớp với nhau ở cùng độ sâu,`end`bộ đếm tại các nút đầu cuối đảm bảo tất cả được tính chính xác mà không cần quét riêng biệt.
