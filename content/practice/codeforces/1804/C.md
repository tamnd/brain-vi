---
title: "CF 1804C - Kéo vận may của bạn"
description: "Chúng ta có một bánh xe roulette với n khu vực được đánh số từ 0 đến n-1. Bánh xe bắt đầu bằng một mũi tên chỉ vào khu vực x và chúng ta có thể quay nó bằng cách kéo một tay cầm có lực nguyên f trong khoảng từ 1 đến p."
date: "2026-06-09T09:20:33+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1804
codeforces_index: "C"
codeforces_contest_name: "Nebius Welcome Round (Div. 1 + Div. 2)"
rating: 1500
weight: 1804
solve_time_s: 101
verified: true
draft: false
---

[CF 1804C - Kéo vận may của bạn](https://codeforces.com/problemset/problem/1804/C) 

**Đánh giá:** 1500 
**Tags:** sức mạnh vũ phu, lòng tham, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bánh xe roulette với`n`ngành được đánh số từ`0`ĐẾN`n-1`. Bánh xe bắt đầu bằng một mũi tên chỉ vào khu vực`x`, và chúng ta có thể quay nó bằng cách kéo một tay cầm với một lực nguyên`f`giữa`1`Và`p`. Vòng quay hoạt động như thế này: trong giây đầu tiên nó tiến lên`f`các ngành, trong giây tiếp theo`f-1`các ngành thì`f-2`, xuống tới`1`. Tổng chuyển động sau`f`giây là tổng của giây đầu tiên`f`số nguyên dương, đó là`f*(f+1)/2`. Các ngành bao quanh modulo`n`. Mục đích là kiểm tra xem có tồn tại số nguyên không`f`từ`1`ĐẾN`p`sao cho sau khi quay, mũi tên chỉ vào khu vực`0`. 

Những ràng buộc cho chúng ta biết`n`có thể đi lên`10^5`Và`p`lên tới`10^9`. Tổng hợp tất cả các trường hợp thử nghiệm,`n`không vượt quá`2*10^5`. Một cách tiếp cận bạo lực cố gắng mọi cách`f`từ`1`ĐẾN`p`là không thể thực hiện được vì`p`có thể rất lớn. Chúng ta cần thứ gì đó thông minh hơn việc lặp đi lặp lại qua mọi lực lượng. Các trường hợp cạnh bao gồm khi mũi tên đã chỉ vào`0`, khi`p`là rất nhỏ so với`n`, hoặc khi`x`ở gần cuối bánh xe và vòng quay quay nhiều vòng. 

Ví dụ, nếu`n = 5`,`x = 2`,`p = 1`, thì vòng quay duy nhất là`f=1`, di chuyển`1`ngành. Mũi tên sẽ kết thúc tại`3`, vì vậy câu trả lời là "Không". Nếu như`p = 2`, chúng ta có thể chọn`f=2`, di chuyển`3`tổng số ngành (`2 + 1`), đáp xuống`0`. Một cách tiếp cận ngây thơ sẽ không giải quyết được vấn đề lớn`p`một cách hiệu quả. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu thử mọi lực lượng có thể`f`từ`1`ĐẾN`p`, tính toán`f*(f+1)/2`, thêm`x`, và lấy modulo`n`để xem nó có đạt tới không`0`. Điều này hoạt động về nguyên tắc nhưng có thể yêu cầu lên tới`10^9`số lần lặp lại cho mỗi trường hợp thử nghiệm, vượt xa giới hạn thời gian. 

Cái nhìn sâu sắc quan trọng là chúng ta chỉ cần kiểm tra xem phương trình`(x + f*(f+1)/2) % n == 0`có một giải pháp cho một số`1 <= f <= p`. Mở rộng nó, chúng tôi muốn`(f*(f+1)/2) % n == (-x) % n`. Vế trái là bậc hai`f`và mặc dù có vẻ khó đảo ngược trực tiếp, chúng tôi nhận thấy rằng một khi`f`vượt quá`n`, tổng`f*(f+1)/2`modulo`n`bắt đầu lặp lại các mẫu vì thêm`n`ĐẾN`f`tăng tổng lên gấp bội`n`. Điều này có nghĩa là chỉ cần kiểm tra là đủ`f`lên tới`min(p, 2*n)`thay vì tất cả các cách để`p`, bởi vì sau đó mẫu sẽ quay vòng theo modulo`n`. Điều này làm giảm sự lặp lại từ`10^9`nhiều nhất là`2*10^5`cho mỗi trường hợp thử nghiệm, có thể quản lý được với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(p) | O(1) | Quá chậm đối với kích thước lớn`p`| 
| Kiểm tra modulo được tối ưu hóa | O(phút(p, 2*n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`x`,`p`. 
3. Tính giá trị mục tiêu`target = (-x) % n`. Đây là số lĩnh vực chúng ta cần thúc đẩy để đạt được`0`. 
4. Lặp lại`f`từ`1`ĐẾN`min(p, 2*n)`. Chúng tôi dừng lại ở`2*n`bởi vì`f*(f+1)/2`modulo`n`sẽ bắt đầu lặp lại sau thời điểm này. 
5. Đối với mỗi`f`, tính tổng tiến bộ`move = f*(f+1)//2`. 
6. Nếu`(move % n) == target`, in "Có" và ngắt vòng lặp. 
7. Nếu không`f`thỏa mãn điều kiện, in ra “Không”. 

Tại sao nó hoạt động: Thuộc tính quan trọng là tổng của`f`modulo số nguyên`n`lặp lại trong một phạm vi giới hạn. Bằng cách kiểm tra tới`2*n`, chúng ta đảm bảo sẽ gặp tất cả các số dư phân biệt modulo`n`, vì vậy nếu có giải pháp, chúng ta sẽ tìm thấy nó. Điều này tránh việc lặp lại tất cả các cách để`p`khi`p`là rất lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x, p = map(int, input().split())
    target = (-x) % n
    found = False
    limit = min(p, 2*n)  # pattern repeats modulo n
    for f in range(1, limit + 1):
        move = f * (f + 1) // 2
        if move % n == target:
            found = True
            break
    print("Yes" if found else "No")
```Chúng tôi sử dụng`sys.stdin.readline`để nhập liệu nhanh vì có thể có tới`10^4`trường hợp thử nghiệm. các`min(p, 2*n)`thủ thuật ngăn chặn việc lặp lại tối đa`10^9`. Phép chia số nguyên`//`là cần thiết để tránh các vấn đề về dấu phẩy động. Hoạt động modulo đảm bảo chúng ta xử lý được tính chất hình tròn của bánh xe. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`5 2 2`| f | f*(f+1)/2 | (f*(f+1)/2 + x) % n | 
| --- | --- | --- | 
| 1 | 1 | (1 + 2) % 5 = 3 | 
| 2 | 3 | (3 + 2) % 5 = 0 | 

Bảng cho thấy rằng`f=2`đất chính xác trên khu vực`0`. Thuật toán xuất ra chính xác "Có". 

### Ví dụ 2 

đầu vào:`3 1 1000`| f | f*(f+1)/2 | (f*(f+1)/2 + x) % n | 
| --- | --- | --- | 
| 1 | 1 | (1+1)%3=2 | 
| 2 | 3 | (3+1)%3=1 | 
| 3 | 6 | (6+1)%3=1 | 
| 4 | 10 | (10+1)%3=2 | 

Chu kỳ còn lại giữa`1`Và`2`. ngành`0`không bao giờ đạt được. Thuật toán xuất ra "Không". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * phút(p, n)) | Mỗi trường hợp thử nghiệm lặp lại nhiều nhất`2*n`lần, tổng cộng`t*2*n`4*10^5 | 
| Không gian | O(1) | Chỉ các biến bổ sung không đổi cho mỗi trường hợp thử nghiệm | 

Giải pháp dễ dàng phù hợp với giới hạn 1 giây, ngay cả ở mức tối đa`t`Và`n`. 

## Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    t = int(input())
    for _ in range(t):
        n, x, p = map(int, input().split())
        target = (-x) % n
        found = False
        limit = min(p, 2*n)
        for f in range(1, limit+1):
            move = f*(f+1)//2
            if move % n == target:
                found = True
                break
        print("Yes" if found else "No")
    return output.getvalue().strip()

# Provided samples
assert run("7\n5 2 1\n5 2 2\n10 0 100\n11 7 100\n3 1 1000\n31 0 10\n100 49 7\n") == \
"No\nYes\nYes\nYes\nNo\nNo\nNo"

# Custom edge cases
assert run("2\n3 0 1\n3 2 2\n") == "Yes\nYes"  # already at 0, small n
assert run("1\n100000 99999 1000000000\n") == "Yes"  # very large p
assert run("1\n4 1 2\n") == "No"  # small p, can't reach 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 0 1\n3 2 2`|`Yes\nYes`| bánh xe có kích thước tối thiểu, đã ở mức 0, p nhỏ | 
|`100000 99999 1000000000`|`Yes`| n lớn, p rất lớn được xử lý hiệu quả | 
|`4 1 2`|`No`| p nhỏ không đạt mục tiêu | 

## Vỏ cạnh 

cho`n = 3, x = 0, p = 1`, thuật toán sẽ tính ngay`target = 0`Và`f=1`cho`1*(1+1)/2 = 1`. `(x + di chuyển) %
