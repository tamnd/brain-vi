---
title: "CF 105079G - Rắc lén"
description: "Chúng tôi được phát một hàng bánh nướng nhỏ, mỗi hàng bắt đầu bằng một số lần rắc. Theo thời gian, Alice thực hiện một chuỗi các hoạt động. Trong mỗi thao tác, cô ấy tăng số lượng rắc trên mỗi chiếc bánh cupcake lên một lượng cố định, lấy từ một mảng theo một thứ tự cố định."
date: "2026-06-27T22:49:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105079
codeforces_index: "G"
codeforces_contest_name: "UTPC x WiCS Contest 04-05-23 (UT Internal)"
rating: 0
weight: 105079
solve_time_s: 89
verified: false
draft: false
---

[CF 105079G - Rắc lén](https://codeforces.com/problemset/problem/105079/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một hàng bánh nướng nhỏ, mỗi hàng bắt đầu bằng một số lần rắc. Theo thời gian, Alice thực hiện một chuỗi các hoạt động. Trong mỗi thao tác, cô ấy tăng số lượng rắc trên mỗi chiếc bánh cupcake lên một lượng cố định, lấy từ một mảng theo một thứ tự cố định. Sau mỗi lần tăng toàn cầu như vậy, Bob ngay lập tức can thiệp: anh ấy nhìn vào chiếc bánh cupcake hiện có nhiều hạt rắc nhất, loại bỏ một nửa số hạt rắc (làm tròn xuống) và giữ nguyên phần còn lại. 

Quá trình lặp lại chính xác một lần cho mỗi lớp được thêm vào. Sau khi tất cả các lớp được áp dụng và tất cả các biện pháp can thiệp của Bob đã diễn ra, chúng tôi được hỏi về tổng số lần rắc còn lại trên tất cả các bánh nướng nhỏ. 

Khó khăn chính là cả hai hoạt động đều mang tính toàn cầu theo những cách khác nhau. Bản cập nhật của Alice chạm đến tất cả các yếu tố một cách đồng nhất, trong khi hành động của Bob phụ thuộc vào mức tối đa toàn cầu thay đổi theo thời gian một cách không hề tầm thường. 

Các ràng buộc đủ lớn nên bất kỳ phương pháp nào mô phỏng từng bước một cách đơn giản trên một cấu trúc lớn đều có thể sẽ thất bại. Với tối đa 50.000 chiếc bánh nướng nhỏ và 50.000 thao tác, một mô phỏng trực tiếp trong đó chúng tôi liên tục quét tất cả các chiếc bánh nướng nhỏ để tìm mức tối đa sau mỗi bước sẽ tiêu tốn khoảng 2,5 tỷ phép so sánh, tốc độ này quá chậm trong Python. 

Vấn đề tế nhị thứ hai là việc đặt hàng. Bob luôn hành động sau khi Alice bổ sung đầy đủ cho bước đó. Nếu chúng ta nhầm lẫn xen kẽ các phép cộng và phép chia một nửa hoặc quên thứ tự, chúng ta sẽ nhận được kết quả không chính xác ngay cả với dữ liệu đầu vào nhỏ. 

Trường hợp cạnh cuối cùng xuất hiện khi nhiều chiếc bánh cupcake có chung giá trị lớn nhất. Bob chỉ ảnh hưởng đến một trong số chúng, nhưng việc chọn bất kỳ mức tối đa tùy ý nào cũng được. Tuy nhiên, nếu chúng tôi không duy trì cấu trúc dữ liệu nhất quán, chúng tôi có thể vô tình chọn giá trị cực đại đã lỗi thời hoặc bỏ lỡ các bản cập nhật sau các đợt halving trước đó. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì số lần rắc hiện tại trong một mảng. Đối với mỗi lớp, trước tiên chúng tôi thêm giá trị lớp vào mỗi chiếc bánh cupcake, sau đó quét toàn bộ mảng để tìm giá trị tối đa, sau đó chia đôi. 

Điều này đúng vì nó phản ánh chính xác quá trình. Vấn đề là chi phí. Mỗi bước yêu cầu O(N) để cập nhật cộng với O(N) để tìm N lần lặp lại tối đa. Điều này mang lại O(N^2), đạt tới 2,5 tỷ thao tác ở giới hạn trên và không khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần phải có kiến ​​thức đầy đủ về tất cả các loại bánh nướng nhỏ nhiều lần. Sau mỗi phép cộng toàn cục, mỗi chiếc bánh cupcake đều tăng cùng một hằng số. Điều này có nghĩa là thứ tự tương đối của chúng theo kích thước không thay đổi chỉ do hoạt động của Alice. Chỉ hoạt động giảm một nửa của Bob mới thay đổi thứ tự tương đối và nó chỉ sửa đổi một phần tử duy nhất. 

Cấu trúc này đề xuất duy trì các bánh nướng nhỏ trong cấu trúc dữ liệu luôn cung cấp quyền truy cập vào mức tối đa hiện tại một cách hiệu quả đồng thời hỗ trợ cập nhật cho một phần tử duy nhất. Vùng heap tối đa là đủ nếu chúng ta lưu trữ các giá trị hiện tại một cách rõ ràng và cập nhật một cách lười biếng hoặc trực tiếp điều chỉnh phần tử bị ảnh hưởng và lắp lại nó. 

Chúng tôi cũng khai thác thực tế rằng phần bổ sung toàn cục của Alice có thể được coi là phần bù đang chạy. Thay vì cập nhật tất cả các giá trị mỗi lần, chúng tôi duy trì một biến tăng chung. Giá trị thực tế của mỗi chiếc bánh cupcake là giá trị cơ bản được lưu trữ cộng với phần bù này. Điều này chuyển đổi phép cộng toàn cục thành O(1). 

Bây giờ, mỗi bước sẽ trở thành: áp dụng mức tăng bù, trích xuất mức tối đa (có tính đến phần bù), giảm một nửa và chèn lại giá trị đã cập nhật. Điều này làm giảm mỗi thao tác xuống O(log N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(N) | Quá chậm | 
| Heap + Bù đắp lười biếng | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì số lượng bánh nướng nhỏ tối đa và phần bù tổng thể đại diện cho tổng số bánh bổ sung từ Alice. 

Mỗi mục nhập heap lưu trữ “giá trị cơ sở”, nghĩa là giá trị thực trừ đi phần bù hiện tại.

1. Khởi tạo vùng heap bằng cách sử dụng tất cả các giá trị cupcake ban đầu trừ đi độ lệch bằng 0. 

Cách biểu diễn này đảm bảo chúng ta có thể xây dựng lại các giá trị thực bất kỳ lúc nào bằng cách thêm phần bù. 
2. Khởi tạo một biến`offset = 0`. 
3. Đối với mỗi giá trị lớp`b[i]`, thêm nó vào`offset`. 

Điều này thể hiện Alice đang tăng mỗi chiếc bánh cupcake lên`b[i]`mà không chạm vào các yếu tố riêng lẻ. 
4. Trích xuất phần tử lớn nhất từ ​​heap. 

Vì heap lưu trữ các giá trị cơ bản nên giá trị này tương ứng với chiếc bánh cupcake có giá trị thực cao nhất. 
5. Chuyển giá trị cơ bản này thành giá trị thực bằng cách cộng`offset`. 
6. Áp dụng thao tác Bob: thay giá trị này bằng`floor(value / 2)`. 
7. Chuyển giá trị đã cập nhật về dạng cơ số bằng cách trừ đi`offset`. 
8. Đẩy giá trị cơ sở đã sửa đổi này trở lại vùng nhớ heap. 

Sau khi xử lý tất cả các lớp, giá trị thực của mỗi chiếc bánh cupcake là giá trị cơ bản được lưu trữ cộng với giá trị bù cuối cùng. Tổng hợp tất cả các phần tử heap và thêm`N * offset`mang lại câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một bất biến được duy trì: tại bất kỳ thời điểm nào, mỗi phần tử heap biểu thị giá trị cupcake thực sự trừ đi cùng một giá trị bù chung và thuộc tính heap phản ánh thứ tự của các giá trị thực vì giá trị bù trừ giống hệt nhau cho tất cả các phần tử. Vì hoạt động của Alice chỉ thay đổi phần bù chung này nên nó duy trì thứ tự tương đối. Hoạt động của Bob ảnh hưởng đến chính xác một phần tử và chúng tôi ngay lập tức chèn lại biểu mẫu đã cập nhật của nó, khôi phục tính chính xác của cấu trúc heap. Điều này đảm bảo mọi bước đều hoạt động ở mức tối đa thực sự tại thời điểm đó. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # max heap via negatives
    heap = []
    offset = 0

    for x in a:
        heapq.heappush(heap, -x)

    for add in b:
        offset += add

        # extract max (real value = -heap[0] + offset)
        x = -heapq.heappop(heap)
        real = x + offset

        # Bob halves it
        real //= 2

        # store back as base value
        heapq.heappush(heap, -(real - offset))

    # final sum
    total = 0
    for x in heap:
        total += (-x + offset)

    print(total)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc lưu trữ các giá trị trong vùng heap tối đa bằng cách sử dụng giá trị âm, điều này tránh việc triển khai vùng nhớ heap tùy chỉnh. Phần bù không bao giờ được áp dụng trực tiếp vào vùng heap, điều này ngăn cản việc cập nhật O(N) lặp đi lặp lại. 

Khi Bob loại bỏ một nửa giá trị tối đa, chúng tôi tạm thời xây dựng lại giá trị thực bằng cách sử dụng giá trị bù, áp dụng thao tác, sau đó lưu lại cơ sở đã điều chỉnh. 

Tổng cuối cùng sẽ thêm phần bù trở lại cho mọi phần tử đúng một lần, tránh tính toán lại nhiều lần. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ: 

đầu vào:```
N = 3
A = [4, 2, 10]
B = [3, 1, 2]
```Chúng tôi theo dõi đống (dưới dạng giá trị cơ sở) và phần bù. 

| Bước | Bù đắp | Đống (cơ sở) | Thực tế tối đa | Sau Bob | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 | [10, 2, 4] | - | - | 
| +3 | 3 | [10, 2, 4] | 13 | 6 | 
| | | [6-3=3 đã chèn] → [4, 2, 3] | | | 
| +1 | 4 | [4, 2, 3] | 8 | 4 | 
| | | [4-4=0 đã chèn] → [3, 2, 0] | | | 
| +2 | 6 | [3, 2, 0] | 9 | 4 | 
| | | [4-6=-2 đã chèn] → [2, 0, -2] | | | 

Tổng cuối cùng: 

Giá trị thực = heap + offset = [8, 6, 4], tổng = 18. 

Dấu vết này cho thấy cách phần bù phân tách rõ ràng các hiệu ứng toàn cục và cục bộ. 

Ví dụ thứ hai có giá trị bằng nhau: 

đầu vào:```
N = 2
A = [5, 5]
B = [10, 10]
```Sau bước đầu tiên, cả hai trở nên bằng nhau, Bob chọn một trong hai, giảm một nửa và cấu trúc vẫn nhất quán vì việc bẻ khóa không ảnh hưởng đến tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi bước trong số N bước thực hiện một heap pop và push | 
| Không gian | O(N) | Heap lưu trữ N phần tử | 

Điều này phù hợp thoải mái trong giới hạn 50.000 phần tử và thao tác, vì log N nhỏ và tổng số thao tác là khoảng một triệu thao tác heap. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    import heapq
    heap = []
    for x in a:
        heapq.heappush(heap, -x)

    offset = 0

    for add in b:
        offset += add
        x = -heapq.heappop(heap)
        real = x + offset
        real //= 2
        heapq.heappush(heap, -(real - offset))

    return str(sum(-x + offset for x in heap))

# sample-style test
assert run("3\n4 2 10\n3 1 2\n") == "18"

# minimum case
assert run("1\n5\n10\n") == "7"

# equal values
assert run("2\n5 5\n1 1\n") == "10"

# decreasing pattern
assert run("3\n9 6 3\n2 2 2\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 chiếc bánh nướng nhỏ | tính chính xác của bản cập nhật duy nhất | ranh giới tối thiểu | 
| giá trị bằng nhau | xử lý cà vạt ở mức tối đa | sự ổn định của sự lựa chọn đống | 
| giảm dần | hiệu ứng giảm một nửa lặp đi lặp lại | tính đúng đắn tích lũy | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả các bánh nướng nhỏ đều bằng nhau sau phép cộng của Alice. Ví dụ:```
3
5 5 5
1 1 1
```Sau phép cộng đầu tiên, tất cả trở thành 6. Bob chọn một và chia nó thành 3. Đống vẫn chứa các biểu diễn nhất quán vì chỉ có một phần tử thay đổi và phần bù tiếp tục thể hiện chính xác mức tăng trưởng chung. Mặc dù tồn tại nhiều cực đại hợp lệ, nhưng bất kỳ một lựa chọn nào cũng dẫn đến trạng thái hợp lệ vì bài toán không yêu cầu lựa chọn tất định. 

Một trường hợp khác là khi giảm một nửa sẽ giảm mức tối đa xuống dưới các phần tử khác. Ví dụ:```
2
1 100
0 0
```Sau bước đầu tiên, 100 giảm một nửa xuống còn 50, có thể vẫn ở mức tối đa nhưng sau vài bước, nó có thể giảm xuống dưới chiếc bánh nướng nhỏ còn lại. Vùng heap xử lý chính xác việc này vì sau khi chèn lại, thứ tự sẽ tự động được khôi phục và truy vấn tối đa tiếp theo phản ánh trạng thái được cập nhật.
