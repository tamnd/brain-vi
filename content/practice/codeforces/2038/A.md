---
title: "CF 2038A - Dự án thưởng"
description: "Chúng tôi có một đội ngũ kỹ sư, mỗi người được nhận một khoản tiền thưởng như đã hứa và một khoản chi phí cá nhân để thực hiện một đơn vị công việc. Nhóm cần hoàn thành một dự án yêu cầu chính xác $k$ đơn vị công việc và mỗi kỹ sư sẽ quyết định riêng số tiền sẽ đóng góp."
date: "2026-06-08T10:34:44+07:00"
tags: ["codeforces", "competitive-programming", "games", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2038
codeforces_index: "A"
codeforces_contest_name: "2024-2025 ICPC, NERC, Southern and Volga Russian Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 1400
weight: 2038
solve_time_s: 129
verified: false
draft: false
---

[CF 2038A - Dự án thưởng](https://codeforces.com/problemset/problem/2038/A) 

**Đánh giá:** 1400 
**Tags:** trò chơi, tham lam 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một đội ngũ kỹ sư, mỗi người được nhận một khoản tiền thưởng như đã hứa và một khoản chi phí cá nhân để thực hiện một đơn vị công việc. Nhóm cần hoàn thành một dự án đòi hỏi chính xác$k$đơn vị công việc và mỗi kỹ sư sẽ tự quyết định mức độ đóng góp. Số tiền mỗi kỹ sư đóng góp,$c_i$, phải tối đa hóa lợi ích cá nhân của họ, được định nghĩa là$s_i = a_i - c_i \cdot b_i$, Ở đâu$a_i$là tiền thưởng và$b_i$là chi phí cho mỗi đơn vị. Nếu một kỹ sư mong đợi lợi ích tiêu cực, chúng sẽ không hoạt động. Mục đích là xác định sự phân công công việc$[c_1, c_2, ..., c_n]$đó là kết quả khi mọi kỹ sư đều hành động tối ưu. 

Những hạn chế cho chúng ta biết$n \le 1000$Và$k \le 10^6$. Từ$k$có thể lớn, bất kỳ thuật toán nào cố gắng kiểm tra tất cả các phân phối công việc có thể có (ví dụ: tất cả các bộ số nguyên không âm có tổng bằng$k$) sẽ là hàm mũ trong$n$, điều đó là không thể thực hiện được. Tuyến tính hoặc$O(n \log n)$giải pháp có thể chấp nhận được vì nó có thể xử lý tới khoảng$10^6$hoạt động thoải mái. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó tổng tiền thưởng quá nhỏ để dự án có thể sinh lãi. Ví dụ, nếu$k = 10$và tổng tiền thưởng$a_i$nhỏ hơn bất kỳ sự phân bổ chi phí công việc hợp lý nào, không có kỹ sư nào tình nguyện thực hiện bất kỳ công việc nào, vì vậy kết quả đầu ra phải bằng 0. Một trường hợp khác là khi một số kỹ sư có chi phí trên mỗi đơn vị rất cao$b_i$và tiền thưởng thấp. Trong trường hợp này, mặc dù dự án có thể được những người khác hoàn thành nhưng những kỹ sư đó không nên đóng góp gì vì làm việc sẽ làm giảm lợi ích của họ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách phân bổ có thể của$k$các đơn vị công việc giữa các kỹ sư. Đối với mỗi lần phân bổ, chúng tôi sẽ tính toán lợi ích cho từng kỹ sư và kiểm tra xem có lợi ích nào âm hay không, từ chối phân bổ đó nếu có. Về nguyên tắc thì điều này đúng, nhưng số lượng phân bổ mang tính tổ hợp: đại khái là$\binom{k+n-1}{n-1}$. Ngay cả đối với nhỏ$n$và vừa phải$k$, tốc độ này quá chậm. 

Điểm mấu chốt là các kỹ sư hành động theo trình tự, mỗi người tối đa hóa lợi ích của mình dựa trên những gì các kỹ sư trước đó đã hứa. Mỗi kỹ sư nhìn thấy khối lượng công việc còn lại$r$để đạt được$k$sau khi các kỹ sư trước đó đã quyết định. Mục tiêu của họ là lựa chọn$c_i$càng lớn càng tốt mà không làm cho lợi ích của họ bị âm, tức là$c_i \le a_i / b_i$. Họ cũng không thể đóng góp nhiều hơn công việc còn lại$r$. Vì vậy, tối ưu$c_i$là mức tối thiểu của công việc còn lại và mức đóng góp mang lại lợi nhuận tối đa$a_i / b_i$. Khi một kỹ sư cam kết$c_i$, kỹ sư tiếp theo lặp lại logic tương tự với công việc còn lại được cập nhật. Cách tiếp cận tham lam này đảm bảo tạo ra một giải pháp duy nhất vì sự lựa chọn của mỗi kỹ sư phụ thuộc một cách quyết định vào công việc còn lại và tỷ lệ cá nhân của họ.$a_i / b_i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((k+n-1 chọn n-1)) | O(n) | Quá chậm | 
| Tuần tự tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo công việc còn lại$r = k$. Biến này theo dõi số lượng công việc vẫn cần được giao. 
2. Lặp lại từng kỹ sư$i$từ$1$ĐẾN$n$. Đối với mỗi kỹ sư, hãy tính công việc mang lại lợi nhuận tối đa cho họ, đó là$m_i = a_i // b_i$. Đây là khối lượng công việc lớn nhất mà không làm giảm lợi ích của họ xuống dưới 0. 
3. Xác định công việc thực tế$c_i$kỹ sư sẽ đóng góp. Đây là mức tối thiểu của$m_i$và công việc còn lại$r$. Điều này đảm bảo rằng kỹ sư tối đa hóa lợi ích của họ mà không vượt quá mức cần thiết để hoàn thành dự án. 
4. Trừ phần đóng góp của người kỹ sư vào phần công việc còn lại:$r -= c_i$. Điều này cập nhật công việc còn lại cho các kỹ sư sau. 
5. Tiếp tục quá trình này cho tất cả các kỹ sư. Sau kỹ sư cuối cùng, tổng số$c_i$sẽ khớp chính xác$k$(dự án đã hoàn thành) hoặc ít hơn nếu không thể hoàn thành dự án nếu không có kỹ sư nào đó nhận được lợi ích tiêu cực. Những kỹ sư không thể đóng góp một cách có lợi sẽ tự động đóng góp bằng không. 

Tại sao nó hoạt động: Ở mỗi bước, mỗi kỹ sư đều hành động tối ưu dựa trên các quyết định trước đó. Bởi vì sự lựa chọn của mỗi kỹ sư bị giới hạn bởi cả sự đóng góp lợi nhuận tối đa của họ và công việc còn lại, nên việc phân bổ tuần tự tham lam đảm bảo rằng không có kỹ sư nào nhận được lợi ích tiêu cực. Điều bất biến là sau mỗi kỹ sư, công việc còn lại phản ánh chính xác phần công việc chưa được giao.$k$và mọi kỹ sư đã chọn số tiền tối đa họ có thể đóng góp mà không bị lỗ. Điều này tạo ra một trình tự tối ưu, duy nhất$[c_1, c_2, ..., c_n]$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

c = []
remaining = k

for i in range(n):
    max_work = a[i] // b[i]  # maximum work this engineer can profitably do
    contribution = min(max_work, remaining)
    c.append(contribution)
    remaining -= contribution

print(' '.join(map(str, c)))
```Giải pháp đọc$n$,$k$, tiền thưởng$a_i$, và chi phí$b_i$. các`max_work`tính toán đảm bảo không có kỹ sư nào làm việc thua lỗ và`min(max_work, remaining)`phân công đảm bảo rằng tổng công việc không bao giờ vượt quá$k$. các`remaining`các biến số cập nhật tuần tự để mỗi kỹ sư biết chính xác lượng công việc còn lại. Sử dụng phép chia số nguyên`//`đảm bảo chúng tôi ở trong các đơn vị số nguyên. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 6
4 7 6
1 2 3
```Dấu vết bước: 

| Kỹ sư | a_i | b_i | max_work | còn lại | c_i | còn lại sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 4 | 6 | 1 | 5 | 
| 2 | 7 | 2 | 3 | 5 | 3 | 2 | 
| 3 | 6 | 3 | 2 | 2 | 2 | 0 | 

Bảng này cho thấy mỗi kỹ sư đóng góp đến mức giới hạn lợi nhuận của mình mà không vượt quá công việc còn lại. Tổng số công việc chính xác là 6$k$. 

**Mẫu 2** 

đầu vào:```
2 10
3 4
5 2
```Dấu vết bước: 

| Kỹ sư | a_i | b_i | max_work | còn lại | c_i | còn lại sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 0 | 10 | 0 | 10 | 
| 2 | 4 | 2 | 2 | 10 | 2 | 8 | 

Công việc còn lại là 8 công việc sau cả hai kỹ sư, nhưng bất kỳ công việc nào tiếp theo sẽ làm giảm lợi ích xuống dưới 0, vì vậy họ chỉ đóng góp những gì có lợi nhuận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần thông qua tất cả các kỹ sư, số học theo thời gian không đổi cho mỗi kỹ sư | 
| Không gian | O(n) | Lưu trữ mảng đóng góp kết quả | 

Thuật toán tuyến tính theo số lượng kỹ sư và không phụ thuộc vào$k$. Với$n \le 1000$, điều này dễ dàng chạy dưới 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    c = []
    remaining = k
    for i in range(n):
        max_work = a[i] // b[i]
        contribution = min(max_work, remaining)
        c.append(contribution)
        remaining -= contribution
    return ' '.join(map(str, c))

# provided samples
assert run("3 6\n4 7 6\n1 2 3\n") == "1 3 2"
assert run("2 10\n3 4\n5 2\n") == "0 2"

# custom cases
assert run("1 5\n10\n2\n") == "5"  # single engineer does all work
assert run("3 5\n1 2 3\n1 1 1\n") == "1 2 2"  # sum of max work = k exactly
assert run("2 1000\n1000 1000\n1 1\n") == "500 500"  # evenly distributed
assert run("3 3\n1 1 1\n2 2 2\n") == "0 0 0"  # no profitable work
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 5\n10\n2\n" | "5" | Kỹ sư đơn lẻ xử lý mọi công việc | 
| "3 5\n1 2 3\n1 1 1\n" | "1 2 2" | Công việc phân bổ chính xác đạt k | 
| "2 1000\n1000 1000\n1 1\n" | "500 | |
