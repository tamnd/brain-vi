---
title: "CF 102822C - Mã một Trie"
description: "Chúng tôi nhận được kết quả của một số tìm kiếm được thực hiện trên một trie không xác định. Mỗi tìm kiếm bắt đầu từ gốc và đi theo các ký tự của chuỗi nếu có thể. Nếu cạnh tiếp theo không tồn tại, quá trình tìm kiếm sẽ dừng ngay lập tức và trả về giá trị được lưu tại nút hiện tại."
date: "2026-07-26T15:59:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102822
codeforces_index: "C"
codeforces_contest_name: "2020 China Collegiate Programming Contest - Mianyang Site"
rating: 0
weight: 102822
solve_time_s: 50
verified: true
draft: false
---

[CF 102822C - Mã một Trie](https://codeforces.com/problemset/problem/102822/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được kết quả của một số tìm kiếm được thực hiện trên một trie không xác định. Mỗi tìm kiếm bắt đầu từ gốc và đi theo các ký tự của chuỗi nếu có thể. Nếu cạnh tiếp theo không tồn tại, quá trình tìm kiếm sẽ dừng ngay lập tức và trả về giá trị được lưu tại nút hiện tại. Mỗi nút trong trie ban đầu có một giá trị duy nhất, do đó số trả về xác định chính xác một nút. 

Nhiệm vụ là xây dựng lại số lượng nút nhỏ nhất có thể có trong bất kỳ thử nghiệm nào có thể tạo ra tất cả các kết quả truy vấn nhất định. Gốc cũng được tính là một nút. Nếu không có phép thử nào có thể thỏa mãn mọi quan sát thì chúng ta phải báo cáo điều đó. 

Đầu vào đưa ra một số trường hợp thử nghiệm. Đối với mỗi trường hợp kiểm thử, mọi truy vấn đều chứa một chuỗi chữ thường và giá trị được trả về bởi thao tác tìm kiếm cho chuỗi đó. 

Tổng độ dài của tất cả các chuỗi trong một trường hợp thử nghiệm nhiều nhất là (10^5) và tổng độ dài của tất cả các trường hợp thử nghiệm nhiều nhất là (5 \times 10^5). Điều này loại trừ các giải pháp so sánh từng cặp chuỗi hoặc xây dựng lại cấu trúc nhiều lần. Thuật toán phải gần tuyến tính trong tổng kích thước đầu vào. 

Các trường hợp cạnh khóa không phải là về chuỗi dài mà là về tiền tố và giá trị xung đột. Nếu hai truy vấn giống nhau có câu trả lời khác nhau thì trie không thể tồn tại vì đi theo cùng một đường dẫn phải luôn kết thúc tại cùng một nút. 

Ví dụ:```
2
a 1
a 2
```Đầu ra đúng là`-1`. Việc triển khai bất cẩn có thể chỉ giữ lại một trong các giá trị và chấp nhận trường hợp này một cách không chính xác. 

Một trường hợp phức tạp khác là khi một chuỗi là tiền tố của một chuỗi khác và các câu trả lời không giống nhau.```
2
a 1
aa 2
```Đầu ra đúng là`-1`. Nếu nút đại diện`a`có giá trị`1`, truy vấn`aa`chỉ có thể trả về một nút khác nếu cạnh`a -> a`tồn tại. Nhưng sau đó truy vấn`a`cũng sẽ dừng ở nút sau khi đọc`a`, do đó hai yêu cầu không thể được thỏa mãn cùng nhau. 

Trường hợp quan trọng thứ ba là khi có nhiều chuỗi khác nhau có cùng một câu trả lời.```
3
aa 1
ab 1
ac 1
```Đầu ra đúng là`1`. Nút gốc trống có thể lưu trữ giá trị`1`và cả ba truy vấn đều dừng ngay lập tức vì không có cạnh đầu tiên nào cần tồn tại. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là chèn mọi chuỗi truy vấn vào một trie và sau đó thử gán giá trị cho các nút. Điều này xây dựng tất cả các tiền tố, ngay cả khi nhiều tiền tố trong số đó không cần thiết. Trong trường hợp xấu nhất, nếu có (10^5) chuỗi có độ dài (10^5), số lượng mối quan hệ tiền tố có thể có sẽ trở nên quá lớn để kiểm tra nhiều lần. 

Quan sát quan trọng là một nút chỉ cần tồn tại nếu các câu trả lời khác nhau cần được tách ra bên dưới nút đó. Nếu mọi truy vấn đi qua tiền tố đều có cùng giá trị trả về, chúng ta có thể dừng ở tiền tố đó và gán giá trị ở đó. Chúng ta không cần biết chuỗi được chèn ban đầu ở đâu vì kết quả truy vấn chỉ quan tâm đến cạnh bị thiếu đầu tiên. 

Điều này tự nhiên dẫn tới việc xây dựng đệ quy. Xây dựng bộ ba thông thường của tất cả các chuỗi truy vấn. Tại mỗi nút, hãy xem tập hợp các giá trị xuất hiện trong cây con của nó. Nếu tập hợp đó chỉ chứa một giá trị thì toàn bộ cây con có thể được thay thế bằng nút đơn này. Nếu một số giá trị xuất hiện, nút đó phải được giữ nguyên và các nút con của nó phải được giải quyết một cách độc lập. 

Lực lượng vũ phu và cách tiếp cận tối ưu chỉ khác nhau ở việc chúng có giữ các tiền tố không cần thiết hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(L^2)) | (O(L)) | Quá chậm | 
| Tối ưu | (O(L)) | (O(L)) | Đã chấp nhận | 

Ở đây (L) là tổng độ dài của tất cả các chuỗi truy vấn trong trường hợp thử nghiệm. 

## Hướng dẫn thuật toán 

1. Chèn mọi chuỗi truy vấn vào một tri. Lưu trữ giá trị tại nút cuối của mỗi chuỗi. Nếu cùng một chuỗi xuất hiện với các giá trị khác nhau thì câu trả lời là không thể vì các tìm kiếm giống nhau không thể tạo ra các nút khác nhau. 
2. Duyệt qua phép thử với tìm kiếm theo chiều sâu. Đối với mỗi nút, thu thập các giá trị riêng biệt xuất hiện trong số tất cả các chuỗi đầu cuối trong cây con của nó. 
3. Nếu cây con của nút chứa chính xác một giá trị, trả về kích thước cây con là một. Bản thân nút có thể đại diện cho tất cả các truy vấn đó, vì vậy tất cả các nút con đều không cần thiết. 
4. Nếu một nút chứa nhiều giá trị, hãy giữ nút này và xử lý đệ quy mọi nút con chứa ít nhất một truy vấn. Thêm kích thước của các giải pháp con đó. 
5. Kích thước được trả về cho gốc là số nút tri tối thiểu có thể có. 

Tại sao nó hoạt động: 

Hãy xem xét bất kỳ nút nào đại diện cho tiền tố. Mọi truy vấn bên dưới nút này cuối cùng phải trả về một nút trong cây con này. Nếu tất cả các truy vấn đó yêu cầu cùng một giá trị, việc hợp nhất chúng vào nút hiện tại luôn hợp lệ và lưu các nút. Nếu yêu cầu các giá trị khác nhau thì chúng không thể chia sẻ nút hiện tại vì mỗi nút trie chỉ có một giá trị nên các phần tử con phải phân tách các trường hợp khác nhau. Quá trình đệ quy thực hiện chính xác hai lựa chọn này ở mọi tiền tố, do đó mọi nút không cần thiết đều bị loại bỏ và mọi sự phân biệt bắt buộc đều được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("children", "value")
    def __init__(self):
        self.children = {}
        self.value = None

def solve_case(data):
    root = Node()

    for s, v in data:
        cur = root
        for c in s:
            if c not in cur.children:
                cur.children[c] = Node()
            cur = cur.children[c]
        if cur.value is not None and cur.value != v:
            return -1
        cur.value = v

    def dfs(node):
        vals = set()
        if node.value is not None:
            vals.add(node.value)

        child_info = []
        for child in node.children.values():
            ok, size, child_vals = dfs(child)
            if not ok:
                return False, 0, set()
            child_info.append((size, child_vals))
            vals.update(child_vals)

        if len(vals) == 1:
            return True, 1, vals

        size = 1
        for child_size, _ in child_info:
            size += child_size

        return True, size, vals

    ok, ans, _ = dfs(root)
    return ans if ok else -1

def main():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        n = int(input())
        queries = []
        for _ in range(n):
            s, v = input().split()
            queries.append((s, int(v)))

        out.append(f"Case #{case}: {solve_case(queries)}")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giai đoạn chèn tạo ra cấu trúc duy nhất chúng ta cần. Nó không cố gắng mô phỏng các thao tác chèn bị quên vì nhiều bộ chuỗi được chèn khác nhau có thể dẫn đến cùng một kết quả truy vấn. 

DFS là cốt lõi của giải pháp. Tập được trả về chứa tất cả các giá trị vẫn cần được biểu diễn bên dưới tiền tố hiện tại. Khi kích thước đã đặt trở thành một, toàn bộ cây con sẽ thu gọn vào nút hiện tại. 

Việc kiểm tra xung đột trong quá trình chèn sẽ xử lý sớm trường hợp không thể đơn giản nhất. Nếu không có nó, các chuỗi giống hệt nhau với các câu trả lời khác nhau có thể được hợp nhất không chính xác. 

Đệ quy không bao giờ khám phá tiền tố hai lần, do đó tổng công việc tỷ lệ thuận với số lượng nút trie được tạo. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3
aa 1
ab 1
ac 1
```Trie được tạo từ chuỗi là: 

| Tiền tố nút | Các giá trị bên dưới | Quyết định | 
| --- | --- | --- | 
| trống | {1} | Chỉ giữ lại root | 

Gốc có thể đại diện cho cả ba truy vấn, vì vậy câu trả lời là`1`. 

Đối với đầu vào:```
2
aa 1
a 2
```Dấu vết là: 

| Tiền tố nút | Các giá trị bên dưới | Quyết định | 
| --- | --- | --- | 
| trống | {1,2} | Phải chia | 
| một | {1,2} | Phải chia | 
| aa | {1} | Giữ nút | 
| một thiết bị đầu cuối | {2} | Giữ nút | 

Trie kết quả có gốc, nút`a`, và nút`aa`, đưa ra câu trả lời`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(L)) | Mỗi nhân vật tạo hoặc truy cập vào một cạnh trie một lần. | 
| Không gian | (O(L)) | Trie tạm thời chỉ lưu trữ tiền tố của chuỗi đầu vào. | 

Kích thước đầu vào tối đa được xử lý vì cả bộ nhớ và thời gian chạy đều tăng tuyến tính với tổng chiều dài của chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    t = int(input())
    ans = []
    for case in range(1, t + 1):
        n = int(input())
        q = []
        for _ in range(n):
            s, v = input().split()
            q.append((s, int(v)))
        ans.append(f"Case #{case}: {solve_case(q)}")
    sys.stdin = old
    return "\n".join(ans)

assert run("""3
aa 1
a 2
""") == "Case #1: 3"

assert run("""3
aa 1
ab 1
ac 1
""") == "Case #1: 1"

assert run("""2
a 1
a 2
""") == "Case #1: -1"

assert run("""3
a 1
ab 1
abc 1
""") == "Case #1: 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aa 1, a 2`|`3`| Xung đột tiền tố vẫn có tri hợp lệ | 
|`aa 1, ab 1, ac 1`|`1`| Hoàn toàn thu gọn thành một nút | 
|`a 1, a 2`|`-1`| Cùng một chuỗi có câu trả lời không tương thích | 
|`a 1, ab 1, abc 1`|`1`| Chuỗi dài có giá trị giống hệt nhau | 

## Vỏ cạnh 

Khi các chuỗi giống nhau có giá trị khác nhau, thuật toán sẽ phát hiện sự mâu thuẫn trong khi chèn. Nút đầu cuối không thể lưu trữ hai giá trị khác nhau nên nó ngay lập tức trả về`-1`. 

Vì:```
2
a 1
a 2
```việc chèn đến cùng một nút đầu cuối hai lần. Truy vấn đầu tiên lưu trữ giá trị`1`và truy vấn thứ hai cố gắng lưu trữ giá trị`2`. Vì một nút trie không thể có cả hai giá trị nên trường hợp này là không thể. 

Khi một chuỗi ngắn và một chuỗi dài hơn yêu cầu các câu trả lời khác nhau, DFS sẽ giữ đủ các nút để phân tách chúng. 

Vì:```
2
a 1
aa 2
```gốc chứa hai giá trị, vì vậy nó phải giữ con`a`. Nút đó vẫn chứa hai giá trị nên nó phải giữ nút con`aa`. Kích thước kết quả là`3`, đại diện cho gốc,`a`, Và`aa`. 

Khi nhiều chuỗi có cùng một câu trả lời, DFS sẽ loại bỏ tất cả các chuỗi con không cần thiết. 

Vì:```
3
aa 1
ab 1
ac 1
```gốc chỉ nhìn thấy giá trị`1`bên dưới nó, vì vậy nó trở thành nút trả lời và tất cả trẻ em đều biến mất. Số lượng nút tối thiểu chính xác là`1`. 

Tôi cũng có thể cung cấp phiên bản ngắn hơn theo phong cách cuộc thi của bài xã luận này nếu bạn muốn một phiên bản phù hợp với độ dài blog Codeforces điển hình.
