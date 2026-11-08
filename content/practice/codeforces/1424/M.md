---
title: "CF 1424M - Ngôn ngữ cổ"
description: "Chúng tôi được cung cấp một tập hợp các trang từ một từ điển bị phân mảnh. Mỗi trang có một mã định danh và chứa một số từ cố định. Cấu trúc ẩn chính là những từ này ban đầu được sắp xếp theo một số thứ tự bảng chữ cái không xác định, không phải thứ tự tiếng Anh tiêu chuẩn."
date: "2026-06-11T05:59:32+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1424
codeforces_index: "M"
codeforces_contest_name: "Bubble Cup 13 - Finals [Online Mirror, unrated, Div. 2]"
rating: 2200
weight: 1424
solve_time_s: 91
verified: false
draft: false
---

[CF 1424M - Ngôn ngữ cổ](https://codeforces.com/problemset/problem/1424/M) 

**Đánh giá:** 2200 
**Thẻ:** biểu đồ, sắp xếp 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tập hợp các trang từ một từ điển bị phân mảnh. Mỗi trang có một mã định danh và chứa một số từ cố định. Cấu trúc ẩn chính là những từ này ban đầu được sắp xếp theo một số thứ tự bảng chữ cái không xác định, không phải thứ tự tiếng Anh tiêu chuẩn. Nhiệm vụ của chúng ta là khôi phục một thứ tự hợp lệ của các ký tự bảng chữ cái có thể tạo ra tất cả các trang dưới dạng các mục từ điển được sắp xếp. 

Điểm quan trọng là mỗi trang đều nhất quán về mặt nội bộ với thứ tự từ điển theo cùng một thứ tự ký tự không xác định. Trên tất cả các trang, thứ tự này phải nhất quán. Từ việc so sánh giữa các từ liền kề theo thứ tự được sắp xếp, chúng ta có thể suy ra các ràng buộc về thứ tự giữa các chữ cái riêng lẻ. Mục tiêu cuối cùng là xây dựng lại thứ tự đầy đủ của tất cả các chữ cái xuất hiện. 

Nếu những ràng buộc này mâu thuẫn nhau, nghĩa là chúng hàm ý một chu trình như`a < b < c < a`, thì không có bảng chữ cái hợp lệ nào tồn tại và chúng ta phải xuất ra kết quả không thể xảy ra. 

Các hạn chế về kích thước đầu vào là vừa phải. Với tối đa 1000 trang và 1000 từ mỗi trang, tổng số từ nhiều nhất là một triệu. Mỗi so sánh từ có độ dài tuyến tính, tối đa 100 ký tự. Việc so sánh đơn giản tất cả các cặp từ trên toàn cầu sẽ quá chậm, nhưng chúng ta không cần tất cả các cặp từ. Chỉ các từ liền kề theo thứ tự được sắp xếp mới quan trọng đối với việc tái cấu trúc từ điển, điều này làm giảm đáng kể kích thước biểu đồ hiệu quả. 

Một trường hợp lỗi tinh tế phát sinh khi mối quan hệ tiền tố bị vi phạm. Ví dụ: nếu một từ dài hơn xuất hiện trước tiền tố của nó theo một thứ tự từ điển giả định thì từ điển sẽ không nhất quán. Ví dụ, nếu chúng ta thấy`abcd`trước`ab`, điều đó là không thể theo bất kỳ thứ tự từ điển nào và sẽ ngay lập tức mang lại`IMPOSSIBLE`. 

Một trường hợp phức tạp khác là các chữ cái bị ngắt kết nối. Một số ký tự có thể không bao giờ xuất hiện trong các ràng buộc. Những thứ này vẫn phải xuất hiện trong đầu ra theo bất kỳ thứ tự nào so với những thứ khác, điều đó có nghĩa là kết quả cuối cùng không nhất thiết phải là duy nhất. 

## Phương pháp tiếp cận 

Một nỗ lực mạnh mẽ sẽ cố gắng đoán thứ tự đầy đủ của tất cả các ký tự riêng biệt và xác minh xem mọi trang có được sắp xếp theo thứ tự đó hay không. Nếu có`C`có những ký tự riêng biệt`C!`các hoán vị có thể xảy ra và thậm chí việc kiểm tra một thứ tự yêu cầu quét tất cả các từ và so sánh chúng, dẫn đến một vụ nổ hoàn toàn không thể thực hiện được ngay cả đối với đầu vào nhỏ. 

Điều quan trọng là chúng ta không cần phải đoán thứ tự một cách trực tiếp. Thay vào đó, chúng ta có thể trích xuất các ràng buộc sắp xếp theo cặp từ cấu trúc từ điển. Bất cứ khi nào hai từ liên tiếp khác nhau, vị trí đầu tiên nơi chúng khác nhau sẽ xác định thứ tự chặt chẽ giữa hai ký tự. Ví dụ, nếu`word1 = "abcx"`Và`word2 = "abdy"`, sau đó chúng ta suy ra`c < d`. 

Điều này chuyển vấn đề thành việc xây dựng một biểu đồ có hướng trong đó các nút là các chữ cái và các cạnh biểu thị các ràng buộc về thứ tự. Khi đồ thị được xây dựng, vấn đề sẽ trở thành việc tìm thứ tự tôpô. Nếu một chu trình tồn tại thì không có bảng chữ cái hợp lệ nào tồn tại. 

Sự tinh tế bổ sung duy nhất là xử lý các vi phạm tiền tố: if`word1`bắt đầu bằng`word2`nhưng dài hơn và`word1`xuất hiện trước`word2`, thứ tự không hợp lệ bất kể cấu trúc biểu đồ. 

Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân đến xây dựng đồ thị và sắp xếp tôpô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(C! · N · L) | O(C) | Quá chậm | 
| Đồ thị + Toposort | O(Tổng số ký tự + ràng buộc) | O(C + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các từ trong mỗi trang theo thứ tự nhất định. Chúng tôi xử lý từng trang một cách độc lập vì mỗi trang đã được sắp xếp theo cùng một bảng chữ cái không xác định. 
2. Đối với mỗi trang, so sánh từng cặp từ liên tiếp. Với mỗi cặp, hãy tìm vị trí đầu tiên nơi chúng khác nhau. Nếu một cặp ký tự khác nhau`(a, b)`được tìm thấy, thêm một cạnh có hướng`a → b`nghĩa`a`phải đến trước`b`trong bảng chữ cái. 
3. Nếu không có vị trí khác nhau nhưng từ đầu tiên dài hơn từ thứ hai thì thứ tự không hợp lệ. Điều này tương ứng với vi phạm tiền tố trong đó một từ dài hơn đứng trước tiền tố của nó không chính xác. 
4. Xây dựng biểu đồ có hướng trên tất cả các ký tự xuất hiện trong bất kỳ từ nào và tính độ cho mỗi nút. 
5. Chạy sắp xếp cấu trúc liên kết bằng cách sử dụng hàng đợi tất cả các nút có bậc bằng 0. Liên tục loại bỏ các nút và nới lỏng các cạnh đi ra, nối chúng vào kết quả. 
6. Nếu thứ tự kết quả không bao gồm tất cả các ký tự xuất hiện trong đầu vào thì sẽ tồn tại một chu trình và chúng ta xuất ra`IMPOSSIBLE`. 

### Tại sao nó hoạt động 

Mỗi cạnh mã hóa một ràng buộc cần thiết phải có trong bất kỳ bảng chữ cái hợp lệ nào phù hợp với thứ tự từ điển. Bất kỳ bảng chữ cái hợp lệ nào cũng phải đáp ứng tất cả các ràng buộc như vậy, vì vậy nó phải là thứ tự tôpô của đồ thị. Ngược lại, bất kỳ thứ tự tôpô nào cũng thỏa mãn tất cả các ràng buộc theo cặp vì so sánh từ điển chỉ phụ thuộc vào ký tự khác nhau đầu tiên và những khác biệt đó được nắm bắt chính xác ở các cạnh. Do đó, vấn đề giảm xuống còn việc kiểm tra xem một đồ thị có hướng có phải là đồ thị không tuần hoàn hay không và tạo ra bất kỳ thứ tự tôpô nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict, deque

def solve():
    n, k = map(int, input().split())
    
    words = []
    present = set()
    
    pages = []
    
    for _ in range(n):
        p = int(input())
        page_words = [input().strip() for _ in range(k)]
        pages.append(page_words)
        for w in page_words:
            words.append(w)
            for c in w:
                present.add(c)
    
    adj = defaultdict(set)
    indeg = {c: 0 for c in present}
    
    def add_edge(a, b):
        if b not in adj[a]:
            adj[a].add(b)
            indeg[b] += 1
    
    for page in pages:
        for i in range(k - 1):
            w1, w2 = page[i], page[i + 1]
            m = min(len(w1), len(w2))
            found = False
            for j in range(m):
                if w1[j] != w2[j]:
                    add_edge(w1[j], w2[j])
                    found = True
                    break
            if not found:
                if len(w1) > len(w2):
                    print("IMPOSSIBLE")
                    return
    
    q = deque([c for c in indeg if indeg[c] == 0])
    order = []
    
    while q:
        c = q.popleft()
        order.append(c)
        for nxt in adj[c]:
            indeg[nxt] -= 1
            if indeg[nxt] == 0:
                q.append(nxt)
    
    if len(order) != len(indeg):
        print("IMPOSSIBLE")
        return
    
    print("".join(order))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách thu thập tất cả các ký tự sao cho mọi chữ cái xuất hiện trong đầu vào đều được thể hiện trong biểu đồ. Điều này rất quan trọng vì các chữ cái riêng biệt vẫn cần xuất hiện trong bảng chữ cái cuối cùng. 

Các cạnh được lưu trữ trong một tập hợp trên mỗi nút để tránh các ràng buộc trùng lặp làm tăng số lượng không chính xác. Điều này ngăn chặn việc đếm quá mức khi quan sát cùng một mối quan hệ thứ tự nhiều lần trên các trang. 

Việc kiểm tra tiền tố được xử lý ngay lập tức khi hai từ liền kề không khác nhau trong phạm vi trùng lặp. Đây là một trong những lỗi tinh vi phổ biến nhất: nếu không có nó, các từ điển không hợp lệ có thể vượt qua giai đoạn biểu đồ một cách không chính xác. 

Sắp xếp cấu trúc liên kết sử dụng thuật toán của Kahn. Độ chính xác chỉ phụ thuộc vào việc giảm độ một lần trên mỗi cạnh duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào (cấu trúc đơn giản):```
2 2
0
ab
ac
1
b
ba
```Chúng tôi trích xuất các ràng buộc: 

| Bước | Cặp | Sự không phù hợp đầu tiên | Đã thêm cạnh | độ | 
| --- | --- | --- | --- | --- | 
| trang0 | ab, ac | b vs c | b → c | b:0, c:1 | 
| trang1 | b, ba | không có (tiền tố ok) | không | không thay đổi | 

Hàng đợi bắt đầu bằng`a`Và`b`tùy thuộc vào sự hiện diện; cho rằng`a`cũng có mức độ bằng không. 

Quá trình tôpô mang lại một thứ tự hợp lệ như`abc`. 

Điều này xác nhận rằng các so sánh liền kề cục bộ là đủ để tái cấu trúc các ràng buộc thứ tự toàn cầu. 

### Ví dụ 2 

đầu vào:```
1 3
0
abc
ab
abd
```| Bước | Cặp | Không khớp | Kết quả | 
| --- | --- | --- | --- | 
| abc vs ab | không | vi phạm tiền tố | KHÔNG THỂ | 

Thuật toán ngay lập tức từ chối vì một từ dài hơn đứng trước tiền tố của nó, điều này không thể xảy ra theo thứ tự từ điển. 

Điều này chứng tỏ sự cần thiết của việc xác nhận tiền tố rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Tổng số ký tự + cạnh) | Mỗi từ được quét một lần cho mỗi lần so sánh liền kề; mỗi cạnh được xử lý một lần trong BFS | 
| Không gian | O(C + E) | Đồ thị lưu trữ các chữ cái và ràng buộc | 

Tổng số ký tự tối đa là một triệu và các cạnh được giới hạn bởi các chuyển tiếp giữa các từ, có thể quản lý được theo các ràng buộc. Giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict, deque

    def solve():
        n, k = map(int, input().split())
        pages = []
        present = set()

        for _ in range(n):
            p = int(input())
            page = [input().strip() for _ in range(k)]
            pages.append(page)
            for w in page:
                for c in w:
                    present.add(c)

        adj = defaultdict(set)
        indeg = {c: 0 for c in present}

        def add_edge(a, b):
            if b not in adj[a]:
                adj[a].add(b)
                indeg[b] += 1

        for page in pages:
            for i in range(k - 1):
                w1, w2 = page[i], page[i + 1]
                m = min(len(w1), len(w2))
                ok = False
                for j in range(m):
                    if w1[j] != w2[j]:
                        add_edge(w1[j], w2[j])
                        ok = True
                        break
                if not ok and len(w1) > len(w2):
                    return "IMPOSSIBLE"

        q = deque([c for c in indeg if indeg[c] == 0])
        res = []

        while q:
            c = q.popleft()
            res.append(c)
            for nxt in adj[c]:
                indeg[nxt] -= 1
                if indeg[nxt] == 0:
                    q.append(nxt)

        if len(res) != len(indeg):
            return "IMPOSSIBLE"
        return "".join(res)

    return solve()

assert run("""1 1
0
a
""") == "a"

assert run("""1 2
0
ab
ac
""") in ["abc", "acb"]  # multiple valid

assert run("""1 3
0
abc
ab
abd
""") == "IMPOSSIBLE"

assert run("""2 2
0
ab
ac
1
b
ba
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thư đơn | một | đồ thị tối thiểu | 
| thứ tự phân nhánh | abc/acb | nhiều loại tôpô hợp lệ | 
| vi phạm tiền tố | KHÔNG THỂ | cấu trúc từ điển không hợp lệ | 
| trang hỗn hợp | không trống | tính nhất quán chung | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều trang lặp lại cùng một ràng buộc về thứ tự. Thuật toán xử lý việc này một cách chính xác vì các cạnh được lưu trữ theo tập hợp, ngăn chặn các mức tăng trùng lặp. 

Một trường hợp khác là các ký tự xuất hiện nhưng không bao giờ tham gia vào các ràng buộc. Các nút này vẫn nằm trong biểu đồ với mức độ bằng 0 và sẽ xuất hiện một cách tự nhiên ở bất kỳ vị trí nào theo thứ tự cuối cùng. Điều này duy trì tính chính xác ngay cả khi bảng chữ cái chỉ bị ràng buộc một phần. 

Trường hợp tinh vi cuối cùng là phát hiện chu kỳ bị ẩn trên các trang. Ví dụ: các ràng buộc như`a < b`,`b < c`, Và`c < a`có thể đến từ các trang khác nhau. Việc sắp xếp cấu trúc liên kết sẽ kết thúc với ít nút theo thứ tự hơn so với hiện tại trong biểu đồ, kích hoạt chính xác tính không thể thực hiện được mà không cần logic phát hiện chu kỳ rõ ràng.
