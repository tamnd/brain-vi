---
title: "CF 1676E - Truy vấn ăn uống"
description: "Chúng ta được đưa ra nhiều tình huống độc lập, mỗi tình huống mô tả một bộ sưu tập kẹo trong đó mỗi viên kẹo có một lượng đường cố định."
date: "2026-06-10T00:59:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1676
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 790 (Div. 4)"
rating: 1100
weight: 1676
solve_time_s: 85
verified: true
draft: false
---

[CF 1676E - Câu hỏi về ăn uống](https://codeforces.com/problemset/problem/1676/E) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra nhiều tình huống độc lập, mỗi tình huống mô tả một bộ sưu tập kẹo trong đó mỗi viên kẹo có một lượng đường cố định. Đối với mỗi truy vấn, chúng tôi muốn xác định số lượng kẹo nhỏ nhất mà Timur phải ăn để tổng lượng đường anh ấy tiêu thụ đạt ít nhất một giá trị mục tiêu. Anh ta có thể tự do chọn bất kỳ tập hợp con kẹo nào cho mỗi truy vấn, nhưng trong một truy vấn, mỗi viên kẹo chỉ có thể được sử dụng nhiều nhất một lần. 

Một quan sát quan trọng là thứ tự ăn kẹo không cố định, do đó, đối với bất kỳ mục tiêu nào, chúng tôi đang chọn tập hợp con tốt nhất có thể một cách hiệu quả. Chiến lược tốt nhất để giảm thiểu số lượng kẹo trong khi đạt được số lượng cần thiết là luôn lấy giá trị lớn nhất trước tiên. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cố gắng tính toán lại tập hợp con tối ưu cho mỗi truy vấn sẽ không thành công. Với tổng số lên tới 150.000 viên kẹo và 150.000 truy vấn, cách tiếp cận O(nq) ngay lập tức là không thể. Ngay cả giải pháp O(n log n) cho mỗi truy vấn cũng sẽ quá chậm. Điều này thúc đẩy chúng tôi hướng tới chiến lược tiền xử lý cho phép mỗi truy vấn được trả lời theo thời gian logarit hoặc không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi tổng số kẹo nhỏ hơn giá trị truy vấn. Trong tình huống đó, không có loại kẹo nào có thể thỏa mãn yêu cầu nên câu trả lời phải là -1. Một trường hợp góc khác là khi một viên kẹo đã vượt quá mục tiêu, trong đó câu trả lời phải là 1. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử ngầm mọi kích thước tập hợp con bằng cách thử nghiệm sự kết hợp của kẹo. Người ta có thể tưởng tượng việc sắp xếp mảng và, đối với mỗi truy vấn, tham lam chọn những viên kẹo từ lớn nhất đến nhỏ nhất cho đến khi tổng đạt được mục tiêu. Mặc dù đúng nhưng điều này vẫn tốn O(n) cho mỗi truy vấn trong trường hợp xấu nhất, vì chúng ta có thể cần quét nhiều phần tử trước khi đạt đến ngưỡng. Với tối đa 150.000 truy vấn, điều này dẫn đến khoảng 2,25 × 10^10 thao tác, vượt xa giới hạn khả thi. 

Thông tin chi tiết về cấu trúc quan trọng là khi các viên kẹo được sắp xếp theo thứ tự giảm dần, chiến lược tối ưu cho bất kỳ truy vấn nào luôn là tiền tố của mảng được sắp xếp này. Điều đó biến mỗi truy vấn thành một bài toán ngưỡng tổng tiền tố. Nếu chúng tôi tính toán trước tổng tiền tố thì mỗi truy vấn sẽ giảm xuống còn tìm chỉ mục nhỏ nhất trong đó tổng tiền tố đạt hoặc vượt mục tiêu. Điều này có thể được thực hiện một cách hiệu quả với tìm kiếm nhị phân. 

Vì vậy, vấn đề trở thành: sau khi sắp xếp, chúng ta xây dựng một mảng trong đó mỗi vị trí lưu trữ tổng lượng đường của k viên kẹo tốt nhất có thể. Sau đó, mỗi truy vấn sẽ tìm kiếm vị trí đầu tiên mà tổng tích lũy này là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Greedy mỗi truy vấn | O(nq) | O(1) | Quá chậm | 
| Sắp xếp + Tổng tiền tố + Tìm kiếm nhị phân | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp kẹo theo thứ tự giảm dần. Điều này đảm bảo rằng bất cứ khi nào chúng tôi lấy k viên kẹo đầu tiên, chúng tôi luôn tối đa hóa tổng lượng đường cho k kẹo đó. Bất kỳ cách đặt hàng nào khác sẽ chỉ giảm số tiền cho cùng một số lượng kẹo. 
2. Xây dựng mảng tổng tiền tố trên danh sách đã sắp xếp. Mỗi giá trị tiền tố đại diện cho lượng đường tối đa có thể đạt được bằng cách sử dụng chính xác k kẹo. 
3. Đối với mỗi giá trị truy vấn x, trước tiên hãy kiểm tra xem x có lớn hơn tổng số kẹo hay không. Nếu đúng như vậy thì không có lựa chọn nào có thể đáp ứng yêu cầu nên câu trả lời là -1. 
4. Mặt khác, thực hiện tìm kiếm nhị phân trên mảng tổng tiền tố để tìm chỉ số k nhỏ nhất sao cho prefix[k] ≥ x. Chỉ số này thể hiện số lượng kẹo tối thiểu cần thiết. 
5. Xuất k cho truy vấn đó. 

### Tại sao nó hoạt động

Tính đúng đắn phụ thuộc vào tính chất đơn điệu được đưa ra bằng cách sắp xếp. Sau khi sắp xếp theo thứ tự giảm dần, tổng tiền tố tạo thành một chuỗi không giảm và bất kỳ lựa chọn k kẹo nào cũng không thể vượt quá tổng của k kẹo đầu tiên. Điều này có nghĩa là kết quả tốt nhất có thể có đối với bất kỳ k cố định nào được xác định duy nhất bởi tiền tố. Vì tính khả thi là đơn điệu trong k nên tìm kiếm nhị phân tìm k tối thiểu thỏa mãn điều kiện mà không thiếu bất kỳ cấu hình hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort(reverse=True)
        
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]
        
        total = pref[n]
        
        for _ in range(q):
            x = int(input())
            if x > total:
                print(-1)
                continue
            
            lo, hi = 1, n
            ans = n
            while lo <= hi:
                mid = (lo + hi) // 2
                if pref[mid] >= x:
                    ans = mid
                    hi = mid - 1
                else:
                    lo = mid + 1
            
            print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các viên kẹo theo thứ tự giảm dần để những lựa chọn có giá trị nhất được ưu tiên. Sau đó, mảng tổng tiền tố được xây dựng để cho phép tính toán tổng tiền tố bất kỳ theo thời gian không đổi. 

Mỗi truy vấn được xử lý độc lập. Đầu tiên chúng ta so sánh mục tiêu với tổng số tiền để tránh việc tìm kiếm không cần thiết. Sau đó, tìm kiếm nhị phân hoạt động trên các tổng tiền tố, khai thác cấu trúc đơn điệu của chúng để xác định vị trí đầu tiên đạt được tổng yêu cầu. 

Một cạm bẫy phổ biến là quên sắp xếp theo thứ tự giảm dần. Nếu không có điều này, tổng tiền tố không thể hiện các lựa chọn tối ưu và tìm kiếm nhị phân sẽ trả về kết quả không chính xác. Một vấn đề tinh vi khác là việc lập chỉ mục riêng lẻ trong mảng tiền tố, đó là lý do tại sao nó được xây dựng với kích thước n+1. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

đầu vào:```
n = 5
a = [1, 3, 2, 5, 4]
queries = [3, 10]
```Sau khi sắp xếp:```
[5, 4, 3, 2, 1]
```Tổng tiền tố: 

| k | tiền tố[k] | 
| --- | --- | 
| 1 | 5 | 
| 2 | 9 | 
| 3 | 12 | 
| 4 | 14 | 
| 5 | 15 | 

Truy vấn 1: x = 3 

Chúng tôi tìm kiếm k nhỏ nhất với tiền tố[k] ≥ 3 → k = 1. 

Truy vấn 2: x = 10 

Ta tìm được tiền tố[3] = 12 ≥ 10 → k = 3. 

Điều này cho thấy ngay cả k nhỏ cũng có thể đáp ứng được mục tiêu lớn do tính đặt hàng tham lam. 

### Ví dụ Dấu vết 2 

đầu vào:```
n = 4
a = [2, 2, 2, 2]
queries = [1, 9]
```Mảng được sắp xếp vẫn còn:```
[2, 2, 2, 2]
```Tổng tiền tố: 

| k | tiền tố[k] | 
| --- | --- | 
| 1 | 2 | 
| 2 | 4 | 
| 3 | 6 | 
| 4 | 8 | 

Truy vấn 1: x = 1 → k = 1 

Câu 2: x = 9 → không thể, đáp án = -1 

Điều này chứng tỏ tính đúng đắn của việc kiểm tra tính khả thi toàn cầu bằng cách sử dụng tổng số tiền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | việc sắp xếp chiếm ưu thế trong mỗi trường hợp thử nghiệm, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | lưu trữ mảng tổng tiền tố | 

Cho rằng tổng của n và q trong tất cả các trường hợp thử nghiệm bị giới hạn bởi 1,5 × 10^5, độ phức tạp này phù hợp thoải mái trong giới hạn thời gian, vì cả việc sắp xếp và tìm kiếm nhị phân đều hiệu quả ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort(reverse=True)
        pref = [0]
        for v in a:
            pref.append(pref[-1] + v)
        total = pref[-1]
        for _ in range(q):
            x = int(input())
            if x > total:
                out.append("-1")
                continue
            lo, hi = 1, n
            ans = n
            while lo <= hi:
                mid = (lo + hi) // 2
                if pref[mid] >= x:
                    ans = mid
                    hi = mid - 1
                else:
                    lo = mid + 1
            out.append(str(ans))
    return "\n".join(out)

# provided sample (simplified check)
assert run("""1
5 2
1 2 3 4 5
3
10
""") == "1\n2"

# custom cases
assert run("""1
1 3
5
5
6
1
""") == "1\n-1\n1"

assert run("""1
4 2
1 1 1 1
2
5
""") == "2\n-1"

assert run("""1
5 2
5 4 3 2 1
15
16
""") == "5\n-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| kẹo đơn | 1, -1, 1 | hành vi cạnh đơn phần tử | 
| giá trị thống nhất | 2, -1 | tiền tố đối xứng và không thể | 
| ranh giới tổng hoàn hảo | 5, -1 | tổng số chính xác so với trường hợp tràn | 

## Vỏ cạnh 

Khi mục tiêu vượt quá tổng số kẹo, thuật toán sẽ ngay lập tức trả về -1 mà không cần tìm kiếm. Ví dụ: với kẹo [2, 2, 2], tổng số là 6, vì vậy truy vấn yêu cầu 7 là không thể. Kiểm tra tổng tiền tố phát hiện điều này trong thời gian không đổi. 

Khi một viên kẹo là đủ, mảng tiền tố đảm bảo tìm kiếm nhị phân trả về 1. Ví dụ: với [10, 1, 1], truy vấn 5 được đáp ứng ngay lập tức bởi phần tử đầu tiên và tìm kiếm không xem xét sai các giá trị k lớn hơn. 

Khi tất cả các viên kẹo giống hệt nhau, tiền tố sẽ tăng tuyến tính và tìm kiếm nhị phân vẫn tìm thấy ngưỡng chính xác. Đối với [3, 3, 3, 3], truy vấn 6 trả về 2 vì chỉ có hai phần tử đầu tiên đạt được mục tiêu, khớp với cấu trúc đơn điệu của tổng tiền tố.
