---
title: "CF 1243A - Hình vuông tối đa"
description: "Chúng ta được cung cấp một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một danh sách chiều cao của tấm ván. Mỗi tấm ván có chiều rộng bằng 1 và chiều cao bằng số nguyên."
date: "2026-06-13T20:13:31+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1243
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 599 (Div. 2)"
rating: 800
weight: 1243
solve_time_s: 384
verified: true
draft: false
---

[CF 1243A - Bình phương tối đa](https://codeforces.com/problemset/problem/1243/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 6 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trường hợp thử nghiệm, mỗi trường hợp bao gồm một danh sách chiều cao của tấm ván. Mỗi tấm ván có chiều rộng bằng 1 và chiều cao bằng số nguyên. Chúng ta được phép chọn bất kỳ tập hợp con nào của những tấm ván này và sắp xếp lại chúng theo bất kỳ thứ tự nào cạnh nhau, tạo thành một hình dạng giống như đường chân trời trong đó mỗi tấm ván được chọn đóng góp một cột thẳng đứng. 

Sau khi các tấm ván đã được sắp xếp xong, chúng ta được phép cắt ra một hình vuông có các cạnh thẳng hàng với các trục. Hình vuông phải nằm hoàn toàn bên trong hình kết hợp. Mục đích là để tối đa hóa độ dài cạnh của hình vuông như vậy. 

Điểm tự do chính của bài toán là chúng ta có thể vừa chọn bất kỳ tập hợp con tấm ván nào vừa hoán vị chúng một cách tùy ý. Điều này loại bỏ mọi ràng buộc về vị trí khỏi mảng ban đầu và giảm cấu trúc thành một tập hợp nhiều chiều cao. 

Giới hạn ràng buộc là nhỏ, tối đa 1000 bảng cho mỗi trường hợp thử nghiệm và nhiều nhất là 10 trường hợp thử nghiệm. Điều này cho phép các giải pháp bậc hai hoặc O(n log n), nhưng bất kỳ giải pháp khối nào hoặc liên quan đến mô phỏng nặng lặp đi lặp lại sẽ không cần thiết. Quét tuyến tính hoặc lý luận dựa trên tần số là đủ. 

Một trường hợp thất bại tinh tế do lý luận ngây thơ xuất hiện khi chỉ tập trung vào chiều cao tối đa hoặc tổng số tiền. Ví dụ, với độ cao`[5, 1, 1, 1, 1]`, Thật hấp dẫn khi nghĩ rằng có thể có hình vuông 5×5 vì tồn tại số 5. ​​Tuy nhiên, chỉ có một cột có chiều cao ít nhất là 5, vì vậy chúng ta không thể tạo thành hình vuông có chiều rộng 5. Câu trả lời đúng là 1. 

Một trường hợp gây hiểu lầm khác là khi có nhiều tấm ván cao nhưng không đủ. Vì`[4, 4, 4, 1, 1]`, người ta có thể mong đợi một hình vuông 4×4, nhưng chỉ có ba tấm ván có chiều cao ít nhất là 4, nên chiều rộng 4 là không thể. 

Những ví dụ này cho thấy rằng cả hai ràng buộc về chiều cao và số lượng phải được thỏa mãn đồng thời. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi kích thước hình vuông k có thể. Đối với mỗi k, chúng tôi kiểm tra xem liệu chúng tôi có thể chọn ít nhất k tấm ván có chiều cao ít nhất là k hay không. Nếu có thì có thể tạo thành một hình vuông k×k bằng cách lấy k tấm ván cao đó và sắp xếp chúng liên tiếp. 

Việc kiểm tra một k đơn lẻ sẽ mất O(n) và việc thử tất cả k cho đến n sẽ dẫn đến O(n²) cho mỗi trường hợp kiểm thử. Với n lên tới 1000, điều này có thể chấp nhận được nhưng không cần thiết. 

Điều quan trọng cần lưu ý là đối với k cố định, chỉ những tấm ván có chiều cao ≥ k mới là vật chất. Nếu có ít nhất k tấm ván như vậy thì chúng ta luôn có thể chọn k tấm ván đó và đặt chúng cạnh nhau. Sự tự do sắp xếp loại bỏ mọi sự phức tạp về mặt hình học; vấn đề giảm xuống một điều kiện đếm. 

Thay vì kiểm tra tất cả các giá trị k một cách độc lập, chúng ta có thể đếm tần số hoặc sắp xếp mảng và tính toán, với mỗi k có thể, có bao nhiêu phần tử ≥ k. Câu trả lời là k lớn nhất thỏa mãn điều kiện này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force hơn k | O(n²) | O(1) | Đã chấp nhận | 
| Sắp xếp/đếm tần số | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán hình học thành bài toán đếm theo độ cao. 

1. Đối với chiều dài cạnh ứng viên cố định k, chúng tôi hiểu yêu cầu là cần k tấm ván có chiều cao ít nhất là k. Điều này là do một hình vuông cạnh k phải có k cột, mỗi cột có chiều cao ít nhất là k. 
2. Đếm xem có bao nhiêu tấm ván thỏa mãn chiều cao ≥ k. Nếu số này ít nhất là k thì hình vuông có kích thước k là khả thi. 
3. Thử tất cả các giá trị k có thể từ 1 đến n và theo dõi giá trị khả thi lớn nhất. 
4. Để tính toán hiệu quả, hãy sắp xếp mảng hoặc xây dựng mảng tần số. Việc sắp xếp cho phép chúng tôi nhanh chóng xác định có bao nhiêu phần tử vượt quá ngưỡng bằng cách sử dụng các vị trí chỉ mục. 
5. Trả về k lớn nhất thỏa mãn điều kiện khả thi. 

Lý do chúng ta có thể bỏ qua các chi tiết sắp xếp một cách an toàn là vì sau khi chọn được k tấm ván hợp lệ, chúng ta luôn có thể đặt chúng liên tiếp. Không hạn chế về khoảng cách hay thứ tự nên tính khả thi chỉ phụ thuộc vào số lượng chứ không phụ thuộc vào cơ cấu. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là một hình vuông k×k yêu cầu k điểm tựa dọc độc lập có chiều cao ít nhất là k. Vì các tấm ván có thể được sắp xếp lại một cách tự do nên bất kỳ tập hợp con nào được chọn đều có thể được đặt liền kề nhau. Điều này loại bỏ các ràng buộc về không gian và giảm vấn đề xuống việc xác minh sự bất bình đẳng đơn giản giữa ngưỡng và số đếm. Không có cấu hình nào vi phạm điều kiện này một khi nó được thỏa mãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())
    for _ in range(k):
        n = int(input())
        a = list(map(int, input().split()))
        
        a.sort()
        
        ans = 0
        for i in range(1, n + 1):
            # number of elements >= i
            # in sorted array, this is n - first index where a[idx] >= i
            lo, hi = 0, n
            while lo < hi:
                mid = (lo + hi) // 2
                if a[mid] >= i:
                    hi = mid
                else:
                    lo = mid + 1
            cnt = n - lo
            if cnt >= i:
                ans = i
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp sắp xếp chiều cao của tấm ván để chúng tôi có thể tính toán một cách hiệu quả số lượng giá trị vượt quá từng ngưỡng ứng cử viên. Đối với mỗi cạnh hình vuông tiềm năng i, chúng tôi tìm kiếm nhị phân vị trí đầu tiên có chiều cao ít nhất là i và lấy số đếm từ vị trí đó. 

Sự tinh tế chính là diễn giải chính xác điều kiện: chúng tôi không trực tiếp xây dựng một hình vuông hình học mà xác minh rằng tồn tại những tấm ván đủ cao để hỗ trợ nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[4, 3, 1, 4, 5]`Chúng tôi đánh giá các giá trị k có thể có: 

| k | số lượng a[i] ≥ k | khả thi | 
| --- | --- | --- | 
| 1 | 5 | vâng | 
| 2 | 4 | vâng | 
| 3 | 4 | vâng | 
| 4 | 3 | không | 
| 5 | 1 | không | 

Giá trị khả thi tối đa là 3. 

Điều này cho thấy rằng mặc dù chiều cao là 5 tồn tại nhưng nó không chiếm ưu thế trong câu trả lời vì chiều rộng yêu cầu nhiều tấm ván cao. 

### Ví dụ 2 

đầu vào:`[4, 4, 4, 4]`| k | số lượng a[i] ≥ k | khả thi | 
| --- | --- | --- | 
| 1 | 4 | vâng | 
| 2 | 4 | vâng | 
| 3 | 4 | vâng | 
| 4 | 4 | vâng | 

Câu trả lời là 4, vì tất cả các tấm ván đều đủ cao để đỡ bất kỳ hình vuông nào có kích thước tối đa là 4. 

Điều này chứng tỏ trường hợp chiều cao đồng đều tối đa hóa kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | việc sắp xếp chiếm ưu thế, tìm kiếm nhị phân trên k là tổng O(n log n) | 
| Không gian | O(n) | lưu trữ mảng | 

Cho n ≤ 1000 và k ≤ 10, điều này dễ dàng nằm trong giới hạn. Ngay cả cách tiếp cận O(n²) cũng có thể vượt qua, nhưng cách tính dựa trên sắp xếp sẽ rõ ràng và trực tiếp hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            a.sort()
            ans = 0
            for i in range(1, n + 1):
                lo, hi = 0, n
                while lo < hi:
                    mid = (lo + hi) // 2
                    if a[mid] >= i:
                        hi = mid
                    else:
                        lo = mid + 1
                if n - lo >= i:
                    ans = i
            print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""4
5
4 3 1 4 5
4
4 4 4 4
3
1 1 1
5
5 5 1 1 5
""") == """3
4
1
3"""

# all equal
assert run("""1
5
2 2 2 2 2
""") == "2"

# single element
assert run("""1
1
10
""") == "1"

# skewed distribution
assert run("""1
5
5 1 1 1 1
""") == "1"

# tight boundary
assert run("""1
6
3 3 3 3 3 3
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | giá trị đơn | hình vuông tối ưu thống nhất | 
| phần tử đơn | 1 | trường hợp ranh giới tối thiểu | 
| phân phối lệch | 1 | không đủ chiều rộng mặc dù cao tối đa | 
| giá trị trung bình thống nhất | 3 | hành vi chia tỷ lệ chính xác | 

## Vỏ cạnh 

Một trường hợp phổ biến là khi tồn tại một tấm ván rất lớn giữa nhiều tấm ván nhỏ. Ví dụ,`[100, 1, 1, 1, 1]`nên trả về 1 vì chỉ một tấm ván có thể chịu được chiều cao 100. 

Một trường hợp khác là khi tất cả các tấm ván đều giống hệt nhau. Vì`[3, 3, 3]`, câu trả lời là 3 vì cả hai ràng buộc về chiều cao và số lượng đều căn chỉnh hoàn hảo ở mọi ngưỡng cho đến 3.
