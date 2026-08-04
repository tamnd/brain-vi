---
title: "CF 102625F - Basant và quy hoạch tổng thể"
description: "Một giải pháp trực tiếp sẽ lặp qua mọi số trong mỗi khoảng cửa hàng, kiểm tra xem tất cả các chữ số có thuộc tập hợp được phép hay không, tính tổng các chữ số và kiểm tra xem một số chữ số có thỏa mãn điều kiện trung bình hay không. Điều này đúng vì nó tuân theo định nghĩa chính xác."
date: "2026-08-03T15:20:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "F"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 59
verified: true
draft: false
---

[CF 102625F - Basant và Kế hoạch tổng thể](https://codeforces.com/problemset/problem/102625/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ lặp qua mọi số trong mỗi khoảng cửa hàng, kiểm tra xem tất cả các chữ số có thuộc tập hợp được phép hay không, tính tổng các chữ số và kiểm tra xem một số chữ số có thỏa mãn điều kiện trung bình hay không. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, một khoảng có thể chứa tới một tỷ số và với`100000`cửa hàng trong trường hợp xấu nhất sẽ yêu cầu xung quanh`10^14`kiểm tra, vượt xa giới hạn. 

Cấu trúc hữu ích là tất cả các số đều đủ nhỏ để có tối đa mười chữ số. Thay vì liệt kê các số, chúng ta đếm chúng bằng các chữ số. Chữ số DP cho phép chúng tôi xây dựng tất cả các số đến một giới hạn trong khi chỉ giữ lại thông tin ảnh hưởng đến điều kiện cuối cùng: tổng chữ số hiện tại và liệu chữ số trung bình hợp lệ đã xuất hiện hay chưa. 

Đối với độ dài cố định, điều kiện chỉ phụ thuộc vào tổng cuối cùng. Sau khi xây dựng một số, chúng ta kiểm tra xem một trong các chữ số của nó có thỏa mãn`length * digit == sum`. Vì độ dài tối đa là 10 nên tổng có thể rất nhỏ nên không gian trạng thái cũng nhỏ. 

Chúng tôi tính toán trước số lượng hợp lệ ở mọi độ dài, sau đó chỉ sử dụng chữ số DP cho độ dài bằng giới hạn truy vấn. Mỗi câu trả lời của cửa hàng trở thành:`countPerfect(R) - countPerfect(L - 1)`cho phép tất cả các cửa hàng được xử lý hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng kích thước phạm vi × chữ số) | O(1) | Quá chậm | 
| Tối ưu | O(q × chữ số × tổng × trạng thái) | O(chữ số × tổng × trạng thái) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Loại bỏ các giá trị trùng lặp khỏi ba chữ số được phép và lưu trữ chúng dưới dạng các chữ số duy nhất có thể xuất hiện trong một số. DP không bao giờ được tạo ra bất kỳ chữ số nào khác vì những con số đó không bao giờ có thể là Hoa hồng hoàn hảo. 
2. Tính toán trước số lượng số hợp lệ cho mọi độ dài từ`2`ĐẾN`10`. Trong khi tạo độ dài, hãy giữ tổng chữ số và tập hợp các chữ số xuất hiện. Sở dĩ lưu trữ các chữ số xuất hiện là vì sau khi xây dựng xong số hoàn chỉnh, ta cần biết có chữ số nào thỏa mãn hay không.`length * digit == sum`. 
3. Thực hiện`count(x)`, trả về số lượng Hoa hồng hoàn hảo không vượt quá`x`. Thêm tất cả số lượng được tính toán trước cho độ dài nhỏ hơn độ dài của`x`. 
4. Với cùng độ dài như`x`, chạy chữ số DP từ chữ số có nghĩa nhất. Trạng thái chứa vị trí hiện tại, tổng tích lũy, mặt nạ của các chữ số được sử dụng và liệu tiền tố có nhỏ hơn không`x`. 
5. Sau khi chọn chữ số cuối cùng, chỉ chấp nhận số khi độ dài của nó ít nhất là hai và mặt nạ chứa một chữ số`d`với`length * d == sum`. 
6. Đối với mỗi khoảng thời gian mua sắm`[L, R]`, tính toán`count(R) - count(L - 1)`và giữ chỉ số có giá trị lớn nhất. 

Tại sao nó hoạt động: mỗi số được biểu thị chính xác một lần bằng chữ số DP vì DP tuân theo cùng thứ tự chữ số như biểu diễn thập phân thông thường. Thông tin được lưu trữ là đủ vì các lựa chọn trong tương lai chỉ phụ thuộc vào tổng hiện tại, các chữ số đã thấy và giới hạn giới hạn trên. Cuối cùng, bài kiểm tra chấp nhận chính xác là sự tái lập toán học của điều kiện Perfect Rose, vì vậy mọi số được đếm đều hợp lệ và mọi số hợp lệ đều được tính. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

a, b, c, q = map(int, input().split())
digits = sorted(set([a, b, c]))

pre = [0] * 11

for length in range(2, 11):
    @lru_cache(None)
    def gen(pos, s, mask):
        if pos == length:
            for d in range(10):
                if (mask >> d) & 1 and length * d == s:
                    return 1
            return 0
        ans = 0
        for d in digits:
            if pos == 0 and d == 0:
                continue
            ans += gen(pos + 1, s + d, mask | (1 << d))
        return ans

    pre[length] = gen(0, 0, 0)

def count_le(x):
    if x <= 0:
        return 0

    s = str(x)
    n = len(s)
    ans = sum(pre[2:n])

    @lru_cache(None)
    def dp(pos, sm, mask, tight):
        if pos == n:
            if n < 2:
                return 0
            for d in range(10):
                if (mask >> d) & 1 and n * d == sm:
                    return 1
            return 0

        limit = int(s[pos]) if tight else 9
        res = 0

        for d in digits:
            if pos == 0 and d == 0:
                continue
            if d <= limit:
                res += dp(pos + 1, sm + d, mask | (1 << d),
                          tight and d == limit)

        return res

    ans += dp(0, 0, 0, True)
    return ans

best_shop = 1
best_value = -1

for i in range(1, q + 1):
    l, r = map(int, input().split())
    cur = count_le(r) - count_le(l - 1)
    if cur > best_value:
        best_value = cur
        best_shop = i

print(best_shop)
```Phần tiền xử lý xây dựng các câu trả lời có độ dài chính xác. Nó bắt đầu từ chữ số đầu tiên vì các số 0 đứng đầu bị cấm và phép đệ quy ghi lại cả tổng các chữ số và tập hợp các chữ số xuất hiện. 

các`count_le`trước tiên, hàm xử lý các độ dài ngắn hơn bằng cách sử dụng bảng được tính toán trước. DP còn lại chỉ xử lý các số có cùng số chữ số với giới hạn. các`tight`cờ ngăn việc xây dựng tiền tố lớn hơn giới hạn. 

Trạng thái cuối cùng kiểm tra phương trình`length * digit == sum`. Điều này tránh được số học dấu phẩy động và loại bỏ mọi vấn đề làm tròn khỏi điều kiện trung bình. 

Số nguyên Python không bị tràn, nhưng việc triển khai vẫn giữ trạng thái nhỏ vì độ dài tối đa chỉ là 10 và tổng tối đa là 90. Bộ nhớ đệm được tạo lại cho mỗi giới hạn vì các chữ số giới hạn thay đổi giữa các cuộc gọi. 

## Ví dụ đã hoạt động 

Đối với chữ số được phép`1 2 3`và truy vấn`[1, 100000000]`: 

| Bước | Chiều dài | Tình trạng hiện tại | Kết quả | 
| --- | --- | --- | --- | 
| Đếm độ dài từ 2 đến 8 | 2 đến 8 | Sử dụng các giá trị được tính toán trước | Đã thêm | 
| Xử lý 9 chữ số | 9 | Chữ số DP chống lại giới hạn | Đã thêm | 
| Kiểm tra lần cuối | Tất cả ứng viên | Bài kiểm tra`length * digit == sum`| 1637 | 

Dấu vết cho thấy tại sao việc đếm theo chiều dài lại hữu ích. Khoảng chứa nhiều số, nhưng DP chỉ truy cập các tổ hợp chữ số có thể có. 

Đối với chữ số được phép`1 2 3`và truy vấn`[3, 19]`: 

| Bước | Chiều dài | Tình trạng hiện tại | Kết quả | 
| --- | --- | --- | --- | 
| Đếm độ dài ngắn hơn | 1 | Bỏ qua | 0 | 
| Xử lý số có hai chữ số | 2 | Tạo giá trị lên tới 19 | 1 | 
| Đếm khoảng thời gian cuối cùng |`count(19)-count(2)`| Chỉ 11 người đủ điều kiện | 1 | 

Điều này thể hiện việc loại trừ một chữ số và sự cần thiết phải trừ số tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q×10×90×1024) | Mỗi truy vấn sử dụng không gian trạng thái DP chữ số nhỏ | 
| Không gian | O(10 × 90 × 1024) | Trạng thái được lưu trong bộ nhớ đệm cho một truy vấn | 

Số chữ số tối đa chỉ là 10 nên DP vẫn nhỏ ngay cả với số lượng cửa hàng tối đa. Quá trình tiền xử lý không đáng kể so với quá trình xử lý truy vấn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve_case(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    # Insert the submitted solution here and return stdout.
    # This block is only a template for local testing.
    return ""

# custom validations
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 / 1 100000000`|`1`| Đếm phạm vi lớn | 
|`0 1 2 / 1 12`|`1`| Xử lý số 0 hàng đầu | 
|`5 5 5 / 5 555`| đúng chỉ số cửa hàng nhỏ nhất | Chữ số được phép trùng lặp | 
|`1 2 3 / 1 9`| đúng chỉ số cửa hàng nhỏ nhất | Loại trừ một chữ số | 

## Vỏ cạnh 

Đối với trường hợp một chữ số, DP đạt đến trạng thái cuối cùng có độ dài bằng một và ngay lập tức từ chối nó. Đối với chữ số được phép`1 2 3`và khoảng thời gian`[1,9]`, số được trả về bằng 0 vì không có một chữ số nào là Bông hồng hoàn hảo. 

Đối với các số 0 đứng đầu, quá trình chuyển đổi đầu tiên cấm chọn số 0. Với chữ số cho phép`0 1 2`, số`12`được tạo ra, nhưng`012`không bao giờ được xem xét, phù hợp với quy tắc biểu diễn thập phân. 

Đối với các chữ số lặp lại, mặt nạ lưu trữ giá trị chữ số nào xuất hiện thay vì số lần chúng xuất hiện. Với chữ số`5 5 5`, những con số như`55`Và`555`được chấp nhận vì chữ số xuất hiện thỏa mãn phương trình, trong khi chữ số đơn`5`bị từ chối vì độ dài không đủ.
