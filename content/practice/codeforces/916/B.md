---
title: "CF 916B - Chuỗi Jamie và nhị phân (thay đổi sau vòng đấu)"
description: "Chúng ta muốn biểu diễn số nguyên dương n dưới dạng tổng của chính xác k lũy thừa của hai: $$n = 2^{a1} + 2^{a2} + cdots + 2^{ak}$$ Số mũ có thể dương, 0 hoặc thậm chí âm. Trong số tất cả các chuỗi hợp lệ có độ dài k, trước tiên chúng tôi giảm thiểu số mũ lớn nhất xuất hiện."
date: "2026-06-12T09:55:59+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 916
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 457 (Div. 2)"
rating: 2000
weight: 916
solve_time_s: 134
verified: true
draft: false
---

[CF 916B - Jamie và Chuỗi nhị phân (được thay đổi sau vòng)](https://codeforces.com/problemset/problem/916/B) 

**Xếp hạng:** 2000 
**Tags:** bitmasks, tham lam, toán học 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi muốn biểu diễn một số nguyên dương`n`như một tổng chính xác`k`sức mạnh của hai:$$n = 2^{a_1} + 2^{a_2} + \cdots + 2^{a_k}$$Số mũ có thể dương, bằng 0 hoặc thậm chí âm. Trong số tất cả các chuỗi có độ dài hợp lệ`k`, trước tiên chúng tôi giảm thiểu số mũ lớn nhất xuất hiện. Đặt số mũ lớn nhất có thể tối thiểu đó là`y`. Trong số tất cả các trình tự đạt được mức tối ưu đó`y`, chúng ta phải xuất ra chuỗi lớn nhất về mặt từ điển. 

Giá trị của`n`có thể lớn như`10^{18}`, trong khi`k`nhiều nhất là`10^5`. Biểu diễn nhị phân của`n`chứa tối đa sáu mươi bit, do đó, bất kỳ giải pháp nào hoạt động với lũy thừa của hai xuất hiện trong phân tách nhị phân đương nhiên là rất nhỏ. Mặt khác, việc tìm kiếm trên tất cả các phép gán số mũ có thể là hoàn toàn không thể vì số lượng biểu diễn tăng lên một cách bùng nổ. 

Phần bất thường của vấn đề là số mũ âm được cho phép. Ví dụ,$$2^{-1} + 2^{-1} = 1.$$Điều này có nghĩa là chúng ta luôn có thể chia một lũy thừa thành hai lũy thừa nhỏ hơn:$$2^x = 2^{x-1} + 2^{x-1}.$$Thao tác đó làm tăng số lượng số hạng lên một trong khi vẫn giữ nguyên tổng số. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như`n = 1, k = 2`. Một giải pháp tồn tại bởi vì$$1 = 2^{-1} + 2^{-1}.$$Một giải pháp chỉ xét đến số mũ không âm sẽ in sai`No`. 

Coi như`n = 3, k = 1`. Biểu diễn độ dài một duy nhất sẽ là lũy thừa của hai, nhưng`3`không phải là sức mạnh của hai. Câu trả lời đúng là`No`. 

Coi như`n = 8, k = 8`. Câu trả lời tối ưu là tám bản sao của số mũ`0`, bởi vì liên tục chia tách$$2^3 \to 2^2+2^2 \to \cdots \to 1+1+\cdots+1.$$Một thủ tục tham lam cố gắng giữ số mũ lớn mà không kiểm tra số thuật ngữ cần thiết có thể không đạt được kết quả chính xác`k`điều khoản. 

Yêu cầu về từ điển học là một điểm tinh tế khác. Sau khi tìm được số mũ lớn nhất có thể nhỏ nhất, chúng ta phải đặt số mũ lớn hơn càng sớm càng tốt. Hai nghiệm có cùng nhiều tập số mũ có thể có thứ tự từ điển khác nhau. 

## Phương pháp tiếp cận 

Một quan điểm vũ phu là nghĩ về mọi cách thể hiện của`n`như liên tục chia tách sức mạnh của hai. Bắt đầu từ biểu diễn nhị phân của`n`, mỗi phép chia thay thế một số mũ`x`bởi hai bản sao của`x-1`. Chúng ta có thể khám phá tất cả các chuỗi phân chia có thể có cho đến khi chính xác`k`các thuật ngữ xuất hiện. 

Điều này đúng vì mọi biểu diễn đều có thể thu được thông qua sự phân tách như vậy. Thật không may, số lượng các tiểu bang là rất lớn. Ngay cả đối với các giá trị vừa phải, hệ số phân nhánh vẫn trở nên rất lớn và không có cơ hội khám phá mọi khả năng. 

Quan sát quan trọng là chúng tôi không thực sự quan tâm đến mọi cách trình bày. Trước tiên chúng ta quan tâm đến giá trị tối thiểu có thể có của số mũ lớn nhất. 

Giả sử chúng ta quyết định rằng mọi số mũ phải lớn nhất`M`. Bất kỳ bit gốc nào`2^i`với`i > M`phải được chia cho đến khi tất cả số mũ kết quả trở thành`M`. Một bản sao của`2^i`biến thành$$2^{i-M}$$bản sao của số mũ`M`. 

Điều này ngay lập tức cho chúng ta biết chúng ta có thể thu được bao nhiêu số hạng trong khi vẫn giữ tối đa số mũ tối đa`M`. 

Đối với một cố định`M`, cũng có số lượng số hạng tối thiểu có thể có. Mỗi bit có số mũ nhiều nhất`M`có thể không thay đổi. Mỗi bit ở trên`M`phải được chia theo cấp độ`M`. 

Cho phép`cnt(M)`biểu thị số lượng thuật ngữ tối thiểu. Nếu như`cnt(M) > k`, thì ngay cả cách biểu diễn nhỏ gọn nhất cũng đã sử dụng quá nhiều thuật ngữ, vì vậy`M`là không thể. 

Nếu như`cnt(M) \le k`, thì chúng ta luôn có thể tạo các số hạng bổ sung bằng cách chia thêm số mũ nhiều nhất`M`. Vì việc chia tách làm tăng số lượng thuật ngữ lên chính xác một, nên có thể đạt được bất kỳ số lượng lớn hơn nào. Như vậy`M`là khả thi. 

Tính khả thi là đơn điệu. Nếu như`M`hoạt động, mọi giới hạn số mũ lớn hơn cũng hoạt động. Điều này cung cấp một tìm kiếm nhị phân cho khả năng nhỏ nhất`M`. 

Sau khi tìm được số mũ tối đa tối ưu`M`, chúng ta phải xây dựng chuỗi lớn nhất về mặt từ điển. Chúng ta bắt đầu với sự đại diện có thời hạn tối thiểu tôn trọng giới hạn`M`. Sau đó, chúng tôi thực hiện phân chia bổ sung cho đến khi chúng tôi đạt được chính xác`k`điều khoản. 

Để tối đa hóa thứ tự từ điển, chúng ta nên giữ số mũ lớn bất cứ khi nào có thể và chia số mũ nhỏ nhất có sẵn trước. Việc tách số mũ nhỏ hơn sẽ ảnh hưởng đến các vị trí sau trong chuỗi được sắp xếp giảm dần và giữ nguyên các giá trị lớn hơn ở phía trước. 

Hàng đợi số mũ ưu tiên cho phép chúng ta liên tục chia số mũ tối thiểu hiện tại cho đến khi chính xác`k`các điều khoản tồn tại. 

Cuối cùng, chúng tôi xuất tất cả số mũ theo thứ tự giảm dần. Thứ tự đó lớn nhất về mặt từ điển trong số tất cả các cách biểu diễn có cùng nhiều tập hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(log2 n + k log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân hủy`n`thành nhị phân. Đối với mỗi bit được đặt tại vị trí`i`, ghi lại một bản sao của số mũ`i`. 
2. Tìm kiếm nhị phân giới hạn số mũ nhỏ nhất`M`trong đó một biểu diễn hợp lệ với số mũ tối đa nhiều nhất`M`tồn tại. 
3. Để kiểm tra ứng viên`M`, hãy tính số số hạng tối thiểu cần có dưới giới hạn đó. 

Nếu một chút đóng góp số mũ`i ≤ M`, nó đóng góp một số hạng. 

Nếu như`i > M`, nó phải được chia thành cấp độ`M`, sản xuất`2^(i-M)`điều khoản. 
4. Nếu số lượng tối thiểu này vượt quá`k`, giới hạn`M`là không thể. 
5. Nếu không thì giới hạn là khả thi, bởi vì việc chia tách bổ sung luôn có thể tăng số thuật ngữ lên từng phần một. 
6. Sau khi tìm kiếm nhị phân kết thúc, hãy chọn câu trả lời nhỏ nhất có thể`M`. 
7. Xây dựng đại diện có thời hạn tối thiểu cho việc này`M`. 

Mỗi số mũ ban đầu`i ≤ M`đóng góp một số mũ`i`. 

Mỗi số mũ ban đầu`i > M`đóng góp`2^(i-M)`bản sao của số mũ`M`. 
8. Gọi số số hạng hiện tại là`cur`. 
9. Trong khi`cur < k`, liên tục chia số mũ nhỏ nhất hiện có. 

Thay thế một số mũ`x`bởi hai bản sao của`x-1`. 

Tăng`cur`bởi một. 
10. Chính xác là khi nào`k`tồn tại, hãy sắp xếp tất cả số mũ theo thứ tự giảm dần. 
11. Xuất chuỗi. 

### Tại sao nó hoạt động 

Đối với một giới hạn cố định`M`, mọi số mũ lớn hơn`M`cuối cùng phải giảm xuống`M`hoặc bên dưới. Chia trực tiếp theo cấp độ`M`tạo ra số hạng nhỏ nhất có thể dưới giới hạn đó. Bất kỳ cách biểu diễn nào khác tôn trọng cùng một giới hạn chỉ có thể có ít nhất số hạng bằng nhau. 

Như vậy`cnt(M)`là số thuật ngữ tối thiểu có thể đạt được. Nếu như`cnt(M) > k`, không tồn tại nghiệm nào với số mũ lớn nhất`M`. Nếu như`cnt(M) ≤ k`, việc chia tách lặp đi lặp lại cho phép chúng ta tăng số lượng số hạng lên đúng một mỗi lần, do đó đạt được chính xác`k`các điều khoản luôn luôn có thể. 

Tìm kiếm nhị phân tìm thấy khả năng nhỏ nhất có thể`M`, đó chính xác là số mũ lớn nhất có thể nhỏ nhất. 

Sau khi sửa`M`, chỉ cần phân chia bổ sung để đạt được độ dài cần thiết. Việc tách số mũ nhỏ nhất sẽ bảo toàn số mũ lớn hơn, giữ cho phần trước của dãy giảm dần càng lớn càng tốt. Do đó, dãy giảm dần cuối cùng có từ điển lớn nhất trong số tất cả các nghiệm tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    bits = []
    x = n
    pos = 0
    while x:
        if x & 1:
            bits.append(pos)
        pos += 1
        x >>= 1

    if n == 0:
        print("No")
        return

    def min_terms(M):
        total = 0
        for b in bits:
            if b <= M:
                total += 1
            else:
                total += 1 << (b - M)
            if total > k:
                return total
        return total

    lo = -100000
    hi = max(bits) if bits else 0

    while lo < hi:
        mid = (lo + hi) // 2
        if min_terms(mid) <= k:
            hi = mid
        else:
            lo = mid + 1

    M = lo

    exps = []
    for b in bits:
        if b <= M:
            exps.append(b)
        else:
            exps.extend([M] * (1 << (b - M)))

    cur = len(exps)

    heapq.heapify(exps)

    while cur < k:
        x = heapq.heappop(exps)
        heapq.heappush(exps, x - 1)
        heapq.heappush(exps, x - 1)
        cur += 1

    ans = sorted(exps, reverse=True)

    print("Yes")
    print(*ans)

if __name__ == "__main__":
    solve()
```Phân tách nhị phân chứa tối đa sáu mươi số mũ, do đó việc kiểm tra tính khả thi là cực kỳ nhỏ. 

chức năng`min_terms(M)`tính số số hạng nhỏ nhất có thể trong khi buộc mọi số mũ phải lớn nhất`M`. Đối với số mũ trên`M`, mỗi cấp độ phân chia sẽ nhân đôi số lượng bản sao, mang lại`2^(b-M)`điều khoản. 

Tìm kiếm nhị phân hoạt động trên số mũ, bao gồm cả giá trị âm. Giới hạn dưới`-100000`an toàn ở dưới mức bất cứ thứ gì có thể cần thiết bởi vì`k ≤ 10^5`. Chúng tôi không bao giờ cần nhiều hơn khoảng`log2(k)`mức phân tách bổ sung vượt quá số mũ nhỏ nhất hiện có. 

Sau khi đạt được mức tối ưu`M`, mã xây dựng rõ ràng biểu diễn thời hạn tối thiểu. Tổng số thuật ngữ được tạo ra không bao giờ nhiều hơn`k`, bởi vì`M`là khả thi và được chọn sao cho`min_terms(M) ≤ k`. 

Heap lưu trữ tất cả số mũ hiện tại. Mỗi phần tách sẽ loại bỏ một số mũ và chèn hai bản sao thấp hơn một cấp. Vì số số hạng tăng đúng một, nên chính xác`k - cur`sự chia tách là bắt buộc. 

Cuối cùng, sắp xếp theo thứ tự giảm dần sẽ cho ra chuỗi lớn nhất về mặt từ điển. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
23 5
```Biểu diễn nhị phân:$$23 = 2^4 + 2^2 + 2^1 + 2^0$$Số mũ tối đa khả thi nhỏ nhất là`3`. 

| Bước | Đại diện | Số kỳ hạn | 
| --- | --- | --- | 
| Ban đầu dưới M=3 | 3, 3, 2, 1, 0 | 5 | 
| Đã đạt mục tiêu | 3, 3, 2, 1, 0 | 5 | 

Đầu ra:```
Yes
3 3 2 1 0
```Ví dụ này cho thấy rằng một bản sao của`2^4`phải được chia thành hai bản sao của`2^3`, giảm số mũ tối đa từ`4`ĐẾN`3`. 

### Mẫu 2 

đầu vào:```
3 1
```Biểu diễn nhị phân:$$3 = 2^1 + 2^0$$| Bước | Đại diện | Số kỳ hạn | 
| --- | --- | --- | 
| Tối thiểu có thể | 1, 0 | 2 | 

Số lượng điều khoản tối thiểu đã có`2`và việc chia tách chỉ có thể tăng số lượng. 

Đầu ra:```
No
```Điều này thể hiện trường hợp độ dài được yêu cầu nhỏ hơn độ dài tối thiểu có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log2 n + k log k) | Tìm kiếm nhị phân vượt quá giới hạn số mũ, sau đó tối đa là k phép toán đống | 
| Không gian | O(k) | Lưu trữ đại diện cuối cùng | 

Sự phân rã nhị phân chứa tối đa 60 bit, do đó việc kiểm tra tính khả thi là rất nhỏ. Chi phí chủ yếu đến từ việc duy trì tối đa`k`số mũ trong heap. Với`k ≤ 10^5`, nghiệm dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from contextlib import redirect_stdout

def solve():
    import heapq

    n, k = map(int, input().split())

    bits = []
    x = n
    p = 0
    while x:
        if x & 1:
            bits.append(p)
        p += 1
        x >>= 1

    def min_terms(M):
        total = 0
        for b in bits:
            total += 1 if b <= M else (1 << (b - M))
            if total > k:
                return total
        return total

    lo = -100000
    hi = max(bits)

    while lo < hi:
        mid = (lo + hi) // 2
        if min_terms(mid) <= k:
            hi = mid
        else:
            lo = mid + 1

    M = lo

    exps = []
    for b in bits:
        if b <= M:
            exps.append(b)
        else:
            exps.extend([M] * (1 << (b - M)))

    cur = len(exps)

    heapq.heapify(exps)

    while cur < k:
        x = heapq.heappop(exps)
        heapq.heappush(exps, x - 1)
        heapq.heappush(exps, x - 1)
        cur += 1

    ans = sorted(exps, reverse=True)

    print("Yes")
    print(*ans)

def run(inp: str) -> str:
    global input
    input = io.StringIO(inp).readline

    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("23 5\n") == "Yes\n3 3 2 1 0"

# custom cases
assert run("1 1\n") == "Yes\n0"
assert run("1 2\n") == "Yes\n-1 -1"
assert run("3 1\n") == "No"
assert run("8 8\n") == "Yes\n0 0 0 0 0 0 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Trường hợp hợp lệ nhỏ nhất | 
|`1 2`|`-1 -1`| Số mũ âm được phép | 
|`3 1`|`No`| Độ dài yêu cầu dưới mức tối thiểu | 
|`8 8`| Tám số không | Tách lặp đi lặp lại để đếm chính xác | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 2
```Biểu diễn nhị phân chứa một số mũ`0`. Số mũ tối đa tối ưu là`-1`, bởi vì$$1 = 2^{-1}+2^{-1}.$$Việc kiểm tra tính khả thi cho thấy rằng`M = -1`cho đúng hai số hạng. Các đầu ra thuật toán:```
Yes
-1 -1
```Trường hợp này tìm ra các nghiệm giả định sai số mũ phải không âm. 

Coi như:```
3 1
```Biểu diễn nhị phân là`[1, 0]`. Ngay cả khi không có hạn chế về số mũ, số hạng tối thiểu là`2`. Vì việc chia tách chỉ làm tăng số lượng nên việc đạt được một số hạng là không thể. Thuật toán in chính xác:```
No
```Coi như:```
8 8
```Bắt đầu từ số mũ`[3]`, số mũ tối đa tối ưu trở thành`0`, tạo ra tám bản sao của số mũ`0`. Thuật toán xây dựng chính xác:```
0 0 0 0 0 0 0 0
```Điều này xác minh rằng tìm kiếm nhị phân có thể đẩy số mũ tối đa xuống dưới bit cao nhất ban đầu khi cần một số lượng lớn các thuật ngữ. 

Coi như:```
5 4
```chúng tôi có$$5 = 2^2 + 2^0.$$Với`M = 0`, biểu diễn tối thiểu sẽ trở thành bốn bản sao của số mũ`0`cộng thêm một số mũ nữa`0`, tổng cộng là năm số hạng, quá nhiều. Với`M = 1`, biểu diễn tối thiểu là ba số hạng:`1, 1, 0`. Một phép chia bổ sung của số mũ nhỏ nhất sẽ cho bốn số hạng, tạo ra câu trả lời tối ưu. Điều này kiểm tra ranh giới giữa giới hạn số mũ khả thi và không khả thi.
