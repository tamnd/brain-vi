---
title: "CF 104835B - Cắt Baklava"
description: "Chúng ta bắt đầu với một chiếc bánh ngọt hình vuông có cạnh dài l. Mila liên tục thực hiện một phép toán hình học thay thế hình vuông hiện tại bằng một hình vuông nhỏ hơn được hình thành bằng cách nối các điểm giữa một cách đối xứng."
date: "2026-06-28T11:45:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104835
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 12-01-23 Div. 2 (Beginner)"
rating: 0
weight: 104835
solve_time_s: 75
verified: false
draft: false
---

[CF 104835B - Cắt Baklava](https://codeforces.com/problemset/problem/104835/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một chiếc bánh hình vuông có chiều dài cạnh`l`. Mila liên tục thực hiện một phép toán hình học thay thế hình vuông hiện tại bằng một hình vuông nhỏ hơn được hình thành bằng cách nối các điểm giữa một cách đối xứng. Mỗi vòng tạo ra một hình vuông mới bên trong hình vuông trước đó và quá trình này được lặp lại chính xác.`k`lần. Nhiệm vụ là xác định “kích thước” dựa trên cạnh của hình vuông bên trong cuối cùng sau tất cả các lần cắt, trong đó các mẫu cho thấy kết quả đầu ra dự kiến ​​tương ứng với diện tích của hình vuông cuối cùng đó. 

Đầu vào là một chiều dài cạnh ban đầu và một số phép biến đổi hình học lặp đi lặp lại. Đầu ra là một số thực, do đó phép biến đổi phải tạo ra biểu thức dạng đóng thay vì mô phỏng. Những ràng buộc cho phép`l`lên tới`10^9`, điều này làm cho việc xây dựng hình học trực tiếp hoặc mô phỏng tọa độ không cần thiết nhưng cũng có rủi ro do lệch dấu phẩy động nếu chúng ta thử nó. Số vòng`k`nhiều nhất là 25, điều này gợi ý rằng mô hình chia tỷ lệ theo cấp số nhân hoặc lặp lại được dự định thay vì bùng nổ tổ hợp. 

Một cách tiếp cận đơn giản sẽ mô phỏng hình học: xây dựng tọa độ của hình vuông, tính điểm giữa, tạo thành hình vuông tiếp theo và lặp lại. Điều này ổn định cho nhỏ`k`, nhưng nó giới thiệu các căn bậc hai dấu phẩy động lặp đi lặp lại và cập nhật tọa độ. Mặc dù`k ≤ 25`nhỏ, lý luận hình học là quá mức cần thiết và làm tăng nguy cơ tích lũy lỗi chính xác. 

Một trường hợp phức tạp là tính không ổn định của dấu phẩy động nếu người ta cố gắng tính toán hình học nhiều lần bằng cách sử dụng lượng giác hoặc căn bậc hai trên mỗi bước. Ví dụ: việc tính toán lại tọa độ liên tục của một hình vuông được xoay có thể tạo ra độ lệch sao cho diện tích cuối cùng hơi lệch so với mức cho phép.`1e-6`sức chịu đựng. Điều này làm cho một công thức trực tiếp trở nên cần thiết. 

## Phương pháp tiếp cận 

Điều quan trọng là mỗi thao tác đều có cấu trúc giống hệt nhau: từ một hình vuông, chúng ta nối trung điểm của các cạnh để tạo thành một hình vuông mới bên trong. Phép biến đổi này là bất biến về tỷ lệ, nghĩa là tỷ lệ giữa chiều dài cạnh của hình vuông mới và cạnh cũ không đổi bất kể`l`. 

Để hiểu điều này, hãy xem xét một hình vuông có tâm ở gốc tọa độ với cạnh`l`. Trung điểm các cạnh của nó cách nhau`l/2`từ tâm dọc theo các trục. Nối các điểm giữa liền kề tạo thành một hình vuông mới quay 45 độ. Độ dài cạnh của hình vuông mới này là khoảng cách giữa hai trung điểm liên tiếp, ví dụ từ`(l/2, 0)`ĐẾN`(0, l/2)`. Khoảng cách đó là:$$\sqrt{(l/2)^2 + (l/2)^2} = \frac{l}{\sqrt{2}}$$Vì vậy, mỗi thao tác nhân độ dài cạnh với`1/√2`. 

Sau đó`k`hoạt động, độ dài cạnh trở thành:$$l \cdot \left(\frac{1}{\sqrt{2}}\right)^k = \frac{l}{2^{k/2}}$$Bài toán yêu cầu “kích thước” cuối cùng và các mẫu xác nhận đó là diện tích của hình vuông thu được. Vì vậy, chúng ta bình phương cạnh:$$\text{area}_k = \left(\frac{l}{2^{k/2}}\right)^2 = \frac{l^2}{2^k}$$Điều này làm giảm toàn bộ vấn đề để tính toán một lũy thừa duy nhất. 

Một mô phỏng hình học mạnh mẽ sẽ tính toán lại tọa độ mỗi vòng trong thời gian không đổi, đưa ra`O(k)`hoạt động, nhưng nó vẫn ẩn giấu sự mất ổn định của dấu phẩy động. Giải pháp dạng đóng tránh hoàn toàn việc lặp lại và chính xác trong phạm vi độ chính xác của dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng hình học Brute Force | O(k) | O(1) | Rủi ro do độ chính xác | 
| Biểu mẫu đóng tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị`l`Và`k`. Chúng xác định bình phương ban đầu và số lần áp dụng phép biến đổi cắt điểm giữa. 
2. Tính diện tích hình vuông ban đầu là`l * l`. Điều này thể hiện đường cơ sở bất biến trước khi xảy ra bất kỳ sự thu hẹp nào. 
3. Tính hệ số tỷ lệ được đưa ra bởi mỗi phép biến đổi. Mỗi vòng chia đôi diện tích hình vuông theo nghĩa nhân, tương ứng với việc chia cho`2`mỗi bước. 
4. Áp dụng phép biến đổi`k`lần bằng cách chia diện tích ban đầu cho`2^k`. Điều này tổng hợp tất cả độ co hình học thành một số mũ duy nhất. 
5. Xuất giá trị kết quả dưới dạng số dấu phẩy động. 

### Tại sao nó hoạt động 

Việc chuyển đổi diễn ra tương tự: mỗi bước ánh xạ một hình vuông sang một hình vuông khác chỉ sử dụng các điểm giữa, giúp giữ nguyên hình dạng và chỉ thay đổi tỷ lệ. Vì tỷ lệ giữa các khu vực liên tiếp là không đổi và không phụ thuộc vào vị trí hoặc kích thước nên quá trình tạo thành một cấp số nhân. Điều bất biến là sau mỗi bước diện tích bằng một nửa diện tích trước đó, nên sau`k`bước thì diện tích phải giảm đi một hệ số`2^k`. Không có sự biến dạng hoặc hướng hình học nào ảnh hưởng đến tỷ lệ này, do đó dạng đóng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

l, k = map(int, input().split())

area = (l * l) / (2 ** k)

print(area)
```Việc tính toán mã hóa trực tiếp công thức dẫn xuất. phép nhân`l * l`được thực hiện trước khi chia để đảm bảo độ chính xác. Sự lũy thừa`2 ** k`là an toàn bởi vì`k ≤ 25`, do đó giá trị vẫn nhỏ và chính xác trong biểu diễn dấu phẩy động. 

Chi tiết triển khai quan trọng là tránh hoàn toàn các cập nhật hình học lặp đi lặp lại. Mặc dù vấn đề mô tả các vết cắt lặp đi lặp lại, giải pháp vẫn dựa vào việc nhận biết hệ số tỷ lệ không đổi thay vì mô phỏng nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
```Sau mỗi bước, chúng tôi theo dõi khu vực: 

| Bước | Bên hiện tại | Khu vực hiện tại | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | 2 | 4 | hình vuông ban đầu | 
| 1 | 2 / √2 | 2 | chia diện tích cho 2 | 

Diện tích cuối cùng là`2`. 

Điều này xác nhận rằng một phép biến đổi đơn lẻ sẽ giảm chính xác một nửa diện tích, khớp với công thức dẫn xuất. 

### Ví dụ 2 

đầu vào:```
10 25
```| Bước | Khu vực hiện tại | Hoạt động | 
| --- | --- | --- | 
| 0 | 100 | ban đầu | 
| 25 | 100/2^25 | giảm một nửa lặp đi lặp lại | 

Giá trị cuối cùng:$$100 / 33554432 = 2.98023223876953 \times 10^{-6}$$Giá trị này phù hợp với giá trị cực kỳ nhỏ được mong đợi và chứng tỏ sự thu nhỏ hình học lặp đi lặp lại sẽ thu hẹp diện tích một cách nhanh chóng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học và một phép lũy thừa | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ | 

Giải pháp là thời gian không đổi bất kể`k`Và`l`, dễ dàng nằm trong giới hạn. Kể cả nếu`k`lớn hơn nhiều, các số nguyên lớn của Python xử lý lũy thừa một cách hiệu quả và phép chia dấu phẩy động vẫn không đổi theo thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    l, k = map(int, input().split())
    area = (l * l) / (2 ** k)
    return str(area)

# provided samples
assert abs(float(run("2 1")) - 2.0) < 1e-9
assert abs(float(run("10 25")) - 0.00000298023223876953) < 1e-18

# custom cases
assert abs(float(run("1 0")) - 1.0) < 1e-9
assert abs(float(run("1 1")) - 0.5) < 1e-9
assert abs(float(run("1000000000 1")) - 5e17) < 1e-1
assert abs(float(run("3 5")) - (9 / 32)) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | không biến đổi | 
| 1 1 | 0,5 | giảm một nửa | 
| 1000000000 1 | ổn định quy mô lớn | tránh vấn đề tràn | 
| 3 5 | 32/9 | tính đúng đắn chung | 

## Vỏ cạnh 

cho`k = 0`, phép biến đổi không bao giờ xảy ra và đầu ra phải bằng diện tích ban đầu`l^2`. Công thức xử lý việc này một cách tự nhiên vì`2^0 = 1`, vì vậy không cần phân nhánh đặc biệt. 

Đối với lớn`k`chẳng hạn như`k = 25`, giá trị trở nên cực kỳ nhỏ nhưng vẫn cao hơn nhiều so với giới hạn tràn dấu phẩy động trong Python. Việc phân chia trực tiếp đảm bảo không tích lũy tổn thất chính xác lặp đi lặp lại. 

Đối với lớn`l`gần`10^9`, giá trị bình phương đạt`10^18`, có thể biểu diễn một cách an toàn ở định dạng dấu phẩy động với độ chính xác đủ cho khả năng chịu lỗi cần thiết.
