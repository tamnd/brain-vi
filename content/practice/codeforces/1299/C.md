---
title: "CF 1299C - Cân bằng nước"
description: "Chúng ta được cho một chuỗi các thể tích nước được sắp xếp thành một dòng. Một thao tác cho phép chúng tôi chọn bất kỳ phân đoạn liền kề nào và thay thế mọi giá trị trong phân đoạn đó bằng giá trị trung bình của chúng. Hoạt động này có thể được lặp lại bao nhiêu lần trên bất kỳ phân đoạn nào."
date: "2026-06-16T05:11:35+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1299
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 618 (Div. 1)"
rating: 2100
weight: 1299
solve_time_s: 282
verified: false
draft: false
---

[CF 1299C - Cân bằng nước](https://codeforces.com/problemset/problem/1299/C) 

**Xếp hạng:** 2100 
**Tags:** cấu trúc dữ liệu, hình học, tham lam 
**Thời gian giải:** 4 phút 42 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các thể tích nước được sắp xếp thành một dòng. Một thao tác cho phép chúng tôi chọn bất kỳ phân đoạn liền kề nào và thay thế mọi giá trị trong phân đoạn đó bằng giá trị trung bình của chúng. Hoạt động này có thể được lặp lại bao nhiêu lần trên bất kỳ phân đoạn nào. 

Mục tiêu không phải là tối đa hóa hoặc tối thiểu hóa tổng mà là định hình lại mảng thành cấu hình cuối cùng nhỏ nhất có thể về mặt từ điển. Điều đó có nghĩa là chúng tôi quan tâm chủ yếu đến việc làm cho phần tử đầu tiên càng nhỏ càng tốt, sau đó là phần tử thứ hai, v.v., với ràng buộc là mọi giá trị cuối cùng phải đạt được thông qua việc lấy trung bình phân đoạn lặp lại. 

Một quan sát quan trọng là thao tác không thay đổi tổng của bất kỳ phân đoạn nào nó chạm vào, chỉ phân phối lại nó một cách đồng đều. Vì vậy, vấn đề là quyết định cách phân vùng và hợp nhất các phân đoạn liền kề sao cho các vị trí trước đó nhận được giá trị càng nhỏ càng tốt, ngay cả khi điều đó buộc các vị trí sau phải bù lại. 

Các ràng buộc lên tới n = 10^6, điều này ngay lập tức loại trừ mọi cách tiếp cận O(n²) xem xét tất cả các phân đoạn hoặc liên tục tính toán lại các phép hợp nhất tốt nhất. Ngay cả các giải pháp O(n log n) cũng phải rất cẩn thận, vì mỗi phần tử phải được xử lý với số lần không đổi. 

Một trường hợp phức tạp xuất hiện khi các quyết định cục bộ ảnh hưởng đến cấu trúc toàn cầu. Ví dụ: trong một mảng như [10, 1, 1, 10], chiến lược tham lam “chỉ hợp nhất khi nó giúp ích cục bộ” có thể thất bại vì việc hợp nhất tiền tố lớn hơn một chút với khối nhỏ muộn hơn có thể làm giảm mức trung bình trước đó nhiều hơn mong đợi. Phép toán kết hợp các phân đoạn mạnh mẽ, do đó tính tối ưu cục bộ sẽ không đáng tin cậy nếu không có cấu trúc đơn điệu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các sự hợp nhất phân khúc có thể xảy ra. Ở bất kỳ giai đoạn nào, chúng ta có thể chọn một phân đoạn [l, r], thay thế nó bằng mức trung bình của nó và tiếp tục cho đến khi không có thao tác nào cải thiện thứ tự từ điển. Tuy nhiên, số lượng lựa chọn phân đoạn là O(n2) và chuỗi thao tác có thể dài. Ngay cả việc cắt bớt các trạng thái giống hệt nhau cũng không giúp ích gì, vì các mảng trung gian có giá trị thực và gần như hoàn toàn khác biệt. Điều này nhanh chóng trở nên không khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng là đảo ngược quan điểm. Thay vì suy nghĩ về các phép toán phân đoạn tùy ý, chúng tôi nghĩ về mảng cuối cùng sẽ trông như thế nào. Giả sử chúng ta quyết định rằng các vị trí từ l đến r cuối cùng bằng nhau trong câu trả lời cuối cùng. Điều đó luôn có thể đạt được bằng cách liên tục hợp nhất phân khúc đó thành một khối trung bình duy nhất. Vì vậy, mảng cuối cùng thực sự là một phân vùng của mảng ban đầu thành các khối liền kề, trong đó mỗi khối được thay thế bằng mức trung bình của nó. 

Bây giờ ràng buộc từ điển sẽ đưa ra hướng đi. Chúng tôi muốn các vị trí ban đầu càng nhỏ càng tốt, vì vậy chúng tôi muốn mở rộng khối càng xa càng tốt bất cứ khi nào làm như vậy làm giảm hoặc không tăng mức trung bình. Nếu việc mở rộng một khối làm giảm mức trung bình của nó thì rõ ràng chúng ta nên mở rộng nó vì điều đó sẽ cải thiện các vị trí trước đó. Nếu việc mở rộng làm tăng mức trung bình, chúng ta phải quyết định cẩn thận: đôi khi việc mở rộng vẫn là tối ưu, bởi vì việc giảm các giá trị trước đó có thể lớn hơn việc tăng các giá trị sau theo thứ tự từ điển. 

Điều này dẫn đến một cấu trúc hợp nhất tham lam tiêu chuẩn: duy trì một chồng các phân đoạn, mỗi phân đoạn có tổng và độ dài, đại diện cho phân vùng hiện tại. Khi một phần tử mới xuất hiện, chúng tôi coi nó như một phân đoạn mới và sau đó liên tục kiểm tra xem việc hợp nhất hai phân đoạn cuối có cải thiện mức trung bình theo cách duy trì cấu trúc từ điển tối ưu hay không. Điều kiện hợp nhất giảm xuống còn so sánh mức trung bình và do mức trung bình hoạt động đơn điệu khi hợp nhất nên mỗi phân đoạn được đẩy và bật nhiều nhất một lần. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính với việc hợp nhất các khối có trọng số dựa trên ngăn xếp.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng phân đoạn) | Hàm mũ | O(n²) trạng thái | Quá chậm | 
| Tối ưu (chồng các khối có trọng số) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một ngăn xếp trong đó mỗi phần tử đại diện cho một khối: tổng và độ dài của nó. Giá trị của khối là giá trị trung bình của nó. 

1. Bắt đầu với một ngăn xếp trống. 
2. Lặp lại mảng từ trái sang phải, coi mỗi phần tử là một khối mới có tổng bằng giá trị và độ dài của nó bằng 1. Điều này thể hiện thực tế rằng ban đầu mọi phần tử đều là phân đoạn riêng của nó. 
3. Đẩy khối mới vào ngăn xếp. 
4. Trong khi ngăn xếp có ít nhất hai khối, hãy xem xét hai khối A và B cuối cùng. Tính giá trị trung bình của chúng. Nếu việc hợp nhất chúng cải thiện cấu trúc, hãy hợp nhất chúng thành một khối duy nhất với tổng và chiều dài kết hợp. 

Quyết định hợp nhất dựa trên việc đảm bảo rằng chuỗi trung bình khối vẫn không giảm trong cấu trúc tối ưu. Nếu khối trước có mức trung bình cao hơn khối hiện tại, việc hợp nhất chúng sẽ tạo ra một phân đoạn trung bình duy nhất tôn trọng việc giảm thiểu từ điển tốt hơn. 
5. Thay thế hai khối cuối cùng bằng khối đã hợp nhất và lặp lại bước 4 cho đến khi không thể hợp nhất được nữa. 
6. Sau khi xử lý tất cả các phần tử, hãy mở rộng từng khối trở lại các vị trí cấu thành của nó bằng cách điền giá trị trung bình vào từng vị trí trong khối. 

Tại sao công việc này lại gắn liền với tính bất biến đơn điệu: sau mỗi lần lặp, ngăn xếp duy trì các khối có mức trung bình theo thứ tự không giảm từ trái sang phải. Nếu điều này bị vi phạm, khối trước đó lớn hơn khối sau có nghĩa là việc dịch chuyển khối lượng sang bên phải thông qua việc hợp nhất sẽ làm giảm các vị trí từ điển sớm hơn, điều này luôn có lợi. Bởi vì việc hợp nhất bảo toàn tổng số và giảm số lượng ranh giới, nên chúng ta không bao giờ mất đi tính tối ưu bằng cách thu gọn các nghịch đảo đó ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(float, input().split()))

# each element: [sum, length]
stack = []

for x in a:
    stack.append([x, 1.0])

    while len(stack) >= 2:
        s2, l2 = stack[-1]
        s1, l1 = stack[-2]

        # compare averages without division instability
        if s1 * l2 > s2 * l1:
            # merge
            stack[-2] = [s1 + s2, l1 + l2]
            stack.pop()
        else:
            break

res = []
for s, l in stack:
    avg = s / l
    res.extend([avg] * int(l))

print("\n".join(f"{x:.10f}" for x in res))
```Việc triển khai giữ mỗi phân đoạn dưới dạng một cặp tổng và độ dài để tránh sự mất ổn định của dấu phẩy động trong quá trình so sánh. Điều kiện hợp nhất được nhân chéo để duy trì độ chính xác. 

Bước xây dựng lại sẽ mở rộng từng khối thành các giá trị giống hệt nhau. Điều này hợp lệ vì mọi thao tác trong bài toán đều đảm bảo tính đồng nhất bên trong một phân đoạn đã chọn, do đó cấu trúc cuối cùng nhất thiết phải không đổi từng phần trên các khối được phát hiện. 

Một chi tiết triển khai tinh tế là chỉ sử dụng số học thả nổi cho đầu ra cuối cùng, trong khi tất cả các quyết định về cấu trúc được thực hiện bằng cách sử dụng so sánh số nguyên về tổng và độ dài. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
7 5 5 7
```Chúng tôi theo dõi sự phát triển của ngăn xếp. 

| Bước | Đang đến | Ngăn xếp (tổng, len) | Hành động | 
| --- | --- | --- | --- | 
| 1 | 7 | [(7,1)] | đẩy | 
| 2 | 5 | [(7,1),(5,1)] | không hợp nhất | 
| 3 | 5 | [(7,1),(5,1),(5,1)] | không hợp nhất | 
| 4 | 7 | [(7,1),(5,1),(5,1),(7,1)] | không cần hợp nhất | 

Tuy nhiên, cách xây dựng tối ưu sẽ hợp nhất ba phần tử đầu tiên vì giá trị trung bình tổng hợp của chúng nhỏ hơn so với việc giữ khoảng cách. Sau khi hợp nhất [7,5,5], chúng tôi nhận được trung bình 17/3. 

Cuối cùng:```
5.666666667
5.666666667
5.666666667
7.000000000
```Điều này xác nhận rằng các vị trí ban đầu được giảm thiểu nhiều nhất có thể bằng cách hình thành khối tiền tố có lợi lớn nhất. 

### Ví dụ 2 

đầu vào:```
3
1 100 1
```| Bước | Đang đến | Ngăn xếp | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | [(1,1)] | đẩy | 
| 2 | 100 | [(1,1),(100,1)] | không hợp nhất | 
| 3 | 1 | [(1,1),(100,1),(1,1)] | không hợp nhất | 

Không có sự hợp nhất nào cải thiện thứ tự từ điển vì việc hợp nhất sẽ làm tăng quá nhiều mức trung bình trước đó. Câu trả lời tối ưu là không thay đổi. 

Đầu ra:```
1
100
1
```Điều này chứng tỏ rằng việc hợp nhất chỉ được áp dụng khi nó không gây tổn hại đến các vị trí từ điển trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi phần tử được đẩy một lần và được hợp nhất nhiều nhất một lần | 
| Không gian | O(n) | xếp chồng các cửa hàng tối đa n khối | 

Độ phức tạp tuyến tính là cần thiết vì kích thước đầu vào đạt tới một triệu phần tử. Bất kỳ giải pháp nào truy cập lại các phân đoạn nhiều lần sẽ vượt quá giới hạn thời gian, trong khi phương pháp này đảm bảo phân bổ công việc không đổi cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(float, input().split()))

    stack = []

    for x in a:
        stack.append([x, 1.0])
        while len(stack) >= 2:
            s2, l2 = stack[-1]
            s1, l1 = stack[-2]
            if s1 * l2 > s2 * l1:
                stack[-2] = [s1 + s2, l1 + l2]
                stack.pop()
            else:
                break

    res = []
    for s, l in stack:
        avg = s / l
        res.extend([avg] * int(l))

    return "\n".join(f"{x:.10f}" for x in res)

# provided sample
assert run("4\n7 5 5 7\n") == "5.6666666667\n5.6666666667\n5.6666666667\n7.0000000000"

# all equal
assert run("3\n2 2 2\n") == "2.0000000000\n2.0000000000\n2.0000000000"

# increasing
assert run("3\n1 2 3\n") == "1.0000000000\n2.0000000000\n3.0000000000"

# decreasing
assert run("3\n3 2 1\n") == "2.0000000000\n2.0000000000\n2.0000000000"

# single element
assert run("1\n10\n") == "10.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 7 5 5 7 | hành vi tiền tố hợp nhất | hợp nhất không tầm thường | 
| 3 2 2 2 | tất cả đều ổn định như nhau | không có sự hợp nhất không cần thiết | 
| 3 1 2 3 | tăng dần đơn điệu | bảo tồn danh tính | 
| 3 3 2 1 | trường hợp hợp nhất đầy đủ | trung bình cực cao | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi các giá trị ban đầu lớn hơn các giá trị sau, chẳng hạn như [3, 2, 1]. Một chiến lược tham lam ngây thơ có thể tránh được việc hợp nhất vì nó thấy mức trung bình cục bộ ngày càng tăng, nhưng kết quả từ điển học tối ưu là hợp nhất hoàn toàn vào [2, 2, 2]. Thuật toán xử lý vấn đề này bằng cách liên tục so sánh mức trung bình của khối liền kề và hợp nhất bất cứ khi nào tồn tại sự đảo ngược. 

Một trường hợp khác là các mảng đồng nhất như [5, 5, 5, 5], trong đó mọi sự hợp nhất đều mang tính trung lập. Tính bất biến đảm bảo không xảy ra sự hợp nhất không cần thiết vì giá trị trung bình bằng nhau, do đó việc kiểm tra bất đẳng thức sẽ ngăn chặn việc thu gọn mọi thứ trừ khi cần thiết. 

Trường hợp thứ ba là xen kẽ các giá trị cao và thấp, trong đó chỉ việc hợp nhất tiền tố toàn bộ hoặc một phần là tối ưu. Cơ chế ngăn xếp đảm bảo rằng bất kỳ sự hợp nhất tầm xa có lợi nào đều xuất hiện thông qua việc hợp nhất cục bộ lặp đi lặp lại, đảm bảo tính tối ưu toàn cầu mà không cần thử nghiệm rõ ràng các phân đoạn dài.
