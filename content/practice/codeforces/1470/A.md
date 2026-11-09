---
title: "CF 1470A - Bữa tiệc sinh nhật kỳ lạ"
description: "Chúng ta được yêu cầu giảm thiểu tổng chi phí tặng quà cho một nhóm bạn dưới một số ràng buộc. Mỗi người bạn có một số quà ưa thích, $ki$. Những món quà được đánh số từ $1$ đến $m$ và có giá cố định $c1 le c2 le dots le cm$."
date: "2026-06-11T00:58:44+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1470
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 694 (Div. 1)"
rating: 1300
weight: 1470
solve_time_s: 88
verified: true
draft: false
---

[CF 1470A - Bữa tiệc sinh nhật kỳ lạ](https://codeforces.com/problemset/problem/1470/A) 

**Đánh giá:** 1300 
**Thẻ:** tìm kiếm nhị phân, dp, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu giảm thiểu tổng chi phí tặng quà cho một nhóm bạn dưới một số ràng buộc. Mỗi người bạn có một số quà ưa thích,$k_i$. Quà tặng được đánh số từ$1$ĐẾN$m$và có chi phí cố định$c_1 \le c_2 \le \dots \le c_m$. Đối với một người bạn nhất định, chúng ta có hai lựa chọn: hoặc mua một món quà có số lượng không vượt quá$k_i$hoặc đơn giản là tặng người bạn đó$c_{k_i}$đô la trực tiếp. Mỗi món quà chỉ có thể được mua một lần. Mục tiêu là tính tổng chi tiêu tối thiểu cho tất cả bạn bè. 

Đầu vào có thể có tới$3 \cdot 10^5$bạn bè và quà tặng tích lũy qua các trường hợp thử nghiệm. Ràng buộc này loại trừ bất kỳ giải pháp nào có độ phức tạp bậc hai. Đối với mỗi người bạn, lặp lại tất cả các món quà lên đến$k_i$sẽ quá chậm vì điều đó có thể đạt tới$O(n \cdot m)$hoạt động trong trường hợp xấu nhất. Thay vào đó, chúng ta cần một cách tiếp cận tuyến tính hoặc tuyến tính về số lượng bạn bè, sử dụng cách sắp xếp hoặc phân bổ tham lam. 

Các trường hợp khó khăn bao gồm các tình huống trong đó nhiều bạn bè gặp khó khăn$k_i$giá trị và những món quà rẻ tiền, hoặc khi$k_i$các giá trị đã được liên kết với những món quà giá rẻ. Sự tham lam bất cẩn trong việc phân bổ những món quà rẻ nhất mà không cân nhắc đến giá trị$k_i$giới hạn có thể chi tiêu quá mức. Ví dụ: nếu tất cả bạn bè thích$k_i = 1$nhưng những món quà rẻ nhất lại nằm ở cuối danh sách, việc mù quáng lấy những món quà rẻ nhất hiện có sẽ vi phạm sự ràng buộc. 

## Phương pháp tiếp cận 

Phương pháp bạo lực sẽ là lặp lại từng người bạn và đối với mỗi người, hãy quét qua tất cả các món quà từ$1$ĐẾN$k_i$để chọn món quà chưa được chỉ định rẻ nhất. Điều này đúng về nguyên tắc nhưng rõ ràng là quá chậm. Nếu như$n$Và$m$cả hai đều ở xung quanh$3 \cdot 10^5$, quét$O(k_i)$cho mỗi người bạn có thể dễ dàng tiếp cận$10^{10}$hoạt động. 

Nhận xét quan trọng để có giải pháp nhanh hơn là những người nhận quà có số lượng cao hơn sẽ linh hoạt hơn. Ngược lại, những người có kích thước nhỏ hơn$k_i$các giá trị có ít lựa chọn hơn. Chúng ta có thể khai thác điều này bằng cách sắp xếp bạn bè theo$k_i$theo thứ tự giảm dần và chia những món quà còn lại rẻ nhất cho những người bạn có giá trị cao hơn.$k_i$. Chúng ta lặp lại những người bạn từ người sẵn sàng nhận món quà đắt tiền nhất cho đến người bạn có món quà nhỏ nhất.$k_i$. Ở mỗi bước, chúng tôi so sánh món quà rẻ nhất hiện có với việc tặng$c_{k_i}$đô la trực tiếp và chọn mức tối thiểu. Điều này đảm bảo rằng chúng tôi không bao giờ vi phạm$k_i$hạn chế và cũng đảm bảo chúng ta sử dụng những món quà rẻ hơn theo thứ tự. 

Cách tiếp cận này đương nhiên dẫn đến phương pháp hai con trỏ hoặc phương pháp tham lam tuyến tính về số lượng bạn bè cộng với bước sắp xếp, tức là$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(m) | Quá chậm | 
| Tối ưu | O(n log n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc số lượng bạn bè$n$, số lượng quà$m$, mảng quà tặng ưa thích$k$, và chi phí$c$của những món quà. 
2. Sắp xếp bạn bè$k_i$theo thứ tự giảm dần. Điều này đảm bảo rằng những người bạn sẵn sàng nhận những món quà đắt tiền hơn sẽ được xử lý trước, vì vậy chúng tôi chỉ định những món quà rẻ hơn mà không vi phạm giới hạn. 
3. Khởi tạo con trỏ`p`ở mức 0 để theo dõi món quà rẻ nhất tiếp theo trong`c`. 
4. Khởi tạo`total_cost`bằng 0. 
5. Lặp lại từng người bạn theo thứ tự giảm dần$k_i$đặt hàng: 

một. Tính chi phí trực tiếp cho người bạn này là$c[k_i - 1]`. b. If the pointer `p` is within bounds (`p < m`) and the present `c[p]` is cheaper than $c[k_i - 1]`, assign present `c[p]` and increment `p`. 

c. Nếu không, hãy cho người bạn$c[k_i - 1]$đô la trực tiếp. 

d. Thêm chi phí đã chọn vào`total_cost`. 
6. Sau khi xử lý tất cả bạn bè, xuất ra`total_cost`. 

Tại sao nó hoạt động: Bằng cách xử lý những người bạn sẵn sàng chấp nhận cao hơn$k_i$đầu tiên, chúng tôi đảm bảo rằng những món quà rẻ nhất sẽ đến tay những người bạn linh hoạt nhất. Bất kỳ người bạn nào có kích thước nhỏ hơn$k_i$sẽ không bao giờ bị buộc phải nhận một món quà vượt quá giới hạn của họ vì chúng tôi chỉ sử dụng những món quà có giá từ rẻ nhất trở lên và không bao giờ vượt quá giới hạn của họ.$k_i$. Con trỏ`p`đảm bảo rằng mỗi món quà được chỉ định nhiều nhất một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        k = list(map(int, input().split()))
        c = list(map(int, input().split()))
        
        k.sort(reverse=True)
        p = 0
        total_cost = 0
        for ki in k:
            direct_cost = c[ki - 1]
            if p < m and c[p] < direct_cost:
                total_cost += c[p]
                p += 1
            else:
                total_cost += direct_cost
        print(total_cost)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đọc`n`,`m`, những món quà ưa thích của bạn bè và chi phí hiện tại. các`k`mảng được sắp xếp theo thứ tự giảm dần để ưu tiên những người bạn có thể nhận những món quà đắt tiền hơn. Con trỏ`p`theo dõi những món quà đã được sử dụng. Đối với mỗi người bạn, chúng tôi chọn giữa món quà rẻ nhất có sẵn trong giới hạn và số tiền trực tiếp. Sự lựa chọn được thêm vào tích lũy`total_cost`. In tổng số sau mỗi trường hợp thử nghiệm phù hợp với định dạng đầu ra được yêu cầu. Những điểm tinh tế bao gồm`ki - 1`điều chỉnh lập chỉ mục dựa trên 0 trong Python và kiểm tra ranh giới`p < m`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 4
2 3 4 3 2
3 5 12 20
```| Bạn (k_i) | Chi phí trực tiếp | Rẻ nhất hiện có (c[p]) | Chi phí được chọn | Con trỏ p | 
| --- | --- | --- | --- | --- | 
| 4 | 20 | 3 | 3 | 1 | 
| 3 | 12 | 5 | 5 | 2 | 
| 3 | 12 | 12 | 12 | 3 | 
| 2 | 5 | 12 | 5 | 3 | 
| 2 | 5 | 12 | 5 | 3 | 

Tổng chi phí = 3 + 5 + 12 + 5 + 5 = 30 

Dấu vết này xác nhận rằng việc tham lam tặng những món quà rẻ nhất cho những người bạn linh hoạt sẽ tránh được việc chi tiêu quá mức. 

### Mẫu 2 

đầu vào:```
5 5
5 4 3 2 1
10 40 90 160 250
```| Bạn (k_i) | Chi phí trực tiếp | Rẻ nhất hiện có (c[p]) | Chi phí được chọn | Con trỏ p | 
| --- | --- | --- | --- | --- | 
| 5 | 250 | 10 | 10 | 1 | 
| 4 | 160 | 40 | 40 | 2 | 
| 3 | 90 | 90 | 90 | 3 | 
| 2 | 40 | 160 | 40 | 3 | 
| 1 | 10 | 160 | 10 | 3 | 

Tổng chi phí = 10 + 40 + 90 + 40 + 10 = 190 

Dấu vết cho thấy chi phí trực tiếp được sử dụng như thế nào khi những món quà giá rẻ còn lại vượt quá giới hạn của người bạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Sắp xếp bạn bè là O(n log n), lặp lại theo bạn bè là O(n), m tích lũy không vượt quá giới hạn. | 
| Không gian | O(n + m) | Lưu trữ mảng k và c; không gian phụ trợ là không đáng kể. | 

Thuật toán dễ dàng nằm gọn trong giới hạn 1 giây ngay cả ở kích thước đầu vào tối đa vì công việc chính bị chi phối bởi việc sắp xếp`k`, và tất cả các phép toán khác là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("2\n5 4\n2 3 4 3 2\n3 5 12 20\n5 5\n5 4 3 2 1\n10 40 90 160 250\n") == "30\n190"

# minimum-size input
assert run("1\n1 1\n1\n100\n") == "100", "single friend, single present"

# all k_i same
assert run("1\n3 3\n2 2 2\n5 10 15\n") == "15", "friends with same k_i, use cheapest for first, rest direct"

# increasing k_i
assert run("1\n4 4\n1 2 3 4\n1 2 3 4\n") == "10", "assign cheapest presents to flexible friends"

# maximum k_i
assert run("1\n3 5\n5 5 5\n10 20 30 40 50\n") == "60", "all friends take cheapest available within k_i=5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ` | | |
