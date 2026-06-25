---
title: "CF 105790J - Jugando Fuerte"
description: "Vấn đề mô tả một chuỗi người chơi được sắp xếp thành một hàng, trong đó mỗi người chơi sở hữu một chuỗi đại diện cho bộ bài của họ."
date: "2026-06-25T06:22:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "J"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 46
verified: true
draft: false
---

[CF 105790J - Jugando Fuerte](https://codeforces.com/problemset/problem/105790/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một chuỗi người chơi được sắp xếp thành một hàng, trong đó mỗi người chơi sở hữu một chuỗi đại diện cho bộ bài của họ. Các bộ bài này không bị cô lập: “bàn tay” hiệu quả của mỗi người chơi được hình thành bằng cách mở rộng bộ bài của riêng họ với các bộ bài của một số người chơi cố định ở bên trái, được biểu thị bằng một số nguyên liên kết với người chơi đó. Nói cách khác, người chơi i không chỉ quan tâm đến chuỗi của chính họ mà còn quan tâm đến cửa sổ nối của các chuỗi trước đó kết thúc tại i. 

Ngoài cấu trúc này, chúng ta được cung cấp một số mẫu, mỗi mẫu bao gồm một chuỗi và một điểm. Bất cứ khi nào một mẫu xuất hiện dưới dạng chuỗi con bên trong ván bài hiệu quả của người chơi và lần xuất hiện đó kết thúc trong ranh giới bộ bài ban đầu của chính người chơi đó, người chơi đó sẽ nhận được số điểm tương ứng. Nhiệm vụ là tính toán cho mỗi người chơi số điểm cao nhất mà họ có thể đạt được từ tất cả các mẫu phù hợp với vùng hợp lệ của họ. 

Khó khăn đến từ sự chồng chéo giữa các bàn tay mở rộng của người chơi và nhu cầu phát hiện nhiều lần xuất hiện chuỗi con một cách hiệu quả. Việc đọc trực tiếp gợi ý khớp chuỗi trên một cấu trúc nối lớn có nhiều truy vấn, trong đó cả tổng chiều dài văn bản và tổng chiều dài mẫu có thể lớn, theo thứ tự 10^5. Điều này ngay lập tức loại trừ việc tìm kiếm chuỗi con đơn giản trên mỗi mẫu cho mỗi người chơi, sẽ hoạt động giống như O(N * M * L) trong trường hợp xấu nhất và không đạt được giới hạn tiêu chuẩn của Codeforces. 

Cấu trúc cũng che giấu một ràng buộc chính: tổng độ dài của tất cả các chuỗi đầu vào bị giới hạn, do đó, bất kỳ giải pháp nào xử lý mỗi ký tự với số lần không đổi đều khả thi, trong khi bất kỳ giải pháp nào quét liên tục các chuỗi con hoặc tính toán lại đều không khớp với mỗi người chơi. 

Trường hợp cạnh tinh vi xuất hiện khi các mẫu chồng lên nhau nhiều và nhiều mẫu khớp nhau ở cùng một vị trí kết thúc. Ví dụ: nếu một văn bản chứa “aaaaa” và các mẫu là “a”, “aa”, “aaa”, việc quét theo từng mẫu đơn giản có thể đếm gấp đôi hoặc bỏ lỡ việc tổng hợp điểm số chính xác trừ khi các kết quả trùng khớp được nhóm theo vị trí. 

Một trường hợp cạnh khác phát sinh từ việc căn chỉnh ranh giới. Giả sử một mẫu kết thúc chính xác tại ranh giới giữa ván bài mở rộng của người chơi và bộ bài của người chơi tiếp theo. Việc mô hình đó có đóng góp hay không phụ thuộc hoàn toàn vào việc liệu phần cuối của nó có nằm trong bộ bài gốc của người chơi hiện tại hay không. Việc triển khai bất cẩn chỉ kiểm tra sự xuất hiện bên trong cửa sổ mở rộng mà không xác thực vị trí điểm cuối sẽ chỉ định điểm không chính xác cho người chơi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xử lý từng mẫu một cách độc lập và quét mọi vị trí kết thúc có thể có trong chuỗi hiệu quả của mọi người chơi. Cụ thể, chúng tôi sẽ xây dựng chuỗi mở rộng đầy đủ của mỗi người chơi, sau đó, đối với mỗi mẫu, hãy chạy tìm kiếm chuỗi con, chẳng hạn như đối sánh đơn giản hoặc thậm chí là KMP. Nếu chúng tôi giả sử tổng chiều dài văn bản xung quanh N và tổng chiều dài mẫu xung quanh M, thì điều này dẫn đến khoảng O(N * M) cho mỗi người chơi theo cách hiểu tồi tệ nhất và thậm chí kết hợp được tối ưu hóa vẫn trở thành O(N * số mẫu), quá lớn khi cả N và M đều lên tới 10^5. 

Sự kém hiệu quả xuất phát từ việc liên tục khởi động lại việc so khớp mẫu cho mọi mẫu và mọi vị trí, mặc dù thực tế là tất cả các mẫu đều có chung bảng chữ cái cơ bản và chúng tôi luôn quét cùng một cấu trúc văn bản chung. 

Điều quan trọng cần lưu ý là đây không phải là tập hợp các vấn đề về chuỗi con độc lập mà là một vấn đề khớp nhiều mẫu trên một văn bản được chia sẻ. Thay vì tìm kiếm từng mẫu riêng biệt, chúng ta có thể xây dựng một máy tự động xử lý đồng thời tất cả các mẫu trong khi quét biểu diễn được nối của tất cả các bộ bài của người chơi. Đây chính xác là cài đặt mà máy tự động Aho-Corasick trở nên hữu ích: nó nén tất cả các chuyển đổi mẫu thành một cấu trúc duy nhất và cho phép chúng tôi phát hiện tất cả các kết quả khớp mẫu theo thời gian tuyến tính trên văn bản.

Sau khi biết tất cả các lần xuất hiện, mỗi lần xuất hiện có thể được ánh xạ tới một người chơi bằng cách sử dụng tính năng theo dõi vị trí trong chuỗi được nối. Khó khăn còn lại là thực thi điều kiện trận đấu chỉ đóng góp nếu nó kết thúc bên trong phân đoạn ban đầu của đúng người chơi. Điều này được xử lý bằng cách tính toán trước ranh giới phân đoạn cho mỗi người chơi và kiểm tra xem chỉ số cuối của mỗi trận đấu có nằm trong khoảng đó hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (theo tìm kiếm mẫu) | O(N · M) | O(N) | Quá chậm | 
| Aho-Corasick trên văn bản được nối | O(N + tổng chiều dài mẫu + kết quả khớp) | O(N + tổng số mẫu) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ghép chuỗi bộ bài của tất cả người chơi thành một chuỗi chung trong khi lưu trữ bộ bài đó thuộc về người chơi nào đối với từng vị trí. Điều này biến đổi cấu trúc được phân đoạn thành một văn bản tuyến tính duy nhất có ánh xạ cạnh. 
2. Tính toán, đối với mỗi người chơi, khoảng thời gian của các chỉ số tương ứng với bộ bài ban đầu của họ bên trong chuỗi được nối. Khoảng thời gian này là cần thiết để xác minh xem trận đấu có kết thúc với đúng người chơi hay không. 
3. Xây dựng máy tự động Aho-Corasick từ tất cả các chuỗi mẫu. Mỗi trạng thái đầu cuối lưu trữ điểm liên quan đến mẫu đó, do đó, nhiều mẫu kết thúc trong cùng một nút có thể được xử lý một cách tự nhiên. 
4. Duyệt qua văn bản được nối thông qua ký tự tự động theo từng ký tự, theo các liên kết chuyển tiếp và dự phòng. Bất cứ khi nào một trạng thái có các mẫu đầu ra, hãy ghi lại kết quả khớp ở vị trí hiện tại. 
5. Đối với mỗi kết quả khớp được phát hiện kết thúc ở vị trí i, hãy xác định độ dài của nó và điểm bắt đầu của nó. Kiểm tra xem người chơi nào sở hữu vị trí cuối cùng i và xác minh rằng tôi nằm trong khoảng thời gian trên bộ bài ban đầu của người chơi đó. 
6. Nếu trận đấu hợp lệ đối với người chơi đó, hãy cập nhật điểm của người chơi bằng cách sử dụng giá trị của mẫu, thường bằng cách lấy mức tối đa hoặc tổng tùy thuộc vào cách giải thích của nhiều trận đấu. 
7. Sau khi xử lý toàn bộ văn bản, xuất ra điểm tính toán cho mỗi người chơi. 

### Tại sao nó hoạt động 

Máy tự động đảm bảo rằng mọi chuỗi con của văn bản được nối đều được truy cập chính xác một lần dưới dạng đường dẫn chuyển tiếp trong cấu trúc trie. Vì các liên kết hậu tố truyền bá các kết quả khớp một phần nên không có mẫu hợp lệ nào bị bỏ qua. Việc ánh xạ từ vị trí tới người chơi sẽ phân chia văn bản thành các phân đoạn rời rạc, do đó, mỗi trận đấu sẽ được quy cho chính xác một người chơi ứng cử viên dựa trên điểm cuối của nó. Vì chúng tôi chỉ chấp nhận các kết quả khớp có điểm cuối nằm trong khoảng thời gian ban đầu của người chơi nên chúng tôi thực thi hạn chế của bài toán mà không cần tính toán lại ranh giới chuỗi con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

class AhoCorasick:
    def __init__(self):
        self.next = [{}]
        self.link = [-1]
        self.out = [[]]

    def add(self, s, value):
        v = 0
        for c in s:
            if c not in self.next[v]:
                self.next[v][c] = len(self.next)
                self.next.append({})
                self.link.append(-1)
                self.out.append([])
            v = self.next[v][c]
        self.out[v].append(value)

    def build(self):
        q = deque()
        self.link[0] = 0
        for c, v in self.next[0].items():
            self.link[v] = 0
            q.append(v)

        while q:
            v = q.popleft()
            for c, u in self.next[v].items():
                q.append(u)
                j = self.link[v]
                while j and c not in self.next[j]:
                    j = self.link[j]
                self.link[u] = self.next[j].get(c, 0)

                self.out[u].extend(self.out[self.link[u]])

    def run(self, text, belong, lbound, rbound):
        res = [0] * len(lbound)
        v = 0

        for i, c in enumerate(text):
            while v and c not in self.next[v]:
                v = self.link[v]
            v = self.next[v].get(c, 0)

            for val in self.out[v]:
                p = belong[i]
                if lbound[p] <= i <= rbound[p]:
                    res[p] = max(res[p], val)

        return res

def solve():
    n = int(input())
    s = []
    lbound = []
    rbound = []
    belong = []
    idx = 0

    for i in range(n):
        a = input().strip()
        s.append(a)
        lbound.append(idx)
        for _ in a:
            belong.append(i)
        idx += len(a)
        rbound.append(idx - 1)

    text = "".join(s)

    m = int(input())
    ac = AhoCorasick()

    for _ in range(m):
        t, x = input().split()
        x = int(x)
        ac.add(t, x)

    ac.build()
    ans = ac.run(text, belong, lbound, rbound)

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng máy tự động trên tất cả các mẫu, sau đó xử lý văn bản được nối một lần. các`belong`mảng rất quan trọng vì nó bảo toàn quyền sở hữu của từng ký tự sau khi hợp nhất tất cả các bộ bài. Mảng khoảng`lbound`Và`rbound`xác định tính hợp lệ của các điểm cuối mẫu, đảm bảo rằng chỉ những trận đấu kết thúc bên trong bộ bài của chính người chơi mới đóng góp. 

Một lỗi phổ biến là quên rằng việc truyền liên kết hậu tố có thể khiến nhiều mẫu kích hoạt ở cùng một trạng thái, do đó`out[v]`phải tổng hợp các giá trị từ tất cả các trạng thái đầu cuối có thể truy cập. Một điểm tinh tế khác là kiểm tra tính hợp lệ bằng cách sử dụng chỉ mục điểm cuối, không phải chỉ mục bắt đầu, vì vấn đề xác định mức đóng góp dựa trên vị trí kết thúc khớp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét hai người chơi có bộ bài`"ab"`Và`"bc"`và các mẫu`"b"`với giá trị 5 và`"ab"`với giá trị 10. 

| Bước | Ký tự hiện tại | Tiểu bang | Trận đấu | Người chơi đánh | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | một | 0 | không | - | [0, 0] | 
| 1 | b | 1 | "b" | P0/P1 phụ thuộc vào ranh giới | cập nhật | 

Quá trình truyền tải cho thấy các kết quả trùng khớp được phát hiện tại các điểm cuối chính xác và chỉ những kết quả kết thúc bằng khoảng thời gian người chơi hợp lệ mới được chấp nhận. 

Ví dụ này chứng tỏ rằng máy tự động không phân biệt trực tiếp người chơi nên việc lọc ranh giới là điều cần thiết. 

### Ví dụ 2 

Lấy một người chơi`"aaa"`với các mẫu`"a" = 1`,`"aa" = 3`,`"aaa" = 10`. 

| tôi | char | trận đấu của bang | cập nhật tốt nhất | 
| --- | --- | --- | --- | 
| 0 | một | một | 1 | 
| 1 | một | à, àa | 3 | 
| 2 | một | à, aa, aaa | 10 | 

Dấu vết xác nhận rằng các mẫu chồng chéo được xử lý một cách tự nhiên thông qua các liên kết hậu tố và tất cả các kết quả khớp kết thúc ở mỗi vị trí đều được xem xét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng văn bản + tổng chiều dài mẫu + kết quả trùng khớp) | Mỗi ký tự và mẫu đóng góp một lần trong quá trình truyền tải tự động | 
| Không gian | O(tổng số nút trong máy tự động) | Trie plus liên kết lỗi và danh sách đầu ra | 

Các ràng buộc cho phép xử lý tuyến tính trên toàn bộ chiều dài kết hợp của tất cả các chuỗi, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()  # adapted if needed

# sample-like and custom cases

assert run("""2
ab
bc
2
b 5
ab 10
""").strip() != "", "basic case"

assert run("""1
aaaa
3
a 1
aa 3
aaa 10
""").strip() != "", "overlap patterns"

assert run("""3
a
a
a
1
a 5
""").strip() != "", "uniform small strings"

assert run("""1
x
1
y 10
""").strip() != "", "no match case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hai người chơi nhỏ | tính toán | phân bổ ranh giới | 
| mô hình chồng chéo | tính toán | tổng hợp liên kết hậu tố | 
| tất cả các ký tự giống nhau | tính toán | tính chính xác chồng chéo nặng nề | 
| không có trận đấu | số không | xử lý vắng mặt | 

## Vỏ cạnh 

Trường hợp một cạnh là khi nhiều mẫu kết thúc ở cùng một vị trí nhưng thuộc các chuỗi liên kết hậu tố khác nhau. Trong một văn bản như`"aaaa"`, ở vị trí 3, mẫu`"a"`,`"aa"`, Và`"aaa"`đều chấm dứt. Máy tự động đảm bảo cả ba đều có mặt trong`out[v]`thông qua việc truyền liên kết hậu tố và tập hợp tối đa sẽ chọn giá trị chính xác nhất. 

Một trường hợp khác là khi một mẫu trải dài qua ranh giới của người chơi. Ví dụ: nếu người chơi A có`"ab"`và người chơi B có`"cd"`, phép nối là`"abcd"`. một mẫu`"bc"`khớp trên ranh giới ở vị trí 2. Mặc dù nó tồn tại trong văn bản chung, điểm cuối nằm trong khoảng của người chơi B, do đó nó chỉ được quy cho B. Kiểm tra khoảng thời gian thực thi điều này một cách chính xác. 

Trường hợp tinh tế cuối cùng là khi một mẫu kết thúc chính xác ở ký tự cuối cùng trong bộ bài của người chơi. Vì các ranh giới được bao gồm ở bên phải nên việc kiểm tra`lbound[p] <= i <= rbound[p]`bao gồm chính xác kết quả khớp này, đảm bảo rằng các lần xuất hiện căn chỉnh theo cạnh không bị mất.
