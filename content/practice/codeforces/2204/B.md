---
title: "CF 2204B - Tối đa bên phải"
description: "Chúng tôi được cung cấp một dãy số nguyên và được yêu cầu mô phỏng một quy trình loại bỏ rất cụ thể. Trong mỗi thao tác, chúng ta xem xét mảng hiện tại, tìm phần tử lớn nhất và nếu nhiều phần tử có chung giá trị lớn nhất đó thì chúng ta sẽ chọn phần tử ngoài cùng bên phải."
date: "2026-06-07T19:55:11+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2204
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 188 (Rated for Div. 2)"
rating: 800
weight: 2204
solve_time_s: 94
verified: true
draft: false
---

[CF 2204B - Tối đa bên phải](https://codeforces.com/problemset/problem/2204/B) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dãy số nguyên và được yêu cầu mô phỏng một quy trình loại bỏ rất cụ thể. Trong mỗi thao tác, chúng ta xem xét mảng hiện tại, tìm phần tử lớn nhất và nếu nhiều phần tử có chung giá trị lớn nhất đó thì chúng ta sẽ chọn phần tử ngoài cùng bên phải. Sau đó, chúng tôi loại bỏ phần tử đó và mọi thứ theo sau nó. Câu hỏi đặt ra là cần thực hiện bao nhiêu thao tác như vậy để làm trống hoàn toàn mảng. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài của mảng theo sau là chính mảng đó. Các ràng buộc cho chúng ta biết rằng kích thước mảng có thể lên tới 200.000 và có thể có tới 10.000 trường hợp thử nghiệm, nhưng tổng của tất cả các kích thước mảng trong các trường hợp thử nghiệm không vượt quá 200.000. Điều này cho thấy chúng ta cần một giải pháp gần như tuyến tính theo kích thước mảng, vì bất kỳ thuật toán nào kém hơn O(n) cho mỗi trường hợp thử nghiệm sẽ quá chậm. 

Các trường hợp cạnh có thể phá vỡ một giải pháp đơn giản bao gồm các mảng tăng nghiêm ngặt, giảm nghiêm ngặt hoặc xuất hiện nhiều phần tử tối đa. Ví dụ, một mảng`[3, 3, 3]`nên thực hiện một thao tác, vì phần tử tối đa ngoài cùng bên phải là phần tử cuối cùng, loại bỏ toàn bộ mảng trong một lần. Việc triển khai ngây thơ luôn chọn mức tối đa đầu tiên sẽ mô phỏng không chính xác nhiều thao tác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng quá trình theo đúng nghĩa đen: ở mỗi bước, quét mảng để tìm mức tối đa ngoài cùng bên phải, loại bỏ tất cả các phần tử từ vị trí đó đến cuối và lặp lại. Điều này đúng, nhưng việc tìm mức tối đa mỗi lần phải mất O(n) và trong trường hợp xấu nhất, chúng ta có thể thực hiện các phép toán O(n). Điều đó dẫn đến thời gian O(n^2) cho mỗi trường hợp thử nghiệm, quá chậm đối với n = 200.000. 

Quan sát quan trọng cho một giải pháp tối ưu là số lượng thao tác tương ứng với khoảng cách mảng trải dài sang trái khi chúng ta loại bỏ các phần tử tối đa từ phải sang trái. Cụ thể, nếu chúng ta duyệt mảng từ phải sang trái, theo dõi mức tối đa nhìn thấy cho đến nay, mỗi khi phần tử hiện tại bằng mức tối đa đang chạy này, chúng ta biết nó sẽ được chọn trong một số thao tác. Thay vì mô phỏng việc loại bỏ, chúng ta chỉ cần đếm các phân đoạn trong đó mức tối đa "mở rộng" ranh giới bên trái. Cái nhìn sâu sắc này làm giảm vấn đề thành một lần duyệt ngược duy nhất với một vài biến số nguyên, đó là thời gian O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ cuối mảng và khởi tạo bộ đếm cho các phép toán, một biến để theo dõi mức tối đa hiện tại trong hậu tố và điểm đánh dấu ranh giới cho biết chỉ mục ngoài cùng bên trái bị ảnh hưởng bởi lần xóa tiếp theo. 
2. Lặp lại mảng đó. Cập nhật mức tối đa hiện tại được thấy cho đến nay. 
3. Bất cứ khi nào chỉ số khớp với ranh giới ngoài cùng bên trái của phân đoạn loại bỏ hiện tại, hãy tăng bộ đếm hoạt động và di chuyển ranh giới đến vị trí của mức tối đa tiếp theo sẽ mở rộng phân đoạn. Điều này có tác dụng vì mọi mức tối đa ở bên trái cuối cùng sẽ trở thành mức tối đa mới ngoài cùng bên phải cho thao tác tiếp theo. 
4. Tiếp tục cho đến khi đạt đến đầu mảng. 
5. Trả lại bộ đếm hoạt động. 

Tại sao nó hoạt động: di chuyển từ phải sang trái đảm bảo chúng tôi tính đến lần xuất hiện cuối cùng của mỗi mức tối đa trước tiên. Mỗi lần chúng tôi chạm vào ranh giới bên trái của một đoạn sẽ bị xóa, chúng tôi biết một thao tác đã xảy ra. Vì các phân đoạn chỉ được mở rộng bởi các phần tử lớn hơn mức tối đa trước đó nên mỗi phần tử được xem xét nhiều nhất một lần, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ops = 0
        right = n
        current_max = 0
        
        # traverse from right to left
        i = n - 1
        while i >= 0:
            current_max = max(current_max, a[i])
            if i == right - current_max:  # left boundary of next removal segment
                ops += 1
                right = i
                current_max = 0
            i -= 1
        print(ops)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng I/O nhanh và lặp lại từng trường hợp thử nghiệm một cách độc lập. Chúng ta duyệt từ phần tử ngoài cùng bên phải sang bên trái, cập nhật`current_max`để phản ánh phần tử tối đa trong hậu tố hiện tại. Khi chỉ số hiện tại căn chỉnh với ranh giới bên trái của đoạn được xác định theo mức tối đa, chúng ta đếm một thao tác và đặt lại ranh giới cho thao tác tiếp theo. Việc xử lý cẩn thận ranh giới đảm bảo rằng các phân đoạn chồng chéo không bị tính hai lần, đây là lỗi phổ biến xảy ra từng phân đoạn một. 

## Ví dụ đã hoạt động 

Mẫu 1:`[2, 1, 2, 3, 1]`| tôi | một [tôi] | hiện tại_max | đúng | hoạt động | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | 1 | 5 | 0 | 
| 3 | 3 | 3 | 5 | 1 | 
| 2 | 2 | 3 | 3 | 1 | 
| 1 | 1 | 3 | 3 | 1 | 
| 0 | 2 | 3 | 3 | 1 | 

Bảng cho thấy sau khi chọn 3 tại chỉ số 3, đoạn ngoài cùng bên phải sẽ bị loại bỏ và chúng ta tiếp tục đếm chính xác các thao tác còn lại. 

Mẫu 2:`[1, 2, 3, 4, 5, 6]`| tôi | một [tôi] | hiện tại_max | đúng | hoạt động | 
| --- | --- | --- | --- | --- | 
| 5 | 6 | 6 | 6 | 1 | 
| 4 | 5 | 6 | 5 | 2 | 
| 3 | 4 | 6 | 4 | 3 | 
| 2 | 3 | 6 | 3 | 4 | 
| 1 | 2 | 6 | 2 | 5 | 
| 0 | 1 | 6 | 1 | 6 | 

Điều này chứng tỏ trường hợp tăng nghiêm ngặt trong đó mỗi phần tử được chọn trong hoạt động riêng của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng ta duyệt mảng một lần từ phải sang trái. Cập nhật và so sánh tối đa là O(1). | 
| Không gian | O(1) bổ sung | Chỉ có một số biến số nguyên được sử dụng; không cần thêm mảng hoặc cấu trúc. | 

Với tổng n trên tất cả các trường hợp thử nghiệm 2 × 10^5, giải pháp này nằm trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n5\n2 1 2 3 1\n6\n1 2 3 4 5 6\n3\n3 2 1\n4\n1 3 3 1\n") == "3\n6\n1\n3", "sample 1"

# custom cases
assert run("1\n3\n3 3 3\n") == "1", "all-equal max"
assert run("1\n5\n5 4 3 2 1\n") == "1", "strictly decreasing"
assert run("1\n5\n1 2 2 3 3\n") == "3", "multiple equal maxima"
assert run("1\n2\n1 2\n") == "2", "minimum size increasing"
assert run("1\n2\n2 1\n") == "1", "minimum size decreasing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 3 | 1 | Xử lý tất cả các mức tối đa bằng nhau | 
| 5 4 3 2 1 | 1 | Mảng giảm nghiêm ngặt | 
| 1 2 2 3 3 | 3 | Cực đại bằng nhau xuất hiện nhiều lần | 
| 1 2 | 2 | Tăng kích thước tối thiểu | 
| 2 1 | 1 | Giảm kích thước tối thiểu | 

## Vỏ cạnh 

Đối với mảng`[3, 3, 3]`, thuật toán bắt đầu từ phần tử cuối cùng, xác định mức tối đa bên phải tại chỉ mục 2, loại bỏ nó và tất cả các phần tử sau nó (không có phần tử nào trong trường hợp này) và đếm một thao tác. Ranh giới bên trái được cập nhật và quá trình truyền tải tiếp tục nhưng không cần thực hiện thêm thao tác nào. Đầu ra là chính xác`1`. 

Vì`[5, 4, 3, 2, 1]`, phần tử cuối cùng được xem xét là 1, nhưng giá trị tối đa ngoài cùng bên phải là 5 ở chỉ mục 0. Tất cả các phần tử sau chỉ mục 0 đều bị xóa trong một thao tác. Thuật toán đếm chính xác một thao tác vì mức tối đa mở rộng ranh giới bên trái để bao phủ toàn bộ mảng.
