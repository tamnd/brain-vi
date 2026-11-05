---
title: "CF 1195C - Bài tập bóng rổ"
description: "Chúng ta có hai dòng học sinh nằm ngang, mỗi dòng chứa số học sinh bằng nhau. Mỗi học sinh có một chiều cao và chúng tôi muốn xây dựng một đội bằng cách chọn học sinh từ hai hàng này theo quy tắc sắp xếp nghiêm ngặt: một khi chúng tôi bắt đầu chọn từ trái sang phải, chúng tôi không bao giờ có thể…"
date: "2026-06-12T00:13:43+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1195
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 574 (Div. 2)"
rating: 1400
weight: 1195
solve_time_s: 83
verified: true
draft: false
---

[CF 1195C - Bài tập bóng rổ](https://codeforces.com/problemset/problem/1195/C) 

**Đánh giá:** 1400 
**Thẻ:** dp 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai dòng học sinh nằm ngang, mỗi dòng chứa số học sinh bằng nhau. Mỗi học sinh có một chiều cao và chúng tôi muốn xây dựng một đội bằng cách chọn học sinh từ hai hàng này theo quy tắc sắp xếp nghiêm ngặt: một khi chúng tôi bắt đầu chọn từ trái sang phải, chúng tôi không bao giờ có thể lùi về chỉ mục và chúng tôi cũng bị cấm chọn hai học sinh liên tiếp từ cùng một hàng. 

Điều này tạo ra vấn đề về trình tự bị ràng buộc trên hai mảng song song. Ở mỗi bước, chúng tôi chọn một học sinh tiếp theo từ hàng trên cùng hoặc dưới cùng, nhưng chỉ khi chỉ số cột của họ lớn hơn chỉ số của học sinh được chọn cuối cùng và hàng thay thế từ lựa chọn trước đó. 

Mục tiêu là tối đa hóa tổng chiều cao của học sinh được chọn. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất kỳ lập trình động hàm mũ hoặc bậc hai nào trên các cặp vị trí. Bất kỳ giải pháp nào cố gắng xem xét tất cả các chuỗi hoặc tất cả các chuyển đổi rõ ràng giữa tất cả các cặp chỉ số sẽ quá chậm. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Một khó khăn nhỏ là chúng ta không bị buộc phải chọn các cột liền kề. Chúng tôi có thể bỏ qua hoàn toàn các cột, điều đó có nghĩa là ở mỗi bước về cơ bản chúng tôi đang chọn ứng viên tương lai tốt nhất trong số tất cả các vị trí hợp lệ ở hàng đối diện. Một lựa chọn tham lam ngây thơ như luôn chọn học sinh cao hơn trong số hai học sinh tiếp theo sẽ không thành công vì lựa chọn tối ưu cục bộ có thể chặn quyền truy cập vào chuỗi xen kẽ tốt hơn sau này. 

Một kịch bản thất bại điển hình là khi lựa chọn ngay lập tức nhỏ hơn một chút cho phép truy cập vào chuỗi tương lai lớn hơn nhiều ở hàng khác. Ví dụ: việc chọn một giá trị lớn sớm trong một hàng có thể buộc bạn phải nhập một chuỗi các giá trị nhỏ sau đó, trong khi chờ đợi một chút và chuyển đổi các hàng sớm hơn sẽ mang lại tổng số cao hơn. 

Cấu trúc này gợi ý rằng quyết định ở mỗi hàng phụ thuộc vào sự tiếp tục tốt nhất bắt đầu từ các chỉ số trong tương lai, đây là tín hiệu cổ điển cho việc lập trình động theo các vị trí. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi xen kẽ hợp lệ bắt đầu từ mọi lựa chọn đầu tiên có thể có. Từ mỗi ô bắt đầu, chúng tôi phân nhánh đến mọi ô hợp lệ tiếp theo ở hàng đối diện có chỉ số cao hơn, tích lũy tổng. Điều này khám phá một không gian trạng thái lớn theo cấp số nhân vì từ mỗi vị trí, chúng ta có thể chuyển sang nhiều vị trí trong tương lai ở hàng đối diện và mỗi vị trí đó đều dẫn đến sự phân nhánh hơn nữa. Ngay cả việc cắt tỉa cũng không ngăn được tình trạng bùng nổ trong trường hợp xấu nhất, vì các chuỗi hợp lệ vẫn có thể rất nhiều. 

Quan sát quan trọng là sau khi chúng tôi sửa lỗi, chúng tôi hiện đang ở cột$i$trong một hàng, các lựa chọn phù hợp duy nhất trong tương lai nằm ở hàng đối diện với các chỉ số lớn hơn$i$. Trong số tất cả những lựa chọn trong tương lai đó, chúng tôi chỉ quan tâm đến sự tiếp tục tốt nhất có thể bắt đầu từ chỉ số đó. Điều này gợi ý tính toán trước các giá trị hậu tố tối ưu. 

Chúng ta hãy xác định hai mảng DP:$dp_1[i]$là tổng tốt nhất chúng ta có thể nhận được nếu học sinh được chọn cuối cùng ở hàng 1, cột$i$. Tương tự,$dp_2[i]$được xác định cho hàng 2. 

Nếu chúng ta đang ở$dp_1[i]$, lựa chọn tiếp theo phải đến từ hàng 2 tại một chỉ số nào đó$j > i$. Sự tiếp nối tốt nhất từ$j$đã được mã hóa trong$dp_2[j]$. Vì vậy chúng tôi muốn:$$dp_1[i] = h_{1,i} + \max_{j > i} dp_2[j]$$và tương tự:$$dp_2[i] = h_{2,i} + \max_{j > i} dp_1[j]$$Cấu trúc này cho phép chúng ta tính DP từ phải sang trái trong khi vẫn duy trì hậu tố cực đại đang chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| DP tối ưu với hậu tố cực đại | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo hai mảng$dp_1$Và$dp_2$, ban đầu chứa đầy số không. Chúng thể hiện số tiền tốt nhất có thể đạt được bắt đầu từ mỗi vị trí khi vị trí đó được chọn làm lựa chọn cuối cùng trong hàng của nó. 
2. Đồng thời duy trì hai biến đang chạy`best1`Và`best2`, đại diện cho giá trị tối đa của$dp_1[j]$Và$dp_2[j]$cho tất cả các chỉ số$j > i$. Chúng mã hóa cực đại hậu tố cần thiết cho quá trình chuyển đổi. 
3. Duyệt các chỉ số từ phải sang trái, bắt đầu từ$i = n-1$xuống tới$0$. Thứ tự này đảm bảo rằng khi chúng ta tính toán trạng thái$i$, tất cả các trạng thái bên phải đều đã được biết. 
4. Tại mỗi chỉ số$i$, tính:$dp_1[i] = h_{1,i} + best2$Và$dp_2[i] = h_{2,i} + best1$. Điều này trực tiếp áp dụng quy tắc chuyển tiếp bằng cách sử dụng các phần tiếp theo tốt nhất đã được tính toán. 
5. Sau khi tính toán cả hai giá trị cho chỉ số$i$, cập nhật`best1`Và`best2`sử dụng$dp_1[i]$Và$dp_2[i]$. Điều này đảm bảo rằng các trạng thái trong tương lai (trái) có thể sử dụng thông tin hậu tố chính xác. 
6. Sau khi xử lý tất cả các chỉ số, câu trả lời là lớn nhất trong số tất cả$dp_1[i]$Và$dp_2[i]$, vì học sinh được chọn đầu tiên có thể bắt đầu ở bất cứ đâu. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào thực tế là mọi chuỗi xen kẽ hợp lệ đều có chỉ số được chọn đầu tiên được xác định rõ ràng và sau đó giảm dần trình tự “các lựa chọn hậu tố còn lại” theo sự phụ thuộc DP. Ở bất kỳ vị trí nào$i$, việc tiếp tục tốt nhất sau khi chuyển hàng chỉ phụ thuộc vào việc tiếp tục tối ưu bắt đầu hoàn toàn ở bên phải. Bằng cách xử lý từ phải sang trái, chúng tôi đảm bảo rằng mọi lựa chọn hậu tố đều đã được giải quyết một cách tối ưu, do đó mỗi trạng thái DP được tính toán với kiến ​​thức đầy đủ về tất cả các phần tiếp theo hợp lệ. Điều này đảm bảo rằng không có cải tiến nào trong tương lai có thể làm mất hiệu lực trạng thái được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    dp1 = [0] * n
    dp2 = [0] * n

    best1 = 0
    best2 = 0

    for i in range(n - 1, -1, -1):
        dp1[i] = a[i] + best2
        dp2[i] = b[i] + best1

        if dp1[i] > best1:
            best1 = dp1[i]
        if dp2[i] > best2:
            best2 = dp2[i]

    print(max(best1, best2))

if __name__ == "__main__":
    solve()
```Mã này triển khai trực tiếp hậu tố DP được mô tả trước đó. Các mảng`dp1`Và`dp2`lưu trữ các giá trị tối ưu bắt đầu từ mỗi chỉ mục, nhưng trong thực tế, chúng ta chỉ cần chúng để cập nhật hậu tố cực đại. Các biến`best1`Và`best2`nén bảng DP vào bộ nhớ O(1) trên mỗi hàng. 

Việc lặp ngược lại là cần thiết vì nó đảm bảo rằng khi chúng ta tính toán vị trí$i$, tất cả các tiểu bang$j > i$đã được đưa vào hậu tố cực đại. Câu trả lời cuối cùng lấy điểm xuất phát tốt nhất trên cả hai hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [9, 3, 5, 7, 3]
b = [5, 8, 1, 4, 5]
```Chúng tôi xử lý từ phải sang trái. 

| tôi | dp1[i] | dp2[i] | tốt nhất1 | tốt nhất2 | 
| --- | --- | --- | --- | --- | 
| 4 | 3 + 0 = 3 | 5 + 0 = 5 | 3 | 5 | 
| 3 | 7 + 5 = 12 | 4 + 3 = 7 | 12 | 7 | 
| 2 | 5 + 7 = 12 | 1 + 12 = 13 | 12 | 13 | 
| 1 | 3 + 13 = 16 | 8 + 12 = 20 | 16 | 20 | 
| 0 | 9 + 20 = 29 | 5 + 16 = 21 | 29 | 21 | 

Câu trả lời cuối cùng là 29. 

Dấu vết này cho thấy sự đóng góp của hậu tố được tích lũy như thế nào: mỗi vị trí được hưởng lợi từ sự tiếp tục tốt nhất ở hàng đối diện ngay bên phải của nó. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [1, 100, 1]
b = [1, 1, 100]
```| tôi | dp1[i] | dp2[i] | tốt nhất1 | tốt nhất2 | 
| --- | --- | --- | --- | --- | 
| 2 | 1 + 0 = 1 | 100 + 0 = 100 | 1 | 100 | 
| 1 | 100 + 100 = 200 | 1 + 1 = 2 | 200 | 100 | 
| 0 | 1 + 100 = 101 | 1 + 200 = 201 | 201 | 101 | 

Đáp án là 201 

Trường hợp này nêu bật lý do tại sao tham lam lại thất bại: chọn số 100 sớm ở hàng 1 cho phép truy cập vào số 100 sau ở hàng 2, tạo ra chuỗi xen kẽ cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần với các lần chuyển đổi theo thời gian không đổi | 
| Không gian | O(n) | Mảng DP được phân bổ trên mỗi hàng, mặc dù chúng có thể được nén thêm | 

Độ phức tạp tuyến tính là đủ cho$n \le 10^5$và mức sử dụng bộ nhớ vẫn nằm trong giới hạn vì chúng tôi chỉ lưu trữ một vài mảng có kích thước$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    dp1 = [0] * n
    dp2 = [0] * n
    best1 = best2 = 0

    for i in range(n - 1, -1, -1):
        dp1[i] = a[i] + best2
        dp2[i] = b[i] + best1
        best1 = max(best1, dp1[i])
        best2 = max(best2, dp2[i])

    return str(max(best1, best2))

# provided sample
assert run("5\n9 3 5 7 3\n5 8 1 4 5\n") == "29"

# minimum size
assert run("1\n10\n20\n") == "20"

# all equal
assert run("4\n5 5 5 5\n5 5 5 5\n") == "20"

# alternating dominance
assert run("3\n1 100 1\n1 1 100\n") == "201"

# increasing only top
assert run("3\n1 2 3\n3 2 1\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | lựa chọn đơn tối đa | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều bình đẳng | chuỗi xen kẽ đầy đủ | hành vi DP thống nhất | 
| đỉnh hỗn hợp | phụ thuộc tầm xa | chuyển tiếp hậu tố | 
| hàng không đối xứng | con đường tối ưu không tham lam | tính đúng đắn của sự lựa chọn DP | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi giải pháp tối ưu bắt đầu ở hàng thứ hai chứ không phải hàng đầu tiên. Thuật toán xử lý việc này một cách tự nhiên vì nó tính toán cả$dp_1[i]$Và$dp_2[i]$đối xứng và đạt mức tối đa toàn cầu ở cuối. Ví dụ: nếu hàng 2 có giá trị lớn ở vị trí 0 và hàng 1 yếu thì DP vẫn nắm bắt được giá trị đó vì$dp_2[0]$có thể trực tiếp đóng góp vào câu trả lời mà không cần phải chọn trước. 

Một trường hợp tinh tế khác là khi cần bỏ qua nhiều chỉ số để đạt được sự thay thế có giá trị cao. Vì các chuyển đổi chỉ phụ thuộc vào cực đại hậu tố nên việc bỏ qua được xử lý ngầm:`best1`Và`best2`tổng hợp khả năng tiếp tục tốt nhất có thể đạt được trên tất cả các vị trí trong tương lai, do đó DP không áp đặt tính liền kề và đánh giá chính xác các chuỗi tối ưu thưa thớt.
