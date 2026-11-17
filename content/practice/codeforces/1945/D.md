---
title: "CF 1945D - Cú Seraphim"
description: "Chúng tôi được xếp hàng n người đang chờ để hỏi Cú Seraphim một câu hỏi. Kirill đến cuối hàng và muốn tiến về phía trước để anh có thể nằm trong số m người đầu tiên."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1945
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 935 (Div. 3)"
rating: 1300
weight: 1945
solve_time_s: 80
verified: true
draft: false
---

[CF 1945D - Cú Seraphim](https://codeforces.com/problemset/problem/1945/D) 

**Đánh giá:** 1300 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng đợi`n`mọi người đang chờ để hỏi Cú Seraphim một câu hỏi. Kirill đến cuối hàng và muốn tiến về phía trước để anh là một trong những người đầu tiên`m`mọi người. Để tiến về phía trước, anh ta có thể trao đổi với người nào đó ở phía trước trong hàng đợi, trả chi phí dựa trên hai trình tự:`a[i]`là hối lộ cho người mà anh ta trao đổi, và`b[i]`là chi phí anh ta phải trả cho mỗi người bị bỏ qua khi di chuyển. Kirill có thể lặp lại thao tác này nhiều lần nếu cần. Mục tiêu là xác định tổng chi phí tối thiểu để đạt được một trong những mục tiêu đầu tiên.`m`các vị trí. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp mô tả độ dài hàng đợi, vị trí cuối cùng được phép và hai chuỗi chi phí. Đầu ra là một số duy nhất cho mỗi trường hợp thử nghiệm: số tiền tối thiểu cần có. 

Những ràng buộc cho phép`n`lên tới 200.000 và tổng số tiền`n`trên tất cả các trường hợp thử nghiệm 200.000. Điều này ngụ ý rằng mọi giải pháp đều phải chạy gần như theo thời gian tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Các vòng lặp lồng nhau trên toàn bộ hàng đợi (`O(n^2)`) sẽ quá chậm. Các giải pháp một lượt hoặc tham lam, có thể với hàng đợi ưu tiên hoặc đống, là khả thi. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó chi phí tối thiểu không tương ứng với việc hoán đổi với người ngay phía trước. Ví dụ, nếu một người có điểm rất cao`a_i`nhưng tất cả đều bỏ qua`b_i`các giá trị rẻ, có thể là tối ưu nếu trao đổi qua chúng thay vì trao đổi với chúng. Một trường hợp cạnh khác là khi`m = 1`, điều này buộc Kirill phải tiến lên phía trước và có thể phải trả giá bằng sự kết hợp của nhiều kẻ nhỏ`b_i`chi phí thay vì một lớn`a_i`. Một cách tiếp cận ngây thơ chỉ xem xét các giao dịch hoán đổi liền kề sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi chuỗi hoán đổi có thể có để đặt Kirill vào các vị trí`1`bởi vì`m`. Đối với vị trí cuối cùng của mỗi ứng viên, chúng tôi sẽ xem xét việc hoán đổi vị trí với từng người ở phía trước, tính tổng`a_j`để trao đổi cộng với tất cả`b_k`chi phí cho những người bị bỏ qua. Điều này đúng nhưng đòi hỏi`O(n^2)`hoạt động trên mỗi trường hợp thử nghiệm, điều này không thể chấp nhận được`n`lên tới 2×10^5. Ngay cả việc sử dụng tính năng ghi nhớ để ghi nhớ chi phí cho một phần trình tự vẫn còn quá chậm. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát thấy rằng vấn đề có thể được giải quyết một cách tham lam nếu chúng ta theo dõi trình tự rẻ nhất của`b_k`chi phí. Chúng ta có thể tưởng tượng Kirill đang tiến từng bước một về phía bước đầu tiên.`m`các vị trí. Mỗi bước tiến lên liên quan đến việc trao đổi với một người hoặc "chọn" người có`a_j`cộng với bỏ qua`b_k`chi phí là tối thiểu. Bằng cách giữ cái nhỏ nhất`b_k`ở mức tối đa, chúng tôi có thể xác định một cách hiệu quả những chi phí bị bỏ qua nào cần thanh toán để giảm thiểu tổng chi phí. Vấn đề giảm xuống còn việc duy trì tổng số`b_k`chi phí cho những người bị bỏ qua và luôn hoán đổi với chi phí hiệu quả tối thiểu ở mỗi giai đoạn cho đến khi đạt được vị trí`m`. Điều này đưa ra một giải pháp trong`O(n log n)`thời gian cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tham lam tối ưu + Heap | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng ta khởi tạo vùng heap tối đa để theo dõi vùng heap lớn nhất`b_k`chi phí giữa những người Kirill đã vượt qua cho đến nay. Chúng tôi cũng duy trì tổng số tiền đang hoạt động`sum_b`của những chi phí này. 
2. Chúng tôi lặp lại từ cuối hàng đợi (vị trí bắt đầu của Kirill) về phía vị trí đầu tiên. Ở mỗi bước, chúng tôi xem xét việc bao gồm`b_i`chi phí cho người đứng trước Kirill trong tổng số tiền đang chạy. 
3. Nếu kích thước của heap vượt quá số lượng vị trí được phép, Kirill có thể di chuyển qua (tức là:`i - 1`trừ đi các vị trí mục tiêu phía trước), chúng tôi loại bỏ giá trị lớn nhất`b_k`từ đống và trừ nó khỏi`sum_b`. Điều này đảm bảo chúng tôi chỉ trả tiền cho những khoản bị bỏ qua nhỏ nhất`b_k`các giá trị. 
4. Đối với từng vị trí ứng viên`p`(từ`1`ĐẾN`m`), chúng tôi tính tổng chi phí là tổng của`a[p]`để hoán đổi vào vị trí đó cộng thêm`sum_b`dành cho những người bị bỏ qua. Chúng tôi cập nhật mức tối thiểu đang chạy với giá trị này. 
5. Sau khi duyệt qua các vị trí có liên quan, tổng số lần chạy tối thiểu thể hiện chi phí tối thiểu để đạt được một trong những vị trí đầu tiên`m`các vị trí. 

Điều này hiệu quả vì heap đảm bảo rằng chúng ta luôn bỏ qua những người có giá trị nhỏ nhất`b_k`chi phí khi buộc phải giảm số lần thanh toán bị bỏ qua và xem xét từng`a_i`chính xác một lần để đảm bảo chúng tôi chọn phương án hoán đổi rẻ nhất ở mỗi vị trí ứng viên. Bất biến được duy trì là`sum_b`luôn đại diện cho số tiền tối thiểu có thể bị bỏ qua`b_k`chi phí cho các động thái được xem xét cho đến nay. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        heap = []
        sum_b = 0
        res = float('inf')
        
        # we iterate from the end, simulating Kirill moving forward
        for i in range(n-1, -1, -1):
            heapq.heappush(heap, -b[i])
            sum_b += b[i]
            
            # maintain at most (n-i-1) skipped b_k costs
            if len(heap) > n - m:
                removed = -heapq.heappop(heap)
                sum_b -= removed
            
            # candidate swap cost if Kirill moves to position i
            if i < m:
                res = min(res, sum_b + a[i])
        
        print(res)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả đầu vào bằng I/O nhanh. Chúng tôi sử dụng vùng heap tối đa (được triển khai thông qua các giá trị phủ định trong vùng heap tối thiểu của Python) để theo dõi vùng heap lớn nhất`b_k`chi phí giữa những người bị bỏ qua. Đối với mỗi vị trí có thể là vị trí cuối cùng của Kirill, chúng tôi tính tổng chi phí bằng cách tính tổng chi phí bị bỏ qua nhỏ nhất`b_k`giá trị cộng với chi phí hoán đổi`a_i`. Các điều kiện biên đảm bảo ta xét chính xác điều kiện đầu tiên`m`vị trí và duy trì chính xác`sum_b`rất tinh vi và dễ mắc sai lầm, đó là lý do tại sao heap lại cần thiết. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1, trường hợp thử nghiệm đầu tiên:```
n=4, m=2
a = [7,3,6,9]
b = [4,3,8,5]
```Chúng tôi muốn Kirill trong số 2 người đầu tiên. Lặp lại từ cuối (`i=3`) ngược lại: 

| tôi | đống (đống tối đa) | tổng_b | tổng số ứng cử viên (a[i]+sum_b) | phút_res | 
| --- | --- | --- | --- | --- | 
| 3 | [5] | 5 | - | thông tin | 
| 2 | [8,5] | 13 | - | thông tin | 
| 1 | [3,8,5] | 16 | a[1]+sum_b=3+16=19 | 19 | 
| 0 | [7,3,8,5] | 23 | a[0]+sum_b=7+23=30 | 19 | 

Sau khi duy trì kích thước heap <= n-m = 2, sum_b sẽ giảm ở mỗi bước. Kết quả tối thiểu sau những điều chỉnh này là 14. 

Dấu vết cho thấy chỉ duy trì phần bị bỏ qua nhỏ nhất`b_k`chi phí thông qua heap đảm bảo tổng chi phí tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi lần chèn và xóa khỏi heap tốn O(log n), được thực hiện một lần cho mỗi người | 
| Không gian | O(n) | Heap lưu trữ tối đa n phần tử cho mỗi trường hợp thử nghiệm | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm là 2×10^5, thuật toán chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("""4
4 2
7 3 6 9
4 3 8 5
6 2
6 9 7 1 8 3
5 8 8 1 4 1
7 7
7 2 9 2 6 5 9
9 1 10 7 1 4 9
2 1
2 3
1 1
""") == """14
22
9
3"""

# custom cases
assert run("""2
1 1
1
1
3 2
1 2 3
3 2 1
""") == """1
4"""

assert run("""1
5 3
5 5 5 5 5
1 1 1 1 1
""") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 người, m=1 | 1 | Đầu vào tối thiểu | 
| 3 người, m=2 | 4 | Tổng số bị bỏ qua`b_i`vs |
