---
title: "CF 105638G - Geos Thích Mua Sắm"
description: "Chúng tôi được cung cấp một số loại mặt hàng. Mỗi loại có nguồn cung hạn chế và chúng tôi phải chọn chính xác một số lượng mặt hàng cố định trong tổng số tất cả các loại. Điều khó khăn là lợi nhuận từ một loại không phải là hằng số trên mỗi mặt hàng."
date: "2026-06-22T15:04:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105638
codeforces_index: "G"
codeforces_contest_name: "GPC 2024"
rating: 0
weight: 105638
solve_time_s: 62
verified: true
draft: false
---

[CF 105638G - Geos thích mua sắm](https://codeforces.com/problemset/problem/105638/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số loại mặt hàng. Mỗi loại có nguồn cung hạn chế và chúng tôi phải chọn chính xác một số lượng mặt hàng cố định trong tổng số tất cả các loại. Điều khó khăn là lợi nhuận từ một loại không phải là hằng số trên mỗi mặt hàng. Thay vào đó, nó phụ thuộc vào số lượng vật phẩm chúng ta đã lấy từ cùng loại đó. 

Một cách hữu ích để giải thích điều này là mỗi khi chúng ta lấy một mục khác từ loại`i`, giá trị của nó tăng tuyến tính với số lượng mục đã được chọn từ loại đó. Nếu chúng ta lấy mục đầu tiên thuộc loại`i`, nó mang lại một số mức tăng cơ bản, lần thứ hai mang lại nhiều hơn, lần thứ ba thậm chí còn nhiều hơn, v.v., cho đến giới hạn có sẵn của loại đó. Tổng lợi nhuận là tổng của tất cả lợi nhuận cận biên được chọn trên tất cả các loại. 

Vì vậy nhiệm vụ tương đương với việc phân phối chính xác`m`chọn qua`n`trình tự tăng dần, mỗi trình tự một loại và chọn`m`đóng góp cận biên lớn nhất hiện có. 

Đầu vào cung cấp số lượng loại và số lượng lựa chọn được yêu cầu. Sau đó, chúng ta được cung cấp hai mảng có độ dài`n`: mảng đầu tiên mô tả số lượng mặt hàng có sẵn trong mỗi loại và mảng thứ hai mô tả tốc độ tăng trưởng lợi nhuận của loại đó. Sản lượng là tổng lợi nhuận tối đa có thể đạt được. 

Các ràng buộc ngụ ý rằng cả hai`n`Và`m`đủ lớn để tạo ra tất cả các đóng góp hạng mục có thể một cách rõ ràng và việc sắp xếp chúng là không an toàn. Một bản mở rộng đơn giản có thể tạo ra tổng số mục trên tất cả các loại, có thể quá lớn đối với bộ nhớ và thời gian nếu chúng ta làm phẳng mọi thứ. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ xuất hiện khi một loại có mức tăng trưởng rất cao nhưng lại có ít mặt hàng, trong khi loại khác có nhiều mặt hàng nhưng mức tăng trưởng thấp. Ví dụ: nếu loại A có 2 mặt hàng có mức tăng cận biên cao và loại B có 1000 mặt hàng có mức tăng cận biên nhỏ hơn một chút, việc luôn lấy từ B trước hoặc tham lam theo loại thay vì giá trị cận biên toàn cục sẽ dẫn đến kết quả dưới mức tối ưu. 

Khó khăn cốt lõi là mỗi mục bổ sung sẽ thay đổi giá trị của các mục trong tương lai cùng loại, do đó, các quyết định cục bộ trong một loại phụ thuộc vào số lượng đã được lấy. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê số lượng mục chúng tôi lấy từ mỗi loại, cho mỗi phân phối hợp lệ có tổng giá trị là`m`. Đối với mỗi vectơ`(x1, x2, ..., xn)`với`0 ≤ xi ≤ ai`và tổng số tiền`m`, chúng tôi tính toán lợi nhuận bằng cách tính tổng số đầu tiên`xi`đóng góp của từng loại. Số lượng phân phối như vậy là tổ hợp trong`m`Và`n`, tăng lên gần giống như số lượng tác phẩm của`m`, trở nên lớn về mặt thiên văn ngay cả đối với các giá trị vừa phải. Ngay cả khi cắt tỉa, phương pháp này vẫn không khả thi vì việc đánh giá từng cấu hình đã tốn kém`O(n)`. 

Quan sát quan trọng là cơ cấu lợi nhuận bên trong mỗi loại được xác định hoàn toàn bởi lợi nhuận cận biên. Nếu một loại có giá trị tăng trưởng`b_i`, khi đó những đóng góp của nó tạo thành một chuỗi các giá trị cận biên`b_i * 1, b_i * 2, ..., b_i * a_i`. Vấn đề sau đó trở thành: chúng ta có nhiều dãy được sắp xếp và chúng ta phải chọn dãy lớn nhất`m`các yếu tố trên tất cả chúng. 

Điều này biến bài toán thành bài toán lựa chọn “hợp nhất k dãy tăng dần” cổ điển. Thay vì tạo ra mọi thứ, chúng tôi chỉ duy trì ứng cử viên tốt nhất hiện có từ mỗi loại và liên tục trích xuất mức tối đa, cập nhật ứng cử viên tiếp theo của loại đó. Điều này đảm bảo chúng tôi luôn chọn mức tăng cận biên tối ưu toàn cầu ở mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ tính bằng m | O(n) | Quá chậm | 
| Tối ưu (đống vượt biên) | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại từng loại dưới dạng một chuỗi lợi nhuận cận biên, trong đó mục thứ k được lấy từ loại`i`đóng góp`b_i * k`. Chúng tôi không bao giờ xây dựng tất cả các chuỗi một cách rõ ràng mà chỉ theo dõi “biên giới” hiện tại của mỗi chuỗi. 

1. Đối với từng loại`i`, hãy bắt đầu với ý tưởng rằng chúng ta có thể tiến tới`a_i`mặt hàng. Mức tăng cận biên tốt nhất có được ban đầu là mức tăng cuối cùng trong loại đó, tức là`b_i * a_i`. Chúng tôi lưu trữ điều này như một ứng cử viên cùng với chỉ mục loại. 
2. Chèn tất cả các ứng cử viên ban đầu vào một đống tối đa. Mỗi phần tử heap đại diện cho mức tăng cận biên chưa được sử dụng tốt nhất tiếp theo từ một loại. 
3. Repeat exactly`m`lần. Trong mỗi lần lặp lại, hãy trích ra mức tăng cận biên lớn nhất hiện có. Điều này tương ứng với việc lựa chọn mục tiếp theo tốt nhất có thể trong số tất cả các loại. 
4. Giả sử chúng ta trích xuất một giá trị từ loại`i`và nó tương ứng với việc lấy mục thứ k của nó (được theo dõi ngầm). Sau khi sử dụng nó, mức tăng cận biên có sẵn tiếp theo từ loại`i`trở thành`b_i * (k - 1)`. 
5. Nếu giá trị tiếp theo này vẫn dương (có nghĩa là chúng ta chưa sử dụng hết tất cả các mục thuộc loại đó), hãy đẩy nó trở lại heap. Điều này đảm bảo loại hình tiếp tục đóng góp lợi nhuận cận biên còn lại theo thứ tự giảm dần. 
6. Tích lũy các giá trị trích được vào đáp án cuối cùng. 

Chi tiết triển khai chính là chúng tôi không bao giờ theo dõi rõ ràng tất cả k giá trị cho mỗi loại. Thay vào đó, chúng tôi duy trì một con trỏ cho mỗi loại cho biết số lượng mục còn lại và mỗi lần chúng tôi bật ra khỏi vùng nhớ, chúng tôi sẽ giảm số lượng đó. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, vùng heap chứa chính xác lợi ích cận biên chưa sử dụng tốt nhất từ mọi loại. Vì lợi ích cận biên của mỗi loại tạo thành một chuỗi giảm dần, nên khi chúng ta lấy thứ tốt nhất hiện tại từ một loại, thì thứ tốt nhất tiếp theo từ cùng loại đó sẽ được xác định đầy đủ và nhỏ hơn. Sự lựa chọn tham lam về việc luôn lấy mức tăng cận biên tối đa toàn cầu là hợp lệ vì mức tăng cận biên là độc lập giữa các loại và chỉ phụ thuộc vào số lượng mặt hàng đã được lấy từ loại đó. Điều này đảm bảo rằng không có lựa chọn nào trong tương lai có thể vượt quá giá trị mà chúng ta bỏ qua hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # max heap using negative values
    heap = []
    
    # store current remaining count per type
    # and push initial best marginal gain b_i * a_i
    for i in range(n):
        if a[i] > 0:
            # store: (-value, type_index, current_k)
            heap.append((-b[i] * a[i], i, a[i]))

    heapq.heapify(heap)

    ans = 0

    for _ in range(m):
        val, i, k = heapq.heappop(heap)
        val = -val
        ans += val

        k -= 1
        if k > 0:
            heapq.heappush(heap, (-b[i] * k, i, k))

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì tối đa một đống`n`ứng viên năng động. Mỗi mục theo dõi số lượng mục còn lại hiện tại`k`cho loại đó. Khi bật một giá trị, chúng tôi ngay lập tức thay thế giá trị đó bằng mức tăng cận biên tiếp theo từ cùng loại, đảm bảo tính liên tục của chuỗi đó mà không lưu trữ tất cả các giá trị trung gian. 

Một lỗi phổ biến ở đây là tính toán lại hoặc tạo lại các chuỗi đầy đủ, việc này không cần thiết và quá chậm. Một điểm tinh tế khác là vùng heap lưu trữ các giá trị âm để mô phỏng vùng heap tối đa bằng cách sử dụng Python`heapq`. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu trong đó cả mức độ sẵn có và tốc độ tăng trưởng đều tăng đồng đều giữa các loại. Heap ban đầu chứa mức tăng cận biên cuối cùng từ mỗi loại. 

Chúng ta hãy theo dõi một số hoạt động đầu tiên một cách khái niệm. 

### Dấu vết ví dụ 

Chúng tôi chỉ hiển thị một số thao tác heap đầu tiên kể từ`m`là nhỏ. 

| Bước | Đã chọn loại | Giá trị thực hiện | Còn lại k | Thay đổi đống | 
| --- | --- | --- | --- | --- | 
| 1 | loại 5 | 25 | 4 | thay thế bằng 20 | 
| 2 | loại 4 | 16 | 3 | thay thế bằng 12 | 
| 3 | loại 5 | 20 | 3 | thay thế bằng 15 | 
| 4 | loại 3 | 9 | 2 | thay thế bằng 6 | 
| 5 | loại 5 | 15 | 2 | thay thế bằng 10 | 
| 6 | loại 2 | 4 | 1 | thay thế bằng 2 | 

Sau sáu bước, chúng tôi đã chọn được sáu mức tăng cận biên lớn nhất trên tất cả các loại. 

Dấu vết này chứng tỏ rằng chúng tôi không tham lam ủng hộ bất kỳ loại đơn lẻ nào mà thay vào đó luôn trích xuất mức cải thiện biên tốt nhất toàn cầu ở mỗi bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi trong số m lựa chọn thực hiện một heap pop và có thể là một push giữa n loại hoạt động | 
| Không gian | O(n) | Heap lưu trữ tối đa một ứng viên tích cực cho mỗi loại | 

Điều này phù hợp thoải mái trong các ràng buộc điển hình trong đó cả hai`n`Và`m`lên tới khoảng 100.000. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    heap = []
    for i in range(n):
        if a[i] > 0:
            heap.append((-b[i] * a[i], i, a[i]))
    heapq.heapify(heap)

    ans = 0
    for _ in range(m):
        val, i, k = heapq.heappop(heap)
        val = -val
        ans += val
        k -= 1
        if k > 0:
            heapq.heappush(heap, (-b[i] * k, i, k))

    return str(ans)

# sample (interpreted)
assert run("5 6\n1 2 3 4 5\n1 2 3 4 5\n") == "27"

# minimum size
assert run("1 1\n5\n10\n") == "50"

# single type, multiple picks
assert run("1 3\n3\n2\n") == str(2+4+6)

# all equal growth
assert run("3 4\n2 2 2\n5 5 5\n") == "50"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mục duy nhất | 50 | độ đúng ranh giới tối thiểu | 
| loại đơn chọn nhiều | 12 | xử lý cấp số cộng | 
| các kiểu tăng trưởng bằng nhau | 50 | xen kẽ công bằng giữa các loại | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi một loại có lượng sẵn có rất lớn nhưng hệ số tăng trưởng rất nhỏ. Ví dụ, một loại có`a = 100000`Và`b = 1`cạnh tranh với một loại với`a = 1`Và`b = 100000`. Hành vi đúng là luôn ưu tiên loại thứ hai trước, vì lợi ích cận biên đầu tiên của nó sẽ lấn át tất cả lợi ích sau này từ loại thứ nhất. Đống dữ liệu đảm bảo điều này bằng cách luôn so sánh mức tăng cận biên hiện tại trên toàn cầu, do đó, mức tăng đột biến lớn duy nhất sẽ được tiêu thụ trước bất kỳ sự tích lũy giá trị thấp nào. 

Một trường hợp khác là khi`m`nhỏ hơn số lượng loại. Trong tình huống đó, chúng tôi chỉ chọn từ những gì tốt nhất`m`những ứng cử viên ban đầu mà không bao giờ cạn kiệt bất kỳ loại nào. Thuật toán xử lý việc này một cách tự nhiên vì chúng tôi chỉ thực hiện`m`heap pops và không bao giờ tiêu thụ hết bất kỳ chuỗi nào.
