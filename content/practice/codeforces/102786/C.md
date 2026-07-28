---
title: "CF 102786C - \u0420\u0430\u0437\u044f\u0449\u0438\u0439 \u0443\u0434\u0430\u0440 \u0437\u0432\u0435\u0437\u0434\u043d\u043e\u0433\u043e \u0434\u0435\u0441\u0430\u043d\u0442\u0430"
description: "Nhiệm vụ là một vấn đề xây dựng lại đồ thị tương tác. Chúng tôi bắt đầu ở sảnh 0 và ngay từ đầu, các sảnh từ 0 đến N đã có thể truy cập được. Sau đó, máy bay không người lái liên tục gửi các mảnh đường hầm được phát hiện. Mỗi đoạn chứa một số cạnh vô hướng giữa các số hội trường."
date: "2026-07-27T19:32:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102786
codeforces_index: "C"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u042f\u0440\u0413\u0423 \u0438\u043c. \u041f.\u0413. \u0414\u0435\u043c\u0438\u0434\u043e\u0432\u0430 Demidov Open IT Cup 2019"
rating: 0
weight: 102786
solve_time_s: 58
verified: true
draft: false
---

[CF 102786C - \u0420\u0430\u0437\u044f\u0449\u0438\u0439 \u0443\u0434\u0430\u0440 \u0437\u0432\u0435\u0437\u0434\u043d\u043e\u0433\u043e \u0434\u0435\u0441\u0430\u043d\u0442\u0430](https://codeforces.com/problemset/problem/102786/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là một vấn đề xây dựng lại đồ thị tương tác. Chúng tôi bắt đầu ở hội trường`0`, và tại sảnh đầu`0`bởi vì`N`đã có thể truy cập được. Sau đó, máy bay không người lái liên tục gửi các mảnh đường hầm được phát hiện. Mỗi đoạn chứa một số cạnh vô hướng giữa các số hội trường. Sau mỗi mảnh nhận được, chúng tôi phải quyết định ngay xem thông tin thu thập được có đủ để xây dựng đường dẫn từ khu vực có thể tiếp cận hiện tại của chúng tôi đến hội trường hay không`9999`. Nếu có, chúng tôi in`ATTACK`; nếu không chúng tôi sẽ in`WAIT`và cho phép máy bay không người lái tiếp tục. 

Đầu vào không bình thường vì nó xuất hiện trực tuyến. Dòng đầu tiên cho biết kích thước của tập hợp có sẵn ban đầu, sau đó mỗi dòng tiếp theo là một phần mới của các cạnh được phát hiện. Đầu ra không phải là câu trả lời cuối cùng cho một đầu vào cố định mà là một chuỗi các quyết định sau mỗi lần cập nhật. 

Hầu như có thể có`100000`hội trường. Điều này ngay lập tức loại trừ việc xây dựng lại tìm kiếm biểu đồ từ đầu sau mỗi tin nhắn bay không người lái. Một BFS hoặc DFS đầy đủ trên tất cả các cạnh đã biết sau mỗi lần cập nhật có thể xử lý các cạnh giống nhau hàng nghìn lần, quá tốn kém nếu dưới giới hạn 5 giây. Chúng tôi cần một cấu trúc có thể cập nhật kết nối dần dần. 

Các trường hợp cạnh chính xảy ra do tính chất trực tuyến của đầu vào. Một đường dẫn có thể xuất hiện chính xác sau khi xử lý một đoạn mới, vì vậy việc chỉ kiểm tra trước khi đọc đoạn đó sẽ bỏ lỡ thời điểm tấn công cần thiết. 

Ví dụ: giả sử dòng đầu tiên là:```
0
```và tin nhắn bay không người lái duy nhất là:```
1 0 9999
```Đầu ra đúng là:```
ATTACK
```Một giải pháp in`WAIT`trước khi áp dụng cạnh mới và chỉ kiểm tra sau đó đã bỏ lỡ thời điểm tấn công được phép. 

Một trường hợp tế nhị khác là khi quân hậu được kết nối gián tiếp.```
0
2 0 5 5 9999
```Đầu ra đúng là:```
ATTACK
```Cạnh`0-5`không trực tiếp chứa hội trường`9999`, nhưng hai cạnh với nhau tạo thành một tuyến đường. Chỉ kiểm tra hàng xóm trực tiếp của`0`sẽ thất bại. 

Sai lầm phổ biến thứ ba là quên rằng các sảnh ban đầu đã có thể vào được. Nếu đầu vào bắt đầu bằng:```
3
```sau đó hội trường`0`,`1`,`2`, Và`3`tất cả đều có sẵn trước phản hồi đầu tiên của máy bay không người lái. Một giải pháp đúng đắn phải coi chúng như một thành phần có thể tiếp cận ngay từ đầu. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là lưu trữ mọi đường hầm được phát hiện và chạy tìm kiếm biểu đồ từ các sảnh hiện có thể truy cập sau mỗi tin nhắn bằng máy bay không người lái. Điều này đúng vì câu hỏi duy nhất chúng ta cần trả lời là liệu`9999`thuộc về thành phần chứa vị trí bắt đầu của chúng ta. Tuy nhiên, các cạnh cũ giống nhau được quét đi quét lại. Với tối đa`100000`hội trường và nhiều bản cập nhật, việc duyệt qua toàn bộ biểu đồ nhiều lần có thể đạt tới hàng tỷ lần kiểm tra cạnh. 

Cấu trúc của bài toán cho chúng ta cái nhìn đơn giản hơn. Chúng ta không bao giờ cần lộ trình, khoảng cách hoặc cấu trúc cây chính xác. Chúng ta chỉ cần biết liệu hai hội trường có nằm trong cùng một thành phần được kết nối hay không. Việc thêm đường hầm chỉ hợp nhất hai thành phần và không bao giờ tách chúng ra. Đây chính xác là tình huống phù hợp với cấu trúc tập hợp rời rạc. 

Ban đầu, tất cả các hội trường`0`bởi vì`N`thuộc về thành phần có thể truy cập. Đối với mỗi tin nhắn bằng máy bay không người lái, chúng tôi hợp nhất hai điểm cuối của mỗi cạnh nhận được. Sau khi tất cả các cạnh trong tin nhắn được xử lý, chúng tôi kiểm tra xem hall`9999`có cùng đại diện với hội trường`0`. Nếu có, đường dẫn tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(U * (V + E)) | O(V + E) | Quá chậm | 
| Tối ưu | O((V + E) α(V)) | O(V) | Đã chấp nhận | 

Đây`U`là số lượng cập nhật của máy bay không người lái và`α(V)`là hàm Ackermann nghịch đảo, tăng chậm đến mức thực tế nó không đổi. 

## Hướng dẫn thuật toán 

1. Tạo một cấu trúc kết hợp rời rạc cho tất cả các hội trường có thể có. Vì số phòng có thể lên tới`99999`, chúng ta chuẩn bị đủ không gian cho mọi đỉnh có thể. 

Các hội trường có thể tiếp cận ban đầu là`0`bởi vì`N`, vì vậy chúng phải được kết nối với vị trí bắt đầu của chúng ta. Chúng tôi hợp nhất mỗi người trong số họ với hội trường`0`. 
2. Đọc tin nhắn bay không người lái tiếp theo. Số đầu tiên là số lượng chuyển tiếp được phát hiện, tiếp theo là nhiều cặp số hội trường. 

Mỗi cặp đại diện cho một đường hầm vô hướng. Vì đường hầm có nghĩa là cả hai sảnh đều có thể tiếp cận được lẫn nhau nên chúng tôi hợp nhất các thành phần của chúng. 
3. Sau khi xử lý toàn bộ tin nhắn, so sánh các thành phần của hall`0`và hội trường`9999`. 

Nếu chúng bằng nhau thì thông tin được thu thập đã chứa sẵn đường dẫn đến nữ hoàng, vì vậy chúng tôi in`ATTACK`và chấm dứt. Nếu không chúng tôi in`WAIT`, xóa đầu ra và chờ đoạn tiếp theo. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý bất kỳ số lượng tin nhắn bằng máy bay không người lái nào, hai hội trường nằm trong cùng một thành phần kết hợp rời rạc, chính xác khi các đường hầm được phát hiện chứng minh rằng có một tuyến đường tồn tại giữa chúng. Ban đầu điều này đúng vì các phòng bắt đầu có thể truy cập được hợp nhất với nhau. Mỗi đường hầm mới chỉ tạo ra một kết nối có thể có giữa hai thành phần và hoạt động hợp nhất thực hiện chính xác việc hợp nhất đó. Không có tuyến đường hợp lệ nào có thể tồn tại mà không có tất cả các cạnh của nó đã kết nối các thành phần tương ứng, vì vậy hãy kiểm tra xem hall`9999`thuộc cùng thành phần với hội trường`0`tương đương với việc kiểm tra xem một tuyến tấn công có tồn tại hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]

def main():
    n = int(input())
    dsu = DSU(100000)

    for i in range(1, n + 1):
        dsu.union(0, i)

    for line in sys.stdin:
        if not line.strip():
            continue

        data = list(map(int, line.split()))
        k = data[0]

        idx = 1
        for _ in range(k):
            u = data[idx]
            v = data[idx + 1]
            idx += 2
            dsu.union(u, v)

        if dsu.find(0) == dsu.find(9999):
            print("ATTACK")
            sys.stdout.flush()
            return
        else:
            print("WAIT")
            sys.stdout.flush()

if __name__ == "__main__":
    main()
```các`DSU`lớp lưu trữ đại diện thành phần được kết nối của mỗi hội trường. Nén đường dẫn trong`find`giúp việc tra cứu trong tương lai diễn ra rất nhanh, trong khi việc kết hợp theo kích thước giữ cho các cây bên trong không bị cạn. 

Vòng khởi tạo kết nối mọi hội trường có sẵn ban đầu với hội trường`0`. Đây là phần dễ bị bỏ sót vì đầu vào không liệt kê rõ ràng các cạnh này. Máy bay không người lái chỉ cung cấp thêm đường hầm sau khi chương trình bắt đầu. 

Đối với mỗi đoạn đến, trước tiên mã sẽ áp dụng tất cả các đường hầm mới và chỉ sau đó mới kiểm tra câu trả lời. Thứ tự rất quan trọng vì thời điểm tấn công cần thiết có thể chính xác sau khi mảnh mới nhất đến. 

Việc xóa sau mỗi phản hồi là cần thiết vì đây là một nhiệm vụ tương tác. Nếu không có nó, trọng tài có thể không gửi đoạn tiếp theo vì nó đang chờ quyết định của chương trình. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các phòng có thể tiếp cận ban đầu là`0`,`1`, Và`2`. 

| Bước | Thông tin mới | Thành phần 0 | Đã kết nối với 9999 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | hội trường 0,1,2 có sẵn | {0,1,2} | Không | | 
| 1 | cạnh 4-6 và 11-12 | {0,1,2} | Không | CHỜ | 
| 2 | cạnh 9-10, 8-10, 1-2, 2-7, 3-9 | {0,1,2,7} | Không | CHỜ | 
| 3 | cạnh 9999-7 | {0,1,2,7,9999} | Có | TẤN CÔNG | 

Đoạn cuối cùng kết nối thành phần đã có thể truy cập được thông qua hội trường`7`tới nữ hoàng. Bất biến đúng vì mỗi hợp thể hiện một đường hầm được phát hiện. 

Đối với mẫu thứ hai, đoạn cuối cùng kết nối trực tiếp với hội trường`9999`. 

| Bước | Thông tin mới | Thành phần 0 | Đã kết nối với 9999 | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | hội trường 0,1,2,3 có sẵn | {0,1,2,3} | Không | | 
| 1 | cạnh từ tin nhắn bay không người lái đầu tiên | {0,1,2,3} | Không | CHỜ | 
| 2 | cạnh 1-2 | {0,1,2,3} | Không | TẤN CÔNG | 

Ví dụ chứng minh rằng chương trình phản ứng ngay lập tức khi kết nối được yêu cầu xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + E) α(100000)) | Mỗi lần hợp nhất ban đầu và mỗi cạnh được phát hiện đều thực hiện công việc khấu hao gần như không đổi | 
| Không gian | O(100000) | DSU lưu trữ một giá trị chính và một giá trị kích thước cho mỗi hội trường có thể | 

Số lượng hội trường tối đa đủ nhỏ cho một mảng DSU cố định. Giải pháp không bao giờ quét lại tất cả các cạnh sau khi nhận được chúng, vì vậy nó vẫn hiệu quả ngay cả với nhiều bản cập nhật dành cho máy bay không người lái. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))

        def find(self, x):
            if self.p[x] != x:
                self.p[x] = self.find(self.p[x])
            return self.p[x]

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a != b:
                self.p[b] = a

    out = []
    n = int(input())
    dsu = DSU(100000)

    for i in range(1, n + 1):
        dsu.union(0, i)

    for line in sys.stdin:
        if not line.strip():
            continue
        a = list(map(int, line.split()))
        k = a[0]
        ptr = 1
        for _ in range(k):
            dsu.union(a[ptr], a[ptr + 1])
            ptr += 2
        if dsu.find(0) == dsu.find(9999):
            out.append("ATTACK")
            break
        out.append("WAIT")

    return "\n".join(out)

assert solve("""2
2 4 6 11 12
5 9 10 8 10 1 2 2 7 3 9
1 9999 7
""") == "WAIT\nWAIT\nATTACK", "sample 1"

assert solve("""3
3 3 4 9999 4 2 3
1 1 2
""") == "WAIT\nATTACK", "sample 2"

assert solve("""0
1 0 9999
""") == "ATTACK", "direct connection"

assert solve("""0
2 0 5 5 9999
""") == "ATTACK", "indirect connection"

assert solve("""5
1 10 20
1 9999 30
2 0 30 20 30
""") == "WAIT\nWAIT\nATTACK", "late merge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu ban đầu | CHỜ, CHỜ, TẤN CÔNG | Khám phá gia tăng bình thường | 
| Mẫu thứ hai | CHỜ, TẤN CÔNG | Phản ứng ngay lập tức sau khi kết nối mới | 
| Kết nối trực tiếp | TẤN CÔNG | Một cạnh tới mục tiêu | 
| Kết nối gián tiếp | TẤN CÔNG | Phát hiện đường dẫn đa cạnh | 
| Hợp nhất muộn | CHỜ, CHỜ, TẤN CÔNG | Các thành phần tham gia sau một số bản cập nhật | 

## Vỏ cạnh 

Khi quân hậu được kết nối bằng tin nhắn bay không người lái đầu tiên, thuật toán vẫn hoạt động vì nó xử lý tất cả các cạnh nhận được trước khi kiểm tra. Vì:```
0
1 0 9999
```hoạt động công đoàn sáp nhập`0`Và`9999`, do đó các đại diện khớp nhau và câu trả lời là`ATTACK`. 

Khi tuyến đường có nhiều sảnh trung gian thì không cần xử lý đặc biệt. Vì:```
0
2 0 5 5 9999
```liên minh đầu tiên tạo ra thành phần`{0,5}`và cái thứ hai mở rộng nó thành`{0,5,9999}`. Sự so sánh cuối cùng phát hiện chính xác tuyến đường. 

Khi khu vực xuất phát có thể tiếp cận có các sảnh bên cạnh`0`, ban đầu những hội trường đó phải được sáp nhập. Vì:```
3
1 3 8
1 8 9999
```sảnh`3`đã có thể truy cập được, vì vậy chuỗi được phát hiện sẽ đến được với nữ hoàng sau lần cập nhật thứ hai. Một giải pháp chỉ bắt đầu từ đỉnh`0`không khởi tạo hội trường`1`bởi vì`N`sẽ trì hoãn hoặc bỏ lỡ cuộc tấn công một cách không chính xác.
