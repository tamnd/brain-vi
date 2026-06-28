---
title: "CF 105123B - Mạng thần kinh"
description: "Chúng ta có một cấu trúc phân lớp trong đó mỗi lớp chứa một số nút nhất định. Giữa mỗi cặp lớp liên tiếp, mọi nút ở lớp bên trái được kết nối với mọi nút ở lớp bên phải."
date: "2026-06-27T19:32:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105123
codeforces_index: "B"
codeforces_contest_name: "BioCode 2024"
rating: 0
weight: 105123
solve_time_s: 75
verified: false
draft: false
---

[CF 105123B - Mạng thần kinh](https://codeforces.com/problemset/problem/105123/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cấu trúc phân lớp trong đó mỗi lớp chứa một số nút nhất định. Giữa mỗi cặp lớp liên tiếp, mọi nút ở lớp bên trái được kết nối với mọi nút ở lớp bên phải. Điều đó có nghĩa là nếu một lớp có`x`các nút và nút tiếp theo có`y`các nút, số cạnh giữa chúng chính xác là`x × y`. 

Nhiệm vụ là tính tổng số cạnh trên tất cả các cặp lớp liền kề và đưa ra tổng. 

Kích thước đầu vào nhỏ, tối đa 1000 lớp và mỗi lớp có tối đa 1000 nút. Điều này ngay lập tức cho chúng ta biết rằng ngay cả giải pháp O(n²) cũng sẽ an toàn, nhưng cấu trúc của vấn đề cho thấy chúng ta không cần bất kỳ giải pháp nào gần với giải pháp đó. Chúng tôi chỉ tương tác với các cặp liền kề, vì vậy chỉ cần một lần truyền tuyến tính là đủ. 

Một sai lầm ngây thơ ở đây là nghĩ theo cách xây dựng hoặc liệt kê biểu đồ. Nếu ai đó cố gắng mô phỏng rõ ràng các cạnh, chẳng hạn như bằng cách lặp qua tất cả các nút trong cả hai lớp, thì họ vẫn thực hiện tính toán chính xác một cách hiệu quả nhưng với chi phí khái niệm không cần thiết. Một cách tiếp cận không chính xác khác là hiểu sai cấu trúc và cố gắng đếm các cạnh trên toàn cầu thay vì trên mỗi cặp liền kề, điều này sẽ bỏ sót rằng chỉ các lớp liên tiếp mới quan trọng. 

Các trường hợp cạnh rất đơn giản nhưng vẫn đáng để kiểm tra. Nếu chỉ có một lớp thì không có cặp liền kề nào nên đáp án phải bằng 0. Ví dụ, đầu vào`1 / 5`nên sản xuất`0`. Nếu các lớp chứa các lớp, chẳng hạn như`1 1 1 1`, mỗi cặp đóng góp chính xác một cạnh, do đó câu trả lời sẽ trở thành số lần chuyển tiếp, tức là`n - 1`. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ xem xét rõ ràng từng cặp lớp liền kề và tính toán sản phẩm bằng cách lặp qua các nút. Đối với hai lớp kích thước`x`Và`y`, điều đó có nghĩa là lặp`x`lần và`y`lần để đếm tất cả các kết nối. Đối với tất cả các cặp liền kề, giá trị này tỷ lệ thuận với tổng các tích được tính bằng phép lặp lồng nhau, điều này không cần thiết vì phép nhân đã cho kết quả trực tiếp. 

Sự kém hiệu quả xuất phát từ việc coi mỗi cạnh như một thứ gì đó để liệt kê riêng lẻ. Vì mọi cặp nút có thể có giữa hai lớp liền kề đều đóng góp chính xác một cạnh nên chúng ta không thu được gì từ việc mô phỏng các kết nối. 

Quan sát quan trọng là cấu trúc đã cung cấp cho chúng ta công thức chính xác cho mỗi lần chuyển đổi: với mọi`i`, sự đóng góp chỉ đơn giản là`a[i] × a[i+1]`. Không có sự phụ thuộc giữa các cặp lớp khác nhau, vì vậy chúng ta có thể tích lũy kết quả trong một lần thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng các cặp nút) | O(∑ a[i]·a[i+1]) | O(1) | Quá chậm | 
| Tối ưu (nhân liền kề) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lớp`n`và danh sách`a`đại diện cho số lượng nút trong mỗi lớp. Điều này thiết lập cấu trúc mà chúng ta sẽ đi qua. 
2. Khởi tạo bộ tích lũy`ans = 0`để lưu trữ tổng số cạnh. Điều này giúp tính toán tăng dần nên chúng tôi không cần thêm bất kỳ bộ nhớ nào. 
3. Lặp lại qua các lớp từ chỉ mục`0`ĐẾN`n - 2`. Mỗi chỉ số đại diện cho một ranh giới giữa hai lớp liên tiếp. 
4. Đối với mỗi chỉ số`i`, tính toán`a[i] * a[i + 1]`và thêm nó vào`ans`. Điều này trực tiếp đếm tất cả các cạnh giữa hai lớp, vì mọi nút trong lớp đầu tiên đều kết nối với mọi nút trong lớp thứ hai. 
5. Sau khi xử lý tất cả các cặp liền kề, xuất ra`ans`. 

### Tại sao nó hoạt động 

Mỗi cạnh trong mạng chỉ tồn tại giữa hai lớp liên tiếp và mỗi cạnh như vậy được xác định duy nhất bởi một cặp nút được chọn từ hai lớp đó. Đối với một cặp lớp cố định`i`Và`i+1`, số cặp nút có thể có chính xác là tích Descartes`a[i] × a[i+1]`. Tính tổng các đóng góp độc lập này trên tất cả các cặp lớp liền kề sẽ tính mỗi cạnh chính xác một lần, không có sự chồng chéo giữa các cặp lớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ans = 0
    for i in range(n - 1):
        ans += a[i] * a[i + 1]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc cấu trúc của mạng. Sau đó, vòng lặp sẽ đi qua từng ranh giới giữa các lớp, đảm bảo chúng tôi chỉ nhân các giá trị liền kề. Bộ tích lũy`ans`được giữ dưới dạng một số nguyên duy nhất vì giá trị tối đa có thể nằm trong dung lượng số nguyên của Python. 

Một điểm tinh tế là chúng tôi không bao giờ cố gắng lưu trữ các cấu trúc cạnh trung gian. Điều này tránh được cả chi phí bộ nhớ và sự phức tạp không cần thiết. Giới hạn vòng lặp`n - 1`rất quan trọng vì mỗi phép nhân yêu cầu một cặp hợp lệ`(i, i+1)`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 5 2 7
```| tôi | một [tôi] | a[i+1] | Sản phẩm | Tổng Chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 5 | 15 | 15 | 
| 1 | 5 | 2 | 10 | 25 | 
| 2 | 2 | 7 | 14 | 39 | 

Dấu vết này cho thấy mỗi ranh giới đóng góp độc lập như thế nào. Tổng chạy tích lũy mỗi lần chuyển đổi lớp mà không bị nhiễu, xác nhận rằng chỉ riêng phần kề đã xác định tất cả các cạnh. 

### Ví dụ 2 

đầu vào:```
5
1 1 1 1 1
```| tôi | một [tôi] | a[i+1] | Sản phẩm | Tổng Chạy | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 
| 1 | 1 | 1 | 1 | 2 | 
| 2 | 1 | 1 | 1 | 3 | 
| 3 | 1 | 1 | 1 | 4 | 

Mỗi quá trình chuyển đổi đóng góp chính xác một cạnh, điều này xác nhận cách giải thích rằng mọi cặp nút đều hợp lệ và được tính một lần cho mỗi cặp lớp liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua danh sách kích thước lớp | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Các ràng buộc cho phép tối đa 1000 lớp, do đó, việc truyền tải tuyến tính là không đáng kể về mặt thời gian chạy. Các phép toán là các phép nhân và phép cộng số nguyên đơn giản, nằm trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    ans = 0
    for i in range(n - 1):
        ans += a[i] * a[i + 1]
    return str(ans)

# provided sample
assert run("4\n3 5 2 7\n") == "39"

# single layer
assert run("1\n5\n") == "0"

# all ones
assert run("4\n1 1 1 1\n") == "3"

# increasing values
assert run("3\n2 3 4\n") == "18"

# max-ish small check
assert run("2\n1000 1000\n") == "1000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`0`| trường hợp cạnh một lớp | 
|`1 1 1 1`|`3`| chuyển tiếp thống nhất | 
|`2 3 4`|`18`| phép nhân không tầm thường | 
|`1000 1000`|`1000000`| ranh giới sản phẩm tối đa | 

## Vỏ cạnh 

Đối với một đầu vào một lớp như`1 / 5`, vòng lặp`for i in range(n - 1)`không bao giờ thực thi vì`n - 1 = 0`. Bộ tích lũy vẫn còn`0`, phù hợp với thực tế là không có cặp lớp liền kề và do đó không có cạnh. 

Đối với sự kề cận tối thiểu như`2 / 3 4`, vòng lặp chạy một lần với`i = 0`. Việc tính toán là`3 × 4 = 12`, và nó được trả về ngay lập tức. Không có trạng thái ẩn hoặc chuyển tiếp giữa các lần lặp, vì vậy mỗi ranh giới hoàn toàn độc lập và được xử lý một cách an toàn một cách riêng biệt.
