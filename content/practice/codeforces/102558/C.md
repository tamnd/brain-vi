---
title: "CF 102558C - \u041f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0441\u0442 \u043d\u0430 \u043f\u043b\u044f\u0436\u0435"
description: "Chúng tôi có một bộ sưu tập ghế bãi biển và mỗi chiếc ghế đều có một con số mô tả các đặc điểm bên ngoài của nó. Nhiệm vụ là chọn hai chiếc ghế khác nhau có số lượng có giá trị XOR nhỏ nhất có thể."
date: "2026-08-03T19:05:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102558
codeforces_index: "C"
codeforces_contest_name: "Contest for Yandex interns 2019"
rating: 0
weight: 102558
solve_time_s: 386
verified: false
draft: false
---

[CF 102558C - \u041f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0441\u0442 \u043d\u0430 \u043f\u043b\u044f\u0436\u0435](https://codeforces.com/problemset/problem/102558/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập ghế bãi biển và mỗi chiếc ghế đều có một con số mô tả các đặc điểm bên ngoài của nó. Nhiệm vụ là chọn hai chiếc ghế khác nhau có số lượng có giá trị XOR nhỏ nhất có thể. XOR đo lường mức độ khác nhau của hai số ở dạng nhị phân, vì vậy mục tiêu là tìm ra cặp số khác nhau ít nhất. 

Đầu vào chứa một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, giá trị đầu tiên là số lượng ghế, theo sau là các giá trị đặc tính được gán cho những chiếc ghế đó. Đầu ra của mỗi trường hợp thử nghiệm là giá trị XOR tối thiểu có thể thu được bằng cách so sánh hai chiếc ghế riêng biệt bất kỳ. 

Tổng số giá trị trên tất cả các trường hợp thử nghiệm có thể đạt tới\(10^6\). Điều này ngay lập tức loại trừ việc kiểm tra từng cặp vì\(n^2\)hoạt động sẽ trở nên xung quanh\(10^{12}\), vượt xa giới hạn chương trình cạnh tranh thông thường cho phép. Chúng ta cần một giải pháp gần tuyến tính hoặc\(n \log C\), Ở đâu\(C\)là giá trị lớn nhất có thể có của một số. Vì mọi giá trị lớn nhất là\(10^9\), chỉ có khoảng 30 chữ số nhị phân là quan trọng. 

Một số trường hợp rất dễ xử lý sai. Nếu hai chiếc ghế có giá trị giống hệt nhau thì câu trả lời là 0 vì XOR của chúng bằng 0. Ví dụ, đầu vào```
1
3
7 7 12
```có đầu ra```
0
```Một giải pháp chỉ kiểm tra các giá trị lân cận sau khi sắp xếp mà không xử lý các giá trị trùng lặp một cách chính xác có thể bỏ sót điều này. 

Một trường hợp khác là khi cặp nhỏ nhất không được hình thành bởi hai giá trị gần nhất về mặt số lượng. Ví dụ,```
1
4
1 2 8 16
```có đầu ra```
3
```Cặp tốt nhất là 1 và 2. Sự khác biệt về số của chúng là nhỏ, nhưng XOR là số liệu thực tế, vì vậy chỉ lý luận bằng phép trừ là không đủ. 

Trường hợp cạnh thứ ba xuất hiện khi giá trị lớn và sử dụng bit cao. Ví dụ,```
1
2
0 1000000000
```có đầu ra```
1000000000
```Việc triển khai thử nhị phân kiểm tra quá ít bit sẽ không thành công vì các giá trị gần\(10^9\)yêu cầu 30 bit. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là so sánh mọi cặp ghế có thể. Với mỗi cặp chỉ số\(i\)Và\(j\), chúng tôi tính toán\(a_i \oplus a_j\)và giữ kết quả nhỏ nhất. Cách tiếp cận này đúng vì nó thực sự xem xét mọi ứng viên có thể trả lời. Vấn đề là số lượng cặp. Với\(n = 10^6\), số lượng so sánh xấp xỉ\(5 \times 10^{11}\), không thể hoàn thành kịp thời. 

Cấu trúc của XOR cho chúng ta một hướng đi tốt hơn. Để giảm thiểu XOR với một số cố định, chúng ta muốn số kia có càng nhiều bit giống nhau càng tốt, đặc biệt là các bit cao nhất. Sự khác biệt ở bit cao đóng góp nhiều hơn vào giá trị XOR cuối cùng so với sự khác biệt ở bit thấp hơn. 

Trie nhị phân lưu trữ các số theo bit của chúng. Bắt đầu từ bit quan trọng nhất, chúng ta có thể duyệt qua bộ ba và luôn ưu tiên nhánh chứa bit giống với số hiện tại. Nếu một nhánh như vậy tồn tại, bit đó sẽ đóng góp 0 vào XOR. Chỉ khi thiếu nhánh phù hợp, chúng ta mới phải lấy bit đối diện, điều này sẽ thêm giá trị của bit đó vào câu trả lời. 

Phương pháp vũ phu hoạt động hiệu quả vì nó kiểm tra mọi đối tác có thể có, nhưng không thành công vì lặp lại quá nhiều công việc. Trie lưu trữ thông tin cần thiết để đưa ra lựa chọn tốt nhất cho từng số một cách độc lập, giảm việc tìm kiếm từ tất cả các cặp xuống còn một lần duyệt qua khoảng 30 cấp độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Trie nhị phân | O(30n) | O(30n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một bản thử nhị phân trống. Mỗi nút đại diện cho một tiền tố biểu diễn nhị phân của các số được chèn. Một nút có nhiều nhất hai nút con, một cho bit 0 và một cho bit 1. 

2. Chèn số đầu tiên vào bộ ba bằng cách xử lý các bit từ bit có trọng số cao nhất xuống bit có trọng số thấp nhất. Tại mỗi bit, tạo con được yêu cầu nếu nó không tồn tại. 

3. Với mỗi số tiếp theo, hãy tìm kiếm giá trị tạo ra XOR nhỏ nhất với nó. Tại mỗi vị trí bit, hãy kiểm tra xem trie có chứa bit giống với số hiện tại hay không. Nếu đúng như vậy, hãy đi theo nhánh đó vì nó giữ bit XOR hiện tại bằng 0. Nếu không, hãy theo nhánh đối diện và thêm giá trị của bit này vào kết quả XOR hiện tại. 

4. Sau khi tìm được đối tác tốt nhất cho số hiện tại, hãy cập nhật câu trả lời tối thiểu toàn cầu và chèn số hiện tại vào bộ thử. 

5. In giá trị nhỏ nhất tìm được sau khi tất cả các số đã được xử lý. 

Lý do việc tìm kiếm có thể tham lam chọn các bit bằng nhau là vì các số nhị phân được so sánh theo ý nghĩa. Sự khác biệt ở bit cao hơn không bao giờ có thể được bù đắp bằng các lựa chọn ở bit thấp hơn. Bảo toàn các bit cao nhất có thể luôn là quyết định tốt nhất. 

Tại sao nó hoạt động: sau khi một số số được chèn vào, trie sẽ chứa mọi đối tác có thể có trước đó. Trong quá trình truy vấn, quá trình truyền tải chọn đường dẫn giảm thiểu từng bit XOR từ bit cao nhất đến bit thấp nhất. Bởi vì các bit cao hơn chiếm ưu thế trong giá trị số cuối cùng, lựa chọn tham lam này tạo ra XOR nhỏ nhất có thể với bất kỳ số nào đã có trong bộ thử. Mọi số ngoại trừ số đầu tiên đều được so sánh với tất cả các số trước đó thông qua truy vấn tối ưu này, vì vậy mọi cặp có thể đều được xem xét một lần. Giá trị tối thiểu trong số các giá trị cặp tối ưu này là câu trả lời tổng thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BinaryTrie:
    def __init__(self):
        self.children = [[-1, -1]]

    def insert(self, x):
        node = 0
        for bit in range(30, -1, -1):
            b = (x >> bit) & 1
            nxt = self.children[node][b]
            if nxt == -1:
                nxt = len(self.children)
                self.children[node][b] = nxt
                self.children.append([-1, -1])
            node = nxt

    def query(self, x):
        node = 0
        result = 0
        for bit in range(30, -1, -1):
            b = (x >> bit) & 1
            if self.children[node][b] != -1:
                node = self.children[node][b]
            else:
                result |= 1 << bit
                node = self.children[node][1 - b]
        return result

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        trie = BinaryTrie()
        trie.insert(a[0])

        best = 1 << 31
        for x in a[1:]:
            best = min(best, trie.query(x))
            trie.insert(x)

        ans.append(str(best))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`BinaryTrie`lớp lưu trữ các biểu diễn nhị phân của các giá trị được xử lý trước đó. các`insert`phương thức tuân theo các bit từ 30 xuống 0 vì\(10^9\)nhỏ hơn\(2^{30}\), vì vậy 31 vị trí này đủ để biểu thị mọi giá trị đầu vào có thể bao gồm cả số 0. 

các`query`phương pháp là phần tham lam của thuật toán. Nó cố gắng giữ nguyên một chút ở mọi cấp độ. Nếu nhánh đó tồn tại, việc chọn nó sẽ đóng góp 0 cho XOR. Nếu nó không tồn tại, nhánh đối diện sẽ bị ép buộc và bit tương ứng sẽ được thêm vào câu trả lời. 

Số đầu tiên được chèn trước bất kỳ truy vấn nào vì một cặp yêu cầu hai chiếc ghế khác nhau. Bắt đầu với một trie trống sẽ làm cho truy vấn đầu tiên không hợp lệ. Sau mỗi truy vấn, số hiện tại sẽ được chèn vào để các số trong tương lai có thể sử dụng nó làm ứng cử viên. 

Số nguyên Python không bị tràn nên các thao tác bit được an toàn. Giá trị ban đầu của`best`lớn hơn bất kỳ kết quả XOR nào có thể có vì XOR tối đa của hai số bên dưới\(2^{30}\)ở dưới\(2^{31}\). 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1
5
1 2 4 8 16
```Trie tăng lên trong khi mỗi số mới tìm kiếm đối tác XOR gần nhất. 

| Giá trị hiện tại | Đã tìm thấy bit ưa thích | Tối thiểu hiện tại | 
|---|---|---| 
| 1 | Chèn đầu tiên | Chưa đặt | 
| 2 | Khớp 1 ngoại trừ bit 1 | 3 | 
| 4 | Đối tác tốt nhất tặng XOR 5 | 3 | 
| 8 | Đối tác tốt nhất tặng XOR 12 | 3 | 
| 16 | Đối tác tốt nhất tặng XOR 24 | 3 | 

Câu trả lời là 3 vì cặp 1 và 2 chỉ khác nhau ở bit thứ hai. 

Đối với mẫu thứ hai:```
2
2
2 4
4
2 4 6 8
```Trường hợp thử nghiệm đầu tiên chỉ có một cặp có thể. 

| Kiểm tra | Giá trị hiện tại | Trie chứa | Kết quả XOR | Trả lời | 
|---|---|---|---|---| 
| 1 | 4 | 2 | 6 | 6 | 

Đối với trường hợp thử nghiệm thứ hai: 

| Giá trị hiện tại | Trie chứa | XOR tốt nhất được tìm thấy | Câu trả lời hiện tại | 
|---|---|---|---| 
| 2 | không | không | Chưa đặt | 
| 4 | 2 | 6 | 6 | 
| 6 | 2, 4 | 2 | 2 | 
| 8 | 2, 4, 6 | 10 | 2 | 

Dấu vết thứ hai cho biết lý do tại sao mọi giá trị được chèn vẫn phải có sẵn. Khi xử lý 6, trie đã chứa cả 2 và 4 và 4 cho XOR tối ưu là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(31n) | Mỗi lần chèn và truy vấn truy cập chính xác ở mức 31 bit. | 
| Không gian | O(31n) | Mỗi giá trị được chèn tạo ra tối đa 31 nút trie. | 

Tổng số giá trị trên tất cả các trường hợp thử nghiệm nhiều nhất là\(10^6\), như vậy có khoảng 31 triệu thao tác thử được thực hiện. Điều này phù hợp với các ràng buộc dự định, trong khi cách tiếp cận bậc hai thì không. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert solve_io("""1
5
1 2 4 8 16
""") == "3\n", "sample 1"

assert solve_io("""2
2
2 4
4
2 4 6 8
""") == "6\n2\n", "sample 2"

assert solve_io("""1
2
0 0
""") == "0\n", "duplicate values"

assert solve_io("""1
2
0 1000000000
""") == "1000000000\n", "large boundary values"

assert solve_io("""1
6
5 5 5 9 12 20
""") == "0\n", "many equal values"

assert solve_io("""1
3
1 4 7
""") == "3\n", "non adjacent numerical values"

# The solve function from the main solution should be available here.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`1 / 5 / 1 2 4 8 16`|`3`| Truyền tải trie cơ bản | 
|`2 / 2 2 4 / 4 2 4 6 8`|`6, 2`| Nhiều trường hợp thử nghiệm | 
|`2 / 0 0`|`0`| Giá trị bằng nhau | 
|`2 / 0 1000000000`|`1000000000`| Bit yêu cầu cao nhất | 
|`6 / 5 5 5 9 12 20`|`0`| Xử lý trùng lặp | 
|`3 / 1 4 7`|`3`| XOR không giống như khoảng cách số | 

## Vỏ cạnh 

Đối với các giá trị bằng nhau, trie tự nhiên tìm thấy bit giống nhau ở mọi cấp độ. Coi như:```
1
3
7 7 12
```7 đầu tiên được chèn vào. Khi số 7 thứ hai được truy vấn, mỗi bit đều theo một nhánh giống hệt nhau, tạo ra XOR 0. Vì số 0 là câu trả lời nhỏ nhất có thể nên các số sau này không thể cải thiện nó. 

Đối với các giá trị mà cặp số gần nhất không rõ ràng, trie theo sau các bit thay vì khoảng cách được sắp xếp. Coi như:```
1
4
1 2 8 16
```Việc tìm kiếm 2 ưu tiên đường dẫn 1 vì chúng chia sẻ tất cả các bit cao và chỉ khác nhau ở bit 1. Kết quả XOR là 3, trở thành câu trả lời. 

Đối với các giá trị lớn, thuật toán bao gồm mọi bit cần thiết cho phạm vi đầu vào. Coi như:```
1
2
0 1000000000
```Trie xử lý bit 30 xuống bit 0, do đó nó thể hiện chính xác giá trị đầy đủ. Cặp duy nhất có thể tạo ra XOR 1000000000 và được trả về.
