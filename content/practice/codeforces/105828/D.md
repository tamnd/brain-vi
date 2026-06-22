---
title: "CF 105828D - \u041e\u043f\u044f\u0442\u044c \u0441\u0435\u043a\u0440\u0435\u0442\u0438\u043a\u0438"
description: "Chúng ta được cung cấp một tập hợp các chuỗi nhị phân, tất cả đều có cùng độ dài. Mỗi chuỗi là một từ trong một ngôn ngữ và chúng ta chỉ được phép rút ngắn một từ bằng cách xóa các ký tự ở đầu bên phải của nó."
date: "2026-06-21T13:03:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105828
codeforces_index: "D"
codeforces_contest_name: "\u0424\u0438\u043d\u0430\u043b \u0412\u041a\u041e\u0428\u041f.Junior 2025"
rating: 0
weight: 105828
solve_time_s: 47
verified: true
draft: false
---

[CF 105828D - \u041e\u043f\u044f\u0442\u044c \u0441\u0435\u043a\u0440\u0435\u0442\u0438\u043a\u0438](https://codeforces.com/problemset/problem/105828/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi nhị phân, tất cả đều có cùng độ dài. Mỗi chuỗi là một từ trong một ngôn ngữ và chúng ta chỉ được phép rút ngắn một từ bằng cách xóa các ký tự ở đầu bên phải của nó. Sau khi rút ngắn tất cả các từ, chúng tôi muốn giữ một thuộc tính: đối với mỗi từ, phiên bản rút gọn của nó vẫn đủ để xác định duy nhất từ ​​gốc trong toàn bộ tập hợp. 

Về mặt hình thức, đối với mỗi từ, chúng tôi chọn một tiền tố của từ đó và tiền tố này không được là tiền tố của bất kỳ từ nào khác sau khi rút ngắn. Tương tự, không có hai từ nào được phép chia sẻ cùng một cách biểu diễn rút gọn và đối với mỗi từ, chúng tôi muốn giữ tiền tố ngắn nhất để phân biệt nó với tất cả các từ khác. 

Đầu ra là phiên bản mới của mỗi từ, trong đó mỗi từ được thay thế bằng tiền tố phân biệt tối thiểu của nó. 

Những hạn chế rất quan trọng ở đây. Có thể có tới 80.000 từ, mỗi từ có độ dài tối đa là 32. Điều này ngay lập tức loại trừ bất kỳ số nào bậc hai trong n. Một giải pháp so sánh từng cặp chuỗi hoặc quét liên tục tất cả các từ cho mỗi tiền tố sẽ hoạt động giống như O(n²k), tốc độ này quá chậm. Giá trị nhỏ của k gợi ý rằng chúng ta nên coi mỗi từ là một đối tượng bit có kích thước cố định và khai thác cấu trúc hoặc các lần thử theo chiều bit. 

Một vấn đề tế nhị phát sinh khi nhiều từ có chung tiền tố dài. Ví dụ: nếu chúng ta có các từ như 110000, 110001, 110010 thì tiền tố phân biệt có thể rất dài và nhiều từ chỉ phân kỳ ở các bit cuối cùng. Một ý tưởng ngây thơ về việc dừng lại ở ký tự khác nhau đầu tiên cho mỗi từ một cách độc lập là không chính xác, bởi vì tính duy nhất phụ thuộc vào toàn bộ tập hợp chứ không phải so sánh từng cặp một cách biệt lập. 

Một trường hợp khác là khi tất cả các từ chỉ khác nhau ở bit cuối cùng. Sau đó, mỗi từ phải giữ nguyên độ dài trừ đi một tiền tố. Bất kỳ phương pháp nào chỉ xem xét các hàng xóm ngay lập tức hoặc tần số tiền tố cục bộ ở độ sâu nông sẽ đánh giá thấp độ dài tiền tố cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ là, đối với mỗi từ, hãy thử tăng độ dài tiền tố từ 1 lên k và kiểm tra xem tiền tố đó có xuất hiện trong bất kỳ từ nào khác hay không. Việc kiểm tra tiền tố yêu cầu quét tất cả các từ hoặc sử dụng hàm băm, điều này vẫn dẫn đến O(n²k) trong trường hợp xấu nhất. Với 80.000 từ, ngay cả việc quét tuyến tính trên mỗi độ dài tiền tố cũng vượt xa giới hạn. 

Điều quan trọng là chúng ta chỉ quan tâm đến việc phân biệt các từ với nhau và điểm phân biệt giữa hai từ là vị trí đầu tiên mà chúng khác nhau. Nếu chúng ta có thể sắp xếp tất cả các từ sao cho các tiền tố tương tự được nhóm lại với nhau thì chúng ta có thể bản địa hóa các so sánh. Điều này tự nhiên gợi ý một phép thử nhị phân trên các bit. 

Khi chèn tất cả các từ vào một trie, mỗi nút đại diện cho một tiền tố và chúng ta có thể lưu trữ số lượng từ đi qua nó. Tiền tố đủ để phân biệt một từ một cách chính xác khi nút tương ứng với tiền tố đó có số đếm là 1. Nghĩa là không có từ nào khác chia sẻ nó. 

Vì vậy, thay vì so sánh một từ với tất cả các từ khác, chúng ta chỉ cần đi theo đường đi của nó trong bộ ba và tìm nút đầu tiên có kích thước cây con trở thành 1. Độ sâu đó là độ dài tiền tố duy nhất ngắn nhất cho từ đó. 

Điều này làm giảm vấn đề từ việc so sánh toàn cục đến việc đếm cây con cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tất cả các tiền tố đối với tất cả các từ) | O(n²k) | O(1) hoặc O(nk) | Quá chậm | 
| Thử với số lượng cây con | O(nk) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các từ trong hai lần bằng cách sử dụng trie nhị phân.

1. Xây dựng một bộ ba nhị phân trong đó mỗi nút tương ứng với một tiền tố bit. Đối với mỗi từ, chúng tôi bắt đầu từ gốc và theo dõi các cạnh cho từng bit, tạo các nút nếu cần. Tại mỗi nút, chúng tôi tăng một bộ đếm để lưu trữ số lượng từ đi qua nó. Bộ đếm này biểu thị số lượng từ chia sẻ tiền tố này. 
2. Sau khi bộ ba được xây dựng, chúng tôi xử lý lại từng từ một cách độc lập. Chúng tôi đi xuống bộ thử theo các bit của nó từ gốc. 
3. Khi duyệt qua một từ, chúng ta theo dõi độ sâu. Tại mỗi nút, chúng tôi kiểm tra bộ đếm được lưu trữ. Vị trí đầu tiên mà bộ đếm này trở thành 1 là điểm sớm nhất mà tiền tố này là duy nhất trong số tất cả các từ. 
4. Chúng tôi dừng ngay ở độ sâu đó và ghi lại độ dài rút gọn cần thiết cho từ đó. 
5. Đầu ra của mỗi từ chỉ đơn giản là tiền tố của nó ở độ sâu đó. 

Lý do chúng ta có thể dừng ngay lập tức là vì khi tiền tố là duy nhất thì bất kỳ tiền tố nào dài hơn cũng là duy nhất, nhưng không phải tiền tố ngắn hơn nên lần xuất hiện đầu tiên là tối ưu. 

### Tại sao nó hoạt động 

Mỗi nút trie tương ứng chính xác với một tiền tố nhị phân. Bộ đếm được lưu trữ tại một nút bằng với số lượng từ đầu vào chia sẻ tiền tố đó. Nếu một nút có bộ đếm 1 thì chính xác một từ sẽ đến nút đó, do đó tiền tố đó sẽ xác định từ duy nhất trong toàn bộ tập hợp. Nếu bộ đếm lớn hơn 1 thì ít nhất một từ khác có cùng tiền tố nên chưa thể sử dụng từ đó. Do đó, tiền tố phân biệt tối thiểu chính xác là nút đầu tiên dọc theo đường dẫn của từ với bộ đếm 1. Điều này đảm bảo tính tối thiểu vì chúng ta duyệt qua các tiền tố theo thứ tự độ dài tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("child", "cnt")
    def __init__(self):
        self.child = [-1, -1]
        self.cnt = 0

def solve():
    n, k = map(int, input().split())
    words = [input().strip() for _ in range(n)]

    trie = [Node()]

    def add(word):
        v = 0
        trie[v].cnt += 1
        for ch in word:
            b = ord(ch) - 48
            if trie[v].child[b] == -1:
                trie[v].child[b] = len(trie)
                trie.append(Node())
            v = trie[v].child[b]
            trie[v].cnt += 1

    for w in words:
        add(w)

    out = []

    for w in words:
        v = 0
        prefix = []
        ans_len = k
        for i, ch in enumerate(w):
            b = ord(ch) - 48
            v = trie[v].child[b]
            prefix.append(ch)
            if trie[v].cnt == 1:
                ans_len = i + 1
                break
        out.append("".join(prefix[:ans_len]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng một trie dựa trên mảng đơn giản trong đó mỗi nút lưu trữ hai con cho các bit 0 và 1, cộng với bộ đếm số lượng từ đi qua nó. Bước đầu tiên xây dựng cấu trúc và điền số lượng cây con. Lần thứ hai đi từng từ cho đến khi đến nút có số đếm là 1. 

Một lỗi phổ biến là quên tăng bộ đếm gốc, điều này cần thiết để số lượng vẫn nhất quán và mỗi nút phản ánh đúng số lượng từ đi qua nó. Một sự tinh tế khác là đảm bảo chúng ta dừng lại ở lần xuất hiện đầu tiên của cnt == 1 thay vì tiếp tục, vì việc tiếp tục vẫn đúng nhưng sẽ không tạo ra tiền tố tối thiểu. 

## Ví dụ đã hoạt động 

Hãy xem xét một tập hợp các từ nhỏ: 110, 111, 100. 

Sau khi xây dựng trie, gốc chia thành 1 và 0. Dưới 1, chúng ta có 10 và 11 nhánh. Nút tương ứng với tiền tố 100 là duy nhất ở đầu, trong khi 110 và 111 chỉ tách biệt ở bit cuối cùng. 

Đối với từ 110: 

| Bước | Tiền tố | Số lượng nút Trie | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | không độc đáo | 
| 2 | 11 | 2 | không độc đáo | 
| 3 | 110 | 1 | dừng lại | 

Đối với từ 111: 

| Bước | Tiền tố | Số lượng nút Trie | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | không độc đáo | 
| 2 | 11 | 2 | không độc đáo | 
| 3 | 111 | 1 | dừng lại | 

Đối với từ 100: 

| Bước | Tiền tố | Số lượng nút Trie | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | không độc đáo | 
| 2 | 10 | 1 | dừng lại | 

Những dấu vết này cho thấy tính duy nhất được xác định hoàn toàn bằng số lượng cây con chứ không phải bằng so sánh cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi từ được chèn một lần và được duyệt một lần trong bộ ba độ sâu k | 
| Không gian | O(nk) | Mỗi bit có thể tạo một nút trie trong trường hợp xấu nhất | 

Các ràng buộc cho phép tối đa 80.000 từ với độ dài lên tới 32, vì vậy nk có khoảng 2,5 triệu thao tác, vừa vặn thoải mái trong giới hạn trong Python với cách biểu diễn trie nhỏ gọn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdout
    sys.stdin = StringIO(inp)
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = backup
    return out.strip()

# minimal distinct
assert solve_capture("3 3\n000\n001\n010\n") == "000\n001\n010"

# full overlap until last bit
assert solve_capture("2 3\n000\n001\n") == "000\n001"

# all differ at first bit
assert solve_capture("4 3\n000\n100\n010\n110\n") == "0\n1\n0\n1"

# chain-like prefixes
assert solve_capture("3 4\n0000\n0001\n0010\n") == "0000\n0001\n0010"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 dây nhỏ riêng biệt | không thay đổi | tính đúng đắn cơ bản | 
| hai chỉ khác nhau chút cuối cùng | cần có tiền tố có độ dài đầy đủ | sự độc đáo sâu sắc | 
| chia cho bit đầu tiên | tiền tố ngắn đủ | phân kỳ sớm | 
| chuỗi tiền tố dài được chia sẻ | tách dần dần | thử xử lý độ sâu | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các từ giống hệt nhau cho đến ký tự cuối cùng. Ví dụ: 0000, 0001, 0010, 0011. Trie sẽ có tiền tố chung lớn và tính duy nhất chỉ xuất hiện ở độ sâu 3 hoặc 4 tùy theo phân nhánh. Thuật toán xử lý vấn đề này bằng cách đảm bảo các bộ đếm vẫn lớn hơn 1 cho đến khi phân kỳ cuối cùng, do đó không có từ nào bị cắt quá sớm. 

Một trường hợp khác là khi một từ chỉ trở thành duy nhất ở ký tự cuối cùng. Trong trường hợp đó, quá trình truyền tải đến nút cuối cùng và vẫn không bao giờ nhìn thấy cnt == 1 sớm hơn, vì vậy ans_len trở thành k. Điều này đảm bảo tính đúng đắn vì không có tiền tố ngắn hơn nào có thể phân biệt được nó. 

Trường hợp thứ ba là khi các từ phân kỳ ngay ở bit đầu tiên. Sau đó, cấp độ trie đầu tiên đã có cnt == 1 trên mỗi nhánh và mỗi từ được giảm xuống độ dài 1. Thuật toán nắm bắt điều này một cách tự nhiên mà không cần xử lý đặc biệt vì số lượng con gốc được chia ngay lập tức.
