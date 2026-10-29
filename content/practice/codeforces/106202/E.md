---
title: "CF 106202E - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u043f\u043e \u0434\u0435\u0440\u0435\u0432\u0443"
description: "Chúng ta có một tập hợp các cặp đỉnh trên một cây chưa xác định có n đỉnh. Mỗi cặp đại diện cho một hành trình từ đỉnh này sang đỉnh khác, nghĩa là nó tương ứng với đường đi đơn giản duy nhất giữa hai đỉnh đó sau khi cây được cố định."
date: "2026-06-19T18:27:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106202
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106202
solve_time_s: 59
verified: true
draft: false
---

[CF 106202E - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u043f\u043e \u0434\u0435\u0440\u0435\u0432\u0443](https://codeforces.com/problemset/problem/106202/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các cặp đỉnh trên một cây chưa xác định có n đỉnh. Mỗi cặp đại diện cho một hành trình từ đỉnh này sang đỉnh khác, nghĩa là nó tương ứng với đường đi đơn giản duy nhất giữa hai đỉnh đó sau khi cây được cố định. 

Chúng ta được phép chọn một tập con của các cặp này, nhưng bản thân cây không cố định. Chúng ta có thể tự do xây dựng bất kỳ cây nào trên n đỉnh. Tập hợp con được chọn phải thỏa mãn một ràng buộc trên cây được xây dựng đó: nếu chúng ta lấy tất cả các đường dẫn tương ứng với các cặp đã chọn thì không có cạnh nào của cây được phép xuất hiện ở nhiều hơn một trong các đường dẫn đó. 

Nhiệm vụ là tối đa hóa số lượng cặp mà chúng ta có thể giữ lại, với giả định rằng chúng ta cũng được phép thiết kế cây theo hướng có lợi cho mình. 

Đầu vào chỉ đơn giản là danh sách m cặp đỉnh. Đầu ra là kích thước tối đa của một tập hợp con của các cặp này có thể được thực hiện đồng thời trên một số cây trong khi vẫn tôn trọng ràng buộc sử dụng cạnh. 

Các ràng buộc lên tới 2 · 10^5 đỉnh và cặp, điều này ngay lập tức loại trừ mọi tìm kiếm tập hợp con theo cấp số nhân hoặc bất kỳ cách tiếp cận nào cố gắng suy luận rõ ràng về tất cả các cây có thể. Bất cứ điều gì vượt quá thời gian tuyến tính hoặc gần tuyến tính cho mỗi thao tác sẽ không thành công. 

Trường hợp cạnh tinh tế xuất hiện khi các cặp hình thành chu kỳ. Ví dụ: xét ba cặp (1,2), (2,3), (1,3). Nếu chúng ta coi chúng một cách ngây thơ như những kết nối độc lập, chúng ta có thể nghĩ rằng cả ba đều có thể được chọn. Tuy nhiên, trong bất kỳ cây nào, ít nhất một trong các đường dẫn tương ứng phải chồng lên các cạnh khác, do đó cả ba đường dẫn này không thể cùng tồn tại dưới ràng buộc. 

Điều này đã gợi ý rằng hạn chế thực sự không phải ở bản thân cấu trúc cây mà là cách các cặp được chọn tương tác tổ hợp với nhau. 

## Phương pháp tiếp cận 

Khó khăn chính là cây không cố định. Điều này cho phép chúng ta diễn giải lại vấn đề từ quan điểm cấu trúc: thay vì lý luận về các đường đi bên trong một cây chưa biết, chúng ta hỏi sự tồn tại của một cây như vậy áp đặt những hạn chế nào đối với các cặp đã chọn. 

Một cách tiếp cận bạo lực trực tiếp sẽ là thử tất cả các tập hợp con của các cặp và, đối với mỗi tập hợp con, cố gắng xây dựng một cây và định tuyến tất cả các đường dẫn tương ứng sao cho không có cạnh nào được chia sẻ. Ngay cả khi chúng ta bỏ qua việc xây dựng cây, chỉ cần kiểm tra tất cả các tập hợp con cũng đã tốn O(2^m), điều này là không thể đối với m lên tới 2 · 10^5. Ngay cả việc lựa chọn tập hợp con tham lam liên tục cố gắng kiểm tra tính khả thi vẫn sẽ yêu cầu các cuộc kiểm tra tính nhất quán toàn cầu quá tốn kém. 

Quan sát quan trọng là chúng ta có thể đơn giản hóa vai trò của đường dẫn. Vì chúng ta được phép thiết kế cây nên chúng ta không bị buộc phải nhúng một con đường như một thứ gì đó dài và gò bó. Nếu hai đỉnh được kết nối trong cấu trúc đã chọn, chúng ta luôn có thể đặt chúng liền kề nhau trên cây trừ khi điều đó tạo ra xung đột cấu trúc với các cặp được chọn khác. 

Điều này có nghĩa là chúng ta có thể suy nghĩ một cách trừu tượng hơn: mỗi cặp được chọn hoạt động giống như một cạnh mà chúng ta muốn đưa vào cây. Sau đó, yêu cầu về cây áp đặt một điều kiện toàn cục duy nhất: tập hợp được chọn phải không theo chu kỳ, bởi vì bất kỳ chu trình nào trong các kết nối được chọn sẽ dẫn đến mâu thuẫn với yêu cầu cấu trúc cuối cùng là một cây. 

Một khi đạt được quan điểm này, vấn đề sẽ trở thành một vấn đề cổ điển: chúng ta đang chọn tập hợp con lớn nhất có thể có của các cạnh từ một tập hợp nhất định sao cho chúng tạo thành một khu rừng. Đây chính xác là tập hợp con không theo chu kỳ tối đa, có thể được tìm thấy bằng cách sử dụng cấu trúc hợp tập hợp rời rạc. 

Chúng tôi xử lý từng cặp một và chỉ giữ từng cặp nếu nó kết nối hai thành phần đã ngắt kết nối trước đó. Điều này đảm bảo rằng chúng tôi không bao giờ giới thiệu một chu kỳ và đảm bảo chúng tôi lấy càng nhiều cặp càng tốt.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các tập hợp con và công trình cây | O(2^m) | O(n) | Quá chậm | 
| Xây dựng rừng tối đa dựa trên DSU | O(m α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành xây dựng một tập hợp con chu kỳ có kích thước tối đa của các cặp đã cho. 

1. Khởi tạo cấu trúc hợp tập hợp rời rạc với mỗi đỉnh trong thành phần riêng của nó. Điều này thể hiện rằng ban đầu không có kết nối bắt buộc nào tồn tại. 
2. Lặp lại tất cả các cặp đã cho (a_i, b_i). Với mỗi cặp, hãy kiểm tra xem a_i và b_i đã có trong cùng một thành phần chưa. 
3. Nếu chúng nằm trong các thành phần khác nhau, hãy đưa cặp này vào câu trả lời của chúng tôi và hợp nhất hai thành phần. Điều này đảm bảo chúng tôi đang mở rộng khu rừng mà không tạo ra chu kỳ. 
4. Nếu chúng đã ở trong cùng một thành phần, hãy bỏ qua cặp này vì việc thêm nó sẽ tạo ra một chu trình, chu trình này không thể là một phần của bất kỳ lựa chọn hợp lệ nào. 
5. Số cặp được chấp nhận cuối cùng chính là đáp án. 

Lý do khiến sự lựa chọn tham lam này có tác dụng là vì mỗi khi chúng ta từ chối một cạnh, đó là vì nó sẽ đóng một chu kỳ đã được thực thi bởi các cặp được chấp nhận trước đó. Bất kỳ giải pháp nào bao gồm cạnh này đều phải loại trừ ít nhất một cạnh trong chu trình đó, do đó việc từ chối nó không thể làm giảm câu trả lời tối ưu. 

### Tại sao nó hoạt động 

Ở mỗi bước, các cặp được chấp nhận sẽ tạo thành một khu rừng. Bất kỳ giải pháp hợp lệ nào cũng phải là một khu rừng, bởi vì một chu trình gồm các cặp được chọn sẽ buộc ít nhất một cạnh của cây được sử dụng lại theo hai đường dẫn khác nhau, vi phạm ràng buộc. Quá trình DSU xây dựng một khu rừng tối đa và bất kỳ khu rừng tối đa nào cũng có kích thước n trừ đi số lượng thành phần được kết nối, đây là số cạnh lớn nhất có thể mà không cần hình thành chu kỳ. Vì mọi lựa chọn hợp lệ đều tương ứng với một số khu rừng nên cách xây dựng này là tối ưu. 

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
            return False
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]
        return True

def main():
    n, m = map(int, input().split())
    dsu = DSU(n)

    ans = 0
    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        if dsu.union(a, b):
            ans += 1

    print(ans)

if __name__ == "__main__":
    main()
```Cấu trúc DSU duy trì các thành phần kết nối của các đỉnh được tạo ra bởi các cặp đã được chọn. Mỗi sự kết hợp thành công tương ứng với việc thêm một cặp một cách an toàn mà không tạo thành một chu trình. Câu trả lời đơn giản là số lượng công đoàn thành công. 

Một cạm bẫy phổ biến là quên rằng chúng ta không cần phải xây dựng cây một cách rõ ràng. DSU đã ngầm đảm bảo rằng các cặp được chọn có thể được nhúng vào một số cây vì bất kỳ khu rừng nào cũng có thể được mở rộng thành cây bao trùm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 3
1 3
4 5
6 7
```Chúng ta bắt đầu với bảy đỉnh cô lập. Mỗi cặp kết nối hai thành phần khác nhau nên tất cả đều được chấp nhận. 

| Bước | Cặp | Cùng một thành phần? | Hành động | Số lượng được chấp nhận | 
| --- | --- | --- | --- | --- | 
| 1 | (1,3) | Không | công đoàn | 1 | 
| 2 | (4,5) | Không | công đoàn | 2 | 
| 3 | (6,7) | Không | công đoàn | 3 | 

Câu trả lời cuối cùng là 3. Điều này cho thấy các cặp hoàn toàn rời rạc không bao giờ gây trở ngại cho nhau. 

### Ví dụ 2 

đầu vào:```
3 3
1 2
2 3
1 3
```Chúng tôi xử lý từng bước. 

| Bước | Cặp | Cùng một thành phần? | Hành động | Số lượng được chấp nhận | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | Không | công đoàn | 1 | 
| 2 | (2,3) | Không | công đoàn | 2 | 
| 3 | (1,3) | Có | bỏ qua | 2 | 

Cặp cuối cùng bị từ chối vì nó đóng một chu kỳ được hình thành bởi hai cặp đầu tiên. Bất kỳ nỗ lực nào nhằm bao gồm cả ba đều nhất thiết sẽ tạo ra một chu trình không tương thích với bất kỳ cách thực hiện cây hợp lệ nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m α(n)) | Mỗi hoạt động liên kết/tìm gần như không đổi do nén đường dẫn | 
| Không gian | O(n) | Mảng DSU cho cha mẹ và kích thước | 

Thuật toán dễ dàng phù hợp với các giới hạn cho n, m lên tới 2 · 10^5, vì nó chỉ thực hiện quét tuyến tính với chi phí khấu hao Ackermann nghịch đảo. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

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
                return False
            if self.size[a] < self.size[b]:
                a, b = b, a
            self.parent[b] = a
            self.size[a] += self.size[b]
            return True

    n, m = map(int, input().split())
    dsu = DSU(n)
    ans = 0
    for _ in range(m):
        a, b = map(int, input().split())
        if dsu.union(a - 1, b - 1):
            ans += 1
    return str(ans)

# provided samples
assert run("7 3\n1 3\n4 5\n6 7\n") == "3"
assert run("3 3\n1 2\n2 3\n1 3\n") == "2"

# custom cases
assert run("2 1\n1 2\n") == "1", "single edge"
assert run("4 5\n1 2\n2 3\n3 4\n4 1\n1 3\n") == "3", "cycle restriction"
assert run("5 4\n1 2\n1 3\n1 4\n1 5\n") == "4", "star graph"
assert run("6 0\n") == "0", "empty edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 1 | trường hợp tối thiểu | 
| hạn chế chu kỳ | 3 | hành vi từ chối chu kỳ | 
| đồ thị sao | 4 | nhiều cạnh từ một trung tâm | 
| cạnh trống | 0 | trường hợp ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các cặp tạo thành một chu trình. Ví dụ, trong một tam giác, mỗi cạnh đều có giá trị riêng lẻ, nhưng cả ba cạnh đều không có giá trị. DSU xử lý việc này chính xác như sau: hai cạnh đầu tiên hợp nhất các thành phần và cạnh thứ ba tìm thấy cả hai điểm cuối đã được kết nối nên nó bị từ chối. Điều này đảm bảo câu trả lời cuối cùng tôn trọng ràng buộc chu kỳ mà không bao giờ phát hiện rõ ràng các chu kỳ. 

Một trường hợp khác là khi đồ thị đã là một cây. Trong trường hợp đó, mọi cạnh đều được chấp nhận, vì không có phép hợp nào tìm được hai đỉnh trong cùng một thành phần. Thuật toán trả về m một cách tự nhiên, phù hợp với thực tế là một cây có chính xác m = n - 1 cạnh và hoàn toàn không có chu kỳ. 

Trường hợp suy biến xảy ra khi không có cặp nào được đưa ra. DSU không bao giờ thực hiện kết hợp và kết quả bằng 0, điều này phản ánh chính xác rằng không thể chọn đường dẫn nào.
