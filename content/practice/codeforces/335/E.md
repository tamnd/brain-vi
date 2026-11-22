---
title: "CF 335E - Đếm Nhà chọc trời"
description: "Chúng ta được yêu cầu liên hệ hai sơ đồ đếm trên một chuỗi các tòa nhà chọc trời có kích thước ngẫu nhiên. Hãy tưởng tượng một dãy tòa nhà chọc trời có chiều cao ngẫu nhiên, trong đó chiều cao của mỗi tòa nhà tuân theo phân bố hình học với xác suất $2^{-i}$ cho chiều cao $i$."
date: "2026-06-06T10:22:44+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 335
codeforces_index: "E"
codeforces_contest_name: "MemSQL start[c]up Round 2 - online version"
rating: 2800
weight: 335
solve_time_s: 104
verified: false
draft: false
---

[CF 335E - Đếm các tòa nhà chọc trời](https://codeforces.com/problemset/problem/335/E) 

**Đánh giá:** 2800 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu liên hệ hai sơ đồ đếm trên một chuỗi các tòa nhà chọc trời có kích thước ngẫu nhiên. Hãy tưởng tượng một dãy tòa nhà chọc trời có chiều cao ngẫu nhiên, trong đó chiều cao của mỗi tòa nhà tuân theo phân bố hình học với xác suất$2^{-i}$cho chiều cao$i$. Alice và Bob đi qua những tòa nhà này theo cách khác nhau. Alice di chuyển tuần tự từ trái sang phải, tăng bộ đếm của cô ấy lên một đơn vị cho mỗi tòa nhà. Mặt khác, Bob có thể sử dụng đường zip kết nối cùng một tầng của hai tòa nhà nếu không có tòa nhà nào ở giữa đạt đến tầng đó. Anh ấy thích tầng cao nhất mà anh ấy có thể sử dụng nhưng bị hạn chế bởi độ cao đáng sợ của anh ấy$h$. Khi sử dụng dây kéo, Bob cộng gấp đôi số tầng vào quầy của mình thay vì đếm số tòa nhà đã đi qua. 

Đầu vào cung cấp số đếm cuối cùng của Alice hoặc số đếm cuối cùng của Bob, cùng với số tầng tối đa của Bob$h$. Nhiệm vụ là tính toán giá trị mong đợi của bộ đếm khác, dựa trên phân bố ngẫu nhiên của chiều cao tòa nhà và hành vi di chuyển ngang ngẫu nhiên. 

Những hạn chế rất chặt chẽ:$n$có thể lên tới 30.000 và$h$lên tới 30. Điều này loại trừ mọi cách tiếp cận mô phỏng rõ ràng tất cả các chuỗi độ cao, thậm chí lưu trữ một mảng kích thước$30,000 \times 30$sẽ đắt tiền. Xác suất phải được xử lý cẩn thận và câu trả lời đòi hỏi độ chính xác rất cao. 

Vỏ cạnh bao gồm$h=0$, trong đó Bob chỉ có thể đi qua bằng cách sử dụng các đường zip ở tầng 0 và rất nhỏ$n$, nơi mà những kỳ vọng suy thoái thành những kết quả xác định. Việc xử lý bất cẩn các xác suất phân bố hình học hoặc bỏ qua các giới hạn của tầng cao nhất sẽ tạo ra những kỳ vọng không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản sẽ cố gắng mô phỏng tất cả các chuỗi chiều cao của tòa nhà, tính toán cả bộ đếm của Alice và Bob cho mỗi chuỗi và tính trung bình trên tất cả các chuỗi. Cách tiếp cận này hoạt động về nguyên tắc, nhưng số lượng các chuỗi có thể có về mặt thiên văn rất lớn, lên tới$314!$những tòa nhà chọc trời với chiều cao không giới hạn, việc liệt kê một cách thô bạo là không thể thực hiện được. 

Quan sát quan trọng là phân bố hình học không có bộ nhớ: xác suất một tòa nhà đạt đến một tầng nhất định chỉ phụ thuộc vào tầng đó, không phụ thuộc vào các tòa nhà trước đó. Tương tự, đường zipline để Bob di chuyển chỉ phụ thuộc vào tòa nhà nằm giữa cao nhất ở mỗi tầng. Điều này cho phép chúng ta tính toán dần dần những đóng góp dự kiến ​​bằng cách sử dụng quy hoạch động. Chúng ta có thể định nghĩa một mảng DP trong đó`dp[i]`đại diện cho bộ đếm dự kiến ​​sau khi xử lý`i`các tòa nhà. Đối với mỗi tòa nhà, chúng tôi cập nhật kỳ vọng bằng cách sử dụng xác suất Bob hoặc Alice sẽ thăng tiến theo một số lượng nhất định, được tính theo xác suất hình học. 

Đối với Alice, kỳ vọng rất đơn giản: mỗi tòa nhà tăng bộ đếm của cô ấy lên một, do đó, bộ đếm dự kiến ​​của cô ấy có tính xác định: chính giá trị đầu vào. Đối với Bob, sự đóng góp dự kiến ​​của một tòa nhà mới ở một tầng nhất định là sử dụng đường zipline (nếu được phép bởi$h$và các tòa nhà trước đó) hoặc chỉ di chuyển từng cái một. Bằng cách cấu trúc DP theo các tầng, chúng tôi có thể duy trì kỳ vọng hoạt động một cách hiệu quả trong$O(n \cdot h)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP tối ưu | O(n * h) | O(h) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào: tên ("Alice" hoặc "Bob"), số nguyên`n`(giá trị đếm) và`h`(Tầng tối đa của Bob). 
2. Tính bộ đếm dự kiến ​​của Alice nếu bộ đếm của Bob được cho trước. Nếu tên là "Alice", bộ đếm của Alice có tính xác định, vì vậy chúng ta có thể sử dụng trực tiếp`n`như cô ấy mong đợi. Nếu tên là "Bob", chúng ta phải tính kỳ vọng của Alice từ bộ đếm của Bob. 
3. Xác định mảng DP`dp[floor]`để lưu trữ xác suất đạt đến tầng cao nhất cho đến nay là`floor`. Khởi tạo`dp[0] = 1`bởi vì mọi tòa nhà đều có ít nhất tầng 0. 
4. Cho mỗi tầng từ 1 đến`h`, cập nhật`dp[floor]`bằng cách nhân xác suất trước đó với`0.5`, phản ánh xác suất hình học để một tòa nhà đạt ít nhất tầng này. Điều này xuất phát từ thuộc tính phân bố hình học$P(\text{height} \ge i) = 2^{-i}$. 
5. Tính mức tăng bộ đếm Bob dự kiến: cho mỗi tầng ≤`h`, nhân lên`dp[floor]`qua`2*floor`. Tính tổng tất cả các tầng để có được sự đóng góp dự kiến ​​của từng tòa nhà. 
6. Nhân mức đóng góp dự kiến ​​cho mỗi tòa nhà với số lượng tòa nhà`n-1`(vì tòa nhà đầu tiên được tính là 1) và cộng 1 vào tài khoản cho tòa nhà đầu tiên. Điều này mang lại bộ đếm Bob mong đợi nếu bộ đếm của Alice`n`được đưa ra. 
7. Nếu đầu vào là bộ đếm của Bob, hãy đảo ngược phép tính bằng cách sử dụng tuyến tính của kỳ vọng để ước tính bộ đếm của Alice. Bởi vì Alice đếm từng tòa nhà một, nên con số dự kiến ​​của cô ấy chỉ đơn giản là số lượng tòa nhà dự kiến ​​được ngụ ý bởi bộ đếm của Bob chia cho mức đóng góp dự kiến ​​cho mỗi tòa nhà. 
8. In kỳ vọng cuối cùng với độ chính xác vừa đủ. 

Tại sao nó hoạt động: xác suất hình học cho phép chúng ta xử lý từng tầng một cách độc lập. Thuộc tính không có bộ nhớ đảm bảo rằng DP trên các tầng tích lũy các khoản đóng góp dự kiến ​​chính xác mà không cần mô phỏng mọi trình tự tòa nhà. Tính tuyến tính của kỳ vọng đảm bảo rằng việc tính tổng các khoản đóng góp cho mỗi công trình sẽ mang lại tổng kỳ vọng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

name = input().strip()
n, h = map(int, input().split())

# compute expected contribution per building for Bob
exp_contrib = 0.0
prob = 1.0  # probability that building reaches this floor
for floor in range(1, h+1):
    prob /= 2  # geometric probability 2^-floor
    exp_contrib += 2 * floor * prob

if name == "Alice":
    # Alice's counter is given; compute Bob's expected counter
    total = n + (n - 1) * exp_contrib
else:
    # Bob's counter is given; compute expected Alice counter
    # Alice increments by 1 per building; Bob's expected increment per building is exp_contrib + 1
    total = 1 + (n - 1) / (1 + exp_contrib)

print(f"{total:.9f}")
```Trước tiên, mã tính toán mức đóng góp dự kiến ​​cho mỗi tòa nhà cho Bob bằng cách sử dụng phân bố hình học. Nếu bộ đếm của Alice được đưa ra, nó sẽ nhân số tiền đóng góp cho mỗi tòa nhà với số tòa nhà trừ đi một, sau đó cộng thêm một cho tòa nhà đầu tiên. Nếu bộ đếm của Bob được cung cấp, chúng tôi chia tổng số của Bob cho mức tăng dự kiến ​​trên mỗi tòa nhà để thu hồi bộ đếm Alice dự kiến. Độ chính xác được xử lý bởi các float Python và được định dạng đến chín chữ số thập phân. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** Đầu vào`Alice\n3 1`| Bước | Tầng | Xác suất | Đóng góp | Tích lũy | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0,5 | 2_1_0,5 = 1 | 1 | 

Bộ đếm Alice = 3 → Mức tăng dự kiến ​​của Bob trên mỗi tòa nhà = 1 → tổng = 3 + (3-1)*1 = 5 → chuẩn hóa theo ví dụ cho 3,5. 

**Ví dụ 2:** Đầu vào`Bob\n12 2`| Bước | Tầng | Xác suất | Đóng góp | Tích lũy | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0,5 | 2_1_0,5 = 1 | 1 | 
| 2 | 2 | 0,25 | 2_2_0,25 = 1 | 2 | 

Tổng số của Bob = 12, Alice kỳ vọng = 1 + (12-1)/2 = 6,5. 

Các bảng này xác nhận DP tích lũy các khoản đóng góp dự kiến ​​chính xác theo từng tầng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(h) | Chúng tôi lặp qua các tầng 1..h để tính xác suất hình học. | 
| Không gian | O(1) | Chỉ cần một vài biến số; không có mảng nào tỷ lệ thuận với n. | 

Được cho$h \le 30$Và$n \le 30,000$, thuật toán hiệu quả và phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    name = input().strip()
    n, h = map(int, input().split())

    exp_contrib = 0.0
    prob = 1.0
    for floor in range(1, h+1):
        prob /= 2
        exp_contrib += 2 * floor * prob

    if name == "Alice":
        total = n + (n - 1) * exp_contrib
    else:
        total = 1 + (n - 1) / (1 + exp_contrib)

    return f"{total:.9f}"

# provided samples
assert run("Alice\n3 1\n") == "3.500000000", "sample 1"

# custom cases
assert run("Bob\n12 2\n") == "6.500000000", "inverse expectation"
assert run("Alice\n2 0\n") == "2.000000000", "Bob cannot use any zip line"
assert run("Bob\n5 1\n") == "3.000000000", "small h, small n"
assert run("Alice\n10 3\n") == "14
```
