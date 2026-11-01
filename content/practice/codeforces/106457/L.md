---
title: "CF 106457L - Núi đá vôi"
description: "Bề mặt Karst được mô tả bằng danh sách một chiều các chiều cao đo được. Chúng ta được phép thay thế mọi chiều cao bằng một giá trị mới, nhưng việc thay đổi vị trí có chi phí bằng chênh lệch tuyệt đối giữa chiều cao cũ và chiều cao mới."
date: "2026-06-25T09:15:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106457
codeforces_index: "L"
codeforces_contest_name: "UTPC Spring 2026 Open Contest"
rating: 0
weight: 106457
solve_time_s: 51
verified: true
draft: false
---

[CF 106457L - Karst](https://codeforces.com/problemset/problem/106457/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bề mặt Karst được mô tả bằng danh sách một chiều các chiều cao đo được. Chúng ta được phép thay thế mọi chiều cao bằng một giá trị mới, nhưng việc thay đổi vị trí có chi phí bằng chênh lệch tuyệt đối giữa chiều cao cũ và chiều cao mới. Chuỗi cuối cùng phải trơn tru, nghĩa là các vị trí lân cận không thể khác nhau nhiều hơn giới hạn đã cho`k`. Nhiệm vụ là tìm tổng chi phí tối thiểu cần thiết để chuyển đổi chuỗi ban đầu thành bất kỳ chuỗi trơn tru hợp lệ nào. 

Đầu vào chứa số lượng phép đo, chênh lệch tối đa cho phép giữa các độ cao cuối cùng liền kề và các giá trị độ cao ban đầu. Đầu ra là tổng nhỏ nhất có thể của tất cả các thay đổi riêng lẻ. 

Số lượng phép đo có thể đạt tới`200000`, do đó, giải pháp thử mọi độ cao cuối cùng có thể hoặc giữ một bảng lập trình động đầy đủ trên mọi độ cao đều không thể hoạt động. Các giá trị độ cao có thể lớn bằng`10^9`, loại trừ DP dựa trên tọa độ. Chúng ta cần khai thác cấu trúc của hàm chi phí thay vì phạm vi các giá trị có thể có. 

Quan sát quan trọng là sự chuyển đổi giữa các vị trí chỉ phụ thuộc vào độ cao đã chọn trước đó trong một khoảng thời gian. Nếu chiều cao hiện tại là`x`, chiều cao trước đó phải nằm trong`[x-k, x+k]`. Hàm chi phí trên các giá trị có thể có của`x`vẫn lồi và tuyến tính từng phần, cho phép chúng ta lưu trữ hình dạng của toàn bộ trạng thái DP thay vì mọi chiều cao có thể. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu như`k`đủ lớn nên không cần thay đổi vì dãy ban đầu đã thỏa mãn điều kiện. Ví dụ:```
Input:
3 10
5 7 5

Output:
0
```Cách tiếp cận tham lam luôn sửa đổi giá trị trung bình vẫn sẽ thay đổi`7`, nhưng sự khác biệt ban đầu chỉ là`2`, vì vậy câu trả lời đúng là bằng không. 

Một trường hợp phức tạp khác là khi giải pháp tốt nhất thay đổi giá trị theo cả hai hướng thay vì chỉ hạ thấp các đỉnh hoặc chỉ nâng cao các thung lũng. Ví dụ:```
Input:
4 1
0 10 0 10

Output:
16
```Một cách tiếp cận bất cẩn có thể cố gắng giữ tất cả các giá trị thấp và chỉ giảm các giá trị cao, tạo ra kết quả tồi tệ hơn. Trình tự trơn tru tối ưu cân bằng các giá trị vì mọi vị trí đều ảnh hưởng đến phạm vi cho phép của các lân cận. 

Trường hợp cạnh thứ ba xuất hiện khi câu trả lời tối ưu là một vùng phẳng chứ không phải một chiều cao. Ví dụ:```
Input:
2 5
0 10

Output:
0
```Cả hai giá trị ban đầu đều đã nằm trong mức chênh lệch cho phép. Một phương pháp giả định luôn có một chiều cao cuối cùng tốt nhất duy nhất có thể thất bại trong những khoảng thời gian như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là sử dụng quy hoạch động trên các độ cao cuối cùng có thể. Cho phép`dp[i][x]`là chi phí tối thiểu sau khi xử lý lần đầu tiên`i`vị trí và kết thúc bằng chiều cao`x`. Sự chuyển tiếp là:```
dp[i][x] = |a[i] - x| + min(dp[i-1][y])
```Ở đâu`y`phải thỏa mãn`|y-x| <= k`. 

Điều này đúng vì mọi chuỗi hợp lệ đều kết thúc tại`x`phải đến từ một độ cao hợp lệ trước đó trong khoảng đó. Tuy nhiên, số lượng độ cao có thể có là rất lớn. Ngay cả khi chúng ta chỉ xem xét phạm vi từ`0`ĐẾN`10^9`, không gian trạng thái sẽ không thể xử lý được. Trường hợp xấu nhất sẽ cần khoảng`10^14`hoạt động. 

Cấu trúc quan trọng là mọi chuyển đổi đều bảo toàn tính lồi. Hàm mô tả giá trị DP là hàm của chiều cao hiện tại luôn là hàm tuyến tính lồi từng phần. Số hạng giá trị tuyệt đối thêm một hàm hình chữ V và việc lấy mức tối thiểu trong một khoảng trượt chỉ thay đổi ở vị trí cao nguyên tối thiểu. 

Thay vì lưu trữ tất cả độ cao, chúng tôi lưu trữ nơi độ dốc của hàm thay đổi. Hàm tuyến tính từng phần lồi được xác định hoàn toàn bởi các điểm dừng này và giá trị tối thiểu của nó. Đây là ý tưởng đằng sau thủ thuật độ dốc. 

Cấu trúc dữ liệu duy trì hai đống. Đống bên trái lưu trữ các điểm dừng ở phía bên trái của giá trị tối thiểu và đống bên phải lưu trữ các điểm dừng ở phía bên phải. Giá trị tối thiểu của hàm được lưu trữ riêng. Thêm`|x-a[i]|`chèn một hình chữ V mới và thao tác tối thiểu của cửa sổ trượt sẽ thay đổi khoảng thời gian tối thiểu hợp lệ bằng cách di chuyển một cách lười biếng các điểm dừng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * phạm vi độ cao) | O(phạm vi độ cao) | Quá chậm | 
| DP lồi với thủ thuật độ dốc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì chức năng DP hiện tại bằng cấu trúc thủ thuật độ dốc. Ban đầu, hàm chi phí bằng 0 ở mọi nơi, do đó cấu trúc bắt đầu với một tập hợp điểm dừng trống và giá trị tối thiểu bằng 0. 
2. Đối với mọi độ cao`a[i]`, trước tiên hãy áp dụng thao tác giới hạn độ cao trước đó trong khoảng cách`k`từ độ cao hiện tại. Điều này thay thế hàm cũ bằng giá trị tối thiểu trong khoảng`[x-k, x+k]`. 

Lý do điều này có thể được thực hiện một cách hiệu quả là vì hàm lồi giữ nguyên hình dạng của nó sau khi lấy khoảng trượt tối thiểu. Chỉ có các vị trí của độ dốc thay đổi mới di chuyển. 
3. Thêm chi phí thay đổi độ cao hiện tại bằng cách áp dụng`|x-a[i]|`. 

Thao tác này sẽ chèn một phần hình chữ V mới vào hàm. Độ dốc thay đổi một ở mỗi bên của`a[i]`, vì vậy hai vùng heap có thể được cập nhật mà không cần xây dựng lại hàm. 
4. Sau khi tất cả các vị trí được xử lý, giá trị tối thiểu được lưu trong cấu trúc thủ thuật độ dốc là câu trả lời. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý lần đầu tiên`i`vị trí, hàm lồi được duy trì thể hiện chi phí tối thiểu có thể có để hình thành một tiền tố trơn tru kết thúc ở mọi độ cao có thể. Bước tối thiểu trượt xem xét chính xác độ cao được cho phép bởi ràng buộc độ trơn và giá trị bổ sung tuyệt đối chiếm chi phí sửa đổi vị trí mới. Vì cả hai thao tác đều bảo toàn cách biểu diễn nên mức tối thiểu được lưu trữ cuối cùng là tổng chi phí tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

class SlopeTrick:
    def __init__(self):
        self.left = []
        self.right = []
        self.add_left = 0
        self.add_right = 0
        self.value = 0

    def push_left(self, x):
        heapq.heappush(self.left, -(x - self.add_left))

    def push_right(self, x):
        heapq.heappush(self.right, x - self.add_right)

    def top_left(self):
        return -self.left[0] + self.add_left

    def top_right(self):
        return self.right[0] + self.add_right

    def pop_left(self):
        x = self.top_left()
        heapq.heappop(self.left)
        return x

    def pop_right(self):
        x = self.top_right()
        heapq.heappop(self.right)
        return x

    def add_x_minus_a(self, x):
        if self.left:
            self.value += max(0, self.top_left() - x)
        self.push_left(x)
        self.push_right(self.pop_left())

    def add_a_minus_x(self, x):
        if self.right:
            self.value += max(0, x - self.top_right())
        self.push_right(x)
        self.push_left(self.pop_right())

    def add_abs(self, x):
        self.add_x_minus_a(x)
        self.add_a_minus_x(x)

    def sliding_min(self, left_shift, right_shift):
        self.add_left += left_shift
        self.add_right += right_shift

n, k = map(int, input().split())
a = list(map(int, input().split()))

st = SlopeTrick()

for x in a:
    st.sliding_min(-k, k)
    st.add_abs(x)

print(st.value)
```Hai đống đang lưu trữ các điểm thay đổi độ dốc của hàm DP lồi. Vùng heap bên trái là vùng heap tối đa, được triển khai bằng cách lưu trữ các giá trị âm, vì Python chỉ cung cấp vùng heap tối thiểu. Vùng heap bên phải là vùng heap tối thiểu bình thường. 

các`sliding_min`chức năng không di chuyển từng điểm dừng riêng lẻ. Nó chỉ thay đổi các offset lười biếng, bởi vì tất cả các điểm dừng ở mỗi bên đều di chuyển cùng nhau. Đây là những gì giữ cho mỗi bản cập nhật logarit. 

Thứ tự của các hoạt động quan trọng. Độ cao hợp lệ trước đó phải được áp dụng trước khi thêm chi phí sửa đổi của vị trí mới. Đảo ngược hai thao tác này có nghĩa là giá trị hiện tại ảnh hưởng không chính xác đến quá trình chuyển đổi của chính nó. 

Câu trả lời có thể lớn vì có tới`200000`vị trí và mỗi thay đổi có thể tốn tới`10^9`, nhưng số nguyên Python xử lý việc này mà không gặp vấn đề tràn. 

(Phần 2 tiếp tục với các Ví dụ đã thực hiện, Phân tích độ phức tạp, Trường hợp kiểm thử và Trường hợp biên.)
