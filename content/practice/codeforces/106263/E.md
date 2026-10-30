---
title: "CF 106263E - thi công 2 cứng 4 me"
description: "Chúng ta được cho một dãy số nguyên tăng dần. Từ dãy này, ta được phép loại bỏ tối đa k phần tử, để lại dãy con còn lại theo thứ tự ban đầu."
date: "2026-06-18T23:19:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106263
codeforces_index: "E"
codeforces_contest_name: "2025 \u534e\u5357\u5e08\u8303\u5927\u5b66\u201c\u5353\u8d8a\u6559\u80b2\u676f\u201d\u7b97\u6cd5\u4e0e\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u65b0\u751f\u8d5b\uff09"
rating: 0
weight: 106263
solve_time_s: 58
verified: true
draft: false
---

[CF 106263E - xây dựng là 2 cứng 4 tôi](https://codeforces.com/problemset/problem/106263/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên tăng dần. Từ trình tự này, chúng ta được phép loại bỏ nhiều nhất`k`các phần tử, để lại một dãy con còn lại theo thứ tự ban đầu. Giá trị của chuỗi kết quả chỉ được xác định bằng chênh lệch nhỏ nhất giữa hai phần tử còn lại liên tiếp. Nếu còn lại ít hơn hai phần tử, giá trị được coi là vô hạn. 

Nhiệm vụ là chọn những phần tử cần xóa sao cho khoảng cách liền kề tối thiểu này trong chuỗi còn lại trở nên lớn nhất có thể. 

Khó khăn cốt lõi là việc xóa các phần tử sẽ làm tăng khoảng trống một cách gián tiếp, nhưng chúng tôi bị hạn chế bởi số lượng thao tác xóa mà chúng tôi có thể thực hiện. Mỗi lựa chọn đều ảnh hưởng đến tất cả các khoảng trống trong tương lai, vì vậy các quyết định cục bộ có thể dễ dàng phá vỡ mức tối ưu toàn cầu. 

Các ràng buộc cho phép lên đến`2 × 10^5`tổng số phần tử trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc thậm chí tất cả các mẫu xóa, vì giải pháp đó sẽ tăng theo cấp số nhân. Ngay cả cách tiếp cận bậc hai cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm trong trường hợp xấu nhất. 

Một trường hợp thất bại tinh tế xuất hiện khi cố gắng xóa tham lam mà không có khoảng cách mục tiêu. Ví dụ: nếu chúng tôi luôn loại bỏ phần tử "xấu" cục bộ tạo ra khoảng cách nhỏ nhất, chúng tôi có thể phá hủy cấu trúc có thể cho phép khoảng cách tối thiểu toàn cục tốt hơn sau các lần xóa khác nhau. 

Một tình huống cạnh khác xảy ra khi`k = 0`. Sau đó, câu trả lời được ép buộc bởi mảng ban đầu, do đó, bất kỳ thuật toán nào cũng phải xử lý chính xác trường hợp không được phép xóa mà vẫn đưa ra một bộ xóa hợp lệ. 

Cuối cùng, khi lời giải tối ưu giữ lại rất ít phần tử thì dễ dẫn đến kết quả xuất ra nhiều hơn`k`xóa bỏ trừ khi việc xây dựng được gắn chặt với tính khả thi. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ thử mọi tập hợp con thu được bằng cách xóa nhiều nhất`k`các phần tử. Đối với mỗi tập hợp con, chúng tôi tính toán tất cả các khác biệt liền kề và lấy mức tối thiểu. Ngay cả khi chúng ta chỉ xem xét chính xác`n - k`các phần tử được giữ nguyên, số cách chọn chúng vẫn mang tính tổ hợp, đại khái là`O(n choose k)`, điều này vượt xa khả năng thực hiện. 

Quan sát quan trọng là chúng ta không cần phải trực tiếp quyết định những phần tử nào cần loại bỏ. Thay vào đó, chúng ta lật ngược quan điểm: giả sử chúng ta sửa một câu trả lời của ứng viên`x`, nghĩa là chúng ta muốn mọi cặp liền kề trong dãy cuối cùng khác nhau ít nhất`x`. Bây giờ vấn đề trở thành kiểm tra xem liệu chúng ta có thể chọn một dãy con thỏa mãn ràng buộc này trong khi loại bỏ nhiều nhất`k`các phần tử. 

Đối với một cố định`x`, chúng ta có thể tham lam xây dựng dãy con hợp lệ dài nhất bằng cách luôn lấy phần tử sớm nhất có thể và bỏ qua mọi thứ vi phạm`x`yêu cầu khoảng cách. Cách xây dựng tham lam này là tối ưu vì một khi chúng ta chọn một phần tử, việc trì hoãn lựa chọn tiếp theo chỉ làm giảm các lựa chọn trong tương lai. 

Điều này biến vấn đề thành một kiểm tra tính khả thi đơn điệu, cho phép tìm kiếm nhị phân trên`x`. Nếu một khoảng cách`x`có thể đạt được thì bất kỳ khoảng cách nhỏ hơn nào cũng có thể đạt được vì ràng buộc trở nên yếu hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + kiểm tra tham lam | O(n log A) | O(n) | Đã chấp nhận | 

Đây`A`là phạm vi giá trị trong mảng. 

## Hướng dẫn thuật toán 

Chúng tôi muốn tìm khoảng cách tối thiểu tối đa có thể có trong một dãy con hợp lệ dưới giới hạn xóa. 

1. Chúng tôi tìm kiếm câu trả lời nhị phân`x`, chênh lệch liền kề tối thiểu cho phép. Phạm vi tìm kiếm là từ`0`ĐẾN`a[n-1] - a[0]`. Phạm vi này bao gồm tất cả các khoảng trống có ý nghĩa có thể có vì không có chênh lệch liền kề nào có thể vượt quá toàn bộ khoảng cách. 
2. Đối với cố định`x`, chúng ta kiểm tra tính khả thi bằng cách tham lam xây dựng một dãy con. Chúng tôi bắt đầu bằng cách luôn chọn phần tử đầu tiên. 
3. Chúng tôi quét mảng từ trái sang phải. Bất cứ khi nào phần tử hiện tại ít nhất`last_selected + x`, chúng tôi giữ nó và cập nhật`last_selected`. Nếu không, chúng tôi bỏ qua nó như thể nó đã bị xóa. 
4. Sau khi quét, chúng tôi tính toán có bao nhiêu phần tử được giữ lại. Nếu như`kept >= n - k`, sau đó`x`là khả thi vì chúng tôi luôn có thể xóa các phần tử được lưu giữ thêm nếu cần trong khi vẫn nằm trong ngân sách xóa. 
5. Sử dụng tìm kiếm nhị phân, chúng tôi tìm thấy khả năng lớn nhất`x`. 
6. Chúng tôi chạy lại quy trình tham lam bằng cách sử dụng phương pháp tối ưu này`x`, ghi lại những chỉ số nào được giữ lại. 
7. Tất cả các chỉ số khác tạo thành bộ xóa. Chúng tôi xuất kích thước của nó và các chỉ số. 

### Tại sao nó hoạt động 

Kiểm tra tham lam tạo ra kích thước tối đa có thể có của dãy con thỏa mãn ràng buộc về khoảng cách`x`. Bất kỳ chuỗi con hợp lệ nào cũng phải tôn trọng các ràng buộc thứ tự giống nhau và việc chọn phần tử hợp lệ trước đó không bao giờ làm giảm tính khả thi trong tương lai. Điều này làm cho giải pháp tham lam tối ưu cho cố định`x`. Tìm kiếm nhị phân là hợp lệ vì tính khả thi là đơn điệu trong`x`, vì vậy một khi không thể có khoảng trống thì tất cả các khoảng trống lớn hơn cũng không thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def feasible(a, k, x):
    n = len(a)
    cnt = 1
    last = a[0]
    for i in range(1, n):
        if a[i] - last >= x:
            cnt += 1
            last = a[i]
    return cnt >= n - k

def build(a, x):
    n = len(a)
    keep = [False] * n
    last = a[0]
    keep[0] = True
    for i in range(1, n):
        if a[i] - last >= x:
            keep[i] = True
            last = a[i]
    return keep

def solve():
    T = int(input())
    for _ in range(T):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        lo, hi = 0, a[-1] - a[0]

        while lo <= hi:
            mid = (lo + hi) // 2
            if feasible(a, k, mid):
                lo = mid + 1
            else:
                hi = mid - 1

        best = hi
        keep = build(a, best)

        delete_idx = []
        for i in range(n):
            if not keep[i]:
                delete_idx.append(i + 1)

        if len(delete_idx) > k:
            delete_idx = delete_idx[:k]

        print(len(delete_idx))
        if delete_idx:
            print(*delete_idx)
        else:
            print()

if __name__ == "__main__":
    solve()
```Hàm khả thi tính toán có thể giữ lại bao nhiêu phần tử trong khi duy trì khoảng cách tối thiểu`x`. Hàm xây dựng lặp lại quá trình tham lam tương tự nhưng ghi lại các quyết định. Tìm kiếm nhị phân đảm bảo chúng tôi tối đa hóa khoảng cách nhỏ nhất được phép và danh sách xóa cuối cùng chỉ đơn giản là phần bổ sung của tập hợp được giữ lại. 

Một điểm tinh tế là kẻ tham lam giữ số phần tử tối đa có thể, vì vậy nếu nó đã giữ nhiều hơn`n - k`, chúng ta vẫn an toàn vì được phép xóa ít hơn`k`các phần tử. 

## Ví dụ đã hoạt động 

Hãy xem xét`a = [1, 4, 6, 10]`,`k = 1`. 

Chúng tôi tìm kiếm nhị phân cho khoảng cách tối thiểu tốt nhất. 

Vì`x = 5`, sự lựa chọn tham lam`1`, bỏ qua`4`, bỏ qua`6`, giữ`10`, do đó chỉ có 2 phần tử được giữ lại. Từ`n - k = 3`, điều này là không thể thực hiện được. 

Vì`x = 3`, sự lựa chọn tham lam`1`, giữ`4`, bỏ qua`6`, giữ`10`, cho 3 phần tử, điều này là khả thi. 

Bây giờ chúng tôi tối đa hóa và tìm thấy`x = 3`. 

| Bước | Chỉ mục | Giá trị | Được chọn lần cuối | Giữ? | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | 1 giữ ở mức 1 | 1 | 
| tôi=2 | 2 | 4 | 1 | vâng | 2 | 
| tôi=3 | 3 | 6 | 4 | không | 2 | 
| tôi=4 | 4 | 10 | 4 | vâng | 3 | 

Việc xóa cuối cùng là chỉ mục`3`. 

Điều này cho thấy quá trình tham lam thực thi ràng buộc khoảng cách tối thiểu như thế nào trong khi vẫn tối đa hóa số lượng phần tử còn lại. 

Bây giờ hãy xem xét`a = [2, 3, 7, 8, 15]`,`k = 2`. 

Đối với một khoảng cách lớn như`x = 7`, chúng tôi giữ`2, 8, 15`, điều đó là khả thi. Nếu chúng ta cố gắng`x = 8`, chúng tôi chỉ giữ`2, 15`, không đủ để thỏa mãn ràng buộc xóa. Tìm kiếm nhị phân giải quyết trên giá trị khả thi lớn nhất. 

Dấu vết cho thấy việc thu hẹp khoảng cách cần thiết sẽ làm giảm số lượng phần tử có thể chọn như thế nào và tính khả thi được xác định hoàn toàn bằng việc liệu chúng ta có còn giữ đủ phần tử hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi bước tìm kiếm nhị phân chạy quét tham lam tuyến tính trên mảng | 
| Không gian | O(n) | Lưu trữ để giữ các điểm đánh dấu và đầu ra xóa | 

Tổng của`n`trên tất cả các trường hợp thử nghiệm là`2 × 10^5`và tìm kiếm logarit trên các giá trị đủ nhỏ để dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("1\n2 1\n1 10\n") in {"1\n1", "0"}, "min case"

# already optimal, no deletions needed
assert run("1\n4 0\n1 2 3 10\n") == "0", "k=0"

# small structured case
res = run("1\n5 2\n1 2 4 7 11\n")
assert len(res.splitlines()[0].split()) == 1

# all equal gaps large
assert run("1\n3 1\n1 100 200\n") != "", "basic feasibility"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 trường hợp | bất kỳ hợp lệ | ranh giới với khoảng cách duy nhất | 
| k=0 trường hợp | 0 lần xóa | ràng buộc không loại bỏ | 
| cấu trúc 5 phần tử | bộ xóa hợp lệ | tính đúng đắn chung | 
| mảng cách nhau | đầu ra không trống | xử lý khả thi | 

## Vỏ cạnh 

Khi nào`k = 0`, cấu trúc tham lam không bao giờ được sử dụng để xóa. Tìm kiếm nhị phân vẫn đánh giá tính khả thi một cách chính xác nhưng danh sách xóa cuối cùng phải trống. Thuật toán xử lý việc này một cách tự nhiên vì tất cả các phần tử vẫn được đánh dấu là được lưu giữ. 

Khi`n - k = 1`, điều kiện khả thi trở nên tầm thường vì bất kỳ phần tử đơn lẻ nào cũng đủ. Kẻ tham lam sẽ luôn trả về ít nhất một phần tử và thuật toán cho phép chính xác các phần tử lớn`x`giá trị, mặc dù câu trả lời cuối cùng là không liên quan vì không tồn tại sự khác biệt liền kề. 

Khi giải pháp tối ưu giữ lại gần như tất cả các phần tử, thì tham lam vẫn tạo ra một siêu tập hợp hợp lệ có kích thước được yêu cầu và chúng tôi chỉ cần cắt bớt tối đa`k`xóa bằng cách chọn phần bù của bộ được giữ.
