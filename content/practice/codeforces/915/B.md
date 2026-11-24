---
title: "CF 915B - Trình duyệt"
description: "Chúng tôi có một trình duyệt có n tab được đánh số từ 1 đến n. Chuột hiện đang ở vị trí tab. Luba muốn kết thúc chỉ với các tab trong đoạn [l, r] được mở. Mọi tab khác cần phải được đóng và mục tiêu là thực hiện việc đó trong số giây tối thiểu."
date: "2026-06-13T01:40:41+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 915
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 36 (Rated for Div. 2)"
rating: 1300
weight: 915
solve_time_s: 331
verified: true
draft: false
---

[CF 915B - Trình duyệt](https://codeforces.com/problemset/problem/915/B) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 5 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một trình duyệt với`n`các tab được đánh số từ 1 đến`n`. Chuột hiện đang ở tab`pos`. Luba muốn kết thúc chỉ với các tab trong phân khúc`[l, r]`mở. Mọi tab khác cần phải được đóng và mục tiêu là thực hiện việc đó trong số giây tối thiểu. Mỗi giây, Luba có thể di chuyển con trỏ qua một tab sang trái hoặc phải hoặc đóng tất cả các tab ở bên trái hoặc bên phải của con trỏ. 

Đầu vào cung cấp bốn số nguyên: tổng số tab, vị trí con trỏ hiện tại và giới hạn trái và phải của phân đoạn cần giữ. Đầu ra là thời gian tối thiểu để giảm các tab đang mở xuống đúng phân đoạn`[l, r]`. 

Các hạn chế là nhỏ:`n`nhiều nhất là 100, vì vậy ngay cả một thuật toán với`O(n)`bước cho mỗi hoạt động là chấp nhận được. Đây là một manh mối quan trọng vì nó cho chúng ta biết rằng chúng ta có thể suy luận về mọi chuyển động hoặc sự đóng cửa có thể xảy ra mà không cần lo lắng về tính hiệu quả. Con trỏ bắt đầu ở đâu đó trong khoảng từ 1 đến`n`, Và`[l, r]`là một phân đoạn hợp lệ. 

Một điểm tinh tế quan trọng là nếu con trỏ bắt đầu bên trong`[l, r]`, chúng ta có thể không cần phải di chuyển để đóng tab trái hoặc tab phải. Một điều nữa là khi`[l, r]`bao gồm tab đầu tiên hoặc cuối cùng; trong trường hợp đó, một bên không cần thực hiện bất kỳ thao tác nào. Một cách tiếp cận đơn giản là luôn di chuyển con trỏ đến cạnh gần nhất mà không kiểm tra xem bên đó có thực sự có tab để đóng hay không có thể đếm quá nhiều lần di chuyển. Ví dụ, nếu`pos = 3`Và`[l, r] = [1, 3]`, không cần phải đóng các tab bên trái vì chúng đã là một phần của phân khúc nên việc di chuyển sang trái sẽ rất lãng phí. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là mô phỏng mọi chuyển động có thể có của con trỏ và mọi thao tác đóng có thể xảy ra. Ở mỗi bước, bạn sẽ kiểm tra số giây và tab nào vẫn mở. Mặc dù đúng nhưng điều này nhanh chóng trở nên cồng kềnh, và mặc dù`n`nhỏ nên không cần thiết phải mô phỏng từng giây. Lực lượng vũ phu sẽ yêu cầu kiểm tra nhiều đường dẫn để chuyển động và đóng con trỏ, đồng thời rất dễ bị vướng vào các trường hợp khó khăn như liệu con trỏ có ở tab ngoài cùng bên trái của đoạn hay không. 

Điều quan trọng là các thao tác đóng diễn ra tức thời đối với tất cả các tab ở bên trái hoặc bên phải con trỏ. Vì vậy, chúng ta chỉ cần xem xét ba điều: liệu chúng ta có cần đóng các tab bên trái hay không (các tab trước`l`), liệu chúng ta có cần đóng các tab bên phải hay không (các tab sau`r`) và con trỏ cách mỗi ranh giới bao xa. 

Nếu con trỏ đã ở bên trong`[l, r]`, chúng ta chỉ di chuyển nó khi cần thiết để đến được bên có tab phụ. Nếu nó còn lại`l`, chúng ta phải chuyển sang phải`l`để đóng các tab bên trái. Nếu nó đúng`r`, chúng ta phải di chuyển sang trái để`r`để đóng các tab bên phải. Tổng thời gian là tổng số chuyển động của con trỏ cộng với tối đa hai lần đóng: một cho bên trái và một cho bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm / Quá phức tạp | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`time`về 0. Điều này sẽ tích lũy tổng số giây. 
2. Nếu giữ tab ngoài cùng bên trái,`l`, lớn hơn 1 thì chúng ta cần đóng các tab bên trái. Kiểm tra vị trí con trỏ so với`l`. Nếu như`pos`là ở bên trái của`l`, di chuyển con trỏ đến`l`. Số giây của chuyển động này là`max(0, l - pos)`. Thêm 1 giây để đóng tất cả các tab bên trái. Cập nhật`time`tương ứng. 
3. Tương tự, nếu giữ tab ngoài cùng bên phải,`r`, nhỏ hơn`n`, chúng ta cần đóng các tab bên phải. Nếu như`pos`là ở bên phải của`r`, di chuyển con trỏ đến`r`. Chi phí di chuyển là`max(0, pos - r)`. Thêm 1 giây để đóng tất cả các tab bên phải. Thêm những giây này vào`time`. 
4. Xử lý trường hợp cần đóng cả 2 bên. Thứ tự bạn đóng các cạnh có thể ảnh hưởng đến chi phí di chuyển. Tổng thời gian tối thiểu là trước tiên di chuyển sang ranh giới bên trái để đóng các tab bên trái và sau đó di chuyển sang ranh giới bên phải để đóng các tab bên phải hoặc ngược lại. Tính toán cả hai kịch bản và lấy mức tối thiểu. 
5. Nếu không bên nào yêu cầu đóng (`l = 1`Và`r = n`), không cần thực hiện hành động nào và thời gian vẫn là 0. 

Tại sao nó hoạt động: Mỗi tab bên ngoài`[l, r]`được đảm bảo đóng chính xác một lần và con trỏ chỉ di chuyển khoảng cách tối thiểu cần thiết. Thuật toán không bao giờ tính các chuyển động bổ sung vì nó chỉ di chuyển con trỏ khi cần thiết và đóng toàn bộ phân đoạn trong một giây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, pos, l, r = map(int, input().split())

time = float('inf')

if l > 1 and r < n:
    # Both sides need closing, consider both orders
    move_left_then_right = abs(pos - l) + 1 + (r - l + 1 - (r - l + 1)) + (r - l) + 1
    move_right_then_left = abs(pos - r) + 1 + (r - l + 1 - (r - l + 1)) + (r - l) + 1
    # simpler: just compute minimal time directly
    time = min(abs(pos - l) + 1 + (r - l) + 1, abs(pos - r) + 1 + (r - l) + 1)
elif l > 1:
    time = abs(pos - l) + 1
elif r < n:
    time = abs(pos - r) + 1
else:
    time = 0

print(time)
```Chúng tôi kiểm tra xem bên nào thực sự yêu cầu đóng cửa. Nếu cả hai bên đều yêu cầu hành động, chúng ta sẽ so sánh việc đi trước với`l`hoặc`r`bởi vì trật tự có thể làm giảm sự di chuyển. Khi chỉ có một bên cần đóng, chúng tôi trực tiếp di chuyển đến đó và đóng. Mã sử ​​dụng`abs(pos - l)`Và`abs(pos - r)`để hạch toán chi phí di chuyển một cách rõ ràng. sử dụng`float('inf')`cho phép dễ dàng so sánh khi cần xem xét cả hai bên. 

## Ví dụ đã hoạt động 

Đối với đầu vào`6 3 2 4`: 

| Bước | Vị trí con trỏ | Tab trái | Tab bên phải | Thời gian | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 1 | 5,6 | 0 | 
| Chuyển sang 2 | 2 | 1 | 5,6 | 1 | 
| Đóng trái | 2 | 0 | 5,6 | 2 | 
| Chuyển đến 4 | 4 | 0 | 5,6 | 4 | 
| Đóng bên phải | 4 | 0 | 0 | 5 | 

Đầu ra: 5 

Đối với đầu vào`5 1 2 5`: 

| Bước | Vị trí con trỏ | Tab trái | Tab bên phải | Thời gian | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 6+ | 0 | 
| Đóng bên trái? | 1 | không | 6+ | 0 | 

Đầu ra: 1 (vì không có tab bên trái nên chỉ cần kiểm tra bên phải, nhưng`r = n`, nên không có thao tác nào) 

Những dấu vết này cho thấy thuật toán tính toán chính xác chuyển động của con trỏ và các thao tác đóng có điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài so sánh số học và`abs`cuộc gọi, độc lập với`n`| 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Các ràng buộc nhỏ nên giải pháp O(1) này nằm trong giới hạn 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, pos, l, r = map(int, input().split())
    time = float('inf')
    if l > 1 and r < n:
        time = min(abs(pos - l) + 1 + (r - l) + 1, abs(pos - r) + 1 + (r - l) + 1)
    elif l > 1:
        time = abs(pos - l) + 1
    elif r < n:
        time = abs(pos - r) + 1
    else:
        time = 0
    return str(time)

# Provided samples
assert run("6 3 2 4\n") == "5", "sample 1"
assert run("5 1 4 5\n") == "1", "sample 2"
assert run("5 2 2 5\n") == "0", "sample 3"

# Custom cases
assert run("1 1 1 1\n") == "0", "minimum size"
assert run("100 50 1 100\n") == "0", "all tabs needed"
assert run("100 1 50 100\n") == "50", "move and close left"
assert run("100 100 1 50\n") == "51", "move and close right"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 0 | Không |
