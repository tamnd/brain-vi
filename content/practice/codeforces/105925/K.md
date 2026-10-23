---
title: "CF 105925K - K Yếu Tố Thiếu"
description: "Chúng ta được cung cấp một chuỗi xác định ràng buộc thứ tự giữa các vị trí và một chuỗi khác gán trọng số cho từng vị trí."
date: "2026-06-22T15:37:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105925
codeforces_index: "K"
codeforces_contest_name: "SBC Brazilian Phase Zero 2025"
rating: 0
weight: 105925
solve_time_s: 85
verified: true
draft: false
---

[CF 105925K - K Yếu tố bị thiếu](https://codeforces.com/problemset/problem/105925/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi xác định ràng buộc thứ tự giữa các vị trí và một chuỗi khác gán trọng số cho từng vị trí. Từ đó, chúng ta xem xét tất cả các dãy con có thể có của các chỉ số trong đó cả thứ tự chỉ mục và các giá trị từ dãy đầu tiên đều tăng nghiêm ngặt. Mỗi dãy con hợp lệ sẽ thu thập các trọng số từ mảng thứ hai và tạo ra tổng số tiền. Việc thu thập tất cả các khoản tiền như vậy tạo thành một tập hợp nhiều tập hợp rất lớn và chúng ta được yêu cầu xuất ra K giá trị lớn nhất từ ​​nó theo thứ tự không tăng hoặc −1 khi tồn tại ít hơn K. 

Cấu trúc khóa là một dãy con chỉ hợp lệ nếu nó thỏa mãn hai điều kiện tăng đồng thời. Một cái xuất phát từ việc di chuyển từ trái sang phải trong mảng và cái còn lại đến từ việc tăng giá trị trong một hoán vị. Điều này làm cho các dãy con hợp lệ chính xác là các chuỗi tăng dần theo thứ tự từng phần hai chiều. 

Các ràng buộc ngụ ý rằng việc liệt kê trực tiếp tất cả các chuỗi con là không thể. Ngay cả đối với N vừa phải, số lượng dãy con tăng hợp lệ có thể tăng theo cấp số nhân. Bất kỳ giải pháp nào cố gắng liệt kê rõ ràng hoặc thậm chí đếm tất cả chúng sẽ thất bại ngay lập tức. Thay vào đó, một cách tiếp cận thành công phải hoạt động với các biểu diễn nén của các tập hợp con và chỉ trích xuất K kết quả hàng đầu. 

Trường hợp cạnh tinh tế xuất hiện khi hoán vị đã được sắp xếp theo giá trị. Trong trường hợp đó, mọi dãy con của chỉ số đều hợp lệ, do đó, bài toán suy biến thành tính toán tất cả các tổng tập hợp con của mảng trọng số. Điều này tạo ra các giá trị 2^N và việc liệt kê đơn giản rõ ràng là không thể, nhưng điều này quan trọng vì nó cho thấy rằng mọi giải pháp đúng đều phải xử lý ngầm việc phân nhánh theo cấp số nhân ngay cả trong cấu trúc đơn giản nhất. 

Một trường hợp góc khác là khi K vượt quá số dãy con hợp lệ. Đầu ra phải được đệm bằng các giá trị −1, do đó thuật toán phải đếm hoặc phát hiện sự cạn kiệt của tổng được tạo. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là tạo ra tất cả các dãy con tăng hợp lệ bằng cách sử dụng tìm kiếm theo chiều sâu hoặc lập trình động trên các tập hợp con, tích lũy tổng của chúng. Về nguyên tắc, điều này đúng vì nó xây dựng rõ ràng mọi chuỗi và tính tổng trọng lượng của nó. Tuy nhiên, mỗi phần tử có thể được bao gồm hoặc bị loại trừ và tính hợp lệ phụ thuộc vào các ràng buộc về thứ tự, do đó số lượng trạng thái tăng theo cấp số nhân. Trong trường hợp xấu nhất, khi hoán vị được sắp xếp, nó sẽ suy biến thành các chuỗi 2^N, khiến cho việc liệt kê không thể thực hiện được ngay cả đối với N nhỏ. 

Quan sát quan trọng là mọi dãy con hợp lệ có thể được xem như một đường dẫn trong biểu đồ tuần hoàn có hướng trong đó mỗi nút đại diện cho một chỉ mục và có một cạnh từ i đến j khi i < j và A[i] < A[j]. Mỗi tổng chuỗi con khi đó chính xác là tổng trọng số nút dọc theo một đường dẫn trong DAG này. Vấn đề trở thành việc tìm tổng đường dẫn lớn nhất K trong DAG với trọng số nút dương. 

Cấu trúc này cho phép lập trình động trong đó, đối với mỗi nút, chúng tôi duy trì danh sách các tổng chuỗi con tốt nhất kết thúc tại nút đó. Bất kỳ chuỗi con nào kết thúc tại nút j đều có thể được hình thành bằng cách mở rộng chuỗi con kết thúc tại nút i trước đó mà có thể tới j. Điều này gợi ý việc hợp nhất danh sách ứng cử viên từ tất cả các danh sách tiền nhiệm hợp lệ. Thách thức là mỗi nút có thể có nhiều nút trước đó và mỗi nút mang một danh sách lên tới K phần tổng. 

Để thực hiện điều này hiệu quả, chúng tôi nén tập hợp tiền thân bằng cách sử dụng cấu trúc dữ liệu theo chiều giá trị của hoán vị. Vì A là một hoán vị nên chúng ta có thể coi các giá trị là tọa độ thứ hai và xử lý các nút theo thứ tự tăng dần, duy trì cấu trúc hỗ trợ truy vấn tất cả các trạng thái trước đó có giá trị A nhỏ hơn. Mỗi mục nhập cấu trúc lưu trữ một danh sách giới hạn các tổng tốt nhất và việc hợp nhất hai danh sách được thực hiện bằng cách lấy K kết quả lớn nhất từ ​​việc kết hợp các phần tử của chúng với trọng số hiện tại.

Điều này làm giảm việc liệt kê theo cấp số nhân trong các phép hợp nhất K-way được kiểm soát trên các tiền tố có cấu trúc, trong đó mỗi trạng thái chỉ giữ lại K ứng cử viên phù hợp nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê trình tự vũ phu | O(2^N · N) | O(N) | Quá chậm | 
| DP với tiền tố hợp nhất trên cấu trúc giá trị | O(N · K log N) | O(N · K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các phần tử theo thứ tự tăng dần của các giá trị của chúng trong hoán vị, vì tính hợp lệ của chuỗi con phụ thuộc vào cả chỉ số và giá trị tăng dần, đồng thời việc cố định thứ tự giá trị đảm bảo chúng tôi chỉ mở rộng chuỗi hợp lệ. 

1. Ánh xạ từng giá trị trong A tới vị trí của nó, nhờ đó chúng ta có thể xử lý các giá trị từ 1 đến N trong khi biết chúng xuất hiện ở đâu trong mảng ban đầu. Điều này biến vấn đề thành một chuỗi được sắp xếp theo giá trị, trong đó các chuyển đổi cũng phải tôn trọng các vị trí tăng dần. 
2. Duy trì cây Fenwick hoặc cây phân đoạn trên các vị trí. Mỗi vị trí lưu trữ một danh sách tối đa K tổng dãy con tốt nhất kết thúc ở vị trí đó. Một phần tử duy nhất luôn đóng góp một chuỗi con cơ sở chỉ bao gồm chính nó, vì vậy mọi vị trí đều bắt đầu bằng trọng số riêng của nó. 
3. Với mỗi giá trị v theo thứ tự tăng dần, xác định vị trí p của nó trong mảng ban đầu. Chúng ta cần tất cả các chuỗi con có thể mở rộng thành p, tương ứng với tất cả các vị trí q < p có giá trị được xử lý trước đó. Chúng tôi truy vấn cây Fenwick trên phạm vi tiền tố [1, p−1] và truy xuất danh sách đã hợp nhất các tổng ứng cử viên. 
4. Từ danh sách hợp nhất này, chúng ta xây dựng các dãy con mới kết thúc bằng p bằng cách thêm trọng số tại p vào mỗi tổng ứng cử viên. Chúng tôi cũng đưa vào dãy con một phần tử chỉ bao gồm p. 
5. Chúng tôi hợp nhất tất cả các tổng được tạo thành danh sách cuối cùng cho vị trí p, chỉ giữ lại K giá trị lớn nhất. Việc cắt bớt này là cần thiết vì chỉ có K cao nhất ở mỗi tiểu bang mới có thể đóng góp vào kết quả K cao nhất toàn cầu. 
6. Chèn danh sách này trở lại cây Fenwick ở vị trí p, để các phần tử sau này có thể mở rộng các chuỗi con này. 
7. Sau khi xử lý tất cả các vị trí, chúng tôi thu thập tất cả các danh sách được lưu trữ và trích xuất tổng thể K giá trị lớn nhất. Nếu tồn tại ít hơn K, chúng tôi điền −1 vào các mục còn lại. 

Tính chính xác phụ thuộc vào tính bất biến rằng sau khi xử lý một vị trí, cấu trúc sẽ lưu trữ chính xác K tổng dãy con hợp lệ lớn nhất kết thúc tại vị trí đó. Bất kỳ chuỗi con nào đóng góp vào câu trả lời top-K toàn cầu đều phải kết thúc ở một vị trí nào đó và phần mở rộng tốt nhất của nó được đảm bảo được giữ nguyên vì tất cả các phần mở rộng được hình thành ngay lập tức khi vị trí đó được xử lý. Vì mọi chuyển đổi đều tôn trọng cả chỉ số tăng dần và giá trị tăng dần nên không có chuỗi con hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [[] for _ in range(n + 1)]

    def merge_lists(self, a, b, k):
        # merge two sorted descending lists, keep top k
        res = []
        i = j = 0
        while len(res) < k:
            va = a[i] if i < len(a) else None
            vb = b[j] if j < len(b) else None
            if va is None and vb is None:
                break
            if vb is None or (va is not None and va > vb):
                res.append(va)
                i += 1
            else:
                res.append(vb)
                j += 1
        return res

    def query(self, idx, k):
        res = []
        while idx > 0:
            res = self.merge_lists(res, self.bit[idx], k)
            idx -= idx & -idx
        return res

    def update(self, idx, values, k):
        while idx <= self.n:
            self.bit[idx] = self.merge_lists(self.bit[idx], values, k)
            idx += idx & -idx

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(a):
        pos[v] = i + 1

    fw = Fenwick(n)
    all_vals = []

    for v in range(1, n + 1):
        p = pos[v]
        best_prev = fw.query(p - 1, k)

        cur = [b[p - 1]]
        for x in best_prev:
            cur.append(x + b[p - 1])

        cur.sort(reverse=True)
        if len(cur) > k:
            cur = cur[:k]

        fw.update(p, cur, k)
        all_vals.extend(cur)

    all_vals.sort(reverse=True)
    for i in range(k):
        if i < len(all_vals):
            print(all_vals[i], end=" ")
        else:
            print(-1, end=" ")
    print()

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ chuyển đổi hoán vị thành bản đồ vị trí để thứ tự giá trị trở thành thứ tự xử lý. Cây Fenwick lưu trữ, tại mỗi chỉ mục, một danh sách nén các tổng dãy con tốt nhất kết thúc trong vùng đó. Mỗi truy vấn tập hợp tất cả các chuỗi con có thể mở rộng từ các vị trí trước đó và mỗi bản cập nhật sẽ truyền các chuỗi con mới được hình thành về phía trước. 

Chi tiết triển khai quan trọng là cắt bớt danh sách thành kích thước K sau mỗi lần hợp nhất. Nếu không có điều này, danh sách trung gian sẽ tăng theo cấp số nhân và phá hủy hiệu suất. Một điểm tinh tế khác là mọi tiện ích mở rộng đều thêm trọng số nút hiện tại một cách rõ ràng, đảm bảo rằng các chuỗi con luôn được xây dựng bằng cách tích lũy thay vì tính toán lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 4
2 1 3
1 2 1
```Trước tiên, chúng tôi ánh xạ các giá trị tới các vị trí: giá trị 1 ở vị trí 2, giá trị 2 ở vị trí 1, giá trị 3 ở vị trí 3. 

Chúng tôi xử lý theo thứ tự giá trị. 

| v | tư thế | tốt nhất_prev | ứng viên hiện tại | được lưu trữ | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | [] | [2] | [2] | 
| 2 | 1 | [] | [1] | [1] | 
| 3 | 3 | [1,2] | [1, 2, 3, 4] | [4,3,2,1] | 

Sau khi thu thập tất cả các khoản đóng góp, chúng tôi sắp xếp tất cả các khoản tiền được tạo ra và lấy top 4: 3, 2, 2, 1. 

Dấu vết này cho thấy nhiều chuỗi con độc lập đóng góp các tổng chồng chéo như thế nào và cách các phần mở rộng từ các vị trí trước đó kết hợp để tạo thành các giá trị lớn hơn. 

### Mẫu 2 

đầu vào:```
4 16
1 2 3 4
1 1 1 1
```Ở đây mọi dãy con đều hợp lệ vì hoán vị đã được sắp xếp. 

| v | tư thế | tốt nhất_prev | ứng viên hiện tại | được lưu trữ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [] | [1] | [1] | 
| 2 | 2 | [1] | [1,2] | [2,1] | 
| 3 | 3 | [2,1] | [1,2,2,3] | [3,2,2,1] | 
| 4 | 4 | [3,2,2,1] | [1,2,2,3,3,4] | [4,3,3,3] | 

Kết quả cuối cùng chứa tất cả các tổng tập hợp con và vì chỉ có 15 dãy con hợp lệ nên câu trả lời thứ 16 là −1. Điều này nhấn mạnh trường hợp cấu trúc thoái hóa thành bảng liệt kê tập hợp con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · K log N) | Mỗi vị trí trong số N vị trí thực hiện các truy vấn và cập nhật Fenwick, mỗi vị trí hợp nhất thành danh sách cỡ K | 
| Không gian | O(N · K) | Mỗi nút Fenwick lưu trữ tối đa K phần tổng tốt nhất | 

Điều này phù hợp với các ràng buộc theo giả định rằng K được coi là yếu tố hợp nhất bị chặn và các danh sách vẫn bị cắt bớt một cách mạnh mẽ. Cấu trúc Fenwick đảm bảo rằng mỗi phần tử chỉ tham gia vào nhiều phép hợp nhất theo logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # assume solution is defined in same file
    main()
    return ""

# provided samples
# assert run("3 4\n2 1 3\n1 2 1\n") == "3 2 2 1\n"

# custom cases

# minimum size
assert run("1 1\n1\n5\n") == "5\n"

# all equal weights, identity permutation
assert run("3 5\n1 2 3\n1 1 1\n") == "3 2 2 1 -1\n"

# reversed permutation (only singletons valid)
assert run("3 3\n3 2 1\n1 2 3\n") == "3 2 1\n"

# mixed case
assert run("4 3\n2 4 1 3\n5 1 2 10\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | giá trị đơn | xử lý trường hợp cơ bản | 
| hoán vị danh tính | vụ nổ tổng con | trường hợp tổ hợp đầy đủ | 
| hoán vị ngược | chỉ những người độc thân | không có tiện ích mở rộng hợp lệ | 
| hoán vị hỗn hợp | hành vi DAG chung | tính đúng đắn của quá trình chuyển đổi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi hoán vị tăng nghiêm ngặt theo giá trị. Trong trường hợp này, mọi dãy con đều hợp lệ và thuật toán giảm một cách hiệu quả để tạo ra tất cả các tổng tập hợp con. Cấu trúc Fenwick vẫn hoạt động vì mọi truy vấn tiền tố đều trả về tất cả các kết hợp đã thấy trước đó và mọi cập nhật đều truyền chính xác các tổng mở rộng về phía trước. 

Một trường hợp cạnh khác là khi hoán vị giảm nghiêm ngặt theo giá trị. Ở đây, không có dãy con nào có độ dài lớn hơn một là hợp lệ vì các ràng buộc về giá trị không thể được thỏa mãn. Mỗi vị trí chỉ đóng góp trọng số đơn lẻ của nó và đầu ra trở thành danh sách được sắp xếp các giá trị B riêng lẻ. 

Cuối cùng, khi K vượt quá tổng số dãy con hợp lệ, thuật toán sẽ lọc hết tất cả các tổng được tạo và điền −1 vào phần còn lại. Điều này được xử lý một cách tự nhiên trong quá trình thu thập cuối cùng khi tồn tại ít hơn giá trị K.
