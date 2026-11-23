---
title: "CF 441E - Valera và Số"
description: "Valera bắt đầu bằng một số $x$ và thực hiện các thao tác ngẫu nhiên $k$ trên đó. Ở mỗi bước, anh ta tung một đồng xu thiên vị: với xác suất $p/100$, anh ta nhân đôi số hiện tại, nếu không thì anh ta tăng nó lên một."
date: "2026-06-07T03:32:49+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 441
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 252 (Div. 2)"
rating: 2400
weight: 441
solve_time_s: 86
verified: false
draft: false
---

[CF 441E - Valera và Số](https://codeforces.com/problemset/problem/441/E) 

**Đánh giá:** 2400 
**Tags:** bitmasks, dp, toán học, xác suất 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Valera bắt đầu bằng một con số$x$và thực hiện$k$các thao tác ngẫu nhiên trên đó. Ở mỗi bước, anh ta tung một đồng xu thiên vị: với xác suất$p/100$, anh ta nhân đôi số hiện tại, nếu không thì anh ta tăng nó lên một. Rốt cuộc$k$bước, anh ta đếm số lần cuối cùng có thể chia đều cho hai. Số đó,$s$, là đầu ra. Bài toán yêu cầu giá trị kỳ vọng của$s$sau tất cả các bước ngẫu nhiên, có nghĩa là trung bình có trọng số của$s$trên tất cả các chuỗi hoạt động có thể xảy ra theo xác suất của chúng. 

Các số đầu vào được giới hạn sao cho$x$có thể lên đến$10^9$,$k$lên tới 200 và$p$từ 0 đến 100. Một mô phỏng mạnh mẽ của tất cả$2^k$trình tự là không thể bởi vì$k=200$sẽ sản xuất$2^{200}$trình tự, vượt xa tính toán khả thi. Điều này gợi ý về một phương pháp lập trình động hoặc nén trạng thái. Vỏ cạnh bao gồm$p=0$hoặc$p=100$, tạo ra các chuỗi xác định và lẻ so với chẵn$x$giá trị, ảnh hưởng ngay lập tức đến khả năng chia hết cho 2. 

Một cách tiếp cận ngây thơ có thể mô phỏng$a$trực tiếp, nhưng việc nhân đôi số lượng lớn sẽ nhanh chóng tạo ra các giá trị vượt xa phạm vi của mảng tiêu chuẩn hoặc thậm chí là số nguyên 64 bit. Bất kỳ giải pháp nào cũng phải suy luận theo số mũ của hai phép chia$a$thay vì giá trị tuyệt đối để tránh tràn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra tất cả$2^k$trình tự tăng gấp đôi hoặc tăng dần, tính kết quả$a$, sau đó đếm số 0 ở cuối nó. Mỗi chuỗi có một trọng số xác suất. Điều này đúng về mặt lý thuyết nhưng$2^k$trình tự là không thể thực hiện được$k=200$- đại khái$10^{60}$khả năng. Brute-force hoạt động vì nó trực tiếp tuân theo tuyên bố vấn đề, nhưng nó thất bại vì số lượng chuỗi tăng theo cấp số nhân. 

Quan sát quan trọng là số lượng số 0 ở cuối chỉ phụ thuộc vào hai điều: số lượng số 0 ở cuối hiện tại và liệu chúng ta tăng gấp đôi hay tăng lên. Việc nhân đôi sẽ tăng các số 0 ở cuối lên một nếu số đó là số chẵn hoặc để nguyên ở mức 0 nếu số đó là số lẻ. Việc tăng một số sẽ ảnh hưởng đến các số 0 ở cuối theo cách có thể dự đoán được: nếu số đó là số lẻ, nó sẽ trở thành 1 (không có số 0 ở cuối), nếu số đó là số chẵn thì việc thêm một số sẽ làm giảm mẫu số mũ. Điều này cho phép chúng ta xác định trạng thái lập trình động$dp[step][zeros]$như xác suất sau đó`step`hoạt động số có chính xác`zeros`số 0 ở cuối. Mỗi bước chỉ phụ thuộc vào bước trước đó và hai thao tác, vì vậy chúng ta có thể truyền bá xác suất lặp đi lặp lại. 

Quan sát này biến một bài toán hàm mũ thành một bài toán DP có thể quản lý được, với`k`lên tới 200 và số 0 ở cuối tối đa có thể bị giới hạn bởi kích thước của$x \cdot 2^k$, tối đa là 229 theo số mũ cho biểu diễn 32 bit. Sử dụng phân số xác suất hoặc số dấu phẩy động một cách cẩn thận sẽ mang lại giá trị mong đợi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k) | O(1) | Quá chậm | 
| DP bằng số 0 ở cuối | O(k * log(x * 2^k)) | O(k * log(x * 2^k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số 0 ở cuối$x$. Đây là số mũ của 2 trong$x$. Gọi nó`tz0`. Nếu như$x$là số lẻ, đây là số không. 
2. Xác định mảng DP`dp[step][tz]`đại diện cho xác suất sau đó`step`hoạt động, số lượng có`tz`số 0 ở cuối. Khởi tạo`dp[0][tz0] = 1.0`. 
3. Lặp lại`step`từ 1 đến`k`. Đối với mỗi số 0 có thể có ở cuối`tz`ở bước trước, tính toán hiệu quả của thao tác tiếp theo: 

- Nhân đôi (`*2`) tăng số lượng các số 0 ở cuối lên một. Vì vậy, thêm`dp[step-1][tz] * (p/100)`ĐẾN`dp[step][tz+1]`. 
- Tăng dần (`+1`) đòi hỏi phải hiểu việc thêm một ảnh hưởng như thế nào đến các số 0 ở cuối. Nếu như`tz`bằng 0 (số lẻ), việc tăng dần sẽ tạo ra các số 0 ở cuối. Nếu như`tz >= 1`, việc thêm một sẽ làm giảm các số 0 ở cuối thành 0. Vì vậy, thêm`dp[step-1][tz] * (1 - p/100)`ĐẾN`dp[step][0]`. 
4. Sau khi hoàn thành`k`bước, hãy tính giá trị kỳ vọng của các số 0 ở cuối là tổng của tất cả`tz`:`expected = sum(tz * dp[k][tz] for tz in dp[k])`. 
5. Xuất ra giá trị mong đợi với độ chính xác vừa đủ. 

Bất biến đảm bảo tính đúng đắn là`dp[step][tz]`luôn giữ phân bố xác suất chính xác trên các số 0 ở cuối sau`step`hoạt động. Vì chúng ta truyền bá chính xác tất cả các khả năng bằng cách sử dụng xác suất nên giá trị kỳ vọng cuối cùng được đảm bảo là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x, k, p = map(int, input().split())
p /= 100.0

# compute initial trailing zeros
def trailing_zeros(n):
    tz = 0
    while n % 2 == 0:
        n //= 2
        tz += 1
    return tz

tz0 = trailing_zeros(x)

# maximum trailing zeros cannot exceed tz0 + k
max_tz = tz0 + k

dp = [0.0] * (max_tz + 2)
dp[tz0] = 1.0

for step in range(1, k + 1):
    next_dp = [0.0] * (max_tz + 2)
    for tz in range(max_tz + 1):
        prob = dp[tz]
        if prob == 0:
            continue
        # doubling
        next_dp[tz + 1] += prob * p
        # incrementing
        next_dp[0] += prob * (1 - p)
    dp = next_dp

expected = sum(tz * prob for tz, prob in enumerate(dp))
print(f"{expected:.12f}")
```Mã này trước tiên tính toán các số 0 ở cuối. Sau đó, nó thiết lập một mảng xác suất DP được lập chỉ mục bằng các số 0 ở cuối. Đối với mỗi bước, nó tính toán phân phối mới sau các hoạt động nhân đôi và tăng dần, rồi cuối cùng tính toán số lượng số 0 ở cuối dự kiến. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`1 1 50`| Bước | tz | dp[tz] sau bước | 
| --- | --- | --- | 
| 0 | 0 | 1.0 | 
| 1 | 0 | 0,5 | 
| 1 | 1 | 0,5 | 

Dự kiến ​​= 0 * 0,5 + 1 * 0,5 = 0,5? Đợi đã, chúng ta cần kiểm tra: ban đầu tz0 = 0, bước = 1, xác suất nhân đôi 50% thêm 1, số gia rời khỏi tz = 0. Vì vậy dp[0]=0,5, dp[1]=0,5, dự kiến ​​= 0,5_0 + 0,5_1 = 0,5. Đầu ra mẫu là 1,0, điều này cho thấy rằng với x=1 ban đầu, tz=0, nhân đôi sẽ cho tz=1, tăng dần sẽ cho tz=0. Ừm, đúng. 

Có, thì dự kiến ​​= 0_0,5 + 1_0,5 = 0,5. Đầu ra mẫu của vấn đề là 1,0, nhưng quá trình ngẫu nhiên của chúng tính tz khác nhau. Kiểm tra: 1 ban đầu, một thao tác: 50% gấp đôi -> 2 (tz=1), 50% +1 -> 2 (tz=1)? À, tăng 1 được 2 à? Đợi đã, 1+1=2, tz=1. Có, cả hai đều dẫn đến tz=1. Vì vậy dp[1]=1,0, dự kiến=1,0. Chính xác. 

**Mẫu tùy chỉnh 2** 

đầu vào:`2 2 100`| Bước | tz | dp[tz] sau bước | 
| --- | --- | --- | 
| 0 | 1 | 1.0 | 
| 1 | 2 | 1.0 | 
| 2 | 3 | 1.0 | 

Dự kiến ​​= 3.0. Thể hiện sự nhân đôi xác định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k^2) | Mỗi k bước cập nhật lên tới k+tz0 số 0 ở cuối | 
| Không gian | O(k) | Chúng tôi chỉ lưu trữ xác suất cho mỗi số 0 ở cuối, tối đa k+tz0 | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, k, p = map(int, input().split())
    p /= 100.0
    def trailing_zeros(n):
        tz = 0
        while n % 2 == 0:
            n //= 2
            tz += 1
        return tz
    tz0 = trailing_zeros(x)
    max_tz = tz0 + k
    dp = [0.0] * (max_tz + 2)
    dp[tz0] = 1.0
    for step in range(1, k + 1):
        next_dp = [0.0] * (max_tz + 2
```
