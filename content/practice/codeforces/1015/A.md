---
title: "CF 1015A - Điểm trong đoạn"
description: "Chúng ta có một trục số một chiều từ 1 đến m và một số khoảng khép kín được đặt trên đó. Mỗi khoảng bao gồm mọi điểm nguyên giữa các điểm cuối của nó, bao gồm cả hai đầu."
date: "2026-06-16T22:25:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1015
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 501 (Div. 3)"
rating: 800
weight: 1015
solve_time_s: 88
verified: true
draft: false
---

[CF 1015A - Điểm trong Phân đoạn](https://codeforces.com/problemset/problem/1015/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một trục số một chiều từ 1 đến m và một số khoảng khép kín được đặt trên đó. Mỗi khoảng bao gồm mọi điểm nguyên giữa các điểm cuối của nó, bao gồm cả hai đầu. Bởi vì các khoảng có thể trùng nhau nên một số điểm có thể được đề cập nhiều lần trong khi những điểm khác có thể không được đề cập đến. 

Nhiệm vụ là xác định tất cả các điểm nguyên trong phạm vi từ 1 đến m không nằm trong bất kỳ khoảng nào. Nói cách khác, chúng ta đang vẽ các đoạn trên một đường thẳng và hỏi vị trí nào vẫn chưa được sơn. 

Các ràng buộc rất nhỏ: cả số lượng phân đoạn và phạm vi tọa độ tối đa là 100. Điều này ngay lập tức cho chúng ta biết rằng ngay cả các phương pháp bậc hai hoặc bậc ba đều an toàn và chúng ta không cần cấu trúc dữ liệu nâng cao. Một mô phỏng trực tiếp trên tất cả các điểm là đủ. 

Sự tinh tế chính đến từ việc xử lý chính xác các điểm chồng chéo và điểm cuối. Vì các phân đoạn là bao hàm nên một điểm chính xác bằng l hoặc r phải được coi là bao phủ. Một lỗi phổ biến là coi các phân đoạn là nửa mở hoặc quên đánh dấu điểm cuối. 

Trường hợp cạnh thứ hai phát sinh khi các phân đoạn bao trùm toàn bộ phạm vi. Ví dụ: nếu chúng ta có [1, m], mọi điểm đều được che đậy và câu trả lời phải là 0 và không có dòng thứ hai. Việc triển khai bất cẩn luôn in dòng thứ hai có thể không đáp ứng được yêu cầu định dạng. 

Một trường hợp góc khác là các đoạn một điểm chồng lên nhau. Ví dụ: [2,2], [2,2], [2,3]. Điểm 2 vẫn phải được coi là được đề cập một lần và các bản sao không được ảnh hưởng đến tính chính xác. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải quyết vấn đề là kiểm tra mọi điểm từ 1 đến m và kiểm tra xem nó có nằm trong ít nhất một đoạn hay không. Đối với mỗi điểm x, chúng ta quét tất cả các đoạn và kiểm tra xem có tồn tại i sao cho li ∼ x ₫ ri hay không. Nếu không có phân đoạn nào như vậy tồn tại, chúng ta thêm x vào câu trả lời. 

Điều này hiệu quả vì các ràng buộc rất nhỏ: tối đa 100 điểm và 100 phân đoạn, dẫn đến tối đa 10.000 lượt kiểm tra, điều này không đáng kể. 

Tuy nhiên, cấu trúc vũ phu có sự dư thừa. Mỗi điểm sẽ kiểm tra lại tất cả các phân đoạn một cách độc lập, mặc dù phạm vi bao phủ có thể được tính toán trước hiệu quả hơn. Vì phạm vi tọa độ nhỏ nên thay vào đó, chúng ta có thể duy trì dấu mảng boolean[1..m], ban đầu là sai và đánh dấu mọi điểm được bao phủ bởi bất kỳ phân đoạn nào trong một lần chuyển qua các phân đoạn. Sau khi xử lý tất cả các phân đoạn, chúng tôi chỉ cần thu thập các chỉ số trong đó dấu vẫn sai. Điều này loại bỏ việc quét lặp đi lặp lại và làm cho giải pháp sạch hơn và trực tiếp hơn. 

Điều quan trọng là chúng ta không cần phải trả lời các truy vấn trực tuyến; chúng tôi chỉ cần bảo hiểm cuối cùng. Điều đó cho phép chúng tôi chuyển đổi việc đánh dấu phạm vi thành quét mảng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) quét mỗi điểm → O(n·m²) | O(1) | Được chấp nhận nhưng dư thừa | 
| Tối ưu (đánh dấu mảng) | O(n·m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng mảng boolean để ghi lại xem mỗi vị trí có được che phủ hay không. 

1. Khởi tạo một mảng có kích thước m+1 với tất cả các giá trị được đặt thành false. Điều này thể hiện rằng ban đầu không có điểm nào được bao phủ bởi bất kỳ đoạn nào. 
2. Lặp lại từng đoạn [l, r]. Với mọi số nguyên x từ l đến r, đặt cover[x] = true. Điều này trực tiếp mã hóa định nghĩa về phạm vi bảo hiểm. 
3. Sau khi xử lý tất cả các phân đoạn, lặp lại từ 1 đến m. Thu thập tất cả các chỉ số x sao cho cover[x] vẫn sai. Đây chính xác là những điểm chưa được khám phá. 
4. Gọi k là số điểm chưa được khám phá. In k và nếu k > 0 in các điểm thu thập theo bất kỳ thứ tự nào, thường là thứ tự tăng dần. 

Lý do chúng tôi có thể thực hiện điều này một cách an toàn là vì mỗi phân đoạn đóng góp phạm vi bao phủ một cách độc lập và việc đánh dấu là bình thường. Nếu nhiều phân đoạn bao trùm cùng một điểm, các phép gán lặp lại không làm thay đổi kết quả. 

### Tại sao nó hoạt động

Ở mỗi bước, cover[x] là đúng khi và chỉ khi tồn tại ít nhất một phân đoạn được xử lý có chứa x. Vì chúng tôi xử lý tất cả các phân đoạn chính xác một lần và chỉ đặt giá trị thành true khi có phạm vi phù hợp nên không có kết quả dương tính giả nào được đưa ra. Tương tự như vậy, bất kỳ điểm nào không nằm trong bất kỳ đoạn nào sẽ không bao giờ được đánh dấu, vì vậy nó vẫn sai. Điều này tạo ra sự tương đương chính xác giữa trạng thái mảng và sự kết hợp của tất cả các phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

covered = [False] * (m + 1)

for _ in range(n):
    l, r = map(int, input().split())
    for x in range(l, r + 1):
        covered[x] = True

ans = []
for x in range(1, m + 1):
    if not covered[x]:
        ans.append(x)

print(len(ans))
if ans:
    print(*ans)
```Giải pháp sử dụng mảng đánh dấu boolean trực tiếp. Vòng lặp bên trong trên mỗi phân đoạn sẽ mở rộng phạm vi phủ sóng một cách rõ ràng, điều này an toàn do có những ràng buộc nhỏ. Lượt cuối cùng thu thập tất cả các điểm chưa được đánh dấu theo thứ tự tăng dần. 

Một chi tiết triển khai tinh tế là việc sử dụng phạm vi (l, r + 1), bao gồm điểm cuối phù hợp một cách chính xác. Việc thiếu +1 sẽ loại trừ không chính xác các điểm cuối của phân đoạn và tạo ra các câu trả lời sai. 

Điều kiện định dạng đầu ra cũng rất quan trọng: khi không có điểm nào bị che khuất, chúng ta không được in dòng thứ hai trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, m = 5
segments = [2,2], [1,2], [5,5]
```| Phân đoạn | Phạm vi được đánh dấu | Mảng phủ (1..5) | 
| --- | --- | --- | 
| [2,2] | 2 | F T F F F | 
| [1,2] | 1,2 | T T F F F | 
| [5,5] | 5 | T T F F T | 

Sau khi xử lý: 

| x | được bảo hiểm[x] | hành động | 
| --- | --- | --- | 
| 1 | Đúng | bỏ qua | 
| 2 | Đúng | bỏ qua | 
| 3 | Sai | thêm | 
| 4 | Sai | thêm | 
| 5 | Đúng | bỏ qua | 

Đầu ra:```
2
3 4
```Điều này xác nhận rằng các phân đoạn chồng chéo được xử lý chính xác và các khoảng trống chưa được phát hiện được phát hiện chính xác. 

### Ví dụ 2 

đầu vào:```
n = 1, m = 7
segments = [1,7]
```| Phân đoạn | Phạm vi được đánh dấu | Mảng phủ (1..7) | 
| --- | --- | --- | 
| [1,7] | 1..7 | T T T T T T T | 

| x | được bảo hiểm[x] | hành động | 
| --- | --- | --- | 
| 1 | Đúng | bỏ qua | 
| 2 | Đúng | bỏ qua | 
| 3 | Đúng | bỏ qua | 
| 4 | Đúng | bỏ qua | 
| 5 | Đúng | bỏ qua | 
| 6 | Đúng | bỏ qua | 
| 7 | Đúng | bỏ qua | 

Đầu ra:```
0
```Điều này cho thấy trường hợp cạnh bao phủ toàn bộ trong đó không tồn tại điểm đầu ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Mỗi đoạn đánh dấu nhiều nhất m vị trí và n ≤ 100 | 
| Không gian | O(m) | Phạm vi lưu trữ mảng Boolean cho từng điểm | 

Số lượng hoạt động tối đa nhiều nhất là 10.000, điều này không đáng kể trong các ràng buộc. Việc sử dụng bộ nhớ cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    covered = [False] * (m + 1)

    for _ in range(n):
        l, r = map(int, input().split())
        for x in range(l, r + 1):
            covered[x] = True

    ans = [str(x) for x in range(1, m + 1) if not covered[x]]
    if ans:
        return str(len(ans)) + "\n" + " ".join(ans)
    else:
        return "0"

# provided sample
assert run("3 5\n2 2\n1 2\n5 5\n") == "2\n3 4"

# all covered
assert run("1 5\n1 5\n") == "0"

# no segments
assert run("0 3\n") == "3\n1 2 3"

# single point gaps
assert run("2 5\n1 1\n5 5\n") == "3\n2 3 4"

# overlapping segments
assert run("3 5\n1 3\n2 4\n4 5\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bảo hiểm đầy đủ | 0 | trường hợp cạnh chồng lên nhau hoàn chỉnh | 
| không có phân đoạn | tất cả các điểm | hành vi nhập trống | 
| khoảng trống chỉ dành cho điểm cuối | khu vực giữa | sự đúng đắn từng cái một | 
| khoảng chồng chéo | không có khoảng trống | xử lý công đoàn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phân đoạn hợp nhất thành một khối liên tục duy nhất. Ví dụ:```
3 5
1 3
2 5
1 5
```Đang xử lý đánh dấu mọi điểm từ 1 đến 5. Mảng được bao phủ hoàn toàn đúng. Lần quét cuối cùng không tìm thấy mục nhập sai nào, do đó ans trống và chúng tôi xuất ra đúng 0. Bất kỳ triển khai nào in dòng thứ hai vô điều kiện sẽ vi phạm định dạng bắt buộc ở đây. 

Một trường hợp khác là khi các phân đoạn chỉ bao gồm các điểm cuối:```
2 5
1 1
5 5
```Chỉ có vị trí 2, 3 và 4 là sai. Vòng đánh dấu đảm bảo rằng chỉ các phạm vi bao gồm chính xác mới được lấp đầy, do đó các điểm cuối không rò rỉ vùng phủ sóng sang các ô lân cận.
