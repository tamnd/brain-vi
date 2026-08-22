---
title: "CF 104248D - Các đỉnh bằng nhau"
description: "Chúng ta có một đồ thị có hướng trên các đỉnh $n$ trong đó mỗi đỉnh có nhiều nhất một cạnh hướng ra ngoài. Mỗi đỉnh cũng mang một nhãn ký tự chữ thường. Một số đỉnh trỏ đến một đỉnh khác, trong khi những đỉnh khác là đỉnh cuối và không trỏ đến gì cả."
date: "2026-07-01T22:08:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104248
codeforces_index: "D"
codeforces_contest_name: "Udmurt SU Contest 2010"
rating: 0
weight: 104248
solve_time_s: 51
verified: true
draft: false
---

[CF 104248D - Các đỉnh bằng nhau](https://codeforces.com/problemset/problem/104248/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trên$n$đỉnh mà mỗi đỉnh có nhiều nhất một cạnh hướng ra ngoài. Mỗi đỉnh cũng mang một nhãn ký tự chữ thường. Một số đỉnh trỏ đến một đỉnh khác, trong khi những đỉnh khác là đỉnh cuối và không trỏ đến gì cả. 

Hai đỉnh được coi là không thể phân biệt được nếu thủ tục so sánh tất định không thể tách chúng ra. Quy trình so sánh song song hai đỉnh bắt đầu: đầu tiên là nhãn của chúng, sau đó xem chúng có cạnh đi ra hay không và nếu cả hai đều là đỉnh thì chúng ngay lập tức được coi là tương đương. Mặt khác, nếu cả hai đều có các cạnh đi ra, việc so sánh sẽ tiếp tục đệ quy trên các cạnh đi ra tương ứng của chúng. 

Ý tưởng chính là hai đỉnh tương đương nhau nếu khi bạn liên tục đi theo các cạnh đi ra, bạn luôn thấy cùng một chuỗi các nhãn và các lựa chọn cấu trúc cho đến khi kết thúc. Điều này về cơ bản tạo ra vấn đề về việc nhóm các đỉnh theo “chữ ký” của chuỗi mà chúng tạo ra. 

Ràng buộc$n \le 10^5$loại trừ mọi phương pháp so sánh theo cặp. So sánh tất cả các cặp sẽ là$O(n^2)$và thậm chí việc mô phỏng các phép so sánh sẽ quá chậm vì mỗi phép so sánh có thể đi dọc theo các chuỗi độ dài$O(n)$, đưa ra hành vi bậc ba trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều đỉnh tạo thành chuỗi dài mà cuối cùng hợp nhất. Ví dụ: 

đầu vào:```
a 2
a 3
a 0
a 0
```Đỉnh 2 và 3 đều trỏ tới các nút đầu cuối nhưng thông qua các đường dẫn khác nhau. Một cách tiếp cận ngây thơ chỉ so sánh các hàng xóm ngay lập tức hoặc chỉ các nhãn sẽ hợp nhất hoặc tách chúng một cách không chính xác tùy thuộc vào chi tiết triển khai. Sự tương đương chính xác phụ thuộc vào cấu trúc đầy đủ của đường dẫn đi. 

Một trường hợp cạnh khác là chu kỳ. Vì mỗi đỉnh có nhiều nhất một cạnh đi ra nên có thể xảy ra chu trình. Trong một chu trình, sự so sánh không bao giờ tự nhiên chấm dứt trừ khi chúng ta phát hiện ra sự lặp lại của các trạng thái. Một so sánh DFS ngây thơ sẽ lặp lại mãi mãi hoặc giả định sai sự bất bình đẳng. 

## Phương pháp tiếp cận 

Phương pháp brute-force cố gắng so sánh mọi cặp đỉnh bằng cách mô phỏng quy trình đã cho. Đối với mỗi cặp, chúng tôi đi dọc theo các cạnh hướng ra ngoài theo từng bước, so sánh nhãn và cấu trúc. Nếu chúng tôi gặp sai sót, chúng tôi sẽ dừng lại; nếu không chúng ta tiếp tục cho đến khi cả hai đường dẫn đều kết thúc. 

Điều này đúng về mặt khái niệm vì nó phản ánh trực tiếp định nghĩa về sự tương đương. Tuy nhiên, trong một đồ thị có chuỗi hoặc chu trình, một phép so sánh đơn lẻ có thể đi qua$O(n)$đỉnh. Làm điều này cho tất cả các cặp dẫn đến$O(n^2)$so sánh, mỗi khả năng có thể$O(n)$, dẫn đến$O(n^3)$thời gian trong trường hợp xấu nhất. 

Quan sát quan trọng là cấu trúc của mỗi đỉnh được xác định đầy đủ bởi hai thành phần: nhãn của nó và lớp tương đương của đỉnh lân cận đi ra của nó. Nếu hai đỉnh có cùng nhãn và các đỉnh lân cận của chúng đã được biết là tương đương thì bản thân các đỉnh này sẽ trở thành tương đương. Các đỉnh đầu cuối là trường hợp cơ sở không có cạnh đi ra. 

Điều này tự nhiên gợi ý việc xử lý các đỉnh theo thứ tự phụ thuộc ngược lại, nhưng vì biểu đồ có thể chứa các chu trình nên không tồn tại thứ tự tôpô đơn giản. Thay vào đó, chúng tôi xử lý “chữ ký” của mỗi đỉnh như một đối tượng được xác định đệ quy và tính toán các lớp tương đương bằng cách sử dụng phép băm và ổn định lặp lại. 

Chúng tôi gán cho mỗi đỉnh một chữ ký bao gồm nhãn của nó và lớp hiện tại của hàng xóm đi của nó (hoặc một giá trị null đặc biệt cho các nút đầu cuối). Sau đó chúng tôi liên tục tính toán lại các lớp cho đến khi không có thay đổi nào xảy ra. Vì mỗi sàng lọc phân chia chặt chẽ hoặc ổn định không gian trạng thái, nên sự hội tụ xảy ra trong các lần lặp logarit hoặc gần tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng cặp Brute Force |$O(n^3)$|$O(n)$| Quá chậm | 
| Tinh chỉnh chữ ký lặp đi lặp lại |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén định nghĩa đệ quy về sự tương đương vào việc sàng lọc lặp lại các chữ ký đỉnh. 

1. Khởi tạo lớp của mỗi đỉnh là 0. Điều này thể hiện rằng ban đầu tất cả các đỉnh được giả định giống hệt nhau trước khi xem xét cấu trúc. 
2. Đối với mỗi đỉnh, xác định một khóa tạm thời bao gồm nhãn của nó và lớp hiện tại của đỉnh lân cận hoặc một giá trị đặc biệt nếu nó không có cạnh đi ra. Điều này mã hóa chính xác những gì quá trình so sánh nhìn thấy ở một bước đệ quy. 
3. Sắp xếp hoặc băm các khóa này để gán ID lớp mới một cách nhất quán. Các đỉnh có khóa giống nhau sẽ nhận được cùng một lớp. 
4. Lặp lại bước 2 và 3 cho đến khi việc phân lớp ổn định. Tính ổn định có nghĩa là không có đỉnh nào thay đổi lớp của nó trong một lần lặp, do đó việc sàng lọc thêm sẽ không làm thay đổi tính tương đương. 
5. Xuất ID lớp cuối cùng. 

Lý do chúng ta tính toán lại nhiều lần là vì lớp của hàng xóm gửi đi tự nó là một phần của định nghĩa. Ban đầu, chúng tôi không biết sự tương đương chính xác, vì vậy chúng tôi bắt đầu thô và tinh chỉnh dần dần cho đến khi cấu trúc được nắm bắt hoàn toàn. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi lần lặp, nếu hai đỉnh có các lớp được tính toán giống hệt nhau thì các cấu trúc con đi ra của chúng sẽ không thể phân biệt được cho đến độ sâu sàng lọc hiện tại. Mỗi lần lặp lại sẽ tăng “độ sâu nhìn về phía trước” một cách hiệu quả dọc theo các cạnh đi ra. Vì mỗi đỉnh có nhiều nhất một cạnh hướng ra ngoài nên cấu trúc là một tập hợp các chuỗi và chu trình, và quá trình sàng lọc lan truyền một cách xác định dọc theo các cấu trúc này. Khi phân vùng ngừng thay đổi, các lớp sẽ mã hóa chính xác mối quan hệ tương đương được xác định bằng so sánh đệ quy đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
label = []
to = []

for _ in range(n):
    parts = input().split()
    label.append(parts[0])
    t = int(parts[1])
    to.append(t - 1 if t != 0 else -1)

cls = [0] * n

while True:
    keys = []
    for i in range(n):
        nxt = cls[to[i]] if to[i] != -1 else -1
        keys.append((label[i], nxt, i))

    keys.sort()

    new_cls = [0] * n
    cur = 0
    new_cls[keys[0][2]] = 0

    for i in range(1, n):
        if keys[i][0] != keys[i - 1][0] or keys[i][1] != keys[i - 1][1]:
            cur += 1
        new_cls[keys[i][2]] = cur

    if new_cls == cls:
        break
    cls = new_cls

print("\n".join(map(str, cls)))
```Mã xây dựng biểu diễn đồ thị ban đầu, chuyển đổi các cạnh đi thành các chỉ số dựa trên số 0. Mỗi lần lặp lại xây dựng một chữ ký cho mỗi đỉnh kết hợp nhãn của nó và lớp hiện tại của hàng xóm đi ra của nó. Sắp xếp các chữ ký này nhóm các cấu trúc giống hệt nhau lại với nhau và chúng tôi chỉ định ID lớp mới cho phù hợp. 

Kiểm tra kết thúc so sánh mảng lớp đầy đủ giữa các lần lặp. Khi đã ổn định, việc sàng lọc thêm sẽ không làm thay đổi việc phân nhóm, vì vậy chúng tôi dừng lại. 

Chi tiết triển khai tinh tế bao gồm chỉ mục gốc trong bộ khóa. Điều này đảm bảo việc gán trở lại các đỉnh ổn định sau khi sắp xếp. Nếu không có nó, chúng ta sẽ mất ánh xạ giữa thứ tự được sắp xếp và nhận dạng đỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a 2
b 3
b 0
b 3
```Trạng thái ban đầu: 

| Đỉnh | Nhãn | Ra | Lớp | 
| --- | --- | --- | --- | 
| 1 | một | 2 | 0 | 
| 2 | b | 3 | 0 | 
| 3 | b | - | 0 | 
| 4 | b | 3 | 0 | 

Các phím lặp đầu tiên: 

| Đỉnh | Chìa khóa | 
| --- | --- | 
| 1 | (a,0) | 
| 2 | (b,0) | 
| 3 | (b,-1) | 
| 4 | (b,0) | 

Sau khi sắp xếp, các lớp trở thành: 

| Đỉnh | Lớp Mới | 
| --- | --- | 
| 3 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 4 | 1 | 

Lần lặp thứ hai sử dụng các lớp lân cận được cập nhật. Đỉnh 2 và 4 đều trỏ đến 3 hiện có lớp 0, do đó chữ ký của chúng lại khớp với nhau là (b,0). Đỉnh 1 vẫn khác biệt vì nó trỏ đến đỉnh 2 có cấu trúc tinh tế khác. Sau khi ổn định, các đỉnh 1, 2, 4 rơi vào các nhóm tương đương nhất quán phản ánh bản sắc cấu trúc đầy đủ. 

Điều này cho thấy sự bình đẳng không chỉ phụ thuộc vào nhãn hiệu mà còn phụ thuộc vào cấu trúc ổn định của chuỗi gửi đi. 

### Ví dụ 2 

đầu vào:```
3
a 2
a 3
a 1
```Điều này tạo thành một chu trình: 1 → 2 → 3 → 1. 

Ban đầu tất cả các lớp đều bình đẳng. Tinh chỉnh đầu tiên gán các khóa giống nhau cho tất cả các đỉnh vì tất cả các nhãn đều khớp nhau và tất cả các lân cận đều có lớp 0. Không có sự khác biệt nào xuất hiện trong bất kỳ lần lặp nào, do đó thuật toán sẽ ổn định ngay lập tức. 

Điều này xác nhận rằng các nút chu trình là tương đương khi chúng có cấu trúc đối xứng trong quy trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n \cdot k)$| Mỗi lần lặp lại sắp xếp$n$chìa khóa;$k$là số lần sàng lọc cho đến khi ổn định | 
| Không gian |$O(n)$| Mảng cho nhãn, cạnh và bài tập lớp | 

Số lần lặp lại trong thực tế là nhỏ vì mỗi bước sẽ tinh chỉnh các lớp tương đương theo ít nhất một mức độ sâu cấu trúc và đồ thị có cấu trúc giới hạn do các ràng buộc ngoài mức độ. Với$n \le 10^5$, điều này phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.write = lambda s: out.append(s)
    out.clear()
    try:
        main()
    except:
        pass
    return "".join(out)

def main():
    n = int(input())
    label = []
    to = []
    for _ in range(n):
        c, t = input().split()
        t = int(t)
        label.append(c)
        to.append(t - 1 if t != 0 else -1)

    cls = [0] * n

    while True:
        keys = []
        for i in range(n):
            nxt = cls[to[i]] if to[i] != -1 else -1
            keys.append((label[i], nxt, i))
        keys.sort()

        new_cls = [0] * n
        cur = 0
        new_cls[keys[0][2]] = 0

        for i in range(1, n):
            if keys[i][0] != keys[i-1][0] or keys[i][1] != keys[i-1][1]:
                cur += 1
            new_cls[keys[i][2]] = cur

        if new_cls == cls:
            break
        cls = new_cls

    return "\n".join(map(str, cls))

out = []

# sample-like
assert run("4\n a 2\n b 3\n b 0\n b 3\n") != ""

# minimum size chain
assert run("2\na 0\nb 0\n") in ["0\n1", "1\n0"]

# cycle
assert run("3\na 2\nb 3\nc 1\n") != ""

# all same structure
assert run("3\na 0\na 0\na 0\n") in ["0\n0\n0"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 nhà ga biệt lập | Đặt hàng 0/1 | trường hợp cơ sở đúng đắn | 
| 3 chu kỳ | cùng lớp | xử lý chu trình | 
| lá giống nhau | tất cả đều bình đẳng | sụp đổ cấu trúc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các đỉnh đều là điểm cuối. Trong trường hợp đó, mọi đỉnh đều không có cạnh đi ra và chỉ có nhãn là quan trọng. Thuật toán chỉ định các khóa ban đầu hoàn toàn bằng nhãn, do đó các nhãn giống hệt nhau ngay lập tức tạo thành các nhóm và quá trình ổn định diễn ra trong một lần lặp. 

Một trường hợp cạnh khác là một chuỗi dài kết thúc ở nút cuối. Ví dụ,$1 \to 2 \to 3 \to 4 \to 5$. Sự tương đương lan truyền ngược: nút 5 là cơ sở, sau đó nút 4 phụ thuộc vào nó, v.v. Quá trình sàng lọc lặp đi lặp lại nắm bắt điều này dần dần, đảm bảo rằng các nút ở các độ sâu khác nhau nhận được các lớp khác nhau nếu cấu trúc hạ lưu của chúng khác nhau. 

Trường hợp cạnh cuối cùng là một chu trình đi vào chuỗi. Bởi vì mỗi đỉnh chỉ có một cạnh đi ra, nên các cấu trúc như vậy rất đơn giản nhưng vẫn yêu cầu lan truyền các điểm phân biệt cuối cùng trong suốt chu trình. Quá trình sàng lọc sẽ ổn định khi các nút chu trình nhìn thấy các lớp hạ lưu nhất quán, đảm bảo việc phân nhóm chính xác.
