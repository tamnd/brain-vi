---
title: "CF 106190A - \u041f\u043e\u0447\u0438\u043d\u043a\u0430 \u0434\u0432\u0438\u0433\u0430\u0442\u0435\u043b\u044f <<\u041f\u0435\u0433\u0430\u0441\u0430>>"
description: "Vấn đề đưa ra hai mảng có cùng độ dài. Một mảng, gọi là mảng, đã được sửa và thể hiện trạng thái hư hỏng của hệ thống."
date: "2026-06-25T10:44:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106190
codeforces_index: "A"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2025-2026. \u041f\u0435\u0440\u0432\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 106190
solve_time_s: 41
verified: true
draft: false
---

[CF 106190A - \u041f\u043e\u0447\u0438\u043d\u043a\u0430 \u0434\u0432\u0438\u0433\u0430\u0442\u0435\u043b\u044f <<\u041f\u0435\u0433\u0430\u0441\u0430>>](https://codeforces.com/problemset/problem/106190/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề đưa ra hai mảng có cùng độ dài. Một mảng, gọi nó là`a`, đã được sửa và thể hiện trạng thái bị hỏng của hệ thống. Chúng ta cần xây dựng một mảng khác`b`có cùng kích thước, với mỗi phần tử nằm trong khoảng từ 0 đến 10^9, với một ràng buộc rất cụ thể: cho mọi vị trí`i`, sự khác biệt tuyệt đối`|a_i - b_i|`hẳn là kỳ quặc. 

Nói lại theo cách đơn giản hơn, mỗi vị trí là độc lập và với mỗi chỉ mục, chúng ta phải chọn một số`b_i`có sự tương đương ngược lại với`a_i`. Nếu như`a_i`là chẵn,`b_i`hẳn là kỳ quặc. Nếu như`a_i`thật kỳ lạ,`b_i`phải chẵn. 

Ràng buộc về độ lớn của các giá trị không bị hạn chế ở đây, vì cả 0 và 1 đều là đại diện hợp lệ cho số chẵn và số lẻ và chúng luôn nằm trong phạm vi cho phép. 

Kích thước đầu vào có thể lớn, tổng cộng lên tới 2·10^5 phần tử trong tất cả các trường hợp thử nghiệm. Điều đó ngay lập tức gợi ý rằng bất kỳ phép tính nặng bậc hai hoặc thậm chí log-tuyến tính nào cho mỗi phần tử là không cần thiết. Việc xây dựng tuyến tính trực tiếp cho mỗi trường hợp thử nghiệm là đủ vì chúng ta chỉ quyết định từng`b_i`một cách độc lập. 

Một điểm tinh tế đáng kiểm tra là liệu có bất kỳ mối liên kết ẩn nào giữa các chỉ số hay không. Cách đọc ngây thơ có thể gợi ý một số cấu trúc toàn cục, nhưng điều kiện hoàn toàn là cho mỗi phần tử. Điều đó loại bỏ mọi nhu cầu sắp xếp, lập trình động hoặc cân bằng tham lam. 

Các trường hợp cạnh chủ yếu liên quan đến giá trị chẵn lẻ và giá trị biên. Ví dụ, nếu`a_i = 0`, chúng ta vẫn phải tạo ra một số lẻ, và nếu`a_i = 10^9`, chúng ta vẫn cần điều chỉnh chẵn hoặc lẻ nằm trong phạm vi. Điều này luôn luôn có thể thực hiện được. 

## Phương pháp tiếp cận 

Một tư duy vũ phu sẽ cố gắng gán các giá trị cho`b`bằng cách quét tất cả các ứng viên có thể cho từng vị trí và kiểm tra xem điều kiện chẵn lẻ có đúng hay không. Đối với mỗi chỉ số`i`, người ta có thể lặp lại tất cả các giá trị từ 0 đến 10^9 và chọn một giá trị hợp lệ. Về nguyên tắc, điều này đúng vì điều kiện dễ xác minh, nhưng không gian tìm kiếm cho mỗi phần tử là 10^9 khả năng, dẫn đến khoảng 10^14 thao tác trong trường hợp xấu nhất khi`n`là lớn. Điều đó hoàn toàn không thể thực hiện được. 

Quan sát chính là ràng buộc không yêu cầu tối ưu hóa, chỉ yêu cầu tính khả thi và nó chỉ phụ thuộc vào tính chẵn lẻ. Khi điều kiện được giảm xuống thành khớp chẵn lẻ, mỗi vị trí có chính xác hai lớp tương đương: các số có cùng chẵn lẻ và các số có chẵn lẻ ngược lại. Chúng tôi luôn được tự do chọn bất kỳ đại diện nào từ đúng lớp. 

Điều này thu gọn vấn đề từ “tìm kiếm trong phạm vi rộng lớn” thành “chọn một trong hai giá trị cố định cho mỗi phần tử”. Một cách xây dựng thuận tiện là lập bản đồ mọi`a_i`đến 1 và mọi số lẻ`a_i`về 0. Điều này luôn thỏa mãn điều kiện lật chẵn lẻ và nằm trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các giá trị b) | O(n · 10^9) | O(1) | Quá chậm | 
| Xây dựng bình đẳng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`. Chúng ta không cần lưu trữ bất cứ thứ gì ngoài việc xử lý từng giá trị, nhưng việc lưu trữ sẽ giúp đơn giản hóa việc triển khai. 
2. Đối với mỗi phần tử`a_i`, xác định tính chẵn lẻ của nó. Điều này được thực hiện bằng cách kiểm tra`a_i % 2`. 
3. Nếu`a_i`chẵn, gán`b_i = 1`. Điều này đảm bảo`|a_i - b_i|`là số lẻ vì số chẵn trừ số lẻ luôn là số lẻ. 
4. Nếu`a_i`là lẻ, gán`b_i = 0`. Điều này một lần nữa đảm bảo một sự khác biệt lẻ vì số lẻ trừ số chẵn là số lẻ. 
5. Xuất mảng đã xây dựng. 

Lý do đằng sau bước 3 và 4 là chúng tôi cố tình đảo ngược tính chẵn lẻ bằng cách sử dụng các đại diện nhỏ nhất có thể. Sử dụng 0 và 1 là đủ vì tính chẵn lẻ là bất biến khi cộng 2 và chúng tôi chỉ quan tâm đến việc hiệu có chia hết cho 2 hay không. 

### Tại sao nó hoạt động 

Bất biến được duy trì là với mọi chỉ số`i`, tính chẵn lẻ của`b_i`luôn khác với tính chẵn lẻ của`a_i`. Vì tính chẵn lẻ xác định đầy đủ xem sự khác biệt là chẵn hay lẻ, điều này đảm bảo rằng`|a_i - b_i|`luôn luôn kỳ quặc. Không có sự tương tác nào tồn tại giữa các chỉ số, do đó việc thỏa mãn từng vị trí một cách độc lập là đủ để thỏa mãn toàn bộ ràng buộc mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    res = []
    for x in a:
        if x % 2 == 0:
            res.append(1)
        else:
            res.append(0)
    
    print(*res)
```Giải pháp đọc từng trường hợp kiểm thử một cách độc lập và xây dựng câu trả lời trong một lần thực hiện. Chi tiết triển khai duy nhất quan trọng là giữ cho việc tạo đầu ra đơn giản vì việc in các vòng lặp bên trong có thể chậm hơn đối với đầu vào lớn. 

Một lỗi phổ biến ở đây là cố gắng “tối ưu hóa” các giá trị bằng cách chọn những thứ như`x + 1`hoặc`x - 1`. Điều đó cũng có giá trị trong nhiều trường hợp, nhưng nó không cần thiết và có thể gây ra rủi ro tràn hoặc các vấn đề về ranh giới khi`a_i = 10^9`. Sử dụng đại diện chẵn lẻ cố định sẽ tránh được tất cả các vấn đề như vậy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [1, 2, 3]
```Chúng tôi xử lý từng phần tử: 

| tôi | a_i | chẵn lẻ | đã chọn b_i | |a_i - b_i| | 

|---|------|--------|-------------|-------------| 

| 1 | 1 | lẻ | 0 | 1 | 

| 2 | 2 | thậm chí | 1 | 1 | 

| 3 | 3 | lẻ | 0 | 3 | 

Đầu ra:```
0 1 0
```Dấu vết này cho thấy mọi vị trí đều thỏa mãn điều kiện sai phân lẻ một cách độc lập mà không cần bất kỳ sự phối hợp nào. 

### Ví dụ 2 

đầu vào:```
a = [0, 4, 7, 10]
```| tôi | a_i | chẵn lẻ | đã chọn b_i | |a_i - b_i| | 

|---|------|--------|-------------|-------------| 

| 1 | 0 | thậm chí | 1 | 1 | 

| 2 | 4 | thậm chí | 1 | 3 | 

| 3 | 7 | lẻ | 0 | 7 | 

| 4 | 10 | thậm chí | 1 | 9 | 

Đầu ra:```
1 1 0 1
```Ví dụ này xác nhận rằng ngay cả ở các giá trị biên như 0 và các số chẵn lớn, việc xây dựng vẫn hợp lệ và nằm trong các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần với công việc O(1) | 
| Không gian | O(n) | Lưu trữ cho mảng đầu ra | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2·10^5, do đó, một lần truyền tuyến tính duy nhất qua tất cả các phần tử có thể dễ dàng đủ nhanh trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng ở mức tối thiểu vì chỉ có mảng đầu ra được lưu trữ cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        res = []
        for x in a:
            res.append(1 if x % 2 == 0 else 0)
        out.append(" ".join(map(str, res)))
    return "\n".join(out)

# provided sample
assert run("""1
3
1 2 3
""") == "0 1 0"

# all equal even
assert run("""1
4
2 2 2 2
""") == "1 1 1 1"

# all equal odd
assert run("""1
5
1 1 1 1 1
""") == "0 0 0 0 0"

# mixed boundaries
assert run("""1
4
0 1 10 11
""") == "1 0 1 0"

# single element
assert run("""1
1
7
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả thậm chí | tất cả 1s | thậm chí xử lý | 
| tất cả đều kỳ quặc | tất cả số 0 | xử lý kỳ quặc | 
| hỗn hợp | đầu ra chẵn lẻ xen kẽ | tính đúng đắn của các giá trị | 
| phần tử đơn | 0 | trường hợp tối thiểu | 

## Vỏ cạnh 

cho`a_i = 0`, công trình vẫn hoạt động trơn tru: vì 0 là số chẵn nên ta gán`b_i = 1`, cho`|0 - 1| = 1`, là số lẻ và nằm trong giới hạn. Không cần xử lý đặc biệt. 

Đối với các giá trị lớn như`a_i = 10^9`, tính chẵn lẻ vẫn được xác định rõ ràng và việc gán 0 hoặc 1 sẽ giữ cho kết quả hợp lệ. Không có nguy cơ tràn hoặc vi phạm ranh giới vì chúng tôi không bao giờ cố gắng sao chép hoặc tăng giá trị đầu vào. 

Đối với các đầu vào chẵn lẻ xen kẽ, mỗi chỉ mục là độc lập, do đó, đầu ra chỉ cần lật phần tử chẵn lẻ mà không có bất kỳ sự can thiệp nào, xác nhận rằng không tồn tại ràng buộc nhất quán toàn cầu.
