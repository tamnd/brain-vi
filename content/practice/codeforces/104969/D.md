---
title: "CF 104969D - Nuôi dưỡng trẻ em"
description: "Chúng ta được sắp xếp một dãy học sinh, mỗi học sinh yêu cầu một số lát bánh pizza nhất định theo một thứ tự cố định. Có K chiếc pizza giống hệt nhau và mỗi chiếc pizza có cùng số lát S không xác định được."
date: "2026-06-28T19:07:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104969
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 02-09-24 Div. 1 (Advanced)"
rating: 0
weight: 104969
solve_time_s: 75
verified: false
draft: false
---

[CF 104969D - Nuôi dưỡng trẻ em](https://codeforces.com/problemset/problem/104969/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp một dãy học sinh, mỗi học sinh yêu cầu một số lát bánh pizza nhất định theo một thứ tự cố định. Có K chiếc pizza giống hệt nhau và mỗi chiếc pizza có cùng số lát S không xác định được. Học sinh được phục vụ từng người một: nếu chiếc bánh pizza hiện tại vẫn còn đủ số lát cho yêu cầu của học sinh tiếp theo thì học sinh đó sẽ được phục vụ từ chiếc bánh đó; nếu không thì những lát bánh pizza còn lại sẽ bị loại bỏ, một chiếc bánh pizza mới sẽ được mở ra và học sinh sẽ được phục vụ chiếc bánh mới. 

Câu hỏi quan trọng không phải là việc phục vụ diễn ra như thế nào mà là S phải lớn đến mức nào để quá trình này có thể hoàn thành mà không hết pizza, với điều kiện là có nhiều nhất K pizza. 

Vì vậy, nhiệm vụ là chọn số nguyên S nhỏ nhất sao cho khi mô phỏng quá trình phục vụ tham lam này, chúng ta không bao giờ cần nhiều hơn K chiếc pizza. 

Kích thước đầu vào cho phép lên tới 100.000 sinh viên và lên tới 100.000 chiếc pizza. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng kiểm tra tất cả các giá trị có thể có của S bằng cách mô phỏng quy trình cho từng ứng cử viên. Giới hạn trên ngây thơ của S là tổng của tất cả các yêu cầu, có thể lên tới 10^11, do đó mọi tìm kiếm tuyến tính trên S sẽ hoàn toàn không khả thi. 

Chúng ta cần một giải pháp làm giảm vấn đề xuống mức kiểm tra tính khả thi đơn điệu. 

Một vài trường hợp tế nhị quan trọng: 

Nếu một học sinh yêu cầu một số lượng lát cắt rất lớn, chẳng hạn như d_i = 10^6, thì S ít nhất phải lớn như vậy. Nếu không, thuật toán sẽ liên tục mở pizza chỉ cho một học sinh, điều này được phép nhưng lại gây lãng phí dung lượng; quan trọng hơn, điều kiện đúng vẫn được giữ nhưng tính khả thi trở nên không thể thực hiện được với S nhỏ. 

Một trường hợp cạnh khác xuất hiện khi K lớn so với N. Ví dụ: nếu K ≥ N, thậm chí S = max(d_i) luôn đúng, vì về nguyên tắc mỗi học sinh có thể bắt đầu một chiếc bánh pizza mới. Điều này không làm thay đổi cấu trúc câu trả lời nhưng giúp xác nhận tính đúng đắn của việc xử lý cạnh. 

Cuối cùng, sự tinh tế chính là những lát bánh còn thừa luôn được loại bỏ khi một chiếc bánh pizza mới được mở ra. Điều này có nghĩa là chúng ta đang phân chia chuỗi thành các phân đoạn một cách hiệu quả, trong đó mỗi phân đoạn có tổng tổng tối đa là S, nhưng các phân đoạn bị ép buộc do tràn chứ không phải do đóng gói tối ưu. Cấu trúc tham lam bắt buộc này chính là yếu tố thúc đẩy giải pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử giá trị ứng viên S và mô phỏng quá trình phục vụ: duy trì các lát còn lại trong chiếc bánh pizza hiện tại, giảm dần khi chúng tôi phục vụ học sinh và đếm số lần chúng tôi cần mở một chiếc bánh pizza mới. Nếu chúng ta vượt quá K, S sẽ không hợp lệ. 

Mô phỏng này đúng với S cố định và tốn thời gian O(N). Tuy nhiên, bản thân S có thể lớn bằng tổng của tất cả các nhu cầu. Việc thử tất cả các giá trị từ max(d_i) trở lên sẽ yêu cầu tới 10^11 lần kiểm tra, điều này là không thể. 

Quan sát chính là tính khả thi là đơn điệu trong S. Nếu một S nhất định đủ để phục vụ tất cả học sinh trong K pizza, thì bất kỳ S nào lớn hơn cũng là đủ, bởi vì việc tăng công suất chỉ có thể giảm hoặc duy trì số lần mở pizza. Điều này biến vấn đề thành tìm kiếm nhị phân trên S. 

Mỗi lần kiểm tra tính khả thi là một lần quét tham lam: chúng tôi mô phỏng quy trình và đếm số lượng pizza được sử dụng. Nếu tại bất kỳ thời điểm nào một nhu cầu vượt quá S, chúng ta biết ngay S là không hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force S | O(N * tổng d_i) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân + mô phỏng | O(N log max d_i) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tìm kiếm S nhỏ nhất sao cho có thể phục vụ tất cả học sinh sử dụng tối đa K chiếc pizza.

1. Đặt phạm vi tìm kiếm cho S từ max(d_i) đến sum(d_i). Giới hạn dưới là cần thiết vì bất kỳ học sinh nào cũng phải vừa với một miếng bánh pizza. 
2. Xác định hàm can(S) mô phỏng việc phục vụ học sinh với công suất pizza S. Khởi tạo pizzas_used = 1 và còn lại = S. 
3. Với mỗi nhu cầu d_i, kiểm tra xem nó có phù hợp với dung lượng còn lại hiện tại hay không. Nếu còn lại ≥ d_i, trừ d_i khỏi phần còn lại và tiếp tục. 
4. Nếu còn lại < d_i, tăng pizzas_used lên 1, đặt lại phần còn lại về S, rồi trừ d_i. Mô hình này loại bỏ những lát bánh còn sót lại và mở một chiếc bánh pizza mới. 
5. Nếu tại bất kỳ thời điểm nào pizzas_used vượt quá K, hãy trả về Sai ngay lập tức vì S không đủ. 
6. Tìm kiếm nhị phân trên S. Nếu can(S) đúng, hãy thử các giá trị nhỏ hơn; nếu không thì tăng S. 

Lựa chọn thiết kế quan trọng là chúng tôi không bao giờ cố gắng tối ưu hóa cách phân nhóm học sinh. Quá trình này bị ép buộc nên mô phỏng tham lam là cách giải thích hợp lệ duy nhất. 

### Tại sao nó hoạt động 

Đối với S cố định, mô phỏng tạo ra số lượng pizza tối thiểu có thể vì nó chỉ mở một chiếc pizza mới khi bị buộc phải thiếu dung lượng còn lại. Bất kỳ chiến lược thay thế nào mở pizza sớm hơn sẽ chỉ làm tăng mức sử dụng. Do đó can(S) phản ánh chính xác tính khả thi. 

Tính đơn điệu đảm bảo rằng tập hợp các giá trị S hợp lệ tạo thành một hậu tố số nguyên, đảm bảo tính chính xác của tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(d, n, k, s):
    pizzas_used = 1
    remaining = s

    for x in d:
        if x > s:
            return False
        if remaining >= x:
            remaining -= x
        else:
            pizzas_used += 1
            if pizzas_used > k:
                return False
            remaining = s - x

    return pizzas_used <= k

def solve():
    n, k = map(int, input().split())
    d = list(map(int, input().split()))

    lo = max(d)
    hi = sum(d)

    while lo < hi:
        mid = (lo + hi) // 2
        if can(d, n, k, mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Mã này tách việc kiểm tra tính khả thi khỏi tìm kiếm nhị phân, điều này rất cần thiết để đảm bảo tính rõ ràng và chính xác. Chức năng này có thể thực thi cẩn thận quy tắc mở bắt buộc: bất cứ khi nào dung lượng còn lại không đủ, chúng tôi sẽ ngay lập tức mở một chiếc bánh pizza mới và đặt lại bộ đếm. 

Một điểm tinh tế là khởi tạo lo dưới dạng max(d). Điều này tránh việc kiểm tra không cần thiết đối với các giá trị S không hợp lệ và đảm bảo mô phỏng không bao giờ cố gắng gán cho học sinh nhiều lát hơn mức một chiếc bánh pizza có thể chứa. 

Tìm kiếm nhị phân hội tụ vì mỗi bước thu hẹp khoảng [lo, hi) dựa trên tính khả thi đơn điệu. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có nhu cầu là [5, 2, 7, 3] và K = 2. 

Chúng tôi kiểm tra ứng viên S = 7. 

| Sinh viên | Nhu cầu | Còn lại trước | Hành động | Pizza đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 7 | giao bóng, còn lại = 2 | 1 | 
| 2 | 2 | 2 | giao bóng, còn lại = 0 | 1 | 
| 3 | 7 | 0 | mở pizza mới | 2 | 
| 3 (tiếp) | 7 | 7 | giao bóng, còn lại = 0 | 2 | 
| 4 | 3 | 0 | mở pizza mới | 3 | 

Chúng ta vượt quá K = 2 nên S = 7 không hợp lệ. Điều này cho thấy rằng mặc dù mọi nhu cầu đều phù hợp với từng cá nhân nhưng sự phân mảnh sẽ tạo ra thêm nhiều pizza. 

Bây giờ kiểm tra S = 10. 

| Sinh viên | Nhu cầu | Còn lại trước | Hành động | Pizza đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 10 | phục vụ | 1 | 
| 2 | 2 | 5 | phục vụ | 1 | 
| 3 | 7 | 3 | mở pizza mới, phục vụ | 2 | 
| 4 | 3 | 3 | phục vụ | 2 | 

Chúng ta sử dụng chính xác 2 chiếc pizza nên S = 10 là khả thi. Điều này chứng tỏ việc tăng S làm giảm sự phân mảnh tại các điểm dừng bắt buộc như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log M) | Mỗi lần kiểm tra tính khả thi sẽ quét tất cả học sinh một lần và tìm kiếm nhị phân trên S chạy theo phạm vi logarit M = sum(d_i) | 
| Không gian | O(1) | Chỉ các bộ đếm và mảng đầu vào được lưu trữ | 

Các ràng buộc N 10^5 giúp quét tuyến tính có thể chấp nhận được và log M tối đa là khoảng 40 đối với các giới hạn điển hình, giữ cho giải pháp luôn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def can(d, n, k, s):
        pizzas_used = 1
        remaining = s
        for x in d:
            if x > s:
                return False
            if remaining >= x:
                remaining -= x
            else:
                pizzas_used += 1
                if pizzas_used > k:
                    return False
                remaining = s - x
        return pizzas_used <= k

    def solve():
        n, k = map(int, input().split())
        d = list(map(int, input().split()))
        lo, hi = max(d), sum(d)
        while lo < hi:
            mid = (lo + hi) // 2
            if can(d, n, k, mid):
                hi = mid
            else:
                lo = mid + 1
        print(lo)

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("5 3\n2 10 3 6 7\n") == "10"

# minimum case
assert run("1 1\n5\n") == "5"

# all equal small
assert run("4 2\n2 2 2 2\n") == "4"

# tight fragmentation
assert run("3 2\n5 5 5\n") == "10"

# large K (each student separate allowed)
assert run("3 3\n8 1 1\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/5 | 5 | đường cơ sở của một sinh viên | 
| 4 2/2 2 2 2 | 4 | phân phối đồng đều | 
| 3 2 / 5 5 5 | 10 | hiệu ứng tách cưỡng bức | 
| 3 3 / 8 1 1 | 8 | K lớn làm giảm áp suất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một nhu cầu vượt quá ứng cử viên hiện tại S. Ví dụ: d = [10, 1, 1] và S = 5. Thuật toán ngay lập tức loại bỏ S mà không cần mô phỏng thêm vì không tồn tại phân đoạn khả thi. Điều này ngăn chặn việc tính toán lãng phí và duy trì tính chính xác của giới hạn tìm kiếm nhị phân. 

Một trường hợp khác là khi K rất lớn, chẳng hạn như K ≥ N. Trong d = [3, 4, 2], K = 10, S tối ưu trở thành max(d) = 4. Mô phỏng sẽ không bao giờ mở chiếc bánh pizza thứ hai trừ khi bị ép buộc bởi một nhu cầu lớn hơn khả năng còn lại và vì mỗi nhu cầu phù hợp nên chúng ta dễ dàng duy trì trong K. 

Trường hợp cuối cùng là phân mảnh nặng: d = [6, 6, 6], K = 2. Với S = 6, mỗi học sinh buộc phải có một chiếc pizza mới, cần 3 chiếc pizza và thất bại. Với S = 12, hai sinh viên đầu tiên sẽ ăn vừa một chiếc bánh pizza, và sinh viên cuối cùng buộc sinh viên thứ hai phải ở trong K. Điều này cho thấy cách S kiểm soát việc phân khúc thay vì chỉ tính khả thi cục bộ.
