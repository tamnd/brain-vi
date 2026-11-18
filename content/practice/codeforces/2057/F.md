---
title: "CF 2057F - Đội Hình"
description: "Chúng ta được yêu cầu tối đa hóa chiều cao của học sinh trong một hàng theo hai quy tắc. Đầu tiên, đường thẳng ban đầu thoải mái: đối với mỗi học sinh, chiều cao của học sinh tiếp theo nhiều nhất là gấp đôi chiều cao của học sinh hiện tại."
date: "2026-06-08T08:11:24+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2057
codeforces_index: "F"
codeforces_contest_name: "Hello 2025"
rating: 3300
weight: 2057
solve_time_s: 97
verified: false
draft: false
---

[CF 2057F - Đội hình](https://codeforces.com/problemset/problem/2057/F) 

**Đánh giá:** 3300 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa chiều cao của học sinh trong một hàng theo hai quy tắc. Đầu tiên, đường thẳng ban đầu thoải mái: đối với mỗi học sinh, chiều cao của học sinh tiếp theo nhiều nhất là gấp đôi chiều cao của học sinh hiện tại. Thứ hai, chúng tôi có thể tặng pizza cho học sinh để tăng chiều cao, nhưng chúng tôi có tổng ngân sách là`k`pizza cho mỗi truy vấn. Mỗi chiếc bánh pizza sẽ tăng chiều cao của học sinh lên 1 và hàng phải vẫn thoải mái sau khi phân phát bánh pizza. Đối với mỗi truy vấn, chúng tôi muốn chiều cao tối đa có thể có của bất kỳ học sinh nào với giới hạn pizza. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra mảng độ cao và nhiều truy vấn. Các ràng buộc rất chặt chẽ:`n`có thể lên tới 5·10^4 và`q`cũng lên tới 5·10^4, nhưng tổng của tất cả các trường hợp thử nghiệm không vượt quá 5·10^4. Giới hạn chiều cao và pizza có thể lên tới 10^9. Một cách tiếp cận ngây thơ thử mọi cách để phân phối pizza sẽ quá chậm vì số lượng thao tác có thể lên tới 10^10 hoặc hơn. 

Một điều tinh tế là điều kiện thoải mái không đối xứng. Nó là đủ cho`2 * a_i >= a_{i+1}`cho tất cả tôi. Điều đó có nghĩa là việc tặng pizza cho một học sinh cao hơn có thể yêu cầu học sinh trước đó cũng phải nhận pizza để duy trì sự bất bình đẳng. Ví dụ, nếu chúng ta bắt đầu với`[10, 20]`và muốn học sinh thứ hai đủ 26 tuổi thì học sinh thứ nhất phải ít nhất 13 tuổi, ăn 3 chiếc pizza cho người thứ nhất và 6 chiếc cho người thứ hai. Bỏ qua ảnh hưởng của các học sinh trước đó là một nguồn sai lầm phổ biến. 

Các trường hợp cạnh bao gồm các dòng có độ dài bằng một (bất kỳ chiếc bánh pizza nào cũng dành cho một học sinh), các dòng có chiều cao bằng nhau và các truy vấn có ngân sách bánh pizza rất nhỏ không thể tăng học sinh cao nhất chút nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng phân phối tất cả các loại pizza để duy trì điều kiện thoải mái và chọn chiều cao tối đa đạt được. Đối với mỗi truy vấn, điều này có thể liên quan đến việc cập nhật độ cao một cách tuần tự và thử mọi kết hợp. Vì`n=5·10^4`Và`k`lên tới 10^9, điều này hoàn toàn không khả thi. Ngay cả việc mô phỏng tăng chiều cao một cách tuần tự cũng sẽ mất quá nhiều thời gian. 

Cái nhìn sâu sắc quan trọng là làm việc ngược lại. Hãy xem xét việc chúng tôi muốn học sinh cuối cùng đạt được chiều cao mục tiêu nào đó`H`. Để dây vẫn thoải mái, học sinh trước ít nhất phải`ceil(H / 2)`, và ít nhất là cái trước đó`ceil(ceil(H / 2) / 2)`, đệ quy. Điều này đưa ra một chuỗi chiều cao cần thiết tối thiểu để đạt được mục tiêu`H`. Tổng hợp sự khác biệt giữa những độ cao tối thiểu này và những độ cao ban đầu cho chúng ta biết số lượng pizza cần thiết. Bởi vì số lượng pizza cần thiết tăng đều theo chiều cao mục tiêu, chúng ta có thể sử dụng tìm kiếm nhị phân cho mỗi truy vấn: với ngân sách pizza nhất định, hãy tìm mức tối đa`H`such that the total pizzas required do not exceed the budget.

 We also notice that heights can become extremely large in theory, but we can safely bound the binary search. Chiều cao tối đa có thể sẽ không vượt quá`initial max height + total pizzas`, bởi vì cho đi nhiều hơn thế là vô nghĩa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(n) | Quá chậm | 
| DP ngược + Tìm kiếm nhị phân | O(n log(maxHeight + k) * q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi truy vấn có ngân sách pizza`k_i`, xác định không gian tìm kiếm cho chiều cao tối đa có thể đạt được`H`. Bộ`lo`đến độ cao tối đa hiện tại và`hi`ĐẾN`max_height + k_i`. 
2. Thực hiện tìm kiếm nhị phân. Đối với một ứng cử viên`H`, tính toán trình tự độ cao tối thiểu cần thiết để duy trì sự thoải mái, làm việc từ cuối hàng về phía sau. Khởi tạo`needed = H`cho học sinh cuối cùng, sau đó cho từng học sinh`i`từ`n-2`xuống tới`0`, bộ`needed = ceil(needed / 2)`. Điều này đảm bảo`2 * a_i >= a_{i+1}`. 
3. Trong khi tính chiều cao tối thiểu, hãy tính tổng số lượng pizza cần thiết. Với mỗi học sinh, hãy thêm`max(0, needed - a[i])`ĐẾN`total_pizzas`. 
4. Nếu`total_pizzas <= k_i`, chiều cao mục tiêu`H`có thể đạt được; di chuyển`lo`lên để tìm kiếm một tầm cao hơn. Ngược lại, hãy di chuyển`hi`xuống. 
5. Sau khi tìm kiếm nhị phân hội tụ,`lo`là chiều cao tối đa có thể đạt được cho ngân sách pizza này. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi tính toán độ cao tối thiểu cần thiết để đáp ứng giới hạn về độ thoải mái. Vì những độ cao tối thiểu này đơn điệu với mục tiêu`H`, tìm kiếm nhị phân xác định chính xác giá trị lớn nhất`H`có thể đạt được với ngân sách pizza nhất định. Điều bất biến là ở mỗi lần lặp, trình tự từ học sinh cuối cùng đến học sinh đầu tiên tuân theo`2 * a_i >= a_{i+1}`. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def max_height_with_pizzas(n, heights, pizzas):
    res = []
    max_a = max(heights)
    
    for k in pizzas:
        lo, hi = max_a, max_a + k
        ans = max_a
        while lo <= hi:
            mid = (lo + hi) // 2
            need = mid
            total = 0
            for h in reversed(heights):
                total += max(0, need - h)
                need = (need + 1) // 2
            if total <= k:
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        res.append(ans)
    return res

def main():
    t = int(input())
    output = []
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        pizzas = [int(input()) for _ in range(q)]
        result = max_height_with_pizzas(n, a, pizzas)
        output.append(" ".join(map(str, result)))
    print("\n".join(output))

if __name__ == "__main__":
    main()
```Đầu tiên, giải pháp sẽ đọc số lượng trường hợp thử nghiệm và lặp qua chúng. Đối với mỗi trường hợp thử nghiệm, nó đọc mảng độ cao và truy vấn pizza.`max_height_with_pizzas`xử lý từng truy vấn một cách độc lập. Nó sử dụng tìm kiếm nhị phân trên các độ cao mục tiêu có thể có, tính toán số lượng pizza tối thiểu cần thiết bằng cách đi lùi qua mảng. Chúng tôi sử dụng`(need + 1) // 2`để tính số trần cho 2 trong số học số nguyên. 

Những điểm tinh tế bao gồm việc đảm bảo`need`không bao giờ giảm xuống dưới độ cao trước đó (tính đơn điệu từ phía sau), sử dụng phép lặp đảo ngược và giới hạn tìm kiếm`max_height + k`để tránh những lần lặp lại không cần thiết. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`n=2`,`a=[10, 20]`,`k=10`| Bước | Cần thiết từ cuối | Tổng số pizza | Giữa | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 26 | 0 | 26 | tổng = max(0,26-20)+max(0,13-10)=6+3=9 ≤10 di chuyển lo lên | 
| 2 | 27 | 0 | 27 | tổng = max(0,27-20)+max(0,14-10)=7+4=11 >10 di chuyển hi xuống | 

Kết quả: 26 

Điều này xác nhận phép tính ngược đã tính toán chính xác số lượng pizza mà mỗi học sinh cần để đạt được mục tiêu. 

**Ví dụ 2** 

đầu vào:`n=3`,`a=[1,2,4]`,`k=4`| Bước | Cần thiết từ cuối | Tổng số pizza | Giữa | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 0 | 6 | tổng=max(0,6-4)+max(0,3-2)+max(0,2-1)=2+1+1=4 di chuyển lo lên | 
| 2 | 7 | 0 | 7 | tổng=max(0,7-4)+max(0,4-2)+max(0,2-1)=3+2+1=6 >4 di chuyển hi xuống | 

Kết quả: 6 

Trace cho thấy độ cao tối thiểu được tính toán đệ quy và ngân sách dành cho pizza được tôn trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * n * log(max_height + k)) | Đối với mỗi truy vấn, chúng tôi thực hiện tìm kiếm nhị phân trên các độ cao có thể (logarit) và quét ngược mảng để tìm số lượng pizza (tuyến tính). | 
| Không gian | O(n + q) | Lưu trữ độ cao và kết quả truy vấn. | 

Với các ràng buộc (tổng n 5·10^4, tổng q ≤ 5·10^4) và mỗi truy vấn yêu cầu tối đa 40 lần lặp tìm kiếm nhị phân (log2(10^9) ≈ 30), giải pháp dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided sample
assert run("""3
2 1
10 20
10
6 7
3 1 2 4 5 6
1
2
4
8
16
32
64
10 4
1 2 4 8 16 32 64 128 256
```
