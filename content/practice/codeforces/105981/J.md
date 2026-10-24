---
title: "CF 105981J - Quy trình giảm dần ngẫu nhiên thống nhất"
description: "Chúng ta được cung cấp một quá trình liên tục thu nhỏ một số nguyên. Bắt đầu từ một giá trị m = n, một thao tác sẽ thay thế nó bằng một số nguyên ngẫu nhiên thống nhất trong phạm vi [0, m-1]. Quá trình dừng lại khi giá trị trở thành 0."
date: "2026-06-22T16:32:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105981
codeforces_index: "J"
codeforces_contest_name: "The 2025 Hunan University Programming Contest"
rating: 0
weight: 105981
solve_time_s: 56
verified: true
draft: false
---

[CF 105981J - Quy trình giảm dần ngẫu nhiên thống nhất](https://codeforces.com/problemset/problem/105981/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một quá trình liên tục thu nhỏ một số nguyên. Bắt đầu từ một giá trị`m = n`, một thao tác thay thế nó bằng một số nguyên ngẫu nhiên thống nhất trong phạm vi`[0, m-1]`. Quá trình dừng lại khi giá trị trở thành`0`. Nhiệm vụ là tính toán số lượng hoạt động dự kiến ​​cần thiết cho đến khi kết thúc, với nhiều giá trị bắt đầu khác nhau.`n`. 

Mỗi trường hợp thử nghiệm đưa ra một số bắt đầu và chúng ta phải xuất ra số lần giảm ngẫu nhiên dự kiến ​​cho đến khi quá trình đạt đến số 0. 

Các ràng buộc rất lớn: lên tới`10^5`các trường hợp thử nghiệm và mỗi trường hợp`n`có thể lớn như`10^5`. Điều này loại trừ mọi cách tiếp cận tính toán lại kỳ vọng từ đầu cho mỗi truy vấn theo thời gian tuyến tính hoặc bậc hai. Thậm chí`O(n^2)`quá trình tiền xử lý đã chặt chẽ rồi, nhưng`O(n log n)`hoặc`O(n)`tiền xử lý với`O(1)`các truy vấn rõ ràng là có thể chấp nhận được. 

Sự tinh tế chính là quá trình này mang tính ngẫu nhiên nhưng có cấu trúc cao. Mỗi tiểu bang`m`chỉ phụ thuộc vào các trạng thái nhỏ hơn`0..m-1`, gợi ý mạnh mẽ về một công thức quy hoạch động. 

Một trường hợp lỗi phổ biến xuất hiện khi coi các quá trình chuyển đổi là độc lập hoặc cố gắng mô phỏng quá trình. 

Ví dụ, nếu`n = 2`, quá trình bắt đầu từ 2, sau đó chuyển sang giá trị ngẫu nhiên trong`{0, 1}`. Nếu nó đi đến`1`, trung bình chúng ta vẫn cần thêm một bước nữa, nhưng nếu nó đi tới`0`, chúng tôi dừng lại ngay lập tức. Một mô phỏng đơn giản lấy trung bình một vài lần chạy ngẫu nhiên có thể hội tụ chậm hoặc sai lệch do sự phân bố bị lệch nhiều về phía kết thúc sớm. 

Một trường hợp cạnh khác là`n = 0`. Quá trình đã hoàn tất nên câu trả lời là chính xác`0`. Bất kỳ sự lặp lại nào đều phải cố định trường hợp cơ sở này một cách rõ ràng, nếu không nó sẽ truyền bá sai những kỳ vọng không xác định. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về kỳ vọng là xác định`E(m)`là số lượng hoạt động dự kiến ​​​​bắt đầu từ giá trị`m`. Từ tiểu bang`m`, một thao tác luôn được thực hiện và sau đó chúng tôi chuyển đổi thống nhất sang bất kỳ trạng thái nào`0`bởi vì`m-1`. Điều này mang lại sự tái diễn trực tiếp```
E(m) = 1 + (1/m) * sum_{k=0}^{m-1} E(k)
```Công thức này đúng nhưng tốn kém nếu đánh giá trực tiếp. Đối với mỗi`m`, tính tổng trên tất cả các chi phí ở trạng thái trước đó`O(m)`, dẫn đến`O(n^2)`tiền xử lý. 

Sự đơn giản hóa chính xuất phát từ việc lưu ý rằng phép truy toán chỉ phụ thuộc vào tổng tiền tố của`E`. Nếu chúng ta duy trì`S(m) = sum_{k=0}^m E(k)`, sau đó sự tái phát trở thành```
E(m) = 1 + (1/m) * S(m-1)
```và do đó```
S(m) = S(m-1) + E(m)
```Điều này biến vấn đề thành một đường tuyến tính duy nhất. 

Cấu trúc sâu hơn là mỗi`E(m)`chỉ phụ thuộc vào thông tin tổng hợp về các trạng thái nhỏ hơn chứ không phụ thuộc vào giá trị riêng lẻ của chúng. Điều này loại bỏ sự cần thiết phải tính toán lại các khoản tiền nhiều lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tái phát trực tiếp với tính toán lại | O(n^2) | O(n) | Quá chậm | 
| Tối ưu hóa DP tổng tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán DP tối ưu 

1. Khởi tạo`E(0) = 0`Và`S(0) = 0`. Điều này phù hợp với định nghĩa rằng không cần thực hiện thao tác nào khi giá trị đã bằng 0. 
2. Lặp lại`m`từ`1`đến mức tối đa`n`trên tất cả các trường hợp thử nghiệm. 
3. Tính toán`E(m)`sử dụng tổng tiền tố được lưu trữ:```
E(m) = 1 + S(m-1) / m
```Điều này hoạt động vì tất cả các chuyển đổi từ`m`đi thống nhất đến các bang nhỏ hơn và kỳ vọng của họ đã được tổng hợp thành`S(m-1)`. 
4. Cập nhật tổng tiền tố:```
S(m) = S(m-1) + E(m)
```5. Sau khi tiền xử lý, mỗi truy vấn`n`được trả lời trực tiếp như`E(n)`. 

Lý do thứ tự này quan trọng là`E(m)`phải luôn được tính toán sau khi tất cả các giá trị nhỏ hơn được hoàn thành, vì nó phụ thuộc vào tiền tố đầy đủ. 

### Tại sao nó hoạt động 

Điều bất biến là khi bắt đầu lặp`m`,`S(m-1)`chính xác là tổng kỳ vọng chính xác cho tất cả các trạng thái nhỏ hơn hoàn toàn so với`m`. Sự tái diễn thể hiện`E(m)`hoàn toàn là sự kết hợp tuyến tính của các giá trị đã đúng này. Vì không có trạng thái nào trong tương lai ảnh hưởng đến các trạng thái trước đó nên DP không bao giờ sửa lại giá trị sau khi nó được tính toán và mỗi kỳ vọng chỉ được xây dựng từ thông tin được xác định đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 100000

E = [0.0] * (MAXN + 1)

S = [0.0] * (MAXN + 1)

for m in range(1, MAXN + 1):
    E[m] = 1.0 + S[m - 1] / m
    S[m] = S[m - 1] + E[m]

t = int(input())
out = []
for _ in range(t):
    n = int(input())
    out.append(str(E[n]))

print("\n".join(out))
```Mã này tính toán trước các kỳ vọng một lần đến mức tối đa có thể`n`. Mảng`E`lưu trữ câu trả lời cuối cùng cho mỗi trạng thái, trong khi`S`duy trì tổng tiền tố để cho phép đánh giá lặp lại theo thời gian không đổi. Mỗi bản cập nhật chỉ sử dụng các giá trị được tính toán trước đó, do đó, vòng lặp hoàn toàn chỉ chuyển tiếp và ổn định về số lượng để có độ chính xác cần thiết. 

Một điểm tinh tế là mọi thứ đều được lưu trữ trong dấu phẩy động. Vì độ chính xác yêu cầu là`1e-4`, độ chính xác kép là đủ và không cần số học hợp lý chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 2 

Chúng tôi tính toán từng bước. 

| m | S(m-1) | E(m) | S(m) | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 1 + 1/2 = 1,5 | 2,5 | 

Vì`n = 2`, kết quả là`1.5`. 

Dấu vết này cho thấy trạng thái như thế nào`2`tổng hợp cả hai kết quả từ`{0,1}`thống nhất. 

### Ví dụ 2: n = 4 

| m | S(m-1) | E(m) | S(m) | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 1,5 | 2,5 | 
| 3 | 2,5 | 1 + 2,5/3 ≈ 1,8333 | 4.3333 | 
| 4 | 4.3333 | 1 + 4,3333/4 ≈ 2,0833 | 6.4166 | 

Vì thế`E(4) ≈ 2.0833`. 

Điều này chứng tỏ mỗi lớp chỉ phụ thuộc vào những kỳ vọng tích lũy chứ không phải những chuyển đổi riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + T) | Một lần xử lý trước đạt đến mức tối đa`n`, thì O(1) cho mỗi truy vấn | 
| Không gian | O(N) | Mảng`E`Và`S`lưu trữ giá trị cho tất cả các trạng thái | 

Giới hạn tiền xử lý`n ≤ 10^5`làm cho DP tuyến tính khả thi trong giới hạn thời gian. Mỗi truy vấn sau đó là thời gian không đổi, dễ dàng hỗ trợ`10^5`trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    MAXN = 100000
    E = [0.0] * (MAXN + 1)
    S = [0.0] * (MAXN + 1)

    for m in range(1, MAXN + 1):
        E[m] = 1.0 + S[m - 1] / m
        S[m] = S[m - 1] + E[m]

    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        out.append(str(E[n]))
    return "\n".join(out)

# provided samples
assert abs(float(run("5\n0\n1\n2\n10\n100000\n").split()[0]) - 0.0) < 1e-6

# custom cases
assert abs(float(run("1\n0\n")) - 0.0) < 1e-6
assert abs(float(run("1\n1\n")) - 1.0) < 1e-6
assert abs(float(run("1\n2\n")) - 1.5) < 1e-6
assert abs(float(run("1\n10\n")).splitlines()[0])  # sanity check existence
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 0 | 0 | chấm dứt trường hợp cơ sở | 
| n = 1 | 1 | bước cưỡng bức duy nhất | 
| n = 2 | 1,5 | phân chia xác suất phân nhánh | 
| n = 10 | ~2,93 | hành vi tích lũy không cần thiết | 

## Vỏ cạnh 

cho`n = 0`, DP phải quay lại`0`mà không cố gắng truy cập các tổng tiền tố ở các chỉ số âm. Việc khởi tạo`E(0) = 0`đảm bảo sự tái phát không bao giờ cần điều chỉnh cho trường hợp này. 

Vì`n = 1`, quá trình chuyển đổi luôn đi đến`0`, vì vậy chỉ cần thực hiện một thao tác. Trong DP, điều này được tính là`E(1) = 1 + S(0)/1 = 1`, xác nhận tính đúng đắn. 

Đối với các giá trị lớn hơn, độ ổn định phụ thuộc vào thứ tự đúng: nếu`E(m)`được tính toán trước`S(m-1)`được xây dựng hoàn chỉnh, các trạng thái sau này sẽ làm ảnh hưởng đến các tính toán trước đó. Cấu trúc chỉ chuyển tiếp tránh hoàn toàn điều này, đảm bảo mỗi kỳ vọng được hoàn thành chính xác một lần.
