---
title: "CF 1367B - Mảng chẵn"
description: "Chúng ta được cung cấp một mảng trong đó mỗi vị trí có một “tính chẵn lẻ bắt buộc” cố định: các chỉ số chẵn phải chứa các số chẵn và các chỉ mục lẻ phải chứa các số lẻ."
date: "2026-06-16T12:02:50+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1367
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 650 (Div. 3)"
rating: 800
weight: 1367
solve_time_s: 238
verified: true
draft: false
---

[CF 1367B - Mảng chẵn](https://codeforces.com/problemset/problem/1367/B) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 3 phút 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi vị trí có một “tính chẵn lẻ bắt buộc” cố định: các chỉ số chẵn phải chứa các số chẵn và các chỉ mục lẻ phải chứa các số lẻ. Chúng tôi được phép sắp xếp lại mảng bằng cách sử dụng các hoán đổi giữa hai vị trí bất kỳ và chúng tôi muốn biết số lượng hoán đổi tối thiểu cần thiết để đáp ứng tất cả các yêu cầu chẵn lẻ hoặc xác định rằng điều đó hoàn toàn không thể thực hiện được. 

Một cách hữu ích để diễn giải lại nhiệm vụ là coi mỗi vị trí có một loại mục tiêu, chỉ mục chẵn hoặc chỉ mục lẻ và mỗi giá trị cũng có một loại, giá trị chẵn hoặc giá trị lẻ. Mục tiêu là hoán vị các giá trị sao cho loại phù hợp với yêu cầu về vị trí. 

Vì việc hoán đổi không bị hạn chế nên cấu trúc không liên quan đến tính liền kề hoặc thứ tự, mà chỉ liên quan đến số lượng không khớp giữa hai nhóm. 

Các ràng buộc nhỏ: n nhiều nhất là 40 và có nhiều nhất 1000 trường hợp thử nghiệm. Điều này có nghĩa là giải pháp O(n) hoặc O(n log n) cho mỗi trường hợp thử nghiệm là đủ dễ dàng và thậm chí giải pháp O(n^2) cũng sẽ vượt qua một cách thoải mái. Tuy nhiên, cấu trúc của bài toán gợi ý rằng chúng ta có thể giải từng trường hợp thử nghiệm trong thời gian tuyến tính bằng cách đếm các kết quả không khớp thay vì mô phỏng các lần hoán đổi. 

Trường hợp cạnh tinh vi xuất hiện khi số lượng giá trị chẵn không khớp với số vị trí chỉ mục chẵn. Ví dụ: nếu n là 3, chỉ số 0 và 2 yêu cầu số chẵn, vì vậy chúng ta cần chính xác hai giá trị chẵn. Nếu mảng chỉ có một giá trị chẵn thì không có phép hoán đổi nào có thể khắc phục được điều này, vì các phép hoán đổi không thay đổi số lượng số chẵn tồn tại. 

Một trường hợp cạnh khác xảy ra khi mọi thứ đã khớp với tính chẵn lẻ. Trong trường hợp đó, câu trả lời là 0 và một chiến lược hoán đổi ngây thơ vẫn có thể tính sai các hoạt động không cần thiết nếu nó cố gắng “sửa” các vị trí đã đúng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ coi vấn đề là tìm kiếm các hoán vị của mảng. Chúng tôi có thể thử mọi cách có thể để sắp xếp lại các phần tử và tính toán số lần hoán đổi cần thiết để đạt được từng cấu hình hợp lệ. Tuy nhiên, số lượng hoán vị là n!, lớn về mặt thiên văn ngay cả khi n = 40. Ngay cả khi chúng ta cố gắng suy luận về khoảng cách hoán đổi giữa các hoán vị, không gian trạng thái vẫn quá lớn. 

Sự đơn giản hóa chính xuất phát từ việc quan sát rằng chỉ tính chẵn lẻ mới quan trọng chứ không phải giá trị thực. Mỗi phần tử thuộc về một trong hai loại: chẵn hoặc lẻ. Mỗi chỉ số cũng thuộc một trong hai loại: vị trí chẵn hoặc vị trí lẻ. Sự cố giảm xuống mức khớp giữa hai phân vùng này. 

Nếu chúng ta đếm xem có bao nhiêu chỉ số của mỗi chẵn lẻ tồn tại và có bao nhiêu phần tử của mỗi chẵn lẻ, chúng ta sẽ thấy ngay liệu một giải pháp có thể thực hiện được hay không. Nếu số đếm không khớp thì không có sự sắp xếp lại nào có thể khắc phục được. 

Sau khi tính khả thi được thiết lập, câu hỏi còn lại là cần bao nhiêu giao dịch hoán đổi. Mỗi lần hoán đổi giữa phần tử vị trí chẵn trên vị trí lẻ bị đặt sai vị trí và phần tử vị trí chẵn trên vị trí lẻ bị đặt sai vị trí sẽ sửa hai vị trí không chính xác cùng một lúc. Cấu trúc ghép nối này trực tiếp xác định câu trả lời. 

Brute-force hoạt động vì nó cố gắng xây dựng một cách rõ ràng một hoán vị hợp lệ, nhưng nó thất bại vì nó không khai thác cấu trúc nhị phân của tính chẵn lẻ. Quan sát cho thấy chỉ có số lượng không khớp mới làm giảm vấn đề thành việc đếm và ghép nối không khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Đếm số chẵn lẻ không khớp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào một trường hợp thử nghiệm.

1. Đếm xem có bao nhiêu vị trí mảng cần giá trị chẵn. Đây chính xác là các chỉ số 0, 2, 4, v.v. Đặt đây là Need_even_positions. 
2. Đếm xem mảng có bao nhiêu số chẵn. Đặt giá trị này là has_even_values. Điều này xác định có bao nhiêu phần tử có sẵn để đáp ứng các vị trí chẵn. 
3. Nếu giá trị_chẵn_có không bằng Need_even_positions, hãy trả về -1. Lý do là các hoán đổi không thể thay đổi tổng số giá trị chẵn, do đó không thể thỏa mãn tất cả các ràng buộc nếu tổng không đồng ý. 
4. Tính toán sự không khớp bằng cách quét mảng. Với mọi chỉ số i, nếu i chẵn nhưng a[i] lẻ thì vị trí này là thiếu giá trị chẵn. Nếu i lẻ nhưng a[i] chẵn thì đây là phần dư của giá trị chẵn được đặt không chính xác. 
5. Đặt mismatch_even_to_odd là số giá trị chẵn nằm trong các chỉ số lẻ. Điều này bằng với số lượng vị trí cần điều chỉnh từ phía bên kia. 
6. Số lần hoán đổi tối thiểu là không khớp_even_to_odd, vì mỗi lần hoán đổi giữa một giá trị chẵn được đặt không chính xác và một giá trị lẻ được đặt không chính xác sẽ sửa cả hai vị trí cùng một lúc. 

### Tại sao nó hoạt động 

Mỗi hoán đổi hợp lệ có thể sửa tối đa hai vị trí không chính xác và mọi vị trí không chính xác thuộc về chính xác một trong hai loại không khớp đối xứng. Khi tính khả thi đảm bảo số lượng khe chẵn lẻ được yêu cầu và các giá trị chẵn lẻ có sẵn bằng nhau, thì sự không khớp sẽ tạo thành các cặp hoàn hảo. Mỗi lần hoán đổi sẽ giải quyết một cặp mà không tạo ra sự không khớp mới ở nơi khác, vì vậy việc đếm các cặp này sẽ mang lại số lượng thao tác tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    need_even = (n + 1) // 2
    have_even = sum(1 for x in a if x % 2 == 0)
    
    if have_even != need_even:
        print(-1)
        continue
    
    mismatches = 0
    for i, x in enumerate(a):
        if i % 2 != x % 2:
            if i % 2 == 0:
                mismatches += 1
    
    print(mismatches)
```Giải pháp bắt đầu bằng cách tính toán có bao nhiêu chỉ số chẵn tồn tại, tức là số vị trí phải được điền bằng số chẵn. Sau đó nó đếm xem có bao nhiêu giá trị chẵn có trong mảng. Việc kiểm tra tính khả thi này là cần thiết vì việc hoán đổi không thể thay đổi nhiều giá trị. 

Vòng đếm không khớp chỉ tăng khi chỉ số chẵn chứa số lẻ. Điều này là đủ vì mọi sự không khớp như vậy phải được ghép với một sự không khớp đối diện tương ứng ở chỉ số lẻ và việc chỉ đếm một bên sẽ tránh được việc hoán đổi tính hai lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [3, 2, 7, 6]
```Chúng tôi theo dõi các giá trị quan trọng. 

| tôi | một [tôi] | tôi chẵn lẻ | a[i] chẵn lẻ | không khớp_even_to_odd | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | thậm chí | lẻ | 1 | 
| 1 | 2 | lẻ | thậm chí | 1 | 
| 2 | 7 | thậm chí | lẻ | 2 | 
| 3 | 6 | lẻ | thậm chí | 2 | 

Chúng tôi nhận được hai điểm không khớp trong đó các chỉ số chẵn đều giữ giá trị lẻ. Mỗi cái tương ứng với một hoán đổi cần thiết với sự không khớp ngược lại ở các chỉ số lẻ, đưa ra câu trả lời 2. 

Dấu vết này cho thấy rằng chúng tôi không bao giờ mô phỏng rõ ràng các giao dịch hoán đổi, chỉ tính sự mất cân bằng giữa hai lớp chẵn lẻ. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [3, 2, 6]
```| tôi | một [tôi] | tôi chẵn lẻ | a[i] chẵn lẻ | không khớp | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | thậm chí | lẻ | 1 | 
| 1 | 2 | lẻ | thậm chí | 1 | 
| 2 | 6 | thậm chí | thậm chí | 1 | 

Kiểm tra tính khả thi: need_even_positions = 2, has_even_values ​​= 2, vậy là có thể. 

Chỉ tồn tại một vị trí không khớp ở vị trí chẵn, vì vậy chỉ cần một lần hoán đổi là đủ. 

Điều này xác nhận rằng thuật toán làm giảm vấn đề về việc ghép các lỗi chẵn lẻ bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét mảng một lần để đếm và phát hiện sự không khớp | 
| Không gian | O(1) | Chỉ có một vài quầy được sử dụng | 

Các ràng buộc cho phép tối đa 1000 trường hợp thử nghiệm với n tối đa 40, do đó, tối đa 40000 phần tử được xử lý. Một đường chuyền tuyến tính duy nhất cho mỗi trường hợp thử nghiệm có thể dễ dàng đủ nhanh trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        need_even = (n + 1) // 2
        have_even = sum(x % 2 == 0 for x in a)
        
        if have_even != need_even:
            out.append("-1")
            continue
        
        mismatches = 0
        for i, x in enumerate(a):
            if i % 2 == 0 and x % 2 == 1:
                mismatches += 1
        
        out.append(str(mismatches))
    
    return "\n".join(out)

# provided samples
assert solve("""4
4
3 2 7 6
3
3 2 6
1
7
7
4 9 2 1 18 3 0
""") == """2
1
-1
0"""

# all already correct
assert solve("""1
5
0 1 2 3 4
""") == "0"

# impossible due to parity mismatch
assert solve("""1
2
1 1
""") == "-1"

# minimal case
assert solve("""1
1
0
""") == "0"

# needs swaps
assert solve("""1
4
1 3 5 7
""") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 yếu tố đã hợp lệ | 0 | không cần trao đổi trường hợp | 
| tất cả các giá trị lẻ trên cấu trúc có kích thước chẵn | -1 | thất bại khả thi | 
| phần tử đơn | 0 | đầu vào hợp lệ nhỏ nhất | 
| tất cả các giá trị lẻ | -1 | không phù hợp cực | 

## Vỏ cạnh 

Mảng tối thiểu có kích thước 1 luôn thỏa mãn điều kiện nếu giá trị là số chẵn, vì chỉ số 0 yêu cầu tính chẵn lẻ. Thuật toán trả về chính xác số lần hoán đổi bằng 0 vì không phát hiện thấy sự không khớp nào. 

Khi mảng có số lượng giá trị chẵn chính xác nhưng tất cả chúng đều ở sai vị trí, tính khả thi sẽ được thông qua nhưng việc đếm không khớp sẽ tạo ra số lượng hoán đổi chính xác cần thiết. Mỗi lần hoán đổi tương ứng với việc ghép nối một phần tử chẵn ở chỉ số lẻ với phần tử có chỉ số lẻ ở chỉ số chẵn và không có cấu trúc nào khác ảnh hưởng đến kết quả.
