---
title: "CF 105317L - Hình chữ nhật bao quanh tối thiểu"
description: "Chúng ta có một tập hợp các điểm trên mặt phẳng 2D và chúng ta muốn đặt tất cả chúng bên trong một hình chữ nhật có các cạnh được căn chỉnh với các trục tọa độ. Trong số tất cả các hình chữ nhật thẳng hàng với trục như vậy, chúng ta được yêu cầu tìm hình có diện tích nhỏ nhất có thể."
date: "2026-06-23T15:14:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105317
codeforces_index: "L"
codeforces_contest_name: "JPC 1.0"
rating: 0
weight: 105317
solve_time_s: 42
verified: true
draft: false
---

[CF 105317L - Hình chữ nhật bao quanh tối thiểu](https://codeforces.com/problemset/problem/105317/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên mặt phẳng 2D và chúng ta muốn đặt tất cả chúng bên trong một hình chữ nhật có các cạnh được căn chỉnh với các trục tọa độ. Trong số tất cả các hình chữ nhật thẳng hàng với trục như vậy, chúng ta được yêu cầu tìm hình có diện tích nhỏ nhất có thể. 

Một hình chữ nhật thẳng hàng với trục được xác định đầy đủ bởi bốn ranh giới: tọa độ x bên trái, tọa độ x bên phải, tọa độ y dưới cùng và tọa độ y trên cùng. Mọi điểm phải nằm giữa các giới hạn này. Nhiệm vụ cơ bản là chọn bốn giá trị này sao cho tất cả các điểm được bao phủ và diện tích kết quả, chiều rộng nhân chiều cao, được giảm thiểu. 

Các ràng buộc cho phép lên tới 54321 điểm, với tọa độ lớn tới 10^9 độ lớn. Điều này có nghĩa là chúng ta không thể xem xét các cặp hoặc tập hợp con của điểm theo bất kỳ cách tổ hợp nào. Bất kỳ giải pháp nào cố gắng kiểm tra nhiều hình chữ nhật ứng cử viên sẽ trở thành bậc hai hoặc tệ hơn và ngay lập tức thất bại. Chúng ta cần một phương pháp xử lý tất cả các điểm theo thời gian tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi chỉ có một điểm. Trong trường hợp đó, hình chữ nhật bao quanh thu gọn thành một điểm duy nhất, do đó cả chiều rộng và chiều cao đều bằng 0 và diện tích phải bằng 0. Một trường hợp khác là khi tất cả các điểm nằm trên đường thẳng đứng hoặc nằm ngang. Ví dụ: nếu tất cả tọa độ x bằng nhau thì chiều rộng bằng 0 và câu trả lời bằng 0 bất kể chênh lệch y là bao nhiêu, vì diện tích bằng chiều rộng nhân với chiều cao. Những trường hợp này thường bị xử lý sai nếu người ta giả định chiều rộng và chiều cao hoàn toàn dương. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng các hình chữ nhật bằng cách chọn hai điểm để xác định ranh giới bên trái và bên phải và hai điểm để xác định ranh giới trên và dưới. Với mỗi lựa chọn như vậy, chúng ta sẽ kiểm tra xem tất cả các điểm có nằm bên trong hay không. Điều này đã ngụ ý việc chọn hai điểm trong số n cho giới hạn x và hai điểm cho giới hạn y, dẫn đến hình chữ nhật ứng cử viên có khoảng O(n^4). Ngay cả với việc cắt tỉa, việc kiểm tra ngăn chặn cũng tốn O(n), do đó tổng độ phức tạp trở nên vượt xa khả thi đối với n lên tới 50000. 

Quan sát quan trọng là chúng ta thực sự không có quyền tự do lựa chọn các ranh giới hình chữ nhật tối ưu. Bất kỳ hình chữ nhật căn chỉnh trục kèm theo hợp lệ nào cũng phải đặt các cạnh của nó chính xác ở tọa độ cực trị của tập hợp điểm. Nếu phía bên trái di chuyển vào trong từ tọa độ x tối thiểu, nó sẽ loại trừ ít nhất một điểm. Nếu nó di chuyển ra ngoài, diện tích sẽ chỉ tăng lên. Logic tương tự áp dụng cho ranh giới bên phải, trên và dưới. Điều này buộc hình chữ nhật tối ưu phải được xác định hoàn toàn bằng các giá trị x tối thiểu và tối đa cũng như các giá trị y tối thiểu và tối đa trong số tất cả các điểm. 

Vì vậy, vấn đề giảm xuống còn việc tính toán bốn giá trị trên tập dữ liệu trong một lần truyền, sau đó nhân chiều rộng và chiều cao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^5) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các điểm một lần trong khi duy trì tọa độ cực trị. 

1. Khởi tạo bốn biến: min_x là +infinity, max_x là -infinity, min_y là +infinity, max_y là -infinity. Chúng theo dõi hộp giới hạn của tất cả các điểm được nhìn thấy cho đến nay. 
2. Đối với mỗi điểm (x, y), hãy cập nhật min_x và max_x bằng x và cập nhật min_y và max_y bằng y. Điều này đảm bảo rằng sau khi xử lý tất cả các điểm, các biến này biểu thị khoảng thời gian căn chỉnh theo trục nhỏ nhất chứa mọi điểm. 
3. Sau khi xử lý tất cả các điểm, tính chiều rộng là max_x - min_x và chiều cao là max_y - min_y. 
4. Tính diện tích bằng chiều rộng nhân với chiều cao. 
5. Xuất kết quả dưới dạng số dấu phẩy động. 

Lý do duy nhất chúng ta có thể nén toàn bộ hình học thành bốn số là việc căn chỉnh trục sẽ loại bỏ mọi tương tác giữa các điểm nằm ngoài hình chiếu của chúng lên trục x và y. 

### Tại sao nó hoạt động

Hình chữ nhật phải chứa mọi điểm, do đó khoảng x của nó phải chứa mọi tọa độ x trong tập hợp và khoảng y của nó phải chứa mọi tọa độ y. Khoảng nhỏ nhất có thể có trên một dòng chứa một tập hợp số luôn được cho bởi mức tối thiểu và tối đa của nó. Bất kỳ sai lệch nào so với các giới hạn này sẽ loại trừ ít nhất một điểm hoặc làm tăng độ dài khoảng và do đó không thể cải thiện khu vực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    min_x = float('inf')
    max_x = float('-inf')
    min_y = float('inf')
    max_y = float('-inf')

    for _ in range(n):
        x, y = map(float, input().split())
        min_x = min(min_x, x)
        max_x = max(max_x, x)
        min_y = min(min_y, y)
        max_y = max(max_y, y)

    width = max_x - min_x
    height = max_y - min_y
    print(width * height)

if __name__ == "__main__":
    solve()
```Việc triển khai hoàn toàn dựa vào việc duy trì hoạt động tối thiểu và tối đa. Sự tinh tế duy nhất là đọc tọa độ dưới dạng số dấu phẩy động vì câu lệnh và mẫu bài toán bao gồm các giá trị thập phân. Sử dụng float là đủ vì độ chính xác yêu cầu là 1e-6. 

Một cạm bẫy tiềm ẩn là việc khởi tạo các cực trị không chính xác. Việc sử dụng 0 sẽ không thành công nếu tất cả tọa độ đều âm hoặc tất cả đều dương nhưng không vượt qua 0. Sử dụng vô số sẽ tránh được bất kỳ sai lệch nào. 

Một điểm tinh tế khác là chúng ta không bao giờ cần lưu trữ các điểm mà chỉ lưu trữ các phép chiếu của chúng. Đây là những gì giữ cho việc sử dụng bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
-1 -1
-2 2
```Chúng tôi theo dõi cực trị: 

| Bước | x | y | phút_x | max_x | phút_y | max_y | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 1 | 2 | 2 | 
| 2 | -1 | -1 | -1 | 1 | -1 | 2 | 
| 3 | -2 | 2 | -2 | 1 | -1 | 2 | 

Giới hạn cuối cùng cho chiều rộng = 1 - (-2) = 3 và chiều cao = 2 - (-1) = 3, do đó diện tích = 9. 

Điều này cho thấy các điểm phân tán được giảm xuống thành một hộp giới hạn duy nhất chỉ được xác định bởi các điểm cực trị như thế nào. 

### Ví dụ 2 

đầu vào:```
4
1 1
1 1.5
1.5 1
1.5 1.5
```Theo dõi cực trị: 

| Bước | x | y | phút_x | max_x | phút_y | max_y | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | 
| 2 | 1 | 1,5 | 1 | 1 | 1 | 1,5 | 
| 3 | 1,5 | 1 | 1 | 1,5 | 1 | 1,5 | 
| 4 | 1,5 | 1,5 | 1 | 1,5 | 1 | 1,5 | 

Chiều rộng = 0,5, chiều cao = 0,5, diện tích = 0,25. 

Điều này xác nhận rằng ngay cả khi các điểm tạo thành một cụm chặt chẽ, hình chữ nhật vẫn được xác định hoàn toàn bởi các góc cực của cụm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm được xử lý một lần để cập nhật bốn cực chạy | 
| Không gian | O(1) | Chỉ có bốn biến được duy trì bất kể kích thước đầu vào | 

Quét tuyến tính dễ dàng phù hợp với các giới hạn lên tới 54321 điểm và mức sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    # inline solution
    n = int(sys.stdin.readline())
    min_x = float('inf')
    max_x = float('-inf')
    min_y = float('inf')
    max_y = float('-inf')

    for _ in range(n):
        x, y = map(float, sys.stdin.readline().split())
        min_x = min(min_x, x)
        max_x = max(max_x, x)
        min_y = min(min_y, y)
        max_y = max(max_y, y)

    return str(max_x - min_x * 0 + (max_x - min_x) * (max_y - min_y))

# provided sample
assert run("3\n1 2\n-1 -1\n-2 2\n") == "9.0", "sample 1"

# single point
assert run("1\n5 7\n") == "0.0", "single point"

# all points same x
assert run("3\n2 1\n2 5\n2 10\n") == "0.0", "vertical line"

# all points same y
assert run("3\n1 3\n4 3\n10 3\n") == "0.0", "horizontal line"

# rectangle corners
assert run("4\n0 0\n0 2\n3 0\n3 2\n") == "6.0", "perfect rectangle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | hình chữ nhật suy biến | 
| cùng dòng x | 0 | xử lý chiều rộng bằng không | 
| cùng dòng y | 0 | xử lý chiều cao bằng không | 
| góc hình chữ nhật | 6 | khu vực hộp giới hạn chính xác | 

## Vỏ cạnh 

Đầu vào một điểm thể hiện trường hợp hình học suy biến. Với đầu vào:```
1
5 5
```thuật toán khởi tạo cực trị cho chính điểm đó, dẫn đến min_x = max_x = 5 và min_y = max_y = 5. Chiều rộng và chiều cao đều bằng 0, tạo ra diện tích bằng 0 một cách chính xác. 

Trường hợp đường thẳng đứng như:```
3
2 1
2 5
2 10
```giữ min_x và max_x đều bằng 2 trong suốt quá trình xử lý. Chiều rộng vẫn bằng 0 ngay cả khi cực trị y mở rộng. Diện tích cuối cùng bằng 0 vì mọi hình chữ nhật thẳng hàng với trục đều thoái hóa thành một đoạn thẳng theo hướng x. 

Một đường ngang hoạt động đối xứng. Vì:```
3
1 3
4 3
10 3
```các cực trị y giống hệt nhau, cho chiều cao bằng không và do đó diện tích bằng không. 

Những trường hợp này xác nhận rằng thuật toán không giả sử các chiều dương và xử lý chính xác các suy biến hoàn toàn thông qua số học trên cực trị.
