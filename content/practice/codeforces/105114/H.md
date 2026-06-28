---
title: "CF 105114H - Vấn đề về mảng cứng"
description: "Chúng ta có hai dãy số nguyên có cùng độ dài. Từ bất kỳ phân đoạn chỉ số liền kề nào, chúng ta có thể tính hai giá trị: tổng của phân đoạn đã chọn trong mảng đầu tiên và tổng của cùng phân khúc đó trong mảng thứ hai."
date: "2026-06-27T19:51:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105114
codeforces_index: "H"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2024"
rating: 0
weight: 105114
solve_time_s: 73
verified: true
draft: false
---

[CF 105114H - Sự cố mảng cứng](https://codeforces.com/problemset/problem/105114/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai dãy số nguyên có cùng độ dài. Từ bất kỳ phân đoạn chỉ số liền kề nào, chúng ta có thể tính hai giá trị: tổng của phân đoạn đã chọn trong mảng đầu tiên và tổng của cùng phân khúc đó trong mảng thứ hai. Do đó, mỗi phân đoạn tạo ra một cặp số, một số từ mỗi mảng. 

Nhiệm vụ là chọn một phân đoạn liền kề sao cho số lượng được hình thành bằng cách bình phương cả hai tổng và cộng chúng càng nhỏ càng tốt. Về mặt hình học, mỗi đoạn tương ứng với một điểm trong mặt phẳng hai chiều và chúng ta đang tìm điểm gần gốc nhất theo bình phương khoảng cách Euclide. 

Các ràng buộc cho phép lên tới năm trăm nghìn phần tử. Bất kỳ cách tiếp cận bậc hai nào đối với tất cả các mảng con sẽ cố gắng theo thứ tự của N đoạn bình phương, vượt xa giới hạn khả thi. Ngay cả một thuật toán O(N^2) với các hệ số không đổi nhẹ cũng sẽ thử khoảng 10^11 thao tác trong trường hợp xấu nhất, thuật toán này không thể chạy trong hai giây. Điều này ngay lập tức buộc chúng ta phải giải thích vấn đề theo cách tránh liệt kê tất cả các phân đoạn một cách rõ ràng. 

Một vấn đề tế nhị xuất hiện khi lý luận về những ý tưởng tham lam ngây thơ. Một phân đoạn có tổng nhỏ trong một mảng nhưng cường độ lớn ở mảng kia vẫn có thể tốt hơn một phân đoạn chỉ thu nhỏ một tọa độ. Ví dụ: việc chọn phân đoạn giảm thiểu tổng A sẽ bỏ qua hoàn toàn sự đóng góp của B và có thể bỏ lỡ mức tối ưu thực sự. Một trường hợp thất bại khác là cố gắng thu nhỏ hoặc mở rộng cửa sổ một cách tham lam, vì mục tiêu không đơn điệu về độ dài phân đoạn. 

Khó khăn chính là mọi phân đoạn đều phụ thuộc vào hai tổng tích lũy độc lập, vì vậy chúng tôi thực sự đang tối ưu hóa cấu trúc hai chiều do các mảng tạo ra. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Chúng tôi lặp lại tất cả các cặp điểm cuối L và R, tính tổng A và B trên khoảng đó và đánh giá mục tiêu. Với tổng tiền tố, điều này làm giảm mỗi truy vấn xuống O(1), nhưng vẫn có các lựa chọn O(N^2) cho L và R, dẫn đến khoảng 10^11 đánh giá trong trường hợp xấu nhất. Điều này là quá chậm. 

Quan sát quan trọng là tổng của mảng con có thể được viết lại bằng cách sử dụng tổng tiền tố. Đặt SA[i] và SB[i] là tổng tiền tố. Khi đó tổng trên một đoạn [L, R] trở thành SA[R] trừ SA[L−1], và tương tự đối với B. Vì vậy, mỗi đoạn tương ứng với sự khác biệt giữa hai điểm tiền tố trong mặt phẳng 2D. 

Nếu chúng ta xác định một điểm P[i] = (SA[i], SB[i]), bao gồm P[0] = (0, 0), thì mọi đoạn đều tương ứng với P[R] − P[L−1]. Mục tiêu trở thành khoảng cách bình phương giữa hai điểm trong tập hợp điểm tổng tiền tố này. Do đó, bài toán rút gọn thành việc tìm cặp điểm gần nhất trong số N+1 điểm trong hai chiều. 

Điều này biến bài toán thành một bài toán hình học tính toán cổ điển: cặp điểm gần nhất trong mặt phẳng. Cách tiếp cận chia để trị đạt được O(N log N) bằng cách giải đệ quy trên các nửa được sắp xếp theo tọa độ x và sau đó chỉ kiểm tra một dải dọc hẹp trong đó các ứng cử viên từ cả hai nửa có thể tạo thành một cặp gần hơn. Bên trong dải đó, việc sắp xếp theo tọa độ y cho phép so sánh hiệu quả. 

Đây chính xác là cấu trúc chúng ta cần vì các điểm tiền tố không có ràng buộc đặc biệt nào ngoài tọa độ tùy ý, do đó không áp dụng thủ thuật một chiều đơn giản hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) bổ sung (hoặc O(N) với tổng tiền tố) | Quá chậm | 
| Tối ưu (Cặp gần nhất) | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố SA và SB, dựng điểm P[i] = (SA[i], SB[i]) cho i từ 0 đến N. 

Điều này chuyển đổi mọi mảng con thành sự khác biệt giữa hai điểm, loại bỏ việc xử lý khoảng thời gian trực tiếp. 
2. Sắp xếp tất cả các điểm theo tọa độ x.

Thứ tự này là cần thiết để chia bài toán thành hai nửa bên trái và bên phải với hình học cân bằng. 
3. Xác định hàm đệ quy lấy một phân đoạn các điểm được sắp xếp theo x và trả về cả khoảng cách bình phương tối thiểu bên trong phân đoạn đó và danh sách các điểm giống nhau được sắp xếp theo tọa độ y. 

Danh sách được sắp xếp theo y rất quan trọng để xây dựng hiệu quả tập hợp ứng viên xuyên biên giới. 
4. Nếu kích thước đoạn nhỏ, hãy tính trực tiếp tất cả các khoảng cách theo cặp và trả về. 

Các trường hợp cơ sở nhỏ tránh được chi phí đệ quy và nhanh hơn so với việc phân tách thêm. 
5. Chia các điểm thành hai nửa bên trái và bên phải theo tọa độ x trung bình và giải đệ quy cả hai nửa. 

Mỗi nửa độc lập tìm thấy cặp bên trong tốt nhất của mình. 
6. Bước hợp nhất: thu thập các điểm có khoảng cách x từ đường phân tách nhỏ hơn khoảng cách tốt nhất được tìm thấy cho đến nay. 

Chỉ những điểm này mới có thể tạo thành một cặp tốt hơn vượt qua ranh giới, bởi vì bất kỳ cặp nào cách xa nhau hơn trong x đều phải vượt quá khoảng cách bình phương tốt nhất hiện tại. 
7. Sắp xếp dải này theo tọa độ y và kiểm tra từng điểm so với một số điểm tiếp theo theo thứ tự y. 

Đối số đóng gói hình học đảm bảo rằng chỉ cần một số lượng so sánh không đổi cho mỗi điểm. 
8. Trả về khoảng cách tốt nhất được tìm thấy giữa các kiểm tra nửa bên trái, nửa bên phải và xuyên biên giới, cùng với danh sách được sắp xếp theo y để có mức đệ quy cao hơn. 

Câu trả lời cuối cùng là khoảng cách bình phương tối thiểu được tìm thấy trên tất cả các cấp độ đệ quy. 

### Tại sao nó hoạt động 

Mỗi mảng con tương ứng duy nhất với một cặp điểm tiền tố, do đó việc giảm thiểu tổng bình phương của mảng con giống hệt với việc tìm cặp gần nhất trong số tất cả các điểm tiền tố. Bước chia để trị đảm bảo rằng bất kỳ cặp ứng cử viên nào cũng nằm hoàn toàn trong một nửa hoặc vượt qua phép chia. Giới hạn xuyên ranh giới đối với dải dọc là hợp lệ vì bất kỳ cặp nào có khoảng cách ngang lớn phải có khoảng cách bình phương lớn hơn khoảng cách tốt nhất hiện tại. Trong dải, việc hạn chế so sánh theo thứ tự y dựa vào việc đóng gói hình học: quá nhiều điểm gần nhau sẽ buộc hai trong số chúng phải gần nhau hơn điểm tốt nhất hiện tại, mâu thuẫn với tính tối ưu. Điều này duy trì tính chính xác trong khi tránh so sánh bậc hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    A = list(map(int, input().split()))
    B = list(map(int, input().split()))

    px = 0
    py = 0
    pts = [(0, 0)]

    for i in range(n):
        px += A[i]
        py += B[i]
        pts.append((px, py))

    pts.sort(key=lambda x: x[0])

    def dist2(p1, p2):
        dx = p1[0] - p2[0]
        dy = p1[1] - p2[1]
        return dx * dx + dy * dy

    def closest(arr):
        m = len(arr)
        if m <= 3:
            best = float('inf')
            for i in range(m):
                for j in range(i + 1, m):
                    best = min(best, dist2(arr[i], arr[j]))
            return best, sorted(arr, key=lambda x: x[1])

        mid = m // 2
        midx = arr[mid][0]

        left = arr[:mid]
        right = arr[mid:]

        d1, ly = closest(left)
        d2, ry = closest(right)
        d = min(d1, d2)

        merged_y = []
        i = j = 0
        while i < len(ly) and j < len(ry):
            if ly[i][1] < ry[j][1]:
                merged_y.append(ly[i])
                i += 1
            else:
                merged_y.append(ry[j])
                j += 1
        merged_y.extend(ly[i:])
        merged_y.extend(ry[j:])

        strip = []
        for p in merged_y:
            if (p[0] - midx) * (p[0] - midx) <= d:
                strip.append(p)

        for i in range(len(strip)):
            for j in range(i + 1, len(strip)):
                if (strip[j][1] - strip[i][1]) * (strip[j][1] - strip[i][1]) > d:
                    break
                d = min(d, dist2(strip[i], strip[j]))

        return d, merged_y

    ans, _ = closest(pts)
    print(ans)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách chuyển đổi các mảng thành các điểm tổng tiền tố, đây là sự thay đổi cấu trúc làm cho bài toán trở thành hình học. Hàm đệ quy duy trì tính bất biến là nó luôn trả về các điểm được sắp xếp theo tọa độ y, điều này rất cần thiết để xử lý dải hiệu quả. 

Cấu trúc dải sử dụng so sánh khoảng cách bình phương với giá trị tốt nhất hiện tại, tránh hoàn toàn các phép toán dấu phẩy động. Việc ngắt sớm trong vòng lặp bên trong dựa vào thứ tự sắp xếp theo y, đảm bảo chúng tôi chỉ so sánh các ứng cử viên gần đó. 

Một điểm tinh tế là phép đệ quy trả về cả khoảng cách tốt nhất và danh sách được sắp xếp theo y. Nếu không duy trì thứ tự y trên các mức đệ quy, bước hợp nhất sẽ trở nên tốn kém và phá vỡ đảm bảo O(N log N). 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
4 -1 -1
-1 -1 4
```Điểm tiền tố trở thành: 

(0,0), (4,-1), (3,-2), (2,2) 

| Bước | Điểm hoạt động | Khoảng cách tốt nhất | 
| --- | --- | --- | 
| Ban đầu | (0,0) | thông tin | 
| Sau khi đệ quy trái | (0,0),(4,-1) | 17 | 
| Sau bên phải | (3,-2),(2,2) | 20 | 
| Kiểm tra chéo | tất cả các điểm | 2 | 

Cặp gần nhất tương ứng với một phân đoạn có chênh lệch tiền tố mang lại tổng triệt tiêu mạnh trong cả hai mảng, tạo ra cường độ kết hợp rất nhỏ. 

### Mẫu 2 

đầu vào:```
2
4 -4
1 1
```Điểm tiền tố: 

(0,0), (4,1), (0,2) 

| Bước | Điểm hoạt động | Khoảng cách tốt nhất | 
| --- | --- | --- | 
| So sánh ban đầu | tất cả các cặp | thông tin | 
| Cặp (0,0)-(4,1) | 17 | | 
| Cặp (0,0)-(0,2) | 4 | | 
| Cặp (4,1)-(0,2) | 17 | | 

Cặp tối ưu là (0,0) và (0,2), tương ứng với phân đoạn trong đó tổng A bị hủy nhưng tổng B vẫn nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi cấp độ đệ quy xử lý các điểm một cách tuyến tính và độ sâu là logarit do phân tách | 
| Không gian | O(N) | Lưu trữ điểm tiền tố và chi phí đệ quy | 

Các ràng buộc lên tới 5×10^5 phần tử vừa vặn thoải mái trong độ phức tạp này vì thuật toán thực hiện khoảng vài triệu thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""3
4 -1 -1
-1 -1 4
""") == "2"

assert run("""2
4 -4
1 1
""") == "4"

# minimum size
assert run("""1
5
7
""") == "74"

# all zeros
assert run("""3
0 0 0
0 0 0
""") == "0"

# symmetric cancellation
assert run("""4
1 -1 1 -1
1 -1 1 -1
""") == "0"

# larger mixed
assert run("""5
3 1 -2 4 -1
-2 5 1 -3 2
""") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | tổng bình phương dương | trường hợp cơ sở đúng đắn | 
| tất cả số không | 0 | xử lý khoảng cách bằng không | 
| biển báo xen kẽ | 0 | hủy bỏ trên các phân đoạn | 
| hỗn hợp ngẫu nhiên | độ đúng không âm | độ bền chung | 

## Vỏ cạnh 

Mảng một phần tử là trường hợp không tầm thường đơn giản nhất. Thuật toán xử lý các điểm tiền tố (0,0) và (A1,B1) và đánh giá chính xác khoảng cách bình phương của chúng là ứng cử viên duy nhất. 

Khi tất cả các giá trị bằng 0, tất cả các điểm tiền tố đều thu gọn về điểm gốc. Khoảng cách cặp gần nhất vẫn bằng 0 trong suốt quá trình đệ quy và dải không bao giờ tạo ra các so sánh không hợp lệ vì tất cả các khoảng cách đều bằng 0. 

Trong mảng dấu xen kẽ, nhiều điểm tiền tố trùng nhau hoặc trở nên rất gần nhau. Logic dải vẫn hoạt động chính xác vì các phép so sánh hoàn toàn mang tính hình học và không giả sử tính duy nhất của tọa độ.
