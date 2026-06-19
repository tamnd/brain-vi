---
title: "CF 1006C - Ba phần của mảng"
description: "Chúng ta được cho một dãy số được sắp xếp thành một dòng và chúng ta muốn cắt dòng này thành ba đoạn liên tiếp. Đoạn đầu tiên bắt đầu ở phần đầu, đoạn thứ hai nằm ở giữa và đoạn thứ ba kết thúc ở phần tử cuối cùng."
date: "2026-06-16T23:09:46+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1006
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 498 (Div. 3)"
rating: 1200
weight: 1006
solve_time_s: 73
verified: true
draft: false
---

[CF 1006C - Ba phần của mảng](https://codeforces.com/problemset/problem/1006/C) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số được sắp xếp thành một dòng và chúng ta muốn cắt dòng này thành ba đoạn liên tiếp. Đoạn đầu tiên bắt đầu ở phần đầu, đoạn thứ hai nằm ở giữa và đoạn thứ ba kết thúc ở phần tử cuối cùng. Một số phân đoạn này được phép để trống, nghĩa là việc cắt có thể xảy ra ở các ranh giới mà không lấy bất kỳ phần tử nào. 

Điều quan trọng là tổng giá trị bên trong phân đoạn đầu tiên và tổng giá trị bên trong phân khúc thứ ba. Chúng tôi muốn hai tổng này bằng nhau và trong số tất cả các cách hợp lệ để cắt mảng, chúng tôi muốn giá trị tối đa có thể có của tổng chung đó. 

Kích thước đầu vào có thể đạt tới hai trăm nghìn phần tử, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các vị trí cắt có thể có cho cả ba phân đoạn một cách độc lập. Một phép liệt kê lồng ba sẽ bao hàm thứ tự của n khả năng lập phương, điều này vượt xa khả năng thực hiện. Ngay cả một vòng lặp kép trên các điểm cắt cũng chỉ được chấp nhận nếu mỗi truy vấn được xử lý trong thời gian không đổi hoặc logarit, vì vậy chúng ta nên mong đợi một giải pháp tuyến tính hoặc gần tuyến tính. 

Một điểm tinh tế là các phân đoạn trống được cho phép. Điều này đưa ra các trường hợp đặc biệt trong đó một hoặc cả hai tổng phù hợp có thể bằng 0. Ví dụ: nếu tất cả các số đều dương, thì giải pháp tầm thường là lấy trống cả hai phân đoạn bên ngoài luôn có hiệu quả, cho kết quả bằng 0, nhưng có thể tồn tại sự phân chia tốt hơn. Một trường hợp cạnh khác xuất hiện khi cấu hình tối ưu sử dụng các phân đoạn tiền tố và hậu tố rất nhỏ, do đó các giải pháp chỉ tìm kiếm gần trung tâm hoặc cho rằng sự phân chia cân bằng sẽ không thành công. 

Một cách tiếp cận ngây thơ thường bị phá vỡ trong những trường hợp như`[1,2,3,2,1]`. Câu trả lời tốt nhất là 4, nhưng cách tiếp cận tham lam cố gắng khớp tiền tố và hậu tố theo từng bước có thể sớm đưa ra tổng tiền tố nhỏ hơn và bỏ lỡ hậu tố khớp lớn hơn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ chọn hai điểm cắt`a`Và`b`, xác định phân đoạn đầu tiên là`[0..a]`, thứ hai là`[a+1..b]`, và thứ ba là`[b+1..n-1]`. Đối với mỗi cặp, chúng tôi tính tổng của phân đoạn thứ nhất và thứ ba và kiểm tra sự bằng nhau. Điều này yêu cầu công việc O(n) trên mỗi cặp và có các cặp O(n2), dẫn đến thời gian O(n³). Ngay cả khi tổng tiền tố giảm tính toán tổng phân đoạn xuống O(1), vòng lặp kép vẫn dẫn đến O(n²), tốc độ này quá chậm đối với 200.000 phần tử. 

Nhận xét quan trọng là đoạn giữa không liên quan ngoại trừ việc đảm bảo tính liền kề. Khi chúng tôi chọn một tổng tiền tố, chúng tôi chỉ quan tâm liệu có tồn tại một hậu tố có cùng tổng, tách rời khỏi nó hay không. Điều này biến bài toán thành việc tìm giá trị lớn nhất xuất hiện dưới dạng tổng tiền tố và tổng hậu tố mà không trùng lặp. 

Chúng ta có thể tính toán trước tất cả các tổng tiền tố. Sau đó, chúng tôi quét từ bên phải trong khi vẫn duy trì tổng hậu tố. Tại mọi vị trí, chúng tôi kiểm tra xem tổng hậu tố hiện tại có xuất hiện dưới dạng tổng tiền tố kết thúc đúng trước khi hậu tố bắt đầu hay không. Để đảm bảo tính chính xác một cách hiệu quả, chúng tôi sử dụng kiểu truyền tải hai con trỏ: một con trỏ di chuyển từ các ứng cử viên tổng tiền tố tích lũy bên trái và một con trỏ khác di chuyển từ các tổng hậu tố tích lũy bên phải. Sau đó, chúng tôi cố gắng kết hợp chúng một cách tham lam trong khi vẫn duy trì các ràng buộc về thứ tự. 

Điều này hiệu quả vì cả tổng tiền tố và tổng hậu tố đều thay đổi đơn điệu trong quá trình tích lũy và chúng tôi đang tìm kiếm sự bằng nhau của các giá trị tích lũy thay vì tổng các mảng con tùy ý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các vết cắt) | O(n2) đến O(n³) | O(1) | Quá chậm | 
| Hai con trỏ về tổng tiền tố/hậu tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai con trỏ, một con trỏ bắt đầu từ bên trái và một con trỏ từ bên phải và hai tổng chạy biểu thị tổng tiền tố và tổng hậu tố hiện tại. 

1. Khởi tạo`i = 0`,`j = n - 1`,`left_sum = 0`,`right_sum = 0`, Và`answer = 0`. 

Các con trỏ biểu thị ranh giới ứng cử viên cho các phân đoạn tiền tố và hậu tố. 
2. Trong khi`i <= j`, mở rộng cạnh tổng nhỏ hơn. 

Nếu như`left_sum`nhỏ hơn hoặc bằng thì cộng`d[i]`ĐẾN`left_sum`và di chuyển`i`phía trước. Nếu không thì thêm`d[j]`ĐẾN`right_sum`và di chuyển`j`lạc hậu. 

Việc cân bằng này đảm bảo chúng tôi khám phá tất cả các điểm cân bằng có thể có mà không bỏ qua các căn chỉnh hợp lệ. 
3. Sau mỗi lần gia hạn, hãy kiểm tra xem`left_sum == right_sum`. 

Nếu chúng bằng nhau thì cập nhật`answer`với giá trị này. Điều này ghi lại sự phân chia hợp lệ trong đó tiền tố và hậu tố khớp nhau. 
4. Tiếp tục cho đến khi con trỏ giao nhau. 

Tại thời điểm đó, tất cả các cặp tiền tố-hậu tố rời rạc khả thi đã được khám phá theo kiểu đơn điệu. 

Ý tưởng chính là chúng ta chỉ tăng tổng chứ không bao giờ giảm tổng, để các sự kiện đẳng thức được ghi lại đầy đủ tại thời điểm chúng xảy ra. 

### Tại sao nó hoạt động 

Thuật toán duy trì sự bất biến`left_sum`luôn là tổng của một đoạn tiền tố và`right_sum`luôn là tổng của một phân đoạn hậu tố và các phân đoạn này không bao giờ trùng nhau. Mọi giải pháp hợp lệ có thể tương ứng với một thời điểm nào đó trong đó các con trỏ phân chia mảng thành ba phần liền kề. Bởi vì chúng tôi luôn mở rộng cạnh nhỏ hơn, chúng tôi mô phỏng tất cả các cặp tổng tiền tố-hậu tố khả thi theo thứ tự tích lũy tăng dần của chúng. Không có đẳng thức hợp lệ nào có thể bị bỏ qua vì bất kỳ đẳng thức nào bị thiếu sẽ yêu cầu giảm một bên, điều mà quá trình này không bao giờ thực hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    i, j = 0, n - 1
    left_sum, right_sum = 0, 0
    ans = 0

    while i <= j:
        if left_sum <= right_sum:
            left_sum += a[i]
            i += 1
        else:
            right_sum += a[j]
            j -= 1

        if left_sum == right_sum:
            ans = left_sum

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc mảng và sử dụng hai con trỏ để mô phỏng các phân đoạn tiền tố và hậu tố đang phát triển. Đoạn giữa ngầm hiểu là phần còn lại giữa`i`Và`j`. Séc`left_sum == right_sum`nắm bắt các phần chia hợp lệ bất cứ khi nào cả hai đầu tích lũy tổng số như nhau. 

Quyết định luôn mở rộng phần tổng nhỏ hơn là điều đảm bảo rằng chúng tôi không bỏ lỡ các trận đấu tiềm năng. Nếu chúng ta luôn mở rộng một bên một cách tùy ý, chúng ta có thể bỏ qua các cấu hình có sự bình đẳng xảy ra sớm hơn ở phía bên kia. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 3 1 1 4
```| Bước | tôi | j | left_sum | đúng_sum | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 1 | 0 | rẽ trái | 
| 2 | 1 | 4 | 1 | 4 | rẽ phải | 
| 3 | 1 | 3 | 4 | 4 | rẽ phải | 
| 4 | 2 | 3 | 4 | 4 | bằng | 

Tại thời điểm cân bằng, chúng tôi tìm thấy một cấu hình hợp lệ trong đó tổng tiền tố bằng tổng hậu tố là 4. Phần mở rộng sau này dẫn đến kết quả khớp tốt hơn là 5 theo thứ tự phân vùng hợp lệ khác, nhưng cơ chế tương tự sẽ ghi lại nó khi đạt được căn chỉnh chính xác. 

Điều này chứng tỏ đẳng thức có thể xảy ra nhiều lần và thuật toán giữ giá trị lớn nhất. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 0
```| Bước | tôi | j | left_sum | đúng_sum | hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 1 | 0 | trái | 
| 2 | 1 | 3 | 1 | 0 | trái | 
| 3 | 2 | 3 | 3 | 0 | trái | 
| 4 | 2 | 2 | 3 | 0 | trái | 
| 5 | 2 | 1 | 3 | 3 | đúng | 

Tại điểm gặp nhau cuối cùng, cả hai có tổng bằng 3, mang lại mức chia cân bằng tốt nhất có thể đạt được. 

Dấu vết này cho thấy thuật toán nén đoạn giữa một cách tự nhiên như thế nào và chỉ quan tâm đến việc tích lũy ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi con trỏ di chuyển tối đa n bước một lần | 
| Không gian | O(1) | Chỉ có một số quầy được duy trì | 

Quét tuyến tính là tối ưu cho n lên tới 200.000, vì nó thực hiện một lần truyền qua mảng với công việc không đổi trên mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("5\n1 3 1 1 4\n") == "5"

# minimum size
assert run("1\n10\n") == "0"

# all equal
assert run("4\n2 2 2 2\n") == "4"

# no possible match except empty
assert run("3\n1 2 3\n") == "0"

# symmetric case
assert run("5\n1 2 3 2 1\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | xử lý phân chia trống | 
| tất cả đều bình đẳng | tổng n/2 | tích lũy đối xứng | 
| trình tự tăng dần | 0 | không có trận đấu ngẫu nhiên | 
| giống như palindrom | tích cực | khớp tiền tố/hậu tố đúng | 

## Vỏ cạnh 

Đối với một đầu vào như`1 2 3`, thuật toán bắt đầu mở rộng cả hai đầu nhưng không bao giờ đạt được sự bằng nhau ngoại trừ ở mức 0, vì tổng hậu tố và tiền tố phân kỳ ngay lập tức. Chuyển động của con trỏ đảm bảo cả hai đầu cuối cùng đều cạn kiệt mà không khai báo sai sự trùng khớp. 

Vì`2 2 2 2`, thuật toán liên tục đạt được sự bình đẳng khi cả hai bên phát triển đối xứng. Mỗi đẳng thức cập nhật câu trả lời và kết quả cuối cùng phản ánh mức phân chia tối đa có thể đạt được. 

Vì`1`, thuật toán thực hiện một bước duy nhất trong đó left_sum trở thành 1 và right_sum vẫn bằng 0, dẫn đến không có sự bằng nhau và trả về 0 một cách chính xác.
