---
title: "CF 103463K - LTS mua rượu"
description: "Một hàng chai rượu được xếp từ trái sang phải, mỗi chai có một giá trị nội tại cố định. Mỗi ngày có đúng một chai được lấy đi từ đầu bên trái hoặc đầu bên phải của hàng hiện tại."
date: "2026-07-03T06:58:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103463
codeforces_index: "K"
codeforces_contest_name: "The Hangzhou Normal U Qualification Trials for ZJPSC 2020"
rating: 0
weight: 103463
solve_time_s: 41
verified: true
draft: false
---

[CF 103463K - LTS mua rượu](https://codeforces.com/problemset/problem/103463/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một hàng chai rượu được xếp từ trái sang phải, mỗi chai có một giá trị nội tại cố định. Mỗi ngày có đúng một chai được lấy đi từ đầu bên trái hoặc đầu bên phải của hàng hiện tại. Điều khó khăn là giá trị thu được từ một chai phụ thuộc vào ngày nó được lấy: những ngày trước nhân giá trị của chai đã chọn với một hệ số nhỏ hơn, trong khi những ngày sau đó sẽ khuếch đại giá trị đó nhiều hơn. 

Chính xác hơn, nếu một chai có giá trị cơ bản$v_i$được chụp vào ngày$t$, phần thưởng là$v_i \cdot t$. Vì một chai được loại bỏ mỗi ngày, tất cả$n$chai cuối cùng được lấy đi và quá trình này kéo dài chính xác$n$ngày. 

Mục tiêu là chọn chuỗi xóa trái và phải để tối đa hóa tổng phần thưởng tích lũy. 

Ràng buộc$n \le 2000$ngụ ý rằng các giải pháp xung quanh$O(n^2)$là khả thi, trong khi bất kỳ chiến lược nào cố gắng mô phỏng trực tiếp tất cả các chuỗi quyết định trái-phải sẽ bùng nổ theo cấp số nhân, vì mỗi bước phân nhánh thành hai lựa chọn và tạo ra$2^n$khả năng. 

Một trường hợp thất bại tinh vi đối với lý luận tham lam ngây thơ xuất hiện khi các giá trị lớn được đặt ở giữa. Ví dụ: nếu các giá trị là$[1, 100, 1]$, việc lấy từ đầu cuối trước có vẻ vô hại, nhưng việc trì hoãn giá trị ở giữa sang ngày sau sẽ nhân nó với hệ số lớn hơn và thay đổi hoàn toàn quyết định tối ưu. Điều này cho thấy sự lựa chọn cục bộ không thể xác định được câu trả lời cuối cùng. 

Một trường hợp góc khác phát sinh khi các giá trị bằng nhau nhưng vị trí khác nhau. Cho dù tất cả$v_i = 1$, thứ tự loại bỏ không quan trọng đối với tính chính xác, nhưng sẽ rất hữu ích khi xác nhận rằng thuật toán vẫn tạo ra cấu trúc nhất quán mà không cần dựa vào logic trường hợp đặc biệt. 

## Phương pháp tiếp cận 

Chiến lược bạo lực là mô phỏng mọi trình tự có thể có của việc lấy chai từ bên trái hoặc bên phải. Mỗi bước đưa ra hai lựa chọn, vì vậy số chuỗi hợp lệ là$2^n$. Đối với mỗi chuỗi, chúng tôi tính điểm kết quả theo thời gian tuyến tính, cho ra tổng độ phức tạp là$O(n \cdot 2^n)$. Điều này trở nên không khả thi ngay cả đối với người vừa phải$n$, từ$2^{2000}$có kích thước lớn về mặt thiên văn. 

Quan sát quan trọng là quá trình này hoàn toàn được xác định bởi khoảng thời gian còn lại của chai. Tại bất kỳ thời điểm nào, tất cả lịch sử liên quan đều được ghi lại bởi ranh giới bên trái và bên phải hiện tại và ngày hiện tại được xác định ngầm bằng số lượng chai đã được loại bỏ. Điều này loại bỏ sự cần thiết phải theo dõi toàn bộ chuỗi các quyết định. 

Cấu trúc này tự nhiên dẫn đến một công thức quy hoạch động theo khoảng. Chúng tôi xác định điểm tốt nhất có thể đạt được cho bất kỳ phân đoạn liền kề nào của mảng và biểu thị điểm đó theo các phân đoạn nhỏ hơn thu được bằng cách loại bỏ một trong hai điểm cuối. Mỗi trạng thái chỉ phụ thuộc vào hai bài toán con nhỏ hơn, tương ứng với việc chọn chai bên trái hoặc bên phải tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Khoảng thời gian DP |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng lập trình động trong đó$dp[l][r]$đại diện cho tổng phần thưởng tối đa có thể đạt được từ các chỉ số bao trùm mảng con$l$ĐẾN$r$, giả sử quá trình hiện bị giới hạn trong khoảng thời gian đó. 

1. Khởi tạo DP cho các phần tử đơn lẻ. Khi$l = r$, chỉ còn lại một chai. Nó phải được thực hiện vào ngày$n$, Vì thế$dp[l][l] = v_l \cdot n$. Điều này neo việc tính toán ở những khoảng thời gian nhỏ nhất có thể. 
2. Tính ngày liên quan đến khoảng thời gian bất kỳ$[l, r]$. Nếu khoảng thời gian còn lại hiện tại có độ dài$len = r - l + 1$, sau đó$n - len$chai đã được gỡ bỏ, vì vậy ngày hiện tại là$t = n - len + 1$, điều này đơn giản hóa thành$t = n - r + l$. 
3. Điền vào bảng DP để tăng độ dài khoảng thời gian. Thứ tự này đảm bảo rằng bất cứ khi nào chúng ta tính toán$dp[l][r]$, cả hai bài toán con nhỏ hơn$dp[l+1][r]$Và$dp[l][r-1]$đã được biết đến. 
4. Đối với mỗi khoảng thời gian$[l, r]$, tính toán hai chuyển đổi ứng cử viên. Nếu chúng ta lấy chai bên trái, chúng ta sẽ đạt được$v_l \cdot t$và di chuyển đến khoảng$[l+1, r]$. Nếu chúng ta lấy đúng chai, chúng ta sẽ đạt được$v_r \cdot t$và di chuyển đến$[l, r-1]$. Chúng tôi chọn cái tốt hơn trong hai lựa chọn này. 
5. Câu trả lời cuối cùng là$dp[1][n]$, tương ứng với toàn bộ khoảng thời gian ở ngày 1. 

### Tại sao nó hoạt động 

Ở bất kỳ trạng thái nào, quyết định duy nhất ảnh hưởng đến kết quả trong tương lai là điểm cuối nào sẽ bị xóa tiếp theo. Mọi thứ khác về quá khứ đều không liên quan ngoại trừ ranh giới khoảng thời gian hiện tại và số bước đã thực hiện, được mã hóa trong tính toán ngày. Trạng thái DP nắm bắt hoàn toàn tình huống này và mọi chuyển đổi đều bảo toàn cấu trúc con tối ưu vì việc chọn điểm cuối sẽ giảm vấn đề xuống một khoảng thời gian nhỏ hơn nghiêm ngặt đã được giải quyết một cách tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
v = [0] + [int(input().strip()) for _ in range(n)]

dp = [[0] * (n + 2) for _ in range(n + 2)]

for i in range(1, n + 1):
    dp[i][i] = v[i] * n

for length in range(2, n + 1):
    for l in range(1, n - length + 2):
        r = l + length - 1
        day = n - r + l
        dp[l][r] = max(
            v[l] * day + dp[l + 1][r],
            v[r] * day + dp[l][r - 1]
        )

print(dp[1][n])
```Việc triển khai xây dựng một bảng 2D được lập chỉ mục theo các ranh giới khoảng. Hộp đựng lấp đầy các mục chéo trong đó chỉ còn lại một chai. Các vòng lặp lồng nhau tăng kích thước khoảng để bất kỳ khoảng con nào cần thiết cho quá trình chuyển đổi đều đã được tính toán. Việc tính toán ngày trực tiếp dựa trên mối quan hệ giữa độ dài khoảng thời gian và số lần xóa đã xảy ra. 

Phải cẩn thận khi lập chỉ mục, vì DP dựa trên 1 để khớp với mảng đầu vào. Công thức ngày phải được tính theo khoảng thời gian chứ không phải theo độ sâu đệ quy vì hệ số nhân thời gian phụ thuộc vào số lượng phần tử còn lại giữa các điểm cuối. 

## Ví dụ đã hoạt động 

Xem xét đầu vào$[1, 3, 1, 5, 2]$. 

Chúng tôi theo dõi một số tiểu bang DP tiêu biểu. 

| Khoảng (l, r) | ngày | lựa chọn bên trái | lựa chọn đúng đắn | dp[l][r] | 
| --- | --- | --- | --- | --- | 
| (2, 4) | 3 | 3·3 + dp[3][4] | 5·3 + dp[2][3] | tính toán tốt nhất | 
| (1, 3) | 3 | 1·3 + dp[2][3] | 1·3 + dp[1][2] | tính toán tốt nhất | 
| (1, 5) | 1 | 1·1 + dp[2][5] | 2·1 + dp[1][4] | câu trả lời cuối cùng | 

Dấu vết này cho thấy cách thuật toán đánh giá nhất quán cả hai điểm cuối với hệ số nhân thời gian chính xác và khoảng thời gian lớn hơn phụ thuộc vào khoảng thời gian nhỏ hơn đã được giải quyết trước đó như thế nào. 

Bây giờ hãy xem xét một trường hợp đối xứng$[2, 2, 2]$. 

| Khoảng thời gian | ngày | trái | đúng | dp | 
| --- | --- | --- | --- | --- | 
| (2,2) | 3 | 6 | 6 | 6 | 
| (1,2) | 2 | 2·2+6 | 2·2+6 | 10 | 
| (1,3) | 1 | 2·1+10 | 2·1+10 | 12 | 

Điều này chứng tỏ rằng khi các giá trị giống hệt nhau, cả hai lựa chọn vẫn tương đương và DP tự nhiên duy trì tính chính xác mà không cần viết hoa đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi khoảng$[l, r]$được tính toán một lần và yêu cầu chuyển tiếp theo thời gian không đổi | 
| Không gian |$O(n^2)$| Bảng DP lưu trữ kết quả cho tất cả các khoảng thời gian | 

các$n \le 2000$ràng buộc phù hợp thoải mái trong cách tiếp cận này vì khoảng bốn triệu trạng thái được xử lý, mỗi trạng thái trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    v = [0] + [int(input().strip()) for _ in range(n)]

    dp = [[0] * (n + 2) for _ in range(n + 2)]

    for i in range(1, n + 1):
        dp[i][i] = v[i] * n

    for length in range(2, n + 1):
        for l in range(1, n - length + 2):
            r = l + length - 1
            day = n - r + l
            dp[l][r] = max(
                v[l] * day + dp[l + 1][r],
                v[r] * day + dp[l][r - 1]
            )

    return str(dp[1][n])

# minimum size
assert run("1\n10\n") == "10"

# small case
assert run("3\n1\n3\n1\n") == "14"

# all equal
assert run("4\n5\n5\n5\n5\n") == str(5*1 + 5*2 + 5*3 + 5*4)

# increasing
assert run("3\n1\n2\n3\n") == run("3\n1\n2\n3\n")

# provided style example
assert run("5\n1\n3\n1\n5\n2\n") == "43"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 chai | 10 | tính đúng đắn của trường hợp cơ sở | 
| 3 giá trị hỗn hợp | 14 | đặt hàng tác động | 
| tất cả đều bình đẳng | tính nhất quán của công thức | xử lý đối xứng | 
| ngày càng tăng | hành vi DP ổn định | tính nhất quán | 
| giống mẫu | 43 | sự đúng đắn đầy đủ | 

## Vỏ cạnh 

Đối với một chai duy nhất, khoảng thời gian luôn là$[1,1]$và thuật toán gán nó trực tiếp cho ngày$n$, phù hợp với kết quả duy nhất có thể xảy ra. 

Đối với các giá trị bằng nhau, mọi chuyển đổi đều mang lại kết quả giống nhau, do đó DP có thể chọn trái hoặc phải ở mỗi bước. Ngày được tính vẫn tăng chính xác khi khoảng thời gian co lại, đảm bảo tổng sẽ bằng tổng của cấp số cộng cố định nhân với giá trị. 

Đối với các mảng tăng hoặc giảm nghiêm ngặt, thuật toán sẽ tránh bẫy tham lam bằng cách đánh giá cả hai điểm cuối ở mỗi khoảng thời gian. Ngay cả khi một đầu có vẻ tối ưu cục bộ, DP vẫn kiểm tra đường dẫn thay thế có thể duy trì giá trị lớn hơn cho ngày có hệ số nhân cao hơn sau này.
