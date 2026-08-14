---
title: "CF 102323I - Thỏa thích mua sắm"
description: "Chúng tôi có vài cuộc mua sắm thoải mái. Đối với mỗi lượt, các vật phẩm xuất hiện theo thứ tự cố định, có giá trị a 1 ​, a 2 ​,…,a s ​. Chúng tôi muốn chọn một tập hợp con của các mục này có tổng giá trị tối đa. Hạn chế là về mọi tiền tố của mảng."
date: "2026-08-13T04:19:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102323
codeforces_index: "I"
codeforces_contest_name: "UCF Locals 2014"
rating: 0
weight: 102323
solve_time_s: 48
verified: true
draft: false
---

[CF 102323I - Thỏa sức mua sắm](https://codeforces.com/problemset/problem/102323/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có vài cuộc mua sắm thoải mái. Đối với mỗi lượt, các vật phẩm xuất hiện theo thứ tự cố định, có giá trị a 1 ​, a 2 ​,…,a s ​. Chúng tôi muốn chọn một tập hợp con của các mục này có tổng giá trị tối đa. 

Hạn chế là về mọi tiền tố của mảng. Với mỗi tiền tố kết thúc ở vị trí k ≥2, chúng ta có thể chọn tối đa ⌊k/2⌋ mục từ tiền tố đó. Có một ngoại lệ đặc biệt ở vị trí 1: mục đầu tiên có thể được tự nó chọn, mặc dù ⌊1/2⌋=0. Tuyên bố ban đầu và dữ liệu mẫu xác nhận rằng đầu vào dự định bao gồm tối đa 100 dãy, mỗi dãy chứa tối đa 100.000 giá trị mục dương, với mỗi giá trị nhiều nhất là 10 6. 

Hậu quả chính của các ràng buộc là chúng ta không đủ khả năng để xem xét các tập hợp con một cách rõ ràng. Một nhóm với 100.000 mặt hàng có thể có 2 100000 tập hợp con, vượt xa mọi giới hạn thời gian thực tế. Chúng tôi cần một cách tiếp cận gần với O(slogs) hoặc tốt hơn cho mỗi lần chơi. Thực tế là tất cả các giá trị vật phẩm đều dương cũng có vấn đề, bởi vì bất cứ khi nào dung lượng tiền tố tăng lên, việc giữ lại một vật phẩm khác không bao giờ có thể ảnh hưởng đến mục tiêu. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai trực tiếp bị sai. Đối với đầu vào nhỏ nhất,`1 5`có câu trả lời`5`: quy tắc đặc biệt cho phép chọn mục đầu tiên. Triển khai chung bằng cách sử dụng`floor(k/2)`ở mọi vị trí sẽ loại bỏ nó một cách không chính xác. 

Vì`2 5 100`, câu trả lời là`100`. Ở tiền tố 2, chúng ta chỉ có thể chọn một mục, vì vậy việc chọn cả hai giá trị sẽ không hợp lệ. Việc triển khai chỉ đơn giản lấy mọi giá trị dương sẽ tạo ra 105. 

cho`3 1 100 99`, câu trả lời là`100`. Ở tiền tố 3, dung lượng vẫn chỉ là một, vì vậy tập hợp con hợp lệ tốt nhất chứa mục có giá trị 100. Việc triển khai bất cẩn có thể giữ lại mục đầu tiên chỉ vì quy tắc đặc biệt và không thể thay thế nó bằng mục sau có giá trị hơn. 

Vì`5 1 2 3 4 5`, câu trả lời là`9`, thu được bằng cách lấy giá trị 4 và 5. Ở tiền tố 5, dung lượng là ⌊5/2⌋=2. Đầu ra mẫu xác nhận kết quả này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi tập hợp con có thể, kiểm tra xem các vị trí được chọn của nó có đáp ứng mọi hạn chế tiền tố hay không và giữ tổng lớn nhất. Điều này đúng vì mọi giải pháp pháp lý đều được xem xét rõ ràng. Thật không may, với s mục có 2 tập con. Ở kích thước tối đa s=100000, điều này có nghĩa là 2 100000 ứng viên, điều này không khả thi chút nào. 

Chúng ta có thể làm tốt hơn nhiều bằng cách xử lý các mục từ trái sang phải. Sau khi xử lý vị trí k, chỉ có một thực tế về các mục được chọn quan trọng đối với các quyết định trong tương lai: trong số k mục đầu tiên, chúng ta phải giữ lại tối đa dung lượng tiền tố hiện tại. Nếu chúng ta tạm thời giữ quá nhiều đồ vật thì đồ vật ít giá trị nhất sẽ là đồ vật an toàn nhất để loại bỏ. Việc loại bỏ một món đồ có giá trị hơn chỉ có thể làm giảm tổng số tiền hơn nữa. 

Điều này đưa ra một mẫu tham lam tiêu chuẩn cho các ràng buộc về số lượng tiền tố. Chúng tôi đặt mọi giá trị gặp phải vào một đống tối thiểu. Bất cứ khi nào số lượng mục được giữ lại vượt quá số lượng được tiền tố hiện tại cho phép, chúng tôi sẽ xóa giá trị nhỏ nhất. Do đó, vùng heap chứa tập hợp con có giá trị tối đa thỏa mãn tất cả các dung lượng tiền tố được thấy cho đến nay. 

Quy tắc vị trí đầu tiên đặc biệt có thể được kết hợp trực tiếp bằng cách coi dung lượng ở vị trí 1 là 1, trong khi đối với mọi vị trí k sau đó, dung lượng là ⌊k/2⌋. Vì tất cả các giá trị đều dương nên mục đầu tiên thường được xem xét với dung lượng 1. Ở vị trí 2, dung lượng giảm xuống 1, do đó vùng heap tự động loại bỏ bất kỳ mục nào trong hai mục đầu tiên có giá trị nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2 giây s) | O (các) | Quá chậm | 
| Tối ưu | O(slog) | O (các) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị mục từ trái sang phải và duy trì một đống tối thiểu chứa các giá trị hiện được chọn. 
2. Đối với vị trí i, chèn i ​ vào heap. Ban đầu chúng tôi cho phép mục mới tham gia vì nó có thể thuộc tập hợp con tối ưu. 
3. Tính số lượng mục được chọn tối đa được phép trong tiền tố kết thúc tại i. Với i=1, dung lượng này là 1 vì ngoại lệ đặc biệt. Đối với i ≥2, nó là ⌊i/2⌋. 
4. Nếu heap chứa nhiều mục hơn dung lượng này, hãy loại bỏ giá trị nhỏ nhất của nó. Vì mỗi mục được chọn tiêu thụ chính xác một đơn vị dung lượng tiền tố và mọi giá trị đều dương nên mục được chọn nhỏ nhất là mục mà việc loại bỏ sẽ mất giá trị ít nhất có thể. 
5. Tiếp tục duyệt toàn bộ mảng. Sau vị trí cuối cùng, heap chứa các giá trị của một tập hợp con hợp lệ tối ưu, do đó tính tổng các phần tử của nó và in tổng đó. 

Bất biến tham lam là sau khi xử lý vị trí i, heap chứa một tập hợp con hợp lệ của mục i đầu tiên có tổng giá trị lớn nhất có thể có trong số tất cả các tập hợp con có kích thước được cho phép bởi mọi tiền tố cho đến i. Khi mục mới có kích thước quá lớn, mọi giải pháp hợp lệ phải loại bỏ ít nhất một mục hiện được chọn. Loại bỏ số nhỏ nhất sẽ được số tiền còn lại lớn nhất có thể. Vì các mục trong tương lai chỉ đưa ra các lựa chọn bổ sung và không bao giờ thay đổi giá trị của các mục đã được xử lý nên bất biến này vẫn hợp lệ ở vị trí tiếp theo. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        data = list(map(int, input().split()))
        s = data[0]
        a = data[1:]

        heap = []

        for i, value in enumerate(a, 1):
            heapq.heappush(heap, value)

            if i == 1:
                capacity = 1
            else:
                capacity = i // 2

            if len(heap) > capacity:
                heapq.heappop(heap)

        out.append(str(sum(heap)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Định dạng đầu vào đặt kích thước và tất cả các giá trị của một spree trên cùng một dòng, do đó, giải pháp sẽ đọc toàn bộ dòng và tách số đầu tiên khỏi các giá trị mục. Câu lệnh đảm bảo rằng tất cả các giá trị s đều tuân theo nó. 

Vùng heap là vùng heap tối thiểu vì chúng ta cần loại bỏ giá trị nhỏ nhất hiện được chọn bất cứ khi nào tiền tố vượt quá dung lượng. của Python`heapq`cung cấp chính xác hoạt động này. 

Dung lượng ở chỉ số 1 được xử lý riêng. Viết`i // 2`vô điều kiện sẽ làm cho dung lượng bằng 0 đối với mục đầu tiên, mâu thuẫn với quy tắc đặc biệt. Đối với mọi`i >= 2`, phép chia số nguyên cho kết quả chính xác là ⌊i/2⌋. 

Nhiều nhất một mục cần được loại bỏ ở mỗi vị trí. Dung lượng tăng bằng 0 hoặc một khi chỉ số tăng, vì vậy sau khi chèn một mục mới, vùng heap có thể vượt quá dung lượng tối đa là một. Đây là lý do tại sao một đơn`heappop`là đủ. 

Số nguyên Python có độ chính xác tùy ý, do đó tổng giá trị không thể tràn ngay cả khi tổng có thể lớn hơn nhiều so với giá trị của một mục riêng lẻ. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`5 1 2 3 4 5`, và câu trả lời mong đợi là`9`. Heap chứa các giá trị mục hiện được giữ lại. 

| Vị trí | Giá trị được chèn | Công suất | Đống sau khi sửa | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | [1] | 1 | 
| 2 | 2 | 1 | [2] | 2 | 
| 3 | 3 | 1 | [3] | 3 | 
| 4 | 4 | 2 | [3, 4] | 7 | 
| 5 | 5 | 2 | [4, 5] | 9 | 

Tại vị trí 2, giá trị 1 bị loại bỏ vì chỉ có thể chọn một mục từ hai vị trí đầu tiên. Ở vị trí 3, chỉ có thể chọn một mục, do đó 2 được thay thế bằng 3. Ở vị trí 4, dung lượng trở thành 2, cho phép 3 và 4, và ở vị trí 5, phần nhỏ hơn của 3 và 5 bị loại bỏ. Câu trả lời cuối cùng là 9, phù hợp với mẫu chính thức. 

Đối với Mẫu 2, đầu vào là`3 12 2 4`, và câu trả lời mong đợi là`12`. 

| Vị trí | Giá trị được chèn | Công suất | Đống sau khi sửa | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| 1 | 12 | 1 | [12] | 12 | 
| 2 | 2 | 1 | [12] | 12 | 
| 3 | 4 | 1 | [12] | 12 | 

Mục đầu tiên đã có giá trị lớn nhất. Khi vị trí 2 và 3 đến, heap tạm thời chứa hai mục, nhưng dung lượng vẫn là một, do đó mỗi mục nhỏ hơn sẽ bị loại bỏ ngay lập tức. Giá trị cuối cùng là 12, khớp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(slog) | Mỗi mục được chèn một lần và có thể được xóa khỏi heap một lần. | 
| Không gian | O (các) | Heap lưu trữ tối đa s giá trị mục. | 

Với s≤100000 mỗi spree, O(slogs) chỉ yêu cầu số logarit của các thao tác heap cho mỗi mục. Nguồn ban đầu cho phép tối đa 100 lượt, do đó tổng thời gian chạy tỷ lệ với tổng số mục được cung cấp thay vì nhân công việc với số lượng tập hợp con có thể có. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng các mẫu chính thức và một số trường hợp nhỏ được thiết kế để thực hiện ngoại lệ ở vị trí đầu tiên, thay thế một mục được chọn ban đầu, tăng công suất ở các vị trí chẵn và đầu vào có hình ranh giới lớn hơn.```python
import sys
import io
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        data = list(map(int, input().split()))
        s = data[0]
        a = data[1:]

        heap = []

        for i, value in enumerate(a, 1):
            heapq.heappush(heap, value)

            capacity = 1 if i == 1 else i // 2

            if len(heap) > capacity:
                heapq.heappop(heap)

        out.append(str(sum(heap)))

    return "\n".join(out)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve() + "\n"
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run(
    "2\n"
    "5 1 2 3 4 5\n"
    "3 12 2 4\n"
) == "9\n12\n", "provided samples"

# Minimum-size input, exercising the special rule for item 1.
assert run("1\n1 7\n") == "7\n", "single item"

# Two items: exactly one can be selected.
assert run("1\n2 5 100\n") == "100\n", "capacity at position 2"

# Three items: capacity is still one, so the largest value wins.
assert run("1\n3 1 100 99\n") == "100\n", "capacity at position 3"

# Four items: capacity becomes two.
assert run("1\n4 1 2 100 99\n") == "199\n", "capacity growth at position 4"

# All equal values, with capacity floor(6 / 2) = 3.
assert run("1\n6 8 8 8 8 8 8\n") == "24\n", "all equal values"

# Larger boundary-shaped case, 100000 items, all equal.
values = " ".join(["1"] * 100000)
assert run(f"1\n100000 {values}\n") == "50000\n", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 7`|`7`| Ngoại lệ vị trí đầu tiên đặc biệt | 
|`2 5 100`|`100`| Chỉ một vật phẩm có thể tồn tại ở hai vị trí đầu tiên | 
|`3 1 100 99`|`100`| Thay thế đúng mục đã chọn nhỏ hơn | 
|`4 1 2 100 99`|`199`| Công suất tăng từ 1 lên 2 | 
|`6 8 8 8 8 8 8`|`24`| Giá trị bằng nhau và nửa công suất chính xác | 
| 100.000 cái |`50000`| Kích thước đầu vào tối đa và dung lượng biên | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là spree một mục`1 7`. Thuật toán chèn 7 và đặt dung lượng thành 1 vì vị trí 1 có ngoại lệ đặc biệt. Đống vẫn còn`[7]`, do đó kết quả là 7. Một công thức sử dụng`i // 2`không có trường hợp đặc biệt sẽ tạo ra một lựa chọn trống không chính xác. 

Vì`2 5 100`, vị trí 1 chèn 5 với dung lượng 1. Vị trí 2 chèn 100, làm cho heap chứa hai giá trị trong khi dung lượng vẫn là 1. Giá trị tối thiểu 5 bị xóa, để lại 100. Đầu ra là 100. Điều này chứng tỏ tại sao heap phải loại bỏ giá trị nhỏ nhất thay vì chỉ loại bỏ mục mới đến. 

Vì`3 1 100 99`, vị trí đầu tiên rời đi`[1]`. Tại vị trí 2, heap trở thành`[1,100]`, nhưng dung lượng 1 buộc phải loại bỏ 1. Tại vị trí 3, việc chèn 99 lại vượt quá dung lượng, do đó 99 bị loại bỏ và 100 còn lại. Đầu ra là 100. Quyết định tham lam là tối ưu cục bộ vì ràng buộc tiền tố chỉ cho phép một mục được chọn, do đó mục có giá trị tối đa nhất thiết phải là lựa chọn tốt nhất. 

Vì`5 1 2 3 4 5`, dung lượng là 1, 1, 1, 2, 2. Đống phát triển từ`[1]`ĐẾN`[2]`, sau đó`[3]`, sau đó`[3,4]`, và cuối cùng`[4,5]`. Tổng kết quả là 9. Trường hợp này thực hiện quá trình chuyển đổi từ dung lượng một sang dung lượng hai và chứng minh tại sao tập hợp con tối ưu không cần chứa mục đầu tiên mặc dù nó được xử lý đặc biệt.
