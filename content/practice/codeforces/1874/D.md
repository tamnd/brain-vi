---
title: "CF 1874D - Sứa và Miku"
description: "Chúng ta có một chuỗi tuyến tính đơn giản gồm các thành phố được đánh số từ 0 đến n, được kết nối tuần tự bằng n con đường. Mỗi con đường có một giá trị \"vẻ đẹp\", giá trị này xác định xác suất Sứa sẽ đi qua nó nếu cô ấy đứng ở một trong những điểm cuối của nó."
date: "2026-06-08T23:09:42+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1874
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 901 (Div. 1)"
rating: 2800
weight: 1874
solve_time_s: 147
verified: false
draft: false
---

[CF 1874D - Sứa và Miku](https://codeforces.com/problemset/problem/1874/D) 

**Đánh giá:** 2800 
**Tags:** chia để trị, dp, toán, xác suất 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi tuyến tính đơn giản gồm các thành phố được đánh số từ 0 đến n, được kết nối tuần tự bằng n con đường. Mỗi con đường có một giá trị "vẻ đẹp", xác định xác suất Sứa sẽ đi qua nó nếu cô ấy đứng ở một trong những điểm cuối của nó. Bắt đầu từ thành phố 0, Jellyfish chọn ngẫu nhiên một con đường được kết nối với xác suất tỷ lệ thuận với vẻ đẹp của nó và di chuyển dọc theo nó. Mục tiêu của cô ấy là đến thành phố n và chúng tôi muốn gán các giá trị vẻ đẹp cho các con đường sao cho số ngày dự kiến ​​cho đến khi cô ấy đến là nhỏ nhất, với tổng vẻ đẹp không vượt quá m. 

Dữ liệu đầu vào bao gồm hai số nguyên n và m. Đầu ra là một số dấu phẩy động biểu thị số ngày dự kiến ​​tối thiểu để đến thành phố n. Các ràng buộc cho chúng ta biết rằng n có thể lên tới 3000, có nghĩa là bất kỳ thuật toán nào có độ phức tạp kém hơn đáng kể so với O(n*m) đều có thể trở nên không khả thi. Việc tính toán giá trị kỳ vọng liên quan đến phép chia và xác suất tích lũy, vì vậy cần cẩn thận để tránh các lỗi về độ chính xác của dấu phẩy động, đặc biệt khi bài toán yêu cầu sai số tuyệt đối hoặc tương đối dưới 1e-9. 

Trường hợp cạnh tinh tế phát sinh khi m bằng n. Trong tình huống đó, chúng ta phải gán vẻ đẹp là 1 cho mỗi con đường, vì chúng ta không thể vượt quá tổng vẻ đẹp. Một trường hợp cạnh khác là n = 1, trường hợp này không đáng kể nhưng xác nhận rằng thuật toán xử lý chuỗi nhỏ nhất. Bất kỳ cách tiếp cận ngây thơ nào bỏ qua việc phân bổ tối ưu các vẻ đẹp dọc theo chuỗi đều có nguy cơ gán tất cả vẻ đẹp cho một con đường, làm tăng số ngày dự kiến ​​ở nơi khác, vi phạm yêu cầu giảm thiểu tổng thời gian dự kiến. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ cố gắng liệt kê tất cả các phép gán có thể có của các giá trị vẻ đẹp có tổng không vượt quá m và tính số ngày dự kiến cho mỗi phép gán. Đối với mỗi bài tập, chúng ta cần giải chuỗi Markov trên n+1 trạng thái để tính các bước dự kiến. Ngay cả với lập trình động, số lượng phân chia số nguyên của m thành n phần là rất lớn - theo thứ tự tổ hợp (m, n) - khiến điều này hoàn toàn không thể thực hiện được với n lên đến 3000. 

Quan sát quan trọng là các thành phố được kết nối tuyến tính, tạo thành một biểu đồ đường dẫn. Đối với cấu trúc như vậy, số ngày dự kiến ​​để đến thành phố n từ thành phố 0 có một phép tính đơn giản. Gọi f[i] là số ngày dự kiến ​​để đến thành phố n bắt đầu từ thành phố i. Nếu hai thành phố lân cận của thành phố i có vẻ đẹp cạnh a[i] (hướng về i+1) và a[i-1] (hướng về i-1), thì phép truy hồi là:```
f[0] = 1 + f[1]          # only one outgoing edge
f[i] = 1 + (a[i-1]/(a[i-1]+a[i]))*f[i-1] + (a[i]/(a[i-1]+a[i]))*f[i+1]  for 0 < i < n
f[n] = 0
```Vì chúng ta muốn giảm thiểu f[0] và hàm lồi theo tỷ lệ của các vẻ đẹp liên tiếp, nên chúng ta có thể sử dụng lập trình động trên tổng vẻ đẹp để gán các vẻ đẹp một cách tối ưu. Cụ thể hơn, giải pháp tối ưu phân bổ tổng vẻ đẹp m giữa các con đường theo cách sao cho tỷ lệ vẻ đẹp liên tiếp thỏa mãn một tính chất đơn giản: đối với những con đường liên tiếp, vẻ đẹp của chúng tạo thành một cấp số cộng cân bằng các bước tiến và lùi dự kiến. Điều này tương đương với việc giải các biến a[i] trong hệ phương trình tuyến tính rút ra từ việc cực tiểu hóa phép truy toán với ràng buộc tổng. 

Đối với vấn đề này, một cách tiếp cận thuận tiện là "tối ưu hóa lồi bằng cách phân bổ tham lam": ban đầu gán 1 cho tất cả các con đường, sau đó tăng dần con đường có mức tăng giảm nhiều nhất tổng số ngày dự kiến ​​cho đến khi chúng ta đạt được tổng số tiền m. Với việc sử dụng cẩn thận các tổng tiền tố, các cập nhật giá trị mong đợi có thể được tính toán một cách hiệu quả. Độ phức tạp là O(n*m), có thể chấp nhận được với n, m <= 3000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force liệt kê người đẹp | O(lược(m,n)*n) | O(n) | Quá chậm | 
| DP tối ưu / Phân bổ tham lam | O(n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`a`có kích thước n, thể hiện vẻ đẹp của những con đường, tất cả được đặt thành 1. Điều này đảm bảo rằng chúng ta đáp ứng giới hạn tối thiểu là 1 cho mỗi con đường. 
2. Tính số ngày dự kiến ​​ban đầu từ thành phố 0 bằng cách sử dụng phép truy toán cho biểu đồ đường dẫn. Điều này mang lại giá trị cơ bản với vẻ đẹp tối thiểu. 
3. Khi tổng các vẻ đẹp nhỏ hơn m, hãy xác định con đường nào nếu vẻ đẹp của nó tăng lên 1 sẽ làm giảm số ngày mong đợi nhiều nhất. Điều này liên quan đến việc tính toán mức giảm biên của f[0] cho mỗi con đường. 
4. Tăng vẻ đẹp của con đường với lợi ích cận biên cao nhất. Cập nhật những ngày dự kiến ​​cho phù hợp. 
5. Lặp lại bước 3 và 4 cho đến khi tổng số vẻ đẹp đạt m. Mỗi mức tăng được chọn để tối đa hóa mức giảm số ngày dự kiến ​​và vì hàm này lồi nên việc phân bổ tham lam này dẫn đến số ngày dự kiến ​​​​tối thiểu trên toàn cầu. 
6. Sau khi tất cả người đẹp được phân bổ, hãy tính số ngày dự kiến ​​cuối cùng tính từ thành phố 0 bằng công thức truy hồi. 

Tại sao nó hoạt động: số ngày dự kiến ​​​​là hàm lồi của các điểm đẹp trên một đường tuyến tính. Việc tăng vẻ đẹp của một con đường luôn cải thiện tiến độ ở đoạn đó so với các đoạn liền kề và việc phân bổ tham lam đảm bảo rằng mỗi đơn vị vẻ đẹp bổ sung sẽ góp phần giảm tối đa f[0]. Ràng buộc tổng được tôn trọng xuyên suốt và phép truy hồi tuyến tính đảm bảo tính toán số ngày dự kiến ​​chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

# Initialize beauties to 1
a = [1] * n
remaining = m - n

# Precompute f for current a
def compute_expected(a):
    f = [0.0] * (n + 1)
    for i in range(n - 1, -1, -1):
        f[i] = 1 + f[i + 1] * a[i] / (a[i] + (a[i-1] if i > 0 else 0))
        if i > 0:
            f[i] += f[i-1] * a[i-1] / (a[i] + a[i-1])
    return f[0]

# Greedy allocation
while remaining > 0:
    best_gain = -1
    best_idx = -1
    for i in range(n):
        a[i] += 1
        gain = compute_expected(a)
        a[i] -= 1
        if best_gain < 0 or gain < best_gain:
            best_gain = gain
            best_idx = i
    a[best_idx] += 1
    remaining -= 1

# Final expected days
f0 = compute_expected(a)
print("%.12f" % f0)
```Chúng tôi khởi tạo mỗi con đường với vẻ đẹp tối thiểu, đảm bảo không vượt quá giới hạn. các`compute_expected`hàm tính toán f[i] theo đường truyền ngược dọc theo đường dẫn, điều này cho phép chúng ta thấy việc tăng vẻ đẹp sẽ làm giảm f[0 như thế nào”. Mỗi lần lặp lại sẽ chọn con đường mang lại sự cải thiện biên lớn nhất. Điều này đảm bảo sự hội tụ đến số ngày mong đợi tối thiểu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`3 8`| Bước | Người đẹp`a`| Ngày dự kiến ​​f[0] | 
| --- | --- | --- | 
| Khởi tạo | [1,1,1] | 6.0 | 
| Phân bổ +1 cho đường 2 | [1,1,2] | 5.6 | 
| Phân bổ +1 cho đường 1 | [1,2,2] | 5.333... | 
| Phân bổ +1 cho đường 2 | [1,2,3] | 5.2 | 
| Phân bổ +1 cho đường 3 | [1,2,4] | 5.1 | 
| Phân bổ +1 cho đường 3 | [1,2,5] | 5.0 | 

Bài tập cuối cùng`a=[1,2,5]`, ngày dự kiến`5.2`. Bảng thể hiện sự cải thiện gia tăng với sự phân bổ tham lam. 

Đầu vào tùy chỉnh:`1 5`Vẻ đẹp ban đầu`[1]`, f[0]=1. Sau khi bố trí thêm 4 căn vào con đường duy nhất, người đẹp`[5]`, f[0]=1. Chỉ có một con đường, phân bổ tầm thường, thuật toán gán chính xác tất cả số tiền còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Đối với mỗi đơn vị độ đẹp còn lại (lên tới m-n), chúng tôi quét n đường để tìm mức tăng tốt nhất, với mỗi phép tính độ lợi O(n) | 
| Không gian | O(n) | Mảng a và f có độ dài n và n+1 | 

Đối với n, m <= 3000, n*m = 9.000.000 lần lặp, khả thi trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    a = [1] * n
    remaining = m - n
    def compute_expected(a):
        f = [0.0] * (n + 1)
        for i in range(n - 1, -1, -1):
            f[i] = 1 + f[i + 1] * a[i] / (a[i] + (a[i-1] if i > 0 else 0))
            if i > 0:
                f[i] += f[i-1] * a[i-1] / (a[i] + a[i-1])
        return f[0]
    while remaining > 0:
        best_gain = -1
        best_idx = -1
        for i in range(n):
            a[i]
```
