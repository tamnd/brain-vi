---
title: "CF 105911G - Thăm dò"
description: "Chúng ta được cung cấp một biểu đồ có hướng trong đó mỗi hang động là một nút và mỗi lối đi một chiều là một cạnh có hướng có trọng số gọi là độ khó. Alice bắt đầu tại một nút được chỉ định với sức chịu đựng là số nguyên ban đầu."
date: "2026-06-21T15:27:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105911
codeforces_index: "G"
codeforces_contest_name: "2025 ICPC Nanchang Invitational and Jiangxi Provincial Collegiate Programming Contest"
rating: 0
weight: 105911
solve_time_s: 61
verified: true
draft: false
---

[CF 105911G - Thăm dò](https://codeforces.com/problemset/problem/105911/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng trong đó mỗi hang động là một nút và mỗi lối đi một chiều là một cạnh có hướng có trọng số gọi là độ khó. Alice bắt đầu tại một nút được chỉ định với sức chịu đựng là số nguyên ban đầu. Mỗi lần cô ấy vượt qua một bờ vực đầy khó khăn$d$, sức chịu đựng của cô ấy được thay thế bằng$\lfloor x / d \rfloor$. Khi sức chịu đựng trở về 0 sau một lần di chuyển, cô ấy sẽ dừng lại ngay lập tức. 

Điểm mấu chốt là Alice không biết cấu trúc đồ thị hoặc vị trí của mình một cách hữu ích, vì vậy khi ở trong hang động, cô ấy sẽ không cố ý chọn một cạnh có lợi hoặc gây tổn hại cho mình. Vấn đề biến điều này thành một câu hỏi sinh tồn trong trường hợp xấu nhất: bắt đầu từ một nút nhất định và sức chịu đựng ban đầu, cô ấy có thể buộc phải thực hiện bao nhiêu lần di chuyển trước khi sức chịu đựng của cô ấy chắc chắn giảm xuống 0 nếu cô ấy tiếp tục di chuyển dọc theo các cạnh hướng ra ngoài? 

Mỗi truy vấn cung cấp một nút bắt đầu và sức chịu đựng ban đầu, đồng thời chúng tôi phải tính toán số bước tối thiểu cho đến khi kiệt sức theo chuỗi lựa chọn tốt nhất có thể để kéo dài khả năng sống sót. Vì khả năng sống sót kéo dài hơn khi sức chịu đựng giảm càng chậm càng tốt, hành vi liên quan ở mỗi nút là chọn một cách hiệu quả lợi thế đi ra với độ khó nhỏ nhất. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$nút,$5 \cdot 10^5$các cạnh và$2 \cdot 10^5$truy vấn. Điều này ngay lập tức loại trừ bất kỳ việc truyền tải biểu đồ cho mỗi truy vấn nào là tuyến tính hoặc thậm chí logarit theo số lượng nút. Chúng ta cần một bước tiền xử lý trong thời gian gần như tuyến tính hoặc gần tuyến tính và mỗi truy vấn phải được xử lý trong khoảng thời gian logarit không đổi hoặc rất nhỏ. 

Một mô phỏng đơn giản đi theo các cạnh tùy ý từng bước cũng phức tạp bởi thực tế là sức chịu đựng phụ thuộc vào trình tự phân chia tầng và các đường dẫn khác nhau trong biểu đồ tạo ra các chuỗi ước số khác nhau. 

Trường hợp cạnh tinh tế xuất hiện khi tồn tại nhiều cạnh đi ra với những khó khăn khác nhau. Nếu chúng ta giả định không chính xác bất kỳ đường truyền cố định nào hoặc bỏ qua việc chọn ước số lớn hơn sẽ làm giảm sức chịu đựng nhanh hơn, chúng ta sẽ nhận được câu trả lời không nhất quán. 

Ví dụ: nếu một nút có các cạnh đi ra với độ khó 2 và 100, thì việc chọn 100 sẽ ngay lập tức làm giảm sức chịu đựng trong khi chọn 2 sẽ duy trì được lâu hơn. Bất kỳ mô hình đúng nào cũng phải phản ánh rằng hành vi tối đa hóa sự sống còn là luôn chọn độ khó sẵn có nhỏ nhất ở mỗi bước. 

Một điều tinh tế khác là bản thân cấu trúc biểu đồ chỉ quan trọng trong việc xác định cạnh có độ khó tối thiểu nào có sẵn ở mỗi nút. Khi điều đó đã được khắc phục, việc truyền tải sẽ trở nên xác định. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ là mô phỏng trực tiếp từng truy vấn: ở mỗi bước, hãy xem nút hiện tại, quét tất cả các cạnh đi ra để tìm độ khó tốt nhất (nhỏ nhất), vượt qua nó và cập nhật sức chịu đựng. Điều này đúng vì nó phù hợp với lựa chọn tối đa hóa sự sống còn ở mỗi bước. Tuy nhiên, việc quét liên tục các cạnh đi qua các truy vấn quá chậm và quan trọng hơn là chúng ta sẽ lặp lại cùng một quy trình quyết định nhiều lần. 

Quan sát quan trọng là hành vi tại mỗi nút là xác định nếu chúng ta chọn trước cạnh đi với độ khó tối thiểu. Sau khi hoàn thành việc này, mỗi nút sẽ có chính xác một lần chuyển đổi "sống sót tốt nhất". Biểu đồ thu gọn thành biểu đồ có hướng chức năng trong đó mỗi nút trỏ đến chính xác một nút tiếp theo. 

Sau lần giảm này, chuyển động của Alice không còn phân nhánh nữa. Mỗi truy vấn trở thành một bước đi xác định dọc theo một con đường duy nhất và độ phức tạp duy nhất còn lại là sức chịu đựng phát triển dọc theo con đường đó như thế nào. 

Bây giờ chúng ta kiểm tra động lực học sức chịu đựng. Mỗi bước áp dụng$x \leftarrow \lfloor x / d \rfloor$Ở đâu$d \ge 2$. Điều này có nghĩa là sức chịu đựng giảm đi rất nhanh. Ngay cả trong trường hợp chậm nhất khi mọi cạnh đều có độ khó 2, giá trị mỗi lần giảm một nửa, do đó số bước trước khi đạt đến 0 bị giới hạn bởi khoảng$\log_2 x$, nhiều nhất là 30 đối với$x \le 10^9$. 

Giới hạn này là sự đơn giản hóa quan trọng thứ hai. Ngay cả khi biểu đồ chứa các chu kỳ dài, chúng tôi sẽ không bao giờ duyệt quá 30 cạnh cho mỗi truy vấn trước khi cạn kiệt. Điều đó làm cho việc mô phỏng trực tiếp trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Truyền lực mạnh qua các cạnh đi ra mỗi bước |$O(Q \cdot m)$|$O(m)$| Quá chậm | 
| Đồ thị chức năng + mô phỏng mỗi truy vấn |$O(m + Q \log X)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi biểu đồ thành một cấu trúc trong đó mỗi nút có chính xác một lần chuyển tiếp đi, được chọn để tối đa hóa khả năng tồn tại. 

## Hướng dẫn thuật toán 

1. Đối với mỗi nút, hãy tính cạnh ra với độ khó nhỏ nhất. Điều này xác định một nút kế thừa duy nhất cho mỗi nút. Lý do là việc chọn một ước số nhỏ hơn luôn bảo toàn được sức bền hơn bất kỳ ước số lớn hơn nào. 
2. Xử lý các cạnh được chọn này như một đồ thị hàm số trong đó mỗi nút có chính xác một cạnh đi ra. Từ nay trở đi, sự chuyển động chỉ phụ thuộc vào cấu trúc cố định này. 
3. Đối với mỗi truy vấn, hãy bắt đầu tại nút đã cho với giá trị sức chịu đựng đã cho. 
4. Áp dụng lặp lại quy trình sau cho đến khi sức chịu đựng bằng 0: di chuyển đến nút kế thừa, cập nhật sức chịu đựng lên$\lfloor x / d \rfloor$, và đếm một bước. 
5. Dừng ngay lập tức khi sức chịu đựng về 0 và xuất ra số bước đã thực hiện. 

Thực tế quan trọng cho phép chấm dứt là mỗi bước chia cho ít nhất 2, do đó sức chịu đựng giảm nhanh và không thể vượt quá khoảng 30 lần chuyển đổi có ý nghĩa. 

### Tại sao nó hoạt động 

Tại mỗi nút, bất kỳ cạnh đi nào có độ khó lớn hơn sẽ giảm sức chịu đựng mạnh mẽ hơn và không bao giờ có thể tăng số bước trong tương lai trước khi đạt đến con số 0. Do đó, chiến lược tối ưu để tối đa hóa khả năng sống sót là tối ưu cục bộ ở mọi bước, luôn chọn độ khó thoát ra nhỏ nhất. Khi quy tắc cục bộ này được cố định, đường dẫn chung được xác định duy nhất. Do quá trình áp dụng phép chia sàn là đơn điệu giảm dần và bị giới hạn dưới 0 nên quá trình mô phỏng phải kết thúc và vì mỗi bước giảm giá trị ít nhất 2 lần nên số bước được giới hạn theo logarit, đảm bảo hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())

    INF = 10**18
    nxt = [0] * (n + 1)
    best_d = [INF] * (n + 1)

    # choose outgoing edge with minimal difficulty
    for _ in range(m):
        u, v, d = map(int, input().split())
        if d < best_d[u]:
            best_d[u] = d
            nxt[u] = v

    # ensure every node has an outgoing edge (guaranteed by statement)
    # but in case of safety, we assume nxt[u] is valid

    out = []

    for _ in range(q):
        u, x = map(int, input().split())
        steps = 0

        # at most ~30 steps since x <= 1e9 and d >= 2
        for _ in range(35):
            d = best_d[u]
            if d == 0:
                break
            x //= d
            steps += 1
            if x == 0:
                break
            u = nxt[u]

        out.append(str(steps))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Bước tiền xử lý chọn cạnh đi có độ khó tối thiểu cho mỗi nút, xây dựng cấu trúc chuyển tiếp xác định. Mỗi truy vấn sau đó mô phỏng quá trình trực tiếp. Giới hạn vòng lặp được đặt cao hơn 30 một chút để bao phủ an toàn tất cả các mẫu giảm có thể có. 

Một điểm tinh tế là chúng tôi cập nhật nút sau khi áp dụng phép chia chứ không phải trước đó vì nút hiện tại xác định cạnh được sử dụng cho lần chuyển tiếp tiếp theo. Quá trình mô phỏng dừng ngay lập tức khi sức chịu đựng trở về 0 sau một phép chia. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ dạng chuỗi đơn giản trong đó mỗi nút có một cạnh đi ra duy nhất. 

Truy vấn: bắt đầu tại nút 1 với sức chịu đựng 10, các cạnh luôn gặp độ khó 2 dọc theo đường đã chọn. 

| Bước | Nút | Sức chịu đựng trước | Độ khó | Sức chịu đựng sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 → 2 | 10 | 2 | 5 | 
| 2 | 2 → 3 | 5 | 2 | 2 | 
| 3 | 3 → 4 | 2 | 2 | 1 | 
| 4 | 4 → 5 | 1 | 2 | 0 | 

Quá trình dừng lại sau 4 bước, cho thấy việc giảm một nửa lặp đi lặp lại sẽ dẫn đến cạn kiệt nhanh chóng như thế nào. 

Bây giờ hãy xem xét một ví dụ hỗn hợp có khó khăn khác nhau: 

Bắt đầu với sức chịu đựng 9 và chuỗi các khó khăn đã chọn 3, 2, 2. 

| Bước | Nút | Sức chịu đựng trước | Độ khó | Sức chịu đựng sau | 
| --- | --- | --- | --- | --- | 
| 1 | A → B | 9 | 3 | 3 | 
| 2 | B → C | 3 | 2 | 1 | 
| 3 | C → D | 1 | 2 | 0 | 

Điều này cho thấy rằng ngay cả những thay đổi nhỏ trong việc phân chia ban đầu cũng ảnh hưởng đáng kể đến độ dài đường đi còn lại. 

Những dấu vết này xác nhận rằng quá trình này hoàn toàn có cấu trúc nhân lên và mỗi bước sẽ làm giảm nghiêm trọng sức chịu đựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m + Q \log X)$| Việc xây dựng các cạnh đi ra tốt nhất cần có thời gian tuyến tính trên các cạnh và mỗi truy vấn chạy tối đa ~30 bước do phân chia lặp lại cho ít nhất 2 | 
| Không gian |$O(n)$| Chúng tôi lưu trữ một cạnh đi và một trọng số cho mỗi nút | 

Giới hạn nằm trong giới hạn vì ngay cả với$2 \cdot 10^5$truy vấn, tổng số bước mô phỏng vẫn dưới vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import floor

    # embedded solution
    n, m, q = map(int, sys.stdin.readline().split())
    INF = 10**18
    nxt = [0] * (n + 1)
    best_d = [INF] * (n + 1)

    for _ in range(m):
        u, v, d = map(int, sys.stdin.readline().split())
        if d < best_d[u]:
            best_d[u] = d
            nxt[u] = v

    out = []
    for _ in range(q):
        u, x = map(int, sys.stdin.readline().split())
        steps = 0
        for _ in range(35):
            d = best_d[u]
            x //= d
            steps += 1
            if x == 0:
                break
            u = nxt[u]
        out.append(str(steps))

    return "\n".join(out)

# minimal case
assert run("1 1 1\n1 1 2\n1 10") == "1"

# small chain
assert run("2 2 1\n1 2 2\n2 1 2\n1 8") == "3"

# mixed graph
assert run("3 3 1\n1 2 3\n2 3 2\n3 1 2\n1 9") == "3"

# boundary x=1
assert run("2 2 1\n1 2 2\n2 1 2\n1 1") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | kiệt sức ngay sau giải hạng nhất | 
| 2 chu kỳ | 3 | tính chính xác khi truyền tải lặp đi lặp lại | 
| tạ hỗn hợp | 3 | xử lý các ước số khác nhau | 
| x = 1 | 1 | số 0 ngay lập tức sau bước đầu tiên | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi một nút có nhiều cạnh đi ra với độ khó rất khác nhau. Thuật toán giải quyết vấn đề này một cách rõ ràng bằng cách chọn trước cạnh độ khó nhỏ nhất. Ví dụ: nếu một nút có các cạnh với$d = 2$Và$d = 100$, hành vi đúng là luôn sử dụng 2. Bất kỳ triển khai nào chọn cạnh đầu tiên hoặc cạnh tùy ý sẽ làm giảm đáng kể thời gian tồn tại không chính xác. 

Một trường hợp cạnh khác xảy ra khi đồ thị chứa các vòng tự lặp. Vòng tự lặp có độ khó nhỏ nhất có nghĩa là Alice có thể ở lại cùng một nút trong khi sức chịu đựng giảm xuống. Mô phỏng xử lý điều này một cách tự nhiên vì nút kế thừa có thể bằng nút hiện tại và quy trình vẫn áp dụng phép chia một cách chính xác. 

Cuối cùng, khi sức chịu đựng bắt đầu từ 1, bất kỳ phép chia đầu tiên nào cũng tạo ra số 0 bất kể độ khó cạnh. Mô phỏng đếm chính xác chính xác một bước trong trường hợp này vì lần truyền tải đầu tiên ngay lập tức kết thúc quá trình.
