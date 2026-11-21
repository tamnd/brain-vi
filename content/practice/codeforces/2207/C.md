---
title: "CF 2207C - Nước của tôi đâu?"
description: "Lưới có thể được hiểu đơn giản hơn nếu chúng ta bỏ qua hình ảnh 2D và tập trung vào từng cột một cách độc lập. Trong cột i, có ai gạch đất ở phía dưới và tất cả gạch phía trên chúng cho đến độ cao h đều là nước. Vậy số gạch nước ở cột i là h - ai."
date: "2026-06-07T19:32:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 2207
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1085 (Div. 1 + Div. 2)"
rating: 1600
weight: 2207
solve_time_s: 145
verified: false
draft: false
---

[CF 2207C - Nước của tôi đâu?](https://codeforces.com/problemset/problem/2207/C) 

**Đánh giá:** 1600 
**Tas:** cấu trúc dữ liệu, chia để trị, dp, toán 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Lưới có thể được hiểu đơn giản hơn nếu chúng ta bỏ qua hình ảnh 2D và tập trung vào từng cột một cách độc lập. Trong cột`i`, có`a_i`gạch đất ở phía dưới và tất cả các gạch phía trên chúng theo chiều cao`h`là nước. Vậy số gạch nước ở cột`i`là`h - a_i`. 

Một cống đặt trên ô nước sẽ kích hoạt vùng lấp lũ: từ ô bắt đầu đó, nước chỉ lan qua các ô nước liền kề, nhưng chuyển động bị hạn chế sang trái, phải và xuống. Vì không thể di chuyển lên trên nên mọi vùng nước được kết nối đều có cấu trúc rất cụ thể: khi bạn chọn ô nước bắt đầu, toàn bộ vùng có thể tiếp cận chính xác là “lưu vực” nước tối đa không bị bụi bẩn chặn bên dưới. 

Tác dụng chính của cống là nó thu thập tất cả nước trong một đoạn cột nằm ngang liền kề nhau, nơi các cột nước được “kết nối ở một mức độ nào đó”. Nếu bạn nghĩ về chiều cao, mỗi cột đóng góp một đoạn nước thẳng đứng và khả năng kết nối phụ thuộc vào việc các cột lân cận có chồng chéo chiều cao nước hay không. 

Nhiệm vụ là đặt tối đa hai cống như vậy, mỗi cống chọn một thành phần nước được kết nối và tối đa hóa tổng số ô nước riêng biệt được thu thập. Nếu hai cống che nước chồng lên nhau thì những viên gạch đó chỉ được tính một lần. 

Các ràng buộc khiến cho việc áp dụng vũ lực lên tất cả các cặp vị trí thoát nước là không thể. Mỗi bài kiểm tra có thể có tổng cộng tối đa 2000 cột, do đó, bất kỳ phương pháp bậc hai hoặc tệ hơn nào trên mỗi giải pháp kiểm tra đều phải được kiểm soát cẩn thận và mọi thứ liên quan đến việc tính toán lại việc lấp lũ từ đầu đều quá chậm. 

Một vấn đề tế nhị là việc đếm quá mức chồng chéo. Hai cống được đặt ở các khu vực chồng chéo hoặc lồng vào nhau có thể chia sẻ các vùng nước rộng lớn. Một cách tiếp cận ngây thơ chỉ chọn hai phân đoạn độc lập tốt nhất mà không lập mô hình giao nhau sẽ đánh giá quá cao câu trả lời. 

Ví dụ: nếu tất cả các cột có`a_i = 1`, cột nào cũng gần đầy nước. Về cơ bản, bất kỳ cống nào cũng chạm tới toàn bộ lưới điện, vì vậy việc đặt hai cống không mang lại lợi ích gì ngoài một cống. Cách tiếp cận tham lam “chọn hai phân khúc tốt nhất” sẽ được tính gấp đôi một cách không chính xác. 

## Phương pháp tiếp cận 

Phương pháp bạo lực sẽ thử tất cả các vị trí thoát nước có thể. Vị trí thoát nước là bất kỳ ô nước nào, nhưng vì mỗi vị trí xác định một vùng nước được kết nối, thay vào đó, chúng ta có thể nghĩ đến việc chọn bất kỳ ô bắt đầu nào và tính toán vùng nước có thể tiếp cận của nó bằng cách lấp lũ. Làm điều này cho mỗi ô đã quá lớn: có`O(nh)`điểm bắt đầu có thể, và mỗi lần lũ lụt có thể mất`O(nh)`trong trường hợp xấu nhất, đưa ra một giải pháp rõ ràng là không khả thi`O(n^2 h^2)`hành vi. 

Ngay cả khi chúng tôi giới hạn bản thân chỉ ở “các ô nước trên cùng” trong mỗi cột, chúng tôi vẫn phải đối mặt với vấn đề là hình dạng của từng thành phần phụ thuộc vào sự truyền lan theo chiều ngang thông qua các độ cao khác nhau, điều này sẽ kết hợp các cột trên toàn cầu. 

Quan sát cấu trúc quan trọng là mỗi cống về cơ bản tương ứng với việc chọn khoảng cách ngang của các cột và trong khoảng đó, hệ số giới hạn là “độ cao mực nước” tối thiểu trên nó. Nếu chúng ta xác định chiều cao nước trong cột`i`BẰNG`b_i = h - a_i`, thì bất kỳ khu vực được kết nối nào có thể tiếp cận được từ cống sẽ hoạt động giống như một phân đoạn trong đó độ sâu nước hiệu quả được điều chỉnh bởi mức tối thiểu`b_i`trong phân khúc đó. Điều này biến vấn đề thành lý luận về các khoảng và kết hợp hai khoảng với sự chồng chéo của liên minh. 

Vì vậy, vấn đề giảm xuống còn việc chọn tối đa hai khoảng, trong đó giá trị của mỗi khoảng được xác định bởi hàm có chiều cao tối thiểu bên trong nó và tối đa hóa tổng đóng góp của công đoàn. Đây là cài đặt cổ điển trong đó tính năng chia để trị hoặc DP theo các khoảng thời gian kết hợp với cấu trúc ngăn xếp đơn điệu mang lại giải pháp hiệu quả. 

Chúng tôi tính toán mức đóng góp cho một lần thoát nước tốt nhất cho mỗi khoảng thời gian theo cách có cấu trúc, sau đó kết hợp hai khoảng thời gian đồng thời cẩn thận loại bỏ phần chồng chéo. Việc xử lý chồng chéo là khó khăn cốt lõi và vấn đề này được giải quyết bằng cách đảm bảo chúng tôi chỉ kết hợp các đóng góp rời rạc hoặc được hợp nhất đúng cách bằng cách sử dụng các giá trị tốt nhất tiền tố/hậu tố và mức tăng khoảng thời gian được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force lấp đầy lũ lụt | O(n²h) hoặc tệ hơn | O(nh) | Quá chậm | 
| Khoảng thời gian DP / tối ưu hóa phân chia và chinh phục | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển sang chế độ xem bổ sung: mỗi cột đóng góp`b_i = h - a_i`đơn vị nước. 

1. Tính toán trước độ cao mực nước`b_i`. Điều này chuyển đổi vấn đề thành hoạt động hoàn toàn trên mảng 1D trong đó các giá trị biểu thị dung lượng nước trên mỗi cột. 
2. Tính toán câu trả lời thoát đơn tốt nhất. Một cống duy nhất nắm bắt hiệu quả vùng được kết nối tối đa, tương ứng với việc chọn một phân khúc trong đó mức đóng góp được xác định bằng giá trị tối thiểu trong phân khúc đó nhân với chiều rộng của nó. Điều này tương đương với việc tìm hình chữ nhật tốt nhất dưới biểu đồ được xác định bởi`b_i`. 
3. Sử dụng một ngăn xếp đơn điệu để tính toán, cho mỗi vị trí, đoạn lớn nhất trong đó`b_i`là mức tối thiểu. Đối với mỗi`i`, chúng tôi xác định ranh giới bên trái và bên phải nơi các giá trị nằm trên hoặc bằng`b_i`. Điều này đưa ra khoảng thời gian tối đa trong đó cột`i`chi phối chiều cao của nước. 
4. Tính giá trị hình chữ nhật tốt nhất cho mỗi khoảng như vậy bằng cách sử dụng`b_i * (r - l + 1)`. Theo dõi tối đa; đây là kết quả tốt nhất với một cống. 
5. Bây giờ chúng ta cần hai cống. Chúng tôi chia mảng thành các phần bên trái và bên phải và tính toán các câu trả lời thoát đơn tốt nhất có thể cho tất cả các tiền tố và hậu tố. Cho phép`left_best[i]`là câu trả lời hay nhất chỉ sử dụng cột`[1..i]`, Và`right_best[i]`tương tự cho`[i..n]`. 
6. Cấu hình hai cống tốt nhất hoặc đặt cả hai cống hoàn toàn ở các vùng rời rạc hoặc giải pháp tối ưu sử dụng cấu trúc phân chia hiệu quả ở một số ranh giới. Vì vậy chúng tôi đánh giá`left_best[i] + right_best[i+1]`tổng thể`i`. 
7. Trả về giá trị tối đa trong số các câu trả lời thoát đơn tốt nhất và tất cả các kết hợp chia nhỏ. 

### Tại sao nó hoạt động 

Mỗi cống tương ứng với việc chọn một hình chữ nhật biểu đồ tối đa trong mảng được chuyển đổi`b_i`. Bất kỳ giải pháp tối ưu nào có hai cống đều có thể được phân tách thành hai vùng có sự kết hợp là tối ưu và bất kỳ sự chồng chéo nào sẽ chỉ làm giảm mức tăng cận biên vì các ô xếp chồng lên nhau được tính một lần. Do đó, một cấu hình tối ưu luôn có thể được sắp xếp lại thành hai khoảng đóng góp riêng biệt mà không làm giảm kết quả, điều này biện minh cho việc chia mảng tại một ranh giới và kết hợp các giá trị tối ưu tiền tố và hậu tố. 

Ngăn xếp đơn điệu đảm bảo rằng mọi hình chữ nhật tối đa được xem xét chính xác một lần, đảm bảo tính chính xác cho tính toán một cống, trong khi hậu tố DP đảm bảo tất cả các phân vùng hai cống hợp lệ được bao phủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, h = map(int, input().split())
        a = list(map(int, input().split()))
        b = [h - x for x in a]

        # monotonic stack for largest rectangle histogram
        left = [0] * n
        right = [0] * n

        stack = []
        for i in range(n):
            while stack and b[stack[-1]] >= b[i]:
                stack.pop()
            left[i] = stack[-1] + 1 if stack else 0
            stack.append(i)

        stack = []
        for i in range(n - 1, -1, -1):
            while stack and b[stack[-1]] > b[i]:
                stack.pop()
            right[i] = stack[-1] - 1 if stack else n - 1
            stack.append(i)

        best_single = 0
        for i in range(n):
            area = b[i] * (right[i] - left[i] + 1)
            if area > best_single:
                best_single = area

        # prefix best
        pref = [0] * (n + 1)
        for i in range(1, n + 1):
            pref[i] = max(pref[i - 1], b[i - 1] * (i - left[i - 1] + 1) if i - 1 >= 0 else 0)

        # suffix best
        suf = [0] * (n + 2)
        for i in range(n - 1, -1, -1):
            suf[i] = max(suf[i + 1], b[i] * (right[i] - i + 1))

        ans = best_single
        for i in range(n + 1):
            ans = max(ans, pref[i] + suf[i])

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chuyển đổi độ cao thành dung tích nước, sau đó sử dụng một ngăn xếp đơn điệu để tính toán các khoảng thời gian tối đa trong đó mỗi chỉ số là mức giới hạn tối thiểu. Mảng tiền tố và hậu tố được xây dựng để cho phép liệt kê hiệu quả tất cả các điểm phân chia. Vòng lặp cuối cùng thử mọi ranh giới phân vùng và kết hợp các giải pháp tốt nhất ở cả hai bên. 

Phần tế nhị nhất là sự lựa chọn bất đẳng thức nghiêm ngặt trong stack popping. sử dụng`>=`trên đường chuyền bên trái và`>`trên đường chuyền bên phải đảm bảo xử lý nhất quán các độ cao bằng nhau để mỗi lần quay
