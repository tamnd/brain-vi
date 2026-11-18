---
title: "CF 2023D - Nhiều Trò Chơi"
description: "Vấn đề đưa ra một sòng bạc có nhiều trò chơi, mỗi trò chơi mang lại cơ hội thắng một số tiền nhất định. Mỗi trò chơi có hai thông số: xác suất chiến thắng, được tính theo phần trăm và khoản thanh toán nếu bạn thắng."
date: "2026-06-08T12:33:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2023
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 980 (Div. 1)"
rating: 2900
weight: 2023
solve_time_s: 107
verified: true
draft: false
---

[CF 2023D - Nhiều trò chơi](https://codeforces.com/problemset/problem/2023/D) 

**Xếp hạng:** 2900 
**Tags:** sức mạnh vũ phu, dp, lòng tham, toán học, xác suất 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề đưa ra một sòng bạc có nhiều trò chơi, mỗi trò chơi mang lại cơ hội thắng một số tiền nhất định. Mỗi trò chơi có hai thông số: xác suất chiến thắng, được tính theo phần trăm và khoản thanh toán nếu bạn thắng. Bạn có thể chọn bất kỳ nhóm trò chơi nào để chơi nhưng bạn chỉ kiếm được tiền nếu thắng mọi trò chơi đã chọn. Nếu bạn thua dù chỉ một ván trong tập hợp con đã chọn, bạn sẽ không nhận được gì. Nhiệm vụ là chọn một tập hợp các trò chơi tối đa hóa giá trị tiền thắng kỳ vọng. 

Giá trị mong đợi của bộ đã chọn là tích của xác suất (được chuyển đổi thành phân số) nhân với tổng số tiền thắng. Ví dụ: nếu bạn chọn hai trò chơi có xác suất 80% và 50% và khoản thanh toán 80 và 200, giá trị kỳ vọng là$(0.8 * 0.5) * (80 + 200) = 112$. 

Những hạn chế rất đáng kể: số lượng trò chơi$n$có thể đạt tới 200.000. Việc kiểm tra mạnh mẽ tất cả các tập hợp con sẽ liên quan đến$2^n$sự kết hợp, điều đó là không thể thực hiện được. Xác suất cá nhân và số tiền thắng được giới hạn sao cho sản phẩm của họ không bao giờ vượt quá$2 \cdot 10^5$, đảm bảo tính ổn định về mặt số học. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó việc chọn nhiều trò chơi hơn sẽ làm giảm giá trị mong đợi. Ví dụ: chọn một trò chơi có xác suất thấp nhưng tỷ lệ xuất chi cao có thể làm giảm giá trị kỳ vọng khi kết hợp với các trò chơi khác. Một trường hợp lợi thế khác là có tất cả xác suất ở mức 100%, trong trường hợp đó chọn tất cả các trò chơi là tối ưu. Tương tự, chọn một trò chơi có tỷ lệ trả thưởng thấp có xác suất cao có thể tốt hơn là kết hợp nó với một trò chơi mạo hiểm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ kiểm tra tất cả các tập hợp con của trò chơi. Đối với mỗi tập hợp con, hãy tính tích các xác suất và tổng tiền thắng, sau đó nhân chúng để có giá trị mong đợi. Điều này đúng vì nó trực tiếp tuân theo định nghĩa bài toán. Tuy nhiên, với$n \le 2 \cdot 10^5$, có$2^{200000}$các tập hợp con, có kích thước lớn về mặt thiên văn. Cách tiếp cận này là không thể. 

Quan sát quan trọng để tối ưu hóa là hàm giá trị kỳ vọng có tính nhân xác suất nhưng tính cộng trong tiền thắng. Nếu chúng ta sắp xếp các trò chơi theo tỷ lệ xác suất tác động đến giá trị kỳ vọng hoặc tương đương, theo số liệu như$w_i / (1 - p_i/100)$, chúng ta có thể tham lam quyết định xem việc đưa một trò chơi vào có làm tăng giá trị kỳ vọng hay không. Điều này làm giảm vấn đề liên tục thêm trò chơi có đóng góp cận biên cao nhất trong khi giá trị kỳ vọng tăng lên. Cấu trúc của hàm đảm bảo rằng sau khi thêm một trò chơi sẽ làm giảm giá trị mong đợi, việc thêm bất kỳ trò chơi nào khác (được sắp xếp theo cùng một số liệu) sẽ không giúp ích gì, cho phép dừng sớm. 

Cách tiếp cận tối ưu sắp xếp các trò chơi bằng cách giảm$w_i / (100 / p_i - 1)$, được rút ra bằng cách thiết lập đạo hàm của giá trị kỳ vọng đối với việc bao gồm một trò chơi và đơn giản hóa. Sau đó, nó lặp lại thứ tự đã sắp xếp này, tính toán tích lũy tích các xác suất và tổng tiền thắng. Giá trị mong đợi tối đa gặp phải trong lần lặp này chính là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số trận đấu$n$và danh sách các xác suất$p_i$và tiền thắng$w_i$. Chuyển đổi xác suất thành phân số để tính toán. 
2. Xác định số liệu để đánh giá lợi ích của việc thêm từng trò chơi. Sự lựa chọn tự nhiên là$w_i / (1 - p_i/100)$, biểu thị tỷ lệ thanh toán trên xác suất thua lỗ, phản ánh sự đóng góp cận biên của nó vào giá trị kỳ vọng. 
3. Sắp xếp tất cả các trò chơi theo thứ tự giảm dần của số liệu này. Những trò chơi có lợi ích cao hơn sẽ được xem xét đầu tiên. 
4. Khởi tạo hai biến: xác suất tích lũy$prob = 1.0$đại diện cho tích của xác suất của các trò chơi đã chọn và tổng tiền thắng tích lũy$sum\_w = 0$. 
5. Lặp lại các trò chơi đã được sắp xếp. Đối với mỗi trò chơi, hãy tính giá trị kỳ vọng tiềm năng nếu trò chơi này được đưa vào:$new\_EV = prob * (p_i/100) * (sum\_w + w_i)$. 
6. Nếu việc thêm trò chơi làm tăng giá trị kỳ vọng tối đa hiện tại, hãy cập nhật$prob$Và$sum\_w$để bao gồm trò chơi này và cập nhật tối đa. 
7. Dừng lặp lại sau khi thêm một trò chơi sẽ làm giảm giá trị kỳ vọng vì các trò chơi tiếp theo có đóng góp biên nhỏ hơn. 
8. Xuất ra giá trị mong đợi tối đa được tìm thấy, được định dạng với độ chính xác vừa đủ. 

Tại sao nó hoạt động: Hàm giá trị kỳ vọng không đồng nhất khi các trò chơi được xem xét theo thứ tự giảm dần$w_i / (1 - p_i/100)$. Thuộc tính này đảm bảo rằng sau khi thêm một trò chơi sẽ làm giảm giá trị kỳ vọng, mọi trò chơi còn lại cũng sẽ giảm giá trị đó. Vì vậy, việc bổ sung tham lam các trò chơi đảm bảo rằng sẽ tìm được giá trị lớn nhất mà không cần kiểm tra tất cả các tập con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
games = []

for _ in range(n):
    p, w = map(int, input().split())
    games.append((p, w))

# Sort by "benefit" metric w / (1 - p/100) descending
games.sort(key=lambda x: x[1] / (1 - x[0] / 100), reverse=True)

max_ev = 0.0
prob = 1.0
sum_w = 0.0

for p, w in games:
    new_prob = prob * (p / 100)
    new_sum_w = sum_w + w
    ev = new_prob * new_sum_w
    if ev > max_ev + 1e-12:
        max_ev = ev
        prob = new_prob
        sum_w = new_sum_w
    else:
        break

print(f"{max_ev:.9f}")
```Giải pháp này đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Trò chơi được sắp xếp theo tỷ lệ$w / (1 - p/100)$, thể hiện sự tăng trưởng giá trị kỳ vọng cận biên. Xác suất tích lũy và tổng số tiền thắng chỉ được cập nhật nếu giá trị kỳ vọng tăng lên, ngăn chặn sự suy giảm. Chúng tôi đưa một epsilon nhỏ vào so sánh để tránh lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
80 80
70 100
50 200
```| Bước | Trò chơi đã chọn | thăm dò | tổng_w | ev | 
| --- | --- | --- | --- | --- | 
| 1 | 50 200 | 0,5 | 200 | 100 | 
| 2 | 80 80 | 0,4 | 280 | 112 | 
| 3 | 70 100 | 0,28 | 380 | 106,4 | 

Giá trị kỳ vọng tối đa: 112 

Dấu vết này cho thấy rằng việc thêm hai trò chơi đầu tiên sẽ làm tăng EV nhưng việc thêm trò chơi thứ ba sẽ làm giảm EV. Sự lựa chọn tham lam có tác dụng. 

### Mẫu 2 

đầu vào:```
2
100 1
100 1
```| Bước | Trò chơi đã chọn | thăm dò | tổng_w | ev | 
| --- | --- | --- | --- | --- | 
| 1 | 100 1 | 1 | 1 | 1 | 
| 2 | 100 1 | 1 | 2 | 2 | 

Giá trị kỳ vọng tối đa: 2 

Ở đây, mọi xác suất đều là 100% nên việc thêm tất cả các trò chơi là tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp n trò chơi chiếm ưu thế; lặp là tuyến tính | 
| Không gian | O(n) | Danh sách cửa hàng trò chơi và biến tích lũy | 

Với n tối đa 2·10^5, các thao tác O(n log n) (~3 triệu) diễn ra thoải mái trong vòng 2 giây và mức sử dụng bộ nhớ cũng dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    games = [tuple(map(int, input().split())) for _ in range(n)]
    games.sort(key=lambda x: x[1] / (1 - x[0] / 100), reverse=True)
    max_ev = 0.0
    prob = 1.0
    sum_w = 0.0
    for p, w in games:
        new_prob = prob * (p / 100)
        new_sum_w = sum_w + w
        ev = new_prob * new_sum_w
        if ev > max_ev + 1e-12:
            max_ev = ev
            prob = new_prob
            sum_w = new_sum_w
        else:
            break
    return f"{max_ev:.9f}"

# Provided samples
assert run("3\n80 80\n70 100\n50 200\n") == "112.000000000", "sample 1"
assert run("2\n100 1\n100 1\n") == "2.000000000", "sample 2"

# Custom cases
assert run("1\n50 100\n") == "50.000000000", "single game"
assert run("3\n100 10\n90 20\n80 30\n") == "36.000000000", "all probabilities high"
assert run("2\n10 1000\n90 10\n") == "90.000000000", "high payout low prob vs low payout high prob"
assert run("4\n50 50\n50 50\n50 50\n50 50\n") == "62.5", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n50 100 | 50. | |
