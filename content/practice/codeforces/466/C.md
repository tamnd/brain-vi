---
title: "CF 466C - Số Cách"
description: "Chúng ta được cho một dãy số nguyên được sắp xếp thành một dòng và chúng ta muốn chia nó thành ba đoạn liền kề nhau. Các điểm cắt phải tạo ra ba phần không trống và mỗi phần phải có tổng bằng nhau."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 466
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 266 (Div. 2)"
rating: 1700
weight: 466
solve_time_s: 78
verified: true
draft: false
---

[CF 466C - Số cách](https://codeforces.com/problemset/problem/466/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, dp, hai con trỏ 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên được sắp xếp thành một dòng và chúng ta muốn chia nó thành ba đoạn liền kề nhau. Các điểm cắt phải tạo ra ba phần không trống và mỗi phần phải có tổng bằng nhau. 

Nói cách khác, chúng ta đang tìm kiếm hai vị trí cắt trong mảng để chia mảng thành tiền tố, phân đoạn giữa và hậu tố, trong đó cả ba phân đoạn đều có cùng một giá trị. 

Kích thước đầu vào có thể rất lớn, lên tới 500.000 phần tử. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào thử trực tiếp tất cả các cặp vị trí cắt, vì điều đó sẽ liên quan đến việc kiểm tra thứ tự của$n^2$cấu hình, vượt xa những gì có thể được tính toán trong hai giây. Chúng ta cần một cái gì đó gần hơn với thời gian tuyến tính hoặc gần tuyến tính. 

Ràng buộc cấu trúc quan trọng đầu tiên là nếu tổng của mảng không chia hết cho 3 thì câu trả lời sẽ tự động bằng 0. Cho dù chúng ta chia như thế nào thì việc phân chia tổng bằng nhau là không thể trong trường hợp đó. 

Có một vài trường hợp tế nhị phá vỡ suy nghĩ ngây thơ: 

Một là khi nhiều tổng tiền tố khớp với một phần ba tổng số bắt buộc, nhưng không có sự căn chỉnh hậu tố hợp lệ. Ví dụ, trong`[0, 0, 0, 0]`, mọi tổng tiền tố đều bằng 0 và mọi phần tách có vẻ hợp lệ ngay từ cái nhìn đầu tiên, nhưng chỉ những vị trí cắt bảo toàn ba phần không trống là hợp lệ và việc đếm quá mức xảy ra nếu chúng tôi không thực thi thứ tự cắt. 

Một trường hợp cạnh khác là khi tổng mục tiêu bằng 0. Điều này rất nguy hiểm vì các tiền tố có tổng bằng 0 có thể xuất hiện rất thường xuyên và việc tính toán ngây thơ có thể vô tình coi các lựa chọn chồng chéo là độc lập khi thực tế không phải vậy. Ví dụ, trong`[0, 0, 0]`, chỉ tồn tại một phần phân chia hợp lệ mặc dù nhiều vị trí tiền tố thỏa mãn các điều kiện từng phần. 

Khó khăn chính là buộc lần cắt đầu tiên xảy ra trước lần cắt thứ hai, trong khi vẫn đếm các kết hợp một cách hiệu quả. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử từng cặp chỉ số cắt$i, j$như vậy$1 \le i < j < n$. Đối với mỗi cặp, chúng tôi tính tổng của ba phân đoạn và kiểm tra sự bằng nhau. Ngay cả khi chúng ta tính toán trước các tổng tiền tố để có được tổng các phân đoạn trong O(1), chúng ta vẫn có các cặp O(n²). Với$n = 5 \cdot 10^5$, điều này sẽ yêu cầu theo thứ tự$10^{11}$kiểm tra, điều này là không thể thực hiện được. 

Quan sát quan trọng là vấn đề giảm xuống còn việc suy luận về tổng tiền tố và giá trị mục tiêu cố định. Gọi tổng số tiền là$S$. Nếu tồn tại sự phân chia hợp lệ thì mỗi phân đoạn phải có tổng bằng$S/3$. Đặt giá trị này là$T$. Sau đó chúng tôi đang tìm kiếm các chỉ số có tổng tiền tố bằng$T$(lần cắt đầu tiên) và các chỉ số có tổng tiền tố bằng$2T$(cắt lần thứ hai). Hạn chế là lần cắt thứ hai phải đến sau lần cắt đầu tiên. 

Điều này biến vấn đề thành việc đếm các cặp lần truy cập tổng tiền tố theo thứ tự: mọi vị trí có tổng tiền tố bằng$T$có khả năng có thể ghép nối với các vị trí sau có tổng tiền tố bằng$2T$. Thay vì tìm kiếm tất cả các cặp một cách rõ ràng, chúng tôi quét một lần từ trái sang phải, đếm xem có bao nhiêu vị trí cắt đầu tiên hợp lệ mà chúng tôi đã thấy cho đến nay. 

Chúng tôi duy trì bộ đếm số lần chúng tôi đã gặp tổng tiền tố$T$. Khi chúng ta đạt đến vị trí có tổng tiền tố bằng$2T$, mọi thứ đã thấy trước đó$T$vị trí tạo thành một cặp hợp lệ với nó, vì vậy chúng tôi thêm bộ đếm đó vào câu trả lời. 

Điều này làm giảm vấn đề xuống còn một lần truyền qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đếm tổng tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định tổng tiền tố trong khi quét mảng một lần. 

1. Tính tổng số tiền$S$. Nếu như$S$không chia hết cho 3, trả về 0 ngay lập tức, vì không tồn tại phân vùng bằng nhau. 
2. Đặt mục tiêu$T = S / 3$. Bây giờ chúng tôi tìm kiếm các giá trị tổng tiền tố bằng$T$Và$2T$. 
3. Khởi tạo tổng tiền tố đang chạy`pref = 0`, một bộ đếm`count_T = 0`, và một câu trả lời`ans = 0`. 
4. Lặp lại mảng từ chỉ số 0 đến n − 1, cập nhật`pref += a[i]`. 
5. Bất cứ khi nào`pref == 2T`và chúng tôi không ở phần tử cuối cùng, chúng tôi thêm`count_T`ĐẾN`ans`. Điều này hoạt động vì mọi vị trí trước đó`pref == T`có thể phục vụ như là vết cắt đầu tiên. 
6. Bất cứ khi nào`pref == T`và chúng tôi không ở vị trí cuối cùng, tăng dần`count_T`. 
7. Tiếp tục cho đến hết mảng. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là các phân tách hợp lệ tương ứng chính xác với các cặp lần truy cập tổng tiền tố có thứ tự tại$T$Và$2T$. Bất kỳ sự phân chia hợp lệ nào đều phải chọn lần cắt đầu tiên tại một số chỉ mục có tổng tiền tố là$T$và lần cắt thứ hai sau đó có tổng tiền tố là$2T$. Bằng cách quét từ trái sang phải, chúng tôi đảm bảo rằng khi chúng tôi đạt được$2T$vị trí, tất cả đều hợp lệ$T$các vị trí trước đó đã được tính và mỗi vị trí tạo ra chính xác một phân vùng ba hợp lệ. Điều này duy trì ánh xạ một-một giữa các cặp được tính và các phần tách hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    if total % 3 != 0:
        print(0)
        return
    
    target = total // 3
    pref = 0
    count_first = 0
    ans = 0
    
    for i in range(n):
        pref += a[i]
        
        if i < n - 1 and pref == 2 * target:
            ans += count_first
        
        if i < n - 1 and pref == target:
            count_first += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc mảng và tính tổng của nó. Việc kiểm tra khả năng chia hết là cần thiết vì nó loại bỏ sớm các trường hợp không thể thực hiện được và ngăn ngừa việc truyền tải logic không cần thiết. 

Biến`pref`duy trì tổng tiền tố một cách linh hoạt, tránh tính toán lại. Điểm tinh tế quan trọng nhất là thứ tự cập nhật: khi chúng ta đạt tới vị trí mà`pref == 2 * target`, đầu tiên chúng ta sử dụng dòng điện`count_first`trước khi có khả năng cập nhật nó sau này. Điều này đảm bảo tính chính xác trong trường hợp cả hai điều kiện có thể tương tác trên các chỉ mục. 

điều kiện`i < n - 1`đảm bảo rằng vết cắt thứ hai không được đặt ở cuối, bảo toàn đoạn thứ ba không trống. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu: 

đầu vào:```
5
1 2 3 0 3
```Ở đây tổng số tiền là 9, vì vậy mục tiêu là 3. 

### Dấu vết 

| tôi | một [tôi] | tiền tố | đếm_first | trả lời | sự kiện | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 0 | không | 
| 1 | 2 | 3 | 1 | 0 | trước == T | 
| 2 | 3 | 6 | 1 | 1 | trước == 2T, thêm 1 | 
| 3 | 0 | 6 | 1 | 2 | pref == 2T nữa | 
| 4 | 3 | 9 | 1 | 2 | kết thúc | 

Dấu vết cho thấy có hai cách hợp lệ để chọn lần cắt đầu tiên ở tổng tiền tố 3 và lần cắt thứ hai ở tổng tiền tố 6. 

Điều này xác nhận rằng sự xuất hiện lặp đi lặp lại của$2T$tích lũy chính xác tất cả hợp lệ trước đó$T$các vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | một mảng truyền qua với công việc không đổi trên mỗi phần tử | 
| Không gian | O(1) | chỉ có một vài quầy được sử dụng | 

Thuật toán phù hợp thoải mái trong các ràng buộc cho$n = 5 \cdot 10^5$, vì nó chỉ thực hiện quét tuyến tính và số học theo thời gian không đổi cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    from sys import stdin
    n = int(stdin.readline())
    a = list(map(int, stdin.readline().split()))
    
    total = sum(a)
    if total % 3 != 0:
        return "0"
    
    target = total // 3
    pref = 0
    count_first = 0
    ans = 0
    
    for i in range(n):
        pref += a[i]
        if i < n - 1 and pref == 2 * target:
            ans += count_first
        if i < n - 1 and pref == target:
            count_first += 1
    
    return str(ans)

# provided sample
assert run("5\n1 2 3 0 3\n") == "2"

# all zeros small
assert run("3\n0 0 0\n") == "1"

# no solution due to indivisible sum
assert run("4\n1 1 1 1\n") == "0"

# multiple zeros creating many prefixes
assert run("5\n0 0 0 0 0\n") == "6"

# minimal invalid size
assert run("2\n1 2\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n0 0 0`|`1`| phân chia hợp lệ tối thiểu | 
|`4\n1 1 1 1`|`0`| tổng không thể chia | 
|`5\n0 0 0 0 0`|`6`| nhiều tiền tố có tổng bằng 0 | 
|`2\n1 2`|`0`| quá nhỏ cho 3 phần | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mảng chứa nhiều số 0, làm cho tổng tiền tố bằng cả hai$T$Và$2T$cực kỳ thường xuyên. 

Đối với đầu vào:```
5
0 0 0 0 0
```Tổng số tiền là 0, vì vậy$T = 0$. Mỗi tổng tiền tố là 0. 

Đi qua thuật toán: 

Tại mỗi chỉ số ngoại trừ chỉ số cuối cùng, mọi vị trí đều tăng`count_first`và còn góp phần`ans`khi đạt đến các vị trí sau này. Cấu trúc của quá trình quét đảm bảo rằng mỗi cặp lần cắt theo thứ tự hợp lệ được tính chính xác một lần, bởi vì mỗi lần cắt thứ hai sẽ tích lũy tất cả các lần cắt đầu tiên trước đó. 

Điều này chứng tỏ rằng ngay cả trong trường hợp suy biến trong đó tất cả các giá trị giống hệt nhau, ràng buộc thứ tự vẫn được bảo toàn hoàn toàn thông qua logic tích lũy một lần.
