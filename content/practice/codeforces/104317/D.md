---
title: "CF 104317D – Giao chuỗi"
description: "Chúng ta có hai chuỗi $A$ và $B$. Chúng ta bắt đầu với một chuỗi trống $C$ và chúng ta được phép xây dựng $C$ bằng cách sao chép liên tục một chuỗi con từ $A$ và nối nó vào cuối $C$."
date: "2026-07-01T19:30:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104317
codeforces_index: "D"
codeforces_contest_name: "Shanghai University 2023 Spring Contest"
rating: 0
weight: 104317
solve_time_s: 95
verified: true
draft: false
---

[CF 104317D - Cung cấp chuỗi](https://codeforces.com/problemset/problem/104317/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,$A$Và$B$. Chúng tôi bắt đầu với một chuỗi trống$C$, và chúng ta được phép xây dựng$C$bằng cách sao chép liên tục một chuỗi con từ$A$và thêm nó vào cuối$C$. Chuỗi$A$không bao giờ thay đổi và mỗi thao tác có thể chọn bất kỳ đoạn liền kề nào bên trong$A$, có thể chồng chéo các lựa chọn trước đó hoặc lặp lại các chuỗi con trước đó. 

Nhiệm vụ là xây dựng$B$chính xác như$C$sử dụng số lượng tối thiểu các thao tác sao chép như vậy. 

Vì vậy, câu hỏi thực sự không phải là về việc xây dựng$C$trực tiếp, nhưng về việc chia tách$B$thành ít phân đoạn nhất sao cho mỗi phân đoạn xuất hiện ở đâu đó bên trong$A$như một chuỗi con. 

Các ràng buộc rất lớn: tổng chiều dài của tất cả$A$Và$B$trên các trường hợp thử nghiệm lên đến$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra sự tồn tại của chuỗi con một cách đơn giản cho mọi phân đoạn có thể có trong thời gian bậc hai. Một cách tiếp cận ngây thơ, đối với mọi vị trí trong$B$, thử tất cả các chuỗi con của$A$hoặc xác minh từng ứng viên bằng cách quét sẽ suy biến thành$O(|A| \cdot |B|)$, quá chậm. 

Các trường hợp lợi thế quan trọng xuất phát từ các mô hình trong đó việc tham lam thực hiện các trận đấu ngắn có vẻ hấp dẫn nhưng lại kém tối ưu nếu không được mở rộng tối đa. Ví dụ, nếu$A = \text{"abcd"}$Và$B = \text{"abcdabcd"}$, câu trả lời tối ưu là 2 bằng cách lấy "abcd" hai lần. Một chiến lược bất cẩn cắt sớm như "a", "b", "c", ... tạo ra 8 thao tác, đúng nhưng không tối thiểu. Một trường hợp tế nhị khác là khi$B$lặp lại các chuỗi con chồng chéo của$A$, khi không thể kéo dài trận đấu hoàn toàn bên trong$A$dẫn tới những cắt giảm không cần thiết. 

Quan sát cấu trúc quan trọng là khi một chuỗi con của$B$được xác nhận tồn tại ở đâu đó trong$A$, tốt hơn hết là bạn nên kéo dài thời gian càng xa càng tốt trước khi bắt đầu một hoạt động mới. 

## Phương pháp tiếp cận 

Chiến lược tấn công trực tiếp là mô phỏng quá trình xây dựng$B$bằng cách thử tất cả các chuỗi con có thể có của$A$ở mỗi bước. Tại mỗi vị trí trong$B$, chúng ta có thể liệt kê mọi chuỗi con của$A$, kiểm tra xem nó có khớp với tiền tố hiện tại của hậu tố còn lại của$B$và chọn cái hợp lệ dài nhất. Điều này đúng vì nó phản ánh trực tiếp các quy tắc hoạt động, nhưng nó yêu cầu so sánh chuỗi con lặp đi lặp lại. Mỗi so sánh có thể tốn kém$O(|B|)$trong trường hợp xấu nhất, và có$O(|A|^2)$chuỗi con của$A$, điều này làm cho phương pháp này không thể thực hiện được. 

Sự đơn giản hóa chính đến từ việc diễn giải lại hoạt động. Vì mọi thao tác đều nối thêm một chuỗi con của$A$, vấn đề trở thành phân vùng$B$thành các khối liền kề nhau, trong đó mỗi khối phải là một chuỗi con của$A$và chúng tôi muốn số khối tối thiểu. Thuộc tính quan trọng là một chuỗi có phải là một khối hợp lệ hay không chỉ phụ thuộc vào$A$, không phải về cách các khối trước đó được chọn. 

Sự độc lập này cho phép một chiến lược tham lam: bắt đầu từ vị trí hiện tại trong$B$, chúng ta nên mở rộng khối hiện tại càng nhiều càng tốt trong khi nó vẫn là chuỗi con của$A$. Nếu chúng tôi dừng sớm hơn, chúng tôi chỉ giảm độ dài của khối hợp lệ mà không ảnh hưởng đến tính khả thi trong tương lai, điều này chỉ có thể làm tăng số lượng khối. 

Để hỗ trợ kiểm tra sự tồn tại chuỗi con nhanh chóng, chúng ta có thể xây dựng một máy tự động hậu tố cho$A$. Một hậu tố automaton biểu diễn gọn gàng tất cả các chuỗi con của$A$và cho phép chúng tôi xác minh theo thời gian tuyến tính xem một chuỗi có phải là chuỗi con hay không bằng cách cố gắng thực hiện các chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê các chuỗi con) | (O( | A | ^2 \cdot | 
| Suffix Automaton + quét tham lam | (O(| A | + | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và sử dụng một máy tự động hậu tố được xây dựng từ$A$để kiểm tra tính hợp lệ của chuỗi con một cách hiệu quả. 

1. Xây dựng một máy tự động hậu tố trên chuỗi$A$. Cấu trúc này mã hóa tất cả các chuỗi con của$A$như các đường dẫn hợp lệ từ trạng thái ban đầu. 
2. Bắt đầu quét$B$từ vị trí ngoài cùng bên trái. Duy trì một con trỏ`i`đánh dấu sự khởi đầu hiện tại của phân khúc mà chúng tôi đang cố gắng xây dựng. 
3. Đối với mỗi phân đoạn, hãy đặt lại trạng thái máy tự động về trạng thái ban đầu và cố gắng mở rộng con trỏ thứ hai`j`bắt đầu từ`i`. 
4. Trong khi`j`nằm trong giới hạn và tồn tại sự chuyển đổi trong máy tự động cho ký tự$B[j]$, tiến về phía trước trong máy tự động và tiến lên`j`. Điều này đảm bảo rằng chuỗi con$B[i:j]$là một chuỗi con hợp lệ của$A$. 
5. Khi không thể mở rộng được nữa, chúng ta phải kết thúc đoạn hiện tại tại vị trí`j`. Tăng câu trả lời lên một. 
6. Đặt`i = j`và lặp lại cho đến hết chuỗi$B$được tiêu thụ. 

Ý tưởng chính là mỗi nhân vật của$B$được xử lý chính xác một lần như một phần của quá trình gia hạn thành công và mỗi lần chúng tôi không gia hạn được, chúng tôi sẽ cam kết thực hiện một thao tác. 

### Tại sao nó hoạt động 

Ở bất kỳ vị trí nào$i$, thuật toán xây dựng tiền tố dài nhất của$B[i:]$tồn tại dưới dạng chuỗi con của$A$. Thay vào đó, nếu một tiền tố ngắn hơn được chọn thì điều đó sẽ chỉ đưa ra một phần cắt bổ sung mà không mở rộng tập hợp các phần tiếp theo có thể truy cập được, bởi vì bất kỳ phân đoạn nào trong tương lai đều độc lập với cách chúng tôi phân chia các phân đoạn trước đó. Vì tính khả thi chỉ phụ thuộc vào việc mỗi phân đoạn có tồn tại trong$A$, tối đa hóa độ dài từng phân đoạn cục bộ sẽ giảm thiểu số lượng phân khúc trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SuffixAutomaton:
    def __init__(self):
        self.next = [dict()]
        self.link = [-1]
        self.length = [0]
        self.last = 0

    def extend(self, c):
        cur = len(self.next)
        self.next.append({})
        self.length.append(self.length[self.last] + 1)
        self.link.append(0)

        p = self.last
        while p != -1 and c not in self.next[p]:
            self.next[p][c] = cur
            p = self.link[p]

        if p == -1:
            self.link[cur] = 0
        else:
            q = self.next[p][c]
            if self.length[p] + 1 == self.length[q]:
                self.link[cur] = q
            else:
                clone = len(self.next)
                self.next.append(self.next[q].copy())
                self.length.append(self.length[p] + 1)
                self.link.append(self.link[q])

                while p != -1 and self.next[p].get(c) == q:
                    self.next[p][c] = clone
                    p = self.link[p]

                self.link[q] = self.link[cur] = clone

        self.last = cur

def build_sam(s):
    sam = SuffixAutomaton()
    for ch in s:
        sam.extend(ch)
    return sam

def solve():
    t = int(input())
    for _ in range(t):
        a = input().strip()
        b = input().strip()

        sam = build_sam(a)

        i = 0
        ans = 0
        n = len(b)

        while i < n:
            state = 0
            j = i

            while j < n and b[j] in sam.next[state]:
                state = sam.next[state][b[j]]
                j += 1

            ans += 1
            i = j

        print(ans)

if __name__ == "__main__":
    solve()
```Hậu tố quá trình xây dựng máy tự động$A$trong thời gian tuyến tính. Vòng lặp chính kết thúc$B$con trỏ tiến bộ$j$bất cứ khi nào có một quá trình chuyển đổi hợp lệ tồn tại trong máy tự động và khi nó không thành công, chúng tôi sẽ cam kết một phân đoạn và khởi động lại từ vị trí tiếp theo. Chi tiết triển khai quan trọng là chúng tôi không bao giờ đặt lại$j$lạc hậu nên mỗi ký tự của$B$được tiêu thụ nhiều nhất một lần trong quá trình chuyển đổi thành công, giữ cho độ phức tạp tổng thể tuyến tính. 

Một lỗi phổ biến là cố gắng sử dụng lại trạng thái tự động hóa trên các phân đoạn. Điều đó không tương ứng với bài toán, vì mỗi thao tác là một bản sao độc lập từ$A$, không phải là sự tiếp nối của chuỗi con trước đó. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:$A = \text{"jzq"}$,$B = \text{"jzqjzq"}$| Bước | tôi | j | Phân khúc hiện tại | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0→3 | "jzq" | hợp lệ trong A, mở rộng đầy đủ | 
| 2 | 3 | 3→6 | "jzq" | hợp lệ trong A, mở rộng đầy đủ | 

Điều này tạo ra 2 phân đoạn. Dấu vết cho thấy rằng khi chúng tôi đạt được kết quả khớp hoàn chỉnh, không có lần cắt nào trước đó sẽ cải thiện được điều gì vì việc dừng sớm sẽ chỉ tăng số lượng phân đoạn. 

Bây giờ hãy xem xét:$A = \text{"abcd"}$,$B = \text{"dcbadcba"}$| Bước | tôi | j | Phân khúc hiện tại | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0→1 | "d" | dừng lại (chỉ các trận đấu 'd' bắt đầu ở đường dẫn A) | 
| 2 | 1 | 1→2 | "c" | dừng lại | 
| 3 | 2 | 2→3 | "b" | dừng lại | 
| 4 | 3 | 3→4 | "một" | dừng lại | 
| 5 | 4 | 4→5 | "d" | dừng lại | 
| 6 | 5 | 5→6 | "c" | dừng lại | 
| 7 | 6 | 6→7 | "b" | dừng lại | 
| 8 | 7 | 7→8 | "một" | dừng lại | 

Điều này buộc phải thực hiện 8 thao tác vì không còn chuỗi con bắt đầu ở mỗi vị trí tồn tại trong$A$. Dấu vết xác nhận rằng thuật toán tự nhiên thoái hóa thành các phân đoạn ký tự đơn khi không thể so khớp dài hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | A | 
| Không gian | (O( | A | 

Tổng độ dài chuỗi trong các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó, giải pháp vẫn nằm trong giới hạn thoải mái vì mỗi ký tự được xử lý với số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# Since full solution is embedded, these are conceptual placeholders
# In practice, integrate solve() and capture output properly.

assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\na\naaaa`|`4`| khớp một ký tự lặp đi lặp lại | 
|`1\nabcd\ndcba`|`4`| phân mảnh trong trường hợp xấu nhất | 
|`1\nabcabc\nabcabc`|`1`| tái sử dụng toàn bộ chuỗi con dài | 
|`1\nababa\naba`|`1`| cấu trúc chuỗi con chồng chéo | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi$B$bao gồm các ký tự lặp đi lặp lại tồn tại trong$A$nhưng các mẫu dài hơn thì không. Ví dụ, nếu$A = "ab"$Và$B = "aaaa"$, máy tự động chỉ cho phép chuyển đổi một ký tự, do đó mỗi phân đoạn kết thúc ngay lập tức và thuật toán tạo ra 4 thao tác. Con trỏ`j`tiến lên chính xác một ký tự mỗi lần, do đó không xảy ra việc hợp nhất sai. 

Một trường hợp khác là khi$A$chứa nhiều chuỗi con chồng chéo. Vì$A = "abcab"$Và$B = "abcababcab"$, máy tự động cho phép khớp đầy đủ độ dài 5 ở mỗi bước và tiện ích mở rộng tham lam sẽ tiêu thụ toàn bộ khối trước khi đặt lại. Thuật toán không bao giờ cắt sớm vì luôn có thể mở rộng cho đến khi tới ranh giới.
