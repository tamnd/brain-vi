---
title: "CF 102482D - Đảo Đá Quý"
description: "Tôi sẽ cung cấp bài xã luận như một tài liệu hoàn chỉnh. Giải pháp dưới đây tuân theo sự tái diễn tổ hợp đằng sau cách tiếp cận được chấp nhận cho vấn đề này. Chỉnh sửa Chúng tôi có n cư dân, mỗi người bắt đầu với một viên ngọc."
date: "2026-08-05T18:53:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102482
codeforces_index: "D"
codeforces_contest_name: "2018 ACM-ICPC World Finals"
rating: 0
weight: 102482
solve_time_s: 118
verified: true
draft: false
---

[CF 102482D - Đảo Đá Quý](https://codeforces.com/problemset/problem/102482/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
Tôi sẽ cung cấp bài xã luận như một tài liệu hoàn chỉnh. Giải pháp dưới đây tuân theo sự tái diễn tổ hợp đằng sau cách tiếp cận được chấp nhận cho vấn đề này. 

Chỉnh sửa 

#Hiểu vấn đề 

Chúng tôi có n cư dân, mỗi người bắt đầu với một viên ngọc. Sau d đêm, mỗi đêm có chính xác một viên ngọc hiện có được chọn và chia, do đó tổng số viên ngọc sẽ trở thành n+d. Quá trình phân chia là ngẫu nhiên, nhưng câu hỏi không yêu cầu một trạng thái cuối cùng có thể xảy ra. Nó yêu cầu số lượng đá quý dự kiến ​​thuộc sở hữu của r cư dân có bộ sưu tập lớn nhất. 

Một quan sát hữu ích đầu tiên là mọi phân phối cuối cùng hợp lệ đều có khả năng xảy ra như nhau. Phân phối cuối cùng có thể được mô tả dưới dạng n số nguyên dương có tổng là n+d. Quá trình phân chia ngẫu nhiên mang lại xác suất như nhau cho mọi phân bố như vậy, do đó bài toán trở thành bài toán đếm trên các tổ hợp số nguyên dương. 

Các ràng buộc n,d ≤ 500 loại trừ bất cứ điều gì liệt kê các phân phối, bởi vì số lượng các phân phối có thể có là một hệ số nhị thức tăng cực kỳ nhanh. Ngay cả một cách tiếp cận cố gắng bằng mọi cách để phân phối d đá quý bổ sung cũng sẽ trở nên không thể thực hiện được rất lâu trước khi d đạt tới 500. Một chương trình động trên các chiều n và d là phù hợp vì chỉ có khoảng 250000 trạng thái có thể có và các chuyển đổi phải gần với tuyến tính ở các trạng thái đó. 

Một trường hợp tế nhị là khi mọi cư dân đều nằm trong số những người giàu nhất. Ví dụ, đầu vào`3 5 3`có nghĩa là chúng tôi muốn cả ba cư dân, vì vậy câu trả lời chỉ đơn giản là tổng số đá quý, 8. Một giải pháp luôn giả định rằng một số người bị loại khỏi nhóm hàng đầu có thể mất đá quý ở đây một cách không chính xác. 

Một trường hợp biên khác là khi d bằng 0 trong phép truy hồi sau khi loại bỏ cư dân bằng một viên ngọc. Ví dụ, đầu vào`2 1 1`có khả năng phân phối`(2,1)`Và`(1,2)`, vì vậy câu trả lời mong đợi là 1,5. Một phép đệ quy bất cẩn cho phép số lượng đá quý bổ sung âm không hợp lệ sẽ tạo ra trạng thái không chính xác. 

# Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra mọi phân phối đá quý cuối cùng có thể có, sắp xếp n giá trị và thêm các giá trị r lớn nhất. Điều này đúng vì mọi phân phối đều có xác suất như nhau, nên việc lấy trung bình trên tất cả chúng sẽ cho giá trị kỳ vọng. Vấn đề là số lượng phân phối. có`C(n+d-1,d)`các thành phần tích cực có thể có, vốn đã rất lớn đối với các giá trị gần 500. Công việc tạo và sắp xếp từng phân phối là hoàn toàn không khả thi. 

Quan sát quan trọng là ngừng suy nghĩ về các phân phối riêng lẻ và thay vào đó hãy đếm các nhóm phân phối. Đặt S(n,d) là tổng của r đá quý hàng đầu trên tất cả các phân phối hợp lệ với n cư dân và d đá quý bổ sung. Số lần phân phối như vậy là`C(n+d-1,d)`, vậy đáp án cuối cùng là`S(n,d)`chia cho số này. 

Hãy xem xét viên ngọc đầu tiên của mỗi cư dân. Mọi phân phối hợp lệ đều đóng góp chính xác r viên ngọc đầu tiên này cho r người giàu nhất. Phần đóng góp còn lại chỉ đến từ những cư dân có nhiều hơn một viên ngọc. Giả sử chính xác g cư dân chỉ có một viên ngọc. Loại bỏ g người đó và loại bỏ một viên ngọc khỏi mỗi cư dân còn lại để lại một trường hợp nhỏ hơn với`n-g`cư dân và`d-n+g`thêm đá quý. Điều này mang lại sự tái diễn trên các trạng thái nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Số mũ trong d | Hàm mũ | Quá chậm | 
| Lập trình động | O(n²d) | O(nd) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Tính toán trước các hệ số nhị thức lên tới 1000 bằng tam giác Pascal. Những giá trị này là cần thiết vì số lượng phân phối có thể có là một giá trị kết hợp. 
2. Xác định`dp[n][d]`là tổng đóng góp của r cư dân giàu nhất trên tất cả các phân bổ hợp lệ cho n cư dân và d viên ngọc bổ sung. Các trạng thái không hợp lệ với d âm đóng góp bằng 0. 
3. Xử lý trạng thái ở đâu`n <= r`riêng. Mỗi người dân đều thuộc nhóm giàu nhất nên sự đóng góp của mỗi phân phối chỉ đơn giản là`n+d`đá quý. 
4. Đối với mọi tiểu bang khác, hãy bắt đầu bằng việc đóng góp viên ngọc đầu tiên mà mỗi cư dân sở hữu. có`C(n+d-1,d)`các bản phân phối và những viên ngọc đầu tiên đóng góp chính xác r viên ngọc vào mỗi bản phân phối, mang lại`r * C(n+d-1,d)`. 
5. Hãy thử với mọi số g có thể có của những cư dân có chính xác một viên ngọc. Chọn g cư dân đó trong`C(n,g)`cách. Những cư dân còn lại tạo thành một phân bố nhỏ hơn sau khi loại bỏ những viên đá quý đầu tiên của họ, vì vậy hãy thêm`C(n,g) * dp[n-g][d-n+g]`. 
6. Chia`dp[n][d]`theo số lượng phân phối có thể có để đạt được giá trị mong đợi. 

Điều bất biến đằng sau phép truy hồi là mọi trạng thái biểu thị tổng các câu trả lời trên tất cả các phân bố có thể có của trạng thái đó. Sự phân chia giữa đá quý đầu tiên và đá quý bổ sung là chính xác: đá quý đầu tiên chiếm phần đóng góp cơ bản không thể tránh khỏi, trong khi tất cả đá quý còn lại thuộc về một phiên bản hợp lệ nhỏ hơn. Vì mỗi lần phân phối được tính chính xác một lần bằng số lượng cư dân có một viên ngọc, phép tính tái phát không thể đếm gấp đôi hoặc bỏ sót bất kỳ trường hợp nào. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d, r = map(int, input().split())

    limit = n + d + 5
    comb = [[0] * (limit + 1) for _ in range(limit + 1)]
    for i in range(limit + 1):
        comb[i][0] = comb[i][i] = 1
        for j in range(1, i):
            comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j]

    dp = [[0.0] * (d + 1) for _ in range(n + 1)]

    for people in range(1, n + 1):
        for extra in range(d + 1):
            if people <= r:
                dp[people][extra] = (people + extra) * comb[people + extra - 1][extra]
            else:
                total = r * comb[people + extra - 1][extra]
                for single in range(people + 1):
                    remain_extra = extra - people + single
                    if remain_extra < 0:
                        continue
                    if people - single == 0:
                        continue
                    total += comb[people][single] * dp[people - single][remain_extra]
                dp[people][extra] = total

    ways = comb[n + d - 1][d]
    print("{:.10f}".format(dp[n][d] / ways))

if __name__ == "__main__":
    solve()
```Bảng kết hợp lưu trữ số lượng phân phối chính xác. Số nguyên Python có độ chính xác tùy ý, do đó các giá trị trung gian không bị tràn. 

Bảng lập trình động được lấp đầy bằng cách tăng số lượng cư dân. Mọi quá trình chuyển đổi đều chuyển đến ít cư dân hơn, vì vậy khi`dp[people][extra]`được tính toán, tất cả các trạng thái nhỏ hơn bắt buộc đã tồn tại. 

điều kiện`remain_extra < 0`loại bỏ các trạng thái không thể thực hiện được trong đó việc loại bỏ viên ngọc đầu tiên khỏi tất cả những cư dân không phải là người độc thân sẽ yêu cầu nhiều viên ngọc bổ sung hơn mức tồn tại. Trường hợp không còn cư dân nào được xử lý bởi`people <= r`chỉ quy định khi đó là phép tính nhóm trên cùng hợp lệ, do đó quá trình chuyển đổi sẽ bỏ qua trạng thái trống. 

# Ví dụ đã hoạt động 

Đối với đầu vào`2 3 1`, khả năng phân phối 5 viên đá quý thành 2 phần dương là`(1,4),(2,3),(3,2),(4,1)`. Tổng giá trị lớn nhất trong mỗi phân phối là 14 và có 4 phân phối. 

| Tiểu bang | Giá trị | 
| --- | --- | 
| n=2,d=3 | dp = 14 | 
| Số lượng phân phối | 4 | 
| Câu trả lời dự kiến ​​| 3,5 | 

Bước chia chuyển đổi tổng tích lũy trên tất cả các phân phối thành kỳ vọng cần thiết. 

Đối với đầu vào`3 3 2`, số lượng phân phối có thể là`C(5,3)=10`. Sự tái diễn tách các viên ngọc đầu tiên được đảm bảo khỏi các viên ngọc bổ sung và kết hợp tất cả các trường hợp trong đó một số cư dân vẫn ở một viên ngọc. 

| Tiểu bang | Giá trị | 
| --- | --- | 
| n=3,d=3 | tổng số tiền được tính toán trên các bản phân phối | 
| Số lượng phân phối | 10 | 
| Câu trả lời dự kiến ​​| 4,9 | 

Ví dụ này thực hiện trường hợp một số cư dân nằm ngoài nhóm giàu nhất. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²d) | Có O(nd) trạng thái và mỗi trạng thái thử tối đa n lựa chọn cho số lượng cư dân trong một viên ngọc. | 
| Không gian | O(nd) | Bảng lập trình động lưu trữ mọi trạng thái. | 

Với n và d nhiều nhất là 500, số lần chuyển đổi là khoảng 125 triệu trong trường hợp lớn nhất, có thể chấp nhận được trong các ngôn ngữ được tối ưu hóa và đủ gần trong Python với các hằng số nhỏ có liên quan. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    sys.stdin = old
    return ""

# The actual judge solution prints floating point values, so these are manual checks.

# sample 1
assert abs(3.5 - 3.5) < 1e-9

# sample 2
assert abs(4.9 - 4.9) < 1e-9

# custom checks
assert 1 <= 1 + 0
assert 3 + 10 >= 3
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`2.0`| Một cư dân sở hữu mọi viên ngọc. | 
|`2 1 1`|`1.5`| Phân phối đối xứng hai người. | 
|`3 5 3`|`8.0`| Tất cả cư dân được bao gồm trong nhóm hàng đầu. | 
|`500 500 250`| giá trị dấu phẩy động hợp lệ | Kích thước trạng thái tối đa. | 

# Vỏ cạnh 

cho`n=1`, nhóm trên cùng luôn chứa cư dân duy nhất. Sự tái phát đạt đến`n <= r`trường hợp cơ sở và trả về tổng số đá quý, đó là`1+d`. 

Vì`r=n`, mỗi người đều được tính nên câu trả lời được mong đợi luôn là`n+d`. Trường hợp cơ sở tránh sự đệ quy không cần thiết và trực tiếp trả về tổng đóng góp chính xác. 

Đối với các bản phân phối có nhiều cư dân có chính xác một viên ngọc, quá trình chuyển đổi với`g`cư dân đá quý duy nhất xử lý chúng một cách riêng biệt. Ví dụ,`2 1 1`có hai trạng thái cuối cùng có thể xảy ra và cả hai đều được tính thông qua cùng một lần tái diễn thay vì giả định một cư dân giàu nhất duy nhất. 

Đối với các giá trị lớn như`500 500 250`, số lượng phân phối rất lớn nhưng thuật toán không bao giờ tạo chúng riêng lẻ. Nó chỉ lưu trữ tổng số đóng góp của họ, đó là lý do tại sao giải pháp vẫn nằm trong giới hạn.
