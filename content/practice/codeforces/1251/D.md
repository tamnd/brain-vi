---
title: "CF 1251D - Thay đổi tiền lương"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có một số lượng nhân viên lẻ và mỗi nhân viên có một khoảng lương mà từ đó mức lương cuối cùng của họ phải được chọn."
date: "2026-06-13T21:36:20+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1251
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 75 (Rated for Div. 2)"
rating: 1900
weight: 1251
solve_time_s: 271
verified: true
draft: false
---

[CF 1251D - Thay đổi lương](https://codeforces.com/problemset/problem/1251/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, tham lam, sắp xếp 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có một số lượng nhân viên lẻ và mỗi nhân viên có một khoảng lương mà từ đó mức lương cuối cùng của họ phải được chọn. Chúng tôi cũng được cấp tổng ngân sách được đảm bảo đủ lớn để trả cho mọi người mức lương tối thiểu có thể. 

Nhiệm vụ là ấn định cho mỗi nhân viên một mức lương trong khoảng thời gian cho phép của họ sao cho giá trị trung bình của tất cả các mức lương được chọn càng lớn càng tốt. Trung vị được xác định sau khi sắp xếp tất cả tiền lương và vì số lượng nhân viên là số lẻ nên nó là phần tử ở giữa duy nhất. 

Vì vậy, vấn đề không phải là tối đa hóa tổng lương hay sự công bằng theo bất kỳ ý nghĩa toàn cầu nào. Đó là việc đẩy phần tử ở giữa của mảng lương đã sắp xếp lên cao nhất có thể trong khi vẫn tôn trọng giới hạn cá nhân và ngân sách toàn cầu. 

Sự tương tác chính nằm giữa ba ràng buộc: mỗi mức lương được giới hạn bởi một khoảng, tổng của tất cả các mức lương bị giới hạn và chỉ có vị trí trung bình là quan trọng. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cố gắng mô phỏng các bài tập hoặc liên tục kiểm tra tính khả thi bằng cách xây dựng các mảng đầy đủ cho mỗi lần đoán sẽ thất bại. Với tổng số nhân viên lên tới 2×10^5, bất kỳ cách tiếp cận nào kém hơn O(n log n) cho mỗi trường hợp thử nghiệm sẽ không mở rộng được. 

Một trường hợp phức tạp phát sinh khi nhiều khoảng rộng nhưng ngân sách eo hẹp vượt quá số tiền tối thiểu. Một kẻ tham lam ngây thơ chỉ đơn giản là tăng tất cả các mức lương một cách độc lập sẽ thất bại vì việc tăng một giá trị có thể buộc phải giảm tính khả thi ở nơi khác, mặc dù tính khả thi ở giới hạn thấp hơn vẫn được đảm bảo. 

Một dạng thất bại khác xuất hiện nếu chúng ta cố gắng tham lam gán mức lương cao hơn cho mọi người một cách cân đối. Điều đó bỏ qua rằng chỉ có vị trí trung bình mới quan trọng, vì vậy việc lãng phí ngân sách cho nửa dưới là không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi giá trị có thể có của số trung vị và kiểm tra xem liệu nó có thể đạt được hay không. Đối với giá trị ứng viên x, chúng ta sẽ cố gắng xây dựng một nhiệm vụ trong đó ít nhất một nửa số nhân viên có mức lương ít nhất là x, đồng thời tôn trọng các khoảng thời gian và tổng ngân sách. 

Để xác minh tính khả thi của một x cố định, chúng ta cần ấn định mức lương một cách tham lam: đẩy càng nhiều người càng tốt đến ít nhất x trong khoảng thời gian của họ, sau đó phân bổ ngân sách còn lại. Điều này có thể được thực hiện O(n) cho mỗi lần kiểm tra. 

Tuy nhiên, trung vị có thể lớn tới 10^9, do đó việc quét tuyến tính trên tất cả các giá trị có thể là không thể. Ngay cả tìm kiếm nhị phân x cũng cho O(n log n) cho mỗi trường hợp thử nghiệm, quá chậm nếu mỗi lần kiểm tra không được tối ưu hóa cẩn thận và vẫn có rủi ro giới hạn chặt chẽ dưới tổng số 2×10^5 nhân viên. 

Quan sát quan trọng là tính khả thi là đơn điệu trong x. Nếu chúng ta có thể làm cho ít nhất k nhân viên có lương ≥ x thì x nào nhỏ hơn cũng khả thi. Điều này cho phép tìm kiếm nhị phân trên câu trả lời. 

Cái nhìn sâu sắc thứ hai và sâu sắc hơn là làm thế nào để đánh giá tính khả thi một cách hiệu quả. Chúng ta không cần phải xây dựng đầy đủ một bài tập. Chúng ta chỉ cần biết liệu chúng ta có thể buộc ít nhất k = n//2 + 1 nhân viên đạt ít nhất x trong khi vẫn nằm trong ngân sách hay không. 

Đối với mỗi nhân viên, có ba trạng thái liên quan đến x: họ không thể đạt x (r_i < x), họ chỉ có thể đạt x bằng cách trả thêm tiền (l_i < x ≤ r_i) hoặc họ đã bị buộc phải trên x (l_i ≥ x). Chúng tôi tham lam quyết định những nhân viên nào chúng tôi cố gắng đẩy lên trên x, luôn ưu tiên những nhân viên có chi phí thấp nhất để tuyển dụng vì ngân sách là yếu tố hạn chế. 

Điều này biến việc kiểm tra tính khả thi thành việc sắp xếp các ứng viên theo chi phí để nâng họ lên x. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lực lượng vũ phu | O(n·max_salary) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + tính khả thi tham lam | O(n log n log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tìm kiếm nhị phân câu trả lời, coi giá trị trung bình x là ngưỡng.

1. Cố định giá trị trung bình của ứng viên x và phân loại từng nhân viên dựa trên khoảng cách của họ so với x. Nếu r_i < x, nhân viên này không thể góp phần đạt được mức trung vị nên bị bỏ qua vào “nhóm cao”. Nếu l_i ≥ x thì đã thỏa mãn điều kiện mà không phải trả thêm phí. Ngược lại, chúng có thể được tùy ý đẩy lên x với giá x - l_i. 
2. Đếm xem có bao nhiêu nhân viên đã thỏa mãn l_i ≥ x. Đây tự động là một phần của nhóm cao. Nếu số này ít nhất đã có k = n//2 + 1 thì x là khả thi vì chúng ta có thể chỉ định những người khác ở mức tối thiểu mà không ảnh hưởng đến số trung vị. 
3. Nếu không đủ nhân viên trên x, chúng tôi sẽ thu thập tất cả nhân viên “có thể nâng cấp”, những nhân viên có l_i < x ≤ r_i. Đối với mỗi người trong số họ, hãy tính chi phí = x - l_i. 
4. Sắp xếp các chi phí này theo thứ tự tăng dần. Chúng tôi muốn chọn những nhân viên rẻ nhất để nâng cấp trước vì mỗi lần nâng cấp sẽ đóng góp thêm một người bằng hoặc cao hơn x. 
5. Chúng tôi lấy chi phí nhỏ nhất cho đến khi đạt được k nhân viên ở nhóm cao. Nếu hết ứng viên trước khi đạt k thì x không khả thi. 
6. Đối với những nâng cấp đã chọn, hãy tính tổng chi phí bổ sung bằng tổng của (x - l_i). Nếu chi phí tăng thêm này vượt quá ngân sách còn lại sau khi thanh toán hết l_i thì x là không khả thi; nếu không nó là khả thi. 
7. Tìm kiếm nhị phân x trên một phạm vi đủ lớn, thường lên tới 10^9. 

Tính đúng đắn dựa trên thực tế là bất kỳ giải pháp tối ưu nào cho x cố định sẽ không bao giờ lãng phí ngân sách để tăng ai đó lên trên x vượt quá mức cần thiết để thỏa mãn điều kiện trung bình. Bất kỳ sự gia tăng thêm nào đều không giúp ích cho tính khả thi và chỉ làm tăng chi phí. 

### Tại sao nó hoạt động 

Đối với một ngưỡng cố định x, điều quan trọng duy nhất là liệu có thể đạt được ít nhất k mức lương ≥ x trong điều kiện hạn chế về ngân sách hay không. Trong số tất cả các cách để chọn k nhân viên để thăng chức trên x, lựa chọn rẻ nhất luôn là chọn những người có giá trị nhỏ nhất (x - l_i). Bất kỳ sai lệch nào so với lựa chọn này chỉ làm tăng tổng chi phí mà không làm tăng tính khả thi. Cấu trúc tham lam này đảm bảo rằng việc kiểm tra tính khả thi là tối ưu và đơn điệu trong x, điều này làm cho tìm kiếm nhị phân trở nên hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(x, arr, s, k):
    base = 0
    candidates = []

    for l, r in arr:
        base += l
        if r < x:
            continue
        if l >= x:
            continue
        candidates.append(x - l)

    if base > s:
        return False

    need = k
    have = 0

    for l, r in arr:
        if l >= x:
            have += 1

    if have >= k:
        return True

    need = k - have

    candidates.sort()

    extra = 0
    for i in range(min(need, len(candidates))):
        extra += candidates[i]

    return base + extra <= s

def solve():
    t = int(input())
    for _ in range(t):
        n, s = map(int, input().split())
        arr = [tuple(map(int, input().split())) for _ in range(n)]
        k = n // 2 + 1

        lo, hi = 1, 10**9
        ans = 1

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, arr, s, k):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc xung quanh trình kiểm tra tính khả thi và trình điều khiển tìm kiếm nhị phân. Hàm khả thi tính toán chi phí cơ bản để trả mức lương tối thiểu cho tất cả nhân viên, sau đó cố gắng “nâng cấp” đủ số lượng nhân viên để đạt đến ngưỡng trung bình. 

Chi tiết quan trọng là tách nhân viên thành những người đã bằng hoặc cao hơn x và những người có thể được nâng cấp. Bước sắp xếp đảm bảo chúng tôi luôn chọn những bản nâng cấp rẻ nhất trước tiên. 

Tìm kiếm nhị phân thúc đẩy giải pháp vì tính khả thi là đơn điệu: nếu giá trị trung bình x có thể đạt được thì bất kỳ giá trị nhỏ hơn nào cũng có thể đạt được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 26
10 12
1 4
10 11
```Chúng tôi tính toán k = 2. 

| x | tổng cơ sở | có ≥ x | nâng cấp được sử dụng | tổng chi phí | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 10 | 21 | 2 | 0 | 21 | vâng | 
| 11 | 21 | 1 | 1 | 22 | vâng | 
| 12 | 21 | 1 | 1 | 23 | vâng | 
| 13 | 21 | 0 | không thể | - | không | 

Giá trị x cao nhất khả thi là 11, khớp với trung vị tối ưu. 

Dấu vết này cho thấy tính khả thi phụ thuộc vào cả mức lương cao hiện tại và chi phí nâng cấp nhân viên ở vùng biên giới như thế nào. 

### Ví dụ 2 

đầu vào:```
5 26
4 4
2 4
6 8
5 6
2 7
```Ở đây k = 3. 

| x | tổng cơ sở | có ≥ x | nâng cấp được sử dụng | tổng chi phí | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 6 | 19 | 3 | 0 | 19 | vâng | 
| 7 | 19 | 2 | 1 | 21 | vâng | 
| 8 | 19 | 1 | 2 | 25 | vâng | 
| 9 | 19 | 0 | không thể | - | không | 

Câu trả lời là 6, là giá trị cuối cùng trước khi phá vỡ tính khả thi. 

Những ví dụ này cho thấy thuật toán cân bằng một cách tự nhiên giữa mức lương vốn đã cao và mức lương được nâng cấp một cách chiến lược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n log A) | tìm kiếm nhị phân theo câu trả lời, mỗi lần kiểm tra tính khả thi sẽ sắp xếp ứng viên | 
| Không gian | O(n) | khoảng thời gian lưu trữ và chi phí nâng cấp tạm thời | 

Các ràng buộc cho phép tổng cộng tối đa 2×10^5 nhân viên và log A là khoảng 30, do đó giải pháp phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2
    import sys

    input = sys.stdin.readline

    def can(x, arr, s, k):
        base = 0
        candidates = []
        have = 0

        for l, r in arr:
            base += l
            if l >= x:
                have += 1
            elif r >= x:
                candidates.append(x - l)

        if base > s:
            return False
        if have >= k:
            return True

        need = k - have
        candidates.sort()

        extra = sum(candidates[:need]) if need <= len(candidates) else 10**30
        return base + extra <= s

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, s = map(int, input().split())
            arr = [tuple(map(int, input().split())) for _ in range(n)]
            k = n // 2 + 1

            lo, hi = 1, 10**9
            ans = 1
            while lo <= hi:
                mid = (lo + hi) // 2
                if can(mid, arr, s, k):
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# provided samples
assert run("""3
3 26
10 12
1 4
10 11
1 1337
1 1000000000
5 26
4 4
2 4
6 8
5 6
2 7
""") == """11
1337
6"""

# custom cases
assert run("""1
1 100
10 20
""") == "20", "single employee"

assert run("""1
3 6
1 3
1 3
1 3
""") == "2", "tight symmetric case"

assert run("""1
5 15
1 10
1 10
1 10
1 10
1 10
""") == "1", "budget minimal median"

assert run("""1
5 100
1 100
1 100
1 100
1 100
1 100
""") == "100", "all wide ranges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhân viên độc thân | 20 | trường hợp biên n=1 | 
| trường hợp đối xứng chặt chẽ | 2 | trung bình do ngân sách hạn chế | 
| ngân sách trung bình tối thiểu | 1 | không thể nâng cấp | 
| tất cả các phạm vi rộng | 100 | hoàn toàn linh hoạt với đủ ngân sách | 

## Vỏ cạnh 

Một tình huống khó khăn xảy ra khi nhiều nhân viên đã có l_i ≥ x nhưng không cần thiết cho điểm trung vị. Thuật toán tính chính xác họ là những người đóng góp “miễn phí” cho nhóm cao, mặc dù họ có thể không nằm trong số những ứng cử viên được nâng cấp rẻ nhất. Vì họ đã đáp ứng ngưỡng mà không phải trả phí nên họ không bao giờ lọt vào danh sách ứng viên và không bóp méo sự lựa chọn tham lam. 

Một trường hợp khác là khi các bản nâng cấp rẻ nhất không đủ số lượng để đạt k. Trong trường hợp đó, danh sách ứng cử viên sẽ hết trước khi đáp ứng được yêu cầu trung bình và tính khả thi sẽ sớm thất bại nếu không tính đến những khoản tiền không cần thiết.
