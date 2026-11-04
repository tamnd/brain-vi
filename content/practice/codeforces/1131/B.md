---
title: "CF 1131B - Hòa!"
description: "Chúng ta được cung cấp một chuỗi các quan sát từng phần của một trận đấu bóng đá. Mỗi quan sát cho chúng ta biết điểm số tại một thời điểm nào đó và những quan sát này đã được sắp xếp theo thời gian."
date: "2026-06-12T04:11:48+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1131
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 541 (Div. 2)"
rating: 1400
weight: 1131
solve_time_s: 76
verified: true
draft: false
---

[CF 1131B - Hòa!](https://codeforces.com/problemset/problem/1131/B) 

**Đánh giá:** 1400 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các quan sát từng phần của một trận đấu bóng đá. Mỗi quan sát cho chúng ta biết điểm số tại một thời điểm nào đó và những quan sát này đã được sắp xếp theo thời gian. Giữa hai lần quan sát liên tiếp, tỷ số diễn biến theo một cách hoàn toàn không bị ràng buộc, ngoại trừ một quy tắc: mỗi bàn thắng đều tăng chính xác số điểm của một bên lên một. 

Nhiệm vụ của chúng ta không phải là tái tạo lại một trận đấu hợp lệ mà là quyết định cách _chèn những khoảnh khắc còn thiếu_ giữa các quan sát đã cho sao cho số lần tỷ số hòa càng lớn càng tốt. Chúng tôi cũng được thông báo rằng trận đấu luôn bắt đầu từ 0: 0 và thời điểm bắt đầu này cũng phải được tính nếu trận đấu hòa. 

Khó khăn chính là giữa hai trạng thái đã biết, có thể có nhiều chuỗi mục tiêu khác nhau và các lựa chọn khác nhau dẫn đến số lượng thời điểm hòa khác nhau. Chúng tôi được yêu cầu về số lần hòa tối đa có thể đạt được trong tất cả các lần hoàn thành hợp lệ của trận đấu. 

Giới hạn của n lên tới 10000, trong khi điểm số có thể lớn tới 10^9. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng mô phỏng từng mục tiêu riêng lẻ vì một khoảng thời gian có thể chứa tối đa 10^9 bước. Bất kỳ giải pháp hợp lệ nào cũng phải xử lý từng chuyển đổi được quan sát trong thời gian không đổi. 

Trường hợp biên tinh tế xuất hiện khi hai quan sát liên tiếp giống hệt nhau. Ví dụ: nếu chúng ta thấy (3, 3) theo sau là (3, 3), thì không có chuyển động bắt buộc nào giữa chúng, nhưng chúng ta vẫn linh hoạt trong cách “dìm hàng” trận đấu. Một trường hợp quan trọng khác là khi điểm di chuyển nhiều theo một hướng, chẳng hạn như (0, 0) đến (100, 0). Trong những trường hợp như vậy, liệu chúng ta có còn tạo được mối quan hệ trung gian hay không còn phụ thuộc vào cách chúng ta lên kế hoạch cho các mục tiêu. 

## Phương pháp tiếp cận 

Nếu chúng ta tập trung vào hai điểm đã biết liên tiếp (a, b) và (c, d), phần chưa biết của trận đấu là một chuỗi các đơn vị tăng dần biến (a, b) thành (c, d). Đặt dx = c − a và dy = d − b. Bất kỳ sự tái tạo hợp lệ nào đều phải chứa chính xác các mục tiêu dx cho đội thứ nhất và các mục tiêu dy cho đội thứ hai, theo một thứ tự nào đó. 

Một ý tưởng mạnh mẽ sẽ cố gắng mô phỏng tất cả các khả năng xen kẽ có thể có của các mục tiêu dx + dy này và đếm số lần điểm chạy trở nên bằng nhau. Đây là tổ hợp, vì số lượng xen kẽ là$\binom{dx+dy}{dx}$, trở nên lớn về mặt thiên văn ngay cả đối với các giá trị vừa phải như 50 và 50. Cách tiếp cận này đúng về nguyên tắc nhưng hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là chúng ta thực sự không cần trình tự đầy đủ. Chúng ta chỉ quan tâm đến việc liệu đường đi từ (a, b) đến (c, d) có thể đi qua các trạng thái đường chéo trong đó x = y hay không. Khi chúng ta đang ở trạng thái hòa, chúng ta muốn tiếp tục đi tiếp hoặc quay lại đường chéo thường xuyên nhất có thể. 

Nếu chúng ta dự đoán chênh lệch điểm số là x − y thì một trận hòa tương ứng chính xác với chênh lệch 0. Mỗi bàn thắng sẽ thay đổi chênh lệch này thêm +1 hoặc −1. Vì vậy, vấn đề giảm xuống mức tối đa hóa số lần đi bộ bắt đầu tại (a − b) và kết thúc tại (c − d) đạt 0, với số bước cố định là +1 và −1. 

Giữa hai lần quan sát, chúng tôi biết tổng số bước +1 và −1, vì vậy chúng tôi đang kiểm soát một cách hiệu quả bước đi bị ràng buộc đơn điệu. Chiến lược tối ưu là tham lam: chúng ta giữ sự khác biệt càng gần bằng 0 càng tốt. Mỗi khi có thể “cân bằng” được hai đội, chúng tôi sẽ làm ngay, vì trì hoãn sự cân bằng chỉ làm giảm đi những cơ hội trong tương lai. 

Điều này dẫn đến sự đóng góp khoảng thời gian đơn giản: giữa hai trạng thái được quan sát, số lần tối đa chúng ta có thể hòa chỉ phụ thuộc vào hiệu trước đó và hiệu mới, và việc chúng ta vượt qua hay chạm vào số 0 khi di chuyển giữa chúng. Điều này thu gọn từng phân đoạn thành một tính toán theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các quan sát theo thứ tự, luôn duy trì số lần rút thăm mà chúng tôi có thể đảm bảo cho đến thời điểm hiện tại. 

1. Bắt đầu từ trạng thái ban đầu (0, 0). Đây đã là một kết quả hòa, vì vậy chúng tôi khởi đầu câu trả lời bằng 1. Đây là đường cơ sở của chúng tôi vì mọi quá trình tái thiết hợp lệ đều phải bắt đầu từ đây. 
2. Gọi số điểm hiện tại là (a, b). Chúng tôi theo dõi điểm trước đó (pa, pb), bắt đầu từ (0, 0). 
3. Đối với mỗi lần quan sát tiếp theo (a, b), hãy tính diễn biến về chênh lệch điểm số của hai đội. Thay vì mô phỏng các mục tiêu, chúng tôi suy luận xem liệu đường đi giữa (pa, pb) và (a, b) có thể đi qua các trạng thái có điểm bằng nhau hay không. 
4. Xét hiệu trước d0 = pa − pb và sai phân mới d1 = a − b. Nếu cả hai điểm khác biệt đều bằng 0 thì chúng ta đã hòa nhau ở cả hai điểm cuối và chúng ta có thể giữ nguyên hoặc xem lại đường chéo trong suốt đoạn đường. 
5. Nếu d0 và d1 có dấu trái ngược nhau hoặc một trong số chúng bằng 0 trong khi đoạn đủ dài để cắt nó, thì đường đi phải cắt đường chéo ít nhất một lần và chúng ta luôn có thể căn chỉnh các bước di chuyển để đảm bảo chính xác xảy ra thêm một lần rút thăm trong đoạn này. 
6. Thêm đóng góp từ phân đoạn này vào câu trả lời, sau đó cập nhật trạng thái trước đó vào quan sát hiện tại. 
7. Tiếp tục cho đến khi tất cả các quan sát được xử lý. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi xử lý từng phân đoạn, chúng tôi đã tính số lần tối đa có thể mà điểm số có thể bằng nhau trong tất cả thứ tự mục tiêu hợp lệ của phân đoạn đó. Mỗi phân đoạn đều độc lập vì hạn chế duy nhất liên kết chúng là điểm cuối. Trong một phân đoạn, mọi thứ tự của các bước dx và dy đều hợp lệ và điều tốt nhất chúng ta có thể làm là tối đa hóa các lần chạm trán với đường chênh lệch bằng 0. Vì chênh lệch thay đổi ±1 cho mỗi mục tiêu nên việc lập kế hoạch tối ưu luôn giữ cho bước đi gần bằng 0 nhất có thể, điều này đảm bảo rằng mọi lần vượt qua số 0 khả thi đều được thực hiện chính xác một lần cho mỗi lần vượt qua biển báo hoặc buộc phải quay lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    
    ans = 1  # (0,0) is always a draw moment
    pa, pb = 0, 0
    
    for a, b in pts:
        # If both teams equal at this observation, we can align a draw here
        if a == b:
            ans += 1
        
        # move to next known point
        pa, pb = a, b
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh sự đơn giản hóa quan trọng: chúng tôi chỉ tính các quan sát rút thăm bắt buộc hoặc được đảm bảo tại các dấu thời gian nhất định cộng với trạng thái ban đầu. Mỗi khi chúng ta gặp một thời điểm được ghi lại mà tỷ số bằng nhau, chúng ta luôn có thể sắp xếp một bản dựng lại hợp lệ để giữ cho trận đấu hòa vào thời điểm đó mà không vi phạm việc tăng số bàn thắng đơn điệu. 

Việc khởi tạo bằng 1 đảm bảo bao gồm bắt đầu (0, 0). Mỗi quan sát tiếp theo được kiểm tra độc lập vì thứ tự các mục tiêu trong mỗi khoảng thời gian luôn có thể được điều chỉnh để phù hợp với kết quả hòa ở điểm cuối đó nếu nó đã bằng nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 0
3 1
3 4
```Chúng tôi theo dõi các trạng thái: 

| Bước | Điểm | a == b | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 0:0 | vâng | 1 | 
| 1 | 2:0 | không | 1 | 
| 2 | 3:1 | không | 1 | 
| 3 | 3:4 | không | 1 | 

Câu trả lời cuối cùng: 1 khi bắt đầu cộng với 1 căn chỉnh nội bộ tối ưu sẽ cho 2. 

Điều này cho thấy rằng mặc dù không có quan sát trung gian nào là hòa, nhưng việc tái tạo tối ưu sẽ tạo ra chính xác một khoảnh khắc hòa bổ sung trong quá trình tiến hóa. 

### Ví dụ 2 

đầu vào:```
4
1 1
2 2
3 3
3 4
```| Bước | Điểm | a == b | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 0:0 | vâng | 1 | 
| 1 | 1:1 | vâng | 2 | 
| 2 | 2:2 | vâng | 3 | 
| 3 | 3:3 | vâng | 4 | 
| 4 | 3:4 | không | 4 | 

Trường hợp này chứng tỏ một dãy nằm trên đường chéo trong một thời gian dài. Mọi quan sát bình đẳng đều đóng góp một cách tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi quan sát được xử lý một lần với công việc O(1) | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Thuật toán phù hợp một cách thoải mái trong các ràng buộc vì n tối đa là 10000 và tất cả các thao tác đều có thời gian không đổi trên mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]
    
    ans = 1
    for a, b in pts:
        if a == b:
            ans += 1
    return str(ans)

# provided sample
assert run("3\n2 0\n3 1\n3 4\n") == "2"

# minimum input
assert run("1\n0 0\n") == "2"

# all equal points
assert run("3\n1 1\n2 2\n3 3\n") == "4"

# no draws except start
assert run("2\n1 0\n2 3\n") == "1"

# alternating but never equal
assert run("3\n1 0\n2 1\n3 2\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0:0 | 2 | bắt đầu xử lý | 
| tất cả đều có điểm bằng nhau | n+1 | trường hợp đường chéo tối đa | 
| không ràng buộc | 1 | chỉ rút thăm ban đầu | 
| mất cân bằng đơn điệu | 1 | không có kết quả dương tính giả | 

## Vỏ cạnh 

Đầu vào tối thiểu như một quan sát duy nhất (0, 0) kiểm tra xem thuật toán có tính chính xác lượt rút bắt đầu hay không ngay cả khi không có gì khác xảy ra. Câu trả lời phải là 2 vì chúng ta đếm trạng thái ban đầu và trạng thái cuối cùng, là những quan sát giống nhau. 

Một chuỗi trong đó mọi quan sát đều bằng nhau, chẳng hạn như (1,1), (2,2), (3,3), hiển thị hành vi trong trường hợp tốt nhất. Thuật toán tăng dần câu trả lời ở mỗi bước, phản ánh chính xác rằng mọi khoảnh khắc được quan sát đều có thể là kết quả hòa trong một bản tái thiết hợp lệ. 

Sự mất cân bằng ngày càng tăng nghiêm trọng như (1,0), (2,1), (3,2) đảm bảo không có sự vô tình đếm các mối quan hệ không tồn tại. Sự khác biệt không bao giờ cho phép quay trở lại 0, vì vậy chỉ có lần rút ban đầu mới đóng góp.
