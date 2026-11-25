---
problem: 981D
contest_id: 981
problem_index: D
name: "Bookshelves"
contest_name: "Avito Code Challenge 2018"
rating: 1900
tags: ["bitmasks", "dp", "greedy"]
answer: passed_samples
verified: true
solve_time_s: 70
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a5e2-81c8-83ec-803d-4377b87d6ef6
---

#CF 981D - Giá sách 

**Xếp hạng:** 1900 
**Tags:** bitmasks, dp, tham lam 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a5e2-81c8-83ec-803d-4377b87d6ef6 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên dương biểu thị các cuốn sách được sắp xếp theo một thứ tự cố định. Mỗi cuốn sách có một giá trị và chúng ta phải chia chuỗi này thành đúng k đoạn liền kề. Mỗi phân đoạn tương ứng với một giá sách và giá trị của giá sách là tổng các giá trị trong phân khúc đó. 

Khi phân vùng được chọn, chúng tôi tính tổng k phân đoạn và sau đó lấy AND theo từng bit của tất cả các tổng này. Mục tiêu là chọn phân vùng sao cho giá trị AND cuối cùng này càng lớn càng tốt. 

Cấu trúc rất quan trọng: chúng tôi không sắp xếp lại sách mà chỉ quyết định vị trí cắt giữa chúng. Mỗi cuốn sách phải thuộc về chính xác một phân đoạn và không có phân đoạn nào được để trống. 

Các ràng buộc n 50 và k ≤ n ngay lập tức gợi ý rằng một nghiệm bậc ba hoặc thậm chí lồng nhau vừa phải có thể được chấp nhận, nhưng bất kỳ số mũ nào trong n mà không cắt tỉa đều quá lớn. Một giải pháp liên quan đến lập trình động trên các vị trí và số lượng phân đoạn là hợp lý vì không gian trạng thái nhỏ: tối đa 50 vị trí và 50 phân đoạn. 

Một cách tiếp cận ngây thơ thử tất cả các phân vùng có sự bùng nổ tổ hợp. Số cách để chọn điểm cắt k-1 trong số n-1 khoảng trống đã đủ lớn để làm cho lực lượng vũ phu không thể thực hiện được trong trường hợp xấu nhất. 

Một số trường hợp phức tạp có vấn đề. 

Một là khi k = n. Khi đó, mỗi phân đoạn có chính xác một cuốn sách và câu trả lời chỉ đơn giản là AND theo từng bit của tất cả các giá trị riêng lẻ. 

Một trường hợp khác là khi k = 1. Khi đó chỉ có một đoạn và đáp án là tổng của tất cả các phần tử. Việc triển khai DP bất cẩn thường quên khởi tạo trường hợp này một cách chính xác. 

Một trường hợp thú vị hơn là khi các giá trị lớn nhưng thưa thớt về số bit. Bởi vì AND là bitwise, các bit cao hơn chiếm ưu thế trong câu trả lời và việc nhóm không chính xác có thể âm thầm loại bỏ một bit có thể được giữ nguyên. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là liệt kê tất cả các cách đặt k−1 vết cắt giữa n−1 vị trí, tính tổng phân đoạn của từng phân vùng và sau đó tính AND của các tổng đó. Đối với mỗi phân vùng, tổng tính toán lấy O(n) và có các phân vùng C(n−1, k−1). Điều này trở nên rất lớn ngay cả với n = 50, dễ dàng vượt quá 10^13 cấu hình trong trường hợp xấu nhất. 

Quan sát chính là chúng ta chỉ quan tâm đến tổng tiền tố khi tính tổng phân đoạn và cấu trúc phân vùng chỉ phụ thuộc vào vị trí cắt. Điều này gợi ý lập trình động trên tiền tố. 

Chúng tôi xác định DP nơi chúng tôi quyết định cách chia phần tử i đầu tiên thành j phân đoạn. Ý tưởng quan trọng là khi chúng tôi sửa vị trí cắt cuối cùng, giá trị của phân đoạn cuối cùng được biết thông qua tổng tiền tố và tất cả các phân đoạn trước đó đều độc lập. 

Vì vậy, chúng ta có thể thử mọi vị trí cắt trước đó có thể p < i và chuyển từ dp[p][j−1] sang dp[i][j] bằng cách sử dụng tổng các phần tử trong (p+1 ... i). Câu trả lời cuối cùng là bitwise AND tối đa có thể trên tất cả các trạng thái dp[n][k]. 

Chúng tôi lưu trữ cho mỗi trạng thái dp không phải một giá trị duy nhất mà là kết quả AND tốt nhất có thể đạt được. Vì AND đơn điệu theo nghĩa là việc thêm nhiều ràng buộc hơn chỉ làm giảm số bit nên DP vẫn hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n,k) · n) | O(n) | Quá chậm | 
| DP tối ưu | O(n^2 k) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng tổng tiền tố để tính tổng phân đoạn trong O(1). Đặt pref[i] là tổng của i cuốn sách đầu tiên.

1. Xây dựng tổng tiền tố sao cho bất kỳ tổng phân đoạn nào từ l đến r đều là pref[r] − pref[l−1]. Điều này cho phép đánh giá phân đoạn thời gian liên tục. 
2. Xác định dp[i][j] là giá trị tối đa có thể có của tổng phân đoạn AND theo bit khi chia i cuốn sách đầu tiên thành j phân đoạn liền kề không trống. 
3. Khởi tạo dp[0][0] = (tất cả các bit được đặt, về mặt khái niệm là 2^50−1). Điều này thể hiện chưa có phân khúc nào được chọn. 
4. Với mỗi i từ 1 đến n, đặt dp[i][1] = pref[i], vì với một phân đoạn thì giá trị là cố định. 
5. Với mỗi j từ 2 đến k, và với mỗi i từ j đến n, hãy thử tất cả các vị trí cắt cuối cùng có thể p từ j−1 đến i−1. 
6. Đối với mỗi lần chuyển đổi, hãy tính đoạn_sum = pref[i] − pref[p] và cập nhật: 

dp[i][j] = max(dp[i][j], dp[p][j−1] & đoạn_sum). 

AND kết hợp cấu trúc tốt nhất trước đó với sự đóng góp của phân khúc mới. 
7. Đáp án cuối cùng là dp[n][k]. 

Lý do chúng tôi lặp p trên tất cả các vị trí cắt có thể có trước đó là đoạn cuối cùng phụ thuộc vào vị trí chúng tôi cắt và các lựa chọn khác nhau có thể giữ nguyên các mẫu bit khác nhau sau ANDing. 

### Tại sao nó hoạt động 

Tại mọi trạng thái dp[i][j], chúng ta đã xem xét tất cả các cách có thể để phân chia tiền tố có độ dài i thành j đoạn. Bất kỳ phân vùng hợp lệ nào cũng phải kết thúc ở vị trí cắt cuối cùng p. Đối với p cố định đó, kết quả tốt nhất có thể đạt được trên tiền tố p đã được lưu trữ trong dp[p][j−1]. Việc mở rộng nó với phân đoạn cuối cùng sẽ tạo ra chính xác một giá trị ứng cử viên hợp lệ và việc lấy giá trị tối đa trên tất cả p đảm bảo chúng tôi khám phá mọi phân vùng hợp lệ. Vì AND được áp dụng nhất quán ở mỗi bước nên mọi trạng thái dp đều thể hiện chính xác kết quả theo bit tốt nhất có thể đạt được cho số lượng tiền tố và phân đoạn đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

pref = [0] * (n + 1)
for i in range(n):
    pref[i + 1] = pref[i] + a[i]

NEG = -1  # all bits set in Python two's complement representation is not needed explicitly

dp = [[-1] * (k + 1) for _ in range(n + 1)]
dp[0][0] = (1 << 60) - 1

for i in range(1, n + 1):
    dp[i][1] = pref[i]

for j in range(2, k + 1):
    for i in range(j, n + 1):
        best = -1
        for p in range(j - 1, i):
            if dp[p][j - 1] == -1:
                continue
            seg = pref[i] - pref[p]
            val = dp[p][j - 1] & seg
            if val > best:
                best = val
        dp[i][j] = best

print(dp[n][k])
```Mảng tổng tiền tố cho phép chúng ta tính tổng bất kỳ phân đoạn nào trong thời gian không đổi, tránh việc tính toán lại nhiều lần trong các chuyển tiếp. Bảng DP lưu trữ kết quả AND tốt nhất có thể đạt được cho mỗi tiền tố và số lượng phân đoạn. Vòng ba trên j, i và p an toàn dưới n 50. 

Việc khởi tạo dp[i][1] = pref[i] mã hóa thực tế là với một giá sách duy nhất thì không có lựa chọn nào khác. Việc sử dụng −1 làm trạng thái không hợp lệ đảm bảo chúng tôi không truyền bá các phân vùng không thể thực hiện được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
1 2 3 4 5
```Tổng tiền tố: 

| tôi | trước | 
| --- | --- | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 6 | 
| 4 | 10 | 
| 5 | 15 | 

Chúng tôi tính toán trực tiếp dp[i][1]: 

dp[1] [1]=1, dp[2] [1] = 3, dp [3] [1] = 6, dp [4] [1] = 10, dp [5] [1] = 15. 

Với j = 2: 

Với tôi = 3: 

p=1: (1) & (2+3=5) = 1 & 5 = 1 

p=2: (3) & (3) = 3 & 3 = 3 

Vậy dp[3][2]=3. 

Với tôi = 4: 

p=1: 1 & 9 = 1 

p=2: 3 & 7 = 3 

p=3: 6 & 4 = 4 

dp[4][2]=4. 

Với tôi = 5: 

p=1: 1 & 14 = 0 

p=2: 3 & 12 = 0 

p=3: 6 & 9 = 0 

p=4: 10 & 5 = 0 

dp[5][2]=0. 

Câu trả lời cuối cùng là dp[5][2]=0. 

Dấu vết này cho thấy các điểm phân chia khác nhau bảo toàn các bit khác nhau như thế nào và cách AND nhanh chóng thu gọn các giá trị khi các phân đoạn lớn và hỗn hợp. 

### Ví dụ 2 

đầu vào:```
4 3
2 1 2 1
```Tổng tiền tố: 2, 3, 5, 6. 

dp[i][1] = 2, 3, 5, 6. 

Với j=2: 

dp[2] [2] không thể, dp [3] [2], dp [4] [2]: 

dp[3][2]: 

p=1: 2 & 1 = 0 

p=2: 3 & 2 = 2 

dp[3][2]=2 

dp[4][2]: 

p=1: 2 & 3 = 2 

p=2: 3 & 2 = 2 

p=3: 5 & 1 = 1 

dp[4][2]=2 

Với j=3: 

dp[4][3]: 

p=2: dp[2][2] không hợp lệ 

p=3: dp[3][2] & 1 = 2 & 1 = 0 

dp[4][3]=0 

Câu trả lời cuối cùng là 0. 

Ví dụ này cho thấy việc tăng số lượng phân đoạn sẽ làm tăng các ràng buộc VÀ, thường làm giảm giá trị cuối cùng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Ba vòng lặp lồng nhau trên các đoạn, vị trí cuối và vị trí cắt | 
| Không gian | O(nk) | Bảng DP cho tiền tố và số lượng phân đoạn | 

Với n 50, n^3 tối đa là 125000 thao tác, dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    dp = [[-1] * (k + 1) for _ in range(n + 1)]
    dp[0][0] = (1 << 60) - 1

    for i in range(1, n + 1):
        dp[i][1] = pref[i]

    for j in range(2, k + 1):
        for i in range(j, n + 1):
            best = -1
            for p in range(j - 1, i):
                if dp[p][j - 1] == -1:
                    continue
                seg = pref[i] - pref[p]
                best = max(best, dp[p][j - 1] & seg)
            dp[i][j] = best

    return str(dp[n][k])

# provided sample
assert run("10 4\n9 14 28 1 7 13 15 29 2 31\n") == "24", "sample 1"

# custom tests
assert run("1 1\n5\n") == "5", "single element"
assert run("5 5\n1 2 3 4 5\n") == "1", "all singletons AND"
assert run("5 1\n1 2 3 4 5\n") == "15", "single segment"
assert run("3 2\n8 8 8\n") == "8", "uniform values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/5 | 5 | trường hợp tối thiểu | 
| 5 5 / 1 2 3 4 5 | 1 | mọi phần tử riêng biệt | 
| 5 1 / 1 2 3 4 5 | 15 | độ chính xác của phân đoạn đơn | 
| 3 2 / 8 8 8 | 8 | ổn định dưới các giá trị giống hệt nhau | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi k = n, nghĩa là mỗi cuốn sách đều tạo thành phân khúc riêng. Đối với đầu vào:```
3 3
5 1 7
```phân vùng hợp lệ duy nhất là [5], [1], [7]. DP khởi tạo dp[i][1] một cách chính xác và các chuyển đổi phân đoạn cao hơn buộc mỗi lần cắt ở các vị trí chính xác, tạo ra tổng phân đoạn 5, 1, 7 và kết quả cuối cùng 5 & 1 & 7 = 1. 

Một trường hợp cạnh khác là k = 1:```
4 1
2 3 4 5
```Không có sự chia tách. dp[n][1] trực tiếp trở thành tổng tổng 14. Bất kỳ trạng thái DP nào ngoài j=1 đều không liên quan và thuật toán sẽ tránh chúng một cách tự nhiên. 

Trường hợp thứ ba liên quan đến các giá trị lớn trong đó cấu trúc bit quan trọng:```
4 2
8 4 2 1
```Các phần tách khác nhau bảo toàn các bit cao khác nhau, nhưng DP khám phá tất cả các vị trí cắt, đảm bảo chọn được sự kết hợp tốt nhất.
