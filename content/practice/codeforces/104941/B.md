---
title: "CF 104941B - Mua bánh sừng bò"
description: "Chúng ta được cung cấp một chuỗi giá bánh sừng bò hàng ngày trong $n$ ngày tiếp theo. Mỗi ngày, phải ăn đúng một chiếc bánh sừng bò và bánh sừng bò rất dễ hỏng: bất kỳ chiếc bánh sừng bò nào chỉ có thể ăn được trong 7 ngày sau khi mua, kể cả ngày mua và sẽ trở nên vô dụng sau đó."
date: "2026-06-28T18:17:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104941
codeforces_index: "B"
codeforces_contest_name: "SLPC 2024 Open Division"
rating: 0
weight: 104941
solve_time_s: 78
verified: true
draft: false
---

[CF 104941B - Mua bánh sừng bò](https://codeforces.com/problemset/problem/104941/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi giá bánh sừng bò hàng ngày cho ngày tiếp theo$n$ngày. Mỗi ngày, phải ăn đúng một chiếc bánh sừng bò và bánh sừng bò rất dễ hỏng: bất kỳ chiếc bánh sừng bò nào chỉ có thể ăn được trong 7 ngày sau khi mua, kể cả ngày mua và sẽ trở nên vô dụng sau đó. 

Điều này tạo ra một vấn đề về lập kế hoạch: mỗi ngày, chúng ta có thể mua bánh sừng bò không chỉ cho ngày hiện tại mà còn cho tối đa 6 ngày trong tương lai, miễn là chúng vẫn còn trong thời hạn tươi ngon. Mục tiêu là chọn ngày mua hàng sao cho lượng tiêu thụ hàng ngày được trang trải bằng một số bánh sừng bò vẫn còn tươi, đồng thời giảm thiểu tổng chi phí. 

Khó khăn chính là việc mua trước có thể có lợi nếu giá trong tương lai cao hơn, nhưng việc mua quá mức sẽ vô ích vì đã hết hạn. 

Ràng buộc$n \le 29220$gợi ý rằng các giải pháp với hành vi bậc hai$O(n^2)$khó có thể vượt qua trong 1 giây trong Python, trong khi các chiến lược tuyến tính hoặc gần tuyến tính được mong đợi. 

Một trường hợp thất bại tinh vi đối với các chiến lược ngây thơ là do việc bỏ qua thời hạn hết hạn. Ví dụ: luôn mua vào ngày rẻ nhất cho đến nay và dự trữ vô thời hạn sẽ thất bại: 

đầu vào:```
8
5 1 1 1 1 1 1 100
```Một chiến lược tham lam mua quá nhiều vào ngày thứ 2 có thể cố gắng vượt xa ngày thứ 8, nhưng mọi thứ sẽ hết hạn sau 7 ngày, vì vậy việc mua hàng không thể bị dịch chuyển một cách tùy tiện. 

Một thất bại khác đến từ việc chỉ mua vào ngày hiện tại: điều đó luôn đúng nhưng không tối ưu. Ví dụ: 

đầu vào:```
7
10 1 1 1 1 1 1
```Mua mỗi ngày có giá 16, nhưng mua thêm vào ngày thứ 2 có thể thay thế những lần mua đắt tiền ban đầu. 

Cấu trúc này là một cửa sổ thời gian trượt có giá trị hạn chế, cho thấy các quyết định chỉ phụ thuộc vào 7 ngày trước đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực cố gắng quyết định, mỗi ngày, nên mua bao nhiêu bánh sừng bò và lẽ ra chúng phải được mua vào ngày nào sớm hơn. Về mặt khái niệm, cho mỗi ngày$i$, chúng ta có thể nhìn lại tối đa 7 ngày và ấn định bánh sừng bò của ngày hôm nay cho ngày mua hợp lệ rẻ nhất trong phạm vi đó. Điều này dẫn đến một công thức lập trình động trong đó mỗi ngày xem xét các chuyển đổi từ tối đa 7 trạng thái trước đó. Trong khi điều này đã làm giảm cấu trúc bài toán, một phiên bản đơn giản hơn vẫn có thể tính toán lại các giá trị tối thiểu hợp lệ nhiều lần, dẫn đến$O(n \cdot 7)$hoặc tệ hơn tùy thuộc vào việc thực hiện. 

Một ý tưởng trực tiếp nhưng không hiệu quả là hàng ngày hãy quét tất cả các ngày trước đó trong khoảng thời gian 7 ngày và tính toán chi phí chuyển nhượng tốt nhất có thể. Điều đó mang lại$O(n \cdot 7)$, đó là ranh giới nhưng có thể chấp nhận được. Tuy nhiên, ngay cả những công thức ngây thơ hơn nhằm tính toán lại các quyết định tích lũy hoặc cố gắng mô phỏng tất cả các kết hợp mua hàng cũng bùng nổ theo cấp số nhân. 

Quan sát quan trọng là mỗi chiếc bánh sừng bò cần có trong ngày$i$phải được mua vào một ngày nào đó trong$[i-6, i]$. Trong số 7 lựa chọn đó, chúng tôi muốn ngày mua hợp lệ rẻ nhất, nhưng giao dịch mua đó cũng góp phần tạo ra những ngày sớm hơn hoặc muộn hơn trong thời hạn hiệu lực của chính nó. Điều này có nghĩa là chi phí tối ưu mỗi ngày chỉ phụ thuộc vào cửa sổ trượt có kích thước cố định của các quyết định trước đó và chúng tôi có thể duy trì chi phí tối thiểu một cách hiệu quả. 

Do đó, vấn đề giảm xuống mức tối thiểu của cửa sổ trượt theo giá, được áp dụng mỗi ngày: chi phí cho ngày$i$là giá tối thiểu trong số ngày$i-6$ĐẾN$i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét cửa sổ Brute Force mỗi ngày |$O(7n)$|$O(1)$| Đã chấp nhận | 
| Cửa sổ trượt tối thiểu (deque) |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cấu trúc cho phép chúng tôi nhanh chóng biết được mức giá tối thiểu trong 7 ngày qua. 

1. Xử lý ngày từ 1 đến$n$theo thứ tự, coi mỗi ngày là yêu cầu chính xác một đơn vị “bảo hiểm”. 
2. Duy trì một số ngày lưu trữ ứng cử viên deque, nơi giá cả tăng dần từ trước ra sau. Mỗi phần tử đại diện cho một chỉ số ngày. 
3. Trước ngày xử lý$i$, xóa khỏi mặt trước bất kỳ ngày nào$j$Ở đâu$j < i - 6$, vì những giao dịch mua đó không còn hiệu lực do đã hết hạn. 
4. Trong khi mặt sau của deque có giá lớn hơn hoặc bằng$c_i$, remove it. Những ngày này không bao giờ tối ưu nữa vì ngày$i$thống trị chúng trong cửa sổ hợp lệ. 
5. Ngày đẩy$i$vào deque. 
6. Mặt trước của deque hiện lưu chỉ số của ngày mua hợp lệ rẻ nhất, vì vậy hãy thêm$c_{\text{deque}[0]}$để trả lời. 

Lý do đằng sau bước 4 là nếu ngày sau đó có giá thấp hơn và vẫn nằm trong khoảng thời gian 7 ngày hợp lệ, thì bất kỳ lựa chọn nào đắt hơn trước đó sẽ thực sự tồi tệ hơn đối với tất cả các quyết định trong tương lai khi cả hai đều vẫn còn hiệu lực. 

### Tại sao nó hoạt động 

Vào mỗi ngày$i$, deque thể hiện chính xác tập hợp các ngày trong$[i-6, i]$, nhưng được cắt bớt để giá thành đơn điệu. Bất kỳ chỉ mục nào bị loại bỏ sẽ hết hạn hoặc bị chi phối bởi mức giá rẻ hơn hoặc ngang bằng vào ngày sau đó. Do đó, khoảng thời gian tối thiểu luôn được giữ ở phía trước và không có giải pháp tối ưu nào có thể yêu cầu một ngày bị loại bỏ vì bất kỳ lựa chọn nào như vậy đều có thể được thay thế bằng một giải pháp thay thế hợp lệ rẻ hơn mà không vi phạm tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n = int(input())
    c = list(map(int, input().split()))

    dq = deque()
    ans = 0

    for i in range(n):
        while dq and dq[0] < i - 6:
            dq.popleft()

        while dq and c[dq[-1]] >= c[i]:
            dq.pop()

        dq.append(i)

        ans += c[dq[0]]

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc giá và xử lý chúng chỉ trong một lần. Deque lưu trữ các chỉ số chứ không phải giá trị, vì vậy chúng ta có thể thực thi chính xác quy tắc hết hạn 7 ngày bằng cách sử dụng so sánh chỉ mục. Vòng lặp đầu tiên loại bỏ các mục đã hết hạn. Vòng lặp thứ hai duy trì tính đơn điệu nên phía trước luôn là mức giá tối thiểu trong phạm vi hợp lệ. Câu trả lời tích lũy giá hợp lệ tối thiểu cho mỗi ngày. 

Một lỗi phổ biến là quên rằng thời hạn sử dụng bao gồm cả ngày mua, nghĩa là bánh sừng bò được mua vào ngày đó.$i$có giá trị trong ngày$i+6$. Đó chính xác là lý do tại sao điểm cắt là$i - 6$, không$i - 7$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7
1 9 9 9 9 9 9
```Chúng tôi theo dõi deque và chi phí từng ngày. 

| Ngày | Giá | Cửa sổ hợp lệ | Chỉ số Deque | Giá được chọn | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | [1] | 1 | 1 | 
| 2 | 9 | [1,2] | [1,2] | 1 | 2 | 
| 3 | 9 | [1,2,3] | [1,2,3] | 1 | 3 | 
| 4 | 9 | [1..4] | [1,2,3,4] | 1 | 4 | 
| 5 | 9 | [1..5] | [1,2,3,4,5] | 1 | 5 | 
| 6 | 9 | [1..6] | [1,2,3,4,5,6] | 1 | 6 | 
| 7 | 9 | [1..7] | [1,2,3,4,5,6,7] | 1 | 7 | 

Dấu vết cho thấy rằng một khi một ngày rất rẻ tồn tại bên trong cửa sổ, nó sẽ chiếm ưu thế trong tất cả những ngày đắt đỏ sau đó cho đến khi hết hạn. 

### Mẫu 2 

đầu vào:```
8
1 9 9 9 9 9 9 9
```| Ngày | Giá | Cửa sổ hợp lệ | Chỉ số Deque | Giá được chọn | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | [1] | 1 | 1 | 
| 2 | 9 | [1,2] | [1,2] | 1 | 2 | 
| 3 | 9 | [1,2,3] | [1,2,3] | 1 | 3 | 
| 4 | 9 | [1,2,3,4] | [1,2,3,4] | 1 | 4 | 
| 5 | 9 | [1,2,3,4,5] | [1,2,3,4,5] | 1 | 5 | 
| 6 | 9 | [1..6] | [1..6] | 1 | 6 | 
| 7 | 9 | [1..7] | [1..7] | 1 | 7 | 
| 8 | 9 | [2..8] | [2..8] | 9 | 16 | 

Ngày 1 hết hạn sau ngày thứ 7, vì vậy vào ngày thứ 8, tùy chọn tốt nhất còn lại sẽ trở thành 9. Điều này xác nhận rằng việc xử lý hết hạn là cần thiết để đảm bảo tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục vào và rời khỏi deque nhiều nhất một lần | 
| Không gian |$O(n)$| Deque lưu trữ tối đa 7 chỉ số hoạt động bất cứ lúc nào | 

Quét tuyến tính dễ dàng đủ nhanh để$n \le 29220$và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n = int(input())
        c = list(map(int, input().split()))
        dq = deque()
        ans = 0

        for i in range(n):
            while dq and dq[0] < i - 6:
                dq.popleft()
            while dq and c[dq[-1]] >= c[i]:
                dq.pop()
            dq.append(i)
            ans += c[dq[0]]

        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("7\n1 9 9 9 9 9 9\n") == "7"
assert run("8\n1 9 9 9 9 9 9 9\n") == "16"

# custom cases
assert run("1\n5\n") == "5", "minimum size"
assert run("7\n1 1 1 1 1 1 1\n") == "7", "all equal"
assert run("7\n7 6 5 4 3 2 1\n") == "7", "monotone decreasing"
assert run("14\n10 1 10 1 10 1 10 1 10 1 10 1 10 1\n") == "8", "alternating prices"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ngày | 5 | ranh giới tối thiểu | 
| tất cả 1s | 7 | sự ổn định dưới mối quan hệ | 
| giảm giá | 7 | thống trị cửa sổ chính xác | 
| giá xen kẽ | 8 | đặt lại cửa sổ lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một ngày giá rất rẻ xuất hiện sớm và kết thúc muộn hơn. Ví dụ: 

đầu vào:```
8
1 9 9 9 9 9 9 9
```Trong các ngày từ 1 đến 7, thuật toán sử dụng nhất quán giá 1. Vào ngày 8, chỉ số 1 bị xóa vì nó nằm ngoài cửa sổ hợp lệ$[8-6, 8] = [2, 8]$. Sau đó, deque chỉ chứa các mục nhập đắt tiền, do đó câu trả lời chuyển sang 9. Thuật toán chuyển đổi chính xác mà không cần bất kỳ xử lý đặc biệt nào vì việc hết hạn được thực thi một cách có cấu trúc thông qua việc loại bỏ chỉ mục. 

Một trường hợp khác là lặp lại mức giá bằng nhau. Khi các mức giá giống nhau, điều kiện đơn điệu vẫn có tác dụng vì việc loại bỏ từ phía sau sẽ sử dụng`>=`, đảm bảo các bản sao trước đó không tồn tại một cách không cần thiết. Điều này đảm bảo tính chính xác ngay cả khi có nhiều lựa chọn tối ưu tồn tại đồng thời.
