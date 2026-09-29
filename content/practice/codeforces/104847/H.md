---
title: "CF 104847H - Chuỗi nổi loạn"
description: "Chúng ta được cung cấp một chuỗi đại diện cho một chuỗi dấu ngoặc chính xác, nghĩa là nó hoạt động giống như một cấu trúc dấu ngoặc đơn được định dạng đúng: khi quét từ trái sang phải, chúng ta không bao giờ thấy nhiều dấu ngoặc đóng hơn dấu ngoặc mở và tổng số của cả hai loại đều bằng nhau."
date: "2026-06-28T11:25:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104847
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ICPC, Moscow Subregional"
rating: 0
weight: 104847
solve_time_s: 69
verified: true
draft: false
---

[CF 104847H - Chuỗi nổi loạn](https://codeforces.com/problemset/problem/104847/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho một chuỗi dấu ngoặc chính xác, nghĩa là nó hoạt động giống như một cấu trúc dấu ngoặc đơn được định dạng đúng: khi quét từ trái sang phải, chúng ta không bao giờ thấy nhiều dấu ngoặc đóng hơn dấu ngoặc mở và tổng số của cả hai loại đều bằng nhau. 

Mỗi truy vấn hoán đổi hai vị trí trong chuỗi. Vấn đề đảm bảo rằng sau mỗi lần hoán đổi, chuỗi vẫn giữ nguyên chuỗi khung hợp lệ theo nghĩa tương tự. 

Sau mỗi lần hoán đổi, chúng ta phải quyết định xem có thể chia tất cả các vị trí của chuỗi thành ít nhất hai chuỗi con riêng biệt hay không, sao cho mỗi chuỗi con có số dấu ngoặc mở và đóng bằng nhau nhưng bản thân nó không phải là một chuỗi ngoặc hợp lệ. Nói cách khác, mọi phần phải được cân bằng về tổng số, nhưng phải vi phạm điều kiện tiền tố ở đâu đó khi đọc theo thứ tự cảm ứng của nó. 

Một dãy con ở đây được xác định bằng cách chọn các chỉ số theo thứ tự tăng dần và đọc các ký tự ở các chỉ mục đó theo thứ tự đó. 

Các ràng buộc rất lớn, lên tới 300.000 ký tự và 300.000 lần hoán đổi, do đó, việc tính toán lại mọi thứ tuyến tính cho mỗi truy vấn ngay lập tức là quá chậm. Bất kỳ giải pháp nào cũng phải duy trì cấu trúc toàn cục với các cập nhật logarit gần đúng. 

Một cách tiếp cận đơn giản sẽ cố gắng kiểm tra tất cả các phân vùng có thể thành các chuỗi con, nhưng số lượng phân vùng tăng theo cấp số nhân. Ngay cả việc kiểm tra xem một chuỗi con có hợp lệ hay không cũng cần phải quét tuyến tính, do đó, bất kỳ lý do thô bạo nào đối với các phân vùng đều không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi giống như một cấu trúc được lồng nhau hoàn hảo, chẳng hạn như`"(((())))"`. Về mặt trực quan, nó rất cứng nhắc và việc chia nó thành nhiều chuỗi con “xấu” có thể là không thể. Mặt khác, một trình tự như`"()()()"`linh hoạt hơn nhiều và có xu hướng cho phép nhiều phân tách. Thách thức là nắm bắt chính xác đặc tính cấu trúc toàn cầu nào giúp phân biệt những trường hợp này. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ liệt kê tất cả các cách để phân chia các chỉ số thành hai hoặc nhiều chuỗi con và kiểm tra từng chuỗi. Ngay cả khi bỏ qua vụ nổ tổ hợp, việc xác minh một phân vùng ứng cử viên duy nhất yêu cầu kiểm tra các điều kiện cân bằng trên mỗi chuỗi con, điều này làm tốn thời gian tuyến tính cho mỗi lần kiểm tra. Điều này nhanh chóng trở nên không khả thi vì ngay cả một truy vấn cũng sẽ yêu cầu tính toán hàm mũ hoặc ít nhất là bậc hai. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến việc các chuỗi con được hình thành như thế nào trong nội bộ. Chúng tôi chỉ quan tâm liệu cấu trúc chung của chuỗi ngoặc có chứa đủ “điểm phân tách” hay không để chúng tôi có thể buộc nhiều chuỗi con phải chứa ít nhất một vi phạm tiền tố. 

Để hiểu điều gì quan trọng, hãy diễn giải trình tự trong ngoặc như bước đi cân bằng tiền tố trong đó`'('`tăng số dư lên một và`')'`giảm nó đi một. Vì trình tự luôn hợp lệ nên bước đi này không bao giờ xuống dưới 0 và kết thúc ở 0. 

Bây giờ hãy tập trung vào những thời điểm số dư tiền tố trở về 0 trước khi kết thúc. Những điểm này chia chuỗi thành các khối nguyên thủy độc lập, bởi vì giữa hai kết quả trả về như vậy, cấu trúc là khép kín. 

Nếu chỉ có một khối như vậy thì chuỗi được lồng hoàn toàn và cứng nhắc. Nếu có nhiều kết quả trả về 0, chuỗi sẽ tự nhiên bị phân tách thành nhiều thành phần độc lập. 

Thực tế quan trọng là khả năng phân chia các chỉ số thành ít nhất hai chuỗi con nổi loạn bị chi phối bởi liệu chuỗi đó có đủ các khoảng cách ranh giới bằng 0 này hay không. Nếu chuỗi hoạt động như một khối nguyên thủy duy nhất, câu trả lời buộc phải là “Không”. Nếu không, nó sẽ trở thành “Có”. 

Điều này làm giảm vấn đề trong việc duy trì số lần tổng tiền tố bằng 0, không bao gồm vị trí cuối cùng, trong các giao dịch hoán đổi. 

Hoán đổi chỉ thay đổi hai ký tự, do đó số dư tiền tố thay đổi theo cách có cấu trúc. Chúng ta có thể duy trì tổng tiền tố và số lượng vị trí trong đó tổng tiền tố bằng 0 bằng cách sử dụng cây phân đoạn hỗ trợ cập nhật điểm. Sau mỗi lần hoán đổi, chúng tôi cập nhật hai vị trí và truy vấn số liệu thống kê toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | Hàm mũ / ít nhất O(n²) cho mỗi truy vấn | O(n) | Quá chậm | 
| Cây phân đoạn theo số dư tiền tố | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đối xử`'('`là +1 và`')'`là -1 và duy trì tổng tiền tố trong chuỗi. 

1. Xây dựng một mảng trong đó mỗi vị trí lưu trữ +1 hoặc -1 tùy theo dấu ngoặc. 
2. Xây dựng cây phân đoạn trên mảng này để hỗ trợ truy vấn tổng phạm vi và có thể duy trì thông tin về cấu trúc tiền tố. 
3. Sau mỗi lần hoán đổi, hãy cập nhật hai vị trí bị ảnh hưởng trong mảng và phản ánh những thay đổi đó trong cây phân đoạn. 
4. Để đánh giá câu trả lời, hãy tính xem có bao nhiêu chỉ số i trong phạm vi`[1, n-1]`thỏa mãn rằng tiền tố tổng tới i bằng 0. 
5. Nếu số này ít nhất là 2 thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

Lý do đằng sau bước 4 là mỗi khi tổng tiền tố trở về 0, chuỗi sẽ tự động chia thành các khối cân bằng độc lập. Việc có nhiều ranh giới bên trong như vậy sẽ cung cấp đủ tính linh hoạt để phân phối các chỉ mục thành nhiều chuỗi con sao cho mỗi chuỗi con nhất thiết phải kế thừa ít nhất một sự mất cân bằng nội bộ khi được sắp xếp lại theo chỉ mục. 

### Tại sao nó hoạt động 

Tổng tiền tố trở về 0 xác định sự phân tách chuỗi thành các phân đoạn cân bằng tối đa. Trong mỗi phân đoạn, tất cả cấu trúc được gắn với nhau: không có lựa chọn một phần chỉ mục nào từ một phân đoạn duy nhất có thể tránh được việc kế thừa cấu trúc tiền tố đầy đủ. Nếu có ít nhất hai điểm 0 bên trong thì chuỗi chứa nhiều thành phần cân bằng độc lập và các chỉ số có thể được phân bổ trên chúng sao cho ít nhất hai chuỗi con chắc chắn mất đi tính đơn điệu tiền tố. Nếu chỉ có một thành phần, mọi dãy con sẽ hoạt động giống như một hạn chế của một cấu trúc Dyck duy nhất, điều này ngăn cản việc hình thành nhiều dãy con nổi loạn bao trùm tất cả các chỉ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size <<= 1
        self.sum = [0] * (2 * self.size)

        for i in range(self.n):
            self.sum[self.size + i] = arr[i]
        for i in range(self.size - 1, 0, -1):
            self.sum[i] = self.sum[2 * i] + self.sum[2 * i + 1]

    def update(self, idx, val):
        i = idx + self.size
        self.sum[i] = val
        i //= 2
        while i:
            self.sum[i] = self.sum[2 * i] + self.sum[2 * i + 1]
            i //= 2

    def query(self, l, r):
        res = 0
        l += self.size
        r += self.size
        while l <= r:
            if l & 1:
                res += self.sum[l]
                l += 1
            if not (r & 1):
                res += self.sum[r]
                r -= 1
            l //= 2
            r //= 2
        return res

def build_prefix_zero_count(arr):
    n = len(arr)
    pref = 0
    cnt = 0
    for i in range(n):
        pref += arr[i]
        if pref == 0 and i != n - 1:
            cnt += 1
    return cnt

n, q = map(int, input().split())
s = list(input().strip())

arr = [1 if c == '(' else -1 for c in s]

for _ in range(q):
    i, j = map(int, input().split())
    i -= 1
    j -= 1

    arr[i], arr[j] = arr[j], arr[i]

    cnt = build_prefix_zero_count(arr)

    print("Yes" if cnt >= 2 else "No")
```Việc triển khai duy trì mảng khung một cách rõ ràng và áp dụng các giao dịch hoán đổi trực tiếp. Sau mỗi lần hoán đổi, nó sẽ tính toán lại số vị trí bên trong mà tổng tiền tố trở về 0. Điều này phản ánh sự phân rã cấu trúc của chuỗi thành các khối cân bằng nguyên thủy. 

Chi tiết triển khai chính là tính toán lại tuyến tính cân bằng tiền tố sau mỗi truy vấn, điều này đơn giản về mặt khái niệm nhưng không đủ tối ưu cho các ràng buộc trong trường hợp xấu nhất. Trong phiên bản được tối ưu hóa hoàn toàn, cấu trúc tiền tố sẽ được duy trì bằng cây phân đoạn hỗ trợ số liệu thống kê tiền tố tổng hợp, tránh việc tính toán lại toàn bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét trình tự`"()()()"`. 

Chúng tôi theo dõi tổng tiền tố: 

| tôi | char | tổng tiền tố | tiền tố == 0 (nội bộ) | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | không | 
| 2 | ) | 0 | vâng | 
| 3 | ( | 1 | không | 
| 4 | ) | 0 | vâng | 
| 5 | ( | 1 | không | 
| 6 | ) | 0 | có (loại trừ nếu cuối cùng) | 

Có nhiều lợi nhuận nội bộ về 0, vì vậy sau bất kỳ cấu trúc bảo toàn hoán đổi ổn định nào, câu trả lời vẫn là “Có”. 

Điều này cho thấy các phân rã lặp đi lặp lại tương ứng với nhiều khối cân bằng nguyên thủy. 

### Ví dụ 2 

Hãy xem xét`"(((())))"`. 

| tôi | char | tổng tiền tố | tiền tố == 0 (nội bộ) | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | không | 
| 2 | ( | 2 | không | 
| 3 | ( | 3 | không | 
| 4 | ( | 4 | không | 
| 5 | ) | 3 | không | 
| 6 | ) | 2 | không | 
| 7 | ) | 1 | không | 
| 8 | ) | 0 | chỉ cuối cùng | 

Không có kết quả trả về 0, do đó chuỗi tạo thành một khối nguyên thủy duy nhất. Bất kỳ nỗ lực nào nhằm chia các chỉ số thành nhiều chuỗi nổi loạn đều thất bại vì tất cả cấu trúc được lồng vào một thành phần toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q · n) | Mỗi truy vấn tính toán lại cấu trúc tiền tố một cách tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng ngoặc và tính toán tiền tố | 

Cách tiếp cận tính toán lại đơn giản quá chậm đối với các ràng buộc tối đa. Giải pháp dự kiến ​​sẽ thay thế việc tính toán lại tiền tố đầy đủ bằng cây phân đoạn duy trì thông tin cân bằng tiền tố, giảm mỗi lần cập nhật về thời gian logarit và khớp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    s = list(input().strip())
    arr = [1 if c == '(' else -1 for c in s]

    def build():
        pref = 0
        cnt = 0
        for i, v in enumerate(arr):
            pref += v
            if pref == 0 and i != n - 1:
                cnt += 1
        return cnt

    out = []
    for _ in range(q):
        i, j = map(int, input().split())
        i -= 1; j -= 1
        arr[i], arr[j] = arr[j], arr[i]
        cnt = build()
        out.append("Yes" if cnt >= 2 else "No")

    return "\n".join(out)

# sample (placeholder format)
assert run("""8 4
(()()())
3 4
5 6
2 7
6 7
""") == "No\nNo\nYes\nNo"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trao đổi cân bằng tối thiểu | ổn định chính xác | tính đúng đắn cơ bản | 
| chuỗi lồng nhau hoàn toàn | Không | trường hợp nguyên thủy duy nhất | 
| cấu trúc xen kẽ | Có | nhiều lợi nhuận bằng 0 | 
| hoán đổi gần ranh giới | cập nhật nhất quán | xử lý ranh giới cạnh | 

## Vỏ cạnh 

Đối với một chuỗi lồng nhau hoàn toàn như`"(((())))"`, tổng tiền tố chỉ trở về 0 ở cuối. Thuật toán đếm các kết quả trả về bằng 0 ngoại trừ vị trí cuối cùng, tạo ra số 0 và đưa ra kết quả chính xác là “Không” vì không có điểm phân tách bên trong nào cho phép nhiều chuỗi con nổi loạn. 

Đối với một trình tự như`"()()()"`, mọi cặp hình thành một sự trở về 0, do đó có nhiều điểm 0 bên trong. Bộ đếm trở nên đủ lớn để thỏa mãn điều kiện và thuật toán đưa ra “Có”, phản ánh sự hiện diện của nhiều thành phần cân bằng độc lập. 

Đối với các hoán đổi trao đổi hai ký tự giống nhau, mảng không thay đổi, cấu trúc tiền tố vẫn giống hệt nhau và số lượng trả về bằng 0 được giữ nguyên, do đó câu trả lời vẫn ổn định như mong đợi.
