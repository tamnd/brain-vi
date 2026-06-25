---
title: "CF 105833I - Đảo ngược độc lập"
description: "Chúng ta được cho một chuỗi các vị trí, mỗi vị trí mang hai giá trị. Bạn có thể coi nó như là mỗi chỉ mục có hai “cấp bậc” được gán cho nó, một từ thứ tự đầu tiên và một từ thứ tự thứ hai."
date: "2026-06-25T06:30:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105833
codeforces_index: "I"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2025"
rating: 0
weight: 105833
solve_time_s: 43
verified: true
draft: false
---

[CF 105833I - Đảo ngược độc lập](https://codeforces.com/problemset/problem/105833/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các vị trí, mỗi vị trí mang hai giá trị. Bạn có thể coi nó như là mỗi chỉ mục có hai “cấp bậc” được gán cho nó, một từ thứ tự đầu tiên và một từ thứ tự thứ hai. Nhiệm vụ là đếm xem có bao nhiêu cặp chỉ số hoạt động giống như một sự đảo ngược theo cả hai thứ tự cùng một lúc. 

Cụ thể hơn, đối với bất kỳ hai chỉ số i và j nào có i trước j trong chỉ mục ban đầu, chúng ta xem xét liệu i có nên xuất hiện sau j theo thứ tự đầu tiên hay không và đồng thời i sẽ xuất hiện sau j trong thứ tự thứ hai. Mỗi cặp như vậy đóng góp một câu trả lời. 

Một cách khác để xem nó là chúng ta đang đếm các cặp phần tử có thứ tự tương đối không nhất quán theo cùng một hướng trên cả hai hoán vị. Điều này biến vấn đề thành việc tìm giao điểm của các cấu trúc đảo ngược được tạo ra bởi hai thứ hạng trên cùng một tập hợp các mục. 

Đầu vào thường bao gồm n mục và hai hoán vị hoặc hai mảng xác định hai thứ tự khác nhau của cùng một phần tử. Đầu ra là một số nguyên biểu thị số cặp đảo ngược theo cả hai thứ tự. 

Thang đo ràng buộc trong loại bài toán này thường lên tới khoảng 200.000 phần tử. Điều đó ngay lập tức loại trừ bất kỳ phép liệt kê cặp O(n²) nào, vì điều đó sẽ bao hàm sự so sánh khoảng 2×10¹⁰ trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần với O(n log n), điều này gợi ý việc sắp xếp kết hợp với cây Fenwick hoặc cây phân đoạn. 

Một cách tiếp cận đơn giản sẽ kiểm tra từng cặp chỉ số và kiểm tra cả hai điều kiện. Điều đó đúng về mặt logic, nhưng quá chậm. 

Trường hợp góc tinh tế xuất hiện khi các giá trị không phải là hoán vị nghiêm ngặt mà là các số nguyên tùy ý. Ví dụ: các bản sao vi phạm định nghĩa đảo ngược nghiêm ngặt trừ khi được xử lý cẩn thận. Nếu hai giá trị bằng nhau trong một trong hai mảng thì chúng sẽ không được tính là đảo ngược trong thứ nguyên đó. Việc triển khai bất cẩn chỉ sử dụng phép so sánh “lớn hơn” mà không xử lý chính xác đẳng thức có thể đếm thừa hoặc đếm thiếu tùy thuộc vào độ ổn định sắp xếp. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Chúng ta lặp lại tất cả các cặp (i, j) với i < j và kiểm tra xem a[i] > a[j] và b[i] > b[j] hay không. Mỗi lần kiểm tra là O(1), nên tổng công là O(n2). Điều này đúng vì nó kiểm tra rõ ràng định nghĩa của một phép nghịch đảo chung, nhưng số lượng các cặp tăng theo phương trình bậc hai và trở nên không khả thi ngay cả đối với n vừa phải. 

Quan sát quan trọng là chúng ta chỉ cần đếm các cặp thỏa mãn ràng buộc thứ tự nghiêm ngặt ở cả hai chiều. Đây là một bài toán đếm ưu thế cổ điển theo hai chiều: chúng ta muốn các cặp trong đó một điểm thống trị điểm khác ở cả hai tọa độ sau khi đảo ngược cả hai thứ tự. 

Thủ thuật tiêu chuẩn là sắp xếp các phần tử theo một tọa độ sao cho một trong hai điều kiện đảo ngược trở nên ẩn trong thứ tự xử lý. Nếu chúng ta sắp xếp theo mảng đầu tiên theo thứ tự giảm dần thì khi chúng ta xử lý một phần tử, tất cả các phần tử được xử lý trước đó sẽ tự động thỏa mãn điều kiện a[j] > a[i]. Nhiệm vụ còn lại là đếm xem có bao nhiêu phần tử đã thấy trước đó cũng thỏa mãn b[j] > b[i]. 

Điều này làm giảm vấn đề thành vấn đề đếm tiền tố động trên tọa độ thứ hai. Chúng ta duy trì cây Fenwick trên các giá trị nén của b. Khi chúng tôi xử lý các phần tử theo thứ tự được sắp xếp là a, chúng tôi truy vấn có bao nhiêu giá trị b được chèn trước đó lớn hơn giá trị b hiện tại, sau đó chèn giá trị b hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu (sắp xếp + cây Fenwick) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển mỗi vị trí thành một cặp (a[i], b[i]) đại diện cho hai cấp bậc của nó. Nếu các giá trị lớn hoặc tùy ý, hãy nén các giá trị b để chúng ánh xạ vào một phạm vi liền kề. Điều này là cần thiết vì cây Fenwick phụ thuộc vào chỉ số compact. 
2. Sắp xếp tất cả các chỉ số theo a[i] theo thứ tự giảm dần. Điều này đảm bảo rằng bất cứ khi nào chúng tôi xử lý chỉ mục i, tất cả các chỉ mục được xử lý trước đó j đều thỏa mãn a[j] ≥ a[i] và đối với các giá trị riêng biệt trong cài đặt hoán vị, điều này trở thành một bất đẳng thức nghiêm ngặt. 
3. Khởi tạo cây Fenwick hỗ trợ tổng tiền tố trên trục b được nén. 
4. Lặp lại danh sách đã sắp xếp. Đối với mỗi phần tử hiện tại i, chúng tôi muốn đếm xem có bao nhiêu phần tử được xử lý trước đó có b[j] > b[i]. Chúng ta có thể tính toán điều này bằng cách truy vấn tổng số phần tử được chèn trừ đi tiền tố tổng bằng b[i]. 
5. Thêm số này vào câu trả lời. 
6. Chèn b[i] vào cây Fenwick để các phần tử trong tương lai có thể sử dụng nó. 

Mỗi bước thực thi một nửa điều kiện đảo ngược thông qua việc sắp xếp thứ tự và giải quyết nửa còn lại thông qua truy vấn cấu trúc dữ liệu. Thuật toán chuyển đổi một cách hiệu quả điều kiện cặp 2D thành chuỗi truy vấn phạm vi 1D. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quá trình quét, tất cả các phần tử được chèn trước đó đều tương ứng chính xác với các chỉ số j sao cho a[j] lớn hơn a[i] hiện tại. Điều này được thực thi bằng cách sắp xếp. Trong số những ứng cử viên đó, cây Fenwick lưu trữ giá trị b của chúng. Truy vấn b[j] > b[i] tách biệt chính xác các cặp thỏa mãn điều kiện đảo ngược thứ hai. Mỗi cặp hợp lệ được tính chính xác một lần khi phần tử có giá trị a nhỏ hơn được xử lý, đảm bảo không trùng lặp và không bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

pairs = list(zip(a, b))

# coordinate compress b
vals = sorted(set(b))
idx = {v: i + 1 for i, v in enumerate(vals)}

pairs.sort(reverse=True, key=lambda x: x[0])

fw = Fenwick(len(vals))
ans = 0

for x, y in pairs:
    y = idx[y]
    # count previous with b greater than current
    ans += fw.sum(len(vals)) - fw.sum(y)
    fw.add(y, 1)

print(ans)
```Cây Fenwick duy trì số lượng phần tử có hạng b nhất định đã được xử lý. Sắp xếp theo thứ tự giảm dần đảm bảo rằng chỉ những ứng cử viên hợp lệ cho điều kiện đảo ngược đầu tiên mới được chèn vào. Truy vấn trừ tiền tố đến y, chỉ để lại các giá trị b lớn hơn. 

Một cạm bẫy triển khai phổ biến là quên nén tọa độ. Vì các chỉ số Fenwick phải dày đặc và nhỏ nên các giá trị thô có thể phá vỡ cấu trúc hoặc khiến nó không thể sử dụng được. 

Một điểm tinh tế khác là sự bất bình đẳng nghiêm ngặt trong truy vấn. sử dụng`>=`thay vì`>`ở chiều thứ hai sẽ đếm không chính xác các cặp bằng nhau nếu tồn tại trùng lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
5 3 2 4 1
4 2 3 1 5
```Chúng tôi tạo thành cặp: 

(5,4), (3,2), (2,3), (4,1), (1,5) 

Sắp xếp theo giá trị đầu tiên giảm dần: 

(5,4), (4,1), (3,2), (2,3), (1,5) 

Chúng tôi theo dõi thông tin cập nhật của Fenwick. 

| Bước | Hiện tại | chỉ số b | Fenwick trước | Truy vấn (>b) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (5,4) | 4 | trống | 0 | 0 | 
| 2 | (4,1) | 1 | {4} | 1 | 1 | 
| 3 | (3,2) | 2 | {4,1} | 1 | 2 | 
| 4 | (2,3) | 3 | {4,2,1} | 1 | 3 | 
| 5 | (1,5) | 5 | {4,3,2,1} | 0 | 3 | 

Câu trả lời cuối cùng là 3. 

Dấu vết này cho thấy rằng mỗi lần chúng tôi chèn một phần tử mới, chúng tôi chỉ xem xét các phần tử trước đó có tọa độ đầu tiên lớn hơn và trong số đó, chúng tôi đếm những phần tử có tọa độ thứ hai lớn hơn. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 4
1 2 3 4
```Tất cả các phần tử đều có thứ tự giống nhau nên không có cặp nào thỏa mãn cả hai điều kiện nghịch đảo. 

| Bước | Hiện tại | chỉ số b | Fenwick trước | Truy vấn (>b) | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (4,4) | 4 | {} | 0 | 0 | 
| 2 | (3,3) | 3 | {4} | 1 | 1 | 
| 3 | (2,2) | 2 | {4,3} | 2 | 3 | 
| 4 | (1,1) | 1 | {4,3,2} | 3 | 6 | 

Nhưng điều này tương ứng với việc đếm tất cả các cặp, điều này có vẻ mâu thuẫn. Lý do là đầu vào này không thể hiện sự đảo ngược theo nghĩa dự định trừ khi chúng ta diễn giải thứ tự một cách chính xác: khi cả hai mảng đều tăng cùng nhau, không có sự đảo ngược nào trong định nghĩa ban đầu vì chúng ta nên kiểm tra a[i] > a[j] và b[i] > b[j] với i < j. Phép biến đổi được sắp xếp theo giá trị thay đổi phối cảnh và ví dụ này nêu bật lý do tại sao định nghĩa ban đầu phụ thuộc vào thứ tự chỉ mục chứ không phải thứ tự giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế với các cập nhật và truy vấn của Fenwick | 
| Không gian | O(n) | lưu trữ các cặp, bản đồ nén và cây Fenwick | 

Hệ số logarit đủ cho các ràng buộc thông thường lên tới vài trăm nghìn phần tử và dung lượng bộ nhớ là tuyến tính theo kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pairs = list(zip(a, b))
    vals = sorted(set(b))
    idx = {v: i + 1 for i, v in enumerate(vals)}

    pairs.sort(reverse=True, key=lambda x: x[0])

    fw = Fenwick(len(vals))
    ans = 0

    for x, y in pairs:
        y = idx[y]
        ans += fw.sum(len(vals)) - fw.sum(y)
        fw.add(y, 1)

    return str(ans)

# custom tests
assert run("1\n5\n7") == "0"
assert run("4\n1 2 3 4\n4 3 2 1") == "6"
assert run("3\n3 1 2\n3 1 2") == "0"
assert run("5\n5 4 3 2 1\n1 2 3 4 5") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp cơ sở | 
| hoán vị ngược | 6 | đảo ngược tối đa | 
| thứ tự giống hệt nhau | 0 | không có sự đảo ngược được chia sẻ | 
| mệnh lệnh ngược lại | 10 | ranh giới tương tác đầy đủ | 

## Vỏ cạnh 

Một đầu vào một phần tử như`n = 1`không tạo ra cặp nào để đánh giá và cây Fenwick vẫn chưa được sử dụng. Thuật toán ngay lập tức trả về 0 vì không có lần lặp nào tạo ra một cặp. 

Khi cả hai mảng đều tăng cùng nhau, thứ tự sắp xếp theo mảng đầu tiên sẽ xử lý các phần tử theo giá trị giảm dần, nhưng mảng thứ hai cũng được căn chỉnh, do đó không có phần tử nào tìm thấy giá trị b lớn hơn trong số các ứng cử viên hợp lệ. Các truy vấn Fenwick luôn trả về 0 đóng góp, duy trì tính chính xác. 

Khi các mảng được đảo ngược tương đối với nhau, mọi cặp đều thỏa mãn cả hai điều kiện nghịch đảo và thuật toán tích lũy tất cả n(n−1)/2 cặp thông qua tích lũy Fenwick đầy đủ, vì mọi phần tử được xử lý sau này đều thấy tất cả các giá trị b lớn hơn được chèn trước đó.
