---
title: "CF 98A - Giúp đỡ Victoria the Wise"
description: "Chúng tôi có sáu viên đá quý, mỗi viên có một trong sáu màu có thể. Sáu viên ngọc phải được đặt trên sáu mặt của khối lập phương. Hai vị trí được coi là giống nhau nếu vị trí này có thể được xoay sang vị trí kia. Đầu vào chỉ đơn giản là một chuỗi có độ dài sáu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 98
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 78 (Div. 1 Only)"
rating: 1700
weight: 98
solve_time_s: 132
verified: true
draft: false
---

[CF 98A - Giúp đỡ Victoria the Wise](https://codeforces.com/problemset/problem/98/A) 

**Đánh giá:** 1700 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có sáu viên đá quý, mỗi viên có một trong sáu màu có thể. Sáu viên ngọc phải được đặt trên sáu mặt của khối lập phương. Hai vị trí được coi là giống nhau nếu vị trí này có thể được xoay sang vị trí kia. 

Đầu vào chỉ đơn giản là một chuỗi có độ dài sáu. Mỗi ký tự đại diện cho màu sắc của một viên đá quý. Ký tự giống nhau có nghĩa là đá quý không thể phân biệt được. Ví dụ, đầu vào`RGBRGB`nghĩa là có hai viên ngọc đỏ, hai viên ngọc xanh và hai viên ngọc xanh. 

Chúng ta phải đếm xem có thể hình thành được bao nhiêu màu khối lập phương khác nhau về cơ bản. Việc quay khối lập phương không tạo ra câu trả lời mới. 

Kích thước của đầu vào là nhỏ. Chỉ có sáu vị trí trên khối lập phương, do đó, ngay cả phương pháp hoán vị mạnh mẽ cũng khả thi. Số lượng sắp xếp thô lớn nhất là`6! = 720`, cực kỳ nhỏ. Thách thức thực sự là xử lý tương đương quay một cách chính xác. 

Phần nguy hiểm của vấn đề này là phép quay khối rất dễ bị đánh giá thấp. Một cách triển khai đơn giản có thể tính riêng hai khối được xoay mặc dù chúng thể hiện cùng một kiểu trang trí. 

Hãy xem xét đầu vào:```
RGBYOV
```Tất cả các màu sắc đều khác biệt. Một giải pháp bất cẩn có thể trả lời`720`, bởi vì có`6!`hoán vị. Điều đó là sai. Mỗi khối vật lý có 24 đối xứng quay, vì vậy nhiều hoán vị sẽ xếp vào cùng một lớp tương đương. Câu trả lời đúng là`720 / 24 = 30`. 

Một lỗi dễ dàng khác xuất hiện khi tồn tại các màu trùng lặp. 

Ví dụ:```
RRGGGG
```Nếu chúng ta chỉ chia cho 24 một cách máy móc thì chúng ta sẽ trở nên vô nghĩa vì các màu lặp lại sẽ tạo ra các cấu hình chồng chéo. Chúng ta phải so sánh các trạng thái khối thực tế khi quay chứ không phải dựa vào các công thức tổ hợp. 

Một trường hợp tinh vi hơn là khi tất cả các màu đều giống hệt nhau:```
YYYYYY
```Chỉ có một trang trí hợp lệ. Mọi hoán vị và mọi phép quay đều tạo ra cùng một khối. Một giải pháp dựa trên hoán vị mà không có sự trùng lặp sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Chúng tôi tạo ra mọi hoán vị của sáu viên ngọc, diễn giải từng hoán vị dưới dạng màu của các mặt khối, sau đó nhóm tất cả các hoán vị tương đương với nhau khi xoay khối. 

Điều này hiệu quả vì tổng không gian tìm kiếm rất nhỏ. Ngay cả với tất cả các màu khác nhau, chỉ có 720 hoán vị. Đối với mỗi hoán vị, chúng ta có thể kiểm tra tất cả 24 phép quay khối. Tổng số công việc vẫn ở mức thoải mái dưới vài chục nghìn thao tác. 

Câu hỏi thực sự là làm thế nào để biểu diễn phép quay một cách rõ ràng. 

Một khối lập phương có đúng 24 hướng khác nhau. Nếu chúng ta đánh số các mặt từ 0 đến 5 thì mỗi phép quay sẽ trở thành một hoán vị của các chỉ số này. Khi chúng tôi tính toán trước 24 hoán vị đó, hai màu sẽ tương đương nhau nếu một màu có thể được chuyển đổi thành màu kia bằng cách sử dụng bất kỳ hoán vị nào trong số này. 

Phiên bản brute-force có thể so sánh mọi hoán vị với mọi hoán vị đã thấy trước đó bằng cách sử dụng tất cả 24 phép quay. Điều đó vẫn trôi qua vì đầu vào quá nhỏ nhưng chúng ta có thể làm cho nó sạch hơn. 

Quan sát quan trọng là mọi lớp tương đương đều có một đại diện chính tắc. Đối với bất kỳ màu nào, chúng tôi tạo tất cả 24 phiên bản xoay và chọn phiên bản nhỏ nhất về mặt từ điển. Mỗi khối trong cùng một lớp quay ánh xạ tới cùng một dạng chính tắc. 

Điều đó biến vấn đề thành sự trùng lặp tập hợp đơn giản. 

Brute-force hoạt động vì không gian trạng thái rất nhỏ, nhưng nó trở nên bất tiện khi kiểm tra tính tương đương theo cặp nhiều lần. Việc quan sát dạng kinh điển làm giảm toàn bộ nhiệm vụ thành: 

1. Tạo tất cả các hoán vị riêng biệt. 
2. Chuẩn hóa từng hoán vị. 
3. Đếm các trạng thái chuẩn hóa duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force so sánh theo cặp | O(720² × 24) | O(720) | Đã chấp nhận | 
| Đại diện kinh điển | O(720 × 24) | O(720) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đánh số sáu mặt khối lập phương từ 0 đến 5. 

Chúng tôi chỉ cần sự nhất quán. Một quy ước chung là: 

- 0 = trên cùng 
- 1 = dưới cùng 
- 2 = phía trước 
- 3 = quay lại 
- 4 = trái 
- 5 = đúng 
2. Tính toán trước tất cả 24 phép quay khối. 

Mỗi phép quay được biểu diễn dưới dạng hoán vị của sáu chỉ số mặt. Ví dụ: một vòng quay có thể di chuyển mặt trên về phía trước, mặt trước xuống dưới, v.v. 

Tính đúng đắn của toàn bộ giải pháp phụ thuộc vào việc tạo ra chính xác 24 hướng khối vật lý, không hơn không kém. 
3. Tạo mọi hoán vị riêng biệt của màu đầu vào. 

Vì các màu giống nhau không thể phân biệt được nên chúng tôi sử dụng một tập hợp để tự động loại bỏ các hoán vị trùng lặp. 
4. Đối với mỗi hoán vị, tạo tất cả 24 phiên bản xoay. 

Áp dụng phép quay có nghĩa là sắp xếp lại sáu màu mặt theo hoán vị phép quay. 
5. Chọn phiên bản xoay nhỏ nhất về mặt từ điển. 

Điều này trở thành dạng chuẩn mực của việc tô màu khối lập phương. 

Mọi màu tương đương xoay đều tạo ra cùng một dạng chính tắc vì chúng đều tạo ra cùng một tập hợp gồm 24 trạng thái xoay. 
6. Chèn biểu mẫu chính tắc vào một bộ. 

Bộ tự động loại bỏ các bản sao. 
7. In kích thước của bộ. 

Số lượng đó bằng với số lượng đồ trang trí hình khối riêng biệt. 

### Tại sao nó hoạt động 

Hai màu khối lập phương thuộc cùng một lớp câu trả lời chính xác khi cái này có thể được xoay thành cái kia. Quá trình chuẩn hóa của chúng tôi liệt kê rõ ràng tất cả các phép quay có thể có của màu khối và chọn một đại diện duy nhất, trạng thái xoay nhỏ nhất về mặt từ điển. 

Nếu hai màu tương đương về mặt quay thì tập hợp trạng thái quay của chúng giống hệt nhau, do đó chúng có cùng dạng chính tắc. 

Nếu hai cách tô màu không tương đương về mặt quay thì tập hợp phép quay của chúng không bao giờ giao nhau, do đó dạng chính tắc của chúng khác nhau. 

Do đó, tập hợp các dạng chính tắc chứa một phần tử cho mỗi lớp tương đương, đây chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
from itertools import permutations
from collections import deque

input = sys.stdin.readline

def compose(a, b):
    return tuple(a[b[i]] for i in range(6))

def solve():
    s = input().strip()

    # Face order:
    # 0 = top
    # 1 = bottom
    # 2 = front
    # 3 = back
    # 4 = left
    # 5 = right

    # Basic cube rotations
    rot_x = (2, 3, 1, 0, 4, 5)
    rot_y = (0, 1, 4, 5, 3, 2)
    rot_z = (4, 5, 2, 3, 1, 0)

    rotations = set()
    q = deque()

    identity = (0, 1, 2, 3, 4, 5)
    rotations.add(identity)
    q.append(identity)

    # Generate all 24 cube rotations
    while q:
        cur = q.popleft()

        for rot in (rot_x, rot_y, rot_z):
            nxt = compose(rot, cur)

            if nxt not in rotations:
                rotations.add(nxt)
                q.append(nxt)

    rotations = list(rotations)

    canonicals = set()

    for p in set(permutations(s)):
        best = None

        for rot in rotations:
            transformed = tuple(p[rot[i]] for i in range(6))

            if best is None or transformed < best:
                best = transformed

        canonicals.add(best)

    print(len(canonicals))

if __name__ == "__main__":
    solve()
```Phần quan trọng đầu tiên là tạo vòng quay khối. Thay vì mã hóa cứng tất cả 24 hướng theo cách thủ công, giải pháp bắt đầu từ hướng nhận dạng và áp dụng liên tục ba phép quay cơ bản quanh trục tọa độ. Bản mở rộng theo chiều rộng đầu tiên cuối cùng sẽ phát hiện ra tất cả 24 hướng hình khối độc đáo. 

các`compose`hàm kết hợp hai hoán vị. Đây là hoạt động cốt lõi cho phép chúng ta xây dựng các phép quay phức tạp từ những phép quay đơn giản. 

Bước tạo hoán vị sử dụng:```
set(permutations(s))
```Điều này là cần thiết. Nếu đầu vào chứa các màu lặp lại, thông thường`permutations`sẽ tạo ra nhiều bản sao. 

Bước chuẩn hóa là trung tâm của thuật toán. Đối với mọi cách sắp xếp, chúng tôi áp dụng tất cả 24 phép quay và giữ lại bộ dữ liệu nhỏ nhất theo từ điển. Việc sử dụng bộ dữ liệu thay vì chuỗi giúp việc so sánh trở nên đơn giản và hiệu quả. 

Một lỗi triển khai phổ biến là vô tình đảo ngược hướng hoán vị. Biểu thức:```
tuple(p[rot[i]] for i in range(6))
```có nghĩa là "màu sắc bây giờ xuất hiện trên khuôn mặt`i`đến từ khuôn mặt`rot[i]`". Tính nhất quán quan trọng hơn quy ước chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
YYYYYY
```Tất cả các hoán vị đều giống hệt nhau. 

| Hoán vị | Mẫu kinh điển | 
| --- | --- | 
| YYYYYY | YYYYYY | 

Kích thước đặt cuối cùng:```
1
```Ví dụ này cho thấy các màu trùng lặp thu gọn nhiều hoán vị thành một cấu hình duy nhất. Mọi vòng quay cũng vẫn giống hệt nhau. 

### Ví dụ 2 

đầu vào:```
RGBYOV
```Tất cả các màu sắc đều khác biệt. 

Giả sử một hoán vị là: 

| Khuôn mặt gốc | Tiểu bang | 
| --- | --- | 
| 0 1 2 3 4 5 | RGBYOV | 

Một số phép quay tạo ra: 

| Xoay | Kết quả | 
| --- | --- | 
| Bản sắc | RGBYOV | 
| Xoay X | BYGRVO | 
| Xoay Y | RGOVYB | 
| Xoay Z | OVRGBY | 

Trong số tất cả 24 trạng thái xoay, trạng thái nhỏ nhất về mặt từ điển sẽ trở thành dạng chính tắc. 

Sau khi xử lý tất cả 720 hoán vị, vẫn còn chính xác 30 dạng chính tắc riêng biệt. 

| Số lượng | Giá trị | 
| --- | --- | 
| Hoán vị thô | 720 | 
| Xoay khối | 24 | 
| Màu sắc riêng biệt | 30 | 

Ví dụ này cho thấy sự sụp đổ quay. Nhiều phép gán khuôn mặt có vẻ khác nhau thực ra lại biểu diễn cùng một khối vật lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(720 × 24) | Tối đa 720 hoán vị riêng biệt, mỗi hoán vị được kiểm tra dưới 24 phép quay | 
| Không gian | O(720) | Lưu trữ cho các trạng thái chuẩn | 

Tổng công việc là rất nhỏ. Ngay cả trong trường hợp xấu nhất với sáu màu riêng biệt, chương trình chỉ thực hiện được vài nghìn thao tác. Điều này dễ dàng phù hợp trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import permutations
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def compose(a, b):
        return tuple(a[b[i]] for i in range(6))

    s = input().strip()

    rot_x = (2, 3, 1, 0, 4, 5)
    rot_y = (0, 1, 4, 5, 3, 2)
    rot_z = (4, 5, 2, 3, 1, 0)

    rotations = set()
    q = deque()

    identity = (0, 1, 2, 3, 4, 5)

    rotations.add(identity)
    q.append(identity)

    while q:
        cur = q.popleft()

        for rot in (rot_x, rot_y, rot_z):
            nxt = compose(rot, cur)

            if nxt not in rotations:
                rotations.add(nxt)
                q.append(nxt)

    canonicals = set()

    for p in set(permutations(s)):
        best = None

        for rot in rotations:
            transformed = tuple(p[rot[i]] for i in range(6))

            if best is None or transformed < best:
                best = transformed

        canonicals.add(best)

    return str(len(canonicals)) + "\n"

# provided sample
assert run("YYYYYY\n") == "1\n", "sample 1"

# all distinct colors
assert run("ROYGBV\n") == "30\n", "all distinct"

# two groups of three
assert run("RRRGGG\n") == "2\n", "three and three"

# one unique gem
assert run("RBBBBB\n") == "1\n", "single special face"

# symmetric pair structure
assert run("RRGGBB\n") == "6\n", "pair symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`YYYYYY`|`1`| Cấu hình hoàn toàn giống nhau | 
|`ROYGBV`|`30`| Số lượng trạng thái riêng biệt tối đa | 
|`RRRGGG`|`2`| Tương đương quay không cần thiết | 
|`RBBBBB`|`1`| Một mặt độc đáo luôn có thể xoay mọi nơi | 
|`RRGGBB`|`6`| Nhiều màu lặp lại với tính đối xứng một phần | 

## Vỏ cạnh 

Xét trường hợp đối xứng hoàn toàn:```
YYYYYY
```Chỉ có một hoán vị riêng biệt. Mỗi vòng quay cũng tạo ra sự sắp xếp tương tự:```
YYYYYY
```Bộ chính tắc nhận chính xác một phần tử, do đó thuật toán đưa ra`1`. 

Bây giờ hãy xem xét:```
RBBBBB
```Thoạt nhìn, dường như có sáu khả năng vì viên ngọc đỏ có thể chiếm giữ bất kỳ khuôn mặt nào. Sự luân chuyển phá hủy sự khác biệt này. Bất kỳ mặt nào cũng có thể được xoay thành bất kỳ mặt nào khác trên khối lập phương. 

Trong quá trình chuẩn hóa, tất cả sáu hoán vị thô đều ánh xạ tới cùng một trạng thái chuẩn hóa. Thuật toán xuất ra chính xác`1`. 

Cuối cùng, hãy xem xét:```
RRRGGG
```Một cách tiếp cận tổ hợp đơn giản có thể cho rằng mọi sự sắp xếp đều tương đương nhau, nhưng điều đó là sai. Thực tế có hai cấu trúc khác nhau về cơ bản: 

1. Ba khuôn mặt đỏ gặp nhau ở một góc. 
2. Ba mặt đỏ bao quanh một mặt theo chu kỳ. 

Quá trình chuẩn hóa sẽ tự động phân biệt những điều này vì không có phép quay khối nào có thể biến đổi cấu trúc này thành cấu trúc khác. Câu trả lời cuối cùng trở thành`2`.
