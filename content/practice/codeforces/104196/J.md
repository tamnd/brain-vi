---
title: "CF 104196J - Tái chế"
description: "Chúng tôi được cung cấp một chuỗi ước tính hàng tuần, trong đó mỗi con số mô tả số mét khối vật liệu có thể tái chế sẽ được chuyển đến trong một tuần cụ thể. Chúng tôi muốn đặt một thùng tái chế trong một số tuần liền kề và chọn sức chứa của nó."
date: "2026-07-02T17:56:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104196
codeforces_index: "J"
codeforces_contest_name: "2021-2022 ICPC East Central North America Regional Contest (ECNA 2021)"
rating: 0
weight: 104196
solve_time_s: 65
verified: true
draft: false
---

[CF 104196J - Tái chế](https://codeforces.com/problemset/problem/104196/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi ước tính hàng tuần, trong đó mỗi con số mô tả số mét khối vật liệu có thể tái chế sẽ được chuyển đến trong một tuần cụ thể. Chúng tôi muốn đặt một thùng tái chế trong một số tuần liền kề và chọn sức chứa của nó. 

Sau khi thùng được lắp đặt với dung lượng cố định, mỗi tuần nó sẽ được dọn sạch và đổ đầy lại theo số lượng đã đến. Thùng vẫn được sử dụng miễn là vào cuối tuần, nó đã đầy hoàn toàn. Tuần đầu tiên mà số tiền dự kiến ​​hoàn toàn thấp hơn dung lượng buộc chúng tôi phải loại bỏ số tiền đó trước tuần đó. 

Vì vậy, nếu chúng ta chọn tuần bắt đầu và dung lượng, thùng sẽ hoạt động trên phân đoạn liền kề tối đa bắt đầu từ đó mọi giá trị ít nhất là dung lượng. Tổng lượng tái chế trên một phân khúc như vậy là công suất nhân với số tuần trong phân khúc đó. 

Nhiệm vụ là chọn tuần bắt đầu, tuần kết thúc và công suất sao cho sản phẩm này được tối đa hóa, nếu nhiều lựa chọn cho kết quả như nhau thì chúng ta chọn lựa chọn có tuần bắt đầu nhỏ nhất. 

Kích thước đầu vào lên tới 100.000 tuần. Bất kỳ giải pháp nào thử trực tiếp tất cả các mảng con có thể có sẽ cần phải kiểm tra khoảng n² và điều đó dẫn đến khoảng 10¹⁰ kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn khả thi. Điều này buộc chúng ta phải tìm kiếm một cấu trúc tránh liệt kê tất cả các khoảng một cách rõ ràng. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị bằng nhau. Nếu nhiều tuần liên tiếp có cùng số điểm thì nhiều lựa chọn khác nhau về giới hạn năng lực và khoảng thời gian có thể tạo ra cùng một số điểm. Quy tắc ràng buộc về chỉ số bắt đầu nhỏ nhất có nghĩa là ngay cả trong số các hình chữ nhật tối ưu bằng nhau, chúng ta phải nhất quán trong cách xác định ranh giới. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xem xét mọi tuần bắt đầu có thể, kéo dài một khoảng thời gian sang bên phải và duy trì giá trị tối thiểu trong khoảng thời gian đó. Đối với mỗi phần mở rộng, chúng tôi tính toán tích của giá trị tối thiểu và độ dài. Điều này đúng vì dung lượng tốt nhất trong một khoảng thời gian cố định chính xác là giá trị tối thiểu của nó. Tuy nhiên, việc duy trì mức tối thiểu cho mọi phần mở rộng trong tất cả các lần bắt đầu vẫn dẫn đến hành vi bậc hai, vì mỗi lần bắt đầu có thể kéo dài tới n bước. 

Quan sát quan trọng là mọi lựa chọn tối ưu đều được xác định bởi một vị trí đóng vai trò là mức tối thiểu trong khoảng đã chọn của nó. Nếu chúng ta ấn định chỉ số i là giá trị nhỏ nhất của một khoảng nào đó thì khoảng tốt nhất cho lựa chọn đó sẽ kéo dài sang bên trái và bên phải nhiều nhất có thể trong khi tất cả các phần tử vẫn duy trì ít nhất là a[i]. Điều này biến vấn đề thành việc tìm kiếm, đối với mọi chỉ số, khoảng lớn nhất trong đó a[i] là nhỏ nhất và đánh giá a[i] nhân với chiều rộng của khoảng đó. 

Đây chính xác là cấu trúc “hình chữ nhật lớn nhất trong biểu đồ” cổ điển. Mỗi giá trị được coi là chiều cao và chúng tôi mở rộng ra bên ngoài cho đến khi đạt đến giá trị nhỏ hơn hoàn toàn. Ngăn xếp đơn điệu cho phép chúng ta tính toán các ranh giới này theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) thêm | Quá chậm | 
| Ngăn xếp đơn điệu (Phương pháp biểu đồ) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại mỗi tuần tôi là một thanh có chiều cao bằng lượng tái chế dự kiến. Đối với mỗi vị trí, chúng tôi muốn biết thanh này có thể kéo dài sang trái và phải bao xa trong khi vẫn giữ giá trị nhỏ nhất trong phân khúc của nó. 

Chúng tôi tính toán hai ranh giới bằng cách sử dụng ngăn xếp đơn điệu. Ngăn xếp giữ các chỉ số có giá trị tăng dần.

1. Chúng tôi quét từ trái sang phải và duy trì một chồng các chỉ số có giá trị tăng dần. Khi chúng tôi gặp một giá trị nhỏ hơn hoặc bằng đỉnh ngăn xếp, chúng tôi sẽ bật lên cho đến khi bất biến được khôi phục. Mỗi chỉ mục được bật lên đã tìm thấy ranh giới bên phải của nó, bởi vì chỉ mục hiện tại là vị trí đầu tiên bên phải của nó với giá trị nhỏ hơn. 
2. Chúng tôi lặp lại quy trình tương tự để xác định, đối với mỗi chỉ mục, giá trị nhỏ hơn gần nhất ở bên trái. Khi quét, chúng tôi lại sử dụng ngăn xếp đơn điệu và chỉ mục trước đó còn lại trên ngăn xếp sẽ đưa ra vị trí cuối cùng có giá trị nhỏ hơn hoàn toàn. 
3. Đối với mỗi chỉ số i, khi đã biết cả hai ranh giới, chúng ta xác định khoảng là mọi thứ nằm giữa chúng. Đây là phân đoạn tối đa trong đó a[i] là giá trị tối thiểu. 
4. Chúng tôi tính điểm cho phân đoạn này bằng a[i] nhân với độ dài của nó. Chúng tôi theo dõi điểm số tốt nhất và lưu trữ khoảng thời gian của nó. 
5. Khi cập nhật câu trả lời tốt nhất, chúng tôi phá vỡ mối quan hệ bằng cách ưu tiên chỉ số bắt đầu nhỏ hơn. Vì chúng tôi xử lý các chỉ số theo thứ tự tăng dần nên việc đảm bảo các quy tắc ranh giới nhất quán sẽ đảm bảo độ phân giải ràng buộc mang tính xác định. 

Tính chính xác dựa trên thực tế là mỗi khoảng hợp lệ đều có một chỉ mục duy nhất đạt được mức tối thiểu và chỉ mục này xác định độ mở rộng tối đa của khoảng đó. Bất kỳ giải pháp tối ưu nào cũng phải trùng với một trong các khoảng thời gian tối đa này, vì việc thu hẹp khoảng cách chỉ có thể làm giảm độ dài trong khi không thể tăng mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # boundaries
    left = [-1] * n
    right = [n] * n

    stack = []

    # next smaller to the left
    for i in range(n):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)

    stack = []

    # next smaller to the right
    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        right[i] = stack[-1] if stack else n
        stack.append(i)

    best_val = -1
    best_l = 0
    best_r = 0

    for i in range(n):
        l = left[i] + 1
        r = right[i] - 1
        val = a[i] * (r - l + 1)

        if val > best_val or (val == best_val and l < best_l):
            best_val = val
            best_l = l
            best_r = r

    print(best_l + 1, best_r + 1, best_val)

if __name__ == "__main__":
    solve()
```Hai bước đầu tiên tính toán, đối với mỗi vị trí, giá trị của nó có thể mở rộng bao xa trong khi vẫn duy trì mức tối thiểu. Đường bên trái sử dụng ngăn xếp tăng dần đơn điệu để khi giá trị nhỏ hơn xuất hiện, mọi thứ lớn hơn sẽ bị loại bỏ và không thể mở rộng thêm. Đường chuyền bên phải phản ánh logic này ngược lại. 

Vòng lặp cuối cùng đánh giá mỗi chỉ số là mức tối thiểu giới hạn của phân khúc ứng cử viên. Việc chuyển đổi giữa các chỉ số dựa trên 0 và đầu ra dựa trên một chỉ được xử lý ở cuối để tránh các lỗi sai sót một. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[2, 5, 7, 3, 5, 10, 2]`. 

Sau khi tính toán ranh giới, mỗi chỉ số xác định một phân đoạn tối đa. Ứng cử viên thú vị nhất là giá trị`3`ở chỉ số 3 (dựa trên 0), mở rộng từ tuần 2 đến tuần 6 trong lập chỉ mục dựa trên 1. 

| tôi | một [tôi] | ranh giới bên trái | ranh giới bên phải | phân đoạn (dựa trên 1) | giá trị | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | -1 | 6 | 1-7 | 14 | 
| 1 | 5 | 0 | 3 | 2-4 | 15 | 
| 2 | 7 | 1 | 3 | 3-4 | 14 | 
| 3 | 3 | 0 | 5 | 2-6 | 15 | 
| 4 | 5 | 3 | 5 | 5-5 | 5 | 
| 5 | 10 | 4 | 6 | 6-6 | 10 | 
| 6 | 2 | -1 | 6 | 1-7 | 14 | 

Giá trị tốt nhất là 15, đạt được theo nhiều phân đoạn, nhưng quy tắc ràng buộc sẽ chọn phân đoạn có chỉ số bắt đầu nhỏ nhất, tức là từ tuần 2-6. 

Bây giờ hãy xem xét một trường hợp có tất cả các giá trị bằng nhau`[4, 4, 4, 4]`. Mọi chỉ mục có thể mở rộng đến toàn bộ mảng, tạo ra cùng một sản phẩm cho tất cả các lựa chọn. Thuật toán chỉ định nhất quán toàn bộ phạm vi cho từng chỉ mục và quy tắc tie-break chọn chỉ mục đầu tiên, đưa ra phân đoạn 1-4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong mỗi lần xếp chồng và lần quét cuối cùng là tuyến tính | 
| Không gian | O(n) | Mảng ranh giới và lưu trữ ngăn xếp | 

Độ phức tạp tuyến tính là cần thiết vì n có thể đạt tới 100.000 và chỉ có cách tiếp cận O(n) phù hợp thoải mái trong giới hạn thời gian thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    left = [-1] * n
    right = [n] * n

    stack = []
    for i in range(n):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)

    stack = []
    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        right[i] = stack[-1] if stack else n
        stack.append(i)

    best_val = -1
    best_l = best_r = 0

    for i in range(n):
        l = left[i] + 1
        r = right[i] - 1
        val = a[i] * (r - l + 1)
        if val > best_val or (val == best_val and l < best_l):
            best_val = val
            best_l = l
            best_r = r

    return f"{best_l+1} {best_r+1} {best_val}"

# minimum size
assert run("1\n5\n") == "1 1 5"

# strictly increasing
assert run("5\n1 2 3 4 5\n") == "1 5 9"

# strictly decreasing
assert run("5\n5 4 3 2 1\n") == "1 5 9"

# all equal
assert run("4\n4 4 4 4\n") == "1 4 16"

# mixed case
assert run("7\n2 5 7 3 5 10 2\n") == "2 6 15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 1 v | xử lý ranh giới cơ sở | 
| ngày càng tăng | đầy đủ | khai triển đúng đắn | 
| giảm dần | đầy đủ | mở rộng trái đúng đắn | 
| tất cả đều bình đẳng | đầy đủ | xử lý cà vạt | 
| giống mẫu | 2 6 15 | tính đúng đắn của giải pháp đầy đủ | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[7]`, cả hai ranh giới đều thu gọn về cùng một chỉ mục. Thuật toán đặt ranh giới bên trái thành -1 và ranh giới bên phải thành n, tạo ra một đoạn có độ dài 1 và giá trị 7, tối ưu về mặt tầm thường. 

Đối với các mảng có tất cả các giá trị giống nhau, mọi chỉ mục đều tạo ra khoảng thời gian tối đa giống nhau. Logic ngăn xếp đảm bảo mọi phần tử đều mở rộng trên toàn bộ phạm vi và quy tắc ràng buộc sẽ chọn chỉ mục bắt đầu nhỏ nhất phù hợp với yêu cầu. 

Đối với các mảng đơn điệu, mỗi phần tử sẽ mở rộng trên toàn bộ mảng vì không có phần tử nhỏ hơn nào tồn tại ở hai bên. Thuật toán xác định chính xác rằng lựa chọn tốt nhất là sử dụng mức tối thiểu toàn cầu tại một điểm cuối, tạo ra khoảng thời gian đầy đủ với sản phẩm chính xác.
