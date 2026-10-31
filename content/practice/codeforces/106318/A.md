---
title: "CF 106318A - \u0414\u0432\u0435 \u0434\u043e\u043c\u0438\u043d\u043e\u0448\u043a\u0438"
description: "Chúng tôi được tặng hai quân domino. Mỗi ô có hai số được viết ở đầu của nó. Chúng ta được phép xoay một ô, nghĩa là chúng ta có thể hoán đổi hai đầu của nó nhưng không thể thay đổi các con số."
date: "2026-06-19T14:44:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106318
codeforces_index: "A"
codeforces_contest_name: "\u0420\u0435\u0441\u043f\u0443\u0431\u043b\u0438\u043a\u0430\u043d\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u041f\u0443\u0442\u044c \u043a \u041e\u043b\u0438\u043c\u043f\u0443 2026"
rating: 0
weight: 106318
solve_time_s: 49
verified: true
draft: false
---

[CF 106318A - \u0414\u0432\u0435 \u0434\u043e\u043c\u0438\u043d\u043e\u0448\u043a\u0438](https://codeforces.com/problemset/problem/106318/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng hai quân domino. Mỗi ô có hai số được viết ở đầu của nó. Chúng ta được phép xoay một ô, nghĩa là chúng ta có thể hoán đổi hai đầu của nó nhưng không thể thay đổi các con số. 

Nhiệm vụ là xác định xem có thể xếp hai quân domino này thành một hàng sao cho các đầu tiếp xúc có cùng số hay không. Vì chỉ có hai ô, điều này giúp giảm việc kiểm tra xem liệu chúng ta có thể chọn hướng cho mỗi ô sao cho đầu bên phải của ô đầu tiên khớp với đầu bên trái của ô thứ hai hay không. 

Một cách trực tiếp hơn để xem vấn đề là coi mỗi quân domino như một tập hợp nhỏ gồm hai giá trị. Chúng ta muốn biết liệu hai tập hợp này có phần tử chung nào không. Nếu đúng như vậy, chúng ta có thể căn chỉnh cả hai quân domino để giá trị chung trở thành điểm kết nối. 

Đầu vào tối thiểu, bao gồm bốn số nguyên mô tả hai quân domino. Đầu ra là một quyết định duy nhất: liệu có tồn tại một thỏa thuận hợp lệ hay không. 

Vì kích thước đầu vào không đổi nên không có ràng buộc về hiệu năng nào thúc đẩy việc thiết kế thuật toán. Giải pháp đơn giản phải chính xác và xử lý được tất cả các lựa chọn định hướng. 

Các trường hợp cạnh chủ yếu liên quan đến các số lặp lại và các quân domino giống hệt nhau. 

Một lỗi phổ biến là chỉ kiểm tra một hướng cố định, ví dụ so sánh số thứ hai của quân domino thứ nhất với số thứ nhất của quân domino thứ hai. Điều này không thành công khi kết quả khớp chỉ tồn tại sau khi xoay vòng. 

Ví dụ: hãy xem xét đầu vào:```
1 2 3 1
```Domino là (1,2) và (3,1). Một phép kiểm tra ngây thơ có thể so sánh 2 với 3 và kết luận là không khớp. Nhưng việc xoay quân domino thứ hai sẽ cho (1,3), và bây giờ 2 vẫn không khớp, nhưng quan trọng hơn là 1 xuất hiện ở cả hai quân domino, do đó tồn tại một kết nối hợp lệ. 

Đầu ra đúng:```
YES
```Một trường hợp thất bại khác là:```
4 4 4 5
```Ở đây cả hai quân domino đều có chung giá trị 4, mặc dù nó xuất hiện hai lần ở ô đầu tiên. Mọi giải pháp đúng đều phải coi mỗi ô là một tập hợp các điểm cuối có thể có chứ không phải là các cặp có thứ tự. 

## Phương pháp tiếp cận 

Cách bạo lực là thử mọi hướng có thể có của cả hai quân domino. Mỗi domino có hai hướng, do đó có tổng cộng bốn cấu hình. Đối với mỗi cấu hình, chúng tôi kiểm tra xem đầu bên phải của cấu hình thứ nhất có bằng đầu bên trái của cấu hình thứ hai hay không. 

Điều này đúng vì nó liệt kê rõ ràng tất cả các vị trí, nhưng việc này tốn nhiều công sức hơn mức cần thiết. Số lần kiểm tra là không đổi, do đó nó đã chạy ở O(1), nhưng nó ẩn cấu trúc cơ bản của vấn đề. 

Quan sát quan trọng là định hướng không quan trọng đối với sự tồn tại của kết nối. Một quân domino luôn có thể được lật lại, vì vậy mỗi quân domino thực sự là một tập hợp gồm hai giá trị. Vấn đề giảm xuống còn việc kiểm tra xem giao điểm của hai tập hợp này có rỗng hay không. 

Điều này làm giảm toàn bộ vấn đề thành một bài kiểm tra thành viên duy nhất qua bốn con số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hướng) | O(1) | O(1) | Đã chấp nhận | 
| Đặt kiểm tra giao lộ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng quân domino thành tập hợp các giá trị mà nó có thể hiển thị ở hai bên, sau đó kiểm tra xem có bất kỳ sự trùng lặp nào không. 

1. Đọc bốn số nguyên đại diện cho hai quân domino, domino thứ nhất (a, b) và domino thứ hai (c, d). 
2. Coi quân domino đầu tiên có hai giá trị kết nối có thể là a và b. Hãy coi quân domino thứ hai có hai giá trị kết nối có thể là c và d. Điều này thể hiện tất cả các định hướng có thể một cách ngầm định. 
3. Kiểm tra xem có giá trị nào trong {a, b} xuất hiện trong {c, d} hay không. Đây là điều kiện duy nhất cần thiết cho một chuỗi hợp lệ, vì giá trị được chia sẻ đó có thể trở thành điểm cuối kết nối bằng cách lật một hoặc cả hai quân domino. 
4. Nếu tồn tại ít nhất một giá trị chung, ghi CÓ. Nếu không thì xuất ra NO. 

### Tại sao nó hoạt động 

Mỗi domino có thể được lật độc lập nên cả hai đầu đều có thể hoán đổi cho nhau. Ràng buộc duy nhất đối với chuỗi hai ô hợp lệ là sự bằng nhau ở điểm nối. Vì bất kỳ giá trị nào trong hai giá trị trên mỗi quân domino đều có thể được chọn làm mặt kết nối nên sự tồn tại của nghiệm tương đương với sự tồn tại của một số chung giữa hai quân domino. Không có ràng buộc cấu trúc nào khác tồn tại trong chuỗi có chiều dài hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c, d = map(int, input().split())

first = {a, b}
second = {c, d}

if first & second:
    print("YES")
else:
    print("NO")
```Giải pháp xây dựng hai tập hợp nhỏ và sử dụng giao điểm để phát hiện phần tử chung. Điều này trực tiếp mã hóa ý tưởng rằng bất kỳ giá trị phù hợp nào cũng có thể đóng vai trò là điểm nối sau khi xoay thích hợp. 

Một chi tiết tinh tế là việc sử dụng các bộ sẽ tự động xử lý các bản sao, vì vậy các trường hợp như (4, 4) được đơn giản hóa một cách tự nhiên mà không cần thêm logic. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 3 1
```Chúng tôi xây dựng: 

- đầu tiên = {1, 2} 
- giây = {3, 1} 

| Bước | đầu tiên | thứ hai | ngã tư | 
| --- | --- | --- | --- | 
| ban đầu | {1,2} | {3,1} | {1} | 

Giao lộ không trống nên câu trả lời là CÓ. 

Dấu vết này cho thấy chúng ta không quan tâm đến việc đặt hàng. Giá trị chia sẻ 1 là đủ để đảm bảo một sự sắp xếp hợp lệ. 

### Ví dụ 2 

đầu vào:```
2 5 3 4
```Chúng tôi xây dựng: 

- đầu tiên = {2, 5} 
- giây = {3, 4} 

| Bước | đầu tiên | thứ hai | ngã tư | 
| --- | --- | --- | --- | 
| ban đầu | {2,5} | {3,4} | ∅ | 

Không có phần tử chung nào tồn tại nên không có phép quay nào có thể tạo ra kết nối và câu trả lời là KHÔNG. 

Điều này xác nhận rằng thuật toán loại bỏ chính xác các trường hợp trong đó tất cả bốn điểm cuối đều khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số lượng so sánh không đổi hoặc một giao điểm cố định trên bốn phần tử | 
| Không gian | O(1) | Chỉ có hai bộ nhỏ có kích thước cố định được tạo | 

Các ràng buộc là không đáng kể nên giải pháp theo thời gian không đổi này là tối ưu và dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b, c, d = map(int, sys.stdin.readline().split())
    first = {a, b}
    second = {c, d}
    return "YES" if first & second else "NO"

# basic samples
assert run("1 2 3 1") == "YES"
assert run("2 5 3 4") == "NO"

# identical dominoes
assert run("4 4 4 4") == "YES"

# duplicate only in one domino
assert run("7 7 1 2") == "NO"

# match after rotation
assert run("1 9 9 3") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 4 4 4 | CÓ | domino giống hệt nhau và trùng lặp | 
| 7 7 1 2 | KHÔNG | giá trị lặp lại không gây nhầm lẫn | 
| 1 9 9 3 | CÓ | chỉ khớp qua vòng quay | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi cả hai số trên quân domino đều giống hệt nhau. Ví dụ:```
4 4 1 2
```Thuật toán chuyển đổi quân domino đầu tiên thành {4} và quân thứ hai thành {1, 2}. Giao lộ vắng nên câu trả lời là KHÔNG. Điều này đúng vì không có phép quay nào có thể đưa ra một giá trị mới. 

Một trường hợp khác là khi giá trị chia sẻ xuất hiện hai lần trong một quân domino nhưng chỉ xuất hiện một lần trong quân cờ còn lại:```
6 6 6 7
```Ở đây các tập hợp trở thành {6} và {6, 7}. Giao điểm là {6}, do đó thuật toán xuất ra CÓ một cách chính xác, phản ánh rằng giá trị lặp lại vẫn đóng vai trò là điểm kết nối hợp lệ.
