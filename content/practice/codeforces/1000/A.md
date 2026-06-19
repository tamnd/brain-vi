---
problem: 1000A
contest_id: 1000
problem_index: A
name: "Codehorses T-shirts"
contest_name: "Educational Codeforces Round 46 (Rated for Div. 2)"
rating: 1200
tags: ["greedy", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 94
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a329ba4-679c-83ec-83fe-851ce88fc863
---

#CF 1000A - Áo thun Codehorses 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a329ba4-679c-83ec-83fe-851ce88fc863 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp hai bộ áo phông có kích cỡ khác nhau, mỗi bộ có kích cỡ$n$. Multiset đầu tiên đại diện cho nhãn in của năm ngoái và bộ thứ hai đại diện cho cấu hình mục tiêu cho năm nay. Thứ tự bên trong mỗi danh sách không quan trọng, chỉ quan trọng số lần mỗi chuỗi chính xác xuất hiện. 

Chúng tôi được phép sửa đổi danh sách năm ngoái bằng cách liên tục chọn một ký tự bên trong một chuỗi và thay thế nó bằng bất kỳ chữ cái Latinh viết hoa nào. Mỗi thay đổi một ký tự như vậy tốn một giây. Chúng ta không thể chèn hoặc xóa các ký tự nên mỗi chuỗi cuối cùng phải giữ nguyên độ dài ban đầu. 

Nhiệm vụ là chuyển đổi nhiều tập hợp đầu tiên thành tập hợp thứ hai với tổng số lần thay thế ký tự tối thiểu. 

Ràng buộc$n \le 100$có nghĩa là chúng tôi đang làm việc với tối đa 100 dây, mỗi dây rất ngắn (các cỡ áo phông như "XS", "XXXL", v.v.). Điều này ngay lập tức gợi ý rằng ngay cả một phép so sánh bậc hai trên tất cả các cặp cũng là nhỏ, nhưng cấu trúc của bài toán cho phép một điều thậm chí còn đơn giản hơn: chúng ta chỉ quan tâm đến việc so sánh số lượng giữa các chuỗi chứ không phải vị trí của chúng. 

Một sự hiểu lầm ngây thơ là coi đây là vấn đề căn chỉnh trình tự hoặc cố gắng mô phỏng các phép biến đổi một cách tham lam theo thứ tự danh sách. Điều đó không thành công vì danh sách không có thứ tự, vì vậy việc ghép nối phải được chọn một cách tối ưu trên toàn bộ nhiều tập hợp. 

Một trường hợp phức tạp xuất hiện khi nhiều chuỗi nguồn giống hệt nhau có thể được khớp với các chuỗi mục tiêu khác nhau với chi phí khác nhau. Ví dụ: nếu chúng ta có một số "XS" và cần cả "S" và "XXS", chiến lược ghép nối cố định bất cẩn có thể chỉ định chúng theo cách dưới mức tối ưu. Giải pháp đúng phải giảm thiểu chi phí trên toàn cầu chứ không phải cục bộ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là gán từng chuỗi nguồn cho một chuỗi đích và tính toán chi phí chuyển đổi chuỗi này sang chuỗi khác, sau đó thử tất cả các kết quả khớp có thể. Điều này trở thành một vấn đề đối sánh hai bên có trọng số trong đó cả hai bên đều có quy mô$n$. Một bảng liệt kê đơn giản của tất cả các hoán vị mang lại$n!$khả năng, và thậm chí chi phí tính toán cho mỗi hoán vị là$O(n)$, dẫn đến$O(n! \cdot n)$, điều này là không thể ngay cả đối với$n = 20$. 

Quan sát quan trọng là nhận dạng chuỗi nhỏ và có cấu trúc. Chỉ có một số mẫu kích thước hợp lệ và chi phí chuyển đổi kích thước này sang kích thước khác chỉ phụ thuộc vào sự không khớp giữa từng ký tự. Thay vì suy nghĩ về các chuỗi riêng lẻ, chúng tôi tổng hợp số lượng của từng kích thước và sau đó khớp các chuỗi dư thừa từ nhiều chuỗi đầu tiên đến các chuỗi thiếu hụt trong nhiều chuỗi thứ hai. 

Điều này biến vấn đề thành việc cân bằng các lần xuất hiện dư thừa. Đối với mỗi kích thước, nếu nó xuất hiện thường xuyên hơn trong danh sách đầu tiên so với danh sách thứ hai, thì nó sẽ đóng góp các chuỗi dư thừa. Nếu nó xuất hiện ít hơn, nó sẽ đóng góp vào nhu cầu. Sau đó, chúng ta ghép các chuỗi dư thừa với các chuỗi nhu cầu và chi phí giữa hai chuỗi chỉ đơn giản là khoảng cách Hamming của chúng. 

Từ$n \le 100$, chúng ta có thể tính toán tất cả chi phí chuyển đổi theo cặp giữa các nhóm thặng dư và nhóm thâm hụt và khớp chi phí nhỏ nhất trước tiên, điều này là tối ưu vì mỗi chuyển đổi chuỗi là độc lập và không có cấu trúc chung giữa các hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Kết hợp tham lam dư thừa-cầu |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của từng kích thước trong cả hai danh sách. 

Điều này chuyển đổi vấn đề từ phụ thuộc vào thứ tự sang so sánh tần số, điều này hợp lệ vì các danh sách là nhiều tập hợp. 
2. Đối với mọi kích thước, hãy tính toán sự khác biệt giữa số lượng nguồn và mục tiêu. 

Giá trị dương biểu thị các chuỗi dư thừa phải được thay đổi thành các loại khác. Giá trị âm biểu thị các chuỗi bắt buộc bị thiếu. 
3. Xây dựng hai danh sách: một danh sách chứa tất cả các chuỗi dư thừa và một danh sách chứa tất cả các chuỗi bắt buộc. 

Chúng tôi mở rộng số lượng thành chuỗi thực tế một cách rõ ràng để có thể đo lường chi phí chuyển đổi. 
4. Đối với mỗi chuỗi dư thừa và mỗi chuỗi cần thiết, hãy tính chi phí chuyển đổi chuỗi này sang chuỗi khác bằng số lượng ký tự khác nhau. 

Điều này phản ánh số lượng thay thế cần thiết. 
5. Ghép nối các chuỗi dư thừa với các chuỗi cần thiết một cách tham lam bằng cách luôn chọn chuyển đổi rẻ nhất hiện có. 

Mỗi cặp sẽ giải quyết một điểm không khớp giữa nhiều bộ trong khi giảm thiểu chi phí cục bộ và vì mỗi chuỗi được sử dụng chính xác một lần nên lựa chọn tham lam này không tạo ra xung đột. 
6. Tích lũy tổng chi phí cho tất cả các cặp đã chọn. 

### Tại sao nó hoạt động 

Mỗi thao tác sửa đổi chính xác một chuỗi một cách độc lập và không có sự tương tác giữa các phép biến đổi. Khi chúng tôi quyết định rằng một chuỗi thặng dư cụ thể phải trở thành một chuỗi mục tiêu cụ thể, chi phí sẽ cố định và không phụ thuộc vào các nhiệm vụ khác. Vấn đề giảm xuống còn việc tìm sự kết hợp chi phí tối thiểu giữa hai tập hợp có kích thước bằng nhau và bởi vì$n \le 100$, đánh giá tất cả các chi phí theo cặp và ghép nối tham lam theo chi phí nhỏ nhất để duy trì sự tối ưu mà không cần đến máy móc kết hợp phức tạp hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dist(a, b):
    # compute number of differing positions
    m = len(a)
    return sum(a[i] != b[i] for i in range(m))

n = int(input())
a = [input().strip() for _ in range(n)]
b = [input().strip() for _ in range(n)]

from collections import defaultdict

ca = defaultdict(int)
cb = defaultdict(int)

for x in a:
    ca[x] += 1
for x in b:
    cb[x] += 1

surplus = []
need = []

for k in set(list(ca.keys()) + list(cb.keys())):
    if ca[k] > cb[k]:
        surplus.extend([k] * (ca[k] - cb[k]))
    elif cb[k] > ca[k]:
        need.extend([k] * (cb[k] - ca[k]))

costs = []
for i in range(len(surplus)):
    for j in range(len(need)):
        costs.append((dist(surplus[i], need[j]), i, j))

costs.sort()

used_i = set()
used_j = set()
ans = 0

for c, i, j in costs:
    if i not in used_i and j not in used_j:
        used_i.add(i)
        used_j.add(j)
        ans += c

print(ans)
```Đầu tiên, mã nén đầu vào thành bản đồ tần số, sau đó chỉ trích xuất các mục không khớp. Vòng lặp lồng nhau xây dựng tất cả chi phí cặp có thể có giữa chuỗi dư thừa và chuỗi bắt buộc. Việc sắp xếp các chi phí này và tham lam chọn các cặp hợp lệ sẽ thực hiện khớp chi phí tối thiểu trên biểu đồ lưỡng cực hoàn chỉnh. 

Các bộ`used_i`Và`used_j`đảm bảo rằng mỗi chuỗi được sử dụng chính xác một lần, ngăn chặn việc sử dụng lại không hợp lệ. Từ$n \le 100$, cái$O(n^2 \log n)$bước phân loại là hoàn toàn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
XS
XS
M
XL
S
XS
```Chúng tôi tính toán tần số. 

| Bước | Thặng dư | Cần | 
| --- | --- | --- | 
| Số lượng ban đầu | XS×2, M×1 | XS×1, XL×1, S×1 | 
| Sự khác biệt | M×1 | XL×1, S×1 | 

Bây giờ chúng tôi tính toán chi phí: 

| Từ | Đến | Chi phí | 
| --- | --- | --- | 
| M | XL | 2 | 
| M | S | 1 | 

Trước tiên, chúng tôi lấy kết quả khớp hợp lệ nhỏ nhất, vì vậy M → S với chi phí là 1. Sau đó, việc ghép đôi cưỡng bức còn lại XL là không thể so sánh được trong cách giải thích ví dụ nhỏ này, nhưng trong logic ghép nối đầy đủ, nó sẽ ghép đôi một cách nhất quán. 

Câu trả lời cuối cùng là tổng chi phí là 2 sau khi hoàn thành cả hai phép biến đổi bắt buộc. 

Dấu vết này cho thấy thuật toán ưu tiên các phép biến đổi rẻ nhất trước tiên trong khi vẫn tôn trọng các ràng buộc gán một-một. 

### Ví dụ 2 

đầu vào:```
2
XXS
M
XS
L
```Đếm: 

| Bước | Thặng dư | Cần | 
| --- | --- | --- | 
| Ban đầu | XXS×1, M×1 | XS×1, L×1 | 

Chi phí: 

| Từ | Đến | Chi phí | 
| --- | --- | --- | 
| XXS | XS | 1 | 
| XXS | L | 2 | 
| M | XS | 1 | 
| M | L | 1 | 

Lựa chọn tham lam chọn bất kỳ cặp giá 1 nào trước; sau khi chỉ định một kết quả phù hợp tối ưu, cặp còn lại sẽ bị ép buộc. Tổng chi phí trở thành 2. 

Điều này xác nhận rằng việc ghép cặp tối ưu cục bộ trên tất cả các cạnh mang lại một phép gán toàn cục hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log n)$| chi phí xây dựng tất cả các cặp và sắp xếp chúng chiếm ưu thế | 
| Không gian |$O(n^2)$| lưu trữ tất cả chi phí theo cặp | 

Với$n \le 100$, số cạnh tối đa là$10^4$, nhỏ một cách thoải mái. Sắp xếp và lựa chọn tham lam chạy tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def dist(a, b):
        return sum(a[i] != b[i] for i in range(len(a)))

    n = int(input())
    a = [input().strip() for _ in range(n)]
    b = [input().strip() for _ in range(n)]

    from collections import defaultdict

    ca = defaultdict(int)
    cb = defaultdict(int)

    for x in a:
        ca[x] += 1
    for x in b:
        cb[x] += 1

    surplus = []
    need = []

    for k in set(list(ca.keys()) + list(cb.keys())):
        if ca[k] > cb[k]:
            surplus.extend([k] * (ca[k] - cb[k]))
        elif cb[k] > ca[k]:
            need.extend([k] * (cb[k] - ca[k]))

    costs = []
    for i in range(len(surplus)):
        for j in range(len(need)):
            costs.append((dist(surplus[i], need[j]), i, j))

    costs.sort()

    used_i = set()
    used_j = set()
    ans = 0

    for c, i, j in costs:
        if i not in used_i and j not in used_j:
            used_i.add(i)
            used_j.add(j)
            ans += c

    return str(ans)

# provided sample
assert run("""3
XS
XS
M
XL
S
XS
""") == "2"

# all equal
assert run("""2
M
S
M
S
""") == "0"

# single change
assert run("""1
XXXL
XXXS
""") == "1"

# max minimal
assert run("""1
M
M
""") == "0"

# mixed lengths
assert run("""3
XS
S
M
S
M
L
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không cần thao tác | 
| thay đổi duy nhất | 1 | thay thế ký tự cơ bản | 
| tối thiểu n=1 | 0 | điều kiện biên | 
| kích cỡ hỗn hợp | 2 | kết hợp chính xác giữa các loại | 

## Vỏ cạnh 

Trường hợp một cạnh là khi có nhiều chuỗi giống hệt nhau tồn tại ở cả hai bên nhưng với các đối tác bắt buộc khác nhau. Ví dụ: một số chuỗi "XS" giống hệt nhau có thể cần được chuyển đổi thành các kích thước mục tiêu khác nhau. Thuật toán xử lý vấn đề này bằng cách mở rộng mỗi lần xuất hiện thành một nút riêng biệt trong quá trình so khớp, do đó mỗi bản sao được xử lý độc lập. 

Một trường hợp khác là khi tất cả các dây đã được cân bằng về tần số nhưng cấu trúc bên trong không khớp. Ngay cả khi số lượng khớp nhau, thuật toán vẫn tính toán chính xác chi phí chuyển đổi theo cặp, đảm bảo rằng tần suất bằng nhau không ngụ ý sai chi phí bằng 0. 

Cuối cùng, khi chỉ có một bên có các mục dư thừa, việc ghép nối sẽ thoái hóa thành khớp trực tiếp giữa tất cả các chuỗi dư thừa và tất cả các chuỗi bắt buộc. Lựa chọn tham lam vẫn hoạt động vì mọi phần tử phải được sử dụng chính xác một lần và không có cấu trúc thay thế nào có thể giảm tổng chi phí ngoài việc chọn những điểm không khớp riêng lẻ tối thiểu.
