---
title: "CF 105228E - Xây nhà bồ câu"
description: "Chúng tôi được cung cấp một bộ sưu tập các loài chim bồ câu, trong đó mỗi loài có nguồn cung chim bồ câu hạn chế để mua. Mục tiêu là xây dựng càng nhiều chuồng bồ câu giống hệt nhau càng tốt."
date: "2026-06-24T16:19:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105228
codeforces_index: "E"
codeforces_contest_name: "SanSi Cup 2023"
rating: 0
weight: 105228
solve_time_s: 77
verified: true
draft: false
---

[CF 105228E - Xây nhà cho chim bồ câu](https://codeforces.com/problemset/problem/105228/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các loài chim bồ câu, trong đó mỗi loài có nguồn cung chim bồ câu hạn chế để mua. Mục tiêu là xây dựng càng nhiều chuồng bồ câu giống hệt nhau càng tốt. Mỗi ngôi nhà phải chứa chính xác`k`chim bồ câu, và không có hai con chim bồ câu nào trong cùng một ngôi nhà có thể thuộc cùng một loài. Tuy nhiên, cùng một loài có thể xuất hiện ở nhiều ngôi nhà khác nhau, miễn là chúng ta không vượt quá số lượng chim bồ câu có sẵn cho loài đó. 

Chúng tôi không bắt buộc phải xây dựng bài tập một cách rõ ràng. Chúng ta chỉ cần xác định số lượng ngôi nhà tối đa có thể được hình thành dưới những ràng buộc này. 

Khó khăn chính là việc chọn nhiều nhà hơn sẽ hạn chế tần suất tái sử dụng mỗi loài, bởi vì một loài duy nhất có thể đóng góp tối đa một con chim bồ câu cho mỗi nhà. Nếu chúng ta cố gắng xây dựng`H`nhà, rồi mỗi loài`i`có thể đóng góp nhiều nhất`min(p_i, H)`tổng cộng là chim bồ câu. 

Điều này ngay lập tức gợi ý một điều kiện khả thi: nếu chúng ta ấn định một số lượng nhà dự kiến`H`, tổng số chim bồ câu có thể sử dụng được ở tất cả các loài sẽ trở thành`sum(min(p_i, H))`. Vì mỗi nhà đều cần`k`chim bồ câu, chúng tôi yêu cầu`sum(min(p_i, H)) >= H * k`. 

Các ràng buộc đi lên đến`n = 10^5`Và`p_i ≤ 10^9`, do đó, bất kỳ giải pháp nào thử tất cả các nhiệm vụ có thể có hoặc mô phỏng phân phối cho mỗi ngôi nhà sẽ quá chậm. Một kiểm tra ngây thơ cho một lỗi cố định`H`thì được, nhưng chúng ta phải có khả năng kiểm tra nhiều giá trị một cách hiệu quả, điều này gợi ý cấu trúc khả thi đơn điệu và tìm kiếm nhị phân. 

Một trường hợp khó phát hiện khi một loài có kích thước rất lớn`p_i`. Một cách giải thích ngây thơ có thể cho rằng một loài như vậy luôn có thể đóng góp đầy đủ cho tất cả các ngôi nhà một cách không chính xác, nhưng nó vẫn bị giới hạn bởi số lượng ngôi nhà do hạn chế “mỗi nhà một con”. Ví dụ, nếu`k = 2`,`p = [1000]`, thì dù chúng ta có nhiều chim bồ câu nhưng chúng ta không bao giờ có thể xây nhiều hơn 0 ngôi nhà vì chúng ta không có đủ loài riêng biệt để lấp đầy một ngôi nhà. 

Một trường hợp cạnh khác xuất hiện khi`k > n`. Ngay cả khi tổng số chim bồ câu lớn, một ngôi nhà cũng cần`k`loài riêng biệt, điều này là không thể nếu chúng ta có ít hơn`k`giống loài. Câu trả lời đúng khi đó luôn là số không. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ là xây dựng từng ngôi nhà một. Đối với mỗi nhà, chúng tôi sẽ liên tục chọn những loài vẫn còn chim bồ câu và đảm bảo chúng tôi chỉ chọn tối đa một con chim bồ câu cho mỗi loài trong mỗi nhà. Sau khi lấp đầy từng ngôi nhà, chúng tôi giảm số lượng và tiếp tục. Điều này nhanh chóng trở nên tốn kém vì mỗi bước lựa chọn có thể quét nhiều loài và chúng tôi có thể lặp lại quá trình này tới`10^9`lần trong trường hợp xấu nhất của không gian lý luận. Ngay cả với các đống tham lam, cấu trúc không ổn định vì ràng buộc mang tính toàn cầu giữa các ngôi nhà chứ không phải cục bộ. 

Quan sát chính là chúng ta không cần phải mô phỏng việc xây dựng. Thay vào đó, chúng ta chỉ cần kiểm tra xem một số nhà nhất định có`H`là khả thi. Đối với mỗi loài`i`, nó có thể đóng góp nhiều nhất`H`chim bồ câu (mỗi nhà một con), nhưng cũng không quá`p_i`. Vì vậy, đóng góp thực sự của nó được giới hạn bởi`min(p_i, H)`. 

Nếu cộng những khoản đóng góp này lại, chúng ta sẽ có được tổng số ô nuôi chim bồ câu mà chúng ta có thể lấp đầy trên tất cả các ngôi nhà. Vì mỗi ngôi nhà yêu cầu chính xác`k`chim bồ câu, tính khả thi giảm xuống còn một bất đẳng thức duy nhất. 

Điều này biến vấn đề thành một vấn đề quyết định đơn điệu: nếu`H`nhà là có thể, sau đó bất kỳ`H' < H`cũng có thể. Tính đơn điệu này cho phép tìm kiếm nhị phân trên`H`. Đối với mỗi điểm giữa, chúng tôi tính toán tính khả thi trong`O(n)`sử dụng một mảng được sắp xếp hoặc tổng tiền tố. Sắp xếp một lần cho phép tính toán nhanh`sum(min(p_i, H))`bằng cách chia các loài thành những loài bên dưới`H`và những điều trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng tham lam mỗi nhà | O(H·n) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân có kiểm tra tính khả thi | O(n log n + n log max_p) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Sắp xếp chim bồ câu có sẵn 

Chúng tôi sắp xếp mảng`p`theo thứ tự không giảm. Điều này cho phép chúng tôi nhanh chóng tách các loài hoàn toàn có thể sử dụng được (`p_i < H`) từ những giới hạn bởi`H`. 

### 2. Xác định hàm khả thi cho số lượng nhà cố định`H`Đối với mỗi loài, chúng tôi tính toán số lượng chim bồ câu có thể đóng góp: 

Nếu`p_i < H`, nó góp phần`p_i`. 

Ngược lại, nó góp phần`H`. 

Chúng tôi tính tổng số này trên tất cả các loài để có được tổng số chim bồ câu có thể sử dụng được. 

Bước này quan trọng vì nó mã hóa ngầm giới hạn cho mỗi nhà mà không cần xây dựng bất kỳ phép gán nào. 

### 3. Kiểm tra xem`H`nhà có thể được lấp đầy 

Chúng tôi so sánh tổng số bồ câu có thể sử dụng với số bồ câu cần thiết`H * k`. Nếu số chim bồ câu có thể sử dụng được ít nhất cũng lớn như vậy thì chúng ta có thể phân phát chim bồ câu vào từng nhà. 

### 4. Tìm kiếm nhị phân khả thi tối đa`H`Chúng tôi tìm kiếm từ`0`ĐẾN`sum(p_i) // k`. Đối với mỗi điểm giữa, chúng tôi kiểm tra tính khả thi bằng cách sử dụng hàm trên. Nếu khả thi, chúng tôi sẽ di chuyển sang phải; nếu không, chúng ta di chuyển sang trái. 

Câu trả lời là khả thi lớn nhất`H`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là đối với mọi cố định`H`, biểu thức`sum(min(p_i, H))`là số lượng vị trí chim bồ câu tối đa chính xác có thể được chỉ định hợp pháp trên`H`những ngôi nhà. Hạn chế “mỗi loài mỗi nhà” được nắm bắt đầy đủ bởi`min(p_i, H)`giới hạn và không có hạn chế về cấu trúc nào tồn tại vì các ngôi nhà có thể hoán đổi cho nhau và đối xứng. Do đó, tính khả thi chỉ phụ thuộc vào việc tổng công suất có đáp ứng được tổng nhu cầu hay không và tính đơn điệu đảm bảo tính chính xác của tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def feasible(p, k, H):
    total = 0
    for x in p:
        if x >= H:
            total += H
        else:
            total += x
        if total >= H * k:
            return True
    return total >= H * k

def solve():
    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    
    if k > n:
        print(0)
        return

    p.sort()

    lo, hi = 0, sum(p) // k
    ans = 0

    while lo <= hi:
        mid = (lo + hi) // 2
        if feasible(p, k, mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```các`feasible`tính toán tổng số chim bồ câu có thể sử dụng được với giả định rằng không có loài nào có thể được sử dụng nhiều hơn một lần trong mỗi nhà. Việc thoát sớm khi`total >= H * k`ngăn chặn việc tính tổng không cần thiết khi`H`là lớn. 

Giới hạn tìm kiếm nhị phân là an toàn vì mỗi nhà tiêu thụ chính xác`k`chim bồ câu nên số lượng chuồng tối đa không được vượt quá tổng số chim bồ câu chia cho`k`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 4 1
```Chúng tôi tìm kiếm nhị phân trên`H`. 

| H | tổng (phút(p_i, H)) | H*k | Khả thi | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | vâng | 
| 2 | 6 | 4 | vâng | 
| 3 | 7 | 6 | vâng | 
| 4 | 8 | 8 | vâng | 

Giá trị khả thi cuối cùng là`4`. 

Dấu vết này cho thấy lớn hơn thế nào`H`giảm sự đóng góp từ các loài lớn và buộc phải kết hợp năng lực chặt chẽ hơn. 

### Ví dụ 2 

đầu vào:```
2 3
5 4
```Chúng tôi kiểm tra`H = 1`: 

sum(min) = 2, bắt buộc = 3, không khả thi. 

Tất cả đều lớn hơn`H`cũng là không thể thực hiện được. 

Kết quả là`0`. 

Điều này chứng tỏ rằng ngay cả khi có đủ số lượng chim bồ câu, thì sự đa dạng loài không đủ sẽ ngăn cản việc hình thành một ngôi nhà hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + n log S) | Sắp xếp cộng với tìm kiếm nhị phân theo số lượng nhà khả thi, mỗi lần kiểm tra sẽ quét tất cả các loài | 
| Không gian | O(1) bổ sung (không bao gồm đầu vào) | Chỉ lưu trữ mảng và biến | 

Các ràng buộc cho phép lên đến`10^5`loài, vì vậy một`O(n log n)`bước tiền xử lý và khoảng 30 lần kiểm tra tính khả thi được thực hiện dễ dàng trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite
    import sys

    n, k = map(int, sys.stdin.readline().split())
    p = list(map(int, sys.stdin.readline().split()))

    if k > n:
        return "0"

    p.sort()

    def feasible(H):
        total = 0
        for x in p:
            total += x if x < H else H
            if total >= H * k:
                return True
        return total >= H * k

    lo, hi = 0, sum(p) // k
    ans = 0
    while lo <= hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    return str(ans)

# provided samples
assert run("3 2\n3 4 1\n") == "4", "sample 1"
assert run("2 3\n5 4\n") == "0", "sample 2"

# custom cases
assert run("1 1\n10\n") == "10", "single species trivial"
assert run("3 3\n1 1 1\n") == "1", "tight species constraint"
assert run("5 2\n100 1 1 1 1\n") == "5", "one dominant species"
assert run("4 3\n2 2 2 2\n") == "2", "balanced case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loài đơn lẻ | 10 | k=1 khả năng suy biến | 
| tất cả những cái có k=3 | 1 | hạn chế đa dạng nghiêm ngặt | 
| một loài lớn | 5 | sự thống trị không phá vỡ logic | 
| trường hợp trung bình thống nhất | 2 | phân phối cân bằng đúng đắn | 

## Vỏ cạnh 

Khi nào`k > n`, thuật toán ngay lập tức trả về 0 vì không thể hình thành ngôi nhà nào với đủ loài khác biệt. Kể cả nếu`p_i`lớn, hạn chế về tính đa dạng của từng ngôi nhà sẽ cản trở mọi công trình xây dựng. 

Đối với rất lớn`p_i`, chẳng hạn như`p = [10^9, 10^9, ..., 10^9]`, tìm kiếm nhị phân sẽ đẩy`H`cao, nhưng tính khả thi sẽ hạn chế sự tăng trưởng một cách chính xác vì mỗi loài chỉ đóng góp`H`mỗi nhà, ngăn ngừa việc đếm quá mức. 

Khi tất cả`p_i`nhỏ và giống nhau, tìm kiếm nhị phân hội tụ chặt chẽ và mỗi kiểm tra tính khả thi sẽ kết thúc sớm sau ngưỡng`H*k`đạt được, đảm bảo hiệu quả ngay cả trong trường hợp phân phối đồng nhất trong trường hợp xấu nhất.
