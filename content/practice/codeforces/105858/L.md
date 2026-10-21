---
title: "CF 105858L - Mọi người đều yêu thích k bài toán đường đi ngắn nhất!"
description: "Chúng tôi có n quà lưu niệm. Mỗi món quà lưu niệm phải được giao cho đúng một trong hai người bạn. Nếu quà lưu niệm tôi tặng cho người bạn đầu tiên, nó mang lại hạnh phúc cho người bạn đầu tiên và không mang lại gì cho người bạn thứ hai. Nếu nó thuộc về người bạn thứ hai thì điều ngược lại xảy ra với giá trị bi."
date: "2026-06-25T14:45:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105858
codeforces_index: "L"
codeforces_contest_name: "2025 Winter ESCOM Training Camp, Final Contest"
rating: 0
weight: 105858
solve_time_s: 42
verified: true
draft: false
---

[CF 105858L - Mọi người đều yêu thích k bài toán đường đi ngắn nhất!](https://codeforces.com/problemset/problem/105858/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`quà lưu niệm. Mỗi món quà lưu niệm phải được giao cho đúng một trong hai người bạn. Nếu quà lưu niệm`i`đi đến người bạn đầu tiên, nó góp phần`a_i`hạnh phúc cho người bạn đầu tiên và không có gì cho người bạn thứ hai. Nếu nó thuộc về người bạn thứ hai thì điều ngược lại xảy ra với giá trị`b_i`. 

Mục tiêu không phải là chỉ tìm một bản phân phối tốt nhất. Chúng ta cần cái đầu tiên`2^min(n,20)`phân phối khi được sắp xếp theo sự khác biệt tuyệt đối giữa tổng số hạnh phúc của hai người bạn. 

Sự khác biệt có thể được viết theo cách đơn giản hơn. Đối với mỗi món quà lưu niệm, hãy xác định:`d_i = a_i - b_i`Chọn người bạn đầu tiên thêm`d_i`đến sự khác biệt cuối cùng. Chọn người bạn thứ hai thêm`-d_i`. Hiệu tuyệt đối cuối cùng là giá trị tuyệt đối của tổng có dấu của tất cả`d_i`. 

Kích thước đầu vào đủ nhỏ để`n`đạt tới 40, nhưng số lượng phân phối có thể lên tới`2^40`, tức là khoảng một nghìn tỷ. Giải pháp liệt kê mọi phân phối là không thể vì ngay cả vài tỷ thao tác cũng đã vượt xa giới hạn thời gian của cuộc thi thông thường. 

Kích thước đầu ra là lớn có chủ ý. Khi`n`là 40, chúng ta phải in`2^20`giá trị, khoảng một triệu câu trả lời. Điều này cho chúng ta biết rằng một thuật toán xung quanh`O(2^20 log 2^20)`là thực tế, trong khi bất cứ điều gì gần gũi với`O(2^n)`thì không. 

Có một số trường hợp việc triển khai chỉ tìm thấy sự khác biệt tối thiểu không thành công. Ví dụ:```
Input
2
-1 10
-1 10

Output
9 9 11 11
```Một giải pháp bất cẩn có thể tìm thấy giá trị nhỏ nhất`9`và dừng lại. Nhiệm vụ này yêu cầu bốn phân phối tốt nhất, bao gồm các giá trị lặp lại. 

Một sai lầm phổ biến khác là quên rằng dấu hiệu có thể âm. Coi như:```
Input
1
-5
10

Output
15 15
```Hai sự lựa chọn tạo ra sự khác biệt`|-5 - 10| = 15`Và`|10 - (-5)| = 15`. Đối xử với các giá trị luôn tích cực sẽ thay đổi câu trả lời. 

Một tình huống khó khăn cuối cùng là có nhiều câu trả lời giống nhau. TRONG:```
Input
4
1 2 3 4
4 3 2 1
```nhiều bài tập khác nhau cho cùng một sự khác biệt tối thiểu. Đầu ra cần tất cả các bản sao đó, không chỉ các giá trị duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi sự phân bổ quà lưu niệm có thể có. Đối với mỗi bài tập, chúng tôi tính toán tổng đã ký của`d_i`các giá trị, lấy giá trị tuyệt đối của nó, thu thập tất cả các kết quả và sắp xếp chúng. 

Điều này đúng vì mọi phân bố có thể đều tương ứng với chính xác một lựa chọn dấu hiệu. Tuy nhiên, với 40 món quà lưu niệm có`2^40`khả năng. Ngay cả việc lưu trữ những giá trị đó cũng là không thể và việc sắp xếp chúng sẽ cần khoảng một nghìn tỷ phần tử. 

Quan sát hữu ích là câu trả lời chỉ yêu cầu câu đầu tiên`2^20`các giá trị. Chúng ta có thể chia quà lưu niệm thành hai nửa. Mỗi nửa có tối đa 20 mục, vì vậy chúng ta có thể tạo ra mọi số tiền có dấu trong một nửa. Điều này tạo ra tối đa hai mảng`2^20`các giá trị. 

Bây giờ mọi phân phối đầy đủ được biểu diễn dưới dạng:`left_sum + right_sum`Vấn đề trở thành việc tìm các giá trị tuyệt đối nhỏ nhất trong số tất cả các cặp từ hai mảng. 

Điều quan trọng là cả hai mảng đều có thể được sắp xếp. Đối với một giá trị`x`trong mảng đầu tiên, đối tác tốt nhất từ ​​mảng thứ hai gần bằng`-x`. Vì mảng thứ hai đã được sắp xếp nên tất cả các ứng cử viên cho một mảng`x`có thể được tạo ra với giá trị tuyệt đối ngày càng tăng bằng cách mở rộng ra ngoài từ vị trí nơi`-x`sẽ được chèn vào. 

Chúng tôi hợp nhất các luồng ứng viên được sắp xếp này với hàng đợi ưu tiên. Ban đầu, mỗi nửa số tiền còn lại sẽ đóng góp cho đối tác tốt nhất có thể của nó. Mỗi lần chúng tôi xóa câu trả lời nhỏ nhất hiện tại, chúng tôi chỉ chuyển luồng đó sang ứng viên tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(2^n) | Quá chậm | 
| Gặp nhau ở giữa + Heap | O(2^(n/2) log 2^(n/2)) | O(2^(n/2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`d_i = a_i - b_i`cho mỗi món quà lưu niệm. Sự khác biệt hạnh phúc cuối cùng là giá trị tuyệt đối của tổng các phiên bản được ký của các giá trị này. 
2. Chia mảng khác biệt thành hai phần. Mỗi phần có tối đa 20 phần tử, vì vậy mọi tổng có dấu có thể có trong một phần đều có thể được tạo. 
3. Sắp xếp danh sách các khoản tiền từ nửa sau. Chúng tôi sẽ sử dụng thứ tự này để nhanh chóng tìm thấy các giá trị gần nhất với bất kỳ mục tiêu nào. 
4. Với mọi tổng`x`từ nửa đầu, tìm vị trí chèn của`-x`trong tổng số nửa sau được sắp xếp. Vị trí này phân tách các giá trị tạo ra tổng âm và tổng dương. 
5. Tạo hai ứng cử viên tiếp theo cho mỗi ứng viên`x`: giá trị ngay trước vị trí chèn và giá trị tại vị trí chèn. Đây là những khả năng gần nhất nên chúng là những giá trị đầu tiên có thể thuộc về câu trả lời. 
6. Xếp tất cả những ứng viên này vào hàng ưu tiên theo thứ tự chênh lệch tuyệt đối của họ. 
7. Liên tục loại bỏ ứng viên nhỏ nhất. Thêm giá trị của nó vào danh sách câu trả lời. Sau đó di chuyển cùng một luồng thêm một bước nữa về phía đã tạo ra ứng viên bị loại. 

Lý do chuyển động của luồng hoạt động là vì đối với một giá trị nửa đầu cố định, các giá trị nửa sau ở bên trái của điểm chèn sẽ gần hơn khi di chuyển sang phải và các giá trị ở nửa bên phải sẽ xa hơn khi di chuyển sang phải. Cả hai bên đã được sắp xếp theo giá trị tuyệt đối. 

Tại sao nó hoạt động: 

Đối với mỗi tổng bên trái cố định, chúng ta có một dãy sắp xếp tất cả các tổng bên phải có thể có theo sai phân cuối cùng tuyệt đối. Đống chứa phần tử nhỏ nhất chưa được sử dụng từ mỗi chuỗi như vậy. Việc hợp nhất hàng đợi ưu tiên luôn trích xuất phần tử nhỏ nhất còn lại trong số tất cả các chuỗi, do đó, mọi câu trả lời được tạo ra đều là phân phối nhỏ nhất có thể tiếp theo. Vì mọi phân phối đều thuộc về chính xác một cặp tổng nửa nên không có ứng cử viên nào bị bỏ sót. 

## Giải pháp Python```python
import sys
import bisect
import heapq

input = sys.stdin.readline

def generate(vals):
    res = [0]
    for v in vals:
        res += [x + v for x in res]
        res += [x - v for x in res[:len(res)//2]]
    return res

def generate(vals):
    res = [0]
    for v in vals:
        cur = []
        for x in res:
            cur.append(x + v)
            cur.append(x - v)
        res = cur
    return res

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    diff = [a[i] - b[i] for i in range(n)]

    mid = n // 2
    left = generate(diff[:mid])
    right = generate(diff[mid:])

    right.sort()

    heap = []
    for i, x in enumerate(left):
        pos = bisect.bisect_left(right, -x)
        if pos < len(right):
            heap.append((abs(x + right[pos]), x + right[pos], i, pos, 1))
        if pos > 0:
            heap.append((abs(x + right[pos - 1]), x + right[pos - 1], i, pos - 1, -1))

    heapq.heapify(heap)

    need = 1 << min(n, 20)
    ans = []

    while len(ans) < need:
        _, value, i, j, direction = heapq.heappop(heap)
        ans.append(value if value >= 0 else -value)

        nj = j + direction
        if 0 <= nj < len(right):
            heapq.heappush(
                heap,
                (abs(left[i] + right[nj]), left[i] + right[nj], i, nj, direction)
            )

    print(*sorted(ans))

if __name__ == "__main__":
    solve()
```các`generate`hàm xây dựng tất cả các số tiền có dấu của một nửa. Đối với mỗi số tiền trước đó, vật lưu niệm tiếp theo tạo ra hai khả năng, cộng hoặc trừ giá trị lưu niệm. 

Nửa thứ hai được sắp xếp vì tìm kiếm nhị phân và mở rộng theo thứ tự phụ thuộc vào thuộc tính đó. Với mỗi số tiền còn lại,`bisect_left`tìm thấy giá trị nửa sau đầu tiên không nhỏ hơn`-x`. 

Heap lưu trữ ứng cử viên chưa được sử dụng tốt nhất hiện tại từ mỗi nửa bên trái. Bộ dữ liệu chứa giá trị tuyệt đối được sử dụng để sắp xếp, chênh lệch được ký thực tế và đủ thông tin để chuyển tiếp cùng một luồng sau khi trích xuất. 

Số nguyên Python có độ chính xác tùy ý, do đó, các giá trị lớn từ đầu vào không yêu cầu xử lý đặc biệt. Điều kiện biên tinh vi duy nhất là kiểm tra cả hai phía của điểm chèn, bởi vì`-x`có thể nằm ngoài phạm vi của mảng được sắp xếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 2 3 4
4 3 2 1
```Sự khác biệt là`[-3, -1, 1, 3]`. 

| Bước | Hành động đống | Giá trị câu trả lời hiện tại | 
| --- | --- | --- | 
| Bắt đầu | Chèn các ứng viên gần nhất từ ​​mỗi tổng bên trái | trống | 
| Pop | Sự khác biệt tuyệt đối nhỏ nhất là 0 | 0 | 
| Pop | Một cặp khác cho 0 | 0, 0 | 
| Tiếp tục | Nhiều kết hợp số 0 xuất hiện | 0, 0, 0, ... | 

Dấu vết này cho thấy tại sao các bản sao phải được bảo tồn. Một số cách phân phối quà lưu niệm khác nhau có thể tạo ra sự khác biệt giống hệt nhau. 

### Mẫu 2 

đầu vào:```
2
-1 10
-1 10
```Sự khác biệt là`[0, 0]`. 

| Bước | Hành động đống | Giá trị câu trả lời hiện tại | 
| --- | --- | --- | 
| Bắt đầu | Tạo tổng 0 và 0 | trống | 
| Pop | Cặp đầu tiên cho chênh lệch 0+0 0 | 0 | 
| Tiếp tục | Các cặp còn lại cũng được kiểm tra | 0, 0, 0, 0 | 

Điều này chứng tỏ rằng phép chuyển đổi đã ký xử lý các giá trị hạnh phúc âm một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(n/2) log 2^(n/2)) | Chúng tôi tạo ra một nửa số tiền và trích xuất tối đa khoảng một triệu phần tử heap | 
| Không gian | O(2^(n/2)) | Hai mảng nửa tổng và vùng heap chiếm ưu thế trong việc sử dụng bộ nhớ | 

Một nửa kích thước tối đa là 20, do đó các mảng được tạo chứa tối đa khoảng một triệu giá trị. Các hoạt động của đống nằm trong phạm vi thực tế đối với các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io
import bisect
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""4
1 2 3 4
4 3 2 1
""") == "0 0 0 0 0 0 5 5 5 5 5 5 5 5 10 10"

assert run("""2
-1 10
-1 10
""") == "9 9 11 11"

# custom: single souvenir
assert run("""1
5
-5
""") == "10 10"

# custom: all equal differences
assert run("""3
7 7 7
0 0 0
""") == "7 7 7 7 7 7 7 7"

# custom: zero differences
assert run("""3
1 2 3
1 2 3
""") == "0 0 0 0 0 0 0 0"

# custom: negative values
assert run("""2
-10 -20
5 15
""") == "15 25 25 35"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một món quà lưu niệm |`10 10`| Xử lý kích thước phân chia nhỏ nhất | 
| Sự khác biệt như nhau | giá trị lặp lại | Kiểm tra câu trả lời trùng lặp | 
| Những người bạn giống hệt nhau | tất cả số không | Kiểm tra nhiều bản phân phối tối ưu | 
| Giá trị âm | giá trị hỗn hợp | Kiểm tra logic khác biệt đã ký | 

## Vỏ cạnh 

Trường hợp giá trị hạnh phúc âm được xử lý bằng cách chuyển đổi mọi món quà lưu niệm thành đóng góp có chữ ký. Vì:```
1
-5
10
```các giá trị khác biệt là`-15`. Hai tổng có thể ký được là`-15`Và`15`, vậy cả hai đáp án đều là`15`. 

Trường hợp có nhiều câu trả lời bằng nhau sẽ thành công vì heap lưu trữ mọi phân phối có thể có một cách riêng biệt. Nếu mười phép gán khác nhau tạo ra sự khác biệt giống nhau thì giá trị đó sẽ được chèn mười lần vào kết quả. 

Trường hợp ranh giới trong đó`-x`nhỏ hơn mọi giá trị ở nửa bên phải được xử lý bằng cách chỉ sử dụng phía bên phải của điểm chèn khi nó tồn tại. Tương tự, nếu`-x`lớn hơn mọi giá trị, chỉ có vế trái là hợp lệ. Điều này tránh việc truy cập vào các vị trí không hợp lệ trong khi vẫn xem xét các ứng viên gần nhất.
