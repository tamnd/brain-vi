---
title: "CF 102500G - Giả thuyết Gnoll"
description: "Chúng ta có một danh sách vòng gồm n loại quái vật. Trước khi cập nhật, loại i xuất hiện với xác suất s[i] phần trăm. Vị trí sinh sản hiện chỉ giữ k loại được chọn ngẫu nhiên."
date: "2026-08-05T18:07:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 227
verified: true
draft: false
---

[CF 102500G - Giả thuyết Gnoll](https://codeforces.com/problemset/problem/102500/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một danh sách vòng tròn của`n`các loại quái vật. Trước khi cập nhật, gõ`i`xuất hiện với xác suất`s[i]`phần trăm. Vị trí sinh sản bây giờ chỉ giữ lại`k`loại được chọn ngẫu nhiên. Các loại được chọn giữ nguyên xác suất của riêng chúng, trong khi mỗi loại bị loại bỏ sẽ đưa ra xác suất của loại đó cho loại được chọn đầu tiên gặp phải khi di chuyển về phía trước quanh vòng tròn. 

Nhiệm vụ là tính xác suất cuối cùng dự kiến ​​của mỗi loại sau khi lấy trung bình trên tất cả các lựa chọn ngẫu nhiên có thể có của`k`giữ các loại. 

Phần quan trọng là chúng ta không cần mô phỏng các nhóm ngẫu nhiên. Số lượng nhóm có thể có là`C(n, k)`, đó là rất lớn ngay cả đối với`n = 500`. Chúng ta cần khai thác tính đối xứng của thứ tự vòng tròn và tính toán đóng góp của mọi xác suất ban đầu một cách độc lập. 

Từ`n`nhiều nhất là 500, một`O(n^2)`thuật toán dễ dàng đủ nhanh. MỘT`O(n^3)`Cách tiếp cận này sẽ có khoảng 125 triệu thao tác và sẽ có rủi ro trong Python, trong khi mọi thứ liên quan đến tất cả các tập hợp con là không thể. 

Có một số trường hợp giải pháp sai thường thất bại. Khi`k = n`, mọi loại luôn được chọn, vì vậy câu trả lời phải bằng đầu vào. Ví dụ:```
3 3
10 20 70
```Đầu ra là:```
10 20 70
```Một công thức vô tình cho phép các loại lân cận đóng góp sẽ tạo ra câu trả lời sai ở đây. 

Khi`k = 1`, chính xác một loại tồn tại và nó nhận được tất cả khối lượng xác suất. Vì:```
3 1
10 20 70
```đầu ra là:```
33.333333333333 33.333333333333 33.333333333333
```Mọi loại đều có khả năng là loại được chọn duy nhất như nhau, bất kể xác suất ban đầu của nó là bao nhiêu. 

Vòng tròn bao quanh là một nguồn sai lầm khác. Vì:```
2 1
30 70
```cả hai câu trả lời đều là`50`. Loại thứ hai có thể nhận được xác suất của loại thứ nhất sau khi gói và loại thứ nhất có thể nhận được xác suất của loại thứ hai. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là liệt kê mọi tập hợp có thể của`k`các loại đã chọn. Đối với mỗi bộ, chúng ta có thể đi vòng quanh vòng tròn, gán mọi loại đã loại bỏ cho loại được chọn tiếp theo và thêm các xác suất kết quả. Điều này đúng vì nó tuân theo chính xác quy tắc sinh sản. 

Vấn đề là số lượng bộ. Ngay cả với`n = 500`,`C(500, 250)`vượt xa những gì có thể được xử lý. Lực lượng vũ phu không gần với độ phức tạp cần thiết. 

Quan sát quan trọng là kỳ vọng cho phép chúng ta phân chia các khoản đóng góp. Thay vì hỏi loại được chọn nhận xác suất từ ​​đâu, chúng tôi hỏi xác suất của loại ban đầu cố định sẽ đi về đâu. 

Xem xét loại`i`. Xác suất ban đầu của nó góp phần tạo nên loại`i + d`nếu đích đến đó được chọn và`d`các loại ngay trước khi nó không được chọn. Số cách chọn các loại còn lại chỉ phụ thuộc vào`d`, không phải ở vị trí thực tế. Điều này tạo ra một hệ số cố định cho mọi khoảng cách xung quanh vòng tròn. 

Đối với một khoảng cách`d`, xác suất loại ở khoảng cách`d`phía sau`i`góp phần vào`i`là:```
C(n-d-1, k-1) / C(n, k)
```Tử số chọn cái còn lại`k-1`các loại được chọn từ các vị trí không bị chặn giữa nguồn và đích. 

Sau khi tính toán các hệ số này một lần, mỗi vị trí trả lời chỉ là tổng trọng số tròn của các xác suất ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n,k) * n) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính các hệ số cho mọi khoảng cách hình tròn có thể. Đối với khoảng cách`d`, lưu trữ xác suất mà một loại`d`các vị trí phía sau loại hiện tại sẽ chuyển xác suất của nó sang loại hiện tại. Điều này chỉ dựa trên`n`,`k`, Và`d`, vì vậy nó có thể được sử dụng lại cho mọi vị trí trả lời. 
2. Tính toán các kết hợp sử dụng các giá trị dấu phẩy động. Bản thân các giá trị nhị thức có thể cực kỳ lớn nhưng chỉ cần tỷ lệ của chúng. Từ`n`nhỏ thì một tam giác Pascal nhân đôi là đủ. 
3. Đối với từng loại điểm đến`i`, lặp qua mọi khoảng cách`d`. Thêm xác suất ban đầu của loại tại`(i - d) mod n`nhân với hệ số cho`d`. 
4. In kết quả xác suất dự kiến. 

Bất biến đằng sau thuật toán là mọi xác suất ban đầu được phân phối chính xác theo xác suất của từng đích đến có thể. Hệ số khoảng cách`d`đếm chính xác các sự kiện trong đó đích đến tồn tại và tất cả những người sống sót gần hơn đều vắng mặt. Vì tất cả các nguồn đóng góp có thể được thêm vào một cách độc lập nên tính tuyến tính của kỳ vọng mang lại xác suất cuối cùng được mong đợi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = list(map(float, input().split()))

    comb = [[0.0] * (n + 1) for _ in range(n + 1)]
    comb[0][0] = 1.0
    for i in range(1, n + 1):
        comb[i][0] = 1.0
        comb[i][i] = 1.0
        for j in range(1, i):
            comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j]

    total = comb[n][k]

    coef = []
    for d in range(n):
        coef.append(comb[n - d - 1][k - 1] / total)

    ans = [0.0] * n
    for i in range(n):
        cur = 0.0
        for d in range(n):
            cur += s[(i - d) % n] * coef[d]
        ans[i] = cur

    print(*ans)

if __name__ == "__main__":
    solve()
```Cửa hàng bảng kết hợp`C(a,b)`các giá trị dưới dạng số dấu phẩy động. Việc lưu trữ số nguyên không phù hợp vì các giá trị này tăng quá lớn, trong khi phép tính cuối cùng chỉ yêu cầu tỷ lệ. 

Vòng lặp hệ số sử dụng`n - d - 1`còn hơn là`n - d`. Bản thân đích phải đã được chọn và các vị trí gần nguồn hơn phải vắng mặt. Ranh giới này là sai lầm phổ biến nhất trong vấn đề này. 

Vòng lặp kép cuối cùng sử dụng lập chỉ mục modulo vì danh sách quái vật có dạng hình tròn. Đối với một điểm đến`i`và khoảng cách`d`, nguồn là`(i-d) mod n`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các hệ số là: 

| Khoảng cách | Hệ số | 
| --- | --- | 
| 0 | 0,6 | 
| 1 | 0,3 | 
| 2 | 0,1 | 
| 3 | 0 | 
| 4 | 0 | 

Vị trí trả lời đầu tiên nhận được: 

| Loại nguồn | Xác suất | Hệ số | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 0,6 | 0,6 | 
| 5 | 23 | 0,3 | 6,9 | 
| 4 | 12 | 0,1 | 1.2 | 

Tổng cộng là`8.7`, phù hợp với đầu ra mẫu. Việc xoay các hệ số tương tự được áp dụng cho mọi vị trí khác. 

Đối với mẫu thứ hai,`n = 3`Và`k = 2`. 

| Khoảng cách | Hệ số | 
| --- | --- | 
| 0 | 0,666666 | 
| 1 | 0,333333 | 
| 2 | 0 | 

Loại thứ nhất nhận được 2/3 xác suất của chính nó và 1/3 xác suất của loại trước: 

| Nguồn | Giá trị | Đóng góp | 
| --- | --- | --- | 
| Loại 1 | 2.019 | 1.346 | 
| Loại 3 | 10.46866 | 3.489 | 

Kết quả là khoảng`4.835553`, phù hợp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Tổ hợp tòa nhà có chi phí O(n^2) và việc tính toán tất cả các câu trả lời có chi phí O(n^2). | 
| Không gian | O(n^2) | Tam giác Pascal lưu trữ tất cả các hệ số nhị thức cần thiết. | 

Với`n = 500`, thuật toán thực hiện khoảng 250.000 phép tính trong phép tính chính và dễ dàng khớp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    s = list(map(float, input().split()))

    comb = [[0.0] * (n + 1) for _ in range(n + 1)]
    comb[0][0] = 1.0
    for i in range(1, n + 1):
        comb[i][0] = comb[i][i] = 1.0
        for j in range(1, i):
            comb[i][j] = comb[i-1][j-1] + comb[i-1][j]

    total = comb[n][k]
    coef = [comb[n-d-1][k-1] / total for d in range(n)]

    ans = []
    for i in range(n):
        ans.append(sum(s[(i-d) % n] * coef[d] for d in range(n)))

    sys.stdin = old
    return ans

a = solve_case("""5 3
1 25 39 12 23
""")
assert all(abs(x-y) < 1e-6 for x, y in zip(a, [8.7, 17.6, 31, 21.4, 21.3]))

a = solve_case("""3 2
2.019 87.51234 10.46866
""")
assert all(abs(x-y) < 1e-6 for x, y in zip(a, [4.835553333333, 59.01456, 36.14988666667]))

a = solve_case("""1 1
100
""")
assert abs(a[0] - 100) < 1e-6

a = solve_case("""4 4
10 20 30 40
""")
assert all(abs(x-y) < 1e-6 for x, y in zip(a, [10, 20, 30, 40]))

a = solve_case("""4 1
25 25 25 25
""")
assert all(abs(x-25) < 1e-6 for x in a)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 100`|`100`| Kích thước tối thiểu và xử lý loại đơn | 
|`4 4 / 10 20 30 40`| Giá trị gốc | các`k=n`ranh giới | 
|`4 1 / equal values`| Giá trị bằng nhau | các`k=1`phân bố xác suất | 
| Mẫu | Kết quả đầu ra mẫu | Tính đúng đắn chung | 

## Vỏ cạnh 

cho`k = n`, mọi quái vật đều sống sót. Trong công thức, tất cả các hệ số ngoại trừ khoảng cách 0 đều trở thành 0 vì không có đủ vị trí không được chọn để tạo chuyển giao. Thuật toán rút gọn về việc trả về mảng ban đầu. 

Vì`k = 1`, mọi hệ số trở thành`1/n`. Có chính xác một quái vật được chọn và mọi xác suất ban đầu cuối cùng đều thuộc về quái vật đó với xác suất được chọn bằng nhau. Cấu trúc vòng tròn không ảnh hưởng đến kết quả. 

Đối với các trường hợp bao quanh, lập chỉ mục modulo coi danh sách là một vòng tròn. Một nguồn ở gần đầu có thể đóng góp cho một đích ở gần cuối và cách tính hệ số tương tự vẫn được áp dụng vì khoảng cách được đo theo chu kỳ.
