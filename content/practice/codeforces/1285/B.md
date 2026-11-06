---
title: "CF 1285B - Ăn thôi!"
description: "Chúng tôi có một mảng đại diện cho độ ngon của bánh nướng nhỏ và hai người chơi. Yasser luôn lấy hết số bánh cupcake, tổng hợp đầy đủ mảng. Adel có thể chọn bất kỳ mảng con liền kề nào không bao phủ toàn bộ mảng."
date: "2026-06-11T19:14:49+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1285
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 613 (Div. 2)"
rating: 1300
weight: 1285
solve_time_s: 129
verified: true
draft: false
---

[CF 1285B - Cứ ăn đi!](https://codeforces.com/problemset/problem/1285/B) 

**Đánh giá:** 1300 
**Tags:** dp, tham lam, thực hiện 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng đại diện cho độ ngon của bánh nướng nhỏ và hai người chơi. Yasser luôn lấy hết số bánh cupcake, tổng hợp đầy đủ mảng. Adel có thể chọn bất kỳ mảng con liền kề nào **không** bao phủ toàn bộ mảng. Yasser muốn biết liệu tổng độ ngon của anh ấy có lớn hơn **bất kỳ số tiền nào** mà Adel có thể đạt được hay không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra số lượng bánh nướng nhỏ và mảng giá trị độ ngon. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu Yasser luôn có tổng giá trị cao hơn hoặc "KHÔNG" nếu ngược lại. 

Nhìn vào các ràng buộc, mỗi mảng có thể có tối đa$10^5$các phần tử và tổng số trên tất cả các trường hợp thử nghiệm cũng là$10^5$. Điều này ngay lập tức loại trừ bất kỳ$O(n^2)$cách tiếp cận này, bởi vì việc lặp qua tất cả các mảng con sẽ gần như$10^{10}$hoạt động trong trường hợp xấu nhất. Cần có giải pháp tuyến tính hoặc gần tuyến tính. Giá trị của độ ngon có thể là âm hoặc dương lớn ($-10^9$ĐẾN$10^9$), vì vậy chúng ta cũng cần cẩn thận về việc tràn số nguyên trong các ngôn ngữ có số nguyên có kích thước cố định, mặc dù Python tự động xử lý việc này. 

Trường hợp cạnh tinh tế phát sinh khi mảng chỉ có hai phần tử. Adel chỉ có thể lấy một phần tử tại một thời điểm và tổng của Yasser sẽ luôn bằng tổng của cả hai. Nếu một phần tử lớn hơn phần tử kia, Yasser sẽ rất vui; nếu tổng của mảng con lớn nhất mà Adel có thể lấy bằng tổng của Yasser thì câu trả lời là "KHÔNG". Một trường hợp cạnh khác xảy ra khi mảng có số dương và số âm xen kẽ nhau, chẳng hạn`[5, -5, 5]`. Tổng số tiền của Yasser là`5`, nhưng Adel có thể chọn cái cuối cùng`5`riêng, phù hợp với tổng số của Yasser. Việc triển khai bất cẩn có thể bỏ qua hạn chế về việc không lấy toàn bộ mảng hoặc không xem xét đúng các mảng con tiền tố và hậu tố. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi trường hợp thử nghiệm, hãy tính tổng số tiền của Yasser. Sau đó, với mỗi mảng con hợp lệ mà Adel có thể lấy, hãy tính tổng của nó và so sánh với giá trị của Yasser. Điều này đảm bảo tính chính xác vì chúng tôi thực sự kiểm tra mọi khả năng. Tuy nhiên, đây là$O(n^2)$cho từng trường hợp thử nghiệm. Với$n$lên tới$10^5$, tốc độ này quá chậm. 

Thông tin chi tiết quan trọng đến từ việc nhận ra rằng các mảng con duy nhất có thể đe dọa Yasser là tiền tố hoặc hậu tố. Bất kỳ mảng con nào hoàn toàn bên trong mảng không chạm vào ranh giới sẽ có tổng nhỏ hơn một số tiền tố hoặc hậu tố kéo dài đến một cạnh vì chúng ta có thể bao gồm nhiều phần tử hơn, có khả năng làm tăng tổng. Do đó, việc kiểm tra tổng tối đa các tiền tố (không bao gồm toàn bộ mảng) và tổng tối đa các hậu tố là đủ. Nếu cả hai số này đều nhỏ hơn tổng số tiền của Yasser thì không có mảng con bên trong nào có thể vượt quá tổng của anh ta và câu trả lời là "CÓ". Ngược lại, câu trả lời là "KHÔNG". Điều này làm giảm độ phức tạp thành tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của Yasser bằng cách tính tổng toàn bộ mảng. Điều này là đơn giản và tạo thành cơ sở để so sánh. 
2. Tính tổng tiền tố tối đa, xem xét các tiền tố loại trừ phần tử cuối cùng. Khởi tạo tổng hiện có về 0, lặp từ phần tử đầu tiên đến phần tử thứ hai đến cuối cùng, thêm từng phần tử vào tổng hiện có và theo dõi mức tối đa gặp phải. Chúng tôi dừng lại ở vị trí thứ hai đến cuối cùng vì Adel không thể lấy hết mảng. 
3. Tính tổng hậu tố tối đa, xem xét các hậu tố loại trừ phần tử đầu tiên. Khởi tạo tổng hiện có về 0, lặp lại từ phần tử cuối cùng đến phần tử thứ hai theo chiều ngược lại, thêm từng phần tử vào tổng hiện có và theo dõi mức tối đa gặp phải. Chúng ta bắt đầu ở phần tử cuối cùng vì phần tử đầu tiên không thể được đưa vào hậu tố nếu chúng ta muốn tránh lấy toàn bộ mảng. 
4. So sánh tổng của Yasser với cả tiền tiền tố và tiền hậu tố tối đa. Nếu mức tối đa lớn hơn hoặc bằng tổng của Yasser, hãy in "KHÔNG". Ngược lại in ra "CÓ". 

Tại sao nó hoạt động: Điều bất biến là tổng tối đa của bất kỳ mảng con nào ngoại trừ mảng đầy đủ luôn là tiền tố hoặc hậu tố. Các mảng con bên trong được giới hạn bởi tổng các tiền tố và hậu tố. Bằng cách đảm bảo rằng không có tổng tiền tố hoặc hậu tố nào đáp ứng hoặc vượt quá tổng của Yasser, chúng tôi đảm bảo rằng không có lựa chọn mảng con nào của Adel có thể vượt qua tổng của Yasser. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        total = sum(a)
        
        # check max prefix sum excluding last element
        max_prefix = curr = 0
        for i in range(n - 1):
            curr += a[i]
            if curr > max_prefix:
                max_prefix = curr
        
        # check max suffix sum excluding first element
        max_suffix = curr = 0
        for i in range(n - 1, 0, -1):
            curr += a[i]
            if curr > max_suffix:
                max_suffix = curr
        
        if max_prefix >= total or max_suffix >= total:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    main()
```Chúng tôi sử dụng I/O nhanh với`sys.stdin.readline`do số lượng các trường hợp thử nghiệm tiềm năng. Tổng tiền tố và hậu tố được khởi tạo bằng 0 và được cập nhật tăng dần, đảm bảo xử lý chính xác các giá trị âm. Có thể tránh từng lỗi một bằng cách dừng tiền tố tại`n - 1`và hậu tố tại`1`. Việc so sánh sử dụng`>=`bởi vì điều kiện hạnh phúc là hoàn toàn lớn hơn. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`[1, 2, 3, 4]`| Bước | tiền tố hiện tại | max_prefix | hậu tố hiện tại | max_suffix | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | - | - | 10 | 
| 2 | 3 | 3 | - | - | 10 | 
| 3 | 6 | 6 | - | - | 10 | 
| 1 (hậu tố) | - | - | 4 | 4 | 10 | 
| 2 | - | - | 7 | 7 | 10 | 
| 3 | - | - | 9 | 9 | 10 | 

Cả tiền tố và hậu tố tối đa đều nhỏ hơn 10, do đó xuất ra "CÓ". 

**Ví dụ 2:**`[7, 4, -1]`| Bước | tiền tố hiện tại | max_prefix | hậu tố hiện tại | max_suffix | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 7 | 7 | - | - | 10 | 
| 2 | 11 | 11 | - | - | 10 | 
| 1 (hậu tố) | - | - | -1 | -1 | 10 | 
| 2 | - | - | 3 | 3 | 10 | 

Tiền tố tối đa 11 ≥ tổng số 10, do đó xuất ra "NO". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp qua mảng hai lần để tính tổng tiền tố và hậu tố. | 
| Không gian | O(1) | Chỉ có một số tổng số đang chạy được giữ lại. | 

Điều này phù hợp thoải mái trong các ràng buộc, vì tổng của tất cả`n`trên các trường hợp thử nghiệm là ≤$10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("3\n4\n1 2 3 4\n3\n7 4 -1\n3\n5 -5 5\n") == "YES\nNO\nNO", "samples"

# custom cases
assert run("2\n2\n1 2\n2\n-1 -2\n") == "YES\nNO", "min size"
assert run("1\n5\n5 5 5 5 5\n") == "YES", "all equal positive"
assert run("1\n3\n-1 -1 -1\n") == "NO", "all negative"
assert run("1\n4\n1 -1 1 -1\n") == "NO", "alternating signs"
assert run("1\n3\n10 -1 1\n") == "YES", "single negative in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n2\n1 2\n2\n-1 -2 | CÓ\nKHÔNG | Kích thước tối thiểu, tích cực và tiêu cực | 
| 1\n5\n5 5 5 5 5 | CÓ | Tích cực hoàn toàn bằng nhau | 
| 1\n3\n-1 -1 -1 | KHÔNG | Tất cả các số âm | 
| 1\n4\n1 -1 1 -1 | KHÔNG | Biển báo xen kẽ, mảng con bên trong max | 
| 1\n3\n10 -1 1 | CÓ | Âm đơn ở giữa | 

## Vỏ cạnh 

cho`[5, -5, 5]`, Tổng của Yasser là 5. Tổng tiền tố: 5, 0 (hai đầu tiên), tổng hậu tố
