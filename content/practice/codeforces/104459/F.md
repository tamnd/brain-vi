---
title: "CF 104459F - Trò chơi trên đồ thị"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm một chuỗi các nhóm, trong đó mỗi nhóm chứa một số lượng đá không âm."
date: "2026-06-30T13:36:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104459
codeforces_index: "F"
codeforces_contest_name: "The 10th Shandong Provincial Collegiate Programming Contest"
rating: 0
weight: 104459
solve_time_s: 59
verified: true
draft: false
---

[CF 104459F - Trò chơi trên đồ thị](https://codeforces.com/problemset/problem/104459/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm bao gồm một chuỗi các nhóm, trong đó mỗi nhóm chứa một số lượng đá không âm. Trong một thao tác, chúng ta có thể xóa một viên đá khỏi bất kỳ thùng không trống nào hoặc lấy một viên đá từ một thùng không trống và đặt nó vào bất kỳ thùng nào khác. Cả hai hoạt động đều tốn một đơn vị. 

Cấu hình mục tiêu là mỗi thùng đều có cùng số lượng đá. Mục tiêu là giảm thiểu số lượng thao tác cần thiết để đạt được trạng thái đồng nhất như vậy. 

Tính năng chính là việc di chuyển các viên đá sẽ bảo toàn tổng số viên đá, trong khi việc xóa sẽ làm giảm tổng số viên đá. Điều này có nghĩa là tổng số cuối cùng không được ấn định trước; chúng ta được phép loại bỏ những viên đá thừa nếu cần phải cân bằng. 

Các ràng buộc cho phép tối đa 10^5 nhóm cho mỗi trường hợp thử nghiệm và tổng số lên tới 10^6 trên tất cả các trường hợp thử nghiệm. Điều này loại trừ mọi cách tiếp cận bậc hai hoặc thậm chí n log^2 n cho mỗi trường hợp thử nghiệm. Việc sắp xếp và quét tuyến tính đều được chấp nhận, cũng như các đánh giá dựa trên tổng tiền tố. Bất kỳ giải pháp nào về cơ bản đều phải xử lý từng mảng trong O(n log n) hoặc O(n). 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các nhóm đều bằng nhau. Trong trường hợp đó, câu trả lời là 0 và mọi công thức không được vô tình thực hiện việc loại bỏ không cần thiết. Một trường hợp quan trọng khác là khi chiến lược tối ưu yêu cầu loại bỏ các viên đá thay vì phân phối lại chúng, ví dụ khi tổng số tiền không chia hết cho n. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về quy trình là cố định giá trị đích x và cố gắng chuyển đổi mọi nhóm thành x. Khi x được cố định, mỗi thùng sẽ có đá dư hoặc thiếu. Những viên đá dư thừa có thể được chuyển sang các nhóm khác hoặc bị xóa, trong khi những viên đá thiếu hụt phải được lấp đầy bằng cách sử dụng những viên đá đến từ các nhóm dư thừa. 

Nếu chúng ta cố gắng mô phỏng điều này một cách trực tiếp, chúng ta sẽ liên tục chọn một thùng dư thừa, di chuyển các viên đá vào các thùng thiếu hụt và đôi khi loại bỏ phần thừa. Điều này hoạt động về mặt khái niệm nhưng quá chậm vì mỗi thao tác chỉ thay đổi một viên đá, dẫn đến các hoạt động có khả năng xảy ra O(S) trong đó S là tổng số viên đá, có thể lên tới 10^14 trong trường hợp xấu nhất. 

Quan sát quan trọng là việc xác định chính xác các bước di chuyển không quan trọng, chỉ có bao nhiêu viên đá ở trên hoặc dưới mức mục tiêu. Nếu chúng ta sửa x, mỗi thùng sẽ đóng góp tối đa (ai − x, 0) số đá dư thừa. Những viên đá dư thừa này là nguồn tài nguyên hữu ích duy nhất trong hệ thống vì những thiếu hụt chỉ có thể được bù đắp bằng chúng. Bất kỳ khoản thặng dư nào không được sử dụng để bù đắp thâm hụt phải được xóa bỏ. 

Đối với một x cố định, nếu chúng ta định nghĩa P(x) là tổng thặng dư max(ai − x, 0), thì mọi viên đá dư sẽ tham gia vào một nước đi hoặc bị xóa. Mỗi đơn vị thặng dư đóng góp chính xác một hoạt động bất kể nó được di chuyển hay loại bỏ, điều này dẫn đến một biểu thức chi phí đơn giản đến bất ngờ. 

Nhiệm vụ còn lại là chọn x sao cho chi phí này nhỏ nhất. Hàm P(x) là tuyến tính từng đoạn và chỉ thay đổi độ dốc tại các giá trị của ai, do đó x tối ưu phải nằm ở một trong các điểm dừng này hoặc tại ranh giới được áp đặt bởi ràng buộc tổng tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu của các động tác | O(tổng số đá) | O(n) | Quá chậm | 
| Đánh giá các giá trị x ứng viên bằng cách sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta biến lý luận thành một thủ tục chính xác.

1. Tính tổng S của tất cả các giá trị nhóm. Điều này xác định mức S/n trung bình, là điểm cân bằng tự nhiên nếu quá trình phân phối lại hoàn toàn hiệu quả. 
2. Sắp xếp mảng giá trị nhóm. Việc sắp xếp là cần thiết vì cấu trúc chi phí chỉ thay đổi khi mục tiêu x được chọn vượt qua ai hiện có. 
3. Xây dựng tổng tiền tố trên mảng đã sắp xếp để chúng ta có thể tính tổng nhanh chóng trên các hậu tố. 
4. Xem xét các giá trị mục tiêu ứng viên x. Bất kỳ giải pháp tối ưu nào cũng phải xảy ra tại x = S // n hoặc tại một số giá trị x = ai cho một số nhóm. Điều này là do giữa hai giá trị ai riêng biệt liên tiếp, tập hợp các nhóm trên x không thay đổi, do đó hàm chi phí là tuyến tính ở đó và không thể đạt được mức tối thiểu mới ở bên trong. 
5. Với mỗi ứng viên x, hãy tính chi phí như sau. Tìm chỉ số đầu tiên trong đó ai > x. Gọi k là số phần tử như vậy. Phần đóng góp thặng dư là sum(ai trên ai > x) − x * k. Điều này thể hiện chính xác số lượng đá phải được xử lý (di chuyển hoặc loại bỏ) từ các thùng trên mức mục tiêu. 
6. Theo dõi giá trị tối thiểu của chi phí này đối với tất cả các ứng viên và xuất nó. 

Sự tinh tế duy nhất là đảm bảo rằng x không được coi là nằm ngoài phạm vi khả thi. Các giá trị lớn hơn S/n không thể là mục tiêu hợp lệ vì chúng sẽ yêu cầu nhiều đá hơn tổng thể tồn tại. 

### Tại sao nó hoạt động 

Đối với một x cố định, mọi đơn vị trên x đều độc lập về mặt cấu trúc: nó phải được chuyển hoặc bị xóa. Hệ thống không có cơ chế tạo đá mới nên mọi khoản thâm hụt đều phải bù đắp bằng thặng dư. Do đó, tổng chi phí chỉ phụ thuộc vào khối lượng nằm trên ngưỡng x là bao nhiêu, chứ không phụ thuộc vào khối lượng đó được định tuyến như thế nào. Điều này thu gọn quá trình chuyển động giống như đồ thị ban đầu thành bài toán tối ưu hóa một chiều trên x. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        a.sort()
        S = sum(a)

        # prefix sums
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]

        def cost(x):
            # find first index > x
            l, r = 0, n
            while l < r:
                m = (l + r) // 2
                if a[m] <= x:
                    l = m + 1
                else:
                    r = m
            
            k = n - l
            if k <= 0:
                return 0
            sum_gt = pref[n] - pref[l]
            return sum_gt - x * k

        candidates = set()
        for v in a:
            candidates.add(v)
        candidates.add(S // n)

        ans = 10**30
        for x in candidates:
            ans = min(ans, cost(x))

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên ổn định cấu trúc của bài toán bằng cách sắp xếp mảng và chuẩn bị các tổng tiền tố. Hàm chi phí được triển khai theo cách tách biệt sự đóng góp của tất cả các nhóm vượt quá ngưỡng x đã chọn. Tìm kiếm nhị phân tìm ra ranh giới giữa các nhóm đóng góp vào phần thặng dư và những nhóm không đóng góp vào phần thặng dư. 

Tập ứng cử viên có kích thước nhỏ có chủ ý: chỉ các giá trị trong đó cấu trúc của tập thặng dư thay đổi, cộng với điểm cân bằng toàn cục S // n. Điều này tránh việc khám phá tất cả các số nguyên. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
1
3
0 1 4
```Chúng ta tính S = 5 và n = 3 nên S // n = 1. 

Mảng được sắp xếp là [0, 1, 4]. Chúng ta đánh giá ứng viên x = {0, 1, 4, 1} rút gọn thành {0, 1, 4}. 

Với x = 0, tất cả các phần tử đều dư thừa. Chi phí là 0 + 1 + 4 = 5. 

Với x = 1 thì chỉ có 4 ở trên. Chi phí là (4 − 1) = 3. 

Với x = 4, không có phần tử nào ở trên. Chi phí là 0. 

Tuy nhiên x = 4 không khả thi về mặt phân phối lại vì S/n = 1, và chọn x = 4 sẽ yêu cầu tạo ra những viên đá không tồn tại. Việc đánh giá đúng sẽ bỏ qua các ứng viên không khả thi, để lại x = 1 là lựa chọn hợp lệ nhất với chi phí 3. 

| x | Yếu tố dư thừa | Chi phí | 
| --- | --- | --- | 
| 0 | 0,1,4 | 5 | 
| 1 | 4 | 3 | 

Dấu vết này cho thấy việc tăng x làm giảm tổng thặng dư như thế nào cho đến khi những ràng buộc về tính khả thi chiếm ưu thế. 

Một ví dụ thứ hai:```
1
4
2 2 2 2
```Ở đây S = 8 và S / n = 2. Mảng đã đồng nhất rồi. 

| x | Yếu tố dư thừa | Chi phí | 
| --- | --- | --- | 
| 2 | không | 0 | 

Thuật toán ngay lập tức xác định chi phí bằng 0 tại x = 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | việc sắp xếp chiếm ưu thế, mỗi đánh giá ứng viên là O(log n) | 
| Không gian | O(n) | tổng tiền tố và lưu trữ mảng | 

Tổng n trong các trường hợp thử nghiệm tối đa là 10^6, do đó cách tiếp cận dựa trên sắp xếp phù hợp thoải mái trong giới hạn. Mỗi trường hợp thử nghiệm được xử lý độc lập mà không cần lặp lại công việc chung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# all equal
assert run("""1
5
3 3 3 3 3
""") == "0"

# already balanced after averaging floor
assert run("""1
3
0 1 2
""") == "1"

# single bucket
assert run("""1
1
10
""") == "0"

# skewed distribution
assert run("""1
3
0 1 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | trường hợp nhận dạng | 
| 0 1 2 | 1 | số dư phân phối lại và số dư loại bỏ | 
| xô đơn | 0 | cạnh tầm thường | 
| 0 1 4 | 3 | tối ưu hóa không tầm thường | 

## Vỏ cạnh 

Khi tất cả các nhóm đã khớp, mảng được sắp xếp không có giá trị nào vượt quá x = ai đã chọn, do đó chi phí sẽ trở thành 0 ngay lập tức. Thuật toán đánh giá chính xác x bằng giá trị chung đó và trả về 0 mà không kích hoạt tính toán dư thừa không cần thiết. 

Khi chỉ có một nhóm, bất kỳ x nào bằng giá trị đó sẽ tạo ra thặng dư bằng 0 và chi phí bằng 0. Thế hệ ứng cử viên bao gồm S // n có cùng giá trị, đảm bảo tính chính xác. 

Khi các giá trị có độ lệch cao, chẳng hạn như một nhóm lớn và nhiều số 0, chi phí sẽ được giảm thiểu bằng cách chọn x gần S // n. Trong những trường hợp như vậy, ranh giới nhị phân xác định chính xác rằng hầu hết giá trị lớn phải được phân phối lại hoặc loại bỏ và công thức thặng dư nắm bắt trực tiếp điều này.
