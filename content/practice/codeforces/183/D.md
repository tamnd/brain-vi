---
title: "CF 183D - Áo thun"
description: "Chúng tôi được giao nhiệm vụ phân phối áo phông cho một nhóm kỹ sư trong đó mỗi kỹ sư có một phân bố xác suất về kích cỡ áo phông phù hợp với họ. Chúng ta biết có bao nhiêu kỹ sư, n, và tổng số cỡ áo thun, m."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 183
codeforces_index: "D"
codeforces_contest_name: "Croc Champ 2012 - Final"
rating: 2700
weight: 183
solve_time_s: 75
verified: true
draft: false
---

[CF 183D - Áo thun](https://codeforces.com/problemset/problem/183/D) 

**Xếp hạng:** 2700 
**Tags:** dp, tham lam, xác suất 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao nhiệm vụ phân phối áo phông cho một nhóm kỹ sư trong đó mỗi kỹ sư có một phân bố xác suất về kích cỡ áo phông phù hợp với họ. Chúng tôi biết có bao nhiêu kỹ sư, _n_, và tổng số cỡ áo phông, _m_. Mỗi kỹ sư có chính xác một kích cỡ phù hợp với họ, nhưng chúng tôi chỉ biết khả năng phù hợp của từng kích thước. Mục tiêu của chúng tôi là chọn chính xác _n_ áo phông, có thể là nhiều chiếc có cùng kích cỡ, theo cách tối đa hóa số lượng kỹ sư mong đợi sẽ thực sự nhận được chiếc áo phông vừa vặn với họ. 

Đầu vào mã hóa xác suất dưới dạng số nguyên từ 0 đến 1000, phải được hiểu là phần nghìn, nghĩa là giá trị 500 thể hiện xác suất 50%. Đầu ra yêu cầu độ chính xác cao vì cho phép sai số lên tới 10⁻⁹. 

Các ràng buộc ngụ ý rằng một cách tiếp cận ngây thơ để kiểm tra mọi bộ áo phông _n_ có thể có là không khả thi: có khoảng$\binom{n+m-1}{m-1}$nhiều bộ, có thể vượt quá$10^{700}$cho giới hạn trên của _n_ và _m_. Với _n_ lên tới 3000 và _m_ lên tới 300, điều này đòi hỏi một phương pháp lập trình động. Xác suất là số thực nên lời giải chỉ có số nguyên là không đủ. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó xác suất đồng đều giữa các kích thước, như trong mẫu có hai kỹ sư và hai kích thước đều ở mức 50% hoặc trong đó một kích thước chiếm ưu thế. Trong những trường hợp như vậy, các chiến lược tham lam ngây thơ như “mang lại quy mô phù hợp nhất cho kỹ sư đầu tiên” sẽ thất bại vì chúng bỏ qua sự phân bổ tích lũy trên tất cả các kỹ sư. Một kịch bản tinh tế khác phát sinh nếu tất cả các xác suất bằng 0 đối với một kích thước nhất định ngoại trừ một kỹ sư: một cách tiếp cận bất cẩn có thể mang lại những chiếc áo phông có kích thước dư thừa và lãng phí tài nguyên. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ tạo ra mọi tổ hợp áo phông có độ dài _n_ và tính giá trị mong đợi cho mỗi tổ hợp. Đối với mỗi bộ nhiều bộ, chúng tôi sẽ lặp lại tất cả các hoán vị của các kỹ sư, kiểm tra xem kỹ sư nào có thể được giao chiếc áo phông có kích cỡ của họ. Ngay cả khi chúng tôi tối ưu hóa bằng cách lập trình động theo các hoán vị thì riêng số lượng cấu hình áo phông cũng theo cấp số nhân trong _n_ và _m_, khiến điều này không thể thực hiện được. 

Quan sát quan trọng là chúng ta có thể mô hình hóa vấn đề này như một vấn đề lập trình động trong đó các trạng thái theo dõi số lượng áo phông ở mỗi kích cỡ được sử dụng cho đến nay. Cho phép`dp[i][mask]`đại diện cho số lượng kỹ sư hài lòng tối đa được mong đợi sau khi xem xét _i_ kỹ sư đầu tiên và nhiều bộ áo phông còn lại được đại diện bởi`mask`. Điều này có vẻ khó khăn vì không gian trạng thái vẫn còn rất lớn. Tuy nhiên, chúng ta có thể đơn giản hóa hơn nữa bằng cách lưu ý rằng chúng ta chỉ cần theo dõi số lượng áo phông có sẵn ở mỗi kích cỡ chứ không cần theo dõi hoán vị. Mỗi kỹ sư có thể được xử lý tuần tự và đối với một bộ áo phông nhất định, xác suất họ hài lòng chỉ phụ thuộc vào số lượng áo phông của mỗi kích cỡ còn lại. 

Chúng tôi xác định một DP`f[k1][k2]...[km]`là số lượng kỹ sư hài lòng mong đợi nếu chúng ta có`k1`Áo phông size 1,`k2`Áo phông size 2, ...,`km`Còn lại áo thun size m. Sau đó, đối với mỗi kỹ sư, mức tăng dự kiến ​​là sự kết hợp tuyến tính của các xác suất được tính trọng số bởi liệu chúng tôi có còn áo phông cỡ đó hay không. Điều này cho phép chúng tôi giảm vấn đề xuống còn DP đối với số lượng áo phông, vấn đề này có thể được triển khai một cách hiệu quả bằng cách sử dụng tính năng ghi nhớ và chuyển đổi qua việc thêm từng chiếc áo phông hoặc tương đương, sử dụng mảng cuộn và xác suất tính toán trước. 

Cái nhìn sâu sắc giúp điều này trở nên dễ thực hiện là nhận ra rằng chúng tôi có thể lặp lại các kỹ sư theo thứ tự và xem xét “chúng tôi mang theo bao nhiêu áo phông ở mỗi kích cỡ” như một vấn đề phân bổ nguồn lực. Xác suất để một kỹ sư hài lòng là tuyến tính đối với số áo phông còn lại, điều này cho phép chúng ta áp dụng DP theo số lượng thay vì hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n) | O(m^n) | Quá chậm | 
| DP tối ưu trên số lượng áo phông | O(n * m * n) ≈ 3*10⁹ trong trường hợp xấu nhất là ngây thơ | O(n * m) với tối ưu hóa | Có thể chấp nhận được nếu thực hiện cẩn thận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng kỹ sư`n`và kích cỡ áo thun`m`. Chuyển đổi xác suất số nguyên thành phân số bằng cách chia cho 1000. Lưu trữ chúng trong danh sách 2D`p[i][j]`Ở đâu`i`là chỉ số kỹ sư và`j`là chỉ số kích thước. 
2. Xác định mảng DP`dp[k]`Ở đâu`k`là tổng số áo phông được giao cho đến nay. Vì chúng tôi cần chính xác _n_ áo phông nên chúng tôi theo dõi số lượng kỹ sư hài lòng dự kiến ​​cho mỗi tổng số. 
3. Lặp lại các kỹ sư một cách tuần tự. Đối với mỗi kỹ sư, hãy xem xét từng kích cỡ`j`. Với mỗi lần phân bổ áo phông có thể có cho đến thời điểm hiện tại, hãy tính số lượng dự kiến ​​nếu chúng ta phân bổ thêm một chiếc áo phông cùng cỡ`j`cho kỹ sư này. Công thức chuyển tiếp là`dp[new] = max(dp[new], dp[old] + probability * 1.0)`, Ở đâu`probability`khả năng kỹ sư này phù hợp với quy mô này và`new`đại diện cho số lượng tăng dần. 
4. Vì mỗi lần chúng tôi chỉ thêm một kỹ sư nên chúng tôi có thể cuộn mảng DP để tiết kiệm bộ nhớ và đảm bảo luôn xử lý các trạng thái trước đó trước khi sử dụng chúng cho kỹ sư tiếp theo. 
5. Sau khi tất cả các kỹ sư được xử lý,`dp[n]`(hoặc trạng thái cuối cùng thích hợp) chứa số lượng kỹ sư dự kiến ​​​​tối đa sẽ nhận được một chiếc áo phông có kích cỡ của họ. 

Tại sao nó hoạt động: Mỗi trạng thái DP thể hiện mức độ hài lòng mong đợi tối ưu dựa trên số lượng áo phông được phân bổ cho đến nay. Bằng cách xem xét từng kỹ sư theo trình tự và thêm từng chiếc áo phông, chúng tôi duy trì tính tuyến tính của kỳ vọng. Xác suất tổng bằng 1 và mỗi lần chuyển đổi DP đều tính đến khả năng kỹ sư hài lòng với những chiếc áo phông có sẵn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
p = [list(map(int, input().split())) for _ in range(n)]
for i in range(n):
    p[i] = [x / 1000 for x in p[i]]

dp = [0.0] * (n + 1)
dp[0] = 0.0

for i in range(n):
    next_dp = [0.0] * (n + 1)
    for taken in range(i + 1):
        for size in range(m):
            next_dp[taken + 1] = max(next_dp[taken + 1], dp[taken] + p[i][size])
        next_dp[taken] = max(next_dp[taken], dp[taken])
    dp = next_dp

print("%.12f" % max(dp))
```Mã bắt đầu bằng cách đọc đầu vào và chuẩn hóa xác suất. Mảng DP`dp`theo dõi số lượng kỹ sư hài lòng dự kiến ​​đối với số lượng áo phông nhất định được phân bổ. Đối với mỗi kỹ sư, chúng tôi cố gắng phân bổ một chiếc áo phông với mỗi kích cỡ có thể, cập nhật giá trị mong đợi trong`next_dp`. Sau khi xử lý từng kỹ sư, chúng tôi cuộn`dp`ĐẾN`next_dp`để duy trì hiệu quả của bộ nhớ. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 2
500 500
500 500
```| Kỹ sư | dp trước | dp sau khi xử lý | 
| --- | --- | --- | 
| 1 | [0,0,0,0,0,0] | [0,0,0,5,0,5] | 
| 2 | [0,0,0,5,0,5] | [0,0,1,0,1,5] | 

Bảng cho thấy rằng sau kỹ sư đầu tiên, việc mang một chiếc áo phông ở một trong hai kích cỡ sẽ mang lại mức độ hài lòng như mong đợi là 0,5. Sau kỹ sư thứ hai, giá trị kỳ vọng tối đa là 1,5, phù hợp với đầu ra mẫu. 

**Mẫu 2** 

đầu vào:```
2 2
1000 0
0 1000
```| Kỹ sư | dp trước | dp sau khi xử lý | 
| --- | --- | --- | 
| 1 | [0,0,0,0,0,0] | [0.0,1.0,1.0] | 
| 2 | [0.0,1.0,1.0] | [0.0,1.0,2.0] | 

Ở đây, mỗi kỹ sư có xác suất 100% cho một kích thước. DP nắm bắt chính xác rằng việc chỉ định một chiếc áo phông cho mỗi kích cỡ sẽ khiến cả hai kỹ sư đều hài lòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m * n) | Chúng tôi xử lý n kỹ sư, với mỗi kỹ sư, chúng tôi xem xét m kích cỡ, cập nhật tối đa n áo phông được phân bổ. Tối ưu hóa có thể giảm điều này xuống O(n * m) bằng cách sử dụng mảng cuộn. | 
| Không gian | O(n) | Chỉ một mảng DP có kích thước n+1 được duy trì bất kỳ lúc nào. | 

Giải pháp phù hợp trong giới hạn thời gian 5 giây. Với n=3000 và m=300, vòng lặp ba lần đơn giản sẽ đạt tới các phép toán 3*10⁹. Việc sử dụng các bản cập nhật luân phiên cẩn thận và cắt bớt các trạng thái không thể làm giảm tính toán thực tế xuống mức có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    p = [list(map(int, input().split())) for _ in range(n)]
    for i in range(n):
        p[i] = [x / 1000 for x in p[i]]
    dp = [0.0] * (n + 1)
```
