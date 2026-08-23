---
title: "CF 104283A - Một tuyên bố ngắn khác"
description: "Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn xác định một khoảng số khép kín từ l đến r, cùng với hai tham số: tổng chữ số đích x và thứ hạng k. Trong khoảng đó, về mặt khái niệm, chúng ta xem xét tất cả các số nguyên dương có tổng các chữ số bằng chính xác x."
date: "2026-07-01T21:00:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104283
codeforces_index: "A"
codeforces_contest_name: "Contest Based on Brain Craft Intra SUST Programming Contest 2023"
rating: 0
weight: 104283
solve_time_s: 56
verified: true
draft: false
---

[CF 104283A - Một tuyên bố ngắn khác](https://codeforces.com/problemset/problem/104283/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn xác định một khoảng số đóng từ`l`ĐẾN`r`, cùng với hai tham số: tổng chữ số mục tiêu`x`và một thứ hạng`k`. Trong khoảng đó, về mặt khái niệm, chúng ta xem xét tất cả các số nguyên dương có tổng các chữ số bằng chính xác`x`. Nếu chúng ta liệt kê những số đó theo thứ tự tăng dần thì nhiệm vụ là trả về`k`-phần tử thứ của danh sách được lọc này. Nếu ít hơn`k`những con số như vậy tồn tại, câu trả lời là`-1`. 

Khó khăn cốt lõi không phải là lọc số mà là thực hiện nhanh chóng. Các giá trị của`l`Và`r`đủ lớn để việc lặp qua mọi số nguyên trong phạm vi là không khả thi. Ngay cả khi một truy vấn duy nhất cho phép tối đa khoảng 10^18 giá trị, việc quét chúng trực tiếp sẽ vượt xa mọi giới hạn thời gian. 

Các ràng buộc về tổng chữ số đưa ra một cấu trúc độc lập với độ dài khoảng. Thay vì suy luận về từng số riêng lẻ, bài toán giảm xuống việc đếm xem có bao nhiêu số hợp lệ tồn tại đến một giới hạn nhất định. Khi chúng ta có thể trả lời các truy vấn tiền tố có dạng “có bao nhiêu số nguyên ≤ n có tổng chữ số chính xác là x”, phiên bản khoảng sẽ trở thành sự khác biệt của hai số tiền tố. 

Một cách tiếp cận ngây thơ sẽ liệt kê tất cả các số trong`[l, r]`, tính tổng các chữ số, thu thập các số hợp lệ và chọn số thứ k. Điều này ngay lập tức thất bại khi`r - l`là lớn. Ví dụ, nếu`l = 1`Và`r = 10^18`, thậm chí một lần vượt qua là không thể. 

Một trường hợp lỗi tinh tế hơn xuất hiện khi thực hiện lọc tổng chữ số mà không ghi nhớ. Việc tính lại tổng các chữ số trên mỗi số không tốn kém nhưng vẫn yêu cầu lặp lại toàn bộ phạm vi. Điểm nghẽn là số lượng chứ không phải chi phí cho mỗi số. 

Trường hợp cạnh thực sự là khi`k`lớn nhưng số lượng hợp lệ lại thưa thớt. Ví dụ, trong một phạm vi như`[10^17, 10^18]`với một ràng buộc tổng chữ số chặt chẽ như`x = 1`, chỉ có một vài số hợp lệ (như lũy thừa của mười). Quét tuyến tính sẽ thực hiện gần một nghìn tỷ lần kiểm tra vô ích trước khi đưa ra câu trả lời. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp từ`l`ĐẾN`r`, tính tổng các chữ số cho mỗi số, lưu trữ những số đó`x`và trả về phần tử thứ k. Điều này đúng vì nó trực tiếp xây dựng thứ tự được yêu cầu. Tuy nhiên, nó thực hiện`O(r - l + 1)`lặp lại cho mỗi truy vấn và trong trường hợp xấu nhất, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là điều kiện “tổng chữ số bằng x” thân thiện với tiền tố. Thay vì quét theo khoảng thời gian, chúng ta có thể đếm xem có bao nhiêu số hợp lệ nằm dưới ngưỡng`n`. Nếu chúng ta định nghĩa một hàm`F(n, x)`bằng số số nguyên trong`[0, n]`tổng các chữ số của nó chính xác là`x`, sau đó đếm vào`[l, r]`trở thành`F(r, x) - F(l - 1, x)`. 

Điều này biến bài toán thành bài toán đếm trên các chữ số, đó chính xác là mục đích của lập trình động chữ số. Khi chúng ta có thể tính toán số lượng tiền tố một cách hiệu quả, chúng ta có thể tìm ra câu trả lời bằng cách tìm kiếm nhị phân số nhỏ nhất`y`TRONG`[l, r]`sao cho số số nguyên hợp lệ lên tới`y`bên trong khoảng đạt`k`. 

Cấu trúc trở thành: chữ số DP để đếm và tìm kiếm nhị phân để định vị phần tử thứ k. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r − l) mỗi truy vấn | O(1) | Quá chậm | 
| Chữ số DP + Tìm kiếm nhị phân | O(log R · chữ số · x) cho mỗi truy vấn | O(chữ số · x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề theo hai lớp khái niệm: đếm các số hợp lệ đến một giới hạn và sử dụng hàm đếm đó để xác định số hợp lệ thứ k trong một phạm vi. 

### 1. Xây dựng hàm đếm DP chữ số 

Chúng tôi xác định một chức năng`count(n, x)`trả về bao nhiêu số nguyên trong`[0, n]`có tổng chữ số chính xác`x`. Chúng tôi xử lý các chữ số từ quan trọng nhất đến ít quan trọng nhất, theo dõi tổng còn lại và liệu chúng tôi có còn bị giới hạn bởi tiền tố của`n`. 

Tại mỗi vị trí chữ số, chúng tôi thử tất cả các chữ số có thể không vượt quá ràng buộc giới hạn hiện tại. Chúng tôi giảm số tiền còn lại cho phù hợp và tiếp tục đệ quy. Các trạng thái được ghi nhớ theo vị trí, số tiền còn lại và ràng buộc chặt chẽ. 

Điều này có tác dụng vì bất kỳ số nào cũng được biểu diễn duy nhất bằng chuỗi chữ số của nó và tính khả thi chỉ phụ thuộc vào dung lượng tổng chữ số còn lại. 

### 2. Chuyển đổi truy vấn phạm vi thành số lượng tiền tố 

Đối với mỗi truy vấn, chúng tôi tính toán có bao nhiêu số hợp lệ tồn tại trong khoảng`[l, r]`sử dụng:`total = count(r, x) − count(l − 1, x)`Nếu như`total < k`, câu trả lời là ngay lập tức`-1`. 

Bước này có hiệu quả vì chữ số DP cho biết số đếm tích lũy và phép trừ sẽ tách biệt khoảng thời gian. 

### 3. Tìm kiếm nhị phân số hợp lệ thứ k 

Chúng tôi tìm kiếm số nhỏ nhất`y`TRONG`[l, r]`như vậy:`count(y, x) − count(l − 1, x) ≥ k`Vị ngữ này đơn điệu trong`y`, do đó tìm kiếm nhị phân được áp dụng một cách rõ ràng. Vị trí đầu tiên mà điều kiện trở thành đúng chính xác là số hợp lệ thứ k. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ hai cấu trúc đơn điệu. Đầu tiên,`count(n, x)`không giảm trong`n`, vì việc thêm giới hạn trên lớn hơn không bao giờ loại bỏ các số hợp lệ. Thứ hai, giới hạn khoảng thời gian`[l, r]`duy trì tính đơn điệu sau khi trừ tiền tố cố định`count(l - 1, x)`. Điều này đảm bảo tìm kiếm nhị phân luôn hội tụ đến ngưỡng hợp lệ đầu tiên nơi phần tử thứ k xuất hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from functools import lru_cache

def digit_dp(n, target_sum):
    s = str(n)

    @lru_cache(None)
    def dfs(pos, remaining, tight):
        if remaining < 0:
            return 0
        if pos == len(s):
            return 1 if remaining == 0 else 0

        limit = int(s[pos]) if tight else 9
        res = 0

        for d in range(limit + 1):
            res += dfs(pos + 1, remaining - d, tight and d == limit)

        return res

    return dfs(0, target_sum, True)

def count_upto(n, x):
    if n < 0:
        return 0
    return digit_dp(n, x)

def solve():
    t = int(input())
    for _ in range(t):
        l, r, k, x = map(int, input().split())

        base = count_upto(l - 1, x)
        total = count_upto(r, x) - base

        if total < k:
            print(-1)
            continue

        lo, hi = l, r
        ans = r

        while lo <= hi:
            mid = (lo + hi) // 2
            cur = count_upto(mid, x) - base

            if cur >= k:
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        print(ans)

if __name__ == "__main__":
    solve()
```Hàm DP chữ số`digit_dp`chịu trách nhiệm đếm các số hợp lệ đến một ranh giới. Trạng thái quan trọng là`(pos, remaining_sum, tight)`, Ở đâu`tight`đảm bảo chúng tôi không vượt quá tiền tố của`n`. Việc ghi nhớ là cần thiết vì nếu không có bộ nhớ đệm, phép đệ quy sẽ lặp lại các bài toán con giống hệt nhau theo cấp số nhân nhiều lần. 

chức năng`count_upto`bao bọc DP này và xử lý trường hợp cạnh khi`n < 0`, cần thiết khi tính toán`count(l - 1, x)`. 

Vòng lặp chính tính toán số lượng ứng viên hợp lệ trong khoảng và sau đó thực hiện tìm kiếm nhị phân trên`[l, r]`. Vị từ bên trong tìm kiếm nhị phân sử dụng lại logic đếm tương tự, được dịch chuyển theo đường cơ sở tiền tố. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản: 

đầu vào:```
1
1 30 3 3
```Chúng ta muốn các số từ 1 đến 30 có tổng chữ số là 3: đó là 3, 12, 21, 30. Vậy danh sách là`[3, 12, 21, 30]`. 

Nếu như`k = 2`, chúng tôi mong đợi 12. 

| Bước | giữa | count(mid,3) − count(0,3) | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 15 | 2 | ở giữa hợp lệ, di chuyển sang trái | 
| 2 | 8 | 1 | quá nhỏ | 
| 3 | 12 | 2 | thắt chặt trái | 

Câu trả lời cuối cùng là 12. 

Dấu vết này cho thấy cách chữ số DP xác định thứ tự tiền tố và tìm kiếm nhị phân trích xuất phần tử được xếp hạng chính xác. 

Bây giờ hãy xem xét trường hợp không tồn tại câu trả lời: 

đầu vào:```
1
10 20 5 50
```Không có số trong`[10, 20]`có tổng chữ số là 50 nên tổng số bằng 0. Thuật toán ngay lập tức trở lại`-1`sau lần kiểm tra sự khác biệt tiền tố đầu tiên mà không cần nhập tìm kiếm nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · log R · chữ số · x) | mỗi bước tìm kiếm nhị phân gọi chữ số đếm DP | 
| Không gian | O(chữ số · x) | bảng ghi nhớ các trạng thái chữ số | 

Độ dài chữ số được giới hạn bởi kích thước của`r`, thường có tới 18 chữ số và`x`đủ nhỏ để các trạng thái DP có thể quản lý được. Hệ số logarit từ tìm kiếm nhị phân giúp mỗi truy vấn hoạt động hiệu quả ngay cả trong nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("1\n1 1 1 1\n") == "1"

# simple range
assert run("1\n1 30 2 3\n") == "12"

# no valid numbers
assert run("1\n10 20 1 50\n") == "-1"

# multiple tests
assert run("2\n1 10 1 1\n1 10 2 1\n") == "1\n10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khớp số đơn | 1 | độ đúng cơ sở | 
| xếp hạng khoảng thời gian nhỏ | 12 | đặt hàng qua tìm kiếm nhị phân | 
| tổng chữ số không thể | -1 | từ chối sớm | 
| nhiều truy vấn | 1, 10 | xử lý nhiều bài kiểm tra | 

## Vỏ cạnh 

Một trường hợp phức tạp xảy ra khi`l = 0`hoặc`l = 1`, kể từ khi tính toán`count(l - 1, x)`yêu cầu xử lý`-1`. Việc triển khai trả về 0 một cách rõ ràng cho các đầu vào âm, đảm bảo phép trừ tiền tố hoạt động chính xác. 

Một trường hợp tế nhị khác là khi`x = 0`. Chỉ có số`0`thỏa mãn điều này, vì vậy các phạm vi không bao gồm số 0 phải trả về ngay lập tức`-1`. DP xử lý chính xác việc này vì nó chỉ chấp nhận tiêu thụ toàn bộ tổng chữ số ở cuối số. 

Trường hợp cạnh cuối cùng xuất hiện khi`k`bằng tổng số giá trị hợp lệ trong phạm vi. Trong trường hợp đó, tìm kiếm nhị phân vẫn phải hội tụ về`r`nếu như`r`bản thân nó là hợp lệ. Vị từ đơn điệu đảm bảo rằng ranh giới bên phải được chọn chính xác mà không có lỗi sai sót nào.
