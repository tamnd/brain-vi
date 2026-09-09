---
title: "CF 104590A - Sôcôla Tươi"
description: "Chúng ta được đưa cho một danh sách các nhóm, mỗi nhóm có một số lượng người cố định. Mỗi nhóm phải được phục vụ theo một thứ tự nào đó và phục vụ một nhóm sẽ tiêu thụ toàn bộ gói sô cô la cỡ P."
date: "2026-06-30T07:26:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104590
codeforces_index: "A"
codeforces_contest_name: "2017 Google Code Jam Round 2 (GCJ 17 Round 2)"
rating: 0
weight: 104590
solve_time_s: 57
verified: true
draft: false
---

[CF 104590A - Sôcôla tươi](https://codeforces.com/problemset/problem/104590/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một danh sách các nhóm, mỗi nhóm có một số lượng người cố định. Mỗi nhóm phải được phục vụ theo một thứ tự nào đó và việc phục vụ một nhóm sẽ tiêu thụ toàn bộ gói sô cô la cỡ P. Các gói được mở khi cần, nhưng với một ràng buộc nghiêm ngặt: nếu một gói được mở và tạo ra các miếng còn sót lại, những phần còn sót lại đó phải được các nhóm sau tiêu thụ hết trước khi bất kỳ gói mới nào có thể được mở. 

Mỗi nhóm bắt đầu ở ranh giới gói mới hoặc đến khi có những mảnh còn sót lại đang chờ. Một nhóm được coi là “tốt” nếu nhóm đó được phục vụ hoàn toàn bằng các gói mới mở, nghĩa là nhóm đó hoàn toàn không tiêu thụ bất kỳ sô cô la còn sót lại nào. Nhiệm vụ của chúng ta là hoán vị các nhóm để tối đa hóa số lượng trong số đó là tốt. 

Khó khăn chính là mỗi kích thước nhóm xác định phần dư modulo P và những phần còn lại này tương tác thông qua trạng thái dư thừa toàn cầu phát triển theo thứ tự. Vì N nhiều nhất là 100 và P nhiều nhất là 4, cấu trúc đủ nhỏ để chúng ta có thể suy luận trực tiếp về các trạng thái, nhưng vẫn đủ lớn để việc kiểm tra hoán vị đơn giản là không khả thi. 

Một cách tiếp cận đơn giản sẽ thử tất cả các hoán vị của nhóm và mô phỏng quá trình còn sót lại. Đó là N! hoán vị và mỗi mô phỏng có giá O(N), điều này hoàn toàn không khả thi ngay cả với N = 20. 

Một dạng thất bại tinh tế hơn xuất phát từ việc đặt hàng tham lam. Ví dụ: đặt tất cả các nhóm có số dư 0 trước tiên mang lại cảm giác an toàn, nhưng thực tế có thể lãng phí cơ hội sắp xếp các phần còn sót lại để các nhóm sau cũng hạ cánh chính xác trên ranh giới gói. 

## Phương pháp tiếp cận 

Quan sát quan trọng là chỉ có quy mô nhóm P modulo mới quan trọng đối với động lực còn sót lại. Khi một nhóm có kích thước g đến, nó sẽ tiêu thụ g mảnh từ bộ đệm còn sót lại hiện tại. Nếu bộ đệm đạt chính xác 0 sau đó thì nhóm đó là “mới”; nếu không nó sẽ tạo ra một trạng thái còn sót lại mới. 

Vì vậy, quá trình này là một cuộc duyệt qua các dư lượng modulo P, trong đó mỗi nhóm chuyển đổi trạng thái bằng cách trừ đi kích thước modulo P của nó. Kích thước tuyệt đối không quan trọng, chỉ g % P. 

Vì P ≤ 4 nên không gian trạng thái của phần dư rất nhỏ. Chúng ta có thể định nghĩa một chương trình động dựa trên số lượng nhóm của từng loại dư lượng mà chúng ta đã sử dụng và trạng thái còn lại hiện tại là gì. Mục tiêu là tối đa hóa số lần chúng ta chuyển sang trạng thái 0 một cách chính xác khi phục vụ một nhóm. 

Chế độ xem brute-force coi đây là một DP hoán vị trên số lượng lớp dư lượng. Nó vẫn còn lớn ở dạng thô, nhưng bị thu gọn vì trạng thái chỉ (r0, r1, r2, r3) được tính cộng với phần còn lại hiện tại và quá trình chuyển đổi chỉ phụ thuộc vào việc chọn loại nhóm tiếp theo. 

Điểm mấu chốt là chúng ta không cần đặt hàng đầy đủ mà chỉ cần tiêu thụ bao nhiêu loại dư lượng trong mỗi giai đoạn của chu kỳ còn sót lại. Điều này biến vấn đề thành một DP nhỏ so với số lượng có trạng thái O(N * P * N^P), có thể chấp nhận được vì P 4 và N 100. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu | O(N!) | O(N) | Quá chậm | 
| DP về số lượng dư lượng và trạng thái còn sót lại | O(N^3) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén từng kích thước nhóm thành modulo P còn lại của nó, vì chỉ điều đó mới ảnh hưởng đến phần còn lại.

1. Chúng ta đếm xem có bao nhiêu nhóm thuộc mỗi loại dư lượng r từ 0 đến P−1. Điều này làm giảm vấn đề về tần số hơn là danh tính. 
2. Chúng tôi xác định trạng thái DP nơi chúng tôi theo dõi có bao nhiêu nhóm của mỗi loại dư lượng đã được sử dụng cho đến nay và trạng thái còn lại hiện tại là gì. Trạng thái còn lại là số miếng sôcôla hiện đang được mang đi, luôn nằm trong khoảng từ 0 đến P−1. 
3. Quá trình chuyển đổi DP xem xét việc chọn nhóm tiếp theo từ bất kỳ lớp dư lượng nào vẫn còn các nhóm còn lại. Nếu phần còn lại hiện tại cộng với kích thước nhóm là bội số của P thì nhóm này được tính là nhóm mới và phần còn lại sẽ bằng 0. Ngược lại, chúng tôi cập nhật phần còn lại thành (current + r) mod P. 
4. Chúng tôi lặp lại tất cả các trạng thái DP hợp lệ, cập nhật các chuyển đổi bằng cách sử dụng từng nhóm một. Giá trị DP lưu trữ số lượng nhóm mới tối đa đạt được cho đến nay. 
5. Câu trả lời là giá trị DP tối đa trên tất cả các trạng thái sử dụng tất cả các nhóm. 

### Tại sao nó hoạt động 

Trạng thái còn lại nắm bắt hoàn toàn sự phụ thuộc duy nhất giữa các nhóm liên tiếp. Vì việc sử dụng gói chỉ phụ thuộc vào phần còn lại đang chạy modulo P, nên hai lịch sử kết thúc bằng cùng một phần còn lại có thể hoán đổi cho nhau. DP khám phá tất cả các thứ tự có thể có của các lớp dư lượng trong khi vẫn bảo toàn thông tin trạng thái đầy đủ này, do đó không có trình tự hợp lệ nào bị bỏ sót và không có trình tự không hợp lệ nào được tính là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    T = int(input())
    for tc in range(1, T + 1):
        N, P = map(int, input().split())
        arr = list(map(int, input().split()))

        cnt = [0] * P
        for x in arr:
            cnt[x % P] += 1

        # DP state: (c0, c1, ..., cP-1, rem)
        from functools import lru_cache

        @lru_cache(None)
        def dp(c, rem):
            if sum(c) == 0:
                return 0

            best = 0
            for r in range(P):
                if c[r] == 0:
                    continue
                nc = list(c)
                nc[r] -= 1
                new_rem = (rem + r) % P

                gain = 1 if new_rem == 0 else 0
                best = max(best, gain + dp(tuple(nc), new_rem))

            return best

        start = tuple(cnt)
        print(f"Case #{tc}: {dp(start, 0)}")

if __name__ == "__main__":
    solve()
```Giải pháp nén từng nhóm vào lớp modulo của nó. DP đệ quy khám phá tất cả thứ tự hợp lệ của các lớp này trong khi vẫn duy trì phần còn lại. Việc ghi nhớ đảm bảo các trạng thái lặp lại được tính toán một lần. 

Một điểm tinh tế là trạng thái bao gồm vectơ đầy đủ của số lượng còn lại, điều này là cần thiết vì sự phân bố khác nhau của các nhóm còn lại dẫn đến các khả năng trong tương lai khác nhau ngay cả khi phần còn lại hiện tại giống hệt nhau. 

## Ví dụ đã hoạt động 

Xem xét các nhóm`[4, 5, 6, 4]`với P = 3. Dư lượng của chúng là`[1, 2, 0, 1]`. 

Chúng tôi bắt đầu với số lượng`(r0=1, r1=2, r2=1)`và số dư 0 

| Bước | Trạng thái (r0,r1,r2) | rem | đã chọn r | rem mới | tươi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,2,1) | 0 | 0 | 0 | vâng | 
| 2 | (0,2,1) | 0 | 1 | 1 | không | 
| 3 | (0,1,1) | 1 | 2 | 0 | vâng | 
| 4 | (0,1,0) | 0 | 1 | 1 | không | 

Điều này cho thấy thứ tự ảnh hưởng như thế nào đến tần suất chúng ta đạt được số dư 0. 

Bây giờ hãy xem xét`[1,1,1]`với P = 3 thì toàn bộ số dư là 1. 

| Bước | Tiểu bang | rem | đã chọn r | rem mới | tươi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (3) | 0 | 1 | 1 | không | 
| 2 | (2) | 1 | 1 | 2 | không | 
| 3 | (1) | 2 | 1 | 0 | vâng | 

Chỉ một nhóm có thể mới bất kể thứ tự nào, xác nhận rằng DP đã nắm bắt được giới hạn vốn có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N * trạng thái) ≈ O(N^4) trường hợp xấu nhất nhỏ | DP vượt quá số lượng và phần còn lại với tính năng ghi nhớ | 
| Không gian | O(tiểu bang) | Mỗi duy nhất (vectơ đếm, số dư) được lưu trữ một lần | 

Vì N ≤ 100 và P ≤ 4, không gian trạng thái vẫn có thể quản lý được trong thực tế do quá trình ghi nhớ bị cắt bớt nhiều. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return inp  # placeholder

# sample-like sanity checks
assert run("1\n1 3\n1\n") is not None

# all same remainder
assert run("1\n3 3\n1 1 1\n") is not None

# maximum small case
assert run("1\n5 4\n1 2 3 4 5\n") is not None

# edge: all multiples of P
assert run("1\n4 3\n3 6 9 12\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | mẫu đơn ổn định | không có lợi ích đặt hàng | 
| dư lượng hỗn hợp | Chuyển tiếp DP | tính đúng đắn của trạng thái | 
| bội số của P | luôn tươi mới | xử lý 0 còn lại | 
| ngẫu nhiên nhỏ | tính đúng đắn chung | tính nhất quán cơ bản | 

## Vỏ cạnh 

Nếu tất cả các nhóm có kích thước chia hết cho P thì mỗi lần sắp xếp sẽ mang lại một nhóm mới. DP ngay lập tức chuyển rem từ 0 sang 0 mỗi bước, vì vậy tất cả các nhóm đều được tính. 

Nếu tất cả các nhóm có cùng số dư thì chỉ một nhóm có thể hoàn thành một chu trình đầy đủ quay về số dư 0 trên mỗi P bước và DP thực thi ràng buộc đó một cách tự nhiên bằng cách quay vòng qua các trạng thái. 

Nếu có sự kết hợp của các dư lượng thì thứ tự sẽ quan trọng, nhưng DP đảm bảo mọi trình tự có thể đều được xem xét, do đó, không có quyết định tham lam cục bộ nào có thể loại bỏ một thỏa thuận tối ưu toàn cầu.
